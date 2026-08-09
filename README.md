<div align="center">

# Front Watermark

### 음성 워터마크 기반 딥페이크 검증 웹 플랫폼

**Streamlit 기반 미디어 업로드 플랫폼 -- 딥페이크 오디오 검출과 보이스 피싱 감지를 한 곳에서**

<br>

<img src="https://img.shields.io/badge/%EC%A0%9C70%ED%9A%8C%20%EC%A0%84%EA%B5%AD%EA%B3%BC%ED%95%99%EC%A0%84%EB%9E%8C%ED%9A%8C-%ED%8A%B9%EC%83%81-FFD700?style=for-the-badge" alt="특상"/>
<img src="https://img.shields.io/badge/%EC%82%B0%EC%97%85%ED%86%B5%EC%83%81%EC%9E%90%EC%9B%90%EB%B6%80%EC%9E%A5%EA%B4%80%EC%83%81-2024.11-0052CC?style=for-the-badge" alt="산업통상자원부장관상"/>

<br><br>

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=flat-square&logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-Web_App-FF4B4B?style=flat-square&logo=streamlit&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?style=flat-square&logo=scikit-learn&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT_API-412991?style=flat-square&logo=openai&logoColor=white)

</div>

---

## 배경

AI 음성 합성 기술의 급격한 발전으로 딥페이크 오디오를 이용한 사기, 여론 조작, 사칭 범죄가 급증하고 있습니다. 동시에 보이스 피싱 피해도 매년 증가하고 있어, 미디어의 진위를 판별하고 음성 사기를 감지하는 통합 플랫폼의 필요성이 커지고 있습니다.

**Front Watermark**는 사용자가 영상을 업로드하면 딥페이크 여부를 즉시 판별하고, 보이스 피싱 의심 문구까지 탐지하는 Streamlit 기반 웹 플랫폼입니다.

## 주요 기능

### 1. 딥페이크 오디오 검출

영상을 업로드하면 오디오 트랙을 자동 추출하고, MFCC 특징 기반 SVM 분류기로 딥페이크 여부를 판별합니다.

```
비디오 업로드 (.mp4/.mov/.avi/.mkv)
    |
    v
오디오 트랙 추출 (MoviePy / FFmpeg)
    |
    v
MFCC 특징 추출 (13-dimensional)
    |
    v
SVM 분류기 판별
    |
    v
결과 표시: "실제 미디어" / "딥페이크 미디어"
```

### 2. 보이스 피싱 감지

의심되는 문구를 입력하면 **TF-IDF 기반 ML 모델**과 **GPT API 기반 LLM 분석**을 앙상블하여 보이스 피싱 여부를 높은 정확도로 판별합니다.

```
텍스트 입력 (의심 문구)
    |
    +---> TF-IDF + ML 모델 (확률 기반)
    |
    +---> GPT Assistants API (의미 분석)
    |
    v
앙상블 판정 + 의심 문장/어휘 하이라이트
```

- **ML 모델**: KoNLPy 형태소 분석 후 TF-IDF 벡터화, 사전 학습된 분류 모델로 피싱 확률 산출
- **LLM 분석**: GPT Assistants API가 문맥을 분석하여 피싱 라벨과 근거를 반환
- **앙상블**: 두 결과가 불일치할 경우 문장 단위 세분화 분석으로 최종 판정

## 기술 스택

| 분류 | 기술 |
|------|------|
| 웹 프레임워크 | Streamlit (멀티페이지 앱) |
| 딥페이크 검출 | librosa (MFCC), scikit-learn (SVM) |
| 오디오 추출 | MoviePy, FFmpeg |
| 보이스 피싱 감지 | KoNLPy (형태소 분석), TF-IDF, scikit-learn |
| LLM 분석 | OpenAI GPT Assistants API |
| 시각화 | matplotlib, pandas |

## 프로젝트 구조

```
front-watermark/
  app.py                        # Streamlit 메인 앱 (비디오 업로드 + 딥페이크 판별)
  app_for_localmac.py           # macOS 로컬 실행용 (FFmpeg 기반 오디오 추출)
  requirements.txt              # Python 의존성 목록
  model/
    model.py                    # MFCC + SVM 딥페이크 검출 모듈
    svm_model.pkl               # 학습된 SVM 분류 모델
    scaler.pkl                  # 학습된 StandardScaler
  pages/
    1_voice_phising.py          # 보이스 피싱 감지 페이지 (TF-IDF + GPT 앙상블)
    utils/
      voicePhishingDetect.py    # 보이스 피싱 감지 클래스
      tfidf_vectorizer.pkl      # 학습된 TF-IDF 벡터라이저
      voice_fraud_detection_model_with_weights.pkl  # 피싱 감지 ML 모델
  test_files/                   # 테스트용 영상 파일
```

## 실행 방법

### 의존성 설치

```bash
pip install -r requirements.txt
```

추가로 librosa와 streamlit이 필요합니다:

```bash
pip install streamlit librosa
```

보이스 피싱 감지 기능을 사용하려면 OpenAI API 키 설정이 필요합니다:

```bash
export OPENAI_API_KEY="your-api-key"
```

### 앱 실행

```bash
# 기본 실행
streamlit run app.py

# macOS 로컬 실행 (FFmpeg 필요)
streamlit run app_for_localmac.py
```

브라우저에서 `http://localhost:8501`로 접속하면 비디오 업로드 및 딥페이크 판별 화면이 나타납니다. 사이드바에서 보이스 피싱 감지 페이지로 이동할 수 있습니다.

## 관련 프로젝트

| 리포지토리 | 설명 |
|-------------|------|
| [voice-watermark](https://github.com/hwan809/voice-watermark) | 딥페이크 오디오 검출 엔진 및 데스크톱 GUI |

## 수상

**제70회 전국과학전람회 특상 (산업통상자원부장관상)** -- 2024년 11월

---

<div align="center">
<sub>음성 워터마크 기반 딥페이크 검증 플랫폼 프로젝트의 일부입니다.</sub>
</div>
