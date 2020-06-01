.. include:: ../../defs.rst

Collections 
********************************

sort 
    Делегирует вызов интерфейсу List, и далее массиву (при реализации по умолчанию)

reverse / rotate 
    Переставляет элементы в переданном List

binarySearch 
    Поиск элемента

shuffle 
    Случайное пересталвение всех лементов

swap 
    Перестановка двух элементов

copy 
    Копирует элементы из одного List в другой List

min/max 
    Характерная свертка

replaceAll 
    Заменить все вхождения переданного элемента

indexOfSubList 
    Индекс из первого массива, начиная с которого полностью повторяется второй

disjoint 
    Истина, если нет общих элементов в коллекции 

addAll 
    Добавить указанные элементы в коллекцию

reverseOrder 
    Компоратор, сравнивающий Comparable элементы, в иным результатом


Создание специальных коллекций
==============================

singleton 
    Возвращает иммутабельное множество с единственным элементом (SingletonSet)

singletonList / singletonMap
    Возвращает иммутабельную коллекцию с единственным элементом

nCopies 
    Возвращает иммутабельный List из указанного количества переданных элементов



emptyIterator / emptyListIterator 
    Возвращает синглтон пустого итератора

emptySet / emptySortedSet / emptyNavigableSet / emptyList / emptyMap / emptySortedMap / emptyNavigableMap
	Возвращает синглтон пустой коллекции


Декарирование коллекций
-----------------------

unmodifiableCollection / unmodifiableSet / unmodifiableSortedSet / unmodifiableNavigableSet / unmodifiableList / unmodifiableMap / unmodifiableSortedMap / unmodifiableNavigableMap
	Декарирует коллекцию, как неизменяемою

synchronizedCollection / synchronizedSet / synchronizedSortedSet / synchronizedNavigableSet / synchronizedList / synchronizedMap / synchronizedSortedMap / synchronizedNavigableMap
	Декарирует коллекцию, как синхронизированную

checkedCollection / checkedQueue / checkedSet / checkedNavigableSet / checkedList / checkedMap / checkedSortedMap / checkedNavigableMap
	Декарирует переданную коллекцию, как коллекцию, в которую можно добавлять элементы строго определенного типа