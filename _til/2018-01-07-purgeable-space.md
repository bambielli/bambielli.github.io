---
title:  "How to Reclaim Purgeable Space on Mac"
date:   2018-01-07 11:43:00
category: til
tags: [purgeable, space, purgeable space, purgeable-space, disk utility, disk, hard disk, hard drive, memory, storage, cleanmymac3, clean, my, mac]
---

TIL about Purgeable Disk Space in OSX, and after hours of struggling, how to reclaim it.

[Link to step by step instructions at the end of this post for reclaiming purgeable space on your mac.][link]

## What is Purgeable Space?

Purgeable Disk Space is a "feature" of more recent versions of OSX. It is storage on your hard drive that the operating system sets aside for files that it thinks you might access again in the future.

An example of files that are moved to `purgeable space` is files that you send to your remote iCloud storage. Presumably, you are sending files to the cloud to free up space on your local machine... if there is space available locally on disk, though, **OSX will keep these files around in `purgeable space`** which speeds up accessing the files again from your local machine.

In my opinion, it's a bit of a silly assumption on behalf of the OS to keep files around that the user is asking to send to remote storage.

I ran in to issues with purgeable space when trying to partition my hard drive for a Windows installation. I had over 40GB of my hard drive confined to purgeable space, and even though that storage counts as "free" from an OSX perspective, it is still technically allocated. This prevented me from being able to pull in as many GB in to my Windows partition as I needed.

Even though I haven't experienced it, I have also heard of folks running in to issues installing games or downloading large files onto their machines, and hitting the purgeable space wall.

There has to be a way to reclaim purgeable space from the OS, right?

## How to Free Purgeable Space

It isn't as easy as it sounds.

There are apps like [cleanmymac3][cmm]{:target="_blank"} which offer to clean purgeable space on your machine (for a fee) but I wanted to find a way to reclaim this space without the need for a paid service.

`Purgeable space` is freed when you ask the operating system to store a `new file` that exceeds the amount of true free space left on disk.

For example, if I have 15GB of space left on my disk, 5GB is truly "free" and 10GB is purgeable, If I create a file that is 6GB large, I will use up the rest of the "free" space and 1GB of purgeable.

Once the file that reclaimed the purgeable space is deleted, **the space goes back to being truly free, instead of purgeable again!**

So the trick is to create enough large files locally that all of the purgeable space is reallocated to support the new large files. After deleting those files, your space will return to a truly free state, instead of returning to purgeable.

**One important caveat** that I learned while attempting this solution: if you create a big file locally, and duplicate it over and over again with `cmd+c/cmd+v` or `cmd+d`, your disk space will not be properly filled up. This is an OSX trick to try and conserve hard disk space for duplicate files, by just creating new references to the original file instead of brand new copies of the file itself.

This is a good feature in most scenarios, just not in our immediate case where we actually want to fill up our disk as fast as possible.

See my step-by-step instructions for creating large files and duplicating them in the next section:

## Step by Step Instructions:

1. Open your terminal by searching for `terminal` in spotlight (open spotlight with `cmd+spacebar`)

2. In the terminal, execute `mkdir ~/largefiles`
  - This creates a new folder called "largefiles" in your home directory.

3. In the terminal, execute `dd if=/dev/random of=~/largefiles/largefile bs=15m`
  - This will create a new file called "largefile" in your largefiles folder, which contains the random output from /dev/random.
  - NOTE: this command will cause your terminal to appear like it is frozen... that is expected, as the command is running!

4. After a few minutes (around 5), hit `ctrl+c` in the terminal window to kill the command from step 3.

5. In the terminal, run the command `cp ~/largefiles/largefile ~/largefiles/largefile2`
  - This will copy the largefile that was created in step 3 to a new file called "largefile2".
  - Remember, this is different than just running `cmd+d` or `cmd+c/cmd+v` on the file... it's forcing the file to be copied over in its entirety, filling up more space on disk.

6. Continue to run the copy command from step 5, changing the name of the copy destination from `largefile2` to something different each time.
  - Change the copy destination name to something like `largefile3`, `largefile4`, etc...
  - You should continue to run this command until you see a OSX message appear that says "disk is critically low".

7. After you see the `disk critically low` message from OSX, execute `rm -rf ~/largefiles/`
  - This will delete all of the largefiles from your system.
  - Make sure you `empty the trash bin` as well, or the files will just sit in there taking up space!

8. Open disk utility by searching for `disk utility` in spotlight
  - You should see either no amount of purgeable space, or a very small amount of purgeable space remaining in your hard drive snapshot!

<figure>
  <img src="/assets/images/PurgeableSpace.png">
  <figcaption>My Disk Utility after running the procedure above. Very little purgeable space remaining!</figcaption>
</figure>

The procedure above was inspired from [this stack overflow post][so]{:target="_blank"}.

I hope the above was helpful! Feel free to reach out to me if you're still having trouble freeing the purgeable space on your mac.

[cmm]: https://macpaw.com/store/cleanmymac
[so]: https://apple.stackexchange.com/questions/254676/how-do-i-clear-the-purgeable-area-on-my-disk
[link]: /til/2018-01-07-purgeable-space/#step-by-step-instructions
