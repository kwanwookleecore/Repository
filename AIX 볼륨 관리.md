# AIX 볼륨 관리

## 볼륨 그룹(VG), 논리 볼륨(LV), 파일 시스템(FS) 생성

### 1. 사용 가능한 물리 볼륨(PV) 확인
```bash
lspv
```
> VG 생성을 위해 사용 가능한 PV를 확인합니다.

### 2. 물리 볼륨의 용량 확인
```bash
bootinfo -s hdisk2
```
> `hdisk2`의 용량을 확인합니다.

### 3. 볼륨 그룹 생성
```bash
smitty vg
```
- **Add a Volume Group**
- **Add an Original Volume Group**
  - **VOLUME GROUP name:** `oraclevg`
  - **PHYSICAL VOLUME names:** `hdisk1`
  - **Force the creation of a volume group?** `yes`

### 4. 논리 볼륨 생성
```bash
smitty lv
```
- **Add a Logical Volume**
  - **VOLUME GROUP name:** `oraclevg`
  - **Number of LOGICAL PARTITIONS:** `50` (PP 크기 확인 후 지정)
  - **PHYSICAL VOLUME names:** `hdisk1`
  - **Logical volume TYPE:** `jfs2`

### 5. VG 및 LV 생성 확인
```bash
lsvg -o | lsvg -il
```
> 생성된 VG와 LV를 확인합니다.

### 6. 파일 시스템 생성
```bash
smitty jfs2
```
- **Add an Enhanced Journaled File System**
  - **SIZE of file system**
    - **Unit Size:** `Gigabytes`
    - **Number of units:** `100`
  - **MOUNT POINT:** `/oracle`
  - **Mount AUTOMATICALLY at system restart?** `yes`

### 7. 파일 시스템 마운트
```bash
mount /oracle
```
> `/oracle` 파일 시스템을 마운트합니다.

### 8. 파일 시스템 확인
```bash
df -gt
```
> 파일 시스템을 확인합니다.

## VG, LV, FS 제거

### 1. 파일 시스템 마운트 해제
```bash
umount /oracle
```
> `/oracle` 파일 시스템 마운트를 해제합니다.

### 2. 파일 시스템 제거
```bash
smitty jfs2
```
- **Remove an Enhanced Journaled File System**
  - **FILE SYSTEM name**
  - **Remove Mount Point:** `yes`

### 3. 논리 볼륨 제거
```bash
smitty lv
```
- **Remove a Logical Volume**
  - **LOGICAL VOLUME name:** (Select LV)

### 4. 볼륨 그룹 제거
```bash
smitty vg
```
- **Remove a Volume Group**
  - **VOLUME GROUP name:** `oraclevg`
