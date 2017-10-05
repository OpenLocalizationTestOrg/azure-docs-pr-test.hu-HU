---
title: "A Feladatütemező kimenő hitelesítésre"
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
ms.openlocfilehash: e345b2e22daae5b24c23645f7d2636f66df630ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="scheduler-outbound-authentication"></a><span data-ttu-id="e1e54-103">A Feladatütemező kimenő hitelesítésre</span><span class="sxs-lookup"><span data-stu-id="e1e54-103">Scheduler Outbound Authentication</span></span>
<span data-ttu-id="e1e54-104">A Feladatütemező szolgáltatás kell hitelesítést igénylő szolgáltatások hívásához.</span><span class="sxs-lookup"><span data-stu-id="e1e54-104">Scheduler jobs may need to call out to services that require authentication.</span></span> <span data-ttu-id="e1e54-105">Ezzel a módszerrel a hívott szolgáltatás is meghatározhatja, ha az ütemezési feladat az erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="e1e54-105">This way, a called service can determine if the Scheduler job can access its resources.</span></span> <span data-ttu-id="e1e54-106">Ezek a szolgáltatások többek között más Azure-szolgáltatások, a Salesforce.com, a Facebook-on és a biztonságos egyéni webhelyeket.</span><span class="sxs-lookup"><span data-stu-id="e1e54-106">Some of these services include other Azure services, Salesforce.com, Facebook, and secure custom websites.</span></span>

## <a name="adding-and-removing-authentication"></a><span data-ttu-id="e1e54-107">Hozzáadása és eltávolítása a hitelesítés</span><span class="sxs-lookup"><span data-stu-id="e1e54-107">Adding and Removing Authentication</span></span>
<span data-ttu-id="e1e54-108">A Feladatütemező feladathoz a hitelesítés az egyszerű – vegyen fel egy JSON gyermek hozzáadása `authentication` való a `request` elem létrehozásakor vagy frissít egy feladatot.</span><span class="sxs-lookup"><span data-stu-id="e1e54-108">Adding authentication to a Scheduler job is simple – add a JSON child element `authentication` to the `request` element when creating or updating a job.</span></span> <span data-ttu-id="e1e54-109">A PUT, a javítás vagy a POST kérelemben – a Feladatütemező szolgáltatás részeként átadott titkokat a `authentication` objektum – soha ne vissza válaszokat.</span><span class="sxs-lookup"><span data-stu-id="e1e54-109">Secrets passed to the Scheduler service in a PUT, PATCH, or POST request – as part of the `authentication` object – are never returned in responses.</span></span> <span data-ttu-id="e1e54-110">A válaszokat, titkos információk értéke null, vagy nem rendelkezik nyilvános a hitelesített entitás jelképező jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="e1e54-110">In responses, secret information is set to null or may have a public token that represents the authenticated entity.</span></span>

<span data-ttu-id="e1e54-111">Távolítsa el a hitelesítést, PUT, vagy a feladat explicit módon, javítás beállítása a `authentication` objektum null értékű.</span><span class="sxs-lookup"><span data-stu-id="e1e54-111">To remove authentication, PUT or PATCH the job explicitly, setting the `authentication` object to null.</span></span> <span data-ttu-id="e1e54-112">Nem jelenik meg a hitelesítési tulajdonságai vissza válaszként.</span><span class="sxs-lookup"><span data-stu-id="e1e54-112">You will not see any authentication properties back in response.</span></span>

<span data-ttu-id="e1e54-113">Az egyetlen támogatott hitelesítési típusok jelenleg a `ClientCertificate` modell (a "az ügyfél SSL/TLS-tanúsítványok használatával), a `Basic` modell (az egyszerű hitelesítés), és a `ActiveDirectoryOAuth` modell (az Active Directory OAuth hitelesítési.)</span><span class="sxs-lookup"><span data-stu-id="e1e54-113">Currently, the only supported authentication types are the `ClientCertificate` model (for using the SSL/TLS client certificates), the `Basic` model (for Basic authentication), and the `ActiveDirectoryOAuth` model (for Active Directory OAuth authentication.)</span></span>

## <a name="request-body-for-clientcertificate-authentication"></a><span data-ttu-id="e1e54-114">Kérelem törzse ClientCertificate hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="e1e54-114">Request Body for ClientCertificate Authentication</span></span>
<span data-ttu-id="e1e54-115">Hitelesítés hozzáadása során a `ClientCertificate` modell, adja meg a következő további elemeket a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="e1e54-115">When adding authentication using the `ClientCertificate` model, specify the following additional elements in the request body.</span></span>  

| <span data-ttu-id="e1e54-116">Elem</span><span class="sxs-lookup"><span data-stu-id="e1e54-116">Element</span></span> | <span data-ttu-id="e1e54-117">Leírás</span><span class="sxs-lookup"><span data-stu-id="e1e54-117">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e1e54-118">*hitelesítési (szülőelem)*</span><span class="sxs-lookup"><span data-stu-id="e1e54-118">*authentication (parent element)*</span></span> |<span data-ttu-id="e1e54-119">Hitelesítési objektum ügyfél SSL-tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="e1e54-119">Authentication object for using an SSL client certificate.</span></span> |
| <span data-ttu-id="e1e54-120">*típusa*</span><span class="sxs-lookup"><span data-stu-id="e1e54-120">*type*</span></span> |<span data-ttu-id="e1e54-121">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="e1e54-121">Required.</span></span> <span data-ttu-id="e1e54-122">Hitelesítés típusa. Az ügyfél SSL-tanúsítványok, az értéknek kell lennie `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="e1e54-122">Type of authentication.For SSL client certificates, the value must be `ClientCertificate`.</span></span> |
| <span data-ttu-id="e1e54-123">*PFX*</span><span class="sxs-lookup"><span data-stu-id="e1e54-123">*pfx*</span></span> |<span data-ttu-id="e1e54-124">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="e1e54-124">Required.</span></span> <span data-ttu-id="e1e54-125">A PFX-fájl Base64-kódolású tartalmát.</span><span class="sxs-lookup"><span data-stu-id="e1e54-125">Base64-encoded contents of the PFX file.</span></span> |
| <span data-ttu-id="e1e54-126">*jelszó*</span><span class="sxs-lookup"><span data-stu-id="e1e54-126">*password*</span></span> |<span data-ttu-id="e1e54-127">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="e1e54-127">Required.</span></span> <span data-ttu-id="e1e54-128">A jelszó megadásával érheti el a PFX-fájlból.</span><span class="sxs-lookup"><span data-stu-id="e1e54-128">Password to access the PFX file.</span></span> |

## <a name="response-body-for-clientcertificate-authentication"></a><span data-ttu-id="e1e54-129">Választörzs ClientCertificate hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="e1e54-129">Response Body for ClientCertificate Authentication</span></span>
<span data-ttu-id="e1e54-130">Kérelmet küldött hitelesítési információval, akkor a válasz a következő hitelesítési kapcsolódó elemeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e1e54-130">When a request is sent with authentication info, the response contains the following authentication-related elements.</span></span>

| <span data-ttu-id="e1e54-131">Elem</span><span class="sxs-lookup"><span data-stu-id="e1e54-131">Element</span></span> | <span data-ttu-id="e1e54-132">Leírás</span><span class="sxs-lookup"><span data-stu-id="e1e54-132">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e1e54-133">*hitelesítési (szülőelem)*</span><span class="sxs-lookup"><span data-stu-id="e1e54-133">*authentication (parent element)*</span></span> |<span data-ttu-id="e1e54-134">Hitelesítési objektum ügyfél SSL-tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="e1e54-134">Authentication object for using an SSL client certificate.</span></span> |
| <span data-ttu-id="e1e54-135">*típusa*</span><span class="sxs-lookup"><span data-stu-id="e1e54-135">*type*</span></span> |<span data-ttu-id="e1e54-136">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="e1e54-136">Type of authentication.</span></span> <span data-ttu-id="e1e54-137">Az ügyfél SSL-tanúsítványok, a értéke `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="e1e54-137">For SSL client certificates, the value is `ClientCertificate`.</span></span> |
| <span data-ttu-id="e1e54-138">*certificateThumbprint*</span><span class="sxs-lookup"><span data-stu-id="e1e54-138">*certificateThumbprint*</span></span> |<span data-ttu-id="e1e54-139">A tanúsítvány ujjlenyomatát.</span><span class="sxs-lookup"><span data-stu-id="e1e54-139">The thumbprint of the certificate.</span></span> |
| <span data-ttu-id="e1e54-140">*certificateSubjectName*</span><span class="sxs-lookup"><span data-stu-id="e1e54-140">*certificateSubjectName*</span></span> |<span data-ttu-id="e1e54-141">Tulajdonosának megkülönböztető neve a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="e1e54-141">The subject distinguished name of the certificate.</span></span> |
| <span data-ttu-id="e1e54-142">*certificateExpiration*</span><span class="sxs-lookup"><span data-stu-id="e1e54-142">*certificateExpiration*</span></span> |<span data-ttu-id="e1e54-143">A tanúsítvány lejárati dátuma.</span><span class="sxs-lookup"><span data-stu-id="e1e54-143">The expiration date of the certificate.</span></span> |

## <a name="sample-rest-request-for-clientcertificate-authentication"></a><span data-ttu-id="e1e54-144">Mintakérelem REST ClientCertificate hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="e1e54-144">Sample REST Request for ClientCertificate Authentication</span></span>
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

## <a name="sample-rest-response-for-clientcertificate-authentication"></a><span data-ttu-id="e1e54-145">Mintaválasz REST ClientCertificate hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="e1e54-145">Sample REST Response for ClientCertificate Authentication</span></span>
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

## <a name="request-body-for-basic-authentication"></a><span data-ttu-id="e1e54-146">Egyszerű hitelesítés esetén a kérelem törzsében</span><span class="sxs-lookup"><span data-stu-id="e1e54-146">Request Body for Basic Authentication</span></span>
<span data-ttu-id="e1e54-147">Hitelesítés hozzáadása során a `Basic` modell, adja meg a következő további elemeket a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="e1e54-147">When adding authentication using the `Basic` model, specify the following additional elements in the request body.</span></span>

| <span data-ttu-id="e1e54-148">Elem</span><span class="sxs-lookup"><span data-stu-id="e1e54-148">Element</span></span> | <span data-ttu-id="e1e54-149">Leírás</span><span class="sxs-lookup"><span data-stu-id="e1e54-149">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e1e54-150">*hitelesítési (szülőelem)*</span><span class="sxs-lookup"><span data-stu-id="e1e54-150">*authentication (parent element)*</span></span> |<span data-ttu-id="e1e54-151">Hitelesítési objektumot az alapszintű hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="e1e54-151">Authentication object for using Basic authentication.</span></span> |
| <span data-ttu-id="e1e54-152">*típusa*</span><span class="sxs-lookup"><span data-stu-id="e1e54-152">*type*</span></span> |<span data-ttu-id="e1e54-153">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="e1e54-153">Required.</span></span> <span data-ttu-id="e1e54-154">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="e1e54-154">Type of authentication.</span></span> <span data-ttu-id="e1e54-155">Az alapszintű hitelesítés, az értéknek kell lennie `Basic`.</span><span class="sxs-lookup"><span data-stu-id="e1e54-155">For Basic authentication, the value must be `Basic`.</span></span> |
| <span data-ttu-id="e1e54-156">*felhasználónév*</span><span class="sxs-lookup"><span data-stu-id="e1e54-156">*username*</span></span> |<span data-ttu-id="e1e54-157">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="e1e54-157">Required.</span></span> <span data-ttu-id="e1e54-158">Felhasználónév hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="e1e54-158">Username to authenticate.</span></span> |
| <span data-ttu-id="e1e54-159">*jelszó*</span><span class="sxs-lookup"><span data-stu-id="e1e54-159">*password*</span></span> |<span data-ttu-id="e1e54-160">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="e1e54-160">Required.</span></span> <span data-ttu-id="e1e54-161">Jelszó a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="e1e54-161">Password to authenticate.</span></span> |

## <a name="response-body-for-basic-authentication"></a><span data-ttu-id="e1e54-162">Egyszerű hitelesítés esetén adott válasz törzse</span><span class="sxs-lookup"><span data-stu-id="e1e54-162">Response Body for Basic Authentication</span></span>
<span data-ttu-id="e1e54-163">Kérelmet küldött hitelesítési információval, akkor a válasz a következő hitelesítési kapcsolódó elemeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e1e54-163">When a request is sent with authentication info, the response contains the following authentication-related elements.</span></span>

| <span data-ttu-id="e1e54-164">Elem</span><span class="sxs-lookup"><span data-stu-id="e1e54-164">Element</span></span> | <span data-ttu-id="e1e54-165">Leírás</span><span class="sxs-lookup"><span data-stu-id="e1e54-165">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e1e54-166">*hitelesítési (szülőelem)*</span><span class="sxs-lookup"><span data-stu-id="e1e54-166">*authentication (parent element)*</span></span> |<span data-ttu-id="e1e54-167">Hitelesítési objektumot az alapszintű hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="e1e54-167">Authentication object for using Basic authentication.</span></span> |
| <span data-ttu-id="e1e54-168">*típusa*</span><span class="sxs-lookup"><span data-stu-id="e1e54-168">*type*</span></span> |<span data-ttu-id="e1e54-169">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="e1e54-169">Type of authentication.</span></span> <span data-ttu-id="e1e54-170">Az egyszerű hitelesítés értéke `Basic`.</span><span class="sxs-lookup"><span data-stu-id="e1e54-170">For Basic authentication, the value is `Basic`.</span></span> |
| <span data-ttu-id="e1e54-171">*felhasználónév*</span><span class="sxs-lookup"><span data-stu-id="e1e54-171">*username*</span></span> |<span data-ttu-id="e1e54-172">A hitelesített felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="e1e54-172">The authenticated username.</span></span> |

## <a name="sample-rest-request-for-basic-authentication"></a><span data-ttu-id="e1e54-173">Az egyszerű hitelesítés REST Mintakérelem</span><span class="sxs-lookup"><span data-stu-id="e1e54-173">Sample REST Request for Basic Authentication</span></span>
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

## <a name="sample-rest-response-for-basic-authentication"></a><span data-ttu-id="e1e54-174">Az egyszerű hitelesítés REST Mintaválasz</span><span class="sxs-lookup"><span data-stu-id="e1e54-174">Sample REST Response for Basic Authentication</span></span>
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

## <a name="request-body-for-activedirectoryoauth-authentication"></a><span data-ttu-id="e1e54-175">Kérelem törzse ActiveDirectoryOAuth hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="e1e54-175">Request Body for ActiveDirectoryOAuth Authentication</span></span>
<span data-ttu-id="e1e54-176">Hitelesítés hozzáadása során a `ActiveDirectoryOAuth` modell, adja meg a következő további elemeket a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="e1e54-176">When adding authentication using the `ActiveDirectoryOAuth` model, specify the following additional elements in the request body.</span></span>

| <span data-ttu-id="e1e54-177">Elem</span><span class="sxs-lookup"><span data-stu-id="e1e54-177">Element</span></span> | <span data-ttu-id="e1e54-178">Leírás</span><span class="sxs-lookup"><span data-stu-id="e1e54-178">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e1e54-179">*hitelesítési (szülőelem)*</span><span class="sxs-lookup"><span data-stu-id="e1e54-179">*authentication (parent element)*</span></span> |<span data-ttu-id="e1e54-180">Hitelesítési objektum ActiveDirectoryOAuth hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="e1e54-180">Authentication object for using ActiveDirectoryOAuth authentication.</span></span> |
| <span data-ttu-id="e1e54-181">*típusa*</span><span class="sxs-lookup"><span data-stu-id="e1e54-181">*type*</span></span> |<span data-ttu-id="e1e54-182">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="e1e54-182">Required.</span></span> <span data-ttu-id="e1e54-183">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="e1e54-183">Type of authentication.</span></span> <span data-ttu-id="e1e54-184">A ActiveDirectoryOAuth hitelesítéshez, az értéknek kell lennie `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="e1e54-184">For ActiveDirectoryOAuth authentication, the value must be `ActiveDirectoryOAuth`.</span></span> |
| <span data-ttu-id="e1e54-185">*Bérlői*</span><span class="sxs-lookup"><span data-stu-id="e1e54-185">*tenant*</span></span> |<span data-ttu-id="e1e54-186">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="e1e54-186">Required.</span></span> <span data-ttu-id="e1e54-187">A bérlő azonosítója az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="e1e54-187">The tenant identifier for the Azure AD tenant.</span></span> |
| <span data-ttu-id="e1e54-188">*Célközönség*</span><span class="sxs-lookup"><span data-stu-id="e1e54-188">*audience*</span></span> |<span data-ttu-id="e1e54-189">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="e1e54-189">Required.</span></span> <span data-ttu-id="e1e54-190">A beállított érték https://management.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="e1e54-190">This is set to https://management.core.windows.net/.</span></span> |
| <span data-ttu-id="e1e54-191">*clientId*</span><span class="sxs-lookup"><span data-stu-id="e1e54-191">*clientId*</span></span> |<span data-ttu-id="e1e54-192">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="e1e54-192">Required.</span></span> <span data-ttu-id="e1e54-193">Adja meg az ügyfél-azonosító az Azure AD-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e1e54-193">Provide the client identifier for the Azure AD application.</span></span> |
| <span data-ttu-id="e1e54-194">*titkos kulcs*</span><span class="sxs-lookup"><span data-stu-id="e1e54-194">*secret*</span></span> |<span data-ttu-id="e1e54-195">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="e1e54-195">Required.</span></span> <span data-ttu-id="e1e54-196">Az ügyfél a jogkivonatot kérő titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e1e54-196">Secret of the client that is requesting the token.</span></span> |

### <a name="determining-your-tenant-identifier"></a><span data-ttu-id="e1e54-197">A bérlői azonosító meghatározása</span><span class="sxs-lookup"><span data-stu-id="e1e54-197">Determining your Tenant Identifier</span></span>
<span data-ttu-id="e1e54-198">Megtalálhatja a bérlő azonosítóját az Azure AD-bérlő futtatásával `Get-AzureAccount` az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e1e54-198">You can find the tenant identifier for the Azure AD tenant by running `Get-AzureAccount` in Azure PowerShell.</span></span>

## <a name="response-body-for-activedirectoryoauth-authentication"></a><span data-ttu-id="e1e54-199">Választörzs ActiveDirectoryOAuth hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="e1e54-199">Response Body for ActiveDirectoryOAuth Authentication</span></span>
<span data-ttu-id="e1e54-200">Kérelmet küldött hitelesítési információval, akkor a válasz a következő hitelesítési kapcsolódó elemeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e1e54-200">When a request is sent with authentication info, the response contains the following authentication-related elements.</span></span>

| <span data-ttu-id="e1e54-201">Elem</span><span class="sxs-lookup"><span data-stu-id="e1e54-201">Element</span></span> | <span data-ttu-id="e1e54-202">Leírás</span><span class="sxs-lookup"><span data-stu-id="e1e54-202">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e1e54-203">*hitelesítési (szülőelem)*</span><span class="sxs-lookup"><span data-stu-id="e1e54-203">*authentication (parent element)*</span></span> |<span data-ttu-id="e1e54-204">Hitelesítési objektum ActiveDirectoryOAuth hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="e1e54-204">Authentication object for using ActiveDirectoryOAuth authentication.</span></span> |
| <span data-ttu-id="e1e54-205">*típusa*</span><span class="sxs-lookup"><span data-stu-id="e1e54-205">*type*</span></span> |<span data-ttu-id="e1e54-206">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="e1e54-206">Type of authentication.</span></span> <span data-ttu-id="e1e54-207">ActiveDirectoryOAuth hitelesítéshez értéke `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="e1e54-207">For ActiveDirectoryOAuth authentication, the value is `ActiveDirectoryOAuth`.</span></span> |
| <span data-ttu-id="e1e54-208">*Bérlői*</span><span class="sxs-lookup"><span data-stu-id="e1e54-208">*tenant*</span></span> |<span data-ttu-id="e1e54-209">A bérlő azonosítója az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="e1e54-209">The tenant identifier for the Azure AD tenant.</span></span> |
| <span data-ttu-id="e1e54-210">*Célközönség*</span><span class="sxs-lookup"><span data-stu-id="e1e54-210">*audience*</span></span> |<span data-ttu-id="e1e54-211">A beállított érték https://management.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="e1e54-211">This is set to https://management.core.windows.net/.</span></span> |
| <span data-ttu-id="e1e54-212">*clientId*</span><span class="sxs-lookup"><span data-stu-id="e1e54-212">*clientId*</span></span> |<span data-ttu-id="e1e54-213">Az Azure AD-alkalmazás ügyfél-azonosítókra.</span><span class="sxs-lookup"><span data-stu-id="e1e54-213">The client identifier for the Azure AD application.</span></span> |

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a><span data-ttu-id="e1e54-214">ActiveDirectoryOAuth hitelesítési REST Mintakérelem</span><span class="sxs-lookup"><span data-stu-id="e1e54-214">Sample REST Request for ActiveDirectoryOAuth Authentication</span></span>
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

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a><span data-ttu-id="e1e54-215">ActiveDirectoryOAuth hitelesítési REST Mintaválasz</span><span class="sxs-lookup"><span data-stu-id="e1e54-215">Sample REST Response for ActiveDirectoryOAuth Authentication</span></span>
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

## <a name="see-also"></a><span data-ttu-id="e1e54-216">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e1e54-216">See Also</span></span>
 [<span data-ttu-id="e1e54-217">A Scheduler ismertetése</span><span class="sxs-lookup"><span data-stu-id="e1e54-217">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="e1e54-218">Az Azure Scheduler alapfogalmai, terminológiája és entitáshierarchiája</span><span class="sxs-lookup"><span data-stu-id="e1e54-218">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="e1e54-219">Ismerkedés a Scheduler szolgáltatás Azure Portalon való használatával</span><span class="sxs-lookup"><span data-stu-id="e1e54-219">Get started using Scheduler in the Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="e1e54-220">Csomagok és számlázás az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="e1e54-220">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="e1e54-221">Az Azure Scheduler REST API-jának leírása</span><span class="sxs-lookup"><span data-stu-id="e1e54-221">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="e1e54-222">Az Azure Scheduler PowerShell-parancsmagjainak leírása</span><span class="sxs-lookup"><span data-stu-id="e1e54-222">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="e1e54-223">Azure Scheduler – magas fokú rendelkezésre állás és megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="e1e54-223">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="e1e54-224">Azure Scheduler – korlátozások, alapértékek és hibakódok</span><span class="sxs-lookup"><span data-stu-id="e1e54-224">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

