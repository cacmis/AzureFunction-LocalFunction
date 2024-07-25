# Create Azure Function CLI

1. Install the core tools packages
```bash
    brew tap azure/functions
    brew install azure-functions-core-tools@4
    # if upgrading on a machine that has 2.x or 3.x installed:
    brew link --overwrite azure-functions-core-tools@4
```


## Create a local function project

1. Run the command func init to create functions project and navigate into the project folder.
``` bash
    func init LocalFunctionProj --worker-runtime dotnet-isolated --target-framework net8.0 

    cd LocalFunctionProj
```
2. Add a function to your project by using the command `func new`
``` bash
    func new --name HttpExample --template "HTTP trigger" --authlevel "anonymous"
```

3. Run the function locally `func start`. Toward the end of the output
```
 ...

 Now listening on: http://0.0.0.0:7071
 Application started. Press Ctrl+C to shut down.

 Http Functions:

         HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
 ...

```

## Create supporting Azure resources for your function


1. Login in azure `az login`

2. Create a resource group 

        az group create --name AzureFunctionsQuickstart-rg --location eastus
3. Create a storage account

        az storage account create --name cacmissa --location eastus --resource-group AzureFunctionsQuickstart-rg --sku Standard_LRS --allow-blob-public-access false

4. Create the function app in Azure

        az functionapp create --resource-group AzureFunctionsQuickstart-rg --consumption-plan-location eastus --runtime dotnet-isolated --functions-version 4 --name LocalFunctionCacmis --storage-account cacmissa

5. Deploy the function project to Azure

        func azure functionapp publish LocalFunctionCacmis


6. Invoke the function on Azure

        func azure functionapp logstream LocalFunctionCacmis
