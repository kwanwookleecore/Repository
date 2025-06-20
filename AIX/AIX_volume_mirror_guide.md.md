# rootvg 미러링 (mirrorvg)

---

## 1. hdisk 용량 확인
> **Mirror 대상 디스크는 기존 디스크보다 용량이 같거나 커야 합니다.**

- **PV 확인**  
    ```shell
    lspv
    ```

- **용량 확인**  
    ```shell
    bootinfo -s hdiskX
    ```

---

## 2. hdisk 상태 확인 (생략 가능)
- **hdisk 상태 확인**  
    ```shell
    lspv
    ```

- **PVID 생성 (이미 생성된 경우 PASS)**  
    ```shell
    chdev -l hdiskX -a pv=yes
    ```

---

## 3. rootvg에 hdisk 추가
- **hdisk 추가**  
    ```shell
    extendvg -f [VG name] [hdiskX]
    ```

- **PV 상태 확인**  
    ```shell
    lspv
    ```

---

## 4. 미러링 (mirrorvg)
- **SMITTY 명령어 실행**  
    ```shell
    smitty mirrorvg
    ```

- **설정 과정**
    - **볼륨 그룹 선택**  
      - `rootvg` 선택
    
    - **Mirror Sync Mode 설정**  
      - `Background` (백그라운드 모드)

    - **PHYSICAL VOLUME Names 선택**  
      - 미러 대상 PV 선택 (`hdisk1`)  
    
    - **STALE PPs 값 확인**  
      - `STALE PPs`가 `0`이 될 때까지 확인 필요

    - **자동 확인 스크립트**
      ```shell
      while true
      do
        lsvg rootvg | grep -i stale
        sleep 3
      done
      ```

---

## 5. 미러링 확인 및 Sync 상태 확인
- **LPs, PPs 및 Sync 확인**  
    ```shell
    lsvg -l rootvg
    ```

---

## 6. 미러링 해제 (unmirrorvg)

- **미러링 해제**  
    ```shell
    unmirrorvg rootvg hdisk1
    ```

- **hdisk 제거**  
    ```shell
    reducevg rootvg hdisk1
    ```

---

## 참고 사항
- 미러링 대상 디스크는 기존 디스크와 **용량이 같거나 커야** 합니다.
- `STALE PPs`가 `0`이 될 때까지 Sync 상태를 **반복 확인**해야 합니다.
- 미러링 작업 중에는 시스템 성능이 **일시적으로 저하**될 수 있습니다.

---
