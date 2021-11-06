+++
title = "Simple script to monitor for website changes"
date = 2018-06-29
tags = [ "hosting" ]
+++

As I may have mentioned in previous posts, I run several small websites, both professionally and for personal interests. The majority are simple static websites, or low volume blogs which only expect a couple of updates in a week. With the various issues recently of crypto-miners and other exploits, I wanted a process in place to monitor for site issues, especially if the site changed unexpectedly. Of course, I could setup a trigger using [IFTTT](https://ifttt.com) to monitor the sites and notify me in the event of changes, but as always, I wanted a “home-grown” solution I could customize. As I already have several VMs in production performing various chores for me, I decided these would be perfect for the job.

For the sake of simplicity, I wrote a simple __bash__ script which downloads the site home page using __curl__ and compares if the page has changed from the previous run. This has the added benefit if the site is down, an error will be generated also causing an alert. If an issue is identified, the script sends an email notifying me to investigate.

```bash
#!/bin/bash
                                                                                                                            
# Monitors a website for changes and sends notification

DIRECTORY=`dirname $0`
CURRENT=$DIRECTORY/current.html
PREVIOUS=$DIRECTORY/previous.html
URL="https://thedeskofbrad.ca"

mv $CURRENT $PREVIOUS 2> /dev/null
curl $URL -L --compressed -s > $CURRENT
DIFF_OUTPUT="$(diff $CURRENT $PREVIOUS)"
if [ "0" != "${#DIFF_OUTPUT}" ]; then
  echo "Site https://thedeskofbrad.ca has changed" | mail -s "Site update report" email@domain.com
fi
```

As most of the sites being monitored are low volume sites, I decided to run this script every 8 hours using __cron__. To do this, I used the command `crontab -e` and added the following line for this purpose.

```bash
# m h  dom mon dow   command
0 */8 * * * /home/pi/bin/thedeskofbrad/monitor.sh
```

With this setup, if the site goes down or is compromised somehow, I will be notified by email within 8 hours at most. Depending on the importance of the site, I could decrease the interval to as low as once per minute, but 8 hours seemed enough for my purposes. 

As this is a simple script which I developed specifically for my purposes, it has some obvious caveats. For example, if a page other than the home page is changed, it will not be reported. Also, if in future I add dynamic content such as __Google Ads__, this would break functionality, as the ads will change on each page request. If you decide to use this script for yourself, take these issues into consideration. 

As always, I hope you found this post useful, or at least interesting. To contact me, please use the [Contact](/contact) page, or message me on [Twitter](https://twitter.com/bftsystems).

Thanks for reading.
