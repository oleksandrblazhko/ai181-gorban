## Крок 1
#### Встановіть СКБД PostgreSQL
Оскільки PostgreSQL вже була встановлена, перевіримо її наявність та версію командою psql --version. Бачимо, що встановлена версія PostgreSQL 14.5.

![image](https://user-images.githubusercontent.com/13904104/210446490-8231835e-4557-4dba-a649-a03ed17a03b9.png)

## Крок 2
#### Створіть термінальну консоль psql через утиліту командного рядка вашої ОС та встановіть з’єднання з БД postgres від імені користувача-адміністратора postgres
Командою psql -U postgres -d postgres створено термінальну консоль. Також введено пароль користувача postgres та встановлен зʼєднання з БД.

![image](https://user-images.githubusercontent.com/13904104/210446642-9478b9af-b3a3-4c5d-8c4b-1a83b641e749.png)

## Крок 3
#### Зареєструйте нового користувача в СКБД PostgreSQL, назва якого співпадає з вашим ім'ям, наприклад ivan, і довільним паролем.

Створено користувача oleksandr з паролем gorban командою CREATE USER oleksandr WITH PASSWORD 'gorban';. Метакомандою \du переглянуто список усіх доступних користувачів.
![image](https://user-images.githubusercontent.com/13904104/210446752-e3549f4d-cf98-42fe-95f3-ba23e8791478.png)

## Крок 4
#### Створіть роль в СКБД PostgreSQL (назва співпадає з вашим прізвищем латинськими літерами) і надайте новому користувачеві можливість наслідувати цю роль.

Створено роль gorban.

CREATE ROLE gorban; GRANT gorban TO oleksandr;

![image](https://user-images.githubusercontent.com/13904104/210446840-8a3e6d8a-3049-484c-b040-26703c01bcc0.png)

## Крок 5
#### Створіть реляційну таблицю з урахуванням варіанту з таблиці 2.1 від імені користувача-адміністратора.

Create table department (d_id integer, name varchar, faculty varchar);
Метакомандою \dt переглянуто усі наявні таблиці в базі даних.
![image](https://user-images.githubusercontent.com/13904104/210446995-05916f9f-2648-4407-bcd3-a1410905dfd8.png)

## Крок 6
#### Внесіть один рядок в таблицю, використовуючи команду insert into ..., відповідно до варіанту.

Командою Insert into department values (1, 'IS', 'ICS'); введено дані у таблицю. Перевіримо, чи ввелись дані, командою SELECT * FROM department;

![image](https://user-images.githubusercontent.com/13904104/210447579-c2955641-160d-4bcb-9d9a-c8bae755679f.png)

## Крок 7
#### Додатково створіть ще одну термінальну консоль psql та та встановіть з’єднання з БД postgres від імені нового користувача

Командою psql -U oleksandr -d postgres виконано зʼєднання з БД PostgreSQL.

![image](https://user-images.githubusercontent.com/13904104/210448043-7dfb05cb-545f-4494-8f6b-13109ccb0ad6.png)

## Крок 8
#### Від імені вашого користувача виконайте запит на отримання даних з таблиці (select * from таблиця). Запротоколюйте результат виконання команди.

Командою SELECT * FROM department; виконаємо запит на отримання даних з таблиці. Як бачимо, користувач oleksandr не має доступу до таблиці (дії у другому вікні).

![image](https://user-images.githubusercontent.com/13904104/210756397-89f9746a-b4d5-46db-b87d-9db844e344a0.png)

## Крок 9
#### Встановіть повноваження на читання таблиці новому користувачеві.

Командою GRANT SELECT ON department TO oleksandr; надамо повноваження (у першому вікні); 
![image](https://user-images.githubusercontent.com/13904104/210757112-d9f190d2-62f1-4761-8fc3-a04aabdce504.png)

## Крок 10
#### Повторіть крок 8.

Оскільки тепер користувач oleksandr має доступ, то командою SELECT * FROM department; отримаємо дані з таблиці department.

![image](https://user-images.githubusercontent.com/13904104/210757553-8cf0cf1c-0fbe-4561-9c19-29745ddbb609.png)

## Крок 11
#### Зніміть повноваження на читання таблиці для нового користувача.

Командою REVOKE SELECT ON department FROM oleksandr; для користувача oleksandr знято повноваження на читання таблиці department (у першому вікні).

![image](https://user-images.githubusercontent.com/13904104/210758639-35166578-ccc4-4cd5-b615-aeb71b832f64.png)


## Крок 12
#### Повторіть крок 8.

Як бачимо, в нас немає доступу до читання таблиці (у другому вікні). 

SELECT * FROM department;

![image](https://user-images.githubusercontent.com/13904104/210759105-53eef0bf-ecac-4b3a-bd6e-9658ea349566.png)

## Крок 13
#### Створіть команду оновлення даних таблиці (UPDATE) і виконайте її від імені нового користувача. Проаналізуйте результат виконання команди.

Спробуємо оновити дані в таблиці командою 
UPDATE department SET d_id = 5;
Як бачимо, прав на оновлення у користувача oleksandr немає (друге вікно).

![image](https://user-images.githubusercontent.com/13904104/210759646-4d09cfa0-5a33-407e-83cf-d46fcefe7460.png)

## Крок 14
#### Встановіть повноваження на оновлення таблиці новому користувачеві.

Командою GRANT UPDATE ON department TO oleksandr;
для користувача oleksandr надано повноваження на оновлення таблиці department (перше вікно).

![image](https://user-images.githubusercontent.com/13904104/210763300-f213b54c-2015-41ee-aca4-6fd9b0cb044a.png)

## Крок 15
#### Повторіть крок 13.

UPDATE department SET d_id = 5; (друге вікно)

![image](https://user-images.githubusercontent.com/13904104/210763815-fe5dfd4d-354c-424a-8530-cb48a9eccd44.png)

## Крок 16
#### Створіть команду видалення запису таблиці (DELETE) і виконайте її від імені нового користувача. Проаналізуйте результат виконання команди.

Повноваження для видалення даних з таблиці відсутні.

DELETE FROM department WHERE d_id = 5; (друге вікно)

![image](https://user-images.githubusercontent.com/13904104/210764671-aba6a2cb-05fe-4b78-a1be-cf19f56096d0.png)

## Крок 17
#### Встановіть повноваження на видалення таблиці новому користувачеві.

GRANT DELETE ON department TO oleksandr; (перше вікно)

![image](https://user-images.githubusercontent.com/13904104/210835491-ec88ea46-f371-47fc-b437-a5ea7924e959.png)

## Крок 18
#### Повторіть крок 16.

DELETE FROM department WHERE d_id = 5; (друге вікно)

![image](https://user-images.githubusercontent.com/13904104/210835637-4672c747-73db-45e5-bc9e-6dceb3f33c8c.png)

## Крок 19
#### Зніміть всі повноваження з таблиці для нового користувача.

REVOKE ALL ON department FROM oleksandr;

![image](https://user-images.githubusercontent.com/13904104/210835777-20edbfd1-6efc-4628-8d78-e1373c3870b5.png)

## Крок 20
#### Створіть команду внесення запису в таблицю (INSERT) і виконайте її від імені нового користувача. Проаналізуйте результат виконання команди.

Повноваження для додавання даних у таблицю відсутні.

![image](https://user-images.githubusercontent.com/13904104/210835940-0336ef84-d3da-40be-9bd1-a47f364359eb.png)

## Крок 21
#### Встановіть повноваження на внесення даних до таблиці для ролі.

GRANT INSERT ON department TO oleksandr; (перше вікно)

![image](https://user-images.githubusercontent.com/13904104/210835941-f25f41a1-789c-42dc-81b0-8d34e2bbaa8c.png)

## Крок 22
#### Повторіть крок 20.

![image](https://user-images.githubusercontent.com/13904104/210835943-ed3ff21b-f847-4ef2-92d1-fd2893bc1020.png)



