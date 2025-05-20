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
