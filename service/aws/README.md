# AWS

## Getting Started
1. Ensure you create a user with the following cloud formation with the policy in: adop-ansible-plat-extn.policy
2. The user doesn't not need a username and password
3. The user does need an API access key and secret access key. Create / locate these.
4. In the Jenkins instance within your ADOP stack navigate to the Platform Management folder
5. Run the Load_Platform_Extension job passing in this repo's git URL and the AWS keys from above.
6. The job should run through cleanly.

Steps for connecting to the machines:
* When connecting via ADOP skip these steps
	* Go into your AWS Console and view Cloud Formation stack list
	* Under the Resources tab of you EC2-PLATFORM-EXTENSION stack click the Physical ID of the Control Stack
	* Click on the securit group then the inbound tab
	* Click edit and change the source to 0.0.0.0/0 and save
	* Repeat these steps for your Target Stack
		* This will allow you to connect to the Machine without going through your ADOP instance
* SSH onto Control Machine using
	* `ssh -i <pem file used to create ADOP intstance> centos@<AnsibleControlMachine Public IP>`
* Once onto the machine change directory to /home/centos
	* `cd /home/centos`
* Create private key file yourpemfile (call it the same name as the one used to create ADOP instance)
	* `vi yourpemfile`
	* Copy contents of own yourpemfile.pem file into this new file
* Change permisions of yourpemfile to 600 (owner can read and write)
	* `chmod 600 yourpemfile`
* Create ansible inventory file
	* `vi inventory`
* Change contents of inventory to:
	* `target ansible_host=<AnsibleTargetMachinePublic IP> ansible_user=centos ansible_ssh_private_key_file=/home/centos/yourpemfile`
* Ping target machine using:
	* `ansible -i /home/centos/inventory -m ping target`

### Troubleshooting
If the platform extension loading job fails with:
```
echo 'ERROR : Stack creation failed after 60 seconds. Please check the AWS console for more information.'
```
Check the Cloud Formation service in the AWS web console.  

If you see the message:
```
In order to use this AWS Marketplace product you need to accept terms and subscribe.
```
You need to follow the link provided and create a nano machine (which you should promptly delete).  
