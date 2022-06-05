## Python 설치

```
//python 버전 확인
python --version
```
<img src="https://user-images.githubusercontent.com/60870438/172067184-1c439563-2d37-4c11-97f6-5f9915f0b49f.png" width=20%>

> 버전 변경하기
- 환경변수 편집
  - 시스템 변수 > Path > Python 위치 설정
  - 보통 위치는 사용자 > AppData > Local > Programs > Python > python39로 설정 되어 있다.

### 주피터 설치

```
pip install jupyter
```

- pip 오류가 날 경우, 환경변수 현재 사용자의 Path에 python39 > Scripts를 연결해줘야 한다.

<img src="https://user-images.githubusercontent.com/60870438/172067404-cfe3d7a1-270b-4520-a023-37efaa6b6c10.png" width=40%>

<img src="https://user-images.githubusercontent.com/60870438/172067373-b3ad34a0-edef-486c-ba78-bbbff150b0d6.png" width=60%>

그런데 이렇게 오류날 때가 있다.

```
-m pip install --upgrade pip
```
💖 완료!

## Anaconda 설치

- 앞서 적은 방법으로 해보려 했으나 anaconda를 사용해 본 적이 있어서 그냥 pytho 삭제하고 다시 했다.
- 설치시 아래 recommand만 체크 후 진행

보통 아나콘다의 위치는 사용자 아래에 있다.
사용자 > {내 이름} > IBG 생성
