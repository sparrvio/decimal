# decimal

Реализация функции библиотеки decimal.h
Двоичное представление Decimal состоит из 1-разрядного знака, 96-разрядного целого числа и коэффициента масштабирования, используемого для деления целого числа и указания того, какая его часть является десятичной дробью. Коэффициент масштабирования неявно равен числу 10, возведенному в степень в диапазоне от 0 до 28.
Decimal число может быть реализовано в виде четырехэлементного массива 32-разрядных целых чисел со знаком (int bits[4];).
bits[0], bits[1], и bits[2] содержат младшие, средние и старшие 32 бита 96-разрядного целого числа соответственно.
bits[3] содержит коэффициент масштабирования и знак, и состоит из следующих частей:

Биты от 0 до 15, младшее слово, не используются и должны быть равны нулю.
Биты с 16 по 23 должны содержать показатель степени от 0 до 28, который указывает степень 10 для разделения целого числа.
Биты с 24 по 30 не используются и должны быть равны нулю.
Бит 31 содержит знак; 0 означает положительный, а 1 означает отрицательный.

Обратите внимание, что битовое представление различает отрицательные и положительные нули. Эти значения могут считаться эквивалентными во всех операциях.

Арифитические операторы
- Сложение
int s21_add(s21_decimal value_1, s21_decimal value_2, s21_decimal *result)
- Вычитание
int s21_sub(s21_decimal value_1, s21_decimal value_2, s21_decimal *result)
- Умножение
int s21_mul(s21_decimal value_1, s21_decimal value_2, s21_decimal *result)
- Деление
int s21_div(s21_decimal value_1, s21_decimal value_2, s21_decimal *result)
- Остаток от деления
int s21_mod(s21_decimal value_1, s21_decimal value_2, s21_decimal *result)

Функции возвращают код ошибки:
- 0 - OK
- 1 - число слишком велико или равно бесконечности
- 2 - число слишком мало или равно отрицательной бесконечности
- 3 - деление на 0

Уточнение про числа, не вмещающиеся в мантиссу:

При получении чисел, не вмещающихся в мантиссу при арифметических операциях, использовать банковское округление (например, 79,228,162,514,264,337,593,543,950,335 - 0.6 = 79,228,162,514,264,337,593,543,950,334)

Уточнение про операцию mod:

Если в результате операции произошло переполнение, то необходимо отбросить дробную часть (например, 70,000,000,000,000,000,000,000,000,000 % 0.001 = 0.000)


Операторы сравнения

- Меньше
int s21_is_less(s21_decimal, s21_decimal)
-Меньше или равно
int s21_is_less_or_equal(s21_decimal, s21_decimal)
- Больше
int s21_is_greater(s21_decimal, s21_decimal)
- Больше или равно
int s21_is_greater_or_equal(s21_decimal, s21_decimal)
- Равно
int s21_is_equal(s21_decimal, s21_decimal)
- Не равно
int s21_is_not_equal(s21_decimal, s21_decimal)

Возвращаемое значение:
- 0 - FALSE
- 1 - TRUE


Преобразователи
- Из int
int s21_from_int_to_decimal(int src, s21_decimal *dst)
- Из float
int s21_from_float_to_decimal(float src, s21_decimal *dst)
- В int
int s21_from_decimal_to_int(s21_decimal src, int *dst)
- В float
int s21_from_decimal_to_float(s21_decimal src, float *dst)

Возвращаемое значение - код ошибки:
- 0 - OK
- 1 - ошибка конвертации

Уточнение про преобразование числа типа float:

- Если числа слишком малы (0 < |x| < 1e-28), вернуть ошибку и значение, равное 0
- Если числа слишком велики (|x| > 79,228,162,514,264,337,593,543,950,335) или равны бесконечности, вернуть ошибку
- При обработке числа с типом float преобразовывать все содержащиеся в нём цифры

Уточнение про преобразование из числа типа decimal в тип int:

- Если в числе типа decimal есть дробная часть, то её следует отбросить (например, 0.9 преобразуется 0)


Другие функции
- Округляет указанное Decimal число до ближайшего целого числа в сторону отрицательной бесконечности.
int s21_floor(s21_decimal value, s21_decimal *result)


- Округляет Decimal до ближайшего целого числа.
int s21_round(s21_decimal value, s21_decimal *result)


- Возвращает целые цифры указанного Decimal числа; любые дробные цифры отбрасываются, включая конечные нули.
int s21_truncate(s21_decimal value, s21_decimal *result)

- Возвращает результат умножения указанного Decimal на -1.
int s21_negate(s21_decimal value, s21_decimal *result)

Возвращаемое значение - код ошибки:
- 0 - OK
- 1 - ошибка вычисления

Необходимо реализовать описанные выше функции библиотеки:

- Библиотека должна быть разработана на языке Си стандарта C11 с использованием компиятора gcc
- Код библиотеки должен находиться в папке src в ветке develop
- Не использовать устаревшие и выведенные из употребления конструкции языка и библиотечные функции. Обращать внимания на пометки legacy и obsolete в официальной документации по языку и используемым библиотекам. Ориентироваться на стандарт POSIX.1-2017
- Оформить решение как статическую библиотеку (с заголовочным файлом s21_decimal.h)
- Библиотека должна быть разработана в соответствии с принципами структурного программирования
- Перед каждой функцией использовать префикс s21_
- Подготовить полное покрытие unit-тестами функций библиотеки c помощью библиотеки Check
- Unit-тесты должны покрывать не менее 80% каждой функции
- Предусмотреть Makefile для сборки библиотеки и тестов (с целями all, clean, test, s21_decimal.a, gcov_report)
- В цели gcov_report должен формироваться отчёт gcov в виде html страницы. Для этого unit-тесты должны запускаться с флагами gcov
- При реализации decimal ориентироваться на двоичное представление с целочисленным массивом bits, как указано в примере выше. Соблюсти положение разрядов числа в массиве bits
- Запрещено использование типа __int128
- Конечные нули можно как оставлять, так и удалять (за исключением функции s21_truncate)
- Определяемый тип должен поддерживать числа от -79,228,162,514,264,337,593,543,950,335 до +79,228,162,514,264,337,593,543,950,335.
