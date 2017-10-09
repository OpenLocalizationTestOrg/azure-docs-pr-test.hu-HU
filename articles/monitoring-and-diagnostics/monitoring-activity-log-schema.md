---
title: "Műveletnapló esemény séma aaaAzure |} Microsoft Docs"
description: "Hello esemény séma hello tevékenységnapló ki adatok ismertetése"
author: johnkemnetz
manager: robb
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: johnkem
ms.openlocfilehash: dfece949a20a4d9b4e8a4d488c1c34842d87d586
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-activity-log-event-schema"></a><span data-ttu-id="06dbe-103">Azure tevékenységnapló esemény séma</span><span class="sxs-lookup"><span data-stu-id="06dbe-103">Azure Activity Log event schema</span></span>
<span data-ttu-id="06dbe-104">Hello **Azure tevékenységnapló** , amely bármely történt az Azure-előfizetés szintű események betekintést biztosít a naplót.</span><span class="sxs-lookup"><span data-stu-id="06dbe-104">hello **Azure Activity Log** is a log that provides insight into any subscription-level events that have occurred in Azure.</span></span> <span data-ttu-id="06dbe-105">Ez a cikk ismerteti az adatok kategóriánként hello esemény séma.</span><span class="sxs-lookup"><span data-stu-id="06dbe-105">This article describes hello event schema per category of data.</span></span>

## <a name="administrative"></a><span data-ttu-id="06dbe-106">Felügyeleti</span><span class="sxs-lookup"><span data-stu-id="06dbe-106">Administrative</span></span>
<span data-ttu-id="06dbe-107">Ez a kategória tartalmazza az összes hello rekordot létrehozása, update, delete és művelet műveleteket Resource Manageren keresztül.</span><span class="sxs-lookup"><span data-stu-id="06dbe-107">This category contains hello record of all create, update, delete, and action operations performed through Resource Manager.</span></span> <span data-ttu-id="06dbe-108">Példát mutatunk be ebbe a kategóriába események típusai hello "virtuális gép létrehozása" és "hálózati biztonsági csoport törlése" minden felhasználó által végrehajtott műveletet, vagy az alkalmazás erőforrás-kezelővel egy adott erőforrástípus művelet van modellezve.</span><span class="sxs-lookup"><span data-stu-id="06dbe-108">Examples of hello types of events you would see in this category include "create virtual machine" and "delete network security group" Every action taken by a user or application using Resource Manager is modeled as an operation on a particular resource type.</span></span> <span data-ttu-id="06dbe-109">Ha hello művelet típusa beírni, a művelet, vagy törlése hello rekordokat hello kezdő és a sikeres vagy sikertelen a művelet rögzíti hello felügyeleti kategória.</span><span class="sxs-lookup"><span data-stu-id="06dbe-109">If hello operation type is Write, Delete, or Action, hello records of both hello start and success or fail of that operation are recorded in hello Administrative category.</span></span> <span data-ttu-id="06dbe-110">hello felügyeleti kategória is bármely módosítások toorole-alapú hozzáférés-vezérlés egy előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="06dbe-110">hello Administrative category also includes any changes toorole-based access control in a subscription.</span></span>

### <a name="sample-event"></a><span data-ttu-id="06dbe-111">Mintaesemény</span><span class="sxs-lookup"><span data-stu-id="06dbe-111">Sample event</span></span>
```json
{
  "authorization": {
    "action": "microsoft.support/supporttickets/write",
    "role": "Subscription Admin",
    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
  },
  "caller": "admin@contoso.com",
  "channels": "Operation",
  "claims": {
    "aud": "https://management.core.windows.net/",
    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
    "iat": "1421876371",
    "nbf": "1421876371",
    "exp": "1421880271",
    "ver": "1.0",
    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
    "puid": "20030000801A118C",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
    "name": "John Smith",
    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
    "appidacr": "2",
    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
    "http://schemas.microsoft.com/claims/authnclassreference": "1"
  },
  "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "description": "",
  "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
  "eventName": {
    "value": "EndRequest",
    "localizedValue": "End request"
  },
  "httpRequest": {
    "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
    "clientIpAddress": "192.168.35.115",
    "method": "PUT"
  },
  "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
  "level": "Informational",
  "resourceGroupName": "MSSupportGroup",
  "resourceProviderName": {
    "value": "microsoft.support",
    "localizedValue": "microsoft.support"
  },
  "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
  "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "operationName": {
    "value": "microsoft.support/supporttickets/write",
    "localizedValue": "microsoft.support/supporttickets/write"
  },
  "properties": {
    "statusCode": "Created"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": "Created",
    "localizedValue": "Created (HTTP Status Code: 201)"
  },
  "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
  "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
  "subscriptionId": "s1"
}
```

### <a name="property-descriptions"></a><span data-ttu-id="06dbe-112">Tulajdonság leírása</span><span class="sxs-lookup"><span data-stu-id="06dbe-112">Property descriptions</span></span>
| <span data-ttu-id="06dbe-113">Elem neve</span><span class="sxs-lookup"><span data-stu-id="06dbe-113">Element Name</span></span> | <span data-ttu-id="06dbe-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="06dbe-114">Description</span></span> |
| --- | --- |
| <span data-ttu-id="06dbe-115">Engedélyezési</span><span class="sxs-lookup"><span data-stu-id="06dbe-115">authorization</span></span> |<span data-ttu-id="06dbe-116">A BLOB RBAC tulajdonságok hello esemény.</span><span class="sxs-lookup"><span data-stu-id="06dbe-116">Blob of RBAC properties of hello event.</span></span> <span data-ttu-id="06dbe-117">Hello "action", "szerepkör" és "hatókör" Tulajdonságok általában tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="06dbe-117">Usually includes hello “action”, “role” and “scope” properties.</span></span> |
| <span data-ttu-id="06dbe-118">hívó</span><span class="sxs-lookup"><span data-stu-id="06dbe-118">caller</span></span> |<span data-ttu-id="06dbe-119">Hello művelet, jogcím vagy SPN jogcímet rendelkezésre állása alapján elvégzett hello felhasználó e-mail címe.</span><span class="sxs-lookup"><span data-stu-id="06dbe-119">Email address of hello user who has performed hello operation, UPN claim, or SPN claim based on availability.</span></span> |
| <span data-ttu-id="06dbe-120">csatornák</span><span class="sxs-lookup"><span data-stu-id="06dbe-120">channels</span></span> |<span data-ttu-id="06dbe-121">Hello a következő értékek egyikét: "Rendszergazda", "Művelet"</span><span class="sxs-lookup"><span data-stu-id="06dbe-121">One of hello following values: “Admin”, “Operation”</span></span> |
| <span data-ttu-id="06dbe-122">Jogcímek</span><span class="sxs-lookup"><span data-stu-id="06dbe-122">claims</span></span> |<span data-ttu-id="06dbe-123">hello JWT jogkivonat Active Directory tooauthenticate hello felhasználó vagy alkalmazás tooperform használja ezt a műveletet az erőforrás-kezelőben.</span><span class="sxs-lookup"><span data-stu-id="06dbe-123">hello JWT token used by Active Directory tooauthenticate hello user or application tooperform this operation in resource manager.</span></span> |
| <span data-ttu-id="06dbe-124">correlationId</span><span class="sxs-lookup"><span data-stu-id="06dbe-124">correlationId</span></span> |<span data-ttu-id="06dbe-125">Általában egy GUID Azonosítót a hello karakterlánc-formátum.</span><span class="sxs-lookup"><span data-stu-id="06dbe-125">Usually a GUID in hello string format.</span></span> <span data-ttu-id="06dbe-126">Az eseményeket, amelyek megosztása a correlationId tartozik toohello uber műveletet.</span><span class="sxs-lookup"><span data-stu-id="06dbe-126">Events that share a correlationId belong toohello same uber action.</span></span> |
| <span data-ttu-id="06dbe-127">leírás</span><span class="sxs-lookup"><span data-stu-id="06dbe-127">description</span></span> |<span data-ttu-id="06dbe-128">Egy esemény statikus szöveges leírása.</span><span class="sxs-lookup"><span data-stu-id="06dbe-128">Static text description of an event.</span></span> |
| <span data-ttu-id="06dbe-129">eventDataId</span><span class="sxs-lookup"><span data-stu-id="06dbe-129">eventDataId</span></span> |<span data-ttu-id="06dbe-130">Az esemény egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="06dbe-130">Unique identifier of an event.</span></span> |
| <span data-ttu-id="06dbe-131">httpRequest</span><span class="sxs-lookup"><span data-stu-id="06dbe-131">httpRequest</span></span> |<span data-ttu-id="06dbe-132">A BLOB leíró hello Http-kérelem.</span><span class="sxs-lookup"><span data-stu-id="06dbe-132">Blob describing hello Http Request.</span></span> <span data-ttu-id="06dbe-133">Általában tartalmazza a hello "clientRequestId", "clientIpAddress" és "method" (HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="06dbe-133">Usually includes hello “clientRequestId”, “clientIpAddress” and “method” (HTTP method.</span></span> <span data-ttu-id="06dbe-134">For example, amelyre az).</span><span class="sxs-lookup"><span data-stu-id="06dbe-134">For example, PUT).</span></span> |
| <span data-ttu-id="06dbe-135">szint</span><span class="sxs-lookup"><span data-stu-id="06dbe-135">level</span></span> |<span data-ttu-id="06dbe-136">Hello esemény szintje.</span><span class="sxs-lookup"><span data-stu-id="06dbe-136">Level of hello event.</span></span> <span data-ttu-id="06dbe-137">Hello a következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes"</span><span class="sxs-lookup"><span data-stu-id="06dbe-137">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="06dbe-138">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="06dbe-138">resourceGroupName</span></span> |<span data-ttu-id="06dbe-139">Hello hello erőforráscsoport nevét érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="06dbe-139">Name of hello resource group for hello impacted resource.</span></span> |
| <span data-ttu-id="06dbe-140">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="06dbe-140">resourceProviderName</span></span> |<span data-ttu-id="06dbe-141">Hello hello erőforrás-szolgáltató neve érintett erőforrás</span><span class="sxs-lookup"><span data-stu-id="06dbe-141">Name of hello resource provider for hello impacted resource</span></span> |
| <span data-ttu-id="06dbe-142">resourceId</span><span class="sxs-lookup"><span data-stu-id="06dbe-142">resourceId</span></span> |<span data-ttu-id="06dbe-143">Erőforrás-azonosítója, hello érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="06dbe-143">Resource id of hello impacted resource.</span></span> |
| <span data-ttu-id="06dbe-144">OperationID azonosítójú</span><span class="sxs-lookup"><span data-stu-id="06dbe-144">operationId</span></span> |<span data-ttu-id="06dbe-145">GUID-azonosítónak megfelelő tooa egyetlen műveletben hello események között meg van osztva.</span><span class="sxs-lookup"><span data-stu-id="06dbe-145">A GUID shared among hello events that correspond tooa single operation.</span></span> |
| <span data-ttu-id="06dbe-146">operationName</span><span class="sxs-lookup"><span data-stu-id="06dbe-146">operationName</span></span> |<span data-ttu-id="06dbe-147">Hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="06dbe-147">Name of hello operation.</span></span> |
| <span data-ttu-id="06dbe-148">properties</span><span class="sxs-lookup"><span data-stu-id="06dbe-148">properties</span></span> |<span data-ttu-id="06dbe-149">Állítsa be a `<Key, Value>` hello esemény részleteinek hello leíró párok (Ez azt jelenti, hogy szótárában).</span><span class="sxs-lookup"><span data-stu-id="06dbe-149">Set of `<Key, Value>` pairs (that is, a Dictionary) describing hello details of hello event.</span></span> |
| <span data-ttu-id="06dbe-150">status</span><span class="sxs-lookup"><span data-stu-id="06dbe-150">status</span></span> |<span data-ttu-id="06dbe-151">Hello művelet hello állapotát leíró karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="06dbe-151">String describing hello status of hello operation.</span></span> <span data-ttu-id="06dbe-152">Néhány gyakori értékek a következők: folyamatban van, sikeres, sikertelen, aktív, a lépések megoldva.</span><span class="sxs-lookup"><span data-stu-id="06dbe-152">Some common values are: Started, In Progress, Succeeded, Failed, Active, Resolved.</span></span> |
| <span data-ttu-id="06dbe-153">a részállapot</span><span class="sxs-lookup"><span data-stu-id="06dbe-153">subStatus</span></span> |<span data-ttu-id="06dbe-154">Általában hello REST-hívást megfelelő hello HTTP-állapotkód, de más a részállapot, például a gyakori értékek leíró karakterláncok is használható: OK (HTTP-állapotkód: 200), létrehozott (HTTP-állapotkód: 201-es), elfogadott (HTTP-állapotkód: 202), nincs tartalom (HTTP Állapotkód: 204), hibás kérelem (HTTP-állapotkód: 400), nem található (HTTP-állapotkód: 404), ütközés (HTTP-állapotkód: 409), belső kiszolgálóhiba (HTTP-állapotkód: 500), a Service nem érhető el (HTTP-állapotkód: 503-as), átjáró időtúllépése (HTTP-állapotkód: 504).</span><span class="sxs-lookup"><span data-stu-id="06dbe-154">Usually hello HTTP status code of hello corresponding REST call, but can also include other strings describing a substatus, such as these common values: OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), Gateway Timeout (HTTP Status Code: 504).</span></span> |
| <span data-ttu-id="06dbe-155">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="06dbe-155">eventTimestamp</span></span> |<span data-ttu-id="06dbe-156">Hello esemény lett létrehozva, amikor hello Azure szolgáltatás feldolgozása hello idejét jelző időbélyegző megfelelő hello esemény kérelmet.</span><span class="sxs-lookup"><span data-stu-id="06dbe-156">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="06dbe-157">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="06dbe-157">submissionTimestamp</span></span> |<span data-ttu-id="06dbe-158">Hello esemény váltak elérhetővé lekérdezése idejét jelző időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="06dbe-158">Timestamp when hello event became available for querying.</span></span> |
| <span data-ttu-id="06dbe-159">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="06dbe-159">subscriptionId</span></span> |<span data-ttu-id="06dbe-160">Az Azure előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="06dbe-160">Azure Subscription Id.</span></span> |

## <a name="service-health"></a><span data-ttu-id="06dbe-161">Szolgáltatás állapota</span><span class="sxs-lookup"><span data-stu-id="06dbe-161">Service health</span></span>
<span data-ttu-id="06dbe-162">Ez a kategória történt az Azure-szolgáltatás állapotának események hello rekord tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="06dbe-162">This category contains hello record of any service health incidents that have occurred in Azure.</span></span> <span data-ttu-id="06dbe-163">Ebbe a kategóriába tartozó mutatunk be hello eseménytípusra példa: "USA keleti régiója az SQL Azure tapasztal állásidő."</span><span class="sxs-lookup"><span data-stu-id="06dbe-163">An example of hello type of event you would see in this category is "SQL Azure in East US is experiencing downtime."</span></span> <span data-ttu-id="06dbe-164">Szolgáltatás állapotával kapcsolatos események térjen öt fajta: beavatkozás szükséges, támogatott helyreállítási, incidens, karbantartási, adatokat vagy biztonsági, és csak jelenik meg, ha egy erőforrást, amely akkor negatív hatással lehet hello esemény hello előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="06dbe-164">Service health events come in five varieties: Action Required, Assisted Recovery, Incident, Maintenance, Information, or Security, and only appear if you have a resource in hello subscription that would be impacted by hello event.</span></span>

### <a name="sample-event"></a><span data-ttu-id="06dbe-165">Mintaesemény</span><span class="sxs-lookup"><span data-stu-id="06dbe-165">Sample event</span></span>
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/mySubscriptionID/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/mySubscriptionID",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "mySubscriptionID",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited tooApp Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows toomitigate hello impact. hello next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited tooApp Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows toomitigate hello impact. hello next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```

### <a name="property-descriptions"></a><span data-ttu-id="06dbe-166">Tulajdonság leírása</span><span class="sxs-lookup"><span data-stu-id="06dbe-166">Property descriptions</span></span>
<span data-ttu-id="06dbe-167">Elem neve</span><span class="sxs-lookup"><span data-stu-id="06dbe-167">Element Name</span></span> | <span data-ttu-id="06dbe-168">Leírás</span><span class="sxs-lookup"><span data-stu-id="06dbe-168">Description</span></span>
-------- | -----------
<span data-ttu-id="06dbe-169">csatornák</span><span class="sxs-lookup"><span data-stu-id="06dbe-169">channels</span></span> | <span data-ttu-id="06dbe-170">Hello a következő értékek egyike: "Rendszergazda", "Művelet"</span><span class="sxs-lookup"><span data-stu-id="06dbe-170">Is one of hello following values: “Admin”, “Operation”</span></span>
<span data-ttu-id="06dbe-171">correlationId</span><span class="sxs-lookup"><span data-stu-id="06dbe-171">correlationId</span></span> | <span data-ttu-id="06dbe-172">Az általában egy GUID Azonosítót a hello karakterlánc-formátum.</span><span class="sxs-lookup"><span data-stu-id="06dbe-172">Is usually a GUID in hello string format.</span></span> <span data-ttu-id="06dbe-173">Események, amelyek azonos uber művelet általában megosztása toohello tartozik azonos correlationId hello.</span><span class="sxs-lookup"><span data-stu-id="06dbe-173">Events with that belong toohello same uber action usually share hello same correlationId.</span></span>
<span data-ttu-id="06dbe-174">leírás</span><span class="sxs-lookup"><span data-stu-id="06dbe-174">description</span></span> | <span data-ttu-id="06dbe-175">Hello esemény leírása.</span><span class="sxs-lookup"><span data-stu-id="06dbe-175">Description of hello event.</span></span>
<span data-ttu-id="06dbe-176">eventDataId</span><span class="sxs-lookup"><span data-stu-id="06dbe-176">eventDataId</span></span> | <span data-ttu-id="06dbe-177">hello egyedi azonosítója egy eseményt.</span><span class="sxs-lookup"><span data-stu-id="06dbe-177">hello unique identifier of an event.</span></span>
<span data-ttu-id="06dbe-178">EventName</span><span class="sxs-lookup"><span data-stu-id="06dbe-178">eventName</span></span> | <span data-ttu-id="06dbe-179">hello cím hello esemény.</span><span class="sxs-lookup"><span data-stu-id="06dbe-179">hello title of hello event.</span></span>
<span data-ttu-id="06dbe-180">szint</span><span class="sxs-lookup"><span data-stu-id="06dbe-180">level</span></span> | <span data-ttu-id="06dbe-181">Hello esemény szintje.</span><span class="sxs-lookup"><span data-stu-id="06dbe-181">Level of hello event.</span></span> <span data-ttu-id="06dbe-182">Hello a következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes"</span><span class="sxs-lookup"><span data-stu-id="06dbe-182">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span>
<span data-ttu-id="06dbe-183">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="06dbe-183">resourceProviderName</span></span> | <span data-ttu-id="06dbe-184">Hello hello erőforrás-szolgáltató neve érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="06dbe-184">Name of hello resource provider for hello impacted resource.</span></span> <span data-ttu-id="06dbe-185">Ha nem ismeri, ez lehet null.</span><span class="sxs-lookup"><span data-stu-id="06dbe-185">If not known, this will be null.</span></span>
<span data-ttu-id="06dbe-186">a resourceType</span><span class="sxs-lookup"><span data-stu-id="06dbe-186">resourceType</span></span>| <span data-ttu-id="06dbe-187">hello típusú erőforrás hello az érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="06dbe-187">hello type of resource of hello impacted resource.</span></span> <span data-ttu-id="06dbe-188">Ha nem ismeri, ez lehet null.</span><span class="sxs-lookup"><span data-stu-id="06dbe-188">If not known, this will be null.</span></span>
<span data-ttu-id="06dbe-189">a részállapot</span><span class="sxs-lookup"><span data-stu-id="06dbe-189">subStatus</span></span> | <span data-ttu-id="06dbe-190">A szolgáltatás állapotával kapcsolatos események általában null értékű.</span><span class="sxs-lookup"><span data-stu-id="06dbe-190">Usually null for Service Health events.</span></span>
<span data-ttu-id="06dbe-191">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="06dbe-191">eventTimestamp</span></span> | <span data-ttu-id="06dbe-192">Hello naplóesemény jött létre, és toohello tevékenység naplója beküldött idejét jelző időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="06dbe-192">Timestamp when hello log event was generated and submitted toohello Activity Log.</span></span>
<span data-ttu-id="06dbe-193">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="06dbe-193">submissionTimestamp</span></span> |   <span data-ttu-id="06dbe-194">Hello esemény váltak elérhetővé az hello tevékenységnapló idejét jelző időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="06dbe-194">Timestamp when hello event became available in hello Activity Log.</span></span>
<span data-ttu-id="06dbe-195">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="06dbe-195">subscriptionId</span></span> | <span data-ttu-id="06dbe-196">Azure-előfizetés, amelyben ez az esemény naplózásának hello.</span><span class="sxs-lookup"><span data-stu-id="06dbe-196">hello Azure subscription in which this event was logged.</span></span>
<span data-ttu-id="06dbe-197">status</span><span class="sxs-lookup"><span data-stu-id="06dbe-197">status</span></span> | <span data-ttu-id="06dbe-198">Hello művelet hello állapotát leíró karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="06dbe-198">String describing hello status of hello operation.</span></span> <span data-ttu-id="06dbe-199">Néhány gyakori értékek a következők: aktív, a megoldott.</span><span class="sxs-lookup"><span data-stu-id="06dbe-199">Some common values are: Active, Resolved.</span></span>
<span data-ttu-id="06dbe-200">operationName</span><span class="sxs-lookup"><span data-stu-id="06dbe-200">operationName</span></span> | <span data-ttu-id="06dbe-201">Hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="06dbe-201">Name of hello operation.</span></span> <span data-ttu-id="06dbe-202">Általában Microsoft.ServiceHealth/incident/action.</span><span class="sxs-lookup"><span data-stu-id="06dbe-202">Usually Microsoft.ServiceHealth/incident/action.</span></span>
<span data-ttu-id="06dbe-203">category</span><span class="sxs-lookup"><span data-stu-id="06dbe-203">category</span></span> | <span data-ttu-id="06dbe-204">"ServiceHealth"</span><span class="sxs-lookup"><span data-stu-id="06dbe-204">"ServiceHealth"</span></span>
<span data-ttu-id="06dbe-205">resourceId</span><span class="sxs-lookup"><span data-stu-id="06dbe-205">resourceId</span></span> | <span data-ttu-id="06dbe-206">Erőforrás-azonosítója, hello érintett erőforrás, ha ismert.</span><span class="sxs-lookup"><span data-stu-id="06dbe-206">Resource id of hello impacted resource, if known.</span></span> <span data-ttu-id="06dbe-207">Ellenkező esetben van megadva előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="06dbe-207">Subscription ID is provided otherwise.</span></span>
<span data-ttu-id="06dbe-208">Properties.Title</span><span class="sxs-lookup"><span data-stu-id="06dbe-208">Properties.title</span></span> | <span data-ttu-id="06dbe-209">Ez a kommunikáció hello honosított címét.</span><span class="sxs-lookup"><span data-stu-id="06dbe-209">hello localized title for this communication.</span></span> <span data-ttu-id="06dbe-210">Angol hello alapértelmezett nyelvét.</span><span class="sxs-lookup"><span data-stu-id="06dbe-210">English is hello default language.</span></span>
<span data-ttu-id="06dbe-211">Properties.Communication</span><span class="sxs-lookup"><span data-stu-id="06dbe-211">Properties.communication</span></span> | <span data-ttu-id="06dbe-212">honosított hello részletei hello kommunikáció a HTML-kódot.</span><span class="sxs-lookup"><span data-stu-id="06dbe-212">hello localized details of hello communication with HTML markup.</span></span> <span data-ttu-id="06dbe-213">Angol hello alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="06dbe-213">English is hello default.</span></span>
<span data-ttu-id="06dbe-214">Properties.incidentType</span><span class="sxs-lookup"><span data-stu-id="06dbe-214">Properties.incidentType</span></span> | <span data-ttu-id="06dbe-215">A lehetséges értékek: AssistedRecovery, ActionRequired, információk, incidens, karbantartási, biztonsági</span><span class="sxs-lookup"><span data-stu-id="06dbe-215">Possible values: AssistedRecovery, ActionRequired, Information, Incident, Maintenance, Security</span></span>
<span data-ttu-id="06dbe-216">Properties.trackingId</span><span class="sxs-lookup"><span data-stu-id="06dbe-216">Properties.trackingId</span></span> | <span data-ttu-id="06dbe-217">Az eseményhez kapcsolódó hello incidens azonosítja.</span><span class="sxs-lookup"><span data-stu-id="06dbe-217">Identifies hello incident this event is associated with.</span></span> <span data-ttu-id="06dbe-218">Használja az toocorrelate hello események kapcsolódó tooan esemény.</span><span class="sxs-lookup"><span data-stu-id="06dbe-218">Use this toocorrelate hello events related tooan incident.</span></span>
<span data-ttu-id="06dbe-219">Properties.impactedServices</span><span class="sxs-lookup"><span data-stu-id="06dbe-219">Properties.impactedServices</span></span> | <span data-ttu-id="06dbe-220">Az escape-karakterrel megjelölt JSON-blob hello szolgáltatások és hello incidens által érintett területek leíró.</span><span class="sxs-lookup"><span data-stu-id="06dbe-220">An escaped JSON blob which describes hello services and regions that are impacted by hello incident.</span></span> <span data-ttu-id="06dbe-221">Szolgáltatások listáját, amelyek mindegyike rendelkezik egy szolgáltatásnév és ImpactedRegions, amelyek mindegyikének a RegionName listáját.</span><span class="sxs-lookup"><span data-stu-id="06dbe-221">A list of Services, each of which has a ServiceName and a list of ImpactedRegions, each of which has a RegionName.</span></span>
<span data-ttu-id="06dbe-222">Properties.defaultLanguageTitle</span><span class="sxs-lookup"><span data-stu-id="06dbe-222">Properties.defaultLanguageTitle</span></span> | <span data-ttu-id="06dbe-223">hello kommunikációs angol nyelven</span><span class="sxs-lookup"><span data-stu-id="06dbe-223">hello communication in English</span></span>
<span data-ttu-id="06dbe-224">Properties.defaultLanguageContent</span><span class="sxs-lookup"><span data-stu-id="06dbe-224">Properties.defaultLanguageContent</span></span> | <span data-ttu-id="06dbe-225">hello kommunikációs html-kódot vagy egyszerű szöveg angol nyelven</span><span class="sxs-lookup"><span data-stu-id="06dbe-225">hello communication in English as either html markup or plain text</span></span>
<span data-ttu-id="06dbe-226">Properties.Stage</span><span class="sxs-lookup"><span data-stu-id="06dbe-226">Properties.stage</span></span> | <span data-ttu-id="06dbe-227">AssistedRecovery, a ActionRequired, a információkat, az incidensek, a biztonsági a lehetséges értékek: aktív, amelyek megoldva.</span><span class="sxs-lookup"><span data-stu-id="06dbe-227">Possible values for AssistedRecovery, ActionRequired, Information, Incident, Security: are Active, Resolved.</span></span> <span data-ttu-id="06dbe-228">A karbantartás vannak: aktív, tervezett, esetbejegyzések, visszavonva, Rescheduled, megoldva, kész</span><span class="sxs-lookup"><span data-stu-id="06dbe-228">For Maintenance they are: Active, Planned, InProgress, Canceled, Rescheduled, Resolved, Complete</span></span>
<span data-ttu-id="06dbe-229">Properties.communicationId</span><span class="sxs-lookup"><span data-stu-id="06dbe-229">Properties.communicationId</span></span> | <span data-ttu-id="06dbe-230">hello kommunikációs ezt az eseményt hozzá rendelve.</span><span class="sxs-lookup"><span data-stu-id="06dbe-230">hello communication this event is associated.</span></span>

## <a name="alert"></a><span data-ttu-id="06dbe-231">Riasztás</span><span class="sxs-lookup"><span data-stu-id="06dbe-231">Alert</span></span>
<span data-ttu-id="06dbe-232">Ez a kategória Azure riasztások valamennyi aktiváláshoz hello rekord tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="06dbe-232">This category contains hello record of all activations of Azure alerts.</span></span> <span data-ttu-id="06dbe-233">Ebbe a kategóriába tartozó mutatunk be hello eseménytípusra példa: "CPU % myVM lett több mint 80-as hello elmúlt 5 perc."</span><span class="sxs-lookup"><span data-stu-id="06dbe-233">An example of hello type of event you would see in this category is "CPU % on myVM has been over 80 for hello past 5 minutes."</span></span> <span data-ttu-id="06dbe-234">Az Azure rendszereket rendelkezik egy riasztási koncepció--valamiféle szabály megadása, és értesítést kaphat, ha feltételek egyeznek meg, hogy a szabály.</span><span class="sxs-lookup"><span data-stu-id="06dbe-234">A variety of Azure systems have an alerting concept -- you can define a rule of some sort and receive a notification when conditions match that rule.</span></span> <span data-ttu-id="06dbe-235">Minden alkalommal, amikor egy támogatott Azure riasztástípus "aktiválja," vagy hello feltételek fennállnak toogenerate értesítést, hello aktiválás rekord is fejlesztőre hello tevékenységnapló toothis kategóriáját.</span><span class="sxs-lookup"><span data-stu-id="06dbe-235">Each time a supported Azure alert type 'activates,' or hello conditions are met toogenerate a notification, a record of hello activation is also pushed toothis category of hello Activity Log.</span></span>

### <a name="sample-event"></a><span data-ttu-id="06dbe-236">Mintaesemény</span><span class="sxs-lookup"><span data-stu-id="06dbe-236">Sample event</span></span>

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in hello last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "mySubscriptionID"
}
```

### <a name="property-descriptions"></a><span data-ttu-id="06dbe-237">Tulajdonság leírása</span><span class="sxs-lookup"><span data-stu-id="06dbe-237">Property descriptions</span></span>
| <span data-ttu-id="06dbe-238">Elem neve</span><span class="sxs-lookup"><span data-stu-id="06dbe-238">Element Name</span></span> | <span data-ttu-id="06dbe-239">Leírás</span><span class="sxs-lookup"><span data-stu-id="06dbe-239">Description</span></span> |
| --- | --- |
| <span data-ttu-id="06dbe-240">hívó</span><span class="sxs-lookup"><span data-stu-id="06dbe-240">caller</span></span> | <span data-ttu-id="06dbe-241">Mindig Microsoft.Insights/alertRules</span><span class="sxs-lookup"><span data-stu-id="06dbe-241">Always Microsoft.Insights/alertRules</span></span> |
| <span data-ttu-id="06dbe-242">csatornák</span><span class="sxs-lookup"><span data-stu-id="06dbe-242">channels</span></span> | <span data-ttu-id="06dbe-243">Mindig "rendszergazda, a művelet"</span><span class="sxs-lookup"><span data-stu-id="06dbe-243">Always “Admin, Operation”</span></span> |
| <span data-ttu-id="06dbe-244">Jogcímek</span><span class="sxs-lookup"><span data-stu-id="06dbe-244">claims</span></span> | <span data-ttu-id="06dbe-245">JSON-blob hello SPN (egyszerű szolgáltatásnév), vagy az erőforrás típusát, hello riasztási motor.</span><span class="sxs-lookup"><span data-stu-id="06dbe-245">JSON blob with hello SPN (service principal name), or resource type, of hello alert engine.</span></span> |
| <span data-ttu-id="06dbe-246">correlationId</span><span class="sxs-lookup"><span data-stu-id="06dbe-246">correlationId</span></span> | <span data-ttu-id="06dbe-247">A karakterlánc-formátum hello GUID.</span><span class="sxs-lookup"><span data-stu-id="06dbe-247">A GUID in hello string format.</span></span> |
| <span data-ttu-id="06dbe-248">leírás</span><span class="sxs-lookup"><span data-stu-id="06dbe-248">description</span></span> |<span data-ttu-id="06dbe-249">Hello figyelmeztetési esemény statikus szöveges leírása.</span><span class="sxs-lookup"><span data-stu-id="06dbe-249">Static text description of hello alert event.</span></span> |
| <span data-ttu-id="06dbe-250">eventDataId</span><span class="sxs-lookup"><span data-stu-id="06dbe-250">eventDataId</span></span> |<span data-ttu-id="06dbe-251">Hello figyelmeztetési esemény egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="06dbe-251">Unique identifier of hello alert event.</span></span> |
| <span data-ttu-id="06dbe-252">szint</span><span class="sxs-lookup"><span data-stu-id="06dbe-252">level</span></span> |<span data-ttu-id="06dbe-253">Hello esemény szintje.</span><span class="sxs-lookup"><span data-stu-id="06dbe-253">Level of hello event.</span></span> <span data-ttu-id="06dbe-254">Hello a következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes"</span><span class="sxs-lookup"><span data-stu-id="06dbe-254">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="06dbe-255">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="06dbe-255">resourceGroupName</span></span> |<span data-ttu-id="06dbe-256">Hello hello erőforráscsoport nevét érintett erőforrás, ha egy metrika riasztás.</span><span class="sxs-lookup"><span data-stu-id="06dbe-256">Name of hello resource group for hello impacted resource if it is a metric alert.</span></span> <span data-ttu-id="06dbe-257">A más típusú Ez az hello figyelmeztetéshez tartalmazó erőforráscsoport hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="06dbe-257">For other alert types, this is hello name of hello resource group that contains hello alert itself.</span></span> |
| <span data-ttu-id="06dbe-258">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="06dbe-258">resourceProviderName</span></span> |<span data-ttu-id="06dbe-259">Hello hello erőforrás-szolgáltató neve érintett erőforrás, ha egy metrika riasztás.</span><span class="sxs-lookup"><span data-stu-id="06dbe-259">Name of hello resource provider for hello impacted resource if it is a metric alert.</span></span> <span data-ttu-id="06dbe-260">A más típusú Ez az hello figyelmeztetéshez hello hello erőforrás-szolgáltató nevét.</span><span class="sxs-lookup"><span data-stu-id="06dbe-260">For other alert types, this is hello name of hello resource provider for hello alert itself.</span></span> |
| <span data-ttu-id="06dbe-261">resourceId</span><span class="sxs-lookup"><span data-stu-id="06dbe-261">resourceId</span></span> | <span data-ttu-id="06dbe-262">Hello hello erőforrás-azonosító nevet érintett erőforrás, ha egy metrika riasztás.</span><span class="sxs-lookup"><span data-stu-id="06dbe-262">Name of hello resource ID for hello impacted resource if it is a metric alert.</span></span> <span data-ttu-id="06dbe-263">A más típusú Ez az erőforrás-azonosító hello hello riasztási erőforrás magát.</span><span class="sxs-lookup"><span data-stu-id="06dbe-263">For other alert types, this is hello resource ID of hello alert resource itself.</span></span> |
| <span data-ttu-id="06dbe-264">OperationID azonosítójú</span><span class="sxs-lookup"><span data-stu-id="06dbe-264">operationId</span></span> |<span data-ttu-id="06dbe-265">GUID-azonosítónak megfelelő tooa egyetlen műveletben hello események között meg van osztva.</span><span class="sxs-lookup"><span data-stu-id="06dbe-265">A GUID shared among hello events that correspond tooa single operation.</span></span> |
| <span data-ttu-id="06dbe-266">operationName</span><span class="sxs-lookup"><span data-stu-id="06dbe-266">operationName</span></span> |<span data-ttu-id="06dbe-267">Hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="06dbe-267">Name of hello operation.</span></span> |
| <span data-ttu-id="06dbe-268">properties</span><span class="sxs-lookup"><span data-stu-id="06dbe-268">properties</span></span> |<span data-ttu-id="06dbe-269">Állítsa be a `<Key, Value>` hello esemény részleteinek hello leíró párok (Ez azt jelenti, hogy szótárában).</span><span class="sxs-lookup"><span data-stu-id="06dbe-269">Set of `<Key, Value>` pairs (that is, a Dictionary) describing hello details of hello event.</span></span> |
| <span data-ttu-id="06dbe-270">status</span><span class="sxs-lookup"><span data-stu-id="06dbe-270">status</span></span> |<span data-ttu-id="06dbe-271">Hello művelet hello állapotát leíró karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="06dbe-271">String describing hello status of hello operation.</span></span> <span data-ttu-id="06dbe-272">Néhány gyakori értékek a következők: folyamatban van, sikeres, sikertelen, aktív, a lépések megoldva.</span><span class="sxs-lookup"><span data-stu-id="06dbe-272">Some common values are: Started, In Progress, Succeeded, Failed, Active, Resolved.</span></span> |
| <span data-ttu-id="06dbe-273">a részállapot</span><span class="sxs-lookup"><span data-stu-id="06dbe-273">subStatus</span></span> | <span data-ttu-id="06dbe-274">A riasztások általában null értékű.</span><span class="sxs-lookup"><span data-stu-id="06dbe-274">Usually null for alerts.</span></span> |
| <span data-ttu-id="06dbe-275">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="06dbe-275">eventTimestamp</span></span> |<span data-ttu-id="06dbe-276">Hello esemény lett létrehozva, amikor hello Azure szolgáltatás feldolgozása hello idejét jelző időbélyegző megfelelő hello esemény kérelmet.</span><span class="sxs-lookup"><span data-stu-id="06dbe-276">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="06dbe-277">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="06dbe-277">submissionTimestamp</span></span> |<span data-ttu-id="06dbe-278">Hello esemény váltak elérhetővé lekérdezése idejét jelző időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="06dbe-278">Timestamp when hello event became available for querying.</span></span> |
| <span data-ttu-id="06dbe-279">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="06dbe-279">subscriptionId</span></span> |<span data-ttu-id="06dbe-280">Az Azure előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="06dbe-280">Azure Subscription Id.</span></span> |

### <a name="properties-field-per-alert-type"></a><span data-ttu-id="06dbe-281">Tulajdonságok mező szerepeljen riasztás típusa</span><span class="sxs-lookup"><span data-stu-id="06dbe-281">Properties field per alert type</span></span>
<span data-ttu-id="06dbe-282">hello tulajdonságok mező hello figyelmeztetési esemény hello forrása függően különböző értékeket fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="06dbe-282">hello properties field will contain different values depending on hello source of hello alert event.</span></span> <span data-ttu-id="06dbe-283">Két közös riasztási eseménynek szolgáltatók a következők: tevékenységnapló riasztások és metrika riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="06dbe-283">Two common alert event providers are Activity Log alerts and metric alerts.</span></span>

#### <a name="properties-for-activity-log-alerts"></a><span data-ttu-id="06dbe-284">A műveletnapló riasztások tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="06dbe-284">Properties for Activity Log alerts</span></span>
| <span data-ttu-id="06dbe-285">Elem neve</span><span class="sxs-lookup"><span data-stu-id="06dbe-285">Element Name</span></span> | <span data-ttu-id="06dbe-286">Leírás</span><span class="sxs-lookup"><span data-stu-id="06dbe-286">Description</span></span> |
| --- | --- |
| <span data-ttu-id="06dbe-287">properties.subscriptionId</span><span class="sxs-lookup"><span data-stu-id="06dbe-287">properties.subscriptionId</span></span> | <span data-ttu-id="06dbe-288">hello előfizetés-azonosító a hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva.</span><span class="sxs-lookup"><span data-stu-id="06dbe-288">hello subscription ID from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="06dbe-289">properties.eventDataId</span><span class="sxs-lookup"><span data-stu-id="06dbe-289">properties.eventDataId</span></span> | <span data-ttu-id="06dbe-290">hello adatok Eseményazonosítójú hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva.</span><span class="sxs-lookup"><span data-stu-id="06dbe-290">hello event data ID from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="06dbe-291">properties.resourceGroup</span><span class="sxs-lookup"><span data-stu-id="06dbe-291">properties.resourceGroup</span></span> | <span data-ttu-id="06dbe-292">hello erőforráscsoport hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva.</span><span class="sxs-lookup"><span data-stu-id="06dbe-292">hello resource group from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="06dbe-293">properties.resourceId</span><span class="sxs-lookup"><span data-stu-id="06dbe-293">properties.resourceId</span></span> | <span data-ttu-id="06dbe-294">hello erőforrás-azonosító a hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva.</span><span class="sxs-lookup"><span data-stu-id="06dbe-294">hello resource ID from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="06dbe-295">properties.eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="06dbe-295">properties.eventTimestamp</span></span> | <span data-ttu-id="06dbe-296">hello esemény időbélyegzője hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva.</span><span class="sxs-lookup"><span data-stu-id="06dbe-296">hello event timestamp of hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="06dbe-297">properties.operationName</span><span class="sxs-lookup"><span data-stu-id="06dbe-297">properties.operationName</span></span> | <span data-ttu-id="06dbe-298">a művelet neve hello hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva.</span><span class="sxs-lookup"><span data-stu-id="06dbe-298">hello operation name from hello activity log event which caused this activity log alert rule toobe activated.</span></span> |
| <span data-ttu-id="06dbe-299">Properties.status</span><span class="sxs-lookup"><span data-stu-id="06dbe-299">properties.status</span></span> | <span data-ttu-id="06dbe-300">hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="06dbe-300">hello status from hello activity log event which caused this activity log alert rule toobe activated.</span></span>|

#### <a name="properties-for-metric-alerts"></a><span data-ttu-id="06dbe-301">A metrika riasztások tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="06dbe-301">Properties for metric alerts</span></span>
| <span data-ttu-id="06dbe-302">Elem neve</span><span class="sxs-lookup"><span data-stu-id="06dbe-302">Element Name</span></span> | <span data-ttu-id="06dbe-303">Leírás</span><span class="sxs-lookup"><span data-stu-id="06dbe-303">Description</span></span> |
| --- | --- |
| <span data-ttu-id="06dbe-304">tulajdonságok. RuleUri</span><span class="sxs-lookup"><span data-stu-id="06dbe-304">properties.RuleUri</span></span> | <span data-ttu-id="06dbe-305">Erőforrás-azonosító hello metrika riasztási szabály saját magát.</span><span class="sxs-lookup"><span data-stu-id="06dbe-305">Resource ID of hello metric alert rule itself.</span></span> |
| <span data-ttu-id="06dbe-306">tulajdonságok. Szabálynév</span><span class="sxs-lookup"><span data-stu-id="06dbe-306">properties.RuleName</span></span> | <span data-ttu-id="06dbe-307">hello hello metrika riasztási szabály neve.</span><span class="sxs-lookup"><span data-stu-id="06dbe-307">hello name of hello metric alert rule.</span></span> |
| <span data-ttu-id="06dbe-308">tulajdonságok. RuleDescription</span><span class="sxs-lookup"><span data-stu-id="06dbe-308">properties.RuleDescription</span></span> | <span data-ttu-id="06dbe-309">hello metrika riasztási szabály (meghatározott hello riasztási szabály) hello leírása.</span><span class="sxs-lookup"><span data-stu-id="06dbe-309">hello description of hello metric alert rule (as defined in hello alert rule).</span></span> |
| <span data-ttu-id="06dbe-310">tulajdonságok. Küszöbérték</span><span class="sxs-lookup"><span data-stu-id="06dbe-310">properties.Threshold</span></span> | <span data-ttu-id="06dbe-311">hello küszöbérték hello metrika riasztási szabály hello kiértékelésekor.</span><span class="sxs-lookup"><span data-stu-id="06dbe-311">hello threshold value used in hello evaluation of hello metric alert rule.</span></span> |
| <span data-ttu-id="06dbe-312">tulajdonságok. WindowSizeInMinutes</span><span class="sxs-lookup"><span data-stu-id="06dbe-312">properties.WindowSizeInMinutes</span></span> | <span data-ttu-id="06dbe-313">hello ablakméret hello metrika riasztási szabály hello kiértékelésekor.</span><span class="sxs-lookup"><span data-stu-id="06dbe-313">hello window size used in hello evaluation of hello metric alert rule.</span></span> |
| <span data-ttu-id="06dbe-314">tulajdonságok. Összesítés</span><span class="sxs-lookup"><span data-stu-id="06dbe-314">properties.Aggregation</span></span> | <span data-ttu-id="06dbe-315">hello összesítéstípusa hello metrika riasztási szabályban definiált.</span><span class="sxs-lookup"><span data-stu-id="06dbe-315">hello aggregation type defined in hello metric alert rule.</span></span> |
| <span data-ttu-id="06dbe-316">tulajdonságok. Operátor</span><span class="sxs-lookup"><span data-stu-id="06dbe-316">properties.Operator</span></span> | <span data-ttu-id="06dbe-317">hello feltételes operátort hello metrika riasztási szabály hello kiértékelésekor.</span><span class="sxs-lookup"><span data-stu-id="06dbe-317">hello conditional operator used in hello evaluation of hello metric alert rule.</span></span> |
| <span data-ttu-id="06dbe-318">tulajdonságok. MetricName</span><span class="sxs-lookup"><span data-stu-id="06dbe-318">properties.MetricName</span></span> | <span data-ttu-id="06dbe-319">hello hello kiértékelésekor hello metrika riasztási szabály hello metrika metrika neve.</span><span class="sxs-lookup"><span data-stu-id="06dbe-319">hello metric name of hello metric used in hello evaluation of hello metric alert rule.</span></span> |
| <span data-ttu-id="06dbe-320">tulajdonságok. MetricUnit</span><span class="sxs-lookup"><span data-stu-id="06dbe-320">properties.MetricUnit</span></span> | <span data-ttu-id="06dbe-321">hello metrika egység hello mérőszám hello metrika riasztási szabály hello kiértékelésekor.</span><span class="sxs-lookup"><span data-stu-id="06dbe-321">hello metric unit for hello metric used in hello evaluation of hello metric alert rule.</span></span> |

## <a name="autoscale"></a><span data-ttu-id="06dbe-322">Automatikus méretezés</span><span class="sxs-lookup"><span data-stu-id="06dbe-322">Autoscale</span></span>
<span data-ttu-id="06dbe-323">Ebbe a kategóriába bármely események kapcsolódó toohello művelet hello automatikus skálázás motor minden automatikus skálázási beállításokat adta meg az előfizetés alapján hello rekord tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="06dbe-323">This category contains hello record of any events related toohello operation of hello autoscale engine based on any autoscale settings you have defined in your subscription.</span></span> <span data-ttu-id="06dbe-324">Ebbe a kategóriába tartozó mutatunk be hello eseménytípusra példa, hogy "Automatikus skálázás felskálázott művelete nem sikerült."</span><span class="sxs-lookup"><span data-stu-id="06dbe-324">An example of hello type of event you would see in this category is "Autoscale scale up action failed."</span></span> <span data-ttu-id="06dbe-325">Használja az automatikus skálázás, automatikusan horizontális felskálázás vagy méretezni a hello támogatott erőforrástípus található példányok száma alapján idő nap és/vagy terhelés (mérték) adatok az automatikus skálázási beállítás használatával.</span><span class="sxs-lookup"><span data-stu-id="06dbe-325">Using autoscale, you can automatically scale out or scale in hello number of instances in a supported resource type based on time of day and/or load (metric) data using an autoscale setting.</span></span> <span data-ttu-id="06dbe-326">Hello feltételek teljesülése esetén tooscale felfelé vagy lefelé, kezdő hello és sikeres vagy sikertelen események rögzítése a kategória.</span><span class="sxs-lookup"><span data-stu-id="06dbe-326">When hello conditions are met tooscale up or down, hello start and succeeded or failed events will be recorded in this category.</span></span>

### <a name="sample-event"></a><span data-ttu-id="06dbe-327">Mintaesemény</span><span class="sxs-lookup"><span data-stu-id="06dbe-327">Sample event</span></span>
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "hello autoscale engine attempting tooscale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count too2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "hello autoscale engine attempting tooscale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count too2 instances count.",
    "ResourceName": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "mySubscriptionID"
}

```

### <a name="property-descriptions"></a><span data-ttu-id="06dbe-328">Tulajdonság leírása</span><span class="sxs-lookup"><span data-stu-id="06dbe-328">Property descriptions</span></span>
| <span data-ttu-id="06dbe-329">Elem neve</span><span class="sxs-lookup"><span data-stu-id="06dbe-329">Element Name</span></span> | <span data-ttu-id="06dbe-330">Leírás</span><span class="sxs-lookup"><span data-stu-id="06dbe-330">Description</span></span> |
| --- | --- |
| <span data-ttu-id="06dbe-331">hívó</span><span class="sxs-lookup"><span data-stu-id="06dbe-331">caller</span></span> | <span data-ttu-id="06dbe-332">Mindig Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="06dbe-332">Always Microsoft.Insights/autoscaleSettings</span></span> |
| <span data-ttu-id="06dbe-333">csatornák</span><span class="sxs-lookup"><span data-stu-id="06dbe-333">channels</span></span> | <span data-ttu-id="06dbe-334">Mindig "rendszergazda, a művelet"</span><span class="sxs-lookup"><span data-stu-id="06dbe-334">Always “Admin, Operation”</span></span> |
| <span data-ttu-id="06dbe-335">Jogcímek</span><span class="sxs-lookup"><span data-stu-id="06dbe-335">claims</span></span> | <span data-ttu-id="06dbe-336">JSON blob hello hello automatikus skálázás motor SPN (egyszerű szolgáltatásnév), vagy az erőforrás típusát.</span><span class="sxs-lookup"><span data-stu-id="06dbe-336">JSON blob with hello SPN (service principal name), or resource type, of hello autoscale engine.</span></span> |
| <span data-ttu-id="06dbe-337">correlationId</span><span class="sxs-lookup"><span data-stu-id="06dbe-337">correlationId</span></span> | <span data-ttu-id="06dbe-338">A karakterlánc-formátum hello GUID.</span><span class="sxs-lookup"><span data-stu-id="06dbe-338">A GUID in hello string format.</span></span> |
| <span data-ttu-id="06dbe-339">leírás</span><span class="sxs-lookup"><span data-stu-id="06dbe-339">description</span></span> |<span data-ttu-id="06dbe-340">Statikus szöveg hello automatikus skálázás esemény leírását.</span><span class="sxs-lookup"><span data-stu-id="06dbe-340">Static text description of hello autoscale event.</span></span> |
| <span data-ttu-id="06dbe-341">eventDataId</span><span class="sxs-lookup"><span data-stu-id="06dbe-341">eventDataId</span></span> |<span data-ttu-id="06dbe-342">Hello automatikus skálázás esemény egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="06dbe-342">Unique identifier of hello autoscale event.</span></span> |
| <span data-ttu-id="06dbe-343">szint</span><span class="sxs-lookup"><span data-stu-id="06dbe-343">level</span></span> |<span data-ttu-id="06dbe-344">Hello esemény szintje.</span><span class="sxs-lookup"><span data-stu-id="06dbe-344">Level of hello event.</span></span> <span data-ttu-id="06dbe-345">Hello a következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes"</span><span class="sxs-lookup"><span data-stu-id="06dbe-345">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="06dbe-346">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="06dbe-346">resourceGroupName</span></span> |<span data-ttu-id="06dbe-347">Hello automatikus skálázási beállítás hello erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="06dbe-347">Name of hello resource group for hello autoscale setting.</span></span> |
| <span data-ttu-id="06dbe-348">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="06dbe-348">resourceProviderName</span></span> |<span data-ttu-id="06dbe-349">Hello automatikus skálázási beállítás hello erőforrás-szolgáltató nevét.</span><span class="sxs-lookup"><span data-stu-id="06dbe-349">Name of hello resource provider for hello autoscale setting.</span></span> |
| <span data-ttu-id="06dbe-350">resourceId</span><span class="sxs-lookup"><span data-stu-id="06dbe-350">resourceId</span></span> |<span data-ttu-id="06dbe-351">Erőforrás-azonosító hello automatikus skálázási beállítás.</span><span class="sxs-lookup"><span data-stu-id="06dbe-351">Resource id of hello autoscale setting.</span></span> |
| <span data-ttu-id="06dbe-352">OperationID azonosítójú</span><span class="sxs-lookup"><span data-stu-id="06dbe-352">operationId</span></span> |<span data-ttu-id="06dbe-353">GUID-azonosítónak megfelelő tooa egyetlen műveletben hello események között meg van osztva.</span><span class="sxs-lookup"><span data-stu-id="06dbe-353">A GUID shared among hello events that correspond tooa single operation.</span></span> |
| <span data-ttu-id="06dbe-354">operationName</span><span class="sxs-lookup"><span data-stu-id="06dbe-354">operationName</span></span> |<span data-ttu-id="06dbe-355">Hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="06dbe-355">Name of hello operation.</span></span> |
| <span data-ttu-id="06dbe-356">properties</span><span class="sxs-lookup"><span data-stu-id="06dbe-356">properties</span></span> |<span data-ttu-id="06dbe-357">Állítsa be a `<Key, Value>` hello esemény részleteinek hello leíró párok (Ez azt jelenti, hogy szótárában).</span><span class="sxs-lookup"><span data-stu-id="06dbe-357">Set of `<Key, Value>` pairs (that is, a Dictionary) describing hello details of hello event.</span></span> |
| <span data-ttu-id="06dbe-358">tulajdonságok. Leírás</span><span class="sxs-lookup"><span data-stu-id="06dbe-358">properties.Description</span></span> | <span data-ttu-id="06dbe-359">Milyen hello automatikus skálázás motor műveletet végzett részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="06dbe-359">Detailed description of what hello autoscale engine was doing.</span></span> |
| <span data-ttu-id="06dbe-360">tulajdonságok. ResourceName</span><span class="sxs-lookup"><span data-stu-id="06dbe-360">properties.ResourceName</span></span> | <span data-ttu-id="06dbe-361">Erőforrás-azonosítója, hello érintett erőforrás (hello erőforrás mely hello a skálázási műveletek végrehajtása)</span><span class="sxs-lookup"><span data-stu-id="06dbe-361">Resource ID of hello impacted resource (hello resource on which hello scale action was being performed)</span></span> |
| <span data-ttu-id="06dbe-362">tulajdonságok. OldInstancesCount</span><span class="sxs-lookup"><span data-stu-id="06dbe-362">properties.OldInstancesCount</span></span> | <span data-ttu-id="06dbe-363">hello több példányban hello automatikus skálázási művelet előtt alkalmaztak.</span><span class="sxs-lookup"><span data-stu-id="06dbe-363">hello number of instances before hello autoscale action took effect.</span></span> |
| <span data-ttu-id="06dbe-364">tulajdonságok. NewInstancesCount</span><span class="sxs-lookup"><span data-stu-id="06dbe-364">properties.NewInstancesCount</span></span> | <span data-ttu-id="06dbe-365">Miután hello automatikus skálázási művelet végrehajtásának hatás példányok hello száma.</span><span class="sxs-lookup"><span data-stu-id="06dbe-365">hello number of instances after hello autoscale action took effect.</span></span> |
| <span data-ttu-id="06dbe-366">tulajdonságok. LastScaleActionTime</span><span class="sxs-lookup"><span data-stu-id="06dbe-366">properties.LastScaleActionTime</span></span> | <span data-ttu-id="06dbe-367">hello időbélyegzője amikor hello automatikus skálázási művelet történt.</span><span class="sxs-lookup"><span data-stu-id="06dbe-367">hello timestamp of when hello autoscale action occurred.</span></span> |
| <span data-ttu-id="06dbe-368">status</span><span class="sxs-lookup"><span data-stu-id="06dbe-368">status</span></span> |<span data-ttu-id="06dbe-369">Hello művelet hello állapotát leíró karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="06dbe-369">String describing hello status of hello operation.</span></span> <span data-ttu-id="06dbe-370">Néhány gyakori értékek a következők: folyamatban van, sikeres, sikertelen, aktív, a lépések megoldva.</span><span class="sxs-lookup"><span data-stu-id="06dbe-370">Some common values are: Started, In Progress, Succeeded, Failed, Active, Resolved.</span></span> |
| <span data-ttu-id="06dbe-371">a részállapot</span><span class="sxs-lookup"><span data-stu-id="06dbe-371">subStatus</span></span> | <span data-ttu-id="06dbe-372">Az automatikus skálázás általában null értékű.</span><span class="sxs-lookup"><span data-stu-id="06dbe-372">Usually null for autoscale.</span></span> |
| <span data-ttu-id="06dbe-373">eventTimestamp</span><span class="sxs-lookup"><span data-stu-id="06dbe-373">eventTimestamp</span></span> |<span data-ttu-id="06dbe-374">Hello esemény lett létrehozva, amikor hello Azure szolgáltatás feldolgozása hello idejét jelző időbélyegző megfelelő hello esemény kérelmet.</span><span class="sxs-lookup"><span data-stu-id="06dbe-374">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="06dbe-375">submissionTimestamp</span><span class="sxs-lookup"><span data-stu-id="06dbe-375">submissionTimestamp</span></span> |<span data-ttu-id="06dbe-376">Hello esemény váltak elérhetővé lekérdezése idejét jelző időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="06dbe-376">Timestamp when hello event became available for querying.</span></span> |
| <span data-ttu-id="06dbe-377">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="06dbe-377">subscriptionId</span></span> |<span data-ttu-id="06dbe-378">Az Azure előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="06dbe-378">Azure Subscription Id.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="06dbe-379">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06dbe-379">Next steps</span></span>
* [<span data-ttu-id="06dbe-380">További tudnivalók hello tevékenységnapló (korábbi nevén naplók)</span><span class="sxs-lookup"><span data-stu-id="06dbe-380">Learn more about hello Activity Log (formerly Audit Logs)</span></span>](monitoring-overview-activity-logs.md)
* [<span data-ttu-id="06dbe-381">Adatfolyam-hello Azure tevékenységnapló tooEvent hubok</span><span class="sxs-lookup"><span data-stu-id="06dbe-381">Stream hello Azure Activity Log tooEvent Hubs</span></span>](monitoring-stream-activity-logs-event-hubs.md)
