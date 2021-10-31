# ファイルで宣言する

ここまでは、Kubernetesオブジェクトをインタラクティブに作ってきました。 しかし、毎回、手動で構成管理をするのは大変です。
Kubernetesを運用する際は、KubernetesオブジェクトのYAMLファイルを作っておき、そのファイルを利用してオブジェクトを作るのが一般的です。

YAMLファイルの書き方を覚えるのは大変なので、すこしズルをして、現在動いているDeploymentの情報をYAMLファイルに保存します。

`kubectl get deployment/echo-node -o yaml > deploy.yaml`{{execute}}

そして、Deploymentを消します。コマンドを実行し、Deploymentが削除されたあとに、さらに待つと、Deploymentに紐づくPodが消えます。

`kubectl delete deployment/echo-node`{{execute}}

Deploymentが削除したあとも、紐づくPodが削除されるまで少し時間がかかります。

`kubectl get pods`{{execute}}

Podがなくなったのを確認したら、さきほどのYAMLファイルからDeploymentオブジェクトをつくりましょう。

`kubectl apply -f ./deploy.yaml`{{execute}}

先ほどと同じように、3つのPodが作成されるのがわかります。

`kubectl get pods`{{execute}}

# YAMLファイルの書き方

既存のリソースから自動生成したファイルには不要な情報も入っています。必要最低限の情報だけを記入したYAMLファイルは以下のようになります。運用する場合、このようなファイルをたくさん作ってInfrastructure as Code (IaC)を実現します。

```

apiVersion: apps/v1
kind: Deployment
metadata:
  name: echo-node
  labels:
    app: echo-node
spec:
  replicas: 3
  selector:
    matchLabels:
      app: echo-node
  template:
    metadata:
      labels:
        app: echo-node
    spec:
      containers:
      - name: echoserver
        image: k8s.gcr.io/echoserver:1.10
        imagePullPolicy: IfNotPresent

```
