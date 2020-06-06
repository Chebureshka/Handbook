.. include:: ../../defs.rst

Akka
##################

.. contents:: 
    :local:

.. container:: left-col

    Akka is a toolkit for building highly concurrent, distributed, and resilient message-driven applications for Java and Scala.

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Официальная документация <https://doc.akka.io/docs/akka/current/typed/actors.html>`_
        
        `Interaction Patterns <https://doc.akka.io/docs/akka/current/typed/interaction-patterns.html#interaction-patterns>`_
        
        `Akka HTTP <https://doc.akka.io/docs/akka-http/current/introduction.html>`_


Актор
******
.. container:: left-col

    Предпологается (в соотвествии с акторной моделью), что является наименьшим юнитом межпоточного взаимодействия.


Правила взаимодействия
======================
.. container:: left-col

    The actor send rule: the send of the message to an actor happens before the receive of that message by the same actor.

    The actor subsequent processing rule: processing of one message happens before processing of the next message by the same actor.

        In layman’s terms this means that changes to internal fields of the actor are visible when the next message is processed by that actor. So fields in your actor need not be volatile or equivalent.


ActorRef
=========
.. container:: left-col

    Ссылка на актор. Актор может быть представлен не только лишь на текущем устройстве, но так же и на удаленном.

    Создается с помощью spawn (см ActorContext_) или Receptionist_.

    .. container:: code-markup

        > ``tell (!)``
            Отправка сообщения в актор.

        > ``narrow[T]``
            Сужает тип принимаемых сообщений актором. Так же есть схожий метод для поведений.



ActorSystem
-----------
.. container:: left-col

    Класс для базового актора в любой иерархии акторов. Является **ActorRef**, может иметь также парента.

    Akka создает 2 guardian актора до старта любого пользовательского актора ``(ActorSystem(..., "name"))``:

        .. container:: code-markup

            (akka://name/) 
                root guardian - базовый актор для созданного в системе актора, при завершении которого работа внутренних акторов прекращается.

            (akka://name/system)
                system guardian - Akka or other libraries built on top of Akka may create actors in the system namespace.

            (akka://name/user)
                user guardian - This is the top level actor that you provide to start all other actors in your application.

    .. container:: code-markup

        > ``ActorSystem(guardianBehavior, "name")``
            Create an **ActorSystem**


Receptionist
----------------
.. container:: left-col

    Предоставляет способ получения ссылки на актор (напр. из кластера) по определенному ключу (Set из акторов) из локального *Receptionist instance*, изменения в котором распределяются по всем нодам на кластере

        .. container:: code-markup

            > ``val sk = ServiceKey[MessageType]("id")``
                Создание **ServiceKey** для возможности получения по нему множества акторов 

                **MessageType** - тип сообщения для получения типизированного **ActorRef[MessageType]** в сообщениях в акторы подписанных по данному ключу

            > ``context.system.receptionist ! Receptionist.Register(sk, context.self)``
                Регистрация актора ``context.self`` в *Receptionist instance* (напр. из базового актора (**ActorSystem**): ``context.system.receptionist``) по ключу ``sk``

                De-registration is implied by the end of the referenced Actor’s lifecycle

            > ``context.system.receptionist ! Receptionist.Subscribe(sk, context.self)``
                Подписка на любую новую (и всех уже зарегестрированных) регистрацию (в ``context.system.receptionist`` по ключу ``sk``) путем отправки сообщения **sk.Listing** в ``context.self``

                The subscription automatically ends with the termination of the subscriber

            > ``context.system.receptionist ! Receptionist.Find(sk, actor)``
                Получение сообщения только с текущими зарегестрированными акторами, и в отличии от **Subscribe** без подписки на дальнейшие регистрации


Configuration
-------------
.. container:: left-col

    Позволяет специфицировать конфигурацию **ActorSystem** от в отношении конфигурации по умолчанию. В момент создания **ActorSystem** можно передать *Config*

    .. tip::    
        ~ Если его не передовать - это эквивалентно передачи ``ConfigFactory.load()``, т.е. конфигурации приложения ``application.conf, application.json, application.properties`` найденная в корне *classpath* объедененные с ``reference.conf``.

        Код никогда не содержит значений по умолчанию, а вместо этого полагается на их наличие в файле ``reference.conf``, поставляемом с рассматриваемой библиотекой.

    If you are writing an Akka application, keep your configuration in ``application.conf`` at the root of the class path.




ActorContext
============
.. container:: left-col

    .. container:: code-markup

        > ``spawn(someBehavior, "someActor"): ActorRef``
            Создает в текущем контексте (акторе) новый именованный актор, реализующий переданное поведение. Ворзвращает **ActorRef** (ссылка на валидный URL актора).

        > ``spawnAnonymous(someBehavior): ActorRef``
            Создает так же как и spawn новый актор с переданным поведением, но уже без имени.

        > ``stop(childRef)``
            Останавлвает выбранный дочерний актор.

        > ``watch(ActorRef)``
          Регистрирует "наблюдаетля", оповещающего данный актор (``self``), об остановке любого переданного, не только дочернего.

          Опевещает передавая сигнал **Terminated**, в частности наследником **ChildFailed**, если дочерний актор упал с ошибкой.
          Если парент не обрабатывает **Terminated** сигнал - он бросает сам себе исключение **DeathPactException**, и падает.

        > ``watchWith(ActorRef[U], msg: T)``
          Регистрирует "наблюдателя", так же как и ``watch``, но вместо сигнала Terminated передает сообщение переданное в параметр

        > ``self: ActorRef``
          Представление текущего актора завязанного на данный контекст

        > ``messageAdapter(U => T): ActorRef[U]``
          Принимает функцию, возвращает актор, отправка сообщений которому переотправляется в текущий актор (``self``), с преобразованием типа сообщения через переданную функцию


        > ``ask(target: RecipientRef[Req], createRequest: ActorRef[Res] => Req)`` |br| |dl| ``(mapResponse: Try[Res] => T)`` |br| |dl| ``(implicit responseTimeout: Timeout, classTag: ClassTag[Res]): Unit``
            1. Адресуется запрос актору ``target``, а именно сообщение **Req**, полученное путем применения функции ``createRequest`` и передачи параметра **ActorRef[Res]**, являющегося адаптером к текущему.

            2. Если ответ был получен и передан в адаптер (**ActorRef[Res]**) соответсвутющее сообщение преобразуется в корректный тип сообщения для текущего актора применением функции ``mapResponse``.

            3. Если за указанное время ``responseTimeout`` ответ не получен то в ``mapResponse`` епредает **Failure**

            .. container:: code-markup

                > ``AskPattern.Askable:`` |br| |dl| ``ActorRef[Req].ask[Res](replyTo: ActorRef[Res] => Req)`` |br| |dl| |dl| ``(implicit timeout: Timeout, scheduler: Scheduler): Future[Res]``
                    Возвращает объект **Future** в котором ожидает ответ от актора которому адресуется (**ActorRef[Req]**) данный метод с сообщением Req, полученное через ``replyTo``.

                    В метод ``replyTo`` принимается актор (адаптер), при адресации сообщения которому **Future** комплитится.

        > ``pipeToSelf(future: Future[Value])(mapResult: Try[Value] => T): Unit``
            Адресует результат ``future`` к собственному актору через метод ``mapResult``

        > ``setReceiveTimeout``
            Отправляет сообщение в текущий актор, по истечению ``timeout'а``

        > ``system``
            Получение **ActorSystem**, являющегося базовым актором в иерархии текущего актора


Behavior
=========
.. container:: left-col

    Состояние актора, переменных и тд. Определяет сущности обработки приходящих сообщений, шаблон исполнения актора.

    Один и тот же объект поведения может быть передан в разные акторы, при этом исполнение поведения будет независимо друг от друга.

    .. rubric:: Инициализация через наследование

    .. container:: code-markup

        :< AbstractBehavior(context)
            .. container:: code-markup

                > ``onMessage(msg): Behavior``
                    Ресивер сообщения

                > ``onSignal: PartialFunction[Signal, Behavior]``
                    Ресивер сигнала

    .. rubric:: Статические поля, идентифицирующие поведения

    .. container:: code-markup

        $ ``Behaviors.stopped``
            Поведение остановленного актора. Исполнение актора прекращается.

        $ ``Behaviors.same``
            Идентифицирует текущее поведение

        $ ``Behaviors.empty``
            ...

        $ ``Behaviors.unhandled``
            ...

    .. rubric:: Инициализация через композицию

    .. container:: code-markup

        > ``Behaviors.receive { (context, message) => Behavior }: Behavior``
            Создает объект поведения в функциональном стиле. Работает так же, как и метод onMessage в **AbstractBehavior**. 

            Возвращающееся поведение является новым поведением текущего актора.

        > ``Behaviors.setup { context => Behavior }: Behavior``
            Создает объект поведения, выполняется перед стартом актора. Т.е. переданная функция сначала производит поведение от контекста, а потом в соответствии с ``spawn(receive(..), ..)`` передает его в актор.

            Может быть возвращено как новое поведение актора, при этом сразу выполнится переданная функция, вернет поведение, которое будет задавать поведение актора.

        > ``Behaviors.receiveMessage { message => Behavior }: Behavior``
            Тоже самое, что и ``Behaviors.receive``, но принимает только лишь ообщение.

        > ``Behaviors.supervise(wrapped: Behavior[T]): Supervise``
            Обарачивает объект поведения для перехвата исключения последующим вызовом метода ``onFailure``.

            .. note:: 
                Все дочерние акторы при остановке или возникновении ошибки (без явного указания соответствующей **SupervisorStrategy**) так же останавливаются. 

            В ``supervise`` можно передать блок ``Behaviors.setup``, инициализирующий начальное состояние каждый раз в соответствии с **SupervisorStrategy**:

                .. code-block:: java

                    Behaviors.supervise{
                        Behaviors.setup{
                            ..initActions.. 
                            Behaviors.receive{..}
                        }
                    } 
                    // каждый раз при перезапуске ..initActions.. будут выполняться заново, так как оно обернуто в поведение supervise

                    Behaviors.setup{
                        ..initActions.. 
                        Behaviors.supervise{
                            Behaviors.receive{..}
                        }
                    } 
                    // ..initActions.. выполняться однажды и в начале
                    // Вместе с блоком .. Behaviors.supervise{..}.onFailure(SupervisorStrategy.restart.withStopChildren(false)) 
                    //   - позволяет не перезапускать однажды уже созданные акторы

            .. container:: code-markup

                > ``Supervise.onFailure(strategy: SupervisorStrategy): Behavior``
                    Возвращает объект поведения, который ведет себя так же как и обернутый, но при возникновении необработанной исключительной ситуации ведет себя в соответствии с переданной стратегией.

        > ``Behaviors.withTimers(TimerScheduler => Behavior): Behavior``
            Добавляет возможность указания таймеров при определении поведения. Таймеры обеспечивают управления жизненным циклом актора, отправку сообщений по расписанию.

            .. container:: code-markup

                > ``TimerScheduler.startSingleTimer(key: Any, msg: T, delay: FiniteDuration)``
                    Start a timer that will send ``msg`` once to the ``self`` actor after the given ``delay``. 
                    Each timer has a key and if a new timer with same key is started the previous is cancelled.

                > ``TimerScheduler.cancel(key)``
                    Отменяет таймер по ключу key

        > ``SpawnProtocol.apply()``
            Возвращает поведение, определяющее актор, для которого это поведение применяется, как сущность добавляющую новый актор (``spawn``) в свой контекст, путем отправки сообщения Spawn

        > ``[Behaviors].receiveSignal(ctx, Signal => Behavior)``
            Создает поведение обрабатывающее сигналы, или же добавляет к существующему поведению обработку сигналов

            .. container:: code-markup

                ``Signal.PreRestart`` / ``Signal.PostStop``   
                    Используются для отчистки ресурсов, во время перезапуска актора, или его остановки соответственно

        > ``Behaviors.withStash(capacity: Int)(factory: StashBuffer[T] => Behavior[T]): Behavior[T]``
            Stash позволяет описывать буффер, в который будут поступать сообщения, не нуждающиеся в текущей обработке, но обработанные в дальнейшем

            If you try to stash more messages than the capacity a **StashOverflowException** will be thrown

            .. container:: code-markup

                > ``buffer.stash(message: T): StashBuffer[T]``
                    Добавить один элемент в конец stash буффера

                > ``buffer.unstashAll(behavior: Behavior[T]): Behavior[T]``
                    Возвращает обернутое поведение, обрабатывающее все накопившиеся сообщения в буффере
                    buffer становится пустым, в него так же можно продолжать добавлять новые сообщения через ``stash`` и через ``unstashAll``/``unstash`` их читать



Dispatchers
************
.. container:: left-col

    Определяют **ExecutionContext** сполняемых акторов. Может быть указан в методе ``spawn``, и следовательно может быть разным по всей иерархии акторов.

    .. note::
        По умолчанию **ForkJoinPool** (*event-based dipatcher* - акторы выполняются в соответствии с получением сообщений, один актор может быть выполнен на нескольких потоках в разное время)

        *Pinned Dispatcher* - для каждого актора прикреплен отдельный поток, в котором он выполняется ~

    Для блокирующих операций лучше использовать **Future** запускаемые в отдельных **Dispatcher**, а именно **ThreadPoolExecutor**

    Получение **Dispatcher** происходит через **DispatcherSelector**, или через **DispatcherSelector** + конфигурацию



Mailbox
********
.. container:: left-col

    Очередь сообщений приходящий к конкретному актору.

    Сообщение - сигнал, приходящий актору из-вне через метод ``tell``. 
    Для распределенных акторов отправка сообщения может не гарантироваться (*At-most-once delivery*)

    Сообщения отпраленные остановившимся акторам отправляются в "*dead letter mailbox*" which will forward them to the EventStream as **DeadLetters**.
    Если для актора специфицирован ограниченный **mailbox**, то сообщения не попавшие в него так же отправляются в **DeadLetters**.
    If an exception is thrown while a message is being processed, message will be lost, nothing happens to the mailbox. If the actor is restarted, the same mailbox will be there.

    Mailbox по умолчанию: *SingleConsumerOnlyUnboundedMailbox (Multiple-Producer Single-Consumer queue)*

    Специфицирование **Mailbox** происходит через MailboxSelector (напр. ``MailboxSelector.bounded(100)``) или через конфигурационный файл (напр. ``MailboxSelector.fromConfig("my-app.my-special-mailbox")``).

    Создание актора с определенным **Mailbox** происходит в методе spawn: ``context.spawn(childBehavior, "from-config-mailbox-child", props)``

    Типы Mailbox:
        SingleConsumerOnlyUnboundedMailbox (default) 
            Multiple-Producer Single-Consumer queue

        UnboundedMailbox 
            java.util.concurrent.ConcurrentLinkedQueue

        NonBlockingBoundedMailbox 
            Very efficient Multiple-Producer Single-Consumer queue

        UnboundedControlAwareMailbox 
            java.util.concurrent.ConcurrentLinkedQueue

        UnboundedPriorityMailbox 
            java.util.concurrent.PriorityBlockingQueue

        UnboundedStablePriorityMailbox 
            java.util.concurrent.PriorityBlockingQueue wrapped in an akka.util.PriorityQueueStabilizer



Routes
**********
.. container:: left-col

    Позволяет независимо обрабатывать очередь сообщений несколькими акторами в паралель.

    Представляется конкретным поведением перенаправляющим сообщения в целевые акторы.


(I) Pool Router
===============
.. container:: left-col

    Представляется множеством одинаковых воркер-акторов, способных брать на себя работу посредством адресации сообщений в роутер

    .. code-block:: java

        val pool = Routers.pool(poolSize)(Behaviors.supervise(Behavior).onFailure[Exception](SupervisorStrategy.restart))
        //Создает поведение ответственное за диспетчиризацию сообщений в переданных (poolSize) повведениях (Behavior)
        // supervise опционально, но лучше, для отказа от отказов, а следовательно и самого роутера - ставить

    Поведение останавливается, когда последний воркер завершится


(II) Group Router
=================
.. container:: left-col

    Представляется множеством воркеров, получаемых из Receptionist_ по определенному ключу.

    That the receptionist is used also means that the set of routees is eventually consistent.

    .. code-block:: java

        Routers.group(serviceKey)
        // Создает поведение диспетчиризации сообщений путем переотправки их в зарегестрированные по данному ServiceKey акторы

Routing strategies
====================
.. container:: left-col

    Определяет, то каким образом (в каком порядке) будут адресоваться (каким акторам) получаемые сообщения роутером 

    Round Robin
        This is the default for pool routers as the pool of routees is expected to remain the same.

    Random
        This is the default for group routers as the group of routees is expected to change as nodes join and leave the cluster

    Consistent Hashing
        ...
