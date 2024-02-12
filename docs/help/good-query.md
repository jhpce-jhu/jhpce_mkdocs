---
tags:
   - done
---
# Helpful Hints For Asking Questions or Reporting Problems

This page describes how to write a good help request email.
      
## A Good Request
1. Write a descriptive summary.
    * Put in a short summary into the Subject.
    * Which cluster are you on? We support two -- JHPCE and C-SUB. We assume JHPCE but specify "C-SUB" if that is where you are working.
 
2. Expand on this in a first paragraph. Try to answer the following questions:
    * Please give us your user name on the cluster.
    * What are you trying to achieve?
    * When did the problem start?
    * Did it work before or is this the first time you are trying to do this?
    * Which steps did you attempt to achieve this?
3. Additional advice:
    * Put enough details in the details section.
    * Send us screenshot images of what you did -- often we can notice little details you might not have thought to mention.
    * Please give us the _exact_ commands you type into your console.
    * What are the symptoms/is the error message
    * Have you tried to google for the error message?
    * Never put your password into the ticket.
    * In the case that you handle person-related data of patients/study participants, never write any of this information into the ticket or subsequent email.

## Specific questions for common issues

### Problems Connecting to the Cluster

* From which machine/IP do you try to connect (ifconfig on Linux/Mac, ipconfig on Windows)?
* Did it work before?
* What is your cluster user name? This is NOT your JHED ID.
* Please send us the output of `ssh-add -l` and add `-vvv` to the SSH command that fails for you.
* What is the response of the server?

### Problems Submitting Jobs

* Please give us the directory that you ran things in.
* Please send us the submission script that you have problems with.
* If the job was submitted, Slurm will give you a job ID. We will need this ID.
* Please send us the output of scontrol show job <jobid> or sacct --long -j <jobid> of your job.
