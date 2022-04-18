# Corrupted

We have Corrupted.001 file without any extension.
When we try to figure out what is this using `file Corrupted.001` we just get 'data' instead of file type.
So, let's open it with Ghex

![](pics/corrupted1.png)

We see a lot of trash before a valid NTFS-header `EB 52 90 4E 54 46 ...`. We just delete this bytes and save files as "Corrupted". Next, mount it using `ntfs-3g` and see a flag inside a directory with pictures:

![](pics/corrupted2.png)
![](pics/corrupted3.jpg)