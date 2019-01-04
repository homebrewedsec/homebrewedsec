---
layout: post
comments: true
title:  "Active Directory Security Tips"
background: '/img/posts/ADsecurity.jpg'
---

<p>I've been noticing something lately. Organizations will go to lengths to harden a server or application, but the <i>permimeter</i> around that object hasn't been considered. While this post talks about Active Directory security, the same principles can be applied to most applications. The focus is essentially, don't just think about what this server does that can be compromised, but consider the infrastructure around it that has a potential path for exploitation. Some of this may be pretty basic, but this should serve as a reminder to <i>not miss the forest for the trees</i>. Or, as a colleague put it, don't leave the door open at Fort Knox when you go out for a smoke break.</p>

<p>Also, you'll notice the <i>risk</i> and <i>remediation</i> verbiage. I do primarily GRC (government regulatory compliance), so this is the jargon I use. Feel free to mentally substitute what words make sense to you if your specialization of InfoSec defines it differently.</p>

<h1>Operating System Security<h1>
<h2>Risk</h2>
<p>One of the features of Windows Server Operating Systems prior to version 2012 is the ability to reset the local administrator password with a Windows installation CD. This means that anyone with physical (or virtual) access to the machine can become a local administrator. While Domain Controllers do not have a <i>local admin password</i>, they have a _<i>Domain Services Restore Password</i>, which functions the same way. Once logged in with this account, someone could add themselves to the Domain Admins Group and establish persistence. In fact, I had to do this in a few years back on the legacy domain for a client in Malaysia to resolve an Exchange Server issue, which was on a Domain Controller.</p>

<h2>Remediation</h2>
<p>Upgrade all Domain Controllers and Servers to Server 2012 or higher, ideally to Server 2016. Alert whenever sensitive groups are modified. For example:</p>
  <ul>
    <li>Schema Admins
    <li>Enterprise Admins
    <li>Domain Admins
    <li>Administrators
    <li>Organization Management
  </ul>

<h1>Exchange Administration Security</h1>
<h2>Risk</h2>
Local Administrators of Exchange Servers are able to obtain plaintext passwords from password hashes.
<h2>Remediation</h2>
<p>Do not grant Exchange local administrator privileges to administrators that are not members of <b>“Organization Management”</b>.</p>

<h1>Hypervisor Security</h1>
<h2>Risk</h2>
<p>Physical security threats are somewhat replaced by virtual security threats in virtualized environments. Regardless of how vCenter or vSphere privileges are delegated, anyone with the hypervisor root password can modify and access any virtual machines on that host, including Domain Controllers. This means they could copy the vmdk, or attach it to another Virtual Machine. Once the Virtual Machine is accessible, they can pull 'ntds.dit' from the Active Directory database, the encryption key, and then crack users' passwords without maintaining persistence on the Domain Controller.</p>
<h2>Remediation</h2>
<ul>
  <li>Restrict knowledge of and access to the root account on ESX hosts and vCenter servers
  <li>Implemented VMDK (VM disk file) encryption from the hypervisor
  <li>Place Domain Controllers on separate ESX hosts
</ul>
  
<h1>Administrator Security</h1>
<h2>Risk</h2>
<p>Domain Admin privileges should only be used to “administer administrators”, and as such should be used very infrequently.</p>

<h2>Remediation</h2>
<p>Anyone with domain admin privileges should have 3 accounts: one <i>non-admin</i>, one <i>admin</i>, and one <i>domain admin</i>. All logons with domain admin credentials should generate an alert. Additionally, delegation should be disabled for all domain admins. Any service accounts with domain admin privileges <b>DO NOT</b> need this level of permissions, and effort should be made to decrease the privileges granted.</p>

<h1>High-Risk Branches</h1>
<h2>Risk</h2>
<p>If there are any branches where physical (or network) security cannot be guaranteed for any reason, certain additional safeguards should be put in place. Hypothetically, these are not needed if the previous recommendations are implemented, but combining all of these increases security exponentially. Network security is <b>slightly</b> less critical due to the fact that Active Directory replication traffic is encrypted, but should still be considered.</p>

<h2>Remediation</h2>
<p>Implement Read-Only Domain Controllers (RODCs) in these high-risk branches. This way passwords are only sent and stored as needed locally. For example, <i>Alice’s*</i> password would only replicate if <i>Alice</i> is in the local office and attempts to authenticate.</p>

<h1>Sources</h1>
<a href="https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4964)">https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4964)</a>
