---
title: Windows 11에서 WSL2로 ext4 HDD 마운트 및 파일 백업하기
description: Windows 11 환경에서 WSL2를 사용해 ext4로 포맷된 하드디스크를 마운트하고, Windows 탐색기에서 파일을 백업하는 방법을 정리합니다.
date: 2026-07-03 13:40:00 +0900
categories: [개발, WSL]
tags: [wsl, linux, hdd, mount, ext4]
---

Linux 환경(ext4)에서 사용하던 하드디스크(HDD)를 Windows 11 PC에 연결하여 데이터를 백업해야 할 때가 있습니다. Windows는 기본적으로 ext4 파일 시스템을 읽을 수 없지만, **WSL2(Windows Subsystem for Linux 2)**의 디스크 마운트 기능을 활용하면 간편하게 연결하여 Windows 파일 탐색기에서 접근할 수 있습니다.

본 포스트에서는 외장 또는 내장 ext4 HDD를 WSL2에 마운트하고 안전하게 해제하는 전체 과정을 단계별로 정리합니다.

---

## 사전 요구사항

* Windows 11 환경
* WSL2 및 Linux 배포판(예: Ubuntu)이 설치되어 있어야 합니다.
* 작업은 관리자 권한을 필요로 하므로 **PowerShell을 관리자 권한으로 실행**해야 합니다.

> 이 디스크 마운트 기능은 **WSL2**에서만 지원됩니다. WSL1 환경에서는 작동하지 않습니다.
{: .prompt-warning }

---

## 1단계: 마운트할 HDD의 물리 디스크 번호 확인

먼저 PC에 연결된 물리 디스크 목록에서 마운트할 디스크의 ID를 찾아야 합니다.

1. **PowerShell을 관리자 권한**으로 실행합니다.
2. 아래 명령어를 입력하여 디스크 정보를 조회합니다.

```powershell
Get-CimInstance -ClassName Win32_DiskDrive | Select-Object DeviceID, Model, Size
```

**출력 예시:**
```text
DeviceID            Model       Size
--------            -----       ----
\\.\PHYSICALDRIVE1  xxxxxx-ssd  256052966400
\\.\PHYSICALDRIVE0  xxxxxx-hdd  2000396321280  << 마운트할 대상 HDD
```

위 출력에서 마운트하려는 하드디스크의 `DeviceID`가 `\\.\PHYSICALDRIVE0`인 것을 확인합니다. (디스크 번호는 시스템마다 다릅니다.)

> **에러 팁**: 명령어를 입력했을 때 `'Get-CimInstance'은(는) 내부 또는 외부 명령...`과 같은 에러가 발생한다면, 일반 명령 프롬프트(CMD)를 사용 중일 가능성이 높습니다. 반드시 **PowerShell** 창에서 실행해 주세요.
{: .prompt-danger }

---

## 2단계: WSL2에 디스크 연결 (Attach)

확인한 물리 디스크를 WSL2 엔진에 연결합니다. 파일 시스템을 리눅스 내부에서 수동으로 마운트하기 위해 `--bare` 옵션을 사용해 디스크 자체만 연결합니다.

```powershell
wsl --mount \\.\PHYSICALDRIVE0 --bare
```

정상적으로 완료되면 `작업을 완료했습니다.`라는 메시지가 나타납니다.

### 💡 발생할 수 있는 오류 해결법

#### 오류 A: `Wsl/Service/AttachDisk/WSL_E_WSL2_NEEDED`
WSL 버젼은 최신이지만, 현재 구동 중인 리눅스 배포판(Ubuntu)이 내적으로 **WSL 1** 버전으로 세팅되어 있을 때 발생합니다.

1. 배포판의 버전을 확인합니다.
   ```powershell
   wsl -l -v
   ```
2. VERSION이 `1`로 되어 있다면 아래 명령어로 WSL2로 변환합니다. (변환에는 수 분이 소요될 수 있습니다.)
   ```powershell
   wsl --set-version Ubuntu 2
   ```

#### 오류 B: `Wsl/Service/WSL_E_DISTRO_NOT_FOUND`
배포판 이름을 찾지 못하는 오류입니다. `wsl -l -v`로 출력되는 정확한 배포판 이름을 지정해 주어야 합니다.

* 예: 배포판 이름이 `Ubuntu-24.04`인 경우
  ```powershell
  wsl --set-version Ubuntu-24.04 2
  ```
* 만약 목록에 아무것도 없다면 리눅스 배포판이 완전히 설치되지 않은 것이므로 다음 명령어로 설치를 완료해야 합니다.
  ```powershell
  wsl --install -d Ubuntu
  ```

---

## 3단계: WSL2 내부에서 파티션 마운트 (Mount)

디스크 장치가 WSL2에 연결되었다면, 이제 리눅스 시스템 내부에서 특정 디렉터리에 마운트해야 합니다.

1. WSL2 터미널(PowerShell에서 `wsl` 입력 또는 Ubuntu 앱 실행)을 엽니다.
2. 추가된 디스크의 파티션 명칭을 확인합니다.
   ```bash
   lsblk
   ```
   보통 `sdb1` 또는 `sdc1` 같은 형태로 나타납니다. 여기서는 해당 디스크 파티션이 `/dev/sdb1`이라고 가정하겠습니다.

3. 마운트 경로로 사용할 디렉터리를 생성합니다.
   ```bash
   sudo mkdir /mnt/ext4_hdd
   ```

4. 생성한 디렉터리에 파티션을 `ext4` 타입으로 마운트합니다.
   ```bash
   sudo mount -t ext4 /dev/sdb1 /mnt/ext4_hdd
   ```

5. 파일이 정상적으로 보이는지 확인합니다.
   ```bash
   ls -la /mnt/ext4_hdd
   ```

---

## 4단계: Windows 탐색기에서 파일 백업하기

마운트가 완료되면 익숙한 Windows 파일 탐색기(GUI)를 통해 편하게 파일을 복사할 수 있습니다.

1. Windows 탐색기(`Win + E`)를 엽니다.
2. 왼쪽 탐색 창의 하단에 있는 **Linux** 아이콘을 클릭합니다.
3. `Ubuntu` > `mnt` > `ext4_hdd` 경로로 차례대로 이동합니다.
4. 필요한 파일이나 폴더를 Windows의 바탕화면이나 다른 C/D 드라이브로 드래그 앤 드롭하여 복사(백업)합니다.

---

## 5단계: 작업 완료 후 안전하게 분리하기 (Unmount)

파일 백업이 완료되었다면 디스크의 데이터 손상을 방지하기 위해 안전하게 마운트를 해제해야 합니다.

1. 관리자 권한의 **PowerShell**로 돌아옵니다.
2. 아래 명령어를 입력하여 WSL2 디스크 연결을 안전하게 분리합니다. (이때 디스크 번호는 2단계에서 마운트했던 번호를 입력합니다.)
   ```powershell
   wsl --unmount \\.\PHYSICALDRIVE0
   ```
3. 이제 하드디스크를 PC에서 안전하게 제거하거나, Windows 디스크 관리에서 NTFS 포맷을 진행하여 Windows 전용 디스크로 재사용할 수 있습니다.