# Docker

今週からぼちぼち埋めていきます（2022/6/16）

## TODOs
- [ ] Docker使ったことない人向け簡単な説明
- [x] Dockerfile分からない人向け記法説明
- [x] 汎用Dockerfileテンプレートアップ
- [ ] 各自が先行研究の追実験などで使用したDockerfileのアップ
  - [x] NeuralRecon (Yamashita)
  - [ ] Atlas (Yamashita)
  - [x] VoRTX (Yamashita)
  - [x] instantNGP (Yamashita)
  - [ ] NeRF (Kumagai)
  - [ ] Neus (Kumagai)
  - [ ] and more!

## 使用方法
* Dockerで普通に使う場合
  * 適当に調べてください
* ABCI上で使いたい場合(Singularity)の例
  1. ```docker build ~~~ && docker login && docker push 'user ID'/hoge-x.xx:latest```
  2. ```module load singualitypro && singularity pull hoge.img docker://'user ID'/hoge-x.xx:latest```
  3. ```singualrity run --nv hoge.img```

## よく使うリンク
* [DockerHub](https://hub.docker.com/)
  * Docker専用GitHubのようなもの
  * BuildしたDockerイメージをDockerHubに置くとURLを指定すればそのイメージをpull出来るようになる
  * ABCI上でSingularityイメージファイルを作成するときはDockerHub経由が楽
* [Nvidia/Cuda](https://hub.docker.com/r/nvidia/cuda/tags)
  * Tags → Filter Tagsから必要なCUDA/cuDNN/Ubuntuのバージョンに合ったものを探す

## 参考
* [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker)
  * DockerはデフォルトではBuild時にGPUが認識されない（例えばtorchsparseはCPU向けにビルドされる）
  * Nvidia GPUを使用したBuildを行う際に必要となる
  * 詳細は少し古いが[この記事](https://qiita.com/tkusumi/items/f275f0737fb5b261a868)を読めばある程度わかる
