API Automation Testing Strategy

Author: Vinothkumar

Overview
This project developed with API automation RESTful API testing for the reqres.in endpoints using RestAssured — a hosted REST API for simulating user operations. It covers Create, Update, and Delete operations with both positive and negative test cases, integrated into a CI/CD pipeline and enhanced with Extent Reports and TestNG reports.
It validates multiple HTTP methods across functional flows with both positive and negative test cases.

Tech Stack
- Java 17
- RestAssured 5.5.1
- TestNG 7.7.0
- Extent Reports 5.0.9
- Maven
- GitHub Actions (CI/CD)


APIAutomationTask/
├── pom.xml
├── testng.xml
├── src/
│   ├── main/java/
│   └── test/java/
│       └── tests/
│           └── APITask.java
├── reports/
│   ├── extent-report.html
│   └── testng-results.xml

Test Flow Design
Each test method follows a consistent pattern:
- Setup: Request specification built dynamically via a utility (ApiUtils) including base URI, headers, and payload.
- Execution: API invocation via RestAssured.
- Assertion: Validation of status codes, response body structure, and data.
- Reporting: Logging every step using Extent Reports for clarity and traceability.

Test flows are grouped by HTTP operation: | Operation | Test Cases | 
| GET     | getUserName_Positive, getUserName_Negative, getUser_InvalidParam | 
| POST    | createUser_Positive, createUser_Negative_EmptyPayload, createUser_MalformedJson, createUser_MissingApiKey | 
| PUT     | updateUser_Positive, updateUser_Negative_InvalidUser, updateUser_EmptyBody | 
| DELETE  | deleteUser_Positive, deleteUser_Negative_InvalidUser, deleteUser_InvalidMethod |

Reliability & Maintainability
To ensure tests are scalable and reliable:
- Modular Design: Used utility classes (ApiUtils, ExtentManager) for base setup and payload creation.
- Data Reusability: Payloads are generated dynamically via parameters.
- Consistent Logging: Integrated Extent Reports to visualize execution status and responses.
- Error Coverage: Designed tests with malformed JSON, invalid parameters, and missing headers.

Extent Report Strategy
- Each test is logged with execution steps, raw API response, and assertion results.
- Reporting is initialized using ExtentManager.createInstance() and flushed in @AfterSuite.
- Test method names dynamically populate the report via reflection (ITestResult.getMethod().getMethodName()).


Challenges & Solutions
| Challenge | How It Was Solved | 
| NullPointerException on ExtentTest logging | Ensured test object was properly initialized in @BeforeMethod using ITestResult. | 
| Method.getName() not resolved | Replaced with TestNG’s ITestResult.getMethod().getMethodName() for reliability. | 
| RestAssured response chain errors | Verified correct import of io.restassured.response.Response and proper response typing. | 
| Handling unsupported API behavior | Created tests that adapt to ReqRes’s non-strict behavior (e.g., PUT on nonexistent user still returns 200). | 


Test Coverage
| Operation | Endpoint | Test Type | 
| Create User | POST /api/users | Positive & Negative | 
| Update User | PUT /api/users/{id} | Positive & Negative | 
| Delete User | DELETE /api/users/{id} | Positive & Negative | 


How to run locally
mvn clean test


CI/CD Integration
Tests are automatically triggered via GitHub Actions on:
- Code push to main
- Scheduled daily runs (cron)
- Manual dispatch

Workflow file: .github/workflows/api-tests.yml

Reporting
- Extent Report: Generated at reports/extent-report.html
- TestNG Report: Available in test-output/ folder

***************************************************************************END******************************************************************************
















 
