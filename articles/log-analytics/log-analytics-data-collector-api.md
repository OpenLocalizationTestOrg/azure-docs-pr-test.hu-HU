---
title: "aaaLog Analytics HTTP adatait gyűjtője API |} Microsoft Docs"
description: "Hello napló Analytics HTTP adatokat gyűjtő API tooadd POST JSON toohello Naplóelemzési adattárház bármely ügyfél, amely meghívhatja hello REST API-t is használhatja. Ez a cikk ismerteti, hogyan toouse hello API, és példákat a rendelkezik toopublish adatok különböző programnyelveken használatával."
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
ms.openlocfilehash: c2921082831c49da764d946ac9c4fab975a38185
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-data-toolog-analytics-with-hello-http-data-collector-api"></a><span data-ttu-id="719d3-104">Adatok tooLog Analytics a hello HTTP adatait gyűjtője API küldése</span><span class="sxs-lookup"><span data-stu-id="719d3-104">Send data tooLog Analytics with hello HTTP Data Collector API</span></span>
<span data-ttu-id="719d3-105">Ez a cikk bemutatja, hogyan toouse hello HTTP adatait gyűjtője API toosend adatok tooLog Analytics a REST API-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="719d3-105">This article shows you how toouse hello HTTP Data Collector API toosend data tooLog Analytics from a REST API client.</span></span>  <span data-ttu-id="719d3-106">Útmutatás a tooformat adatokat gyűjtött a parancsfájl vagy az alkalmazáshoz, adja hozzá a kérelem, és a van a Naplóelemzési által engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="719d3-106">It describes how tooformat data collected by your script or application, include it in a request, and have that request authorized by Log Analytics.</span></span>  <span data-ttu-id="719d3-107">A példák PowerShell, a C# és Python.</span><span class="sxs-lookup"><span data-stu-id="719d3-107">Examples are provided for PowerShell, C#, and Python.</span></span>

## <a name="concepts"></a><span data-ttu-id="719d3-108">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="719d3-108">Concepts</span></span>
<span data-ttu-id="719d3-109">Hello HTTP adatait gyűjtője API toosend adatok tooLog Analytics minden ügyfélről, amely a REST API meghívása is használhatja.</span><span class="sxs-lookup"><span data-stu-id="719d3-109">You can use hello HTTP Data Collector API toosend data tooLog Analytics from any client that can call a REST API.</span></span>  <span data-ttu-id="719d3-110">Ez lehet egy runbook az Azure Automationben, hogy a gyűjtő felügyeleti Azure vagy egy másik felhőben vagy adatait, előfordulhat, hogy egy másik felügyeleti rendszer Naplóelemzési tooconsolidate használó és adatok elemzése.</span><span class="sxs-lookup"><span data-stu-id="719d3-110">This might be a runbook in Azure Automation that collects management data from Azure or another cloud, or it might be an alternate management system that uses Log Analytics tooconsolidate and analyze data.</span></span>

<span data-ttu-id="719d3-111">Egy rekord, egy adott rekordtípus hello Naplóelemzési tárházban összes adatot tárolja.</span><span class="sxs-lookup"><span data-stu-id="719d3-111">All data in hello Log Analytics repository is stored as a record with a particular record type.</span></span>  <span data-ttu-id="719d3-112">Az adatok toosend toohello HTTP adatait gyűjtője API formázza a több rekordot a JSON-ban.</span><span class="sxs-lookup"><span data-stu-id="719d3-112">You format your data toosend toohello HTTP Data Collector API as multiple records in JSON.</span></span>  <span data-ttu-id="719d3-113">Hello adatokat küld, ha egy egyéni rekord hello tárházban hello-kérések forgalma lévő minden egyes rekord jön létre.</span><span class="sxs-lookup"><span data-stu-id="719d3-113">When you submit hello data, an individual record is created in hello repository for each record in hello request payload.</span></span>


![HTTP adatgyűjtő áttekintése](media/log-analytics-data-collector-api/overview.png)



## <a name="create-a-request"></a><span data-ttu-id="719d3-115">Kérelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="719d3-115">Create a request</span></span>
<span data-ttu-id="719d3-116">toouse hello HTTP adatgyűjtő API-hoz létre, amely tartalmazza az hello adatok toosend JavaScript Object Notation (JSON) a POST-kérelmet.</span><span class="sxs-lookup"><span data-stu-id="719d3-116">toouse hello HTTP Data Collector API, you create a POST request that includes hello data toosend in JavaScript Object Notation (JSON).</span></span>  <span data-ttu-id="719d3-117">hello a következő három táblák listáját hello attribútumok, amelyek szükségesek az egyes kérelmek.</span><span class="sxs-lookup"><span data-stu-id="719d3-117">hello next three tables list hello attributes that are required for each request.</span></span> <span data-ttu-id="719d3-118">Azt írja le, minden egyes attribútum hello cikk későbbi részében részletesebben.</span><span class="sxs-lookup"><span data-stu-id="719d3-118">We describe each attribute in more detail later in hello article.</span></span>

### <a name="request-uri"></a><span data-ttu-id="719d3-119">Kérelem URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="719d3-119">Request URI</span></span>
| <span data-ttu-id="719d3-120">Attribútum</span><span class="sxs-lookup"><span data-stu-id="719d3-120">Attribute</span></span> | <span data-ttu-id="719d3-121">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="719d3-121">Property</span></span> |
|:--- |:--- |
| <span data-ttu-id="719d3-122">Módszer</span><span class="sxs-lookup"><span data-stu-id="719d3-122">Method</span></span> |<span data-ttu-id="719d3-123">POST</span><span class="sxs-lookup"><span data-stu-id="719d3-123">POST</span></span> |
| <span data-ttu-id="719d3-124">URI</span><span class="sxs-lookup"><span data-stu-id="719d3-124">URI</span></span> |<span data-ttu-id="719d3-125">https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01</span><span class="sxs-lookup"><span data-stu-id="719d3-125">https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01</span></span> |
| <span data-ttu-id="719d3-126">Tartalom típusa</span><span class="sxs-lookup"><span data-stu-id="719d3-126">Content type</span></span> |<span data-ttu-id="719d3-127">application/json</span><span class="sxs-lookup"><span data-stu-id="719d3-127">application/json</span></span> |

### <a name="request-uri-parameters"></a><span data-ttu-id="719d3-128">A kérelem URI-paraméterei</span><span class="sxs-lookup"><span data-stu-id="719d3-128">Request URI parameters</span></span>
| <span data-ttu-id="719d3-129">Paraméter</span><span class="sxs-lookup"><span data-stu-id="719d3-129">Parameter</span></span> | <span data-ttu-id="719d3-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="719d3-130">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="719d3-131">CustomerID</span><span class="sxs-lookup"><span data-stu-id="719d3-131">CustomerID</span></span> |<span data-ttu-id="719d3-132">a Microsoft Operations Management Suite-munkaterülettel hello hello egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="719d3-132">hello unique identifier for hello Microsoft Operations Management Suite workspace.</span></span> |
| <span data-ttu-id="719d3-133">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="719d3-133">Resource</span></span> |<span data-ttu-id="719d3-134">hello API erőforrásnév: / api/logs.</span><span class="sxs-lookup"><span data-stu-id="719d3-134">hello API resource name: /api/logs.</span></span> |
| <span data-ttu-id="719d3-135">API-verzió</span><span class="sxs-lookup"><span data-stu-id="719d3-135">API Version</span></span> |<span data-ttu-id="719d3-136">az ehhez a kérelemhez hello API toouse hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="719d3-136">hello version of hello API toouse with this request.</span></span> <span data-ttu-id="719d3-137">Ez jelenleg 2016-04-01.</span><span class="sxs-lookup"><span data-stu-id="719d3-137">Currently, it's 2016-04-01.</span></span> |

### <a name="request-headers"></a><span data-ttu-id="719d3-138">Kérelem fejlécei</span><span class="sxs-lookup"><span data-stu-id="719d3-138">Request headers</span></span>
| <span data-ttu-id="719d3-139">Fejléc</span><span class="sxs-lookup"><span data-stu-id="719d3-139">Header</span></span> | <span data-ttu-id="719d3-140">Leírás</span><span class="sxs-lookup"><span data-stu-id="719d3-140">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="719d3-141">Engedélyezés</span><span class="sxs-lookup"><span data-stu-id="719d3-141">Authorization</span></span> |<span data-ttu-id="719d3-142">hello engedélyezési aláírás.</span><span class="sxs-lookup"><span data-stu-id="719d3-142">hello authorization signature.</span></span> <span data-ttu-id="719d3-143">Hello cikk későbbi részében olvashat hogyan toocreate egy HMAC-SHA256-fejléc.</span><span class="sxs-lookup"><span data-stu-id="719d3-143">Later in hello article, you can read about how toocreate an HMAC-SHA256 header.</span></span> |
| <span data-ttu-id="719d3-144">Napló-típusa</span><span class="sxs-lookup"><span data-stu-id="719d3-144">Log-Type</span></span> |<span data-ttu-id="719d3-145">Adja meg az Ön küldi hello adatok hello rekord típusát.</span><span class="sxs-lookup"><span data-stu-id="719d3-145">Specify hello record type of hello data that is being submitted.</span></span> <span data-ttu-id="719d3-146">Hello típussal jelenleg csak alfanumerikus karaktereket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="719d3-146">Currently, hello log type supports only alpha characters.</span></span> <span data-ttu-id="719d3-147">Nem támogatja írhatók vagy speciális karaktereket.</span><span class="sxs-lookup"><span data-stu-id="719d3-147">It does not support numerics or special characters.</span></span> |
| <span data-ttu-id="719d3-148">x-ms-dátuma</span><span class="sxs-lookup"><span data-stu-id="719d3-148">x-ms-date</span></span> |<span data-ttu-id="719d3-149">hello dátum, hogy hello kérelem feldolgozása, RFC 1123 formátumban.</span><span class="sxs-lookup"><span data-stu-id="719d3-149">hello date that hello request was processed, in RFC 1123 format.</span></span> |
| <span data-ttu-id="719d3-150">idő generált mező</span><span class="sxs-lookup"><span data-stu-id="719d3-150">time-generated-field</span></span> |<span data-ttu-id="719d3-151">hello hello adatok hello időbélyeg hello adatelem tartalmazó mező nevét.</span><span class="sxs-lookup"><span data-stu-id="719d3-151">hello name of a field in hello data that contains hello timestamp of hello data item.</span></span> <span data-ttu-id="719d3-152">Ha a megadott mező, akkor annak tartalmát használt **TimeGenerated**.</span><span class="sxs-lookup"><span data-stu-id="719d3-152">If you specify a field then its contents are used for **TimeGenerated**.</span></span> <span data-ttu-id="719d3-153">Ha ez a mező nincs megadva, az alapértelmezett hello **TimeGenerated** az hello idő adott hello üzenet van okozhatnak.</span><span class="sxs-lookup"><span data-stu-id="719d3-153">If this field isn’t specified, hello default for **TimeGenerated** is hello time that hello message is ingested.</span></span> <span data-ttu-id="719d3-154">hello tartalmát hello üzenet érdemes követnie hello ISO 8601 formátum éééé-hh-SSz.</span><span class="sxs-lookup"><span data-stu-id="719d3-154">hello contents of hello message field should follow hello ISO 8601 format YYYY-MM-DDThh:mm:ssZ.</span></span> |

## <a name="authorization"></a><span data-ttu-id="719d3-155">Engedélyezés</span><span class="sxs-lookup"><span data-stu-id="719d3-155">Authorization</span></span>
<span data-ttu-id="719d3-156">A kérelem toohello napló Analytics HTTP adatokat gyűjtő API tartalmaznia kell egy engedélyezési fejléc.</span><span class="sxs-lookup"><span data-stu-id="719d3-156">Any request toohello Log Analytics HTTP Data Collector API must include an authorization header.</span></span> <span data-ttu-id="719d3-157">a kérelmet, tooauthenticate hello kérelem hello elsődleges vagy másodlagos kulcsát hello hello munkaterület, amely lehetővé teszi a hello kérelem be kell jelentkeznie.</span><span class="sxs-lookup"><span data-stu-id="719d3-157">tooauthenticate a request, you must sign hello request with either hello primary or hello secondary key for hello workspace that is making hello request.</span></span> <span data-ttu-id="719d3-158">Ezt követően adja át az adott aláírás hello kérelem részeként.</span><span class="sxs-lookup"><span data-stu-id="719d3-158">Then, pass that signature as part of hello request.</span></span>   

<span data-ttu-id="719d3-159">Hello hello engedélyezési fejléc formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="719d3-159">Here's hello format for hello authorization header:</span></span>

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

<span data-ttu-id="719d3-160">*WorkspaceID* hello hello Operations Management Suite-munkaterülettel az egyedi azonosító.</span><span class="sxs-lookup"><span data-stu-id="719d3-160">*WorkspaceID* is hello unique identifier for hello Operations Management Suite workspace.</span></span> <span data-ttu-id="719d3-161">*Aláírás* van egy [kivonat-alapú üzenethitelesítési kód (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) , hello kérelemből összeállított és hello segítségével majd számított [SHA-256 algoritmus](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx).</span><span class="sxs-lookup"><span data-stu-id="719d3-161">*Signature* is a [Hash-based Message Authentication Code (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) that is constructed from hello request and then computed by using hello [SHA256 algorithm](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx).</span></span> <span data-ttu-id="719d3-162">Majd hogy kódolása Base64 kódolással.</span><span class="sxs-lookup"><span data-stu-id="719d3-162">Then, you encode it by using Base64 encoding.</span></span>

<span data-ttu-id="719d3-163">A formátum tooencode hello használata **SharedKey** aláírás-karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="719d3-163">Use this format tooencode hello **SharedKey** signature string:</span></span>

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

<span data-ttu-id="719d3-164">Íme egy példa látható egy aláírás karakterláncra:</span><span class="sxs-lookup"><span data-stu-id="719d3-164">Here's an example of a signature string:</span></span>

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

<span data-ttu-id="719d3-165">Ha rendelkezik hello aláírás karakterlánc kódolása használatával hello hello UTF-8 kódolású karakterlánc a HMAC-SHA-256 algoritmust, és majd a hello eredmény Base64 kódolása.</span><span class="sxs-lookup"><span data-stu-id="719d3-165">When you have hello signature string, encode it by using hello HMAC-SHA256 algorithm on hello UTF-8-encoded string, and then encode hello result as Base64.</span></span> <span data-ttu-id="719d3-166">Ezt a formátumot használja:</span><span class="sxs-lookup"><span data-stu-id="719d3-166">Use this format:</span></span>

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

<span data-ttu-id="719d3-167">hello minták hello következő szakaszokban lévő minta kód toohelp létrehozhat egy engedélyezési fejléc rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="719d3-167">hello samples in hello next sections have sample code toohelp you create an authorization header.</span></span>

## <a name="request-body"></a><span data-ttu-id="719d3-168">Kérés törzsében</span><span class="sxs-lookup"><span data-stu-id="719d3-168">Request body</span></span>
<span data-ttu-id="719d3-169">hello üzenet törzsét hello JSON kell lennie.</span><span class="sxs-lookup"><span data-stu-id="719d3-169">hello body of hello message must be in JSON.</span></span> <span data-ttu-id="719d3-170">Egy vagy több rekordot hello tulajdonság név-érték párokat kell tartalmaznia a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="719d3-170">It must include one or more records with hello property name and value pairs in this format:</span></span>

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

<span data-ttu-id="719d3-171">A köteg egyetlen kérelemmel együtt a több rekordot hello a következő formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="719d3-171">You can batch multiple records together in a single request by using hello following format.</span></span> <span data-ttu-id="719d3-172">Minden hello kell hello azonos rekordtípus.</span><span class="sxs-lookup"><span data-stu-id="719d3-172">All hello records must be hello same record type.</span></span>

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

## <a name="record-type-and-properties"></a><span data-ttu-id="719d3-173">Bejegyzéstípus és tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="719d3-173">Record type and properties</span></span>
<span data-ttu-id="719d3-174">Az egyéni típusát határozza meg, amikor hello napló Analytics HTTP adatokat gyűjtő API használatával adatokat küld.</span><span class="sxs-lookup"><span data-stu-id="719d3-174">You define a custom record type when you submit data through hello Log Analytics HTTP Data Collector API.</span></span> <span data-ttu-id="719d3-175">Jelenleg nem írhat adatok tooexisting rekordtípusokhoz létrehozott más adattípusok és megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="719d3-175">Currently, you can't write data tooexisting record types that were created by other data types and solutions.</span></span> <span data-ttu-id="719d3-176">A Naplóelemzési hello bejövő adatokat olvas, és ezután hoz létre, amelyek megfelelnek a megadott értékek hello hello adatok típusú tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="719d3-176">Log Analytics reads hello incoming data and then creates properties that match hello data types of hello values that you enter.</span></span>

<span data-ttu-id="719d3-177">Minden egyes kérelem toohello napló Analytics API tartalmaznia kell egy **hibanapló-típus** hello bejegyzéstípus hello nevű fejléc.</span><span class="sxs-lookup"><span data-stu-id="719d3-177">Each request toohello Log Analytics API must include a **Log-Type** header with hello name for hello record type.</span></span> <span data-ttu-id="719d3-178">hello utótag **_CL** meg más naplóból, meg kell adnia egy egyéni naplót, toodistinguish automatikusan hozzáfűzött toohello neve.</span><span class="sxs-lookup"><span data-stu-id="719d3-178">hello suffix **_CL** is automatically appended toohello name you enter toodistinguish it from other log types as a custom log.</span></span> <span data-ttu-id="719d3-179">Például, ha hello beírása **MyNewRecordType**, Log Analyticshez létrehoz egy rekordot hello típusú **MyNewRecordType_CL**.</span><span class="sxs-lookup"><span data-stu-id="719d3-179">For example, if you enter hello name **MyNewRecordType**, Log Analytics creates a record with hello type **MyNewRecordType_CL**.</span></span> <span data-ttu-id="719d3-180">Ezzel biztosíthatja, hogy nincsenek-e a felhasználó által létrehozott típusnevek és a jelenlegi vagy jövőbeli Microsoft megoldások szállított közötti ütközések.</span><span class="sxs-lookup"><span data-stu-id="719d3-180">This helps ensure that there are no conflicts between user-created type names and those shipped in current or future Microsoft solutions.</span></span>

<span data-ttu-id="719d3-181">a Naplóelemzési veszi fel utótag toohello tulajdonság nevét, tooidentify a tulajdonság adattípusát.</span><span class="sxs-lookup"><span data-stu-id="719d3-181">tooidentify a property's data type, Log Analytics adds a suffix toohello property name.</span></span> <span data-ttu-id="719d3-182">Ha egy tulajdonság null értéket tartalmaz, hello tulajdonság nem szerepel a rekordban levő.</span><span class="sxs-lookup"><span data-stu-id="719d3-182">If a property contains a null value, hello property is not included in that record.</span></span> <span data-ttu-id="719d3-183">Ez a táblázat felsorolja a hello tulajdonságadat-típus és a megfelelő utótag:</span><span class="sxs-lookup"><span data-stu-id="719d3-183">This table lists hello property data type and corresponding suffix:</span></span>

| <span data-ttu-id="719d3-184">Tulajdonságadat-típus</span><span class="sxs-lookup"><span data-stu-id="719d3-184">Property data type</span></span> | <span data-ttu-id="719d3-185">Utótag</span><span class="sxs-lookup"><span data-stu-id="719d3-185">Suffix</span></span> |
|:--- |:--- |
| <span data-ttu-id="719d3-186">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="719d3-186">String</span></span> |<span data-ttu-id="719d3-187">z</span><span class="sxs-lookup"><span data-stu-id="719d3-187">_s</span></span> |
| <span data-ttu-id="719d3-188">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="719d3-188">Boolean</span></span> |<span data-ttu-id="719d3-189">_B</span><span class="sxs-lookup"><span data-stu-id="719d3-189">_b</span></span> |
| <span data-ttu-id="719d3-190">Dupla</span><span class="sxs-lookup"><span data-stu-id="719d3-190">Double</span></span> |<span data-ttu-id="719d3-191">_D</span><span class="sxs-lookup"><span data-stu-id="719d3-191">_d</span></span> |
| <span data-ttu-id="719d3-192">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="719d3-192">Date/time</span></span> |<span data-ttu-id="719d3-193">_Szo</span><span class="sxs-lookup"><span data-stu-id="719d3-193">_t</span></span> |
| <span data-ttu-id="719d3-194">GUID</span><span class="sxs-lookup"><span data-stu-id="719d3-194">GUID</span></span> |<span data-ttu-id="719d3-195">_G</span><span class="sxs-lookup"><span data-stu-id="719d3-195">_g</span></span> |

<span data-ttu-id="719d3-196">hello adattípus, amely Naplóelemzési alkalmaz minden egyes tulajdonság függ hello rekordtípus hello új rekord létezik-e.</span><span class="sxs-lookup"><span data-stu-id="719d3-196">hello data type that Log Analytics uses for each property depends on whether hello record type for hello new record already exists.</span></span>

* <span data-ttu-id="719d3-197">Ha hello rekordtípus nem létezik, a Naplóelemzési létrehoz egy új.</span><span class="sxs-lookup"><span data-stu-id="719d3-197">If hello record type does not exist, Log Analytics creates a new one.</span></span> <span data-ttu-id="719d3-198">Naplóelemzési hello JSON típus megállapítás toodetermine hello adattípus alkalmaz minden tulajdonság hello rekordban.</span><span class="sxs-lookup"><span data-stu-id="719d3-198">Log Analytics uses hello JSON type inference toodetermine hello data type for each property for hello new record.</span></span>
* <span data-ttu-id="719d3-199">Ha hello rekordtípus nem létezik, Naplóelemzési megpróbál toocreate egy új rekordot a létező tulajdonságok alapján.</span><span class="sxs-lookup"><span data-stu-id="719d3-199">If hello record type does exist, Log Analytics attempts toocreate a new record based on existing properties.</span></span> <span data-ttu-id="719d3-200">Ha hello hello új rekordban a tulajdonság adattípusát nem megfelelő típusú meglévő konvertált toohello, és nem Ha hello rekordot tartalmaz olyan tulajdonságon, amely nem létezik, Log Analyticshez hoz létre új tulajdonság, amely rendelkezik a megfelelő utótag hello.</span><span class="sxs-lookup"><span data-stu-id="719d3-200">If hello data type for a property in hello new record doesn’t match and can’t be converted toohello existing type, or if hello record includes a property that doesn’t exist, Log Analytics creates a new property that has hello relevant suffix.</span></span>

<span data-ttu-id="719d3-201">Például a küldésének bejegyzés hozna létre egy olyan rekordot három tulajdonságokkal **number_d**, **boolean_b**, és **string_s**:</span><span class="sxs-lookup"><span data-stu-id="719d3-201">For example, this submission entry would create a record with three properties, **number_d**, **boolean_b**, and **string_s**:</span></span>

![A minta rekord 1](media/log-analytics-data-collector-api/record-01.png)

<span data-ttu-id="719d3-203">Ha ezután a következő bejegyzés formázott karakterláncok értékekkel, hello tulajdonságok nem megváltozna.</span><span class="sxs-lookup"><span data-stu-id="719d3-203">If you then submitted this next entry, with all values formatted as strings, hello properties would not change.</span></span> <span data-ttu-id="719d3-204">Ezek az értékek lehetnek konvertált tooexisting adattípusok:</span><span class="sxs-lookup"><span data-stu-id="719d3-204">These values can be converted tooexisting data types:</span></span>

![2. példa rekord](media/log-analytics-data-collector-api/record-02.png)

<span data-ttu-id="719d3-206">De ha a következő küldésének majd végrehajtott Naplóelemzési létrehoznia hello új tulajdonságok **boolean_d** és **string_d**.</span><span class="sxs-lookup"><span data-stu-id="719d3-206">But, if you then made this next submission, Log Analytics would create hello new properties **boolean_d** and **string_d**.</span></span> <span data-ttu-id="719d3-207">Ezeket az értékeket nem lehet konvertálni:</span><span class="sxs-lookup"><span data-stu-id="719d3-207">These values can't be converted:</span></span>

![A minta rekord 3](media/log-analytics-data-collector-api/record-03.png)

<span data-ttu-id="719d3-209">Hello hello rekordtípus létrehozása előtt a következő bejegyzést, majd benyújtott Naplóelemzési volna rekordot kell létrehozni három tulajdonságokkal **sikeresek**, **boolean_s**, és **string_s**.</span><span class="sxs-lookup"><span data-stu-id="719d3-209">If you then submitted hello following entry, before hello record type was created, Log Analytics would create a record with three properties, **number_s**, **boolean_s**, and **string_s**.</span></span> <span data-ttu-id="719d3-210">Ez a bejegyzés a hello kezdeti értékekre van formázva karakterláncként:</span><span class="sxs-lookup"><span data-stu-id="719d3-210">In this entry, each of hello initial values is formatted as a string:</span></span>

![4. példa rekord](media/log-analytics-data-collector-api/record-04.png)

## <a name="data-limits"></a><span data-ttu-id="719d3-212">Adatok korlátok</span><span class="sxs-lookup"><span data-stu-id="719d3-212">Data limits</span></span>
<span data-ttu-id="719d3-213">Nincsenek közzétett toohello Log Analytics-adatok gyűjtése API hello adatok körül bizonyos korlátozások.</span><span class="sxs-lookup"><span data-stu-id="719d3-213">There are some constraints around hello data posted toohello Log Analytics Data collection API.</span></span>

* <span data-ttu-id="719d3-214">Legfeljebb 30 MB post tooLog Analytics adatait gyűjtője API.</span><span class="sxs-lookup"><span data-stu-id="719d3-214">Maximum of 30 MB per post tooLog Analytics Data Collector API.</span></span> <span data-ttu-id="719d3-215">Ez az egyetlen post méretkorlátot.</span><span class="sxs-lookup"><span data-stu-id="719d3-215">This is a size limit for a single post.</span></span> <span data-ttu-id="719d3-216">Ha nagyobb 30 MB egyetlen post hello adatait, kell felosztása hello adatok mentése toosmaller méretű adattömböket, és küldje el egyszerre.</span><span class="sxs-lookup"><span data-stu-id="719d3-216">If hello data from a single post that exceeds 30 MB, you should split hello data up toosmaller sized chunks and send them concurrently.</span></span>
* <span data-ttu-id="719d3-217">Legfeljebb 32 KB-os korlát mezők értékét.</span><span class="sxs-lookup"><span data-stu-id="719d3-217">Maximum of 32 KB limit for field values.</span></span> <span data-ttu-id="719d3-218">Ha hello mezőérték 32 KB-nál nagyobb, hello adatok csonkolva lesz.</span><span class="sxs-lookup"><span data-stu-id="719d3-218">If hello field value is greater than 32 KB, hello data will be truncated.</span></span>
* <span data-ttu-id="719d3-219">Egy adott típusú mezőket ajánlott maximális száma érték az 50.</span><span class="sxs-lookup"><span data-stu-id="719d3-219">Recommended maximum number of fields for a given type is 50.</span></span> <span data-ttu-id="719d3-220">Ez a használhatóság és keresési élményt szempontból gyakorlati korlátozni.</span><span class="sxs-lookup"><span data-stu-id="719d3-220">This is a practical limit from a usability and search experience perspective.</span></span>  

## <a name="return-codes"></a><span data-ttu-id="719d3-221">A visszatérési kódok</span><span class="sxs-lookup"><span data-stu-id="719d3-221">Return codes</span></span>
<span data-ttu-id="719d3-222">200-as HTTP-állapotkód: hello azt jelenti, hogy adott hello kérelmet kapott a feldolgozáshoz.</span><span class="sxs-lookup"><span data-stu-id="719d3-222">hello HTTP status code 200 means that hello request has been received for processing.</span></span> <span data-ttu-id="719d3-223">Ez azt jelzi, hogy hello művelet sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="719d3-223">This indicates that hello operation completed successfully.</span></span>

<span data-ttu-id="719d3-224">Ez a táblázat hello szolgáltatás előfordulhat, hogy vissza állapotkódokat teljes készletének hello:</span><span class="sxs-lookup"><span data-stu-id="719d3-224">This table lists hello complete set of status codes that hello service might return:</span></span>

| <span data-ttu-id="719d3-225">Kód</span><span class="sxs-lookup"><span data-stu-id="719d3-225">Code</span></span> | <span data-ttu-id="719d3-226">status</span><span class="sxs-lookup"><span data-stu-id="719d3-226">Status</span></span> | <span data-ttu-id="719d3-227">Hibakód:</span><span class="sxs-lookup"><span data-stu-id="719d3-227">Error code</span></span> | <span data-ttu-id="719d3-228">Leírás</span><span class="sxs-lookup"><span data-stu-id="719d3-228">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="719d3-229">200</span><span class="sxs-lookup"><span data-stu-id="719d3-229">200</span></span> |<span data-ttu-id="719d3-230">OKÉ</span><span class="sxs-lookup"><span data-stu-id="719d3-230">OK</span></span> | |<span data-ttu-id="719d3-231">hello kérés sikeresen elfogadva.</span><span class="sxs-lookup"><span data-stu-id="719d3-231">hello request was successfully accepted.</span></span> |
| <span data-ttu-id="719d3-232">400</span><span class="sxs-lookup"><span data-stu-id="719d3-232">400</span></span> |<span data-ttu-id="719d3-233">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="719d3-233">Bad request</span></span> |<span data-ttu-id="719d3-234">InactiveCustomer</span><span class="sxs-lookup"><span data-stu-id="719d3-234">InactiveCustomer</span></span> |<span data-ttu-id="719d3-235">hello munkaterület be lett zárva.</span><span class="sxs-lookup"><span data-stu-id="719d3-235">hello workspace has been closed.</span></span> |
| <span data-ttu-id="719d3-236">400</span><span class="sxs-lookup"><span data-stu-id="719d3-236">400</span></span> |<span data-ttu-id="719d3-237">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="719d3-237">Bad request</span></span> |<span data-ttu-id="719d3-238">InvalidApiVersion</span><span class="sxs-lookup"><span data-stu-id="719d3-238">InvalidApiVersion</span></span> |<span data-ttu-id="719d3-239">a megadott hello API-verzió nem ismerte fel hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="719d3-239">hello API version that you specified was not recognized by hello service.</span></span> |
| <span data-ttu-id="719d3-240">400</span><span class="sxs-lookup"><span data-stu-id="719d3-240">400</span></span> |<span data-ttu-id="719d3-241">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="719d3-241">Bad request</span></span> |<span data-ttu-id="719d3-242">InvalidCustomerId</span><span class="sxs-lookup"><span data-stu-id="719d3-242">InvalidCustomerId</span></span> |<span data-ttu-id="719d3-243">a megadott hello munkaterület azonosítója érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="719d3-243">hello workspace ID specified is invalid.</span></span> |
| <span data-ttu-id="719d3-244">400</span><span class="sxs-lookup"><span data-stu-id="719d3-244">400</span></span> |<span data-ttu-id="719d3-245">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="719d3-245">Bad request</span></span> |<span data-ttu-id="719d3-246">InvalidDataFormat</span><span class="sxs-lookup"><span data-stu-id="719d3-246">InvalidDataFormat</span></span> |<span data-ttu-id="719d3-247">Érvénytelen JSON el lett küldve.</span><span class="sxs-lookup"><span data-stu-id="719d3-247">Invalid JSON was submitted.</span></span> <span data-ttu-id="719d3-248">hello adott válasz törzsének hogyan tooresolve hello hiba további információt tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="719d3-248">hello response body might contain more information about how tooresolve hello error.</span></span> |
| <span data-ttu-id="719d3-249">400</span><span class="sxs-lookup"><span data-stu-id="719d3-249">400</span></span> |<span data-ttu-id="719d3-250">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="719d3-250">Bad request</span></span> |<span data-ttu-id="719d3-251">InvalidLogType</span><span class="sxs-lookup"><span data-stu-id="719d3-251">InvalidLogType</span></span> |<span data-ttu-id="719d3-252">hello típussal megadott tartalmazott különleges karaktereket vagy írhatók.</span><span class="sxs-lookup"><span data-stu-id="719d3-252">hello log type specified contained special characters or numerics.</span></span> |
| <span data-ttu-id="719d3-253">400</span><span class="sxs-lookup"><span data-stu-id="719d3-253">400</span></span> |<span data-ttu-id="719d3-254">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="719d3-254">Bad request</span></span> |<span data-ttu-id="719d3-255">MissingApiVersion</span><span class="sxs-lookup"><span data-stu-id="719d3-255">MissingApiVersion</span></span> |<span data-ttu-id="719d3-256">hello API-verzió nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="719d3-256">hello API version wasn’t specified.</span></span> |
| <span data-ttu-id="719d3-257">400</span><span class="sxs-lookup"><span data-stu-id="719d3-257">400</span></span> |<span data-ttu-id="719d3-258">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="719d3-258">Bad request</span></span> |<span data-ttu-id="719d3-259">MissingContentType</span><span class="sxs-lookup"><span data-stu-id="719d3-259">MissingContentType</span></span> |<span data-ttu-id="719d3-260">hello tartalomtípus nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="719d3-260">hello content type wasn’t specified.</span></span> |
| <span data-ttu-id="719d3-261">400</span><span class="sxs-lookup"><span data-stu-id="719d3-261">400</span></span> |<span data-ttu-id="719d3-262">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="719d3-262">Bad request</span></span> |<span data-ttu-id="719d3-263">MissingLogType</span><span class="sxs-lookup"><span data-stu-id="719d3-263">MissingLogType</span></span> |<span data-ttu-id="719d3-264">hello szükséges érték napló típusa nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="719d3-264">hello required value log type wasn’t specified.</span></span> |
| <span data-ttu-id="719d3-265">400</span><span class="sxs-lookup"><span data-stu-id="719d3-265">400</span></span> |<span data-ttu-id="719d3-266">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="719d3-266">Bad request</span></span> |<span data-ttu-id="719d3-267">UnsupportedContentType</span><span class="sxs-lookup"><span data-stu-id="719d3-267">UnsupportedContentType</span></span> |<span data-ttu-id="719d3-268">hello tartalomtípus túl nincs beállítva**az application/json**.</span><span class="sxs-lookup"><span data-stu-id="719d3-268">hello content type was not set too**application/json**.</span></span> |
| <span data-ttu-id="719d3-269">403</span><span class="sxs-lookup"><span data-stu-id="719d3-269">403</span></span> |<span data-ttu-id="719d3-270">Tiltott</span><span class="sxs-lookup"><span data-stu-id="719d3-270">Forbidden</span></span> |<span data-ttu-id="719d3-271">InvalidAuthorization</span><span class="sxs-lookup"><span data-stu-id="719d3-271">InvalidAuthorization</span></span> |<span data-ttu-id="719d3-272">hello szolgáltatás tooauthenticate hello kérelem sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="719d3-272">hello service failed tooauthenticate hello request.</span></span> <span data-ttu-id="719d3-273">Hello munkaterület azonosítója és a kapcsolat kulcs érvényességének ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="719d3-273">Verify that hello workspace ID and connection key are valid.</span></span> |
| <span data-ttu-id="719d3-274">404</span><span class="sxs-lookup"><span data-stu-id="719d3-274">404</span></span> |<span data-ttu-id="719d3-275">Nem található</span><span class="sxs-lookup"><span data-stu-id="719d3-275">Not Found</span></span> | | <span data-ttu-id="719d3-276">Hello URL-cím helytelen, vagy hello kérelme, mert túl nagy.</span><span class="sxs-lookup"><span data-stu-id="719d3-276">Either hello URL provided is incorrect, or hello request is too large.</span></span> |
| <span data-ttu-id="719d3-277">429</span><span class="sxs-lookup"><span data-stu-id="719d3-277">429</span></span> |<span data-ttu-id="719d3-278">Túl sok kérelem</span><span class="sxs-lookup"><span data-stu-id="719d3-278">Too Many Requests</span></span> | | <span data-ttu-id="719d3-279">hello szolgáltatás tapasztal nagyszámú fiókja adatait.</span><span class="sxs-lookup"><span data-stu-id="719d3-279">hello service is experiencing a high volume of data from your account.</span></span> <span data-ttu-id="719d3-280">Próbálkozzon újra később hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="719d3-280">Please retry hello request later.</span></span> |
| <span data-ttu-id="719d3-281">500</span><span class="sxs-lookup"><span data-stu-id="719d3-281">500</span></span> |<span data-ttu-id="719d3-282">Belső kiszolgálóhiba.</span><span class="sxs-lookup"><span data-stu-id="719d3-282">Internal Server Error</span></span> |<span data-ttu-id="719d3-283">UnspecifiedError</span><span class="sxs-lookup"><span data-stu-id="719d3-283">UnspecifiedError</span></span> |<span data-ttu-id="719d3-284">hello szolgáltatás belső hibát észlelt.</span><span class="sxs-lookup"><span data-stu-id="719d3-284">hello service encountered an internal error.</span></span> <span data-ttu-id="719d3-285">Próbálja meg újra hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="719d3-285">Please retry hello request.</span></span> |
| <span data-ttu-id="719d3-286">503</span><span class="sxs-lookup"><span data-stu-id="719d3-286">503</span></span> |<span data-ttu-id="719d3-287">A szolgáltatás nem érhető el</span><span class="sxs-lookup"><span data-stu-id="719d3-287">Service Unavailable</span></span> |<span data-ttu-id="719d3-288">ServiceUnavailable</span><span class="sxs-lookup"><span data-stu-id="719d3-288">ServiceUnavailable</span></span> |<span data-ttu-id="719d3-289">hello szolgáltatás jelenleg nem érhető el tooreceive kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="719d3-289">hello service currently is unavailable tooreceive requests.</span></span> <span data-ttu-id="719d3-290">Próbálja megismételni a kérést.</span><span class="sxs-lookup"><span data-stu-id="719d3-290">Please retry your request.</span></span> |

## <a name="query-data"></a><span data-ttu-id="719d3-291">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="719d3-291">Query data</span></span>
<span data-ttu-id="719d3-292">hello napló Analytics HTTP adatokat gyűjtő API, keresse meg a rekordokat által küldött tooquery adatokat **típus** , amely egyenlő toohello **LogType** , amely a megadott érték hozzáíródik **_CL**.</span><span class="sxs-lookup"><span data-stu-id="719d3-292">tooquery data submitted by hello Log Analytics HTTP Data Collector API, search for records with **Type** that is equal toohello **LogType** value that you specified, appended with **_CL**.</span></span> <span data-ttu-id="719d3-293">Például, ha a használt **MyCustomLog**, majd az összes rekordot alakítanák vissza **típus = MyCustomLog_CL**.</span><span class="sxs-lookup"><span data-stu-id="719d3-293">For example, if you used **MyCustomLog**, then you'd return all records with **Type=MyCustomLog_CL**.</span></span>

>[!NOTE]
> <span data-ttu-id="719d3-294">Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezés hello megváltozna toohello következő.</span><span class="sxs-lookup"><span data-stu-id="719d3-294">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then hello above query would change toohello following.</span></span>

> `MyCustomLog_CL`

## <a name="sample-requests"></a><span data-ttu-id="719d3-295">A minta-kérelmek</span><span class="sxs-lookup"><span data-stu-id="719d3-295">Sample requests</span></span>
<span data-ttu-id="719d3-296">A következő szakaszokban hello látni fogja, hogy hogyan minták toosubmit adatok toohello napló Analytics HTTP adatokat gyűjtő API különböző programnyelveken használatával.</span><span class="sxs-lookup"><span data-stu-id="719d3-296">In hello next sections, you'll find samples of how toosubmit data toohello Log Analytics HTTP Data Collector API by using different programming languages.</span></span>

<span data-ttu-id="719d3-297">Minden egyes minta tooset hello változók hello engedélyezési fejléc tegye ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="719d3-297">For each sample, do these steps tooset hello variables for hello authorization header:</span></span>

1. <span data-ttu-id="719d3-298">A hello Operations Management Suite portálját, válassza ki a hello **beállítások** csempére, majd válassza ki a hello **csatlakoztatott források** fülre.</span><span class="sxs-lookup"><span data-stu-id="719d3-298">In hello Operations Management Suite portal, select hello **Settings** tile, and then select hello **Connected Sources** tab.</span></span>
2. <span data-ttu-id="719d3-299">jobbra toohello **munkaterület azonosítója**hello másolás ikon, között, majd illessze be a hello azonosító hello hello értékeként **ügyfél azonosítója** változó.</span><span class="sxs-lookup"><span data-stu-id="719d3-299">toohello right of **Workspace ID**, select hello copy icon, and then paste hello ID as hello value of hello **Customer ID** variable.</span></span>
3. <span data-ttu-id="719d3-300">jobbra toohello **elsődleges kulcs**hello másolás ikon, között, majd illessze be a hello azonosító hello hello értékeként **megosztott kulcsos** változó.</span><span class="sxs-lookup"><span data-stu-id="719d3-300">toohello right of **Primary Key**, select hello copy icon, and then paste hello ID as hello value of hello **Shared Key** variable.</span></span>

<span data-ttu-id="719d3-301">Módosíthatja azt is megteheti, hello változók hello napló típusa és a JSON-adatokat.</span><span class="sxs-lookup"><span data-stu-id="719d3-301">Alternatively, you can change hello variables for hello log type and JSON data.</span></span>

### <a name="powershell-sample"></a><span data-ttu-id="719d3-302">PowerShell-példa</span><span class="sxs-lookup"><span data-stu-id="719d3-302">PowerShell sample</span></span>
```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify hello name of hello record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with hello created time for hello records
$TimeStampField = "DateValue"


# Create two records with hello same set of properties toocreate
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

# Create hello function toocreate hello authorization signature
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


# Create hello function toocreate and post hello request
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

# Submit hello data toohello API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a><span data-ttu-id="719d3-303">C#-minta</span><span class="sxs-lookup"><span data-stu-id="719d3-303">C# sample</span></span>
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

        // Update customerId tooyour Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

        // For sharedKey, use either hello primary or hello secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

        // LogName is name of hello event type that is being submitted tooLog Analytics
        static string LogName = "DemoExample";

        // You can use an optional field toospecify hello timestamp from hello data. If hello time field is not specified, Log Analytics assumes hello time is hello message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
            // Create a hash for hello API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

        // Build hello API signature
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

        // Send a request toohello POST API endpoint
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

### <a name="python-sample"></a><span data-ttu-id="719d3-304">Python-minta</span><span class="sxs-lookup"><span data-stu-id="719d3-304">Python sample</span></span>
```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update hello customer ID tooyour Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For hello shared key, use either hello primary or hello secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# hello log type is hello name of hello event that is being submitted
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

# Build hello API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request toohello POST API
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

## <a name="next-steps"></a><span data-ttu-id="719d3-305">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="719d3-305">Next steps</span></span>
- <span data-ttu-id="719d3-306">Használjon hello [napló Search API](log-analytics-log-search-api.md) adattárból hello Naplóelemzési tooretrieve adatokat.</span><span class="sxs-lookup"><span data-stu-id="719d3-306">Use hello [Log Search API](log-analytics-log-search-api.md) tooretrieve data from hello Log Analytics repository.</span></span>
