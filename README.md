# Docker Template

先行研究などの環境構築に使用したDockerfileとそれに関するメモなどを保管する場所
> 後輩などが同じ先行研究をとりあえず動かしたい時の環境構築が楽になるように
> 
> このリポジトリの作成者はABCI上でSingularityで環境構築をする時にDockerfileを使用しています
>
> 具体的な手順は以下
> 
> ① DockerfileをbuildしてDockerHubにpush
> 
> ②ABCI上でDockerHubからイメージをpullしてきてsingularityイメージに変換 


## TODOs
- [ ] Docker使ったことない人向け簡単な説明
- [x] 汎用Dockerfileテンプレートアップ
  - [ ] 汎用Dockerfileテンプレートのアップデート


## 使用方法
* DockerHubのアカウントは作成済みと仮定
  * 各種設定
    * ユーザ名 : 'user ID'
    * イメージ名 : hoge-x.xx
    * タグ : latest
  * 注1. 自分はイメージ名の末尾にCUDAのバージョンをつけています
  * 注2. タグは好きなものをつけてください（CUDAバージョンはこっちに書くべきかもしれない）
* ABCI上で使いたい場合(Singularity)の例
  * dockerイメージのbuildとpush
    1. ```cd 'dockerfile-dir-path'```
    2. ```docker build -m 8g -t hoge-x.xx:latest . && docker login && docker push 'user ID'/hoge-x.xx:latest```
  * ABCI上でのSingularityイメージの作成
    1. ```cd 'singularity-img-dir-path' ```
    2. ```module load singualitypro && singularity pull hoge.img docker://'user ID'/hoge-x.xx:latest```
  * Singularityイメージ環境内での作業
    * インタラクティブに使用する場合
      * ```singularity run --nv hoge.img```
      * singularity環境下に入るので自由に操作してください
        * 注1. ABCIのgroup領域のデータは```--bind```でマウントしないと参照できません(共有領域のデータセットを使うときなど)
        * 注2. ```--nv```はGPU使わない時は外してください
    * コマンドを投げる場合
      * ```singularity exec --nv hoge.img python run.py```
      * 例はsigularity環境下で```python run.py```を走らせる場合です
        * 注. ```python run.py```は必要な命令に書き換えてください
      * バッチジョブを投げる時はこちらの方が書きやすいのはと思います
* Dockerイメージとして使う場合
  * WIP

## よく使うリンク
* [DockerHub](https://hub.docker.com/)
  * Docker専用GitHubのようなもの
  * BuildしたDockerイメージをDockerHubに置くとURLを指定すればそのイメージをpull出来るようになります
  * ABCI上でSingularityイメージファイルを作成するときはDockerHub経由が学習コストが低くて楽なのではないかと思っています
* [Nvidia/Cuda](https://hub.docker.com/r/nvidia/cuda/tags)
  * Tags → Filter Tagsから必要なCUDA/cuDNN/Ubuntuのバージョンに合ったものを探す
  * 基本的にDockerfileの1行目でよく使われている環境を```FROM```で引っ張ってくるのが良いです
    * 公式環境をベースにするのが推奨らしい(?)

## 参考
* [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker)
  * DockerはデフォルトではBuild時にGPUが認識されません（例えばtorchsparseはCPU向けにビルドされます）
  * Nvidia GPUを使用したBuildを行う際に必要となります(ABCI側には不要)
  * 詳細は少し古いが[この記事](https://qiita.com/tkusumi/items/f275f0737fb5b261a868)を読めばある程度わかります
