1. Write a query to display the ff columns ID (should start with T + 11 digits of trn_teacher.id with leading zeros like'T00000088424'), Nickname, Status and Roles (like
Trainer/Assessor/Staff) using table trn_teacher and trn_teacher_role.

Answer:
SELECT 
 CONCAT('T000', a.id) as ID,
 a.nickname as Nickname,
 CASE WHEN a.status = 0 THEN 'Discontinued' 
  WHEN a.status = 1 THEN 'Active'
  WHEN a.status = 2 THEN 'Deactivated'
  ELSE 'NA' END as Status,
 CASE WHEN b.role = 1 THEN 'Trainer' 
  WHEN b.role = 2 THEN 'Assessor' 
  WHEN b.role = 3 THEN 'Staff' 
  ELSE 'NA' END as Roles  
FROM trn_teacher a
LEFT JOIN trn_teacher_role b ON a.id=b.teacher_id


2. Write a query to display the ff columns ID (from teacher.id), Nickname, Open (total open slots from trn_teacher_time_table), Reserved (total reserved slots from trn_teacher_time_table), Taught (total taught from trn_evaluation) and NoShow (total no_show from trn_evaluation) using all tables above. Should show only those who are active (trn_teacher.status = 1 or 2) and those who have both Trainer.

Answer:
SELECT
 a.id AS ID,
 a.nickname AS Nickname,
 (SELECT COUNT(status) FROM trn_time_table WHERE teacher_id=a.id AND status=1) AS 'Open',
 (SELECT COUNT(status) FROM trn_time_table WHERE teacher_id=a.id AND status=3) AS 'Reserved',
 (SELECT COUNT(result) FROM trn_evaluation WHERE teacher_id=a.id AND result=1) AS 'Taught',
 (SELECT COUNT(result) FROM trn_evaluation WHERE teacher_id=a.id AND result=2) AS 'No Show'
FROM
 trn_teacher a

