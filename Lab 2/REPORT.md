#№ Отчет по лабораторной работе №2
## по курсу "Логическое программирование"

## Решение логических задач

### студент: Калугин К.А.

## Результат проверки

| Преподаватель     | Дата         |  Оценка       |
|-------------------|--------------|---------------|
| Сошников Д.В. |              |               |
| Левинская М.А.|              |        3-     |

> *Комментарии проверяющих (обратите внимание, что более подробные комментарии возможны непосредственно в репозитории по тексту программы)*

## Введение
Большинство логических задач можно решить самому, при помощи лишь бумаги и ручки. Однако это требует сил и времени. Поэтому люди изобрели множество способов, упрощающих решение таких задач, в том числе и связанных с компьютерами или легко переводящихся на компьютерную логику - таблицы истинности, алгебра логики, логика предикатов и т.д. Prolog - это язык, который прекрасно подходит для решения логических задач, так как любая из них решается методом подбора и отсева всех вариантов. А Prolog работает именно по этому принципу.

## Задание
В одном городе живут 5 человек. Их имена Леонид, Михаил, Николай, Олег и Петр. Их фамилии Атаров, Бартенев, Кленов, Данилин и Иванов. Бартенев знаком только с двумя из перечисленных мужчин. Петр знаком со всеми, кроме одного. Леонид знаком только с одним из всех. Данилин и Михаил незнакомы. Николай и Иванов знают друг друга. Михаил, Николай и Олег знакомы между собой. Атаров незнаком только с одним из всех. Только один из всех знаком с Кленовым. Назовите имена и фамилии каждого. С кем знаком каждый из них?

## Принцип решения
Основным рабочим предикатом является ```sol(V, W, X, Y, Z)``` - именно его мы запускаем. Также используется другие важные предикаты:

1) ```rela(...)``` - предикат, описывающий дружбу нескольких людей.
```prolog
rela([]) :- fail.
rela([[N, S], [N1, S1]]) :- 
   pers(N, S), pers(N1, S1), 
   uniq([N, N1]),
   uniq([S, S1]).
rela([[N, S], [N1, S1], [N2, S2]]) :- 
   pers(N, S), pers(N1, S1), pers(N2, S2),
   uniq([N, N1, N2]), less(N1, N2),
   uniq([S, S1, S2]).
rela([[N, S], [N1, S1], [N2, S2], [N3, S3]]) :- 
   pers(N, S), pers(N1, S1), pers(N2, S2), pers(N3, S3),
   uniq([N, N1, N2, N3]), less(N1, N2), less(N1, N3), less(N2, N3),
   uniq([S, S1, S2, S3]).
```


2)```z(A)``` - предикат, проверяющий соответствие имени и фамилии.
```prolog
z([[_, _]]).
z([[N, S] | T]) :- 
   member([N, AS], T), not(AS = S), !, fail;
   member([AN, S], T), not(AN = N), !, fail;
   z(T).
```

Кроме того, активно используется такие стандартные предикаты, как ```member``` или ```length```.

## Выводы
Эта лабораторная научила меня тому, что программы в прологе, при отсутствии оптимизации могут работать ОЧЕНЬ долго, причем на скорость вычислений влияет даже порядок строк. Кроме того, очень часто не хватает возможностей для отсева массива решений, и как следствие, программа возвращает множество решений. Также серьезнейшей проблемой стало то, что Prolog, по причине декларативности, при ошибках в программе продолжает компилироваться и даже работает, причем зачастую довольно долго, возвращая в итоге совершенно неинформативное "false". Все это очень затрудняет дебаг программы. Для проверки работоспособности финальной, но еще не оптимизированной версии программы мне не хватило полутора часов, однако я оставил программу работать и оставил компьютер включенным. На поиск решения ушло три дня, однако оно было верным. Подводя итоги, написание программы в Prolog'е для решения логической задачи может быть не такой хорошей идеей, как это кажется на первый взгляд. Основной причиной является то, что Prolog ищет решения по совершенно неоптимальному пути, а следовательно существует немалая вероятность того, что время работы программы будет далеко за рамками приемлемых. Однако, стоит признать, что решение той же задачи на любом из императивных языков намного сложнее именно с точки зрения алгоритма.
