### BEVFusion

```
def evaluate_map(self, results):

        thresholds = torch.tensor([0.35, 0.4, 0.45, 0.5, 0.55, 0.6, 0.65])

  

        num_classes = len(self.map_classes)

        num_thresholds = len(thresholds)

  

        tp = torch.zeros(num_classes, num_thresholds)

        fp = torch.zeros(num_classes, num_thresholds)

        fn = torch.zeros(num_classes, num_thresholds)

  

        for result in results:

            pred = result["masks_bev"]

            label = result["gt_masks_bev"]

  

            pred = pred.detach().reshape(num_classes, -1)

            label = label.detach().bool().reshape(num_classes, -1)

  

            pred = pred[:, :, None] >= thresholds

            label = label[:, :, None]

  

            tp += (pred & label).sum(dim=1)

            fp += (pred & ~label).sum(dim=1)

            fn += (~pred & label).sum(dim=1)

  

        ious = tp / (tp + fp + fn + 1e-7)

  

        metrics = {}

        for index, name in enumerate(self.map_classes):

            metrics[f"map/{name}/iou@max"] = ious[index].max().item()

            for threshold, iou in zip(thresholds, ious[index]):

                metrics[f"map/{name}/iou@{threshold.item():.2f}"] = iou.item()

        metrics["map/mean/iou@max"] = ious.max(dim=1).values.mean().item()

        return metrics
```
- 각 cls 별로 threshold를 0.35부터 0.65까지 0.05단위로 IoU를 계산하여 각 cls 별 최대값을 선택
> As different classes may have overlappings (e.g. car-parking area is also drivable), we evaluate the binary segmentation performance for each class separately and select the highest IoU across different thresholds [70].
- [70]에 해당하는 [**Cross-view Transformers for real-time Map-view Semantic Segmentation**](http://www.philkr.net/media/zhou2022crossview.pdf) 에서는 [0.4, 0.5]를 사용
- 