Secure Employee Onboarding Notifier with Cross-Instance Data Transfer

Overview
This project demonstrates a secure, real-time, and automated data transfer process between two ServiceNow instances: an HR instance (source) and a Manager instance (target). The use case simulates an enterprise HR onboarding process that securely notifies the manager system when a new employee joins.

Core Objective
To automate employee onboarding by transferring necessary employee data between two instances using REST API, while ensuring data integrity, security, and reliability.

Technical Implementation

Custom Onboarding Application

* Developed a scoped application in the HR instance.
* Designed a form on a custom table (for example: x\_hr\_onboarding\_employee) with the following fields:

  * Name (Reference field pointing to sys\_user table)
  * Email (Fetched dynamically using a Client Script based on selected Name)
  * Department (Choice or Reference field)
  * Joining Date (Date type)
* Field-level validation implemented to ensure data consistency before submission.

Client Script

* Type: onChange
* Triggered on the Name field to auto-fetch the associated email from the selected user in the sys\_user table.
* Used g\_form.setValue to dynamically populate the Email field.
* Ensures dynamic data binding and reduced user errors.

REST Integration

* Created an Outbound REST Message in the HR instance.
* Authentication: Basic Auth configured with target instance credentials (username and password stored securely).
* REST Endpoint: URL of the target instance’s Scripted REST API.
* HTTP Method: POST
* HTTP Headers: Content-Type set to application/json
* JSON Payload constructed using RESTMessageV2 API inside Business Rule.

Business Rule

* Type: Before Insert
* Table: Custom onboarding table (e.g., x\_hr\_onboarding\_employee)
* Purpose: Automatically trigger the REST call once a new onboarding record is inserted.
* Logic:

  * Instantiate the RESTMessageV2 object
  * Set endpoint, headers, and body using setEndpoint(), setRequestHeader(), setRequestBody()
  * Used setStringParameterNoEscape() to prevent field encoding issues
  * Send the request using .execute()
  * Captured response using getStatusCode() and getBody()
  * Logged the success or failure using gs.info()

Script Include

* Created a reusable Script Include to construct and send REST messages
* Called from the Business Rule to maintain modular design
* Functionality:

  * Accepts GlideRecord as input
  * Constructs JSON object from record values
  * Handles all REST API configurations and response management
  * Promotes reusable and testable logic

Scripted REST API (Manager Instance)

* Created an endpoint using Scripted REST API feature
* Allowed the target instance to receive and process the JSON payload
* Used request.body.data to extract incoming parameters
* Inserted received data into the Manager's custom table (e.g., x\_mgr\_onboarding\_notify)
* Performed validation and logged outcomes

Security and Data Integrity

* Used Basic Authentication with encrypted credentials
* Validated input fields and JSON structure before sending
* Checked REST response codes and logged errors for traceability
* Data sanitization performed to avoid injection or encoding errors
* Ensured secure transfer of sensitive employee information over HTTPS

Logging and Debugging

* Used gs.info() in Business Rules and Script Include for tracking REST call success and failures
* Logged status code and response body
* Enabled “Log Transactions” for the REST Message to track activity under “Outbound HTTP Requests”

Skills Demonstrated

* REST Integration using RESTMessageV2
* Business Rules (Before Insert trigger)
* Script Includes (Modular integration logic)
* Client Scripts (onChange logic with g\_form API)
* Reference fields and dynamic field population
* Basic Authentication setup and secure transmission
* Multi-instance communication using Scripted REST APIs
* Logging, error handling, and debugging integrations

Project Outcome

* Real-world simulation of HR to Manager handoff process
* Achieved secure, real-time cross-instance integration
* Highlighted ServiceNow’s ability to act as a backend integration platform
* Reinforced best practices in REST API usage, modular scripting, and platform security

Sample images are provided Below 👇 
![Screenshot (18)](https://github.com/user-attachments/assets/34235b5f-cf3b-4ba4-baec-586e6de8f03a)
![Screenshot (14)](https://github.com/user-attachments/assets/1e43b030-4d7d-4322-a09c-0bef5d3a05d4)
![Screenshot (11)](https://github.com/user-attachments/assets/73616f03-5060-4766-a5d1-0881e13b91e0)
![Screenshot (15)](https://github.com/user-attachments/assets/203c41c0-383b-4df3-99e8-01e93ccee59a)
![Screenshot (19)](https://github.com/user-attachments/assets/5cbfef65-f358-4a98-aa73-1e99c22496a7)
![Screenshot (13)](https://github.com/user-attachments/assets/e873b84e-aaeb-47f4-afc3-af45badd28a3)
![Screenshot (12)](https://github.com/user-attachments/assets/04acabe2-d4dd-4524-96b6-c1919a477b10)
![Screenshot (16)](https://github.com/user-attachments/assets/fcfbe019-4eed-4055-8487-49d411d97548)
![Screenshot (7)](https://github.com/user-attachments/assets/d6ef44eb-7d5d-4a1a-b580-f02f9b36cfca)
![Screenshot (17)](https://github.com/user-attachments/assets/0e2f8b04-fba7-4bbf-bfb7-e274eb11fc76)
![Screenshot (9)](https://github.com/user-attachments/assets/506e7b4e-f887-412b-9011-23cd44fb62f0)
![Screenshot (8)](https://github.com/user-attachments/assets/bc838471-13bf-4a42-9f38-f456a614f029)
![Screenshot (11)](https://github.com/user-attachments/assets/d6f8f958-1fa7-4536-a3a2-9119b8e3b873)
