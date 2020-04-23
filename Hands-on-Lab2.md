 # 演習２ - Azure Firewall によるセキュリティ強化

　[演習１](/Hands-on-Lab1.md)では、仮想ネットワークと Azure PaaS をサービスエンドポイントで結び、仮想ネットワーク上の仮想マシンにデプロイした Data Factory のセルフホステッド統合ランタイムがサービスエンドポイント経由でデータのコピーを実行するシナリオを構築しました。演習２では、この環境に Azure Firewall を組み込むことで Outgoing 方向の通信をより厳密に制限し、セキュリティを強化していきます。

 # Azure Firewall のデプロイ
1. [Azure portal](https://portal.azure.com)  にサインインします。
2. Azure portal の左上メニューまたはホームページから **\[リソースの作成]** を選択します。

   <img src="/images/hands-on-lab1-ADF-001.png" title="リソースの作成">
3. **\[分析]** を選択してから、 **\[Data Factory]** を選択します。
