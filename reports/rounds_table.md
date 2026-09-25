# Bảng so sánh các vòng

Tập kiểm thử: 20 ảnh, 403 box tham chiếu (bỏ qua 14 box cao dưới 16 px). Ngưỡng IoU 0.5; P, R, F1 tính tại conf 0.25.

| vòng | model | ảnh train | box train | AP50 | Δ AP50 so cold start | P@0.25 | R@0.25 | F1 | R small | R medium | R large |
| ---: | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| 0 | yolov8n cold start (COCO car+bus+truck) | 0 | 0 | 0.771 | — | 0.925 | 0.489 | 0.640 | 0.182 | 0.547 | 0.561 |
| 1 | active learning round 1 | 12 | 169 | 0.815 | +0.044 | 0.930 | 0.510 | 0.650 | 0.250 | 0.600 | 0.650 |
