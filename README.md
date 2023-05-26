# Tollbooth App
Toolbootn app hosted in Azure, following [What The Hack - Azure Serverless](https://microsoft.github.io/WhatTheHack/015-Serverless/)

# Solution Architecture
The solution begins with vehicle photos being uploaded to an Azure Storage blob storage container, as they are captured. A blob storage trigger fires on each image upload, executing the photo processing Azure Function endpoint (on the side of the diagram), which in turn sends the photo to the Cognitive Services Computer Vision API OCR service to extract the license plate data.

If processing was successful and the license plate number was returned, the function submits a new Event Grid event, along with the data, to an Event Grid topic with an event type called "savePlateData". However, if the processing was unsuccessful, the function submits an Event Grid event to the topic with an event type called "queuePlateForManualCheckup".

Two separate functions are configured to trigger when new events are added to the Event Grid topic, each filtering on a specific event type, both saving the relevant data to the appropriate Azure Cosmos DB collection for the outcome, using the Cosmos DB output binding.

A Logic App that runs on a 15-minute interval executes an Azure Function via its HTTP trigger, which is responsible for obtaining new license plate data from Cosmos DB and exporting it to a new CSV file saved to Blob storage. If no new license plate records are found to export, the Logic App sends an email notification to the Customer Service department via their Office 365 subscription.

Application Insights is used to monitor all of the Azure Functions in real-time as data is being processed through the serverless architecture. This real-time monitoring allows you to observe dynamic scaling first-hand and configure alerts when certain events take place.

- Diagram
![diagram](https://microsoft.github.io/WhatTheHack/015-Serverless/images/preferred-solution.png)

# Azure services to create the solution architecture
- Azure Functions
- Azure Cognitive Services
- Azure Event Grid
- Application Insights
- Azure Cosmos DB
- Azure Key Vault
- Logic Apps

# Prerequisites
- Laptop: Win, MacOS or Linux OR A development machine that you have administrator rights.
- [Azure account](https://azure.microsoft.com/en-ca/) with contributor level access or equivalent to create or modify resources.
- [Node.js 8+](https://www.npmjs.com/): Install latest long-term support (LTS) runtime environment for local workstation development. A package manager is also required. Node.js installs NPM in the 8.x version. The Azure SDK generally requires a minimum version of Node.js of 8.x. Azure hosting services, such as Azure App service, provides runtimes with more recent versions of Node.js. If you target a minimum of 8.x for local and remove development, your code should run successfully.
- [Visual Studio Code](https://code.visualstudio.com)
- [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) and Web jobs tools
- [.NET](https://dotnet.microsoft.com/en-us/download/dotnet/6.0) 6 or [7](https://dotnet.microsoft.com/en-us/download/dotnet/7.0) SDK
- Any extentions required by your language of choice

# Steps
1. [Setup](https://microsoft.github.io/WhatTheHack/015-Serverless/Student/Challenge-01.html)
2. [Create a Hello World Function](https://microsoft.github.io/WhatTheHack/015-Serverless/Student/Challenge-02.html)
3. [Create Resources](https://microsoft.github.io/WhatTheHack/015-Serverless/Student/Challenge-03.html)
4. [Configuration](https://microsoft.github.io/WhatTheHack/015-Serverless/Student/Challenge-04.html)
5. [Deployment](https://microsoft.github.io/WhatTheHack/015-Serverless/Student/Challenge-05.html)
6. [Create Functions in the Portal](https://microsoft.github.io/WhatTheHack/015-Serverless/Student/Challenge-06.html)
7. [Monitoring](https://microsoft.github.io/WhatTheHack/015-Serverless/Student/Challenge-07.html)
8. [Data Export Workflow](https://microsoft.github.io/WhatTheHack/015-Serverless/Student/Challenge-08.html)

- Optional steps
  1. [Scale the Cognitive Service](https://microsoft.github.io/WhatTheHack/015-Serverless/Student/Challenge-07A.html)
  2. [View Data in Cosmos DB](https://microsoft.github.io/WhatTheHack/015-Serverless/Student/Challenge-07B.html)