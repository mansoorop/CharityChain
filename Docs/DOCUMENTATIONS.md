
                                                 VEHICLE MANAGEMENT SYSTEM

**Note-Use the Correct Private Key of each Department for transactions because of Implementation of permission** 

**UI  User Guide**
1. Open http://localhost:3000 in the browser.
2. Navigate to the "Registration Department" page.
3. Input the Registration Department Private key, VIN (Unique vehicle Identification Number) and all other details in the form.
4. Submit the form.
5. Navigate to "Latest Transaction ID" page from the registration page.
6. View and copy the latest transaction Id.
7. Navigate to the "Transaction Receipts" page and paste the latest transaction Id in the form.
8. Now you can view and confirm the transaction receipt and the registration is successful.
9. Now navigate to the "Vehicle Details" page and input the "Registration Department" private key and VIN of the vehicle to view the registration details.
10. Similarly, repeat the above steps for "Insurance Department" page, "Pollution Department" page with their corresponding Private keys and Vehicle VIN.
11. Navigate to "Police Department" Page and input the private key and VIN to enter the complaint no: regarding the vehicle.
12. Navigate to the "Vehicle Details" page to view the details of the vehicle.
13. Note that there are no transaction receipts for Police Department transactions.
14. Now In case of Government vehicle registration, we have a separate Transaction Processor (Family Name-Government). Navigate to page "Government Vehicle Registration" and enter the private key of the Government Vehicle Registration Department and the VIN of vehicle.
15. After submitting the form, navigate to the Transaction Receipts Page and enter the Latest Transaction Id and the user can view the Transaction Receipt. 
16. Similarly, for viewing the Government Vehicle Details, navigate to "Government Vehicles Details" page.
17. For deleting vehicle details, navigate to the "Delete Vehicle Details" page from " Registration Department " page and enter the private key and the corresponding VIN of the vehicle whose data is to be deleted.
18. For deleting the Government vehicle registration details, navigate to "Delete Vehicle Details" page from the " Government Vehicles Registration" page and enter the private key of Government Registration Department and the corresponding VIN of the vehicle.


**Client side Transaction Flow**

1. Vehicle Registration ---- When the user enters the data in the "Registration department" page the function "registrationDepartment" in main.js file is triggered and the entered data is posted to the "/registration" in index.js file, which triggers the "addRegistration" function in UserClient.js file in which the data entered is set into a payload and the payload, inputaddresslist, outputaddresslist, familyname, familyversion, etc are passed as parameter to trigger "createTransaction" function and the transactions is made into batches and batches are finally send to the Transaction Processors via rest-api port 8008/batches. The "addRegistration" function returns a value, which is validated in "/registration" to check if registration has been completed, or alerts the user that this vehicle with VIN no. has been previously registered in the system.

2. Adding Vehicle Insurance Details ---- When the user enters a data in the "Insurance Department" page the function "insuranceDepartment" in main.js file is triggered and the entered data is posted to the "/insurance" in index.js file, which triggers the "addInsurance" Function in UserClient.js file in which the data entered is set into a payload and the payload, inputaddresslist, Outputaddresslist, familyname, familyversion, etc are passed as parameter to trigger "createTransaction" function and the transactions is made into batches and batches are finally send to the Transaction Processors via rest-api port 8008/batches. The "addInsurance" function returns a value, which is validated in "/insurance" to check if insurance has been completed or alerts the user that the same vehicle with the VIN has previously entered the insurance details into the system.
 
3. Adding Vehicle Pollution Details ---- When the user enters the data in the "Pollution Department" page the function "pollutionDepartment" in main.js file is triggered and the entered data is posted to the "/pollution" in index.js file, which triggers the "addPollution" Function in UserClient.js file in which the data entered is set into a payload and the payload, inputaddresslist, outputaddresslist, familyname, familyversion, etc are passed as parameter to trigger "createTransaction" function and the transactions is made into batches and batches are finally send to the Transaction Processors via rest-api port 8008/batches. The "addPollution" function returns a value, which is validated in "/pollution" to check if pollution has been completed, or alerts the user that the same vehicle with the VIN has previously entered the pollution details into the system.
 
4. Adding Vehicle Traffic Violation Details by Police department ---- When the user enters the data in the "Police Department" page, the function "policeDepartment" in main.js file is triggered and the entered data is posted to the "/police" in index.js file, which triggers the "addPolice" Function in UserClient.js file in which the data entered is set into a payload and the payload, inputaddresslist, outputaddresslist, familyname, familyversion, etc are passed as parameter to trigger "createTransaction" function and the transactions is made into batches and batches are finally send to the Transaction Processors via rest-api port 8008/batches. The "addPolice" function returns a value, which is validated in "/police" to check if police details have been completely entered.

5. Government Vehicle Registration ---- When the user enters the data in the "Government Registration Department" page the function "governmentVehicleDepartment" in main.js file is triggered and the entered data is posted to the "/government" in index.js file, which triggers the "addGovernmentRegistration" function in UserClient.js file in which the data entered is set into a payload and the payload, inputaddresslist, outputaddresslist, familyname, familyversion, etc are passed as parameter to trigger "makeTransaction" function and the transactions is made into batches and batches are finally send to the Transaction Processors via rest-api port 8008/batches. The "addGovernmentRegistration" function returns a value, which is validated in "/government" to check if registration is complete, or alerts the user that this Government vehicle with VIN has previously registered into the system.


 


**Transaction Processor side Transaction Flow**

1. Vehicle Registration ---- When the transaction from the client side through rest-api arrives at the Transaction Family "Vehicle", and the action is 'Add Registration' then it triggers the function "registration" in the vehicleHandler.js file. The address to which the data is to be written is generated using the VIN of the vehicle in the payload and the transaction signer public key. After that, the current state of the generated address is queried and if the state data is empty or null a receipt data is added to the transaction and then triggers the function "writeToStore" which will produce an event if the payload consists of the Vehicle/WordMatch event match string. Then again the transaction receipt data is written and finally, the data is written to the state by function 'setState'. Else if the state data is not null or empty, returns an InvalidTransaction stating the registration of the vehicle already exists.


2. Adding Vehicle Insurance Details ---- When the transaction from the client side through rest-api arrives at the Transaction Family "Vehicle", and the action is 'Add Insurance' then it triggers the function "insurance" in the vehicleHandler.js file. The address to which the data is to be written is generated using the VIN of the vehicle in the payload and the transaction signer public key. After that, the current state of the generated address is queried and if the state data is empty or null a receipt data is added to the transaction and then triggers the function "writeToStore" then again a transaction receipt data is written and finally the data is written to the state by function "setState". Else if the state data is not null or empty, it returns an InvalidTransaction stating the insurance of the vehicle already exists. 


3. Adding Vehicle Pollution Details ---- When the transaction from the client side through rest-api arrives at the Transaction Family "Vehicle", and the action is 'Add Pollution' then it triggers the function "pollution" in the vehicleHandler.js file. The address to which the data is to be written is generated using the VIN of the vehicle in the payload and the transaction signer public key. After that, the current state of the generated address is queried and if the state data is empty or null a receipt data is added to the transaction and then triggers the function "writeToStore" Then again a transaction receipt data is written and finally the data is written to the state by function "setState". Else if the state data is not null or empty, returns an InvalidTransaction stating the pollution of the vehicle already exists. 

4. Adding Vehicle Traffic Violation Details by Police Department ---- When a Transaction from the client side through rest-api arrives at the Transaction Family "Vehicle", and the action is 'Add Police' then it triggers the function "police" in the vehicleHandler.js file. The address to which the data is to be written is generated using the VIN of the vehicle in the payload and the transaction signer public key. After that triggers the function "writeToStorePolice" in which the current state of the generated address is queried and if the state is empty or null a receipt data is added to the transaction and then data is written to the state by function "setState". Else if the state is not empty, a receipt data is added to the transaction and the new data is appended to the existing data in the state and then finally the data is written in the state by function "setState". 
  


5. Government Vehicle Registration ---- When a Transaction from the client side through rest-api arrives at the Transaction Family "Government", and the action is 'Add Government Registration' then it triggers the function "registration" in the governmentHandler.js file. The address to which the data is to be written is generated using the VIN of the vehicle in the payload and the transaction signer public key. After that, a receipt data is entered in the transaction receipt and triggers the function "writeToStore" which will produce an event if the payload consists of the Government/WordMatch event match string. Then a transaction receipt data is written and finally, the data is written to the state by function setState.

**Events in Application**

Event's - In this application, we have used 3 events, one is the sawtooth inbuild block commit event, second and third are custom events in the Transaction Processors.

Custom Event 1: In Transaction family "Vehicle".
When an ordinary vehicle Registration is done by the Registration Department, if the model of the vehicle is "BMW", a custom event occurs as a notification.

Custom Event 2: In Transaction family "Government".
When a government vehicle is registered, if the department is "Police", a custom event is triggered.


