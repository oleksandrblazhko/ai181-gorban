## Крок 1
#### Створіть у БД структури даних, необхідні для роботи повноважного керування доступом.

![image](https://user-images.githubusercontent.com/13904104/210837104-c54913c5-3d12-485c-985c-9857f27efecd.png)

DROP TABLE IF EXISTS Access_Levels CASCADE;

CREATE TABLE Access_Levels (
access_level_id INTEGER PRIMARY KEY,
access_level VARCHAR UNIQUE
);

INSERT INTO Access_Levels VALUES (1, 'public');
INSERT INTO Access_Levels VALUES (2, 'private');
INSERT INTO Access_Levels VALUES (3, 'secret');

DROP TABLE IF EXISTS Role_Access_Level CASCADE;

CREATE TABLE Role_Access_Level (
role_name VARCHAR PRIMARY KEY,
access_level INTEGER REFERENCES
Access_Levels (access_level_id)
);

REVOKE ALL
ON Role_Access_Level
FROM GROUP oleksandr;

GRANT SELECT
ON Role_Access_Level
TO GROUP oleksandr;

## Крок 2
#### Визначте для кожного рядка таблиці мітки конфіденційності (для кожного рядка свою мітку).
UPDATE PUBLIC.department SET spot_conf = 1;

## Крок 3
#### Визначте для користувача його рівень доступу

![image](https://user-images.githubusercontent.com/13904104/210837105-07c8b2c3-a0e2-427c-ac12-73f0170ea4e5.png)

## Крок 4
#### Створіть нову схему даних.
DROP SCHEMA IF EXISTS oleksandr CASCADE;
CREATE SCHEMA oleksandr;

![image](https://user-images.githubusercontent.com/13904104/210837109-1f900eb9-6b1e-466c-93d0-d7b7959f3a3d.png)

ALTER SCHEMA oleksandr OWNER TO oleksandr;

![image](https://user-images.githubusercontent.com/13904104/210837111-48417cdf-77e5-4330-9a74-b47755959621.png)

## Крок 5
#### Створіть віртуальну таблицю, назва якої співпадає з назвою реальної таблиці та яка забезпечує SELECT-правила повноважного керування доступом для користувача.

![image](https://user-images.githubusercontent.com/13904104/210837114-30fdd3d0-28de-46ba-915b-3738290546fe.png)

DROP VIEW IF EXISTS oleksandr.department;

CREATE VIEW oleksandr.department AS
SELECT
d_id,
name,
faculty
FROM PUBLIC.department s, Role_Access_Level l
WHERE
l.role_name = CURRENT_USER and
l.access_level >= s.spot_conf;

## Крок 6
#### Створіть INSERT/UPDATE/DELETE-правила повноважного керування доступом для користувача.

REVOKE ALL ON public.department FROM oleksandr;

GRANT SELECT  ON oleksandr.department TO oleksandr;

SELECT * FROM public.oleksandr;
SELECT * FROM oleksandr.department;

UPDATE public.department 
SET spot_conf = 2 
WHERE d_id = 2;

![image](https://user-images.githubusercontent.com/13904104/210837117-a73c6cc6-6a6f-4c80-a5a5-aa1a8a0ff5b5.png)

select * from department;

GRANT DELETE 
ON oleksandr.department 
TO oleksandr;

DELETE from department;

![image](https://user-images.githubusercontent.com/13904104/210837120-e136491b-8eac-4e72-9d4b-af88877fd284.png)


CREATE RULE department_delete 
AS ON DELETE TO oleksandr.department
DO INSTEAD
DELETE FROM public.department 
WHERE d_id = OLD.d_id;

![image](https://user-images.githubusercontent.com/13904104/210837124-87f9e7f0-2244-4537-8382-4979ec06cffa.png)

## Крок 7 
#### Перевірте роботу механізму повноважного керування, виконавши операції SELECT, INSERT, UPDATE, DELETE

![image](https://user-images.githubusercontent.com/13904104/210837126-ebb858c7-3dd2-408d-b371-0c28a75a37e5.png)



