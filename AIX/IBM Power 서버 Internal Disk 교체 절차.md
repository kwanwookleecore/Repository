# 🛠 IBM Power 서버 Internal Disk 교체 절차

## 1. 🔍 Error Message 확인
```bash
# errpt
# errpt -aj [IDENTIFIER_ID]
```

## 2. 💽 Internal Disk 상세 정보 확인 (FRU Number, Location Code 등)
```bash
# lscfg -vl hdisk0
# lscfg -vl hdisk1
```

## 3. ❌ OS Volume Group 미러 해제
```bash
# unmirrorvg rootvg [hdiskX]  # 제거할 디스크
```

## 4. 📦 Logical Volume 잔존 여부 확인
```bash
# lsvg -l rootvg
# lspv -l hdisk0
# lspv -l hdisk1
```

- LV가 제거할 디스크에 남아 있으면 이동 필요:
```bash
# migratepv -l hd5 hdiskX hdiskY
```

- Primary dump device 변경:
```bash
# sysdumpdev -P /dev/sysdumpnull
```

## 5. 🔻 Volume Group에서 디스크 제거
```bash
# reducevg rootvg hdiskX
```

## 6. ❎ 시스템 디바이스에서 디스크 제거
```bash
# rmdev -dl hdiskX
```

## 7. 🔧 물리 디스크 제거 (Hot Plug)
```text
# diag
→ Task Selection
→ Hot Plug Tasks
→ SCSI and SCSI RAID Hot Plug Manager
→ Replace/Remove Device
→ 제거할 디스크 선택 (LED 점등 확인)
```

## 8. 📥 New Disk 인식 및 확인
```bash
# cfgmgr -v
# lscfg -vpl hdisk0
# lscfg -vpl hdisk1
# chdev -l hdisk0 -a pv=yes
# lspv
```

## 9. ➕ Volume Group에 새 디스크 추가
```bash
# extendvg rootvg hdisk0
```

## 10. 🔁 rootvg 미러 재구성
```bash
# mirrorvg -S rootvg hdisk0 hdisk1
```

## 11. ✅ 미러 상태 확인 및 sync
```bash
# lsvg -l rootvg
# syncvg -v rootvg
```

## 12. 💽 부트이미지 생성
```bash
# bosboot -ad /dev/hdisk0
```

## 13. 📋 부트리스트 설정 및 확인
```bash
# bootlist -m normal -o
# bootlist -m normal hdisk0 hdisk1
# bootlist -m normal -o
```
