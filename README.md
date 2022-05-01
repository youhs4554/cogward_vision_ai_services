## 스마병실(Cogward) vision AI 서비스 컨테이너

![cogward_demo_gif](media/cogward_demo.gif)

- 병실내 환자의 행동을 실시간 비디오 스트림으로 부터 받아서 인식된 5가지 행동상태를 실시간으로 분류하고 모니터링
- 5가지 행동 : Walking, Sleeping, Standing, Sitting, Falling

### 서비스 목록

- `camera-detection` : YOLO v3를 사용하는 simple server
- `camera-action` : 딥러닝 기반 행위 인식 서비스
  - TorchServe 사용 (Official repo: https://github.com/pytorch/serve)
    - gRPC inference port : 7070
    - Http inference port : 8080
    - server management port : 8081
      - model registration, resource control, etc
  - 사용된 모델 : Activation Guided Network (AGNet)
    - 보행 분석 v2.0에서 사용된 모델
    - human mask로 activation을 가이드
- `camera-monitor` : Kafka로부터 비디오 스트림을 받아서 camera-detection, camera-action 모듈에 API 요청하고 그 결과를 Kafka로 전달

### Usage

`ROOM_NUMBER` 변수로 특정 room에 설치된 카메라로부터 비디오 스트림을 가져올지 결정할 수 있다. 아래 명령어를 통해 AI 서비스를 실행/종료하거나 로그를 확인할 수 있다.

#### 서비스 실행

```
ROOM_NUMBER={ROOM_NUMBER} docker-compose up -d
```

#### 서비스 종료

```
ROOM_NUMBER={ROOM_NUMBER} docker-compose down
```

#### 로그 확인

```
ROOM_NUMBER={ROOM_NUMBER} docker-compose logs -f
```
