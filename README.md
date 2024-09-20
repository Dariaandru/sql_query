# sql_query

# Создание баз данных для разных предметных областей

Задача этого проекта - реализовать хранение данных в базах данных с помощью sql-запросов для 4 предметных областей:

- библиотека
- ресторан
- учебное заведение
- фитнес-центр

## Требования

Для каждой из предметных областей нужно:

1. Создать таблицу с объектами, атрибутами и ограничениями каждого из атрибутов

2. Нарисовать ER-диаграмму

3. Написать sql-запросы

## Библиотека

### Таблица с объектами и атрибутами

|Таблица   | Атрибуты        | Ограничения                      |
|----------|-----------------|----------------------------------|
|книги     | id              | только числа, авто инкремент     |
|          |название         |  строка не более 50 символов     |
|          | автор           |строка не более 50 символов       |
|          | год издания     |  число больше чем 0              |
|          | жанр            |  строка не более 50 символов     |
|          | статус наличия  |  строка не более 50 символов     |
|          | обложка          |  строка не более 50 символов    |
||        ||                 ||                                ||
| читатели | id              | только числа, авто инкремент     |
|          | имя             |  строка не более 50 символов     |
|          | фамилия         | строка не более 50 символов      |
|          | дата рождения   | формат даты: ГГГГ-ММ-ДД          |
|          | адрес           | строка не более 150 символов     |
|          | телефон         | строка строго 11 символов        |
||        ||                 ||                                ||
| выдача   |  id             | только числа, авто инкремент     |
|          | id книги        | только числа, ссылка             | 
|          | id читателя     | только числа, ссылка             |
|          | дата выдачи     | формат даты: ГГГГ-ММ-ДД          |
|          | дата возврата   | формат даты: ГГГГ-ММ-ДД          |

### ER-диаграмма библиотеки

![](./images/библиотека.png)

Таблица для учета выдачи книг не является сущностью, это просто соответсятвие книги и читателя так как между ними связь много ко многому.

Книга и читатель могут существовать друг без друга.

### SQL-запросы

#### Таблица с книгами

1. Создание таблицы для книг

```sql
CREATE TABLE books (
	id SERIAL PRIMARY KEY,
	name VARCHAR(50),
	author VARCHAR(50),
	year INTEGER,
	janr VARCHAR(50),
	status VARCHAR(50)
);

ALTER TABLE books
ADD COLUMN cover VARCHAR(50)
```
2. Добавление экземпляров и просмотр результата

```sql
INSERT INTO books (name, author, year, janr, status, cover) VALUES ('Война и мир', 'Лев Толстой', 1867, 'роман', 'В наличии', 'бумажная');

INSERT INTO books (name, author, year, janr, status, cover) VALUES ('Белые ночи', 'Достоевский', 1848, 'повесть', 'Нет на складе', 'картонная');

INSERT INTO books (name, author, year, janr, status, cover) VALUES ('Мастер и Маргарита', 'Булгаков', 1940, 'роман', 'В наличии', 'кожаная');


SELECT * FROM books
```

![](./images/постгрес_книги.png)

#### Таблица с читателями

1. Создание таблицы для читателей

```sql
CREATE TABLE readers (
	id SERIAL PRIMARY KEY,
	name VARCHAR(50),
	surname VARCHAR(50),
	birth DATE,
	addres VARCHAR(150)
);

ALTER TABLE readers
ADD COLUMN phone CHAR(11)
```
2. Добавление экземпляров и просмотр результата

```sql
INSERT INTO readers (name, surname, birth, addres, phone) VALUES ('Ivan', 'Ivanov', '1990-01-01', 'Moscow', '89135553535');

INSERT INTO readers (name, surname, birth, addres, phone) VALUES ('Petr', 'Petrov', '1990-01-01', 'Moscow', '89536783535');

INSERT INTO readers (name, surname, birth, addres, phone) VALUES ('Sidor', 'Sidorov', '1990-01-01', 'Moscow', '89055553429');


SELECT * FROM readers
```

![](./images/постгрес_читатели.png)

#### Таблица с учетом выдачи книг

1. Создание таблицы для чучета выдачи книг

```sql
CREATE TABLE book_out (
	id SERIAL PRIMARY KEY,
	id_book INTEGER,
	id_reader INTEGER,
	date_out date,
	date_retutn date,
	FOREIGN KEY (id_reader) REFERENCES readers(id),
	FOREIGN KEY (id_book) REFERENCES books(id)
);
```
2. Добавление экземпляров и просмотр результата

```sql
INSERT INTO book_out (date_out, date_retutn) VALUES ('2020-01-01', '2020-01-03');

INSERT INTO book_out (date_out, date_retutn) VALUES ('2020-02-06', '2020-02-19');

INSERT INTO book_out (date_out, date_retutn) VALUES ('2020-03-08', '2020-03-16');


SELECT * FROM book_out
```
![](./images/посттгрес_выдача_книг.png)