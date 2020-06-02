.. include:: ../../defs.rst

kotlinx.coroutines
##################

Green threads are threads that are scheduled by a runtime library or virtual machine (VM) instead of natively by the underlying operating system (OS). 

Green threads emulate multithreaded environments without relying on any native OS abilities, and they are managed in user space instead of kernel space, 
enabling them to work in environments that do not have native thread support.


.. raw:: html

    <iframe width="560" height="315" src="https://www.youtube.com/embed/HYhJmK9nKS4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

.. raw:: html

    <iframe width="560" height="315" src="https://www.youtube.com/embed/fd9EVSxINKw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

|br|


Базовые понятия
******************
Блокирующие операции
    Останавливают текущее выполнение потоков или корутин(превязанному к корутине потоку), до того, как некоторое условие будет достигнуто. 
    При этом поток переходит в режим ожидания и не выполняет никакую полезную работу.

Suspend функции
    Приостанавливают выполнение корутины, при этом текущий поток начинает выполнять другую корутину.
    Специфицирование функции модификатором suspend позволяет приостанавливать выполнение корутины, при исполнении данной функции, а следовательно вызывать другие suspend функции


CoroutineScope 
**************
Определяет область для создания новых корутин и управления ними.

Каждый Coroutine Builder добавляет инстанс CoroutineScope в область исполнения корутин (Structured concurrency).
При этом есть возможность управления всеми созданными корутинами.

``object GlobalScope : CoroutineScope``
    Область, ограниченная временем работы приложения. Корутины созданные в данной области, как и потоки-демоны, выполняются, пока приложение запущенно.


Coroutine Builders
******************

``fun <T> runBlocking(CoroutineContext, suspend CoroutineScope.() -> T): T``
    Блокирует выполнение текущего потока/корутины до завершения всех корутин (создаваемых при вызове других Coroutine Builders) в некотором CoroutineScope, определенном в области исполнения переданной корутины. 
    
    Служит для создания корутин и выполнения suspend функций (приостанавливающих текущее выполнение корутин) из вне CoroutineScope.

``suspend fun <R> coroutineScope(suspend CoroutineScope.() -> R): R``
    Позволяет определять CoroutineScope, так же как и в runBlocking, но при этом поток не блокируется, а исполняющаяся корутина приостанавливается до окончания исполнения всех созданных корутин в CoroutineScope.

``fun CoroutineScope.launch(CoroutineContext, CoroutineStart, suspend CoroutineScope.() -> Unit): Job``
    Запускает паралельное исполнение переданной корутины в указанном CoroutineScope и возвращает управление текущему потоку/корутине.

    Возвращает объект Job, через который можно дождаться (приостановив текущую корутину - suspend) окончания выполнения корутины.


Стандартные suspend функции
***************************

``delay``

``repeat``



Links
********************************

`Документация kotlin <https://kotlinlang.org/docs/reference/coroutines/coroutines-guide.html>`_

