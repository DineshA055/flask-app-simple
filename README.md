# flask-app-simple
What is Container ?
A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.
What is docker?
 A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application


Simple term of Container and Docker :- 
    • A container is a standard unit of software that packages up code and all its dependencies
    • A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application


Step by Step guide for create and run container using Docker 

 
1. Create a EC2 Instance 

2. install jenkins 
Below step to to install jenkins
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins

3. After open port 8080 for jenkins in security group to access by myip
4. Now you can access jenkins and create user profile
    get initial password to set user profile  [ sudo cat /var/lib/jenkins/secrets/initialAdminPassword ]   
4. Create a project can done through free-style project or pipeline based on scope 
5. Name a project and add git repository http (copy from git <code> click to clone <copy to URL clipboard)
6. Add http clone URL in Git repo [----------]
7. Now required to add credentials 
   step 1 :- go to ec2 instance terminal generate a private key and public using ssh-keygen -t rsa
   step 2 :- public and private key where created available in current directory
   step 3 :- you can check CMD [./ssh]  to display the created key name
   step 4 :- cat publickey & cat privatekey of you key name 
   step 5 :- copy publickey and go to github page -> go to setting -> right side SSH key -> click to add the key in text page to generate -> save
   step 6 :- cat private key copy and paste in jenkins where you have add git repo belwo you can see to add credentials 
   step 6 :- Add :- jenkins icon -> select has SSH user name and add key in textbox and save 
   step 7 :- Git and jenkins can talk by using SSH credentials
8. Now you add the source code in repository are already added -> go for next step
9. In jenkins page left side for created project -> Tap the button to click------> Build NOW in Jenkins 
10. Now you can see build will be started show the green colour build is succeded ----> menas jenkins can communicate with github repository 
11. Now you can see workspace created with you source code folder and file will be present in jenkins workspace -[/var/lib/jenkins/workspace/flask-simple-app]

12. Now you go with build a docker image to run the conatainer 
     step1 :- Linux 
               $ sudo apt install docker.io
                Now docker demon has to run because it is heart of docker like kernel in linux
                Docker have a base image 
   step2 :- Now required to add user to docker group for access
              $ sudo usermod -aG docker $USER
        some time required to restart instance to apply the changes if demon is not starting
   step3 :- now start build the container 
         create a dockerfile -> required to add step to build a image 
         requirement.txt -> mention system dependencies to run the application in container
          app.py or view.py -> have you python code defined to expose to 5000 port when it is executed
          if python driven to expose the application in web browser if required to mention flask to install 
   step4 :- Now all the file available to build the conatiner
           CMD :- $ docker build -t simple-app .          ##### -t tag  && simple-app is project name in git have with your file and folder to build 
dot . (take all your file inside the repo to build Sorce code and collect the requirements to run application)
  step 5:- Now you can see step by step source is build in container but not started
  step 6:- Once it shows build is success
  step7 :- you can run the container      $ docker run docker run -p 5000:5000 -d flask_docker       -p port to expose to app to run in port 5000 to bind to run machine with another 5000
   step7 :- Now you can see a container ID was created means conatainer is running 
   step8 :- if you want to see container ID and port it was exposed status
         $ docker ps 
display conatiner ID and PORT exposed in 5000 and status of running
   step9 :- if you want to run the application only in your machineip   
   step10 :- Go to instance ->security group -> allow -> port 5000 with myip

last you can see the applicaton expose to 5000 port you can see application running in browser as well in container
    incase if you want to stop container 
       $docker (contanerID)


NEXT AUTOMATE PROCESS

when there is commit happen in repository
automate trigger and build the image and run in container through CI/CD process jenkins

Step1 :- Please go to jenkins configure with current project

     Click ------> Configure ------> Scroll down -----> in Build steps --> Execute Shell command ---> 
Add in shell command test box 
        docker image build -t flask-app-simple .
       docker run -p 5000:5000 -d flask-app-simple

      Step2 :- Before build please give permission to workspace for project ----> chmod 777 [workspace-of-project-github-repo-project]
      step3 :- Add jenkins to docker group    
            $ sudo usermod -a -G docker jenkins
            $ sudo systemctl restart jenkins    
Done with steps
     Now navigate to project app -> left side build-Now -> Automated the process from Git-commot -> jenkins -> build a image -> run image in container -> expose port 5000 to render application in browser
     
    final you can see application rendering in browser
          $ docker ps      -> cmd know the status of your container running has UP



Important :-

Create a docker file 
   CONTAINERS - RUN
1. Os - ex:- Ubuntu required for support base image
2. Code required 
3. Libraries - to run requirement
4. Command - to run the application and expose 

Process (Docker-Container)   :- 1st Build process -----> 2nd Image build -----> 3rd Run application in container
