---
title: "aaaGet REST API használatával a Data Lake Analytics használatába |} Microsoft Docs"
description: "A Data Lake Analytics WebHDFS REST API-k tooperform műveletek használata"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 5e133d92-baaa-44c9-890c-ab2d85c91122
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/03/2017
ms.author: jgao
ms.openlocfilehash: a0b13d521821fd2d74716cc52485585feb7c51b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Az Azure Data Lake Analytics használatának első lépései REST API-k használatával
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Ismerje meg, hogyan toouse WebHDFS REST API-k és a Data Lake Analytics REST API-k toomanage Data Lake Analytics fiókok, feladatok és a katalógus. 

## <a name="prerequisites"></a>Előfeltételek
* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Egy Azure Active Directory-alkalmazás létrehozása**. Hello Azure AD alkalmazás tooauthenticate hello Data Lake Analytics-alkalmazást az Azure ad-val használhatja. Nincsenek különböző szempontok tooauthenticate az Azure ad-vel, amelyek **végfelhasználói hitelesítési** vagy **szolgáltatások közötti hitelesítési**. További információt és útmutatást tooauthenticate, lásd: [hitelesítés az Azure Active Directory használatával a Data Lake Analytics](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).
* [cURL](http://curl.haxx.se/). Ez a cikk a cURL toodemonstrate hogyan toomake REST API meghívja a Data Lake Analytics-fiók ellen használja.

## <a name="authenticate-with-azure-active-directory"></a>Hitelesítés az Azure Active Directoryval
Az Azure Active Directoryval kétféle módon lehet hitelesíteni.

### <a name="end-user-authentication-interactive"></a>Végfelhasználó hitelesítése (interaktív)
Ezzel a módszerrel alkalmazás kérni fogja a hello felhasználói toolog, és minden hello műveleteket hello hello felhasználó környezetében. 

Az interaktív hitelesítéshez kövesse az alábbi lépéseket:

1. Az alkalmazáson keresztül átirányítási URL-cím a következő hello felhasználói toohello:
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > \<REDIRECT-URI > használható URL-kódolású toobe kell. A https://localhost esetében tehát használja a következőt: `https%3A%2F%2Flocalhost`)
   > 
   > 
   
    Az oktatóanyagban szereplő hello célból cserélje le a hello helyőrző értékeket a fenti hello URL-ben, és beillesztheti egy webböngésző címsorába. Az Azure bejelentkezési azonosítójával átirányított tooauthenticate lesz. Miután sikeresen bejelentkezett, hello válasz hello böngésző címsorában megjelenik. hello válasz hello formátuma a következő lesz:
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. Hello engedélyezési kód a hello válaszban rögzítéséhez. Ebben az oktatóanyagban hello webböngésző címsorába hello hello engedélyezési kód másolását, és adja át hello POST kérelem toohello jogkivonat végpontjához az alább látható módon:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > Ebben az esetben hello \<REDIRECT-URI > kódolása nem szükséges.
   > 
   > 
3. a rendszer hello választ, egy JSON-objektum, amely egy hozzáférési jogkivonatot tartalmaz (például `"access_token": "<ACCESS_TOKEN>"`) és egy frissítési (pl. `"refresh_token": "<REFRESH_TOKEN>"`). Az alkalmazás használja hello hozzáférési jogkivonatot az Azure Data Lake Store és hello frissítési jogkivonat tooget való hozzáférés során egy új hozzáférési jogkivonat mikor jár le a hozzáférési tokent.
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. Amikor hello hozzáférési jogkivonat lejár, kérhet egy új hozzáférési jogkivonat hello frissítési jogkivonat használatával alább látható módon:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<CLIENT-ID> \
             -F refresh_token=<REFRESH-TOKEN>

További információk az interaktív felhasználói hitelesítéssel kapcsolatban: [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx) (Az engedélyezési kód engedélyezési folyamata).

### <a name="service-to-service-authentication-non-interactive"></a>Szolgáltatások közötti hitelesítés (nem interaktív)
Ezzel a módszerrel alkalmazás maga biztosítja saját hitelesítő adatait tooperform hello műveletek. Ehhez hello az alábbihoz hasonló POST-kérelmet kell kiadnia: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

a kérelem hello kimenete egy engedélyezési jogkivonatot tartalmazza (kimaradásával `access-token` hello kimenetben alább), amely ezt követően továbbítják a REST API-hívásokat. Mentse ezt az engedélyezési jogkivonatot egy szövegfájlba, mivel később még szüksége lesz rá a cikkben.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Ebben a cikkben az hello **nem interaktív** megközelítést. A nem interaktív (szolgáltatások közötti hívások) további információkért lásd: [tooservice hívások hitelesítő szolgáltatás](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics-fiók létrehozása
Data Lake Analytics-fiók létrehozása előtt létre kell hoznia egy Azure-erőforráscsoportot és egy Data Lake Store-fiókot.  Lásd: [Data Lake Store-fiók létrehozása](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

a következő Curl-parancs bemutatja hogyan hello toocreate fiók:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Cserélje le \< `REDACTED` \> az hello engedélyezési jogkivonatot, \< `AzureSubscriptionID` \> az előfizetés-Azonosítóval rendelkező \< `AzureResourceGroupName` \> egy meglévő Azure-erőforrás Csoport neve, és \< `NewAzureDataLakeAnalyticsAccountName` \> új Data Lake Analytics-fiók névvel. hello kérelem hasznos adatai ezen parancs hello lévő **CreateDatalakeAnalyticsAccountRequest.json** hello a megadott fájl `-d` fenti paraméter. hello input.json fájl tartalmának hello hello alábbihoz:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>A Data Lake Analytics-fiókok felsorolása egy előfizetésben
a következő Curl-parancsot hello bemutatja, hogyan toolist fiókok egy előfizetésben:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Cserélje le \< `REDACTED` \> az hello engedélyezési jogkivonatot, \< `AzureSubscriptionID` \> az előfizetés-azonosítóval. hello kimeneti hasonlít:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Data Lake Analytics-fiókkal kapcsolatos információk beszerzése
a következő Curl-parancs bemutatja hogyan hello tooget egy fiók adatait:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Cserélje le \< `REDACTED` \> az hello engedélyezési jogkivonatot, \< `AzureSubscriptionID` \> az előfizetés-Azonosítóval rendelkező \< `AzureResourceGroupName` \> egy meglévő Azure-erőforrás Csoport neve, és \< `DataLakeAnalyticsAccountName` \> hello nevet, egy meglévő Data Lake Analytics-fiók. hello kimeneti hasonlít:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Data Lake Analytics-fiókhoz tartozó Data Lake-tárolók felsorolása
a következő Curl-parancsot hello bemutatja, hogyan toolist Data Lake tárolja egy olyan fiók:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Cserélje le \< `REDACTED` \> az hello engedélyezési jogkivonatot, \< `AzureSubscriptionID` \> az előfizetés-Azonosítóval rendelkező \< `AzureResourceGroupName` \> egy meglévő Azure-erőforrás Csoport neve, és \< `DataLakeAnalyticsAccountName` \> hello nevet, egy meglévő Data Lake Analytics-fiók. hello kimeneti hasonlít:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>U-SQL-feladatok küldése
a következő Curl-parancs bemutatja hogyan hello toosubmit egy U-SQL-feladatot:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Cserélje le \< `REDACTED` \> az hello engedélyezési jogkivonatot, \< `DataLakeAnalyticsAccountName` \> hello nevet, egy meglévő Data Lake Analytics-fiók. hello kérelem hasznos adatai ezen parancs hello lévő **SubmitADLAJob.json** hello a megadott fájl `-d` fenti paraméter. hello input.json fájl tartalmának hello hello alábbihoz:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    too\"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

hello kimeneti hasonlít:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>U-SQL-feladatok felsorolása
a következő Curl-parancs bemutatja hogyan hello toolist U-SQL feladatok:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Cserélje le \< `REDACTED` \> az hello engedélyezési jogkivonatot, és \< `DataLakeAnalyticsAccountName` \> hello nevet, egy meglévő Data Lake Analytics-fiók. 

hello kimeneti hasonlít:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Katalóguselemek beolvasása
a következő Curl-parancsot hello bemutatja, hogyan tooget hello adatbázisok hello katalógus:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

hello kimeneti hasonlít:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Lásd még:
* egy összetettebb lekérdezés toosee lásd [elemzés webhely naplózza az Azure Data Lake Analytics használatával](data-lake-analytics-analyze-weblogs.md).
* megkezdődött a U-SQL-alkalmazások fejlesztésével tooget lásd [Data Lake Tools for Visual Studio használatával fejlesztése U-SQL-parancsfájlok](data-lake-analytics-data-lake-tools-get-started.md).
* toolearn U-SQL, lásd: [Ismerkedés az Azure Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md).
* Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure Portallal).
* a Data Lake Analytics áttekintésének tooget lásd [Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md).
* toosee hello ugyanaz az oktatóanyagot más eszközök használatával hello szeretné a hello hello lap tetején kattintson.

