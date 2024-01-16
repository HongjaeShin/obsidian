input image size : $1024\times1024$

### loading.py
#### LoadPanopticAnnotations
- img를 rgb로 불러와서 cocopanapi의 rgb2id 사용해서 panoptic gt 생성
- semantic segmentation gt의 shape이 [612, 612]. 따로 cls를 구분하지 않는 형태

### mask2former_head.py
#### Mask2FormerHead
- d
### msdeformattn_pixel_decoder.py
#### MSDoeformAttnPixelDecoder
- Input feat shape: $[[bs, 2048, 32, 32], [bs, 1024, 64, 64], [bs, 512, 128, 128], [bs, 256, 256, 256]]$
	- 위에 작성한 순서로 입력하는데 input_convs를 이용해서 channel을 feat_channel(256)으로 맞춰줌
- memory : multi scale img feat
- 