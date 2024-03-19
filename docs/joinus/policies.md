## 1. Introduction to the commons
<p>The JHPCE service center operates a High Performance Computing (HPC) facility as a Common Pool Resource (CPR) Hierarchy, with rights to specific resources based on stakeholder ownership of specific resources.  A formal computational model of the HPC resources provides a starting point for the self governance of the the commons, first by defining a hierarchy of resources that are governed locally by individual stakeholders, second, by distributing common and resource-specific expenses throughout the hierarchy, third by calculating fees in proportion to actual <em>measured</em> consumption of specific resources and fourth, by providing a data based method that incentivizes sharing of excess resource capacity.  CPR principles have provided a framework that has helped us address issues of overuse, congestion, and sustainability. The strategy has harnesses the intellectual and grant writing skills of the entire community, for the benefit of the entire community, while enabling the growth of an HPC facility that is up-to-date and very matched to the needs of its PIs. This contrasts with traditional governance models that rely on central planning, F&amp;A and a handful of rainmakers. Our experience is that the CPR model benefits not just the largest and best funded stakeholders, but also the much larger numbers of smaller stakeholders and non-stakeholders who make up the tail of the HPC consumption distribution – where the bulk of the science is produced.</p>
## 2. Computing Resources
### 2.1 Background
<p>All user are permitted to submit jobs to the shared queue. The shared queue provides low-priority access to unused capacity throughout the cluster. Capacity on the shared queue is provided on a strictly “as-available” basis and serves two purposes. First it provides <em>surge capacity</em> to stakeholders who temporarily need more compute capacity than they own, and second, it gives provides non-stakeholders access to computing capacity. Scheduling polices attempt to harvest unused capacity as efficiently as possible while mitigating the impact on the rights of stakeholders to use their resources. The service center does not guarantee that stakeholders will provide sufficient excess capacity to meet non-stakeholders needs, however in practice the cluster is rarely operating at full capacity so there is usually ample computing capacity on the shared queue. The JHPCE service center provides the following computing services and capabilities to all PIs and their users:</p>
### 2.2 Computing privileges and services afforded to all PIs
<ol>
<li>Low-priority access to cluster-wide excess computing capacity via the shared queue.</li>
<li>Detailed quarterly reports of monthly usage and charges, stratified user.</li>
<li>System-level support from the System Administrators</li>
<li>Access to a download queue that is used for file transfer to and from the facility</li>
<li>Access to a low-priority “interactive queue” for small tasks</li>
</ol>
2.3 Computing privileges and services afforded to compute-system stakeholders
<p>The JHPCE service center is organized to promote the research agendas of its stakeholders. We provide the following computing services and capabilities to stakeholders who purchase compute nodes:</p>
<ol>
<li>Professional system administration and support of stakeholder-owned compute nodes</li>
<li>Management of service contracts, warranties and interaction with vendors (e.g. repair tickets).</li>
<li>System-level support from the system administrators</li>
<li>Priority access to owned compute nodes via individual stakeholder queues.</li>
<li>A stable maintenance fee that is assessed on each compute node on a quarterly basis.</li>
<li>Discounts on maintenance fees in exchange for providing excess compute capacity to other users.</li>
<li>Access to cluster-wide excess computing capacity via the shared queue.</li>
<li>Detailed quarterly reports of monthly usage and charges, stratified by queue, host and user.</li>
<li>Freedom to remove owned nodes from the cluster at any time.</li>
</ol>
## 3. Storage Resources
### 3.1 Background
There are numerous storage spaces available for long term storage of data on the
JHPCE cluster.  There are spaces available for all users of the cluster, and also
provide that opportunity to buy into a large storage purchase Every 12 - 18 months.
### 3.2 Storage privileges and services afforded to all PIs
<p>All users have home directory space (currently a 100 GB quota). The charge for storage on home directories is proportional to actual usage. Backup of home directories is mandatory and is charged separately.</p>
### 3.3 Storage privileges and services afforded to storage system stakeholders
<p>The bulk of JHPCE storage is dedicated to specific research projects and is owned by stakeholders. Currently the service center has essentially no storage to hand out to users. To meet the continually growing storage requirements, we periodically build large, shared, low-cost, low-power storage arrays, and provide all PIs the opportunity to purchase space on these arrays. The JHPCE provides the following services/capabilities to stakeholders who have purchased a share on a storage device:</p>
<ol>
<li>A dedicated, fixed allocation on shared devices that stakeholders can manage as they see fit.</li>
<li>Professional system administration and support of stakeholder-owned storage</li>
<li>Management of service contracts, warranties and interaction with vendors (e.g. repair tickets).</li>
<li>Support from the system administrator, including configuration of top-level directories, quotas and root-level operations, e.g. on ZFS devices stakeholders can request compressed shares, thereby doubling their usable storage.</li>
<li>Stakeholders may share their space with collaborators, either for free or in exchange for sharing the maintenance charge.</li>
<li>Once a storage allocation is purchased, stakeholders maintain access to their space by paying a stable quarterly charge that covers operating expenses and amortization of capital costs.</li>
<li>Buy-in and operating charges are calculated in proportion to the total capital costs and the total operating expenses of the storage device.</li>
<li>Due to the fact that ownership of a storage device is shared, it is not possible for a stakeholder to remove their share of a device.</li>
<li>We expect storage devices to have a 5 year lifetime. Beyond that we may discontinue operation of the device. The industry standard is to include 3 year service and support contracts in the purchase cost of storage devices. Thus years 4 and 5 may incur higher operating charges due to the addition of year 4-5 service contracts.</li>
</ol>
## 4. Cost Recovery Principles
### 4.1 The JHPCE facility is a Common Pool Resource Hierarchy
<p>The facilities in the JHPCE are managed as a Common Pool Resource (CPR) hierarchy. The amount of cost that we recover from each resource in the hierarchy is determined by assigning expenses to individual resources in the CPR via a rigorous graph-theoretic time-dependent model of the entire CPR. Expenses are distributed throughout the hierarchy to determine the costs that must be assigned to specific resources, e.g. specific nodes or specific storage partitions.</p>
### 4.2 Compliance with OMB regulations
<p>OMB regulations require that service centers do not make a profit or loss. Year-end deficits or surpluses must be carried forward to the next fiscal year. OMB regulations do not require that rates be estimated and fixed at the beginning of each fiscal year. Instead the requirement is that either rates be fixed periodically OR a well defined systematic charge-back methodology be applied uniformly to all users. We employ the latter strategy. Expense sharing and rate calculations are performed with a software tool that creates a hierarchical model of the HPC facility and incorporates institutional subsidies, expenses and depreciation schedules to calculate the operating costs assigned to individual resources. The software tool then combines the operating costs with the usage data from system logs to calculate rates and chargebacks for individual resources and users. The JHPCE bills quarterly. To guarantee that the service center exactly recovers all it’s quarterly expenses, the JHPCE uses retrospectively calculated rates which are based on actual quarterly expenses and actual quarterly usage (see discussions below). This differs from the conventional approach of setting rates at the beginning of each fiscal year. To appreciate the advantages of this approach we first consider the disadvantages of the conventional approach.</p>
### 4.3 Difficulties associated with the conventional approach to setting rates
<p>With the conventional approach, budgets are typically reconciled once per year to: 1) account for the deficit/surplus that has accumulated over the previous year and 2) account for changes in the services and configuration of service center and 3) guess the projected resource usage. By construction, the resulting rates lag the realities of the service center, i.e. the actual expenses, resource usage and configuration. This lag leads to the buildup of deficits (and occasionally surpluses). The resulting uncertainty causes organizational inertia that resists change and innovation. PIs demand and deserve predictable costs. They ask for the current “rate schedule”. Unfortunately, setting fixed rates one year ahead actually leads to more variability than rates that are adjusted quarterly in response to “realities on the ground”. With the conventional approach, rates change (by a difficult to predict amount) at the beginning of each fiscal year. There is an annual jump in rates of unknown sign and magnitude. This is not a formula for a financially stable service center or for the predictability that PIs need.</p>
### 4.4 Allocation and smoothing of expenses and subsidies
<p>Expenses are spread over time. Capital expenses are depreciated across 3-5 year intervals as determined by their asset class. Non-capital expenses are spread across 12 months, even if the interval spans two fiscal years. Thus the impact of large expenses is spread across individual fiscal years. Expenses are allocated to the appropriate sub-hierarchy of the resource hierarchy according to the part of the resource hierarchy that incurs the expense. This is performed with the aid of a formal graphical model of the facility. For example, expenses related to storage would be assigned to the storage-related sub-hierarchy. Thus the storage expense would not directly impact compute-related rates. In general, this allows expenses to be allocated to the subset of users who actually need and use particular resources. As resources are added and removed from the JHPCE facility, in response to waxing and waning research initiative and grant funding of particular stakeholders and users, the costs associated with those changes are shared with the appropriate subset of the JHPCE community. Finally, subsidies are applied at the level of the hierarchy that is required by the donor. For example, institutional and departmental subsidies are typically for staff salaries, and are therefore applied at the staff salary level of the hierarchy.</p>
### 4.5 Retrospective rate calculations
<p>Rates in the JHPCE are calculated retrospectively at the end of every quarter and are based on that quarter’s actual configuration, actual expenses, actual institutional support, and actual usage. While the precise rates are not known until the end of the quarter, 7 years of experience has shown that rates have been stable from quarter-to-quarter with smooth and gradual changes. The only exception has been when major system upgrades have occurred. To PIs that are initially uncomfortable with retrospective rates, we point out that charges are the product of two terms: rates and usage. The latter is the dominant source of uncertainty in budget estimation. Our system allows PIs to use historical rates from recent quarters to estimate their computing budgets in grant applications. By smoothing and appropriate allocation of expenses, we are able to create highly stable rates and to substantially reduce quarterly fluctuations to only a few percent. The final advantage of the retrospective system is that since the software reconciles the budget on a quarterly basis, the service center knows on a quarterly basis, exactly where it stands financially. This is the recipe for a successful service center.</p>
### 4.5 Computing charges from rates and usage
<p>Charges to individual users and stakeholders are calculated strictly in proportion to their share of usage of particular resources. Users with dedicated (i.e. unshared) resources are charged based on 100% usage (i.e. independently of actual usage). Thus stakeholders who purchase a compute node (or a fixed allocation in a shared storage device) will see a fixed charge (subject to aforementioned quarterly fluctuations). In addition, stakeholders who share their resources (e.g. by sharing excess compute cycles) see reduced cost in proportion to the amount of usage that is shared with others.</p>
## 5 Current rate examples (Spring 2024)
<h3>Computing</h3>
<table border="1">
<tbody>
<tr>
<th>queue</th>
<th>rate</th>
</tr>
<tr>
<td>shared</td>
<td>$0.0015/GB-hour</td>
</tr>
<tr>
<td>sas</td>
<td>$0.0224/GB-hour</td>
</tr>
</tbody>
</table>
<h3>Storage</h3>
<table style="height: 200px;" border="1">
<tbody>
<tr>
<th>device</th>
<th>rate</th>
<th>rate</th>
</tr>
<tr>
<td>home</td>
<td>$0.0159/GB-month</td>
<td>$190.99<span style="font-weight: inherit;">/TB-year</span></td>
</tr>
<tr>
<td>dcl02 (Lustre allocation)</td>
<td>$0.0021/GB-month</td>
<td>$25.20/TB-year</td>
</tr>
<tr>
<td>dcs04 (ZFS allocation)</td>
<td>$0.0019/GB-month</td>
<td>$23.000/TB-year</td>
</tr>
<tr>
<td>dcs07 (ZFS allocation)</td>
<td>$0.0016/GB-month</td>
<td>$19.50/TB-year</td>
</tr>
</tbody>
</table>
<p>Allocations may be purchased on custom devices whenever we build them. Allocations are
paid off over a 5 year amortization schedule, where the cost includes the hardware
costs for the storage plus the quarterly JHPCE Managed Storage charge.</p>

### 5.1 Historical rate examples (Summer 2021)

<h3>Computing</h3>
<table border="1">
<tbody>
<tr>
<th>queue</th>
<th>rate</th>
</tr>
<tr>
<td>shared</td>
<td>$0.0024/GB-hour</td>
</tr>
<tr>
<td>sas</td>
<td>$0.0160/GB-hour</td>
</tr>
</tbody>
</table>
<h3>Storage</h3>
<table border="1">
<tbody>
<tr>
<th>device</th>
<th>rate</th>
<th>rate</th>
</tr>
<tr>
<td>home</td>
<td>$0.0243/GB-month</td>
<td>$291.99<span style="font-weight: inherit;">/TB-year</span></td>
</tr>
<tr>
<td>dcs01 (ZFS allocation)</td>
<td>$0.0031/GB-month</td>
<td>$37.90/TB-year</td>
</tr>
<tr>
<td>dcl01 (Lustre allocation)</td>
<td>$0.0025/GB-month</td>
<td>$29.60/TB-year</td>
</tr>
</tbody>
</table>

### 5.2 Historical rate example (Spring 2015)
<h3>Computing</h3>
<table border="1">
<tbody>
<tr>
<th>queue</th>
<th>rate</th>
</tr>
<tr>
<td>shared</td>
<td>$0.0017/GB-hour</td>
</tr>
<tr>
<td>sas</td>
<td>$0.0139/GB-hour</td>
</tr>
</tbody>
</table>
<h3>Storage</h3>
<table border="1">
<tbody>
<tr>
<th>device</th>
<th>rate</th>
<th>rate</th>
</tr>
<tr>
<td>home</td>
<td>$0.0631/GB-month</td>
<td>$757.65/TB-year</td>
</tr>
<tr>
<td>dcs01 (ZFS allocation)</td>
<td>$0.0055/GB-month</td>
<td>$66.38/TB-year</td>
</tr>
<tr>
<td>dcl01 (Lustre allocation)</td>
<td>$0.0049/GB-month</td>
<td>$59.39/TB-year</td>
</tr>
</tbody>
</table>
