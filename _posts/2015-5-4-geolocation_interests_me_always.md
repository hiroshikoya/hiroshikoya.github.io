---
layout: post
title: GeoLocation interests me always.
date: 2015-05-04
tags: [IE,twitter,android,tech]
---

Finally, my first app for Android shipped, so I'm going to write some about this.  
Although it still has some bugs to be fixed, it gained me some experience, so I will write it down here.  
  
##About AN APP.  
***

First, show you what I've developed : [SONAA TWEET](https://play.google.com/store/apps/details?id=com.sonaatweet.koya.sonaa)
  
The concept of this app is to show tweets order by Locations.  
Usually in Twitter Apps, tweets are shown by timeline.  
  
But sometimes, like when you are in a sightseeing area, it would be fun to share a impressions about certain place.  
And at that time, freshness of tweets may not be mattered, what matters is the distance from the place where you are now.
  
When I come to develop android app, to implement some functions related to geolocation came to my mind.  
And also, Twitter is very interesting media, so wanted to use some of APIs given by Twitter. 
    

##About FABRICs & Twitter APIs
***  
FABRIC.io is a plugin to use some of TWITTER developments. 
It was very easy to use, very graphical introduction, implements many sample codes too. 
  
But to use APIs in a apps like my apps, it has some difficulty to achieve what I wanted to do first.  
It has some bugs related to geocodes: [Search API returning (very) sparse geocode results](https://twittercommunity.com/t/search-api-returning-very-sparse-geocode-results/27998).  
  
You can't get enough tweets to show, and it seems to search Tweet by API be limited in certain period.
As I tried several times, I can't get any tweets when I gave some geoinfo to Twitter API.
Maybe it's related to some matter of security problems...

##About DEVELOPMENT IN ANDROID STUDIO.
***  
As I've got used to Eclipse based IDE, I need some time to get used to Intellij kind of IDE.
It makes me new feeling that it no longer need to press cmd+s to save source.  
  
##At Last  
***
    
Though it took more time to launch one app than I estimated, it was not so difficult just to launch one.  
My app is still so coarse, needs more update...  
  
  
なんか社内マターで英文書く必要あったが一向に進まず,むしゃくしゃしてこっちも英語で書き始めた.  
もっと詳細には色々気づき(アプリのデザイン.表示にフィルタを掛けるべき.検索タイミング.Tipsとか.)があるんだけど総括というか.とりあえず.  
社内マターのめんどくささ半端ないー...  
