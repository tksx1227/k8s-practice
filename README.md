# k8s-practice
kubernetes勉強用リポジトリ

## k8s概要
コンテナを複数束ねたものをPodとよび、k8sが管理する最小単位となる。

複数のPodの集合をNodeと呼ぶ。Nodeにはマスターとワーカーの２種類があり、Podを持つものはワーカーノードである。マスターノードはユーザーからの指示を受けるクラスタ制御用のノード。

複数のNodeの集合をクラスタと呼ぶ。

## 用語

## コマンドなど
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
$ minikube addons enable [ADDON_NAME]
$ minikube addons unable [ADDON_NAME]

# kubectlの基本構文
# kubectl [command] [TYPE] [NAME] [flags]

# バージョン確認
$ kubectl version

# クラスタの情報を見る
$ kubectl cluster-info

# クラスタ内の情報を見る
# オプション <-l KEY=VALUE> をつけると特定ラベルを持つものを操作対象として指定できる
$ kubectl get nodes   # ノード
$ kubectl get deployments   # デプロイ
$ kubectl get services   # サービス
$ kubectl get pods   # Pod
$ kubectl get events   # イベント（操作ログ）

# 設定を確認する
$ kubectl config view

# デプロイを作成する
$ kubectl create deployment [DEPLOY_NAME] [--image=IMAGE_URL]

# サービスを作成してアプリケーションを公開する
# portは最終的にアクセスするアプリケーションのポート番号を指定する
$ kubectl expose deployment [DEPLOY_NAME] --type=LoadBalancer --port=80

# プロキシサーバーを立てる
$ kubectl proxy

# 指定したコンテナでコマンドを実行する
# Podにコンテナが１つしかない場合はコンテナ名を省略可
$ kubectl exec [POD_NAME] <-c [CONTAINER_NAME]> -- [CMD]

# オブジェクトの詳細をみる（コマンドは一例）
$ kubectl describe services [SERVICE_NAME]
$ kubectl describe pods [POD_NAME]

# オブジェクトにラベルを付与する（コマンドは一例）
$ kubectl label pods [POD_NAME] [KEY]=[VALUE]
```

