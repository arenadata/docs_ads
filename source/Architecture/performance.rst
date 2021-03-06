Производительность
===================

Специалисты **Arenadata** приложили значительные усилия для повышения производительности платформы. Одним из основных вариантов использования является обработка данных веб-активности, которая очень велика: каждый просмотр страницы может генерировать десятки записей. Кроме того, каждое опубликованное сообщение читается как минимум одним потребителем (а часто -- многими), поэтому есть стремление сделать потребление как можно более дешевым.

По опыту разработки и эксплуатации ряда аналогичных систем выяснилось, что производительность является ключом к эффективным многопользовательским операциям. Если нижестоящий сервис инфраструктуры может легко стать узким местом из-за незначительного увеличения по использованию приложения, то такие небольшие изменения часто создают проблемы. В результате приложение выйдет из строя под нагрузкой перед инфраструктурой. Это особенно важно при попытке запуска централизованного сервиса, поддерживающего десятки или сотни приложений в централизованном кластере, поскольку изменения в шаблонах использования происходят почти ежедневно.

После устранения неудачных шаблонов доступа к диску остается две общие причины неэффективности в подобном типе системы: слишком много мелких операций ввода-вывода и избыточное копирование байтов.

Проблема множества мелких операций ввода-вывода происходит как между клиентом и сервером, так и в собственных персистентных операциях сервера. Во избежание этого протокол **Arenadata** построен вокруг абстракции "набор сообщений", группирующей сообщения. Это позволяет сетевым запросам объединять сообщения вместе, амортизировав накладные расходы в сети, а не отправлять каждый раз по одному сообщению. Сервер единоразово добавляет фрагменты сообщений в свой журнал, а потребитель извлекает большие линейные фрагменты за раз.

Такая простая оптимизация на порядок увеличивает скорость работы. Пакетирование приводит к увеличению сетевых пакетов, последовательных операций с дисками и смежными блоками памяти и т.д., что позволяет платформе **ADS** превращать поток случайных сообщений в линейные записи, которые поступают потребителям.

Другая непродуктивность заключается в копировании байтов. При низких скоростях передачи сообщений это не проблема, но под нагрузкой влияние значительно. Во избежание этого **ADS** использует стандартный бинарный формат сообщений, который совместно применяется поставщиком, брокером и потребителем (таким образом, блоки данных могут передаваться без изменений).

Журнал сообщений, поддерживаемый брокером, сам по себе является просто каталогом файлов, каждый из которых заполняется рядом наборов сообщений, которые были записаны на диск в том же формате, который используется поставщиком и потребителем. Поддержка общего формата позволяет оптимизировать наиболее важную операцию -- сетевую передачу персистентных блоков журнала. Современные операционные системы **unix** предлагают высоко оптимизированный путь кода для передачи данных из pagecache в сокет; в **Linux** это делается с помощью системного вызова *sendfile*.

Путь передачи данных из файла в сокет заключается в следующем:

1. Операционная система считывает данные с диска в pagecache в пространстве ядра.
2. Приложение считывает данные из пространства ядра в буфер пространства пользователя.
3. Приложение записывает данные в пространство ядра в буфер сокета.
4. Операционная система копирует данные из буфера сокета в буфер сетевого адаптера по сети.

Такой метод явно неэффективен -- он предполагает четыре операции копирования и два системных вызова. А при использовании *sendfile* повторное копирование исключается (zero-copy), позволяя ОС напрямую отправлять данные из pagecache в сеть. Таким образом, из оптимизированного пути требуется только последняя копия в буфер сетевого адаптера.

Как правило, общий вариант использования состоит из нескольких потребителей топика. При приведенной выше оптимизации данные копируются в pagecache только один раз и при каждом потреблении используются повторно, а не хранятся в памяти и копируются в пространство пользователя при каждом чтении. Это позволяет считывать сообщения со скоростью, приближенной к пределу сетевого подключения.

Такая комбинация использования pagecache и *sendfile* означает, что в кластере **ADS** нет активности чтения на дисках, поскольку данные полностью обслуживаются из кэша.

Дополнительные сведения о поддержке *sendfile* и zero-copy в **Java** приведены в `статье <http://www.ibm.com/developerworks/linux/library/j-zerocopy>`_.
