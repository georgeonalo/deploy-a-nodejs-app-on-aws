# Deploy a nodejs-mysql crud app on aws 

In this project, i deployed a nodejs-mysql soccer app on aws. This app allows players to add/input their details like, position, name, username and photo in a database.

## Project reference architecture

![nodejs vpc diagram](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/20695247-9310-40ca-bce4-ab83ebe42e23)

## Steps taken to complete this project:

### Step 1: Created a custom 3 tier vpc with public and private subnets.

I created a vpc with two(2) public subnet and four(4), internet gateway, nat gateway, route table and more.

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/53244734-addb-4855-bbad-217702814777)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/a23f7e71-65ec-4368-a118-87460e99ffd4)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/43dc2e58-4420-4170-b04f-a613c5f129b0)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/f860631d-5237-4a4f-935b-9d206f6fd7dc)






### Step 2: Create an rds instance.

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/e5841786-9e2f-4a35-a4cf-e268378d2712)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/3280d3b8-d049-4b0c-b148-1c361470d8c6)






### Step 3: Create and S3 bucket and upload the Nodejs web files(codes)

Before uploading, i first edited the app.js file buy adding my rds endpoint to the section in the app.js code containing the rds endpoint.

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/715ba2e9-48fb-4fca-bf7d-e0e93a4eeeee)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/9f512c9d-c793-4b92-8d43-884e5c8581be)







### Step 4: Connect to the rds instance with mysql workbench

The purpose of connecting to the rds database is to add the needed database and tables to our already created RDS-MySqL database.




```
Let's first add the needed database and tables to our already created RDS-MySqL database... Open MySQL Workbench, create a new connection with the credentials you used when creating your MySQL RDS in the AWS Console.
Now copy the code below, paste and run it.

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






![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/aee38130-54e6-41ad-8faa-bab450fe470e)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/1140670a-d58b-46dd-aeb4-e75e40e4a6ee)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/ed276123-0d3c-41b1-8d89-ac643e49e393)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/5719dcc3-19be-42dc-84e0-2825104326af)






### Step 5: Create an EC2 instance

I created an ec2 instance with s3 and session manager role attached to it, its on this instance my nodejs application is installed.



![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/ca06910a-0153-4a2c-a3e8-968f52ecf622)



After remotely connecting with my ec2 server with session manager, i used the following commands below to install the nodejs app on it.




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
aws s3 cp s3://nodemysoccerapp --region us-east-1 . --recursive
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



If creating a load balancer for this, for healthcheck use "/"
Now, let's use nginx to forward requests to port 80 i.e. we don't have to add "5000" to the browser anymore, we can just type the IP or the provided DNS name of the EC2 instance in any browser and it should work:
 
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
********************************************

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
```




![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/6342dc93-6e51-4b59-8df3-c38358a1a875)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/de0f5868-d9f6-4f0b-9cbd-cc8f14b35985)



To test if all my installation was properly done, i simply copied the public ipv4 address of my instance and pasted it on my browser to access the app.


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/46370cd8-6a42-4156-8cd4-9d177b335156)


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/ff8d76d3-5aa3-418b-86e3-832e14b543d4)



To add details on a soccer player profile simply click "add player", this allows one to such info as, player position, name, username, photo and much more, once done simply click the send button.

To comfirm, if this details have been properly captured by the rds database, simply connect to it using mysql workbench tool.


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/6a77c436-fa5d-484f-bad0-fed4ce241ad7)



Yes its captured.




### Step 6: Create an Application Load Balancer

I created an application load balancer to direct traffic to my webserver.


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/ea332f78-877c-47ff-bc6b-560f7b731e4a)

![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/dbd1fc68-3890-463d-85d4-ce46760d0334)




### Step 7: Secured connection with ssl/tls certificate

To ensure, my application is secured when users interract with it, i simply created a certificate in certicate manager console.

This cerficate was now used to link the https listenner in the application load balancer.



![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/c88861da-f2ce-4c9b-ba5c-26e6546b401a)


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/303792d1-093d-44c7-b9dd-e922e3b0e469)



### Step 8: Create a record in route 53 

Finally, create a record in route 53 to point it to the dns of the load balancer service created earlier.


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/585bd49b-153c-4733-bb03-bdb1bb885715)


Once the record has been created, i paste the record name on a browser to access my nodejs-mysql web application.


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/b81bc9ab-3f3b-41bb-8b96-7ed6eccd3046)


![image](https://github.com/georgeonalo/deploy-a-nodejs-app-on-aws/assets/115881685/b1c73889-c9e6-4b5b-afac-3a19363b32fc)






