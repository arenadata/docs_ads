Настройка безопасности
=======================

В целях безопасности серсис **NiFi** предоставляет несколько различных параметров конфигурации. Наиболее важными являются свойства под заголовком *"security properties"* в файле *nifi.properties*. 

Для безопасной работы должны быть установлены свойства, приведенные в таблице.

.. csv-table:: Описание свойств безопасности NiFi
   :header: "Свойство", "Описание"
   :widths: 50, 50

   "nifi.security.keystore", "Имя файла Keystore, содержащего закрытый ключ сервера"
   "nifi.security.keystoreType", "Тип Keystore. Должен быть либо PKCS12, либо JKS. JKS является предпочтительным типом, файлы PKCS12 загружаются библиотекой BouncyCastle"
   "nifi.security.keystorePasswd", "Пароль Keystore"
   "nifi.security.keyPasswd", "Пароль для сертификата в Keystore. Если значение не установлено, используется nifi.security.keystorePasswd"
   "nifi.security.truststore", "Имя файла Truststore для авторизации тех, кто подключается к NiFi. Защищенный инстанс без Truststore отклоняет все входящие подключения"
   "nifi.security.truststoreType", "Тип Truststore. Должен быть либо PKCS12, либо JKS. JKS является предпочтительным типом, файлы PKCS12 загружаются библиотекой BouncyCastle"
   "nifi.security.truststorePasswd", "Пароль Truststore"
   "nifi.security.needClientAuth", "Значение true требует пройти аутентификацию при подключении клиентов. Свойство используется протоколом кластера NiFi для подтверждения, что узлы в кластере аутентифицированы и имеют сертификаты, которым доверяют Truststores"


После того, как указанные свойства были настроены, мы можем разрешить доступ к пользовательскому интерфейсу через HTTPS вместо HTTP. Это достигается установкой свойств nifi.web.https.host и nifi.web.https.port. Свойство nifi.web.https.host указывает, какое имя хоста должно запускаться сервером. Если желательно, чтобы интерфейс HTTPS был доступен из всех сетевых интерфейсов, должно использоваться значение 0.0.0.0. Чтобы администраторы могли настраивать приложение для работы только на определенных сетевых интерфейсах, можно указать свойства nifi.web.http.network.interface * или nifi.web.https.network.interface *.

После того, как вышеуказанные свойства были настроены, мы можем включить пользовательский интерфейс для доступа через HTTPS вместо HTTP. Это достигается установкой nifi.сеть.протокол https.хозяин и НИФИ.сеть.протокол https.свойство Port. НИФИ.сеть.протокол https.свойство host указывает, на каком хосте должен работать сервер. Если требуется, чтобы интерфейс HTTPS был доступен со всех сетевых интерфейсов, следует использовать значение 0.0.0.0. Чтобы разрешить администраторам настраивать приложение для работы только на определенных сетевых интерфейсах, nifi.сеть.http.сеть.интерфейс* или НИФИ.сеть.протокол https.сеть.интерфейс* можно указать свойства.

Once the above properties have been configured, we can enable the User Interface to be accessed over HTTPS instead of HTTP. This is accomplished by setting the nifi.web.https.host and nifi.web.https.port properties. The nifi.web.https.host property indicates which hostname the server should run on. If it is desired that the HTTPS interface be accessible from all network interfaces, a value of 0.0.0.0 should be used. To allow admins to configure the application to run only on specific network interfaces, nifi.web.http.network.interface* or nifi.web.https.network.interface* properties can be specified.
