# Q1 Windows11で、DockerDesktopを利用し、PowerShellからコマンドを入力することを想定して、開発環境の構築の手順を示してください。
A1

はい。ここまでの検討では、**最初の土台をMoodleにして、GPAppl独自の「Experience → Evidence → Skill」を追加する**構成を前提にすると進めやすいです。

Windows 11 + Docker Desktop + PowerShellなら、まず **Moodleの開発環境をDockerで動かすところまで**を確実に作り、その後GPApplの機能を追加していくのがおすすめです。

Moodleには公式の開発用Docker環境 `moodlehq/moodle-docker` があり、Docker Composeを使ってMoodle本体とデータベース等を起動できます。([GitHub][1])

---

# 1. 全体構成

まず目標をこの構成にします。

```text
Windows 11
│
├─ PowerShell
│
├─ Git
│
├─ Docker Desktop
│    │
│    └─ WSL 2
│
└─ GPAppl-dev
     │
     ├─ moodle
     │
     ├─ moodle-docker
     │
     └─ Moodle DB
```

Docker DesktopはWindowsではWSL 2バックエンドを利用する構成が推奨されており、WSL 2上のLinuxコンテナをWindowsのターミナルから利用できます。([Docker Documentation][2])

---

# 2. WSL 2を確認

**PowerShellを管理者として起動**します。

```powershell
wsl --version
```

バージョン情報が表示されればOKです。

もしWSLが入っていなければ、

```powershell
wsl --install
```

を実行します。

Microsoft公式にも、Windows 11ではこのコマンドでWSLとUbuntuを導入する方法が案内されています。再起動が必要になる場合があります。([Microsoft Learn][3])

再起動後、

```powershell
wsl --version
```

さらに、

```powershell
wsl -l -v
```

を実行します。

例えば、

```text
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

ならOKです。

---

# 3. Docker Desktopをインストール

Docker Desktop for Windowsをインストールします。

[Docker Desktop for Windows 公式インストール手順](https://docs.docker.com/desktop/setup/install/windows-install/?utm_source=chatgpt.com)

現在のDocker公式資料では、WindowsのWSL 2バックエンドを利用する場合、WSL 2.1.5以上が必要で、最新のWSLを使用することが推奨されています。([Docker Documentation][4])

インストール時には、

> **Use WSL 2 instead of Hyper-V**

の構成を選びます。

インストール後、Docker Desktopを起動してください。

---

# 4. Dockerが動いていることを確認

PowerShellで、

```powershell
docker version
```

続いて、

```powershell
docker compose version
```

を実行します。

例えば、

```text
Docker version ...
Docker Compose version ...
```

と表示されればOKです。

さらに、

```powershell
docker run --rm hello-world
```

を実行します。

最後に、

```text
Hello from Docker!
```

が表示されれば、Dockerの基本環境は完成です。

---

# 5. Gitを確認

PowerShellで、

```powershell
git --version
```

を実行します。

なければGit for Windowsをインストールします。

[Git for Windows 公式サイト](https://git-scm.com/download/win?utm_source=chatgpt.com)

---

# 6. 開発用ディレクトリを作る

例えば、

```powershell
mkdir C:\work
cd C:\work
mkdir GPAppl
cd GPAppl
```

とします。

ここから、

```text
C:\work\GPAppl
```

をGPAppl/Moodleの開発ルートにします。

---

# 7. Moodle本体をclone

```powershell
git clone https://github.com/moodle/moodle.git
```

すると、

```text
C:\work\GPAppl
└── moodle
```

になります。

確認：

```powershell
dir
```

---

# 8. MoodleのDocker環境をclone

次に、

```powershell
git clone https://github.com/moodlehq/moodle-docker.git
```

とします。

結果：

```text
C:\work\GPAppl
│
├── moodle
│
└── moodle-docker
```

となります。

`moodle-docker`はMoodle開発者向けのDocker環境で、Docker Composeを利用してMoodle開発環境を構成するためのものです。([GitHub][1])

---

# 9. Moodleのバージョンを決める

ここは卒業研究では重要です。

最新版を無条件に使うのではなく、まず、

```powershell
cd C:\work\GPAppl\moodle
git branch -a
```

で利用可能なブランチを確認します。

例えば、

```text
MOODLE_500_STABLE
MOODLE_405_STABLE
```

などが表示されます。

**卒業研究では、途中でバージョンを変えないことをおすすめします。**

---

# 10. Moodle Docker環境へ移動

```powershell
cd C:\work\GPAppl\moodle-docker
```

そして、Moodle本体の場所を環境変数に設定します。

PowerShellでは、

```powershell
$env:MOODLE_DOCKER_WWWROOT="C:\work\GPAppl\moodle"
```

とします。

確認：

```powershell
$env:MOODLE_DOCKER_WWWROOT
```

---

# 11. Docker Composeを起動

Moodle Docker環境の構成に従って、

```powershell
docker compose up -d
```

を実行します。

少し時間がかかります。

その後、

```powershell
docker compose ps
```

でコンテナを確認します。

---

# 12. ブラウザからMoodleを確認

Docker環境のポート設定に従って、例えば、

```text
http://localhost:8000
```

などでアクセスします。

ここは**実際に使用する `moodle-docker` のCompose設定に合わせて確認**してください。

Moodleの初回セットアップ画面が表示されれば成功です。

---

# 13. ここからがGPAppl研究

ここまでで、

```text
Windows 11
    │
    ▼
PowerShell
    │
    ▼
Docker Desktop
    │
    ▼
Moodle Docker
    │
    ▼
Moodle
```

が完成します。

しかし、**ここでGPApplをMoodle本体に直接書き始めない**ことを強くおすすめします。

---

# 14. GPApplはMoodle Pluginとして作る

例えば、

```text
moodle/
│
├── mod/
│
├── local/
│
├── blocks/
│
└── ...
```

のうち、最初は `local` pluginとして、

```text
moodle/local/gpappl/
```

を作る方向が分かりやすいです。

概念的には、

```text
Moodle
│
├── User
├── Course
├── Activity
├── Quiz
├── Forum
│
└── GPAppl
     │
     ├── Experience
     ├── Evidence
     ├── Skill
     ├── Quest
     ├── Teaching
     ├── GitHub
     └── AI/BOT
```

です。

---

# 15. 最初のGPAppl機能は小さくする

いきなりGitHub、AI、動画、ポイント、特許まで作らないほうがいいです。

卒業研究の最初のMVPは、

```text
Experience
    ↓
Evidence
    ↓
Skill
```

だけで十分です。

例えば、

### Experience

```text
2026-08-26
C++ programming
```

### Evidence

```text
Compile Error
Runtime Error
Bug Fix
Test
```

### Skill

```text
C++ Debugging
```

という3層を作ります。

---

# 16. 次にGitHubを接続

その次に、

```text
GitHub
   │
   ├── Commit
   ├── Issue
   ├── Pull Request
   └── CI
        │
        ▼
   Experience
        │
        ▼
     Evidence
        │
        ▼
       Skill
```

とします。

ここで初めて、

> **「GitHub上の実際の開発履歴からSkill Evidenceを作る」**

という卒業研究らしい実験ができます。

---

# 17. BOTはさらにその後

次の段階で、

```text
Developer
    │
    ▼
BOT
    │
    ├── 質問
    ├── 回答
    ├── 修正
    └── 検証
          │
          ▼
      Experience
```

を追加します。

ここで、

> 「AIに何回質問したか」

ではなく、

> **「AIとの対話によって、どのような問題解決経験を得たか」**

を記録する設計にします。

---

# 18. 最終的な開発環境

最終的には、次の構成を目標にするとよいと思います。

```text
Windows 11
│
├── PowerShell
│
├── Git
│
├── Docker Desktop
│    │
│    └── WSL 2
│
└── C:\work\GPAppl
      │
      ├── moodle
      │    │
      │    └── local/gpappl
      │
      └── moodle-docker
             │
             ├── Moodle
             ├── MariaDB/PostgreSQL
             └── Web server
```

そしてアプリケーションとしては、

```text
                  GPAppl
                    │
        ┌───────────┼────────────┐
        │           │            │
        ▼           ▼            ▼
    Experience    Evidence     Skill
        │           │            │
        ├───────────┼────────────┤
        │           │            │
        ▼           ▼            ▼
     GitHub        BOT        Teaching
        │           │            │
        └───────────┼────────────┘
                    ▼
                  Quest
                    │
                    ▼
          Patent / Paper / Standard
```

という構造です。

### まず今日やる範囲

卒業研究の学生さんに環境構築をしてもらうなら、**今日は次の5段階だけで十分**です。

```powershell
wsl --version
docker version
docker compose version
git --version
docker run --rm hello-world
```

↓

```powershell
mkdir C:\work
cd C:\work
mkdir GPAppl
cd GPAppl

git clone https://github.com/moodle/moodle.git
git clone https://github.com/moodlehq/moodle-docker.git
```

↓

Moodle Dockerを起動
↓
ブラウザでMoodleを確認
↓
**「Hello World」的なGPApplプラグインを作る**

という順番が安全です。

なお、Docker DesktopのWindows版は現在もWSL 2バックエンドが主要な構成で、WSL 2のコード領域にプロジェクトを置くこともDocker公式が開発上のベストプラクティスとして案内しています。性能を重視する段階になったら、`C:\work` ではなくWSL側のLinuxファイルシステムにリポジトリを置く構成も検討できます。([Docker Documentation][5])

**次の段階では、「PowerShellのコマンドを1個ずつ実行 → 期待される画面/結果 → エラーになった場合の対処」まで含めた、学生向けのGPAppl/Moodle環境構築手順書にすると、そのまま卒業研究の実験環境構築資料として使えます。**

[1]: https://github.com/moodlehq/moodle-docker?utm_source=chatgpt.com "GitHub - moodlehq/moodle-docker: A docker environment for moodle developers · GitHub"
[2]: https://docs.docker.com/desktop/features/wsl/?utm_source=chatgpt.com "Docker Desktop WSL 2 backend on Windows | Docker Docs"
[3]: https://learn.microsoft.com/en-us/windows/wsl/install?utm_source=chatgpt.com "Install WSL | Microsoft Learn"
[4]: https://docs.docker.com/desktop/setup/install/windows-install/?utm_source=chatgpt.com "Install Docker Desktop on Windows | Docker Docs"
[5]: https://docs.docker.com/desktop/features/wsl/use-wsl/?utm_source=chatgpt.com "Develop with Docker Desktop using WSL 2 on Windows | Docker Docs"

# 作業記録

```shell
> mkdir gpappl


    ディレクトリ: C:\Users\kunren


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        2026/08/26     13:50                gpappl


> wsl --version
WSL バージョン: 2.6.3.0
カーネル バージョン: 6.6.87.2-1
WSLg バージョン: 1.0.71
MSRDC バージョン: 1.2.6353
Direct3D バージョン: 1.611.1-81528511
DXCore バージョン: 10.0.26100.1-240331-1435.ge-release
Windows バージョン: 10.0.26200.9168

> docker version
Client:
 Version:           29.3.1
 API version:       1.54
 Go version:        go1.25.8
 Git commit:        c2be9cc
 Built:             Wed Mar 25 16:16:33 2026
 OS/Arch:           windows/amd64
 Context:           desktop-linux

Server: Docker Desktop 4.67.0 (222858)
 Engine:
  Version:          29.3.1
  API version:      1.54 (minimum version 1.40)
  Go version:       go1.25.8
  Git commit:       f78c987
  Built:            Wed Mar 25 16:13:48 2026
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v2.2.1
  GitCommit:        dea7da592f5d1d2b7755e3a161be07f43fad8f75
 runc:
  Version:          1.3.4
  GitCommit:        v1.3.4-0-gd6d73eb8
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

> git --version
git version 2.53.0.windows.2

> cd gpappl
> git clone https://github.com/moodle/moodle.git
Cloning into 'moodle'...
remote: Enumerating objects: 1704791, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 1704791 (delta 0), reused 0 (delta 0), pack-reused 1704787 (from 1)
Receiving objects: 100% (1704791/1704791), 942.63 MiB | 7.96 MiB/s, done.
Resolving deltas: 100% (1184528/1184528), done.
Updating files: 100% (62300/62300), done.
> git clone https://github.com/moodlehq/moodle-docker.git
Cloning into 'moodle-docker'...
remote: Enumerating objects: 1496, done.
remote: Counting objects: 100% (432/432), done.
remote: Compressing objects: 100% (148/148), done.
remote: Total 1496 (delta 362), reused 285 (delta 284), pack-reused 1064 (from 1)
Receiving objects: 100% (1496/1496), 350.
> PS C:\Users\kunren\gpappl> cd moodle
PS C:\Users\kunren\gpappl\moodle> git branch -a
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/MOODLE_13_STABLE
  remotes/origin/MOODLE_14_STABLE
  remotes/origin/MOODLE_15_STABLE
  remotes/origin/MOODLE_16_STABLE
  remotes/origin/MOODLE_17_STABLE
  remotes/origin/MOODLE_18_STABLE
  remotes/origin/MOODLE_19_STABLE
  remotes/origin/MOODLE_20_STABLE
  remotes/origin/MOODLE_21_STABLE
  remotes/origin/MOODLE_22_STABLE
  remotes/origin/MOODLE_23_STABLE
  remotes/origin/MOODLE_24_STABLE
  remotes/origin/MOODLE_25_STABLE
  remotes/origin/MOODLE_26_STABLE
  remotes/origin/MOODLE_27_STABLE
  remotes/origin/MOODLE_28_STABLE
  remotes/origin/MOODLE_29_STABLE
  remotes/origin/MOODLE_30_STABLE
  remotes/origin/MOODLE_310_STABLE
  remotes/origin/MOODLE_311_STABLE
  remotes/origin/MOODLE_31_STABLE
  remotes/origin/MOODLE_32_STABLE
  remotes/origin/MOODLE_33_STABLE
  remotes/origin/MOODLE_34_STABLE
  remotes/origin/MOODLE_35_STABLE
  remotes/origin/MOODLE_36_STABLE
  remotes/origin/MOODLE_37_STABLE
  remotes/origin/MOODLE_38_STABLE
  remotes/origin/MOODLE_39_STABLE
  remotes/origin/MOODLE_400_STABLE
  remotes/origin/MOODLE_401_STABLE
  remotes/origin/MOODLE_402_STABLE
  remotes/origin/MOODLE_403_STABLE
  remotes/origin/MOODLE_404_STABLE
  remotes/origin/MOODLE_405_STABLE
  remotes/origin/MOODLE_500_STABLE
  remotes/origin/MOODLE_501_STABLE
  remotes/origin/MOODLE_502_STABLE
  remotes/origin/main
> cd ../moodle-docker
> ls

    ディレクトリ: C:\Users\kunren\gpappl\moodle-docker


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        2026/08/26     13:58                .github
d-----        2026/08/26     13:58                .gitpod
d-----        2026/08/26     13:58                assets
d-----        2026/08/26     13:58                bin
d-----        2026/08/26     13:58                tests
-a----        2026/08/26     13:58             40 .gitattributes
-a----        2026/08/26     13:58             21 .gitignore
-a----        2026/08/26     13:58           2152 .gitpod.yml
-a----        2026/08/26     13:58           1029 base.yml
-a----        2026/08/26     13:58            133 bbb-mock.yml
-a----        2026/08/26     13:58            197 behat-faildump.yml
-a----        2026/08/26     13:58           5324 config.docker-template.php
-a----        2026/08/26     13:58             75 db.mariadb.port.yml
-a----        2026/08/26     13:58            552 db.mariadb.yml
-a----        2026/08/26     13:58             75 db.mssql.port.yml
-a----        2026/08/26     13:58            269 db.mssql.yml
-a----        2026/08/26     13:58            315 db.mysql.5.6.yml
-a----        2026/08/26     13:58            315 db.mysql.5.7.yml
-a----        2026/08/26     13:58             75 db.mysql.port.yml
-a----        2026/08/26     13:58            476 db.mysql.yml
-a----        2026/08/26     13:58             75 db.oracle.port.yml
-a----        2026/08/26     13:58            188 db.oracle.yml
-a----        2026/08/26     13:58             75 db.pgsql.port.yml
-a----        2026/08/26     13:58            250 db.pgsql.yml
-a----        2026/08/26     13:58          35823 LICENSE
-a----        2026/08/26     13:58            139 matrix-mock.yml
-a----        2026/08/26     13:58            165 mlbackend.yml
-a----        2026/08/26     13:58            531 moodle-app-dev.yml
-a----        2026/08/26     13:58            326 moodle-app.yml
-a----        2026/08/26     13:58            389 phpunit-external-services.yml
-a----        2026/08/26     13:58          22715 README.md
-a----        2026/08/26     13:58            240 selenium.chrome.yml
-a----        2026/08/26     13:58            219 selenium.debug.yml
-a----        2026/08/26     13:58            260 service.mail.yml
-a----        2026/08/26     13:58            206 volumes-cached.yml
-a----        2026/08/26     13:58            154 webserver.port.yml


> $env:MOODLE_DOCKER_WWWROOT="C:\Users\kunren\gpappl\moodle"
> $env:MOODLE_DOCKER_WWWROOT
C:\Users\kunren\gpappl\moodle
> docker compose up -d
no configuration file provided: not found



```
