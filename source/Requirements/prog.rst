Программные требования
========================

Минимальные программные требования для установки кластера **ADS** следующие:

* ОС -- *RHEL/CentOS 7.6.1810/ALT Linux c8.0*;

* Настроенный NTP на серверах;

* Процесс установки модифицирует следующие системные файлы:

  * */etc/hosts*

  * */etc/selinux/config*

  * */etc/sysctl.conf*

  * Создает новые файлы в */usr/lib/systemd/system/*

Перечисленные файлы не должны контролироваться системой управления конфигурации (при ее наличии);

* Процесс установки останавливает и выключает сервисы firewalld и SELinux, данные сервисы не должны контролироваться системой управления конфигурации (при ее наличии);

* Процесс установки создает новые сервисы, данные сервисы не должны контролироваться системой управления конфигурации (при ее наличии);

* Со всех машин в кластере должен быть доступ к официальному репозиторию CentOS Extras (например, РБК http://centos-mirror.rbc.ru/pub/centos/7/extras/x86_64/ или создано локальное зеркало);

* Со всех машин в кластере должен быть доступ к официальному репозиторию CentOS Updates (например, РБК http://centos-mirror.rbc.ru/pub/centos/7/updates/x86_64/ или создано локальное зеркало);

* Со всех машин в кластере должен быть доступ к официальному репозиторию CentOS Base (например, РБК http://centos-mirror.rbc.ru/pub/centos/7/os/x86_64/ или создано локальное зеркало).

.. important:: В случае инсталляции с доступом в Интернет машины должны иметь доcтуп к официальным репозиториям:
    
    - **RHEL/CentOS**:

      - Репозиторий *ADS*: http://downloads.arenadata.io/ADS/1.5.0/centos/7/community/x86_64/

      - Репозиторий *Arenadata Monitoring*: http://downloads.arenadata.io/ADM/2.2/centos/7/community/x86_64/

      - Репозитоий *Zookeeper*: http://downloads.arenadata.io/zookeeper/3.5.6/centos/7/community/x86_64/

    - **Alt Linux**:

      - Репозиторий *ADS*: http://downloads.arenadata.io/ADS/1.5.0/altlinux/8.0/community/

      - Репозиторий *Arenadata Monitoring*: http://downloads.arenadata.io/ADM/2.2/altlinux/8.0/community/

      - Репозитоий *Zookeeper*: http://downloads.arenadata.io/zookeeper/3.5.6/altlinux/8.0/community/
