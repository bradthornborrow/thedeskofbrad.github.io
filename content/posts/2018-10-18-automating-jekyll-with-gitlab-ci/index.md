+++
title = "Automating Jekyll site updates with GitLab CI Schedules"
date =  2018-10-18
tags = [ "gitlab","jekyll" ]
+++
One of the earliest posts I made on this site described the process I used to automate daily updates using a Raspberry Pi server running Raspbian. With my recent move of the site to [GitLab Pages](https://about.gitlab.com/features/pages/), I was able to automate daily site updates without the need of an external server. This was made possible through the magic of the GitLab CI (Continuous Integration) Scheduling. 

For those not familiar with __GitLab Pages__, it uses a CI configuration file (__.gitlab-ci.yml__) to define how the website is built from the GitLab project source files. See my [previous post](/posts/2018/06/moving-your-jekyll-site-from-github-pages-to-gitlab/) which covers this process in detail. To summarize, each time the site source files are updated, __GitLab__ starts a CI Pipeline and rebuilds the website following the directions in the CI configuration file. As I learned more about the GitLab CI process, I discovered CI Pipelines can be scheduled to run automatically.

This site uses the __Jekyll__ `sample` filter to select a random quote for the main page _Message of the Day_, which requires only a simple site _rebuild_ to update the message. For the full details on this process, see my original post [here](https://bftsystems.ca/automating-updates-to-github-pages/). Using the GitLab CI Scheduler, I could schedule a _rebuild_ at midnight each day to update the site, which would eliminate all external server dependencies.

To add a _Pipeline Schedule_ to a project in GitLab, open the project and select __CI / CD__, __Schedules__. All active and inactive schedules will appear on this page as shown below.

{{< image src="gitlab_ci_schedules.png" alt="GitLab CI Schedules" position="left" style="border-radius: 4px; margin-left: 1em; width: 90%;" >}}

To add a new schedule, click on the __New Schedule__ button and fill in the details as necessary, such as the _Description_, _Interval_, _Timezone_ and _Target Branch_. There are several default intervals available, and custom intervals can be entered using standard Unix _cron_ format. This schedule shown below will force a _rebuild_ of the project each day at 12:01am, no external server needed.

{{< image src="gitlab_ci_schedule.png" alt="GitLab CI Schedule" position="left" style="border-radius: 4px; margin-left: 1em; width: 90%;" >}}

There are many more advanced options available through _GitLab CI_, for example, variables can be included in the schedule, which can be referenced in the _CI configuration_ file allowing further customizations of the build process. For more information on this process, refer to the [GitLab Pipeline schedules documentation](https://docs.gitlab.com/ee/api/pipeline_schedules.html). I'm already looking into other ways to customize this site using variables in the __GitLab CI__ build process. Watch for future updates here.

To contact me, please use the [Contact](/contact) page, or message me on [Twitter](https://twitter.com/bftsystems). Thanks for reading.