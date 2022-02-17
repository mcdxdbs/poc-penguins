- [Board Games through the Ages](#board-games-through-the-ages)
- [Data Profiling](#data-profile)
- [Concept Model](#concept-model)
- [DDL](#ddl)
- [Notebook](#notebook)
- [Issues and TO DOs](#issues-and-to-dos)

---

# Board Games through the Ages

`Board Games through the Ages` is a Proof of Concept (PoC) project made by Team Penguin for Daugherty University. <br />
Team Penguin comprises of a Business Analtyics and a Data Engineering student.

Selected a dataset from [Kaggle](https://www.kaggle.com/threnjen/board-games-database-from-boardgamegeek) for our PoC project.
### What we would like to accomplish?
  1. Introduce the board games from years BCE. 
  2. Ratings of earliest board games
  3. All time favorite categories and board games.

### This PoC includes:
- A data profile
- A Concept model from LucidCharts
- The Data Definition Language (DDL) used to create the tables in an RDBMS (Postgres)
- A notebook detailing how to clean, transform, and load into the RDBMS
- Visualizations using PowerBI.

# Data Profile
![image](https://user-images.githubusercontent.com/99750060/154560129-4485fde1-1c6b-49ca-ba6b-4c28efa34ecb.png)


# Concept Model
![image](https://user-images.githubusercontent.com/99750060/154523366-5aae4e38-6ff4-4ab5-b321-25434b098204.png)

# DDL

The Data Definition Language (DDL) was written inside of the notebook to create the tables required for the loading of the data. 
After cleaning the data and merging some tables we were able to have a flat table to work form. 
This was the first table incorporated into the database as a starting point.

```sql   
CREATE TABLE games_flat(
  bggid INTEGER, PRIMARY KEY,
  name TEXT,
  yearpublished INTEGER,,
  category TEXT,
  mfgplaytime INTEGER,,
  minplayers INTEGER,,
  maxplayers INTEGER,,
  avgrating REAL,
  mfgagerec INTEGER,,
  numuserratings INTEGER,
  );
```

Then we cut the flat table into smaller tables: games, users, ratings, playtime, and category.

```sql
CREATE TABLE games(
  bggid INTEGER PRIMARY KEY,
  name TEXT,
  yearpublished INTEGER,
  mfgagerec INTEGER
  );
        
CREATE TABLE users(
  bggid INTEGER PRIMARY KEY,
  minplayers INTEGER,
  maxplayers INTEGER
  );
        
CREATE TABLE ratings (
  bggid INTEGER PRIMARY KEY,
  avgrating INTEGER,
  numuserratings INTEGER
  );
            
CREATE TABLE playtime(
  BGGId INTEGER PRIMARY KEY,
  MfgPlaytime INTEGER
);
        
CREATE TABLE category(
  BGGId integer PRIMARY KEY,
  category TEXT
);        
 ```
 
In order to connect to the postggres databse from the jupyter notebook, `psycopg2` was used.
A connection was made including the host, database name, user, and password.
The user and password was made beforehand in pgadmin 4 which allowed access into the database.

You may use a different platform so your steps may be different, but here are the generalized steps to  create a new role if you do not wish to use your own username and password to connect. <br />

These are the steps to create a new role in pgadmin4:
1. Open your platform and database, find the Login/Group Roles.
2. Right click - Create - Login/Group Role
3. General Tab - Enter a name
4. Defintion Tab - Enter a password
5. Privledges Tab - Allow "Can Login?" - the others are optional
6. Right click on your database and change the Owner to the new login role you created.

Your database should now be ready to link with the notebook.


# Notebook

This monstrous dataset Team Penguin came across was the ultimate repository for boardgames throughout human history. Tens of thousands of rows that included ranks, categories, recommended timeframe and user age, ratings, and expansive columns that described each board game and the year it was published. From how this dataset is being described, you can probably guess that there was just an overwhelming load of information. A lot we didn't need in order to portray our goal as a team. We wanted to deliver a complete and fully saturated file of thousands of games without running into blank spaces and disingenuous information. 

Some problems we encountered:
-We started with about 29 thousand rows. 
-It was a massive file, categories and ranks of each game was in binary format, so each game was not assigned one category or one rank, it was assigned several and sometimes no category at all. 
Hundreds of games that were published in the year "0" was due to not being assigned a year to begin with. 

You may need to change the login credentials for psycopg2 in order for the notebook and databse to link together.


# Issues and TO DOs

Tighten/Loosen requirements for games thus having a smaller/larger set of data to work with. <br />
Analyze further detail about the games high correlation.<br />
Possible: 
- MfgPlaytime vs AvgRating
- MfgAgeRec vs AvgRating
- MinPlayers or MaxPLayers vs AvgRating
- MinPLayer/MaxPLayer vs Category





