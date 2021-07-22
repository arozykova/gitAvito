# gitAvito

здесь создание таблиц и наполнение тестовыми данными
```
CREATE TABLE task_status  ( 
issue_key INT(8) UNSIGNED, 
status VARCHAR(30) NOT NULL, 
start_time DATETIME NOT NULL
); 

CREATE TABLE task_info  ( 
issue_key INT(8) UNSIGNED AUTO_INCREMENT PRIMARY KEY, 
type VARCHAR(30) NOT NULL, 
name VARCHAR(100) NOT NULL
); 

INSERT INTO `task_info` (`issue_key`, `type`, `name`) VALUES ('1', 'Bug', 'landing error');
INSERT INTO `task_info` (`issue_key`, `type`, `name`) VALUES ('2', 'Task', 'create new page');
INSERT INTO `task_info` (`issue_key`, `type`, `name`) VALUES ('3', 'Bug', 'auth error');
INSERT INTO `task_info` (`issue_key`, `type`, `name`) VALUES ('4', 'Story', 'new project');
INSERT INTO `task_info` (`issue_key`, `type`, `name`) VALUES ('5', 'Bug', 'db connection error');
INSERT INTO `task_info` (`issue_key`, `type`, `name`) VALUES ('6', 'Bug', 'cache connection error');

INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('1', 'open', '2021-04-01 00:00:00');
INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('1', 'in progress', '2021-04-02 00:00:00');
INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('1', 'done', '2021-04-03 00:00:00');
INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('1', 'in progress', '2021-04-04 00:00:00');

INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('2', 'open', '2021-03-01 00:00:00');
INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('2', 'in progress', '2021-03-02 00:00:00');
INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('2', 'done', '2021-03-03 00:00:00');

INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('3', 'open', '2021-04-10 00:00:00');
INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('3', 'in progress', '2021-04-12 00:00:00');
INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('3', 'done', '2021-04-14 00:00:00');

INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('5', 'open', '2021-05-10 00:00:00');
INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('5', 'in progress', '2021-05-12 00:00:00');
INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('5', 'done', '2021-05-14 00:00:00');

INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('6', 'open', '2021-04-10 00:00:00');
INSERT INTO `task_status` (`issue_key`, `status`, `start_time`) VALUES ('6', 'done', '2021-04-14 00:00:00');

а это сам ответ на ТЗ

SELECT DISTINCT task_status.issue_key, task_info.name
FROM task_status 
INNER JOIN task_info
ON task_status.issue_key = task_info.issue_key
WHERE task_info.type = 'Bug'
AND
task_status.status = 'in progress'
AND
task_status.start_time BETWEEN '2021-04-01 00:00:00' AND '2021-04-30 23:59:59';
```

ТЗ №2

Что бы покупатель гарантированно получил скидку в соответствии со своими накопленными баллами, необходимо изменить диапазон баллов, дающих определённый % скидки. При текущем условии "От 0 до 100 баллов - скидка 1% От 100 до 200 баллов - скидка 3 ", при наличии 100 баллов покупатель может получить, как 1%, так и 3% скидку, при условии "От 100 до 200 баллов - скидка 3 % От 200 до 500 баллов - скидка 5%" при наличии 200 баллов, скидка может быть как 3% так и 5%, при условии "От 200 до 500 баллов - скидка 5% От 500 баллов - скидка 10%", при наличии 500 баллов ,скидка будет рассчитана либо 5% либо 10%.

Что бы покупатель гарантированно получил скидку в соответствии со своими накопленными баллами предлагаю след диапазон значений:

0-100 баллов -1%
101-200 баллов - 3%
201-500 баллов -5%
от 501 - бесконечности - 10%
Тогда позитивные сценарии проверки скидки в 1% это 0, 100, 1, 50, 99 баллов. Негативные 101 и отрицательный баланс баллов 
Позитивные сценарии для 3% скидки это 101, 200, 102, 150, 199. Негативные 100 и 201 
Позитивные сценарии для 5% скидки это 201, 500, 202, 300, 499. Негативные 200 и 501 
Позитивные сценарии для 10% скидки это 501 и 1000, 10000, 100000, 1000000 Негативные 500
