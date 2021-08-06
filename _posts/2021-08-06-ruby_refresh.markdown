---
layout: post
title:      "Ruby Refresh"
date:       2021-08-06 06:00:06 +0000
permalink:  ruby_refresh
---


I have been feeling a little far from ruby basics recently, I have been mostly working on frontend projects as my time as student wrapped up. So I went back to my first ruby project.

I wrote this rugby_games CLI so that I could look up the scores of games I had missed because most games are happening while I'm sleeping in the US. It looked something like this

```
Today's Rugby Games
1. CANCELED -- Gloucester Rugby vs Bath Rugby
2. FT -- Newcastle Falcons vs Worcester Warriors
3. FT -- Leicester Tigers vs Bristol Rugby
4. FT -- London Irish vs Wasps
5. FT -- Bayonne vs Stade Francais Paris
6. FT -- Castres Olympique vs Toulon
7. FT -- Clermont Auvergne vs La Rochelle
8. FT -- Lyon vs Agen
9. FT -- Pau vs Montpellier Herault
10. FT -- Racing 92 vs Brive
11. FT -- Bordeaux Begles vs Stade Toulousain
12. FT -- Highlanders vs New South Wales Waratahs
13. FT -- Brumbies vs Hurricanes
14. FT -- Burkina Faso vs Burundi
Enter the list number to get more information about a match or type list to list games or type exit to end
9
Pau  41  FT  25  Montpellier Herault
Enter the list number to get more information about a match or type list to list games or type exit to end
```

## The problems

This current ittereation has a hard coded date in the espn url. I'm still trying to work out how to fix this so that we don't ever have days where there are no games but part of the problem is just how espn makes the url slug. 

To aid the user until I can make this update I wanted to get rid of the hard coded date int the slug and then also scrap a date to put at the top so the user would know when the games where happening.

To do this i made another class called dates and then scrapped the dates from the page

```class RugbyGames::Dates
    attr_accessor :text
  
  @@all = []
  
  def self.game_day
    @@all
  end
  
  def save 
    @@all << self
  end
end
```
```
class RugbyGames::Scraper
  BASE_URL = "http://www.espn.com"
  def self.scrape_espn
    @doc = Nokogiri::HTML(URI.open("http://www.espn.com/rugby/scoreboard?"))
   
    @doc.css("a.competitors").each do |contest|
      date = RugbyGames::Dates.new
      date.text = contest.css("span.game-date").text
      date.save

      game = RugbyGames::Games.new
      #game.url = BASE_URL + contest.attr("href")
      game.time = contest.css("span.game-time").text
      game.home_team = contest.css("div.team.team-a.possession span.short-name").text
      game.home_score = contest.css("div.team.team-a.possession div.score.icon-font-after").text
      game.away_team = contest.css("div.team.team-b.possession span.short-name").text
      game.away_score = contest.css("div.team.team-b.possession div.score.icon-font-before").text
      game.save
    end
  end
end
```

The date scrapped here is only the month and day but I want to add the year so in the CLI file I created a variablefor the year. so that I could add it to the title.

```
def list_games
    date = RugbyGames::Dates.game_day.first
    year = Time.now.year
    puts "Rugby Games #{date.text}/#{year}"

    RugbyGames::Games.today.each.with_index(1) do |game, i|
      puts "#{i}. #{game.time} -- #{game.home_team} vs #{game.away_team}"
    end
  end
	```
	
	Now when we call rugby-games we get
	```
	Rugby Games 7/8/2021
1. 12:00 PM EDT -- South Africa vs British and Irish Lions
2. 03:05 AM EDT -- New Zealand vs Australia
Enter the list number to get more information about a match or type list to list games or type exit to end
1
South Africa    12:00 PM EDT    British and Irish Lions
Enter the list number to get more information about a match or type list to list games or type exit to end
```

## Next Steps
This still needs some refractoring to make it more eloquent. I don't actually think I need the new class at all for example but it was nice to work with just plain ruby a little bit and get those muscles flexing again.
	
