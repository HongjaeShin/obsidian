input image size : $1024\times1024$

### LoadPanopticAnnotations
- img를 rgb로 불러와서 cocopanapi의 rgb2id 사용해서 panoptic gt 생성
- semantic segmentation gt의 shape이 [612, 612]. 따로 cls를 구분하지 않는 형태

### Mask2FormerHead
- d

### MSDoeformAttnPixelDecoder
- 