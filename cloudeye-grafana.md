## Cloudeye Datasource

[Cloud Eye](https://www.huaweicloud.com/en-us/product/ces.html) - это многомерная платформа мониторинга ресурсов. Используется для мониторинга использования ресурсов и отслеживания состояния работы облачных сервисов, настройки правил оповещений и уведомлений, а также быстрого реагирования на изменения ресурсов.

**Grafana** — технология, которая используется для создания панелей мониторинга и метрик для различных ресурсов.

### Инструкции по установке Grafana с помощью других систем

- [Installing on Debian/Ubuntu](http://docs.grafana.org/installation/debian/)
- [Installing on RPM-based Linux (CentOS, Fedora, OpenSuse, RedHat)](http://docs.grafana.org/installation/rpm/)
- [Installing on Windows](http://docs.grafana.org/installation/windows/)
- [Installing on Mac](http://docs.grafana.org/installation/mac/)

## Установка cloudeye-grafana

Installation cloudeye-grafana:
Скопируйте CloudEye Grafana в папку “datasource grafana”, чтобы установить плагин. 
```
sudo git clone https://github.com/huaweicloud/cloudeye-grafana
sudo cp -r cloudeye-grafana  /var/lib/grafana/plugins/
sudo service grafana-server restart
```
Проверьте, что CloudEye Grafana успешно установлена. Для этого перейдите по этой ссылке (http://local_ip:3000)  или (http://localhost:3000/) и зарегистрируйтесь в Grafana.

1.	Откройте браузер и перейдите по ссылке http://localhost:3000/. 
    Порт HTTP по умолчанию, прослушиваемый Grafana должен быть “3000”, если вы не указали в конфигурации иной порт.
2.	На странице авторизации в поле **Username** укажите “admin”, это же значение укажите в поле Password.
3.	Нажмите **Log In**. Если авторизация прошла успешно, то появится окно с запросом на смену пароля.
4.	Измените пароль и нажмите **ОК**.


### Config

В конфигурационном файле CloudEye-Grafana для создания собственного источника данных подставьте следующие значения в поля: “URL", "Domain", "Project", "User", "Password".

Значение "URL" можно узнать в  [Identity and Access Management (IAM) endpoint list](https://developer.huaweicloud.com/en-us/endpoint).
![image](https://github.com/huaweicloud/cloudeye-grafana/blob/master/config.png)

**Описание параметров:** 

- `auth_url` - IAM актуального, нашего региона.Используйте:
   - https://iam.ru-moscow-1.hc.sbercloud.ru:443/v3
   - https://iam.myhuaweicloud.com/v3  - это интернациональный эндпойнт, подходит для внутреннего пользования
-  `project_name` - укажите наименование базового проекта в IAM, если не был создан новый. Основным проектом сейчас является "ru-moscow-1".
-	`user_name` - укажите имя тенанта\юзера(саб-тенанта).
-	`password` - укажите пароль тенанта\юзера(саб-тенанта).
-	`domain_name` - укажите id тенанта(account id).


### Использование

1. Перейдите на сайт [Grafana](http://localhost:3000/).
2. Создайте новую панель, нажав на кнопку **+** (**Create**) --> **Dashboard**.
3. Введите нужные значения в поля "Project", "Metric" и "Dimention", чтобы получить данные метрики.

![image](https://github.com/huaweicloud/cloudeye-grafana/blob/master/dashboard.png)
