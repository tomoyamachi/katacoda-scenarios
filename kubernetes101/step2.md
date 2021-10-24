# cURLでKubernetes APIを呼び出す

さきほど実行した`kubectl config view`の項目で、現在操作しているKubernetes APIの情報がわかります。

`curl $(kubectl config view --minify | grep server | cut -f 2- -d ":" | tr -d " ")/api --insecure`{{execute}}

以下のようなエラーが出てきます。
これはKubernetes APIにアクセスするために必要な認証/認可情報がないためです。

```
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {
    
  },
  "status": "Failure",
  "message": "forbidden: User \"system:anonymous\" cannot get path \"/api\"",
  "reason": "Forbidden",
  "details": {
    
  },
  "code": 403
}
```

Kubernetes APIの認証/認可方法は複数あります。ここでは詳細を述べませんが、Kubernetesの深淵をすこしのぞくために、2つの方法でアクセスしてみましょう。

## クライアント証明書と秘密鍵を利用する

以下の方法では、Kubernetes APIに、クライアント証明書と秘密鍵を利用してアクセスします。

```sh
cat /etc/kubernetes/admin.conf | grep client-certificate-data | awk '{print $2}' | base64 -d > client.crt
cat /etc/kubernetes/admin.conf | grep client-key-data | awk '{print $2}' | base64 -d > client.key
curl --cert ./client.crt --key ./client.key $(kubectl config view --minify | grep server | cut -f 2- -d ":" | tr -d " ")/api --insecure
```{{execute}}

## Authorizationヘッダを利用する

```sh
APISERVER=$(kubectl config view --minify | grep server | cut -f 2- -d ":" | tr -d " ")
SECRET_NAME=$(kubectl get secrets | grep ^default | cut -f1 -d ' ')
TOKEN=$(kubectl describe secret $SECRET_NAME | grep -E '^token' | cut -f2 -d':' | tr -d " ")
curl $APISERVER/api --header "Authorization: Bearer $TOKEN" --insecure
```{{execute}}

# このステップのまとめ

KubernetesがAPI経由で動作していること、`kubectl`はKubernetes APIを動作させるためのツールであることが学べたと思います。

`Kubernetesは、大規模な複数のコンテナを、複数のサーバー上で動かすことが前提になってます。そのため、デフォルトでもある程度APIの保護はされていますが、攻撃されやすい部分であるため、理解した上で適切に保護することが重要です。`
