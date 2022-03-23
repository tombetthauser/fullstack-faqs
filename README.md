![fullstack bbq](https://media.tenor.co/images/8a95f6c7faba89227ff436a250a53948/raw)

# Fullstack FAQs

An ongoing list of common questions and solutions observed during fullstack portfolio project development, specifically for Flask and Express React applications.

---

## AWS S3 Acl Permissions

***Issue:*** 
* Errors related to AWS S3 acl (access control list) permissions not being enabled

***Solution:*** 
* Google exact error message
* Look for instructions on how to enable acl permissions
* There should be a checkbox in the browser interface for your AWS S3 bucket

## WSL Unexpectedly Devouring Memory (Windows users)
There have been a number of incidence where students encounter an issue where WSL for some reason begins devouring memory without bound, causing systems to slow to a hault and become unusable.
If you are experience extreme system slowdowns and breakage when starting your VSCode or WSL, have a look at your task manager.

![task_manager_ex](https://i.ibb.co/Sndpy2z/wsldeath.png)

Notice the insane memory usage of process vmmem, if you are seeing this, this most likely means that WSL is being allocated too much virtual memory; so while this much memory is not actually being dedicated to any real process, it is marked as being consumed as far as the Windows kernel is concerned. 

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

NOTES:
- You can easily access user profile directory by entering %USERPROFILE% in file manager.
- If config is not loading, first verify that .wslconfig DOES NOT have .txt extension. To verify it does not, make sure you do not have hidden file extensions option enabled in your file manager. (Views > Options)

For more details, please read this article: https://blog.simonpeterdebbarma.com/2020-04-memory-and-wsl/

## Mix Content Error
"Mixed content occurs when initial HTML is loaded over a secure HTTPS connection, but other resources (such as images, videos, stylesheets, scripts) are loaded over an insecure HTTP connection."

![image (4)](https://user-images.githubusercontent.com/76798385/159103068-49f56ab8-e350-42b7-8c92-6a7c075e8732.png)

If you encounter this error, you most likely have a trailing slash in the fetch request or in the backend API route. Make sure the resource (URL) you pass in the global fetch() method on the frontend exactly matches the API route in the backend. If the resources (URLs) are not exactly matched, you may encounter a mix content error on Heroku. 

***Good Example***: The resource in the Fetch Method ***"/api/users/1/"*** has to exactly match the backend API route ***"/api/users/1/"***


***Bad Example***: The resource in the Fetch Method ***"/api/users/1"*** does NOT match the backend API route ***"/api/users/1/"***

For more details about mix content errors, visit https://developer.mozilla.org/en-US/docs/Web/Security/Mixed_content/How_to_fix_website_with_mixed_content

You can also add ```<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">``` in the header tag and this will "upgrade" any resources that is loaded over HTTP to HTTPS. More information visit https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests.

## Heroku: Assets Retrieved From Backend API Calls Not Rendering
Make sure that a trailing backslash is appended to paths in fetches within action thunks or straight fetch calls; i.e.
```
fetch('/api/images/');
```
and NOT
```
fetch('/api/images');
```
