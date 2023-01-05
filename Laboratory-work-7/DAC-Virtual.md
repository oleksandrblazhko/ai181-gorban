## Крок 1
#### Заповніть таблицю БД ще трьома рядками.
![image](https://user-images.githubusercontent.com/13904104/210836233-74b100fb-5eed-4084-801f-22aea594bcd0.png)

## Крок 2
#### Створіть схему даних користувача та віртуальну таблицю у цій схемі з правилами вибіркового керування доступом для користувача так, щоб він міг побачити тільки один з рядків таблиці з урахуванням одного значення її останньої колонки.

![image](https://user-images.githubusercontent.com/13904104/210836238-638d55b6-e91e-45c8-8fe3-8ba3faa6b58e.png)

GRANT SELECT ON department TO oleksandr;

CREATE TABLE role2department (role_name VARCHAR(30), year INTEGER);

GRANT SELECT ON role2department TO oleksandr;

INSERT INTO role2department VALUES ('oleksandr', 1999);

CREATE SCHEMA OLEKSANDR;

ALTER SCHEMA OLEKSANDR OWNER TO oleksandr;

![image](https://user-images.githubusercontent.com/13904104/210836240-b30622b0-7d11-4cfb-b636-19b239bfeb2e.png)

CREATE OR REPLACE VIEW OLEKSANDR.department AS
SELECT S.*
FROM PUBLIC.department S, role2department RLS
WHERE RLS.ROLE_NAME = UPPER(CURRENT_USER)
AND RLS.YEAR = S.year;

GRANT SELECT ON OLEKSANDR.department TO OLEKSANDR;
REVOKE SELECT ON PUBLIC.department FROM OLEKSANDR;

## Крок 3
#### Перевірте роботу механізму вибіркового керування, виконавши операції SELECT, INSERT, UPDATE, DELETE.

SELECT * FROM department;

![image](https://user-images.githubusercontent.com/13904104/210836246-1355cf00-83e3-45b5-8bc6-6f1363862e7f.png)

Insert into department values (1, 'IS', 'ICS');
![image](https://user-images.githubusercontent.com/13904104/210836248-549d24a2-40e6-4577-b8d9-f01e10c994aa.png)

UPDATE department SET d_id = 0;
![image](https://user-images.githubusercontent.com/13904104/210836250-5db860ca-6fba-4b04-b98b-46befa4b6fa5.png)

DELETE FROM department WHERE d_id = 0;
![image](https://user-images.githubusercontent.com/13904104/210836251-fafb0fd3-38f5-4379-9914-eb3a7ab6f32d.png)
