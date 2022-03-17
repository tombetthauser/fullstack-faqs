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

To fix this, you will first want to note the amount of memory your system has. After you have done this, create a .wslconfig file in your home user directory, i.e. C:\\User\\<You>. Inside of that file, include the following

```
[wsl2]
memory=<MAX_MEMORY>
```
Where <MAX_MEMORY> is approximately 1/4 the total memory of your system in G. For instance, if you had 8 GB RAM, your .wslconfig should something look like
```
[wsl2]
memory=2GB
```
Restart your machine and observe your memory usage. For more details, please read this article: https://blog.simonpeterdebbarma.com/2020-04-memory-and-wsl/
