# Two Tier Architecture  

![two-tier-arch-image-fixed](https://user-images.githubusercontent.com/88166874/131900069-8efd6227-b859-4ea0-bae7-fca9f185a32d.png)  

## AWS Introduction  
**UNFINISHED**  
Amazon Web Services (AWS)

## Making an EC2 Instance
- Follow the instructions here -> https://github.com/am93596/intro_to_cloud_computing_AWS

## Security Groups  
**UNFINISHED**  

## Making an AMI of an EC2 Instance
- Select the db instance
- Click Actions -> Image... -> Create Image
- Set the name and description to `SRE_Amy_db_ami`
- Click Add tag
- Set the name to `Name` and the value to `SRE_Amy_db_ami`
- Click Create Image
- Click the link in the green bar at the top to see your AMI
- Give it time to load
- Stop your db instance
- Select your AMI and click Launch
- Set up the new instance in the same way as the previous db instance - in the Tags page, set the name to `SRE_Amy_ami_db`
- Select the instance and click connect
- Copy the code in the example section of step 4 of the SSH connection page  

**You are now in the db instance that you created from your AMI!**  

- Exit and follow the above instructions for the app instance
- In the Security Group of the db ami EC2 instance, change the TCP for port 27017 source IP to the IP for the app ami instance, then save
- SSH into the app ami EC2 instance, and reset the DB_HOST -> change the ip section to the new ip for the db ami instance ip
- Then `cd app` and `npm start`
