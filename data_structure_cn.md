# IO数据集

## 数据获取途径

可以填写[这里](https://forms.gle/fDdyipTKDZaL34zC6)来加入等待列表，或者通过[io@io-ai.tech](mailto:io@io-ai.tech)联系我们。

![waiting_list](assets/waiting_list_form.png)

## 数据集目录结构

每一组数据为一个tar压缩包，名称为XXX.tar，每个压缩包中有若干条轨迹，每条轨迹为一个文件夹，文件夹名称从XXX_000到XXX_00N，N为每组数据中的轨迹数量。

每个文件夹中包含以下文件：

```
├── annotation.json
└── images
    ├── cam_fisheye
    ├── cam_left
    ├── cam_rgb
    └── cam_right
```

## 标注文件说明

标注文件为json格式，文件名称为annotation.json，文件内容如下：

```json
{
  "belong_to": "20240527_assemble_gripper_caid_06",
  "sequence_id": 4,
  "start_frame_id": 708,
  "end_frame_id": 888,
  "description": "Assemble the claw gripper"
}
```

## 图像序列说明

images文件夹中包含四个子文件夹，分别为cam_fisheye、cam_left、cam_rgb、cam_right，分别对应相机视角。

每个子文件夹中包含若干张图像，图像命名格式为`FRAMEID{:06d}_TIMESTAMP{:d}.ext`。FRAMEID范围为标注文件中的start_frame_id到end_frame_id，TIMESTAMP为图像采集时间戳，单位为纳秒，ext为图像格式，深度图像为16位单通道的PNG图像（无损压缩），其他图像为JPEG格式，例如`001505_1716792351875812408.jpg`。

> 对于某些视角，可能存在缺失帧，则该视角下该帧序号的图像文件不会在文件夹中出现。

## 末端执行器位姿说明

`XXX_0NN_ee_pose.csv`文件（XXX为对应的轨迹文件夹名称）包含cam_rgb坐标系下左右末端执行器（左右手手背中心）位姿，其中：

- `seq`：记录序列号
- `timestamp`：时间戳（单位：纳秒）
- `left_ee_pos_x, left_ee_pos_y, left_ee_pos_z`：左末端执行器位置
- `left_ee_quat_w, left_ee_quat_x, left_ee_quat_y, left_ee_quat_z：`左末端执行器姿态（四元数）
- `right_ee_pos_x, right_ee_pos_y, right_ee_pos_z`：右末端执行器位置
- `right_ee_quat_w, right_ee_quat_x, right_ee_quat_y, right_ee_quat_z`：右末端执行器姿态（四元数）
