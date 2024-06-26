# Отчет

ФИО: Ермолаева Елена Александровна

Группа: БПИ224

Вариант: 5

## Условие 

Задача о читателях и писателях. Базу данных, представленную массивом целых положительных чисел, разделяют два типа
процессов: N читателей и K писателей. Читатели периодически
просматривают случайные записи базы данных и выводя номер
свой номер (например, PID), индекс записи, ее значение, а также
вычисленное значение числа Фибоначчи. Писатели изменяют случайные записи на случайное число и также выводят информацию о
своем номере, индексе записи, старом значении и новом значении.
Предполагается, что в начале БД находится в непротиворечивом
состоянии (все числа отсортированы, например, по возрастанию).
Каждая отдельная новая запись переводит БД из одного непротиворечивого состояния в другое (то есть, новая сортировка может
поменять индексы записей или переставить числа). Для предотвращения взаимного влияния транзакций процесс–писатель должен
иметь исключительный доступ к БД. Если к БД не обращается
ни один из процессов–писателей, то выполнять транзакции могут
одновременно сколько угодно читателей.

Создать многопроцессное приложение с потоками-писателями
и потоками-читателями.


## 4 - 5 баллов

В сценарии задачи процессы-писатели и процессы-читатели 
взаимодействуют с общим массивом данных через разделяемую память.
Этот метод использует два __именованных семафора__ (`r` и `w`) для контроля доступа к данным и
два __дополнительных именованных семафора__ для синхронизации счетчиков активных 
читателей и писателей. Когда писатель желает записать, он блокирует доступ новым читателям, 
а уже работающие читатели могут завершить чтение. Это предотвращает 
новые операции чтения, пока писатели не выполнены, давая писателям приоритет.

Так же используется обработка сигналов, для обеспечения удаления семафоров и разделяемой
памяти по ее завершению любым из способов.

### Тесты 

Без прерывания:


![img_1.png](img/img_1.png)

С прерыванием:

![img.png](img/img.png)

![img.png](img/img.png)

С прерыванием:

![img_1.png](img/img_1.png)


![img.png](img/img.png)

С прерыванием:

![img_1.png](img/img_1.png)

## 6 - 7 баллов

Доступ к массиву регулируется через __неименованные POSIX семафоры__, 
которые также находятся в разделяемой памяти. 
Семафор `w` контролирует доступ к массиву для писателей, а `r` -- для читателей,
блокируя его при активной записи. `mutex1` используется для управления счетчиком активных
читателей, увеличивая его при входе читателя и уменьшая при выходе. 
Если первый читатель входит, он блокирует `w`, предотвращая запись писателями. 
`mutex2` помогает контролировать количество писателей, готовящихся к записи, и блокирует `r`,
если писатель первым начинает операцию, чтобы предотвратить доступ новых читателей. 
Это обеспечивает, что писатели имеют приоритет, в то время как читатели могут свободно читать,
если писатели не активны.

В случае прерываний(SIGINT) обработчик сигналов освобождает ресурсы.

Без прерывания:

![img_3.png](img/img_3.png)

С прерыванием:

![img_2.png](img/img_2.png)

![img_2.png](img/img_2.png)

С прерыванием:

![img_3.png](img/img_3.png)




