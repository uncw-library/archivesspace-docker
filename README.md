Scratching together some ideas.

Problems:
  - to build an image, we could inject the plugins & config into the docker image.  But that requires also pulling in all the archivesspace code.
  - so far, I've gone half-way down both direction, but should pick only one.
  - maybe make this only the production docker-compose, and leave Lyrasis to make the docker image



# ArchivesSpace docker compose

revised from [ArchivesSpace docs.](https://docs.archivesspace.org/administration/docker/)  Lyrasys offers this as an officially supported production install.

## Dev box
1) Create a file archivesspace-docker/.env with contents:
    ```
    ...add contents
    ```
1) (optional) place a recent sql dump into archivesspace-docker/sql/  This folder is gitignored.
1) (optional) revise the config file at ./archivesspace/config/config.rb
1) (optional) add any plugins to archivesspace-docker/archivesspace/plugins.  This folder is gitignored.
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
```
docker compose down
docker volume rm archivesspace_db-data
docker compose up -d
```

# Production
1) git clone this repo and git checkout branch 'production'
1) Create an env file at ./.env with contents:
    ```
    ...add contents
    ```
1) our db is on a different server, so setup that database and point to it the archivesspace/config/config.rb
1) `docker compose up -d`
1) the same commands for clearing the solr or the app data


