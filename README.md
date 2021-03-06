![visibly confused obi wan](https://media0.giphy.com/media/1X7lCRp8iE0yrdZvwd/giphy.gif?cid=ecf05e4744ex0860xek4aze59njvn2abamk8y5d3nackahaq&rid=giphy.gif&ct=g)

# Capstone FAAQs
*(Frequently/Any Asked Questions)*
> An informal and very incomplete list of somewhat common and / or unique problems observed 
> by the Mod 7 TAs during capstone project development. You may find the answers you seek here, 
> but if not we will continue to add to it whenever possible! Specifically for **Flask** and
> **Express** React applications.
> 
> [\[Jump to end of list\]](https://github.com/tombetthauser/fullstack-faqs#end-of-list)

## Table of Contents
1. **[Frequently Asked Questions/Issues](https://github.com/tombetthauser/fullstack-faqs#1-frequent-questions--issues)**
	- [AWS S3 Acl Permissions](https://github.com/tombetthauser/fullstack-faqs#aws-s3-acl-permissions)
	-  [WSL Unexpectedly Devouring Memory (Windows users)](https://github.com/tombetthauser/fullstack-faqs#wsl-unexpectedly-devouring-memory-windows-users)
	- [Mix Content Error](https://github.com/tombetthauser/fullstack-faqs#mix-content-error)
	- [Heroku: Assets Retrieved From Backend API Calls Not Rendering](https://github.com/tombetthauser/fullstack-faqs#heroku-assets-retrieved-from-backend-api-calls-not-rendering)
	- [Heroku: 500 level errors on login / signup](https://github.com/tombetthauser/fullstack-faqs#heroku-500-level-errors-on-login--signup)
	- [npm ERR! code ENOSPC](https://github.com/tombetthauser/fullstack-faqs#npm-err-code-enospc)
	- [GitHub Secrets aka Environment Variables for GitHub Actions](https://github.com/tombetthauser/fullstack-faqs#github-secrets-aka-environment-variables-for-github-actions)
	- [Using Object.values](https://github.com/tombetthauser/fullstack-faqs#using-objectvalues)
	- [Proper use of Google Maps API](https://github.com/tombetthauser/fullstack-faqs#proper-use-of-google-maps-api)
	- [Unexpected token < in JSON at position 0](https://github.com/tombetthauser/fullstack-faqs#unexpected-token--in-json-at-position-0)
	- [AWS AccessDenied when calling the PutObject error](https://github.com/tombetthauser/fullstack-faqs#AWS-AccessDenied-when-calling-the-PutObject-error)
	- [Fetch blocked by CORS policy when using wavesurfer.js](https://github.com/tombetthauser/fullstack-faqs#fetch-blocked-by-CORS-policy-when-using-wavesurfer)
	- [useState Variable Not Updating Correctly](https://github.com/tombetthauser/fullstack-faqs#usestate-variable-not-updating-correctly) 
2. **[All Questions List](https://github.com/tombetthauser/fullstack-faqs#2-any--all-questions)**
	> *Search page for tags using* `Ctrl + F` / `Cmd + F`*, including the brackets.*
	
	Tags:
	- **[Auth], [Unauthorized], [Log out], [CSS], [Overlap], [Meta], [Form]**
	- **[Input], [Required], [Validation], [Docker], [Dockerfile], [Build], [404], [Static]**
	- **[Await], [Async], [.then()], [loading mechanism]**
	- **[AWS], [Access Key], [error], [bucket], [security]**
	- **[migrate], [alembic], [versions], [flask], [upgrade], [init], [models], [sqlalchemy]**
3. **[Additional Resources](https://github.com/tombetthauser/fullstack-faqs#3-additional-resources)**
-----------------------------------

# 1. Frequent Questions & Issues
![IDK](https://www.wired.com/wp-content/uploads/2015/03/855.gif)

---
###  AWS S3 Acl Permissions

***Issue:*** 
* Errors related to AWS S3 acl (access control list) permissions not being enabled

***Solution:*** 
* Google exact error message
* Look for instructions on how to enable acl permissions
* There should be a checkbox in the browser interface for your AWS S3 bucket

---
## WSL Unexpectedly Devouring Memory (Windows users)

***Issue:*** 
There have been a number of incidence where students encounter an issue where WSL for some reason begins devouring memory without bound, causing systems to slow to a hault and become unusable.
If you are experience extreme system slowdowns and breakage when starting your VSCode or WSL, have a look at your task manager.

![task_manager_ex](https://i.ibb.co/Sndpy2z/wsldeath.png)

Notice the insane memory usage of process vmmem, if you are seeing this, this most likely means that WSL is being allocated too much virtual memory; so while this much memory is not actually being dedicated to any real process, it is marked as being consumed as far as the Windows kernel is concerned. 

***Solution:*** 
To fix this, you will first want to note the amount of memory your system has. After you have done this, create a .wslconfig file in your home user directory, i.e. C:\\User\\[YOUR_USERNAME]. Inside of that file, include the following

```
[wsl2]
memory=<MAX_MEMORY>
```
Where <MAX_MEMORY> is approximately 1/4 the total memory of your system in G. For instance, if you had 8 GB RAM, your .wslconfig should something look like
```
[wsl2]
memory=2GB
```
Restart your machine and observe your memory usage. Run free -g in your WSL shell, if allocated memory matches your config, it works.

***NOTES:***
- You can easily access user profile directory by entering %USERPROFILE% in file manager.
- If config is not loading, first verify that .wslconfig DOES NOT have .txt extension. To verify it does not, make sure you do not have hidden file extensions option enabled in your file manager. (Views > Options)

For more details, please read this article: https://blog.simonpeterdebbarma.com/2020-04-memory-and-wsl/

------
## Mix Content Error

***Issue:***
"Mixed content occurs when initial HTML is loaded over a secure HTTPS connection, but other resources (such as images, videos, stylesheets, scripts) are loaded over an insecure HTTP connection."

![image (4)](https://user-images.githubusercontent.com/76798385/159103068-49f56ab8-e350-42b7-8c92-6a7c075e8732.png)

If you encounter this error, you most likely have a trailing slash in the fetch request or in the backend API route. Make sure the resource (URL) you pass in the global fetch() method on the frontend exactly matches the API route in the backend. If the resources (URLs) are not exactly matched, you may encounter a mix content error on Heroku. 

***Good Example***: The resource in the Fetch Method ***"/api/users/1/"*** has to exactly match the backend API route ***"/api/users/1/"***


***Bad Example***: The resource in the Fetch Method ***"/api/users/1"*** does NOT match the backend API route ***"/api/users/1/"***

***NOTES:***
For more details about mix content errors, visit https://developer.mozilla.org/en-US/docs/Web/Security/Mixed_content/How_to_fix_website_with_mixed_content

You can also add ```<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">``` in the header tag and this will "upgrade" any resources that is loaded over HTTP to HTTPS. More information visit https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests.

------
## Heroku: Assets Retrieved From Backend API Calls Not Rendering
Make sure that a trailing backslash is appended to paths in fetches within *action thunks* or straight *fetch calls*; i.e.
```
fetch('/api/images/');
```
and NOT
```
fetch('/api/images');
```

---
## Heroku: 500 level errors on login / signup
* triple-check environment variables and PostgreSQL installation on Heroku specifically
* make sure to actually add environment variables on Heroku, they have to be saved

---
## npm ERR! code ENOSPC
This means there is no space left on user's device. Appears to be insufficient space on your system to finish. 
You have to clear up some disc space (try deleting unused docker container, images) and then try running the command again. 

```
docker system prune
```
This will remove all unused containers, networks, images to help free up space.

---
## GitHub Secrets aka Environment Variables for GitHub Actions
- Q: How do I reference sensitive environment variables I would typically store in a local .env file when I do not expose that file to git version control?
- A: GitHub Secrets: https://docs.github.com/en/actions/security-guides/encrypted-secrets

---
## Using Object.values 
- Q:  I have the query result ordered by ratings of the jokes in descending order from the backend. However, on the frontend the query is still rendering the result in order of the id (back to ascending order). 
- A: When dealing with an Array-like object with random key ordering, notice how the array is rearranged in acending order based on the numberic key. When using numeric keys, **the values are returned in the keys' numerical order**. 
```
const arrayLikeObj2 = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.values(arrayLikeObj2 )); // ['b', 'c', 'a']
```
- Visit https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values for more information.

---
## Proper use of Google Maps API
Google Maps API comes in an embeddable form which references an old version with deprecated methods: this is intended for use in static pages which only reference the API via iframes, as it allows the user to interface with the API without using Javascript. Google provides a developer targeted API and an associated npm package for interfacing with it. The documentation for this is here:
- https://developers.google.com/maps/documentation/javascript/overview#maps_map_simple-javascript

---
## Unexpected token < in JSON at position 0
*Sorry! Don't have a very helpful answer for this!* This error (and variations of this error) occur for many many reasons. A step closer to figuring out these messages would be to add custom error handling for Flask to be able to read the JSON errors.

In `app/__init__.py` add the following routes:
```py
@app.errorhandler(405)
def method_not_allowed(e):
    return {"errors": ["Method Not Allowed"]}, 405
```
In each api `--_routes.py`files and change the ROUTENAME to the name of that file's Blueprint:
```py
@ROUTENAME_routes.errorhandler(500)
def internal_server_error(e):
    return {"errors": ["Internal Server Error"]}, 500
```
- [Flask Docs on Blueprint Error Handlers](https://flask.palletsprojects.com/en/2.1.x/errorhandling/#blueprint-error-handlers)

---
## AWS AccessDenied when calling the PutObject error

Credit to Nicholas Yuan for putting this resource together for all of us!

Possible errors to keep an eye on:

``` 
{"errors":"An error occurred (SignatureDoesNotMatch) when calling the PutObject operation: 
The request signature we calculated does not match the signature you provided. 
Check your key and signing method."}
```
```
 "An error occurred (AccessDenied) when calling the PutObject operation: Access Denied" 
 This error message indicates that your IAM user or role needs permission for the kms:GenerateDataKey action. 
 This permission is required for buckets that use default encryption with a custom AWS KMS key
```
Possible reasons and solutions:
1. It may be that the actual environment variables in the pipenv shell are being used instead of the environment variables in the .env file. If you see S3_BUCKET or access keys in the terminal when you run printenv , those will be used before the ones in .env files. To reset these, just restart your shell / terminal. The environment variables that are there are loaded from your ~/.bashrc (or equivalent) on terminal startup. Also pipenv shell will inherit environment variables from your terminal, but if you set an environment variable in the pipenv shell, it will disappear on exiting the pipenv shell.
2. For some reason, some buckets require signature_version to be s3v4 , as opposed to v3 or v4. I would recommend everyone use this signature_version, unless it for some reason doesn't work with buckets than can use v3. [More reading here](https://boto3.amazonaws.com/v1/documentation/api/1.9.42/guide/s3.html#generating-presigned-urls).
3. If it works on localhost but not on Heroku, it may be because a region has not been set, and Heroku's datacenters are on AWS's us-east-1. So if you are using a different region than this on your bucket, the requests from Heroku are going to the wrong region.
```
from botocore.config import Config
my_config = Config(
    region_name = 'us-east-1',
    signature_version = 's3V4',
    retries = {
        'max_attempts': 10,
        'mode': 'standard'
    }
)

s3 = boto3.client(
    "s3",
    aws_access_key_id=os.environ.get("S3_KEY"),
    aws_secret_access_key=os.environ.get("S3_SECRET"),
    config=my_config
)
```
---
## Fetch blocked by CORS policy when using wavesurfer
![Screen Shot 2022-06-13 at 9 49 39 AM (1)](https://user-images.githubusercontent.com/88914047/173454161-3f6aff6f-67f4-48b8-ae8f-ac77a1038d2a.png)

Posible solution to this is to configure the xhr option when creating an instance of the player (with const waveSurfer = WaveSurfer.create({ ... }))
Setting the ```mode: "no-cors"``` attribute will allow us to fetch the resource with CORS disabled. 
Example code:
```
const waveSurfer = WaveSurfer.create({
      container: containerRef.current,
      waveColor: "#D8D8D8",
      progressColor: "#ED2784",
      barRadius: 3,
      cursorColor: "transparent",
      responsive: true,
      xhr: {
        cache: "default",
        mode: "no-cors",
        method: "GET",
        credentials: "include",
        headers: [
          { key: "cache-control", value: "no-cache" },
          { key: "pragma", value: "no-cache" }
        ]
      }
});
```

## useState Variable Not Updating Correctly
Make sure you are ***not*** setting the useState variable's default value with something that comes from the Redux state that you've grabbed with useSelector.
- First off, there is a delay in dispatches and updating the state, so it can be unreliable in grabbing the info and give you undefined. And depending on the variable, this could mess up other code that relies on reading this value.
- Secondly, this will not update on a re-render. JS works from top to bottom and once it goes past that initialization of the useState variables, it won't go back to it. You need to use a useEffect() for anything to change--since they run on re-renders!
> Bad example:
```js
// ??? WHAT NOT TO DO ??? ???
const post = useSelector((state) => state.session?.post);

const [shareBool, setShareBool] = useState(post[1]?.share);
...
```
> Good example:
```js
// ??? WHAT TO DO ??? ???
const post = useSelector((state) => state.session?.post);

const [shareBool, setShareBool] = useState('');

useEffect(() => {
	if (post && Object.values(post).length > 0) {
        setShareBool(post[1]?.share)
    }
}, [post])
```

 - Now the variable will be updated on the first render and on subsequent re-renders when the dependencies change!
 - You put the set function in the if state to make sure it's not going to run  before the Redux state has loaded. `Remember that grabbing from the state is an object and in Javascript, an empty object is truthy.`
 - The default value can be set to anything else, as long as its not relying on something that has to load. `false, {}, [], "default", etc...`


--------------------------------------------------------------------------------------------------------------------

# 2. Any & All Questions 
![milo questions](https://66.media.tumblr.com/f63b7d217d4606797a5940d9dc77eb4a/tumblr_na95wuGHdN1tq4of6o1_500.gif)

*Note to editors:*
>  - *Please add tags in the format of square brackets around a keyword, so searching with* `Ctrl + F` / `Cmd + F` *is more
> precise. Ex: **"[Express]"** not just **"Express"**.*
>  - *Then, please add any new unique tags to the tags list in Table of Contents!*
>  - *If you've come to write a question, but it has already been included here,  put a ?????? or  ??? next to the title. Then we can determine which questions can be moved to the FAQ section.*

---
## User is Logged out/Session disappears on Refresh
???? **Tags:** *[Auth] [Unauthorized] [Log out]*

***Issue:***
The session user disappears on refresh, essentially logging out a user. From the auth thunk in session.js, console.logging the error  gives "Unauthorized". The api route /api/auth is not being hit. 

***Solution:*** 
The functions that handle the auth are all working in the starter. This occurs when the trailing "/" is removed from `authenticate` function in session.js. It must be there to hit the route. 
```
const response = await fetch('/api/auth/', { ...
```
---
## Minor CSS overlapping issue
???? **Tags:** *[CSS] [Overlap] [Meta]*

***Issue:***
The styling and formatting of divs only works on very large screens for various reasons.

***Solution:***
(Quick fix) Put this meta tag within the `<head>` HTML tag. 
Adjust the initial-scale value to what works best; this is essentially zooming out the users screen automatically.
```
<meta name="viewport" content="width=device-width, initial-scale=0.5">
```
---
## Required Attribute Not Working!
???? **Tags:** *[Form] [Input] [Required] [Validation]*

***Issue:***
The requried attribute isn't stopping a form from submitting a blank input.

***Solution:***
A few reasons why this could be happening, here are a few things to check:
1. Input tag must be inside a form tag.
2. Form needs to have a `type=submit` button/div/input. 
3. The input tag's event attribute must be `onSubmit=...`

---

## Weird Fetch 404 Errors only on Heroku
???? **Tags:** *[Docker], [Dockerfile], [Build], [404], [Static]*

***Issue:***
App works on localhost, but on Heroku fetches fail. Specifically getting 404 errors from `method=GET path="/static/css/main.3467ce7e.chunk.css` (and other places similar to this). However, Github actions doesn't throw any errors in the build.

***Solution:***
Typos within the Dockerfile. Most common ones we've seen is a missing slash after "build".
It should look like this: 
```
/react-app/build/* app/static/
```
---

## Alternative to Async/Await - .then()
???? **Tags:** *[Await], [Async], [.then()], [loading mechanism]*

***Issue:***
If async/await-ing a dispatch isn't working correctly. Like, a second dispatch is running before the first one finishes. Amd you need the first dispatch to completely finish before moving onto the second dispatch...

***Solution:***
In React, it is typical to chain .then() as a loading mechanism. Especially when you get a warning on VSCode that says "await has no effect on the dispatch".
Like so:
```
dispatch(createLike(like).then(() => dispatch(getMatches))
```
[Here is an article detailing why & the key difference](https://dev.to/kylejb/a-key-difference-between-then-and-async-await-in-javascript-53e9)

---

## AWS Access Key Exposed -- Error Message
???? **Tags:** *[AWS], [Access Key], [error], [bucket], [security]*

***Issue:***
Suddenly your AWS image urls aren't working and you are getting this error:
```
cannot schedule new futures after interpreter shutdown
```

***Solution:***
Check your email and your AWS account. This is from a security issue, most likely from your access key being uploaded to your github repo and being exposed! They shut it down to prevent it from being used anymore.
1. Disable and delete exposed access key.
2. Create new access key
3. Update `.env` file!

---

## Alembic Migration File not Updating
???? **Tags:** *[migrate], [alembic], [versions], [flask], [upgrade], [init], [models], [sqlalchemy]*

***Issue:***
When running `db flask migrate` the migration file does not change. Possibly only includes the User table from the starter. 

***Solution:***
In the `models/__init__.py` file, check if all your models are imported. The starter will only have User imported. Follow its example to import all models. If you had already upgraded, either `flask db downgrade` or manually delete all the tables in Postbird. And make sure to delete the alembic table in Postbird. Then `flask db migrate`. You will also want to reset your Heroku database too. (See [Helpful Heroku Tips](https://github.com/whitnessme/helpful-heroku-tips) if you don't know how to reset it. It is better to do this than remove the Postgres add-on.)

```
from .db import db
from .user import User
```

---

# 3. Additional Resources
![more please gif](https://media0.giphy.com/media/FyKfqRxVbzciY/giphy.gif?cid=790b7611a132ec08c46a50eaea6072821c8fb18362a86592&rid=giphy.gif&ct=g)
### Capstone:
- [Module 7 Welcome](https://github.com/tombetthauser/module-7-welcome)
- [Helpful Heroku Tips](https://github.com/whitnessme/helpful-heroku-tips)
- [Capstone Required Error Messages](https://github.com/whitnessme/capstone-minimum-required-error-messages)
- [SQLAlchemy Multiple Relationships Between Two Tables](https://github.com/whitnessme/sqlalchemy_multiple_relationships_between_two_tables)

### Career Quest:
- [LinkedIn: How to properly link your work on the featured section of your profile as a developer](https://www.linkedin.com/pulse/how-properly-link-your-work-featured-section-profile-jeremy-seckinger/)
- [Meta Tags -- Preview, Edit, and Generate](https://metatags.io/)
- [LinkedIn: Reset Cache/Get insights into how content shows up](https://www.linkedin.com/post-inspector/)

##### End of List
##### [\[Back to Top\]](https://github.com/tombetthauser/fullstack-faqs#full-stack-faqs--all-questions-list)
