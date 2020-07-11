# git-stats
Individual or Team Analysis the insight of development velocity from git based repositories.

# Start Guide

# Launch Stack
Make sure you have docker-compose and docker installed.

```bash
docker-compose up -d
```

# Configure Metabase
Open http://localhost:3000 1st time to sign up a user, and then configure username/password.

Create a file: session.properties with user/pass configured in previous step.
```bash
username=zhongdj@gmail.com
password=1q2w3e4r5t
```
and put this file under ~/metabase.

# Kickoff a run
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

output will be generated on docker-compose console as: 
```bash
backend_1  | Cloning into '/root/.tasks/3/git-stats-backend'...
backend_1  | InsertionStatsService: 2013-04-25, 2019-06-09
backend_1  | (2013-04-25,2019-06-09,Thu Apr 25 00:00:00 GMT 2013,Sun Jun 09 00:00:00 GMT 2019,2236)
...
backend_1  | /opt/docker/stats-inserts.sh /root/.tasks/3/git-stats-backend 2019-06-07 2019-06-07
backend_1  | /opt/docker/stats-inserts.sh /root/.tasks/3/git-stats-backend 2019-06-08 2019-06-08
backend_1  | /opt/docker/stats-inserts.sh /root/.tasks/3/git-stats-backend 2019-06-09 2019-06-09
backend_1  | 2019-06-08
backend_1  | 2019-06-08
backend_1  | 2019-06-08
backend_1  | 2019-06-08
backend_1  | 2019-06-07
backend_1  | 2019-06-07
backend_1  | 2019-06-07
backend_1  | 2019-06-07
backend_1  | 2019-06-07
backend_1  | 2019-06-07
backend_1  | 2019-06-07
backend_1  | 2019-06-07

```

After the run done, screen stopped scrolling, and metrics data were generated into database, then 
you can browse database with mysql client, such as sequal pro (for mac) with configuration as:

```properties
host=127.0.0.1
port=3307
user=root
password=1q2w3e4r5t
```

![metrics-view](https://github.com/zhongdj/git-stats/blob/master/images/metrics.png)

Query Net changes Group by day:
![net-lines-changed](https://github.com/zhongdj/git-stats/blob/master/images/net.screenshot.png)
Generate Charts with Excel:
![net-lines-chart](https://github.com/zhongdj/git-stats/blob/master/images/net.chart.png)

# Known Issue

1. Same requests called twice within 5 minutes will cause error.

 
