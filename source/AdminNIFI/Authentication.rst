Аутентификация пользователя
=============================

**NiFi** поддерживает аутентификацию пользователей через сертификаты клиента, через имя пользователя и пароль, через **Apache Knox** или через `OpenId Connect <http://openid.net/connect>`_.

Проверка подлинности имени пользователя и пароля выполняется с помощью "Идентификатора входа в систему" ("Login Identity Provider") -- это подключаемый механизм для аутентификации пользователей через их имя и пароль, настройка которого осуществляется в файле *nifi.properties*. В настоящее время **NiFi** предлагает проверку имени пользователя и пароля с параметрами Provider Identity Provider для **LDAP** и **Kerberos**.

Свойство *nifi.login.identity.provider.configuration.file* указывает файл конфигурации для Идентификатора входа в систему. Свойство *nifi.security.user.login.identity.provider* указывает, какой из настроенных Login Identity Provider должен использоваться. По умолчанию свойство не настроено, что означает, что username/password должно быть явно включено.

При аутентификации через **OpenId Connect** сервер **NiFi** перенаправляет пользователей для проверки подлинности в Провайдер, а затем **NiFi** вызывает Провайдер для получения идентификации пользователя.

При аутентификации через **Apache Knox** сервер **NiFi** перенаправляет пользователей для проверки подлинности в **Apache Knox**, а затем **NiFi** во время аутентификации пользователя проверяет токен **Apache Knox**.

.. important:: Аутентификация пользователя в NiFi может быть настроена только по username/password, OpenId Connect или Apache Knox. Сервер не поддерживает одновременный запуск каждого из них. При этом NiFi потребуются сертификаты клиентов для аутентификации пользователей через HTTPS, если ничто иное не настроено

К защищенному инстансу **NiFi** нельзя получить доступ анонимно, если в **LDAP** или **Kerberos** не настроен Login Identity Provider, который, в свою очередь, должен быть настроен на явное разрешение анонимного доступа. При этом анонимный доступ в настоящее время невозможен по умолчанию в *FileAuthorizer* (`NIFI-2730 <https://issues.apache.org/jira/browse/NIFI-2730>`_).

.. important:: NiFi не выполняет аутентификацию пользователя через HTTP (используя HTTP, всем пользователям предоставляются все роли)


Lightweight Directory Access Protocol (LDAP)
---------------------------------------------

Далее приведен пример с описанием настроек Login Identity Provider, который интегрируется с Directory Server для аутентификации пользователей.

  ::
  
   <provider>
       <identifier>ldap-provider</identifier>
       <class>org.apache.nifi.ldap.LdapProvider</class>
       <property name="Authentication Strategy">START_TLS</property>
   
       <property name="Manager DN"></property>
       <property name="Manager Password"></property>
   
       <property name="TLS - Keystore"></property>
       <property name="TLS - Keystore Password"></property>
       <property name="TLS - Keystore Type"></property>
       <property name="TLS - Truststore"></property>
       <property name="TLS - Truststore Password"></property>
       <property name="TLS - Truststore Type"></property>
       <property name="TLS - Client Auth"></property>
       <property name="TLS - Protocol"></property>
       <property name="TLS - Shutdown Gracefully"></property>
   
       <property name="Referral Strategy">FOLLOW</property>
       <property name="Connect Timeout">10 secs</property>
       <property name="Read Timeout">10 secs</property>
   
       <property name="Url"></property>
       <property name="User Search Base"></property>
       <property name="User Search Filter"></property>
   
       <property name="Identity Strategy">USE_DN</property>
       <property name="Authentication Expiration">12 hours</property>
   </provider>

С помощью данной конфигурации аутентификация имени пользователя и пароля может быть активирована путем ссылки на провайдер в *nifi.properties*:

  ::
  
   nifi.security.user.login.identity.provider=ldap-provider


.. csv-table:: Описание настроек Login Identity Provider для LDAP
   :header: "Свойство", "Описание"
   :widths: 50, 50

   "Authentication Strategy", "Аутентификация подключения к LDAP-серверу. Возможные значения: ANONYMOUS, SIMPLE, LDAPS или START_TLS"
   "Manager DN", "DN менеджера, который используется для привязки к LDAP-серверу для поиска пользователей"
   "Manager Password", "Пароль менеджера, который используется для привязки к LDAP-серверу для поиска пользователей"
   "TLS - Keystore", "Путь к Keystore при подключении к LDAP с использованием LDAPS или START_TLS"
   "TLS - Keystore Password", "Пароль для Keystore при подключении к LDAP с использованием LDAPS или START_TLS"
   "TLS - Keystore Type", "Тип Keystore при подключении к LDAP с использованием LDAPS или START_TLS (то есть JKS или PKCS12)"
   "TLS - Truststore", "Путь к Truststore при подключении к LDAP с использованием LDAPS или START_TLS"
   "TLS - Truststore Password", "Пароль для Truststore при подключении к LDAP с использованием LDAPS или START_TLS"
   "TLS - Truststore Type", "Тип Truststore при подключении к LDAP с использованием LDAPS или START_TLS (то есть JKS или PKCS12)"
   "TLS - Client Auth", "Политика аутентификации клиента при подключении к LDAP с использованием LDAPS или START_TLS. Возможные значения: REQUIRED, WANT, NONE"
   "TLS - Protocol", "Протокол при подключении к LDAP с использованием LDAPS или START_TLS (TLS, TLSv1.1, TLSv1.2 и т.д.)"
   "TLS - Shutdown Gracefully", "Указывает, следует ли корректно завершать работу TLS перед закрытием целевого контекста. По умолчанию: false"
   "Referral Strategy", "Стратегия обработки рефералов. Возможные значения: FOLLOW, IGNORE, THROW"
   "Connect Timeout", "Время ожидания соединения (10 секунд)"
   "Read Timeout", "Время ожидания чтения (10 секунд)"
   "Url", "Разделенный пробелами список URL-адресов серверов LDAP (например, ldap://<hostname>:<port>)"
   "User Search Base", "Базовый DN для поиска пользователей (например, CN=Users,DC=example,DC=com)"
   "User Search Filter", "Фильтр для поиска пользователей в User Search Base (sAMAccountName={0}). Указанное пользователем имя вставляется в *{0}*"
   "Identity Strategy", "Стратегия идентификации пользователей. Возможные значения: USE_DN и USE_USERNAME. По умолчанию: - USE_DN (для сохранения обратной совместимости). USE_DN использует полный DN пользовательской записи (рекомендуется). USE_USERNAME использует имя пользователя, под которым он вошел в систему"
   "Authentication Expiration", "Продолжительность действия проверки подлинности пользователя. Если пользователь никогда не выходит из системы, он должен будет снова войти в систему в течение указанного времени"


Kerberos
---------

Далее приведен пример с описанием настроек Login Identity Provider, который интегрируется с Kerberos Key Distribution Center (KDC) для аутентификации пользователей.

  ::
  
   <provider>
       <identifier>kerberos-provider</identifier>
       <class>org.apache.nifi.kerberos.KerberosProvider</class>
       <property name="Default Realm">NIFI.APACHE.ORG</property>
       <property name="Kerberos Config File">/etc/krb5.conf</property>
       <property name="Authentication Expiration">12 hours</property>
   </provider>


С помощью данной конфигурации аутентификация имени пользователя и пароля может быть активирована путем ссылки на провайдер в *nifi.properties*:

  ::
  
   nifi.security.user.login.identity.provider=kerberos-provider


.. csv-table:: Описание настроек Login Identity Provider для Kerberos
   :header: "Свойство", "Описание"
   :widths: 50, 50

   "Default Realm", "Область по умолчанию для предоставления пользователю в случае, если пользователь вводит неполный пользовательский принципал (например, NIFI.APACHE.ORG)"
   "Kerberos Config File", "Абсолютный путь к файлу конфигурации клиента Kerberos"
   "Authentication Expiration", "Продолжительность действия проверки подлинности пользователя. Если пользователь никогда не выходит из системы, он должен будет снова войти в систему в течение указанного времени"

Описание настройки допуска по единому входу через клиентские тикеты Kerberos приведено в `Kerberos Service <>`_.


OpenId Connect
---------------

Для включения аутентификации через `OpenId Connect <http://openid.net/connect>`_ необходимо настроить свойства в *nifi.properties*, представленные далее в таблице.


.. csv-table:: Описание настроек для аутентификации через OpenId Connect
   :header: "Свойство", "Описание"
   :widths: 50, 50

   "nifi.security.user.oidc.discovery.url", "URL-адрес обнаружения необходимого OpenId Connect Provider (http://openid.net/specs/openid-connect-discovery-1_0.html)"
   "nifi.security.user.oidc.connect.timeout", "Время ожидания соединения при обмене данными с OpenId Connect Provider"
   "nifi.security.user.oidc.read.timeout", "Время ожидания чтения при обмене данными с OpenId Connect Provider"
   "nifi.security.user.oidc.client.id", "Идентификатор клиента для NiFi после регистрации в OpenId Connect Provider"
   "nifi.security.user.oidc.client.secret", "Секрет клиента для NiFi после регистрации в OpenId Connect Provider"
   "nifi.security.user.oidc.preferred.jwsalgorithm", "Предпочтительный алгоритм проверки токенов идентификации. Если значение свойства пустое, по умолчанию используется *RS 256*, поддерживаемое OpenId Connect Provider в соответствии со спецификацией. Если значение равно *HS256*, *HS384* или *HS512*, NiFi пытается проверить защищенные токены HMAC, используя указанный секретный ключ. Если значение свойства равно *none*, NiFi пытается проверить незащищенные/простые токены. Иные значения для алгоритма анализируются как алгоритм RSA или EC, который используется совместно с JSON Web Key (JWK), предоставленным через jwks_uri в метаданных URL-обнаружения"


Apache Knox
------------

Для включения аутентификации через **Apache Knox** необходимо настроить свойства в *nifi.properties*, представленные далее в таблице.


.. csv-table:: Описание настроек для аутентификации через Apache Knox
   :header: "Свойство", "Описание"
   :widths: 50, 50

   "nifi.security.user.knox.url", "URL-адрес страницы входа в Apache Knox"
   "nifi.security.user.knox.publicKey", "Путь к открытому ключу Apache Knox для проверки подписей токенов аутентификации в HTTP Cookie"
   "nifi.security.user.knox.cookieName", "Имя файла HTTP Cookie, которое Apache Knox создает после успешного входа в систему"
   "nifi.security.user.knox.audiences", "(Опционально) Разделенный запятыми список разрешенных audiences. Если значение задано, audience должна присутствовать в списке. Аudience, заполненная токеном, может быть настроена в Knox"

