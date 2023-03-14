# Labs for the standard product track in Python

![banner](../assets/banner.png)

In this section, we will learn how to write code in Python to get Twitter data using the standard product on the Twitter API v2 for various scenarios. We will be using the [twarc](https://twarc-project.readthedocs.io/en/latest/api/client2/) library in Python. All the example code used in these labs can be found [here](../labs-code/python/standard-product-track).

## Prerequisite

Make sure you have Python installed on your machine in order to run these samples. See our [appendix section](./0-appendix.md) for instructions on installing Python.

Also, make sure you have your bearer token from an app connected to your standard project, as shown in [module 4](./4-getting-your-keys-and-token.md)

## Installing twarc

In order to install the twarc library, in your terminal, run the following:

```bash
pip3 install twarc
```

## Importing the twarc library and the Twarc2 and expansions classes

In all of the code samples, you will first need to import the twarc library and Twarc2 client and the expansions class, in order to connect to the Twitter API v2 programmatically and get the response. You will also need the json library in order to save the response from the Twitter API in the JSON format

- Twarc2 has the necessary methods you need to connect to the Twitter API v2
- Expansions has utilities that make it easy for you to work with the response returned from the Twitter API.
- JSON library lets you convert the response to JSON for printing, writing to a file etc.

```python
# This will import the Twarc2 client and expansions class from twarc library and also the json library
from twarc import Twarc2, expansions
import json

# This is where you initialize the client with your own bearer token (replace the XXXXX with your own bearer token)
client = Twarc2(bearer_token="XXXXX")
```

Once you have initialized the client, and imported the expansions class and the json library, you are ready to use the functions available to get Twitter data using the Twitter API v2.

**Note:** These 3 lines of code will remain the same through the all of the samples. The only thing that will change based on the scenarios is the **main()** method

## 1. Searching for Tweets from the last 7 days

In the code below, make sure to specify the **start_time** and **end_time** from the last 7 days, otherwise the code sample will not work

```Python
from twarc import Twarc2, expansions
import datetime
import json

# Replace your bearer token below
client = Twarc2(bearer_token="XXXXX")


def main():
    # Specify the start time in UTC for the time period you want Tweets from
    start_time = datetime.datetime(2021, 6, 27, 0, 0, 0, 0, datetime.timezone.utc)

    # Specify the end time in UTC for the time period you want Tweets from
    end_time = datetime.datetime(2021, 6, 28, 0, 0, 0, 0, datetime.timezone.utc)

    # This is where we specify our query as discussed in module 5
    query = "from:twitterdev -is:retweet"

    # The search_recent method call the recent search endpoint to get Tweets based on the query, start and end times
    search_results = client.search_recent(query=query, start_time=start_time, end_time=end_time, max_results=100)

    # Twarc returns all Tweets for the criteria set above, so we page through the results
    for page in search_results:
        # The Twitter API v2 returns the Tweet information and the user, media etc.  separately
        # so we use expansions.flatten to get all the information in a single JSON
        result = expansions.flatten(page)
        for tweet in result:
            # Here we are printing the recent Tweet object JSON to the console
            print(json.dumps(tweet))


if __name__ == "__main__":
    main()

```

The source code for this sample can be found [here](../labs-code/python/standard-product-track/recent_search.py)

**Note:**

- You can modify the query here by using the operators available for recent search as shown in [module 5](./5-how-to-write-search-queries.md)
- If you do not specify the start_time, the default time period will be 7 days for recent search
- The **max_results** parameter for the search_all method above tells Twarc how many Tweets to get per request. Twarc will get all the Tweets for the specified time period and query, which can be more than the max_results, because it will make multiple calls in the example above and for each call it will get 100 requests until it gets all Tweets for the time period your specified.

## 2. Getting Tweet volume for Tweets from the last 7 days

In the code below, make sure to specify the **start_time** and **end_time** from the last 7 days, otherwise the code sample will not work

```Python
from twarc import Twarc2
import datetime
import json

# Replace your bearer token below
client = Twarc2(bearer_token="XXXXX")


def main():
    # Specify the start time in UTC for the time period you want Tweets from
    start_time = datetime.datetime(2021, 6, 27, 0, 0, 0, 0, datetime.timezone.utc)

    # Specify the end time in UTC for the time period you want Tweets from
    end_time = datetime.datetime(2021, 6, 28, 0, 0, 0, 0, datetime.timezone.utc)

    # This is where we specify our query as discussed in module 5
    query = "lakers"

    # The counts_recent method call the recent Tweet counts endpoint to get Tweets based on the query, start and end times
    count_results = client.counts_recent(query=query, start_time=start_time, end_time=end_time)


    # Recent Tweet counts returns all the Tweet volume for the last 7 days in one page so we break after that
    for page in count_results:
        print(json.dumps(page['data']))
        break

if __name__ == "__main__":
    main()

```

The source code for this sample can be found [here](../labs-code/python/standard-product-track/recent_tweet_counts.py)

**Note:**

- You can modify the query here by using the operators available for recent search as shown in [module 5](./5-how-to-write-search-queries.md)
- If you do not specify the start_time, the default time period will be 7 days for recent search

## 3. Building the conversation thread for a Tweet ID (from last 7 days)

This code sample will only get replies for a Tweet from the last 7 days, because it uses the recent search endpoint

```Python
from twarc import Twarc2, expansions
import json

# Replace your bearer token below
client = Twarc2(bearer_token="XXXXX")


def main():
    # Specify the Tweet ID for which you want the conversation thread
    query = "conversation_id:1403738886275096605"

    # The search_all method call the recent search endpoint to get the Tweets (replies) for the conversation 
    search_results = client.search_recent(query=query, max_results=100)

    # Twarc returns all Tweets for the criteria set above, so we page through the results
    for page in search_results:
        # The Twitter API v2 returns the Tweet information and the user, media etc.  separately
        # so we use expansions.flatten to get all the information in a single JSON
        result = expansions.flatten(page)
        for tweet in result:
            # Here we are printing the full Tweet object JSON to the console
            print(json.dumps(tweet))


if __name__ == "__main__":
    main()

```

**Note:** If you do not specify the start_time, you will only get replies from the last 30 days

The source code for this sample can be found [here](../labs-code/python/standard-product-track/conversation_thread.py)

## 4. Writing response from recent search to a text file

In the code below, make sure to specify the **start_time** and **end_time** from the last 7 days, otherwise the code sample will not work. Alternatively, you can also remove the start and end time parameters and the code will default to Tweets from the last 7 days.

```Python
from twarc import Twarc2, expansions
import datetime
import json

# Replace your bearer token below
client = Twarc2(bearer_token="XXXXX")


def main():
    # Specify the start time in UTC for the time period you want replies from
    start_time = datetime.datetime(2021, 6, 26, 0, 0, 0, 0, datetime.timezone.utc)

    # Specify the end time in UTC for the time period you want Tweets from
    end_time = datetime.datetime(2021, 6, 27, 0, 0, 0, 0, datetime.timezone.utc)

    # This is where we specify our query as discussed in module 5
    query = "from:twitterdev"

    # Name and path of the file where you want the Tweets written to
    file_name = 'tweets.txt'

    # The search_all method call the recent search endpoint to get Tweets based on the query, start and end times
    search_results = client.search_recent(query=query, start_time=start_time, end_time=end_time, max_results=100)

    # Twarc returns all Tweets for the criteria set above, so we page through the results
    for page in search_results:
        # The Twitter API v2 returns the Tweet information and the user, media etc.  separately
        # so we use expansions.flatten to get all the information in a single JSON
        result = expansions.flatten(page)
        # We will open the file and append one JSON object per new line
        with open(file_name, 'a+') as filehandle:
            for tweet in result:
                filehandle.write('%s\n' % json.dumps(tweet))


if __name__ == "__main__":
    main()

```

The source code for this sample can be found [here](../labs-code/python/standard-product-track/recent_search_to_txt_file.py)

## 5. Getting a user's Tweet timeline

```Python
from twarc import Twarc2, expansions
import json

# Replace your bearer token below
client = Twarc2(bearer_token="XXXXX")


def main():
    # This timeline functions gets the Tweet timeline for a specified user
    user_timeline = client.timeline(user="twitterdev")

    # Twarc returns all Tweets for the criteria set above, so we page through the results
    for page in user_timeline:
        # The Twitter API v2 returns the Tweet information and the user, media etc.  separately
        # so we use expansions.flatten to get all the information in a single JSON
        result = expansions.flatten(page)
        for tweet in result:
            # Here we are printing the full Tweet object JSON to the console
            print(json.dumps(tweet))


if __name__ == "__main__":
    main()

```

**Note:** The timeline function calls the user Tweet timeline endpoint which returns the 3200 most recent public Tweets for a user

The source code for this sample can be found [here](../labs-code/python/standard-product-track/user_tweet_timeline.py)

## 6. Getting a user's mentions

```Python
from twarc import Twarc2, expansions
import json

# Replace your bearer token below
client = Twarc2(bearer_token="XXXXX")


def main():
    # This mentions functions gets the mentions for a specified user
    user_mentions = client.mentions(user="twitterdev")

    # Twarc returns all Tweets for the criteria set above, so we page through the results
    for page in user_mentions:
        # The Twitter API v2 returns the Tweet information and the user, media etc.  separately
        # so we use expansions.flatten to get all the information in a single JSON
        result = expansions.flatten(page)
        for tweet in result:
            # Here we are printing the full Tweet object JSON to the console
            print(json.dumps(tweet))


if __name__ == "__main__":
    main()

```

**Note:** The mentions function calls the user mentions endpoint which returns the 3200 most recent public mentions for a user

The source code for this sample can be found [here](../labs-code/python/standard-product-track/user_mentions.py)

## 7. Getting a random 1% sample of Tweets in real-time

```Python
from twarc import Twarc2, expansions
import json

# Replace your bearer token below
client = Twarc2(bearer_token="XXXXX")


def main():
    # The sample method gives a 1% random sample of all Tweets
    for count, result in enumerate(client.sample()):
        # The Twitter API v2 returns the Tweet information and the user, media etc.  separately
        # so we use expansions.flatten to get all the information in a single JSON
        tweet = expansions.flatten(result)
        # Here we are printing the full Tweet object JSON to the console
        print(json.dumps(tweet))

        # Replace with the desired number of Tweets, otherwise it will stop after 100 Tweets
        if count > 100:
            break


if __name__ == "__main__":
    main()

```

The source code for this sample can be found [here](../labs-code/python/standard-product-track/stream_sampled.py)

## 8. Filtering for Tweets on a topic using filtered-stream

In the code sample below:

- We first get any existing rules using the **get_stream_rules** function, that we may have set and then delete those using the **delete_stream_rule_ids** function.
- Then we add the rules for which we want to get real-time Tweets using the **add_stream_rules** function.
- Then we connect to the filtered stream endpoint using the **stream** function
- Finally, when we have the desired number of Tweets collected, then we delete those rules using **delete_stream_rule_ids**

```Python
from twarc import Twarc2, expansions
import json

# Replace your bearer token below
client = Twarc2(bearer_token="XXXXX")


def main():
    # Remove existing rules
    existing_rules = client.get_stream_rules()
    if 'data' in existing_rules and len(existing_rules['data']) > 0:
        rule_ids = [rule['id'] for rule in existing_rules['data']]
        client.delete_stream_rule_ids(rule_ids)

    # Add new rules
    # Replace the rules below with the ones that you want to add as discussed in module 5
    new_rules = [
        {"value": "covid OR covid19 lang:en", "tag": "covid-related-tweets"},
        {"value": "corona OR coronavirus", "tag": "corona-related-tweets"}
    ]
    added_rules = client.add_stream_rules(rules=new_rules)

    # Connect to the filtered stream
    for count, result in enumerate(client.stream()):
        # The Twitter API v2 returns the Tweet information and the user, media etc.  separately
        # so we use expansions.flatten to get all the information in a single JSON
        tweet = expansions.flatten(result)
        # Here we are printing the full Tweet object JSON to the console
        print(json.dumps(tweet))
        # Replace with the desired number of Tweets
        if count > 100:
            break

    # Delete the rules once you have collected the desired number of Tweets
    rule_ids = [rule['id'] for rule in added_rules['data']]
    client.delete_stream_rule_ids(rule_ids)


if __name__ == "__main__":
    main()

```

The source code for this sample can be found [here](../labs-code/python/standard-product-track/stream_filtered.py)

## 9. Get followers for a user

```Python
from twarc import Twarc2, expansions
import json

# Replace your bearer token below
client = Twarc2(bearer_token="XXXXX")


def main():
    # The followers function gets followers for specified user
    followers = client.followers(user="twitterdev")
    for page in followers:
        result = expansions.flatten(page)
        for user in result:
            # Here we are printing the full Tweet object JSON to the console
            print(json.dumps(user))


if __name__ == "__main__":
    main()

```

The source code for this sample can be found [here](../labs-code/python/standard-product-track/get_followers.py)

## 10. Get users that a user is following

```Python
from twarc import Twarc2, expansions
import json

# Replace your bearer token below
client = Twarc2(bearer_token="XXXXX")


def main():
    # The following function gets users that the specified user follows
    following = client.following(user="twitterdev")
    for page in following:
        result = expansions.flatten(page)
        for user in result:
            # Here we are printing the full Tweet object JSON to the console
            print(json.dumps(user))


if __name__ == "__main__":
    main()

```

The source code for this sample can be found [here](../labs-code/python/standard-product-track/get_following.py)

## 11. Lookup Users using User IDs

```Python
from twarc import Twarc2, expansions
import json

# Replace your bearer token below
client = Twarc2(bearer_token="XXXXX")


def main():
    # List of user IDs to lookup, add the ones you would like to lookup
    users = ['2244994945', '783214', '6253282']
    # The user_lookup function gets the hydrated user information for specified users
    lookup = client.user_lookup(users=users)
    for page in lookup:
        result = expansions.flatten(page)
        for user in result:
            # Here we are printing the full Tweet object JSON to the console
            print(json.dumps(user))


if __name__ == "__main__":
    main()

```

The source code for this sample can be found [here](../labs-code/python/standard-product-track/user_lookup.py)

## 12. Lookup Tweets using Tweet IDs

```Python
from twarc import Twarc2, expansions
import json

# Replace your bearer token below
client = Twarc2(bearer_token="XXXXX")


def main():
    # List of Tweet IDs you want to lookup
    tweet_ids = ['1404192093803741184', '1403738886275096605', '1397216898593525762']
    # The tweet_lookup function allows 
    lookup = client.tweet_lookup(tweet_ids=tweet_ids)
    for page in lookup:
        # The Twitter API v2 returns the Tweet information and the user, media etc.  separately
        # so we use expansions.flatten to get all the information in a single JSON
        result = expansions.flatten(page)
        for tweet in result:
            # Here we are printing the full Tweet object JSON to the console
            print(json.dumps(tweet))


if __name__ == "__main__":
    main()

```

The source code for this sample can be found [here](../labs-code/python/standard-product-track/tweets_lookup.py)

## 13. Keeping your dataset compliant

In this code sample, we will read a file with Tweet IDs, one Tweet ID per line and call the tweet_lookup method. We will compare the Tweet IDs returned from this endpoint with the Tweet IDs in our original dataset. The difference between the 2 will give us the Tweet IDs that are not compliant (e.g. deleted Tweets or from suspended accounts)

```Python
from twarc import Twarc2, expansions

# Replace your bearer token below
client = Twarc2(bearer_token="XXXXX")


def main():
    # Path and name of file that contains the Tweet IDs, one Tweet ID per line
    read_path = 'ids.txt'

    compliant_tweet_ids = []

    with open(read_path) as f:
        all_tweet_ids = f.read().splitlines()
    # The tweet_lookup function will look up Tweets with the specified ids
    lookup = client.tweet_lookup(tweet_ids=all_tweet_ids)
    for page in lookup:
        result = expansions.flatten(page)
        for tweet in result:
            compliant_tweet_ids.append(tweet['id'])
    # Here we get a difference between the original 
    non_compliant_tweet_ids = list(set(all_tweet_ids) - set(compliant_tweet_ids))
    # Here we are printing the list of Tweet IDs that are not compliant in your dataset
    print(non_compliant_tweet_ids)


if __name__ == "__main__":
    main()

```

The source code for this sample can be found [here](../labs-code/python/standard-product-track/compliance.py)

So this concludes the labs for the standard product track. In the next section, let us take a look at some additional information and best practices around storing Twitter data and how you can keep you dataset in compliance with Twitter's developer policy.

[![Next](../assets/next.png)](../modules/7-storage-and-compliance.md)
