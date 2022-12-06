# k8s-practice
kubernetes勉強用リポジトリ

# Kubernetes
コンテナを複数束ねたものをPodとよび、k8sが管理する最小単位となる。

複数のPodの集合をNodeと呼ぶ。Nodeにはマスターとワーカーの２種類があり、Podを持つものはワーカーノードである。マスターノードはユーザーからの指示を受けるクラスタ制御用のノード。

複数のNodeの集合をクラスタと呼ぶ。

## コマンドなど
**minikube**

```bash
# バージョン確認
$ minikube version

# クラスタを起動
$ minikube start

# クラスタの停止・削除
$ minikube stop
$ minikube delete

# ダッシュボードを開く
$ minikube dashboard

# アドオンを確認する
$ minikube addons list

# アドオンを有効化・無効化
$ minikube addons enable <ADDON_NAME>
$ minikube addons unable <ADDON_NAME>
```

**kubectl**

```bash
# kubectlの基本構文
# kubectl <-n [NAMESPACE]> [command] [TYPE] [NAME] [flags]

# バージョン確認
$ kubectl version

# クラスタの情報を見る
$ kubectl cluster-info

# 接続先一覧をみる
$ kubectl config get-contexts

# 接続先を切り替える
$ kubectl config use-context <CONTEXT_NAME>

# クラスタ内の情報を見る
# オプション <-l KEY=VALUE> をつけると特定ラベルを持つものを操作対象として指定できる
$ kubectl get nodes   # ノード
$ kubectl get deployments   # デプロイ
$ kubectl get services   # サービス
$ kubectl get pods   # Pod
$ kubectl get events   # イベント（操作ログ）
$ kubectl get rs   # レプリカセット

# 設定を確認する
$ kubectl config view

# デプロイを作成する
$ kubectl create deployment <DEPLOY_NAME> [--image=IMAGE]

# サービスを作成してアプリケーションを公開する
# portは最終的にアクセスするアプリケーションのポート番号を指定する
$ kubectl expose deployment <DEPLOY_NAME> --type=LoadBalancer --port=80

# プロキシサーバーを立てる
$ kubectl proxy

# 指定したコンテナでコマンドを実行する
# Podにコンテナが１つしかない場合はコンテナ名を省略可
$ kubectl exec <POD_NAME> [-c CONTAINER_NAME] -- <CMD>

# オブジェクトの詳細をみる（コマンドは一例）
$ kubectl describe services <SERVICE_NAME>
$ kubectl describe pods <POD_NAME>

# オブジェクトにラベルを付与する（コマンドは一例）
$ kubectl label pods <POD_NAME> <KEY>=<VALUE>

# レプリカの数を変更する
$ kubectl scale deployments/<DEPLOY_NAME> --replicas=<REPLICA_NUMBER>

# コンテナのイメージを変更する
$ kubectl set image deployements/<DEPLOY_NAME> <CONTAINER_NAME>=<IMAGE>

# ロールアウト状況を確認する
$ kubectl rollout status deployments/<DEPLOY_NAME>

# ロールアウトを直前の状態にリバートする
$ kubectl rollout undo deployments/<DEPLOY_NAME>

# マニュフェストファイルを読み込む
$ kubectl apply -f <FILE_PATH>
```

# Helm
helmはk8s用のパッケージマネージャーであり、よく使われるマニフェストをテンプレートとして使用することができる。

helmはリポジトリからチャートと呼ばれるテンプレートをk8sにインストールし、それをもとにアプリケーションを構築する。

この時チャートによって作られるアプリケーションをリリースと呼ぶ。

helmを使うことで、HTTPサーバーやデータベース、キャッシュなどを備えたWebアプリスタックなどの複雑なものを一括でデプロイすることができる。

リポジトリを追加すると、そこに含まれるチャートは全てクライアント側に保存される。

## コマンドなど

**helm**

```bash
# インストール済みのリリースを表示する
$ helm list

# 設定されたリポジトリを表示する
$ helm repo list

# リポジトリを追加する
$ helm repo add <REPOSITORY_NAME> <REPOSITORY_URL>

# リポジトリを削除する
$ helm repo remove <REPOSITORY_NAME>

# helmクライアントが持つチャートを最新のものに更新する
$ helm repo update

# Artifact Hub, ローカルに追加したリポジトリの中からChartを探す
$ helm search hub <TARGET_NAME>
$ helm search repo <TARGET_NAME>

# チャートをインストールする
# リリース名は任意の値を指定する
# --generate-nameオプションをつけるとリリース名を省略できる
# チャートは配布されたパッケージのパスを指定することもできる
$ helm install <RELEASE> <CHART>
$ helm instlal <CHART> --generate-name

# クラスタからリリースをアンインストールする
$ helm uninstall <RELEASE>

# リリースの状態や設定情報を確認する
$ helm status <RELEASE>

# チャートで設定可能なオプションを確認する
$ helm show values <CHART>

# 指定したリリースで独自に設定したオプションを表示する
$ helm get values <RELEASE>

# 既存のリリースをアップグレードする
# -fオプションでファイルを指定することで任意の差分を追加できる
# --installオプションをつけることで、リリースが存在しない場合はチャートのインストールを行う
$ helm upgrade [--install] [-f CONFIG_FILE] <RELEASE> <CHART>

# 独自のチャートを作成する
$ helm create <CHART>

# helmファイルを整形する
$ helm lint

# チャートを配布用にパッケージ化する
$ helm package <CHART>
```
