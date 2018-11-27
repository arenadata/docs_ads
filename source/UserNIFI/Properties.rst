Пользовательские свойства с использованием Expression Language
================================================================


Expression Language (язык выражений) **NiFi** можно использовать для отображения значений атрибутов FlowFile, сравнения их с другими значениями и управления ими при создании и настройке потоков данных. Сведения о языке выражений по ссылке `Expression Language Guide <https://nifi.apache.org/docs/nifi-docs/html/expression-language-guide.html>`_.

В дополнение к использованию атрибутов FlowFile, системных свойств и свойств среды с помощью Express Language также можно определить пользовательские свойства, что обеспечивает большую гибкость в управлении и обработке потоков данных. Также возможно создание пользовательских свойств для соединения, сервера и сервиса с целью упрощения настройки потока данных.

Свойства **NiFi** имеют приоритет разрешения, о котором следует знать при создании пользовательских свойств:

+ Атрибуты процессора;
+ Свойства FlowFile;
+ Атрибуты FlowFile;
+ Из реестра переменных:

  + Пользовательские свойства;
  + Системные свойства;
  + Переменные среды операционной системы.

.. important:: При создании пользовательских свойств необходимо убедиться, что каждое такое свойство содержит отдельное значение с целью предотвращения его переопределения существующими свойствами среды, системными свойствами или атрибутами FlowFile

Есть два способа использования и управления пользовательскими свойствами:

+ В интерфейсе NiFi через окно переменных "Variables";
+ По ссылке *nifi.properties*.


Окно переменных Variables
--------------------------

Переменные могут быть созданы и настроены в пользовательском интерфейсе **NiFi** в любом поле, поддерживающем Expression Language. **NiFi** автоматически подбирает новые или измененные переменные, созданные в UI.

Для перехода к диалоговому окну "Variables" необходимо кликнуть правой кнопкой мыши по пустому пространству рабочей области приложения и выбрать пункт контекстного меню "Variables" (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-open1>`).


.. _ADS_UserNIFI_Properties_Variables-open1:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-open1.*
   :align: center

   Переход к "Variables" с рабочей области NiFi


При выборе группы процессов переход к "Variables" также доступен из контекстного меню (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-open2>`).


.. _ADS_UserNIFI_Properties_Variables-open2:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-open2.*
   :align: center

   Переход к "Variables" через группу процессов


Диалоговое окно создания и настроек пользовательских свойств "Variables" представлено на рисунке (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables>`).


.. _ADS_UserNIFI_Properties_Variables:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables.*
   :align: center

   Диалоговое окно "Variables"


Создание переменной
^^^^^^^^^^^^^^^^^^^^^^

Для создания новой переменной необходимо в окне "Variables" нажать кнопку с символом "+" и в открывшейся форме ввести наименование (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-name>`).


.. _ADS_UserNIFI_Properties_Variables-name:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-name.*
   :align: center

   Наименование новой переменной


Затем нажать кнопку "OK" и в новом окне ввести значение переменной (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-value>`).


.. _ADS_UserNIFI_Properties_Variables-value:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-value.*
   :align: center

   Значение новой переменной


После чего в экранной форме "Variables" нажать кнопку "Apply", результатом действия которой является иформационное окно по обновлению/созданию переменной (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-apply>`).


.. _ADS_UserNIFI_Properties_Variables-apply:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-apply.*
   :align: center

   Обновление переменной


При этом запускается процесс обновления переменной (идентификация затронутых компонентов, остановка затронутых процессоров и т.д.). Например, в разделе "Referencing Processors" теперь отображается процессор "PutFile-Root". Выбор имени процессора в списке приводит к переходу к указанному процессору на рабочей области. А при просмотре свойств процессора свойство *Directory* ссылается на созданную переменную *${putfile_dir}* (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-property>`).


.. _ADS_UserNIFI_Properties_Variables-property:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-property.*
   :align: center

   Переменная в свойствах процессора



Область действия переменной
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Область действия переменных определяется группой процессов, в которой они заданы, и доступны любому Процессору, определенному на данном уровне и ниже (то есть любому наследованному Процессору).

При этом переменные в наследованной группе переопределяют значения в родительской группе. В частности, если переменная задана в группе *root*, а также получает иное значение внутри группы процессов, то в таком случае компоненты внутри группы процессов используют значение, определенное непосредственно в самой группе процессов.

Например, переменная *putfile_dir* существует в группе процессов *root*, и в то же время создается другая переменная *putfile_dir* в группе процессов *A*. В таком случае, если один из компонентов в группе процессов *A* ссылается на переменную *putfile_dir*, то указываются обе переменные, но *putfile_dir* из группы *root* при этом перечеркнута, так как она переопределена (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-override>`).


.. _ADS_UserNIFI_Properties_Variables-override:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-override.*
   :align: center

   Переопределение переменной


Значение переменной может быть изменено только в группе процессов, в которой она создана (данная группа указывается в верхней части окна "Variables"). Для изменения переменной, определенной в другой группе процессов, необходимо выбрать значок стрелки в строке интересующей переменной (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-modify>`).


.. _ADS_UserNIFI_Properties_Variables-modify:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-modify.*
   :align: center

   Изменение переменной


При этом происходит переход к экранной форме "Variables" группы процессов, создавшей переменную (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-navigate>`).


.. _ADS_UserNIFI_Properties_Variables-navigate:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-navigate.*
   :align: center

   Переход к группе процессов, создавшей переменную



Разрешения переменных
^^^^^^^^^^^^^^^^^^^^^^^

Разрешения переменных основаны исключительно на привилегиях, настроенных для соответствующей группы процессов. Например, если у пользователя нет доступа к просмотру группы процессов ("View a process group"), окно "Variables" для данной группы процессов не может быть открыто (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-permissions>`).


.. _ADS_UserNIFI_Properties_Variables-permissions:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-permissions.*
   :align: center

   Отсутствие доступа к экранной форме "Variables"


При наличии у пользователя прав доступа к просмотру группы процессов, но при этом отсутствии доступа к изменению настроек ("Modify the process group"), переменные так же можно только просматривать, но не изменять. 

Сведения об управлении привилегиями компонентов приведены в разделе `Политики доступа <https://docs.arenadata.io/ads/AdminNIFI/Policies.html#id3>`_ документа `Руководство администратора по работе с сервисом Nifi <https://docs.arenadata.io/ads/AdminNIFI/index.html>`_.



Controller Services
^^^^^^^^^^^^^^^^^^^^^

В экранной форме "Variables" также отображаются ссылки на контроллеры (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-Controller>`).


.. _ADS_UserNIFI_Properties_Variables-Controller:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-Controller.*
   :align: center

   Ссылка на Controller Services в "Variables"


При выборе контроллера происходит переход к экранной форме сервиса окна конфигурации (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-Controller-config>`).


.. _ADS_UserNIFI_Properties_Variables-Controller-config:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-Controller-config.*
   :align: center

   Окно конфигурации Controller Services


Ссылки на компоненты
^^^^^^^^^^^^^^^^^^^^^

В случаях, когда компоненту, ссылающемуся на переменную, не предоставлены права на просмотр или изменение, в окне "Variables" отображается UUID данного компонента (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-Unauthorized>`).


.. _ADS_UserNIFI_Properties_Variables-Unauthorized:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-Unauthorized.*
   :align: center

   Ссылка на UUID компонента в "Variables"


В приведенном примере свойство *property1* ссылается на процессор, в котором у пользователя *user1* нет прав доступа на просмотр (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-example>`).


.. _ADS_UserNIFI_Properties_Variables-example:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-example.*
   :align: center

   Пример отсутствия прав доступа пользователя к компоненту


Ссылка nifi.properties
------------------------

Пользовательские свойства могут быть созданы и настроены с использованием ссылки *nifi.properties*. Для этого необходимо определить один или несколько наборов пар ключ/значение и передать их системному администратору. После добавления новых пользовательских свойств важно убедиться, что поле *nifi.variable.registry.properties* в файле *nifi.properties* обновлено.

.. important:: Для активации обновлений необходимо перезапустить NiFi 

