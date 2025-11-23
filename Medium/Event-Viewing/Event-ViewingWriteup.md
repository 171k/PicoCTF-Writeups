# Event-Viewing Writeup

> Description
> 
> One of the employees at your company has their computer infected by malware! Turns out every time they try to switch on the computer, it shuts down right after they log in. The story given by the employee is as follows:
> 
> 1. They installed software using an installer they downloaded online
> 2. They ran the installed software but it seemed to do nothing
> 3. Now every time they bootup and login to their computer, a black command prompt screen quickly opens and closes and their computer shuts down instantly.
> 
> See if you can find evidence for the each of these events and retrieve the flag (split into 3 pieces) from the correct logs!Download the Windows Log file [here](https://challenge-files.picoctf.net/c_verbal_sleep/123d9b79cadb6b44ab6ae912f25bf9cc18498e8addee851e7d349416c7ffc1e1/Windows_Logs.evtx)



As we know, we have to find 3 flags inside the event. This is a good challenge for beginner to start learning about event viewing and event id. I spent hours reading about event logs to understand this challenge so here we go!



> Hints
> 
> Try to filter the logs with the right event ID
> 
> What could the software have done when it was ran that causes the shutdowns every time the system starts up?



The hints give away a big clue to help us analyze the event logs. So we simply have to filter based on the event id. I use this [cheatsheet]([windows event logs cheat sheet · GitHub](https://gist.github.com/githubfoam/69eee155e4edafb2e679fb6ac5ea47d0)) to guide me. Now lets find each flag one by one.



> 1. They installed software using an installer they downloaded online

The first flag suggested that user installed the software using an installer so I scrolled through the cheatsheet and found this:

![Screenshot 2025-11-21 202707.png](Screenshot%202025-11-21%20202707.png)

For new MSI (Windows Installer) file download, there are 2 event id we can try to browse which is 1022 and 1033. So I tried both and found the first flag in 1033!

![Screenshot 2025-11-21 202941.png](Screenshot%202025-11-21%20202941.png)

Flag was hidden in base64. Decode this and get the first part: "picoCTF{Ev3nt_vi3wv3r_"



Since we already know that these flags are hidden in base64, we can simply just dump everything into txt and search for base64 to get the whole flags. BUT that would definitely defeat the purpose of this challenge which is to learn about event viewing. So lets continute our flag hunting!



> 2. They ran the installed software but it seemed to do nothing

The second one is quite tricky because when a software does this, it could have done many things including running a script or executing other software.  But..



> 3. Now every time they bootup and login to their computer, a black command prompt screen quickly opens and closes and their computer shuts down instantly.

Considering that this program have made its victim's computer to always shutdown. My guess is the program couldve edit the registry run key on startup! With this information, we can try reading the cheatsheet again to get a suitable event id.



![Screenshot 2025-11-21 204933.png](Screenshot%202025-11-21%20204933.png)

So I searched for "registry" and found this. 4657 could be the numbers we are looking for!

![Screenshot 2025-11-21 205103.png](Screenshot%202025-11-21%20205103.png)

Aha! so we found the 2nd flag here! The object value name literally is "immediate shutdown" which shows that the registry edit indeed change the computer behaviour to always shutdown on startup. Decode this base64 and get the 2nd flag: "1s_a_pr3tty_us3ful_".



> 3. Now every time they bootup and login to their computer, a black command prompt screen quickly opens and closes and their computer shuts down instantly.

Now, since the registry key has been edited to always shutdown on startup. Lets take a look at the cheatsheet againnnn.

![Screenshot 2025-11-21 205607.png](Screenshot%202025-11-21%20205607.png)

Okay so we have multiple options here. We will be choosing 1074 because the computer will logs the event as the user shutting down the computer since the registry key executed the shutdown on startup.

![Screenshot 2025-11-21 205757.png](Screenshot%202025-11-21%20205757.png)

and there we go, the final piece of our flag! Decode this from base64 again and get the flag: "t00l_81ba3fe9}"



Combining all the flags and we will get:

flag: 
`picoCTF{Ev3nt_vi3wv3r_1s_a_pr3tty_us3ful_t00l_81ba3fe9}`








