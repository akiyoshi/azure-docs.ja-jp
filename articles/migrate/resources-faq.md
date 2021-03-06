---
title: Azure Migrate - よく寄せられる質問 (FAQ) | Microsoft Docs
description: Azure Migrate についてよく寄せられる質問に対応します
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: snehaa
ms.openlocfilehash: ebc4393341341b3b73165a166a650ae1a6f431ff
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2018
ms.locfileid: "53257796"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure Migrate - よく寄せられる質問 (FAQ)

この記事には、Azure Migrate に関してよく寄せられる質問が含まれます。 この記事の内容についてさらに質問がある場合は、[Azure Migrate フォーラム](https://aka.ms/AzureMigrateForum)に投稿してください。

## <a name="general"></a>全般

### <a name="does-azure-migrate-support-assessment-of-only-vmware-workloads"></a>Azure Migrate がサポートするのは VMware ワークロードのアセスメントだけですか。

はい、Azure Migrate は現在、VMware ワークロードのアセスメントのみをサポートしています。 将来的には、Hyper-V および物理サーバーのサポートが計画されています。

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Azure Migrate が VMware 環境を検出するために vCenter Server は必要ですか。

はい、Azure Migrate が VMware 環境を検出するには vCenter Server が必要です。 vCenter Server で管理されていない ESXi ホストの検出はサポートされていません。

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Azure Migrate と Azure Site Recovery の違いは何ですか。

Azure Migrate は、オンプレミスのワークロードを検出し、Azure への移行の計画を支援するアセスメント サービスです。 [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) は、ディザスター リカバリー ソリューションであると同時に、オンプレミスのワークロードを Azure 内の IaaS VM に移行するのを支援します。

### <a name="whats-the-difference-between-using-azure-migrate-for-assessments-and-the-map-toolkit"></a>Azure Migrate をアセスメントに使用することと Map Toolkit を使用することの違いは何ですか。

[Azure Migrate](migrate-overview.md) が提供する移行アセスメントは、具体的には、オンプレミス ワークロードを Azure に移行するための準備および評価を支援します。 [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=7826) には他の機能があります。 たとえば、Windows クライアントおよびサーバー オペレーティング システムの新しいバージョンの移行計画、ソフトウェア使用状況の追跡などです。そのような目的には、引き続き MAP Toolkit を使用します。


### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>Azure Migrate と Azure Site Recovery Deployment Planner の違いは何ですか。

Azure Migrate は移行計画ツールで、Azure Site Recovery Deployment Planner はディザスター リカバリー (DR) 計画ツールです。

**VMware から Azure への移行**:オンプレミスのワークロードを Azure に移行する場合は、Azure Migrate を使用して移行を計画します。 Azure Migrate はオンプレミスのワークロードを評価し、ガイダンス、分析情報、メカニズムを提供して、Azure への移行を支援します。 移行計画の準備ができたら、Azure Site Recovery や Azure Database Migration Service などのサービスを使用して、Azure にマシンを移行できます。

**Hyper-V から Azure への移行**:Azure Migrate は現在、Azure への移行のための VMware 仮想マシンのアセスメントのみをサポートしています。 Azure Migrate では現在、Hyper-V をサポートするよう進めております。 その間は Site Recovery Deployment Planner を利用できます。 Azure Migrate で Hyper-V のサポートが有効になったら、Azure Migrate を使用して Hyper-V のワークロードの移行を計画できます。

**VMware/Hyper-V から Azure へのディザスター リカバリー**:Azure Site Recovery (Site Recovery) を使用して Azure のディザスター リカバリー (DR) を実行する場合は、Site Recovery Deployment Planner を使用して DR を計画します。 Site Recovery Deployment Planner はお使いのオンプレミス環境の ASR 固有の詳細なアセスメントを実行します。 仮想マシンのレプリケーション、フェールオーバーなど、DR 操作が適切に実行されるために Site Recovery によって要求されるレコメンデーションを提供します。  

### <a name="which-azure-geographies-are-supported-by-azure-migrate"></a>Azure Migrate でサポートされている Azure の地域を教えてください。

Azure Migrate では現在、プロジェクトの地域として米国と Azure Government がサポートされています。 移行プロジェクトの作成先はこれらの地域に限られていますが、評価するマシンは、[複数のターゲットの場所](https://docs.microsoft.com/azure/migrate/how-to-modify-assessment#edit-assessment-properties)に存在していてもかまいません。 プロジェクトの地域は、検出されたメタデータを格納するためにのみ使用されます。

**地理的な場所** | **メタデータ ストレージの場所**
--- | ---
米国 | 米国中西部または米国東部
Azure Government | 米国政府バージニア州

### <a name="how-does-the-on-premises-site-connect-to-azure-migrate"></a>オンプレミス サイトはどのようにして Azure Migrate に接続しますか。

インターネット経由の接続、または ExpressRoute とパブリック ピアリングを使用した接続が可能です。

### <a name="can-i-harden-the-vm-set-up-with-the-ova-template"></a>OVA テンプレートを使用して VM セットアップを強化できますか。

Azure Migrate アプライアンスの動作に必要な通信およびファイアウォール規則が変わらない限り、(ウイルス対策などの) 追加コンポーネントを OVA テンプレートに追加することができます。   

## <a name="discovery"></a>探索

### <a name="what-data-is-collected-by-azure-migrate"></a>Azure Migrate によって収集されるデータは何ですか。

Azure Migrate は、アプライアンスベースの検出とエージェントベースの検出の 2 種類の検出をサポートしています。
アプライアンスベースの検出はオンプレミスの VM に関するメタデータを収集します。以下にアプライアンスによって収集されるメタデータの全一覧を示します。

**VM の構成データ**
- (vCenter 上の) VM の表示名
- VM のインベントリ パス (vCenter でのホスト/クラスター/フォルダー)
- IP アドレス
- MAC アドレス
- オペレーティング システム
- コア、ディスク、NIC の数
- メモリ サイズ、ディスク サイズ

**VM のパフォーマンス データ**
- CPU 使用率
- メモリ使用量
- VM に接続されている各ディスクの場合:
  - ディスク読み取りスループット
  - ディスク書き込みスループット
  - ディスク読み取り操作/秒
  - ディスク書き込み操作/秒
- VM に接続されている各ネットワーク アダプターの場合:
  - ネットワーク受信
  - ネットワーク送信

エージェントベースの検出は、アプライアンスベースの検出に加えて利用できるオプションで、ユーザーがオンプレミスの VM の[依存関係を視覚化する](how-to-create-group-machine-dependencies.md)のに役立ちます。 依存関係エージェントは FQDN、OS、IP アドレス、MAC アドレス、VM 内で実行されているプロセスや VM の受信/送信 TCP 接続などの詳細を収集します。 エージェントベースの検出は省略可能で、VM の依存関係を視覚化する必要がない場合は、エージェントをインストールしないことを選択できます。

### <a name="would-there-be-any-performance-impact-on-the-analyzed-esxi-host-environment"></a>分析対象となる ESXi ホスト環境へのパフォーマンスへの影響はありますか?

[1 回限りの検出によるアプローチ](https://docs.microsoft.com/azure/migrate/concepts-collector#discovery-methods)の場合、パフォーマンス データを収集するためには、vCenter Server の統計情報レベルが 3 に設定されている必要があると考えられます。 このレベルに設定した場合、大量のトラブルシューティング データが収集されますが、それらのデータは vCenter Server データベースに格納されます。 それが引き金となって、vCenter Server にパフォーマンスの問題が生じる可能性はあります。 ESXi ホストへの影響はごくわずかだと思われます。

現在はプレビュー段階ですが、パフォーマンス データの継続的プロファイリングを導入しました。 継続的プロファイリングでは、パフォーマンス ベースの評価を実行するために、vCenter Server の統計情報レベルを変更する必要はありません。 コレクター アプライアンスがオンプレミス マシンをプロファイリングして、仮想マシンのパフォーマンス データを測定することになります。 これにより、ESXi ホストだけでなく、vCenter Server についても、パフォーマンスへの影響がほぼゼロになると考えられます。

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>収集されたデータはどこに、どれくらいの期間保存されますか。

コレクター アプライアンスによって収集されたデータは、移行プロジェクトの作成中に指定した Azure の場所に格納されます。 データは Microsoft サブスクリプションに安全に格納され、ユーザーがその Azure Migrate プロジェクトを削除すると削除されます。

依存関係の視覚化で、VM にエージェントをインストールした場合、依存関係エージェントによって収集されたデータは、ユーザーのサブスクリプションで作成された米国の Log Analytics ワークスペースに格納されます。 サブスクリプションの Log Analytics ワークスペースを削除すると、このデータは削除されます。 [詳細情報](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization)。

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>保存データや転送中のデータは暗号化されますか。

はい、収集された保存データや転送中のデータは暗号化されます。 アプライアンスによって収集されたメタデータは、https を介してインターネット上で安全に Azure Migrate サービスに送信されます。 収集されたメタデータは Microsoft サブスクリプション内の [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) と [Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) に格納され、保存データは暗号化されます。

依存関係エージェントによって収集されたデータも転送中に暗号化され (安全な https チャネル)、ユーザーのサブスクリプションの Log Analytics ワークスペースに格納されます。 また、保存データも暗号化されます。

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>コレクターは vCenter Server や Azure Migrate サービスとどのように通信しますか。

コレクター アプライアンスは、そのアプライアンス内のユーザーが提供した資格情報を使用して、vCenter Server (ポート 443) に接続します。 VMware PowerCLI を使用して vCenter Server を照会し、vCenter Server によって管理される VM に関するメタデータを収集します。 VM の構成データ (コア数、メモリ、ディスク数、NIC など) のほか、過去 1 か月の各 VM のパフォーマンス履歴を vCenter Server から収集します。 その後、収集されたメタデータが Azure Migrate サービスに (https を介してインターネット上で) 送信され、アセスメントが行われます。 [詳細情報](concepts-collector.md)

### <a name="can-i-connect-the-same-collector-appliance-to-multiple-vcenter-servers"></a>同じコントローラー アプライアンスを複数の vCenter Server に接続できますか。

はい、1 台のコレクター アプライアンスを使用して複数の vCenter Server を検出できますが、同時検出はできません。 1 つずつ検出を実行する必要があります。

### <a name="is-the-ova-template-used-by-site-recovery-integrated-with-the-ova-used-by-azure-migrate"></a>Site Recovery によって使用される OVA テンプレートは、Azure Migrate によって使用される OVA と統合されますか。

現在、統合はありません。 Site Recovery の .OVA テンプレートは、VMware VM/物理サーバー レプリケーションのための Site Recovery 構成サーバーをセットアップするために使用されます。 Azure Migrate によって使用される .OVA は、移行アセスメントの目的で、vCenter Server によって管理されている VMware VM を検出するために使用されます。

### <a name="i-changed-my-machine-size-can-i-rerun-the-assessment"></a>マシン サイズを変更しました。 アセスメントを再実行できますか。

評価する VM の設定を変更する場合は、コレクター アプライアンスを使用して検出をもう一度トリガーします。 アプライアンスで、**[コレクションをもう一度開始します]** オプションを使用してこの操作を実行します。 コレクションが完了したら、ポータルで評価の **[再計算]** オプションを選択して、更新された評価結果を取得します。

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>Azure Migrate でマルチテナント環境を検出するにはどうすればよいですか。

ある環境がテナント間で共有されており、別のテナントのサブスクリプションにある特定のテナントの VM を検出しないようにするには、コレクター アプライアンスのスコープ フィールドを使用して、検出範囲を指定します。 テナントがホストを共有している場合は、特定のテナントに属する VM のみに読み取り専用アクセス権を持つ資格情報を作成してから、コレクター アプライアンスでこの資格情報を使用し、スコープをホストとして指定して検出を実行します。 または、vCenter Server で共有ホストの下にフォルダー (tenant1 の場合は folder1、tenant2 の場合は folder2 とします) を作成し、tenant1 の VM を folder1 に、tenant2 の VM を folder2 に移動してから、適切なフォルダーを指定して、適宜、コレクターの検出範囲を指定することもできます。

### <a name="how-many-virtual-machines-can-be-discovered-in-a-single-migration-project"></a>1 つの移行プロジェクトで検出できる仮想マシンの数はいくつですか。

1 つの移行プロジェクトで 1500 台の仮想マシンを検出できます。 オンプレミス環境に複数のマシンがある場合は、Azure Migrate で大規模な環境を検索する方法の[詳細](how-to-scale-assessment.md)を確認してください。

## <a name="assessment"></a>評価

### <a name="does-azure-migrate-support-enterprise-agreement-ea-based-cost-estimation"></a>Azure Migrate は Enterprise Agreement (EA) に基づくコスト見積もりをサポートしていますか。

Azure Migrate は現在、[Enterprise Agreement プラン](https://azure.microsoft.com/offers/enterprise-agreement-support/)のコスト見積もりをサポートしていません。 これを回避するには、プランに従量課金制を指定し、評価のプロパティの [割引] フィールドに、(サブスクリプションに適用される) 割引率を手動で指定します。

  ![Discount](./media/resources-faq/discount.png)


## <a name="dependency-visualization"></a>依存関係の視覚化

> [!NOTE]
> 依存関係可視化機能は、Azure Government では使用できません。

### <a name="what-is-dependency-visualization"></a>依存関係の視覚化とは何ですか。

依存関係の視覚化により、アセスメントを実行する前にマシンの依存をクロスチェックすることで、移行対象の VM のグループの評価の信頼性を高めることができます。 依存関係の視覚化により、後に何も残さないようにし、Azure への移行時に予期しない障害を回避することに役立ちます。 Azure Migrate は、依存関係を視覚化できるように Log Analytics の Service Map ソリューションを活用します。

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>依存関係の視覚化機能の利用には料金の支払いが発生しますか。

いいえ。 Azure Migrate の価格については、[こちら](https://azure.microsoft.com/pricing/details/azure-migrate/)を参照してください。

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>依存関係の視覚化のために何をインストールする必要がありますか。

依存関係の視覚化を使用するには、評価するオンプレミスの各マシンにエージェントをダウンロードしてインストールする必要があります。

- 各マシンに [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) をインストールする必要があります。
- 各マシンに [Dependency Agent](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure) をインストールする必要があります。
- また、インターネットに接続されていないマシンの場合、それらに Log Analytics ゲートウェイをダウンロードしてインストールする必要があります。

依存関係の視覚化を使用している場合を除き、評価するマシンにこれらのエージェントは不要です。

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>依存関係の視覚化に既存のワークスペースを使用できますか。

はい。現在は、Azure Migrate で既存のワークスペースを移行プロジェクトにアタッチして、依存関係の視覚化に活用することができます。 [詳細情報](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization#how-does-it-work)。

### <a name="can-i-export-the-dependency-visualization-report"></a>依存関係の視覚化のレポートはエクスポートできますか。

いいえ。依存関係の視覚化はエクスポートできません。 ただし、Azure Migrate では依存関係の視覚化に Service Map が使用されているため、[Service Map REST API](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) を使用すると、JSON 形式で依存関係を取得できます。

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>Microsoft Monitoring Agent (MMA) と依存関係エージェントのインストールを自動化するには、どうすればよいですか。

依存関係エージェントのインストールには、[こちら](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples)のスクリプトをご利用ください。 MMA については、TechNet で提供されている[こちら](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab)のスクリプトをご活用ください。

スクリプトに加え、System Center Configuration Manager (SCCM)、[Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) などのデプロイ ツールを利用して、エージェントをデプロイすることもできます。

### <a name="what-are-the-operating-systems-supported-by-mma"></a>MMA でサポートされるオペレーティング システムは何ですか？

MMA でサポートされる Windows オペレーティング システムの一覧は[ここ](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems)にあります。
MMA でサポートされる Linux オペレーティング システムの一覧は[ここ](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems)にあります。

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>依存関係エージェントでサポートされるオペレーティング システムは何ですか？

依存関係エージェントでサポートされる Windows オペレーティング システムの一覧は[ここ](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems)にあります。
依存関係エージェントでサポートされる Linux オペレーティング システムの一覧は[ここ](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems)にあります。

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>Azure Migrate で 1 時間を超えて依存関係を視覚化することはできますか。
いいえ。Azure Migrate で依存関係を視覚化できる期間は、最大 1 時間です。 Azure Migrate を使用すると、過去 1 か月までの特定の日付に戻ることができますが、依存関係を視覚化できる期間は最大 1 時間です。 たとえば、依存関係マップにある期間機能を使用して、昨日の依存関係を表示することが可能ですが、1 時間ウィンドウの表示のみできます。

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-10-vms"></a>依存関係の視覚化では、10 を超える VM を含むグループはサポートされていますか。
最大 10 個 の VM を含むグループについて[依存関係を視覚化](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies)できます。グループに含まれる VM が 10 個 を超える場合は、そのグループを小さなグループに分割して、依存関係を視覚化することをお勧めします。


## <a name="next-steps"></a>次の手順

- [Azure Migrate](migrate-overview.md) の概要を確認する
- VMware 環境での[検出とアセスメント](tutorial-assessment-vmware.md)の方法を確認しする
