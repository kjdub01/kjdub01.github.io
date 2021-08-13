---
layout: post
title:      "Ruby Refresh II"
date:       2021-08-13 06:14:09 +0000
permalink:  ruby_refresh_ii
---


This week I have been working more on polishing up my Ruby CLI project. Last week I managed to scrape the date of the games from where i was getting the individual game infomation. This didn't seem like the right place to do it though because I ended up with an array full of the same dates. To fix this problem I picked a new place to scrape the date information from.

```
scraper.rb

class RugbyGames::Scraper
  BASE_URL = "http://www.espn.com"
  def self.scrape_espn
    @doc = Nokogiri::HTML(URI.open("http://www.espn.com/rugby/scoreboard?"))

    @doc.css("div.carousel-day").each do |date|
      games_date = RugbyGames::Dates.new
      games_date.text = date.css("span#sbpDate.date").text
      games_date.save
    end
   
    @doc.css("a.competitors").each do |contest|

      game = RugbyGames::Games.new
		
			Rugby Games 7/8/2021
1. 12:00 PM EDT -- South Africa vs British and Irish Lions
2. 03:05 AM EDT -- New Zealand vs Australia
Enter the list number to get more information about a match or type list to list games or type exit to end

Scores for Aug 14th, 2021
1. 05:05 PM EDT -- South Africa vs Argentina
2. 07:05 PM EDT -- New Zealand vs Australia
Enter the list number to get more information about a match or type list to list games or type exit to end
```


Scraping the date information from a new place gave em cleaner information than before as well.
			
I also added back in the game url to the addtional information you can get when you select a ruby game. Then I decided to scrape each teams win, tie, loss record as well and add it the the list view. I'll probably go back in this week and do some regex work to either get rid of the begiining text "Form:" or replce it with "Record:" I think though that next week will be mostly spent reviewing my sinatra project and then deciding if it's ready for deployment.





```scraper.rb

@doc.css("a.competitors").each do |contest|

      game = RugbyGames::Games.new
      game.url = BASE_URL + contest.attr("href")
      game.time = contest.css("span.game-time").text
      game.home_team = contest.css("div.team.team-a.possession span.short-name").text
      game.home_score = contest.css("div.team.team-a.possession div.score.icon-font-after").text
      game.home_record = contest.css("div.team.team-a.possession span.record").text
      game.away_team = contest.css("div.team.team-b.possession span.short-name").text
      game.away_score = contest.css("div.team.team-b.possession div.score.icon-font-before").text
      game.away_record = contest.css("div.team.team-b.possession span.record").text
      game.save
    end
		
		def list_games
    date = RugbyGames::Dates.game_day.last
    puts "#{date.text}"
		
		RugbyGames::Games.today.each.with_index(1) do |game, i|
      puts "#{i}. #{game.time} -- #{game.home_team} #{game.home_record} vs #{game.away_team} #{game.away_record}"
    end
  end
	...
	
	if input.to_i > 0 && input.to_i <= RugbyGames::Games.today.size
        selected_game = RugbyGames::Games.today[input.to_i - 1] 
        puts "#{selected_game.home_team}  #{selected_game.home_score}  #{selected_game.time}  #{selected_game.away_score}  #{selected_game.away_team}"
        puts "#{selected_game.url}"


********************************************************************************************
Scores for Aug 14th, 2021
1. 05:05 PM EDT -- South Africa Form: WWLW vs Argentina Form: WTTTL
2. 07:05 PM EDT -- New Zealand Form: WWWWW vs Australia Form: LWLWT
Enter the list number to get more information about a match or type list to list games or type exit to end

1
South Africa    05:05 PM EDT    Argentina
http://www.espn.com/rugby/match?gameId=593791&league=289232
Enter the list number to get more information about a match or type list to list games or type exit to end```
			
			
