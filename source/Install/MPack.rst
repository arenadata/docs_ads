Подготовка к установке Arenadata Streaming MPack
==================================================

Основные компоненты **ADS** устанавливаются с помощью **Ambari Management Pack** (**MPack**):

+ `Загрузка Arenadata Streaming MPack`_
+ `Импортирование MPack в Ambari стек`_


Загрузка Arenadata Streaming MPack
------------------------------------

Для инсталляции **Arenadata Streaming MPack** необходимо загрузить и распаковать следующие архивы в выделенном для них месте (при этом следует избегать использования каталога */tmp*):

+ *ads-ambari-mpack-{version}.tar.gz* – набор конфигураций сервисов и скриптов для развертывания и настройки платформы через **Ambari**.

В случае если архивы загружены в каталог */tmp*, то для их распаковки в каталоге, например, */staging*, необходимо выполнить следующую команду:

:command:`tar –xzvf /tmp/ads-ambari-mpack-{version}.tar.gz -C /staging/`



Импортирование MPack в Ambari стек
------------------------------------

Для импортирования стека **ADS** в **Ambari** необходимо выполнить следующие команды:

1. Сохранить текущую папку *Ambari Resources* для возможного восстановления системы:

  :command:`cp -r /var/lib/ambari-server/resources /var/lib/ambari-server/resources.backup`

2. Запустить команду импорта стека **ADS**:

  :command:`ambari-server install-mpack --mpack=/staging/ads-ambari-mpack-{version}.tar.gz --purge --verbose`

3. Перезапустить **Ambari**:

  :command:`ambari-server restart`
