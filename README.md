# Two Tier Architecture  

![two-tier-arch-image-fixed](https://user-images.githubusercontent.com/88166874/131900069-8efd6227-b859-4ea0-bae7-fca9f185a32d.png)  

> This guide uses the EC2 Instance setup instructions from [intro_to_cloud_computing_AWS](https://github.com/am93596/intro_to_cloud_computing_AWS)
> and the app folder from [SRE_Intro_To_Cloud_Computing](https://github.com/am93596/SRE_Intro_To_Cloud_Computing).

## Making AMIs from EC2 Instances
### Create the db AMI
- Select the db instance
- Click Actions -> Image... -> Create Image
- Set the name and description to `SRE_Amy_db_ami`
- Click Add tag
- Set the name to `Name` and the value to `SRE_Amy_db_ami`
- Click Create Image
- Click the link in the green bar at the top to see your AMI
- Give it time to load
### Switch from original instance to AMI
- On the Instances page, stop your db instance
- On the AMIs page, select your AMI and click Launch
- Set up the new instance in the same way as the previous db instance - in the Tags page, set the name to `SRE_Amy_ami_db`
### Connect to the new instance
- Select the instance and click connect
- Copy the code in the example section of step 4 of the SSH connection page  
You are now in the db instance that you created from your AMI!  
### Create the app AMI
- Exit and repeat the above instructions for the app instance
### Connect the db AMI Instance to the app AMI Instance
- In the Security Group of the db AMI EC2 instance, change the TCP for port 27017 source IP to the IP for the app AMI instance, then save
### Correct the DB_HOST IP address
- SSH into the app AMI EC2 instance, and reset the DB_HOST -> change the IP section to the new IP for the db AMI instance IP
- Then `cd app` and `npm start`
