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

Several of the players that I met throughout my 800+ games changed their name at one point, and I met them again under a different name. 
My current goal:
-> For all of my 800 games I want to extract all game history of all other participants:
  -> This means 7200 players.
  -> For all players, I think I will be able to extract 500-600 games for each one.
  -> In total 420,000 games will be stored and analyzed for this project, and seeing which players have changed their name, and when.
  
Besides this programming goal, my intention is to practice pandas, and start making some graphs, plots, etc. rather than excel.
