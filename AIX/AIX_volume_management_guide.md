
# 📘 AIX 볼륨 관리 가이드

AIX에서 **볼륨 그룹(VG)**, **논리 볼륨(LV)**, **파일 시스템(FS)**을 생성하고 관리하는 방법입니다.

---

## 📁 1. 사용 가능한 물리 볼륨(PV) 확인
```shell
lspv
```
> **설명:** 사용 가능한 PV 목록과 상태를 확인합니다.

---

## 📏 2. 물리 볼륨 용량 확인
```shell
bootinfo -s hdisk2
```
> **설명:** `hdisk2` 디스크의 용량(GB 단위)을 확인합니다.  
> **참고:** VG 생성 시 필요한 디스크 용량을 미리 확인합니다.

---

## 📦 3. 볼륨 그룹(VG) 생성
```shell
smitty vg
```
- **Add a Volume Group** 선택  
  - **Add an Original Volume Group** 선택
    - **VOLUME GROUP name:** `oraclevg`
    - **PHYSICAL VOLUME names:** `hdisk1`
    - **Force the creation of a volume group?** `yes`

> **설명:**
> - **VOLUME GROUP name:** VG의 이름을 지정 (`oraclevg`)
> - **PHYSICAL VOLUME names:** VG에 포함할 PV (`hdisk1`)
> - **Force the creation of a volume group?:** PV가 기존에 사용 중이더라도 강제로 생성 (`yes`)

---

## 📐 4. 논리 볼륨(LV) 생성
```shell
smitty lv
```
- **Add a Logical Volume** 선택
  - **VOLUME GROUP name:** `oraclevg`
  - **Number of LOGICAL PARTITIONS:** `50` (PP 크기 확인 후 지정)
  - **PHYSICAL VOLUME names:** `hdisk1`
  - **Logical volume TYPE:** `jfs2`

> **설명:**
> - **VOLUME GROUP name:** 논리 볼륨이 포함될 VG (`oraclevg`)
> - **Number of LOGICAL PARTITIONS:** 할당할 PP 개수 (용량에 따라 결정)
> - **Logical volume TYPE:** `jfs2` (파일 시스템 타입)

---

## 🔍 5. VG 및 LV 생성 확인
```shell
lsvg -o | lsvg -il
```
> **설명:** 생성된 VG와 LV 목록을 확인합니다.

---

## 📂 6. 파일 시스템(FS) 생성
```shell
smitty jfs2
```
- **Add an Enhanced Journaled File System** 선택
  - **SIZE of file system**
    - **Unit Size:** `Gigabytes`
    - **Number of units:** `100`
  - **MOUNT POINT:** `/oracle`
  - **Mount AUTOMATICALLY at system restart?** `yes`

> **설명:**
> - **SIZE of file system:** 생성할 파일 시스템 크기
> - **MOUNT POINT:** 파일 시스템 마운트 위치
> - **Mount AUTOMATICALLY at system restart?:** 재부팅 시 자동 마운트 설정

---

## 🔗 7. 파일 시스템 마운트
```shell
mount /oracle
```
> **설명:** `/oracle` 파일 시스템을 마운트합니다.

---

## 🔎 8. 파일 시스템 확인
```shell
df -gt
```
> **설명:** 생성된 파일 시스템과 마운트 상태를 확인합니다.

---

## 🔄 VG, LV, FS 제거

### 📌 1. 파일 시스템 마운트 해제
```shell
umount /oracle
```
> **설명:** `/oracle` 파일 시스템 마운트를 해제합니다.

### 📌 2. 파일 시스템 제거
```shell
smitty jfs2
```
- **Remove an Enhanced Journaled File System** 선택
  - **FILE SYSTEM name** 선택
  - **Remove Mount Point:** `yes`

### 📌 3. 논리 볼륨 제거
```shell
smitty lv
```
- **Remove a Logical Volume** 선택
  - **LOGICAL VOLUME name:** (삭제할 LV 선택)

### 📌 4. 볼륨 그룹 제거
```shell
smitty vg
```
- **Remove a Volume Group** 선택
  - **VOLUME GROUP name:** `oraclevg`
