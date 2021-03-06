---
title: .NET Core コンソール アプリケーションを作成して Azure Cosmos DB SQL API アカウントのデータを管理する (SDK バージョン 3 プレビュー)
description: Azure Cosmos DB SQL API .NET Core SDK を使用してオンライン データベースと C# コンソール アプリケーションを作成するチュートリアル。
author: deborahc
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/01/2018
ms.author: dech
ms.openlocfilehash: 92dcd62fa0079b22ba6a2721959a200c2556a4c1
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2018
ms.locfileid: "52852592"
---
# <a name="build-a-net-core-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account-sdk-version-3-preview"></a>.NET Core コンソール アプリケーションを作成して Azure Cosmos DB SQL API アカウントのデータを管理する (SDK バージョン 3 プレビュー)

> [!div class="op_single_selector"]
> * [.NET Core (プレビュー)](sql-api-dotnet-core-get-started-preview.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET (プレビュー)](sql-api-dotnet-get-started-preview.md)
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

.NET Core で Azure Cosmos DB SQL API を使用するチュートリアルへようこそ。 このチュートリアルに従うことで、Azure Cosmos DB リソースを作成し、クエリする .NET Core コンソール アプリケーションを準備することができます。 このチュートリアルでは、[.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) をターゲットとする Azure Cosmos DB .NET SDK の[バージョン 3.0 以降](https://www.nuget.org/packages/Microsoft.Azure.Cosmos)を使用します。

このチュートリアルの内容:

> [!div class="checklist"]
> * Azure Cosmos アカウントを作成し、アカウントに接続する
> * Visual Studio でプロジェクトを構成する
> * データベースとコンテナーを作成する
> * コンテナーに項目を追加する
> * コンテナーにクエリを実行する
> * 項目に対する CRUD 操作
> * データベースを削除する


アプリケーションを作成する時間がありませんか。 心配はありません。 [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-core-getting-started) で完全なソリューションを入手できます。 簡単な手順については「[完全なソリューションの取得](#GetSolution)」を参照してください。

SQL API および .NET Core SDK を使用して Xamarin iOS、Android、またはフォーム アプリケーションを作成する場合は、 「[Xamarin と Azure Cosmos DB を使用したモバイル アプリケーションの構築](mobile-apps-with-xamarin.md)」を参照してください。

## <a name="prerequisites"></a>前提条件

* アクティブな Azure アカウントアカウントがない場合、Azure 試用版にサインアップして、最大 10 件の無料 Mobile Apps を入手できます。 お持ちでない場合は、 [無料アカウント](https://azure.microsoft.com/free/)にサインアップしてください。 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* まだ Visual Studio 2017 をインストールしていない場合は、[無料](https://www.visualstudio.com/downloads/)の Visual Studio 2017 Community エディションをダウンロードして使用できます。 ユニバーサル Windows プラットフォーム (UWP) アプリを開発している場合は、**バージョン 15.4 以降の Visual Studio 2017** を使う必要があります。 Visual Studio のセットアップ中に、必ず **[Azure の開発]** ワークロードを有効にしてください。

    * MacOS または Linux で作業している場合、選択したプラットフォーム用の [.NET Core SDK](https://www.microsoft.com/net/core#macos) をインストールすることで、コマンド ラインから .NET Core アプリを開発できます。 

    * Windows で作業している場合、[.NET Core SDK](https://www.microsoft.com/net/core#windows) をインストールすることで、コマンド ラインから .NET Core アプリを開発できます。 

    * 独自のエディターを使用するか、[Visual Studio Code](https://code.visualstudio.com/) をダウンロードできます。Visual Studio Code は無料で、Windows、Linux、MacOS で動作します。 

## <a name="step-1-create-an-azure-cosmos-db-account"></a>手順 1: Azure Cosmos DB アカウントを作成する

それでは、Azure Cosmos DB アカウントを作成してみましょう。 使用するアカウントが既にある場合は、「 [Visual Studio ソリューションをセットアップする](#SetupVS)」に進んでかまいません。 Azure Cosmos DB Emulator を使用する場合は、[Azure Cosmos DB Emulator](local-emulator.md) に関する記事に記載されている手順に従ってエミュレーターをセットアップし、「[Visual Studio ソリューションをセットアップする](#SetupVS)」に進んでください。

[!INCLUDE [cosmos-db-create-dbaccount-preview](../../includes/cosmos-db-create-dbaccount-preview.md)]

## <a name="step-1-create-an-azure-cosmos-db-account"></a>手順 1: Azure Cosmos DB アカウントを作成する
それでは、Azure Cosmos DB アカウントを作成してみましょう。 使用するアカウントが既にある場合は、「 [Visual Studio ソリューションをセットアップする](#SetupVS)」に進んでかまいません。 Azure Cosmos DB Emulator を使用する場合は、[Azure Cosmos DB Emulator](local-emulator.md) に関する記事に記載されている手順に従ってエミュレーターをセットアップし、「[Visual Studio プロジェクトをセットアップする](#SetupVS)」に進んでください。

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>手順 2: Visual Studio プロジェクトをセットアップする
1. コンピューターで **Visual Studio 2017** を開きます。
1. **[ファイル]** メニューで、**[新規]**、**[プロジェクト]** の順に選択します。
1. **[新しいプロジェクト]** ダイアログで、**[Visual C#]** / **[コンソール アプリ (.NET Core)]** の順に選択し、プロジェクトの名前を指定して、**[OK]** をクリックします。
    ![[新しいプロジェクト] ウィンドウのスクリーン ショット](./media/sql-api-dotnetcore-get-started/dotnetcore-tutorial-visual-studio-new-project.png)
1. **ソリューション エクスプローラー**で、Visual Studio ソリューションの下にある新しいコンソール アプリケーションを右クリックし、**[NuGet パッケージの管理]** をクリックします。
    
    ![プロジェクトの右クリック メニューのスクリーン ショット](./media/sql-api-dotnetcore-get-started/dotnetcore-tutorial-visual-studio-manage-nuget.png)
1. **[NuGet]** タブの **[参照]** をクリックし、検索ボックスに「**Microsoft.Azure.Cosmos**」と入力します。
1. 結果の中から **Microsoft.Azure.Cosmos** を探し、**[インストール]** をクリックします。
   Azure Cosmos DB SQL API クライアント ライブラリのパッケージ ID は [Microsoft Azure Cosmos DB クライアント ライブラリ](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) です。
   ![Azure Cosmos DB クライアント SDK を見つける NuGet メニューのスクリーン ショット](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget-2.png)

    ソリューションの変更の確認に関するメッセージが表示されたら、**[OK]** をクリックします。 ライセンスの同意に関するメッセージが表示されたら、**[同意する]** をクリックします。

これでセットアップは終了です。 いくつかのコードの記述を開始しましょう。 このチュートリアルの完成したコード プロジェクトは [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-core-getting-started/)にあります。

## <a id="Connect"></a>手順 3: Azure Cosmos DB アカウントに接続する
1. まず、**Program.cs** ファイルで、C# アプリケーションの先頭にある参照を、以下の参照で置き換えます。
    ```csharp
    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.Cosmos;
    using System.Collections.Generic;
    using System.Net;
    ```
    
1. 次に、パブリック クラス ``Program`` に次の定数と変数を追加します。
    ```csharp
    public class Program
    {
        // ADD THIS PART TO YOUR CODE

        // The Azure Cosmos DB endpoint for running this sample.
        private static readonly string EndpointUri = "<your endpoint here>";
        // The primary key for the Azure Cosmos account.
        private static readonly string PrimaryKey = "<your primary key>";

        // The Cosmos client instance
        private CosmosClient cosmosClient;

        // The database we will create
        private CosmosDatabase database;

        // The container we will create.
        private CosmosContainer container;

        // The name of the database and container we will create
        private string databaseId = "FamilyDatabase";
        private string containerId = "FamilyContainer";
    }
    ```
    以前のバージョンの .NET SDK に精通している場合は、"コレクション" や "ドキュメント" という用語をよく目にしたことに注意してください。 Azure Cosmos DB では複数の API モデルをサポートしているため、バージョン 3.0 以降の .NET SDK では、"コンテナー" と "項目" という一般的な用語が使用されています。 コンテナーは、コレクション、グラフ、またはテーブルを表します。 項目は、ドキュメント、エッジ/頂点、行など、コンテナー内の内容を表します。 [データベース、コンテナー、項目についてはこちらを参照してください。](databases-containers-items.md)

1. エンドポイント URL とプライマリ キーを [Azure portal](https://portal.azure.com) で取得します。

    Azure Portal で Azure Cosmos DB アカウントに移動し、**[キー]** をクリックします。

    ポータルから URI をコピーし、```Program.cs``` ファイルの `<your endpoint URL>` に貼り付けます。 ポータルからプライマリ キーをコピーし、`<your primary key>` に貼り付けます。

   ![Azure portal から Azure Cosmos DB キーを取得するスクリーン ショット](./media/sql-api-get-started/dotnet-tutorial-portal-keys.png)

1. 次に、```CosmosClient``` の新しいインスタンスを作成し、プログラムのためにいくつかのスキャフォールディングを設定します。

    **Main** メソッドの下に、**GetStartedDemoAsync** という新しい非同期タスクを追加します。これによって新しい ```CosmosClient``` はインスタンス化されます。 Azure Cosmos DB リソース上で実行するメソッドを呼び出すエントリ ポイントとして **GetStartedDemoAsync** を使用します。

    ```csharp
    public static async Task Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    /*
        Entry point to call methods that operate on Azure Cosmos DB resources in this sample
    */   
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
    }
    ```

1. 次のコードを追加して、**Main** メソッドから **GetStartedDemoAsync** 非同期タスクを実行します。 **Main** メソッドは例外をキャッチし、コンソールに書き込みます。
    ```csharp
    public static async Task Main(string[] args)
    {
        // ADD THIS PART TO YOUR CODE
        try
        {
            Console.WriteLine("Beginning operations...\n");
            Program p = new Program();
            await p.GetStartedDemoAsync();
        }
        catch (CosmosException de)
        {
            Exception baseException = de.GetBaseException();
            Console.WriteLine("{0} error occurred: {1}\n", de.StatusCode, de);
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: {0}\n", e);
        }
        finally
        {
            Console.WriteLine("End of demo, press any key to exit.");
            Console.ReadKey();
        }
    }
    ```

1. **F5** キーを押してアプリケーションを実行します。 コンソール ウィンドウの出力には、Azure Cosmos DB との接続が確立されたことを示す、`End of demo, press any key to exit.` というメッセージが表示されます。 表示されたら、コンソール ウィンドウを閉じます。 

お疲れさまでした。 これで、Azure Cosmos DB アカウントに接続しました。 

## <a name="step-4-create-a-database"></a>手順 4: データベースを作成する
データベースは、``Databases`` クラスの [**CreateDatabaseIfNotExistsAsync**](https://aka.ms/CosmosDotnetAPIDocs) または [**CreateDatabaseAsync**](https://aka.ms/CosmosDotnetAPIDocs) 関数を使用して作成できます。 データベースは、コンテナーに分割された項目の論理上のコンテナーです。
    
1. **CreateDatabase** メソッドをコピーして、**GetStartedDemoAsync** メソッドの下に貼り付けます。 **CreateDatabase** によって、``databaseId`` フィールドに指定された id で新しいデータベース ``FamilyDatabase`` が作成されます (このデータベースがまだ存在していない場合)。 

    ```csharp
    /*
        Create the database if it does not exist
    */    
    private async Task CreateDatabase()
    {
        // Create a new database
        this.database = await this.cosmosClient.Databases.CreateDatabaseIfNotExistsAsync(databaseId);
        Console.WriteLine("Created Database: {0}\n", this.database.Id);
    }
    ```

1. CosmosClient をインスタンス化したところに、次のコードをコピーして貼り付けて、追加した **CreateDatabase** メソッドを呼び出します。

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);

        //ADD THIS PART TO YOUR CODE
        await this.CreateDatabase(); 
    }
    ```

    この時点で、エンドポイントと主キーが入力され、コードは次のようになります。 名前空間はプロジェクトの名前によって変わる点に注意してください。
    ```csharp
    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.Cosmos;
    using System.Collections.Generic;
    using System.Net;

    namespace CosmosGettingStartedDotnetCoreTutorial
    {
        class Program
        {
            // The Azure Cosmos DB endpoint for running this sample.
            private static readonly string EndpointUri = "<your endpoint here>";
            // The primary key for the Azure Cosmos account.
            private static readonly string PrimaryKey = "<your primary key>";

            // The Cosmos client instance
            private CosmosClient cosmosClient;
        
            // The database we will create
            private CosmosDatabase database;

            // The container we will create.
            private CosmosContainer container;

            // The name of the database and container we will create
            private string databaseId = "FamilyDatabase";
            private string containerId = "FamilyContainer";

            public static async Task Main(string[] args)
            {
                try
                {
                    Console.WriteLine("Beginning operations...");
                    Program p = new Program();
                    await p.GetStartedDemoAsync();
                }
                catch (CosmosException de)
                {
                    Exception baseException = de.GetBaseException();
                    Console.WriteLine("{0} error occurred: {1}\n", de.StatusCode, de);
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: {0}\n", e);
                }
                finally
                {
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
                }
            }

            /*
                Entry point to call methods that operate on Azure Cosmos DB resources in this sample
            */
            public async Task GetStartedDemoAsync()
            {
                // Create a new instance of the Cosmos Client
                this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
                await this.CreateDatabase();
            }

            /*
                Create the database if it does not exist
            */    
            private async Task CreateDatabase()
            {
                // Create a new database
                this.database = await this.cosmosClient.Databases.CreateDatabaseIfNotExistsAsync(databaseId);
                Console.WriteLine("Created Database: {0}\n", this.database.Id);
            }
        }
    }
    ```

**F5** キーを押してアプリケーションを実行します。

お疲れさまでした。 これで、Azure Cosmos DB データベースが作成されました。  

## <a id="CreateColl"></a>手順 5: コンテナーを作成する
> [!WARNING]
> **CreateContainerIfNotExistsAsync** メソッドを呼び出すと、価格に影響する新しいコンテナーが作成されます。 詳細については、[価格のページ](https://azure.microsoft.com/pricing/details/cosmos-db/)を参照してください。
> 
> 

コンテナーは、**Containers** クラスの [**CreateContainerIfNotExistsAsync**](https://aka.ms/CosmosDotnetAPIDocs) または [**CreateContainerAsync**](https://aka.ms/CosmosDotnetAPIDocs) 関数のいずれかを使用して作成できます。 コンテナーは、項目 (SQL API の場合は JSON ドキュメント) および関連する JavaScript サーバー側アプリケーション ロジック (ストアド プロシージャ、ユーザー定義関数、トリガーなど) で構成されます。

1. **CreateContainer** メソッドをコピーして、**CreateDatabase** メソッドの下に貼り付けます。 **CreateContainer** によって、``containerId`` フィールドに指定された id で新しいコンテナー ``FamilyContainer`` が作成されます (このコンテナーがまだ存在していない場合)。 

    ```csharp
    /*
        Create the container if it does not exist. 
        Specifiy "/LastName" as the partition key since we're storing family information, to ensure good distribution of requests and storage.
    */
    private async Task CreateContainer()
    {
        // Create a new container
        this.container = await this.database.Containers.CreateContainerIfNotExistsAsync(containerId, "/LastName");
        Console.WriteLine("Created Container: {0}\n", this.container.Id);
    }
    ```

1. CosmosClient をインスタンス化したところに、次のコードをコピーして貼り付けて、追加した **CreateContainer** メソッドを呼び出します。

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 

        //ADD THIS PART TO YOUR CODE
        await this.CreateContainer();
    }

Select  **F5** to run your application.

Congratulations! You have successfully created an Azure Cosmos DB container.  

## <a id="CreateDoc"></a>Step 6: Add items to the container
An item can be created by using the [**CreateItemAsync**](https://aka.ms/CosmosDotnetAPIDocs) function of the **Items** class. When using the SQL API, items are projected as documents, which are user-defined (arbitrary) JSON content. You can now insert an item into your Azure Cosmos DB container.

First, we need to create a **Family** class that will represent objects stored within Azure Cosmos DB in this sample. We will also create **Parent**, **Child**, **Pet**, **Address** subclasses that are used within **Family**. Note that documents must have an **Id** property serialized as **id** in JSON. 
1. Select  **Ctrl+Shift+A** to open the **Add New Item** dialog. Add a new class **Family.cs** to your project. 

    ![Screen shot of adding a new Family.cs class into the project](./media/sql-api-get-started/dotnet-tutorial-visual-studio-add-family-class.png)

1. Copy and paste the **Family**, **Parent**, **Child**, **Pet**, and **Address** class into **Family.cs**. Note your namespace will differ based on the name of your project.
    ```csharp
    using Newtonsoft.Json;

    namespace CosmosGettingStartedDotnetCoreTutorial
    {
        public class Family
        {
            [JsonProperty(PropertyName = "id")]
            public string Id { get; set; }
            public string LastName { get; set; }
            public Parent[] Parents { get; set; }
            public Child[] Children { get; set; }
            public Address Address { get; set; }
            public bool IsRegistered { get; set; }
            public override string ToString()
            {
                return JsonConvert.SerializeObject(this);
            }
        }

        public class Parent
        {
            public string FamilyName { get; set; }
            public string FirstName { get; set; }
        }

        public class Child
        {
            public string FamilyName { get; set; }
            public string FirstName { get; set; }
            public string Gender { get; set; }
            public int Grade { get; set; }
            public Pet[] Pets { get; set; }
        }

        public class Pet
        {
            public string GivenName { get; set; }
        }

        public class Address
        {
            public string State { get; set; }
            public string County { get; set; }
            public string City { get; set; }
        }
    }
    ```
1. **Program.cs** に戻り、**CreateContainer**メソッドの下に **AddItemsToContainer** メソッドを追加します。 コードによって、同じ ID を持つ項目が存在していないことが項目の作成前に確認されます。 2 つの項目を挿入します。1 つは Andersen Family のドキュメント、もう 1 つは Wakefield Family のドキュメントです。

    ```csharp
    /*
        Add Family items to the container
    */
    private async Task AddItemsToContainer()
    {
        // Create a family object for the Andersen family
        Family andersenFamily = new Family
        {
            Id = "Andersen.1",
            LastName = "Andersen",
            Parents = new Parent[]
            {
                new Parent { FirstName = "Thomas" },
                new Parent { FirstName = "Mary Kay" }
            },
            Children = new Child[]
            {
                new Child
                {
                    FirstName = "Henriette Thaulow",
                    Gender = "female",
                    Grade = 5,
                    Pets = new Pet[]
                    {
                        new Pet { GivenName = "Fluffy" }
                    }
                }
            },
            Address = new Address { State = "WA", County = "King", City = "Seattle" },
            IsRegistered = true
        };

        // Read the item to see if it exists. Note ReadItemAsync will not throw an exception if an item does not exist. Instead, we check the StatusCode property off the response object. 
        CosmosItemResponse<Family> andersenFamilyResponse = await this.container.Items.ReadItemAsync<Family>(andersenFamily.LastName, andersenFamily.Id);

        if (andersenFamilyResponse.StatusCode == HttpStatusCode.NotFound)
        {
            // Create an item in the container representing the Andersen family. Note we provide the value of the partition key for this item, which is "Andersen"
            andersenFamilyResponse = await this.container.Items.CreateItemAsync<Family>(andersenFamily.LastName, andersenFamily);

            // Note that after creating the item, we can access the body of the item with the Resource property off the CosmosItemResponse. 
            //We can also access the RequestCharge property to see the amount of RUs consumed on this request.
            Console.WriteLine("Created item in database with id: {0} Operation consumed {1} RUs.\n", andersenFamilyResponse.Resource.Id, andersenFamilyResponse.RequestCharge);
        }
        else
        {
            Console.WriteLine("Item in database with id: {0} already exists\n", andersenFamilyResponse.Resource.Id);
        }

        // Create a family object for the Wakefield family
        Family wakefieldFamily = new Family
        {
            Id = "Wakefield.7",
            LastName = "Wakefield",
            Parents = new Parent[]
            {
                new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                new Parent { FamilyName = "Miller", FirstName = "Ben" }
            },
            Children = new Child[]
            {
                new Child
                {
                    FamilyName = "Merriam",
                    FirstName = "Jesse",
                    Gender = "female",
                    Grade = 8,
                    Pets = new Pet[]
                    {
                        new Pet { GivenName = "Goofy" },
                        new Pet { GivenName = "Shadow" }
                    }
                },
                new Child
                {
                    FamilyName = "Miller",
                    FirstName = "Lisa",
                    Gender = "female",
                    Grade = 1
                }
            },
            Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
            IsRegistered = false
        };

        // Read the item to see if it exists
        CosmosItemResponse<Family> wakefieldFamilyResponse = await this.container.Items.ReadItemAsync<Family>(wakefieldFamily.LastName, wakefieldFamily.Id);

        if (wakefieldFamilyResponse.StatusCode == HttpStatusCode.NotFound)
        {
            // Create an item in the container representing the Wakefield family. Note we provide the value of the partition key for this item, which is "Wakefield"
            wakefieldFamilyResponse = await this.container.Items.CreateItemAsync<Family>(wakefieldFamily.LastName, wakefieldFamily);

            // Note that after creating the item, we can access the body of the item with the Resource property off the CosmosItemResponse. 
            //We can also access the RequestCharge property to see the amount of RUs consumed on this request.
            Console.WriteLine("Created item in database with id: {0} Operation consumed {1} RUs.\n", wakefieldFamilyResponse.Resource.Id, wakefieldFamilyResponse.RequestCharge);
        }
        else
        {
            Console.WriteLine("Item in database with id: {0} already exists\n", wakefieldFamilyResponse.Resource.Id);
        }
    }
    ```
1. ``GetStartedDemoAsync`` メソッドに ``AddItemsToContainer`` への呼び出しを追加します。

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();

        //ADD THIS PART TO YOUR CODE
        await this.AddItemsToContainer();
    }
    ```

**F5** キーを押してアプリケーションを実行します。

お疲れさまでした。 これで、Azure Cosmos DB の項目が 2 つ作成されました。  

## <a id="Query"></a>手順 7: Azure Cosmos DB リソースにクエリを実行する
Azure Cosmos DB では、各コレクションに格納された JSON ドキュメントに対する豊富な[クエリ](sql-api-sql-query.md)がサポートされています。 次のサンプル コードを使用して、前の手順で挿入した項目に対してどのようにクエリを実行するかを説明します。

1. **RunQuery** メソッドをコピーして、**AddItemsToContainer** メソッドの下に貼り付けます。

    ```csharp
    /*
        Run a query (using Azure Cosmos DB SQL syntax) against the container
    */
    private async Task RunQuery()
    {
        var sqlQueryText = "SELECT * FROM c WHERE c.LastName = 'Andersen'";
        var partitionKeyValue = "Andersen";

        Console.WriteLine("Running query: {0}\n", sqlQueryText);

        CosmosSqlQueryDefinition queryDefinition = new CosmosSqlQueryDefinition(sqlQueryText);
        CosmosResultSetIterator<Family> queryResultSetIterator = this.container.Items.CreateItemQuery<Family>(queryDefinition, partitionKeyValue);

        List<Family> families = new List<Family>();

        while (queryResultSetIterator.HasMoreResults)
        {
            CosmosQueryResponse<Family> currentResultSet = await queryResultSetIterator.FetchNextSetAsync();
            foreach (Family family in currentResultSet)
            {
                families.Add(family);
                Console.WriteLine("\tRead {0}\n", family);
            }
        }
    }
    ```
1. ``GetStartedDemoAsync`` メソッドに ``RunQuery`` への呼び出しを追加します。

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();
        await this.AddItemsToContainer();

        //ADD THIS PART TO YOUR CODE
        await this.RunQuery();
    }
    ```

**F5** キーを押してアプリケーションを実行します。

お疲れさまでした。 これで、Azure Cosmos DB コンテナーに対するクエリが実行されました。

## <a id="ReplaceItem"></a>手順 8: JSON 項目を置換する
ここでは、Azure Cosmos DB 内の項目を更新します。

1. **ReplaceFamilyItem** メソッドをコピーして、**RunQuery** メソッドの下に貼り付けます。 家族の ``IsRegistered`` プロパティと子どもの 1 人の ``Grade`` を変更していることに注意してください。
    ```csharp
    /*
    Update an item in the container
    */
    private async Task ReplaceFamilyItem()
    {
        CosmosItemResponse<Family> wakefieldFamilyResponse = await this.container.Items.ReadItemAsync<Family>("Wakefield", "Wakefield.7");
        var itemBody = wakefieldFamilyResponse.Resource;
        
        // update registration status from false to true
        itemBody.IsRegistered = true;
        // update grade of child
        itemBody.Children[0].Grade = 6;

        // replace the item with the updated content
        wakefieldFamilyResponse = await this.container.Items.ReplaceItemAsync<Family>(itemBody.LastName, itemBody.Id, itemBody);
        Console.WriteLine("Updated Family [{0},{1}]\n. Body is now: {2}\n", itemBody.LastName, itemBody.Id, wakefieldFamilyResponse.Resource);
    }
    ```
1. ``GetStartedDemo`` メソッドに ``ReplaceFamilyItem`` への呼び出しを追加します。

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();
        await this.AddItemsToContainer();
        await this.RunQuery();

        //ADD THIS PART TO YOUR CODE
        await this.ReplaceFamilyItem();
    }

Select  **F5** to run your application.

Congratulations! You have successfully replaced an Azure Cosmos DB item.

## <a id="DeleteDocument"></a>Step 9: Delete item
Now, we will delete an item in Azure Cosmos DB.

1. Copy and paste the **DeleteFamilyItem** method below your **ReplaceFamilyItem** method.
    ```csharp
    /*
    Delete an item in the container
    */
    private async Task DeleteFamilyItem()
    {
        var partitionKeyValue = "Wakefield";
        var familyId = "Wakefield.7";

        // Delete an item. Note we must provide the partition key value and id of the item to delete
        CosmosItemResponse<Family> wakefieldFamilyResponse = await this.container.Items.DeleteItemAsync<Family>(partitionKeyValue, familyId);
        Console.WriteLine("Deleted Family [{0},{1}]\n", partitionKeyValue, familyId);
    }
    ```
1. ``GetStartedDemo`` メソッドに ``DeleteFamilyItem`` への呼び出しを追加します。

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();
        await this.AddItemsToContainer();
        await this.RunQuery();
        await this.ReplaceFamilyItem();

        //ADD THIS PART TO YOUR CODE
        await this.DeleteFamilyItem();
    }
    ```

**F5** キーを押してアプリケーションを実行します。

お疲れさまでした。 これで、Azure Cosmos DB 項目が削除されました。

## <a id="DeleteDatabase"></a>手順 10: データベースを削除する
ここでは、データベースを削除します。 作成したデータベースを削除すると、データベースとすべての子リソース (コンテナー、項目、ストアド プロシージャ、ユーザー定義関数、トリガーなど) が削除されます。 ここでは、**CosmosClient** インスタンスも破棄します。

1. **DeleteDatabaseAndCleanup** メソッドをコピーして、**DeleteFamilyItem** メソッドの下に貼り付けます。
    ```csharp
    /*
    Delete the database and dispose of the Cosmos Client instance
    */
    private async Task DeleteDatabaseAndCleanup()
    {
        CosmosDatabaseResponse databaseResourceResponse = await this.database.DeleteAsync();
        // Also valid: await this.cosmosClient.Databases["FamilyDatabase"].DeleteAsync();

        Console.WriteLine("Deleted Database: {0}\n", this.databaseId);

        //Dispose of CosmosClient
        this.cosmosClient.Dispose();
    }
    ```
1. ``GetStartedDemo`` メソッドに ``DeleteDatabaseAndCleanup`` への呼び出しを追加します。
    
    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();
        await this.AddItemsToContainer();
        await this.RunQuery();
        await this.ReplaceFamilyItem();
        await this.DeleteFamilyItem();

        //ADD THIS PART TO YOUR CODE        
        await this.DeleteDatabaseAndCleanup();
    }
    ```

**F5** キーを押してアプリケーションを実行します。

お疲れさまでした。 これで、Azure Cosmos DB データベースが削除されました。

## <a id="Run"></a>手順 11: C# コンソール アプリケーションの全体的な実行の流れ
Visual Studio で **F5** キーを押して、デバッグ モードでアプリケーションをビルドします。

開始したアプリ全体の出力がコンソール ウィンドウに表示されます。 出力では追加したクエリの結果が表示されます。次の例のようなものになるはずです。

```
Beginning operations...

Created Database: FamilyDatabase

Created Container: FamilyContainer

Created item in database with id: Andersen.1 Operation consumed 11.43 RUs.

Created item in database with id: Wakefield.7 Operation consumed 14.29 RUs.

Running query: SELECT * FROM c WHERE c.LastName = 'Andersen'

        Read {"id":"Andersen.1","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":false}

Updated Family [Wakefield,Wakefield.7].
        Body is now: {"id":"Wakefield.7","LastName":"Wakefield","Parents":[{"FamilyName":"Wakefield","FirstName":"Robin"},{"FamilyName":"Miller","FirstName":"Ben"}],"Children":[{"FamilyName":"Merriam","FirstName":"Jesse","Gender":"female","Grade":6,"Pets":[{"GivenName":"Goofy"},{"GivenName":"Shadow"}]},{"FamilyName":"Miller","FirstName":"Lisa","Gender":"female","Grade":1,"Pets":null}],"Address":{"State":"NY","County":"Manhattan","City":"NY"},"IsRegistered":true}

Deleted Family [Wakefield,Wakefield.7]

Deleted Database: FamilyDatabase

End of demo, press any key to exit.
```

お疲れさまでした。 チュートリアルが完了し、実際に動作する C# コンソール アプリケーションが完成しました。

## <a id="GetSolution"></a>チュートリアルのソリューションの完全版を入手する

この記事のすべてのサンプルを含む GetStarted ソリューションをビルドするには、以下が必要です。

* アクティブな Azure アカウントアカウントがない場合、Azure 試用版にサインアップして、最大 10 件の無料 Mobile Apps を入手できます。 お持ちでない場合は、 [無料アカウント](https://azure.microsoft.com/free/)にサインアップしてください。
* [Azure Cosmos DB アカウント][cosmos-db-create-account]。
* GitHub で入手可能な [GetStarted](https://github.com/Azure-Samples/cosmos-dotnet-core-getting-started) ソリューション。

Visual Studio で SQL API for Azure Cosmos DB .NET Core SDK への参照を復元するには、ソリューション エクスプローラーで **GetStarted** ソリューションを右クリックし、**[NuGet パッケージの復元]** を選択します。 次に、「[Azure Cosmos DB アカウントに接続する](#Connect)」の説明に従って、**Program.cs** ファイルの ``EndpointUri`` と ``PrimaryKey`` の値を更新します。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、.NET Core アプリを構築して Azure Cosmos DB SQL API データを管理する方法について説明しました。 これで、Cosmos DB アカウントに追加のデータをインポートできます。 

> [!div class="nextstepaction"]
> [Azure Cosmos DB へのデータのインポート](import-data.md)

[cosmos-db-create-account]: create-sql-api-dotnet-preview.md#create-account

