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
I got approved for a personal API (which means it's running 24/7! yay!)
Currrently we have 2,308,884 unique players 

Migrated SQL from google cloud to a vm entity, will see how the cost turns out to be.. still having some free credits left.
