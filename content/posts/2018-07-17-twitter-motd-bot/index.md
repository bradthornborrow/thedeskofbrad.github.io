+++
title = "Build a Twitter Message of the Day (MOTD) Bot"
date = 2018-07-17
tags = [ "twitter", "python" ]
+++

I was first introduced to the concept of the [message of the day (MOTD)](https://en.wikipedia.org/wiki/Motd_(Unix)) while working as a __SunOS__ system administrator back in my university days. At our school, the systems used the Unix `fortune` command to display random messages and quotes on login. Being a bit nostalgic about these early days of the personal computer era, I decided to build a _Message of the Day_ bot for Twitter. This would give me the pleasure of seeing my own custom messages of the day in my feed, and help me learn the ins and outs of the Twitter API. I have plans to develop more advanced Twitter bots in future, so now was as good a time as any to learn the new API.

As with my previous projects, I am running this Twitterbot using Python on one of my VMs running on AWS. I researched several libraries available for accessing Twitter in Python and decided to use the [Twython library](https://github.com/ryanmcgrath/twython) to handle the API calls. The __Twython__ library can be installed using the __PIP__ package manager for Python, as shown below. Most newer versions of __Linux__ include __PIP__ by default, so the first command may not be necessary on some systems, but I included it just in case. 

```python
sudo apt-get install python-pip
sudo pip install twython
```

Once __Twython__ has been installed successfully, the next step is to create a Twitter account for the bot. Please note, you __must__ assign a mobile phone number to this account, at least temporarily. Twitter will not allow apps to be created using an account which does not have a verified mobile phone number.

After creating your new Twitter bot account, go to [https://apps.twitter.com/](https://apps.twitter.com) and click the __Create New App__ button. When prompted, enter the following information:

- Name for the application
- Description
- Link to valid website (I used my site homepage)

All other fields can be left blank. Check the boxed labelled _Yes, I have read and agree to the Twitter Developer Agreement_ (assuming you read and agree), then click the button labelled __Create your Twitter application__. Once the app has been created, click on the __Keys and Access Tokens__ tab to access your new app Consumer (API) keys.

{{< image src="Twitter_App_Keys.jpg" alt="Twitter App keys" position="left" style="border-radius: 5px; margin-left: 1em; width: 95%;" >}}

You will also need __Access Tokens__ to enable API access for your bot account. Click on the __Regenerate Consumer Key and Secret__ link to generate these codes. Keep a copy of the _Consumer (API) Key_, _Consumer (API) Secret_, _Access Token_ and _Access Token Secret_ to be used later in your bot script. Also note, once this is done you can remove the mobile phone number assigned in _Account Settings_ under the Twitter bot account. It is no longer required to create additional apps. 

We're finally at the point where we can build and run our new Twitterbot. Here is a breakdown of the Python code for my _Message of the Day_ bot. The first section includes standard declarations identifying any required Pythong libraries. It then opens a file named __quotes.csv__ which is a simple comma delimited file of quotes I have collected over the years. The file contains two columns of information, the first column being the `quote`, the second column being the `citation` associated with the quote, as shown below.

  | quote                                   | citation   |
  | --------------------------------------- | ---------- |
  | When opportunity knocks, bolt the door. | Grumpy Cat |
  | The unexamined life is not worth living.| Socrates   |

The file is read directly into a Python _dictionary_ array for simple access in memory. Unfortunately, this includes the CSV file _header_ row, which the last line of code deletes from the array to ensure it is not selected randomly in error.

```python
#!/usr/bin/python
import csv, random, sys
from twython import Twython, TwythonError

# Import quotes dictionary from csv file
quotes = dict(csv.reader(open("/home/pi/etc/quotes.csv", mode='r')))
# Remove CSV header row from dictionary to avoid random selection
del quotes['quote']
```

The next section of code uses the Python `random.choice` function to select a random quote from the _quotes_ array. The quote and citation are assembled in the variable `motd` using `utf-8` encoding which allows the _em dash_ character to be included as part of the citation. Lastly, some quotes in the __quotes.csv__ source file may be longer than the maximum allowed Tweet size (280 characters). To avoid this issue, the script uses a selection loop that repeats until a _Message of the Day (motd)_ is selected which is less than 278 characters (just to play it safe).

```python
# Select random quote
while True:
	quote = random.choice(quotes.keys())
	cite = quotes[quote]
	motd = u"\"" + quote.decode('utf8') + u"\"\n" + u" \u2014 " + cite.decode('utf8') + u"  #motd"

	# if less than max tweet size, break out of loop and tweet
	if len(motd) < 278:
		break
```

Once a quote has been selected and properly formatted, the script uses the __Twython__ library to submit the Tweet via the Twitter API. For this to function, the _Consumer (API) Key_, _Consumer (API) Secret_, _Access Token_ and _Access Token Secret_ generated previously need to be inserted into the appropriate variables in the code below.

```python
# Twitter authentication settings. Create a Twitter app at https://apps.twitter.com/ and
# generate key, secret, etc, and insert them below.
apiKey = ""
apiSecret = ""
accessToken = ""
accessTokenSecret = ""

# Tweet Message of the Day
try:
	twitter_api = Twython(apiKey,apiSecret,accessToken,accessTokenSecret)
	twitter_api.update_status(status = motd)
	print motd
except TwythonError as e:
	print(e)
```

On my server, I created this script with the filename `motd_bot.py`. I also enabled the _executable bit_ on this file with the command `chmod +x motd_bot.py`. This allows running the script directly from the command line without calling Python first. After a few manual test runs to ensure it was working properly, I used the `crontab -e` command and added the following lines in the `crontab` file. This scheduled the script to run daily at 7:00am.

```bash
# Tweet random MOTD at 7:00am
0 7 * * * /home/bft/bin/motd_bot.py
```

One last item to note, as this script includes your Twitter API keys and secrets, it should only be stored and run from a properly secured system. In my example, the script is running on an Ubuntu Server VM which has been hardened following some of the steps from this [page](https://gist.github.com/lokhman/cc716d2e2d373dd696b2d9264c0287a3) on GitHub. Refer to the Twitter Developer [Security Best Practices](https://developer.twitter.com/en/docs/basics/security-best-practices.html) page for more information.

As always, I hope you found this post useful, or at least entertaining. To contact me, please use the [Contact](/contact) page, or message me on [Twitter](https://twitter.com/bftsystems).

Thanks for reading.
