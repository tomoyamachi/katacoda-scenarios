# YAMLファイルの書き方

さきほど自動生成したファイルには不要な情報も入っています。必要最低限の情報だけを記入したYAMLファイルは以下のようになります。運用する場合、このようなファイルをたくさん作ってInfrastructure as Code (IaC)を実現します。


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


# 最後に

ここで説明したことは、Kubernetesを動かす上で必要な知識のほんの一部です。
本番環境でKubernetesを動かすには、膨大な知識が必要になるでしょう。

幸いなことに[Kubernetesの公式ドキュメント](https://kubernetes.io/ja/docs/home/)は日本語版も充実しており、多くの書籍や動画があります。ここで得た理解をもとに、さらに理解を深めていただけると幸いです。

# Tips

もしもあなたがKubernetesを本当に利用するのであれば、これから100万回 `kubectl`と打ち込むことになるでしょう。
以下のように、`kubectl`を`k`で呼び出せるようにしておくと、あなたの腱鞘炎を防ぐことができます。

```sh
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
alias k=kubectl
complete -F __start_kubectl k
```{{execute}}
