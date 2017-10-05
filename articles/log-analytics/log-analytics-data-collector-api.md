---
title: "Elemzés API HTTP adatgyűjtő naplózása |} Microsoft Docs"
description: "A napló Analytics HTTP Data Collector API segítségével POST JSON-adatokat hozzáadni a Naplóelemzési tárház minden ügyfélről, amely a REST API meghívása. Ez a cikk ismerteti, hogyan lehet az API-val, és példák közzétegyék az adataikat a különböző programnyelveken rendelkezik."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: bwren
ms.openlocfilehash: b0c45ff8c1d4c9d35fbb3c8839b38a20df277055
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="send-data-to-log-analytics-with-the-http-data-collector-api"></a><span data-ttu-id="53234-104">Adatokat küldeni a Log Analyticshez HTTP adatait gyűjtője API-val</span><span class="sxs-lookup"><span data-stu-id="53234-104">Send data to Log Analytics with the HTTP Data Collector API</span></span>
<span data-ttu-id="53234-105">Ez a cikk bemutatja, hogyan használja a HTTP adatok adatgyűjtő API REST API-ügyfél Naplóelemzési adatküldéshez.</span><span class="sxs-lookup"><span data-stu-id="53234-105">This article shows you how to use the HTTP Data Collector API to send data to Log Analytics from a REST API client.</span></span>  <span data-ttu-id="53234-106">Bemutatja, hogyan lehet a parancsfájl vagy az alkalmazás által összegyűjtött adatok formázása, adja hozzá a kérelem és a kérésre Naplóelemzési engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="53234-106">It describes how to format data collected by your script or application, include it in a request, and have that request authorized by Log Analytics.</span></span>  <span data-ttu-id="53234-107">A példák PowerShell, a C# és Python.</span><span class="sxs-lookup"><span data-stu-id="53234-107">Examples are provided for PowerShell, C#, and Python.</span></span>

## <a name="concepts"></a><span data-ttu-id="53234-108">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="53234-108">Concepts</span></span>
<span data-ttu-id="53234-109">A HTTP-adatokat gyűjtő API segítségével bármely ügyfélnek, amely a REST API meghívása Naplóelemzési adatküldéshez.</span><span class="sxs-lookup"><span data-stu-id="53234-109">You can use the HTTP Data Collector API to send data to Log Analytics from any client that can call a REST API.</span></span>  <span data-ttu-id="53234-110">Ez lehet egy runbook az Azure Automationben, hogy a gyűjtő felügyeleti adatokat Azure vagy egy másik felhőben, vagy azt egy másik felügyeleti rendszer összefogása és elemezhetik a Naplóelemzési használó lehet.</span><span class="sxs-lookup"><span data-stu-id="53234-110">This might be a runbook in Azure Automation that collects management data from Azure or another cloud, or it might be an alternate management system that uses Log Analytics to consolidate and analyze data.</span></span>

<span data-ttu-id="53234-111">A Naplóelemzési tárházban található összes adat egy rekord, egy adott rekordtípus tárolja.</span><span class="sxs-lookup"><span data-stu-id="53234-111">All data in the Log Analytics repository is stored as a record with a particular record type.</span></span>  <span data-ttu-id="53234-112">Az adatok több bejegyzést a JSON-ban, a HTTP-adatokat gyűjtő API küldendő formázása.</span><span class="sxs-lookup"><span data-stu-id="53234-112">You format your data to send to the HTTP Data Collector API as multiple records in JSON.</span></span>  <span data-ttu-id="53234-113">Elküldeni az adatokat, ha egy egyéni rekord jön létre az egyes rekordokhoz, a kérelem hasznos a tárházban.</span><span class="sxs-lookup"><span data-stu-id="53234-113">When you submit the data, an individual record is created in the repository for each record in the request payload.</span></span>


![HTTP adatgyűjtő áttekintése](media/log-analytics-data-collector-api/overview.png)



## <a name="create-a-request"></a><span data-ttu-id="53234-115">Kérelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="53234-115">Create a request</span></span>
<span data-ttu-id="53234-116">A HTTP-adatokat gyűjtő API használatához hozzon létre egy POST kérést, amely tartalmazza az adatokat küldeni a JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="53234-116">To use the HTTP Data Collector API, you create a POST request that includes the data to send in JavaScript Object Notation (JSON).</span></span>  <span data-ttu-id="53234-117">A következő három táblázatokban az attribútumok, amelyek szükségesek az egyes kérelmek.</span><span class="sxs-lookup"><span data-stu-id="53234-117">The next three tables list the attributes that are required for each request.</span></span> <span data-ttu-id="53234-118">Azt írja le a cikk későbbi részében részletesebben összes attribútumot.</span><span class="sxs-lookup"><span data-stu-id="53234-118">We describe each attribute in more detail later in the article.</span></span>

### <a name="request-uri"></a><span data-ttu-id="53234-119">Kérelem URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="53234-119">Request URI</span></span>
| <span data-ttu-id="53234-120">Attribútum</span><span class="sxs-lookup"><span data-stu-id="53234-120">Attribute</span></span> | <span data-ttu-id="53234-121">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="53234-121">Property</span></span> |
|:--- |:--- |
| <span data-ttu-id="53234-122">Módszer</span><span class="sxs-lookup"><span data-stu-id="53234-122">Method</span></span> |<span data-ttu-id="53234-123">POST</span><span class="sxs-lookup"><span data-stu-id="53234-123">POST</span></span> |
| <span data-ttu-id="53234-124">URI</span><span class="sxs-lookup"><span data-stu-id="53234-124">URI</span></span> |<span data-ttu-id="53234-125">https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01</span><span class="sxs-lookup"><span data-stu-id="53234-125">https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01</span></span> |
| <span data-ttu-id="53234-126">Tartalom típusa</span><span class="sxs-lookup"><span data-stu-id="53234-126">Content type</span></span> |<span data-ttu-id="53234-127">application/json</span><span class="sxs-lookup"><span data-stu-id="53234-127">application/json</span></span> |

### <a name="request-uri-parameters"></a><span data-ttu-id="53234-128">A kérelem URI-paraméterei</span><span class="sxs-lookup"><span data-stu-id="53234-128">Request URI parameters</span></span>
| <span data-ttu-id="53234-129">Paraméter</span><span class="sxs-lookup"><span data-stu-id="53234-129">Parameter</span></span> | <span data-ttu-id="53234-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="53234-130">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="53234-131">CustomerID</span><span class="sxs-lookup"><span data-stu-id="53234-131">CustomerID</span></span> |<span data-ttu-id="53234-132">A Microsoft Operations Management Suite-munkaterülettel egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="53234-132">The unique identifier for the Microsoft Operations Management Suite workspace.</span></span> |
| <span data-ttu-id="53234-133">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="53234-133">Resource</span></span> |<span data-ttu-id="53234-134">Az API-erőforrás neve: / api/logs.</span><span class="sxs-lookup"><span data-stu-id="53234-134">The API resource name: /api/logs.</span></span> |
| <span data-ttu-id="53234-135">API-verzió</span><span class="sxs-lookup"><span data-stu-id="53234-135">API Version</span></span> |<span data-ttu-id="53234-136">A kérelem használandó API verziója.</span><span class="sxs-lookup"><span data-stu-id="53234-136">The version of the API to use with this request.</span></span> <span data-ttu-id="53234-137">Ez jelenleg 2016-04-01.</span><span class="sxs-lookup"><span data-stu-id="53234-137">Currently, it's 2016-04-01.</span></span> |

### <a name="request-headers"></a><span data-ttu-id="53234-138">Kérelem fejlécei</span><span class="sxs-lookup"><span data-stu-id="53234-138">Request headers</span></span>
| <span data-ttu-id="53234-139">Fejléc</span><span class="sxs-lookup"><span data-stu-id="53234-139">Header</span></span> | <span data-ttu-id="53234-140">Leírás</span><span class="sxs-lookup"><span data-stu-id="53234-140">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="53234-141">Engedélyezés</span><span class="sxs-lookup"><span data-stu-id="53234-141">Authorization</span></span> |<span data-ttu-id="53234-142">Az engedélyezési aláírás.</span><span class="sxs-lookup"><span data-stu-id="53234-142">The authorization signature.</span></span> <span data-ttu-id="53234-143">A cikk későbbi részében olvashat létrehozása egy HMAC-SHA256-fejlécben.</span><span class="sxs-lookup"><span data-stu-id="53234-143">Later in the article, you can read about how to create an HMAC-SHA256 header.</span></span> |
| <span data-ttu-id="53234-144">Napló-típusa</span><span class="sxs-lookup"><span data-stu-id="53234-144">Log-Type</span></span> |<span data-ttu-id="53234-145">Adja meg az adatok küldése folyamatban rekord típusát.</span><span class="sxs-lookup"><span data-stu-id="53234-145">Specify the record type of the data that is being submitted.</span></span> <span data-ttu-id="53234-146">A napló típusa jelenleg csak alfanumerikus karaktereket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="53234-146">Currently, the log type supports only alpha characters.</span></span> <span data-ttu-id="53234-147">Nem támogatja írhatók vagy speciális karaktereket.</span><span class="sxs-lookup"><span data-stu-id="53234-147">It does not support numerics or special characters.</span></span> |
| <span data-ttu-id="53234-148">x-ms-dátuma</span><span class="sxs-lookup"><span data-stu-id="53234-148">x-ms-date</span></span> |<span data-ttu-id="53234-149">A kérelem feldolgozása, RFC 1123 formátumban dátuma.</span><span class="sxs-lookup"><span data-stu-id="53234-149">The date that the request was processed, in RFC 1123 format.</span></span> |
| <span data-ttu-id="53234-150">idő generált mező</span><span class="sxs-lookup"><span data-stu-id="53234-150">time-generated-field</span></span> |<span data-ttu-id="53234-151">Az adatok, amely tartalmazza az elem a Timestamp típusú mező neve.</span><span class="sxs-lookup"><span data-stu-id="53234-151">The name of a field in the data that contains the timestamp of the data item.</span></span> <span data-ttu-id="53234-152">Ha a megadott mező, akkor annak tartalmát használt **TimeGenerated**.</span><span class="sxs-lookup"><span data-stu-id="53234-152">If you specify a field then its contents are used for **TimeGenerated**.</span></span> <span data-ttu-id="53234-153">Ha ez a mező nincs megadva, az alapértelmezett **TimeGenerated** a alkalom, hogy az üzenet van okozhatnak.</span><span class="sxs-lookup"><span data-stu-id="53234-153">If this field isn’t specified, the default for **TimeGenerated** is the time that the message is ingested.</span></span> <span data-ttu-id="53234-154">A mező tartalmának érdemes követnie az ISO 8601 formátum éééé-hh-SSz.</span><span class="sxs-lookup"><span data-stu-id="53234-154">The contents of the message field should follow the ISO 8601 format YYYY-MM-DDThh:mm:ssZ.</span></span> |

## <a name="authorization"></a><span data-ttu-id="53234-155">Engedélyezés</span><span class="sxs-lookup"><span data-stu-id="53234-155">Authorization</span></span>
<span data-ttu-id="53234-156">A napló Analytics HTTP adatokat gyűjtő API kérésének tartalmaznia kell egy engedélyezési fejléc.</span><span class="sxs-lookup"><span data-stu-id="53234-156">Any request to the Log Analytics HTTP Data Collector API must include an authorization header.</span></span> <span data-ttu-id="53234-157">A kérés hitelesítéséhez, be kell jelentkeznie a kérelmet az elsődleges vagy másodlagos kulcsát a munkaterületen, a kérést.</span><span class="sxs-lookup"><span data-stu-id="53234-157">To authenticate a request, you must sign the request with either the primary or the secondary key for the workspace that is making the request.</span></span> <span data-ttu-id="53234-158">Így továbbítsa az adott aláírás a kérelem részeként.</span><span class="sxs-lookup"><span data-stu-id="53234-158">Then, pass that signature as part of the request.</span></span>   

<span data-ttu-id="53234-159">Az engedélyezési fejléc formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="53234-159">Here's the format for the authorization header:</span></span>

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

<span data-ttu-id="53234-160">*WorkspaceID* az Operations Management Suite-munkaterülettel egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="53234-160">*WorkspaceID* is the unique identifier for the Operations Management Suite workspace.</span></span> <span data-ttu-id="53234-161">*Aláírás* van egy [kivonat-alapú üzenethitelesítési kód (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) , a kérelem összeállított és majd számított használatával a [SHA-256 algoritmus](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx).</span><span class="sxs-lookup"><span data-stu-id="53234-161">*Signature* is a [Hash-based Message Authentication Code (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) that is constructed from the request and then computed by using the [SHA256 algorithm](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx).</span></span> <span data-ttu-id="53234-162">Majd hogy kódolása Base64 kódolással.</span><span class="sxs-lookup"><span data-stu-id="53234-162">Then, you encode it by using Base64 encoding.</span></span>

<span data-ttu-id="53234-163">Ezt a formátumot használja kódolására a **SharedKey** aláírás-karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="53234-163">Use this format to encode the **SharedKey** signature string:</span></span>

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

<span data-ttu-id="53234-164">Íme egy példa látható egy aláírás karakterláncra:</span><span class="sxs-lookup"><span data-stu-id="53234-164">Here's an example of a signature string:</span></span>

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

<span data-ttu-id="53234-165">Ha az aláírás karakterlánc, a HMAC-SHA-256 algoritmus használatával az UTF-8 kódolású karakterlánc kódolása, és majd a az eredmény Base64 kódolása.</span><span class="sxs-lookup"><span data-stu-id="53234-165">When you have the signature string, encode it by using the HMAC-SHA256 algorithm on the UTF-8-encoded string, and then encode the result as Base64.</span></span> <span data-ttu-id="53234-166">Ezt a formátumot használja:</span><span class="sxs-lookup"><span data-stu-id="53234-166">Use this format:</span></span>

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

<span data-ttu-id="53234-167">A mintákat a következő szakaszokban lévő mintakód hozhat létre egy engedélyezési fejléc rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="53234-167">The samples in the next sections have sample code to help you create an authorization header.</span></span>

## <a name="request-body"></a><span data-ttu-id="53234-168">Kérés törzsében</span><span class="sxs-lookup"><span data-stu-id="53234-168">Request body</span></span>
<span data-ttu-id="53234-169">Az üzenet törzsét JSON kell lennie.</span><span class="sxs-lookup"><span data-stu-id="53234-169">The body of the message must be in JSON.</span></span> <span data-ttu-id="53234-170">A tulajdonság név-érték párok egy vagy több rekordot a következő formátumban kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="53234-170">It must include one or more records with the property name and value pairs in this format:</span></span>

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

<span data-ttu-id="53234-171">A köteg egyetlen kérelemmel együtt a több rekordot a következő formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="53234-171">You can batch multiple records together in a single request by using the following format.</span></span> <span data-ttu-id="53234-172">A rekordok az azonos rekord típusúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="53234-172">All the records must be the same record type.</span></span>

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a><span data-ttu-id="53234-173">Bejegyzéstípus és tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="53234-173">Record type and properties</span></span>
<span data-ttu-id="53234-174">Az egyéni típusát határozza meg, amikor a napló Analytics HTTP adatait gyűjtője API-n keresztül adatokat.</span><span class="sxs-lookup"><span data-stu-id="53234-174">You define a custom record type when you submit data through the Log Analytics HTTP Data Collector API.</span></span> <span data-ttu-id="53234-175">Jelenleg, akkor nem lehet adatokat írni rekord típusú létező létrehozott más adattípusok és megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="53234-175">Currently, you can't write data to existing record types that were created by other data types and solutions.</span></span> <span data-ttu-id="53234-176">A Naplóelemzési a bejövő adatokat olvas, és ezután hoz létre, amelyek megfelelnek a megadott értékek adatok típusú tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="53234-176">Log Analytics reads the incoming data and then creates properties that match the data types of the values that you enter.</span></span>

<span data-ttu-id="53234-177">A napló Analytics API kéréseknek tartalmaznia kell egy **hibanapló-típus** a rekordtípus nevű fejléc.</span><span class="sxs-lookup"><span data-stu-id="53234-177">Each request to the Log Analytics API must include a **Log-Type** header with the name for the record type.</span></span> <span data-ttu-id="53234-178">A utótag **_CL** automatikusan hozzáfűzi a neve meg egy egyéni napló, a más napló megkülönböztetésére.</span><span class="sxs-lookup"><span data-stu-id="53234-178">The suffix **_CL** is automatically appended to the name you enter to distinguish it from other log types as a custom log.</span></span> <span data-ttu-id="53234-179">Például, ha beírja a nevét **MyNewRecordType**, Log Analyticshez azzal a típussal, létrehoz egy rekordot **MyNewRecordType_CL**.</span><span class="sxs-lookup"><span data-stu-id="53234-179">For example, if you enter the name **MyNewRecordType**, Log Analytics creates a record with the type **MyNewRecordType_CL**.</span></span> <span data-ttu-id="53234-180">Ezzel biztosíthatja, hogy nincsenek-e a felhasználó által létrehozott típusnevek és a jelenlegi vagy jövőbeli Microsoft megoldások szállított közötti ütközések.</span><span class="sxs-lookup"><span data-stu-id="53234-180">This helps ensure that there are no conflicts between user-created type names and those shipped in current or future Microsoft solutions.</span></span>

<span data-ttu-id="53234-181">A tulajdonság adattípusát kikereséséhez Naplóelemzési hozzáadja egy utótagot a tulajdonságnév.</span><span class="sxs-lookup"><span data-stu-id="53234-181">To identify a property's data type, Log Analytics adds a suffix to the property name.</span></span> <span data-ttu-id="53234-182">Tulajdonság értéke null, ha a tulajdonság nem szerepel a rekordban.</span><span class="sxs-lookup"><span data-stu-id="53234-182">If a property contains a null value, the property is not included in that record.</span></span> <span data-ttu-id="53234-183">Ez a táblázat felsorolja a tulajdonságadat-típus és a megfelelő utótag:</span><span class="sxs-lookup"><span data-stu-id="53234-183">This table lists the property data type and corresponding suffix:</span></span>

| <span data-ttu-id="53234-184">Tulajdonságadat-típus</span><span class="sxs-lookup"><span data-stu-id="53234-184">Property data type</span></span> | <span data-ttu-id="53234-185">Utótag</span><span class="sxs-lookup"><span data-stu-id="53234-185">Suffix</span></span> |
|:--- |:--- |
| <span data-ttu-id="53234-186">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="53234-186">String</span></span> |<span data-ttu-id="53234-187">z</span><span class="sxs-lookup"><span data-stu-id="53234-187">_s</span></span> |
| <span data-ttu-id="53234-188">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="53234-188">Boolean</span></span> |<span data-ttu-id="53234-189">_B</span><span class="sxs-lookup"><span data-stu-id="53234-189">_b</span></span> |
| <span data-ttu-id="53234-190">Dupla</span><span class="sxs-lookup"><span data-stu-id="53234-190">Double</span></span> |<span data-ttu-id="53234-191">_D</span><span class="sxs-lookup"><span data-stu-id="53234-191">_d</span></span> |
| <span data-ttu-id="53234-192">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="53234-192">Date/time</span></span> |<span data-ttu-id="53234-193">_Szo</span><span class="sxs-lookup"><span data-stu-id="53234-193">_t</span></span> |
| <span data-ttu-id="53234-194">GUID</span><span class="sxs-lookup"><span data-stu-id="53234-194">GUID</span></span> |<span data-ttu-id="53234-195">_G</span><span class="sxs-lookup"><span data-stu-id="53234-195">_g</span></span> |

<span data-ttu-id="53234-196">Az adattípus, amely Naplóelemzési alkalmaz minden egyes tulajdonság attól függ, az új bejegyzés bejegyzéstípus létezik-e.</span><span class="sxs-lookup"><span data-stu-id="53234-196">The data type that Log Analytics uses for each property depends on whether the record type for the new record already exists.</span></span>

* <span data-ttu-id="53234-197">Ha a rekord típusa nem létezik, a Naplóelemzési hoz létre egy új.</span><span class="sxs-lookup"><span data-stu-id="53234-197">If the record type does not exist, Log Analytics creates a new one.</span></span> <span data-ttu-id="53234-198">A Naplóelemzési a JSON-típusok kikövetkeztetése alapján határozza meg az új rekord minden egyes tulajdonság adattípusára.</span><span class="sxs-lookup"><span data-stu-id="53234-198">Log Analytics uses the JSON type inference to determine the data type for each property for the new record.</span></span>
* <span data-ttu-id="53234-199">Ha a rekord típusa létezik, a Naplóelemzési megkísérli hozzon létre egy új rekordot a meglévő tulajdonságok alapján.</span><span class="sxs-lookup"><span data-stu-id="53234-199">If the record type does exist, Log Analytics attempts to create a new record based on existing properties.</span></span> <span data-ttu-id="53234-200">Ha az adattípust a egy tulajdonság az új rekordban nem felel meg, és a már meglévő típus nem konvertálható, vagy ha a rekord tartalmaz olyan tulajdonságon, amely nem létezik, Log Analyticshez hoz létre egy új tulajdonság, amely rendelkezik a megfelelő utótag.</span><span class="sxs-lookup"><span data-stu-id="53234-200">If the data type for a property in the new record doesn’t match and can’t be converted to the existing type, or if the record includes a property that doesn’t exist, Log Analytics creates a new property that has the relevant suffix.</span></span>

<span data-ttu-id="53234-201">Például a küldésének bejegyzés hozna létre egy olyan rekordot három tulajdonságokkal **number_d**, **boolean_b**, és **string_s**:</span><span class="sxs-lookup"><span data-stu-id="53234-201">For example, this submission entry would create a record with three properties, **number_d**, **boolean_b**, and **string_s**:</span></span>

![A minta rekord 1](media/log-analytics-data-collector-api/record-01.png)

<span data-ttu-id="53234-203">Ha ezután a következő bejegyzés formázott karakterláncok értékekkel, a tulajdonságok nem megváltozna.</span><span class="sxs-lookup"><span data-stu-id="53234-203">If you then submitted this next entry, with all values formatted as strings, the properties would not change.</span></span> <span data-ttu-id="53234-204">Ezek az értékek konvertálhatók meglévő adattípusok:</span><span class="sxs-lookup"><span data-stu-id="53234-204">These values can be converted to existing data types:</span></span>

![2. példa rekord](media/log-analytics-data-collector-api/record-02.png)

<span data-ttu-id="53234-206">De ha a következő küldésének majd végrehajtott Naplóelemzési rekordot kell létrehoznia az új tulajdonságok **boolean_d** és **string_d**.</span><span class="sxs-lookup"><span data-stu-id="53234-206">But, if you then made this next submission, Log Analytics would create the new properties **boolean_d** and **string_d**.</span></span> <span data-ttu-id="53234-207">Ezeket az értékeket nem lehet konvertálni:</span><span class="sxs-lookup"><span data-stu-id="53234-207">These values can't be converted:</span></span>

![A minta rekord 3](media/log-analytics-data-collector-api/record-03.png)

<span data-ttu-id="53234-209">A következő bejegyzést, majd a rekordtípus létrehozása előtt elküldve, ha Naplóelemzési három tulajdonságokkal hozna létre egy olyan rekordot **sikeresek**, **boolean_s**, és **string_s**.</span><span class="sxs-lookup"><span data-stu-id="53234-209">If you then submitted the following entry, before the record type was created, Log Analytics would create a record with three properties, **number_s**, **boolean_s**, and **string_s**.</span></span> <span data-ttu-id="53234-210">Ez a bejegyzés a kezdeti értékeit formázott karakterláncként:</span><span class="sxs-lookup"><span data-stu-id="53234-210">In this entry, each of the initial values is formatted as a string:</span></span>

![4. példa rekord](media/log-analytics-data-collector-api/record-04.png)

## <a name="data-limits"></a><span data-ttu-id="53234-212">Adatok korlátok</span><span class="sxs-lookup"><span data-stu-id="53234-212">Data limits</span></span>
<span data-ttu-id="53234-213">Nincsenek bizonyos korlátozások a a Log Analytics-adatok gyűjtése API közzé adatok körül.</span><span class="sxs-lookup"><span data-stu-id="53234-213">There are some constraints around the data posted to the Log Analytics Data collection API.</span></span>

* <span data-ttu-id="53234-214">Legfeljebb 30 MB post napló Analytics adatokat gyűjtő API.</span><span class="sxs-lookup"><span data-stu-id="53234-214">Maximum of 30 MB per post to Log Analytics Data Collector API.</span></span> <span data-ttu-id="53234-215">Ez az egyetlen post méretkorlátot.</span><span class="sxs-lookup"><span data-stu-id="53234-215">This is a size limit for a single post.</span></span> <span data-ttu-id="53234-216">Ha az adatok egyetlen utáni, amely 30 MB meghaladja, az adatok akár kisebb méretű adattömböket írnak és elküldheti azokat párhuzamosan kell.</span><span class="sxs-lookup"><span data-stu-id="53234-216">If the data from a single post that exceeds 30 MB, you should split the data up to smaller sized chunks and send them concurrently.</span></span>
* <span data-ttu-id="53234-217">Legfeljebb 32 KB-os korlát mezők értékét.</span><span class="sxs-lookup"><span data-stu-id="53234-217">Maximum of 32 KB limit for field values.</span></span> <span data-ttu-id="53234-218">Ha 32 KB-nál nagyobb értéket, az adatok csonkolva lesz.</span><span class="sxs-lookup"><span data-stu-id="53234-218">If the field value is greater than 32 KB, the data will be truncated.</span></span>
* <span data-ttu-id="53234-219">Egy adott típusú mezőket ajánlott maximális száma érték az 50.</span><span class="sxs-lookup"><span data-stu-id="53234-219">Recommended maximum number of fields for a given type is 50.</span></span> <span data-ttu-id="53234-220">Ez a használhatóság és keresési élményt szempontból gyakorlati korlátozni.</span><span class="sxs-lookup"><span data-stu-id="53234-220">This is a practical limit from a usability and search experience perspective.</span></span>  

## <a name="return-codes"></a><span data-ttu-id="53234-221">A visszatérési kódok</span><span class="sxs-lookup"><span data-stu-id="53234-221">Return codes</span></span>
<span data-ttu-id="53234-222">200-as HTTP-állapotkód: azt jelenti, hogy a kérelem érkezett a feldolgozáshoz.</span><span class="sxs-lookup"><span data-stu-id="53234-222">The HTTP status code 200 means that the request has been received for processing.</span></span> <span data-ttu-id="53234-223">Ez azt jelzi, hogy a művelet sikeresen befejeződött-e.</span><span class="sxs-lookup"><span data-stu-id="53234-223">This indicates that the operation completed successfully.</span></span>

<span data-ttu-id="53234-224">Ez a táblázat felsorolja a szolgáltatás esetleg vissza állapotkódokat teljes készletének:</span><span class="sxs-lookup"><span data-stu-id="53234-224">This table lists the complete set of status codes that the service might return:</span></span>

| <span data-ttu-id="53234-225">Kód</span><span class="sxs-lookup"><span data-stu-id="53234-225">Code</span></span> | <span data-ttu-id="53234-226">status</span><span class="sxs-lookup"><span data-stu-id="53234-226">Status</span></span> | <span data-ttu-id="53234-227">Hibakód:</span><span class="sxs-lookup"><span data-stu-id="53234-227">Error code</span></span> | <span data-ttu-id="53234-228">Leírás</span><span class="sxs-lookup"><span data-stu-id="53234-228">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="53234-229">200</span><span class="sxs-lookup"><span data-stu-id="53234-229">200</span></span> |<span data-ttu-id="53234-230">OKÉ</span><span class="sxs-lookup"><span data-stu-id="53234-230">OK</span></span> | |<span data-ttu-id="53234-231">A kérés sikeresen elfogadva.</span><span class="sxs-lookup"><span data-stu-id="53234-231">The request was successfully accepted.</span></span> |
| <span data-ttu-id="53234-232">400</span><span class="sxs-lookup"><span data-stu-id="53234-232">400</span></span> |<span data-ttu-id="53234-233">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="53234-233">Bad request</span></span> |<span data-ttu-id="53234-234">InactiveCustomer</span><span class="sxs-lookup"><span data-stu-id="53234-234">InactiveCustomer</span></span> |<span data-ttu-id="53234-235">A munkaterület be lett zárva.</span><span class="sxs-lookup"><span data-stu-id="53234-235">The workspace has been closed.</span></span> |
| <span data-ttu-id="53234-236">400</span><span class="sxs-lookup"><span data-stu-id="53234-236">400</span></span> |<span data-ttu-id="53234-237">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="53234-237">Bad request</span></span> |<span data-ttu-id="53234-238">InvalidApiVersion</span><span class="sxs-lookup"><span data-stu-id="53234-238">InvalidApiVersion</span></span> |<span data-ttu-id="53234-239">A megadott API-verzió nem ismerte fel a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="53234-239">The API version that you specified was not recognized by the service.</span></span> |
| <span data-ttu-id="53234-240">400</span><span class="sxs-lookup"><span data-stu-id="53234-240">400</span></span> |<span data-ttu-id="53234-241">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="53234-241">Bad request</span></span> |<span data-ttu-id="53234-242">InvalidCustomerId</span><span class="sxs-lookup"><span data-stu-id="53234-242">InvalidCustomerId</span></span> |<span data-ttu-id="53234-243">A megadott munkaterület-azonosító érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="53234-243">The workspace ID specified is invalid.</span></span> |
| <span data-ttu-id="53234-244">400</span><span class="sxs-lookup"><span data-stu-id="53234-244">400</span></span> |<span data-ttu-id="53234-245">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="53234-245">Bad request</span></span> |<span data-ttu-id="53234-246">InvalidDataFormat</span><span class="sxs-lookup"><span data-stu-id="53234-246">InvalidDataFormat</span></span> |<span data-ttu-id="53234-247">Érvénytelen JSON el lett küldve.</span><span class="sxs-lookup"><span data-stu-id="53234-247">Invalid JSON was submitted.</span></span> <span data-ttu-id="53234-248">Az adott válasz törzsének tartalmazhatnak a hiba megoldásával kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="53234-248">The response body might contain more information about how to resolve the error.</span></span> |
| <span data-ttu-id="53234-249">400</span><span class="sxs-lookup"><span data-stu-id="53234-249">400</span></span> |<span data-ttu-id="53234-250">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="53234-250">Bad request</span></span> |<span data-ttu-id="53234-251">InvalidLogType</span><span class="sxs-lookup"><span data-stu-id="53234-251">InvalidLogType</span></span> |<span data-ttu-id="53234-252">A napló típusa megadott tartalmazott különleges karaktereket vagy írhatók.</span><span class="sxs-lookup"><span data-stu-id="53234-252">The log type specified contained special characters or numerics.</span></span> |
| <span data-ttu-id="53234-253">400</span><span class="sxs-lookup"><span data-stu-id="53234-253">400</span></span> |<span data-ttu-id="53234-254">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="53234-254">Bad request</span></span> |<span data-ttu-id="53234-255">MissingApiVersion</span><span class="sxs-lookup"><span data-stu-id="53234-255">MissingApiVersion</span></span> |<span data-ttu-id="53234-256">Az API-verzió nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="53234-256">The API version wasn’t specified.</span></span> |
| <span data-ttu-id="53234-257">400</span><span class="sxs-lookup"><span data-stu-id="53234-257">400</span></span> |<span data-ttu-id="53234-258">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="53234-258">Bad request</span></span> |<span data-ttu-id="53234-259">MissingContentType</span><span class="sxs-lookup"><span data-stu-id="53234-259">MissingContentType</span></span> |<span data-ttu-id="53234-260">A tartalom típusa nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="53234-260">The content type wasn’t specified.</span></span> |
| <span data-ttu-id="53234-261">400</span><span class="sxs-lookup"><span data-stu-id="53234-261">400</span></span> |<span data-ttu-id="53234-262">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="53234-262">Bad request</span></span> |<span data-ttu-id="53234-263">MissingLogType</span><span class="sxs-lookup"><span data-stu-id="53234-263">MissingLogType</span></span> |<span data-ttu-id="53234-264">A szükséges érték napló típusa nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="53234-264">The required value log type wasn’t specified.</span></span> |
| <span data-ttu-id="53234-265">400</span><span class="sxs-lookup"><span data-stu-id="53234-265">400</span></span> |<span data-ttu-id="53234-266">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="53234-266">Bad request</span></span> |<span data-ttu-id="53234-267">UnsupportedContentType</span><span class="sxs-lookup"><span data-stu-id="53234-267">UnsupportedContentType</span></span> |<span data-ttu-id="53234-268">Nincs beállítva a tartalomtípus **az application/json**.</span><span class="sxs-lookup"><span data-stu-id="53234-268">The content type was not set to **application/json**.</span></span> |
| <span data-ttu-id="53234-269">403</span><span class="sxs-lookup"><span data-stu-id="53234-269">403</span></span> |<span data-ttu-id="53234-270">Tiltott</span><span class="sxs-lookup"><span data-stu-id="53234-270">Forbidden</span></span> |<span data-ttu-id="53234-271">InvalidAuthorization</span><span class="sxs-lookup"><span data-stu-id="53234-271">InvalidAuthorization</span></span> |<span data-ttu-id="53234-272">A szolgáltatás nem tudta hitelesíteni a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="53234-272">The service failed to authenticate the request.</span></span> <span data-ttu-id="53234-273">Győződjön meg arról, hogy a munkaterület azonosítója és a kapcsolat kulcs érvényesek.</span><span class="sxs-lookup"><span data-stu-id="53234-273">Verify that the workspace ID and connection key are valid.</span></span> |
| <span data-ttu-id="53234-274">404</span><span class="sxs-lookup"><span data-stu-id="53234-274">404</span></span> |<span data-ttu-id="53234-275">Nem található</span><span class="sxs-lookup"><span data-stu-id="53234-275">Not Found</span></span> | | <span data-ttu-id="53234-276">A megadott URL-cím érvénytelen, vagy a kérelem túl nagy.</span><span class="sxs-lookup"><span data-stu-id="53234-276">Either the URL provided is incorrect, or the request is too large.</span></span> |
| <span data-ttu-id="53234-277">429</span><span class="sxs-lookup"><span data-stu-id="53234-277">429</span></span> |<span data-ttu-id="53234-278">Túl sok kérelem</span><span class="sxs-lookup"><span data-stu-id="53234-278">Too Many Requests</span></span> | | <span data-ttu-id="53234-279">A szolgáltatás problémát nagyszámú fiókja adatait.</span><span class="sxs-lookup"><span data-stu-id="53234-279">The service is experiencing a high volume of data from your account.</span></span> <span data-ttu-id="53234-280">Próbálkozzon újra később a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="53234-280">Please retry the request later.</span></span> |
| <span data-ttu-id="53234-281">500</span><span class="sxs-lookup"><span data-stu-id="53234-281">500</span></span> |<span data-ttu-id="53234-282">Belső kiszolgálóhiba.</span><span class="sxs-lookup"><span data-stu-id="53234-282">Internal Server Error</span></span> |<span data-ttu-id="53234-283">UnspecifiedError</span><span class="sxs-lookup"><span data-stu-id="53234-283">UnspecifiedError</span></span> |<span data-ttu-id="53234-284">A szolgáltatás belső hibát észlelt.</span><span class="sxs-lookup"><span data-stu-id="53234-284">The service encountered an internal error.</span></span> <span data-ttu-id="53234-285">Próbálja megismételni a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="53234-285">Please retry the request.</span></span> |
| <span data-ttu-id="53234-286">503</span><span class="sxs-lookup"><span data-stu-id="53234-286">503</span></span> |<span data-ttu-id="53234-287">A szolgáltatás nem érhető el</span><span class="sxs-lookup"><span data-stu-id="53234-287">Service Unavailable</span></span> |<span data-ttu-id="53234-288">ServiceUnavailable</span><span class="sxs-lookup"><span data-stu-id="53234-288">ServiceUnavailable</span></span> |<span data-ttu-id="53234-289">A szolgáltatás jelenleg nem érhető el a kérelmek fogadására.</span><span class="sxs-lookup"><span data-stu-id="53234-289">The service currently is unavailable to receive requests.</span></span> <span data-ttu-id="53234-290">Próbálja megismételni a kérést.</span><span class="sxs-lookup"><span data-stu-id="53234-290">Please retry your request.</span></span> |

## <a name="query-data"></a><span data-ttu-id="53234-291">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="53234-291">Query data</span></span>
<span data-ttu-id="53234-292">A napló Analytics HTTP adatokat gyűjtő API rekordokat keres által küldött lekérdezési adatok **típus** , amely megegyezik a **LogType** , amely a megadott érték hozzáíródik **_CL**.</span><span class="sxs-lookup"><span data-stu-id="53234-292">To query data submitted by the Log Analytics HTTP Data Collector API, search for records with **Type** that is equal to the **LogType** value that you specified, appended with **_CL**.</span></span> <span data-ttu-id="53234-293">Például, ha a használt **MyCustomLog**, majd az összes rekordot alakítanák vissza **típus = MyCustomLog_CL**.</span><span class="sxs-lookup"><span data-stu-id="53234-293">For example, if you used **MyCustomLog**, then you'd return all records with **Type=MyCustomLog_CL**.</span></span>

>[!NOTE]
> <span data-ttu-id="53234-294">Ha a munkaterületet lett frissítve a [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezés megváltozna a következők.</span><span class="sxs-lookup"><span data-stu-id="53234-294">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then the above query would change to the following.</span></span>

> `MyCustomLog_CL`

## <a name="sample-requests"></a><span data-ttu-id="53234-295">A minta-kérelmek</span><span class="sxs-lookup"><span data-stu-id="53234-295">Sample requests</span></span>
<span data-ttu-id="53234-296">A következő szakaszokban lévő látni fogja, hogy hogyan szeretné elküldeni a napló Analytics HTTP Data Collector API segítségével különböző programnyelveken minták.</span><span class="sxs-lookup"><span data-stu-id="53234-296">In the next sections, you'll find samples of how to submit data to the Log Analytics HTTP Data Collector API by using different programming languages.</span></span>

<span data-ttu-id="53234-297">Minden egyes minta hajtsa végre ezeket a lépéseket, a változók beállítása hitelesítési fejlécéhez:</span><span class="sxs-lookup"><span data-stu-id="53234-297">For each sample, do these steps to set the variables for the authorization header:</span></span>

1. <span data-ttu-id="53234-298">Az Operations Management Suite portálját, válassza ki a **beállítások** csempére, majd válassza ki a **csatlakoztatott források** fülre.</span><span class="sxs-lookup"><span data-stu-id="53234-298">In the Operations Management Suite portal, select the **Settings** tile, and then select the **Connected Sources** tab.</span></span>
2. <span data-ttu-id="53234-299">Jobb oldalán **munkaterület azonosítója**, válassza a Másolás ikonra, és illessze be az azonosító az értékeként a **ügyfél azonosítója** változó.</span><span class="sxs-lookup"><span data-stu-id="53234-299">To the right of **Workspace ID**, select the copy icon, and then paste the ID as the value of the **Customer ID** variable.</span></span>
3. <span data-ttu-id="53234-300">Jobb oldalán **elsődleges kulcs**, válassza a Másolás ikonra, és illessze be az azonosító az értékeként a **megosztott kulcsos** változó.</span><span class="sxs-lookup"><span data-stu-id="53234-300">To the right of **Primary Key**, select the copy icon, and then paste the ID as the value of the **Shared Key** variable.</span></span>

<span data-ttu-id="53234-301">Azt is megteheti módosíthatja a változók a napló típusa és a JSON-adatokat.</span><span class="sxs-lookup"><span data-stu-id="53234-301">Alternatively, you can change the variables for the log type and JSON data.</span></span>

### <a name="powershell-sample"></a><span data-ttu-id="53234-302">PowerShell-példa</span><span class="sxs-lookup"><span data-stu-id="53234-302">PowerShell sample</span></span>
```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a><span data-ttu-id="53234-303">C#-minta</span><span class="sxs-lookup"><span data-stu-id="53234-303">C# sample</span></span>
```
using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Security.Cryptography;
using System.Text;
using System.Threading.Tasks;

namespace OIAPIExample
{
    class ApiExample
    {
        // An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField3"":""DemoValue3"",""DemoField4"":""DemoValue4""}]";

        // Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

        // For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

        // LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

        // You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
            // Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

        // Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

        // Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            try
            {
                string url = "https://" + customerId + ".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";

                System.Net.Http.HttpClient client = new System.Net.Http.HttpClient();
                client.DefaultRequestHeaders.Add("Accept", "application/json");
                client.DefaultRequestHeaders.Add("Log-Type", LogName);
                client.DefaultRequestHeaders.Add("Authorization", signature);
                client.DefaultRequestHeaders.Add("x-ms-date", date);
                client.DefaultRequestHeaders.Add("time-generated-field", TimeStampField);

                System.Net.Http.HttpContent httpContent = new StringContent(json, Encoding.UTF8);
                httpContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                Task<System.Net.Http.HttpResponseMessage> response = client.PostAsync(new Uri(url), httpContent);

                System.Net.Http.HttpContent responseContent = response.Result.Content;
                string result = responseContent.ReadAsStringAsync().Result;
                Console.WriteLine("Return Result: " + result);
            }
            catch (Exception excep)
            {
                Console.WriteLine("API Post Exception: " + excep.Message);
            }
        }
    }
}

```

### <a name="python-sample"></a><span data-ttu-id="53234-304">Python-minta</span><span class="sxs-lookup"><span data-stu-id="53234-304">Python sample</span></span>
```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code >= 200 and response.status_code <= 299):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a><span data-ttu-id="53234-305">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="53234-305">Next steps</span></span>
- <span data-ttu-id="53234-306">Használja a [napló Search API](log-analytics-log-search-api.md) adatok lekérése a Log Analytics-tárházat.</span><span class="sxs-lookup"><span data-stu-id="53234-306">Use the [Log Search API](log-analytics-log-search-api.md) to retrieve data from the Log Analytics repository.</span></span>
