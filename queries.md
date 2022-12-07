Selezionare tutti gli studenti nati nel 1990 (160)
```sql
SELECT * FROM `students` WHERE `date_of_birth` LIKE "1990%";
```

Selezionare tutti i corsi che valgono più di 10 crediti (479)
```sql
SELECT * FROM `courses` where `cfu` > '10';
```

Selezionare tutti gli studenti che hanno più di 30 anni
```sql
SELECT * FROM `students` WHERE TIMESTAMPDIFF(YEAR, `date_of_birth`, CURDATE()) > 30;
```

Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
```sql
SELECT * FROM `courses` WHERE `courses`.`period` LIKE "I %" AND `courses`.`year` = 1;
```

Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
```sql
SELECT * FROM `exams` WHERE `exams`.`date` = "2020-06-20" AND `exams`.`hour` > "14:00";
```

Selezionare tutti i corsi di laurea magistrale (38)
```sql
SELECT * FROM `degrees` WHERE `degrees`.`level` = "magistrale";
```

Da quanti dipartimenti è composta l'università? (12)
```sql
SELECT COUNT(*) FROM `departments`;
```

Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
```sql
SELECT COUNT(*) FROM `teachers` WHERE `teachers`.`phone` IS NULL;
```


##### GROUP BY:

Contare quanti iscritti ci sono stati ogni anno
```sql
SELECT year(`students`.`enrolment_date`) AS `enrolment_year`, COUNT(`id`) AS `students_number` 
FROM `students` 
GROUP BY `enrolment_year`;
```
Contare gli insegnanti che hanno l'ufficio nello stesso edificio
```sql
SELECT COUNT(`id`) as `teachers_number`, `teachers`.`office_address` AS `office_add` 
FROM `teachers` 
GROUP BY `office_add`;
```
Calcolare la media dei voti di ogni appello d'esame
```sql
SELECT `exam_student`.`exam_id`, AVG(`exam_student`.`vote`) 
FROM exam_student 
GROUP BY `exam_student`.`exam_id`;
```
Contare quanti corsi di laurea ci sono per ogni dipartimento
```sql
SELECT COUNT(*), `degrees`.`department_id` AS `department_degrees` 
FROM `degrees` 
GROUP BY `department_degrees`;
```


##### JOIN:

Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
```sql
SELECT `students`.`id`, `students`.`name`, `students`.`surname`, `students`.`registration_number` 
FROM `students` 
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id` 
WHERE `degrees`.`name` = "Corso di Laurea in Economia";
```
Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
```sql
SELECT * FROM `departments` 
JOIN `degrees` ON `degrees`.`department_id` = `departments`.`id` 
WHERE `degrees`.`level` = "magistrale" AND `departments`.`name` = "Dipartimento di Neuroscienze";
```
Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
```sql
SELECT `courses`.`id`, `courses`.`name` 
FROM `courses` 
JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id` 
JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id` 
WHERE `course_teacher`.`teacher_id` = 44;
```
Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
```sql
SELECT `students`.`surname`, `students`.`name`, `degrees`.`name`, `departments`.`name` 
FROM `students` 
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id` 
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` 
ORDER BY `students`.`surname`, `students`.`name` ASC;
```
Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
```sql
SELECT `degrees`.`name`, `courses`.`name`, `teachers`.`name`, `teachers`.`surname` 
FROM `degrees` 
JOIN `courses` ON `courses`.`degree_id` = `degrees`.`id` 
JOIN `course_teacher` ON `course_teacher`.`course_id` = `courses`.`id` 
JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`;
```
Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
```sql
SELECT DISTINCT `teachers`.`id`, `teachers`.`name`, `teachers`.`surname`, `departments`.`name`
FROM `teachers`
JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = "Dipartimento di Matematica"
```
BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per superare ciascuno dei suoi esami
```sql
SELECT `students`.`name`, `students`.`surname`,`courses`.`name` AS `course_name`, COUNT(`exams`.`id`) AS `tries_number`, MAX(`exam_student`.`vote`) AS `exam_vote`
FROM `students`
JOIN `exam_student` ON `students`.`id` = `exam_student`.`student_id`
JOIN `exams` ON `exam_student`.`exam_id` = `exams`.`id`
JOIN `courses` ON `exams`.`course_id` = `courses`.`id`
GROUP BY `students`.`id`, `courses`.`id`
HAVING `exam_vote` >= 18
ORDER BY `students`.`surname`, `students`.`name`, `course_name`;
```