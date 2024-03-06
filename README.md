<h1>DNS Transfers Between Primary and Secondary Zones on Windows Server 2022</h1>

![DNS Transfers Between Primary and Secondary Zones](https://github.com/jonathansantacruz3/DNS-Transfers-Between-Primary-and-Secondary-Zones-on-Windows-Server-2022/assets/151465848/ad53ee4c-3f43-4e3c-99de-3b6052a59f5a)


In this project, I am going to transfer the records of one DNS server on another network to my primary DNS server. This will eliminate the need for a client computer to navigate to another server, on another subnet, and grant access to those same resources within its network utilizing the primary DNS server. In enterprise environments, this isolation from external subnets creates a protective layer of security and greater control of what resources are available to employees. I added another virtual machine with another instance of Windows Server 2022 to serve as the secondary DNS named DNS-THOR. I used a private virtual switch in my infrastructure within Hyper-V to be able to communicate between these servers and I configured zone transfers on DNS-THOR to make this process possible. 

<h2>Environments and Technologies Used</h2>

- Microsoft Hyper-V (Virtual Machines/Compute)

<h2>Operating Systems Used </h2>

- Windows 11 Home Edition</b> (23H2)[HOST]
- Windows Server 2022 (21H2) (two different machines)

<h2>List of Prerequisites</h2>

- Hyper-V Enabled 
- Windows Server 2022 ISO file

<h2>Configuration Steps </h2>
<h3>Set Up Second DNS Server</h3>
The first thing I did was create a new VM with Windows Server 2022 installed on it. I configured primary zones, including forward and reverse lookups, for the server DNS-THOR. I followed the steps I took in my last project, "How to Deploy a Standalone DNS on Windows Server 2022 (https://github.com/jonathansantacruz3/How-to-Deploy-a-Standalone-DNS-on-Windows-Server-2022)" . I verified that the records were properly configured by opening the command line and doing an “nslookup DNS-THOR 192.168.0.2” command. This returned the desired results by displaying the correct name of the server and its associated IP address. 

<h3>Set Up Zone Transfer Permissions</h3>
This part of the configuration required setting up the permissions to transfer records from the server DNS-THOR over to the primary DNS server named DNS-IRONMAN. In the DNS panel of DNS-THOR’s Server Manager console, right-click its forward lookup zone and choose “Properties” from the list of options. Click on the “Zone Transfers” tab and select the “Only to the following servers” option. Click on “Edit”, type in the primary server’s IP address, and press “OK”, “Apply, and “OK”. 
 
   

<h3>Configuring  Forward Secondary Zone in Primary Server</h3>

To complete the process, navigate to the primary server’s DNS panel under the Server Manager console and create a “Secondary Zone” by right-clicking on the “Forward Lookup Zone” folder.
 
In the Wizard, click on “Next”, then select “Secondary Zone” and click on “Next”.
  
In this window, I typed in the name, asgard.com, which is the domain where the secondary DNS lives, and then input the address of the secondary DNS server. 
    
In the next window, I pressed, “Finish” and completed the permissions in the secondary server.
 

<h3>Configuring Secondary Zone Reverse Lookup</h3>
In DNS-THOR, I navigated to the DNS panel and clicked on the “Reverse Lookup Zones” folder to display its records. I right clicked on the first record that holds the IPv4 reverse lookup record and chose “Properties”. I went to the “Zone Transfers” tab and marked the “Allow zone transfers” to set up the permission. I marked the 3rd option to configure the permission to only DNS-IRONMAN. I pressed “OK”, “Apply”, then “OK”. I switched over to DNS-IRONMAN and opened the DNS panel. 
 
  
In this step, I clicked on the “Reverse Lookup Zones” and created a “PTR record” to populate a reverse lookup for DNS-THOR. I entered its address and hostname and created a PTR record. 
 
   

I was able to create this copy of records (reverse and forward lookups) from the second DNS server into my master server.
 
 
<h3>Test the Zone Transfers</h3>
I wanted to test the records update on DNS-IRONMAN from DNS-THOR. To remedy this, I created a test “A” record as a primary forward lookup zone on DNS-THOR. I named the record “TEST” and gave it a random IP host address maintaining its network ID. Once created, I verified it and traversed over to DNS-IRONMAN. 
 
    
This is the record I created on DNS-THOR.
 
I had to wait a few minutes for it to appear in the secondary forward lookup zone for DNS-IRONMAN. 
 
<h3>Conclusion</h3>
DNS zone transfers are especially useful if the organization wants to utilize a primary DNS for its network’s queries. This decreases any potential breaches and cancels the need for external communication from any client computers. The zone transfers will automatically update any records in the primary DNS server that are added or deleted in the secondary DNS server. It is a convenient DNS automation tool for system administrators to incorporate within their systems. 


