# Azure PaaS をセキュアな閉域ネットワークで利用する
Hands-on：Moving data between Azure SQL Database and Azure Storage by using Azure Data Factory inside secure network.

## はじめに
　多くのエンタープライズ企業の求める要件に、クラウドであってもセキュアな閉域ネットワーク内に自社のサービスを展開したいという希望があります。仮想マシンに代表される Azure IaaS であれば、仮想ネットワーク内にデプロイする事で要件をクリアする事は簡単です。この Hands-on では、同様の対策が難しい Global IP でサービス提供されている Azure PaaS をサービスエンドポイントや Private Link を利用する事で仮想ネットワークと統合し、よりセキュアな閉域ネットワークで利用する為のノウハウを学びます。
 
 ## シナリオ１
 　Data Factory
