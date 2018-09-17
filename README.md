# Matrix Log
Matrix Log has following projects
 - [Docker containers](https://github.com/denis019/matrixLogDocker)
 - [Matrix Log Importer](https://github.com/denis019/MatrixLogImporter)
 - [Matrix Log Presenter](https://github.com/denis019/MatrixLogPresenter)
 
### Docker containers
 - mongodb, Matrix Log Importer will import logs and Presenter will read
 - php-cli, for the Matrix Log Importer
 - nginx, for the Matrix Log Presenter
 - php-lumen, for the Matrix Log Presenter

### Microservices
### [Matrix Log Importer](https://github.com/denis019/MatrixLogImporter)
CLI application which can parse very large log files, log file will be splited in the smaller files for the reading purpose, batch size its 10000 lines, with this approach we can have much faster reading and in combination with yield we can not have some memory usage errors. When we start with import we will read the last entered line and the script will start from the next one.
### Usage
```sh
$ php application.php {filePath}
$ php application.php {filePath}
```
Run Unit tests
```sh
$ php vendor/bin/phpunit tests/
```
### [Matrix Log Presenter](https://github.com/denis019/MatrixLogPresenter)
Lumen application to read log data, for now it's only implemented counting feature by provided filter

Example Request:
http://localhost/matrix?services=USER-SERVICE&statusCode=201&fromDateTime=17-08-2018 10:00:00 +0000&toDateTime=18-08-2018 10:32:00 +0000

Example Response:
```sh
{
    "counter":2
}
```
With this microservices we can easily scale import and data presentation.

# Instalation
### 1. Clone All repositories
```sh
git clone https://github.com/denis019/matrixLogDocker.git .
git clone https://github.com/denis019/MatrixLogPresenter.git
git clone https://github.com/denis019/MatrixLogImporter.git
```
In the end we will have the following file structure
```sh
matrixLogDocker
MatrixLogImporter
MatrixLogPresenter
```
### 2. Start Docker images with docker-compose
```sh
docker-compose up --build -d
```
### 3. Install composer dependencies
```sh
docker exec -it matrix_php_cli bash
root@c7cc74533e5e:/var/www/matrix-log/MatrixLogImporter# composer install
```
```sh
docker exec -it matrix_php_lumen bash
root@c7cc74533e5e:/var/www/matrix-log/MatrixLogImporter# composer install
## Run Unit Tests
php vendor/bin/phpunit tests/
## Import example logs
php application.php app:import-logs /var/www/matrix-log/MatrixLogImporter/tests/log-pool/example-logs.log
```
```sh
docker exec -it matrix_php_lumen bash
root@ced986f2e3d7:/var/www/matrix-log/MatrixLogPresenter# composer install
```
Open
http://localhost/matrix?services=USER-SERVICE&statusCode=201&fromDateTime=17-08-2018+10%3A00%3A00+%2B0000&toDateTime=18-08-2018+10%3A32%3A00+%2B0000

## Notes
If you want to handle periodicaly log file add cron to hit the following script
 ```sh
php application.php app:import-logs /var/www/matrix-log/MatrixLogImporter/tests/log-pool/example-logs.log
```

## TODO
 - handle better different environment for Matrix Log Importer
 - improve Repostiory abstraction for Matrix Log Importer


