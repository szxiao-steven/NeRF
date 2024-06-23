本项目实现了基于NeRF的物体重建和新视图合成，采用了[instant-ngp](https://github.com/NVlabs/instant-ngp)框架进行训练，完成了骷髅手办的三维重建。


### 安装依赖
`pip install -r requirements.txt`

### 编译
```sh
cmake . -B build -DCMAKE_BUILD_TYPE=RelWithDebInfo
cmake --build build --config RelWithDebInfo -j
```

### 数据准备
将骷髅手办的拍摄视频 skeleton.mp4 （下载地址详见实验报告）放入自定义的目录 `<filename of video>`。执行以下命令抽取关键帧图片：
```python
python scripts/colmap2nerf.py --video_in <filename of video> --video_fps 1.3 --run_colmap --aabb_scale 8
```

将抽取出的图像放在名为images的文件夹中，移动至`data\skeleton\images`。

### 模型训练
命令行训练：
```sh
python scripts/run.py --scene data/nerf/skeleton --save_snapshot data/nerf/skeleton/base.ingp --n_steps 100000
```

或者通过开启GUI界面进行训练：
```sh
./instant-ngp data/nerf/skeleton
```

### 模型测试
```sh
python scripts/run.py --scene data/nerf/skeleton --load_snapshot data/nerf/skeleton/base.ingp --test_transforms data/nerf/skeleton/transforms.json
```

### 渲染重建视频
```sh
python scripts/rendervideo.py --scene data/nerf/skeleton --n_seconds 10 --fps 30 --render_name skeleton_render --width 1920 --height 1080
```
