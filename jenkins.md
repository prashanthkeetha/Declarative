In jenkins there is
------------------- 
CI-Continous Integration where the changes continously integrate in the dev branch
CD-Continous Deployment where the changes continously Deploy
CD-Continous Delivery where the changes is continous Delivery

# CI
-----
In continous Integration the regular changes that made by the devloper is pushed.
Steps
1. Integrated
2. Built: The package
3. Tested: Automated test suites are run
4. Archived: We can archive or store the data.
5. Deployed: we deploy the data the data that which is working. 
* Benefits:
----------
1. immediate bug detection.
2. No integration step in the lifecycle
3. A deploy

# In the jenkins we have Newitem to run the jobs or the projects

1. New-Item
           Diffrent types of projects in jenkins.
           1.Freestyle
           In Freestyle projec tif we need to mention the label that to build in the selected node use <Restrict where the project need to run>
           2.Pipeline
           3.Multibranch-pipeline:
                 This is used for project where we have different branches and each branch has different pipeline.
                 In multibranch pipeline there is a <Discover branches> in that <Filter by name (wildcard)> which means by specify the branch we can build 
           4.Mavenproject 
              To see the maven project we need to install <Maven Integration plugin>
           5.MultiConfigration:
                 For building projects which have different configurations for different environments such as linux, windows, mac etc

* Homedirectory of Jenkins: /var/lib/jenkins.
---------------------------------------------------
# Maven is a build tool where we package the java project code
the steps of maven is
1. mvn compile - It compile the project
2. mvn test - It 
3. mvn package
4. mvn install
5. mvn deploy
In Maven project there are multiple folders
'''
In maven project we have main and test,targetfolder
1. In main folder we have src(source code)
2. In test folder we have test cases related code.
3. In target folder we have output which is jar/war files
'''
Node Configration:
-----------------
the node configration works as a slaves for the master node 
The jenkins have two types of nodes.
1. Master node: where we install the jenkins and attach the nodes to this master node
2. NOde: where we install the requied project softwares and attach it to the master using labels.
Adding more nodes increase the more executors where we can run the more projects.

Environmet variables:
--------------------
printenv: we used this to see the environment vriables of the host machine
set
declear
\*
FOLDER="/tmp/${JOB_NAME}/${BUILD_ID}" 
mkdir -p "${FOLDER}"
cp "./gameoflife/target/gameoflife.war" "${FOLDER}" 
\*
Upstream and downstream job
---------------------------
the job which build depending on the other job is called upsteram and downstream job.    

Global Configration tool
------------------------
In this Go to manage jenkins and add the path of the necessary dependency software "HOME_PATH"
ex: Java
    maven

PLUGINS
-------
Jenkins plugins should also be installled in the server as well not only in the GUI.
The plugins that we are installing in the jenkins GUI is just a functionality which means you are supposed to install it in the server as well.

# In the jobs we have multiple steps.
1. General
     in this there is prameterized
2. source code management: 
                        It is a version control system where we give the url of source code.
3. Build Triggres:
   * Trigger builds remotely(eg from scripts).
         Authntication Token.
    * Build after other projects are built
             in this (projects to watch)
                   we give the job name or project name.
    * Build periodically
            WE CAN SHEDULE THE BUILD PERIODICALLY.
   * Github hook trigger for GITSCM polling:
                when some changes happen in the git repository it need to be automatically triggred to trigger that you need to DO THIS PROcess
                Go to <MANAGE JENKINS->SYSTEM->GITHUB SERVERS> In that click on the <ADVANCE->Manage additional GitHUB actions>
                to trigger the changes to the jenkins server
                1. in github settings there is <webhooks > and click on <add webhook> in this enter the jenkins url<ip:port/github-webhook/>and click on add

   * POll Scm:
             In this in between the time there are any changes it will trigger the changes.
4. Build Environment
5. Build steps
6. post build actions

# we can create a extra user in the jenkins in manage users
* in configure global security there is <authorization> in that there is <matrix-based security> where we give permissions to the users.
* Jenkins has following ways for usermangement
  1. Delegate to servlet container: This is external application which maintains users
  2. Jenkins own user database
  3. LDAP
  4. Unix User/group database
  5. None
   The above Configuration is Manage Jenkins => Configure Global Security

# Backup configrations
----------------------
Plugin installations:
Available from jenkins community (Manage Plugins => Available=>install)
TomBAck complete data in the jenkins home directory can creat a <freestyle job> and go to executeshell  <rsync /var/lib/jenkins /destination to store the backup eg : /project/data/>

# Jenkins PIPELINE:
-------------------
In pipeline there are two types 
1. scripted pipeline: this is were we directly execute in the pipeline project.
2. Declarative pipeline: this is the pipeline where we execute by using the Version Control System by pushing jenkinsfile into the Project repository.

1.In pipeline we write the key word called parametes to take the data from the parameters.
In Parameters we have different types of parameters.
1. string parameters: can write the values by our own and we can give the parameter in the <build steps in that is parameterized>
2. Choice parameters: can give multiple options and we can choice from it.
we write it in
pipeline {
     agent {label 'name of the label'}
     parameters{
          string(inside the string we give the parameter data)
     }
}
----------
In pipeline we can give the (input) inside the steps of the stage weather to execute the next stage or not
To see the clone repository url you can mention <git config --list>
# Declarative PIPELINE
THe declarative pipeline we write in the domain specific language(DSL) by using groovy language.
pipeline {
     agent {label 'SPC_17'}
     triggers {corn (* * * * *)}
     parameters { string }
     stages{
          stage(stage1){
               steps{
                    echo 'Dev'
               }
          }
          stage(stege2){
               steps{
                    echo 'QA'
                    input "Does the staging environment looks ok?"
               }
          }
          stage(stage3){
               steps{
                    echo 'UAT'
               }
          }
     }
     post {

     }
}
# Pipeline steps:
---------------
1. Pipeline: This is were we start the pipeline.
2. Agent: agent is used to execute the place of the node that were we need to execute the pipeline, By using label we give the name of the node <agent{label 'neameofthenodelabel we configured'}
3. Triggres: using this we can schedule the corn jobs using 
3. Stages: In stages we have stage. 
4. stage: In stage we mention the build name we have steps.
5. steps: we write the DSL 
6. post: where we give the mail information to send the notification whether it is success or failuer.


# Scripted Pipeline
node('MAVEN_JDK8') 
{
    stage('vcs')
    {
        git 'https://github.com/wakaleo/game-of-life.git'
    }
    stage('build') 
    {
        sh 'export PATH="/usr/lib/jvm/java-1.8.0-openjdk-amd64/bin:$PATH" && mvn package'

    }
    stage('postbuild')
    {
        archiveArtifacts artifacts: '**/target/gameoflife.war', followSymlinks: false
        junit '**/surefire-reports/TEST-*.xml'
    }
}

# Builds:
---------
There are two types of builds ona is day builds and anothr one is night builds.
1. Day Builds :
--------------
     Day Builds represent builds during active development time. During this we will have pipelines which will give feedback to developers on their commited code quality.This build will have.
build
package
unit test
Static Code Analysis
2. Night Builds:
----------------
Night Build represents the work of the team for the entire day. This build will have
build
package
unit test
Static Code Analysis
store the package into binary repository
creating/update test environments with latest packages and executing automated tests
Generate the reports which QA and management team will be interested.
---------------------------
PLUGINS TO INSTALL
---------------------------
1. To see each stage of the job or stage in the sequence that include in the pipeline we are supposed to install the (DELIVERY PIPELINE) PLUGIN. and add it to jobs. 
2. To deploy to container we need to install the plugin called (Deploy Container).


ARTIFACTORY:
-----------
The Artifactory is used to store builded package of binary repository.
There are three types of Artifactory
1. local repository: It creates artifactory in the attached artifactory (eg:Jfrog)
2. Remote repository: It stores in the external artifactory (Eg: Maven Repository,Docker Repository)
3. Virtual Repository:

# cornjobs sytnax:
------------------
* * * * *
MINUTE HOUR DOM MONTH DOW
MINUTE  Minutes within the hour (0â€“59)
HOUR    The hour of the day (0â€“23)
DOM The day of the month (1â€“31)
MONTH   The month (1â€“12)
DOW The day of the week (0â€“7) where 0 and 6 are Sunday.

Email Notifications:
-------------------
Manage Jenkins->configur system->Email Notification in this
STMP server
After everything we have an <checkbox> called <test configrution by sending the email>


pipeline {
    agent { label 'MAVEN_JDK8' }
    triggers { pollSCM ('H/30 * * * *') }
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean'], description: 'Maven Goal')
    }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/khajadevopsmarch23/game-of-life.git',
                    branch: 'declarative'
            }
        }
        stage('package') {
            tools {
                jdk 'JDK_8_UBUNTU'
            }
            steps {
                sh "mvn ${params.MAVEN_GOAL}"
            }
        }
        stage('copy build') {
            steps {
                sh 'mkdir -p /tmp/$JOB_NAME/${BUILD_ID} && cp ./gameoflife-web/target/gameoflife.war /tmp/$JOB_NAME/${BUILD_ID}/'
            }
        }
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/gameoflife.war',
                onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}


# Static code analysis:

Static code analysis is used to check the build quality of the code by staticly analysis the code.
Static Code Analysis focuses on
Coding Standards
Best Practices
Security Checks
Code Coverage
to run the static code analysis we need to install the required  <plugin> 

# Jfrog:
--------
Artifactory is used to store the artifacts in the remote repository which have binaries of the version builds and deploy the package.
the process is
Install plugin <Artifactory pulgin> after that go to <system>and store the setting.xml and pom.xml file to deploy.
In <username and password> we need to give the name that present in the <setting.xml file>.
from there we need to work on the <Build Environment> in there have <Reslove artifacts from Artifactory>

In pipeline 
the artifacty stage have 
steps in there
 rtserver(  # this is the server information

 )
 rtMavendeployer(  # this is please to deploy

 )
 rtMavenResolver(

 )




 ClassNotes Url:
 https://directdevops.blog/2023/03/04/devops-classroomnotes-04-mar-2023-2/
 https://directdevops.blog/2023/03/05/devops-classroomnotes-05-mar-2023/
 https://directdevops.blog/2023/03/05/devops-classroomnotes-05-mar-2023-2/
 https://directdevops.blog/2023/03/07/devops-classroomnotes-07-mar-2023/
 https://directdevops.blog/2023/03/08/devops-classroomnotes-08-mar-2023/
 https://directdevops.blog/2023/03/09/devops-classroomnotes-09-mar-2023/
 https://directdevops.blog/2023/03/11/devops-classroomnotes-11-mar-2023/
 https://directdevops.blog/2023/03/11/devops-classroomnotes-11-mar-2023-2/
 https://directdevops.blog/2023/03/12/devops-classroomnotes-12-mar-2023/

Jenkins Questions:

1. What is Jenkins and why is it used in DevOps?
2. Explain the key features of Jenkins.
3. What are Jenkins plugins, and how do they extend Jenkins functionality?
4. How do you install Jenkins?
5. What are the different ways to set up Jenkins?

#Jenkins Pipeline and Job Configuration:

6. What is a Jenkins Pipeline?
7. What are the differences between Declarative and Scripted Pipelines in Jenkins?
8. How do you configure a Jenkins job?
9. Explain how you would create and use Jenkinsfiles.
10. What is the difference between a Freestyle project and a Pipeline in Jenkins?
11. How do you schedule a Jenkins job?

#Jenkins Administration:

12. How do you secure Jenkins?
13. How do you manage users and roles in Jenkins?
14. Explain how to backup and restore Jenkins configurations.
15. What strategies would you use to scale Jenkins?

#Integration and Automation:

16. How do you integrate Jenkins with version control systems like Git?
17. What are some common CI/CD tools that integrate with Jenkins?
18. How do you automate tests with Jenkins?
19. Describe how to set up a continuous deployment pipeline with Jenkins.
20. How do you use Jenkins to deploy applications to different environments (e.g., dev, test, prod)?

#Troubleshooting and Optimization:

21. How do you monitor Jenkins and its jobs?
22. What are some common issues you might encounter with Jenkins and how do you resolve them?
23. How can you optimize Jenkins performance?
24. What strategies would you use to handle long-running jobs in Jenkins?
25. How do you handle failing Jenkins builds?

#Advanced Jenkins Topics:

26. Explain the use of Jenkins agents and how to configure them.
27. What is the role of Blue Ocean in Jenkins?
28. How do you use Jenkins for building Docker images?
29. Describe how you can trigger Jenkins jobs remotely.
30. How do you use Jenkins with Kubernetes for CI/CD?

#Practical and Scenario-Based Questions:

31. Describe a CI/CD pipeline you have implemented using Jenkins.
32. How do you handle secrets and credentials in Jenkins?
33. How would you migrate Jenkins jobs from one server to another?
34. Explain a situation where you improved the CI/CD process using Jenkins.
35. How do you manage dependencies in a Jenkins pipeline?