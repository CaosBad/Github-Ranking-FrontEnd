# 太阁极客榜 (BitTiger Geeks Ranking)

**太阁极客榜**（or **BitTiger Geeks Ranking**）is a real-time ranking board for BitTiger's Github members. Its results are based on members' Github activities in the past seven days with daily updates at **06:30 PDT**.

This project consists of three components: 

1.  A crawler digs members' public data and calculates their rankings. The results is stored in the persistence layer mentioned below.
2.  A Firebase-powered persistence layer.
3.  A single-page web app shows members' rankings from the persistence layer.

> **This repository is for the front-end app showing members' rankings.** Click here if you are interested in the crawler part.

## Usage
A static web sever should be enough to launch this web app. Please use `index.html` as the [webserver directory index page](https://en.wikipedia.org/wiki/Webserver_directory_index).

## Firebase Date Structure
**Firebase URL**: [https://bittiger-ranking.firebaseio.com](https://bittiger-ranking.firebaseio.com)

```
bittiger-ranking
 |__ + user_events
 |__ + user_ranking_info
```

`user_events` is the data displayed on the ranking board. The data is set by the crawler app and retrieved by the front-end web app.

- `created_time`: the most recent update time
- `events`: a sorted array of the latest user data
- `event - <number> - ranking_history`: the user's most recent 10 rankings
 
```
user_events
 |__ created_time: "2016-05-09T18:37:44-07:00"
 |__ + events
       |__ + 0
       |__ + 1
       		 |__ CreateEvent: 11
       		 |__ ForkEvent: 4
       		 |__ PullRequestEvent: 6
       		 |__ PushEvent: 23
       		 |__ Total: 44
       		 |__ avatar_url: "https://avatars.githubusercontent.com/u/7756581?v=3"
       		 |__ html_url: "https://github.com/hackjustu"
       		 |__ login: "hackjustu"
       		 |__ organization: "bittiger"
       		 |__ ranking_change: 0
       		 |__ + ranking_history
       		 		    |__ + 0	  
       		 		    |__ + 1
       		 		          |__ ranking: 3
       		 		          |__ timestamp: "2016-05-09T06:30:03-07:00"
       		 			    
```

`user_ranking_info` helps store users' most recent 100 ranking history. As now, this data is only used by the crawler app.

- `<user1's login>`: Usually we call it the username for the Github member. It is the same value as shown in the previous `user-events - events - <number> - login`

```
user_ranking_info
     |__ + <user1's login>
     |__ + <user2's login> 
     |__ + hackjustu
     			|__ + 0
     			|__ + 1
       		 		  |__ ranking: 3
       		 		  |__ timestamp: "2016-05-08T06:30:03-07:00"
```

## Ranking Algorithm
We compare `Total`, `PushEvent`, `PullRequestEvent`, `CreateEvent` and `ForkEvent` in sequence and stop at the first available result. 

**Total = PushEvent + PullRequestEvent + CreateEvent + ForkEvent**

> **Note:** If you have multiple commits in one push event, we still count it one score. People have suggested using the number of `commit` instead of `push`. We could probably adopt this some time, but at this moment, we still count `push`.

Users can sort the members by other order such as just `PushEvent` or `ForkEvent`, but we use the ranking of `Total` for our medal system in the next section.

## Medal System
We adopt Clash of Clans's medal system for fun, beacasue I'm its big fan~~

| Medal      |    Scores (Total) |
| :--------: | :--------:| 
| Champion   | > 100     |
| Gold       | 50 - 100  |
| Silver     | 20 - 50   |
| Copper     | < 20      |

>**Note:** If you are **Super Cell** and feel unhappy about how your lovely medal icons are used here, please just let me know. I'm more than happy to change them.


## Team


## License
MIT © [hackjustu](https://github.com/hackjustu)

## Project Information











