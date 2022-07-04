# [instantNGP](https://github.com/NVlabs/instant-ngp)

## 環境
* OS : Ubuntu 18.04
* CUDA : 11.3
* cuDNN : 8

## DockerHub
> 2022/7/4現在

```
naglsa/ngp-11.3:latest
```

## TODO
* 現状GUIの起動ができない
    * GLXのバージョンが原因？調査中

## 備考
* バージョンに特に深い理由はない(issue参考に書いてたらこうなった)
* やっていることはPython/cmake/ceres-solver/COLMAP/instantNGPを順にビルドしてパス通してるだけ
* instantNGPのPythonライブラリを使いたい場合を想定
    * ```import pyngp as npg```で呼べる
    * ただし```python3 hoge.py```の必要がある
        * ```python hoge.py```ではない
    * scripts/run.pyの引数いろいろ指定したらレンダリング結果は保存可能
        ```
        python3 ./scripts/run.py --scene ./data/nerf/fox --mode nerf --load_snapshot ./results/nerf/fox/train_original_0629.msgpack --screenshot_transforms ./data/nerf/fox/transforms.json --screenshot_dir ./results/nerf/fox/test_images --width 2048 --height 2048 --n_steps 0
        ``` 
* build/testbedが試したい場合は
    * ```/opt/instant-ngp/build```に移動すれば試せる

