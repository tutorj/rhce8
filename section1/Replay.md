# Replay

## 1. 提問的藝術 & 職場

- **理解命令比乾敲命令更爲重要！**

  

- **描述好問題**

  - 不要開局一張圖
  - 截圖而不是手機拍攝

  

- **保持謙虛**

  

- **盡量靠自己解決**

  - Search Engine

    

- **關於運維**

  - **摸魚**
    - 需要大量的實踐來學習，進而提升自己，一點騰出時間
    - 不要太明顯消極怠工
    - 有活盡量推，別推的太明顯
    - 如果提前完成，不要輕易告訴別人，可以拖延匯報進度，否則有可能後面的活會越來越多，間接導致學習的時間被壓縮
  - **跳槽** - ==做錢的奴隸，不要做老闆的奴隸！==
    - 第一跳：增加見識 + 摸魚學習
    - 第二跳：明顯增加
    - 第三跳：養老跳 - 舒適的工作，企業文化好，加班少，錢差不多



## 2. 文件管理

### 1. grep + cut

- `grep` -  filter row

  - `grep -E` = `egrep`

    

- `cut`  - filter col

  - `-d` - divide by separator

  - `-f` - field

    

  ```bash
  # example#1
  [tutorj@RHEL8-RHCE ~]$ ip a | grep -E '[0-9]*\.' | grep '/' | awk '{print A$2}'
127.0.0.1/8
  10.0.3.4/24
  192.168.56.3/32
  192.168.122.1/24
  [tutorj@RHEL8-RHCE ~]$ ip a | grep -E '[0-9]*\.' | grep '/' | awk '{print A$2}' | cut -d / -f 1
  127.0.0.1
  10.0.3.4
  192.168.56.3
  192.168.122.1
  [tutorj@RHEL8-RHCE ~]$ ip a | grep -E '[0-9]*\.' | grep '/' | awk '{print A$2}' | cut -d / -f 2
  8
  24
  32
  24
  
  # example 2
  [tutorj@RHEL8-RHCE ~]$ echo $PATH
  /home/tutorj/.local/bin:/home/tutorj/bin:/usr/share/Modules/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin
  [tutorj@RHEL8-RHCE ~]$ echo $PATH | cut -d : -f 1,3
  /home/tutorj/.local/bin:/usr/share/Modules/bin
  [tutorj@RHEL8-RHCE ~]$ echo $PATH | cut -d : -f 1-3
  /home/tutorj/.local/bin:/home/tutorj/bin:/usr/share/Modules/bin
  
  # example 3
  [tutorj@RHEL8-RHCE ~]$ for i in `cat /etc/passwd | cut -d ":" -f 1`
  > do
  > id $i
  > done
  uid=0(root) gid=0(root) groups=0(root)
  uid=1(bin) gid=1(bin) groups=1(bin)
  uid=2(daemon) gid=2(daemon) groups=2(daemon)
  uid=3(adm) gid=4(adm) groups=4(adm)
  uid=4(lp) gid=7(lp) groups=7(lp)
  uid=5(sync) gid=0(root) groups=0(root)
  uid=6(shutdown) gid=0(root) groups=0(root)
  uid=7(halt) gid=0(root) groups=0(root)
  uid=8(mail) gid=12(mail) groups=12(mail)
  ```
  

### 2. regex + grep

- regex - match file content

  | Symbol | Description         |
  | ------ | ------------------- |
  | ^      | 匹配開頭            |
  | $      | 匹配結尾            |
  | .      | 匹配任意1個字符     |
  | .*     | 匹配0個or若干個字符 |
  | h*     | 匹配0個或若干個h    |
  | h+     | 匹配1個或若干個h    |
  | h?     | 匹配0個或1個h       |
  | h{2}   | 匹配hh              |
  | [abc]  | 匹配a或b或c         |
  | [a-z]  | 匹配小寫英文字符    |
  | [A-Z]  | 匹配大寫英文字符    |
  | [a-Z]  | 匹配所有英文字符    |
  | [0-9]  | 匹配所有數字字符    |

- practice

  ```bash
  # create file under /tmp
  cat > /tmp/regex_practice << EOF 
  caaat
  catdog
  
  cat2dog
  catanddog
  dogcat
  ccat
  catdogcccc
  c1235
  
  c45678t
  Cat
  cAt
  
  catdogDogCAT
  #this is a cat
  ;this is a dog 
  $a
  
  EOF
  ```

  ```bash
  grep "cat" /tmp/regex_practice
  grep -i "cat" /tmp/regex_practice          # 忽略大小寫
  grep -o "cat" /tmp/regex_practice          # only cat
  
  grep ^cat /tmp/regex_practice              # 以cat為開頭
  grep dog$ /tmp/regex_practice              # 以dog為結尾
  grep ^catdog$ /tmp/regex_practice          # catdog
  
  grep ^cat.*dog$ /tmp/regex_practice        # 以cat為開頭，dog為結尾，中間0個或若干個字符
  grep ^cat.dog$ /tmp/regex_practice         # 以cat為開頭，dog為結尾，中間1個字符
  grep ^cat...dog$ /tmp/regex_practice       # 以cat為開頭，dog為結尾，中間3個字符
  grep -E ^cat.{3}dog$ /tmp/regex_practice   # 以cat為開頭，dog為結尾，中間3個字符, {}記得-E
  
  grep ^c[0-9]*t$ /tmp/regex_practice        # 以c開頭，t結尾，中間0個或若干個數字
  grep -E ^"[#;]" /tmp/regex_practice        # 以#或者；為開頭
  grep -e ^"#" -e ^";" /tmp/regex_practice   # multiple regex
  ```

### 3. sed

- `sed` - stream editor for filtering and transforming text

  - :) - non-interactive in script
  - 默認不會真正執行，只有-i才會執行
  
  ```bash
  # delete Nth row
  sed -i 'Nd' file
  
  # delete N~Mth row
  sed -i 'N,Md' file
  
  # delete last row
  sed -i "$d" file
  
  # delete rows contain "cat"   -> commonly used
  sed -i '/cat/d' file
  ```
  
  ```bash
  # insert before Nth row
  sed 'Ni <content>' file
  
  # append to Nth row
  sed 'Na <content>' file
  
  # insert before row starting with "ccat"
  sed '/^ccat/i <content>' file
  
  # append to row starting with "ccat"
  sed '/^ccat/a <content>' file
  ```
  
  ```bash
  # similar to "1,$s///"" in vim
  
  # substitude -> commonly used, starting "s///"
  sed 's/<old>/<new>' file
  sed 's/<old>/<new>/g' file
  
  sed 's/^SELINUX=.*/SELINUX=permissive/' /etc/selinux/config
  ```
  
  ```bash
  # print Nth row
  sed -n 'Np' file
  
  # print Nth row to last row
  sed -n 'N,$p' file
  ```

### 4. awk

- `awk` - pattern scanning and processing language

  - break file content to matrix - row & col(*)
  
  ```bash
  # print 
  awk '{print}' /etc/passwd
  awk '{print $0}' /etc/passwd
  ```
  
  ```bash
  # print row = sed -n 'Xp' file
  awk 'NR==X' /etc/passwd
  awk 'NR==X,NR==Y' /etc/passwd
  ```
  
  ```bash
  # print col
  awk -F ":" '{print $X,$Y}' /etc/passwd
  awk -F ":" '{print "uid: "$X, "\tgid is: "$Y}' /etc/passwd
  awk -F ":" 'NR==Z{print "Col#3 is: "$X, "\tCol#4 is: "$Y}' /etc/passwd
  
  # print last col
  awk -F ":" '{print $NF}' /etc/passwd
  # print 1st N col
  awk -F ":" 'NF=5' /etc/passwd
  
  
  # example
  [tutorj@RHEL8-RHCE ~]$ ifconfig | grep -A1 enp0s3 | grep inet | awk '{print $2}'
  10.0.3.4
  ```
  
  ```bash	
  # remove emtpy row
  awk NF file
  ```

### 5. screen (epel repo)

- `screen` - screen manager with VT100/ANSI terminal emulation
  
- 保證指令不會因ssh的終端而停止
  
- ssh username@host_ip

  - sshd -> sshd subprocess -> bash shell -> script cmd
  - 儅關閉shell terminal窗口時sshd subprocess會被殺掉，進而後面bash shell和script cmd也都會停止:(

  ```bash
  # install screen
  yum -y install epel-release
  yum -y install screen
  
  # create a screen session, it won't interrupt ssh 
  screen -S <scoket name>
  
  # list screen session
  screen -ls
  
  # resume session
  screen -r <socket number>.<socket name>
  
  # dettach temorarily
  Ctrl + a then press d
  
  # -x - Attach to a not detached screen. (Multi display mode = Share screen)
  scree -x <socket number>.<socket name>
  ```

### 6. tmux (baseos repo)

- `tmux` - terminal multiplexer

  - better than `screen`
  - 可以在一個terminal上分裂出很多個session

  ```bash
  # install
  yum -y tmux
  
  # create session 
  tmux -L <session name>
  
  # list session
  tmux ls
  
  # resume session
  tmux attach -t <session id>
  
  # dettach temporarily
  Ctrl + b then press d
  
  # 上下分割屏幕
  Ctrl + b then press “
  # 左右分割屏幕
  Ctrl + b then press %
  # switch between session
  Ctrl + b then press ↑↓←→
  
  # session folder
  /tmp/tmux-0
  ```

## 3. 用戶管理

### 1. 統一認證

### 2. freeipa

### ~~3. login.defs~~

### 4. $PS1

- Linux ENV loading

  - `/etc/profile` -> `/etc/profile.d/*.sh` -> `~/.bash_profile` -> `~/.bashrc` -> `/etc/bashrc`

```bash
# env
[tutorj@RHEL8-RHCE ~]$ echo $PS
$PS1  $PS2  $PS4
[tutorj@RHEL8-RHCE ~]$ echo $PS1
[tutorj@RHEL8-RHCE ~]$ echo $PS1
[\u@\h \W]\$
[tutorj@RHEL8-RHCE ~]$ echo $PS2
>
[tutorj@RHEL8-RHCE ~]$ echo $PS3

[tutorj@RHEL8-RHCE ~]$

# export
export PS1=

# persist in .bashrc
```

### 5. zabbix繼承Windows AD實現統一認證

## 4. 權限管理

