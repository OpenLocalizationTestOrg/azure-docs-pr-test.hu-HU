---
title: "Kivételkezelés & hiba naplózási forgatókönyv – Azure Logic Apps |} Microsoft Docs"
description: "Speciális kivétel- és az Azure Logic Apps hibanaplózás valódi használati eset ismerteti"
keywords: 
services: logic-apps
author: hedidin
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 63b0b843-f6b0-4d9a-98d0-17500be17385
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/29/2016
ms.author: LADocs; b-hoedid
ms.openlocfilehash: 044de27c75da93c95609110d2b73336c42f746fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="scenario-exception-handling-and-error-logging-for-logic-apps"></a><span data-ttu-id="26432-103">Forgatókönyv: Kivételkezelést és a hibanaplózás a logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="26432-103">Scenario: Exception handling and error logging for logic apps</span></span>

<span data-ttu-id="26432-104">Ez a forgatókönyv leírja, hogyan bővítheti a logikai alkalmazás kivételkezelést hatékonyabb támogatásához.</span><span class="sxs-lookup"><span data-stu-id="26432-104">This scenario describes how you can extend a logic app to better support exception handling.</span></span> <span data-ttu-id="26432-105">A kérdés megválaszolásához jelenleg használt valós használati eset: "Az Azure Logic Apps támogatja kivétel és hibakezelés?"</span><span class="sxs-lookup"><span data-stu-id="26432-105">We've used a real-life use case to answer the question: "Does Azure Logic Apps support exception and error handling?"</span></span>

> [!NOTE]
> <span data-ttu-id="26432-106">A jelenlegi Azure Logic Apps séma művelet válaszokat biztosít a szabványos sablon.</span><span class="sxs-lookup"><span data-stu-id="26432-106">The current Azure Logic Apps schema provides a standard template for action responses.</span></span> <span data-ttu-id="26432-107">Ez a sablon belső érvényesítési és API-alkalmazás által visszaadott hiba válaszai egyaránt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="26432-107">This template includes both internal validation and error responses returned from an API app.</span></span>

## <a name="scenario-and-use-case-overview"></a><span data-ttu-id="26432-108">A forgatókönyv és -felhasználási eset áttekintése</span><span class="sxs-lookup"><span data-stu-id="26432-108">Scenario and use case overview</span></span>

<span data-ttu-id="26432-109">A szövegegység itt van, mint az ebben a forgatókönyvben használata esetében:</span><span class="sxs-lookup"><span data-stu-id="26432-109">Here's the story as the use case for this scenario:</span></span> 

<span data-ttu-id="26432-110">Egy jól ismert egészségügyi szervezet részt vevő számunkra, hogy egy rekordot kell létrehoznia egy beteg portál a Microsoft Dynamics CRM Online Azure megoldást fejleszthet.</span><span class="sxs-lookup"><span data-stu-id="26432-110">A well-known healthcare organization engaged us to develop an Azure solution that would create a patient portal by using Microsoft Dynamics CRM Online.</span></span> <span data-ttu-id="26432-111">Azok a Dynamics CRM Online beteg portál és a Salesforce időpontot egyeztessen rekordok elküldéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="26432-111">They needed to send appointment records between the Dynamics CRM Online patient portal and Salesforce.</span></span> <span data-ttu-id="26432-112">Azt is kéri, hogy használja a [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) összes beteg rekordok szabványos.</span><span class="sxs-lookup"><span data-stu-id="26432-112">We were asked to use the [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) standard for all patient records.</span></span>

<span data-ttu-id="26432-113">A projekt két fő követelmény rendelkezett:</span><span class="sxs-lookup"><span data-stu-id="26432-113">The project had two major requirements:</span></span>  

* <span data-ttu-id="26432-114">A Dynamics CRM Online portálon küldött rekordok naplózására metódus</span><span class="sxs-lookup"><span data-stu-id="26432-114">A method to log records sent from the Dynamics CRM Online portal</span></span>
* <span data-ttu-id="26432-115">A munkafolyamaton belül történt hibákról megtekintése</span><span class="sxs-lookup"><span data-stu-id="26432-115">A way to view any errors that occurred within the workflow</span></span>

> [!TIP]
> <span data-ttu-id="26432-116">A projekt egy magas szintű videót: [integrációs felhasználói csoport](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "Integration User Group").</span><span class="sxs-lookup"><span data-stu-id="26432-116">For a high-level video about this project, see [Integration User Group](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "Integration User Group").</span></span>

## <a name="how-we-solved-the-problem"></a><span data-ttu-id="26432-117">Hogyan azt megoldotta a problémát</span><span class="sxs-lookup"><span data-stu-id="26432-117">How we solved the problem</span></span>

<span data-ttu-id="26432-118">Választottuk [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/ "Azure Cosmos DB") , a napló és a hiba (Cosmos DB-dokumentumokként rekordok jelenti) rekordok tára.</span><span class="sxs-lookup"><span data-stu-id="26432-118">We chose [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/ "Azure Cosmos DB") As a repository for the log and error records (Cosmos DB refers to records as documents).</span></span> <span data-ttu-id="26432-119">Mivel Azure Logic Apps összes szabványos sablonját, azt nem kell létrehozni egy egyéni séma.</span><span class="sxs-lookup"><span data-stu-id="26432-119">Because Azure Logic Apps has a standard template for all responses, we would not have to create a custom schema.</span></span> <span data-ttu-id="26432-120">API-alkalmazás sikerült létrehozhatunk **beszúrása** és **lekérdezés** hiba és a naplófájlokon is rekordok.</span><span class="sxs-lookup"><span data-stu-id="26432-120">We could create an API app to **Insert** and **Query** for both error and log records.</span></span> <span data-ttu-id="26432-121">Azt is meghatározhatja a séma minden, az API-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="26432-121">We could also define a schema for each within the API app.</span></span>  

<span data-ttu-id="26432-122">Egy további követelmény a rekordok kiüríteni egy bizonyos dátum után volt.</span><span class="sxs-lookup"><span data-stu-id="26432-122">Another requirement was to purge records after a certain date.</span></span> <span data-ttu-id="26432-123">Cosmos DB nevű tulajdonsággal rendelkezik [élettartama](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "élettartama") (TTL), amely engedélyezett, hogy állítsa be a **élettartama** rekordok vagy gyűjtemény értékét.</span><span class="sxs-lookup"><span data-stu-id="26432-123">Cosmos DB has a property called [Time to Live](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Time to Live") (TTL), which allowed us to set a **Time to Live** value for each record or collection.</span></span> <span data-ttu-id="26432-124">Ez a funkció Cosmos DB rekord manuálisan törölnie kell számolni.</span><span class="sxs-lookup"><span data-stu-id="26432-124">This capability eliminated the need to manually delete records in Cosmos DB.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="26432-125">Az oktatóanyag teljesítéséhez szüksége egy Cosmos DB adatbázisban és két gyűjtemények (naplózás és -hibák) létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="26432-125">To complete this tutorial, you need to create a Cosmos DB database and two collections (Logging and Errors).</span></span>

## <a name="create-the-logic-app"></a><span data-ttu-id="26432-126">A logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="26432-126">Create the logic app</span></span>

<span data-ttu-id="26432-127">Az első lépés a logikai alkalmazás létrehozása, és nyissa meg az alkalmazás a Logic App tervezőben.</span><span class="sxs-lookup"><span data-stu-id="26432-127">The first step is to create the logic app and open the app in Logic App Designer.</span></span> <span data-ttu-id="26432-128">A jelen példában használjuk a logic apps szülő-gyermek.</span><span class="sxs-lookup"><span data-stu-id="26432-128">In this example, we are using parent-child logic apps.</span></span> <span data-ttu-id="26432-129">Tegyük fel, hogy azt a szülő már létrehozott és tervezi, hogy egy gyermek logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="26432-129">Let's assume that we have already created the parent and are going to create one child logic app.</span></span>

<span data-ttu-id="26432-130">A Dynamics CRM Online kívül hamarosan naplórekord fogjuk, kezdjük, míg a lap tetején.</span><span class="sxs-lookup"><span data-stu-id="26432-130">Because we are going to log the record coming out of Dynamics CRM Online, let's start at the top.</span></span> <span data-ttu-id="26432-131">Igazolnia kell használnia egy **kérelem** eseményindítót, mert a szülő logikai alkalmazás elindítja a gyermek.</span><span class="sxs-lookup"><span data-stu-id="26432-131">We must use a **Request** trigger because the parent logic app triggers this child.</span></span>

### <a name="logic-app-trigger"></a><span data-ttu-id="26432-132">Logic app eseményindító</span><span class="sxs-lookup"><span data-stu-id="26432-132">Logic app trigger</span></span>

<span data-ttu-id="26432-133">Használjuk a **kérelem** indítható el, mert a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="26432-133">We are using a **Request** trigger as shown in the following example:</span></span>

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


## <a name="steps"></a><span data-ttu-id="26432-134">Lépések</span><span class="sxs-lookup"><span data-stu-id="26432-134">Steps</span></span>

<span data-ttu-id="26432-135">A forrás (kérelem) a beteg rekord azt kell jelentkeznie a Dynamics CRM Online portálon.</span><span class="sxs-lookup"><span data-stu-id="26432-135">We must log the source (request) of the patient record from the Dynamics CRM Online portal.</span></span>

1. <span data-ttu-id="26432-136">A Dynamics CRM Online azt kell szereznie egy új időpontot egyeztessen rekordot.</span><span class="sxs-lookup"><span data-stu-id="26432-136">We must get a new appointment record from Dynamics CRM Online.</span></span>

   <span data-ttu-id="26432-137">Az eseményindító CRM érkező nyújt nekünk a a **CRM PatentId**, **rekordtípus**, **új vagy frissített rekord** (új vagy frissíteni a logikai értéket), és **SalesforceId**.</span><span class="sxs-lookup"><span data-stu-id="26432-137">The trigger coming from CRM provides us with the **CRM PatentId**, **record type**, **New or Updated Record** (new or update Boolean value), and **SalesforceId**.</span></span> <span data-ttu-id="26432-138">A **SalesforceId** null is lehet, mert csak a frissítéshez használja.</span><span class="sxs-lookup"><span data-stu-id="26432-138">The **SalesforceId** can be null because it's only used for an update.</span></span>
   <span data-ttu-id="26432-139">A CRM-bejegyzés azt lekérése a CRM használatával **PatientID** és a **rekordtípus**.</span><span class="sxs-lookup"><span data-stu-id="26432-139">We get the CRM record by using the CRM **PatientID** and the **Record Type**.</span></span>

2. <span data-ttu-id="26432-140">A következő igazolnia kell a DocumentDB API-alkalmazás hozzáadása **InsertLogEntry** művelet Logic App Designer itt látható módon.</span><span class="sxs-lookup"><span data-stu-id="26432-140">Next, we need to add our DocumentDB API app **InsertLogEntry** operation as shown here in Logic App Designer.</span></span>

   <span data-ttu-id="26432-141">**Naplóbejegyzés beszúrása**</span><span class="sxs-lookup"><span data-stu-id="26432-141">**Insert log entry**</span></span>

   ![Naplóbejegyzés beszúrása](media/logic-apps-scenario-error-and-exception-handling/lognewpatient.png)

   <span data-ttu-id="26432-143">**Hiba bejegyzés beillesztése**</span><span class="sxs-lookup"><span data-stu-id="26432-143">**Insert error entry**</span></span>

   ![Naplóbejegyzés beszúrása](media/logic-apps-scenario-error-and-exception-handling/insertlogentry.png)

   <span data-ttu-id="26432-145">**Ellenőrizze a rekord hiba létrehozása**</span><span class="sxs-lookup"><span data-stu-id="26432-145">**Check for create record failure**</span></span>

   ![Az állapot](media/logic-apps-scenario-error-and-exception-handling/condition.png)

## <a name="logic-app-source-code"></a><span data-ttu-id="26432-147">Logic app forráskód</span><span class="sxs-lookup"><span data-stu-id="26432-147">Logic app source code</span></span>

> [!NOTE]
> <span data-ttu-id="26432-148">A következő példák csak minták.</span><span class="sxs-lookup"><span data-stu-id="26432-148">The following examples are samples only.</span></span> <span data-ttu-id="26432-149">Mivel ebben az oktatóanyagban alapul most az üzemi környezetben, értékének megvalósítását egy **forráscsomópont** nem jelenik meg az ütemezés egy időpontot egyeztessen. kapcsolódó Tulajdonságok ></span><span class="sxs-lookup"><span data-stu-id="26432-149">Because this tutorial is based on an implementation now in production, the value of a **Source Node** might not display properties that are related to scheduling an appointment.></span></span> 

### <a name="logging"></a><span data-ttu-id="26432-150">Naplózás</span><span class="sxs-lookup"><span data-stu-id="26432-150">Logging</span></span>

<span data-ttu-id="26432-151">A következő logic app példakód bemutatja, hogyan naplózási kezelésére.</span><span class="sxs-lookup"><span data-stu-id="26432-151">The following logic app code sample shows how to handle logging.</span></span>

#### <a name="log-entry"></a><span data-ttu-id="26432-152">Naplóbejegyzés</span><span class="sxs-lookup"><span data-stu-id="26432-152">Log entry</span></span>

<span data-ttu-id="26432-153">Ez a logic app forráskódja naplóbejegyzést beszúrni.</span><span class="sxs-lookup"><span data-stu-id="26432-153">Here is the logic app source code for inserting a log entry.</span></span>

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a><span data-ttu-id="26432-154">Napló kérelem</span><span class="sxs-lookup"><span data-stu-id="26432-154">Log request</span></span>

<span data-ttu-id="26432-155">Ez a napló kérelemüzenet közzé az API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="26432-155">Here is the log request message posted to the API app.</span></span>

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{/"Pragma/":/"no-cache/",/"x-ms-request-id/":/"e750c9a9-bd48-44c4-bbba-1688b6f8a132/",/"OData-Version/":/"4.0/",/"Cache-Control/":/"no-cache/",/"Date/":/"Fri, 10 Jun 2016 22:31:56 GMT/",/"Set-Cookie/":/"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1/",/"Server/":/"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0/",/"X-AspNet-Version/":/"4.0.30319/",/"X-Powered-By/":/"ASP.NET/",/"Content-Length/":/"1935/",/"Content-Type/":/"application/json; odata.metadata=minimal; odata.streaming=true/",/"Expires/":/"-1/"}"
        }
    }

```


#### <a name="log-response"></a><span data-ttu-id="26432-156">Válasz naplózása</span><span class="sxs-lookup"><span data-stu-id="26432-156">Log response</span></span>

<span data-ttu-id="26432-157">Ez a napló válaszüzenetet az API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="26432-157">Here is the log response message from the API app.</span></span>

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "/"0400fc2f-0000-0000-0000-575b3ff00000/"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{/"Pragma/":/"no-cache/",/"x-ms-request-id/":/"e750c9a9-bd48-44c4-bbba-1688b6f8a132/",/"OData-Version/":/"4.0/",/"Cache-Control/":/"no-cache/",/"Date/":/"Fri, 10 Jun 2016 22:31:56 GMT/",/"Set-Cookie/":/"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1/",/"Server/":/"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0/",/"X-AspNet-Version/":/"4.0.30319/",/"X-Powered-By/":/"ASP.NET/",/"Content-Length/":/"1935/",/"Content-Type/":/"application/json; odata.metadata=minimal; odata.streaming=true/",/"Expires/":/"-1/"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

<span data-ttu-id="26432-158">Most már a hibakezelési lépéseket vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="26432-158">Now let's look at the error handling steps.</span></span>

### <a name="error-handling"></a><span data-ttu-id="26432-159">Hibakezelés</span><span class="sxs-lookup"><span data-stu-id="26432-159">Error handling</span></span>

<span data-ttu-id="26432-160">A következő logic app példakód bemutatja, hogyan implementálható hibakezelés.</span><span class="sxs-lookup"><span data-stu-id="26432-160">The following logic app code sample shows how you can implement error handling.</span></span>

#### <a name="create-error-record"></a><span data-ttu-id="26432-161">Hiba rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="26432-161">Create error record</span></span>

<span data-ttu-id="26432-162">Ez a logic app forráskód egy hibarekordot létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="26432-162">Here is the logic app source code for creating an error record.</span></span>

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}             
```

#### <a name="insert-error-into-cosmos-db--request"></a><span data-ttu-id="26432-163">Helyezze be a hiba a Cosmos DB--kérése</span><span class="sxs-lookup"><span data-stu-id="26432-163">Insert error into Cosmos DB--request</span></span>

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MasterID_mp__c/":/"/",/"C_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"ONY_ID__c/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-cosmos-db--response"></a><span data-ttu-id="26432-164">Hiba beszúrása Cosmos DB--válasz</span><span class="sxs-lookup"><span data-stu-id="26432-164">Insert error into Cosmos DB--response</span></span>

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "/"0c00eaac-0000-0000-0000-575b3fdc0000/"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/":/"DO - Degree level is DO/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MterID_mp__c/":/"/",/"Medicis_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"XXXXXXX/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a><span data-ttu-id="26432-165">Salesforce hibaválaszba</span><span class="sxs-lookup"><span data-stu-id="26432-165">Salesforce error response</span></span>

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="return-the-response-back-to-parent-logic-app"></a><span data-ttu-id="26432-166">Térjen vissza a választ szülő logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="26432-166">Return the response back to parent logic app</span></span>

<span data-ttu-id="26432-167">Miután lekérni a választ, adja meg a választ a szülő logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="26432-167">After you get the response, you can pass the response back to the parent logic app.</span></span>

#### <a name="return-success-response-to-parent-logic-app"></a><span data-ttu-id="26432-168">Szülő logikai alkalmazás sikeres választ küldi vissza</span><span class="sxs-lookup"><span data-stu-id="26432-168">Return success response to parent logic app</span></span>

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "    Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-parent-logic-app"></a><span data-ttu-id="26432-169">Szülő logikai alkalmazás hiba választ küldi vissza</span><span class="sxs-lookup"><span data-stu-id="26432-169">Return error response to parent logic app</span></span>

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="cosmos-db-repository-and-portal"></a><span data-ttu-id="26432-170">Cosmos DB tárház és -portál</span><span class="sxs-lookup"><span data-stu-id="26432-170">Cosmos DB repository and portal</span></span>

<span data-ttu-id="26432-171">A megoldás hozzáadott képességei a [Cosmos DB](https://azure.microsoft.com/services/documentdb).</span><span class="sxs-lookup"><span data-stu-id="26432-171">Our solution added capabilities with [Cosmos DB](https://azure.microsoft.com/services/documentdb).</span></span>

### <a name="error-management-portal"></a><span data-ttu-id="26432-172">Hiba történt a felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="26432-172">Error management portal</span></span>

<span data-ttu-id="26432-173">Meg a hibákat, létrehozhat egy MVC webalkalmazás Cosmos DB hiba rekordját megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="26432-173">To view the errors, you can create an MVC web app to display the error records from Cosmos DB.</span></span> <span data-ttu-id="26432-174">A **lista**, **részletek**, **szerkesztése**, és **törlése** műveletek szerepelnek a jelenlegi verziójával.</span><span class="sxs-lookup"><span data-stu-id="26432-174">The **List**, **Details**, **Edit**, and **Delete** operations are included in the current version.</span></span>

> [!NOTE]
> <span data-ttu-id="26432-175">Művelet szerkesztése: Cosmos DB a felváltja a teljes dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="26432-175">Edit operation: Cosmos DB replaces the entire document.</span></span> <span data-ttu-id="26432-176">A bejegyzések a **lista** és **részletes** nézetek olyan csak minták.</span><span class="sxs-lookup"><span data-stu-id="26432-176">The records shown in the **List** and **Detail** views are samples only.</span></span> <span data-ttu-id="26432-177">Nincsenek tényleges beteg időpontot egyeztessen rögzíti.</span><span class="sxs-lookup"><span data-stu-id="26432-177">They are not actual patient appointment records.</span></span>

<span data-ttu-id="26432-178">Az alábbiakban az MVC-alkalmazás adatait, a korábban ismertetett módszert létrehozott példát.</span><span class="sxs-lookup"><span data-stu-id="26432-178">Here are examples of our MVC app details created with the previously described approach.</span></span>

#### <a name="error-management-list"></a><span data-ttu-id="26432-179">Hiba a felügyeleti lista</span><span class="sxs-lookup"><span data-stu-id="26432-179">Error management list</span></span>
![Hiba](media/logic-apps-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a><span data-ttu-id="26432-181">Hiba felügyeleti részletes nézet</span><span class="sxs-lookup"><span data-stu-id="26432-181">Error management detail view</span></span>
![A hiba adatai](media/logic-apps-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a><span data-ttu-id="26432-183">Napló felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="26432-183">Log management portal</span></span>

<span data-ttu-id="26432-184">A naplók megtekintéséhez is létrehoztunk egy MVC webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="26432-184">To view the logs, we also created an MVC web app.</span></span> <span data-ttu-id="26432-185">Az alábbiakban az MVC-alkalmazás adatait, a korábban ismertetett módszert létrehozott példát.</span><span class="sxs-lookup"><span data-stu-id="26432-185">Here are examples of our MVC app details created with the previously described approach.</span></span>

#### <a name="sample-log-detail-view"></a><span data-ttu-id="26432-186">A minta napló részletes nézet</span><span class="sxs-lookup"><span data-stu-id="26432-186">Sample log detail view</span></span>
![Napló részletes nézet](media/logic-apps-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a><span data-ttu-id="26432-188">API-alkalmazás adatait</span><span class="sxs-lookup"><span data-stu-id="26432-188">API app details</span></span>

#### <a name="logic-apps-exception-management-api"></a><span data-ttu-id="26432-189">Logic Apps kivételek kezelése API</span><span class="sxs-lookup"><span data-stu-id="26432-189">Logic Apps exception management API</span></span>

<span data-ttu-id="26432-190">A nyílt forráskódú Azure Logic Apps kivétel felügyeleti API app funkciókat biztosítja a itt leírtak – két vezérlők:</span><span class="sxs-lookup"><span data-stu-id="26432-190">Our open-source Azure Logic Apps exception management API app provides functionality as described here - there are two controllers:</span></span>

* <span data-ttu-id="26432-191">**ErrorController** egy hibarekordot (dokumentumok) szúr be egy DocumentDB-gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="26432-191">**ErrorController** inserts an error record (document) in a DocumentDB collection.</span></span>
* <span data-ttu-id="26432-192">**LogController** egy naplóbejegyzést képviselnek (dokumentumok) szúr be egy DocumentDB-gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="26432-192">**LogController** Inserts a log record (document) in a DocumentDB collection.</span></span>

> [!TIP]
> <span data-ttu-id="26432-193">Mindkét vezérlők használata `async Task<dynamic>` műveletek, így a műveletek megoldásához futásidőben, így a DocumentDB séma létrehozhatjuk törzsében. a művelet.</span><span class="sxs-lookup"><span data-stu-id="26432-193">Both controllers use `async Task<dynamic>` operations, allowing operations to resolve at runtime, so we can create the DocumentDB schema in the body of the operation.</span></span> 
> 

<span data-ttu-id="26432-194">Minden dokumentumot a documentdb egy egyedi azonosítóval kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="26432-194">Every document in DocumentDB must have a unique ID.</span></span> <span data-ttu-id="26432-195">Használjuk `PatientId` és egy Unix timestamp értéket (kétirányú) konvertált időbélyeg hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="26432-195">We are using `PatientId` and adding a timestamp that is converted to a Unix timestamp value (double).</span></span> <span data-ttu-id="26432-196">Az érték, a tört érték eltávolítása csonkolja azt.</span><span class="sxs-lookup"><span data-stu-id="26432-196">We truncate the value to remove the fractional value.</span></span>

<span data-ttu-id="26432-197">Megtekintheti a hiba vezérlőhöz API forráskódját [a Githubról](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).</span><span class="sxs-lookup"><span data-stu-id="26432-197">You can view the source code of our error controller API [from GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).</span></span>

<span data-ttu-id="26432-198">Az API-t a logikai alkalmazás, a következő szintaxissal nevezzük:</span><span class="sxs-lookup"><span data-stu-id="26432-198">We call the API from a logic app by using the following syntax:</span></span>

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

<span data-ttu-id="26432-199">Az előző példakódban kifejezést keresi a *Create_NewPatientRecord* állapotának **sikertelen**.</span><span class="sxs-lookup"><span data-stu-id="26432-199">The expression in the preceding code sample checks for the *Create_NewPatientRecord* status of **Failed**.</span></span>

## <a name="summary"></a><span data-ttu-id="26432-200">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="26432-200">Summary</span></span>

* <span data-ttu-id="26432-201">Naplózás és a logikai alkalmazás hibakezelési könnyedén alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="26432-201">You can easily implement logging and error handling in a logic app.</span></span>
* <span data-ttu-id="26432-202">A DocumentDB a tárház védelemként is alkalmazhatják a napló és a hiba a rekordok (dokumentumok).</span><span class="sxs-lookup"><span data-stu-id="26432-202">You can use DocumentDB as the repository for log and error records (documents).</span></span>
* <span data-ttu-id="26432-203">MVC hozhat létre a napló és a hiba a rekordok megjelenítéséhez a portálon.</span><span class="sxs-lookup"><span data-stu-id="26432-203">You can use MVC to create a portal to display log and error records.</span></span>

### <a name="source-code"></a><span data-ttu-id="26432-204">Forráskód</span><span class="sxs-lookup"><span data-stu-id="26432-204">Source code</span></span>

<span data-ttu-id="26432-205">A Logic Apps kivétel felügyeleti API-alkalmazás forráskódja érhető el ezen [GitHub-tárházban](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logic App kivétel felügyeleti API").</span><span class="sxs-lookup"><span data-stu-id="26432-205">The source code for the Logic Apps exception management API application is available in this [GitHub repository](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logic App Exception Management API").</span></span>

## <a name="next-steps"></a><span data-ttu-id="26432-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="26432-206">Next steps</span></span>

* [<span data-ttu-id="26432-207">További logic app példák és forgatókönyvek megtekintése</span><span class="sxs-lookup"><span data-stu-id="26432-207">View more logic app examples and scenarios</span></span>](../logic-apps/logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="26432-208">További információk a logic Apps alkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="26432-208">Learn about monitoring logic apps</span></span>](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [<span data-ttu-id="26432-209">A logic Apps alkalmazások automatikus központi telepítési sablonok létrehozása</span><span class="sxs-lookup"><span data-stu-id="26432-209">Create automated deployment templates for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
