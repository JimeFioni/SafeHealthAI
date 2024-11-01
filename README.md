![Logo](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/th5xamgrr6se0x5ro4g6.png)

# SafeHealthAI
## Web application with Virtual Medical Assistant, Predictive Analysis, and Dynamic Data Protection

Virtual assistant that facilitates the management of medical information, with a focus on the protection of sensitive data (PII and PHI), predictive analysis of diagnoses and interactive dashboard for the visualization of key data.  

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/InnovationChallengeGrupo5/SafeHealthAI)

## Screenshots

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

## Tech Stack

This application utilizes the following Azure resources:

- [**Azure App Service**](https://docs.microsoft.com/azure/app-service/) to host the web app
- [**Azure Monitor**](https://docs.microsoft.com/azure/azure-monitor/) for monitoring and logging
- [**Azure Key Vault**](https://docs.microsoft.com/azure/key-vault/) for securing secrets
- [**Storage Accounts**](https://docs.microsoft.com/azure/storage/) to facilitate the secure ingestion of massive patient data
- [**Azure Functions**](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview?pivots=programming-language-python) to process sensitive patient data
- [**Azure AI Language**](https://docs.microsoft.com/azure/cognitive-services/language-service/) for automatic detection of sensitive data (PII and PHI) in medical documents
- [**Event Grid**](https://docs.microsoft.com/azure/event-grid/) for reliable event delivery at scale
- [**Azure SQL Database**](https://docs.microsoft.com/azure/azure-sql/database/sql-database-paas-overview?view=azuresql) for the storage of securely processed data
- [**Power BI Embedded**](https://azure.microsoft.com/services/power-bi-embedded) to embed visualizations of key data such as the total count and categories of sensitive data collected.
- [**Defender for Cloud**](https://docs.microsoft.com/azure/defender-for-cloud/) for unified security management and advanced threat protection
- [**Azure Policy and Compliance Center**](https://docs.microsoft.com/azure/governance/policy/) for auditing and monitoring
- [**Microsoft Entra ID**](https://azure.microsoft.com/services/active-directory) to enable single sign-on
- [**Application Insights**](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) to detect, triage, and diagnose issues in web services and applications
- [**Azure RBAC and Built-in Roles**](https://docs.microsoft.com/azure/role-based-access-control/) for role-based access control


## Architecture Diagram

Here's a high level architecture diagram that illustrates these components. These are all contained within a single [resource group](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal)

![Architecture Diagram](./docs/architecture-diagram.png)

## Demo Video

The following video shows the user interface.

https://github.com/user-attachments/assets/540766e0-5ad6-4528-b453-1796e6b629ac


## Deploy to Azure

The easiest way to deploy this app is using the [Azure Developer CLI](https://aka.ms/azd).  If you open this repo in GitHub CodeSpaces the AZD tooling is already preinstalled.

To provision and deploy:
1) Open a new terminal and do the following from root folder:
```bash
azd up
```
## Run Locally

### Pre-reqs
1) [Python 3.8+](https://www.python.org/) required 
2) [Azure Functions Core Tools](https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=v4%2Cmacos%2Ccsharp%2Cportal%2Cbash#install-the-azure-functions-core-tools)
3) [Azurite](https://github.com/Azure/Azurite)
4) Once you have your Azure subscription, run the following in a new terminal window to create all the AI Language and other resources needed:
```bash
azd provision
```

Take note of the value of `TEXT_ANALYTICS_ENDPOINT` which can be found in `./.azure/<env name from azd provision>/.env`.  It will look something like:
```bash
TEXT_ANALYTICS_ENDPOINT="https://<unique string>.cognitiveservices.azure.com/"
```

Alternatively you can [create a Language resource](https://portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) in the Azure portal to get your key and endpoint. After it deploys, click Go to resource and view the Endpoint value.

5) [Azure Storage Explorer](https://azure.microsoft.com/en-us/products/storage/storage-explorer/) or storage explorer features of [Azure Portal](https://portal.azure.com)
6) Add this `local.settings.json` file to the `./sensitive_data_processor` folder to simplify local development.  Optionally fill in the AI_URL and AI_SECRET values per step 4.  This file will be gitignored to protect secrets from committing to your repo.  
```json
{
    "IsEncrypted": false,
    "Values": {
        "AzureWebJobsStorage": "UseDevelopmentStorage=true",
        "FUNCTIONS_WORKER_RUNTIME": "python",
        "TEXT_ANALYTICS_ENDPOINT": "<insert from step 4>"
    }
}
```

### Using VS Code
1) Open the root folder in VS Code:

```bash
code .
```
2) Ensure `local.settings.json` exists already using steps above
3) Run and Debug by pressing `F5`
4) Open Storage Explorer, Storage Accounts -> Emulator -> Blob Containers -> and create a container `unprocessed-text` if it does not already exists
5) Copy any .txt document file with text into the `unprocessed-text` container

You will see AI analysis happen in the Terminal standard out.  The analysis will be saved in a .txt file in the `processed-text` blob container.

### Using Functions Core Tools CLI
0) Ensure `local.settings.json` exists already using steps above
1) Open a new terminal and do the following:

```bash
cd sensitive_data_processor
func start
```
2) Open Storage Explorer, Storage Accounts -> Emulator -> Blob Containers -> and create a container `test-samples-trigger` if it does not already exists
3) Copy any .txt document file with text into the `test-samples-trigger` container

You will see AI analysis happen in the Terminal standard out.  The analysis will be saved in a .txt file in the `test-samples-output` blob container.

## Contributing

Contributions are always welcome!

See [CONTRIBUTING.md](CONTRIBUTING.md) for ways to get started.

Please adhere to this project's [code of conduct](.github\CODE_OF_CONDUCT.md).

## License

[MIT](LICENSE)

## Authors

- [Noelia Altamirano](https://www.github.com/noelia-alt)
- [Freddy Pinto](https://www.github.com/FreddyPinto)
- [Jimena Fioni](https://www.github.com/JimeFioni)
- [Francisco Sanchez](https://www.github.com/fjsanchezm)
