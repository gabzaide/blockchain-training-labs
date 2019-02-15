Before starting, download Postman and check all the pre-requisites from this website:

https://hyperledger.github.io/composer/latest/installing/installing-prereqs.html


Getting Started

1.	Open the terminal and paste these commands:

curl -O https://hyperledger.github.io/composer/latest/prereqs-ubuntu.sh
chmod u+x prereqs-ubuntu.sh
./prereqs-ubuntu.sh

2.	Install GoLang by following these steps: 

•	Got to ‘https://golang.org/dl/’ and download Go for Linux
•	Go to the directory where the file is located and right click inside the folder and click Open in terminal type "tar -C /usr/local -xzf go" then press tab to complete then enter it should be look like this Eg: "tar -C /usr/local -xzf go1.11.5.linux-amd64.tar.gz"	
•	Go to the system variables and add Go to the environment variables by typing these: 

	            export PATH=$PATH:/usr/local/go/bin
	            export GOPATH=$HOME/go
                export PATH=$PATH:$GOPATH/bin

3.	 Clone fabric-samples repository by following these steps:

•	Open the terminal 
•	Copy and paste these to the terminal: git clone https://github.com/hyperledger/fabric-samples.git
•	Type cd fabric-sample

4.	Download the images and binaries by using these command:

                curl -sSL http://bit.ly/2ysbOFE | bash -s -- 1.4.0

5.	 Download blockchain-training-labs repository

•	In the same directory as fabric-samples, copy and paste this to the terminal: git clone https://github.com/gabzaide/blockchain-training-labs
•	Type cd fabric-sample

6.	 Copy chaincode and supply folder to fabric-samples folder

•	Open file manager and go to the directory where blockchain-training-labs located
•	Copy chaincode folder and supply folder
•	Paste it inside the fabric-sample
•	Click merge if already exist
•	Go back to terminal 
•	Type cd supply



NOTE: Before proceeding, check if you have downloaded the required library for the chaincode and GoLang must be installed. After confirming that all things are good, type these to the terminal:

go get github.com/golang/protobuf/proto
go get github.com/hyperledger/fabric/common/attrmgr
go get github.com/pkg/errors
go get github.com/hyperledger/fabric/core/chaincode/lib/cid

•	Open file manager go to Home/go/src/github.com copy these three folders:

hyperledger
pkg
golang

paste it inside fabric-sample/chaincode

7.	 Copy and paste these commands in the terminal to start the fabric

 ./startFabric.sh
npm install
node enrollAdmin.js
node registerSupplier.js
                node registerOem.js
node registerBank.js
node app

You should see a localhost:3000 in the terminal


8.	 Check if the app is running

•	Open Postman and you should see a GET untitled request
•	Change method from GET to POST
•	Add the URL localhost:3000/invoice
•	Below the URL area, you should see Params Authroization Headers Body
•	Click headers
•	Add another key below Content-Type
•	Type user and the value as supplier
•	Next open the body tab
•	Click the x-www-form-url-encoded
•	You should see a form there and type the following: 

KEY                      	VALUE

invoicenumber                 INVOICE001
billedto                	OEM
invoicedate             	02/08/19
invoiceamount           	10000
itemdescription        	KEYBOARD
goodreceived          	False
ispaid                 	False
paidamount         	0
repaid                 	False
repaymentamount      	0
•	Click send and the result should be ‘Success’
•	Add another request GET method localhost:3000 on header add user with value of supplier
•	Click send to see your invoices in the response below

9.	 Declare goodreceived

•	Beside POST localhost:3000/invoice click the + sign
•	Change the method to PUT and localhost:3000/invoice
•	Go to header tab and add user with value of OEM
•	Next go to body x-www-form-urlencoded and add these data:

invoicenumber           INVOICE001
goodreceived              True

•	Click send and the result should be ‘Success’

10.	 Bank will pay the supplier

•	Add another request PUT method and localhost:3000/invoice
•	On header tab, add user with value of bank
•	Ow on body tab x-www-form, add these data:

invoicenumber           INVOICE001
paidamount                 9000      

	NOTE: The paid amount should be less than invoice amount

•	Click send then you should see result: Success
•	Go to GET localhost:3000 tab then hit send 
•	Check if data is updated
•	The invoice will indicate that the isPaid = true and the paidamount will be 9000 


11.	 OEM will pay the bank

•	Add another request PUT method localhost:3000/invoice
•	On header tab add user with value of oem
•	On body tab x-www-form add these data:

invoicenumber                INVOICE001
repaymentamount         11000

	NOTE: The repayment amount should be more than paidamount

•	Click send and you should see the result: Success 
•	Go to GET localhost:3000 tab then hit send then check if data is updated








12.	Check invoice audit trail

•	Add another request GET localhost:3000
•	On header add user with value of supplier
•	On body x-www-form add invoicenumber with value of INVOICE001
•	Click send you should see the respond from the server change it from HTML to JSON to see a JSON format of the response


Summary 

There are 3 users registered earlier during setup:

•	Supplier 
•	oem 
•	Bank

Those are values we used in header 
user is the key while supplier, oem ,bank is the value

There are conditions every user:

•	Supplier can only raise invoice
•	em can only change the invoice if the good is received
•	oem can only pay the bank
•	Bank can only pay the supplier



Reference:

blockchain-training-labs repository from github
https://github.com/khrandm/blockchain-training-labs

Jaymar Dingcong
Michael Forte
Rhenzo Pacho



	
