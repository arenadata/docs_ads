Использование пользовательских свойств с помощью Expression Language
=======================================================================


Expression Language (язык выражений) **NiFi** можно использовать для отображения значений атрибутов FlowFile, сравнения их с другими значениями и управления ими при создании и настройке потоков данных. Дополнительные сведения о языке выражений по ссылке `Expression Language Guide <https://nifi.apache.org/docs/nifi-docs/html/expression-language-guide.html>`_.

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

Существует два способа использования и управления пользовательскими свойствами:

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

   Переход к "Variables" через группы процессов


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


При этом запускается процесс обновления переменной (идентификация затронутых компонентов, остановка затронутых процессоров и т.д.). Например, в разделе "Referencing Processors" теперь отображается процессор "PutFile-Root". Выбор имени процессора в списке приводит к переходу к указанному процессору на рабочей области. При просмотре свойств процессора *${putfile_dir}* ссылается на свойство *Directory* (:numref:`Рис.%s.<ADS_UserNIFI_Properties_Variables-property>`).


.. _ADS_UserNIFI_Properties_Variables-property:

.. figure:: ../imgs/ADS_UserNIFI_Properties_Variables-property.*
   :align: center

   Просмотр свойств процессора








