# Dear Diary Writeup

This one took me quite long to solve. An interesting disk image challenge it is.



> #### Description
> 
> If you can find the flag on this disk image, we can close the case for good! Download the disk image [here](https://artifacts.picoctf.net/c_titan/63/disk.flag.img.gz).

> #### Hint
> 
> If you're observing binary data raw in the terminal you may be misled about the contents of a block.



So all we gotta do is find the flag in the disk image. Shouldnt be that hard right? haha.. 



Since its a disk image, I analyze the disk img using autopsy. I tried to search for `flag`, `pico` and `diary` but nothing interesting comes out. Then I tried `.txt` because flag usually kept in .txt files.

![Screenshot 2025-11-25 124606.png](Screenshot%202025-11-25%20124606.png)

after searching through the .txt, i finally found this weirdly-named file `inocuous-file.txt`. Looking at the filepath we can see where it is hidden so I take a look at the folder.

![Screenshot 2025-11-25 124908.png](Screenshot%202025-11-25%20124908.png)

Surprisingly, it shows that the file is 0kb in size which mean its empty so i cannot simply extract the file...


After a few hours stuck here, I try to start fresh and read the hint again:

> If you're observing binary data raw in the terminal you may be misled about the contents of a block.

Maybe the clue is saying that the 0kb is misleading?



So I tried to strings + grep the innocuous-file.txt from my terminal to inspect more:

![Screenshot 2025-11-25 125152.png](Screenshot%202025-11-25%20125152.png)

and wow, we found out that there are multiple innocuous-file.txt but maybe these arent showed in the autopsy because it is deleted (inside the unallocated space)



Then I tried to read the file contents using the command:

```bash
grep -a "innocuous-file.txt" disk.flag.img
# use -a to treat the file as ascii txt even though it is binary so it will display file content
```

The output:

![Screenshot 2025-11-25 125723.png](Screenshot%202025-11-25%20125723.png)

We can see the flag is scattered!



Combining all the parts and we will get:

`picoCTF{1_533_n4m35_80d24b30}`


