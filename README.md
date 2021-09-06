# Two Tier Architecture with Amazon Web Services (AWS)  

![aws-image](https://user-images.githubusercontent.com/88166874/131985801-faadee4a-50fb-44fb-b26a-b4d6c1cbf11b.gif)

AWS is a widely used cloud computing service provider from Amazon. They offer a wide variety of services, including Simple Storage Service (S3), Virtual Private Cloud (VPC), and Elastic Compute Cloud (EC2). All AWS services are available on a pay-as-you-go basis, making them more economical for large apps and organisations than using on-premises servers and data centres.   

![two-tier-arch-image-fixed](https://user-images.githubusercontent.com/88166874/131900069-8efd6227-b859-4ea0-bae7-fca9f185a32d.png)  

> This guide uses the EC2 Instance setup instructions from [intro_to_cloud_computing_AWS](https://github.com/am93596/intro_to_cloud_computing_AWS)
> and the app folder from [SRE_Intro_To_Cloud_Computing](https://github.com/am93596/SRE_Intro_To_Cloud_Computing).  

![db-and-app-instance-diagram](https://user-images.githubusercontent.com/88166874/131906372-30678efb-c40f-4e58-badd-9d91051e1ac1.png)

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

![AMI-of-db-instance-img](https://user-images.githubusercontent.com/88166874/131903872-8a0e036e-b228-4fce-964c-e9417996f264.png)

### Switch from original instance to AMI
- On the Instances page, stop your db instance
- On the AMIs page, select your AMI and click Launch
- Set up the new instance in the same way as the previous db instance - in the Tags page, set the name to `SRE_Amy_ami_db`  

### Connect to the new instance
- Select the instance and click connect
- Copy the code in the example section of step 4 of the SSH connection page; switch out the word `root` for `ubuntu` and make sure the key path is correct  
> You are now in the db instance that you created from your AMI!
> You can make multiple instances from one AMI by following the instructions from the `Create the db AMI` section above, as many times as needed!

![path-from-ec2-instance-to-ec2-copies](https://user-images.githubusercontent.com/88166874/131904372-50c7e832-f8a0-489e-84b9-8cc137755310.png)

### Create the app AMI
- Exit and repeat the above instructions (from `Create the db AMI` to `Connect to the new instance`) for the app instance
### Connect the db AMI Instance to the app AMI Instance
- In the Security Group of the db AMI EC2 instance, change the TCP for port 27017 source IP to the IP for the app AMI instance, then save
### Final command in the db AMI EC2 instance
- SSH into the db AMI EC2 instance
- `sudo nano /etc/mongod.conf`
- Change the bindip to 0.0.0.0
- CTRL-X, then `y` (saves and closes)
- `sudo systemctl restart mongod`
- `exit`
### Correct the DB_HOST IP address
- SSH into the app AMI EC2 instance, and reset the DB_HOST -> change the IP section to the new IP for the db AMI instance IP
### Final commands in app AMI EC2 instance
- `sudo nano /etc/nginx/sites-available/default`
- Replace the contents of the first `location` section with:
```
proxy_pass http://localhost:3000;      
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection 'upgrade'; 
proxy_set_header Host $host;
proxy_cache_bypass $http_upgrade;
```
- CTRL-X, then `y` (saves and closes)
- `sudo nginx -t` -> should pass
- Then `cd app` and `npm start`
