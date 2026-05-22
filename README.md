Program 3: Maven Project with JSON Dependency (CI/CD Pipeline)
Step 1: Create Maven Project in Eclipse
Open Eclipse IDE
Go to:
File → New → Maven Project → Next
Select:
maven-archetype-quickstart
Version 1.1 or 1.4
Click Next

Fill:

Group Id → anything
Artifact Id → same as package name
Package name → same as artifact name
Step 2: Add Dependency

Open browser and search:

Maven Repository

Search:

json simple

Open:

json-simple 1.1.1

Copy dependency and paste inside pom.xml

<dependency>
    <groupId>com.googlecode.json-simple</groupId>
    <artifactId>json-simple</artifactId>
    <version>1.1.1</version>
</dependency>
Step 3: Create JSON File

Right click package → New → File

Filename:

san.json

Content:

{
  "fname":"Sam",
  "lname":"Shetty"
}
Step 4: Create Java Class

Right click src/test/java

New → Class → Demo.java

Code:

import java.io.FileReader;

import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;

public class Demo {

    public static void main(String[] args) {

        JSONParser p = new JSONParser();

        try {

            Object obj = p.parse(new FileReader("san.json"));

            JSONObject jo = (JSONObject) obj;

            String fname = (String) jo.get("fname");
            String lname = (String) jo.get("lname");

            System.out.println("Fname : " + fname);
            System.out.println("lname : " + lname);

        }
        catch (Exception e) {
            e.printStackTrace();
        }
    }
}

Save and Run.
.............................................
Program 1: Docker Program
Step 1: Create Folder

Create a folder and open it in Visual Studio Code

Step 2: Create Dockerfile

Create file:

Dockerfile

Content:

FROM eclipse-temurin:21-jdk
WORKDIR /app
COPY . /app
RUN javac prime.java
CMD ["java","prime"]

Replace prime.java with your Java filename.

Step 3: Create Java File

Example:

class prime {
    public static void main(String args[]) {

        int n = 7, flag = 0;

        for(int i = 2; i <= n / 2; i++) {
            if(n % i == 0) {
                flag = 1;
                break;
            }
        }

        if(flag == 0)
            System.out.println("Prime Number");
        else
            System.out.println("Not Prime");
    }
}

Save the file.

Step 4: Run Commands

Open terminal and run:

docker build -t h1 .
docker run h1

Output will be shown.

Make sure:

Docker Desktop is running
Docker Engine is ON
Image appears in Docker Desktop
............................................................................
Program 2: Jenkins Workflow and Push Docker Image
Step 1: Create GitHub Repository

Create repo in GitHub

Add:

Dockerfile
Java file
Jenkinsfile
Dockerfile
FROM eclipse-temurin:17
WORKDIR /app
COPY . .
RUN javac app.java
CMD ["java", "app"]
Java Program
class prime {
    public static void main(String args[]) {
        System.out.println("Hello Jenkins");
    }
}
Jenkinsfile
pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'Docker-credentials'
        IMAGE_NAME = 'pannu27/new_docker_image'
    }

    stages {

        stage('Build Java Application') {
            steps {
                bat 'javac prime.java'
            }
        }

        stage('Run Java Program') {
            steps {
                bat 'java prime'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:latest .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                credentialsId: 'Docker-credentials',
                usernameVariable: 'USER',
                passwordVariable: 'PASS')]) {

                    bat 'echo %PASS% | docker login -u %USER% --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %IMAGE_NAME%:latest'
            }
        }
    }
}
Configure Jenkins Credentials

Open Jenkins

Go to:

Manage Jenkins
Manage Credentials
Add Credentials

Select:

Username with password

Enter:

DockerHub username
DockerHub password

ID:

Docker-credentials

Create.

Create Pipeline Project
Jenkins → New Item
Select:
Pipeline Project
OK

Under:

Pipeline Script from SCM
SCM → Git
Paste GitHub repo link
Add credentials
Branch → main

Apply and Save.

Click:

Build Now

All stages should show green check marks.

Docker image will appear in DockerHub.
..............................................................................
Program 4: Slack Notification in Jenkins
Step 1: Open Slack

Open:

Slack

Create:

New Workspace / Channel

Example channel:

lab-practice
Step 2: Add Jenkins App

In Slack:

More → Tools → Apps
Search:
Jenkins
Open App
Configure
Add to Slack
Select Channel
Integrate

Go to Step 3 and copy the Token.

Step 3: Install Jenkins Plugins

In Jenkins:

Manage Jenkins
Plugins

Install:

Slack Notification
Global Slack Notifier

No restart needed.

Step 4: Configure Slack in Jenkins

Go to:

Manage Jenkins
System
Slack

Fill:

Workspace:

csections-workspace

Credentials:

Add
Global credentials
Secret text

Paste Slack Token.

Give any ID and Create.

Select the credential from dropdown.

Click:

Test Connection

It should show:

Success

Apply and Save.
..........................................................................
Step 5: Create FreeStyle Project
New Item
FreeStyle Project
OK
Build Steps

Add Build Step:

Execute Windows Batch Command

Command:

java -version
Post Build Actions

Add:

Slack Notifications

Select:

Notify Build Start
Notify Success
Notify Abort

Save.

Click:

Build Now

Slack notifications will appear in the selected Slack channel.
