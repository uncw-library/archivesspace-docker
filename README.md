# ArchivesSpace docker compose + traefik

revised from [ArchivesSpace docs.](https://docs.archivesspace.org/administration/docker/)  Lyrasys offers this as an officially supported production install.

if needing to build own images (i.e., arm64, etc), `git clone https://github.com/archivesspace/archivesspace`  and `docker build` the desired image.

(i've stashed the relevant solr Dockerfile & configs to ./solr, to help me find the arm64 solr image build)

## Dev box
1) Create a file archivesspace-docker/.env with contents similar to .env_example

1) (optional) place a recent sql dump into archivesspace-docker/db_autoimport/  The contents of this folder are gitignored.
1) (optional) created a config.rb file similar to the the ./mounted/app/archivesspace/config/config_example.rb  This file is gitignored.
1) Set the values in config.rb to match to these docker-compose.yml values:
    - AppConfig[:frontend_proxy_url] = to the docker-compose.yml routers.as_staff.rule.  i.e., 'https://archivesspace.localhost/staff'
    - AppConfig[:public_proxy_url] = to the docker-compose.yml routers.as_public.rule.  i.e., 'https://archivesspace.localhost'
    - AppConfig[:db_url] = to the docker-compose.yml db service name + listening port.  i.e., 'jdbc:mysql://as_db:3306/archivess.....'
    - AppConfig[:solr_url] = to the docker-compose.yml solr service name + listening port.  i.e., 'http://as_solr:8983/solr/archivesspace'
1) (optional) add any plugins to archivesspace-docker/mounted/app/archivesspace/plugins/.  This folder is gitignored.
    - cd ./mounted/archivesspace/plugins
    - git clone https://github.com/archivesspace-plugins/lcnaf.git
    - git clone https://github.com/hudmol/digitization_work_order.git
    - git clone https://github.com/hudmol/user_defined_in_basic.git
    - git clone https://github.com/uncw-library/archivesspace-plugins-local.git local      {i.e., clone our local plugin from our repo}
1) `docker compose up -d`

### To clear the solr index
```
docker compose down
docker volume rm archivesspace_solr-data
docker compose up -d
```

### To clear the app data
```
docker compose down
docker volume rm archivesspace_app-data
docker compose up -d
```

### To clear the db
1) (optional) place a recent good archivesspace sqldump into ./sql

2)  ```
    docker compose down
    docker volume rm archivesspace_db-data
    docker compose up -d
    ```

## Production
1) we use traefik instead of nginx, and our db is on a dedicated server.  but see git branch 'archivesspace-dev' for an example of our pre-prod server.


