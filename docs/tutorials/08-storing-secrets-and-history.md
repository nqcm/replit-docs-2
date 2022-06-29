---
title: Keeping secrets
---

# Staying safe: Keeping your passwords and other secrets secure

While developing software fully in public has many benefits, it also means that we need to be extra careful about leaking sensitive information. Because all of our repls are public by default, we shouldn't store passwords, access keys, personal information, or anything else sensitive in them.

Even if you're coding offline or only in private repls, it's good practice to keep your code separate from any private information in any case.

In this tutorial, we'll look at how to set [environment variables](https://en.wikipedia.org/wiki/Environment_variable) for our repls. We can use these to store sensitive information and Replit will make sure that this data isn't included when others fork our repl.

## Understanding the Environment Variables pane

In the sidebar, the icon with a padlock will take you to a pane where you can set and view your environment variables. Each environment variable has a *key* and a *value*. The key is public information and you code can refer to it directly, while the value is secret, and shouldn't be shared with others.

You can set any variable to an arbitrary value, and this is often how you will store secrets such as API keys, database credentials, or configuration information for your projects.

You can read more about how Replit integrates with environment variables [here](/programming-ide/storing-sensitive-information-environment-variables).

## Refactoring our weather project to keep our API key secure

In the [working with files tutorial](http://www.codewithrepl.it/02-managing-files-using-repl-it.html) we used the WeatherStack API to fetch weather data and save it to disk. Part of this involved getting an API key from WeatherStack. Because WeatherStack limits the number of calls each key can make per day, it would be bad if someone else used up the quota for our key and broke our app as a result.

Let's refactor the WeatherStack project to prevent our key from being made public.

Visit [https://replit.com/@ritza-co/cwr-02-weather-report](https://replit.com/@GarethDwyer1/cwr-02-weather-report) (or your own version of this if you followed along previously) and create a new fork by pressing the pencil icon and then `fork`. 

![**Image 1:** *Forking our repl before refactoring it.*](https://replit-docs-images.util.repl.co/images/tutorials/08-storing-secrets/08-01-fork-repl.png)

We have `API_KEY` defined near the top of `main.py`, and this is the value that we want to keep secret. Let's add it as an environment variable instead. In the environment variables pane, add a new variable called `API_KEY` and the API key as a value.

![**Image 2:** *Adding an environment variable*](https://replit-docs-images.util.repl.co/images/tutorials/08-storing-secrets/08-saving-env-var.png)

Save the new variable and remove the API_KEY variable from the `main.py.

## Testing that the file is not copied into others' forks

If you copy your project's URL into an incognito window (or use a separate browser), you'll see all of the other files as usual, but when you navigate to the environment variables pane, you'll see they are all blank. If you want to share a functioning version of your project, you'll have to explain to your users how to add their own API key to their fork.

## Using environment variables in our script

Our API key is now securely defined and available to the project, but we still need to tell our code where to find it. Because this data is stored in environment variables, we need to use the operating system (`os`) module to access it.

At the top of your `main.py` file, add an import for `os` and load the API_KEY into a variable as follows.

```python
import requests
import os

API_KEY = os.getenv("API_KEY")
```

The `getenv` function looks for an environment variable of a specific name. Now our code (and anyone who sees it) only needs to know the name of the key that stores our private API key, instead of the API key itself. You should be able to run your code again at this point to verify that new weather entries are correctly added to the relevant files.

There are many other environment variables that make various parts of an operating system work correctly. For example, you could also take a look at the `LANG` and `PATH` environment variables, which will show you that Replit has their servers configured to use US English and 8-bit unicode character encoding, and have some default places where the system looks for executable programs.

![**Image 4:** *Looking at other environment variables.*](https://replit-docs-images.util.repl.co/images/tutorials/08-storing-secrets/08-04-using-env-variables.png)

## Time travelling to find secrets

We removed the sensitive information from our project, but it's not actually completely gone. It's securely placed in our environment variables pane, but it's also still saved in the repl's history.

Replit saves every change you make to a project so that you can always go back to previous versions if you make a mistake or need to check what has changed.

Click on the history button in the top bar, as shown below.

![**Image 5:** *Diving into the history of our repl.*](https://replit-docs-images.util.repl.co/images/tutorials/08-storing-secrets/08-05-open-history.png)

You should see a bunch of entries from each change you've made to this project. Click through them and find the one where you deleted your API key. As you can see, the history viewer shows not only which lines have been changed, but also what was there before.

![**Image 6:** *Finding the credentials in the change logs.*](https://replit-docs-images.util.repl.co/images/tutorials/08-storing-secrets/08-06-key-visible-history.png)

Luckily history is not included when other people fork your repl so this is not a huge problem, but it's important to keep in mind where people might find your credentials.

In our case, the worst case scenario is that someone finds our WeatherStack API key and uses up the quota, which is not the end of the world. A far more painful (and very common) scenario involves real money. For example, a developer signs up for a free trial on an expensive service like AWS, links a credit card, and then accidentally pushes the credentials for the service to GitHub or similar. Even if they realise their mistake and delete these within seconds, the credentials themselves are still available in the history of their repository. Hackers have bots that regularly look out for mistakes like this and use the credentials to spin up thousands of servers (often to mine cryptocurrency or join a botnet attack), potentially costing the poor developer thousands of dollars before they notice.

## Rotating credentials

Even if there's a small chance that your API key has been exposed, it's important to rotate it. This involves creating a new key, ensuring the new key works with your service, and then disabling the old one.

In the case of WeatherStack, there is no option to create a new key while keeping the old one active, so we need to reset it and then copy the new key to our environment variables pane (meaning that our app can't function between the time that we disable the old key and replace it with the new one).

![**Image 7:** *Rotating our WeatherStack API key.*](https://replit-docs-images.util.repl.co/images/tutorials/08-storing-secrets/08-07-weatherstack-reset-api.png)

Visit your WeatherStack account and press the `reset` button to get your new API key.

## Make it your own

You can make a copy of the new repl below. If you fork it, you'll need to create an `API_KEY` environment variable and add your WeatherStack API key before it will run.

<iframe height="400px" width="100%" src="https://replit.com/@GarethDwyer1/cwr-08-secrets-env?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## Where next?

There's a lot more that you can do with environment variables. In a later tutorial, we'll use it to store database credentials (which are more important to keep safe than a free API key).

You could also keep other private information in environment variables. For example, if you code a hangman game, you could keep the word that people need to guess in there so that you can share your code without spoiling the game.
