# ceph-osd-mapping
## TLDR (Or go right to the [rant below](#rant))
This project is meant to map an OSD to it's corresponding Logical Volume, and device associated with it.  It will also coorelate which disks don't have OSDs currently managing them and list those separately.

## Rant
I've been doing a bunch with Ceph lately and have found it's pretty annoying to see which device an OSD maps to.  In the newest Ceph v13.2.x (AKA *Mimic*) bluestore devices are created using Logical Volumes which adds one more layer of abstraction on top of the "filesystem" (basically the raw device with bluestore).  

For example, the current workflow for finding a device from an OSD is:

**OSD 25 -> Logical Volume -> Physical Volume -> Device -> Actual Disk**.  

This is pretty annoying but the biggest problem, in my opinion, is trying to find which device went down... So probably the most important problem.  Basically what you'd have to do is go from the Logical Volumes and figure out which device is **NOT** there.  This is obviously a pretty long process if you have over 5 disks in one OSD node.