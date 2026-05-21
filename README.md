1. DOCKERFILE FOR NORMAL JAVA APPLICATION
Use when project has:
App.java
and uses:
javac App.javajava App

Project Structure
javaapp ├── App.java └── Dockerfile

Dockerfile
FROM openjdk:17WORKDIR /appCOPY App.java .RUN javac App.javaCMD ["java","App"]

Build Image
docker build -t javaapp .

Run Container
docker run javaapp

2. DOCKERFILE FOR MAVEN JAVA APPLICATION
Use when project has:
pom.xmlsrc/target/
and uses:
mvn clean package

Project Structure
mavenapp ├── pom.xml ├── target │    └── app.jar └── Dockerfile

Dockerfile
FROM openjdk:17WORKDIR /appCOPY target/mavenapp-1.0-SNAPSHOT.jar app.jarCMD ["java","-jar","app.jar"]

Build Image
docker build -t mavenapp .

Run Container
docker run mavenapp

3. DOCKERFILE FOR REACT APPLICATION
Use when project uses:
npm start

Project Structure
reactapp ├── package.json ├── src └── Dockerfile

Dockerfile
FROM node:18WORKDIR /appCOPY . .RUN npm installEXPOSE 3000CMD ["npm","start"]

Build Image
docker build -t reactapp .

Run Container
docker run -p 3000:3000 reactapp
Open:
http://localhost:3000

4. DOCKERFILE FOR NODE.JS APPLICATION
Use for Express or basic Node apps.

Project Structure
nodeapp ├── package.json ├── server.js └── Dockerfile

Dockerfile
FROM node:18WORKDIR /appCOPY . .RUN npm installEXPOSE 5000CMD ["node","server.js"]

Build Image
docker build -t nodeapp .

Run Container
docker run -p 5000:5000 nodeapp

5. DOCKERFILE FOR FLASK APPLICATION
Use for Python Flask apps.

Project Structure
flaskapp ├── app.py ├── requirements.txt └── Dockerfile

requirements.txt
flask

Dockerfile
FROM python:3.10WORKDIR /appCOPY . .RUN pip install -r requirements.txtEXPOSE 5000CMD ["python","app.py"]

Build Image
docker build -t flaskapp .

Run Container
docker run -p 5000:5000 flaskapp

6. DOCKERFILE FOR STANDALONE PYTHON APPLICATION
Use for simple Python scripts.

Project Structure
pythonapp ├── app.py └── Dockerfile

Dockerfile
FROM python:3.10WORKDIR /appCOPY . .CMD ["python","app.py"]

Build Image
docker build -t pythonapp .

Run Container
docker run pythonapp

7. DOCKERFILE FOR SPRING BOOT APPLICATION
Use for Spring Boot Maven projects.

Dockerfile
FROM openjdk:17WORKDIR /appCOPY target/*.jar springapp.jarCMD ["java","-jar","springapp.jar"]

8. DOCKERFILE FOR MULTISTAGE MAVEN BUILD
Very important for viva.
This builds Maven INSIDE Docker.

Dockerfile
FROM maven:3.9.6-eclipse-temurin-17 AS buildWORKDIR /appCOPY . .RUN mvn clean packageFROM openjdk:17WORKDIR /appCOPY --from=build /app/target/*.jar app.jarCMD ["java","-jar","app.jar"]

QUICK DIFFERENCE TABLE
ApplicationBase ImageCommandJavaopenjdk:17java AppMavenopenjdk:17java -jarReactnode:18npm startNode.jsnode:18node server.jsFlaskpython:3.10python app.pyPythonpython:3.10python app.pySpring Bootopenjdk:17java -jar

MOST IMPORTANT COMMANDS
Build Docker Image
docker build -t imagename .
NOTE:
That final . is VERY IMPORTANT.
Without it Docker becomes spiritually confused 🐳💀

Run Container
docker run imagename

Port Mapping
docker run -p 3000:3000 reactapp
Format:
HOST_PORT : CONTAINER_PORT

VIVA QUESTIONS
Why Docker?
Answer:

Docker provides containerization, portability, isolated execution environment, scalability, and consistency across different systems.


Difference between Image and Container
ImageContainerBlueprintRunning instanceStaticExecutingRead-onlyActive process

What does EXPOSE do?

EXPOSE informs Docker about the port used by the application.


What is WORKDIR?

WORKDIR sets the working directory inside the container.


GOLDEN RULE
TechnologyBase ImageJavaopenjdkReact/NodenodePython/Flaskpython
Remember this and half the Docker exam already surrendered 🏳️


1. JENKINS PIPELINE SCRIPT FOR NORMAL JAVA APPLICATION

Use when project contains:

App.java

and compilation uses:

javac App.java
Jenkinsfile
pipeline {

    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'YOUR_GITHUB_URL'
            }
        }

        stage('Compile Java Program') {
            steps {
                sh 'javac App.java'
            }
        }

        stage('Run Java Program') {
            steps {
                sh 'java App'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t javaapp .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run javaapp'
            }
        }
    }
}
2. JENKINS PIPELINE SCRIPT FOR MAVEN APPLICATION

Use for Maven projects.

Jenkinsfile
pipeline {

    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'YOUR_GITHUB_URL'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t mavenapp .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run mavenapp'
            }
        }
    }
}
3. MAVEN + DOCKER HUB PUSH PIPELINE

Very important practical variation.

Jenkinsfile
pipeline {

    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'YOUR_GITHUB_URL'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t mavenapp .'
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh 'docker tag mavenapp YOUR_DOCKER_USERNAME/mavenapp:v1'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push YOUR_DOCKER_USERNAME/mavenapp:v1'
            }
        }
    }
}
4. MAVEN + CRON TRIGGER PIPELINE
Jenkinsfile
pipeline {

    agent any

    triggers {
        cron('* * * * *')
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'YOUR_GITHUB_URL'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t cronapp .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run cronapp'
            }
        }
    }
}
5. MAVEN + AGENT NODE PIPELINE

For Jenkins Master-Agent architecture.

Jenkinsfile
pipeline {

    agent {
        label 'agent1'
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'YOUR_GITHUB_URL'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t agentapp .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run agentapp'
            }
        }
    }
}
6. MAVEN + CRON + AGENT PIPELINE

Most complete exam version 🔥

Jenkinsfile
pipeline {

    agent {
        label 'agent1'
    }

    triggers {
        cron('* * * * *')
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'YOUR_GITHUB_URL'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t agentapp .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run agentapp'
            }
        }
    }
}
7. JENKINS PIPELINE FOR REACT APPLICATION
Jenkinsfile
pipeline {

    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'YOUR_GITHUB_URL'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t reactapp .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 3000:3000 reactapp'
            }
        }
    }
}
8. REACT + DOCKER HUB PUSH PIPELINE
Jenkinsfile
pipeline {

    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'YOUR_GITHUB_URL'
            }
        }

        stage('Install Packages') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React Project') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t reactapp .'
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh 'docker tag reactapp YOUR_DOCKER_USERNAME/reactapp:v1'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push YOUR_DOCKER_USERNAME/reactapp:v1'
            }
        }
    }
}
9. NODE.JS JENKINS PIPELINE
Jenkinsfile
pipeline {

    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'YOUR_GITHUB_URL'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Application') {
            steps {
                sh 'node server.js'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t nodeapp .'
            }
        }
    }
}
10. FLASK APPLICATION PIPELINE
Jenkinsfile
pipeline {

    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'YOUR_GITHUB_URL'
            }
        }

        stage('Install Requirements') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Flask App') {
            steps {
                sh 'python app.py'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t flaskapp .'
            }
        }
    }
}
11. STANDALONE PYTHON PIPELINE
Jenkinsfile
pipeline {

    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'YOUR_GITHUB_URL'
            }
        }

        stage('Run Python Script') {
            steps {
                sh 'python app.py'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t pythonapp .'
            }
        }
    }
}
12. FULL ENTERPRISE STYLE PIPELINE

This one looks VERY impressive in viva 😭🔥

Jenkinsfile
pipeline {

    agent {
        label 'agent1'
    }

    triggers {
        cron('*/5 * * * *')
    }

    environment {
        IMAGE_NAME = 'enterpriseapp'
        DOCKER_USER = 'YOUR_DOCKER_USERNAME'
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'YOUR_GITHUB_URL'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh 'docker tag $IMAGE_NAME $DOCKER_USER/$IMAGE_NAME:v1'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $DOCKER_USER/$IMAGE_NAME:v1'
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker run -d $DOCKER_USER/$IMAGE_NAME:v1'
            }
        }
    }
}
MOST IMPORTANT PARTS OF JENKINSFILE
Section	Purpose
pipeline	Defines pipeline
agent	Selects node/agent
stages	Groups tasks
steps	Commands executed
sh	Linux shell command
triggers	Automatic execution
environment	Global variables
CRON CHEAT SHEET
Expression	Meaning
* * * * *	every minute
*/5 * * * *	every 5 minutes
0 * * * *	every hour
GOLDEN EXAM FORMULA
Clone
↓
Build
↓
Test
↓
Docker Build
↓
Push
↓
Deploy

That sequence is basically the Avengers lineup of CI/CD ⚔️🤖
