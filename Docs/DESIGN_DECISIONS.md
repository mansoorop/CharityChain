


**Note** - A Unique Vehicle Identification Number (VIN) is assigned for every single vehicle considered in the application.

**Note** - The application is designed for a five-member network, each participant from the Vehicle Registration Authority in a state, an Insurance company, a Pollution certifying agency, a Police department and finally a government vehicle registration authority.

1. Vehicle addressing Scheme --- Vehicle addressing scheme is the first 6 hex character is the hash of FamilyName, then the next 6 hex character is hash of the VIN and the next 58 hex character is hash of the public key of the concerned department who is initiating the transaction.
We have decided to store the details of a vehicle in the different address corresponding to the department. So each department will have details regarding the vehicle and also we get the entire details of the vehicle when queried with "Transaction family + VIN".
Of course, there are several other possibilities but we have considered the above.

Address = hash(FamilyName).slice(0,6)+hash(vin).slice(0,6)+hash(publickey of concern department).slice(0,58)

Hashing is done using SHA512.

2. We considered 2 categories of vehicles, for example, ordinary vehicles and government official vehicles.

3. About Transaction Family's --- We have 2 transaction family's, one is "Vehicle" which handles the ordinary vehicles Registrations, Insurances, Pollutions, and Police (Traffic) details.
The second transaction family is "Government" which handles the Government vehicle registrations and deletion only.

4. Node's of the network - Vehicle Registration Authorities of each state in India, Certified Insurance Agencies, Pollution Certification Agencies, and Law Enforcement Agencies.

5. Here in case of government vehicle registrations in transaction family name "Government," we have decided to send the transaction from the client side as multiple transactions in order to demonstrate that multiple transactions can be done in a batch in sawtooth. So we have sent simple string and VIN number in the first three transactions and the actual data that that is to be written in the state is sent as the fourth transaction.

6. Event's - In this application, we have used 3 events. First, sawtooth inbuild block commit event, second and third are the custom events in the Transaction Processors.

Custom Event 1: In Transaction family "Vehicle".
When an ordinary Vehicle Registration is done by the registration department if the model of the vehicle is "BMW", a custom event occurs as a notification.

Custom Event 2: In Transaction family "Government".
When a government vehicle is registered, if the department is "police", a custom event triggers.

7. Permissioning: Note that, in this application, we have designed in such a way that when the network turns up, 5 sets of public and private keys are produced for 5 participants mentioned at the beginning of this document. Here we have given commands for permission in the readme file in such a way that the first four participants (with key name  --- registration, insurance, pollution, police) have access to Transaction family "Vehicle" and the fifth participant with key name government has only access to the Transaction family "Government". 
So the highlight is, any keys other than the above four cannot access the T.P with family name "Vehicle".
In the case of the second T.P with the family name "Government", only the single government key can access it. all other keys will be denied.

Note: Even though if a user tries for malpractice and gives the private key which has no permission to do a transaction, the User Interface will give an alert for a successful transaction, but the batch will not be added due to unauthorized access and this can be seen in the terminal window.


