# Lab 1A: Azure Machine Learningワークスペースを作成する

このラボでは、このコースの残りの部分で使用するAzure Machine Learningワークスペースを作成します。

## 始める前に

まだお持ちでない場合は、[https://azure.microsoft.com](https://azure.microsoft.com)で無料試用版にサインアップできます。

このコース用に提供されたAzure Passサブスクリプションを使用している場合、アカウントがサブスクリプション**所有者**ロールに割り当てられていない可能性があります。これは、このコースの一部の操作に必要な場合があります。アカウントを**所有者**ロールに追加するには：

1. [Azureポータル](https://portal.azure.com)にサインインして、**サブスクリプション**を表示します。
2. Azureパスを使用して作成したサブスクリプションを選択します（おそらく**Azure Pass-スポンサーシップ**のような名前になります）。
3.サブスクリプションのブレードで、**My Permissions**ページを開き、そのリンクをクリックして、サブスクリプションの完全なアクセス詳細を表示します。
4. [**役割の割り当てを追加**]で、[**追加**]をクリックします。次に、以下を選択して**保存**をクリックします。

- **役割**：所有者
- **アクセスを割り当てる**：Azure ADユーザー、グループ、またはサービスプリンシパル
- **選択**：Azureへのサインインに使用したMicrosoftアカウント。




## Task 1: Azure MLワークスペースを作成する

その名前が示すように、ワークスペースは、機械学習プロジェクトで作業するために必要なすべてのAzure ML資産を管理するための集中管理された場所です。

1. [Azureポータル](https://portal.azure.com)で、新しい** Machine Learning **リソースを作成し、一意のワークスペース名を指定して、** North Central US *に新しいリソースグループを作成します*または** UK South **地域。 ** Enterprise **ワークスペースエディションを選択します。


   > **Note**: Basicエディションのワークスペースは低コストですが、Auto ML、ビジュアルデザイナー、データドリフトモニタリングなどの機能は含まれません。詳細については、[Azure Machine Learningの価格設定](https://azure.microsoft.com/en-us/pricing/details/machine-learning/)を参照してください。

2.ワークスペースとそれに関連するリソースが作成されたら、ポータルでワークスペースを表示します。

## Task 2: Azure ML Studioインターフェイスを調べる

Azureポータルでワークスペース資産を管理できますが、データサイエンティスト向けのこのツールには、一般的なAzureリソースの管理に関連する多くの無関係な情報とリンクが含まれています。代わりに、ワークスペースを管理するためのAzure ML固有のWebインターフェイスを利用できます。

> **Note**: Azure MLのWebベースのインターフェイスの名前は **Azure Machine Learning studio**です。ビジュアルデザイナーを使用して機械学習モデルを作成するための無料の**Azure Machine Learning Studio**製品もあるため、混乱するかもしれません。このビジュアルデザイナーのよりスケーラブルなバージョンは、新しいスタジオインターフェイスに含まれています。

1. Azure Machine LearningワークスペースのAzureポータルブレードで、リンクをクリックして**Azure Machine Learning studio**を起動します。または、新しいブラウザタブで[https://ml.azure.com]（https://ml.azure.com）を開きます。プロンプトが表示されたら、前のタスクで使用したMicrosoftアカウントを使用してサインインし、Azureサブスクリプションとワークスペースを選択します。

1. ワークスペースのAzure Machine Learning Studioインターフェイスを表示します。ここからワークスペース内のすべてのアセットを管理できます。

## Task 3: コンピューティングリソースの作成

Azure Machine Learningの利点の1つは、実験やトレーニングスクリプトを大規模に実行できるクラウドベースのコンピューティングを作成できることです。


1. ワークスペースのAzure Machine Learning Studio Webインターフェイスで、**Compute**ページを表示します。ここで、データサイエンスアクティビティのすべての計算ターゲットを管理します。

1. [**Compute Instances**]タブで、新しいコンピューティングインスタンスを追加し、一意の名前を付けて、**STANDARD_DS3_V2** VMタイプテンプレートを使用します。このVMは、後続のラボで開発環境として使用します。

1. ノートブックVMの作成中に、**トレーニングクラスター**タブに切り替え、次の設定で新しいトレーニングクラスターを追加します。

    * **計算名**：aml-cluster
    * **仮想マシンのサイズ**：Standard_DS12_V2
    * **仮想マシンの優先度**：専用
    * **最小ノード数**：0
    * **ノードの最大数**：4
    * **スケールダウンのアイドル秒数**：120
    
    **推論クラスター**タブに注意してください。ここで、クライアントアプリケーションが使用するWebサービスとしてトレーニング済みモデルを展開する計算ターゲットを作成および管理できます。

4. **Attached Compute**タブに注意してください。これは、ワークスペースの外部に存在する仮想マシンまたはDatabricksクラスターを接続できる場所です。


    > **Note**: コースの後半で、コンピューティングターゲットについて詳しく説明します。

## Task 4:データリソースを作成する

データの処理に使用できるコンピューティングリソースが用意できたので、処理するデータを保存および取り込む方法が必要になります。

1. *Studio*インターフェースで、**Datastores**ページを表示します。 Azure MLワークスペースには、ワークスペースと共に作成されたAzureストレージアカウントに基づく2つのデータストアが既に含まれています。これらは、ノートブック、構成ファイル、およびデータの保存に使用されます。

   > **Note**: 実際の環境では、ビジネスデータストアを参照するカスタムデータストア（たとえば、Azure BLOBコンテナー、Azure Data Lakes、Azure SQL Databaseなど）を追加する可能性があります。これについては、コースの後半で学習します。

2. *Studio*インターフェースで、**データセット**ページを表示します。データセットは、Azure MLで作業する予定の特定のデータファイルまたはテーブルを表します。

3. 次の設定を使用して、Webファイルから新しいデータセットを作成します:

    * **Web URL**：https://aka.ms/diabetes-data
    * **名前**：diabetesdataset（*大文字と小文字の区別に注意してください*）
    * **データセットタイプ**：表形式
    * **説明**：糖尿病データ
    * **設定とプレビュー**：自動検出された設定を確認します。
    * **スキーマ**：デフォルトの列選択とデータ型を確認します。

4. データセットが作成されたら、それを開き、**Explore**ページを表示してデータのサンプルを表示します。このデータは、糖尿病の検査を受けた患者の詳細を表します。このデータは、このコースの後続の多くのラボで使用します。

    > **Note**: オプションで、データセットの*プロファイル*を生成して、詳細を確認できます。コースの後半で、データセットをさらに詳しく調べます。
