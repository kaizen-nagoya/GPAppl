> docker ps
CONTAINER ID   IMAGE                            COMMAND                  CREATED          STATUS                      PORTS                               NAMES
9e1f1a67c83b   moodlehq/moodle-php-apache:8.3   "moodle-docker-php-e…"   57 minutes ago   Up 57 minutes               127.0.0.1:8000->80/tcp              moodle-docker-webserver-1
20838570f21e   selenium/standalone-firefox:4    "/opt/bin/entry_poin…"   57 minutes ago   Up 57 minutes               4442-4444/tcp, 5900/tcp, 9000/tcp   moodle-docker-selenium-1
ca52bd2232a1   moodlehq/moodle-exttests         "docker-php-entrypoi…"   57 minutes ago   Up 57 minutes (unhealthy)   80/tcp                              moodle-docker-exttests-1
683286ec5eae   postgres:17                      "docker-entrypoint.s…"   57 minutes ago   Up 57 minutes               5432/tcp                            moodle-docker-db-1
0de86d8cfaa8   axllent/mailpit:v1.10            "/mailpit"   
