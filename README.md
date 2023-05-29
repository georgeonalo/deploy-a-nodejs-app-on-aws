# Deploy a nodejs app on aws

In this project, i deployed a Nodejs and Mysql soccer app on aws using a three(3) tier vpc architecture.

The Nodejs and Mysql app is an application that lets you add players to a database and also display their details from the database. in addition, it allows one to delete and edit player details.

## Overview of the Three Tier VPC architecture diagram 


![Untitled Diagram (10)](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/e4412e64-9936-4fc6-b88b-9f49e901a47e)






🛠️ In the course of completing this project, i created/used the following resources below:


✅ Vscode to edit codes in the Nodejs webfiles.

✅ 3 Tier VPC with public and private subnets in two availability zones.

✅ Internet Gateway to allow communication between resources in vpc and the internet.

✅ Nat Gateway to allow resources in the private subnets access to the Internet.

✅ MYSQL RDS for relational database.

✅ MYSQL WORKBENCH to connect to RDS database

✅ Application Load Balancer to distribute web traffic to the web servers in the private subnets.

✅ Auto Scaling Group to dynamically create and scale the web servers in the private subnets.

✅ Route 53 to create a record set and point it to the load balancer dns.

✅ TLS certificate to encrypt/secure all communication/data in transit.

✅ Security groups to control traffic to resources.

✅ Setup server in the public subnet from which an AMI was created.

✅ S3 and Session manger(SSM) Role to grant setup server(ec2) permission access to S3 bucket.

✅ S3 bucket to store Nodejs web files.

✅ AMI to create web servers and auto scaling group.


### Steps taken to complete this project.

#### Step 1: Create a  custom vpc with nategateway, subnets, route table and internet gateway

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/14157be5-cd5b-4ece-9ab6-3d600873b151)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/66b8b1e0-a51d-4a87-8352-72659dee7ac0)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/13c9fd98-ab65-4919-8da5-cb60b5cee0ad)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/3f3bfdad-c46a-471c-81b9-90c3a781ae8f)



#### 2: Go to the two public subnets and enable auto assign public ip


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/8fb85b5d-e70f-41f2-9ec5-6bb947ab30de)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/b652e82d-55d3-45aa-994c-5516c0106a9d)


#### Step 3: Create 3 Security groups: application loadbalancer sg, webserver sg and database sg.


#### ALB SG

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/b03fc442-455e-4617-879c-ded5e4b1fc65)

#### WEBSERVER SG

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/62f7dda2-d9e9-40de-8831-d6b1de80c740)


#### DATABASE SG

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/5ac95237-58d9-4ee7-a67b-135bb2ca9b27)


#### Step 4: Create the rds mysql database


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/eb22c042-a752-42f0-a8a1-fa20688dd3a9)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/63a58b11-530c-4050-9096-1b1dbceaef7e)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/609c2eeb-3f68-4bb7-bca4-405ea134d412)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/54d0da56-dfc7-40c2-8417-0e351c17b540)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/4199e24a-778d-4a25-822f-13437cdc1818)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/a15f7945-aa59-4457-a3cd-1db829891cd2)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/76e34ec6-7f3a-409c-b9d1-f02a0b3fa061)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/10c18640-9582-4582-a06d-21b68f02031c)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/3e95ef0a-6262-4f1c-a481-06cd1bf80da2)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/992bc755-7021-477d-a252-76ac90f6d7e8)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/a722c89a-e902-45ed-8b26-09b63605b332)



#### Step 5: Create the setup server

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/7de6c5ae-888b-43b9-9b75-52d249ebc790)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/57f07775-ba2c-4b4d-a166-fd17d73f42f3)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/95770589-9fcd-461e-83ed-b56c0c490781)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/c14c8a8d-e1c5-4b28-82d5-465a19415daa)

#### Step 6: Create a S3 and Session manager(ssm) role and attatched it to the server and click launch instance

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/17e37522-0053-4da5-8469-5659d514d66a)




#### Step 7: Create and S3 bucket, update update the app.js file of the Nodejs app with the rds endpoint, then upload all the Nodejs app webfiles in the S3 bucket

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/fe8fa59e-ca61-4531-ac3a-6f20a956d172)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/dc1086c3-ab84-4331-afe6-a7c314eba338)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/815532c0-5b72-4da7-963b-b7f37e5df002)



#### Step 8:

Before installing the nodejs app on our server, Let's first add the needed database and tables to our already created RDS-MySqL database... Open MySQL Workbench, create a new connection with the credentials you used when creating your MySQL RDS in the AWS Console.
Now copy the code below, paste and run it.


```
CREATE DATABASE socka;
show databases;
use socka;
show tables;
CREATE TABLE IF NOT EXISTS `players` (
  `id` int NOT NULL AUTO_INCREMENT,
  `first_name` varchar(255) NOT NULL,
  `last_name` varchar(255) NOT NULL,
  `position` varchar(255) NOT NULL,
  `number` int NOT NULL,
  `image` varchar(255) NOT NULL,
  `user_name` varchar(20) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB  DEFAULT CHARSET=latin1 AUTO_INCREMENT=1;
select * from players;
```


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/53f67387-d8c0-4ee8-9d7c-df31256922c5)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/a3699e83-8d84-4985-91f8-091be0369524)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/cfa6a357-380f-499b-a1f0-211f9369719c)



#### Step 9: Connect to the Setup server and install the Nodejs and mysql app with the commands below.




```
sudo yum update -y
sudo amazon-linux-extras list | grep nginx
sudo amazon-linux-extras enable nginx1
sudo yum clean metadata
sudo yum -y install nginx
nginx -v
sudo su
cd /
ls
mkdir nodesoccerapp
cd nodesoccerapp
aws s3 cp s3://georgenodesoccerapp --region us-east-1 . --recursive
ls



sudo curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
. ~/.nvm/nvm.sh
nvm install 16
node -e "console.log('Running Node.js ' + process.version)"
ls
npm install express body-parser cors mysql
While still in nodesoccerapp folder...
rm -rf package-lock.json
ls
npm install
ls (you'll notice that "package-lock.json" is back)

Make sure you're still in the nodesoccerapp folder, then do the following
npm install -g pm2 (to install pm2)


Now, there is one quick edit you need to make to the "app.js" file before we run the app using pm2. 
nano app.js
Go to the line where you have const port = 2000;
change the 2000; to 5000;


Now, let's start app...
npm install express express-fileupload body-parser mysql ejs req-flash --save
pm2 start --name app.js npm -- start
pm2 list (to list your runnig apps)

Now, let's use the commands below to generate an active startup script for PM2 so that if our server restarts for any reason, PM2 will automatically start our apps again (Read about this here https://pm2.keymetrics.io/docs/usage/quick-start/#setup-startup-script):
pm2 startup
pm2 save


If you need to stop or delete the app, use the following commands (you don't need to do this if you're going to terminate the EC2 instance at the end, but just good to know)
pm2 stop 0 (to stop app with IP 0)
pm2 delete all (to delete/completely remove the apps from the list)
```


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/5a937987-8a56-461e-bf1b-7b49626a8bec)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/723dd42a-42a0-429b-8d39-697f526ae312)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/be58fb7b-2db1-48f3-8612-f85dd073d7e6)


#### Step 9: Access the the nodejsapp on port 5000, by copying the public ip of the setup server, paste it on any browser and add ":5000" to the end.


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/1cbe65b1-0c3c-496d-9a44-847dc41ba5b3)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/119af54f-eb16-410e-872d-61ae9f831b57)

Hurray, our app can now be accessed.


#### Step 10: Test the app by inputing some details and login to the rds database via a myaql workbench tool to check.

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/0033f95b-8fd7-45dc-8d43-a92ecf3a5787)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/c384b679-8127-4959-9e91-bb55963288c3)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/99fce0c7-0179-4371-8c2e-f42d2c80d229)




If creating a load balancer for this, for healthcheck use "/"
Now, let's use nginx to forward requests to port 80 i.e. we don't have to add "5000" to the browser anymore, we can just type the IP or the provided DNS name of the EC2 instance in any browser and it should work:


``` 
sudo nano /etc/nginx/nginx.conf
Now, add this to the file, in the server section
********************************************
server {
   listen         80 default_server;
   listen         [::]:80 default_server;
   server_name    localhost;
   root           /usr/share/nginx/html;
   location / {
       proxy_pass http://localhost:5000;
   }
}
```

Now do:
sudo service nginx restart (Restart nginx)
sudo chkconfig nginx on (atomatically restart nginx if server restart or something goes wrong)
systemctl status nginx.service (to check the status of nginx and see if running)

To test, grab the DNS name of your Load Balancer and put it in your browser to see if the app will load, further test by adding a new player and adding an image higher, but less than 2MB. Great job if it worked :)



Troubleshooting nginx "413 Request Entity Too Large" error message. By default, you will get that error message if the image you upload after using nginx as a reverse proxy is high. To fix this, do:
nano /etc/nginx/nginx.conf

Now, set client body size to 5M (i.e 5MB) or whatever size your image needs. Add  the command below after the server section.
client_max_body_size 5M;

Now, reload the server:
sudo systemctl reload nginx.service
systemctl status nginx.service (to make sure nginx is still running)



Useful Links:
These links helped with some of my troublehsooting:
https://stackoverflow.com/a/66915607
https://www.cyberciti.biz/faq/linux-unix-bsd-nginx-413-request-entity-too-large/

My Google Drive: https://drive.google.com/drive/folders/1Odgbg6mR7zdJW9AFn_-xHNzQ9eWaFOyC?usp=sharing



![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/ce4a7c15-c0fa-433a-9994-1ebd2477f702)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/98959488-ce64-4201-a1f2-09b0e20936ed)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/6bdc17e6-f334-48b4-a60b-6f1dc4010267)




#### Step 11: Create an AMI of the Setup Server and delete it.

This AMI will be used for creating our Application Loadbalancer and Autoscaling Group.


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/de043d77-fdf8-40a0-af50-fff248ec41e1)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/fa3596ae-7716-4c2d-8010-09a04c232186)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/7268d27d-2ce9-4807-8486-fd8ca27ba20e)



#### Step 12: Create application Loadbalancer for distributing traffic.

First launch two servers in the private subnets with the help of the AMI.


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/ac9a114d-dd7b-4fbe-a0ab-66408e1ef8d6)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/1b588181-32d2-4942-9015-45098bfe7931)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/08e3f246-0939-425d-8d2f-1b81b1fae419)


#### Step 13: Paste the Loadbalancer dns in the browser to access the app


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/5bbcdbad-29a1-4d37-ae54-b64d3878d235)


#### Step 13: Create SSL Certificate and Secure communication in transit.

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/e3d37e11-64ff-4a16-b2f1-7ada9b35456e)


#### Add HTTPS LISTENER ON PORT 443 AND REDIRECT HTTP LISTENER (EDIT IT) TO 443



![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/7ad92cbf-c58a-4721-8e46-d80727da5444)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/27a4db87-e446-4265-9246-2a81f1317984)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/e7547ccc-dca7-4f96-aca4-4c0b14a9436c)


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/0f496de4-bd9b-4bcd-be65-f42c957b1999)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/b69485e8-5aa1-487f-a450-12343817cb57)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/707d3dcc-fb09-43e5-a09e-4233f9db9e8b)



#### Step 14: Create Record set in Route 53 and point it to the the load balancer dns

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/ee442608-a8bb-4efe-9a0b-ed01a49410d6)


#### Step 15: Copy the A record dns and paste it a browser to access the website

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/28b5d569-3636-488f-8f9c-092674d6a440)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/0dd90c3e-e7e3-4c89-8121-46e617aab2d9)



#### Step 16: Launch an Auto Scaling Group.

first delete the two(2) running servers, and launch two new servers from the AMI and attach the load balancer to it.

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/ea3136c1-1112-407e-87a2-ef8836ebb40f)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/8b6b9954-a10b-4c6d-9640-f3f75efe698b)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/1ba30251-b086-4109-83a3-543b0dd83d20)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/b447d7fc-b271-41d9-91ad-77845cd68c4d)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/eb4ce642-bc16-43fc-8f96-02978a00ff1e)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/de14468e-b484-4191-99a8-0ee6364434a2)


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/b4e26d5d-7f71-4be1-b5a2-e13a8720d11f)



#### Step 17: Type in your domain name in the browser to see if the app is accessible.


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/46157e5f-3118-40da-8a93-aa0d4dda770d)



Yes, it is.


#### In conclusion,

If you carry out all the afformentioned steps, you will succeed in deploying a nodejs/mysql app on aws.


#### Note:

Take down all created resources, to avoid charges.



































