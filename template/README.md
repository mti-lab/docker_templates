# Dockerfile Template

## TODOs
- [ ] .dockerignoreファイルを追加
- [ ] ABCIで動作確認

## 解説
* FROM
    * 公開リポジトリから適切なイメージを取得してベースとする
    * 基本的DockerfileはFROMから始まる
    * 我々はGPUを使うことが多いのでテンプレートではCUDAとcuDNNが最初から入っているUbuntuをベースにしているが，必要に応じて変えれば良い
* RUN
    * コマンドを走らせる
    * 以下の2種類の記法があり，テンプレートでは下のexec方式は利用していない
    * RUN \<command>
    * RUN ["exec file", "param1", "param2"]
        * JSONとして構文解析されるので " " を使用する
* ENV \<key>=\<value>
    * 環境変数keyに対して値valueを設定する
    * 実行コンテナ上でも保持される

> Q. なぜUbuntu20.04じゃないのか
> 
> A. 2022/6現在GPGキーの更新があったか何かでそのままだとBuildできないので
>
> 参考：[20.04使いたい人向け](https://zenn.dev/takakurasato/scraps/d3e0ee6132a5c5)

## 参考

* [Dickerfileのベストプラクティス（公式）](https://docs.docker.jp/engine/articles/dockerfile_best-practice.html)
    * ちゃんと読んでないのであんまり従ってないかも