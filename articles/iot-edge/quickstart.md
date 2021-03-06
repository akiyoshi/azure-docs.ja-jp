---
title: Windows 上に Azure IoT Edge デバイスを作成するクイック スタート | Microsoft Docs
description: このクイック スタートでは、IoT Edge デバイスを作成した後、Azure portal から事前作成されたコードをリモートで展開する方法を学習します。
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/02/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 941d5d8f356fbd1477b4559f1475511165c01341
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/13/2018
ms.locfileid: "53340098"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>クイック スタート:初めての IoT Edge モジュールを Azure portal から Windows デバイスに展開する - プレビュー

このクイック スタートでは、Azure IoT Edge クラウド インターフェイスを使用して、事前作成されたコードを IoT Edge デバイスにリモートで展開します。 このタスクを実行するには、最初に Windows デバイスを使用して IoT Edge デバイスをシミュレートし、モジュールを展開します。

このクイック スタートでは、次の方法について説明します。

1. IoT Hub を作成します。
2. IoT Edge デバイスを IoT ハブに登録します。
3. IoT Edge ランタイムをデバイスにインストールして開始します。
4. モジュールを IoT Edge デバイスにリモートでデプロイし、テレメトリを IoT Hub に送信します。

![図 - デバイスとクラウドのクイック スタートのアーキテクチャ](./media/quickstart/install-edge-full.png)

このクイック スタートで展開するモジュールは、温度、湿度、および圧力のデータを生成するシミュレートされたセンサーです。 その他の Azure IoT Edge チュートリアルは、ここで行う作業を基盤としており、ビジネスに関する分析情報を得るためにシミュレートされたデータを分析するモジュールをデプロイします。

>[!NOTE]
>Windows 向けの IoT Edge ランタイムは、[パブリック プレビュー](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)段階にあります。

アクティブな Azure サブスクリプションをお持ちでない場合は、開始する前に[無料アカウント](https://azure.microsoft.com/free)を作成してください。

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

このクイック スタートの多くの手順は、Azure CLI を使用して実行します。Azure IoT には、追加機能を有効にする拡張機能が用意されています。

Azure IoT の拡張機能を Cloud Shell インスタンスに追加します。

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prerequisites"></a>前提条件

クラウド リソース:

* このクイック スタートで使用するすべてのリソースを管理するためのリソース グループです。

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus
   ```

IoT Edge デバイス: 

* IoT Edge デバイスとして機能する Windows コンピューターまたは仮想マシン。 サポートされている Windows バージョンを使用します。次のバージョンが該当します。
  * Windows 10 以降
  * Windows Server 2016 以降
* Windows コンピューターの場合は、Hyper-V の[システム要件](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements)を満たしていることを確認します。
* 仮想マシンの場合は、[入れ子になった仮想化](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization)を有効にして、少なくとも 2 GB のメモリを割り当てます。
* [Docker for Windows](https://docs.docker.com/docker-for-windows/install/) をインストールし、実行します。

> [!TIP]
> Docker のセットアップ中に、Windows コンテナーを使用するか、Linux コンテナーを使用するかを選択できます。 このクイック スタートでは、Linux コンテナーで使用するように IoT Edge ランタイムを構成する方法を説明します。

## <a name="create-an-iot-hub"></a>IoT Hub の作成

このクイック スタートでは、最初に Azure CLI で IoT Hub を作成します。

![図 - クラウドで IoT ハブを作成する](./media/quickstart/create-iot-hub.png)

このクイック スタートでは無料レベルの IoT Hub を使用できます。 IoT Hub を以前に使用したことがあり、無料のハブを作成済みである場合は、その IoT ハブを使用できます。 各サブスクリプションで使用できる無料 IoT ハブは 1 つのみです。

次のコードにより、無料の **F1** ハブがリソース グループ **IoTEdgeResources** に作成されます。 *{hub_name}* は、IoT ハブの一意の名前に置き換えてください。

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1
   ```

   サブスクリプションに無料のハブが既に 1 つあるためにエラーが発生する場合は、SKU を **S1** に変更します。 IoT ハブの名前が利用できないというエラーが発生した場合、自分以外のだれかが既にその名前のハブを所有していることを意味します。 新しい名前を試してください。 

## <a name="register-an-iot-edge-device"></a>IoT Edge デバイスを登録する

新しく作成された IoT Hub に IoT Edge デバイスを登録します。
![図 - IoT ハブ ID でデバイスを登録する](./media/quickstart/register-device.png)

お使いの IoT ハブと通信できるようにシミュレートされたデバイスのデバイス ID を作成します。 デバイス ID はクラウドに置かれるので、デバイスの一意の接続文字列を使用して、物理デバイスとデバイス ID とを関連付けることになります。

IoT Edge デバイスは、一般的な IoT デバイスとは異なる動作をし、別に管理できるため、この ID は IoT Edge デバイス用として `--edge-enabled` フラグで宣言します。 

1. Azure Cloud Shell で、次のコマンドを入力して、**myEdgeDevice** という名前のデバイスをハブに作成します。

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --hub-name {hub_name} --edge-enabled
   ```

   iothubowner ポリシー キーに関するエラーが表示された場合は、Cloud Shell で最新バージョンの azure-cli-iot-ext 拡張機能が実行されていることを確認してください。 

2. デバイスの接続文字列を取得します。この接続文字列により、IoT Hub 内で物理デバイスとその ID をリンクさせます。

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

3. 接続文字列をコピーして保存します。 次のセクションで、IoT Edge ランタイムを構成するときにこの値を使用します。

## <a name="install-and-start-the-iot-edge-runtime"></a>IoT Edge ランタイムをインストールして開始する

Azure IoT Edge ランタイムを IoT Edge デバイスにインストールし、デバイスの接続文字列を使用してその構成を行います。
![図 - デバイスでランタイムを開始する](./media/quickstart/start-runtime.png)

IoT Edge ランタイムはすべての IoT Edge デバイスに展開されます。 これは 3 つのコンポーネントで構成されます。 **IoT Edge セキュリティ デーモン**は、Edge デバイスが起動するたびに開始され、IoT Edge エージェントを起動してデバイスをブートストラップします。 **IoT Edge エージェント**は、IoT Edge ハブなど、IoT Edge デバイス上のモジュールの展開と監視を容易にします。 **IoT Edge ハブ**は、IoT Edge デバイス上のモジュール間、およびデバイスと IoT ハブの間の通信を管理します。

ランタイムのインストール中に、デバイスの接続文字列を求められます。 Azure CLI から取得した文字列を使用してください。 この文字列によって、Azure 内の IoT Edge デバイス ID と物理デバイスとが関連付けられます。

このセクションの手順では、Linux コンテナーを使用して IoT Edge ランタイムを構成します。 Windows コンテナーを使用する場合は、「[Install Azure IoT Edge runtime on Windows to use with Windows containers (Windows コンテナーを使用するために Azure IoT Edge ランタイムを Windows にインストールする)](how-to-install-iot-edge-windows-with-windows.md)」を参照してください。

### <a name="connect-to-your-iot-edge-device"></a>IoT Edge デバイスに接続する

このセクションの手順はすべて、お使いの IoT Edge デバイスで行います。 独自のマシンを IoT Edge デバイスとして使用している場合、このパートはスキップしてかまいません。 仮想マシンまたはセカンダリ ハードウェアを使用している場合、ここでそのマシンに接続する必要があります。 

### <a name="download-and-install-the-iot-edge-service"></a>IoT Edge サービスをダウンロードしてインストールする

PowerShell を使用して IoT Edge ランタイムをダウンロードし、インストールします。 IoT Hub から取得したデバイスの接続文字列を使用して、目的のデバイスを構成します。

1. IoT Edge デバイスで、PowerShell を管理者として実行します。

2. IoT Edge サービスをデバイスにダウンロードしてインストールします。

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Manual -ContainerOs Linux
   ```

3. **DeviceConnectionString** を求められたら、前のセクションでコピーした文字列を入力します。 接続文字列の前後に引用符は含めないでください。

### <a name="view-the-iot-edge-runtime-status"></a>IoT Edge ランタイムの状態を確認する

ランタイムが正常にインストールされ、構成されていることを確認します。

1. IoT Edge サービスの状態を確認します。

   ```powershell
   Get-Service iotedge
   ```

2. サービスのトラブルシューティングが必要な場合は、サービス ログを取得します。

   ```powershell
   # Displays logs from today, newest at the bottom.

   Get-WinEvent -ea SilentlyContinue `
    -FilterHashtable @{ProviderName= "iotedged";
      LogName = "application"; StartTime = [datetime]::Today} |
    select TimeCreated, Message |
    sort-object @{Expression="TimeCreated";Descending=$false} |
    format-table -autosize -wrap
   ```

3. IoT Edge デバイス上で実行されているすべてのモジュールを表示します。 初めてサービスが開始されたので、**edgeAgent** モジュールが実行されていることのみが確認できます。 edgeAgent モジュールは既定で実行され、デバイスにデプロイする追加モジュールのインストールと起動に役立ちます。

   ```powershell
   iotedge list
   ```

   ![ご自身のデバイス上の 1 つのモジュールを表示する](./media/quickstart/iotedge-list-1.png)

IoT Edge デバイスの構成はこれで完了です。 クラウドからモジュールをデプロイして実行することができます。

## <a name="deploy-a-module"></a>モジュールを展開する

Azure IoT Edge デバイスをクラウドから管理し、IoT Hub に利用統計情報を送信するモジュールを展開します。
![図 - クラウドからデバイスにモジュールを展開する](./media/quickstart/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>生成されたデータを表示する

このクイック スタートでは、新しい IoT Edge デバイスを作成し、そこに IoT Edge ランタイムをインストールしました。 その後、Azure Portal を使用して、IoT Edge モジュールをプッシュし、デバイス自体を変更せずにモジュールをデバイスで実行しました。 この場合は、プッシュしたモジュールによって、チュートリアルで使用できる環境データが作成されます。

IoT Edge デバイスで、クラウドからデプロイされたモジュールが実行されていることを確認します。

```powershell
iotedge list
```

   ![ご利用のデバイスで 3 つのモジュールを表示する](./media/quickstart/iotedge-list-2.png)

tempSensor モジュールからクラウドに送信されているメッセージを確認します。

```powershell
iotedge logs tempSensor -f
```

  ![モジュールからのデータを表示する](./media/quickstart/iotedge-logs.png)

[Visual Studio Code 用の Azure IoT Hub Toolkit の拡張機能](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (旧称: Azure IoT Toolkit 拡張機能) を使用して、IoT ハブに到着したメッセージを監視することもできます。 

## <a name="clean-up-resources"></a>リソースのクリーンアップ

IoT Edge のチュートリアルに進む場合は、このクイック スタートで登録および設定したデバイスを使用できます。 それ以外の場合は、作成した Azure リソースを削除し、デバイスから IoT Edge ランタイムを削除することができます。

### <a name="delete-azure-resources"></a>Azure リソースを削除する

新しいリソース グループで仮想マシンと IoT ハブを作成した場合、そのグループと関連するすべてのリソースを削除できます。 リソース グループの内容を再確認して、残しておくべきものがないことを確認してください。 グループ全体を削除したくない場合は、リソースを個別に削除してもかまいません。

**IoTEdgeResources** グループを削除します。

   ```azurecli-interactive
   az group delete --name IoTEdgeResources
   ```

### <a name="remove-the-iot-edge-runtime"></a>IoT Edge ランタイムを削除する

今後のテストに IoT Edge デバイスを使用する予定があるものの、IoT ハブを使用していない間は tempSensor モジュールからのデータの送信を停止したいという場合は、次のコマンドを使用して、IoT Edge サービスを停止します。

   ```powershell
   Stop-Service iotedge -NoWait
   ```

テストを再開する準備ができたら、サービスを再起動することができます。

   ```powershell
   Start-Service iotedge
   ```

デバイスからインストールを削除するには、次のコマンドを使用します。  

IoT Edge ランタイムを削除します。

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Uninstall-SecurityDaemon
   ```

IoT Edge ランタイムが削除されると、作成したコンテナーは停止されますが、デバイスには残っています。 すべてのコンテナーを表示します。

   ```powershell
   docker ps -a
   ```

IoT Edge ランタイムによってデバイス上に作成されたコンテナーを削除します。 tempSensor コンテナーを別の名前で呼ぶ場合は、名前を変更します。

   ```powershell
   docker rm -f tempSensor
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```
   
## <a name="next-steps"></a>次の手順

このクイック スタートでは、新しい IoT Edge デバイスを作成し、Azure IoT Edge クラウド インターフェイスを使用してコードをデバイスにデプロイしました。 その環境に関する生データを生成するテスト デバイスができあがりました。

これで、引き続き他のチュートリアルを実行し、Azure IoT Edge が、エッジでこのデータをビジネスに関する分析情報に変えるうえで、どのように役立つかを確認する準備が整いました。

> [!div class="nextstepaction"]
> [Azure Function を使用してセンサー データをフィルター処理する](tutorial-deploy-function.md)
