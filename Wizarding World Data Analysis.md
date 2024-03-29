# Wizarding World Data Analysis - Wizards and Their Patronuses
This repository contains an SQL project that illustrates a whimsical exploration of the Wizarding World, revolving around its celebrities and their patronuses. The dataset presents a combination of fictional characters from popular literature, primarily the Harry Potter series, and aligns them with their professions and magical patronuses.

Two main tables constitute the dataset in this project: celebrities_wizards and patronuses.

## Dataset Description
The celebrities_wizards table contains information about popular characters from the Wizarding World, including their names and professions.
Each character is assigned a unique id which serves as a primary key for the table.

The patronuses table holds data on magical patronuses, which are protective entities conjured by wizards and witches. 
The id field in this table acts as a primary key, and it aligns with the id in the celebrities_wizards table, establishing a relationship 
between the two tables.

### SQL Queries
This project incorporates SQL queries that retrieve and join data from both tables. This way, we can correlate each character with their 
patronus. Note that not every character in the Wizarding World has a known patronus, and thus some fields in the patronuses table may be 
left null.

Through these queries, we create an enjoyable mashup of the Wizarding World, observing the relationship between celebrities and their 
respective patronuses. This project demonstrates how SQL can be utilized for data analysis, even when exploring fictional universes.

Feel free to clone the repository, run the queries, and immerse yourself in this magical data exploration!*/
CREATE TABLE celebrities_wizards (
id INTEGER PRIMARY KEY,
name TEXT,
profession TEXT);

INSERT INTO celebrities_wizards (id, name, profession) VALUES (1, "Hermione Granger", "healer");
INSERT INTO celebrities_wizards (id, name, profession) VALUES (2, "Draco Malfoy", "High_Reeve");
INSERT INTO celebrities_wizards (id, name, profession) VALUES (3,"Ron Weasley", "sidekick");
INSERT INTO celebrities_wizards (id, name, profession) VALUES (4,"Lord Voldemort", "main_character_vibes" );
INSERT INTO celebrities_wizards (id, name, profession) VALUES (5, "Bellatrix Lestrange", "mommy");


SELECT * FROM CELEBRITIES_WIZARDS;

CREATE TABLE patronuses (
id  INTEGER PRIMARY KEY,
patronus TEXT);

INSERT INTO patronuses (id, patronus) VALUES (1, 'dragon');
INSERT INTO patronuses (id, patronus) VALUES (2, 'otter');
INSERT INTO patronuses (id, patronus) VALUES (3, 'russell terrier');
INSERT INTO patronuses (id) VALUES (4);
INSERT INTO patronuses (id) VALUES (5);


SELECT celebrities_wizards.name, patronuses.patronus 
FROM celebrities_wizards
JOIN patronuses
ON celebrities_wizards.id = patronuses.id

