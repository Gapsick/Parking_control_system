# Plate Servo Control 프로젝트

이 프로젝트는 **차량 번호판 인식**을 기반으로 서보 모터를 제어하여 차단기를 작동시키는 시스템입니다.  
Jetson Nano를 활용해 실시간 번호판 탐지 및 인식을 수행하고, GPIO를 통해 차단기를 열고 닫는 작업을 구현했습니다.  
이 프로젝트는 주차장, 아파트, 기업 출입구 등에서 자동화된 차량 출입 통제 시스템을 구축하는 데 활용될 수 있습니다.

---

## 주요 기능
1. **실시간 번호판 탐지 및 인식**:
   - YOLOv8을 사용해 차량 번호판을 탐지하고, EasyOCR로 번호판의 텍스트(숫자)를 인식합니다.
2. **UART 통신**:
   - 인식된 번호판 데이터를 Jetson Orin으로 UART를 통해 전송합니다.
3. **GPIO 제어 및 서보 모터 작동**:
   - 번호판 인식 결과에 따라 서보 모터를 제어하여 차단기를 열고 닫습니다.

---

## 실행 전 준비사항

### 1. Docker 컨테이너 실행
Docker 컨테이너를 실행하여 필요한 환경을 설정합니다. 아래 명령어를 사용하세요:
```bash
sudo docker run -it --rm --ipc=host --runtime=nvidia --gpus all \
-e DISPLAY=$DISPLAY \
-v /tmp/.X11-unix:/tmp/.X11-unix \
--device /dev/video0:/dev/video0 \
--device /dev/ttyTHS1:/dev/ttyTHS1 \
-v /home/kim/ocryolo:/ocryolo \
--privileged ultralytics/ultralytics:latest-jetson-jetpack4

### 2. 필요 라이브러리 설치
Docker 컨테이너 안에서 아래 명령어를 실행하여 필요한 라이브러리를 설치하세요:
apt-get update && apt-get install -y python3-tk nano pip python3-pip
pip install pyserial easyocr==1.4
apt-get install -y busybox

### EasyOCR 수정
EasyOCR의 utils.py 파일을 수정해야 합니다.
수정 경로: /usr/local/lib/python3.8/dist-packages/easyocr/utils.py
# 기존 코드:
img = cv2.resize(img, (int(model_height*ratio), model_height), interpolation=Image.ANTIALIAS)

# 수정 코드:
img = cv2.resize(img, (int(model_height*ratio), model_height), interpolation=1)
