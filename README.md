
# 💻 Linux Average Load Test

## 📝 개요
> Linux에서 **Average Load**를 분석하는 부하 테스트는 시스템 성능 최적화와 안정성 평가를 위한 필수 과정입니다. **stress**, **sysstat** 등을 사용해 **CPU**와 **I/O** 부하를 시뮬레이션하고, **uptime**, **mpstat**, **pidstat**으로 성능을 모니터링합니다. 이를 통해 성능 저하나 병목 현상을 사전에 파악하고, 서버 확장성과 안정성을 강화하여 서비스 품질을 유지하고 비용 효율적인 인프라 운영을 지원합니다. 또한 이 테스트를 통해 리눅스 시스템의 평균 부하에 대한 이해를 높이고, 시스템 성능 분석의 기초를 다질 수 있습니다.


## :raising_hand: 팀원

| <img src="https://github.com/yuwankang.png" width="80"> | <img src="https://github.com/cshharry.png" width="80"> |
|:---:|:---:|
| [강유완](https://github.com/yuwankang) | [조성현](https://github.com/cshharry) |

## 🔧 요구 사항

- 🐧 **Ubuntu 18.04 이상**
- 💾 **2개의 CPU, 8GB RAM** (이하 실습 환경과 유사한 시스템 권장)
- 🔧 `stress` 및 `sysstat` 패키지

### 📦 패키지 설치

```bash
sudo apt update
sudo apt install stress sysstat
```

![image](https://velog.velcdn.com/images/yuwankang/post/b1afdaff-0d44-4831-bffe-3a910aad0276/image.png)
![image](https://velog.velcdn.com/images/yuwankang/post/8b151759-e90c-4c1e-b1d7-c5042c1552e1/image.png)

## 🛠️ 테스트 단계

### 1️⃣ 기본 시스템 상태 확인

먼저 현재 시스템의 평균 부하를 확인합니다. 이 명령어는 시스템 가동 시간, 로그인된 사용자 수, 그리고 1분, 5분, 15분 간의 평균 부하를 출력합니다.

```bash
uptime
```
![image](https://github.com/user-attachments/assets/8d72e3fe-a1ff-4d57-834f-388a69ffe2d7)

---

### 2️⃣ CPU 집약적인 프로세스로 평균 부하 증가시키기

#### 🚀 CPU 100% 부하 시뮬레이션

CPU 하나를 100%로 사용하여 부하를 걸어봅니다. 600초 동안 CPU에 부하를 줍니다.

```bash
stress --cpu 1 --timeout 600
```
![image](https://velog.velcdn.com/images/yuwankang/post/762fcdda-f00a-4448-9953-0539c591daa1/image.png)

#### 👁️ 실시간으로 평균 부하 모니터링

`watch` 명령어로 실시간으로 시스템의 평균 부하 변화를 확인합니다.

```bash
watch -d uptime
```
![image](https://velog.velcdn.com/images/yuwankang/post/7a501d26-0013-41bc-b6a4-b831de957217/image.png)

#### 📊 CPU 사용률 확인

`mpstat` 명령어를 사용해 CPU 사용률을 모니터링합니다. 모든 CPU의 사용률을 5초 간격으로 출력합니다.

```bash
mpstat -P ALL 5
```
![image](https://velog.velcdn.com/images/yuwankang/post/e4c15248-b691-482e-9921-df46a2b72d1c/image.png)

#### 🔍 CPU를 많이 사용하는 프로세스 확인

`pidstat` 명령어로 CPU 사용량이 많은 프로세스를 확인합니다. 5초 간격으로 프로세스 정보를 출력합니다.

```bash
pidstat -u 5 1
```
![image](https://velog.velcdn.com/images/yuwankang/post/84fced42-5ed0-47df-bf6f-9987d3b1ba80/image.png)

**결과**:
- `stress`라는 프로세스가 CPU 사용률 100%를 차지하는 것을 확인할 수 있습니다.
- 시간이 지나면서 평균 부하가 1.00으로 증가하며, 이는 시스템의 CPU가 완전히 사용 중임을 나타냅니다.

---

### 3️⃣ I/O 집약적인 프로세스로 평균 부하 증가시키기

#### 🔄 I/O 부하 시뮬레이션

I/O 작업에 부하를 주는 명령어를 실행하여 600초 동안 I/O 부하를 시뮬레이션합니다.

```bash
stress -i 1 --timeout 600
```
![image](https://velog.velcdn.com/images/yuwankang/post/250cd2ae-665c-4cf4-89b7-37e1c374886f/image.png)

#### 👁️ 실시간으로 평균 부하 모니터링

다시 `watch` 명령어를 사용해 평균 부하 변화를 실시간으로 모니터링합니다.

```bash
watch -d uptime
```
![image](https://velog.velcdn.com/images/yuwankang/post/fb72bf88-610f-424b-9c8b-963a5a316c06/image.png)

#### 📊 CPU 사용률 및 I/O 대기 상태 확인

`mpstat` 명령어를 사용하여 CPU 사용률 및 I/O 대기 상태를 모니터링합니다.

```bash
mpstat -P ALL 5
```
![image](https://velog.velcdn.com/images/yuwankang/post/ad59d2d7-dc73-405f-bfda-b554e5289118/image.png)

#### 🔍 I/O 대기 중인 프로세스 확인

`pidstat` 명령어로 I/O 대기 상태에 있는 프로세스를 확인합니다.

```bash
pidstat -u 5 1
```
![image](https://velog.velcdn.com/images/yuwankang/post/8f577bfe-f33e-46f7-b23e-6286a1d71fa6/image.png)

**결과**:
- CPU 사용률은 높지 않지만, I/O 대기 시간(`iowait`)이 크게 증가하는 것을 확인할 수 있습니다.
- 이는 디스크 작업으로 인해 시스템 부하가 증가한 것입니다.

---

### 4️⃣ 많은 프로세스를 실행하여 CPU 오버로드 발생시키기

#### 🚨 CPU 오버로드 시뮬레이션

이번에는 CPU 수보다 많은 8개의 프로세스를 실행하여 시스템을 과부하 상태로 만듭니다.

```bash
stress -c 8 --timeout 600
```
![image](https://velog.velcdn.com/images/yuwankang/post/d8c89b07-815e-4b39-9b56-bcc9cd9b28d9/image.png)

#### ⏳ 평균 부하 확인

`uptime` 명령어를 사용하여 평균 부하가 어떻게 증가하는지 확인합니다.

```bash
uptime
```
![image](https://velog.velcdn.com/images/yuwankang/post/98af7f5f-f42a-40a2-887f-9dd9c070a5c9/image.png)

#### 🔍 프로세스 상태 확인

`mpstat` 명령어를 사용하여 CPU 사용률 및 I/O 대기 상태를 모니터링합니다.

```bash
mpstat -P ALL 5
```
![image](https://velog.velcdn.com/images/yuwankang/post/06bdb467-5b50-4dfe-832b-bfa37a434ed0/image.png)

`pidstat` 명령어로 CPU를 대기 중인 프로세스를 확인합니다.

```bash
pidstat -u 5 1
```
![image](https://velog.velcdn.com/images/yuwankang/post/587d1326-a7e2-424d-85f5-084e8b6aa8f5/image.png)

**결과**:
- 8개의 프로세스가 2개의 CPU를 공유하면서 CPU 대기 시간(`%wait`)이 크게 증가합니다.
- 평균 부하가 7점대로 증가하며, 시스템이 심각하게 과부하된 상태임을 알 수 있습니다.

---

## 📌 결론
- 평균 부하는 시스템의 전체적인 성능 상태를 빠르게 파악하는 데 유용합니다.
- 하지만 평균 부하만으로는 정확한 병목 지점을 파악하기 어려우므로, `mpstat`, `pidstat` 등의 추가적인 도구를 활용하여 상세 분석이 필요합니다.
- 평균 부하가 높다고 해서 반드시 CPU 사용률이 높은 것은 아니며, I/O 활동 증가 등 다른 요인도 고려해야 합니다.
