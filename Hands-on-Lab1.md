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
3. **\[既存の仮想ネットワーク追加]** を選択します。

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

1. Azure Storage Explorer を起動し、接続ダイアログで **Add an Azure Account** を選択します。

   <img src="/images/hands-on-lab1-AzureStorageExplorer-001.png" title="Add an Azure Account">
   
2. Azure にログインするための認証情報を入力し、サブスクリプションを選択します。

3. 左側のツリービューで、該当するストレージアカウントを展開します。

   <img src="/images/hands-on-lab1-AzureStorageExplorer-002.png" title="該当するストレージアカウントを展開します">

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

## 特定の Azure Storage のみに接続を許可　サービスエンドポイントポリシーの適用

　ここまでの構成で、Azure Storage は自分が設定した仮想ネットワーク(VNET)からのみ着信を許可するよう構成されていますが、仮想ネットワーク側は東南アジアリージョンの全ての Azure Storage へアクセスが可能な状況となっています。情報漏洩の観点から、自分が所有する特定の Azure Storage にのみアクセス制限をするために使用するのが [サービス エンドポイント ポリシー](https://docs.microsoft.com/ja-jp/azure/virtual-network/virtual-network-service-endpoint-policies-overview) となります。
 
   <img src="/images/vnet-service-endpoint-policies-overview.png" title="サービスエンドポイントポリシー">

 1. Azure Portal の左上隅にある [+ リソースの作成] を選択します。

 2. 検索ウィンドウに「Service endpoint policy」と入力し、 \[Service endpoint policy] 、 \[作成] の順に選択します。

   <img src="/images/hands-on-lab1-Service-endpoint-policy-001.png" title="Service endpoint policy">
   
 3. **\[基本]** に次の情報を入力するか選択します。 **\[ 次: ポリシー定義 > ]** へ進みます。
 
    <img src="/images/hands-on-lab1-Service-endpoint-policy-002.png" title="基本">

 4. **\[リソース]** の下にある **\[+ 追加]** を選択し **\[リソースの追加]** ウィンドウで次の情報を入力または選択し、　**\[追加]** を押します。

    <img src="/images/hands-on-lab1-Service-endpoint-policy-003.png" title="ポリシー定義">
 
 5. **\[ 確認および作成 > ]** を押して、ポリシーを作成します。 
