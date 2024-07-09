# IO数据集

## 数据获取途径

可以填写[这里](https://forms.gle/fDdyipTKDZaL34zC6)来加入等待列表，或者通过[io@io-ai.tech](mailto:io@io-ai.tech)联系我们。

公开的数据仅包含4个视角的 RGB 图像序列和 手末端EE位姿，请他数据请单独联系合作方式。

![waiting_list](assets/waiting_list_form.png)

## 数据集目录结构

每一组数据为一个tar压缩包，名称为XXX.tar，每个压缩包中有若干条轨迹，每条轨迹为一个文件夹，文件夹名称从XXX_000到XXX_00N，N为每组数据中的轨迹数量。

每个文件夹中包含以下文件：

```
XXX_[ddd]
├── annotation.json
├── audio
│   └── audio_{timestamp}.pcm
└── images/
│   ├── cam_fisheye
│   │   └── [dddddd]_{timestamp}.jpg
│   ├── cam_left
│   │   └── [dddddd]_{timestamp}.jpg
│   ├── cam_rgb
│   │   └── [dddddd]_{timestamp}.jpg
│   ├── cam_depth
│   │   └── [dddddd]_{timestamp}.png
│   └── cam_right
│       └── [dddddd]_{timestamp}.jpg
├── mocap/
│   └── *.csv
├── haptics
│   └── *.csv
├── calibration
│   └── *.yml
└── process_results
    └── ee_pose.csv

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

## 音频文件说明

`audio/audio_{timestamp}.pcm`文件包含音频数据，其中：

- `timestamp`：开始录制时的时间戳（单位：纳秒）
- `audio`：音频数据（16位单通道，采样率为16000Hz），6通道（前4通道数据为麦克风数据）

## 图像序列说明

images文件夹中包含五个子文件夹，分别为cam_fisheye、cam_left、cam_rgb、cam_depth、cam_right，分别对应相机视角。
（cam_depth为深度图像，由于体积较大，未发布至开放平台）

每个子文件夹中包含若干张图像，图像命名格式为`FRAMEID{:06d}_TIMESTAMP{:d}.ext`。FRAMEID范围为标注文件中的start_frame_id到end_frame_id，TIMESTAMP为图像采集时间戳，单位为纳秒，ext为图像格式，深度图像为16位单通道的PNG图像（无损压缩），其他图像为JPEG格式，例如`001505_1716792351875812408.jpg`。

> 对于某些视角，可能存在缺失帧，则该视角下该帧序号的图像文件不会在文件夹中出现。

## Mocap数据说明

`mocap/*.csv`文件包含mocap数据，其中：

- `seq`：记录序列号
- `timestamp`：时间戳（单位：纳秒）
- `q_w, q_x, q_y, q_z`：姿态（四元数）
- 其他数据说明待补充

> 部分数据由于设备原因存在丢帧，则该行为空

## 触觉数据说明

触觉传感器分为手部传感器和足底传感器，使用 CSV 格式存储。

传感器与文件名称的对应表格为：

|传感器| 文件名 |
| --- | --- |
|左手| touch-l-hand.csv |
|右手| touch-r-hand.csv |
|左脚| touch-l-foot.csv |
|右脚| touch-r-foot.csv |

手部有13个点位，足底有99个点位，手部数据的范围为[0, 65535]，足底数据的范围为[0, 4095]。数值越小，该点位受到的压力越大。CSV文件头分别如下:

|seq|timestamp|contact_0|contact_1|contact_2|contact_3|contact_4|……|
| --- | --- | --- | --- | --- | --- | --- | --- |


## 末端执行器位姿说明

`XXX_0NN_ee_pose.csv`文件（XXX为对应的轨迹文件夹名称）包含cam_rgb坐标系下左右末端执行器（左右手手背中心）位姿，其中：

- `seq`：记录序列号
- `timestamp`：时间戳（单位：纳秒）
- `left_ee_pos_x, left_ee_pos_y, left_ee_pos_z`：左末端执行器位置
- `left_ee_quat_w, left_ee_quat_x, left_ee_quat_y, left_ee_quat_z：`左末端执行器姿态（四元数）
- `right_ee_pos_x, right_ee_pos_y, right_ee_pos_z`：右末端执行器位置
- `right_ee_quat_w, right_ee_quat_x, right_ee_quat_y, right_ee_quat_z`：右末端执行器姿态（四元数）
