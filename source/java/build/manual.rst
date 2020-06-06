.. include:: ../../defs.rst

Мануальная сборка
************************************
.. container:: left-col

    .. code-block:: bash

        > jar {ctxui}[vfm0Me] [jar-file] [manifest-file] [entry-point] [-C dir] files ...

        > jar cf jar-file input-file(s)

        > jar cmf jar-file existing-manifest input-file(s)
            # The c option indicates that you want to create a JAR file
            # The f option indicates that you want the output to go to a file rather than to stdout
            # The input-file(s) argument can contain the wildcard * symbol. 
            #     If any of the "input-files" are directories, the contents of those directories are added to the JAR archive recursively

        > javac

        > java

.. container:: right-col

    .. container:: links-block

        .. rubric:: Ссылки:

        https://www.codejava.net/java-core/tools/using-jar-command-examples