# pipeline-plugin

Этот плагин добавляет шаги (**steps**) Jenkins Pipline для взаимодействия с API SberCloud.

## Как установить плагин

Склонируйте репозиторий с github: 
```
git clone https://github.com/huaweicloud/pipeline-huaweicloud-plugin
cd pipeline-huaweicloud-plugin
```
Установите **DskipTests** с помощью фреймворка Apache Maven. 

Обратите внимание, что версия Java должна быть 9 или выше.
```
mvn package -DskipTests
```
Если у вас ранее не был установлен Apache Maven, установите его с помощью команды: 

```
sudo apt install maven
```
Установите pipeline-huaweicloud-plugin/target/pipeline-huaweicloud.hpi в jenkins (например, такой http://127.0.0.1/pluginManager/advanced)

## Использование / Шаги (Steps)

### withOBS

Для выполнения кода `withOBS` пройдите авторизацию. Для этого укажите: 

1.	`region` — регион, в котором открыт ваш тенант. В данном случае это ru-moscow-1;
2.	`endpointUrl` — перейдите в вашу корзину OBS. В разделе Basic Information найдите строку **Endpoint**.  Скопируйте значение из этой строки и подставьте в `endpointURL`.
3.	`credentials` — укажите наименование или ID вашего тенанта. Для этого перейдте в раздел **Credentials**.

>**Примечание.** 
>Доступ к бакетам ОBS (приватным) осуществляется только для добавленных в доступ тенантов (или IАМ пользователей), которые успешно прошли аутентификацию (с помощью Аccess Кey или Security Key), поэтому >достаточно только их имени или ID.


```groovy
 withOBS(endpointUrl:"http://obs.ru-moscow-1.hc.sbercloud.ru/",region:'ru-moscow-1',credentials:'TenantName/TenantId') {
    // do something
}
```

Если вы используете Jenkins Declarative Pipelines, вы можете добавить значения в блок опций (**options**) в `withOBS`:

```groovy
options {
        withOBS(endpointUrl:"http://obs.ru-moscow-1.hc.sbercloud.ru/",region:'ru-moscow-1',credentials:'TenantName/TenantId')
}
stages {
        ...
}
```
## obsUpload

Загрузка файла в рабочую область корзины OBS (or a String).

1. Для загрузки файла пройдите авторизацию.
2. В steps укажите значения для:
   - `file` - наименование файла и его расширение, который вы хотите загрузить в корзину OBS.
   - `bucket` - наименование корзины OBS.
   - `path` - путь до загружаемого файла.

```groovy
options {
  withOBS(endpointUrl:"http://obs.ru-moscow-1.hc.sbercloud.ru/",region:'ru-moscow-1',credentials:'TenantName/TenantId')
}
steps {
  obsUpload(file:'FileName.xyz', bucket:'BucketName', path:'/')
}
```

### invokeFunction

Вызов функции.

Шаг возвращает объект (object), заданный функцией.

```groovy
steps {
  script {
    def result = invokeFunction(functionName: 'FunctionName', payloadAsString: '{"key": "value"}')
    echo "Testing the ${result} browser"
 }
}
```