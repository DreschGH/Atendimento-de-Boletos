# Invoice-Processing
Power Apps and Power Automate solution that aims to improve control over the invoice email inbox processing.

--- 
# Solution Components
This project consists of two groups of solutions:
- Power Apps application for managing requests and services
- Power Automate automations to manage external customer contacts

## Power Apps Application
### Interface:
The application features a standardized interface, with theme control (light/dark), collapsible side menu, screen selection indicator, and a text box for searching across screens.
#### Open Side Menu (Light Theme)
<img width="1489" height="828" alt="image" src="https://github.com/user-attachments/assets/b56a3aa1-80b8-4773-80b3-a08fb7a85ae2" />

#### Open Side Menu (Dark Theme)
<img width="1515" height="844" alt="image" src="https://github.com/user-attachments/assets/8fd02a52-a077-4d67-af53-039c7401e5a4" />


Power Apps application with 3 screens:
### Invoice Control:
<img width="1426" height="824" alt="image" src="https://github.com/user-attachments/assets/e2be4c95-d4b3-49e9-9e6a-e5ca3ea8e8c5" />
Central panel of the application where the main activity of the service process will be executed. The panel features two cards indicating the volume of pending items and recent services, an interactive table, a button for predefined filtering, and a button that activates a side panel for additional filters.

#### Additional Filters Panel:
  
<img width="1417" height="801" alt="image" src="https://github.com/user-attachments/assets/c45c4b25-f8c9-4dca-88db-b6f88f15d0cc" />

#### Interactive Table

<img width="880" height="453" alt="image" src="https://github.com/user-attachments/assets/cf990f15-07b4-4aec-a85b-c80c38691575" />

The interactive table is a composite object made up of a series of elements (Containers, Labels, Galleries, and Buttons) that together function as a unified table with click interaction capability:

<img width="320" height="616" alt="image" src="https://github.com/user-attachments/assets/2e46bef5-8077-4b77-8050-ae40fb9dbdb3" />

Interacting with the table collects the selected item via Set() and opens a modal panel that displays the most important information related to the selected request:
 - Request data
 - Service Timeline
 - Available customer data
 - Documents attached to the request
 - History of emails sent to the invoice inbox
 - Action options for the request

   <img width="718" height="523" alt="image" src="https://github.com/user-attachments/assets/11cd4b63-ac58-4377-a6c6-793df28342dd" />

Selecting the "Finalize Service" action will open a new window, with a text box for the analyst to record the service response and an attachment control for the analyst to attach necessary documents. Clicking "Send" will finalize the service process and send the response to the customer via Outlook, through the defined outgoing email inbox.

<img width="651" height="464" alt="image" src="https://github.com/user-attachments/assets/0c886c11-d900-4db6-8f31-16c5339c44f7" />

--- 
## Control Panel

It is a set of two screens, in the sidebar navigation, that function as an observability center for the processes involved in the application, providing the visualization of indicators and indirect interaction with some management mechanics of the available attendants:

<img width="260" height="135" alt="image" src="https://github.com/user-attachments/assets/dd772b72-1921-462c-a515-1563389aabfa" />

### Control Panel

The "Control Panel" page has a more general characteristic, providing indicators of the service process as a whole. The charts use ChartJS, allowing for more flexible customization and a more pleasant user experience than the standard Power Apps chart controls.
<img width="1512" height="841" alt="image" src="https://github.com/user-attachments/assets/4a9d1410-feff-4c30-94f7-e6ad7e4db098" />

The calculation processes for determining the indicators use temporary variables via 'With()' to avoid unwanted impacts on filtering processes due to delegations resulting from aggregation calculations such as 'AVERAGE()', 'COUNT()', etc. Thus, the calculation and grouping processes execute more optimally and without unforeseen events.

<img width="422" height="740" alt="image" src="https://github.com/user-attachments/assets/56e6d0c3-e5ad-422a-b7e2-febc49b5b267" />

### Attendant Management

The Attendant Management page is relatively similar to the Control Panel page, but with a greater focus on analyzing the indicators of each attendant, as well as the ability to define whether an attendant is, or is not, available to receive new service requests, in addition to adding new attendants to the service list.

<img width="1264" height="712" alt="image" src="https://github.com/user-attachments/assets/8f95bfa6-15ff-4ec3-924d-2b77664c9bcc" />

On this screen, it is possible to filter the indicators for each attendant by selecting the attendant in the registered attendants table:

<img width="1377" height="720" alt="image" src="https://github.com/user-attachments/assets/b309eb28-0cec-465b-903a-86d19d3e4a80" />

For the most part, activities within this application are recorded through logs, collected during user interactions with the application.

--- 
## Automations

This project's Automations use Power Automate as a central hub for directing HTML pages via a logic similar to RESTful API servers, manipulating endpoints with custom methods, paths, queries, and parameters. Power Automate handles incoming inputs from external agents, validates communications, returns responses to the client, tracks interactions, and records the results for eventual internal handling, in a way that optimizes and centralizes the customer experience, facilitating the process of requesting more personalized services.

### RPA Processing Receipt and Customer Notification
After the automated process that extracts customer invoices from our invoice platform, an RPA records the occurrence in a SharePoint list. This process initiates a flow that obtains this information from the list and sends an email with a personalized HTML body, containing the result of the RPA service and, at the end, two buttons with the options 'I was served' and 'I was not served'.
<img width="650" height="896" alt="image" src="https://github.com/user-attachments/assets/88e604e5-eeba-4dee-97e1-450e93a37bd3" />


### Positive Response Receipt
If the customer selects the "I was served" option, Outlook will redirect them to a GET method endpoint that activates a flow in Power Automate. The Endpoint returns an HTML that informs the customer that the response has been received and thanks them for interacting with the email.
<img width="678" height="739" alt="image" src="https://github.com/user-attachments/assets/f2ba422a-d1c3-40ff-ba0f-2ccec4f42770" />
After this, the flow activated by the call records in the SharePoint list that the item was served by the automated invoice extraction process.

### Negative Response Receipt
If the customer selects the "I was not served" option, Outlook will redirect them to a GET method endpoint that activates a flow in Power Automate. This Endpoint returns an HTML that contains a text box where the customer can detail their need. Within this HTML, there is a script that sends a POST to another endpoint, whose function is to register the customer's demand along with the corresponding record in SharePoint. This process performs a validation to assess whether the request is available for alteration and also whether the user sending the information is the same as the internal records.

<img width="669" height="883" alt="image" src="https://github.com/user-attachments/assets/8778611e-193f-4669-b0d5-942d484a695d" />


### Receipt of Customer Need Details
Upon receiving customer details, if validated by the request receipt evaluation process, the attendant definition process begins. This captures the last attendant to receive a demand, finds the next attendant in the list, and defines them as the attendant for the received demand. If there is no "next" attendant, the first attendant in the list of available attendants receives the demand.

### Sending Analyst's Response to Customer
When the attendant sends the information within the service application, the record in the SharePoint list is updated and receives the status "Served". At this moment, a flow is activated to send the notification to the customer via Outlook. The analyst's opinion is sent by email, embedded in an HTML that, in addition to the analysis result, contains a service evaluation menu, allowing the customer to provide feedback on the quality of the channel's service.

<img width="632" height="642" alt="image" src="https://github.com/user-attachments/assets/8569b609-2355-4a59-8b56-f7f9142fa7e9" />


### Receipt of Service Evaluation
Finally, if a customer selects one of the service evaluation options, the HTML will redirect them to an endpoint that will return a thank you message for the feedback and, on the backend, record the evaluation result within the item in SharePoint.
<img width="650" height="982" alt="image" src="https://github.com/user-attachments/assets/4f680365-fc4a-459e-9677-4949e5f5c7d9" />







