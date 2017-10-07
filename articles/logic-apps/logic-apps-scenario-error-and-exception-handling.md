---
title: "aaaException kezelési & hiba naplózási forgatókönyv – Azure Logic Apps |} Microsoft Docs"
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
ms.openlocfilehash: e893a7b652254dca7b8a82398e8afd571f6ccd25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scenario-exception-handling-and-error-logging-for-logic-apps"></a><span data-ttu-id="018ce-103">Forgatókönyv: Kivételkezelést és a hibanaplózás a logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="018ce-103">Scenario: Exception handling and error logging for logic apps</span></span>

<span data-ttu-id="018ce-104">Ez a forgatókönyv leírja, hogyan bővítheti a logic app toobetter támogatási kivételkezelést.</span><span class="sxs-lookup"><span data-stu-id="018ce-104">This scenario describes how you can extend a logic app toobetter support exception handling.</span></span> <span data-ttu-id="018ce-105">Azt már felhasznált egy valós használja kis tooanswer hello kérdést: "Az Azure Logic Apps támogatja kivétel és hibakezelés?"</span><span class="sxs-lookup"><span data-stu-id="018ce-105">We've used a real-life use case tooanswer hello question: "Does Azure Logic Apps support exception and error handling?"</span></span>

> [!NOTE]
> <span data-ttu-id="018ce-106">hello aktuális Azure Logic Apps séma művelet válaszokat biztosít a szabványos sablon.</span><span class="sxs-lookup"><span data-stu-id="018ce-106">hello current Azure Logic Apps schema provides a standard template for action responses.</span></span> <span data-ttu-id="018ce-107">Ez a sablon belső érvényesítési és API-alkalmazás által visszaadott hiba válaszai egyaránt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="018ce-107">This template includes both internal validation and error responses returned from an API app.</span></span>

## <a name="scenario-and-use-case-overview"></a><span data-ttu-id="018ce-108">A forgatókönyv és -felhasználási eset áttekintése</span><span class="sxs-lookup"><span data-stu-id="018ce-108">Scenario and use case overview</span></span>

<span data-ttu-id="018ce-109">Hello szövegegység itt van, mint a forgatókönyv hello használati eset:</span><span class="sxs-lookup"><span data-stu-id="018ce-109">Here's hello story as hello use case for this scenario:</span></span> 

<span data-ttu-id="018ce-110">Egy jól ismert egészségügyi szervezet végző velünk toodevelop egy Azure megoldás, amely a beteg portál hozna létre a Microsoft Dynamics CRM Online használatával.</span><span class="sxs-lookup"><span data-stu-id="018ce-110">A well-known healthcare organization engaged us toodevelop an Azure solution that would create a patient portal by using Microsoft Dynamics CRM Online.</span></span> <span data-ttu-id="018ce-111">Toosend időpontot egyeztessen rekordok hello Dynamics CRM Online beteg portál és a Salesforce között szükség rájuk.</span><span class="sxs-lookup"><span data-stu-id="018ce-111">They needed toosend appointment records between hello Dynamics CRM Online patient portal and Salesforce.</span></span> <span data-ttu-id="018ce-112">Azt is ismételt toouse hello [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) összes beteg rekordok szabványos.</span><span class="sxs-lookup"><span data-stu-id="018ce-112">We were asked toouse hello [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) standard for all patient records.</span></span>

<span data-ttu-id="018ce-113">hello projekt két fő követelmény rendelkezett:</span><span class="sxs-lookup"><span data-stu-id="018ce-113">hello project had two major requirements:</span></span>  

* <span data-ttu-id="018ce-114">A metódus toolog rekordok küldött hello Dynamics CRM Online portálon</span><span class="sxs-lookup"><span data-stu-id="018ce-114">A method toolog records sent from hello Dynamics CRM Online portal</span></span>
* <span data-ttu-id="018ce-115">A módszer tooview hibákról hello munkafolyamaton belül</span><span class="sxs-lookup"><span data-stu-id="018ce-115">A way tooview any errors that occurred within hello workflow</span></span>

> [!TIP]
> <span data-ttu-id="018ce-116">A projekt egy magas szintű videót: [integrációs felhasználói csoport](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "integrációs felhasználói csoport").</span><span class="sxs-lookup"><span data-stu-id="018ce-116">For a high-level video about this project, see [Integration User Group](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "Integration User Group").</span></span>

## <a name="how-we-solved-hello-problem"></a><span data-ttu-id="018ce-117">Hogyan lehet megoldani azt az hello probléma</span><span class="sxs-lookup"><span data-stu-id="018ce-117">How we solved hello problem</span></span>

<span data-ttu-id="018ce-118">Választottuk [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/ "Azure Cosmos DB") , a napló és a hiba (Cosmos DB hivatkozik-dokumentumokként toorecords) rekordok hello tára.</span><span class="sxs-lookup"><span data-stu-id="018ce-118">We chose [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/ "Azure Cosmos DB") As a repository for hello log and error records (Cosmos DB refers toorecords as documents).</span></span> <span data-ttu-id="018ce-119">Mivel Azure Logic Apps összes szabványos sablonját, akkor nem tudunk toocreate egyéni séma.</span><span class="sxs-lookup"><span data-stu-id="018ce-119">Because Azure Logic Apps has a standard template for all responses, we would not have toocreate a custom schema.</span></span> <span data-ttu-id="018ce-120">Jelenleg túl létrehozhat API-alkalmazás**beszúrása** és **lekérdezés** hiba és a naplófájlokon is rekordok.</span><span class="sxs-lookup"><span data-stu-id="018ce-120">We could create an API app too**Insert** and **Query** for both error and log records.</span></span> <span data-ttu-id="018ce-121">Azt is meghatározhatja a séma az egyes hello API-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="018ce-121">We could also define a schema for each within hello API app.</span></span>  

<span data-ttu-id="018ce-122">Egy további követelmény toopurge rögzíti egy bizonyos dátum után volt.</span><span class="sxs-lookup"><span data-stu-id="018ce-122">Another requirement was toopurge records after a certain date.</span></span> <span data-ttu-id="018ce-123">Cosmos DB nevű tulajdonsággal rendelkezik [tooLive idő](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "tooLive idő") (TTL), amely engedélyezett velünk tooset egy **tooLive idő** értéke rekordok vagy gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="018ce-123">Cosmos DB has a property called [Time tooLive](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Time tooLive") (TTL), which allowed us tooset a **Time tooLive** value for each record or collection.</span></span> <span data-ttu-id="018ce-124">Ez a funkció hello kell toomanually delete rekordok Cosmos DB szüntetni.</span><span class="sxs-lookup"><span data-stu-id="018ce-124">This capability eliminated hello need toomanually delete records in Cosmos DB.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="018ce-125">toocomplete ebben az oktatóanyagban egy Cosmos DB adatbázisban toocreate és két gyűjtemények (naplózás és -hibák) van szükség.</span><span class="sxs-lookup"><span data-stu-id="018ce-125">toocomplete this tutorial, you need toocreate a Cosmos DB database and two collections (Logging and Errors).</span></span>

## <a name="create-hello-logic-app"></a><span data-ttu-id="018ce-126">Hello logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="018ce-126">Create hello logic app</span></span>

<span data-ttu-id="018ce-127">hello első lépéseként toocreate hello logikai alkalmazás és a nyitott hello app Logic App Designer.</span><span class="sxs-lookup"><span data-stu-id="018ce-127">hello first step is toocreate hello logic app and open hello app in Logic App Designer.</span></span> <span data-ttu-id="018ce-128">A jelen példában használjuk a logic apps szülő-gyermek.</span><span class="sxs-lookup"><span data-stu-id="018ce-128">In this example, we are using parent-child logic apps.</span></span> <span data-ttu-id="018ce-129">Tegyük fel, hogy azt már létrehozott hello szülő és toocreate egy gyermek logikai alkalmazás is.</span><span class="sxs-lookup"><span data-stu-id="018ce-129">Let's assume that we have already created hello parent and are going toocreate one child logic app.</span></span>

<span data-ttu-id="018ce-130">Dynamics CRM Online kívül hamarosan toolog hello rekordot fogjuk kezdjük, míg hello tetején.</span><span class="sxs-lookup"><span data-stu-id="018ce-130">Because we are going toolog hello record coming out of Dynamics CRM Online, let's start at hello top.</span></span> <span data-ttu-id="018ce-131">Igazolnia kell használnia egy **kérelem** eseményindítót, mert hello szülő logikai alkalmazás elindítja a gyermek.</span><span class="sxs-lookup"><span data-stu-id="018ce-131">We must use a **Request** trigger because hello parent logic app triggers this child.</span></span>

### <a name="logic-app-trigger"></a><span data-ttu-id="018ce-132">Logic app eseményindító</span><span class="sxs-lookup"><span data-stu-id="018ce-132">Logic app trigger</span></span>

<span data-ttu-id="018ce-133">Használjuk a **kérelem** indítható el, mert a következő példa hello látható:</span><span class="sxs-lookup"><span data-stu-id="018ce-133">We are using a **Request** trigger as shown in hello following example:</span></span>

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


## <a name="steps"></a><span data-ttu-id="018ce-134">Lépések</span><span class="sxs-lookup"><span data-stu-id="018ce-134">Steps</span></span>

<span data-ttu-id="018ce-135">Hello forrás (kérelem) hello beteg rekord hello Dynamics CRM Online portálon azt kell jelentkeznie.</span><span class="sxs-lookup"><span data-stu-id="018ce-135">We must log hello source (request) of hello patient record from hello Dynamics CRM Online portal.</span></span>

1. <span data-ttu-id="018ce-136">A Dynamics CRM Online azt kell szereznie egy új időpontot egyeztessen rekordot.</span><span class="sxs-lookup"><span data-stu-id="018ce-136">We must get a new appointment record from Dynamics CRM Online.</span></span>

   <span data-ttu-id="018ce-137">hello eseményindító CRM érkező ad hello **CRM PatentId**, **rekordtípus**, **új vagy frissített rekord** (új vagy frissíteni a logikai értéket), és  **SalesforceId**.</span><span class="sxs-lookup"><span data-stu-id="018ce-137">hello trigger coming from CRM provides us with hello **CRM PatentId**, **record type**, **New or Updated Record** (new or update Boolean value), and **SalesforceId**.</span></span> <span data-ttu-id="018ce-138">Hello **SalesforceId** null is lehet, mert csak a frissítéshez használja.</span><span class="sxs-lookup"><span data-stu-id="018ce-138">hello **SalesforceId** can be null because it's only used for an update.</span></span>
   <span data-ttu-id="018ce-139">Hello CRM-bejegyzés azt lekérése hello CRM használatával **PatientID** és hello **rekordtípus**.</span><span class="sxs-lookup"><span data-stu-id="018ce-139">We get hello CRM record by using hello CRM **PatientID** and hello **Record Type**.</span></span>

2. <span data-ttu-id="018ce-140">Ezt követően kell tooadd a DocumentDB API-alkalmazás **InsertLogEntry** művelet Logic App Designer itt látható módon.</span><span class="sxs-lookup"><span data-stu-id="018ce-140">Next, we need tooadd our DocumentDB API app **InsertLogEntry** operation as shown here in Logic App Designer.</span></span>

   <span data-ttu-id="018ce-141">**Naplóbejegyzés beszúrása**</span><span class="sxs-lookup"><span data-stu-id="018ce-141">**Insert log entry**</span></span>

   ![Naplóbejegyzés beszúrása](media/logic-apps-scenario-error-and-exception-handling/lognewpatient.png)

   <span data-ttu-id="018ce-143">**Hiba bejegyzés beillesztése**</span><span class="sxs-lookup"><span data-stu-id="018ce-143">**Insert error entry**</span></span>

   ![Naplóbejegyzés beszúrása](media/logic-apps-scenario-error-and-exception-handling/insertlogentry.png)

   <span data-ttu-id="018ce-145">**Ellenőrizze a rekord hiba létrehozása**</span><span class="sxs-lookup"><span data-stu-id="018ce-145">**Check for create record failure**</span></span>

   ![Az állapot](media/logic-apps-scenario-error-and-exception-handling/condition.png)

## <a name="logic-app-source-code"></a><span data-ttu-id="018ce-147">Logic app forráskód</span><span class="sxs-lookup"><span data-stu-id="018ce-147">Logic app source code</span></span>

> [!NOTE]
> <span data-ttu-id="018ce-148">a következő példák hello csak minták.</span><span class="sxs-lookup"><span data-stu-id="018ce-148">hello following examples are samples only.</span></span> <span data-ttu-id="018ce-149">Ebben az oktatóanyagban alapú megvalósítását most az üzemi környezetben, mert hello értékének egy **forráscsomópont** nem jelenik meg, amelyek az kapcsolódó tooscheduling egy időpontot egyeztessen. ></span><span class="sxs-lookup"><span data-stu-id="018ce-149">Because this tutorial is based on an implementation now in production, hello value of a **Source Node** might not display properties that are related tooscheduling an appointment.></span></span> 

### <a name="logging"></a><span data-ttu-id="018ce-150">Naplózás</span><span class="sxs-lookup"><span data-stu-id="018ce-150">Logging</span></span>

<span data-ttu-id="018ce-151">hello logic app kód a következő példa azt mutatja be hogyan toohandle naplózás.</span><span class="sxs-lookup"><span data-stu-id="018ce-151">hello following logic app code sample shows how toohandle logging.</span></span>

#### <a name="log-entry"></a><span data-ttu-id="018ce-152">Naplóbejegyzés</span><span class="sxs-lookup"><span data-stu-id="018ce-152">Log entry</span></span>

<span data-ttu-id="018ce-153">Itt található hello logic app forráskódja naplóbejegyzést beszúrni.</span><span class="sxs-lookup"><span data-stu-id="018ce-153">Here is hello logic app source code for inserting a log entry.</span></span>

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

#### <a name="log-request"></a><span data-ttu-id="018ce-154">Napló kérelem</span><span class="sxs-lookup"><span data-stu-id="018ce-154">Log request</span></span>

<span data-ttu-id="018ce-155">Itt található hello napló kérelemüzenet toohello API-alkalmazás közzé.</span><span class="sxs-lookup"><span data-stu-id="018ce-155">Here is hello log request message posted toohello API app.</span></span>

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


#### <a name="log-response"></a><span data-ttu-id="018ce-156">Válasz naplózása</span><span class="sxs-lookup"><span data-stu-id="018ce-156">Log response</span></span>

<span data-ttu-id="018ce-157">Itt található hello napló válaszüzenetet hello API alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="018ce-157">Here is hello log response message from hello API app.</span></span>

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

<span data-ttu-id="018ce-158">Most hello hibakezelési lépéseket vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="018ce-158">Now let's look at hello error handling steps.</span></span>

### <a name="error-handling"></a><span data-ttu-id="018ce-159">Hibakezelés</span><span class="sxs-lookup"><span data-stu-id="018ce-159">Error handling</span></span>

<span data-ttu-id="018ce-160">hello következő logic app mintakód bemutatja, hogyan implementálható hibakezelés.</span><span class="sxs-lookup"><span data-stu-id="018ce-160">hello following logic app code sample shows how you can implement error handling.</span></span>

#### <a name="create-error-record"></a><span data-ttu-id="018ce-161">Hiba rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="018ce-161">Create error record</span></span>

<span data-ttu-id="018ce-162">Itt található hello logic app forráskód egy hibarekordot létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="018ce-162">Here is hello logic app source code for creating an error record.</span></span>

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

#### <a name="insert-error-into-cosmos-db--request"></a><span data-ttu-id="018ce-163">Helyezze be a hiba a Cosmos DB--kérése</span><span class="sxs-lookup"><span data-stu-id="018ce-163">Insert error into Cosmos DB--request</span></span>

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed toocomplete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MasterID_mp__c/":/"/",/"C_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"ONY_ID__c/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-cosmos-db--response"></a><span data-ttu-id="018ce-164">Hiba beszúrása Cosmos DB--válasz</span><span class="sxs-lookup"><span data-stu-id="018ce-164">Insert error into Cosmos DB--response</span></span>

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
        "body": "CRM failed toocomplete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
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

#### <a name="salesforce-error-response"></a><span data-ttu-id="018ce-165">Salesforce hibaválaszba</span><span class="sxs-lookup"><span data-stu-id="018ce-165">Salesforce error response</span></span>

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
        "message": "Salesforce failed toocomplete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="return-hello-response-back-tooparent-logic-app"></a><span data-ttu-id="018ce-166">Visszatérési hello válasz hátsó tooparent logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="018ce-166">Return hello response back tooparent logic app</span></span>

<span data-ttu-id="018ce-167">Hello választ kap, miután átadhatók hello válasz hátsó toohello szülő logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="018ce-167">After you get hello response, you can pass hello response back toohello parent logic app.</span></span>

#### <a name="return-success-response-tooparent-logic-app"></a><span data-ttu-id="018ce-168">Térjen vissza a sikeres válasz tooparent logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="018ce-168">Return success response tooparent logic app</span></span>

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

#### <a name="return-error-response-tooparent-logic-app"></a><span data-ttu-id="018ce-169">Térjen vissza a hiba válasz tooparent logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="018ce-169">Return error response tooparent logic app</span></span>

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


## <a name="cosmos-db-repository-and-portal"></a><span data-ttu-id="018ce-170">Cosmos DB tárház és -portál</span><span class="sxs-lookup"><span data-stu-id="018ce-170">Cosmos DB repository and portal</span></span>

<span data-ttu-id="018ce-171">A megoldás hozzáadott képességei a [Cosmos DB](https://azure.microsoft.com/services/documentdb).</span><span class="sxs-lookup"><span data-stu-id="018ce-171">Our solution added capabilities with [Cosmos DB](https://azure.microsoft.com/services/documentdb).</span></span>

### <a name="error-management-portal"></a><span data-ttu-id="018ce-172">Hiba történt a felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="018ce-172">Error management portal</span></span>

<span data-ttu-id="018ce-173">tooview hello hibák, a Cosmos-Adatbázisból hozhat létre az MVC webes alkalmazás toodisplay hello hiba rögzíti.</span><span class="sxs-lookup"><span data-stu-id="018ce-173">tooview hello errors, you can create an MVC web app toodisplay hello error records from Cosmos DB.</span></span> <span data-ttu-id="018ce-174">Hello **lista**, **részletek**, **szerkesztése**, és **törlése** műveletek szerepelnek hello jelenlegi verziójával.</span><span class="sxs-lookup"><span data-stu-id="018ce-174">hello **List**, **Details**, **Edit**, and **Delete** operations are included in hello current version.</span></span>

> [!NOTE]
> <span data-ttu-id="018ce-175">Művelet szerkesztése: Cosmos DB a felváltja hello teljes dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="018ce-175">Edit operation: Cosmos DB replaces hello entire document.</span></span> <span data-ttu-id="018ce-176">hello bejegyzések hello **lista** és **részletes** nézetek olyan csak minták.</span><span class="sxs-lookup"><span data-stu-id="018ce-176">hello records shown in hello **List** and **Detail** views are samples only.</span></span> <span data-ttu-id="018ce-177">Nincsenek tényleges beteg időpontot egyeztessen rögzíti.</span><span class="sxs-lookup"><span data-stu-id="018ce-177">They are not actual patient appointment records.</span></span>

<span data-ttu-id="018ce-178">Példa az MVC alkalmazás hello korábban létrehozott részletek leírt módszer.</span><span class="sxs-lookup"><span data-stu-id="018ce-178">Here are examples of our MVC app details created with hello previously described approach.</span></span>

#### <a name="error-management-list"></a><span data-ttu-id="018ce-179">Hiba a felügyeleti lista</span><span class="sxs-lookup"><span data-stu-id="018ce-179">Error management list</span></span>
![Hiba](media/logic-apps-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a><span data-ttu-id="018ce-181">Hiba felügyeleti részletes nézet</span><span class="sxs-lookup"><span data-stu-id="018ce-181">Error management detail view</span></span>
![A hiba adatai](media/logic-apps-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a><span data-ttu-id="018ce-183">Napló felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="018ce-183">Log management portal</span></span>

<span data-ttu-id="018ce-184">tooview hello naplókat, is létrehozott egy MVC webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="018ce-184">tooview hello logs, we also created an MVC web app.</span></span> <span data-ttu-id="018ce-185">Példa az MVC alkalmazás hello korábban létrehozott részletek leírt módszer.</span><span class="sxs-lookup"><span data-stu-id="018ce-185">Here are examples of our MVC app details created with hello previously described approach.</span></span>

#### <a name="sample-log-detail-view"></a><span data-ttu-id="018ce-186">A minta napló részletes nézet</span><span class="sxs-lookup"><span data-stu-id="018ce-186">Sample log detail view</span></span>
![Napló részletes nézet](media/logic-apps-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a><span data-ttu-id="018ce-188">API-alkalmazás adatait</span><span class="sxs-lookup"><span data-stu-id="018ce-188">API app details</span></span>

#### <a name="logic-apps-exception-management-api"></a><span data-ttu-id="018ce-189">Logic Apps kivételek kezelése API</span><span class="sxs-lookup"><span data-stu-id="018ce-189">Logic Apps exception management API</span></span>

<span data-ttu-id="018ce-190">A nyílt forráskódú Azure Logic Apps kivétel felügyeleti API app funkciókat biztosítja a itt leírtak – két vezérlők:</span><span class="sxs-lookup"><span data-stu-id="018ce-190">Our open-source Azure Logic Apps exception management API app provides functionality as described here - there are two controllers:</span></span>

* <span data-ttu-id="018ce-191">**ErrorController** egy hibarekordot (dokumentumok) szúr be egy DocumentDB-gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="018ce-191">**ErrorController** inserts an error record (document) in a DocumentDB collection.</span></span>
* <span data-ttu-id="018ce-192">**LogController** egy naplóbejegyzést képviselnek (dokumentumok) szúr be egy DocumentDB-gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="018ce-192">**LogController** Inserts a log record (document) in a DocumentDB collection.</span></span>

> [!TIP]
> <span data-ttu-id="018ce-193">Mindkét vezérlők használata `async Task<dynamic>` műveletek, lehetővé téve a műveletek tooresolve futásidőben, így hozhatunk létre hello DocumentDB séma hello művelet hello törzsében.</span><span class="sxs-lookup"><span data-stu-id="018ce-193">Both controllers use `async Task<dynamic>` operations, allowing operations tooresolve at runtime, so we can create hello DocumentDB schema in hello body of hello operation.</span></span> 
> 

<span data-ttu-id="018ce-194">Minden dokumentumot a documentdb egy egyedi azonosítóval kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="018ce-194">Every document in DocumentDB must have a unique ID.</span></span> <span data-ttu-id="018ce-195">Használjuk `PatientId` és hozzáadása, amely időbélyeg tooa Unix timestamp érték (kétirányú) alakítja át.</span><span class="sxs-lookup"><span data-stu-id="018ce-195">We are using `PatientId` and adding a timestamp that is converted tooa Unix timestamp value (double).</span></span> <span data-ttu-id="018ce-196">Hello tooremove hello tört érték csonkolja azt.</span><span class="sxs-lookup"><span data-stu-id="018ce-196">We truncate hello value tooremove hello fractional value.</span></span>

<span data-ttu-id="018ce-197">Megtekintheti a hiba vezérlőhöz API hello forráskódját [a Githubról](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).</span><span class="sxs-lookup"><span data-stu-id="018ce-197">You can view hello source code of our error controller API [from GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).</span></span>

<span data-ttu-id="018ce-198">A logikai alkalmazás API hello nevezzük hello a következő szintaxis használatával:</span><span class="sxs-lookup"><span data-stu-id="018ce-198">We call hello API from a logic app by using hello following syntax:</span></span>

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

<span data-ttu-id="018ce-199">a kód a minta ellenőrzések hello megelőző hello kifejezés hello *Create_NewPatientRecord* állapotának **sikertelen**.</span><span class="sxs-lookup"><span data-stu-id="018ce-199">hello expression in hello preceding code sample checks for hello *Create_NewPatientRecord* status of **Failed**.</span></span>

## <a name="summary"></a><span data-ttu-id="018ce-200">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="018ce-200">Summary</span></span>

* <span data-ttu-id="018ce-201">Naplózás és a logikai alkalmazás hibakezelési könnyedén alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="018ce-201">You can easily implement logging and error handling in a logic app.</span></span>
* <span data-ttu-id="018ce-202">A DocumentDB hello tárház védelemként is alkalmazhatják a napló és a hiba a rekordok (dokumentumok).</span><span class="sxs-lookup"><span data-stu-id="018ce-202">You can use DocumentDB as hello repository for log and error records (documents).</span></span>
* <span data-ttu-id="018ce-203">MVC toocreate portál toodisplay a napló és a hiba is használhatja.</span><span class="sxs-lookup"><span data-stu-id="018ce-203">You can use MVC toocreate a portal toodisplay log and error records.</span></span>

### <a name="source-code"></a><span data-ttu-id="018ce-204">Forráskód</span><span class="sxs-lookup"><span data-stu-id="018ce-204">Source code</span></span>

<span data-ttu-id="018ce-205">hello Logic Apps kivétel felügyeleti API-alkalmazás forráskódja hello érhető el ezen [GitHub-tárházban](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logic App kivétel felügyeleti API").</span><span class="sxs-lookup"><span data-stu-id="018ce-205">hello source code for hello Logic Apps exception management API application is available in this [GitHub repository](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logic App Exception Management API").</span></span>

## <a name="next-steps"></a><span data-ttu-id="018ce-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="018ce-206">Next steps</span></span>

* [<span data-ttu-id="018ce-207">További logic app példák és forgatókönyvek megtekintése</span><span class="sxs-lookup"><span data-stu-id="018ce-207">View more logic app examples and scenarios</span></span>](../logic-apps/logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="018ce-208">További információk a logic Apps alkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="018ce-208">Learn about monitoring logic apps</span></span>](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [<span data-ttu-id="018ce-209">A logic Apps alkalmazások automatikus központi telepítési sablonok létrehozása</span><span class="sxs-lookup"><span data-stu-id="018ce-209">Create automated deployment templates for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
