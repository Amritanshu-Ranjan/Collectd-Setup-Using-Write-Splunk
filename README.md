Step 1 . Create a test metrics index.




![image](https://user-images.githubusercontent.com/61896697/115965558-7e96b880-a547-11eb-9a8d-f15cb85cbe1e.png)

 
The datatype should be metrics.
 

Step 2. Check the Ubuntu Version.



![image](https://user-images.githubusercontent.com/61896697/115965564-83f40300-a547-11eb-93e0-bcacdca893a1.png)

 

Step 3 . Check for Update : apt-get update 



![image](https://user-images.githubusercontent.com/61896697/115965570-86eef380-a547-11eb-9116-62988b700563.png)


Step 4 . Validate if collectd already installed : dpkg -L collectd




![image](https://user-images.githubusercontent.com/61896697/115965579-8e160180-a547-11eb-8aa6-1e66a4ef4bc5.png)

 

Step 5. Choose the collectd version according to OS. There are different versions of collectd, you can check what suits your OS requirements.
Help Link: https://docs.splunk.com/Documentation/InfraApp/2.2.3/Admin/collectdSourceCommands

Step 6. Choose the version of collectd which matches your OS from the above-mentioned Link.



![image](https://user-images.githubusercontent.com/61896697/115965582-92421f00-a547-11eb-9914-d5903a352d36.png)

 

Step 7. Install collectd: apt-get install collectd



![image](https://user-images.githubusercontent.com/61896697/115965587-966e3c80-a547-11eb-975b-932753a3d2dd.png)


 

Step 8. Validate collectd version, .conf file directory, plug-in-directory according to the path specified based on OS.


![image](https://user-images.githubusercontent.com/61896697/115965593-9b32f080-a547-11eb-8635-078a508b4261.png)


 
After installation collectd will be in running state – please stop it and do the changes 
systemctl stop collectd 


![image](https://user-images.githubusercontent.com/61896697/115965598-9f5f0e00-a547-11eb-954b-0d7a3c2d1235.png)

 


Plugin Directory Validation : 


 ![image](https://user-images.githubusercontent.com/61896697/115965599-a2f29500-a547-11eb-8c39-4ee3e01170e2.png)

Collectd.conf Validation:


![image](https://user-images.githubusercontent.com/61896697/115965606-a5ed8580-a547-11eb-87d8-cc343ee12aca.png)

 

Step 9. Install libcurl package.

Help Link: https://docs.splunk.com/Documentation/InfraApp/2.2.3/Admin/ManageAgents
Choose appropriate package according to OS. - We will be choosing lincurl4 as suggested by the document.


 ![image](https://user-images.githubusercontent.com/61896697/115965618-aa19a300-a547-11eb-9206-9b8a3ac2011d.png)
 
 
 ![image](https://user-images.githubusercontent.com/61896697/115965637-b00f8400-a547-11eb-8078-ec2b85ffce64.png)


 

Step 10. We need to copy the Plugins into collectd plugin dir.: /usr/lib/collectd
write_splunk.so 
processmon.so 


As it does not come pre-loaded with the collectd package.
a.	Choose the correct plugin, based on libcurl version and your OS.
Help Link: https://docs.splunk.com/Documentation/InfraApp/2.2.3/Admin/ManageAgents
Since, we have installed collectd version 5.9 and using ubuntu:


![image](https://user-images.githubusercontent.com/61896697/115965643-b7369200-a547-11eb-9e29-878f0262801c.png)

 

b.	Validate the package in collectd plugin dir:


![image](https://user-images.githubusercontent.com/61896697/115965645-ba318280-a547-11eb-8961-13077cab3d2d.png)

 



Step 11. Edit the collectd.conf located at:  /etc/collectd – Load the plugins by un-hashing them and call plugins with custom settings.
![image](https://user-images.githubusercontent.com/61896697/115965649-c289bd80-a547-11eb-99d2-66bbc5d6eb8f.png)

 
Validations for write_splunk.so plugin and processmon.so plugin loaded in collectd.conf
<Plugin processmon>
ReadIo true
whitelist "collectd"
whitelist "bash"
whitelist "splunkd"
</Plugin>
<Plugin write_splunk>
server "ubuntu_dev"
buffersize 9000
useudp true
udpport 1970
Dimension "entity_type:nix_host"
</Plugin>

Step 12. Create inputs.conf: 


![image](https://user-images.githubusercontent.com/61896697/115965656-c87f9e80-a547-11eb-901c-d42d76532ce4.png)

 
Step 13. Create props.conf and transforms.conf for em_metrics_udp, sourcetype – to find the metrics data point while ingestion – if not added the data will not be indexed.


![image](https://user-images.githubusercontent.com/61896697/115965661-ccabbc00-a547-11eb-9d0a-d37ff1251eb0.png)

 

Step 14. Restart the Splunk UF.  /opt/splunk/bin/splunk restart
Step 15. Restart collectd: systemctl restart collectd


![image](https://user-images.githubusercontent.com/61896697/115965666-d2090680-a547-11eb-986d-bfca5eaf9c3b.png)

 





Step16. Check the traffic on udp port 1970 


![image](https://user-images.githubusercontent.com/61896697/115965671-d6352400-a547-11eb-8572-671d9fe161db.png)


 

Observation: When copying the 5_7_5_8 , write_splunk.so and processmon.so


 ![image](https://user-images.githubusercontent.com/61896697/115965673-da614180-a547-11eb-87db-69a27fb8303e.png)

 ![image](https://user-images.githubusercontent.com/61896697/115965675-dcc39b80-a547-11eb-8263-7d408e080d40.png)





Copying the write_splunk.so from 5_9_5_10.

Just after copying the correct version the write_splunk.so starts working:

![image](https://user-images.githubusercontent.com/61896697/115965734-2ad89f00-a548-11eb-840b-15e16c4d1920.png)


 
 ![image](https://user-images.githubusercontent.com/61896697/115965736-2dd38f80-a548-11eb-902d-3d52eb085531.png)





