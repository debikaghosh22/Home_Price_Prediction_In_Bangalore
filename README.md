# Home_Price_Prediction_In_Bangalore
![Website_img](https://github.com/debikaghosh22/Home_Price_Prediction_In_Bangalore/blob/master/Website_img.png?raw=true)


This is a project on prediction of prices of houses in Bangalore city using the method of Linear Regression. At first I downloaded the data set from https://www.kaggle.com/amitabhajoy/bengaluru-house-price-data#Bengaluru_House_Data.csv . After that I performed the usual process of data cleaning, detection and removal of outliers, feature reduction, dimensionality reduction. I then made use of sci-kit learn library and linear regression to build the model for the purpose of prediction of home prices following which the techniques of gridsearchcv for hyper parameter tuning and K-fold cross validation were also used.

The framework of the website was created using some HTML whcih was then styled by some CSS and finally some Javascript codes were used to make the website dynamic in nature. A Python flask server was also created which will use the saved model to serve HTTP requests . The website created offers provisions for the user to enter the location,BHK, number of bathrooms,area which will then call the Python Flask server to retrieve the predicted price. The different tools that were used to carry out this project includes the following:

* Python
* Numpy and Pandas for data cleaning
* Matplotlib for data visualization
* Scikit learn for model building
* Jupyter Notebook, Pycharm and Visual Studio Code as IDE
* Python flask for HTTP server
* HTML/CSS/Javascript for user interface

# Deploy the application on cloud using AWS EC2:

1. From the AWS console, I've created an EC2 instance and in the security group new rules were added to allow incoming traffic from HTTP and HTTPS.
2. Using GitBash, I've used the following command to connect to the instance.
```
ssh -i "C:\Users\Admin\BHP\bhp1.pem" ubuntu@ec2-3-129-207-119.us-east-2.compute.amazonaws.com
```
3. Setting up of nginx on the EC2 instance.
The following commands were used:
```
sudo apt-get update
sudo apt-get install nginx
```
Check the status nginx using the following command:
```
sudo service nginx status
```
Now when the cloud url is loaded in the browser, a message will be displayed saying "Welcome to nginx" which proves that it has been successfully setup and is running.
4. The next step is to copy all the code to the EC2 instance using WINSCP.
5. Once connected to EC2 instance from WINSCP, I copied all the code files to /home/ubuntu folder. The full path of the root folder is now: /home/ubuntu/BHP .
6. After copying code on EC2 server, I have disabled the default configuration on nginx so that it is able to render the app created instead of the default HTML page.
   Remove symlink for default file in /etc/nginx/sites-enabled directory:
```
sudo unlink default
```
   Create a new file called bhp.conf in the/etc/nginx/sites-available directory with the command:
```
sudo vim bhp.conf
```
   The contents of the file look like this:
```
server {
    listen 80;
        server_name bhp;
        root /home/ubuntu/BHP/client;
        index app.html;
        location /api/ {
             rewrite ^/api(.*) $1 break;
             proxy_pass http://127.0.0.1:5000;
        }
}
```
   Create symlink for this file in /etc/nginx/sites-enabled by running this command:
```
sudo ln -v -s /etc/nginx/sites-available/bhp.conf
```
   Now restart nginx:
```
sudo service nginx restart
```
7. The final step is to install Python packages and launch the flask server.
```
sudo apt-get install python3-pip
sudo pip3 install -r /home/ubuntu/BHP/server/requirements.txt
python3 /home/ubuntu/BHP/client/server.py
```
Running last command above will prompt that server is running on port 5000. Now just load cloud url in browser (for me it is http://ec2-3-129-207-119.us-east-2.compute.amazonaws.com/) and this will be fully functional website running in production cloud environment.

## Acknowledgement:
I'm extremely grateful to Mr Dhaval Patel whose website https://codebasicshub.com has provided a step by step guidance which has enabled me to complete this project. I've learnt so many new skills in Python, applied some concepts of Machine learning that I had only done in theory so far. Overall it was a wonderful journey with new things to learn at every step. 
