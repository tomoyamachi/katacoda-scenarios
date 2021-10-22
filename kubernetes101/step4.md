# 最後に
ここで説明したことは、Kubernetesを動かす上で必要な知識のほんの一部です。
本番環境でKubernetesを動かすには、膨大な知識が必要になるでしょう。

幸いなことに[Kubernetesの公式ドキュメント](https://kubernetes.io/ja/docs/home/)は日本語版も充実しており、多くの書籍や動画があります。ここで得た理解をもとに、さらに理解を深めていただけると幸いです。

# Tips

もしもあなたがKubernetesを本当に利用するのであれば、これから100万回 `kubectl`と打ち込むことになるでしょう。
以下のように、`kubectl`を`k`で呼び出せるようにしておくと、あなたの腱鞘炎を防ぐことができます。

```
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
alias k=kubectl
complete -F __start_kubectl k
```