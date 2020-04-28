# Azure PaaS をセキュアな閉域ネットワークで利用する
Hands-on：Moving data between SQL Database and Azure Storage by using Data Factory inside secure network.

## はじめに
　多くのエンタープライズ企業の求める要件に、クラウドであってもセキュアな閉域ネットワークに自社のサービスを展開したいという要望があります。仮想マシンに代表される Azure IaaS であれば、[仮想ネットワーク](https://docs.microsoft.com/ja-jp/azure/virtual-network/virtual-networks-overview)にデプロイする事で要件をクリアする事は簡単ですが、Global IP でサービス提供されている Azure PaaS では構成に工夫が必要となってきます。
 
　このワークショップでは[サービスエンドポイント](https://docs.microsoft.com/ja-jp/azure/virtual-network/virtual-network-service-endpoints-overview)を利用する事で Azure PaaS と仮想ネットワークを閉域に接続し、更に [Azure Firewall](https://docs.microsoft.com/ja-jp/azure/firewall/overview) を組み込むことで、よりセキュアな閉域ネットワークでデータのやり取りをする構成を学びます。
 
 　次に示す２つの演習を通して、サービスエンドポイント、[ルートテーブル](https://docs.microsoft.com/ja-jp/azure/virtual-network/tutorial-create-route-table-portal)、[NSG](https://docs.microsoft.com/ja-jp/azure/virtual-network/security-overview)、Azure Firewall といった Azure のネットワーク周りのセキュリティ知識を深めていきます。
  
 ## 演習１ - 概要
 　[Data Factory セルフホステッド統合ランタイム](https://docs.microsoft.com/ja-jp/azure/data-factory/concepts-integration-runtime)を仮想マシンにインストールします。仮想マシンがデプロイされている仮想ネットワークのサブネットに対して Azure SQL Database と Azure Storage のサービスエンドポイントを設定します。この構成では Data Factory セルフホステッド統合ランタイムはセキュアに接続された 2 つのサービスエンドポイントを通してデータのやり取りが出来ます。  
　<img src="/images/シナリオ1.png" title="ハンズオン - シナリオ１">
  
 ## 演習２ - 概要
 　シナリオ１の仮想ネットワーク内に Azure Fiewall をデプロイします。Azure PaaS のサービスエンドポイントは AzureFirewallSubnet に対して設定します。Data Factory のコントロールプレーンを含め、全ての通信が Azure Fiewall 経由でセキュアに通信されるよう構成します。  
　<img src="/images/シナリオ2.png" title="ハンズオン - シナリオ２">

 ## 予備知識：サービスエンドポイントと Private Link の比較
 　サービスエンドポイントは、仮想ネットワーク内から直接 Azure PaaS の Global IP を呼び出します。Azure PaaS 側では自分が指定した仮想ネットワークのサブネットからしか着信を許可しないよう F/W が構成されます。他方[Private Link](https://docs.microsoft.com/ja-jp/azure/private-link/) は、仮想ネットワーク内にプライベートエンドポイントという NAT サービスが配置され、その Private IP アドレスが NAT 変換され Azure PaaS へ接続する仕組みとなります。
 
**サービスエンドポイントの特徴**
- [追加料金が不要](https://docs.microsoft.com/ja-jp/azure/virtual-network/virtual-network-service-endpoints-overview#pricing-and-limits)です。また NAT サービスなどが間に入らないので余計な通信オーバーヘッドも発生しません。
- サービスエンドポイントは、仮想ネットワークの特定サブネットに対して設定します。このためマップされたサブネット（上の VM 等）からのみ Azure PaaS へのアクセスが可能です。このデメリットは Azure Fiewall と組み合わせサービスエンドポイントを AzureFirewallSubnet に対して設定する事で緩和されます。
- Outgoing 方向の通信を自身の Azure PaaS インスタンスだけに絞るには、[サービスエンドポイントポリシー](https://docs.microsoft.com/ja-jp/azure/virtual-network/virtual-network-service-endpoint-policies-portal)を利用する必要がありますが、現在対応しているのは Azure Storage のみとなります。
 
**Private Link の特徴**
- Private Link は、オンプレミスやピアリングされた他の仮想ネットワークなど、NAT サービスの Private IP にリーチできる場所であれば、どこからでも Azure PaaS へセキュアにアクセスする事が可能です。
- 仮想ネットワーク内のプライベートエンドポイント (Private IP) と自身がデプロイした Azure PaaS インスタンスとの完全な 1 : 1 接続となります。
  
  
 ## [演習１ - Data Factory セルフホステッド統合ランタイムとサービスエンドポイント](/Hands-on-Lab1.md)**
 
 ## [演習２ - Azure Firewall によるセキュリティ強化](/Hands-on-Lab2.md)**
