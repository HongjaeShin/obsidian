input image size : $1024\times1024$

### loading.py
#### LoadPanopticAnnotations
- img를 rgb로 불러와서 cocopanapi의 rgb2id 사용해서 panoptic gt 생성
- semantic segmentation gt의 shape이 [612, 612]. 따로 cls를 구분하지 않는 형태

### mask2former_head.py
#### Mask2FormerHead
- ##### forward_head
	- Query LayerNorm하고 $[bs, query\_num,channel]$
	- 
### msdeformattn_pixel_decoder.py
#### MSDoeformAttnPixelDecoder
- Input feat shape: $[[bs, 2048, 32, 32], [bs, 1024, 64, 64], [bs, 512, 128, 128], [bs, 256, 256, 256]]$
	- 위에 작성한 순서로 입력하는데 input_convs를 이용해서 channel을 feat_channel(256)으로 맞춰줌
- memory : multi scale img feat
- multi scale img_feat에서 가장 low level feat$[bs, 256, 256, 256]$을 제외한 3개의 feat을 flatten, concat해서 MSDeformableSelfAttn돌림
- 마지막 feat은 conv 한번 하고 다음 level feat을 bilinear해서 더하고 conv
- mask_feature는 제일 큰 size고 multi_scale_feature는 size가 커지는 순으로 정렬