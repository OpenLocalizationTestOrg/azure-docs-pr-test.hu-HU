---
title: "Azure Data Lake Analytics aaaManage pythonos környezetekben |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Python toocreate egy Data Lake tárolásához fiók, valamint feladatok elküldéséhez. "
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
ms.openlocfilehash: 3c0fff155db7c4fd4e84c2562816995eb156be16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-python"></a><span data-ttu-id="5c6ba-103">Azure Data Lake Analytics használatának Python kezelése</span><span class="sxs-lookup"><span data-stu-id="5c6ba-103">Manage Azure Data Lake Analytics using Python</span></span>

## <a name="python-versions"></a><span data-ttu-id="5c6ba-104">Python-verziók</span><span class="sxs-lookup"><span data-stu-id="5c6ba-104">Python versions</span></span>

* <span data-ttu-id="5c6ba-105">A Python egy 64 bites verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-105">Use a 64-bit version of Python.</span></span>
* <span data-ttu-id="5c6ba-106">Használhatja a hello a következő címen található standard Python elosztási  **[Python.org letölti](https://www.python.org/downloads/)**.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-106">You can use hello standard Python distribution found at **[Python.org downloads](https://www.python.org/downloads/)**.</span></span> 
* <span data-ttu-id="5c6ba-107">Sok fejlesztők megkeresi azt kényelmes toouse hello  **[Anaconda Python elosztási](https://www.continuum.io/downloads)**.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-107">Many developers find it convenient toouse hello **[Anaconda Python distribution](https://www.continuum.io/downloads)**.</span></span>  
* <span data-ttu-id="5c6ba-108">Ez a cikk írásának hello szabványos Python terjesztési 3.6 verzió pythonos környezetekben</span><span class="sxs-lookup"><span data-stu-id="5c6ba-108">This article was written using Python version 3.6 from hello standard Python distribution</span></span>

## <a name="install-azure-python-sdk"></a><span data-ttu-id="5c6ba-109">Az Azure Python SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="5c6ba-109">Install Azure Python SDK</span></span>

<span data-ttu-id="5c6ba-110">Telepítse a következő modulok hello:</span><span class="sxs-lookup"><span data-stu-id="5c6ba-110">Install hello following modules:</span></span>

* <span data-ttu-id="5c6ba-111">Hello **azure-mgmt-erőforrás** modul más Azure modult tartalmaz az Active Directory stb.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-111">hello **azure-mgmt-resource** module includes other Azure modules for Active Directory, etc.</span></span>
* <span data-ttu-id="5c6ba-112">Hello **azure-mgmt-datalake-tároló** modul tartalmaz hello Azure Data Lake Store fiókkezelési műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-112">hello **azure-mgmt-datalake-store** module includes hello Azure Data Lake Store account management operations.</span></span>
* <span data-ttu-id="5c6ba-113">Hello **azure-datalake-tároló** modul hello Azure Data Lake Store fájlrendszer-műveleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-113">hello **azure-datalake-store** module includes hello Azure Data Lake Store filesystem operations.</span></span> 
* <span data-ttu-id="5c6ba-114">Hello **azure-datalake-analytics** modul hello Azure Data Lake Analytics műveleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-114">hello **azure-datalake-analytics** module includes hello Azure Data Lake Analytics operations.</span></span> 

<span data-ttu-id="5c6ba-115">Először is győződjön meg arról hogy hello legújabb `pip` hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="5c6ba-115">First, ensure you have hello latest `pip` by running hello following command:</span></span>

```
python -m pip install --upgrade pip
```

<span data-ttu-id="5c6ba-116">Ez a dokumentum használatával készült `pip version 9.0.1`.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-116">This document was written using `pip version 9.0.1`.</span></span>

<span data-ttu-id="5c6ba-117">Hello következő `pip` tooinstall hello modulok hello commandline parancsokat:</span><span class="sxs-lookup"><span data-stu-id="5c6ba-117">Use hello following `pip` commands tooinstall hello modules from hello commandline:</span></span>

```
pip install azure-mgmt-resource
pip install azure-datalake-store
pip install azure-mgmt-datalake-store
pip install azure-mgmt-datalake-analytics
```

## <a name="create-a-new-python-script"></a><span data-ttu-id="5c6ba-118">Hozzon létre egy új Python-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="5c6ba-118">Create a new Python script</span></span>

<span data-ttu-id="5c6ba-119">Illessze be a kódját hello parancsfájlba a következő hello:</span><span class="sxs-lookup"><span data-stu-id="5c6ba-119">Paste hello following code into hello script:</span></span>

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

<span data-ttu-id="5c6ba-120">Futtassa a parancsfájl tooverify adott hello modulok importálása.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-120">Run this script tooverify that hello modules can be imported.</span></span>

## <a name="authentication"></a><span data-ttu-id="5c6ba-121">Authentication</span><span class="sxs-lookup"><span data-stu-id="5c6ba-121">Authentication</span></span>

### <a name="interactive-user-authentication-with-a-pop-up"></a><span data-ttu-id="5c6ba-122">Interaktív felhasználói hitelesítéssel egy előugró ablak</span><span class="sxs-lookup"><span data-stu-id="5c6ba-122">Interactive user authentication with a pop-up</span></span>

<span data-ttu-id="5c6ba-123">Ez a metódus nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-123">This method is not supported.</span></span>

### <a name="interactive-user-authentication-with-a-device-code"></a><span data-ttu-id="5c6ba-124">Interaktív felhasználói hitelesítéssel rendelkező eszköz</span><span class="sxs-lookup"><span data-stu-id="5c6ba-124">Interactive user authentication with a device code</span></span>

```python
user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
password = getpass.getpass()
credentials = UserPassCredentials(user, password)
```

### <a name="noninteractive-authentication-with-spi-and-a-secret"></a><span data-ttu-id="5c6ba-125">Nem interaktív hitelesítés használata az SPI és a titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="5c6ba-125">Noninteractive authentication with SPI and a secret</span></span>

```python
credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')
```

### <a name="noninteractive-authentication-with-api-and-a-certificate"></a><span data-ttu-id="5c6ba-126">Az API és a tanúsítvány nem interaktív hitelesítés</span><span class="sxs-lookup"><span data-stu-id="5c6ba-126">Noninteractive authentication with API and a certificate</span></span>

<span data-ttu-id="5c6ba-127">Ez a metódus nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-127">This method is not supported.</span></span>

## <a name="common-script-variables"></a><span data-ttu-id="5c6ba-128">Általános parancsfájl-változókat</span><span class="sxs-lookup"><span data-stu-id="5c6ba-128">Common script variables</span></span>

<span data-ttu-id="5c6ba-129">Ezek a változók hello mintákat használjuk.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-129">These variables are used in hello samples.</span></span>

```python
subid= '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>' # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adla = '<Azure Data Lake Analytics Account Name>'
```

## <a name="create-hello-clients"></a><span data-ttu-id="5c6ba-130">Hello ügyfelek létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c6ba-130">Create hello clients</span></span>

```python
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient( credentials, 'azuredatalakeanalytics.net')
```

## <a name="create-an-azure-resource-group"></a><span data-ttu-id="5c6ba-131">Azure-erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c6ba-131">Create an Azure Resource Group</span></span>

```python
armGroupResult = resourceClient.resource_groups.create_or_update( rg, ResourceGroup( location=location ) )
```

## <a name="create-data-lake-analytics-account"></a><span data-ttu-id="5c6ba-132">Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c6ba-132">Create Data Lake Analytics account</span></span>

<span data-ttu-id="5c6ba-133">Először létre kell hoznia egy store-fiók.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-133">First create a store account.</span></span>

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
<span data-ttu-id="5c6ba-134">Ezután hozzon létre egy ezt a tárolót használó ADLA fiókot.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-134">Then create an ADLA account that uses that store.</span></span>

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

## <a name="submit-a-job"></a><span data-ttu-id="5c6ba-135">Feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="5c6ba-135">Submit a job</span></span>

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
    too"/data.csv"
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

## <a name="wait-for-a-job-tooend"></a><span data-ttu-id="5c6ba-136">Várjon, amíg a feladat tooend</span><span class="sxs-lookup"><span data-stu-id="5c6ba-136">Wait for a job tooend</span></span>

```python
jobResult = adlaJobClient.job.get(adla, jobId)
while(jobResult.state != JobState.ended):
    print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
    time.sleep(3)
    jobResult = adlaJobClient.job.get(adla, jobId)

print ('Job finished with result: ' + jobResult.result.value)
```

## <a name="list-pipelines-and-recurrences"></a><span data-ttu-id="5c6ba-137">Lista folyamatok és ismétlődések</span><span class="sxs-lookup"><span data-stu-id="5c6ba-137">List pipelines and recurrences</span></span>
<span data-ttu-id="5c6ba-138">Attól függően, hogy a feladatok rendelkeznek-e a folyamat vagy ismétlődési metaadatok csatolt, listázhatja folyamatok és ismétlődések.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-138">Depending whether your jobs have pipeline or recurrence metadata attached, you can list pipelines and recurrences.</span></span>

```python
pipelines = adlaJobClient.pipeline.list(adla)
for p in pipelines:
    print('Pipeline: ' + p.name + ' ' + p.pipelineId)

recurrences = adlaJobClient.recurrence.list(adla)
for r in recurrences:
    print('Recurrence: ' + r.name + ' ' + r.recurrenceId)
```

## <a name="manage-compute-policies"></a><span data-ttu-id="5c6ba-139">Számítási házirendjeinek kezelése</span><span class="sxs-lookup"><span data-stu-id="5c6ba-139">Manage compute policies</span></span>

<span data-ttu-id="5c6ba-140">hello DataLakeAnalyticsAccountManagementClient objektum biztosít hello kezelésére szolgáló módszerek számítási házirendek a Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-140">hello DataLakeAnalyticsAccountManagementClient object provides methods for managing hello compute policies for a Data Lake Analytics account.</span></span>

### <a name="list-compute-policies"></a><span data-ttu-id="5c6ba-141">Számítási házirendek felsorolása</span><span class="sxs-lookup"><span data-stu-id="5c6ba-141">List compute policies</span></span>

<span data-ttu-id="5c6ba-142">hello a következő kód lekéri a Data Lake Analytics-fiók számítási házirendek listáját.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-142">hello following code retrieves a list of compute policies for a Data Lake Analytics account.</span></span>

```python
policies = adlaAccountClient.computePolicies.listByAccount(rg, adla)
for p in policies:
    print('Name: ' + p.name + 'Type: ' + p.objectType + 'Max AUs / job: ' + p.maxDegreeOfParallelismPerJob + 'Min priority / job: ' + p.minPriorityPerJob)
```

### <a name="create-a-new-compute-policy"></a><span data-ttu-id="5c6ba-143">Hozzon létre egy új számítási házirendet</span><span class="sxs-lookup"><span data-stu-id="5c6ba-143">Create a new compute policy</span></span>

<span data-ttu-id="5c6ba-144">a következő kód hello létrehoz egy új számítási házirendet a Data Lake Analytics-fiók, beállítás hello maximális ausztráliai elérhető toohello megadott felhasználói too50, és hello minimális feladat prioritása too250.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-144">hello following code creates a new compute policy for a Data Lake Analytics account, setting hello maximum AUs available toohello specified user too50, and hello minimum job priority too250.</span></span>

```python
userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde"
newPolicyParams = ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250)
adlaAccountClient.computePolicies.createOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams)
```

## <a name="next-steps"></a><span data-ttu-id="5c6ba-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c6ba-145">Next steps</span></span>

- <span data-ttu-id="5c6ba-146">toosee hello ugyanaz az oktatóanyagot más eszközök használatával hello szeretné a hello hello lap tetején kattintson.</span><span class="sxs-lookup"><span data-stu-id="5c6ba-146">toosee hello same tutorial using other tools, click hello tab selectors on hello top of hello page.</span></span>
- <span data-ttu-id="5c6ba-147">toolearn U-SQL, lásd: [Ismerkedés az Azure Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="5c6ba-147">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
- <span data-ttu-id="5c6ba-148">Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure Portallal).</span><span class="sxs-lookup"><span data-stu-id="5c6ba-148">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>

