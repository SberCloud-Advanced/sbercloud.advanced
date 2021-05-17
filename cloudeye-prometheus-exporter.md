# cloudeye-exporter

**Cloud Eye** — это многомерная платформа мониторинга ресурсов. Используется для мониторинга использования ресурсов и отслеживания состояния работы облачных сервисов, настройки правил оповещений и уведомлений, а также быстрого реагирования на изменения ресурсов.

## Установка Cloudeye exporter

Для запуска CloudEye Exporter выполните следующие шаги: 

1.	Склонируйте репозиторий c github.com.

    ```
    $ git clone https://github.com/huaweicloud/cloudeye-exporter
    ```

2. (необязательно) Пошаговая установка языка go на чистую Ubuntu 16.04
    ```
    $ wget https://dl.google.com/go/go1.12.5.linux-amd64.tar.gz
    $ sudo tar -C /usr/local -xzf go1.12.5.linux-amd64.tar.gz
    ```
    Укажите, путь до папки, в которую вы хотите установить go: в папку .profile или в .bashrc
    ```
    $ export PATH=$PATH:/usr/local/go/bin
    ```

    Проверьте, что вы установили **go**. Ответ в строке указывает версию установленного файла go и будет отображаться следующим образом **go1.12.5.linux-amd64.tar.gz**.
    ```
    $ go version
    ```

3. Загрузите недостающие файлы cloudeye exporter с github.com и запустите установку.
```
$ go get github.com/huaweicloud/cloudeye-exporter
$ cd ~/go/src/github.com/huaweicloud/cloudeye-exporter
$ go build
```

## Использование

1. Запустите сборку cloudeye exporter

```
./cloudeye-exporter  -config=clouds.yml
```

    Порт для cloudeye-exporter по умолчанию **8087**, конфигурационный файл по умолчанию - `./clouds.yml`.

    Просмотр метрик доступен по следующей ссылке:  http://localhost:8087/metrics?services=SYS.VPC,SYS.ELB

2.	Для запуска необходимо подставить требуемые данные в заготовку в конфигурационном файле. Для этого откройте в вашем редакторе конфигурационный файл:


## Help
```
Usage of ./cloudeye-exporter:
  -config string
        Path to the cloud configuration file (default "./clouds.yml")
  -debug
        If debug the code.

```

## Пример значений конфигурационного файла(clouds.yml)
The "URL" value can be get from [Identity and Access Management (IAM) endpoint list](https://developer.huaweicloud.com/en-us/endpoint).
```
global:
  prefix: "huaweicloud"
  port: ":8087"
  metric_path: "/metrics"

auth:
  auth_url: "https://iam.ru-moscow-1.hc.sbercloud.ru:443/v3"
  project_name: "ru-moscow-1"
  access_key: "{access_key}"
  secret_key: "{secret_key}"
  region: "ru-moscow-1"

```
**Описание параметров:** 

* `auth_url` - IAM актуального, нашего региона.Используйте:
    * https://iam.ru-moscow-1.hc.sbercloud.ru:443/v3. 
    * https://iam.myhuaweicloud.com/v3 - это интернациональный эндпойнт, подходит для внутреннего пользования.
* `project_name` - укажите наименование базового проекта в IAM, если не был создан новый. Основным проектом сейчас является ru-moscow-1.
* `access_key` - ключ, который создает пользователь внутри IAM. 
* secret_key - ключ, который получает пользователь из iam. Приходит в файле и создается внутри IAM. 
* `region` - id региона OP4-Москва. Например, для Москвы "ru-moscow-1".

>**Примечание.**
>Для получения ключей access_key и secret_key:
>1.	Зайдите в ваш аккаунт IAM → выберите My Credentials.
>2.	Переключитесь на вкладку Access Keys.
>3.	Нажмите на кнопку Create Access Keys. 
>    Далее на ваше устройство скачается файл Excel, в котором будут >указаны ключи Access Key и Secret Key.
>4.	Скопируйте ключи из файла и подставьте значения в файл clouds.yml.
>5.	Сохраните изменения.

or

```
auth:
  auth_url: "https://iam.ru-moscow-1.hc.sbercloud.ru:443/v3"
  project_name: "ru-moscow-1"
  user_name: "{username}"
  password: "{password}"
  region: "ru-moscow-1"
  domain_name: "{domain_name}"
```

**Описание параметров:**

* `auth_url` - IAM актуального, нашего региона.Используйте:
  * https://iam.ru-moscow-1.hc.sbercloud.ru:443/v3. 
  * https://iam.myhuaweicloud.com/v3 - это интернациональный эндпойнт, подходит для внутреннего пользования.
* `project_name` - укажите наименование базового проекта в IAM, если не был создан новый. Основным проектом сейчас является ru-moscow-1.
* `user_name` - укажите имя тенанта\юзера(саб-тенанта).
* `password` - укажите пароль тенанта\юзера(саб-тенанта).
* `region` - укажите id региона ОР4-Москва. Например, для Москвы "ru-moscow-1".
* `domain_name` - укажите id тенанта(account id).

После подстановки значений в конфигурационном файле `clouds.yml` сохраните изменения и запустите СloudEye Exporter.
```
./cloudeye-exporter  -config=clouds.yml
```

Перейдите по ссылке http://localhost:8087/metrics?services=SYS.VPC,SYS.ELB для просмотра метрик.


## Конфигурация Prometheus 
Экспортеру sbercloud необходимо передавать адрес в качестве параметра. Это можно сделать с помощью переназначения.

Example config:

```
global:
  scrape_interval: 1m # Set the scrape interval to every 1 minute seconds. Default is every 1 minute.
  scrape_timeout: 1m
scrape_configs:
  - job_name: 'sbercloud'
    static_configs:
    - targets: ['10.0.0.10:8087']
    params:
      services: ['SYS.VPC,SYS.ELB']
```
