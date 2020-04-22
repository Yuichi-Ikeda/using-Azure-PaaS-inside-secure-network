# 演習１ - Data Factory セルフホステッド統合ランタイムとサービスエンドポイント

# 仮想マシンのデプロイ
1. [Azure portal](https://portal.azure.com)  にサインインします。
2. Azure portal の左上メニューまたはホームページから **\[リソースの作成]** を選択します。

   <img src="/images/hands-on-lab1-ADF-001.png" title="リソースの作成">
3. 検索ボックスに「Windows Server」と入力し、表示された一覧の中から同じタイトルのリンクをクリックします。  

   <img src="/images/hands-on-lab1-VM-001.png" title="検索ボックスに「Windows Server」と入力">
4. VM を作成する Windows Server バージョンは複数から選択できます。 \[Windows Server] イメージ概要パネルで \[ソフトウェア プランの選択] ドロップダウン リストをクリックして、\[Windows Server 2019 Datacenter] を探します。  

   <img src="/images/hands-on-lab1-VM-002.png" title="Windows Server 2019 Datacenter">
5. \[作成] ボタンをクリックして、VM の構成を始めます。

## VM 設定を構成する
 
　ポータルでの VM 作成エクスペリエンスは "ウィザード" の形式で提示され、VM のすべての構成領域が順番に示されます。 [次へ] ボタンをクリックすると、次の構成可能なセクションに移動します。 上部に並んでいるタブで各セクションが示されており、自由にセクション間を移動できます。

1. リソースグループで **DataFactory-Lab01-rg** を選択します。  

   <img src="/images/hands-on-lab1-VM-003.png" title="リソースグループを作成">
2. 必須項目 **\*** の各種パラメータを以下のように設定します。VM サイズは [Data Factory 前提要件](https://docs.microsoft.com/ja-jp/azure/data-factory/create-self-hosted-integration-runtime#prerequisites)の最小構成をクリアするように **D4s v3** に変更します。  
   <img src="/images/hands-on-lab1-VM-004.png" title="各種パラメータを設定">
3. その他のパラメーターは既定値のまま **\[ 次: ディスク > ]** へ行きます。 
4. ディスク項目も既定値のまま **\[ 次: ネットワーク > ]** へ行き、仮想ネットワークの **\[ 新規作成 ]** をクリックし、以下のアドレス空間とサブネットに構成します。

   <img src="/images/hands-on-lab1-VM-005.png" title="ネットワーク設定">
   
5. **\[ 次: 管理 > ]** へ行きます。 **\[ ブート診断 ]** をオフに設定し、これ以外は既定値とします。
 
6. **\[ 確認および作成 ]** をクリックし、検証に成功したら、そのまま **\[ 作成 ]** をします。

   デプロイ作業が開始され、数分後に仮想マシンのデプロイが完了します。

# Azure Storage のデプロイ
1. Azure portal の左上メニューまたはホームページから **\[リソースの作成]** を選択します。

   <img src="/images/hands-on-lab1-ADF-001.png" title="リソースの作成">
2. **\[ストレージ]** を選択してから、 **\[ストレージアカウント]** を選択します。

   <img src="/images/hands-on-lab1-Storage-001.png" title="ストレージアカウントの作成">
3. **\[サブスクリプション]** で、該当する Azure サブスクリプションを選択します。

4. **\[リソース グループ]** で、**\[新規作成]** を選択し、リソース グループの名前に「**Storage-rg**」と入力します。

5. **\[ストレージ アカウント名]** に **\[一意の名前]** を入力します。

6. **\[場所]** で、データ ファクトリの場所 **(Asia Pacific) 東南アジア** を選択します。

7. **\[レプリケーション]** で、**ローカル冗長ストレージ (LRS)** を選択します。

   <img src="/images/hands-on-lab1-Storage-002.png" title="ストレージアカウント 基本設定">
   
8. その他は既定値のまま **\[ 次: ネットワーク > ]** へ行きます。

9. **\[接続方法]** で、 **\[パブリック エンドポイント（選択されたネットワーク）]** を選択します。

10. **\[仮想ネットワーク]** で、**DataFactoryVNet** を選択します。

11. **\[サブネット]** で、**ADFSubnet** を選択します。

   <img src="/images/hands-on-lab1-Storage-003.png" title="ストレージアカウント 基本設定">

　※ これによりストレージアカウントは**選択した仮想ネットワークのサブネットからのみ通信を受け入れる**ように Firewall が構成されます。
 
12. **\[ 確認および作成 ]** をクリックし、検証に成功したら、そのまま **\[ 作成 ]** をします。

# Azure SQL Database のデプロイ
1. Azure portal の左上メニューまたはホームページから **\[リソースの作成]** を選択します。

   <img src="/images/hands-on-lab1-ADF-001.png" title="リソースの作成">
2. **\[データーベース]** を選択してから、 **\[SQL Database]** を選択します。

   <img src="/images/hands-on-lab1-SQLDatabase-001.png" title="SQL Database の作成">
3. **\[サブスクリプション]** で、該当する Azure サブスクリプションを選択します。

4. **\[リソース グループ]** で、**\[新規作成]** を選択し、リソース グループの名前に「**SQLDatabase-rg**」と入力します。

5. **\[データベース名]** に **\[一意の名前]** を入力します。

6. **\[サーバー]** で、 **新規作成** を選択します。

7. ポップアップウィンドウの **\[サーバー名]** に **\[一意の名前]** を入力します。

8. **\[サーバー管理者ログイン]** と **\[パスワード]** を入力します。

9. **\[場所]** で、データ ファクトリの場所 **(Asia Pacific) 東南アジア** を選択します。

   <img src="/images/hands-on-lab1-SQLDatabase-002.png" title="SQLDatabase 基本設定"> 
10. その他は既定値のまま **\[ 次: ネットワーク > ]** へ行きます。

11. **\[接続方法]** で、 **\[アクセスなし]** を選択します。サービスエンドポイントの構成は後で実施します。

   <img src="/images/hands-on-lab1-SQLDatabase-003.png" title="SQLDatabase ネットワーク設定">

12. その他は既定値のまま **\[ 次: 追加設定 > ]** へ行きます。

13. **\[既存のデータを使用します]** で、 **\[サンプル]** を選択します。

   <img src="/images/hands-on-lab1-SQLDatabase-004.png" title="SQLDatabase 追加設定">

14. **\[ 確認および作成 ]** をクリックし、検証に成功したら、そのまま **\[ 作成 ]** をします。

## SQL Database サービスエンドポイント設定を構成する
1. デプロイ完了後の通知メニューより **\[リソースに移動]** を選択します。

   <img src="/images/hands-on-lab1-SQLConfig-001.png" title="リソースに移動">
2. **\[概要]** を選択して、 **\[サーバー ファイアウォールの設定]** を選択します。

   <img src="/images/hands-on-lab1-SQLConfig-002.png" title="サーバー ファイアウォールの設定">
3. [Connection Policy](https://docs.microsoft.com/ja-jp/azure/sql-database/sql-database-connectivity-architecture#connection-policy) を  **Proxy** に変更します。これでクライアントからの着信を常に 1433 ポートで受けるようになります。そして下側にある **\[既存の仮想ネットワーク追加]** を選択します。

   <img src="/images/hands-on-lab1-SQLConfig-003.png" title="既存の仮想ネットワーク追加">
   
4. 仮想マシンのデプロイ時に作成した **\[既存の仮想ネットワーク]** と **\[サブネット名]** を選択します。

   <img src="/images/hands-on-lab1-SQLConfig-004.png" title="既存の仮想ネットワーク追加">

5. **\[有効]** を押して、続けて **\[OK]** を押します。サーバーの VNET ルールが更新されます。

6. 成功すると次のようにサービスエンドポイントが構成されます。

   <img src="/images/hands-on-lab1-SQLConfig-005.png" title="サービスエンドポイント確認">

# 仮想マシンへのクライアントツールのインストール

　ここで仮想マシンからサービスエンドポイント経由で Azure Storage と Azure SQL Database へのアクセス確認をする為に、クライアントとツールをインストールします。

1. リモートデスクトップで仮想マシンにログインします。
   <img src="/images/hands-on-lab1-VMConfig-001.png" title="リモートデスクトップ">
   
2. IE Enhanced Security Configuration を Off にします。この後の作業に際し **Edge や Chrome をダウンロード**しておく事を推奨します。
   <img src="/images/hands-on-lab1-VMConfig-002.png" title="IE Enhanced Security Configuration を OFF">
   
3. Azure Storage Explorer のインストール

   サービスエンドポイント経由で Azure Storage への接続確認のために、Azure Storage Explorer をインストールします。

   **Azure Storage Explorer のダウンロード**  
   https://azure.microsoft.com/ja-jp/features/storage-explorer/

4. SQL Server Management Studio (SSMS) のインストール

   サービスエンドポイント経由で Azure SQL Database への接続確認のために、SQL Server Management Studio (SSMS) をインストールします。
 
   **SQL Server Management Studio (SSMS) のダウンロード**  
   https://docs.microsoft.com/ja-jp/sql/ssms/download-sql-server-management-studio-ssms

# Azure Storage への接続確認

1. Azure Storage Explorer を起動し、接続ダイアログで **Use a storage account name and key** を選択します。

   <img src="/images/hands-on-lab1-AzureStorageExplorer-001.png" title="Azure Storage への接続確認">
   
2. Azure ポータルで該当するストレージの **\[アクセス キー]** を選択し、アカウント名とキーをコピーします。

   <img src="/images/hands-on-lab1-AzureStorageExplorer-002.png" title="Use a storage account name and key">
   
   <img src="/images/hands-on-lab1-AzureStorageExplorer-003.png" title="Use a storage account name and key">

3. 左側のツリービューで、該当するストレージアカウントを展開します。

   <img src="/images/hands-on-lab1-AzureStorageExplorer-004.png" title="該当するストレージアカウントを展開します">

4. 右クリックメニューの **Create Blob Container** で Blob への書き込みが可能な事を確認します。

# Azure SQL Database への接続確認

1. SQL Server Management Studio (SSMS) を起動し、接続情報に **Azure SQL Database のデプロイ - 手順 9** で入力したサーバー名、ユーザー名、パスワードを使用して接続します。この時、SQL Server Authentication を選択します。

   <img src="/images/hands-on-lab1-SSMS-001.png" title="SQL Database への接続">
   
2. 左側のツリービューで、データベースとテーブルを展開し、テーブルの内容が参照(Select) 出来ることを確認します。

   <img src="/images/hands-on-lab1-SSMS-002.png" title="テーブルの内容を参照">

# ファイアウォール (NSG) にてインターネットへの接続を拒否

　本来ファイアウォールの構成は、最初にしておく方がセキュリティの観点から望ましいですが、今回の演習では全体の動作を確認しながら進めて行く為にあえて、このタイミングで構成を実施します。
 
## インターネットへの接続(Outgoing)を拒否

1. ADFRuntime-nsg の **\[概要]** で、NSG の構成が以下のように確認できます。受信セキュリティでは仮想マシンの RDP 接続以外はインターネットからの着信は許可されていませんが、送信セキュリティではインターネットへの送信が許可されています。

   <img src="/images/hands-on-lab1-NSG-001.png" title="NSG の既定値">
   
2. 送信セキュリティに対して、以下のように、より高い優先度（少ない番号）でインターネットへの送信を拒否します。

   <img src="/images/hands-on-lab1-NSG-002.png" title="インターネットへの送信を拒否">
   
3. 適用が完了すると、次のような構成状態となります。

   <img src="/images/hands-on-lab1-NSG-003.png" title="インターネットへの送信を拒否">

   ※ この状態では、仮想マシンから一切のインターネットアクセスが拒否されるためサービスエンドポイントの先にある Azure Storage や Azure SQL Database にもアクセスが出来ない状況となっています。
   
## 東南アジアリージョンの Azure Storage への接続を許可 

1. NSG サービスタグを利用して、東南アジアリージョンの Azure Storage への接続を許可します。

   <img src="/images/hands-on-lab1-NSG-004.png" title="東南アジアリージョンの Azure Storage への接続を許可">
   
2. この状態で仮想ネットワークからは、東南アジアリージョンにデプロイされている全ての Azure Storage へアクセスが可能です。他方サービスエンドポイントで接続されている Azure Storage 側は、この仮想ネットワークのサブネットからのみ着信を許可するよう F/W が構成されています。

   <img src="/images/hands-on-lab1-NSG-005.png" title="東南アジアリージョンの Azure Storage への接続を許可">

## 仮想ネットワーク側から特定の Azure Storage のみに接続を許可させる

　ここまでの構成で、Azure Storage は自分が設定した仮想ネットワーク(VNET)からのみ着信を許可するよう構成されていますが、仮想ネットワーク側は東南アジアリージョンの全ての Azure Storage へアクセスが可能な状況となっています。情報漏洩の観点から、自分が所有する特定の Azure Storage にのみアクセス制限をするために使用するのが [サービス エンドポイント ポリシー](https://docs.microsoft.com/ja-jp/azure/virtual-network/virtual-network-service-endpoint-policies-overview) となります。 
    
   <img src="/images/vnet-service-endpoint-policies-overview.png" title="サービスエンドポイントポリシー">
    
## サービスエンドポイントポリシーの適用

 1. Azure Portal の左上隅にある [+ リソースの作成] を選択します。

 2. 検索ウィンドウに「Service endpoint policy」と入力し、 \[Service endpoint policy] 、 \[作成] の順に選択します。

    <img src="/images/hands-on-lab1-Service-endpoint-policy-001.png" title="Service endpoint policy">
   
 3. **\[基本]** に次の情報を入力するか選択します。 **\[ 次: ポリシー定義 > ]** へ進みます。
 
    <img src="/images/hands-on-lab1-Service-endpoint-policy-002.png" title="基本">

 4. **\[リソース]** の下にある **\[+ リソースを追加する]** を選択し、次の情報を入力または選択し **\[追加]** を押します。

    <img src="/images/hands-on-lab1-Service-endpoint-policy-003.png" title="ポリシー定義">
 
 5. **\[ 確認および作成 > ]** を押して、検証に成功したら **\[作成]** を押してポリシーを作成します。 

    <img src="/images/hands-on-lab1-Service-endpoint-policy-004.png" title="確認および作成">
    
 6. 作成されたリソースに移動し、ポリシーに仮想ネットワークのサブネットを関連付けます。
     
     <img src="/images/hands-on-lab1-Service-endpoint-policy-005.png" title="サブネットを関連付け">

　以上で、サービスエンドポイントが接続されたサブネットで、該当する Azure Storage にのみアクセスが制限されるようポリシーが適用されました。東南アジアリージョンの他のストレージにアクセスしようとしても以下のようにアクセスが拒否されます。
    
 <img src="/images/hands-on-lab1-Service-endpoint-policy-006.png" title="エラー確認">
   
## 東南アジアリージョンの Azure SQL Database への接続を許可 

　続けて SQL Database に対しても NSG の構成をしていきます。
 
1. ADFRuntime-nsg の送信セキュリティに対して、NSG サービスタグを利用して、東南アジアリージョンの SQL Database への接続を許可します。

    <img src="/images/hands-on-lab1-SQL-NSG-001.png" title="東南アジアリージョンの SQL Database への接続を許可">
    
2. この状態で仮想ネットワークからは、東南アジアリージョンにデプロイされている全ての SQL Database へアクセスが可能です。他方サービスエンドポイントで接続されている SQL Database 側は、この仮想ネットワークのサブネットからのみ着信を許可するよう F/W が構成されています。

    <img src="/images/hands-on-lab1-SQL-NSG-002.png" title="東南アジアリージョンの SQL Database への接続を許可">

## 仮想ネットワーク側から特定の Azure Storage のみに接続を許可させる

　情報漏洩の観点から Azure Storage 同様に、仮想ネットワークからのアクセスを特定の SQL Database のみに制限したいところですが、サービスエンドポイントポリシーは現在 Azure Storage だけが対応している状況です。これを実現するには、演習 2 で出てくる [Azure Firewall](https://docs.microsoft.com/ja-jp/azure/firewall/overview) を利用する必要があります。
 
# Azure Data Factory のデプロイ

　基本的なリソースのデプロイとサービスエンドポイントの構成が完了したので、ここから ETL サービスである Azure Data Factory をデプロイしていきます。

1. Azure portal の左上メニューまたはホームページから **\[リソースの作成]** を選択します。

   <img src="/images/hands-on-lab1-ADF-001.png" title="リソースの作成">
2. **\[分析]** を選択してから、 **\[Data Factory]** を選択します。

   <img src="/images/hands-on-lab1-ADF-002.png" title="Data Factoryの作成">
3. **\[新しいデータ ファクトリ]** ページで、 **\[名前]** に「**\<yourname>-WorkshopDataFactory**」と入力します。

4. **\[サブスクリプション]** で、データ ファクトリを作成する Azure サブスクリプションを選択します。

5. **\[リソース グループ]** で、**\[新規作成]** を選択し、リソース グループの名前に「**DataFactory-Lab01-rg**」と入力します。

6. **\[場所]** で、データ ファクトリの場所 **(Asia Pacific) 東南アジア** を選択します。

7. **\[Git を有効にする]** のチェックを外します。

8. その他は既定値のまま **作成** を選択します。

 ## Azure Data Factory を構成する
1. **\[概要]** を選択して、 **\[作成と監視]** を選択します。

   <img src="/images/hands-on-lab1-ADFConfig-001.png" title="作成と監視">
2. Data Factory のポータルサイトで、左端のペインの **\[鉛筆アイコン]** を選択し、下部にある **\[Connections]** を選択します。右ウィンドウで **\[Integration Runtimes]** を選択し、 **\[+ New]** を選択します。

   <img src="/images/hands-on-lab1-ADFConfig-002.png" title="Integration Runtimes の作成">
3. 表示されたポップアップウィンドウで **\[Azure, Self-Hosted]** を選択し、 **\[Continue]** を選択します。

4. 続けて **\[Self-Hosted]** を選択し、 **\[Continue]** を選択します。

5. 名前を既定値の **integrationRuntime1** のまま **\[Create]** を選択します。

6. 表示された **Key1** をコピーしておきます。後ほどセルフホステッド統合ランタイムをインストールする際に必要となります。

   
## 東南アジアリージョンの Azure Data Factory への接続を許可 

　[NSG のサービスタグ](https://docs.microsoft.com/ja-jp/azure/virtual-network/service-tags-overview#available-service-tags)には、DataFactory, DataFactoryManagement といった Data Factory 用のサービスタグが存在しますが、仮想マシンで利用するセルフホステッド統合ランタイムからの通信に際しては Azure Data Factory をデプロイした Azure リージョンへの通信を許可する必要があります。本シナリオでは AzureCloud.SoutheastAsia のサービスタグを利用する事になります。

1. NSG サービスタグを利用して、東南アジアリージョンの Azure Data Factory への接続を許可します。

   <img src="/images/hands-on-lab1-ADF-NSG-001.png" title="東南アジアリージョンの Azure Data Factory への接続を許可">
   
2. この状態で仮想ネットワークからは、東南アジアリージョンの Azure データセンター全ての Public IP レンジ 443 ポートへアクセスが可能です。また優先度 3990 で定義した AllowStorageSoutheastAsisa の範囲も包含しているため、この状態では 3990 定義は不要とも言えます。

   <img src="/images/hands-on-lab1-ADF-NSG-002.png" title="東南アジアリージョンの Azure Data Factory への接続を許可">

　より厳密に送信ポートを絞るには、[セルフホステッド統合ランタイムが必要とするポートとファイアウォール](https://docs.microsoft.com/ja-jp/azure/data-factory/create-self-hosted-integration-runtime#ports-and-firewalls)が公開されています。こちらは Azure Firewall を利用する必要があり、後半の演習 2 で組み込んでいきます。

 ## Data Factory セルフホステッド統合ランタイムのインストール
 
1. リモートデスクトップで仮想マシンにログインします。
   <img src="/images/hands-on-lab1-VMConfig-001.png" title="リモートデスクトップ">
   
2. 次のドキュメントの手順に従い、Microsoft ダウンロード センターからセルフホステッド IR をインストールして登録します。**言語は全て English (United States)** で実行します。
   
   https://docs.microsoft.com/ja-jp/azure/data-factory/create-self-hosted-integration-runtime#install-and-register-a-self-hosted-ir-from-microsoft-download-center
   
3. **Azure Data Factory を構成**した際に控えておいた Key1 を入力して Self-Hosted Runtime の**登録**を完了します。

   <img src="/images/hands-on-lab1-VMConfig-003.png" title="Self-Hosted Runtime の登録">

4. あとは既定値のまま進み **\[Launch Configuration Manager]** で正常に Self-hosted node が cloud serrvice に接続されている事を確認します。

   <img src="/images/hands-on-lab1-VMConfig-004.png" title="Self-Hosted Runtime の稼働確認">

5. Data Factory のポータルサイトでも Integration Rumtime が Running となっている事を確認します。

   <img src="/images/ADF-IntegrationRuntimes.png" title="Self-Hosted Runtime の稼働確認">
   
# Data Factory によるデータのコピー

　ここまでのステップで演習 1 のインフラ環境が整いました。最後に Data Factory のパイプラインを作成して Azure Blob Storage から SQL データベースにデータをコピーします。

#### ソース BLOB を作成する

1. 仮想マシンでメモ帳を起動します。 次のテキストをコピーし、**emp.txt** ファイルとしてディスクに保存します。

    ```
    Tarou,Yamada
    Hanako,Tanaka
    ```

1. [Azure Storage Explorer](https://storageexplorer.com/) を使用して、Blob Storage に **adftutorial** という名前のコンテナーを作成します。 このコンテナーに **input** という名前のフォルダーを作成します。 

    ![フォルダ作成](/images/azure-storage-create-new-folder.png)

1. **input** フォルダーに **emp.txt** ファイルをアップロードします。 

#### シンク SQL テーブルを作成する

1. 仮想マシンで SQL Server Management Studio を起動し、Azure SQL Database へ接続します。次の SQL スクリプトを使用して **dbo.emp** テーブルを SQL データベースに作成します。

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

## パイプラインを作成する

　Data Factory ポータルでパイプラインを作成し、その後パイプラインの構成に必要なリンクされたサービスとデータセットを作成していきます。

1. Data Factory ポータルの **[Let's get started]\(始めましょう\)** ページで **[Create pipeline]\(パイプラインの作成\)** を選択します。

   ![パイプラインの作成](/images/get-started-page.png)
1. パイプラインの **[全般]** タブで、パイプラインの**名前**として「**CopyPipeline**」と入力します。

1. **[アクティビティ]** ツール ボックスで **[Move and Transform]\(移動と変換\)** カテゴリを展開し、ツール ボックスからパイプライン デザイナー画面に **[データのコピー]** アクティビティをドラッグ アンド ドロップします。 **[名前]** に「**CopyFromBlobToSql**」と指定します。

    ![コピー アクティビティ](/images/drag-drop-copy-activity.png)

### ソースの構成

1. **[ソース]** タブに移動します。 **[+ 新規]** を選択して、ソース データセットを作成します。

1. **[新しいデータセット]** ダイアログ ボックスで **[Azure Blob Storage]** を選択し、 **[続行]** をクリックします。 ソース データは Blob Storage にあるので、ソース データセットには **Azure Blob Storage** を選択します。

1. **[形式の選択]** ダイアログ ボックスで、データの形式の種類を選択して、 **[続行]** を選択します。

    ![データの形式の種類](/images/select-data-format.png)

1. **[プロパティの設定]** ダイアログ ボックスで、[名前] に「**SourceBlobDataset**」を入力します。 **[リンクされたサービス]** ボックスの横にある **[+ 新規]** をクリックします。

1. **[New Linked Service (Azure Blob Storage)]\(新しいリンクされたサービス (Azure Blob Storage)\)** ダイアログ ボックスで、名前として「**AzureStorageLinkedService**」と入力し、 **[ストレージ アカウント名]** の一覧から該当のストレージ アカウントを選択します。 接続をテストし、 **[完了]** を選択して、リンクされたサービスをデプロイします。

1. リンクされたサービスが作成されると、 **[プロパティの設定]** ページに戻ります。 **[ファイル パス]** の横にある **[参照]** を選択します。

1. **adftutorial/input** フォルダーに移動し、**emp.txt** ファイルを選択して、 **[完了]** を選択します。

1. 自動的にパイプライン ページに移動します。 **[ソース]** タブで、 **[SourceBlobDataset]** が選択されていることを確認します。 このページのデータをプレビューするには、 **[データのプレビュー]** を選択します。

    ![ソース データセット](/images/source-dataset-selected.png)

### シンクの構成

1. **[シンク]** タブに移動し、 **[+ 新規]** を選択してシンク データセットを作成します。

1. **[新しいデータセット]** ダイアログ ボックスで、検索ボックスに「SQL」と入力してコネクタをフィルター処理し、 **[Azure SQL Database]** を選択して、 **[続行]** を選択します。 

1. **[プロパティの設定]** ダイアログ ボックスで、[名前] に「**OutputSqlDataset**」を入力します。 **[リンクされたサービス]** ボックスの横にある **[+ 新規]** をクリックします。 データセットをリンクされたサービスに関連付ける必要があります。 リンクされたサービスには、Data Factory が実行時に SQL データベースに接続するために使用する接続文字列が含まれています。 データセットは、コンテナー、フォルダー、データのコピー先のファイル (オプション) を指定します。

1. **[New Linked Service (Azure SQL Database)]\(新しいリンクされたサービス (Azure SQL Database)\)** ダイアログ ボックスで、次の手順を実行します。

    a. **[名前]** に「**AzureSqlDatabaseLinkedService**」と入力します。

    b. **[サーバー名]** で、使用する SQL Server インスタンスを選択します。

    c. **[データベース名]** で、使用する SQL データベースを選択します。

    d. **[ユーザー名]** に、ユーザー名を入力します。

    e. **[パスワード]** に、ユーザーのパスワードを入力します。

    f. **[テスト接続]** を選択して接続をテストします。

    g. **[完了]** を選択して、リンクされたサービスをデプロイします。

    ![新しいリンクされたサービスの保存](/images/new-azure-sql-linked-service-window.png)

1. **[プロパティの設定]** ダイアログ ボックスに自動的に移動します。 **[テーブル]** で **[dbo].[emp]** を選択します。 **[完了]** を選択します。

1. パイプラインがあるタブに移動し、 **[Sink Dataset]\(シンク データセット\)** で **OutputSqlDataset** が選択されていることを確認します。

    ![パイプラインのタブ](/images/pipeline-tab-2.png)       

必要に応じて「[コピー アクティビティでのスキーマ マッピング](https://docs.microsoft.com/ja-jp/azure/data-factory/copy-activity-schema-and-type-mapping)」に従い、コピー元のスキーマをコピー先の対応するスキーマにマッピングすることができます。

## パイプラインを検証する
パイプラインを検証するには、ツール バーから **[検証]** を選択します。

パイプラインに関連付けられている JSON コードを確認するには、右上にある **[コード]** をクリックします。

## パイプラインをデバッグして発行する
Data Factory または独自の Azure Repos Git リポジトリにアーティファクト (リンクされたサービス、データセット、パイプライン) を発行する前に、パイプラインをデバッグできます。

1. パイプラインをデバッグするには、ツール バーで **[デバッグ]** を選択します。 ウィンドウ下部の **[出力]** タブにパイプラインの実行の状態が表示されます。

1. パイプラインを適切に実行できたら、上部のツール バーで **[すべて発行]** を選択します。 これにより、作成したエンティティ (データセットとパイプライン) が Data Factory に発行されます。

1. **[正常に発行されました]** というメッセージが表示されるまで待機します。 通知メッセージを表示するには、右上にある **[通知の表示]** (ベル ボタン) をクリックします。

## パイプラインを手動でトリガーする
この手順では、前の手順で発行したパイプラインを手動でトリガーします。

1. ツール バーの **[トリガーの追加]** を選択し、 **[Trigger Now]\(今すぐトリガー\)** を選択します。 **[Pipeline Run]\(パイプラインの実行\)** ページで **[完了]** を選択します。  

1. 左側の **[監視]** タブに移動します。 手動トリガーによってトリガーされたパイプラインの実行が表示されます。 **[アクション]** 列のリンクを使用して、アクティビティの詳細を表示したりパイプラインを再実行したりできます。

    ![パイプラインの実行を監視する](/images/monitor-pipeline.png)

1. パイプラインの実行に関連付けられているアクティビティの実行を表示するには、 **[アクション]** 列の **[View Activity Runs]\(アクティビティの実行の表示\)** リンクを選択します。 この例では、アクティビティが 1 つだけなので、一覧に表示されるエントリは 1 つのみです。 コピー操作の詳細を確認するために、 **[アクション]** 列にある **[詳細]** リンク (眼鏡アイコン) を選択します。 再度パイプラインの実行ビューに移動するには、一番上にある **[Pipeline Runs]\(パイプラインの実行\)** を選択します。 表示を更新するには、 **[最新の情報に更新]** を選択します。

    ![アクティビティの実行を監視する](/images/view-activity-runs.png)

1. SQL データベースの **emp** テーブルに 2 つの行が追加されていることを確認します。

## スケジュールに基づいてパイプラインをトリガーする
このスケジュールでは、パイプラインのスケジュール トリガーを作成します。 このトリガーは、指定されたスケジュール (1 時間に 1 回、毎日など) に基づいてパイプラインを実行します。 ここでは、指定された終了日時まで毎分実行されるようトリガーを設定します。

1. [監視] タブの上にある左側の **[作成者]** タブに移動します。

1. パイプラインに移動し、ツール バーの **[トリガーの追加]** をクリックして、 **[新規/編集]** を選択します。

1. **[トリガーの追加]** ダイアログ ボックスで、 **[Choose trigger]\(トリガーの選択\)** 領域の **[+ 新規]** を選択します。

1. **[新しいトリガー]** ウィンドウで、次の手順を実行します。

    a. **[名前]** に「**RunEveryMinute**」と入力します。

    b. **[終了]** で **[指定日]** を選択します。

    c. **[End On]\(終了日\)** のドロップダウン リストを選択します。

    d. **現在の日付**のオプションを選択します。 既定では、終了日は翌日に設定されています。

    e. **[終了時刻]** の部分を現在の日時の数分後に変更します。 トリガーは、変更を発行した後にのみアクティブ化されます。 これをわずか数分後に設定し、それまでに発行しなかった場合、トリガー実行は表示されません。

    f. **[適用]** を選択します。

    g. **[アクティブ化]** オプションで **[はい]** を選択します。

    h. **[次へ]** を選択します。

    ![[アクティブ化] ボタン](/images/trigger-activiated-next.png)

    > [!IMPORTANT]
    > パイプラインの実行ごとにコストが関連付けられるため、終了日は適切に設定してください。
1. **[Trigger Run Parameters]\(トリガー実行のパラメーター\)** ページで、警告を確認し、 **[完了]** を選択します。 この例のパイプラインにはパラメーターはありません。

1. **[すべて発行]** をクリックして、変更を発行します。

1. 左側の **[モニター]** タブに移動して、トリガーされたパイプラインの実行を確認します。

    ![トリガーされたパイプラインの実行](/images/triggered-pipeline-runs.png)   

1. **パイプラインの実行**ビューから**トリガーの実行**ビューに切り替えるには、ウィンドウ上部の **[Trigger Runs]\(トリガーの実行\)** を選択します。

1. トリガーの実行が一覧で表示されます。

1. 指定された終了日時まで、(各パイプライン実行について) 1 分ごとに 2 つの行が **emp** テーブルに挿入されていることを確認します。
