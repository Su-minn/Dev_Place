# 텐서플로우 튜토리얼, 딥러닝 시작하기



- 출처 : [퇴근후딴짓 Youtube](https://www.youtube.com/watch?v=t3EmjFsFs0s&list=PLSlDi2AkDv810N_uje_mCJuMthkU-oM1i&index=2)





## 코랩 Colab 작업환경 만들기



1) [구글 마켓플레이스](https://gsuite.google.com/marketplace/?hl=ko) 들어가기

2) 설치 후, 구글 드라이브로 이동

3) 새로만들기 - Google Colaboratory 선택 후, 생긴 파일을  
 'Tensorflow 2.0' 이라는 새 디렉토리를 하나 생성해서 안에 넣어주기

4) Colab 창으로 이동하여,  
런타임 - 런타임 유형 변경 - 하드웨어 가속기를 GPU로 변경  
노트북 CPU로 작업하면 학습시키는 데에 오래걸리므로, Colab의 장점은 GPU를 제공하는 것

- 참고 - GPU 사용 시간이 길면, 다음 날 연결이 안되는 경우도 있을 수 있으므로,  
  사용하지 않을 때는 다시 None으로 돌려놓는 것 추천





## 초보자를 위한 빠른 시작



- colab 환경에서 작업

```python
import tensorflow as tf
# tensorflow import하기

tf.__version__
# version 확인
# 2.2.0 이 출력됨
```



- 추후 학습 예정
- tensorflow 자격증 존재