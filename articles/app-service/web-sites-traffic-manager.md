---
title: Traffic Manager によるトラフィックの制御 - Azure App Service
description: この記事では、Azure App Service に関連する Azure Traffic Manager の概要を説明します。
services: app-service\web
documentationcenter: ''
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: dabda633-e72f-4dd4-bf1c-6e945da456fd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 85405a96d141188203ddc88410c96654667fa83a
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53270112"
---
# <a name="controlling-azure-app-service-traffic-with-azure-traffic-manager"></a>Azure Traffic Manager による Azure App Service トラフィックの制御
> [!NOTE]
> この記事では、Azure App Service に関連する Microsoft Azure Traffic Manager の概要を説明します。 "Azure トラフィック マネージャー自体の詳細については、この記事の最後にある関連情報を参照してください。
> 
> 

## <a name="introduction"></a>はじめに
Azure Traffic Manager を使用すると、Web クライアントからの要求を Azure App Service のアプリに振り分ける方法を制御できます。 App Service のエンドポイントが Azure Traffic Manager のプロファイルに追加されると、Azure Traffic Manager は App Service アプリの状態 (実行中、停止、または削除済み) を追跡して、トラフィックを受信する必要のあるエンドポイントを決定できるようになります。

## <a name="routing-methods"></a>ルーティング方法
Azure Traffic Manager は 4 つのルーティング方法を使用します。 Azure App Service に関連する、これらの方法については、次の一覧で説明します。

* **[優先順位](../traffic-manager/traffic-manager-routing-methods.md#priority):** すべてのトラフィックにプライマリ アプリケーションを使用し、プライマリまたはバックアップ アプリケーションが使用できなくなった場合に備えてバックアップを用意します。
* **[重み付け](../traffic-manager/traffic-manager-routing-methods.md#weighted):** 一連のアプリに、均等にまたは定義した重みに従ってトラフィックを分散させます。
* **[パフォーマンス](../traffic-manager/traffic-manager-routing-methods.md#performance):** 異なる地理的位置にアプリケーションがある場合は、最短のネットワーク待機時間という観点から "最も近い" アプリケーションを使用します。
* **[地域](../traffic-manager/traffic-manager-routing-methods.md#geographic):** ユーザーの DNS クエリが送信された地理的位置に基づいて、ユーザーを特定のアプリケーションにルーティングします。 

詳細については、「[Traffic Manager のルーティング方法](../traffic-manager/traffic-manager-routing-methods.md)」を参照してください。

## <a name="app-service-and-traffic-manager-profiles"></a>App Service と Azure Traffic Manager のプロファイル
App Service アプリのトラフィック制御を構成するには、Azure Traffic Manager でプロファイルを作成し、前に説明している 3 つの負荷分散方法のいずれかをプロファイルで指定します。その後、トラフィックを制御するエンドポイント (この場合は App Service) をプロファイルに追加します。 アプリの状態 (実行中、停止、または削除済み) は定期的にプロファイルに反映されて、その状態に応じて Azure Traffic Manager はトラフィックを振り分けることができます。

Azure トラフィック マネージャーを Azure で使用する場合は、次の点に留意してください。

* 同じリージョンでのアプリのみの展開の場合、App Service はそのモードには関係なく、フェールオーバーとラウンド ロビンの機能を既に備えています。
* 同じリージョンでの App Service のデプロイで、Azure の別のクラウド サービスと連携させる場合、両方の種類のエンドポイントを組み合わせたハイブリッドのシナリオが可能です。
* リージョンごとに 1 つのみの App Service エンドポイントをプロファイルで指定することもできます。 1 つのリージョンのエンドポイントとしてアプリを選択すると、そのリージョン内の残りの アプリはそのプロファイルで選択できなくなります。
* Azure Traffic Manager のプロファイルで指定した App Service エンドポイントは、プロファイルでアプリの構成ページの **[ドメイン名]** セクションに表示されますが、そこでは構成できません。
* アプリをプロファイルに追加した後、アプリのポータル ページのダッシュボードの **[サイトの URL]** には、アプリのカスタム ドメインを設定していればその URL が表示されます。 それ以外の場合は、Traffic Manager のプロファイルの URL (`contoso.trafficmanager.net` など) が表示されます。 アプリの直接のドメイン名と Traffic Manager の URL の両方が、アプリの構成ページの **[ドメイン名]** セクションに表示されます。
* カスタム ドメイン名は予期したとおりに機能しますが、それらのドメイン名をアプリに追加するだけでなく、Traffic Manager の URL を参照するように DNS マップを構成する必要もあります。 App Service アプリのカスタム ドメインを設定する方法については、「[既存のカスタム DNS 名を Azure Web Apps にマップする](app-service-web-tutorial-custom-domain.md)」を参照してください。
* 標準またはプレミアム モードのアプリのみを Azure Traffic Manager のプロファイルに追加できます。

## <a name="next-steps"></a>次の手順
Azure トラフィック マネージャーの概念と技術的概要については、「 [Traffic Manager Overview (トラフィック マネージャーの概要)](../traffic-manager/traffic-manager-overview.md)」を参照してください。

App Service での Traffic Manager の使用の詳細については、ブログ記事「[Using Azure Traffic Manager with Azure Web Sites (Azure Web サイトでの Azure Traffic Manager の使用)](https://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx)」および「[Azure Traffic Manager can now integrate with Azure Web Sites (Azure Traffic Manager と Azure Web サイトの統合が可能になりました)](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/)」を参照してください。

