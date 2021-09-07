## ScrapyDouban

A scraper used to scrape Chinese movie and reading website(Douban), it can download the frontpage, parse metadata and store the comments.
 Scrapy 2.5.0

### Docker
-------
3 docker containers: 

1. douban_scrapyd: scrapyd container, docker image based on [python:3.9-slim-buster](https://pythonspeed.com/articles/base-image-python-docker-images/);  these libs are installed for python3: scrapy scrapyd pymysql pillow arrow; default port mapping is 6800:6800, you can use IP:6800 to access scrapyd admin page, the login info is scrapyd/public.

2. douban_db: container for mysql instance, docker image based on  mysql:8, root password is public，when container start,  docker/mysql/douban.sql will be executed.

3. douban_adminer: adminer container, docker image based on  adminer:4，default port 8080:8080，username:root password:public.


### SQL
------

Project SQL file docker/mysql/douban.sql. 

### Steps
-------

    Scrape Subject ID --> Scrape item pages/meta data using Subject ID --> Scrape comments using Subject ID

### Usage
-------
    # Build the docker container
    $ cd ./ScrapyDouban/docker
    $ sudo docker-compose up --build -d
    # Get into douban_scrapyd container
    $ sudo docker exec -it douban_scrapyd bash
    # scrapy directory
    $ cd /srv/ScrapyDouban/scrapy
    $ scrapy list
    # Scrape movie data
    $ scrapy crawl movie_subject # Scrape Subject ID
    $ scrapy crawl movie_meta # Scrape movie meta info
    $ scrapy crawl movie_comment # Scrape movie comments
    # Scrape book data
    $ scrapy crawl book_subject # Scrape Subject ID
    $ scrapy crawl book_meta # Scrape book meta info
    $ scrapy crawl book_comment # Scrape book comments

### Proxy
--------

Use IP Proxy to pass website's anti-crawler protection, look douban.middlewares.ProxyMiddleware for the details.


### Download Picture
--------

douban.pipelines.CoverPipeline is the implementation for picture download, downloaded pictures are in /srv/ScrapyDouban/storage.
