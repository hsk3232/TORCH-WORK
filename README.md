
# 파이토치 학습

# 설치 lib
!pip install tensorboard
!pip install torch torchvision torchaudio
!pip install torchsummary


# 🐍 Miniconda + PyTorch + NLP 환경 구축 가이드 (Windows 기준)

## 1. Miniconda 설치
- https://repo.anaconda.com/miniconda/
- Python 3.9 버전 권장: `Miniconda3-py39_23.3.1-0-Windows-x86_64.exe`

설치 시:
- "Add to PATH" 체크 ❌ (비권장)
- "Register Miniconda as default Python" 체크 ⭕ (권장)

---

## 2. Conda 가상환경 생성 및 활성화
- cmd 창에서 실행시 실행이 안된다면, Anaconda Prompt에서 실행

```bash

# 가상환경 생성
conda create -n torch-gpu python=3.9

# 가상환경 활성화
conda activate torch-gpu
```

---

## 3. PyTorch GPU 설치 (CUDA 11.8 기준)

```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

---

## 4. 필수 패키지 설치 (버전 고정)

- 기존에 설치된 lib와 충돌 난다면? 삭제후 설치
- pip uninstall -y numpy scipy gensim

```bash
pip install numpy==1.23.5
pip install scipy==1.9.3
pip install gensim==4.3.2
pip install nltk matplotlib pandas seaborn scikit-learn jupyter
pip install tensorboard
pip install torch torchvision torchaudio
pip install torchsummary
```

---

## 5. NLTK 리소스 다운로드 (처음 1회만)

```python
import nltk
nltk.download('punkt')
nltk.download('stopwords')
nltk.download('wordnet')
nltk.download('omw-1.4')
```

---

## 6. VS Code 연동

### 6-1. Python 확장 설치
- VS Code 좌측 `📦 Extensions` > `Python` 검색 > Microsoft 확장 설치

### 6-2. 가상환경 연동

```bash
pip install ipykernel
python -m ipykernel install --user --name torch-gpu --display-name "Python (torch-gpu)"
```

VS Code에서:
- 우측 상단 커널 선택 > `Python (torch-gpu)` 선택

---

## 7. 주피터 노트북 실행

```bash
cd C:\Users\user\Documents\NLP-Project
jupyter notebook
```

또는 VS Code에서 `.ipynb` 파일 열고 커널 연결

