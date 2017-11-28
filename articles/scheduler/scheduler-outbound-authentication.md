---
title: "Kimenő hitelesítési aaaScheduler"
description: "A Feladatütemező kimenő hitelesítésre"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 6707f82b-7e32-401b-a960-02aae7bb59cc
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2016
ms.author: deli
ms.openlocfilehash: ef713f4770b48d0a9176415e87c1042a823582e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-outbound-authentication"></a><span data-ttu-id="39b15-103">A Feladatütemező kimenő hitelesítésre</span><span class="sxs-lookup"><span data-stu-id="39b15-103">Scheduler Outbound Authentication</span></span>
<span data-ttu-id="39b15-104">A Feladatütemező szolgáltatás esetleg toocall hitelesítést igénylő tooservices ki.</span><span class="sxs-lookup"><span data-stu-id="39b15-104">Scheduler jobs may need toocall out tooservices that require authentication.</span></span> <span data-ttu-id="39b15-105">Ezzel a módszerrel a hívott szolgáltatás is meghatározhatja, ha hello Scheduler feladatot az erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="39b15-105">This way, a called service can determine if hello Scheduler job can access its resources.</span></span> <span data-ttu-id="39b15-106">Ezek a szolgáltatások többek között más Azure-szolgáltatások, a Salesforce.com, a Facebook-on és a biztonságos egyéni webhelyeket.</span><span class="sxs-lookup"><span data-stu-id="39b15-106">Some of these services include other Azure services, Salesforce.com, Facebook, and secure custom websites.</span></span>

## <a name="adding-and-removing-authentication"></a><span data-ttu-id="39b15-107">Hozzáadása és eltávolítása a hitelesítés</span><span class="sxs-lookup"><span data-stu-id="39b15-107">Adding and Removing Authentication</span></span>
<span data-ttu-id="39b15-108">Hitelesítési tooa ütemezési feladat felettébb egyszerű – vegyen fel egy JSON gyermek hozzáadása `authentication` toohello `request` elem létrehozásakor vagy frissít egy feladatot.</span><span class="sxs-lookup"><span data-stu-id="39b15-108">Adding authentication tooa Scheduler job is simple – add a JSON child element `authentication` toohello `request` element when creating or updating a job.</span></span> <span data-ttu-id="39b15-109">Titkos kulcsok megnevezése toohello a Feladatütemező szolgáltatás PUT, a javítás vagy a POST kérelemben – hello `authentication` objektum – soha ne vissza válaszokat.</span><span class="sxs-lookup"><span data-stu-id="39b15-109">Secrets passed toohello Scheduler service in a PUT, PATCH, or POST request – as part of hello `authentication` object – are never returned in responses.</span></span> <span data-ttu-id="39b15-110">A válaszokat titkos információk toonull vagy előfordulhat, hogy rendelkezik egy nyilvános hitelesített hello entitás jelképező jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="39b15-110">In responses, secret information is set toonull or may have a public token that represents hello authenticated entity.</span></span>

<span data-ttu-id="39b15-111">tooremove hitelesítési, PUT vagy hello feladat explicit módon, javítás hello beállítása `authentication` toonull objektum.</span><span class="sxs-lookup"><span data-stu-id="39b15-111">tooremove authentication, PUT or PATCH hello job explicitly, setting hello `authentication` object toonull.</span></span> <span data-ttu-id="39b15-112">Nem jelenik meg a hitelesítési tulajdonságai vissza válaszként.</span><span class="sxs-lookup"><span data-stu-id="39b15-112">You will not see any authentication properties back in response.</span></span>

<span data-ttu-id="39b15-113">Csak akkor támogatott hello hitelesítési típusok jelenleg hello `ClientCertificate` modellje (hello SSL/TLS ügyféltanúsítványok segítségével), a hello `Basic` modell (az egyszerű hitelesítés), és hello `ActiveDirectoryOAuth` (az Active Directory OAuth modell hitelesítési.)</span><span class="sxs-lookup"><span data-stu-id="39b15-113">Currently, hello only supported authentication types are hello `ClientCertificate` model (for using hello SSL/TLS client certificates), hello `Basic` model (for Basic authentication), and hello `ActiveDirectoryOAuth` model (for Active Directory OAuth authentication.)</span></span>

## <a name="request-body-for-clientcertificate-authentication"></a><span data-ttu-id="39b15-114">Kérelem törzse ClientCertificate hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="39b15-114">Request Body for ClientCertificate Authentication</span></span>
<span data-ttu-id="39b15-115">Hitelesítés hello hozzáadásakor `ClientCertificate` modell, adja meg a következő további elemek hello kérés törzsében hello.</span><span class="sxs-lookup"><span data-stu-id="39b15-115">When adding authentication using hello `ClientCertificate` model, specify hello following additional elements in hello request body.</span></span>  

| <span data-ttu-id="39b15-116">Elem</span><span class="sxs-lookup"><span data-stu-id="39b15-116">Element</span></span> | <span data-ttu-id="39b15-117">Leírás</span><span class="sxs-lookup"><span data-stu-id="39b15-117">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="39b15-118">*hitelesítési (szülőelem)*</span><span class="sxs-lookup"><span data-stu-id="39b15-118">*authentication (parent element)*</span></span> |<span data-ttu-id="39b15-119">Hitelesítési objektum ügyfél SSL-tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="39b15-119">Authentication object for using an SSL client certificate.</span></span> |
| <span data-ttu-id="39b15-120">*típusa*</span><span class="sxs-lookup"><span data-stu-id="39b15-120">*type*</span></span> |<span data-ttu-id="39b15-121">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="39b15-121">Required.</span></span> <span data-ttu-id="39b15-122">Hitelesítés típusa. Az ügyfél SSL-tanúsítványok, hello értéknek kell lennie `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="39b15-122">Type of authentication.For SSL client certificates, hello value must be `ClientCertificate`.</span></span> |
| <span data-ttu-id="39b15-123">*PFX*</span><span class="sxs-lookup"><span data-stu-id="39b15-123">*pfx*</span></span> |<span data-ttu-id="39b15-124">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="39b15-124">Required.</span></span> <span data-ttu-id="39b15-125">Base64-kódolású tartalmak hello PFX-fájl.</span><span class="sxs-lookup"><span data-stu-id="39b15-125">Base64-encoded contents of hello PFX file.</span></span> |
| <span data-ttu-id="39b15-126">*jelszó*</span><span class="sxs-lookup"><span data-stu-id="39b15-126">*password*</span></span> |<span data-ttu-id="39b15-127">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="39b15-127">Required.</span></span> <span data-ttu-id="39b15-128">Jelszó tooaccess hello PFX-fájlt.</span><span class="sxs-lookup"><span data-stu-id="39b15-128">Password tooaccess hello PFX file.</span></span> |

## <a name="response-body-for-clientcertificate-authentication"></a><span data-ttu-id="39b15-129">Választörzs ClientCertificate hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="39b15-129">Response Body for ClientCertificate Authentication</span></span>
<span data-ttu-id="39b15-130">Egy kérelmet küldött hitelesítési információval, hello válasz tartalmazza a következő hitelesítési kapcsolódó elemeket hello.</span><span class="sxs-lookup"><span data-stu-id="39b15-130">When a request is sent with authentication info, hello response contains hello following authentication-related elements.</span></span>

| <span data-ttu-id="39b15-131">Elem</span><span class="sxs-lookup"><span data-stu-id="39b15-131">Element</span></span> | <span data-ttu-id="39b15-132">Leírás</span><span class="sxs-lookup"><span data-stu-id="39b15-132">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="39b15-133">*hitelesítési (szülőelem)*</span><span class="sxs-lookup"><span data-stu-id="39b15-133">*authentication (parent element)*</span></span> |<span data-ttu-id="39b15-134">Hitelesítési objektum ügyfél SSL-tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="39b15-134">Authentication object for using an SSL client certificate.</span></span> |
| <span data-ttu-id="39b15-135">*típusa*</span><span class="sxs-lookup"><span data-stu-id="39b15-135">*type*</span></span> |<span data-ttu-id="39b15-136">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="39b15-136">Type of authentication.</span></span> <span data-ttu-id="39b15-137">Az ügyfél SSL-tanúsítványok, hello értéke `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="39b15-137">For SSL client certificates, hello value is `ClientCertificate`.</span></span> |
| <span data-ttu-id="39b15-138">*certificateThumbprint*</span><span class="sxs-lookup"><span data-stu-id="39b15-138">*certificateThumbprint*</span></span> |<span data-ttu-id="39b15-139">hello hello tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="39b15-139">hello thumbprint of hello certificate.</span></span> |
| <span data-ttu-id="39b15-140">*certificateSubjectName*</span><span class="sxs-lookup"><span data-stu-id="39b15-140">*certificateSubjectName*</span></span> |<span data-ttu-id="39b15-141">hello megkülönböztető hello tanúsítvány tulajdonosnevét.</span><span class="sxs-lookup"><span data-stu-id="39b15-141">hello subject distinguished name of hello certificate.</span></span> |
| <span data-ttu-id="39b15-142">*certificateExpiration*</span><span class="sxs-lookup"><span data-stu-id="39b15-142">*certificateExpiration*</span></span> |<span data-ttu-id="39b15-143">hello hello tanúsítvány lejárati dátumát.</span><span class="sxs-lookup"><span data-stu-id="39b15-143">hello expiration date of hello certificate.</span></span> |

## <a name="sample-rest-request-for-clientcertificate-authentication"></a><span data-ttu-id="39b15-144">Mintakérelem REST ClientCertificate hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="39b15-144">Sample REST Request for ClientCertificate Authentication</span></span>
```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a><span data-ttu-id="39b15-145">Mintaválasz REST ClientCertificate hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="39b15-145">Sample REST Response for ClientCertificate Authentication</span></span>
```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a><span data-ttu-id="39b15-146">Egyszerű hitelesítés esetén a kérelem törzsében</span><span class="sxs-lookup"><span data-stu-id="39b15-146">Request Body for Basic Authentication</span></span>
<span data-ttu-id="39b15-147">Hitelesítés hello hozzáadásakor `Basic` modell, adja meg a következő további elemek hello kérés törzsében hello.</span><span class="sxs-lookup"><span data-stu-id="39b15-147">When adding authentication using hello `Basic` model, specify hello following additional elements in hello request body.</span></span>

| <span data-ttu-id="39b15-148">Elem</span><span class="sxs-lookup"><span data-stu-id="39b15-148">Element</span></span> | <span data-ttu-id="39b15-149">Leírás</span><span class="sxs-lookup"><span data-stu-id="39b15-149">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="39b15-150">*hitelesítési (szülőelem)*</span><span class="sxs-lookup"><span data-stu-id="39b15-150">*authentication (parent element)*</span></span> |<span data-ttu-id="39b15-151">Hitelesítési objektumot az alapszintű hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="39b15-151">Authentication object for using Basic authentication.</span></span> |
| <span data-ttu-id="39b15-152">*típusa*</span><span class="sxs-lookup"><span data-stu-id="39b15-152">*type*</span></span> |<span data-ttu-id="39b15-153">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="39b15-153">Required.</span></span> <span data-ttu-id="39b15-154">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="39b15-154">Type of authentication.</span></span> <span data-ttu-id="39b15-155">Az egyszerű hitelesítés hello értéknek kell lennie `Basic`.</span><span class="sxs-lookup"><span data-stu-id="39b15-155">For Basic authentication, hello value must be `Basic`.</span></span> |
| <span data-ttu-id="39b15-156">*felhasználónév*</span><span class="sxs-lookup"><span data-stu-id="39b15-156">*username*</span></span> |<span data-ttu-id="39b15-157">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="39b15-157">Required.</span></span> <span data-ttu-id="39b15-158">Felhasználónév tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="39b15-158">Username tooauthenticate.</span></span> |
| <span data-ttu-id="39b15-159">*jelszó*</span><span class="sxs-lookup"><span data-stu-id="39b15-159">*password*</span></span> |<span data-ttu-id="39b15-160">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="39b15-160">Required.</span></span> <span data-ttu-id="39b15-161">Jelszó tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="39b15-161">Password tooauthenticate.</span></span> |

## <a name="response-body-for-basic-authentication"></a><span data-ttu-id="39b15-162">Egyszerű hitelesítés esetén adott válasz törzse</span><span class="sxs-lookup"><span data-stu-id="39b15-162">Response Body for Basic Authentication</span></span>
<span data-ttu-id="39b15-163">Egy kérelmet küldött hitelesítési információval, hello válasz tartalmazza a következő hitelesítési kapcsolódó elemeket hello.</span><span class="sxs-lookup"><span data-stu-id="39b15-163">When a request is sent with authentication info, hello response contains hello following authentication-related elements.</span></span>

| <span data-ttu-id="39b15-164">Elem</span><span class="sxs-lookup"><span data-stu-id="39b15-164">Element</span></span> | <span data-ttu-id="39b15-165">Leírás</span><span class="sxs-lookup"><span data-stu-id="39b15-165">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="39b15-166">*hitelesítési (szülőelem)*</span><span class="sxs-lookup"><span data-stu-id="39b15-166">*authentication (parent element)*</span></span> |<span data-ttu-id="39b15-167">Hitelesítési objektumot az alapszintű hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="39b15-167">Authentication object for using Basic authentication.</span></span> |
| <span data-ttu-id="39b15-168">*típusa*</span><span class="sxs-lookup"><span data-stu-id="39b15-168">*type*</span></span> |<span data-ttu-id="39b15-169">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="39b15-169">Type of authentication.</span></span> <span data-ttu-id="39b15-170">Az egyszerű hitelesítés hello értéke `Basic`.</span><span class="sxs-lookup"><span data-stu-id="39b15-170">For Basic authentication, hello value is `Basic`.</span></span> |
| <span data-ttu-id="39b15-171">*felhasználónév*</span><span class="sxs-lookup"><span data-stu-id="39b15-171">*username*</span></span> |<span data-ttu-id="39b15-172">hello hitelesített felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="39b15-172">hello authenticated username.</span></span> |

## <a name="sample-rest-request-for-basic-authentication"></a><span data-ttu-id="39b15-173">Az egyszerű hitelesítés REST Mintakérelem</span><span class="sxs-lookup"><span data-stu-id="39b15-173">Sample REST Request for Basic Authentication</span></span>
```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a><span data-ttu-id="39b15-174">Az egyszerű hitelesítés REST Mintaválasz</span><span class="sxs-lookup"><span data-stu-id="39b15-174">Sample REST Response for Basic Authentication</span></span>
```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a><span data-ttu-id="39b15-175">Kérelem törzse ActiveDirectoryOAuth hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="39b15-175">Request Body for ActiveDirectoryOAuth Authentication</span></span>
<span data-ttu-id="39b15-176">Hitelesítés hello hozzáadásakor `ActiveDirectoryOAuth` modell, adja meg a következő további elemek hello kérés törzsében hello.</span><span class="sxs-lookup"><span data-stu-id="39b15-176">When adding authentication using hello `ActiveDirectoryOAuth` model, specify hello following additional elements in hello request body.</span></span>

| <span data-ttu-id="39b15-177">Elem</span><span class="sxs-lookup"><span data-stu-id="39b15-177">Element</span></span> | <span data-ttu-id="39b15-178">Leírás</span><span class="sxs-lookup"><span data-stu-id="39b15-178">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="39b15-179">*hitelesítési (szülőelem)*</span><span class="sxs-lookup"><span data-stu-id="39b15-179">*authentication (parent element)*</span></span> |<span data-ttu-id="39b15-180">Hitelesítési objektum ActiveDirectoryOAuth hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="39b15-180">Authentication object for using ActiveDirectoryOAuth authentication.</span></span> |
| <span data-ttu-id="39b15-181">*típusa*</span><span class="sxs-lookup"><span data-stu-id="39b15-181">*type*</span></span> |<span data-ttu-id="39b15-182">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="39b15-182">Required.</span></span> <span data-ttu-id="39b15-183">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="39b15-183">Type of authentication.</span></span> <span data-ttu-id="39b15-184">ActiveDirectoryOAuth hitelesítéshez hello értéknek kell lennie `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="39b15-184">For ActiveDirectoryOAuth authentication, hello value must be `ActiveDirectoryOAuth`.</span></span> |
| <span data-ttu-id="39b15-185">*Bérlői*</span><span class="sxs-lookup"><span data-stu-id="39b15-185">*tenant*</span></span> |<span data-ttu-id="39b15-186">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="39b15-186">Required.</span></span> <span data-ttu-id="39b15-187">hello Azure AD-bérlő hello a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="39b15-187">hello tenant identifier for hello Azure AD tenant.</span></span> |
| <span data-ttu-id="39b15-188">*Célközönség*</span><span class="sxs-lookup"><span data-stu-id="39b15-188">*audience*</span></span> |<span data-ttu-id="39b15-189">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="39b15-189">Required.</span></span> <span data-ttu-id="39b15-190">Toohttps://management.core.windows.net/ értéke.</span><span class="sxs-lookup"><span data-stu-id="39b15-190">This is set toohttps://management.core.windows.net/.</span></span> |
| <span data-ttu-id="39b15-191">*clientId*</span><span class="sxs-lookup"><span data-stu-id="39b15-191">*clientId*</span></span> |<span data-ttu-id="39b15-192">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="39b15-192">Required.</span></span> <span data-ttu-id="39b15-193">Adja meg hello ügyfél-azonosítókra hello Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="39b15-193">Provide hello client identifier for hello Azure AD application.</span></span> |
| <span data-ttu-id="39b15-194">*titkos kulcs*</span><span class="sxs-lookup"><span data-stu-id="39b15-194">*secret*</span></span> |<span data-ttu-id="39b15-195">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="39b15-195">Required.</span></span> <span data-ttu-id="39b15-196">Titkos kulcs hello jogkivonatot kérő hello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="39b15-196">Secret of hello client that is requesting hello token.</span></span> |

### <a name="determining-your-tenant-identifier"></a><span data-ttu-id="39b15-197">A bérlői azonosító meghatározása</span><span class="sxs-lookup"><span data-stu-id="39b15-197">Determining your Tenant Identifier</span></span>
<span data-ttu-id="39b15-198">Hello bérlői azonosító hello Azure AD-bérlő futtatásával található `Get-AzureAccount` az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="39b15-198">You can find hello tenant identifier for hello Azure AD tenant by running `Get-AzureAccount` in Azure PowerShell.</span></span>

## <a name="response-body-for-activedirectoryoauth-authentication"></a><span data-ttu-id="39b15-199">Választörzs ActiveDirectoryOAuth hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="39b15-199">Response Body for ActiveDirectoryOAuth Authentication</span></span>
<span data-ttu-id="39b15-200">Egy kérelmet küldött hitelesítési információval, hello válasz tartalmazza a következő hitelesítési kapcsolódó elemeket hello.</span><span class="sxs-lookup"><span data-stu-id="39b15-200">When a request is sent with authentication info, hello response contains hello following authentication-related elements.</span></span>

| <span data-ttu-id="39b15-201">Elem</span><span class="sxs-lookup"><span data-stu-id="39b15-201">Element</span></span> | <span data-ttu-id="39b15-202">Leírás</span><span class="sxs-lookup"><span data-stu-id="39b15-202">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="39b15-203">*hitelesítési (szülőelem)*</span><span class="sxs-lookup"><span data-stu-id="39b15-203">*authentication (parent element)*</span></span> |<span data-ttu-id="39b15-204">Hitelesítési objektum ActiveDirectoryOAuth hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="39b15-204">Authentication object for using ActiveDirectoryOAuth authentication.</span></span> |
| <span data-ttu-id="39b15-205">*típusa*</span><span class="sxs-lookup"><span data-stu-id="39b15-205">*type*</span></span> |<span data-ttu-id="39b15-206">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="39b15-206">Type of authentication.</span></span> <span data-ttu-id="39b15-207">ActiveDirectoryOAuth hitelesítéshez hello értéke `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="39b15-207">For ActiveDirectoryOAuth authentication, hello value is `ActiveDirectoryOAuth`.</span></span> |
| <span data-ttu-id="39b15-208">*Bérlői*</span><span class="sxs-lookup"><span data-stu-id="39b15-208">*tenant*</span></span> |<span data-ttu-id="39b15-209">hello Azure AD-bérlő hello a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="39b15-209">hello tenant identifier for hello Azure AD tenant.</span></span> |
| <span data-ttu-id="39b15-210">*Célközönség*</span><span class="sxs-lookup"><span data-stu-id="39b15-210">*audience*</span></span> |<span data-ttu-id="39b15-211">Toohttps://management.core.windows.net/ értéke.</span><span class="sxs-lookup"><span data-stu-id="39b15-211">This is set toohttps://management.core.windows.net/.</span></span> |
| <span data-ttu-id="39b15-212">*clientId*</span><span class="sxs-lookup"><span data-stu-id="39b15-212">*clientId*</span></span> |<span data-ttu-id="39b15-213">hello hello Azure AD alkalmazás ügyfél-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="39b15-213">hello client identifier for hello Azure AD application.</span></span> |

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a><span data-ttu-id="39b15-214">ActiveDirectoryOAuth hitelesítési REST Mintakérelem</span><span class="sxs-lookup"><span data-stu-id="39b15-214">Sample REST Request for ActiveDirectoryOAuth Authentication</span></span>
```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a><span data-ttu-id="39b15-215">ActiveDirectoryOAuth hitelesítési REST Mintaválasz</span><span class="sxs-lookup"><span data-stu-id="39b15-215">Sample REST Response for ActiveDirectoryOAuth Authentication</span></span>
```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a><span data-ttu-id="39b15-216">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="39b15-216">See Also</span></span>
 [<span data-ttu-id="39b15-217">A Scheduler ismertetése</span><span class="sxs-lookup"><span data-stu-id="39b15-217">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="39b15-218">Az Azure Scheduler alapfogalmai, terminológiája és entitáshierarchiája</span><span class="sxs-lookup"><span data-stu-id="39b15-218">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="39b15-219">Az ütemező hello Azure-portálon az első lépéseiben</span><span class="sxs-lookup"><span data-stu-id="39b15-219">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="39b15-220">Csomagok és számlázás az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="39b15-220">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="39b15-221">Az Azure Scheduler REST API-jának leírása</span><span class="sxs-lookup"><span data-stu-id="39b15-221">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="39b15-222">Az Azure Scheduler PowerShell-parancsmagjainak leírása</span><span class="sxs-lookup"><span data-stu-id="39b15-222">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="39b15-223">Azure Scheduler – magas fokú rendelkezésre állás és megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="39b15-223">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="39b15-224">Azure Scheduler – korlátozások, alapértékek és hibakódok</span><span class="sxs-lookup"><span data-stu-id="39b15-224">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

