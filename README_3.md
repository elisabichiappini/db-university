Queries JOIN:

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
SELECT `students`.`degree_id`,`students`.`surname` AS `cognome`, `students`.`name` AS `nome` FROM `students` 
INNER JOIN `degrees` 
ON `students`.`degree_id` = `degrees`.`id` 
WHERE `degrees`.`name` = "Corso di Laurea in Economia" 
ORDER BY `cognome`, `nome`;

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
SELECT `departments`.`name` AS `dipartimento`,`degrees`.`department_id` AS `codice_dipartimento`, `degrees`.`id` AS `codice_corso`,  `degrees`.`name` AS `dipartimento`
FROM `degrees`
INNER JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`
WHERE `degrees`.`level` = "magistrale" AND `departments`.`name` = "Dipartimento di Neuroscienze"

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
SELECT `teachers`.*, `courses`.*
FROM `teachers`
INNER JOIN `course_teacher`
ON `teachers`.`id` = `course_teacher`.`teacher_id`
INNER JOIN `courses`
ON `course_teacher`.`course_id` = `courses`.`id`
WHERE `teachers`.`id` = 44;

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
SELECT `students`.`surname` AS `cognome`,`students`.`name` AS `nome`, `degrees`.`name` AS `corso_laurea`, `departments`.`name` AS `dipartimento`
FROM `students`
INNER JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
ORDER BY `cognome`, `nome`

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
SELECT `courses`.*, `teachers`.*
FROM `degrees` 
LEFT JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
INNER JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
INNER JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`; 

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)
SELECT DISTINCT `teachers`.`id`, `teachers`.`surname`, `teachers`.`name`
FROM `teachers`
INNER JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
INNER JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
INNER JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = "Dipartimento di Matematica"
ORDER BY `teachers`.`surname`, `teachers`.`name`

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18.
SELECT `students`.`id`, `students`.`name`, `students`.`surname`, `courses`.`name`, COUNT(`exam_student`.`vote`) AS `tentativi_esame`, MAX(`exam_student`.`vote`)  AS `voto_massimo_esame`, MIN(`exam_student`.`vote`)  AS `voto_massimo_esame`
FROM `students`
INNER JOIN `exam_student` ON `students`.`id` = `exam_student`.`student_id`
INNER JOIN `exams` ON `exam_student`.`exam_id` = `exams`.`id`
INNER JOIN `courses` ON `exams`.`course_id` = `courses`.`id`
GROUP BY `students`.`id`,`courses`.`id`
HAVING `voto_massimo_esame` >= 18

Queries GROUP BY:
1. Contare quanti iscritti ci sono stati ogni anno
SELECT COUNT(`id`), YEAR(`enrolment_date`) AS `year_enrolment`
FROM `students`
GROUP BY `year_enrolment`;

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
SELECT COUNT(`id`), `office_address`
FROM `teachers` 
GROUP BY `office_address`
ORDER BY `office_address` ASC;

3. Calcolare la media dei voti di ogni appello d'esame
SELECT `exam_id` AS `appello esame`, AVG(`vote`) AS `media_esame`
FROM `exam_student` 
GROUP BY `exam_id`;

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
SELECT `department_id` AS `dipartimento`, COUNT(`id`) AS `n_degrees`
FROM `degrees`
GROUP BY `department_id`;

✨ 23_gennaio_2024