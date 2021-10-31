# 命令型コマンドと宣言型オブジェクト設定

Step 4で行った`kubectl create`と、Step5で行った`kubectl apply -f`ではアプローチが違います。
`kubectl create`を命令形アプローチ、`kubectl apply`を宣言型アプローチと呼びます。

命令形アプローチはオブジェクトに対して直接操作をし、リソースの作成/更新/削除を行います。覚えやすく、すぐに操作が完了する反面、複雑な構成には向いておらず、運用に向いていません。

宣言型アプローチは、既存のオブジェクトからの差分をもとに、作成/更新/削除の操作をkubectlが判断します。文法など覚えることが多い反面、複雑な構成にも耐え、複数人での運用にも向いている方法です。

Kubernetesでは、さらに具体的な分類として、[命令型コマンド、命令型オブジェクト設定、宣言型オブジェクト設定](https://kubernetes.io/ja/docs/concepts/overview/working-with-objects/object-management/)と分類しています。

宣言型アプローチによる自己回復(Step3参照)はKubernetesのもっとも重要な特徴です。 

# 最後に

ここで説明したことは、Kubernetesを動かす上で必要な知識のほんの一部です。
本番環境でセキュリティを保ちつつ、Kubernetesの恩恵を受けるためには努力が必要です。

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
