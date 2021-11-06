+++
title = "Create Twitterbot API Access Tokens using Twurl"
date= 2018-08-28
tags = [ "twitter" ]
+++

If you do a quick Google search for __"twitterbot"__, you'll find countless articles outlining the steps to setup a Twitterbot account and create the necessary Twitter API keys using this account. Unfortunately, with the recent Twitter Developer platform changes, they have significantly restricted the Developer approval process. In this new environment, it makes little sense to setup a separate Developer account for the purposes of a single Twitterbot (and in all likelihood, the request may be denied by Twitter anyway). This is where the __twurl__ utility comes in.

[Twurl](https://github.com/twitter/twurl) is a Ruby utility similar to the Unix tool __curl__, but designed specifically for  the Twitter API. The primary purpose of this tool is for testing and debugging Twitter API calls, but it can also be used to generate Twitter User __Access Tokens__ for Twitter test accounts, or in this case, for a Twitterbot. __Twurl__ can be easily installed using Ruby Gems with this command (assuming you already have Ruby setup on your system).

	gem install twurl

Once __twurl__ is installed, follow the usual steps to create a new Twitter Application ID in the [Twitter Developer Portal](https://developer.twitter.com/) using your primary Twitter Developer account. This process is covered in my previous [post](https://bftsystems.ca/twitter-developer-changes/) regarding the Twitter Developer Platform changes. In addition to this, you will need to setup a separate Twitter account for your bot.

Once you have the Twitter App consumer (API) key and secret you must authorize __twurl__ to make API requests using them. Open the `terminal` and enter the following command (inserting your Twitter APP api key and secret where appropriate).

	twurl authorize --consumer-key API_KEY --consumer-secret API_SECRET

__Twurl__ will return a URL on the command line and prompt you to enter a __PIN__ number. Cut and paste this URL into your browser and authenticate to Twitter using your Twitterbot account username and password. You should then be prompted with the usual Twitter app authorization page.

{{< image src="twitter_authorize_app.png" alt="Twitter Authorize App" position="left" style="border-radius: 4px; margin-left: 1em; width: 95%" >}}

Once you click __Authorize app__, Twitter will return a __PIN__ number

{{< image src="twitter_pin.png" alt="Twitter App Pin" position="left" style="border-radius: 4px; margin-left: 1em; width: 95%;" >}}

Enter this __PIN__ number in the terminal where prompted by __Twurl__. Assuming there are no issues, __twurl__ should report `Authorization successful`. Once this is done, your new _Consumer (API) Key_, _Consumer (API) Secret_, _Access Token_ and _Access Token Secret_ will be stored in the `.twurlrc` file. This file can be viewed in any text editor. For example, on my system running __macOS__, I opened this file using `nano ~/.twurlrc` and it looks as follows (keys blanked out).

```yaml
---
profiles:
  BFTbot:
    5E7GtcDZdcqYWjMxjhdzeZqF1:
      username: BFTbot
      consumer_key: XXXXXXXXXXXXXXXXXXXXXXXXX
      consumer_secret: XXXXXXXXXXXXXXXXXXXX-XXXXXXXXXXXXXXXXXXXXXXXX
      token: XXXXXXXXXXXXXXXXXXXXXXXXX
      secret: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
configuration:
  default_profile:
  - BFTbot
  - 5E7GtcDZdcqYWjMxjhdzeZqF1
```

You can the cut and paste the _consumer_key_, _consumer_secret_, _token_ and _secret_ from this file into your bot script and run it following the usual process (see my previous Twitterbot [post](http://localhost:4000/twitter-motd-bot/)  for details). Your bot will access the Twitter API and authenticate using your Twitterbot account, with no developer credentials needed.

One item to note, Twitter limits each developer account to a maximum of 10 apps to _combat spam and multi-key abuse_. If you wish to create more than 10 apps, you will need to follow the steps [here](https://developer.twitter.com/en/docs/basics/developer-portal/guides/apps) to submit a request to Twitter for additional app approvals. Lastly, as any Twitterbots you create are now associated with APP IDs under your primary Twitter account, you will want to ensure they don't break any of Twitter's rules to ensure your account stays in good standing. Refer to the Twitter Developer [Security Best Practices](https://developer.twitter.com/en/docs/basics/security-best-practices) page for more information.

To contact me, please use the [Contact](/contact) page, or message me on [Twitter](https://twitter.com/bftsystems).

Thanks for reading.