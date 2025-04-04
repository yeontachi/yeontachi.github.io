---
layout: single
title: "[딥러닝] Visual Studio Code 딥러닝 신경망 실습을 위한 개발환경 세팅(macOS)"
categories: AI
tag: [AI, DeepLearning]
toc: true
---

## 개발 환경 세팅(macOS)
 - **Python 설치(권장 버전: 3.10)**
    - [공식 홈페이지](https://www.python.org/downloads/)
    - 또는 macOS에서 Homebrew 사용

```
brew install python@3.10
```

 - **Conda 가상 환경 생성(추천)**
    
```
conda create -n tf_310 python=3.10
conda activate tf_310
```

 - **필요한 라이브러리 설치**

```
pip install tensorflow-macos tensorflow-metal matplotlib
```

 - **vsCode 확장 프로그램**
    - Python
    - Jupyter(노트북 파일용)
    - Pylance(IntelliSense 향상)


