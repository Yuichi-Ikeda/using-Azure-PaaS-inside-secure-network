# Azure PaaS をセキュアな閉域ネットワークで利用する
Hands-on：Moving data between SQL Database and Azure Storage by using Data Factory inside secure network.

## はじめに
　多くのエンタープライズ企業の求める要件に、クラウドであってもセキュアな閉域ネットワーク内に自社のサービスを展開したいという要望があります。仮想マシンに代表される Azure IaaS であれば、[仮想ネットワーク](https://docs.microsoft.com/ja-jp/azure/virtual-network/virtual-networks-overview)内にデプロイする事で要件をクリアする事は簡単ですが、Global IP でサービス提供されている Azure PaaS では構成に工夫が必要となってきます。
 
　この Hands-on Workshop では[サービスエンドポイント](https://docs.microsoft.com/ja-jp/azure/virtual-network/virtual-network-service-endpoints-overview)や [Private Link](https://docs.microsoft.com/ja-jp/azure/private-link/) を利用する事で Azure PaaS と仮想ネットワークを結合し、さらに Azure Firewall を組み込むことで、よりセキュアな閉域ネットワークでデータのやり取りをする為の構成を学びます。

 ## サービスエンドポイントと Private Link の比較
 　Private Link は、仮想ネットワーク内にプライベートエンドポイントという NAT サービスが配置され、その Private IP アドレスが NAT 変換され Azure PaaS へ接続する仕組みとなります。他方サービスエンドポイントは、仮想ネットワーク内から直接 Azure PaaS の Global IP を呼び出すことになります。Azure PaaS 側では自分が指定した仮想ネットワークのサブネットからしか着信を許可しないよう F/W が構成されます。
 
**サービスエンドポイントの特徴**
- [追加料金が不要](https://docs.microsoft.com/ja-jp/azure/virtual-network/virtual-network-service-endpoints-overview#pricing-and-limits)です。また NAT サービスなどが間に入らないので余計な通信オーバーヘッドも発生しません。
- サービスエンドポイントは、仮想ネットワークの特定サブネットに対して設定します。このためマップされたサブネット（上の VM 等）からのみ Azure PaaS へのアクセスが可能です。このデメリットは Azure Fiewall と組み合わせサービスエンドポイントを AzureFirewallSubnet に対して設定する事で緩和されます。
- Outgoing 方向の通信を自身の Azure PaaS インスタンスだけに絞るためには、サービスエンドポイントポリシーを利用する必要があります。
 
**Private Linkの特徴**
- Private Link は、オンプレミスやピアリングされた他の仮想ネットワークなど、NAT サービスの Private IP にリーチできる場所であれば、どこからでも Azure PaaS へセキュアにアクセスする事が可能です。
- 仮想ネットワーク内のプライベートエンドポイント (Private IP) と自身がデプロイした Azure PaaS インスタンスとの完全な 1 : 1 接続となります。

 ## シナリオ１
 　[Data Factory セルフホステッド統合ランタイム](https://docs.microsoft.com/ja-jp/azure/data-factory/concepts-integration-runtime)を仮想マシンにインストールします。仮想マシンがデプロイされている仮想ネットワークのサブネットに対して Azure SQL Database と Azure Storage のサービスエンドポイントを設定します。Data Factory セルフホステッド統合ランタイムはセキュアに接続された 2 つのサービスエンドポイントを通してデータのやり取りが出来ます。
