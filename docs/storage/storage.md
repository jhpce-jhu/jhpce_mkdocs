Test 2
# Current Storage Offerings

Test1

There are 2 main categories of storage space available for purchase on the JHPCE cluster, detailed below:

1. **Pay-as-you-go space**: This includes home directory space, legacy storage space, and leased spaces. Users are charged only for the actual space used and the time data is stored there. For example, using 10 TB of `/dcl01/leased` space for a year costs $500/year.

2. **Project spaces**: These are large storage arrays built approximately every 18 months, funded by various labs purchasing allocations on the storage array. For example, the buy-in cost for `/dcl02` was $43/TB, with a $300/year storage management fee for a 10TB allocation.

Pay-as-you-go spaces are generally more expensive than project spaces due to their smaller size, upfront costs being included in the annual fee, and higher maintenance requirements.

Additionally all users have access to 1TB of "fastscratch" storage for storing files less than 30 days.  More information on using fastscratch can be found at [/jhpce_mkdocs/knowledge_base/#fastscratch]
- **Personal Scratch space**: A network-based filesystem with a backend NVME based storage array, intended for short-term storage of large files.

Off-site backup space is also available, with `/users` directory currently backed up, and other directories backed up upon request.

For inquiries about purchasing storage, please email [bitsupport@lists.jhu.edu](mailto:bitsupport@lists.jhu.edu).

## Current Offerings

| Type | Location | Env Var | Capacity | Quota | Lifetime | FY21Q3 Rate | Charge Basis | Notes |
| ---- | -------- | ------- | -------- | ----- | -------- | ----------- | ------------ | ----- |
| Home | ZFS /users/<userid> | $HOME | 34TB | 100GB | Long | $345/TByr | Used TB | - |
| Temp scratch | /scratch/temp/<JQT> | $TMPDIR | 500GB-4TB | None | Transient | Free | - | [1][2] |
| Personal scratch | /fastscratch/myscratch/<userid> | $MYSCRATCH | 22TB | 1TB | 30 days | Free | - | [1][3] |
| Leased | Lustre /dcl02/leased | - | 200TB | As agreed | Intermediate | $50/TByr | Used TB | - |
| Project | ZFS /dcs04, /dcs05, /dcs07 | - | 5,000TB | As purchased | Long | $20/TByr + buyin | Purchased TB | [4] |
| Backup | ZFS varies | - | 2,675TB | - | Permanent | $11/TByr | Purchased TB | [5] |

**Notes:**
- [All] Rates fluctuate slightly from quarter to quarter based on actual JHPCE expenses and capacities.
- [1] Scratch space is only visible on compute and transfer nodes. Scratch is not visible on the login node. `<JQT>` = 'job.queue.task'.
- [2] Scratch space varies from node to node based on local disk space.
- [3] Currently there is a 1TB quota on files in /fastscratch/myscratch, with a 30-day file retention limit.
- [4] The buy-in cost for DCS04 is $42/TB. Limited capacity still available for sale.
- [5] Backups are done of the /users filesystem. Other filesystems may be backed up upon arrangement with PI.

## Previous/Legacy Offerings

The storage spaces listed below are currently in use but are no longer available for purchase.

| Type | Location | Env Var | Capacity | Quota | Lifetime | FY2019Q2 Rate | Charge Basis | Notes |
| ---- | -------- | ------- | -------- | ----- | -------- | ------------- | ------------ | ----- |
| Leased | Lustre /dcl01/leased | - | 200TB | As agreed | Intermediate | $50/TByr | Used TB | - |
| Leased | ZFS /legacy | - | 100TB | From legacy | Short | < $1350/TByr | Used TB | [1] |
| Leased | ZFS /starter/starter-02 | - | 10TB | 10TB | Short | $1,041/TByr | Used TB | [1] |
| Project | Lustre /dcl01 | - | 3,400TB | As purchased | Long | $26-$

There are 2 main categories of storage space available for purchase on the JHPCE cluster, detailed below:

1. **Pay-as-you-go space**: This includes home directory space, legacy storage space, and leased spaces. Users are charged only for the actual space used and the time data is stored there. For example, using 10 TB of `/dcl01/leased` space for a year costs $500/year.

2. **Project spaces**: These are large storage arrays built approximately every 18 months, funded by various labs purchasing allocations on the storage array. For example, the buy-in cost for `/dcl02` was $43/TB, with a $300/year storage management fee for a 10TB allocation.

Pay-as-you-go spaces are generally more expensive than project spaces due to their smaller size, upfront costs being included in the annual fee, and higher maintenance requirements.

Additionally all users have access to 1TB of "fastscratch" storage for storing files less than 30 days.  More information on using fastscratch can be found at [/jhpce_mkdocs/knowledge_base/#fastscratch]
- **Personal Scratch space**: A network-based filesystem with a backend NVME based storage array, intended for short-term storage of large files.
- 
| Method      | Description                          |
| ----------- | ------------------------------------ |
| `GET`       | :material-check:     Fetch resource  |
| `PUT`       | :material-check-all: Update resource |
| `DELETE`    | :material-close:     Delete resource |
