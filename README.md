# LeagueofLegends-Project
Using Python I have extracted a large amount of data from Riot Games API, and the end goal is to practice using Pandas using a real, accurate dataset, with the hopes of creating something useful.
Jumping into this project took me a while, I had no idea how to make API calls, what kwargs are or even how to structure a function.
I have practiced reading and writing to CSV files using solely python, improved my programming logic and realised the fact that efficient code is paramount.

I have already found a use case for my project and the amount of data I am looking to scrape.
For context, League of Legends matches are played with 10 players. Everyone has a public Summoner name, and a backend user ID. 

By scraping through thousands of games, I noticed that you can track and trace who changed their public Summoner name, and at the same time, hold a history of the summoner names.
![Name History](https://user-images.githubusercontent.com/16565764/170566385-819283a2-ff94-4693-827c-b1344e868b6f.png)

You can see the start date of when I had a particular name. When the summoner name switches, that's the month when I changed it.

There is currently **NO** website that tracks this, I find it interesting. 
  
Besides this programming goal, my intention is to practice pandas, and start making some graphs, plots, etc. rather than excel.

Update 11th June 2022:
This project took a sudden and scaled up turn.
I wanted to keep this project running 24/7 and keep scraping games and gathering data.
As such, I learned how to use Google Cloud's tools, especially VM.
After setting up the VM, I wasnt't satisfied with how the project worked, it was very annoying to have to pull the data from VM everytime.

As such, I connected the project to google Psql database.

Starting from my match history, I now have over 500,000 games stored and indexed in my database. 

Wrote the code that for every game I index, I also keep track of the participant's ID, thus allowing me to branch out furter with every iteration of indexing. 

The scheme is as such:
-> Initial 800 games from me
800 games * 9 other players = 7200 players (if we don't consider duplicates)
7200 players * 800 games = 5,760,000 games 
5,760,000 games * 10 players = 57,600,000 games. 

Currently, the main issue is that I'm pulling duplicate games, and I haven't gotten around to finding a solution as to how to check if I already indexed that game. Possibly keeping a local copy of all indexed games and before writing games checking against it, but I want a more efficient solution.


Solved the duplicates issue by using pandas, pretty simple. Right now I have about 900k (unique) games to go through. 

Currently, I have over 1.3 million entires in my database, so over 130k games already parsed, at 100 requests / 2 minutes.

Update 21st July:
We've passed 10 million entires in our database. 
I got approved for a personal API (which means it's running 24/7!)
Currrently we have 2,308,884 unique players 

Migrated SQL from google cloud to a vm entity, will see how the cost turns out to be.. still having some free credits left.


Update 23rd August: 
Working on the Website, turns out it's pretty hard to build something from scratch.

Turned the google VM off as I have more than enough data to make a prototype.

Few sample queries:

Presenting
->
It all starts with a dynamic query:

  SELECT * FROM "League" WHERE "Puuid" IN 
                               (SELECT "Puuid" FROM "League" WHERE "Name" IN ('ZERI OR ZERO')
                             
In this way, a player can insert his name, and the query will select the player ID. This allows the query to interrogate the entire match history, rather than only games that contain that particular name.

  SELECT
      DISTINCT min("GameDate") as "First Game",max("GameDate") as "Last Game","Puuid","Name"
  FROM
      "League"
  WHERE
      "Puuid" NOT IN ('BOT')   
  GROUP BY
      "Puuid","Name"
  HAVING "Puuid" IN (SELECT "Puuid" FROM "League" GROUP BY "Puuid" HAVING count(DISTINCT "Name")>10)
  ORDER BY "Puuid","Name" desc;

This code will interrogate the database and search for player’s name history, allowing me to see a history of the player’s name, as well as a history of the dates that the name was used on.
This is only searching for players that had 10 or more names throughout their history.
(This works best if the entire match history of a player has been parsed)
Output is as follows:
 ![image](https://user-images.githubusercontent.com/16565764/186248262-52d431ef-cb37-42c7-b68c-7f22416161cf.png)


SELECT distinct "Name",count("Puuid"),"Puuid","GameDate" from "League"
WHERE "GameID" IN (SELECT "GameID" FROM "League" GROUP BY "GameID","Puuid" HAVING "Puuid" IN ('BOT'))
AND "Puuid" NOT IN ('BOT')
GROUP BY "GameDate","Name","Puuid"
HAVING count("Puuid") > 40
ORDER BY count("Puuid") DESC;

This query will interrogate the database and return players that have played over 40 games in a single day, as well as the count and the date.
Output:
 
![image](https://user-images.githubusercontent.com/16565764/186248252-3870f4c0-a512-4860-acec-c8d3be330e20.png)


`SELECT count("Champion") as gamesplayed,(select distinct count("GameID") from "League") as total_games_recorded,`

`(count("Champion") / SUM(count("GameID"))OVER ()*100) AS "% of total","Champion"  FROM "League"`

`GROUP BY "Champion"  ORDER BY count("Champion") desc;`


This query checks how many games a Champion participated in compared to the entire database.
It will return the Name of the Champion, as well as the play rate in percentages.
Output:
 


![image](https://user-images.githubusercontent.com/16565764/186248219-25a5cfdf-2467-4686-ae57-d210c7bca36d.png)
