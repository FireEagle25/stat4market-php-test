# Ответы на тестовое задание для PHP-разработчика (Senior, Middle+)

## Задание 1 - Написать SQL-запросы (MySQL)

### 1 - выборка пользователей, у которых количество постов больше, чем у пользователя их пригласившего.

 ``` sql
SELECT users.*
FROM users 
LEFT JOIN users AS inviters
  ON users.invited_by_user_id = inviters.id
WHERE users.posts_qty > inviters.posts_qty
```
### 2 - выборка пользователей, имеющих максимальное количество постов в своей группе.
``` sql
SELECT DISTINCT on (group_id) users.*
FROM users
LEFT JOIN groups
  ON users.group_id = groups.id
ORDER BY group_id, posts_qty DESC
```

### 3 - выборка групп, количество пользователей в которых превышает 10000.
``` sql
SELECT groups.*        
FROM groups
LEFT JOIN users
  ON groups.id = users.group_id
GROUP BY groups.id
HAVING COUNT(users.group_id) > 10000
```

### 4 - выборка пользователей, у которых пригласивший их пользователь из другой группы.
``` sql 
SELECT users.*
FROM users 
LEFT JOIN users as inviters
  ON users.invited_by_user_id = inviters.id
WHERE users.group_id != inviters.group_id
```

### 5 - выборка групп с максимальным количеством постов у пользователей.
``` sql
SELECT groups.*, SUM(users.posts_qty) as users_posts_count        
FROM groups
LEFT JOIN users
  ON groups.id = users.group_id
GROUP BY groups.id
ORDER BY users_posts_count DESC
LIMIT 2
```

## Задание 2 - Написать SQL-запросы (MySQL) для добавления трех полей, изменения имени одного поля и добавления двух индексов в базу данных размером свыше 100 ГБ и более 8 миллионов строк.
``` sql
CREATE TABLE db.tablename_new LIKE db.tablename;

ALTER TABLE db.tablename_new
ADD new_column_name_1 DataType NOT NULL DEFAULT defaultValue;
ADD new_column_name_2 DataType NOT NULL DEFAULT defaultValue;
ADD new_column_name_3 DataType DEFAULT NULL;
CHANGE old_column_name new_column_name datatype,
ADD INDEX (example_index_1);
ADD INDEX (example_index_2);

INSERT INTO db.tablename_new (column1, column2, column3, ...) SELECT column1, column2, column3, ... db.tablename;
ALTER TABLE db.tablename RENAME db.tablename_old;
ALTER TABLE db.tablename_new RENAME db.tablename;

TRUNCATE TABLE db.tablename_old;
ALTER TABLE db.tablename_old ENGINE=InnoDB;
DROP TABLE db.tablename_old;
```

## Задание 3 - Написать рассчет количества вторников между двумя датами на PHP.
``` php
function getDayOfWeekCountBetweenDateTimes(DateTime $from, DateTime $to, $searched_day_of_the_week=2)
{
    $DAYS_IN_WEEK = 7;
	
    $diff_in_days = $from->diff($to)->days;
	
    $full_weeks = intdiv($diff_in_days, $DAYS_IN_WEEK);
    $remaining_days = $diff_in_days % $DAYS_IN_WEEK;
    
    if (isDayIncludedWhenAddingDays($from, $remaining_days, $searched_day_of_the_week)) {
    	return $full_weeks + 1;
    }
    
    return $full_weeks;

}

function isDayIncludedWhenAddingDays(DateTime $start_date, $days_to_add, $searched_day_of_the_week)
{
	$DAYS_IN_WEEK = 7;
	
	$from = intval($start_date->format("N"));
	$to = $from + $days_to_add;
	
	if ($to > $DAYS_IN_WEEK) {
		$searched_day_of_the_week += $DAYS_IN_WEEK;
  }
	
	return $searched_day_of_the_week > $from && $searched_day_of_the_week <= $to;
}
```
