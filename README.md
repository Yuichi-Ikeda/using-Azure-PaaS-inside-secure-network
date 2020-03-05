# Azure PaaS をセキュアな閉域ネットワークで利用する
Hands-on：Moving data between SQL Database and Azure Storage by using Data Factory inside secure network.

## はじめに
　多くのエンタープライズ企業の求める要件に、クラウドであってもセキュアな閉域ネットワーク内に自社のサービスを展開したいという希望があります。仮想マシンに代表される Azure IaaS であれば、[仮想ネットワーク](https://docs.microsoft.com/ja-jp/azure/virtual-network/virtual-networks-overview)内にデプロイする事で要件をクリアする事は簡単です。
 
　この Hands-on では、同様の対策が難しい Global IP でサービス提供されている Azure PaaS を[サービスエンドポイント](https://docs.microsoft.com/ja-jp/azure/virtual-network/virtual-network-service-endpoints-overview)や [Private Link](https://docs.microsoft.com/ja-jp/azure/private-link/) を利用する事で仮想ネットワークと統合し、よりセキュアな閉域ネットワークで利用する為のノウハウを学びます。
 
 ## シナリオ１
 　[Data Factory セルフホステッド統合ランタイム](https://docs.microsoft.com/ja-jp/azure/data-factory/concepts-integration-runtime)を仮想マシンにインストールします。仮想マシンがデプロイされている仮想ネットワークのサブネットに対して Azure SQL Database と Azure Storage のサービスエンドポイントを設定します。Data Factory セルフホステッド統合ランタイムはセキュアに接続された 2 つのサービスエンドポイントを通してデータのやり取りが出来ます。
