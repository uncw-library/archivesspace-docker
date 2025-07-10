# ArchivesSpace docker compose

ideas borrowed from [ArchivesSpace docs.](https://docs.archivesspace.org/administration/docker/)  Lyrasys offers this as an officially supported production install.

if needing to build own images (i.e., arm64, etc), `git clone https://github.com/archivesspace/archivesspace`  and `docker build` the desired image.

## Dev box
1) Create a file archivesspace-docker/.env with contents similar to .env_example

1) (optional) place a recent sql dump into archivesspace-docker/db_autoimport/  The contents of this folder are gitignored.
1) (optional) created a ./mounted/archivesspace/config/config.rb file similar to the the ./mounted/archivesspace/config/config_example.rb  This file is gitignored.
1) (optional) add any plugins into archivesspace-docker/mounted/archivesspace/plugins/.  This folder is gitignored.
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
