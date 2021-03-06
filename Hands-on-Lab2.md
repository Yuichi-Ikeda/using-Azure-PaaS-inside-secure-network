# 演習２ - Azure Firewall によるセキュリティ強化

　[演習１](/Hands-on-Lab1.md)では、仮想ネットワークと Azure PaaS をサービスエンドポイントで結び、仮想ネットワーク上の仮想マシンにデプロイした Data Factory のセルフホステッド統合ランタイムがサービスエンドポイント経由でデータのコピーを実行するシナリオを構築しました。演習２では、この環境に Azure Firewall を組み込むことで Outgoing 方向の通信をより厳密に制限し、セキュリティを強化していきます。

# 仮想ネットワークへ AzureFirewallSubnet を追加
　Azure Firewall はサイズが /26 以上の AzureFirewallSubnet という決まった名前のサブネットにデプロイする必要があります。
1. [Azure portal](https://portal.azure.com)  にサインインします。
1. 演習１でデプロイした **DataFactoryVNet** を選択します。
1. **[サブネットの追加]** で、以下の値を入力します。 **[名前]** は「**AzureFirewallSubnet**」にする必要があります。

    ![サブネットの追加](/images/add-azurefirewallsubnet.png)
1. **[OK]** を選択して、完了します。

 
# Azure Firewall のデプロイ
1. Azure portal の左上メニューまたはホームページから **[リソースの作成]** を選択します。

    ![リソースの作成](/images/hands-on-lab1-ADF-001.png)
1. **[ネットワーキング]** を選択してから、 **[Firewall]** を選択します。
1. **[ファイアウォールの作成]** ページで、次のようにファイアウォールを構成します。ファイアウォール パブリック IP アドレスは、 **[新規追加]** します。

    ![ファイアウォールを構成](/images/deploy-azurefirewall.png)
1. **[確認および作成]** を選択します。
1. 概要を確認し、 **[作成]** を選択してファイアウォールを作成します。

   デプロイが完了するまでに数分かかります。
1. デプロイが完了したら、**DataFactory-Lab01-rg** リソース グループに移動し、**Firewall** リソースを選択します。
1. プライベート IP アドレスをメモします。 次にで既定のルートを作成するときにこれを使用します。

    ![プライベート IP](/images/add-azurefirewall-privateip.png)
## 既定のルートを作成する

**Workload-SN** サブネットでは、アウトバウンドの既定ルートがファイアウォールを通過するように構成します。

1. Azure portal メニューで **[すべてのサービス]** を選択するか、または任意のページから *[すべてのサービス]* を検索して選択します。
2. **[ネットワーキング]** で、 **[ルート テーブル]** を選択します。
3. **[追加]** を選択します。
4. **[名前]** に「**Firewall-route**」と入力します。
5. **[サブスクリプション]** で、ご使用のサブスクリプションを選択します。
6. **[リソース グループ]** で、 **[DataFactory-Lab01-rg]** を選択します。
7. **[場所]** で、 **[(Asia Pacific) 東南アジア]** を選択します。
8. **［作成］** を選択します
9. 作成が完了したら、 **[Firewall-route]** ルート テーブルを選択します。
10. **[サブネット]** を選択し、 **[関連付け]** を選択します。

    ![ルート テーブル](/images/firewall-routetable.png)
11. **[仮想ネットワーク]**  >  **[DataFactoryVNet]** の順に選択します。
12. **[サブネット]** で、 **[ADFSubnet]** を選択します。 必ずこのルートの **ADFSubnet** サブネットのみを選択してください。それ以外の場合、ファイアウォールが正常に動作しません。

13. **[OK]** を選択します。
14. **[ルート]** 、 **[追加]** の順に選択します。
15. **[ルート名]** に「**fw-dg**」と入力します。
16. **[アドレス プレフィックス]** に「**0.0.0.0/0**」と入力します。
17. **[次ホップの種類]** で、 **[仮想アプライアンス]** を選択します。

    Azure Firewall は実際はマネージド サービスですが、この状況では仮想アプライアンスが動作します。
18. **[次ホップ アドレス]** に、前にメモしておいたファイアウォールのプライベート IP アドレスを入力します。
19. **[OK]** を選択します。

　この状態で仮想マシンからのデフォルトルート(0.0.0.0/0)が Azure Firewall に向けられたため、インターネットからの着信、インターネットへの送信が全て Azure Firewall を経由する事になります。まずは RDP によるリモートデスクトップ接続を許可するために Azure Firewall で NAT ルールを追加します。

## RDP 用の NAT ルールの追加
1. Azure Firewall の **[ルール]** 、 **[NAT ルール コレクションの追加]** の順に選択します。

    ![NAT ルール コレクションの追加](/images/azurefirewall-NAT-rule-001.png)
1. 次のように値を入力します。 **[宛先アドレス]** は Azure Fireall のパブリック IP アドレス、 **[変換されたアドレス]** は ADFRuntime 仮想マシンのプライベート IP アドレス（静的に設定）となります。

    ![NAT ルール コレクションの追加](/images/azurefirewall-NAT-rule-002.png)
    ※ 必要に応じてソース IP アドレスもクライアント端末のグローバル IP に絞っていただく方がベターです。
1. 構成が完了したら、次のように Azure Fireawll のパブリック IP アドレスに対してリモートデスクトップ接続をします。 Azure Fireall によりポート 3389 が NAT され仮想マシンに接続がされます。

    ![リモートデスクトップ接続](/images/RemoteDesktop.png)

# 不要なリソースの削除

　ここではインターネットに直接接続されなくなり不要となった ADFRuntime 仮想マシンの Public IP アドレスや NSG ルールを削除していきます。

## 仮想マシンのパブリック IP アドレスの削除
 1. Azure ポータルより ADFRuntime 仮想マシンの ネットワークインターフェースを選択します。
 1. **[IP 構成]** にて赤枠部分をクリックします。
 
     ![IP 構成](/images/Remove-PublicIP-From-NIC-001.png)
 1. **[パブリック IP アドレス]** の **「関連付け解除」** を選択します。 **[プライベート IP アドレス]** の割り当ても **「静的」** とします。
 
     ![関連付け解除](/images/Remove-PublicIP-From-NIC-002.png)
 1. **[保存]** を選択して構成を保存します。
 1. 次にパブリック IP アドレス **[ADFRuntime-ip]** の **[概要]** を選択し、**[削除]** を選択します。
 
      ![Public IP アドレス削除](/images/Delete-ADFRuntime-Public-IP.png)
      
## NSG ルールの削除

1. **[ADFRuntime-nsg]** の **[概要]** を選択します。
1. 赤枠のルールを全て削除します。

     ![NSG ルールの削除](/images/Delete-NSG-Rule.png)
　※ インターネットへの **Outgoing** は Azure Firewall によりブロックされています。

# サービスエンドポイントの再設定

　ここでは既存のサービスエンドポイントを削除してから、AzureFirewallSubnet に対してサービスエンドポイントを再設定していきます。

## サービスエンドポイントの削除

1. 最初に **[サービスエンドポイント ポリシー]** で **[サブネットの関連付けを解除]** （チェックを外）します。

   ![サブネットの関連付けを解除](/images/remove-service-endpoint-policy-001.png)
1. 次に **[サービスエンドポイント ポリシー]** の **[概要]** で **[削除]** をします。

   ![サブネットの関連付けを解除](/images/remove-service-endpoint-policy-002.png)
1. 最後に **[DataFactoryVNet]** の **[サービスエンドポイント]** を選択し、２つある既存のサービスエンドポイント Microsoft.Storage, Microsoft.Sql の両方を削除します。

   ![サービスエンドポイントの削除](/images/remove-service-endpoint.png)

## Storage サービスエンドポイントの再設定
1. **[demostorageadf]** の **[ファイアウォールと仮想ネットワーク]** で既存のサブネットを削除します。

   ![既存のサブネットを削除](/images/replace-storage-service-endpoint-001.png)
1. **[既存の仮想ネットワークを追加]** で **[AzureFirewallSubnet]** を **[有効化]** をします。

   ![有効化](/images/replace-storage-service-endpoint-002.png)
1. 有効化が完了したら、続けて **[追加]** を選択し、最後に **[保存]** をします。

   ![保存](/images/replace-storage-service-endpoint-003.png)

## SQL Database サービスエンドポイントの再設定
1. **[demodbadf]** の **[サーバー ファイアウォールの設定]** をクリックします。

   ![サーバー ファイアウォールの設定](/images/replace-sql-service-endpoint-001.png)
1. **[ファイアウォール設定]** で既存の関連付けをクリックし **[AzureFirewallSubnet]** を **[有効]** にします。

   ![サブネットの関連付け](/images/replace-sql-service-endpoint-002.png)
1. 有効化が完了したら、続けて **[OK]** を選択します。

   ![設定](/images/replace-sql-service-endpoint-003.png)

## サービスエンドポイントの確認
　最後に **[DataFactoryVNet]** の **[サービスエンドポイント]** を選択し、以下のように AzureFirewallSubnet に対して**成功**している事を確認します。

   ![サービスエンドポイントの確認](/images/reconfirme-service-endpoint.png)
   
# Azure Fiewall のルール設定
　Azure Firewall は許可ルールを追加しない限りは、既定で全ての通信を遮断しています。ここまでの構成では、仮想マシンへのリモートデスクトップ接続のために RDP 用の NAT ルールだけが適用されています。ここでは仮想ネットワーク側から Azure Storage, SQL Database, Data Factory サービスへアクセスするためのアプリケーション ルール (FQDN) を追加していきます。
 
1. **[Firewall]** の **[ルール]** で **[アプリケーション ルール コレクション]** を選択し、 **[アプリケーション ルール コレクションの追加]** をクリックします。

   ![アプリケーション ルール コレクションの追加](/images/azure-firewall-application-rule-001.png)
1. **[AllowOutgoingFQDN]** という名前で、以下のように5つの FQDN ターゲットを **[追加]** します。

   ![FQDN ターゲットの追加](/images/azure-firewall-application-rule-002.png)

   **参考資料：セルフホステッド統合ランタイムのファイアウォール構成**   
   https://docs.microsoft.com/ja-jp/azure/data-factory/create-self-hosted-integration-runtime#ports-and-firewalls

# 接続の確認
　演習１で実施したように、リモートデスクトップで仮想マシンにログインし Azure PaaS への接続を確認します。
 
1. Azure Storage Explorer によるストレージへの接続確認
1. SQL Server Management Studio (SSMS) による SQL Database への接続確認

# Data Factory パイプラインの実行
　接続の確認が成功したら、同じく演習１で実施したように Data Factory パイプラインのデバッグ実行やトリガーによる実行を確認します。

# まとめ
　演習２では、全ての通信を Azure Firewall 経由で行うように構成する事で、仮想ネットワークからの Outgoing(送信方向) 通信を制限し情報漏洩のセキュリティ強化を実施しています。セルフホステッド統合ランタイムのファイアウォール構成で *.servicebus.windows.net* のようなワイルドカード(\*) 指定の FQDN はより厳密に制限を絞る余地がまだありますが、演習1、演習2を通して Azure Firewall とサービスエンドポイントの組み合わせによるセキュリティ強化について理解が深まれば幸いです。
