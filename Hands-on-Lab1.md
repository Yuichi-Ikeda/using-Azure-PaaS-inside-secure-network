 # 演習１ - Data Factory セルフホステッド統合ランタイムとサービスエンドポイント
 
 ## 仮想マシンのデプロイ
1. [Azure portal](https://portal.azure.com)  にサインインします。
2. Azure portal の左上メニューまたはホームページから **\[リソースの作成]** を選択します。
3. 検索ボックスに「Windows Server」と入力し、表示された一覧の中から同じタイトルのリンクをクリックします。  

   <img src="/images/hands-on-lab1-001.png" title="検索ボックスに「Windows Server」と入力">
4. VM を作成する Windows Server バージョンは複数から選択できます。 \[Windows Server] イメージ概要パネルで \[ソフトウェア プランの選択] ドロップダウン リストをクリックして、\[Windows Server 2019 Datacenter] を探します。  

   <img src="/images/hands-on-lab1-002.png" title="Windows Server 2019 Datacenter">
5. \[作成] ボタンをクリックして、VM の構成を始めます。

 ## VM 設定を構成する
 
　ポータルでの VM 作成エクスペリエンスは "ウィザード" の形式で提示され、VM のすべての構成領域が順番に示されます。 [次へ] ボタンをクリックすると、次の構成可能なセクションに移動します。 しかし、上部に並んでいるタブで各セクションが示されており、自由にセクション間を移動できます。

1. リソースグループ **DataFactory-Lab01-rg** を新規作成します。  

   <img src="/images/hands-on-lab1-003.png" title="リソースグループを作成">
2. 必須項目 **\*** の各種パラメータを以下のように設定します。VM サイズは [Data Factory 前提要件](https://docs.microsoft.com/ja-jp/azure/data-factory/create-self-hosted-integration-runtime#prerequisites)の最小構成をクリアするように **D4s v3** に変更します。  
   <img src="/images/hands-on-lab1-004.png" title="各種パラメータを設定">
3. その他のパラメーターは既定値のまま **\[ 次: ディスク > ]** へ行きます。 
4. ディスク項目も既定値のまま **\[ 次: ネットワーク > ]** へ行きます。  
  
   仮想ネットワークの **\[ 新規作成 ]** をクリックし、以下のようなアドレス空間とサブネットに構成します。
   <img src="/images/hands-on-lab1-005.png" title="ネットワーク設定">
   
5. **\[ 次: 管理 > ]** へ行きます。 **\[ ブート診断 ]** をオフに設定し、これ以外は既定値とします。
 
6. **\[ 確認および作成 ]** をクリックし、検証に成功したら、そのまま **\[ 作成 ]** をします。

   デプロイ作業が開始され、数分後に仮想マシンのデプロイが完了します。

 ## Data Factory セルフホステッド統合ランタイムのインストール
 
1. リモートデスクトップで仮想マシンにログインします。
   <img src="/images/hands-on-lab1-006.png" title="リモートデスクトップ">
2. 次のドキュメントの手順に従い、Microsoft ダウンロード センターからセルフホステッド IR をインストールして登録します。
   
   https://docs.microsoft.com/ja-jp/azure/data-factory/create-self-hosted-integration-runtime#install-and-register-a-self-hosted-ir-from-microsoft-download-center
   
3．注意点は以下です。

4. テスト


