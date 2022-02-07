# Deep Thoughts

![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E)
![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)
![Express.js](https://img.shields.io/badge/express.js-%23404d59.svg?style=for-the-badge&logo=express&logoColor=%2361DAFB)
![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![AmazonDynamoDB](https://img.shields.io/badge/Amazon%20DynamoDB-4053D6?style=for-the-badge&logo=Amazon%20DynamoDB&logoColor=white)
![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white)

An application where users can post thoughts, read other users' thoughts, and include images with their thoughts.

![Deep Thoughts homepage](./assets/images/deep-thoughts-sc.PNG)

## Table of Contents
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Object-Oriented Programming](#object-oriented-programming)
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


## Usage


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