# Plate Servo Control 프로젝트

이 프로젝트는 번호판 인식을 기반으로 서보 모터를 제어합니다.

---

## 실행 전 준비사항

`plateServoControl` 파일을 실행하기 전에 아래의 단계를 반드시 수행해야 합니다:

1. **Docker 컨테이너 실행**
   먼저 Docker 컨테이너를 실행하여 환경을 준비해야 합니다. 아래 명령어를 사용합니다:
   
   sudo docker run -it --rm --ipc=host --runtime=nvidia --gpus all \
-e DISPLAY=$DISPLAY \
-v /tmp/.X11-unix:/tmp/.X11-unix \
--device /dev/video0:/dev/video0 \
--device /dev/ttyTHS1:/dev/ttyTHS1 \
-v /home/kim/ocryolo:/ocryolo \
-v /sys/class/gpio:/sys/class/gpio \
-v /proc:/proc \
--privileged \
ultralytics/ultralytics:latest-jetson-jetpack4

2. **필요 라이브러리 설치 Docker 컨테이너 안에서 필요한 라이브러리를 설치합니다**
   
apt-get update && apt-get install -y python3-tk nano
pip install pyserial
pip install easyocr==1.4
apt install python3-jetson-gpio
apt install -y busybox

EasyOCR 수정 EasyOCR의 utils.py를 아래와 같이 수정합니다:

경로: /usr/local/lib/python3.8/dist-packages/easyocr

수정 전:
img = cv2.resize(img, (int(model_height*ratio), model_height), interpolation=Image.ANTIALIAS)

수정 후:
img = cv2.resize(img, (int(model_height*ratio), model_height), interpolation=I
