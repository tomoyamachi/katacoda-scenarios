# Kubernetesの基礎

Kubernetesは、もっとも利用されているコンテナオーケストレーションツールです。
Kubernetesを利用することで、複数あるコンテナの設定や管理を自動化することができます。

# `kubectl`を使って、Kubernetes APIを呼び出す

Kubernetesは、原則、以下の構成でクラスターをつくります。
- 1つのControl Plane : 管理システム
- 1つ以上のNode : 実際にアプリケーションを動かす

`以前は、Master NodeとWorker Nodeと呼ばれていましたが、奴隷制を喚起させる呼称で不適切だという批判があり、名称が変更されました。過去の記事を読むときは、古い呼称が使われているので変換してください。`

そして、Kubernetesは、Control Planeで動いているKubernetes APIを利用して、クラスターを操作することができます。実際に動かしてみましょう。

`kubectl config view`{{execute}}

Kubernetes APIが動作していれば、以下のような情報が出てきます。

```
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://172.17.0.59:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: kubernetes-admin
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
```