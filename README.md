## Todo

SELECT

1- Selezionare tutte le software house americane (3)

```sh
SELECT * FROM `software_houses`
    WHERE country like 'United States';
```

2- Selezionare tutti i giocatori della cittÃ di 'Rogahnland' (2)

```sh
SELECT * FROM `players`
    WHERE city LIKE 'Rogahnland';
```

3- Selezionare tutti i giocatori il cui nome finisce per "a" (220)

```sh
SELECT * FROM `players`
    WHERE name LIKE '%a';
```

4- Selezionare tutte le recensioni scritte dal giocatore con ID = 800 (11)

```sh
SELECT * FROM `reviews`
    WHERE player_id = 800;
```

5- Contare quanti tornei ci sono stati nell'anno 2015 (9)

```sh
SELECT COUNT(*) FROM `tournaments`
    WHERE year = 2015;
```

6- Selezionare tutti i premi che contengono nella descrizione la parola 'facere' (2)

```sh
SELECT * FROM `awards`
    WHERE description LIKE '%facere%';
```

7- Selezionare tutti i videogame che hanno la categoria 2 (FPS) o 6 (RPG), mostrandoli una sola volta (del videogioco vogliamo solo l'ID) (287)

```sh

```

8- Selezionare tutte le recensioni con voto compreso tra 2 e 4 (2947)

```sh
SELECT * FROM `reviews`
	WHERE rating BETWEEN 2 and 4;
```

9- Selezionare tutti i dati dei videogiochi rilasciati nell'anno 2020 (46)

```sh
SELECT * FROM `videogames`
    WHERE YEAR(release_date) = 2020;
```

10- Selezionare gli id videogame che hanno ricevuto almeno una recensione da 5 stelle, mostrandoli una sola volta (443)

```sh
SELECT DISTINCT videogames.id
    FROM videogames
    JOIN reviews
        ON videogames.id = reviews.videogame_id
    WHERE reviews.rating = 5;
```

\***\*\*\*\*\*\*** BONUS \***\*\*\*\*\*\***

11- Selezionare il numero e la media delle recensioni per il videogioco con ID = 412 (review number = 12, avg_rating = 3.16 circa)

```sh
SELECT
    COUNT(*) AS numero_recensioni,
        AVG(rating) AS media_voti
	FROM reviews
	WHERE videogame_id = 412;
```

12- Selezionare il numero di videogame che la software house con ID = 1 ha rilasciato nel 2018 (13)

```sh
SELECT COUNT(*) FROM `videogames`
	WHERE software_house_id = 1
		AND YEAR(release_date) = 2018;
```

---

GROUP BY

1- Contare quante software house ci sono per ogni paese (3)

```sh
SELECT country,
    COUNT(*) as 'number'
	    FROM `software_houses`
GROUP BY country;
```

2- Contare quante recensioni ha ricevuto ogni videogioco (del videogioco vogliamo solo l'ID) (500)

```sh
SELECT videogame_id,
	COUNT(*) AS 'number'
FROM `reviews`
GROUP BY videogame_id;
```

3- Contare quanti videogiochi hanno ciascuna classificazione PEGI (della classificazione PEGI vogliamo solo l'ID) (13)

```sh
SELECT pegi_label_id AS 'PEGI',
	COUNT(*) as 'numero videogiochi'
FROM `pegi_label_videogame`
GROUP BY pegi_label_id;
```

4- Mostrare il numero di videogiochi rilasciati ogni anno (11)

```sh
SELECT YEAR(release_date) AS 'anno',
    COUNT(*) AS 'quantità videogiochi'
FROM `videogames`
GROUP BY YEAR(release_date);
```

5- Contare quanti videogiochi sono disponbiili per ciascun device (del device vogliamo solo l'ID) (7)

```sh
SELECT device_id,
COUNT(*) AS 'videogiochi'
FROM `device_videogame`
GROUP BY device_id;
```

6- Ordinare i videogame in base alla media delle recensioni (del videogioco vogliamo solo l'ID) (500)

```sh
SELECT videogame_id, AVG(rating) AS 'media voti',
    COUNT(*) AS 'numero recensioni'
	FROM reviews
    GROUP BY videogame_id;
```

---

JOIN

1- Selezionare i dati di tutti giocatori che hanno scritto almeno una recensione, mostrandoli una sola volta (996)

```sh
SELECT DISTINCT players.*
	FROM players
	JOIN reviews
    	ON players.id= reviews.player_id;
```

2- Sezionare tutti i videogame dei tornei tenuti nel 2016, mostrandoli una sola volta (226)

```sh
SELECT DISTINCT videogames.*
	FROM `videogames`
    JOIN tournament_videogame
    	ON tournament_videogame.videogame_id = videogames.id
       	JOIN tournaments
        	ON tournament_videogame.tournament_id = tournaments.id
    WHERE tournaments.year = 2016;
```

3- Mostrare le categorie di ogni videogioco (1718)

```sh

```

4- Selezionare i dati di tutte le software house che hanno rilasciato almeno un gioco dopo il 2020, mostrandoli una sola volta (6)

```sh
SELECT DISTINCT software_houses.*
    FROM software_houses
    JOIN videogames
        ON software_houses.id = videogames.software_house_id
    WHERE YEAR(videogames.release_date) > 2020;
```

5- Selezionare i premi ricevuti da ogni software house per i videogiochi che ha prodotto (55)

```sh
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

```sh

```

7- Selezionare quali giochi erano presenti nei tornei nei quali hanno partecipato i giocatori il cui nome inizia per 'S' (474)

```sh

```

8- Selezionare le cittÃ in cui Ã¨ stato giocato il gioco dell'anno del 2018 (36)

```sh

```

9- Selezionare i giocatori che hanno giocato al gioco piÃ¹ atteso del 2018 in un torneo del 2019 (3306)

```sh

```

\***\*\*\*\*\*\*** BONUS \***\*\*\*\*\*\***

10- Selezionare i dati della prima software house che ha rilasciato un gioco, assieme ai dati del gioco stesso (software house id : 5)

```sh

```

11- Selezionare i dati del videogame (id, name, release_date, totale recensioni) con piÃ¹ recensioni (videogame id : potrebbe uscire 449 o 398, sono entrambi a 20)

```sh

```

12- Selezionare la software house che ha vinto piÃ¹ premi tra il 2015 e il 2016 (software house id : potrebbe uscire 3 o 1, sono entrambi a 3)

```sh

```

13- Selezionare le categorie dei videogame i quali hanno una media recensioni inferiore a 2 (10)

```sh

```
