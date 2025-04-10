Dear JHPCE Community,
 
There is going to be scheduled power work done at the MARCC datacenter which will affect the MARCC colocation facility, which is where the JHPCE cluster is located. This work is scheduled for the week of July 15th.  In order to accommodate this work, we will need to take the JHPCE cluster down, as all power to the facility will be offline for the duration of the work. 
 
We are planning to take the JHPCE cluster down starting on **Friday, July 12th at 5:00 PM**, and expect it to be back in service by **Monday, July 22nd at 5:00 PM**. As part of this work, all running and pending jobs will be cancelled on the cluster on July 12th at 5:00 PM.  A reservation will be put in place to prevent jobs from running into the maintence window.  When submitting jobs, please be sure to specify a time limit for your jobs so that they end before the maintenance window. Scheduled jobs that may run into the maintenance window will have a “Reserved for maintenance” reason in the squeue output.
 
We are going to make use of this unexpected downtime to perform a number of upgrades that have been in the works. Primarily we will be installing a new SSD-based storage array for housing home directories and core JHPCE software modules, and we will be applying a number of patches and updates on the compute nodes.
 
Thank you for your understanding during this outage, and we will do whatever we can to minimize this downtime.
