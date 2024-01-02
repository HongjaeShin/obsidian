- waymo key
	- ['sample_idx', 'timestamp', 'ego2global', 'images', 'lidar_points', 'instances', 'cam_sync_instances', 'cam_instances']
	1. sample_idx
		- 1000000
	2. timestamp
		- 1522688014970187
	3. ego2global
		- 4 * 4 ndarray
	4. images
		- ['CAM_FRONT', 'CAM_FRONT_LEFT', 'CAM_FRONT_RIGHT', 'CAM_SIDE_LEFT', 'CAM_SIDE_RIGHT']
		- CAM_FRONT
			- ['img_path', 'height', 'width', 'cam2img', 'lidar2img', 'lidar2cam', 'timestamp']
			- img_path: 1000000.jpg
	5. lidar_points
		- ['num_pts_feats', 'lidar_path', 'timestamp', 'Tr_velo_to_cam']
		1. lidar_path: 1000000.bin
		2. Tr_velo_to_cam: 4 * 4 list
	6. instances : GT 정보인듯 다음의 키를 가진 list
		 - ['bbox', 'bbox_label', 'bbox_3d', 'bbox_label_3d', 'num_lidar_pts', 'difficulty', 'truncated', 'occluded', 'alpha', 'index', 'group_id', 'camera_id']
- nuscenes key
	- ['lidar_path', 'cam_front_path', 'cam_intrinsic', 'token', 'sweeps', 'ref_from_car', 'car_from_global', 'timestamp', 'cams', 'gt_boxes', 'gt_boxes_velocity', 'gt_names', 'gt_boxes_token', 'num_lidar_pts', 'num_radar_pts']