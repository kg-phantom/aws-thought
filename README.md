# Deep Thoughts

![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E)
![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)
![Express.js](https://img.shields.io/badge/express.js-%23404d59.svg?style=for-the-badge&logo=express&logoColor=%2361DAFB)
![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![AmazonDynamoDB](https://img.shields.io/badge/Amazon%20DynamoDB-4053D6?style=for-the-badge&logo=Amazon%20DynamoDB&logoColor=white)
![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white)

An application where users can post thoughts, read other users' thoughts, and include images with their thoughts.

[Click here](http://18.117.159.38/) to go to the deployed application!

![Deep Thoughts homepage](./assets/images/deep-thoughts-sc.PNG)

## Table of Contents
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Deployment](#deployment)
- [Technologies](#technologies)
- [Contributing](#contributing)
- [License](#license)

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

Download and install the following in order to install the run this project:
- [Node.js](https://nodejs.dev/learn/how-to-install-nodejs)
- [AWS CLI](https://aws.amazon.com/cli/)
    - Also create an [AWS account](https://aws.amazon.com/) if you haven't already.
- [DynamoDB](https://aws.amazon.com/dynamodb/)

### Installation

To get a development environment running:

1. Clone this repository onto your machine.
2. Navigate to the root of this repo in the command line and run `npm install`.
3. On the command line, run `aws configure` and input your IAM user Access Key ID, Secret Access Key, region name, and `json` as the Default output format:
```
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-east-2
Default output format [None]: json
```
4. From the root directory of the application, run `node create-bucket.js` on the command line to create the S3 bucket.
    - You can verify that the bucket was created by checking the S3 console from your AWS Management Console in the browser.
5. On the command line, navigate to the downloaded folder for [DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html) and run the following to create a local DynamoDB instance for development:
```
java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -sharedDb
```
6. Open `./server/db/CreateThoughtsTable.js` in your IDE and modify the AWS config object to connect to the local DynamoDB instance by adding `endpoint: "http://localhost:8000"` underneath the `region`:
```js
AWS.config.update({
  region: "us-east-2",
  endpoint: "http://localhost:8000" // add this line
});
```
7. Repeat the previous step in `./server/db/LoadThoughts.js` if you want to seed the database.
8. Navigate to the root directory of the application and run `node ./server/db/CreateThoughtsTable.js` on the command line to create the `Thoughts` table.
    - If you want to seed the database, run `node ./server/db/LoadThoughts.js`.

## Usage

To run the application locally, navigate to the root of the application on the command line and run `npm start`.
- Type `Ctrl + C` to quit the application.

From the application in the browser:
- Thoughts can be viewed on the homepage.
- Clicking on a username will take you to the user's page with all of their thoughts.
- From the homepage, you can input a username, thought, and/or upload an image.
    - To upload an image, click the `Choose File` button, select an image from your machine, and then click the `Upload` button.
    - Submit the thought and uploaded image, click the `Submit` button.

From Insomnia:
- Use the base route `localhost:3001/api` to make `GET` and `POST` requests.
    - `/users`: GET all thoughts
    - `/users/:username`: GET all thoughts from a user
    - `/users`: POST a thought
    - `/image-upload`: POST an image

## Deployment

To deploy a production version of the application:

### Deploy DynamoDB to AWS:

1. Open `./server/db/CreateThoughtsTable.js` in your IDE and remove the `endpoint` property in the `AWS.config.update` statement:
```js
AWS.config.update({
  region: "us-east-2"
  // removed endpoint: "http://localhost:8000"
});
```
2. Repeat the previous step in `./server/db/LoadThoughts.js` if you want to seed the database.
3. From the root directory of the application, run `node ./server/db/CreateThoughtsTable.js` to create the `Thoughts` table in DynamoDB.
    - If you want to seed the database, run `node ./server/db/LoadThoughts.js`.
    - To check that the table was created in AWS, log into your AWS account in the browser and select the DynamoDB service. Select the Tables option and then the `Thoughts` table. You should see data in the `Thoughts` table if you seeded it with `LoadThoughts.js`.

### Create an EC2 instance in AWS:

1. Go to the AWS Management Console in the browser and select EC2 in Services.
2. From the EC2 console, click the Launch Instance button.
3. Select the Ubuntu Server AMI Template as the application server. This should be on the free-tier.
4. Select the `t2.micro` option as the instance type.
5. Create an IAM role in the IAM console in another browser tab.
    - Create a policy and paste the following into the JSON tab:
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "DescribeQueryScanBooksTable",
                "Effect": "Allow",
                "Action": "dynamodb:*",
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": "s3:*",
                "Resource": "*"
            }
        ]
    }
    ```
    - Click the Review button and name the IAM role `S3-DynamoDB`.
    - Assign this role to the EC2 instance.
6. Keep the default settings for Storage and Tags.
7. On the Security Group page, add a new security group called `launch-wizard-1` and add the following rules:
    - Type: HTTPS, Custom IP Range: 0.0.0.0/0, ::/0
    - Type: HTTP, Custom IP Range: 0.0.0.0/0, ::/0
8. Select the Review and Launch button.
9. Select `Create a new key pair` from the dropdown, name it something relevant like `deep-thoughts`, and then download the key pair.
10. Save the downloaded `.pem` file in the `~/.ssh/` folder. If this folder doesn't exist, create it.

### Set up the EC2 environment:

1. On the command line, navigate to the `.ssh` folder and navigate to the EC2 console in the browser.
2. From the EC2 dashboard, select Running Instances. The instance we just created should be running.
3. Look at the Network tab and copy down the public IPv4 address.
4. Select the Connect button at the upper left section of the page.
5. On the command line in the `~/.ssh` directory, run the following:
```
chmod 400 aws-thought.pem
ssh -i "aws-thought.pem" ubuntu@ec2-[INSERT IPv4 ADDRESS HERE].us-east-2.compute.amazonaws.com
```
6. If the commands are successful you will be prompted to confirm connection. Respond with `yes` and then you should see a message similar to the following:
![Ubuntu success message](./assets//images/900-ubuntu-connect.png)

### Install Application Environment Dependencies

1. After receiving the success message in the previous section, run `sudo su -` to gain root access control.
2. Run `apt update` to update the environment.
3. Run `apt install awscli` to install AWS-CLI.
4. Run `mkdir ~/.ssh; cd ~/.ssh;` to create the `.ssh` folder and `cd` into it.
5. Run `aws configure` and enter the access key, private key, and region when prompted. Assign data type to JSON.
6. Run the following commands to install Node and npm:
```
curl -sL https://deb.nodesource.com/setup_10.x | bash -
apt install -y nodejs
```
- Run `node --version` to verify installation.

7. Run `apt install git-all` to install Git.
- Run `git --version` to verify installation.
8. Copy the HTTPS URL on the main branch of your GitHub repo for this application.
9. On the command line, navigate to `/opt/` and clone your repo.

### Install and Configure the NGINX Server

1. Run `apt install nginx`.
- Test the installation by running `systemctl start nginx`.
2. Run `nano /etc/nginx/sites-available/default` and replace the configuration with the following:
```json
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name _;

    location /{
            root /opt/aws-thought/client/build;
            index index.html;
            try_files $uri /index.html;
    }
    location /api/ {
            proxy_pass http://localhost:3001;
    }
}
```
3. Exit and save the config file by pressing `Ctrl+X`.
4. Run `systemctl restart nginx` to restart the server.
5. Run `systemctl enable nginx` to automatically launch the server whenever the EC2 instance is started.

### Modify the API Calls

The API calls need to be changed to target the correct production path.

1. Run `nano /opt/aws-thought/client/src/pages/Home.js` and replace the path in the `fetch` call to update the URI with the IPv4 address:

![Example replacement for the fetch call](./assets/images/1100-home-fetch.png)

2. Repeat the previous step in `/opt/aws-thought/client/src/pages/Profile.js` and `/opt/aws-thought/client/src/components/ThoughtForm/index.js`.
3. Run the following commands to install the app's dependencies:
```
cd /opt/aws-thought
npm install
```

### Build and Run a Production Version of the Application

1. Navigate to the client directory and run `npm run build`.
2. Run `npm install pm2 -g` to install the process manager pm2 globally.
3. Run `pm2 start node_modules/react-scripts/scripts/start.js --name "deepthoughts"`.
4. Run `pm2 start server.js` from the `aws-thought/server` location to start the server.

Go to the public IPv4 address in the browser to go to your deployed application!

## Technologies

* [React](https://reactjs.org/) - Library for building front-end components.
* [Node.js](https://nodejs.org/en/docs/) - JavaScript runtime environment.
* [Express](http://expressjs.com/) - Node framework for API and routes.
* [AWS](https://aws.amazon.com/) - Amazon Web Services used for cloud computing.
    - [S3](https://aws.amazon.com/s3/) - Simple Storage Service to store uploaded images.
    - [DynamoDB](https://aws.amazon.com/dynamodb/) - NoSQL database to store thoughts.
    - [EC2](https://aws.amazon.com/ec2/) - Elastic Compute Cloud to configure a Linux server to host the application.
* [nginx](https://www.nginx.com/) - Web server to expose the EC2 instance to the internet.

## Contributing

If you would like to contribute to this project, you can reach out to me by [email](mailto:ksurbayan@gmail.com)!

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.