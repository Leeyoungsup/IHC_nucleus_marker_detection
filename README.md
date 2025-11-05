# IHC Nucleus Marker TPS

담당자: 영섭 이
진행 상태: 완료
프로젝트: Breast ER/PR AllredScore (https://www.notion.so/Breast-ER-PR-AllredScore-27f42971c02f80e3ac05c40d376b329a?pvs=21)
git repositories: https://github.com/Leeyoungsup/IHC_nucleus_marker_detection

# DataSet

## 🔍 BCData 개요

- 논문 제목: *"BCData: A Large-Scale Dataset and Benchmark for Cell Detection and Counting"* [ACM Digital Library+2OUCI+2](https://dl.acm.org/doi/10.1007/978-3-030-59722-1_28?utm_source=chatgpt.com)
- 목적: 유방암 조직의 **Ki-67 IHC 염색된 이미지**에서 세포 검출 및 계수(cell detection/counting)를 위한 대규모 데이터셋 구축 및 벤치마크 제공 [ACM Digital Library+2ResearchGate+2](https://dl.acm.org/doi/10.1007/978-3-030-59722-1_28?utm_source=chatgpt.com)
- 역할: IHC 기반 핵 마커(특히 Ki-67) 이미지에서 핵 중심점(point) 레이블을 포함한 검출 작업을 지원하는 공개 벤치마크 데이터셋

---

## 📊 구성 및 통계

| 항목 | 상세 내용 |
| --- | --- |
| 이미지 개수 | 총**1,338장**의 Ki-67 IHC 염색 조직 이미지[ResearchGate+2ACM Digital Library+2](https://www.researchgate.net/publication/346084229_BCData_A_Large-Scale_Dataset_and_Benchmark_for_Cell_Detection_and_Counting?utm_source=chatgpt.com) |
| 레이블된 세포 수 | 이미지 전체에서 약**181,074개 세포(positive & negative tumor cells 포함)**라벨 포함[ResearchGate+2ACM Digital Library+2](https://www.researchgate.net/publication/346084229_BCData_A_Large-Scale_Dataset_and_Benchmark_for_Cell_Detection_and_Counting?utm_source=chatgpt.com) |
| 학습/검증/테스트 분할 | - 학습(Training): 803장[ResearchGate+1](https://www.researchgate.net/publication/346084229_BCData_A_Large-Scale_Dataset_and_Benchmark_for_Cell_Detection_and_Counting?utm_source=chatgpt.com)- 검증(Validation): 133장[ResearchGate+1](https://www.researchgate.net/publication/346084229_BCData_A_Large-Scale_Dataset_and_Benchmark_for_Cell_Detection_and_Counting?utm_source=chatgpt.com)- 테스트(Testing): 402장[ResearchGate+1](https://www.researchgate.net/publication/346084229_BCData_A_Large-Scale_Dataset_and_Benchmark_for_Cell_Detection_and_Counting?utm_source=chatgpt.com) |
| 이미지 해상도/크기 | 일반적으로**640 × 640 픽셀**크기의 타일 이미지로 제공됨[ACM Digital Library+2ResearchGate+2](https://dl.acm.org/doi/10.1007/978-3-030-59722-1_28?utm_source=chatgpt.com) |
| 레이블 형태 | 각 이미지 내에서**핵 중심점(centroid/point annotation)**으로 레이블링됨(각 세포의 중심 좌표)[ACM Digital Library+2ResearchGate+2](https://dl.acm.org/doi/10.1007/978-3-030-59722-1_28?utm_source=chatgpt.com) |

---

## ⚙️ 사용 목적 및 한계

### ✅ 장점/활용 가능성

- **세포 검출/계수(detection & counting)** 연구에 매우 유용한 벤치마크: 중심점(point) 레이블 기반 detection task에 적합함
- **Dense cell 환경**에서의 모델 학습 및 일반성 평가 가능
- 여러 연구들(딥러닝 기반 counting, density map 추정 등)에서 벤치마크로 활용됨 [ResearchGate+2ACM Digital Library+2](https://www.researchgate.net/publication/346084229_BCData_A_Large-Scale_Dataset_and_Benchmark_for_Cell_Detection_and_Counting?utm_source=chatgpt.com)
- 공개된 데이터셋 분할(training/validation/testing)을 기준으로 공유 → 모델 비교의 공정성 확보

### ⚠️ 한계/고려점

- **개별 핵의 경계(segmentation polygon) 레이블은 제공되지 않음**: 중심점(point) 레이블만 있음 → instance segmentation 수준 과제에는 추가 라벨링 필요
- Ki-67 마커 중심: ER/PR/기타 IHC 핵 마커까지 포함하지 않음
- 단일 마커(Ki-67) 및 단일 조직(유방암) 중심: 다른 조직, 다른 IHC 마커로 일반화하려면 도메인 갭 존재 가능
- 이미지는 타일(tile) 이미지이고, 전체 WSI(whole-slide image) 수준은 제공되지 않을 수 있음

---

## 🧩 관계된 확장 및 응용

- DeepLIIF 연구에서는 BCData(혹은 유사한 Ki-67 IHC 데이터) 기반 detection task에 적용하여 IHC 정량화 모델을 개발한 사례가 있음 [ResearchGate+1](https://www.researchgate.net/publication/346084229_BCData_A_Large-Scale_Dataset_and_Benchmark_for_Cell_Detection_and_Counting?utm_source=chatgpt.com)
- 다른 논문에서는 BCData를 사용해 multi-class 세포 카운팅 또는 weak supervision 방식 실험에 활용함 [ResearchGate](https://www.researchgate.net/publication/346084229_BCData_A_Large-Scale_Dataset_and_Benchmark_for_Cell_Detection_and_Counting?utm_source=chatgpt.com)
- BCData는 "IHC 중심점 검출" 벤치마크 데이터셋 중 대표적 사례로 자리잡고 있음 [ACM Digital Library+1](https://dl.acm.org/doi/10.1007/978-3-030-59722-1_28?utm_source=chatgpt.com)

# Model

- 검출 모델 YOLOv11 사용
- 커스텀을 위하여 모델구조와 loss 만 사용하여  전체 running 코드 구축
- 관련 링크
-[https://docs.ultralytics.com/ko/models/yolo11/](https://docs.ultralytics.com/ko/models/yolo11/)
-[https://github.com/Leeyoungsup/ihc_cell_count](https://github.com/Leeyoungsup/ihc_cell_count)

![image.png](image/image.png)

## Train result

- **mAP@0.5:0.95** : 0.4981
- **mAP@0.5** : 0.8558
- **Precision** : 0.8191
- **Recall** : 0.7775

![image.png](image/image%201.png)

![image.png](image/image%202.png)