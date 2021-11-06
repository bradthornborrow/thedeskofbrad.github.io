+++
title = "Twitter Developer Platform process changes"
date = 2018-08-23
tags = [ "twitter" ]
+++

If you are any kind of Web developer, you have likely heard all about the changes Twitter has made to their Developer Platform. Primarily, the changes impact developers of 3rd party Twitter apps due to the removal of several legacy API endpoints, such as the Tweet streaming and Direct Messaging APIs. These changes have received a lot of coverage, but there has been little said about the impact for small in-house and student application developers. In many cases, the primary concern is the requirement to [apply for a Twitter Developer account](https://developer.twitter.com/en/apply/user) and be approved by Twitter before you can create new apps.

{{< image src="twitter_developer_changes.png" alt="Twitter App Developer Changes" position="left" style="border-radius: 4px; margin-left: 1em; width: 95%;" >}}

The process is simple enough, just click on the link and complete the developer account application form (which now includes an essay question where you must explain how you intend to use the Twitter APIs). How long it takes for an account to be approved seems to vary wildly. For developers with applications already deployed under the old system, the process seems instantaneous. Complete the application, confirm your email address and you are approved. In other cases, especially for students or new developers building their first Twitter application, it can take days to weeks, with little communication provided by Twitter when their application will be approved, other than this status page.

{{< image src="twitter_app_under_review.png" alt="Twitter App Under Review" position="left" style="border-radius: 4px; margin-left: 1em; width: 95%;" >}}

One can only hope this is a temporary issue due to the number of new applications being reviewed by Twitter at the moment. 

Once your application has been approved, this gives you access to the new [Twitter Developer Portal](https://developer.twitter.com/). Twitter has now unified all developer documentation and account management through this new interface, and there is plenty of information to be found there. For any new developers, the most important sections are the __Get Started__ and __Apps__ menus on the far right of the page.

{{< image src="twitter_apps_menu.png" alt="Twitter Apps Menu" position="left" style="border-radius: 4px; margin-left: 1em; width: 95%;" >}}

The __Get Started__ page covers all the basic steps to begin using the Developer Portal, such as adding developers to your account (if working in a team), and setting up Twitter __dev environments__, which are necessary if you will be using the new paid [Premium and Enterprise APIs](https://developer.twitter.com/en/pricing).

Next, the __Apps__ page lists any previously created applications, and new apps can be created by simply clicking on the __Create an app__ button.

{{< image src="twitter_create_an_app.png" alt="Twitter Create an App" position="left" style="border-radius: 4px; margin-left: 1em; width: 95%;" >}}

The process for creating new Twitter apps is similar to the previous application process, although under Twitter's new process you __must__ include a long-form description how the app will be used.

{{< image src="twitter_app_use.png" alt="Twitter Create App Use" position="left" style="border-radius: 4px; margin-left: 1em; width: 95%;" >}}

Once your app has been successfully created, click on the __Keys and tokens__ tab to access the Consumer (API) keys and User Access tokens for your application. At this point, the process to use these keys in your application matches the old Twitter API key process. Just copy the keys into your application and start testing.

{{< image src="twitter_api_keys.png" alt="Twitter API keys" position="left" style="border-radius: 4px; margin-left: 1em; width: 95%;" >}}

In this post, I have only scratched the surface of the new __Twitter Developer Portal__ to help you get started building apps. In a future post I will review some of the new features available, such as the __Developer Sandbox Environments__ for testing access to the Twitter Premium APIs. 

To contact me, please use the [Contact](/contact) page, or message me on [Twitter](https://twitter.com/bftsystems).

Thanks for reading.