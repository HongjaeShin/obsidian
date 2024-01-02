sparsebev_sampling.py

```
# reorganize the tensor to stack T and G to the batch dim for better parallelism

sample_points_cam = sample_points_cam.reshape(B, T, Q, G, P, 1, 3)

sample_points_cam = sample_points_cam.permute(0, 1, 3, 2, 4, 5, 6) # [B, T, G, Q, P, 1, 3]

sample_points_cam = sample_points_cam.reshape(B * T * G, Q, P, 3).contiguous()
# sample_points_cam이 contiguous하지 않아서 에러발생 -> 이를 추가
  

# reorganize the tensor to stack T and G to the batch dim for better parallelism

scale_weights = scale_weights.reshape(B, Q, G, T, P, -1)

scale_weights = scale_weights.permute(0, 2, 3, 1, 4, 5)

scale_weights = scale_weights.reshape(B * G * T, Q, P, -1).contiguous()
# 얘도 마찬가지
```

sparsebev_transformer.py
```
num_groups가 의미하는 값이 뭐지? 뭔가 채널을 나누는 느낌인데...
num_groups는 feature의 C를 g로 나눔. 이는 AdaMixer에서 제안된 방법인데 Sampling points를 g배수만큼 늘려 더 많은 sampling point를 만들기 위함인듯?
multi-head같은 느낌으로 생각하면 될 듯 함.
```
