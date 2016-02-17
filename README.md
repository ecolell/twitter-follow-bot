[![Beerpay](http://test.beerpay.io/ecolell/twitter-follow-bot/badge.svg?style=flat-square)](http://test.beerpay.io/ecolell/twitter-follow-bot)
#Twitter Bot

A Python bot that automates several actions on Twitter, such as following users and favoriting tweets.

##Disclaimer

I hold no liability for what you do with this bot or what happens to you by using this bot. Abusing this bot *can* get you banned from Twitter, so make sure to read up on proper usage of the Twitter API.

##Dependencies

You will need to install Python's [python-twitter](https://github.com/sixohsix/twitter/) library first:

    easy_install twitter
    
You will also need to create an app account on https://dev.twitter.com/apps

1. Sign in with your Twitter account
2. Create a new app account
3. Modify the settings for that app account to allow read & write
4. Generate a new OAuth token with those permissions
5. Manually edit this script and put those tokens in the script

##Usage

###Configuring the bot

Before running the bot, you must first set it up so it can connect to the Twitter API. Edit config.txt and fill in the missing Twitter information:

    OAUTH_TOKEN:
    OAUTH_SECRET:
    CONSUMER_KEY:
    CONSUMER_SECRET:
    TWITTER_HANDLE:
    ALREADY_FOLLOWED_FILE:already-followed.txt
    FOLLOWERS_FILE:followers.txt
    FOLLOWS_FILE:following.txt
    USERS_KEEP_FOLLOWING:
    USERS_KEEP_UNMUTED:
    USERS_KEEP_MUTED:
    
You can change the `FILE` entries if you want to store that information elsewhere on your computer.

Add comma-separated Twitter user IDs to the `USERS_KEEP_*` entries to:

* `USERS_KEEP_FOLLOWING`: Keep following these users even if they don't follow you back.

* `USERS_KEEP_UNMUTED`: Keep these users unmuted (i.e., you receive a mobile notification when they tweet)

* `USERS_KEEP_MUTED`: Keep these users muted (i.e., you don't receive a mobile notification when they tweet)

For example:

    ...
    FOLLOWS_FILE:following.txt
    USERS_KEEP_FOLLOWING:1234,1235,1236
    USERS_KEEP_UNMUTED:
    ...
    
You can look up a users' Twitter ID [here](http://tweeterid.com/).

###Create an instance of the bot

To create an instance of the bot:

    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot()
    
If you want to use a different configuration file, pass the configuration file to the bot as follows:

    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot("my-other-bot-config.txt")
    
Note that this allows you to run multiple instances of the bot with different configurations, for example if you run multiple Twitter accounts:

    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot()
    my_other_bot = TwitterBot("my-other-bot-config.txt")

###Syncing your Twitter following locally

Due to Twitter API rate limiting, the bot must maintain a local cache of all of your followers so it doesn't use all of your API time looking up your followers. It is highly recommended to sync the bot's local cache daily:

    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot()
    my_bot.sync_follows()
    
**DO NOT** delete the cache files ("followers.txt", "follows.txt", and "already-followed.txt" by default) unless you want to start the bot over with a fresh cache.

###Automating Twitter actions with the bot

This bot has several functions for programmatically interacting with Twitter:

####Automatically follow any users that tweet something with a specific phrase

    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot()
    my_bot.auto_follow("phrase")
    
You can also search based on hashtags:

    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot()
    my_bot.auto_follow("#hashtag")
  
By default, the bot looks up the 100 most recent tweets. You can change this number with the `count` parameter:

    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot()
    my_bot.auto_follow("phrase", count=1000)
    
####Automatically follow any users that have followed you

    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot()
    my_bot.auto_follow_followers()

####Automatically follow any users that follow a user
    
    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot() 
    my_bot.auto_follow_followers_of_user("jack", count=1000)

####Automatically favorite any tweets that have a specific phrase

    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot()
    my_bot.auto_fav("phrase", count=1000)
    
####Automatically retweet any tweets that have a specific phrase

    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot()
    my_bot.auto_rt("phrase", count=1000)

####Automatically unfollow any users that have not followed you back

    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot()
    my_bot.auto_unfollow_nonfollowers()
  
You will need to manually edit the code if you want to add special users that you will keep following even if they don't follow you back.

####Automatically mute all users that you have followed

    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot()
    my_bot.auto_mute_following()

You will need to manually edit the code if you want to add special users that you will not mute.

####Automatically unmute everyone you have muted

    from TwitterBot import TwitterBot
    
    my_bot = TwitterBot()
    my_bot.auto_unmute()
    
You will need to manually edit the code if you want to add special users that will remain muted. 

##Have questions? Need help with the bot?

If you're having issues with or have questions about the bot, please [file an issue](https://github.com/rhiever/twitter-follow-bot/issues) in this repository so one of the project managers can get back to you. Please check the existing (and closed) issues to make sure your issue hasn't already been addressed.
