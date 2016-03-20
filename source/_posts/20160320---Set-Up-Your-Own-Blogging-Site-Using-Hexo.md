---
title: Set Up Your Own Blogging Site Using Hexo
date: 2016-03-20 15:40:55
tags:
---


Recently, I came across [github pages](https://pages.github.com/), github pages is a service which github helps you host your static site in github repository, it's completely free to everyone. So I'm thinking to have my own blog site to start blogging. The first tool I started using is [jekyll](https://jekyllrb.com/) because it's recommended at the bottom of github pages website.
{% asset_img jekyll_recommend.png "jekyll"%}
I played around with jekyll and octopress( a blogging framework built on top of jekyll ) for a few days. I finally give up. The reason is jekyll is written in ruby, all the thing around ruby including the language , tools and commands are just foreign to me as a javascripter. And another reason is that I feel it's getting a little complex when octopress is involved ( maybe it's due to the first reason, ruby is not my thing... ).  

I googled around static site generator built on nodejs ( if you're javascripter, you know the feeling we have for node ). Finally, I found [__Hexo__](https://hexo.io/), which is extremly simple, easy to use blog generator, it only took me a few minutes to get my site running on my local pc, and a few more minutes to deploy it on github pages. If you know javascript and nodejs, I highly recommend try hexo and start blogging and share your good ideas to the world! Now let's get started! BTW, the blog you're looking at now is the very first blog written on Hexo.

# Prerequisites
### Knowledge
1. You should have some basic knowledge about git and github
2. You'll need to use some command line tools
3. You'll need to know markdown to blog

### Things you need to have
1. terminal ( I'm using [iTerm2](https://www.iterm2.com/))
2. [git](https://git-scm.com/)
3. [nodejs](https://nodejs.org/en/)
4. of course, your favorite text editor (mine is vim)
5. github user account (prefer ssh key configured, otherwise ssh git repo url won't work, if you use https repo url, then you'll have to enter username and password when deploying)

# Get Started

### Install Hexo
Go to your terminal and enter the following command:
~~~
npm install -g hexo-cli
~~~
It'll take a while to have hexo installed, have some patience and treat yourself a cup of coffee.

### Init Hexo Blog Site
Create your blog site using following command:
~~~
hexo init [sitename]
~~~
Hexo will start initiating your initial blog site in the `sitename` directory. `sitename` is just a placeholder, choose whatever sitename you want. After done, you'll have the directory looks like below:
{% asset_img hexo_initial_file_structure.png "initial hexo blog site structure" %}

### Running Your Blog Site Locally
In the directory of your blog site, run command:
~~~
hexo server
~~~
This will start a local http server serving your site on port 4000 by default, so now you should be able to open your site locally at http://localhost:4000. If everything is working fine, you should now see your blog site like below:
{% asset_img hexo_default_site.png "Local Blog Site" %}
Your blog site is already looking great, isn't it?  And hexo has included a default "hello world" post to show you how your blog looks like.

### Create Your First Post
I won't cover all the details about how to use hexo to blog, you can find all the information on hexo's [official website](https://hexo.io/docs/). But in here, I'll show you how easy it is to create a post and start blogging. In the blog site directory, run following command to create your first post:
~~~
hexo new "My First Blog on Hexo"
~~~
This will create a markdown file `My-First-Blog-on-Hexo.md` under `source/_posts/` directory, and now if you open your local blog site again, you can see your first blog is shown on your blog site ( if you're doing along with me, you need to use `ctrl`+`c` to terminate the previous server and run `hexo server` again to see the changes )
{% asset_img myfirstpost.png "My First Blog on Hexo" %}
Next, let's deploy our blog site to github pages and show the world __WHO I AM__! (don't carry away too far)

### Create a Github Site Repository for Your User
Follow the guide on [github pages](https://pages.github.com/) to create a repository for a website fo your user. In the end, you'll create a repository like this: `youruser/youruser.github.io`.

### Install hexo-deployer-git npm module
In your blog site directory, run following command:
~~~
npm install hexo-deployer-git --save
~~~
This will install the `hexo-deployer-git` npm module locally as a dependency for your site, the module is a plugin to Hexo to help you deploy your site to github repository with zero effort.

### Config Deploy to Git in _config.yml
There's a `_config.yml` file located in root directory of your blog site. The config file is a central place where you do configuration to your whole website. Some default configuration is already in the file when Hexo initialze your blog site. You can find a tonne of available configuration option at Hexo's website. For now, we'll just add configuration of deploy to git at the bottom of the config file:
{% asset_img deploy_to_git_config.png "Deploy to Git Configuration" %}

### Deploy to Github Pages
Finally, we reach here! Now Let's deploy to our github page repository. It's rather simple, just run the following command in your blog site directory:
~~~
hexo deploy
~~~
You should see some log generated by hexo and in the end, it tells you `deploy done`. Congrats!!! Now you have your own blog site up and running. Quickly open url in browser to see your website: http://youruser.github.io/.

### Work on Same Blog Any Time Any Where
Now you have your blog site up and running, you can start writing your wonderful blog in markdown and deploy to github pages without any issue. It's fine for simple cases: you always blog on the same machine and you'll bring it with you wherever you go. For a more practical scenario, the way we did lacks flexibility, for instance, you start a blog at home on your home PC, you can't finish it immediately and you go to work and you want to continue to work on that blog on your company PC.

To be able to work on the same blog on different machines at any time, we need to use git repository. It's really simple, probably you already know how to do that. But I'll still elaborate the details for readers who are git and github beginners.

The theory here is: github pages will only serve static content on the `master` branch (it'll be `gh-pages` branch for project site) of your repository. So you only need to create another branch to store your work in process blogs, and these blogs won't be served until you deploy them to the `master` branch. Let's see how to do that:

~~~
git init                        
git checkout -b source          
git add .                       
git commit -m "initial commit"  
git remote add origin git@github.com:youruser/youruser.github.io.git 
git push --set-upstream origin source  
~~~

What above command does:
1. initialize your blog site directory as a git repository
2. create a new branch: source and checkout to that branch
3. staging all the untracked content and changed content
4. create a commit with message "initial commit"
5. add a remote source: origin whose url is git@github.com:youruser/youruser.github.io.git
6. push your work in process content to the source branch of origin upstream

Now you wip content is pushed and stored on the source branch of github. Next time you want to work on that blog, you only need to pull the source branch to your local machine and keep working on that. Now You can close your laptop and go to work with confidence that you can keep working on that blog when you reach office.

### The End
Happy blogging!!!
Thanks for reading!!!
Leave comment if you have any question or comments.
