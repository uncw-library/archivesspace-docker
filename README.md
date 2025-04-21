Scratching together some ideas.

Problems:
  - to build an image, we could bake the plugins & config into the docker image.  But that requires also pulling in all the archivesspace code.
  - so far, I've gone half-way down both direction, but should pick only one.
  - maybe make this only the production docker-compose, and leave Lyrasis to make the docker image

  - a different approach is to make this repo a wrapper around the main archivesspace folder

# Config

1. Copy the file `.env_example` to `.env`   then revise as desired.  No change is required.
1. Copy the file `./config/config-example.rb` to `.config/config.rb`   then revise as desired.
1. Set the values in config.rb to match to these docker-compose.yml values:
    - AppConfig[:frontend_proxy_url] = to the docker-compose.yml routers.as_staff.rule.  i.e., 'https://archivesspace.localhost/staff'
    - AppConfig[:public_proxy_url] = to the docker-compose.yml routers.as_public.rule.  i.e., 'https://archivesspace.localhost'
    - AppConfig[:db_url] = to the docker-compose.yml db service name + listening port.  i.e., 'jdbc:mysql://as_db:3306/archivess.....'
    - AppConfig[:solr_url] = to the docker-compose.yml solr service name + listening port.  i.e., 'http://as_solr:8983/solr/archivesspace'
1. Add any plugins to the ./archivesspace/plugins folder
    - cd ./archivesspace/plugins
    - git clone https://github.com/hudmol/digitization_work_order.git
    - git clone https://github.com/hudmol/user_defined_in_basic.git
    - git clone https://github.com/uncw-library/archivesspace-plugins-local.git local   {i.e., clone our local plugin from our repo}
1. Solr config is copied from [archivesspace repo](https://github.com/archivesspace/archivesspace/tree/master/solr) 



# ArchivesSpace docker compose

revised from [ArchivesSpace docs.](https://docs.archivesspace.org/administration/docker/)  Lyrasys offers this as an officially supported production install.

## Dev box
1) Create a file archivesspace-docker/.env similar to .env_example
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
1) place a recent good archivesspace sqldump into ./sql

2)  ```
    docker compose down
    docker volume rm archivesspace_db-data
    docker compose up -d
    ```

# Production
1) git clone this repo and git checkout branch 'production'
1) Create an env file at ./.env similar to ./.env_example:
1) revise the mounted/archivesspace/config/config.rb to production values  (i.e., db, etc)
1) git clone the plugins to mounted/archivesspace/plugins
1) `docker compose up -d`
1) the same commands for clearing the solr or the app data


