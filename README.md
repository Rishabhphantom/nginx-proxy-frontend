This is frontend part of the react app. It is a very simple app as the main motto of this project is to create CI/CD pipeline using github and run our project using docker container on ec2 server

# Run project locally
make sure you have installed nodejs in your machine
create mysql database in local machine and run command

- npm install
- npm start


# Run project using docker
create mysql database in local machine and run command

- docker build -t frontend .
- docker run -d -name frontend -p 3000:80 frontend

# Run using github action in ec2 server

you need to copy project to your own repo and change the yml file according to it. you need to mention your credentials in secrets of your github repo

for more info on this process you can go through
