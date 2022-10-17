+++
author = "ADS"
tags = ["Hugo", "Html", "CSS", "Javascript", "JAMStack", "data"]
comments = false
date = "2021-01-01"
draft = false
image = ""
menu = ""
share = false
slug = "sample1"
title= "Deploying Static Websites with Hugo & GoDaddy"
+++

# Hugo + Netlify + GoDaddy
## _How to create a Hugo Website


Hugo is a potent static website creator created in the GO programming language. You may use it to construct uncluttered, up-to-date websites with minimum overhead that load rapidly.

- Buying a domain in [GoDaddy]
- Downloading your favorite hugo theme in local repository with [Github]
- Deploying the website in [Netlify]
- Magic ✨


You generally don't want to spend a lot of time building a website that will be straightforward and content-driven. If you're building a personal portfolio, business website, or blog, this might be the case.

## What is Hugo?

With the help of Hugo, a static site generator, you can create a website with little to no coding. You can typically write your content in a basic markup language, like Markdown, using static site generators.

Your content is subsequently transformed into static HTML files by the static site generator. Before serving up your pages to the user, it applies any templates or styles.

Hugo offers a library of pre-built themes and website layouts that you may select from. Once a theme has been downloaded, you can begin creating your content. Hugo allows you to write information in a variety of formats.


![Hugo_themes](/blog/images/hugo_themes.jpg)(https://themes.gohugo.io/)


![Hugo_themes](/blog/images/hugo_themes_github.jpg)(https://github.com/half-duplex/hugo-arcana/)

## Hugo Installation

Dillinger is currently extended with the following plugins.
Instructions on how to use them in your own application are linked below.

| OS             | Step-by-Step Guide   |
| -------------- | -------------------- |
| Windows        | [Hugo Tutorial][Win] |
| Windows Link   | [Hugo Tutorial][Win] |
| Mac            | [Hugo Tutorial][Win] |
| Mac Tutorial   | [Hugo Tutorial][Win] |
| Linux          | [Hugo Tutorial][Win] |
| Linux Tutorial | [Hugo Tutorial][Win] |

```Mac
brew install hugo
```


## How to create Hugo project


1. Open your terminal or command prompt. Navigate to the folder where you would like to create your project.
2. Execute the hugo new site command:
```
hugo new site new-hugo-website
```
3. Navigate to the location of your Hugo project in your file explorer.
4. Open the project folder. You will see that your new Hugo website has the file and folder structure required for your website to work.

![Hugo_themes](/blog/images/hugo_folder.jpg)

```
hugo server
```

![Hugo_themes](/blog/images/cd_hugo_server.jpg)

## GoDaddy And Netlify Deployment
Getting a static website up and running is simple and fast with Hugo. You can use and configure pre-built themes and run your Hugo website locally for testing. You can also add content pages to your website using Markdown.

![Hugo_themes](/blog/images/godaddy.png)

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [Netlify]: <https://gohugo.io/hosting-and-deployment/hosting-on-netlify/>
   [GoDaddy]: <https://www.godaddy.com/>
   [Github]: <http://daringfireball.net>
    [Win]: <https://www.youtube.com/watch?v=C04dlR1Ufj4>
   