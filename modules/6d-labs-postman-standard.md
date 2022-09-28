# Module 6D: Getting Started with Postman

<img width=100% src=https://user-images.githubusercontent.com/60015240/191588591-935e18ca-bf11-4cf0-a45a-39facb6b77a6.png>

# What is Postman

> "Postman is an API platform for building and using APIs. Postman simplifies each step of the API lifecycle and streamlines collaboration so you can create better APIs—faster."

Find more information about Postman [here](https://www.postman.com/product/what-is-postman/).

The Twitter API is accessible via Postman, which makes it simpler to get started with testing and exploring the API.

Let us look at setting up Authorization and explore some use cases below, to learn how Postman can be leveraged as a tool to enable use of the Twitter API.

# Setting up
## Fork the Postman Learning Modules collection using the Run in Postman button
In order to follow the learning module, please fork the `Twitter Academic Research API course - Postman modules` collection by selecting the **Run in Postman** button below.

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/21506762-d6778526-c8a7-4a88-8f01-da54d4926c05?action=collection%2Ffork&collection-url=entityId%3D21506762-d6778526-c8a7-4a88-8f01-da54d4926c05%26entityType%3Dcollection%26workspaceId%3D3f93788b-6ab4-48ea-ae1f-7ee642de36b9)

If you prefer to fork directly from the Postman app, you can accomplish a successful fork by selecting the horizontal ellipses (or [Meatballs](https://uxplanet.org/choose-correct-menu-icon-for-your-navigation-7ffc22df80ac)) menu on the collection, select **Create a Fork**, and provide details about the destination workspace.

For more information on Forking in Postman, see our [learning center documentation](https://learning.postman.com/docs/collaborating-in-postman/version-control/#forking-postman-entities).

If you would like to fork the Twitter API V2 collection for reference, you can do so at this [link](https://www.postman.com/twitter/workspace/twitter-s-public-workspace/collection/9956214-784efcda-ed4c-4491-a4c0-a26470a67400?action=share&creator=21506762).

<p align="center"> <img width=70% border-width=3px border-color="black" src=https://user-images.githubusercontent.com/60015240/187228316-b96f5316-3dad-4b9d-9c34-d219a5959396.gif> </p>

## Set up Authorization 

For these learning modules, you will set up authorization at the **Collection** level in Postman. In the [Postman collection](https://www.postman.com/the-student-programs-team/workspace/twitter-learning-modules-in-postman/collection/21506762-d6778526-c8a7-4a88-8f01-da54d4926c05?action=share&creator=21506762) which you have forked (either through the Run in Postman button or manually), authorization is set to Bearer Token at the collection level. Be sure to assign the `{{bearer_token}}` variable in order to authorize the requests within the collection. 

### Bearer Token

For some Twitter API V2 endpoints, a Bearer Token is all that's needed and will be provided to developers who have set up a project and app in Twitter's [developer portal](developer.twitter.com). In this case, you can insert the token provided by twitter by hard-coding it where you see `{{bearer_token}}` or by assigning the `bearer_token` variable in the **Variables** tab at the same collection level. 

<p align="center"> <img width=70% border-width=3px border-color="black" src="https://content.pstmn.io/75b06b8c-0c05-4eb5-97f8-232f135f5508/QXVnLTI2LTIwMjIgMTMtNTQtMTEuZ2lm" alt=""> </p>

<p align="center"> <img width=70% border-width=3px border-color="black" src=https://user-images.githubusercontent.com/60015240/191787830-9ac86159-3ebb-452f-9e88-9b3c80634d49.png> </p>

# Use Cases
Select the `Run in Postman` button to run a collection which contains all of the use cases below in order to see their documentation within Postman and practice making API calls to the Twitter API.

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/21506762-d6778526-c8a7-4a88-8f01-da54d4926c05?action=collection%2Ffork&collection-url=entityId%3D21506762-d6778526-c8a7-4a88-8f01-da54d4926c05%26entityType%3Dcollection%26workspaceId%3D3f93788b-6ab4-48ea-ae1f-7ee642de36b9)

>1. "I want to get Tweets about something that happened this past week."

>2. "I want to build the entire conversation thread for a Tweet including replies."

>3. "I want to get Tweets from a user’s timeline."

>4. "I want to write a response from a recent search to a text file."

>5. "I want to get a user's tweet timeline."

>6. "I want to get a user's mentions."

>7. "I want to collect tweets in real time."

>8. "I want to collect Tweets in real-time on a breaking event."

>9. "I want to get the followers for a user."

>10. "I want to get the users that a UserID is following."

>11. "I want to lookup users using User IDs."

>12. "A fellow researcher shared a set of Tweet IDs and I want build the Twitter dataset for these Tweet IDs."

>13. "I want to keep my dataset compliant."


# Get API certified through Postman Student Programs

If you're a student or educator learning or teaching APIs, the Postman student program provides training, resources, and opportunities to support and amplify your projects.

Find out more about Postman Student Programs [here](https://www.postman.com/student-program/) and check out our Student Expert training [here](https://www.postman.com/student-program/#student-expert-program).

<p align="center"><img width="40%" alt="Student hero image" src="https://user-images.githubusercontent.com/60015240/191765935-22599ee6-c1c5-4652-bdd9-96592a0f6879.png"></p>