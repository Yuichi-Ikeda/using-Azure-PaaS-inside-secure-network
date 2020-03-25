 # 演習１ - Data Factory セルフホステッド統合ランタイムとサービスエンドポイント

 # Azure Data Factory のデプロイ
1. [Azure portal](https://portal.azure.com)  にサインインします。
2. Azure portal の左上メニューまたはホームページから **\[リソースの作成]** を選択します。

   <img src="/images/hands-on-lab1-ADF-001.png" title="リソースの作成">
3. **\[分析]** を選択してから、 **\[Data Factory]** を選択します。

   <img src="/images/hands-on-lab1-ADF-002.png" title="Data Factoryの作成">
4. **\[新しいデータ ファクトリ]** ページで、 **\[名前]** に「**\<yourname>-WorkshopDataFactory**」と入力します。

5. **\[サブスクリプション]** で、データ ファクトリを作成する Azure サブスクリプションを選択します。

6. **\[リソース グループ]** で、**\[新規作成]** を選択し、リソース グループの名前に「**DataFactory-Lab01-rg**」と入力します。

7. **\[場所]** で、データ ファクトリの場所 **(Asia Pacific) 東南アジア** を選択します。

8. **\[Git を有効にする]** のチェックを外します。

9. その他は既定値のまま **作成** を選択します。

 ## Azure Data Factory を構成する
1. **\[概要]** を選択して、 **\[作成と監視]** を選択します。

   <img src="/images/hands-on-lab1-ADFConfig-001.png" title="作成と監視">
2. Data Factory のポータルサイトで、左端のペインの **\[鉛筆アイコン]** を選択し、下部にある **\[Connections]** を選択します。右ウィンドウで **\[Integration Runtimes]** を選択し、 **\[+ New]** を選択します。

   <img src="/images/hands-on-lab1-ADFConfig-002.png" title="Integration Runtimes の作成">
3. 表示されたポップアップウィンドウで **\[Azure, Self-Hosted]** を選択し、 **\[Continue]** を選択します。

4. 続けて **\[Self-Hosted]** を選択し、 **\[Continue]** を選択します。

5. 名前を既定値の **integrationRuntime1** のまま **\[Create]** を選択します。

6. 表示された **Key1** をコピーしておきます。後ほどセルフホステッド統合ランタイムをインストールする際に必要となります。

 # 仮想マシンのデプロイ
1. Azure portal の左上メニューまたはホームページから **\[リソースの作成]** を選択します。
2. 検索ボックスに「Windows Server」と入力し、表示された一覧の中から同じタイトルのリンクをクリックします。  

   <img src="/images/hands-on-lab1-VM-001.png" title="検索ボックスに「Windows Server」と入力">
3. VM を作成する Windows Server バージョンは複数から選択できます。 \[Windows Server] イメージ概要パネルで \[ソフトウェア プランの選択] ドロップダウン リストをクリックして、\[Windows Server 2019 Datacenter] を探します。  

   <img src="/images/hands-on-lab1-VM-002.png" title="Windows Server 2019 Datacenter">
4. \[作成] ボタンをクリックして、VM の構成を始めます。

 ### VM 設定を構成する
 
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

 ## Data Factory セルフホステッド統合ランタイムのインストール
 
1. リモートデスクトップで仮想マシンにログインします。
   <img src="/images/hands-on-lab1-VMConfig-001.png" title="リモートデスクトップ">
   
2. IE Enhanced Security Configuration を Off にします。この後の作業に際し **Edge や Chrome をダウンロード**しておく事を推奨します。
   <img src="/images/hands-on-lab1-VMConfig-002.png" title="IE Enhanced Security Configuration を OFF">
   
3. 次のドキュメントの手順に従い、Microsoft ダウンロード センターからセルフホステッド IR をインストールして登録します。**言語は全て English (United States)** で実行します。
   
   https://docs.microsoft.com/ja-jp/azure/data-factory/create-self-hosted-integration-runtime#install-and-register-a-self-hosted-ir-from-microsoft-download-center
   
4. **Azure Data Factory を構成**した際に控えておいた Key1 を入力して Self-Hosted Runtime の**登録**を完了します。

   <img src="/images/hands-on-lab1-VMConfig-003.png" title="Self-Hosted Runtime の登録">

5. あとは既定値のまま進み **\[Launch Configuration Manager]** で正常に Self-hosted node が cloud serrvice に接続されている事を確認します。

   <img src="/images/hands-on-lab1-VMConfig-004.png" title="Self-Hosted Runtime の稼働確認">

 ## Azure Storage Explorer のインストール

サービスエンドポイント経由で Azure Storage への接続確認のために、Azure Storage Explorer をインストールします。

Azure Storage Explorer のダウンロード  
https://azure.microsoft.com/ja-jp/features/storage-explorer/

 ## SQL Server Management Studio (SSMS) のインストール

サービスエンドポイント経由で Azure SQL Database への接続確認のために、SQL Server Management Studio (SSMS) をインストールします。
 
SQL Server Management Studio (SSMS) のダウンロード  
https://docs.microsoft.com/ja-jp/sql/ssms/download-sql-server-management-studio-ssms

# Azure Storage のデプロイ
1. [Azure portal](https://portal.azure.com)  にサインインします。
2. Azure portal の左上メニューまたはホームページから **\[リソースの作成]** を選択します。

   <img src="/images/hands-on-lab1-ADF-001.png" title="リソースの作成">
3. **\[ストレージ]** を選択してから、 **\[ストレージアカウント]** を選択します。

   <img src="/images/hands-on-lab1-Storage-001.png" title="ストレージアカウントの作成">
4. **\[サブスクリプション]** で、データ ファクトリを作成する Azure サブスクリプションを選択します。

5. **\[リソース グループ]** で、**\[新規作成]** を選択し、リソース グループの名前に「**Storage-rg**」と入力します。

6. **\[ストレージ アカウント名]** に、 **\[一意の名前]** を入力します。

7. **\[場所]** で、データ ファクトリの場所 **(Asia Pacific) 東南アジア** を選択します。

8. **\[レプリケーション]** で、**ローカル冗長ストレージ (LRS)** を選択します。
   <img src="/images/hands-on-lab1-Storage-002.png" title="ストレージアカウント 基本設定">
   
9. その他は既定値のまま **\[ 次: ネットワーク > ]** へ行きます。

