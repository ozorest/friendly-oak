+++
title = "Mirroring repositories at Gitlab.com"
description = ""
tags = [
    "git",
    "gitlab",
    "github",
]
date = "2020-06-09"
categories = [
    "Gitlab",
]
menu = "main"
+++

I'm migrating most part of my repositories to the [Gitlab.com](https://gitlab.com), because they ultimately have a nice set of features, but I'm not intended to left [Github](https://github.com), because it has another nice set of features and also they have a certain charm ;-)

And with that on mind, I was wondering how to sync my repos from Github to Gitlab and also the reverse. I checked some options to do that, but the simplest option that I've found was to use an awesome feature available at Gitlab.com, called **mirroring repositories**.

It is possible to mirror any internet acessible git repository in the both ways (push and pull), so if you update in the Gitlab the other repo is updated as well and also the inverse. 

So, I synchronized my github repos with gitlab in that way, follow the steps to configure it, below:

1) Go to the project page inside Gitlab

2) In the left sidebar, look for "Settings" option

3) That option will open a set of other options, click in the "Repository" option

![Gitlab screen showing Settings and Repository options](https://i.imgur.com/r8Kz5al.png)

4) Expand the "Mirroring repositories" option

5) Fill the "Git repository URL" with the address of your repo, if you use https, you must fill the URL together with the user. Ex.: https://user@github.com/user/repo.git

6) In the combo "Mirror direction" choose if you will do a Push or a Pull (you can configure both at the same time)

7) After choose the authentication method, password if you will use https, public key if you will use ssh

8) If you chose password, you must fill the textbox below with the password of your source repo, if you chose public key you must add a new ssh key.

9) If you want you can check "Keep divergent refs" or "Only mirror protected branches" (I usually don't do that)

10) At last, click in the beautiful green button called "Mirror repository" and that's it. 

![Gitlab screen showing the "Git URL repository", "Mirror direction" and "Authentication method" fields](https://i.imgur.com/8HLEE1s.png)

![Detailed view of "Mirror repository" button](https://i.imgur.com/CrWW8OP.png)

So, that's it for now.