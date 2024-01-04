<p align="center">
<img src="https://github.com/Kasen-Elliot/network-fileshare-/assets/127895952/059b79e9-1899-4937-9a8b-2749f153a277" height = 20% width = 20%/>
</p>

<h1 align = "center">Network Fileshare and Permissions in Active Directory</h1>
File sharing and permission set up is an essential in a business structure in order to organize resources and make sure users have the appropriate permissions and access to files they need. This lab demonstrates how file sharing and permissions work in the Active Directory domain. This lab assumes you have fully installed and configured Active Directory on a Domain Controller in a virtual machine as well as set connection with a Client virtual machine.

<br />

<h2>Environments and Technologies Used</h2>
<ul>
  <li>Microsoft Azure (Virtual Machines/Compute)</li>
  <li>Remote Desktop</li>
  <li>Active Directory Domain Services</li>
</ul>

<br />

<h2>Operating Systems Used</h2>
<ul>
  <li>Windows Server 2022</li>
  <li>Windows 10 Pro (21H2)</li>
</ul>

<br />

<h2>Steps</h2>

<h3>Creating Sample Fileshares with Various Permissions</h3>

<p>
  <ul>
    <li>Have your Domain Controller VM connect and log in as an admin (<b>mydomain.com\jane_admin</b>) and have your Client VM connect and log in as a random user generated through the powershell script during configuration</li>
    <li>In the Domain Controller VM, create the four folders below in the C:\ Drive and set the <b>Permissions</b> in these folders (by opening the folder's <b>Properties</b> and click on <b>Share</b> under the Sharing tab)</li>
    <ul>
      <li><b>read_access</b> - add the group Domain Users and set Permissions to Read</li>
      <li><b>write_access</b> - add the group Domain Users and set Permissions to Read/Write</li>
      <li><b>no_access</b> - add the group Domain Admins and set Permissions to Read/Write</li>
      <li><b>accounting</b> - skipped for now</li>
    </ul>
    <li>Example of setting group and permissions for read_access</li>
    <ul>
      <li><img src ="https://github.com/Kasen-Elliot/network-fileshare-/assets/127895952/76d027ea-4d5b-47d4-814c-c12fb3963265" width = 80% height = 80%/></li>
    </ul>
  </ul>
</p>

<br/>

<h3>Attempting to Access Fileshares</h3>

<p>
  <ul>
    <li>Go to the Client VM and navigate to the shared folder through <b>File Explorer</b> and typing <b>\\dc-1</b></li>
    <ul>
      <li><img src ="https://github.com/Kasen-Elliot/network-fileshare-/assets/127895952/c8835af4-8374-4525-94d2-3a42a2e76b56" width = 80% height = 80%/></li>
    </ul>
    <li>Attempt to access the folders through the Client VM; the only one that should be inaccessible by how the permissions are set should be <b>no_access</b></li>
    <ul>
      <li><img src ="https://github.com/Kasen-Elliot/network-fileshare-/assets/127895952/285768ac-29b4-43fc-b99d-111c9eea954f" width = 80% height = 80%/></li>
    </ul>
  </ul>
</p>

<br/>

<h3>Creating the "ACCOUNTANTS" Security Group</h3>

<p>
  <ul>
    <li>Head back to the Domain Controller VM, go to the Server Manager Board and go to <b>Active Directory Users and Computers</b> and create a new <b>Organizational Unit (OU)</b> and name it "_SECURITY_GROUP."</li>
    <ul>
      <li><img src = "https://github.com/Kasen-Elliot/network-fileshare-/assets/127895952/854b72ee-71ab-4c10-9308-19f0f8d16b47" width = 80% height = 80%/></li>
    </ul>
    <li>Inside the OU, create a <b>Group</b> and name it "ACCOUNTANTS"</li>
    <ul>
      <li><img src = "https://github.com/Kasen-Elliot/network-fileshare-/assets/127895952/aaec89f1-3aaf-440b-aacd-95426d1efd74" width = 80% height = 80%/></li>
    </ul>
    <li>Locate the <b>accounting</b> folder created in C:\ Drive and add the ACCOUNTANTS group and set its permissions to Read/Write</li>
    <ul>
      <li><img src = "https://github.com/Kasen-Elliot/network-fileshare-/assets/127895952/344af5ac-825f-45c5-a54d-c5bc905af31c" width = 80% height = 80%/></li>
    </ul>
    <li>The user logged in the Client VM should not have access to the accounting folder since it's not part of the Group. Log off the Client VM and remember the username you used to log in to the Client as it's going to be set as part of the ACCOUNTANTS group</li>
    <li>In the Domain Controller VM, go to the _SECURITY_GROUP OU,  right click on the ACCOUNTANTS to open up <b>Properites</b>, go to the <b>Members</b> tab and add the user as a member of the Group</li>
    <ul>
      <li>In this example, the user "ban.doh" is used</li>
      <li><img src = "https://github.com/Kasen-Elliot/network-fileshare-/assets/127895952/556167f4-1c8a-4de2-8552-27e3d1916352" width = 80% height = 80%/></li>
    </ul>
    <li>Sign back into the Client VM with user you made part of the ACCOUNTANTS group and it should now have access to the accounting folder</li>
  </ul>
</p>

<br/>
