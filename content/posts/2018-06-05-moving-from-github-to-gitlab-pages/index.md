+++
title = "Moving your Jekyll site from GitHub Pages to GitLab"
date = 2018-06-05
tags = [ "github", "gitlab", "jekyll" ]
+++

For the last several months, I've been pondering the move of this site from GitHub Pages to GitLab. I have several reasons which I won't get into here, but the recent [news](https://www.bloomberg.com/news/articles/2018-06-03/microsoft-is-said-to-have-agreed-to-acquire-coding-site-github) regarding the acquisition of GitHub by Microsoft gave me the impetus to get this done. I ran into several interesting "hiccups" during the process, and decided to write this post to help anyone else going down this road.

Obviously, the first step is to migrate your site source files from GitHub to GitLab. In my case, I have a local copy of this site __cloned__ on my local system, and I used this as the source. For other options I recommend you read the GitLab document [Import your project from GitHub to GitLab](https://docs.gitlab.com/ee/user/project/import/github.html). To migrate this site, my first step was to create a new __empty__ project on GitLab named `bftsystems.gitlab.io`. I then cloned this empty repository to my local system, copied my site source files into the empty project, then pushed the results back to GitLab. Here is an overview of the commands I used locally.

```bash
cd git

git clone git@gitlab.com:bftsystems/bftsystems.gitlab.io.git

cp -R bftsystems.github.io/* bftsystems.gitlab.io/

cd bftsystems.gitlab.io

git add --all

git commit -am "Initial commit to Gitlab hosting"

git push origin master
```

Once your repository has been moved to GitLab, nothing will work immediately and no pages will be built. Unlike GitHub, GitLab uses a CI (continuous integration) configuration file to instruct the GitLab site _Runners_ how to build your site. This process is a little more complex than GitHub, but also more flexible. Primarily, this flexibility is what brought me to GitLab for hosting.

To setup the build process, you must create a file named `.gitlab-ci.yml` in your repository. The contents of this file will direct GitLab how to build your site. The full documentation on this process can be found in the document [Hosting on GitLab.com with GitHub Pages](https://about.gitlab.com/2016/04/07/gitlab-pages-setup/). As this site is built using Jekyll, I followed the steps in the document outlining the configuration process specifically for Jekyll. 

This is where things get a little interesting. When it comes to building the CI file for Jekyll, the GitLab documentation is somewhat vague, and misses several edge cases. Sadly, I seemed to have run into many of those edge cases. Firstly, you must configure your site to use the correct version of Jekyll. This will not be obvious, as the errors provided by GitLab may be non-intuitive. In my case, the errors I received were related to `Liquid syntax errors` in several source files. After some digging, I discovered these issues were caused by a Jekyll version mismatch. To resolve this, I confirmed the version of Jekyll I'm using locally (`bundle exec jekyll --version`), and updated the `Gemfile` in my repository to use this version (more on this later).

Further to the Jekyll issue, you must also ensure any Ruby _gems_ (aka _plugins_) required by your site are added to your site `Gemfile` as dependencies. Many dependencies included in GitHub by default are not included by GitLab, which will cause build issues. I also believe this is an issue for many other Jekyll templates which assume they are being hosted on GitHub Pages and will encounter dependency issues when hosted on GitLab. To confirm which _gems_ are required, open your site `_config.yml` file, and locate the section labelled `gems:` or `plugins:` (depending on the Jekyll version). Here is the section from this site as an example.

```yaml
# Plugins (previously gems:)
plugins:
  - jekyll-paginate
  - jekyll-sitemap
  - jekyll-gist
  - jekyll-feed
  - jemoji
```

Using this information and the Jekyll version I confirmed previously, I updated the `Gemfile` for my site accordingly[^1].

```yaml
source "https://rubygems.org"

gem 'jekyll', '3.6.2'
gem 'jekyll-paginate'
gem 'jekyll-sitemap'
gem 'jekyll-gist'
gem 'jekyll-feed'
gem 'jemoji'
```

Interestingly, once these changes were made, I now received page build errors from GitLab regarding __Invalid US-ASCII__ characters in several of the CSS files. After further digging through the CSS source files and searching on-line, I discovered GitLab does not support `UTF-8` by default in their Docker instances. A setting was required in the CI file to enforce this on every build (thanks to [this](https://gitlab.com/pages/jekyll/issues/9) Jekyll issue report for the discovery). Once this additional configuration was added to the CI file, the site built successfully on GitLab (woot!). Here is the final version of the `.gitlab-ci.yml` file with all the necessary changes for your reference.

```yaml
# requiring the environment of Ruby 2.3.x
image: ruby:2.3

# add bundle cache to 'vendor' for speeding up builds
cache:
  paths: 
    - vendor/

before_script:
  - bundle install --path vendor

# the 'pages' job will deploy and build your site to the 'public' path
pages:
  stage: deploy
  script:
    - bundle exec jekyll build -d public/
  variables:
    JEKYLL_ENV: production
    LANG: "C.UTF-8"
  artifacts:
    paths:
      - public
  only:
    - master # this job will affect only the 'master' branch
```

Once the build was finished, the site was immediately available through the GitLab Pages subdomain (eg. `https://bftsystems.gitlab.io/`). The final step in the process was to update DNS for my domain to use GitLab Pages. Once again, this was a little more complex than with GitHub, as there is no automated __https__ integration using Let's Encrypt (which GitHub has added recently). I won't go into the specific details of the process here, as it is well documented in the following articles.

- [Hosting on GitLab.com with GitLab Pages](https://about.gitlab.com/2016/04/07/gitlab-pages-setup/#custom-domains) - __Step 4: Add your custom domain__ covers the steps to update your DNS records for GitLab Pages
- [Securing your GitLab Pages with TLS and Let's Encrypt](https://about.gitlab.com/2016/04/11/tutorial-securing-your-gitlab-pages-with-tls-and-letsencrypt/)
- [Setting up GitLab Pages with CloudFlare Certificates](https://about.gitlab.com/2017/02/07/setting-up-gitlab-pages-with-cloudflare-certificates/)

In the end, I added two `A` records for my domain, one for the _apex_ or _root_ domain, and a second for the _www_ subdomain. I could have used a single _wildcard_ record, but I try to avoid using wildcard DNS records (just a personal preference). I then added both these domains, with their associated SSL certificates under __Settings, Pages__. I found both domains were required to avoid any SSL warnings when using some browsers.

{{< image src="GitLab_Pages_Settings.jpg" alt="GitLab_Pages_Settings" position="left" style="border-radius: 4px; margin-left: 1em;" >}}

As I've never used __Let's Encrypt__ previously, I went through their process to request new certificates and am currently using them for the site. However, I'll likely switch to __CloudFlare__ in future. This will avoid the complexity of automating regular certificate updates when using __Let's Encrypt__ (90 day expiry versus 15 years with __CloudFlare__, not a difficult choice).

All in all, I'm happy with my new setup and have learned a lot moving to GitLab Pages. As always, I hope you found this post useful, or at least interesting. To contact me or provide feedback, use the [Contact](/contact) page, or message me on [Twitter](http://twitter.com/bftsystems).

Thanks!

[^1]: One final note, updating these dependencies in my site `Gemfile` has reduced the site build-time in my local test environment by 50%. I'm not sure why this has occurred, but a happy side effect nonetheless.