# ✅ AIX 시스템 점검 명령어 정리

AIX 시스템 점검 및 기본 정보 수집에 자주 사용하는 명령어를 목적별로 정리한 문서입니다.

---

## 🔧 시스템 정보

```sh
prtconf
```
- 시스템 전체 하드웨어 구성 요약 (CPU, 메모리, 장치 등)
- M/T, S/N, FW, IP 전체 구성 확인

```sh
oslevel -s
```
- AIX의 상세 OS 버전 정보 (TL, SP 수준까지 표시)

---

## 🧠 CPU / 메모리 정보

```sh
lsdev -Cc processor
```
- 시스템에 인식된 프로세서 목록 확인

```sh
lsdev -Cc memory
```
- 메모리 장치 상태 확인

```sh
lsattr -El mem0
```
- `mem0` 장치의 속성 정보 출력 (메모리 용량 등)

---

## 📡 어댑터 / 장치

```sh
lsdev -Cc adapter
```
- 네트워크 및 기타 어댑터 목록 확인

```sh
syssasraidmgr -Tl sissas0
```
- RAID 어댑터 상태 확인 (환경에 따라 명령어가 다를 수 있음)

---

## 💽 디스크 및 볼륨 그룹

```sh
lspv
```
- 시스템에 연결된 물리 디스크(PV) 목록 확인

```sh
lsvg
```
- 정의된 볼륨 그룹(VG) 목록 확인

```sh
lsvg -o | lsvg -il
```
- 활성화된 VG의 상세 LV 정보까지 확인 (sync 확인)

---

## 🚀 부팅 구성

```sh
bootlist -m normal -o
```
- 일반 부팅 시도의 부팅 순서 확인

---

## 📁 파일시스템 및 스왑

```sh
df -gt
```
- 전체 파일시스템 사용량 확인 (GB 단위)

```sh
lsps -as
```
- 스왑 구성 및 사용 현황 확인

---

## 🌐 네트워크 구성 및 상태

```sh
ifconfig -a
```
- 전체 네트워크 인터페이스 IP 및 설정 확인

```sh
entstat -d en" " | grep -i link
```
- NIC의 링크 상태 확인 (`en0` 등 실제 인터페이스 이름으로 교체 필요)

```sh
netstat -rn
```
- 라우팅 테이블 확인 → 이후 기본 게이트웨이에 ping 테스트

---

## 🧭 디스크 경로 상태 (MPIO 등)

```sh
lspath
```
- 디스크 다중 경로 상태 확인

---

## 📊 리소스 모니터링

```sh
topas
```
- 실시간 CPU/메모리/디스크/네트워크 사용률 모니터링

---

## 🧾 에러 로그

```sh
errpt
```
- 시스템 에러 로그 요약 출력

---

## 👥 로그인 이력

```sh
who /var/adm/wtmp
```
- 로그인 기록 (바이너리 포맷, `last` 명령 대체 가능)

```sh
who /etc/security/failedlogin
```
- 로그인 실패 기록

```sh
cat /etc/security/lastlog
```
- 사용자별 마지막 로그인 시간 확인

---
