# 演習２ - Azure Firewall によるセキュリティ強化

　[演習１](/Hands-on-Lab1.md)では、仮想ネットワークと Azure PaaS をサービスエンドポイントで結び、仮想ネットワーク上の仮想マシンにデプロイした Data Factory のセルフホステッド統合ランタイムがサービスエンドポイント経由でデータのコピーを実行するシナリオを構築しました。演習２では、この環境に Azure Firewall を組み込むことで Outgoing 方向の通信をより厳密に制限し、セキュリティを強化していきます。

# 仮想ネットワークへ AzureFirewallSubnet を追加
　Azure Firewall はサイズが /26 以上の AzureFirewallSubnet という決まった名前のサブネットにデプロイする必要があります。
1. [Azure portal](https://portal.azure.com)  にサインインします。
1. 演習１でデプロイした **DataFactoryVNet** を選択します。
1. **\[サブネットの追加]** で、以下の値を入力します。 **[名前]** は「**AzureFirewallSubnet**」である必要があります。

    ![サブネットの追加](/images/add-azurefirewallsubnet.png)
1. **\[OK]** を選択して、完了します。

 
# Azure Firewall のデプロイ
1. Azure portal の左上メニューまたはホームページから **\[リソースの作成]** を選択します。

    ![リソースの作成](/images/hands-on-lab1-ADF-001.png)
1. **\[ネットワーキング]** を選択してから、 **\[Firewall]** を選択します。
1. **[ファイアウォールの作成]** ページで、以下のようにファイアウォールを構成します。パブリック IP アドレスは、 **\[新規追加]** に追加します。

    ![ファイアウォールを構成](/images/deploy-azurefirewall.png)
1. **[確認および作成]** を選択します。
1. 概要を確認し、 **[作成]** を選択してファイアウォールを作成します。

   デプロイが完了するまでに数分かかります。
1. デプロイが完了したら、**DataFactory-Lab01-rg** リソース グループに移動し、**Firewall** リソースを選択します。
1. プライベート IP アドレスをメモします。 後で既定のルートを作成するときにこれを使用します。
