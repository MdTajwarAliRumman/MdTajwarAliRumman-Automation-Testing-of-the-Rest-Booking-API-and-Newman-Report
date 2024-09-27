### Rest Booking API Testing with Postman & Newman
This project provides an example of utilizing Postman for API testing, including a set of tests to verify different API endpoints.

### **Features**

- Tests for GET, POST, PUT, DELETE requests
- Collection of tests covering different API endpoints
- Environment setup for easy switching between environments
- Pre-request scripts for data setup
- Test scripts for assertions and validations

## API Documentation:
- https://documenter.getpostman.com/view/13082503/2sA2xmUAJ1
  
### **Technology used:**
- Postman
- Newman

### **Prerequisite:**
- Node Js
- Newman
- Newman Html Report Library

### **Installation**

1. Postman: If you haven't already, [download and install Postman.](https://www.postman.com/downloads/)
2. Clone the repository:

 ```console 
  git clone https://github.com/MdTajwarAliRumman/MdTajwarAliRumman-Automation-Testing-of-the-Rest-Booking-API-and-Newman-Report.git
```
3. Import the Postman collection:
    - Open Postman.
    - Click on the Import button.
    - Select the file from the repository.
4. Import the Postman environment:
    - In Postman, click on the gear icon in the top right corner.
    - Select **Import** and choose the file.
5. Newman and Report Installation Process:
    - Newman Install Command:
     ```console 
      npm install -g newman
    ```
    - Newman Html Report Install Command:
     ```console 
      npm install -g newman-reporter-htmlextra
    ```
### **Usage**
1. Select Environment:
    -   In Postman, select the appropriate environment (e.g., Development, Production) from the top-right dropdown.
3. Run Collection:
    -   Select the imported collection from the Collections sidebar.
    -   Click on the Runner button to open the collection runner.
    -   Select the desired environment.
    -   Click Start Test to run the collection.
8. View Results:
    -   Once the tests are complete, view the results in the Runner tab.
    -   Detailed test results can be viewed for each request.
  
## **Testing**

## Test Case Scenarios:

## _**1. Create New Booking**_

### Request URL: https://restful-booker.herokuapp.com/booking/
### Request Method: POST
### Pre-request Script:

```console 
     var firstName = pm.variables.replaceIn("{{$randomFirstName}}")
     console.log(firstName)
     pm.environment.set("firstName", firstName)
     
     var lastName = pm.variables.replaceIn("{{$randomLastName}}")
     console.log(lastName)
     pm.environment.set("lastName", lastName)
     
     var totalPrice = pm.variables.replaceIn("{{$randomInt}}")
     pm.environment.set("totalPrice", totalPrice)
     console.log(totalPrice)
     
     var depositPaid = pm.variables.replaceIn("{{$randomBoolean}}")
     pm.environment.set("depositPaid", depositPaid)
     console.log(depositPaid)
     
     var additionalNeeds = pm.variables.replaceIn("{{$randomProduct}}")
     pm.environment.set("additionalNeeds", additionalNeeds)
     console.log(additionalNeeds)
     
     // date checkin
     const moment = require("moment")
     const today = moment()
     pm.environment.set("checkin", today.format("YYYY-MM-DD"))
     // date checkout
     pm.environment.set("checkout", today.add(5,'d').format("YYYY-MM-DD"))
     console.log(today.format("YYYY-MM-DD"))
     
     
     // future date example
     //pm.environment.set("checkin", today.add(5,"d").add(1,"M").format("YYYY-MM-DD"))
     
     // past date example
     //pm.environment.set("checkin", today.subtract(5,"d").subtract(1,"M").format("YYYY-MM-DD"))

 ```
**Request Body:**
```console 
      {
	"firstname" : "{{firstName}}",
	"lastname" : "{{lastName}}",
	"totalprice" : {{totalPrice}},
	"depositpaid" : {{depositPaid}},
	"bookingdates" : {
    	"checkin" : "{{checkin}}",
    	"checkout" : "{{checkout}}"
	},
	"additionalneeds" : "{{additionalNeeds}}"
}
  ```
**Response Body:**
```console 
    {
    "bookingid": 2208,
    "booking": {
        "firstname": "Samantha",
        "lastname": "Schaefer",
        "totalprice": 488,
        "depositpaid": false,
        "bookingdates": {
            "checkin": "2024-09-27",
            "checkout": "2024-10-02"
        },
        "additionalneeds": "Computer"
    }
}  
  ```
 ## _**2. Get Booking Details By ID**_
### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: GET
### Post-request Script:
```console 
      var statusCode= pm.response.code
console.log(statusCode)

switch(statusCode){
    case 200:
        var jsonData = pm.response.json()

        pm.test("OK, this status code indicates the request was successful and server returned requested data.", function(){
            pm.response.to.have.status(200);
        })

        pm.test("First Name Validation", function(){
        pm.expect(jsonData.firstname).to.eql(pm.environment.get("firstName"))  
        })

        pm.test("Last Name Validation", function(){
            pm.expect(jsonData.lastname).to.eql(pm.environment.get("lastName"))
        })

        pm.test("Total Price Validation", function(){
        pm.expect(jsonData.totalprice.toString()).to.eql(pm.environment.get("totalPrice"))
        })

        pm.test("Deposit Paid Validation", function(){
            pm.expect(jsonData.depositpaid.toString()).to.eql(pm.environment.get("depositPaid"))
        })

        pm.test("Check in Validation", function(){
            pm.expect(jsonData.bookingdates.checkin).to.eql(pm.environment.get("checkin"))
        })

        pm.test("Check out Validation", function(){
            pm.expect(jsonData.bookingdates.checkout).to.eql(pm.environment.get("checkout"))
        })

        pm.test("Additional needs Validation", function(){
            pm.expect(jsonData.additionalneeds).to.eql(pm.environment.get("additionalNeeds"))
        })
    break;

    case 201:
        pm.test("Created: This status code indicates that the resource was generated by the server and the request was successful.", function(){
            pm.response.to.have.status(201);
        })   
    break;
    
    case 400:
        pm.test("Bad Request: This status code indicates that there was an invalid request.")
    break;

    case 401:
        pm.test("Unauthorized: This status code means that the request was it is not authorized to access the requested resource ")
    break;
    case 403:
        pm.test("Forbidden: This status code means that the client is authenticated but not authorized to access the requested resource ")
    break;
    case 404:
        pm.test("Not Found: This status code means that the requestd resource was not found on the server  ")
    break;

    case 500:
    pm.test("Server Error: The server encountered an unexpected condition that prohibited it from completing the request, as indicated by this generic error code.")
    break;

      break;
    case 502:
    pm.test("Bad Gateway: This status code means that an upstream server sent an incorrect answer to a server operating as a gatway or proxy.")
    break;

      break;
    case 503:
    pm.test("Server Unavailable: the server cannot process a request for a short while, it returns the status code Server Unavailable. When the server is undergoing maintenance or during times of high demand, it is frequently observed")
    break;

    default:
        pm.test("Something Went Wrong.......!!")

}
    
  ```
### Response Body:
```console 
      {
    "firstname": "Samantha",
    "lastname": "Schaefer",
    "totalprice": 488,
    "depositpaid": false,
    "bookingdates": {
        "checkin": "2024-09-27",
        "checkout": "2024-10-02"
    },
    "additionalneeds": "Computer"
}
  ```
