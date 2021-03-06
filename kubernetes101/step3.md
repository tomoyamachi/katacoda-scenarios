# `kubectl`でアプリケーションを動かす

kubectlでアプリケーションを動かしてみます。一番基本的なコマンドは、`kubectl get <Kubernetesオブジェクトの種類>`です。 Kubernetesオブジェクトの詳細は、[公式ドキュメント](https://kubernetes.io/ja/docs/concepts/overview/working-with-objects/kubernetes-objects/)でご確認ください。

Kubernetesには多くの種類のオブジェクトがありますが、今回はDeploymentのオブジェクトを作ります。以下のコマンドは、`k8s.gcr.io/echoserver`イメージを利用して、`echo-node`という名前のDeploymentを作成できます。

`kubectl create deployment echo-node --image=k8s.gcr.io/echoserver:1.10`{{execute}}

以下のコマンドで、Deploymentが正常に作成されたか確認できます。

`kubectl get deployment`{{execute}}

DeploymentはPodを作成します。Podとは、1つ以上のコンテナが入った最低限の実行単位です。まずはPodを確認してみましょう。1つのPodが作成されているはずです。

![](./assets/module_02_first_app.svg)

`kubectl get pods`{{execute}}

そして、Deploymentは複数のPodを管理することができます。以下のコマンドで、`echo-server`のDeploymentを編集し、つねに対象のPodが3個動作するようにします。

`kubectl scale --replicas=3 deployment/echo-node`{{execute}}

確認すると、Podが増えたのがわかります。

`kubectl get pods`{{execute}}

試しに`kubectl delete pods/<podの名前>`で1つのPodを削除しても、自動で3つのPodに回復することがわかります。

```sh
$ kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
echo-node-756866fc9-jrzfc   1/1     Running   0          2m58s
echo-node-756866fc9-mwdzf   1/1     Running   0          2m58s
echo-node-756866fc9-wgpft   1/1     Running   0          96s   ← このPodを削除する

$ kubectl delete pods/echo-node-756866fc9-wgpft
pod "echo-node-756866fc9-wgpft" deleted

$ kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
echo-node-756866fc9-46mw6   1/1     Running   0          44s   ← 削除したPodとは別のPodが作成されている
echo-node-756866fc9-jrzfc   1/1     Running   0          4m12s
echo-node-756866fc9-mwdzf   1/1     Running   0          4m12s
```

# 自己回復

ここまでみたように、Kubernetesは状態を定義すると、理想的な状態を保つように動作します。 このことをSelf-healing(自己回復)と呼びます。