# 📘 SAMBA 서버 구축 보고서

**🗓 작업 일자:** 2025년 4월 23일

**🧑‍💻 작업자:** (Mr.LEE)  
**🖥 대상 시스템:** CentOS 7.1 (오프라인, ISO 기반 설치 환경)

---

## ✅ 1. 시스템 구성 배경

- 내부 테스트 환경에서 Windows ↔ Linux 간 파일 공유 구현
- 인터넷 연결이 불가능한 환경에서 ISO 기반으로 yum 패키지 설치 진행
- SAMBA 서버 구축 후 Windows에서 공유 폴더 접근 가능하게 구성

---

## ✅ 2. 작업 목표

- CentOS 7.1에 SAMBA 서버 설치
- 계정 기반 인증 방식 설정
- Windows 클라이언트에서 공유 폴더 접근 테스트 완료

---

## ✅ 3. 상세 작업 내역

### 📦 3.1 ISO 기반 yum Repository 구성

```bash
mkdir -p /mnt/centos
mount -o loop /root/CentOS-7.1-x86_64-DVD-1503-01.iso /mnt/centos
```

#### `/etc/yum.repos.d/CentOS-Local.repo` 생성

```ini
[centos-local]
name=CentOS 7.1 Local ISO
baseurl=file:///mnt/centos
gpgcheck=1
enabled=1
gpgkey=file:///mnt/centos/RPM-GPG-KEY-CentOS-7
```

#### 기존 레포 비활성화 (예: `CentOS-Base.repo`, `CentOS-Vault.repo`)

```ini
enabled=0
```

---

### ⚙️ 3.2 SAMBA 및 클라이언트 설치

```bash
yum install samba samba-client -y
```

---

### 📂 3.3 공유 디렉토리 생성

```bash
mkdir -p /srv/samba/share
chmod -R 777 /srv/samba/share
```

---

### 🧾 3.4 SAMBA 설정 (`/etc/samba/smb.conf`)

```ini
[share]
   path = /srv/samba/share
   browseable = yes
   writable = yes
   guest ok = no
   read only = no
   valid users = samba
```

---

### 👤 3.5 사용자 계정 및 SAMBA 사용자 등록

```bash
useradd samba
smbpasswd -a samba   # 비밀번호 입력
```

#### 등록 확인

```bash
pdbedit -L
```

---

### 🔁 3.6 SAMBA 서비스 실행 및 자동 시작 설정

```bash
systemctl restart smb
systemctl restart nmb
systemctl enable smb
systemctl enable nmb
```

---

### 🧪 3.7 Windows 클라이언트 접속 테스트

- 실행창(Win + R)에 입력:

```
\\<리눅스 IP>\share
```

- 사용자명: `samba`  
- 비밀번호: 등록된 SAMBA 비밀번호
  
  ![image](https://github.com/user-attachments/assets/d5bed147-58c6-4e11-9279-1e64f9fc81f2)

---

윈도우에서 인증 기반 SAMBA 공유 동작 확인.
