# Service Note: AWS EBS Volume Management and Snapshot Recovery

**Date:** 05-12-2025  
**Created by:** Brite Sendedza  
**Service:** Amazon Elastic Block Store (EBS) & Amazon EC2  
**Region:** us-west-2  


---

## Overview

So today I got my hands dirty with **EBS volume management** and **snapshot recovery** on AWS. The whole point was to figure out how to properly attach storage to EC2 instances, create backups using snapshots, and actually restore data when things go sideways. This stuff is super important for production environments where you can't afford to lose data.

---

## Services I Worked With

### Amazon EC2 (Elastic Compute Cloud)
Alright, so EC2 is basically AWS's way of giving you virtual servers in the cloud. I was working with a **t3.micro** instance sitting in **us-west-2a**. Think of EC2 as the actual computer, and EBS is like the hard drive you plug into it. Pretty straightforward.

**What I actually used:**
- Launched and managed the instance
- Used EC2 Instance Connect to SSH in (way easier than dealing with key pairs)
- Paid attention to availability zones—turns out this matters a lot
- Checked status to make sure everything was healthy

### Amazon EBS (Elastic Block Store)
EBS is where your persistent storage lives. The cool thing about EBS is that your data sticks around even if you stop or kill your instance, which is different from instance store volumes that just... vanish. I went with **General Purpose SSD (gp2)** volumes because they're solid for dev work and general use cases.

**What I did with it:**
- Created volumes from scratch
- Attached them to my running instance
- Made snapshots for backups
- Restored volumes from those snapshots
- Had multiple volumes attached at once (pretty neat actually)

### Linux Filesystem Stuff
Once I got the volume attached to my EC2 instance, I had to prep it with regular Linux commands:
- mkfs to format the thing
- mount to actually make it usable
- ls and rm to test that everything was working

---

## What I Actually Did (Step by Step)

### 1. Checked What I Had
First thing I did was look at my existing EC2 instance and volumes. The instance looked good—all status checks passed—and I already had one 8 GiB root volume sitting there. AWS was also giving me a little nudge about setting up backups, which made sense.

### 2. Created a New Volume and Hooked It Up
I made a fresh **1 GiB gp2 volume** in **us-west-2a**—same AZ as my instance, which is super important (more on that later). Tagged it "My Volume" so I wouldn't lose track of it, then attached it to my instance as **/dev/sdb**.

**Here's why the AZ thing matters:** You literally can't attach a volume to an instance in a different availability zone. If your volume is in us-west-2a and your instance is in us-west-2b, AWS will just say nope. Learned that one the hard way.

### 3. Formatted the Volume
After I SSH'd into the instance using **EC2 Instance Connect**, I had to format the volume:

sudo mkfs -t ext3 /dev/sdb


This basically created an **ext3 filesystem** on the raw storage. Without doing this, the OS has no idea what to do with the volume—it's just unformatted space sitting there.

### 4. Created My First Snapshot
I went back to the AWS console and created a snapshot of **vol-097bc07ece2143c3e**. Tagged it "My Snapshot" to keep things organized. It finished at **100%** and came out to **53 MiB**. 

**Something cool I learned:** Snapshots are incremental, which means the first one is a full backup, but after that, AWS only saves what changed. Super efficient and saves you money on storage.

### 5. Tested the Recovery Process
This is where I actually proved that the backup thing works:

1. **Made a new volume** from my snapshot (**vol-0f35dc7ea140bd75c**)
2. **Attached it** to my instance as **/dev/sdc**
3. **Deleted a test file** from the original volume (simulating data loss)
4. **Mounted** the restored volume to /mnt/data-store2
5. **Checked if the file was there** on the restored volume

And boom—the file was there! So my whole backup-and-restore process actually worked. Pretty satisfying honestly.

---

## Challenges

- **Volume wouldn't attach at first:** I tried attaching a volume from **us-west-2b** to my instance in **us-west-2a**. AWS shut that down immediately. Turns out you really need to match those AZs.
- **Couldn't mount the volume:** I tried mounting the volume right after attaching it, but I forgot to first. The OS had no idea what filesystem to read, so it just failed.

---

## Solutions

- **Fixed the AZ issue:** I just recreated the volume in **us-west-2a** to match where my instance was running. Problem solved.
- **Formatted properly:** Ran sudo mkfs -t ext3 /dev/sdb before trying to mount it again. After that, everything worked perfectly.

---

## Key Things I Learned

**Always check your availability zones**—volumes and instances need to be in the same one  
**Tag literally everything**—makes life so much easier when you're looking for stuff later **Don't skip the mkfs step**—new volumes need to be formatted before you can use them  
**Actually test your b**Use IAM roles for security**—way better than hardcodi
**Snapshots save you money**—they're incremental, so you're not backing up the same data over and over  

---

## Best Practices I Followed

- **Named everything clearly:** Used tags like "My Volume" and "My Snapshot" instead of leaving them as random IDs
- **Made backups before changes:** Created snapshots before doing anything that could mess up my data
- **Validated the restore:** Actually went through the recovery process instead of just hoping it would work
- **Kept it secure:** Used EC2 Instance Connect instead of managing SSH keys manually
- **Documented everything:** Took screenshots at each step so I can remember how I did this later

---

## What's Next

- Set up **automated snapshots** using AWS Backup or Data Lifecycle Manager so I don't have to remember to do it manually
- Figure out **snapshot retention policies** to keep storage costs under control
- Try **copying snapshots to another region** for disaster recovery
- Look into **encrypted volumes** for when I'm working with sensitive data

---

## My Thoughts

This lab really made me appreciate how EC2 and EBS work together. Storage isn't just something you can ignore—you need to understand volume management, backups, and recovery if you're running anything important. The fact that I could restore data from a snapshot in just a few minutes is honestly pretty cool, and I'd definitely use this approach in real projects.

Also, getting practice with the Linux filesystem commands was super valuable. When AWS gives you raw block storage, you need to know how to format it, mount it, and verify everything's working at the OS level. It's not as simple as just clicking "attach" in the console—there's actual work to do on the instance side too.
