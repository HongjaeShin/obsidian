
before pre-pipeline
```
'img_fields','bbox3d_fields', 'pts_mask_fields', 'pts_seg_fields', 'bbox_fields', 'mask_fields', 'seg_fields', 'box_type_3d', 'box_mode_3d'
```
after pre-pipeline
```
dict_keys(['sample_idx', 'pts_filename', 'sweeps', 'ego_pose', 'ego_pose_inv', 'prev_idx', 'next_idx', 'scene_token', 'frame_idx', 'timestamp', 'img_timestamp', 'img_filename', 'lidar2img', 'intrinsics', 'extrinsics', 'prev_exists', 'ann_info', 'img_fields', 'bbox3d_fields', 'pts_mask_fields', 'pts_seg_fields', 'bbox_fields', 'mask_fields', 'seg_fields', 'box_type_3d', 'box_mode_3d'])
```
after pipeline
```
dict_keys(['img_metas', 'gt_bboxes_3d', 'gt_labels_3d', 'img', 'gt_bboxes', 'gt_labels', 'centers2d', 'depths', 'prev_exists', 'lidar2img', 'intrinsics', 'extrinsics', 'timestamp', 'img_timestamp', 'ego_pose', 'ego_pose_inv'])
```

MapTR 'nuscenes_dataset'
```
ann_info, sample_idx, pts_filename, sweeps, ego2global_translation, ego2global_rotation, prev_idx, next_idx, scene_token, can_bus, frame_idx, timestamp, img_filename, lidar2img, cam_intrinsic, lidar2cam
```

**'sample_idx'**, **'pts_filename'**, **'sweeps'**, **'ego_pose'**, **'ego_pose_inv'**, **'prev_idx'**, **'next_idx'**, **'scene_token'**, **'frame_idx'**, **'timestamp'**, **'img_timestamp'**, **'img_filename'**, **'lidar2img'**, **'intrinsics'**, **'extrinsics'**, 'prev_exists', **'ann_info'**, 'box_type_3d', 'box_mode_3d'

can_bus, ego2global_translation, ego2global_rotation, lidar2cam == extrinsics