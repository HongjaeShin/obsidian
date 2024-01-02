# base
### \_\_init__
- input args: num_query_instance, fixed_points, dir_interval, loss_pts, loss_dir 추가

### _init_layers
- self.init_query_bbox를 (900, 10) -> (900, 3)
- self.init_instance_query_enc를 추가 (900//fixed_points, embed_dims)
- self.init_query_bbox.weight의 z값을 0.425(대충 지면)으로 고정
- self.init_instance_query_enc.weight을 xavier_uniform으로 init

### forward
- 기존의 나중의 bbox와 구분을 위해 bbox_preds를 vectors_preds로 변경
- vectors_preds를 포함하는 bbox_preds를 self.transform_box로 계산하고 vectors_preds와 함께 outs에 추가
- **왜 cls_scores, bbox_preds, vectors_preds에 contiguous를 추가?**

### prepare_for_dn_input
: 빠른 수렴을 위한 DN-DETR의 방법(base에서 사용하진 않는데 나중을 보기 위해..)
- init_instance_query_enc를 이용하여 init_instance_query_feat을 num_query_instance(20)만큼 repeat
- init_query_feat에 indicator0(900,1)를 concat하고 init_instance_query_feat을 더해줌
- known_num은 배치당 object 수
- know_indice가 torch arange같은 느낌이라는데 안찍어보고 눈으로만 봐선 잘 모르겠음
	- **known_indice를 확인해보고 싶은데 돌리는게 있어서 불가능 추후 해보기..**
	- **GT가 달라서 안돌아감 나중에 DN 버전볼 때 다시 확인

### transform_box
- vector를 입력받아 vector를 포함하는 bbox를 cxcywh로 출력

### \_get_target_single
- MapTR의 target을 가져옴



# spamapnet_head_DN

### prepare_for_dn_input
``` known_indice = torch.nonzero(unmask_label + unmask_bbox)```
- 왜 이런식으로? : batch때문인가?
```known_indice = known_indice.unsqueeze(1).repeat(1, self.dn_group_num).view(-1)```
- self.dn_group_num = fixed_points(=20)
	- 원래는 하나의 쿼리에 하나의 gt지만 지금은 하나의 gt에 20개의 pts
```
rand_prob = torch.rand_like(known_bbox_expand) * 2 * self.dn_bbox_noise_scale
known_bbox_expand[..., 0:4] += rand_prob
```
- known_bbox에 noise를 추가