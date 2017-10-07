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
# <a name="manage-azure-data-lake-analytics-using-python"></a>Azure Data Lake Analytics használatának Python kezelése

## <a name="python-versions"></a>Python-verziók

* A Python egy 64 bites verzióját használja.
* Használhatja a hello a következő címen található standard Python elosztási  **[Python.org letölti](https://www.python.org/downloads/)**. 
* Sok fejlesztők megkeresi azt kényelmes toouse hello  **[Anaconda Python elosztási](https://www.continuum.io/downloads)**.  
* Ez a cikk írásának hello szabványos Python terjesztési 3.6 verzió pythonos környezetekben

## <a name="install-azure-python-sdk"></a>Az Azure Python SDK telepítése

Telepítse a következő modulok hello:

* Hello **azure-mgmt-erőforrás** modul más Azure modult tartalmaz az Active Directory stb.
* Hello **azure-mgmt-datalake-tároló** modul tartalmaz hello Azure Data Lake Store fiókkezelési műveletekhez.
* Hello **azure-datalake-tároló** modul hello Azure Data Lake Store fájlrendszer-műveleteket tartalmaz. 
* Hello **azure-datalake-analytics** modul hello Azure Data Lake Analytics műveleteket tartalmaz. 

Először is győződjön meg arról hogy hello legújabb `pip` hello a következő parancs futtatásával:

```
python -m pip install --upgrade pip
```

Ez a dokumentum használatával készült `pip version 9.0.1`.

Hello következő `pip` tooinstall hello modulok hello commandline parancsokat:

```
pip install azure-mgmt-resource
pip install azure-datalake-store
pip install azure-mgmt-datalake-store
pip install azure-mgmt-datalake-analytics
```

## <a name="create-a-new-python-script"></a>Hozzon létre egy új Python-parancsfájl

Illessze be a kódját hello parancsfájlba a következő hello:

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

Futtassa a parancsfájl tooverify adott hello modulok importálása.

## <a name="authentication"></a>Authentication

### <a name="interactive-user-authentication-with-a-pop-up"></a>Interaktív felhasználói hitelesítéssel egy előugró ablak

Ez a metódus nem támogatott.

### <a name="interactive-user-authentication-with-a-device-code"></a>Interaktív felhasználói hitelesítéssel rendelkező eszköz

```python
user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
password = getpass.getpass()
credentials = UserPassCredentials(user, password)
```

### <a name="noninteractive-authentication-with-spi-and-a-secret"></a>Nem interaktív hitelesítés használata az SPI és a titkos kulcs

```python
credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')
```

### <a name="noninteractive-authentication-with-api-and-a-certificate"></a>Az API és a tanúsítvány nem interaktív hitelesítés

Ez a metódus nem támogatott.

## <a name="common-script-variables"></a>Általános parancsfájl-változókat

Ezek a változók hello mintákat használjuk.

```python
subid= '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>' # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adla = '<Azure Data Lake Analytics Account Name>'
```

## <a name="create-hello-clients"></a>Hello ügyfelek létrehozása

```python
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient( credentials, 'azuredatalakeanalytics.net')
```

## <a name="create-an-azure-resource-group"></a>Azure-erőforráscsoport létrehozása

```python
armGroupResult = resourceClient.resource_groups.create_or_update( rg, ResourceGroup( location=location ) )
```

## <a name="create-data-lake-analytics-account"></a>Data Lake Analytics-fiók létrehozása

Először létre kell hoznia egy store-fiók.

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
Ezután hozzon létre egy ezt a tárolót használó ADLA fiókot.

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

## <a name="submit-a-job"></a>Feladat elküldése

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

## <a name="wait-for-a-job-tooend"></a>Várjon, amíg a feladat tooend

```python
jobResult = adlaJobClient.job.get(adla, jobId)
while(jobResult.state != JobState.ended):
    print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
    time.sleep(3)
    jobResult = adlaJobClient.job.get(adla, jobId)

print ('Job finished with result: ' + jobResult.result.value)
```

## <a name="list-pipelines-and-recurrences"></a>Lista folyamatok és ismétlődések
Attól függően, hogy a feladatok rendelkeznek-e a folyamat vagy ismétlődési metaadatok csatolt, listázhatja folyamatok és ismétlődések.

```python
pipelines = adlaJobClient.pipeline.list(adla)
for p in pipelines:
    print('Pipeline: ' + p.name + ' ' + p.pipelineId)

recurrences = adlaJobClient.recurrence.list(adla)
for r in recurrences:
    print('Recurrence: ' + r.name + ' ' + r.recurrenceId)
```

## <a name="manage-compute-policies"></a>Számítási házirendjeinek kezelése

hello DataLakeAnalyticsAccountManagementClient objektum biztosít hello kezelésére szolgáló módszerek számítási házirendek a Data Lake Analytics-fiók.

### <a name="list-compute-policies"></a>Számítási házirendek felsorolása

hello a következő kód lekéri a Data Lake Analytics-fiók számítási házirendek listáját.

```python
policies = adlaAccountClient.computePolicies.listByAccount(rg, adla)
for p in policies:
    print('Name: ' + p.name + 'Type: ' + p.objectType + 'Max AUs / job: ' + p.maxDegreeOfParallelismPerJob + 'Min priority / job: ' + p.minPriorityPerJob)
```

### <a name="create-a-new-compute-policy"></a>Hozzon létre egy új számítási házirendet

a következő kód hello létrehoz egy új számítási házirendet a Data Lake Analytics-fiók, beállítás hello maximális ausztráliai elérhető toohello megadott felhasználói too50, és hello minimális feladat prioritása too250.

```python
userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde"
newPolicyParams = ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250)
adlaAccountClient.computePolicies.createOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams)
```

## <a name="next-steps"></a>Következő lépések

- toosee hello ugyanaz az oktatóanyagot más eszközök használatával hello szeretné a hello hello lap tetején kattintson.
- toolearn U-SQL, lásd: [Ismerkedés az Azure Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md).
- Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure Portallal).

