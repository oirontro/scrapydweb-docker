# scrapydweb docker #


Docker image of [scrapydweb](https://github.com/my8100/scrapydweb).

1. `scrapydweb` does not support Python beyond 3.7.
2. default settings provided.
3. aliyun mirror used for apt install.
4. douban mirror used for pypi.



Example of `docker-compose.yml`:

    version: '3'

    services:

      scrapyd:
        image: caky/scrapyd
        restart: always
        volumes:
          - /data/scrapyd:/app
        ports:
          - 6800:6800
        networks:
          - default

      scrapydweb:
        image: caky/scrapydweb
        restart: always
        depends_on:
          - scrapyd
        volumes:
          - /data/scrapydweb:/app/data
          - /data/scrapy_projects:/app/scrapy_projects
        ports:
          - 8085:5000
        networks:
          - default
        environment:
          DATA_PATH: "/app/data"
          SCRAPYDWEB_USER: ${SCRAPYDWEB_USER}
          SCRAPYDWEB_PASSWORD: ${SCRAPYDWEB_PASSWORD}
          TZ : "Asia/Shanghai"

      networks:
        default:
          driver: bridge
