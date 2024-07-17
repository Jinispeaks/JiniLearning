# Azure Cloud Data engineer
 Azure synapse pipelines
 Azure data factory
 SSIS
Azure Sql Database
Key vault
logic Apps
Azure Data lake gen2
Blob storage 
Design the framework to move SSIS & SQL code to Azure cloud.
Deployed changes to various environments using Azure Dev Ops CICD


what is cloud?
managing services through intranet 


Azure Data Lake Storage (adls)
Azure Data Factory
Python
DataBricks
Azure Synapse
Power BI

1) Login into https://portal.azure.com/
2) First create azure blob storage
3) Second create container

   ## Azure Storage
    1. Azure blob (unstructured ,video,audio)
    2. Azure File ( mainly as shared location)
    3. Azure table
    4. Azure Queue (paytm,google pay ,messaging)
  
       # Tier
       1. Hot Tier ( data that is frequently used)
       2. Cold Tier ( less frequently)
       3. Archieve Tier
       


# Azure Data Factory

i) Create resource 
ii) Author Window (create pipelines)
iii)Manage Window ( to view data pipeline)






## ETL (Extract Transform Load)

### azure data pipelines
  1.Source
  2.Sink
  3. Linked Service (connection Manager)

  Source --> Linked Service---> Dataset --> Copy Activity ---> Dataset ---> Linked Service -->Sink                                                                                           The Integration Runtime (IR) is the compute infrastructure used by Azure Data Factory and Azure Synapse pipelines to provide the following data integration capabilities across different network environments
  -- azure( to transfer data between clouds)
  --self hosted
  --azure ssis 
