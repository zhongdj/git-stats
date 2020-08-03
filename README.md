# git-stats
Individual or Team Analysis the insight of development velocity from git based repositories.

# Start Guide

* Step 1. Launch Stack
Make sure you have docker-compose and docker installed.

```bash
docker-compose up -d
```

* Step 2. Kickoff a run with default onion structure.

```bash

curl -H "Content-Type:application/json" \
  http://localhost:9000/v1/productivity/tasks \
  -d \
  '{ 
     "repositories" : [
       { 
         "repositoryUrl": "git@github.com:zhongdj/git-stats-backend.git" , "branch" : "master", "excludes" : []
       }
     ], 
     "fromDay": "2019-04-25" 
  }'
```

output will be generated on docker-compose console as following: 
```bash
backend_1  | Cloning into '/root/.tasks/3/git-stats-backend'...
backend_1  | InsertionStatsService: 2013-04-25, 2019-06-09
backend_1  | (2013-04-25,2019-06-09,Thu Apr 25 00:00:00 GMT 2013,Sun Jun 09 00:00:00 GMT 2019,2236)
...
backend_1  |     add tweet related repositories and entities))
backend_1  |       |graphGenerated:
backend_1  |       |Layered: 1
backend_1  |       |Daily Loc/Commit: 2
backend_1  |       |Weekly Loc/Commit: 3
backend_1  |       |    )
backend_1  | akka://application/user/1/1-git@gitlab.com:architectures-lab@tweets.git-bob-spec-pojo-java
```
until graphGenerated is printed, move to next step

* Step 3. Browse Result at http://localhost:3000.
   * login by default username/password: zhongdj@gmail.com/1q2w3e4r5t
![metrics-view](https://github.com/zhongdj/git-stats/blob/master/images/2.login_metabase.png)
   * click 'Browse All Items'
![metrics-view](https://github.com/zhongdj/git-stats/blob/master/images/3.analytics_page_link.png)
   * click any of the result 
![metrics-view](https://github.com/zhongdj/git-stats/blob/master/images/7.analytics_result_page.png)

 Onion Architecture Layered Commits Distribution
![metrics-view](https://github.com/zhongdj/git-stats/blob/master/images/8.result_onion_layers.png)
 Daily Loc Per Commits Distribution
![metrics-view](https://github.com/zhongdj/git-stats/blob/master/images/9.result_daily_loc_per_commit.png)
 Weekly Loc Per Commits Distribution
![metrics-view](https://github.com/zhongdj/git-stats/blob/master/images/10.result_weekly_loc_per_commit.png)


# Additional Configurable 
## Custom Tagging on Project Directory Structure
By default, commits are tagged by the files included. Tags are matched by filePath.contains(key) condition, 
and key is defined as left hand side, tags are defined as right hand side as following by default:
```
infrastructure/=infrastructure
application/=application
domain/=domain
controller=controllers
gateway=gateway
configuration=configuration
```

Custom Tagging profile can be provided as following:
* Create a file ends with properties and put it under ~/tags folder, such as: my.onion.properties
```asciidoc
infrastructure=infrastructure
application=application
domain=domain
```

and then you can kick off a run as following, which also exclude vendor and thrift_gen directory as well:
```bash
curl -H "Content-Type:application/json" \
  http://localhost:9000/v1/productivity/tasks \
  -d \
  '{
     "repositories" : [
       {
         "repositoryUrl": "git@gitlab.com:architectures-lab/tweets.git" , "branch" : "bob-spec-pojo-java", "profile" : "my.onion", "excludes" : ["vendor", "thrift_gen" ]
       }
     ],
     "fromDay": "2020-01-01"
  }'
```

## Configure Metabase Username/Password
If you changed the metabase's admin username/password, then
Create a session.properties as following:
```bash
username=zhongdj@gmail.com
password=1q2w3e4r5t
```
and put this file under ~/metabase.
