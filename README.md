# UniStay

UniStay è un sistema informativo per la gestione di prenotazioni di alloggi, ispirato al funzionamento di piattaforme come Airbnb, ma progettato in forma semplificata per il progetto di Programmazione Object Oriented e Basi di Dati.


### Obiettivo del progetto

L’obiettivo del sistema è permettere a utenti di tipo diverso di interagire con una piattaforma di prenotazione: gli **Host** possono pubblicare e gestire annunci relativi ai propri alloggi, mentre i **Guest** possono cercare annunci disponibili, effettuare prenotazioni, gestire i pagamenti e lasciare recensioni.
Il sistema è progettato per mostrare:
- una corretta modellazione del dominio applicativo;
- l’uso dell’ereditarietà tramite la generalizzazione `User → Host / Guest`;
- la separazione delle responsabilità tra interfaccia, logica applicativa, modello dati e accesso al database;
- l’utilizzo del pattern BCE insieme al pattern DAO;
- la gestione persistente dei dati tramite PostgreSQL.

### Architettura generale

UniStay segue il pattern architetturale **BCE + DAO**.
La struttura del progetto è divisa nei seguenti livelli:

### Boundary / GUI

Il livello Boundary gestisce l’interazione con l’utente.  
Contiene le classi responsabili della visualizzazione delle schermate e della raccolta degli input.

Classi principali:
- `LoginView`
- `ListingView`
- `BookingView`

Responsabilità:
- mostrare le schermate dell’applicazione;
- ricevere dati dall’utente;
- inviare le richieste ai controller;
- visualizzare messaggi di conferma o errore.

### Control / Controller

Il livello Control contiene la logica applicativa.  
I controller coordinano le operazioni tra GUI, model e DAO.

Classi principali:
- `UserController`
- `ListingController`
- `BookingController`

Responsabilità:
- gestire registrazione e login;
- coordinare la creazione e ricerca degli annunci;
- gestire il processo di prenotazione;
- verificare la disponibilità degli alloggi;
- calcolare il prezzo finale della prenotazione;
- controllare la correttezza dei dati ricevuti dalla GUI.

### Entity / Model

Il livello Entity rappresenta il dominio del problema.  
Contiene le classi che modellano gli oggetti principali del sistema.

Classi principali:
- `User`
- `Host`
- `Guest`
- `Listing`
- `Booking`
- `Payment`
- `Review`

La classe `User` rappresenta un utente generico ed è specializzata in:
- `Host`, cioè l’utente che pubblica annunci;
- `Guest`, cioè l’utente che effettua prenotazioni.

Questa generalizzazione permette di rappresentare ruoli diversi mantenendo attributi comuni come nome, email e password.

### DAO / Persistenza

Il livello DAO gestisce l’accesso al database.  
Le interfacce DAO definiscono le operazioni disponibili, mentre le classi DAOImpl contengono l’implementazione concreta tramite JDBC.

Interfacce:
- `UserDAO`
- `ListingDAO`
- `BookingDAO`
- `PaymentDAO`
- `ReviewDAO`

Implementazioni:
- `UserDAOImpl`
- `ListingDAOImpl`
- `BookingDAOImpl`
- `PaymentDAOImpl`
- `ReviewDAOImpl`

Responsabilità:
- recuperare dati dal database;
- salvare nuovi record;
- aggiornare informazioni esistenti;
- eliminare o annullare elementi;
- separare la logica SQL dalla logica applicativa.

### Database

Il database sarà realizzato in **PostgreSQL**.

Conterrà le tabelle relative a:
- utenti;
- host;
- guest;
- annunci;
- prenotazioni;
- pagamenti;
- recensioni.

Saranno inoltre previsti vincoli e trigger per garantire l’integrità dei dati, ad esempio:
- impedire prenotazioni con date non valide;
- evitare sovrapposizioni tra prenotazioni dello stesso annuncio;
- controllare lo stato dei pagamenti;
- mantenere coerenti prenotazioni, annunci e recensioni.

### Flusso principale dell’applicazione

Il funzionamento generale di UniStay segue questo flusso:

1. L’utente accede o si registra tramite la schermata di login.
2. Il sistema distingue il ruolo dell’utente tra Host e Guest.
3. Un Host può creare e aggiornare annunci.
4. Un Guest può cercare annunci disponibili.
5. Il Guest seleziona un annuncio e inserisce le date della prenotazione.
6. Il sistema verifica la disponibilità dell’alloggio.
7. Se la prenotazione è valida, viene creato un oggetto `Booking`.
8. Il sistema calcola il prezzo finale.
9. Viene gestito il pagamento tramite `Payment`.
10. Dopo il soggiorno, il Guest può lasciare una recensione.

### Tecnologie utilizzate

- Java
- Apache Maven
- PostgreSQL
- JDBC
- UML
- GitHub

### Struttura prevista del progetto

```text
src/
 └── main/
     └── java/
         ├── gui/
         ├── controller/
         ├── model/
         ├── dao/
         ├── dao/impl/
         └── db/

sql/
 ├── schema.sql
 ├── data.sql
 └── triggers.sql

docs/
 ├── class-diagram.pdf
 └── relazione.pdf
