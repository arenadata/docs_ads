Настройка пользователей и политик доступа
==========================================

В зависимости от характеристик настроенных параметров *UserGroupProvider* и *AccessPolicyProvider* пользователи, группы и политики могут конфигурироваться в пользовательском интерфейсе. Если расширения не настроены, то в пользовательском интерфейсе пользователи, группы и политики доступны только для чтения. Если сконфигурированный авторизатор не использует *UserGroupProvider* и *AccessPolicyProvider*, пользователи и политики могут быть или не быть видимыми и настраиваемыми в пользовательском интерфейсе на основе базовой реализации.

Далее в главе предполагается, что пользователи, группы и политики настраиваются в пользовательском интерфейсе, и описывается:

+ `Создание пользователей и групп`_
+ `Политики доступа`_
+ `Настройка политик доступа на основе конкретных примеров`_


Создание пользователей и групп
-------------------------------

В пользовательском интерфейсе необходимо выбрать "Users" в глобальном меню, при этом открывыается диалоговое окно для создания пользователей и групп и управления ими (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Users>`). Для создания пользователей и групп используется кнопка "Add User".


.. _ADS_AdminNIFI_Policies_Users:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Users.*
   :align: center

   Nifi Users


Для создания пользователя в новом открывшемся диалоговом окне необходимо выбрать "Individual" и ввести информацию "Identity", соответствующую методу аутентификации защиты инстанса **NiFi**. После чего нажать "ОК" (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Identity>`).


.. _ADS_AdminNIFI_Policies_Identity:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Identity.*
   :align: center

   Создание пользователя


Для создания группы в диалоговом окне следует выбрать "Group", ввести имя группы в поле "Identity" и отметить пользователей в "Members", которые необходимо включить в группу. После чего нажать "ОК" (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Group>`).


.. _ADS_AdminNIFI_Policies_Group:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Group.*
   :align: center

   Создание группы


Политики доступа
-----------------

Управление возможностями пользователей и групп **NiFi** осуществляется с помощью политик доступа. Существует два типа политик доступа, которые могут быть применены к ресурсу:

+ View (просмотр) -- если ресурсу назначается политика просмотра, то добавленные в эту политику пользователи и группы могут только видеть детали данного ресурса;

+ Modify (изменение) -- если ресурсу назначается политика изменения, то добавленные в эту политику пользователи и группы могут изменить конфигурацию данного ресурса.

Создавать и применять политики доступа можно как на глобальном уровне (`Global Access Policies`_), так и на уровне компонентов (`Component Level Access Policies`_).


Global Access Policies
^^^^^^^^^^^^^^^^^^^^^^^^

Политики глобального доступа управляют следующими полномочиями на уровне системы:

.. csv-table:: Global Access Policies
   :header: "Policy", "Privilege", "Global Menu Selection", "Resource Descriptor"
   :widths: 25, 25, 25, 25

   "view the UI", "Разрешение пользователям просматривать UI", "N/A", "/flow"
   "access the controller", "Позволяет пользователям просматривать/изменять контроллер, включая задачи отчетности, службы контроллеров и узлы в кластере", "Controller Settings", "/controller"
   "query provenance", "Позволяет пользователям отправлять Provenance Search и запрашивать Event Lineage", "Data Provenance", "/provenance"
   "access restricted components", "Позволяет пользователям создавать/изменять ограниченные компоненты при условии наличия других разрешений. Ограниченные компоненты могут указывать, какие конкретные разрешения требуются. Разрешения могут предоставляться для определенных ограничений или независимо от них. Если разрешение предоставляется независимо от ограничений, пользователь может создавать/изменять все ограниченные компоненты", "N/A", "/restricted-components"
   "access all policies", "Позволяет пользователям просматривать/изменять политики для всех компонентов", "Policies", "/policies"
   "access users/user groups", "Позволяет пользователям просматривать/изменять пользователей и группы пользователей", "Users", "/tenants"
   "retrieve site-to-site details", "Позволяет другим инстансам NiFi извлекать информацию site-to-site", "N/A", "/site-to-site"
   "view system diagnostics", "Позволяет пользователям просматривать системную диагностику", "Summary", "/system"
   "proxy user requests", "Позволяет прокси отправлять запросы от имени других пользователей", "N/A", "/proxy"
   "access counters", "Позволяет пользователям просматривать/изменять счетчики доступа", "Counters", "/counters"


Component Level Access Policies
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Политики доступа на уровне компонентов управляют следующими полномочиями на уровне компонентов:

.. csv-table:: Component Level Access Policies
   :header: "Policy", "Privilege", "Resource Descriptor & Action"
   :widths: 30, 30, 40

   "view the component", "Позволяет пользователям просматривать детали конфигурации компонентов", resource="/<component-type>/<component-UUID>" action="R"
   "modify the component", "Позволяет пользователям изменять детали конфигурации компонентов", resource="/<component-type>/<component-UUID>" action="W"
   "view provenance", "Позволяет пользователям просматривать события происхождения, созданные компонентом", resource="/provenance-data/<component-type>/<component-UUID>" action="R"
   "view the data", "Позволяет пользователям просматривать метаданные и содержимое компонента в очередях потока в исходящих соединениях и через события происхождения", resource="/data/<component-type>/<component-UUID>" action="R"
   "modify the data", "Позволяет пользователям очищать очереди потоков в исходящих соединениях и повторно отправлять через события происхождения", resource="/data/<component-type>/<component-UUID>" action="W"
   "view the policies", "Позволяет пользователям просматривать список пользователей, которые могут просматривать/изменять компонент", resource="/policies/<component-type>/<component-UUID>" action="R"
   "modify the policies", "Позволяет пользователям изменять список пользователей, которые могут просматривать/изменять компонент", resource="/policies/<component-type>/<component-UUID>" action="W"
   "receive data via site-to-site", "Позволяет порту получать данные из инстансов NiFi", resource="/data-transfer/input-ports/<port-UUID>" action="W"
   "send data via site-to-site", "Позволяет порту отправлять данные из инстансов NiFi", resource="/data-transfer/output-ports/<port-UUID>" action="W"

.. important:: Политики доступа можно применять ко всем типам компонентов, кроме соединений. Разрешения на соединения определяются по индивидуальным политикам доступа к исходному и целевому компонентам соединения, а так же по политике доступа группы процессов, содержащей компоненты. Более подробно это описано далее в примерах

.. important:: Для доступа к List Queue и Delete Queue для соединения пользователю требуются политики "view the data" и "modify the data" на компоненте. Так же все узлы в кластерной среде должны быть добавлены к этим политикам, так как запрос пользователя может быть реплицирован через любой узел в кластере


Настройка политик доступа на основе конкретных примеров
--------------------------------------------------------

Самый эффективный способ понять, как создавать и применять политики доступа, -- это пройтись по некоторым распространенным примерам. В приведенных далее сценариях *User1* является администратором, а *User2* -- недавно добавленным пользователем, которому предоставлен доступ только к пользовательскому интерфейсу. На рисунке в качестве отправных точек показаны два процессора в рабочей области: GenerateFlowFile и LogAttribute (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Examples>`).

.. _ADS_AdminNIFI_Policies_Examples:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Examples.*
   :align: center

   GenerateFlowFile и LogAttribute


*User1* может добавлять компоненты в поток данных, а так же перемещать, редактировать и подключать все процессоры. Детали и свойства процессоров и групп процессов root видны для *User1* (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_User1>`).

.. _ADS_AdminNIFI_Policies_User1:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_User1.*
   :align: center

   User1 (администратор)

*User2* не может добавлять компоненты в поток данных, а так же перемещать, редактировать и подключать компоненты. Детали и свойства процессоров и групп процессов root скрыты от *User2* (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_User2>`).

.. _ADS_AdminNIFI_Policies_User2:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_User2.*
   :align: center

   User2 (недавно добавленный пользователь)


Перемещение процессора
^^^^^^^^^^^^^^^^^^^^^^^

*User1* необходимо выполнить следующие шаги для выдачи разрешения пользователю *User2* на перемещение процессора GenerateFlowFile в потоке данных с сохранением привилегий у *User1*:

1. Выбрать процессор GenerateFlowFile.

2. Нажать значок "Access Policies" на панели управления "Operate". При этом открывается диалоговое окно "Access Policies".

3. Выбрать "modify the component" в раскрывающемся списке политики. Политика "modify the component", которая в настоящее время существует на процессоре (дочернем), является унаследованной от группы процессов root (родительской), на которой *User1* имеет привилегии (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Modify>`).

.. _ADS_AdminNIFI_Policies_Modify:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Modify.*
   :align: center

   Modify the component


4. Нажать ссылку "Override". При замещении политики необходимо выбрать ее переопределение либо на копию унаследованной политики, либо на пустую политику. Для создания копии следует в диалоговом окне "Override Policy" выбрать "Copy" и нажать кнопку "Override" (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Override>`).

.. _ADS_AdminNIFI_Policies_Override:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Override.*
   :align: center

   Override Policy


5. В созданной политике выбрать значок "Add User". В поле "User Identity" ввести вручную или найти в списке *User2* и нажать "OK" (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Modify-add>`).

.. _ADS_AdminNIFI_Policies_Modify-add:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Modify-add.*
   :align: center

   Добавление User2 в политику
   

С такими изменениями *User1* сохраняет возможность перемещения обоих процессоров в рабочей области. А *User2* теперь может перемещать процессор GenerateFlowFile (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Result>`).

.. _ADS_AdminNIFI_Policies_Result:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Result.*
   :align: center

   Результат действий


Изменение процессора
^^^^^^^^^^^^^^^^^^^^^^

В приведенном примере "Перемещение процессора" *User2* добавлен в политику "modify the component" для процессора GenerateFlowFile. Но без возможности просмотра свойств процессора *User2* не может изменять его конфигурацию -- чтобы отредактировать компонент, пользователь должен быть также включен в политику "view the component". 

*User1* необходимо выполнить следующие шаги для реализации возможности изменения конфигурации процессора пользователю *User2*:

1. Выбрать процессор GenerateFlowFile.

2. Нажать значок "Access Policies" на панели управления "Operate". При этом открывается диалоговое окно "Access Policies".

3. Выбрать "view the component" в раскрывающемся списке политики. Политика "view the component", которая в настоящее время существует на процессоре (дочернем), является унаследованной от группы процессов root (родительской), на которой *User1* имеет привилегии (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_View>`).

.. _ADS_AdminNIFI_Policies_View:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_View.*
   :align: center

   View the component


4. Нажать ссылку "Override" и в открывшемся диалоговом окне, сохранив политику копирования по умолчанию, нажать кнопку "Override".

5. В созданной политике выбрать значок "Add User". В поле "User Identity" ввести вручную или найти в списке *User2* и нажать "OK" (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_View-add>`).

.. _ADS_AdminNIFI_Policies_View-add:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_View-add.*
   :align: center

   Добавление User2 в политику 


С такими изменениями *User1* сохраняет возможность просмотра и редактирования процессоров в рабочей области. А *User2* теперь может просматривать и редактировать процессор GenerateFlowFile (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Result-view>`).

.. _ADS_AdminNIFI_Policies_Result-view:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Result-view.*
   :align: center

   Результат действий 


Создание подключения
^^^^^^^^^^^^^^^^^^^^^^

При настройке политик так, как описано в предыдущих двух примерах, *User1* может подключить GenerateFlowFile к LogAttribute (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Connect>`).

.. _ADS_AdminNIFI_Policies_Connect:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Connect.*
   :align: center

   User1 -- подключение процессоров 


При этом *User2* не имеет права доступа на установку соединения процессоров (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_No-connect>`).

.. _ADS_AdminNIFI_Policies_No-connect:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_No-connect.*
   :align: center

   User2 -- невозможность подключения процессоров 


Это объясняется тем, что:

+ *User2* не имеет доступа к изменениям в группе процессов;
+ Несмотря на то, что *User2* имеет право на просмотр и изменение исходного компонента (GenerateFlowFile), *User2* не имеет политики доступа к целевому компоненту (LogAttribute).

*User1* необходимо выполнить следующие шаги для реализации возможности подключения GenerateFlowFile к LogAttribute пользователю *User2*:

1. Выбрать группу процессов root, при этом панель управления "Operate" обновляется с подробными сведениями.

2. Выбрать значок "Access Policies" на панели управления "Operate". При этом открывается диалоговое окно "Access Policies".

3. В диалоговом окне в раскрывающемся списке политики выбрать "modify the component" (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Connect-modify>`).

.. _ADS_AdminNIFI_Policies_Connect-modify:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Connect-modify.*
   :align: center

   Modify the component 


4. Выбрать значок "Add User". В поле "User Identity" ввести вручную или найти в списке *User2* и нажать "OK" (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Connect-add>`).

.. _ADS_AdminNIFI_Policies_Connect-add:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Connect-add.*
   :align: center

   Добавление User2 в политику группы 


Добавляя *User2* в политику "modify the component" группы процессов, *User2* так же добавляется к политике "modify the component" в процессоре LogAttribute путем наследования. Чтобы проверить это, необходимо в рабочей области выделить процессор LogAttribute и выбрать значок "Access Policies" на панели управления "Operate". При этом открывается диалоговое окно политик доступа процессора LogAttribute с наличием пользователя *User2* в политике "modify the component" (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Connect-confirm>`).

.. _ADS_AdminNIFI_Policies_Connect-confirm:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Connect-confirm.*
   :align: center

   Проверка наличия политики User2


С такими изменениями *User2* теперь может подключать процессор GenerateFlowFile к процессору LogAttribute (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Connect-result-1>`, :numref:`Рис.%s.<ADS_AdminNIFI_Policies_Connect-result-2>`).

.. _ADS_AdminNIFI_Policies_Connect-result-1:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Connect-result-1.*
   :align: center

   User2 -- подключение процессоров


.. _ADS_AdminNIFI_Policies_Connect-result-2:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Connect-result-2.*
   :align: center

   User2 -- подключение процессоров



Изменение соединения
^^^^^^^^^^^^^^^^^^^^^^^

В следующем сценарии *User1* и *User2* добавляют процессор ReplaceText в группу процессов root (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_ReplaceText>`).

.. _ADS_AdminNIFI_Policies_ReplaceText:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_ReplaceText.*
   :align: center

   Добавление процессора ReplaceText


*User1* может выбрать и изменить существующее соединение между GenerateFlowFile и LogAttribute, чтобы подключить GenerateFlowFile к ReplaceText (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_EditConnect-user1>`).

.. _ADS_AdminNIFI_Policies_EditConnect-user1:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_EditConnect-user1.*
   :align: center

   User1 -- изменение соединения


При этом *User2* не имеет возможности выполнить такое действие (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_EditConnect-user2>`).

.. _ADS_AdminNIFI_Policies_EditConnect-user2:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_EditConnect-user2.*
   :align: center

   User2 недоступно изменение соединения


*User1* необходимо выполнить следующие шаги для реализации возможности подключения GenerateFlowFile к ReplaceText пользователю *User2*:

1. Выбрать группу процессов root, при этом панель управления "Operate" обновляется с подробными сведениями.

2. Выбрать значок "Access Policies" на панели управления "Operate". При этом открывается диалоговое окно "Access Policies".

3. В диалоговом окне в раскрывающемся списке политики выбрать "view the component" (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Connect-view>`).

.. _ADS_AdminNIFI_Policies_Connect-view:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Connect-view.*
   :align: center

   View the component 


4. Выбрать значок "Add User". В поле "User Identity" ввести вручную или найти в списке *User2* и нажать "OK" (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_Connect-view-add>`).

.. _ADS_AdminNIFI_Policies_Connect-view-add:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_Connect-view-add.*
   :align: center

   Добавление User2 в политику группы


Будучи добавленным к политикам просмотра и изменения для группы процессов *User2* теперь может подключать процессор GenerateFlowFile к процессору ReplaceText (:numref:`Рис.%s.<ADS_AdminNIFI_Policies_EditConnect-user2-result>`).

.. _ADS_AdminNIFI_Policies_EditConnect-user2-result:

.. figure:: ../imgs/ADS_AdminNIFI_Policies_EditConnect-user2-result.*
   :align: center

   User2 -- изменение соединения



