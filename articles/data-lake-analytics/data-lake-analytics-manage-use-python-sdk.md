---
title: "Azure Data Lake Analytics használatának Python kezelése |} Microsoft Docs"
description: "Ebből a cikkből megtudhatja, hogyan használhatja a Pythont Data Lake Store-fiók létrehozásához és feladatok elküldéséhez. "
services: data-lake-analytics
documentationcenter: 
author: matt1883
manager: jhubbard
editor: cgronlun
ms.assetid: d4213a19-4d0f-49c9-871c-9cd6ed7cf731
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: saveenr
ms.openlocfilehash: 31326a32f8748e6cfb8bfe24cda46c511ab59352
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="manage-azure-data-lake-analytics-using-python"></a><span data-ttu-id="26977-103">Azure Data Lake Analytics használatának Python kezelése</span><span class="sxs-lookup"><span data-stu-id="26977-103">Manage Azure Data Lake Analytics using Python</span></span>

## <a name="python-versions"></a><span data-ttu-id="26977-104">Python-verziók</span><span class="sxs-lookup"><span data-stu-id="26977-104">Python versions</span></span>

* <span data-ttu-id="26977-105">A Python egy 64 bites verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="26977-105">Use a 64-bit version of Python.</span></span>
* <span data-ttu-id="26977-106">Használhatja a Python elosztási a következő címen található standard  **[Python.org letölti](https://www.python.org/downloads/)**.</span><span class="sxs-lookup"><span data-stu-id="26977-106">You can use the standard Python distribution found at **[Python.org downloads](https://www.python.org/downloads/)**.</span></span> 
* <span data-ttu-id="26977-107">Sok fejlesztők találhatja használja a  **[Anaconda Python elosztási](https://www.continuum.io/downloads)**.</span><span class="sxs-lookup"><span data-stu-id="26977-107">Many developers find it convenient to use the **[Anaconda Python distribution](https://www.continuum.io/downloads)**.</span></span>  
* <span data-ttu-id="26977-108">Ez a cikk írásának szabványos Python elosztási 3.6 verzió pythonos környezetekben</span><span class="sxs-lookup"><span data-stu-id="26977-108">This article was written using Python version 3.6 from the standard Python distribution</span></span>

## <a name="install-azure-python-sdk"></a><span data-ttu-id="26977-109">Az Azure Python SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="26977-109">Install Azure Python SDK</span></span>

<span data-ttu-id="26977-110">A következő modulok telepítése:</span><span class="sxs-lookup"><span data-stu-id="26977-110">Install the following modules:</span></span>

* <span data-ttu-id="26977-111">A **azure-mgmt-erőforrás** modul más Azure modult tartalmaz az Active Directory stb.</span><span class="sxs-lookup"><span data-stu-id="26977-111">The **azure-mgmt-resource** module includes other Azure modules for Active Directory, etc.</span></span>
* <span data-ttu-id="26977-112">A **azure-mgmt-datalake-tároló** modul tartalmazza az Azure Data Lake Store fiókkezelési műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="26977-112">The **azure-mgmt-datalake-store** module includes the Azure Data Lake Store account management operations.</span></span>
* <span data-ttu-id="26977-113">A **azure-datalake-tároló** modul tartalmazza az Azure Data Lake Store fájlrendszer-műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="26977-113">The **azure-datalake-store** module includes the Azure Data Lake Store filesystem operations.</span></span> 
* <span data-ttu-id="26977-114">A **azure-datalake-analytics** modul tartalmazza az Azure Data Lake Analytics műveleteket.</span><span class="sxs-lookup"><span data-stu-id="26977-114">The **azure-datalake-analytics** module includes the Azure Data Lake Analytics operations.</span></span> 

<span data-ttu-id="26977-115">Először ellenőrizze, hogy a legújabb `pip` a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="26977-115">First, ensure you have the latest `pip` by running the following command:</span></span>

```
python -m pip install --upgrade pip
```

<span data-ttu-id="26977-116">Ez a dokumentum használatával készült `pip version 9.0.1`.</span><span class="sxs-lookup"><span data-stu-id="26977-116">This document was written using `pip version 9.0.1`.</span></span>

<span data-ttu-id="26977-117">Használja a következő `pip` parancsok a modulok telepítése a parancssor:</span><span class="sxs-lookup"><span data-stu-id="26977-117">Use the following `pip` commands to install the modules from the commandline:</span></span>

```
pip install azure-mgmt-resource
pip install azure-datalake-store
pip install azure-mgmt-datalake-store
pip install azure-mgmt-datalake-analytics
```

## <a name="create-a-new-python-script"></a><span data-ttu-id="26977-118">Hozzon létre egy új Python-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="26977-118">Create a new Python script</span></span>

<span data-ttu-id="26977-119">Illessze be az alábbi kódot a parancsfájlba:</span><span class="sxs-lookup"><span data-stu-id="26977-119">Paste the following code into the script:</span></span>

```python
## Use this only for Azure AD service-to-service authentication
#from azure.common.credentials import ServicePrincipalCredentials

## Use this only for Azure AD end-user authentication
#from azure.common.credentials import UserPassCredentials

## Required for Azure Resource Manager
from azure.mgmt.resource.resources import ResourceManagementClient
from azure.mgmt.resource.resources.models import ResourceGroup

## Required for Azure Data Lake Store account management
from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
from azure.mgmt.datalake.store.models import DataLakeStoreAccount

## Required for Azure Data Lake Store filesystem management
from azure.datalake.store import core, lib, multithread

## Required for Azure Data Lake Analytics account management
from azure.mgmt.datalake.analytics.account import DataLakeAnalyticsAccountManagementClient
from azure.mgmt.datalake.analytics.account.models import DataLakeAnalyticsAccount, DataLakeStoreAccountInfo

## Required for Azure Data Lake Analytics job management
from azure.mgmt.datalake.analytics.job import DataLakeAnalyticsJobManagementClient
from azure.mgmt.datalake.analytics.job.models import JobInformation, JobState, USqlJobProperties

## Required for Azure Data Lake Analytics catalog management
from azure.mgmt.datalake.analytics.catalog import DataLakeAnalyticsCatalogManagementClient

## Use these as needed for your application
import logging, getpass, pprint, uuid, time
```

<span data-ttu-id="26977-120">Győződjön meg arról, hogy a modulok importálása a parancsfájl futtatásához.</span><span class="sxs-lookup"><span data-stu-id="26977-120">Run this script to verify that the modules can be imported.</span></span>

## <a name="authentication"></a><span data-ttu-id="26977-121">Authentication</span><span class="sxs-lookup"><span data-stu-id="26977-121">Authentication</span></span>

### <a name="interactive-user-authentication-with-a-pop-up"></a><span data-ttu-id="26977-122">Interaktív felhasználói hitelesítéssel egy előugró ablak</span><span class="sxs-lookup"><span data-stu-id="26977-122">Interactive user authentication with a pop-up</span></span>

<span data-ttu-id="26977-123">Ez a metódus nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="26977-123">This method is not supported.</span></span>

### <a name="interactive-user-authentication-with-a-device-code"></a><span data-ttu-id="26977-124">Interaktív felhasználói hitelesítéssel rendelkező eszköz</span><span class="sxs-lookup"><span data-stu-id="26977-124">Interactive user authentication with a device code</span></span>

```python
user = input('Enter the user to authenticate with that has permission to subscription: ')
password = getpass.getpass()
credentials = UserPassCredentials(user, password)
```

### <a name="noninteractive-authentication-with-spi-and-a-secret"></a><span data-ttu-id="26977-125">Nem interaktív hitelesítés használata az SPI és a titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="26977-125">Noninteractive authentication with SPI and a secret</span></span>

```python
credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')
```

### <a name="noninteractive-authentication-with-api-and-a-certificate"></a><span data-ttu-id="26977-126">Az API és a tanúsítvány nem interaktív hitelesítés</span><span class="sxs-lookup"><span data-stu-id="26977-126">Noninteractive authentication with API and a certificate</span></span>

<span data-ttu-id="26977-127">Ez a metódus nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="26977-127">This method is not supported.</span></span>

## <a name="common-script-variables"></a><span data-ttu-id="26977-128">Általános parancsfájl-változókat</span><span class="sxs-lookup"><span data-stu-id="26977-128">Common script variables</span></span>

<span data-ttu-id="26977-129">Ezek a változók a mintában használt.</span><span class="sxs-lookup"><span data-stu-id="26977-129">These variables are used in the samples.</span></span>

```python
subid= '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>' # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adla = '<Azure Data Lake Analytics Account Name>'
```

## <a name="create-the-clients"></a><span data-ttu-id="26977-130">Az ügyfelek létrehozása</span><span class="sxs-lookup"><span data-stu-id="26977-130">Create the clients</span></span>

```python
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient( credentials, 'azuredatalakeanalytics.net')
```

## <a name="create-an-azure-resource-group"></a><span data-ttu-id="26977-131">Azure-erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="26977-131">Create an Azure Resource Group</span></span>

```python
armGroupResult = resourceClient.resource_groups.create_or_update( rg, ResourceGroup( location=location ) )
```

## <a name="create-data-lake-analytics-account"></a><span data-ttu-id="26977-132">Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="26977-132">Create Data Lake Analytics account</span></span>

<span data-ttu-id="26977-133">Először létre kell hoznia egy store-fiók.</span><span class="sxs-lookup"><span data-stu-id="26977-133">First create a store account.</span></span>

```python
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adls)]
    )
).wait()
```
<span data-ttu-id="26977-134">Ezután hozzon létre egy ezt a tárolót használó ADLA fiókot.</span><span class="sxs-lookup"><span data-stu-id="26977-134">Then create an ADLA account that uses that store.</span></span>

```python
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adls)]
    )
).wait()
```

## <a name="submit-a-job"></a><span data-ttu-id="26977-135">Feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="26977-135">Submit a job</span></span>

```python
script = """
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"""

jobId = str(uuid.uuid4())
jobResult = adlaJobClient.job.create(
    adla,
    jobId,
    JobInformation(
        name='Sample Job',
        type='USql',
        properties=USqlJobProperties(script=script)
    )
)
```

## <a name="wait-for-a-job-to-end"></a><span data-ttu-id="26977-136">Várjon, amíg a feladat befejezéséhez</span><span class="sxs-lookup"><span data-stu-id="26977-136">Wait for a job to end</span></span>

```python
jobResult = adlaJobClient.job.get(adla, jobId)
while(jobResult.state != JobState.ended):
    print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
    time.sleep(3)
    jobResult = adlaJobClient.job.get(adla, jobId)

print ('Job finished with result: ' + jobResult.result.value)
```

## <a name="list-pipelines-and-recurrences"></a><span data-ttu-id="26977-137">Lista folyamatok és ismétlődések</span><span class="sxs-lookup"><span data-stu-id="26977-137">List pipelines and recurrences</span></span>
<span data-ttu-id="26977-138">Attól függően, hogy a feladatok rendelkeznek-e a folyamat vagy ismétlődési metaadatok csatolt, listázhatja folyamatok és ismétlődések.</span><span class="sxs-lookup"><span data-stu-id="26977-138">Depending whether your jobs have pipeline or recurrence metadata attached, you can list pipelines and recurrences.</span></span>

```python
pipelines = adlaJobClient.pipeline.list(adla)
for p in pipelines:
    print('Pipeline: ' + p.name + ' ' + p.pipelineId)

recurrences = adlaJobClient.recurrence.list(adla)
for r in recurrences:
    print('Recurrence: ' + r.name + ' ' + r.recurrenceId)
```

## <a name="manage-compute-policies"></a><span data-ttu-id="26977-139">Számítási házirendjeinek kezelése</span><span class="sxs-lookup"><span data-stu-id="26977-139">Manage compute policies</span></span>

<span data-ttu-id="26977-140">A DataLakeAnalyticsAccountManagementClient objektum a számítási házirendek előnyeit a Data Lake Analytics-fiók kezelésére szolgáló módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="26977-140">The DataLakeAnalyticsAccountManagementClient object provides methods for managing the compute policies for a Data Lake Analytics account.</span></span>

### <a name="list-compute-policies"></a><span data-ttu-id="26977-141">Számítási házirendek felsorolása</span><span class="sxs-lookup"><span data-stu-id="26977-141">List compute policies</span></span>

<span data-ttu-id="26977-142">A következő kód lekéri a Data Lake Analytics-fiók számítási házirendek listáját.</span><span class="sxs-lookup"><span data-stu-id="26977-142">The following code retrieves a list of compute policies for a Data Lake Analytics account.</span></span>

```python
policies = adlaAccountClient.computePolicies.listByAccount(rg, adla)
for p in policies:
    print('Name: ' + p.name + 'Type: ' + p.objectType + 'Max AUs / job: ' + p.maxDegreeOfParallelismPerJob + 'Min priority / job: ' + p.minPriorityPerJob)
```

### <a name="create-a-new-compute-policy"></a><span data-ttu-id="26977-143">Hozzon létre egy új számítási házirendet</span><span class="sxs-lookup"><span data-stu-id="26977-143">Create a new compute policy</span></span>

<span data-ttu-id="26977-144">Az alábbi kód létrehoz egy új számítási házirendet Data Lake Analytics-fiók esetén a rendelkezésre álló maximális ausztráliai beállítása a megadott felhasználó 50 és 250 minimális feladat prioritása.</span><span class="sxs-lookup"><span data-stu-id="26977-144">The following code creates a new compute policy for a Data Lake Analytics account, setting the maximum AUs available to the specified user to 50, and the minimum job priority to 250.</span></span>

```python
userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde"
newPolicyParams = ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250)
adlaAccountClient.computePolicies.createOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams)
```

## <a name="next-steps"></a><span data-ttu-id="26977-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="26977-145">Next steps</span></span>

- <span data-ttu-id="26977-146">Ha ugyanezt az oktatóanyagot más eszközök használatával szeretné megtekinteni, kattintson az oldal tetején található lapválasztókra.</span><span class="sxs-lookup"><span data-stu-id="26977-146">To see the same tutorial using other tools, click the tab selectors on the top of the page.</span></span>
- <span data-ttu-id="26977-147">A U-SQL nyelv megismerése: [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md) (Ismerkedés az Azure Data Lake Analytics U-SQL nyelvével).</span><span class="sxs-lookup"><span data-stu-id="26977-147">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
- <span data-ttu-id="26977-148">Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure Portallal).</span><span class="sxs-lookup"><span data-stu-id="26977-148">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>

