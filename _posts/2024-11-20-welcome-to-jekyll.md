---
layout: post
title: 'Welcome to Jekyll!'
date: 2024-11-20 11:00:00 +0700
categories: blog
---

# Introduction

## Background

I will explain what brings me here. So actually, I was learning about CI/CD, specifically about GitHub Actions from a Udemy course, when my YouTube recommendation suggested a Jekyll video. Then I thought about how I could implement GitHub Actions on a Jekyll site, knowing that it works well with GitHub Pages. So I decided to watch that Jekyll video and learn how to build a static site using Jekyll. After deciding what the goal is, now I need to know how I am going to start building the site.

## Jekyll Setup

As usual, when learning a new language or tool, I tend to visit its official website first. So I went to `https://jekyllrb.com/` to learn how to set up and run Jekyll in development. Unfortunately, it is really complicated to set this up on Windows. You can look into Jekyll installation on Windows at `https://jekyllrb.com/docs/installation/windows/`. If you managed to set up Jekyll using that guide, I have to congratulate you 🚀, good job. As a future DevOps Engineer 😂, we hate to install many dependencies on our local machine, so I needed to find another way to set up my Jekyll for development.

Thanks to Mr. Bretfisher, my DevOps teacher from the Udemy course 😎, who provided a `jekyll-serve` image that includes all of Jekyll's dependencies for development purposes, so I don't have to install it manually on my computer. You can visit the `jekyll-serve` repository on [GitHub](https://github.com/BretFisher/jekyll-serve) or go directly to its image on [Docker Hub](https://hub.docker.com/r/bretfisher/jekyll-serve). We can pull the image using

```sh
docker pull bretfisher/jekyll-serve
```

and that is our first step toward developing Jekyll using the jekyll-serve image.

As far as I know, there are two ways to run Jekyll in development using the jekyll-serve image:

1. Docker command `docker run`
2. Docker Compose with `docker-compose.yaml`

Regardless of which method you choose, you have to mount your working directory to `/site` on the container. Here is how it's done using the docker command:

```sh
docker run -v $(pwd):/site bretfisher/jekyll new . # initiate a new project
docker run -p 4000:4000 -v $(pwd):/site bretfisher/jekyll-serve # running development environment
```

NB: if you are working on a Windows machine, change `$(pwd)` to `${pwd}`.

Prefer using the compose method? Just create a `docker-compose.yaml` file in your project directory and insert the code below:

```yaml
services:
  jekyll:
    image: bretfisher/jekyll-serve
    container_name: jekyll-site-pirates # optional, you can omit this option, or adjust based on your needs
    volumes:
      - .:/site
    ports:
      - '4000:4000'
```

Execute compose by

```sh
    docker compose up
```

It will serve our Jekyll site for development.

## Next Steps

We are successfully running Jekyll in development, so what's next? The next step is to answer these questions:

1. What if we want to add another library or dependency to the Jekyll site? What should we do?
2. How do we build the site so it can be deployed on a web server?

The general answer is we need to navigate inside our container using either `docker exec -it <container_name> sh` or `docker compose exec -it <container_name> sh`. Why is that? Because the gem package manager is only accessible inside the container, so whether you like it or not, you have to run the `bundle` command inside your Jekyll container.

Want to update dependencies?

```sh
    bundle update
```

Want to build Jekyll?

```sh
    bundle exec jekyll build
```

That's it for now. That is the background of this project. Hope you enjoy it and thank you 😊

Nov 11, 2024

```
    Part-time developer, full-time learner 🚀🚀🚀
```

Keep coding 👋 - Skyes
