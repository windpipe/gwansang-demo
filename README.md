# 관상 분석 데모 (gwansang-demo)

카메라나 사진으로 얼굴 특징 19가지(얼굴형, 눈썹, 눈, 코, 입, 팔자주름 등)를 분류하는 실험용 웹 데모입니다.

**바로 열기:** https://windpipe.github.io/gwansang-demo/

- 모든 처리는 브라우저 안에서만 합니다. 카메라 영상과 사진은 어디에도 전송되지 않습니다.
- PC Chrome·Edge 최신 버전 권장 (WebGPU가 있으면 GPU로, 없으면 CPU(WASM)로 실행).
- 정면을 보고, 밝은 곳에서, 앞머리로 이마를 가리지 않고, 안경을 벗으면 결과가 좋아집니다.

## 동작 방식

1. MediaPipe Face Landmarker로 얼굴을 찾고 촬영 조건(얼굴 1개, 기울기, 정면, 크기)을 실시간 확인
2. 촬영 시 MediaPipe 사람 분할로 배경을 흰색으로 바꾸고, 두 눈 위치를 학습 사진의 평균 구도에 맞춰 정렬
3. EfficientNet-B0 분류기(ONNX, 16MB, onnxruntime-web)로 19개 항목 분류
4. 입 크기·입술 두께는 얼굴 비율 측정값으로 판정

## 성능과 한계

- 학습 7,871명, 처음 보는 400명 시험에서 항목별 균형정확도는 34%(얼굴형)~86%(눈썹 짙음). 무작위로 찍는 수준보다 평균 약 23%p 높음.
- 학습 데이터가 배경이 제거된 정면 사진이고 20~30대 남성 위주라, 실제 카메라·다른 연령·여성에서는 정확도가 더 낮을 수 있습니다.
- 관상 해석이 아니라 얼굴 특징 분류까지만 합니다. 결과는 참고용입니다.

## 출처

- 분류기 학습 데이터: 한국지능정보사회진흥원(NIA) AI Hub [「페르소나 기반의 가상 인물 몽타주 데이터」](https://www.aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=data&dataSetSn=618). 본 모델은 과학기술정보통신부·한국지능정보사회진흥원 사업 결과물인 해당 데이터를 활용한 2차적 저작물입니다. 데이터 자체는 이 저장소에 포함하지 않습니다.
- 얼굴 랜드마크·배경 분리: [Google MediaPipe](https://ai.google.dev/edge/mediapipe) (모델은 실행 시 Google 서버에서 받음)
- 분류기 백본: EfficientNet-B0 (timm), 실행: [ONNX Runtime Web](https://onnxruntime.ai/)
