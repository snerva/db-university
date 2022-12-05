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
SELECT * FROM `students` WHERE NOW() - `students`.`date_of_birth` > 30;
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