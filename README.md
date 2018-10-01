# Cargo Dron을 활용한 QR코드 인식

드론을 이용해 공장처럼 넓은 실내에서 물류들의 종류 및 재고상태를 이동하지않고 손쉽게 확인할 수 있는 기술입니다. 드론에 옵티컬플로우를 장착하여 실내에서 드론이 안정적으로 비행할 수 있도록 구성하였고 OpenCV와 Zbar를 활용하여 드론이 비행 중 물류에 부착되어있는 QR코드를 인식하면 LED에 불이 들어오고 정보를 전송하여 파일을 만드는 형식입니다.

# 사전에 필수 설치가 필요한 오픈소스 및 구성

Raspberry-Pi 3 Model B+

Linux Ubuntu 16.04 LTS

https://github.com/dltpdn/opencv-for-rpi를 활용하여 Opnecv 설치

http://zbar.sourceforge.net를 활용하여 Zbar 설치

# 개발환경 구성

 1. 위의 오픈소스를 활용하였으며 우선 패키지를 업데이트합니다.

 $ sudo apt get update
 $ sudo apt get upgrade

2. 오픈소스를 다운받습니다.

 $ git clone https://github.com/dltpdn/opencv-for-rpi.git 

3. 다운로드 완료 되면 다운 받은 디렉토리 경로로 이동합니다.

 $ cd opencv-for-rpi
 $ cd stetch
 $ cd 3.3.0

4. 오픈소스를 통해 얻은 *.deb 파일들을 설치합니다.

 $ sudo apt install -y ./OpenCV*.deb

5. OpenCV 설치가 완료되고 다음으로 Zbar를 설치합니다.

 $ wget http://sourceforge.net/projects/zbar/files/zbar/0.10/zbar-0.10.tar.bz2/download -O zbar-0.10.tar.bz2
 $ bunzip2 zbar-0.10.tar.bz2
 $ sudo apt-get install libzbar0

6. pyzbar를 설치합니다

 $ pip install pyzbar
 
다음으로는 PX4 빌드 환경 구성입니다. 

1. 해당 사이트에서 바쉬스크립트를 이용하여 설치를 권장하기에 명령어로 실행합니다.

 https://raw.githubusercontent.com/PX4/Devguide/master/build_scripts/ubuntu_sim_common_deps.sh
     
2. 해당 스크립트를 복사하여 ubuntu_sim_common_dep.sh로 저장합니다. 그리고 다음을 사용하여 멤버로 지정합니다.

 $ sudo usermod -a -G dialout $USER

3. 로그아웃 후 다시 들어옵니다. 

 $ source ubuntu_sim_common_dep.sh

4. 시뮬레이션 프로그래밍을 사용하지 않으므로, 바로 빌드로 넘어갑니다. 

 $ cd 
 $ ..cd ~/src/Firmware
 $ make px4fmu-v3_default

5. PX4Flow 센서를 사용하기 위해선 PX4 사이트에서 제공하는 QGroundControl를 설치하여야 합니다. 
다음의 사이트에서 QGroundControl.AppImage를 다운받은 다음, 아래 명령을 터미널에 넣습니다.

 https://docs.qgroundcontrol.com/en/getting_started/download_and_install.html
 $ chmod +x ./QGroundControl.AppImage
 $ ./QGroundControl.AppImage

# 파이썬 파일 사용
 
 위의 과정이 모두 완료되면 업로드 된 파이썬 파일을 이용하여 진행합니다. 
 
 모든 파일은 OpenCV와 Zbar를 함께 이용할 수 있도록 프로그래밍 하였으며 이미지 및 Video Stream에서 QR코드 인식이 가능합니다.
 
▶ 이미지 파일에 대한 QR코드의 인식을 원하는 경우
 
 barcode_scanner_image.py(qrcode.png 파일을 활용하여 인식여부를 확인하면 됩니다.)

▶ Video Stream에서 QR코드의 인식을 원하는 경우

 barcode_scaonner_video.py

▶ Video Stream에서 QR코드의 인식을 원하는 경우 + LED(2개)를 활용하여 QR코드 인식 여부 확인

 barcode_scaonner_video1.py

▶ Video Stream에서 QR코드의 인식을 원하는 경우 + LED(4개)를 활용하여 QR코드 인식 여부 확인

 barcode_scaonner_video2.py

※ LED의 회로 연결상태에 따라 점등 여부가 달라집니다. LED(2개)의 경우에는 6(GND), 37(GPIO20), 40(GPIO21)를 활용하였고

LED(4개)의 경우에는 6(GND), 32(GPIO12), 36(GPIO16), 37(GPIO20), 40(GPIO21)로 연결하였습니다.
