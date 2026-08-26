# docker ps
> docker ps
> 
CONTAINER ID   IMAGE                            COMMAND                  CREATED          STATUS                      PORTS                               NAMES

9e1f1a67c83b   moodlehq/moodle-php-apache:8.3   "moodle-docker-php-e…"   57 minutes ago   Up 57 minutes               127.0.0.1:8000->80/tcp              moodle-docker-webserver-1

20838570f21e   selenium/standalone-firefox:4    "/opt/bin/entry_poin…"   57 minutes ago   Up 57 minutes               4442-4444/tcp, 5900/tcp, 9000/tcp   moodle-docker-selenium-1

ca52bd2232a1   moodlehq/moodle-exttests         "docker-php-entrypoi…"   57 minutes ago   Up 57 minutes (unhealthy)   80/tcp                              moodle-docker-exttests-1

683286ec5eae   postgres:17                      "docker-entrypoint.s…"   57 minutes ago   Up 57 minutes               5432/tcp                            moodle-docker-db-1

0de86d8cfaa8   axllent/mailpit:v1.10            "/mailpit"               57 minutes ago   Up 57 minutes               1025/tcp, 8025/tcp                  moodle-docker-mailpit-1

# Q1 docker ps するとdockerが５つ起動しています。これをdocker hubに登録する方法はありますか。
A1 by Chatgpt

はい、できます。ただし、ここは少し重要です。

**`docker ps` に表示される5つのコンテナを、そのままDocker Hubへ登録することはできません。**
Docker Hubへ登録する基本単位は **Docker image** です。

現在のMoodle環境は、おそらく概念的に、

```text
Docker Desktop
│
├─ Moodle Web
├─ Database
├─ Web Server
├─ Mail
└─ その他の補助コンテナ
```

のように複数コンテナで構成されています。

つまり、

```text
docker ps
   ↓
5 containers
   ↓
それぞれの image
   ↓
Docker Hub
```

という関係です。

---

# まず5つの正体を確認しましょう

PowerShellで、

```powershell
docker ps
```

を実行してください。

例えば、

```text
CONTAINER ID   IMAGE                  NAMES
xxxx           moodlehq/moodle-...   moodle-docker-webserver-1
xxxx           postgres:...           moodle-docker-db-1
xxxx           ...
```

のように表示されると思います。

さらに、

```powershell
docker images
```

を実行してください。

こちらが非常に重要です。

---

# Docker Hubに登録する方法は2種類あります

今回のGPApplでは、私は **②をおすすめします**。

## ① コンテナをイメージ化してDocker Hubへ登録

例えば現在のMoodleコンテナを、

```powershell
docker commit コンテナ名
```

でimageにできます。

例えば、

```powershell
docker commit moodle-docker-webserver-1 kaizen-nagoya/gpappl-moodle:dev
```

その後、

```powershell
docker login
```

して、

```powershell
docker push kaizen-nagoya/gpappl-moodle:dev
```

とすればDocker Hubへpushできます。

ただし……

**これは今回あまりおすすめしません。**

なぜなら、Moodle Docker環境は複数コンテナから構成されているためです。

---

# ② Docker ComposeをGitHubに保存する ← おすすめ

今回はこちらが本命です。

Docker Hubには、

> 「5個のコンテナを保存する」

のではなく、

> **「この5個のコンテナをどうやって再現するか」**

を保存する設計にします。

例えば、

```text
GPAppl
│
├── moodle/
│
├── moodle-docker/
│
├── compose.yaml
├── .env
└── README.md
```

として、

```text
compose.yaml
```

に、

```text
Moodle
PostgreSQL
Web server
Mail
...
```

の構成を記述します。

すると別のWindows 11 PCでも、

```powershell
git clone ...
cd GPAppl
docker compose up -d
```

で同じ環境を再現できます。

これは卒業研究では非常に価値があります。

---

# Docker Hubは「環境の配布」に使う

例えば最終的に、

```text
GitHub
│
│ ソースコード
│
├── GPAppl Moodle Plugin
├── UML
├── README
└── Docker Compose
       │
       ▼
Docker Hub
│
├── gpappl-moodle
├── gpappl-worker
└── gpappl-bot
```

という構成にできます。

---

# 今回のGPApplなら、さらに良い構成があります

最終的には、

```text
                 GitHub
                   │
        ┌──────────┴──────────┐
        │                     │
   GPAppl Source          UML / Docs
        │
        ▼
   Docker Build
        │
        ▼
    Docker Hub
        │
 ┌──────┼────────┐
 ▼      ▼        ▼
Moodle  DB      GPAppl
```

とするのがきれいです。

そして他の学生が、

```powershell
git clone https://github.com/kaizen-nagoya/GPAppl.git
cd GPAppl
docker compose up -d
```

だけで研究環境を再現できるようにする。

---

# まず今は「5つのコンテナを調べる」のが先です

いきなり `docker commit` や `docker push` をしないでください。

PowerShellで、

```powershell
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Ports}}"
```

を実行してください。

さらに、

```powershell
docker images
```

も実行します。

この2つの結果を貼っていただければ、

**「現在のMoodle Docker環境の5コンテナのうち、何をDocker Hubに登録すべきか」**

を具体的に整理できます。

特に今回の卒業研究では、**Moodle本体やPostgreSQLを丸ごとDocker Hubに保存するより、「GPAppl独自部分だけをDocker image化し、Moodle等は公式imageを利用する」**ほうが、再現性・保守性ともにかなり良い構成になります。


# docker images
> docker images
                                                                                                    i Info →   U  In Use
IMAGE                             ID             DISK USAGE   CONTENT SIZE   EXTRA
axllent/mailpit:v1.10             105bcbd657cc       41.5MB         11.5MB    U
ev3_web-web:latest                eeb553ea6707        238MB         63.1MB    U
fugo-server-app:latest            6704555e69b3        771MB          193MB
gcc-c-works:latest                1b77919ee51b        748MB          187MB    U
kaizenjapan/pcc:latest            fe1abdbddf4e        1.5GB          389MB    U
kaizenjapan/pipe:latest           0770cdf9a090       1.41GB          392MB
kaizenjapan/sns:latest            3f9a0090b92c        926MB          314MB
moodlehq/moodle-exttests:latest   c23932615f5b        580MB          149MB    U
moodlehq/moodle-php-apache:8.3    7fd5f3356a71       1.64GB          402MB    U
postgres:17                       0b657ff48d7f        645MB          167MB    U
selenium/standalone-firefox:4     54bf6b69b790       3.11GB          961MB    U
spike_web-web:latest              efa49f37a05a        238MB         63.1MB    U
ubuntu:latest                     3131b4cc82a7        161MB         45.3MB    U
