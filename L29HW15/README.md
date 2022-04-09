# Домашнее задание по теме "Анализ и профилирование запроса в MySql"

Запрос:

![изображение](https://user-images.githubusercontent.com/63968897/161402247-2cc7fc7f-7598-4c59-84e6-0e45606c6a14.png)

Результат EXPLAIN:

![изображение](https://user-images.githubusercontent.com/63968897/161402317-f06bd364-de41-41d1-9e2a-821d36062b64.png)

В столбце select_type значение SIMPLE говорит о том, что запрос простой, т.е. не содержит подзапросов и объединений.

В столбце table псевдонимы таблиц, к которым относятся строки: o – orders, op - order_product, pc - product_category, p – product.

В столбце partitions значение NULL т.к. партиционирование не используется.

Начинаем читать EXPLAIN снизу вверх:
1.	В четвертой строке в столбце type значение eq_ref говорит о том, что по ключу PRIMARY считывается всего одна строка из таблицы orders, а значение Using where говорит о том, что используется фильтр перед следующей связкой или выдачей. 
2.	В третьей строке значение ref в колонке type говорит о том, что поиск идет по ключу в таблице order_product. Оптимизатор может использовать ключи PRIMARY или order_product_product_id_fk, но фактически будет использовать order_product_product_id_fk.
3.	Вторая строка говорит о том, что поиск будет производиться в таблице product_category по ключу PRIMARY с использованием индекса.
4.	Первая строка говорит о том, что таблица product будет просматриваться целиком, но в порядке, заданном индексом product_producer_id_fk. Кроме того, Using temporary означает, что для сортировки и группировки результатов запроса будет создана временная таблица.


Прочитать план выполнения и информацию о стоимости запроса с помощью команд EXPLAIN TREE FORMAT и EXPLAIN ANALYZE не удалось, т.к. версия MySQL 8.0.15. EXPLAIN TREE FORMAT появился с версии 8.0.16, а EXPLAIN ANALYZE с версии 8.0.18.