                        VEHICLE MANAGEMENT SYSTEM (VMS)



**Brief description:**

This is an application on private Sawtooth network that creates a distributed ledger with a decentralized platform for all Motor Vehicles Department of India. 

This connects Registration authorities, Insurance companies, Pollution certifying authorities and Police (Traffic) Department into a single platform, where they engage in the Motor Vehicles Departments functions. The advantages of this application include an immutable ledger, improved traceability, more efficiency, faster transactions, and cost-efficient.

**Description:**

VMS (Vehicle Management System) is an application designed to bring different participants that take part in the vehicle registration process into one platform under the Motor Vehicles Department.

Our current system works in such a way that each state has it's own Motor Vehicles Department and new vehicle registrations are done by the corresponding states Motor Vehicles Departments. The Registration department registers a new vehicle and stores the details in their database. After the registration process is completed the owner takes the insurance from an insurance company of his choice and the pollution certificate from a government authorized pollution certifier. Each of these parties keeps the record of function in their individual databases. Similarly, the Police (Traffic) Department keeps the history of traffic violations in their database. In our current system, these parties function as separate modules.

This project presents a design which can bring all the parties associated with a vehicle under one single platform through a network. We have chosen a private network using Hyperledger Sawtooth with devmode consensus. This blockchain network can accommodate Motor Vehicles Departments of all states and union territories of India. This network will have all Vehicle Registration Departments, government-affiliated  Insurance companies, pollution testing and certifying agencies and Police (Traffic) Departments of all states. All these above-mentioned parties will be running a validator node on the network and will have permission to access the network. Through this implementation, we will be able to record data of above-mentioned participants into a distributed ledger and we will have the advantages of improved traceability, efficiency and much more.

One of the prime factors that influenced to design such a system is to coordinate the above-mentioned participants into a single network. This will provide increased efficiency and coordination between different Motor Vehicles Departments of each state.
Consider the example in which a vehicle of Kerala registration travels to any other state, say Karnataka. The patrolling officers of Karnataka will have access only to their own states (Karnataka) Motor Vehicle Department and they can't give the clearance to the vehicle until they can find the vehicle details. Through our proposed design we can solve these issues. Any member in the network with permissions can access the blockchain and all participants will be associated with the Motor Vehicles Departments in India.

In this Proof of Concept (POC), we consider one member from each sector of participants in the network. Like selecting one single participant from the whole Motor Vehicle Registration department of a state, Insurance companies, Pollution testing and certifying agencies, Police (Traffic) departments and from Government vehicle registration department which is exclusive for registering Government vehicles only.

**System requirements:**

1. Operating system: Ubuntu 16.04
2. System RAM: 4 GB or above (recommended 8 GB)
3. Free System storage: 4 GB on /home


**Installation prerequisites:**

1. Docker must be installed in the system
2. docker compose must be installed


3. Ensure that NodeJS (version 8.15 ideally) is installed in the system. For more information about NodeJS, go to https://nodejs.org. To check if installed, open a terminal window: and give the command
   (-) node -v
4. If NodeJS is not installed, go to https://nodejs.org and download the compatible version (version 8.15) based on system OS, or in a terminal window: and give the command
   (-) sudo apt-get install -y nodejs
5. Ensure that Docker is installed. Docker is a platform for developers and system administrators to develop, ship, and run applications. For more information, go to https://www.docker.com/resources/what-container. To check if installed, in the terminal window: give the command
   (-) sudo docker --version
6. If Docker is not installed, in the terminal window:
   SET UP THE REPOSITORY
   Update the apt package index:
   (-) sudo apt-get update
   Install packages to allow apt to use a repository over HTTPS:
   (-) sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
   Add Docker’s official GPG key:
   (-) curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   Use the following command to set up the stable repository.
   (-) sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   (-)(lsb_release -cs) \
   stable"
   INSTALL DOCKER CE
   Update the apt package index.
   (-) sudo apt-get update
   Install the latest version of Docker CE.
   (-) sudo apt-get install docker-ce
   Verify that Docker CE is installed correctly by running the hello-world image.
   (-) sudo docker run hello-world
   This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits.
7. Ensure that Docker Compose is installed. Compose is a tool for defining and running multi-container Docker applications. To check if installed, in the terminal window:
   (-) sudo docker-compose --version
8. If Docker Compose is not installed, in the terminal window:
   (-) sudo apt-get update
   (-) sudo apt-get install docker-compose


**Instructions for Installation of Application:**

1. Download the folder "VMS-app"
2. Open terminal from the folder "VMS-app" and give the command 
3. (-) sudo docker-compose up
4. After running all the containers 
5.  Open another terminal from the same folder "VMS-app" and give the command 
6. (-) sudo docker exec -it validator bash
7. This will open the validator bash and we have to set the permissions in this validator bash by giving the commands below

**Permissioning commands**

8. (-) sawset proposal create --key  ~/.sawtooth/keys/my_key.priv  sawtooth.identity.allowed_keys=$(cat ~/.sawtooth/keys/my_key.pub) --url http://rest-api:8008

9. (-) sawtooth identity policy create --key /root/.sawtooth/keys/my_key.priv policy_1 "PERMIT_KEY $(cat /root/.sawtooth/keys/my_key.pub)" "PERMIT_KEY $(cat /root/.sawtooth/keys/registration.pub)" "PERMIT_KEY $(cat /root/.sawtooth/keys/insurance.pub)" "PERMIT_KEY $(cat /root/.sawtooth/keys/pollution.pub)" "PERMIT_KEY $(cat /root/.sawtooth/keys/police.pub)" --url http://rest-api:800​8 

10. Now set the role as transaction signer for Family name "Vehicle" for the keys under the policy file name policy_1

(-) sawtooth identity role create --key ~/.sawtooth/keys/my_key.priv transactor.transaction_signer.Vehicle policy_1 --url http://rest-api:8008 

11. (-) sawtooth identity policy create --key /root/.sawtooth/keys/my_key.priv policy_2 "PERMIT_KEY $(cat /root/.sawtooth/keys/my_key.pub)" "PERMIT_KEY $(cat /root/.sawtooth/keys/government.pub)" --url http://rest-api:800​8 

12. Now set the role as transaction signer for Family name "Government" for the keys under the policy file name policy_2
(-) sawtooth identity role create --key ~/.sawtooth/keys/my_key.priv transactor.transaction_signer.Government policy_2 --url http://rest-api:8008 

13. Now give the below commands to view the keys of each department. Using these private keys we can access the UI

(-) cat ~/.sawtooth/keys/registration.priv
(-) cat ~/.sawtooth/keys/insurance.priv
(-) cat ~/.sawtooth/keys/pollution.priv
(-) cat ~/.sawtooth/keys/police.priv
(-) cat ~/.sawtooth/keys/government.priv

Simply copy-paste the entire code below to get keys.
----------------------------------------
 cat ~/.sawtooth/keys/registration.priv
 cat ~/.sawtooth/keys/insurance.priv
 cat ~/.sawtooth/keys/pollution.priv
 cat ~/.sawtooth/keys/police.priv
 cat ~/.sawtooth/keys/government.priv
----------------------------------------

14. Now go to the browser and go to http://localhost:3000
15. Now you can access the application using the corresponding departments private key
16. To terminate the app execution, go to the terminal window (where docker-compose is running) and give CTRL+C
17. Wait for docker-compose to gracefully shutdown. Then: give the command
    (-) sudo docker-compose down








