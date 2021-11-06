+++
title= "Script for Exporting Micro.blog posts to Jekyll"
date = 2018-05-23
tags = [ "jekyll", "microblog", "python" ]
+++

As I've mentioned in previous posts, I'm a fan of [Jekyll](https://jekyllrb.com/) for building static websites, but I have also been experimenting with [Micro.blog](http://Micro.blog) for my personal site. There was a recent discussion on __Micro.blog__ looking for options to mirror posts from __Micro.blog__ to Jekyll. This question stuck with me, and as I already have a Raspberry Pi server in production for handling other daily maintenance tasks, I decided to investigate if this could be done in Python, building on my previous experience.

There are several options for exporting posts from __Micro.blog__, such as _RSS_, _JSON_ or the _MetaWeblog_ API. From my research, I discovered the _MetaWeblog_ API had one significant advantage over the other options. This API presents posts in their original _Markdown_ source format, allowing for easy export to Jekyll without translation.

Also, for the purposes of simplicity, I built the script to export all posts from the past 'x' hours, rather than tracking which posts have already been exported. This avoided the need for a backend database or log file to track post IDs. In the worst case, if a post was downloaded twice, it would simply over-write the matching post in Jekyll. Perhaps in future I will add this functionality, but I'll wait and see if it becomes an issue after long-term use.

Here is a breakdown of the current version of the script, highlighting the most important sections. I also posted the script in full [here](https://github.com/bftsystems/micro-blog-to-jekyll) on Github, so you can download or submit code improvements. As always, the first section declares any external modules and functions required by the script.

```python
#!/usr/bin/env python

import xmlrpclib
import re, string, sys, time, urllib, urlparse
from datetime import datetime, timedelta

def datetime_from_utc_to_local(utc_datetime):
    now_timestamp = time.time()
    offset = datetime.fromtimestamp(now_timestamp) - datetime.utcfromtimestamp(now_timestamp)
    return utc_datetime + offset
```

In the next section, all the __Micro.blog__ specific settings are declared. Primarily, this includes the app token, full domain name, and the maximum number of posts to read. This variable should be set to the maximum number of posts you expect between scheduled runs of the script.

```python
# Micro.blog domain details
app_token = 'xxxxxxxxxxxxxxxxxxxx'
# enter Micro.blog or custom domain name (to properly handle Micro.blog img links)
domain = 'www.thedeskofbrad.ca'
user_id = 'thedeskofbrad'
# maximum number of posts expected in time range 
max_posts = 3

# Micro.blog xml-rpc server endpoint
server = xmlrpclib.ServerProxy('https://Micro.blog/xmlrpc')
posts = server.metaWeblog.getRecentPosts(domain, user_id, app_token, max_posts)
```

As noted in the documentation, __Micro.blog__ does not use passwords, the script uses an app token to authenticate connections. To generate an app token, open __Settings__ on the __Micro.blog__ site, and click the __Edit Apps__ link at the bottom of the page. Enter a suitable name for the script, and click the __Generate Token__ link. Once created, update the _app_token_ variable in the script with this code. 

{{< image src="Create-Micro-Blog-Token.jpg" alt="Create Micro.blog Token" position="left" style="border-radius: 4px; margin-left: 2em;" >}}

The next section covers any Jekyll specific settings, such as the path to your local Jekyll repository, and the page style to be used for posts. Also included is the time range of posts to be exported by the script. On my system, the script is scheduled to run once daily, so the time range is set for 24 hours.

```python
# jekyll blog path
jekyll_root = '/home/pi/git/thedeskofbrad.github.io'
jekyll_post_layout = "single"

# time range to scan posts
time_range = datetime.now() - timedelta(hours = 24)
```

This last section does all the work of pulling each post from __Micro.blog__, checking for a title, and confirming if the post falls within the specified time range (correcting for UTC to local time). The script also downloads any image attached to the post and updates the `img` tag to use the Jekyll `site.url` variable. Currently, __Micro.blog__ only allows for one image per post, which greatly simplifies the check to a single specifically formatted `img` tag (this is required, as the __Micro.blog__ API does not use the _enclosure_ field for attachments). Once all this is done, each post is exported to Jekyll.

```python
for post in posts:
	# Correct for UTC date returned from Micro.blog API
	dateCreated = datetime_from_utc_to_local(datetime.strptime(str(post['dateCreated'])[:-1], '%Y%m%dT%H:%M:%S'))
	# Download posts with titles in selected time range
	if dateCreated > time_range and post['title'] != "":
		title = post['title']
		content = post['description']
		
		# If there is a Micro.blog img attribute, download the image to Jekyll blog assets folder
		img = re.search("(?P<url>img src=\"https?://"+domain+"[^\"]+)", content)
		if img is not None:
			img_url = img.group("url")[9:]
			img_filename = img_url.split("/")[-1] 
			download_path = jekyll_root + "/assets/images/" + img_filename
			download = urllib.URLopener()
			download.retrieve(img_url, download_path)
			content = string.replace(content, img_url, '{% raw %}{{ site.url }}{{ site.baseurl }}{% endraw %}/assets/images/' + img_filename)

		jekyll_post_filename = dateCreated.strftime("%Y-%m-%d") + '-' + title.replace(" ","-") + ".md"		
		with open(jekyll_root + "/_posts/" + jekyll_post_filename, "w") as text_file:
			text_file.write("---\n")
			text_file.write("layout: " + jekyll_post_layout + "\n")
			text_file.write("title: " + title + "\n")
			text_file.write("date: " + dateCreated.strftime("%Y-%m-%d %H:%M:%S") + "\n")
			text_file.write("---\n")
			text_file.write(content.encode("utf-8"))
```

As you can see, there isn't a lot of error checking in the code. I decided this was unnecessary as I assume the __Micro.blog__ API presents "clean" data which has been validated as part of the posting process. Of course, someone could infiltrate their servers and corrupt the database, but I considered this a low risk for my purposes.

Lastly, to ensure the script runs once every 24 hours (matching the _time_range_ specified in the script), I added it to the __crontab__ file (shown below). This configuration runs the script daily at one minute after midnight. Using __cron__ also has an added bonus, if issues are encountered by the script, the error output is included in the report emailed from __cron__.

```bash
# m h  dom mon dow   command
1 0 * * * /home/pi/bin/micro-blog-to-jekyll.sh
```

One final point, this script simply downloads new posts from __Micro.blog__ and saves them to the local Jekyll repository. Additional steps will be needed to force a rebuild of your site to actually publish any new posts. For my site using [Github Pages](https://pages.github.com), I have a script which runs daily to perform a `git commit [...]`, and `git push origin master` to make any new posts live on my site.

I hope you found this post useful, or at least interesting. If you would like to contact me or provide feedback, use the [Contact](/contact) page, or message me on [Twitter](http://twitter.com/bftsystems).