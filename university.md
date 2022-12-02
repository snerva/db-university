Modellizzare la struttura di una tabella per memorizzare tutti i dati riguardanti una università:
- sono presenti diversi Dipartimenti (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);
- ogni Dipartimento offre più Corsi di Laurea (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc..)
- ogni Corso di Laurea prevede diversi Corsi (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);
- ogni Corso può essere tenuto da diversi Insegnanti;
- ogni Corso prevede più appelli d'Esame;
- ogni Studente è iscritto ad un solo Corso di Laurea;
- ogni Studente può iscriversi a più appelli di Esame;
- per ogni appello d'Esame a cui lo Studente ha partecipato, è necessario memorizzare il voto ottenuto, anche se non sufficiente.

Dipartimenti:
-id | AI PK NN  UN
- nome | VARCHAR(60) NN
- descrizione | TEXT N

Studente

-id | AI PK NN  UN
- nome | VARCHAR(20) NN
- cognome | VARCHAR(20) NN
- email | VARCHAR(30) N
- phone | VARCHAR(15) N
- corso_di_laurea_id | FK BIGINT
- matricola | VARCHAR(10) NN

Corso 

-id | AI PK NN  UN
- ore | SMALLINT NN
- docente_id | FK BIGINT
- numero_aula | VARCHAR(5) N
- nome | VARCHAR(30) NN
- programma | TEXT NN
- modalitá frequenza | VARCHAR(20) NN


Corso di Laurea:

-id | AI PK NN  UN
- corso_id | FK BIGINT
- programma | TEXT NN
- durata | VARCHAR(30) NN
- data_inizio | DATE NN
- sede |  VARCHAR(20) NN
- dipartimento_id | FK BIGINT
- lingua | VARCHAR(2) N

Insegnanti:

-id | AI PK NN  UN
- nome | VARCHAR(20) NN
- cognome | VARCHAR(20) NN
- corso_id  | FK BIGINT

Appello:

-id | AI PK NN  UN
- numero_aula | VARCHAR(5) N
- corso_id | FK BIGINT
- nome | VARCHAR(30) N
- tipologia | VARCHAR(20) N
- numero_aula | VARCHAR(5) N

appelli_studente(pivot):

- appello_status | TINYINT N
- studente_id  | FK BIGINT
- voto | DECIMAL(3, 1) N
- appello_id  | FK BIGINT

corso_insegnanti:

- insegnanti_id  | FK BIGINT
- corso_id  | FK BIGINT



Relazioni:

- onetomany => dipartimenti and corsi_di_laurea
- onetomany => corso_di_laurea and corsi
- manytomany => corso and insegnanti(pivot)
- onetomany => corso and appelli
- onetoone => studente and corso_di_laurea
- manytomany => studente and appello(pivot)