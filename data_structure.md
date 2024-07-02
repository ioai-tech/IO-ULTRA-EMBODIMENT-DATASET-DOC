# IO Dataset

## Data Acquisition

You can fill in [here](https://forms.gle/fDdyipTKDZaL34zC6) to join the waiting list, or contact us through [io@io-ai.tech](mailto:io@io-ai.tech).

![waiting_list](assets/waiting_list_form.png)

## Dataset Structure

Each group of data is a tar compressed package, named XXX.tar, and each compressed package contains several trajectories, each track is a folder, and the folder name is from XXX_000 to XXX_00N, where N is the number of trajectories in each group of data.

Each folder contains the following files:

```
├── annotation.json
└── images
    ├── cam_fisheye
    ├── cam_left
    ├── cam_rgb
    └── cam_right
```

> More data types will be gradually released.

## Annotation File Description

The annotation file is in json format, and the file name is annotation.json, and the file content is as follows:

```json
{
  "belong_to": "20240527_assemble_gripper_caid_06",
  "sequence_id": 4,
  "start_frame_id": 708,
  "end_frame_id": 888,
  "description": "Assemble the claw gripper"
}

```

The "images" folder contains four subfolders, namely "cam_fisheye," "cam_left," "cam_rgb," and "cam_right," corresponding to different camera perspectives.

Each subfolder contains several images, and the image naming format is FRAMEID{:06d}_TIMESTAMP{:d}.ext. The FRAMEID ranges from the start_frame_id to the end_frame_id specified in the annotation file. TIMESTAMP represents the timestamp of the image acquisition, measured in nanoseconds, and ext denotes the image format. Depth images are 16-bit single-channel PNG images (lossless compression), while other images are in JPEG format. For example, 001505_1716792351875812408.jpg.

For certain perspectives, there may be missing frames, which means that the image file for that frame number will not be present in the folder.

## End Effector Pose Description

The `XXX_0NN_ee_pose.csv` file (where XXX is the corresponding trajectory folder name) contains the poses of the left and right end effectors (center of the back of the left and right hands) in the cam_rgb coordinate system. The columns are as follows:

- `seq`: Record sequence number
- `timestamp`: Timestamp (nanoseconds)
- `left_ee_pos_x, left_ee_pos_y, left_ee_pos_z`: Position of the left end effector
- `left_ee_quat_w, left_ee_quat_x, left_ee_quat_y, left_ee_quat_z`: Orientation of the left - end effector (quaternion)
- `right_ee_pos_x, right_ee_pos_y, right_ee_pos_z`: Position of the right end effector
- `right_ee_quat_w, right_ee_quat_x, right_ee_quat_y, right_ee_quat_z`: Orientation of the - right end effector (quaternion)
