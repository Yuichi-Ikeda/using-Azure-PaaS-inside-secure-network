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
2. 必須項目 <span style="color: red; ">*</span> の各種パラメータを以下のように設定します。VM サイズは [Data Factory 前提要件](https://docs.microsoft.com/ja-jp/azure/data-factory/create-self-hosted-integration-runtime#prerequisites)の最小構成をクリアするように **D4s v3** に変更します。  
   <img src="/images/hands-on-lab1-004.png" title="各種パラメータを設定">
