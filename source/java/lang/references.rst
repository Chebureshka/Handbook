.. include:: ../../defs.rst

References
************************************

.. container:: left-col

    .. rubric:: Reference

    .. container:: code-markup

        T ``get()``
            Получает ссылаемый объект или null, при отсутствии оного.

        void ``clear()``
            Обнулить ссылку. Но не вызов gc на нем.

            This method is invoked only by Java code; when the garbage collector clears references it does so directly, without invoking this method.

        static void ``reachabilityFence​(Object ref)``
            Ensures that the object referenced by the given reference remains strongly reachable, regardless of any prior actions of the program that might otherwise cause the object to become unreachable. 
            (Устанавливает порядок для strong reachability в отношении сбора мусора).

    .. rubric:: ReferenceQueue

    Reference queues, to which registered reference objects are appended by the garbage collector after the appropriate reachability changes are detected (обнаружены соответствующие изменения достижимости).

    .. container:: code-markup

        Reference<? extends T> ``poll()``
            Polls this queue to see if a reference object is available.

        Reference<? extends T> ``remove()``
            Removes the next reference object in this queue, blocking until one becomes available.

        Reference<? extends T> ``remove​(long timeout)``
            Removes the next reference object in this queue, blocking until either one becomes available or the given timeout period expires.


.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Package java.lang.ref <https://docs.oracle.com/javase/10/docs/api/java/lang/ref/package-summary.html>`_

        `Class Reference<T> <https://docs.oracle.com/javase/10/docs/api/java/lang/ref/Reference.html>`_

        `Class ReferenceQueue <https://docs.oracle.com/javase/10/docs/api/java/lang/ref/ReferenceQueue.html>`_

Виды ссылок
============================================
.. container:: left-col

    .. container:: code-markup

        strong reference
            В Java объекты, создаваемые через оператор new создаются по strong ссылке.
            Уничтожаются при отсутствии strong ссылок.

        SoftReference
            Объекты, созданные через **SoftReference**, будут собраны в случае, если JVM требует память.
            То есть имеется гарантия, что все soft reference объекты будут собраны перед тем, как JVM выбросит **OutOfMemoryError**.
    
            **SoftReference** часто используется для кешей, потребляющих большое количество памяти.

            Алгоритм работы, при отчистке ссылки сборщиком мусора:

                * Атомарно чистятся все **SoftReference** на этот объект и все **SoftReference** на любые другие *softly-reachable* объекты, 
                  из которых этот объект доступен через цепочку **strong reference**.

                * В то же время данный объект объявляется *finalizable* и вызывается ``finalize()``.

                * В то же или в более позднее время отчищенные ссылки добавляются в **ReferenceQueue** (если **ReferenceQueue** указана в конструкторе)

        WeakReference
            **WeakReference** не спасает объект от финализации, даже если имеется достаточное количество свободной памяти.

            Как только на объект не останется **strong** и **soft** ссылок, он может быть финализирован.

            .. tip::
                Используется для кешей и для создания цепей связанных между собой объектов.

            Алгоритм работы, при отчистке ссылки сборщиком мусора:

                * Атомарно чистятся все **WeakReference** на этот объект и все **WeakReference** на любые другие *weakly-reachable* объекты, 
                  из которых этот объект доступен через цепочку **SoftReference** и **WeakReference**.

                * В то же время данный объект объявляется *finalizable* и вызывается ``finalize()``.

                * В то же или в более позднее время отчищенные ссылки добавляются в **ReferenceQueue** (если **ReferenceQueue** указана в конструкторе)

.. container:: left-col

    .. container:: code-markup

        **PhantomReference**
            Только если на объект не остаётся никаких ссылок, кроме **PhantomReference**, при ближайшей сборке их механизм вступает в действие:

                * Атомарно чистятся все **PhantomReference** на этот объект и все **PhantomReference** на любые другие *phantom-reachable* объекты, из которых этот объект доступен.

                * Во время ближайшей сборки мусора у объекта будет вызван метод ``finalize()``. Но, если метод ``finalize()`` не был переопределен, этот шаг пропускается, а выполнится сразу второй шаг.

                * Ссылка на объект (уже не **PhantomReference**) будет помещена в **ReferenceQueue** (указанную в конструкторе **ReferenceQueue**), из которой этот объект будет удален, когда у ссылки вызовут метод ``clear()``.
                  (В отличии от **SoftReference** или **WeakReference** – фантомная ссылка добавляется к своему **ReferenceQueue** перед тем как освободится память, т.е. объект не отчищается сборщиком мусора до вызова ``clear``)

                Фактически объект "умер" в Java мире, но не исчез, а остался в нем призраком – на него хранится ссылка в очереди призрачных объектов (до вызова ``clear``).

            Этот тип ссылок используется как альтернатива финализации (для более гибкого освобождения ресурсов).

            .. note:: 
                Объект из **PhantomReference** никогда не доступен, поэтому метод ``get()`` всегда возвращает ``null``.

.. container:: right-col

    .. code-block:: java

        // специальная очередь для призрачных объектов
        ReferenceQueue<Integer> queue = new ReferenceQueue<Integer>();

        // список призрачных ссылок
        ArrayList<PhantomReference<Integer>> list = new ArrayList<PhantomReference<Integer>>();

        // создается 10 объектов и далее добавляются в список через PhantomReference
        for (int i = 0; i < 10; i++) {
            Integer x = new Integer(i);
            list.add(new PhantomReference<Integer>(x, queue));
        }

        // вызывается сборщик мусора
        // он должен убить все «призрачно достижимые» объекты и поместить их в
        // ReferenceQueue
        System.gc();

        // достаются из очереди все объекты
        Reference<? extends Integer> referenceFromQueue;
        while ((referenceFromQueue = queue.poll()) != null) {
            System.out.println(referenceFromQueue.get());
            // ссылка отчищается
            referenceFromQueue.clear();
        }


Cleaner
============================================
.. container:: left-col

    |br|

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        `Class Cleaner <https://docs.oracle.com/javase/10/docs/api/java/lang/ref/Cleaner.html>`_

        