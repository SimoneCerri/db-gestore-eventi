# !QUEST! #

DB Gestore Eventi

Il nostro cliente vuole creare una piattaforma per la gestione degli eventi che organizza.

Il gestionale permetterà agli `utenti` di gestire `eventi` e vedere le `prenotazioni` a loro collegati. 
Ci saranno 4 tipi di utenti, con `ruoli` diversi: 
- chi può amministrate tutti gli utenti
- chi può accedere al gestionale per creare o modificare eventi
- chi può solo accedere al gestionale senza la possibilità di modifica dei dati
- chi non ha accesso al gestionale e può solo prenotarsi agli eventi

Ogni evento potrà avere più `tag` per definirne la tipologia.
Il cliente organizza eventi solo in una serie di `location` esclusive che tramite il gestionale vuole tenere sempre sott’occhio, per capire quali eventi si terranno in ognuna di esse.

🎯 Obiettivo
Progettare il database per la piattaforma in questione, creando il diagramma ER e poi svolgendo le query fornite.

1️⃣ Milestone 1 (Diagramma ER)
Create il diagramma ER. Pensiamo a quali entità (tabelle) creare per il nostro database, definiamo le colonne e i tipi di dato per ognuna di esse e infine stabiliamo le relazioni.

Esportate quindi il diagramma come immagine e caricatelo nella repo.

2️⃣ Milestone 2 (Query SQL)
Dopo aver creato un nuovo database su phpMyAdmin e aver importato lo schema fornito (db_gestionale_eventi.sql), svolgete le query che trovate nel file query.pdf
In ogni query è indicato il numero di risultati previsti, per permettervi di verificare che sia corretta.

Dopo aver testato le vostre query con phpMyAdmin, riportatele in un file txt e caricatelo nella repo.


## Query ##

- Selezionare tutti gli eventi gratis, cioè con prezzo nullo (19)
    - SELECT * FROM events WHERE price IS NULL (19) ✅

- Selezionare tutte le location in ordine alfabetico (82)
    - SELECT * FROM locations ORDER BY name ASC (82) ✅
    
- Selezionare tutti gli eventi che costano meno di 20 euro e durano meno di 3 ore (38)
    - SELECT * FROM events WHERE price < 20 AND duration < '03:00:00' (38) ✅
    
- Selezionare tutti gli eventi di dicembre 2023 (25)
    - SELECT * FROM events WHERE start >= '2023-12' (25) ✅
    
- Selezionare tutti gli eventi con una durata maggiore alle 2 ore (823)
    - SELECT * FROM events WHERE duration > '02:00:00' (946) ❌
    
- Selezionare tutti gli eventi, mostrando nome, data di inizio, ora di inizio, ora di fine e
durata totale (1040)
    - SELECT name,start,duration,ADDTIME(TIME(start),duration) AS end FROM events (1040) ✅
    
- Selezionare tutti gli eventi aggiunti da “Fabiano Lombardo” (id: 1202) (132)
    - SELECT * FROM events WHERE user_id = 1202 (132) ✅
    
- Selezionare il numero totale di eventi per ogni fascia di prezzo (81)
    - ?
    
- Selezionare tutti gli utenti admin ed editor (9)
    - SELECT * FROM users WHERE role_id IN (1,2) (9) ✅
    
- Selezionare tutti i concerti (eventi con il tag “concerti”) (72)
    - SELECT * FROM event_tag WHERE tag_id = 1 (72) ✅
    
- Selezionare tutti i tag e il prezzo medio degli eventi a loro collegati (11)
    - SELECT tags.id, tags.name, ROUND(AVG(events.price),2) AS avg_price FROM tags JOIN event_tag ON tags.id = event_tag.tag_id JOIN events ON event_tag.event_id = events.id GROUP BY tags.id, tags.name (11) ✅
    
- Selezionare tutte le location e mostrare quanti eventi si sono tenute in ognuna di
esse (82)
    - SELECT location_id, COUNT(*) AS number_of_events  FROM events GROUP BY location_id (82) ✅

- Selezionare tutti i partecipanti per l’evento “Concerto Classico Serale” (slug:
concerto-classico-serale, id: 34) (30)
    - SELECT user_id FROM bookings WHERE event_id = 34 (30) ✅
    
- Selezionare tutti i partecipanti all’evento “Festival Jazz Estivo” (slug:
festival-jazz-estivo, id: 2) specificando nome e cognome (13)
    - SELECT users.first_name, users.last_name FROM users JOIN bookings ON bookings.user_id = users.id JOIN events ON events.id = bookings.event_id WHERE bookings.event_id = 2 (13) ✅
    
- Selezionare tutti gli eventi sold out (dove il totale delle prenotazioni è uguale ai
biglietti totali per l’evento) (18)
    - SELECT events.name FROM events JOIN bookings ON events.id = bookings.event_id GROUP BY events.name, events.total_tickets HAVING COUNT(bookings.id) = events.total_tickets (18) ✅
    
- Selezionare tutte le location in ordine per chi ha ospitato più eventi (82)
    - 
    
- Selezionare tutti gli utenti che si sono prenotati a più di 70 eventi (74)
    - 
    
- Selezionare tutti gli eventi, mostrando il nome dell’evento, il nome della location, il
numero di prenotazioni e il totale di biglietti ancora disponibili per l’evento (1040)
    - 
    