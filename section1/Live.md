# Live

## 1. 計算機發展史基礎和操作系統的安裝

### 1. 工慾善其事，必先利其器

1. 顯示器 - 27~31寸，KVM分屏器

2. 臺式機（有條件，工作站，3000-3500）

3. 固態硬盤 - m.2 SDD

4. 筆記軟件 - typora + git

5. 效率軟件 - ssh client - mobaxterm/draw.io

6. 科學上網 - vpn

   

### 2. 模塊#1概述講解

- 計算機的發展歷史

  

- **運維必備**：必須要學會各種操作系統發行版本的安裝

  - 虛擬機一般比較順利，物理機比較困難
  - 一定要完全理解安裝操作系統的流程
  - 至少：在非硬件問題的條件下快速將操作系統安裝上

  

- **裝系統步驟**

  1. 準備硬件

  2. 下載ISO

  3. 準備USB啓動設備，將ISO燒錄進U盤

  4. 直接通過BMC裏的虛擬鏡像將本地的ISO映射到服務器上 - ikvm/vmedia/https/https/nfs

     - 相當於通過網絡將本地光盤映射到服務器的虛擬光盤上 = 將真正的光盤查到服務器的光驅上

  5. 設置引導順序，一般來説會設置為ISO引導

  6. 裝系統

  7. 設置

     

### 3. [Linux發行版本分支](https://en.wikipedia.org/wiki/Linux_distribution)

- **Branch**

  - slackware 

  - debian

  - Red Hat

    

- [Ranking](https://distrowatch.com/dwres.php?resource=popularity)



- **Why RHEL?**

  - 認證Since 1999.1

    - `RHCSA8 - RedHat Certificated System Administrator` = RH124 + RH134

    - `RHCE8 - RedHat Certificate Engineer`               = RH294

      

  - UpstreamFirst
    - **原則：緊跟上游，不在自己產品中放任何沒有被Upstream接納的代碼，在上游社區的代碼基礎上做減法，篩選出隊企業用戶有價值，成熟問題的代碼來構建自己的發行版**
      - ==Upstream==   - 各種開源項目 - Linux Kernel/Apach/K8s
      - ==Downstream== - fork Upstream形成自己的一個分支進行維護
    
  - Red Hat Linux
    - `Fedora` - ==Upstream==
      - 面嚮社區完全免費
      - 每6個月發佈一個新版本
    - `RHEL` - ==Downstream==
      - 根據Fedora的使用情況和社區反饋，選擇成熟穩定的功能和版本，從Fedora中fork出來一個分支
      - 在該分支上進行QE/QA，壓力測試，和其他商業公司進行各種面嚮生產的技術支持並最終發佈

  

- **CentOS**

  - Red Hat Linux needs to subscribe, upset:(
  - Gregory Kurtzer -> `CentOS` - Community Enterprise Operating System
  - `Red Hat Enterprise Linux` -> `CentOS`



- **No CentOS6/7/8？**

  - `CentOS Stream` since [2020/12/8](2020/12/8)
    - `Fedora` -> `CentOS Stream` -> `Red Hat Enterprise Linux`
      - 有了`CentOS Stream`后，相當於Red Hat將完全封閉的`Red Hat Enterprise Linux`開發環境完全公開（以前是封閉的）
      - 任何人都可以為`CentOS Stream`進行貢獻參與到`Red Hat Enterprise Linux`早期的開發工作

  

- **[Rocky Linux](https://rockylinux.org/)**

  - Sussussor of `CentOS`
  - `Red Hat Enterprise Linux` -> `Rocky Linux`
  - [aliyun mirror](https://developer.aliyun.com/mirror/rockylinux?spm=a2c6h.13651102.0.0.6bd71b11r8sZOY)

## 2. Linux基礎/高級文件管理

### 1. Install Rocky Linux



### 2. 重點提煉

- Linux File Management cmd mainly for config & script

- Linux的一級目錄

- Absolute/Relative path

- Before cmd
  - who am I? - `whoami`
  - Where am I? - `pwd`
  - What I'm gonna do?

- Basic cmd

  ```bash
  # 不要可以背參數，要去理解
  ls
  cd
  touch
  mkdir
  rm
  cp
  mv
  cat
  tac
  head
  tail
  more
  less
  file
  * tar 
  * zip
  grep
  vi
  vim
  ```

- Regex

- 

## 3. Linux用戶/用戶組/密碼管理

