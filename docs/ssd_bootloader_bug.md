SSD Stopped working after an update seems like Partition ID was going to the wrong spot

What I did :
Plug ssd into another Jetson or computer
Change etc/fstab with the partition id using lsblk and blkid if it is generic replace with  this UUID

I also changed extlinux.conf but i dont think that is really a problem not sure 

Then What I did is go to the boot menu from startup and go to boot manager and change extlinux to kernel 

I believe my extlinux did not save 

So you can change either file 
