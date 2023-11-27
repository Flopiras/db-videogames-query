## Todo

SELECT

1- Selezionare tutte le software house americane (3)

```sql
SELECT * FROM `software_houses`
    WHERE country like 'United States';
```

2- Selezionare tutti i giocatori della cittÃ di 'Rogahnland' (2)

```sql
SELECT * FROM `players`
    WHERE city LIKE 'Rogahnland';
```

3- Selezionare tutti i giocatori il cui nome finisce per "a" (220)

```sql
SELECT * FROM `players`
    WHERE name LIKE '%a';
```

4- Selezionare tutte le recensioni scritte dal giocatore con ID = 800 (11)

```sql
SELECT * FROM `reviews`
    WHERE player_id = 800;
```

5- Contare quanti tornei ci sono stati nell'anno 2015 (9)

```sql
SELECT COUNT(*) FROM `tournaments`
    WHERE year = 2015;
```

6- Selezionare tutti i premi che contengono nella descrizione la parola 'facere' (2)

```sql
SELECT * FROM `awards`
    WHERE description LIKE '%facere%';
```

7- Selezionare tutti i videogame che hanno la categoria 2 (FPS) o 6 (RPG), mostrandoli una sola volta (del videogioco vogliamo solo l'ID) (287)

```sql
SELECT DISTINCT videogame_id
    FROM category_videogame
    WHERE category_id = 2
        OR category_id = 6;
```

8- Selezionare tutte le recensioni con voto compreso tra 2 e 4 (2947)

```sql
SELECT * FROM `reviews`
	WHERE rating BETWEEN 2 and 4;
```

9- Selezionare tutti i dati dei videogiochi rilasciati nell'anno 2020 (46)

```sql
SELECT * FROM `videogames`
    WHERE YEAR(release_date) = 2020;
```

10- Selezionare gli id videogame che hanno ricevuto almeno una recensione da 5 stelle, mostrandoli una sola volta (443)

```sql
SELECT DISTINCT videogames.id
    FROM videogames
    JOIN reviews
        ON videogames.id = reviews.videogame_id
    WHERE reviews.rating = 5;
```

\***\*\*\*\*\*\*** BONUS \***\*\*\*\*\*\***

11- Selezionare il numero e la media delle recensioni per il videogioco con ID = 412 (review number = 12, avg_rating = 3.16 circa)

```sql
SELECT
    COUNT(*) AS numero_recensioni,
        AVG(rating) AS media_voti
	FROM reviews
	WHERE videogame_id = 412;
```

12- Selezionare il numero di videogame che la software house con ID = 1 ha rilasciato nel 2018 (13)

```sql
SELECT COUNT(*) FROM `videogames`
	WHERE software_house_id = 1
		AND YEAR(release_date) = 2018;
```

---

GROUP BY

1- Contare quante software house ci sono per ogni paese (3)

```sql
SELECT country,
    COUNT(*) as 'number'
	    FROM `software_houses`
GROUP BY country;
```

2- Contare quante recensioni ha ricevuto ogni videogioco (del videogioco vogliamo solo l'ID) (500)

```sql
SELECT videogame_id,
	COUNT(*) AS 'number'
FROM `reviews`
GROUP BY videogame_id;
```

3- Contare quanti videogiochi hanno ciascuna classificazione PEGI (della classificazione PEGI vogliamo solo l'ID) (13)

```sql
SELECT pegi_label_id AS 'PEGI',
	COUNT(*) as 'numero videogiochi'
FROM `pegi_label_videogame`
GROUP BY pegi_label_id;
```

4- Mostrare il numero di videogiochi rilasciati ogni anno (11)

```sql
SELECT YEAR(release_date) AS 'anno',
    COUNT(*) AS 'quantità videogiochi'
FROM `videogames`
GROUP BY YEAR(release_date);
```

5- Contare quanti videogiochi sono disponbiili per ciascun device (del device vogliamo solo l'ID) (7)

```sql
SELECT device_id,
COUNT(*) AS 'videogiochi'
FROM `device_videogame`
GROUP BY device_id;
```

6- Ordinare i videogame in base alla media delle recensioni (del videogioco vogliamo solo l'ID) (500)

```sql
SELECT videogame_id, AVG(rating) AS 'media voti',
    COUNT(*) AS 'numero recensioni'
	FROM reviews
    GROUP BY videogame_id
    ORDER BY 'media voti';
```

---

JOIN

1- Selezionare i dati di tutti giocatori che hanno scritto almeno una recensione, mostrandoli una sola volta (996)

```sql
SELECT DISTINCT players.*
	FROM players
	JOIN reviews
    	ON players.id= reviews.player_id;
```

2- Sezionare tutti i videogame dei tornei tenuti nel 2016, mostrandoli una sola volta (226)

```sql
SELECT DISTINCT videogames.*
	FROM `videogames`
    JOIN tournament_videogame
    	ON tournament_videogame.videogame_id = videogames.id
       	JOIN tournaments
        	ON tournament_videogame.tournament_id = tournaments.id
    WHERE tournaments.year = 2016;
```

3- Mostrare le categorie di ogni videogioco (1718)

```sql
SELECT videogames.name, categories.name
    FROM videogames
    JOIN category_videogame
        ON videogames.id = category_videogame.videogame_id
    JOIN categories
        ON category_videogame.category_id = categories.id;
```

4- Selezionare i dati di tutte le software house che hanno rilasciato almeno un gioco dopo il 2020, mostrandoli una sola volta (6)

```sql
SELECT DISTINCT software_houses.*
    FROM software_houses
    JOIN videogames
        ON software_houses.id = videogames.software_house_id
    WHERE YEAR(videogames.release_date) > 2020;
```

5- Selezionare i premi ricevuti da ogni software house per i videogiochi che ha prodotto (55)

```sql
SELECT
    software_houses.id AS ID_software_house,
    software_houses.name AS nome_software_house,
    awards.name AS nome_premio
    FROM
        software_houses
    JOIN videogames
            ON software_houses.id = videogames.software_house_id
    JOIN award_videogame
            ON videogames.id = award_videogame.videogame_id
    JOIN awards
        ON award_videogame.award_id = awards.id;
```

6- Selezionare categorie e classificazioni PEGI dei videogiochi che hanno ricevuto recensioni da 4 e 5 stelle, mostrandole una sola volta (3363)

```sql
SELECT DISTINCT videogames.name 'videogameName', categories.name 'categoryName', pegi_labels.name 'pegiName'
FROM videogames
    JOIN reviews
        ON videogames.id = reviews.videogame_id
    JOIN category_videogame
        ON videogames.id = category_videogame.videogame_id
    JOIN categories
        ON category_videogame.category_id = categories.id
    JOIN pegi_label_videogame
        ON videogames.id = pegi_label_videogame.videogame_id
    JOIN pegi_labels
        ON pegi_label_videogame.pegi_label_id = pegi_labels.id
WHERE reviews.rating >= 4;
```

7- Selezionare quali giochi erano presenti nei tornei nei quali hanno partecipato i giocatori il cui nome inizia per 'S' (474)

```sql
SELECT videogames.*
FROM players
    JOIN player_tournament
        ON players.id = player_tournament.player_id
    JOIN tournaments
        ON player_tournament.tournament_id = tournaments.id
    JOIN tournament_videogame
        ON tournaments.id = tournament_videogame.tournament_id
    JOIN videogames
        ON tournament_videogame.videogame_id = videogames.id
WHERE players.name LIKE 's%';
```

8- Selezionare le cittÃ in cui Ã¨ stato giocato il gioco dell'anno del 2018 (36)

```sql
SELECT tournaments.city
FROM awards
    JOIN award_videogame
        ON awards.id = award_videogame.award_id
    JOIN videogames
        ON award_videogame.videogame_id = videogames.id
    JOIN tournament_videogame
        ON videogames.id = tournament_videogame.videogame_id
    JOIN tournaments
        ON tournaments.id = tournament_videogame.tournament_id
WHERE award_videogame.year = 2018
    AND awards.name LIKE "gioco dell'anno";
```

9- Selezionare i giocatori che hanno giocato al gioco piÃ¹ atteso del 2018 in un torneo del 2019 (3306)

```sql
SELECT players.*
FROM videogames
    JOIN award_videogame
        ON videogames.id = award_videogame.videogame_id
    JOIN awards
        ON award_videogame.award_id = awards.id
    JOIN tournament_videogame
        ON videogames.id = tournament_videogame.videogame_id
    JOIN tournaments
        ON tournament_videogame.tournament_id = tournaments.id
    JOIN player_tournament
        ON tournaments.id = player_tournament.tournament_id
    JOIN players
        ON player_tournament.player_id = players.id
WHERE awards.name LIKE "Gioco più atteso"
    AND award_videogame.year = 2018
    AND tournaments.year = 2019;
```

\***\*\*\*\*\*\*** BONUS \***\*\*\*\*\*\***

10- Selezionare i dati della prima software house che ha rilasciato un gioco, assieme ai dati del gioco stesso (software house id : 5)

```sql
SELECT software_houses.id, software_houses.name 'softwareHouse', videogames.name 'videogame', videogames.release_date
FROM videogames
    JOIN software_houses
        ON videogames.software_house_id = software_houses.id
ORDER BY videogames.release_date
LIMIT 1;
```

11- Selezionare i dati del videogame (id, name, release_date, totale recensioni) con più recensioni (videogame id : potrebbe uscire 449 o 398, sono entrambi a 20)

```sql
SELECT videogames.id, videogames.name, COUNT(*) 'revCount'
FROM videogames
    JOIN reviews
        ON videogames.id = reviews.videogame_id
GROUP BY videogames.id
ORDER BY revCount DESC
LIMIT 1;
```

12- Selezionare la software house che ha vinto piÃ¹ premi tra il 2015 e il 2016 (software house id : potrebbe uscire 3 o 1, sono entrambi a 3)

```sql
SELECT software_houses.id, software_houses.name, COUNT(*) as 'awardCount'
FROM videogames
    JOIN award_videogame
        ON videogames.id = award_videogame.videogame_id
    JOIN software_houses
        ON software_houses.id = videogames.software_house_id
WHERE award_videogame.year BETWEEN 2015 AND 2016
GROUP BY software_houses.id
ORDER BY awardCount DESC
LIMIT 1;
```

13- Selezionare le categorie dei videogame i quali hanno una media recensioni inferiore a 2 (10)

```sql
SELECT DISTINCT categories.name
FROM videogames
    JOIN reviews
        ON videogames.id = reviews.videogame_id
    JOIN category_videogame
        ON videogames.id = category_videogame.videogame_id
    JOIN categories
        ON category_videogame.category_id = categories.id
GROUP BY categories.id, videogames.id
HAVING AVG(reviews.rating) < 2;
```
