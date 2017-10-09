---
title: "aaaAzure példány metaadatok szolgáltatás áttekintése |} Microsoft Docs"
description: "RESTful felület tooget információ a virtuális gép számítási, hálózati, és a közelgő karbantartásokról események."
services: virtual-machines-windows, virtual-machines-linux,virtual-machines-scale-sets, cloud-services
documentationcenter: virtual-machines
author: harijay
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 03/27/2017
ms.author: harijay
ms.openlocfilehash: e87cdf28f80b9ef8cc566b637549c48846862f0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-instance-metadata-service"></a><span data-ttu-id="2fbfc-103">Azure-példányokon metaadat-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="2fbfc-103">Azure Instance Metadata Service</span></span> 


<span data-ttu-id="2fbfc-104">hello Azure példány metaadat-szolgáltatás fut a virtuális gépet állíthat be és használt toomanage lehet virtuálisgép-példányok információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-104">hello Azure Instance Metadata Service provides information about running virtual machine instances that can be used toomanage and configure your virtual machines.</span></span>
<span data-ttu-id="2fbfc-105">Ide tartoznak például Termékváltozat, a hálózati konfigurációs és a közelgő karbantartásokról események.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-105">This includes information such as SKU, network configuration, and upcoming maintenance events.</span></span> <span data-ttu-id="2fbfc-106">Milyen típusú információkat érhető el a további információkért lásd: [metaadat-kategóriák](#instance-metadata-data-categories).</span><span class="sxs-lookup"><span data-stu-id="2fbfc-106">For more information on what type of information is available, see [metadata categories](#instance-metadata-data-categories).</span></span>

<span data-ttu-id="2fbfc-107">Azure példány metaadatok szolgáltatás nem érhető el tooall IaaS virtuális gépeket hello segítségével létrehozott REST-végpont [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="2fbfc-107">Azure's Instance Metadata Service is a REST Endpoint accessible tooall IaaS VMs created via hello [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="2fbfc-108">hello végpont egy jól ismert nem irányítható IP-címen érhető el (`169.254.169.254`), amely elérhető csak hello VM belül.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-108">hello endpoint is available at a well-known non-routable IP address (`169.254.169.254`) that can be accessed only from within hello VM.</span></span>

### <a name="important-information"></a><span data-ttu-id="2fbfc-109">Fontos információk</span><span class="sxs-lookup"><span data-stu-id="2fbfc-109">Important information</span></span>

<span data-ttu-id="2fbfc-110">Ez a szolgáltatás **általánosan elérhető** globális Azure-régióban.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-110">This service is  **generally available** in Global Azure Regions.</span></span> <span data-ttu-id="2fbfc-111">Nyilvános előzetes kormányzati, Kína és német Azure felhőben van.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-111">It is in Public preview for Government, China, and German Azure Cloud.</span></span> <span data-ttu-id="2fbfc-112">Rendszeresen kap frissítéseket tooexpose vonatkozó új információt virtuálisgép-példánya.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-112">It regularly receives updates tooexpose new information about virtual machine instances.</span></span> <span data-ttu-id="2fbfc-113">Ezen a lapon tükrözi naprakész hello [az adatkategóriákat](#instance-metadata-data-categories) érhető el.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-113">This page reflects hello up-to-date [data categories](#instance-metadata-data-categories) available.</span></span>

## <a name="service-availability"></a><span data-ttu-id="2fbfc-114">Szolgáltatás rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="2fbfc-114">Service Availability</span></span>
<span data-ttu-id="2fbfc-115">hello szolgáltatás minden általánosan elérhető globális Azure területen érhető el.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-115">hello service is available in all generally available Global Azure regions.</span></span> <span data-ttu-id="2fbfc-116">hello szolgáltatás nyilvános előzetes hello kormányzati, Kína vagy Németország régióban van.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-116">hello service is in public preview  in hello Government, China, or Germany regions.</span></span>

<span data-ttu-id="2fbfc-117">Régiók</span><span class="sxs-lookup"><span data-stu-id="2fbfc-117">Regions</span></span>                                        | <span data-ttu-id="2fbfc-118">Rendelkezésre állási?</span><span class="sxs-lookup"><span data-stu-id="2fbfc-118">Availability?</span></span>
-----------------------------------------------|-----------------------------------------------
[<span data-ttu-id="2fbfc-119">Minden általánosan elérhető globális Azure-régió</span><span class="sxs-lookup"><span data-stu-id="2fbfc-119">All Generally Available Global Azure Regions</span></span>](https://azure.microsoft.com/en-us/regions/)     | <span data-ttu-id="2fbfc-120">Általánosan elérhető</span><span class="sxs-lookup"><span data-stu-id="2fbfc-120">Generally Available</span></span> 
[<span data-ttu-id="2fbfc-121">Azure Government</span><span class="sxs-lookup"><span data-stu-id="2fbfc-121">Azure Government</span></span>](https://azure.microsoft.com/en-us/overview/clouds/government/)              | <span data-ttu-id="2fbfc-122">Az előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="2fbfc-122">In Preview</span></span> 
[<span data-ttu-id="2fbfc-123">Az Azure Kínában</span><span class="sxs-lookup"><span data-stu-id="2fbfc-123">Azure China</span></span>](https://www.azure.cn/)                                                           | <span data-ttu-id="2fbfc-124">Az előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="2fbfc-124">In Preview</span></span>
[<span data-ttu-id="2fbfc-125">Az Azure-Németország</span><span class="sxs-lookup"><span data-stu-id="2fbfc-125">Azure Germany</span></span>](https://azure.microsoft.com/en-us/overview/clouds/germany/)                    | <span data-ttu-id="2fbfc-126">Az előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="2fbfc-126">In Preview</span></span>

<span data-ttu-id="2fbfc-127">Ez a táblázat frissülni fog, amikor más Azure felhőben hello szolgáltatás elérhetővé válik.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-127">This table is updated when hello service becomes available in other Azure clouds.</span></span>

<span data-ttu-id="2fbfc-128">tootry kimenő hello példány metaadat-szolgáltatás, a virtuális gép létrehozása [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) vagy hello [Azure-portálon](http://portal.azure.com) a hello régiók és kövesse hello példák az alábbi felett.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-128">tootry out hello Instance Metadata Service, create a VM from [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) or hello [Azure portal](http://portal.azure.com) in hello above regions and follow hello examples below.</span></span>

## <a name="usage"></a><span data-ttu-id="2fbfc-129">Használat</span><span class="sxs-lookup"><span data-stu-id="2fbfc-129">Usage</span></span>

### <a name="versioning"></a><span data-ttu-id="2fbfc-130">Versioning</span><span class="sxs-lookup"><span data-stu-id="2fbfc-130">Versioning</span></span>
<span data-ttu-id="2fbfc-131">hello példány metaadat-szolgáltatás rendszerverzióval ellátott.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-131">hello Instance Metadata Service is versioned.</span></span> <span data-ttu-id="2fbfc-132">Verziók kötelező, és hello verziószámának `2017-04-02`.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-132">Versions are mandatory and hello current version is `2017-04-02`.</span></span>

> [!NOTE] 
> <span data-ttu-id="2fbfc-133">Előző előzetes kiadásaiban ütemezett események {legújabb} hello api-verziója támogatott.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-133">Previous preview releases of scheduled events supported {latest} as hello api-version.</span></span> <span data-ttu-id="2fbfc-134">Ez a formátum már nem támogatott, és a jövőbeli hello elavulttá válik.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-134">This format is no longer supported and will be deprecated in hello future.</span></span>

<span data-ttu-id="2fbfc-135">Azt hozzáadja az újabb verziók, régebbi verziók továbbra is elérhető kompatibilitás, ha a parancsfájlok tartalmazhat függőségeket, meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-135">As we add newer versions, older versions can still be accessed for compatibility if your scripts have dependencies on specific data formats.</span></span> <span data-ttu-id="2fbfc-136">Vegye figyelembe azonban, hogy hello aktuális előzetes version(2017-03-01) nem érhető el hello szolgáltatás általánosan elérhetővé válik.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-136">However, note that hello current preview version(2017-03-01) may not be available once hello service is generally available.</span></span>

### <a name="using-headers"></a><span data-ttu-id="2fbfc-137">Fejlécek használata</span><span class="sxs-lookup"><span data-stu-id="2fbfc-137">Using Headers</span></span>
<span data-ttu-id="2fbfc-138">Ha hello példány metaadat-szolgáltatás, meg kell adnia hello fejléc `Metadata: true` tooensure hello kérést nem volt szándékos átirányítja.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-138">When you query hello Instance Metadata Service, you must provide hello header `Metadata: true` tooensure hello request was not unintentionally redirected.</span></span>

### <a name="retrieving-metadata"></a><span data-ttu-id="2fbfc-139">Metaadatainak lekérése során</span><span class="sxs-lookup"><span data-stu-id="2fbfc-139">Retrieving metadata</span></span>

<span data-ttu-id="2fbfc-140">Példány metaadatai nem érhető el a futó virtuális gépek létrehozása/segítségével kezelt [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="2fbfc-140">Instance metadata is available for running VMs created/managed using [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="2fbfc-141">Elérhető összes az adatkategóriákat a virtuális gép példány hello kérelem a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="2fbfc-141">Access all data categories for a virtual machine instance using hello following request:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> <span data-ttu-id="2fbfc-142">Minden példány metaadatok lekérdezés-és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-142">All instance metadata queries are case-sensitive.</span></span>

### <a name="data-output"></a><span data-ttu-id="2fbfc-143">Kimeneti adatok</span><span class="sxs-lookup"><span data-stu-id="2fbfc-143">Data output</span></span>
<span data-ttu-id="2fbfc-144">Alapértelmezés szerint hello példány metaadat-szolgáltatás adatokat adja vissza JSON formátumban (`Content-Type: application/json`).</span><span class="sxs-lookup"><span data-stu-id="2fbfc-144">By default, hello Instance Metadata Service returns data in JSON format (`Content-Type: application/json`).</span></span> <span data-ttu-id="2fbfc-145">Azonban más API-k adhatnak vissza adatokat különböző formátumokban kérésre.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-145">However, different APIs can return data in different formats if requested.</span></span>
<span data-ttu-id="2fbfc-146">hello következő tábla más adatok formátumú API-k támogathatja a hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-146">hello following table is a reference of other data formats APIs may support.</span></span>

<span data-ttu-id="2fbfc-147">API</span><span class="sxs-lookup"><span data-stu-id="2fbfc-147">API</span></span> | <span data-ttu-id="2fbfc-148">Alapértelmezett adatformátum</span><span class="sxs-lookup"><span data-stu-id="2fbfc-148">Default Data Format</span></span> | <span data-ttu-id="2fbfc-149">Eltérő formátumban</span><span class="sxs-lookup"><span data-stu-id="2fbfc-149">Other Formats</span></span>
--------|---------------------|--------------
<span data-ttu-id="2fbfc-150">/instance</span><span class="sxs-lookup"><span data-stu-id="2fbfc-150">/instance</span></span> | <span data-ttu-id="2fbfc-151">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="2fbfc-151">json</span></span> | <span data-ttu-id="2fbfc-152">Szöveg</span><span class="sxs-lookup"><span data-stu-id="2fbfc-152">text</span></span>
<span data-ttu-id="2fbfc-153">/scheduledevents</span><span class="sxs-lookup"><span data-stu-id="2fbfc-153">/scheduledevents</span></span> | <span data-ttu-id="2fbfc-154">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="2fbfc-154">json</span></span> | <span data-ttu-id="2fbfc-155">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="2fbfc-155">none</span></span>

<span data-ttu-id="2fbfc-156">egy nem alapértelmezett válaszformátum tooaccess hello kért formátum hello kérelem lekérdezési karakterlánc paraméterként adja meg.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-156">tooaccess a non-default response format, specify hello requested format as a querystring parameter in hello request.</span></span> <span data-ttu-id="2fbfc-157">Példa:</span><span class="sxs-lookup"><span data-stu-id="2fbfc-157">For example:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### <a name="security"></a><span data-ttu-id="2fbfc-158">Biztonság</span><span class="sxs-lookup"><span data-stu-id="2fbfc-158">Security</span></span>
<span data-ttu-id="2fbfc-159">hello példány metaadatok szolgáltatás végpontjának értéke csak nem irányítható IP-címnek a virtuálisgép-példányt futtató hello belülről érhetők el.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-159">hello Instance Metadata Service endpoint is accessible only from within hello running virtual machine instance on a non-routable IP address.</span></span> <span data-ttu-id="2fbfc-160">Emellett a kérelmet egy `X-Forwarded-For` fejléc hello szolgáltatás elutasította.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-160">In addition, any request with a `X-Forwarded-For` header is rejected by hello service.</span></span>
<span data-ttu-id="2fbfc-161">Azt is igénylik kérelmek toocontain egy `Metadata: true` fejléc tooensure, amelyek tényleges kérelem hello közvetlenül volt a tervezett és nem szándékos átirányítási része.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-161">We also require requests toocontain a `Metadata: true` header tooensure that hello actual request was directly intended and not a part of unintentional redirection.</span></span> 

### <a name="error"></a><span data-ttu-id="2fbfc-162">Hiba</span><span class="sxs-lookup"><span data-stu-id="2fbfc-162">Error</span></span>
<span data-ttu-id="2fbfc-163">Ha nem található egy adatelem vagy hibás formátumú kérelem, a példány metaadat-szolgáltatás hello szabványos HTTP-hibák adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-163">If there is a data element not found or a malformed request, hello Instance Metadata Service returns standard HTTP errors.</span></span> <span data-ttu-id="2fbfc-164">Példa:</span><span class="sxs-lookup"><span data-stu-id="2fbfc-164">For example:</span></span>

<span data-ttu-id="2fbfc-165">HTTP-állapotkód</span><span class="sxs-lookup"><span data-stu-id="2fbfc-165">HTTP Status Code</span></span> | <span data-ttu-id="2fbfc-166">Ok</span><span class="sxs-lookup"><span data-stu-id="2fbfc-166">Reason</span></span>
----------------|-------
<span data-ttu-id="2fbfc-167">200 OK</span><span class="sxs-lookup"><span data-stu-id="2fbfc-167">200 OK</span></span> |
<span data-ttu-id="2fbfc-168">400 Hibás kérés</span><span class="sxs-lookup"><span data-stu-id="2fbfc-168">400 Bad Request</span></span> | <span data-ttu-id="2fbfc-169">Hiányzó `Metadata: true` fejléc</span><span class="sxs-lookup"><span data-stu-id="2fbfc-169">Missing `Metadata: true` header</span></span>
<span data-ttu-id="2fbfc-170">404 – Nem található</span><span class="sxs-lookup"><span data-stu-id="2fbfc-170">404 Not Found</span></span> | <span data-ttu-id="2fbfc-171">hello kért elem does't létezik</span><span class="sxs-lookup"><span data-stu-id="2fbfc-171">hello requested element does't exist</span></span> 
<span data-ttu-id="2fbfc-172">405 metódus nem engedélyezett</span><span class="sxs-lookup"><span data-stu-id="2fbfc-172">405 Method Not Allowed</span></span> | <span data-ttu-id="2fbfc-173">Csak `GET` és `POST` kérelmek támogatottak.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-173">Only `GET` and `POST` requests are supported</span></span>
<span data-ttu-id="2fbfc-174">429 túl sok kérelem</span><span class="sxs-lookup"><span data-stu-id="2fbfc-174">429 Too Many Requests</span></span> | <span data-ttu-id="2fbfc-175">hello API jelenleg legfeljebb 5 lekérdezések száma másodpercenként</span><span class="sxs-lookup"><span data-stu-id="2fbfc-175">hello API currently supports a maximum of 5 queries per second</span></span>
<span data-ttu-id="2fbfc-176">500 szolgáltatási hiba</span><span class="sxs-lookup"><span data-stu-id="2fbfc-176">500 Service Error</span></span>     | <span data-ttu-id="2fbfc-177">Némi várakozás után próbálja meg újra</span><span class="sxs-lookup"><span data-stu-id="2fbfc-177">Retry after some time</span></span>

### <a name="examples"></a><span data-ttu-id="2fbfc-178">Példák</span><span class="sxs-lookup"><span data-stu-id="2fbfc-178">Examples</span></span>

> [!NOTE] 
> <span data-ttu-id="2fbfc-179">Minden API-válasz olyan JSON-karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-179">All API responses are JSON strings.</span></span> <span data-ttu-id="2fbfc-180">Minden, a következő példa válaszok pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-180">All following example responses  are pretty-printed for readability.</span></span>

#### <a name="retrieving-network-information"></a><span data-ttu-id="2fbfc-181">Hálózati adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="2fbfc-181">Retrieving network information</span></span>

<span data-ttu-id="2fbfc-182">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="2fbfc-182">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-04-02"
```

<span data-ttu-id="2fbfc-183">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="2fbfc-183">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="2fbfc-184">a rendszer hello választ, egy JSON-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-184">hello response is a JSON string.</span></span> <span data-ttu-id="2fbfc-185">a következő példa egy válasz hello pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-185">hello following example response is pretty-printed for readability.</span></span>

```
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="retrieving-public-ip-address"></a><span data-ttu-id="2fbfc-186">Nyilvános IP-cím beolvasása</span><span class="sxs-lookup"><span data-stu-id="2fbfc-186">Retrieving public IP address</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a><span data-ttu-id="2fbfc-187">Egy példányát minden metaadatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="2fbfc-187">Retrieving all metadata for an instance</span></span>

<span data-ttu-id="2fbfc-188">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="2fbfc-188">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

<span data-ttu-id="2fbfc-189">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="2fbfc-189">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="2fbfc-190">a rendszer hello választ, egy JSON-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-190">hello response is a JSON string.</span></span> <span data-ttu-id="2fbfc-191">a következő példa egy válasz hello pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-191">hello following example response is pretty-printed for readability.</span></span>

```
{
  "compute": {
    "location": "westcentralus",
    "name": "IMDSSample",
    "offer": "UbuntuServer",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "Canonical",
    "sku": "16.04.0-LTS",
    "version": "16.04.201610200",
    "vmId": "5d33a910-a7a0-4443-9f01-6a807801b29b",
    "vmSize": "Standard_A1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.0.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.0.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3AF806EC"
      }
    ]
  }
}
```

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a><span data-ttu-id="2fbfc-192">A Windows rendszerű virtuális gép metaadatainak lekérése során</span><span class="sxs-lookup"><span data-stu-id="2fbfc-192">Retrieving metadata in Windows Virtual Machine</span></span>

<span data-ttu-id="2fbfc-193">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="2fbfc-193">**Request**</span></span>

<span data-ttu-id="2fbfc-194">Példány metaadatok lehet beolvasni a Windows hello PowerShell segédprogram segítségével `curl`:</span><span class="sxs-lookup"><span data-stu-id="2fbfc-194">Instance metadata can be retrieved in Windows via hello PowerShell utility `curl`:</span></span> 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

<span data-ttu-id="2fbfc-195">Vagy hello `Invoke-RestMethod` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="2fbfc-195">Or through hello `Invoke-RestMethod` cmdlet:</span></span>
    
```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

<span data-ttu-id="2fbfc-196">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="2fbfc-196">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="2fbfc-197">a rendszer hello választ, egy JSON-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-197">hello response is a JSON string.</span></span> <span data-ttu-id="2fbfc-198">a következő példa egy válasz hello pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-198">hello following example response  is pretty-printed for readability.</span></span>

```
{
  "compute": {
    "location": "westus",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "MicrosoftSQLServer",
    "sku": "Enterprise",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmSize": "Standard_DS2"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.0.1.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.1.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": [
            
          ]
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="instance-metadata-data-categories"></a><span data-ttu-id="2fbfc-199">Példány metaadat az adatkategóriákat</span><span class="sxs-lookup"><span data-stu-id="2fbfc-199">Instance metadata data categories</span></span>
<span data-ttu-id="2fbfc-200">az adatkategóriákat a következő hello hello példány metaadat-szolgáltatás keresztül érhetők el:</span><span class="sxs-lookup"><span data-stu-id="2fbfc-200">hello following data categories are available through hello Instance Metadata Service:</span></span>

<span data-ttu-id="2fbfc-201">Adatok</span><span class="sxs-lookup"><span data-stu-id="2fbfc-201">Data</span></span> | <span data-ttu-id="2fbfc-202">Leírás</span><span class="sxs-lookup"><span data-stu-id="2fbfc-202">Description</span></span>
-----|------------
<span data-ttu-id="2fbfc-203">location</span><span class="sxs-lookup"><span data-stu-id="2fbfc-203">location</span></span> | <span data-ttu-id="2fbfc-204">Az Azure régióban hello virtuális gép fut.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-204">Azure Region hello VM is running in</span></span>
<span data-ttu-id="2fbfc-205">név</span><span class="sxs-lookup"><span data-stu-id="2fbfc-205">name</span></span> | <span data-ttu-id="2fbfc-206">Hello virtuális gép neve</span><span class="sxs-lookup"><span data-stu-id="2fbfc-206">Name of hello VM</span></span> 
<span data-ttu-id="2fbfc-207">az ajánlat</span><span class="sxs-lookup"><span data-stu-id="2fbfc-207">offer</span></span> | <span data-ttu-id="2fbfc-208">Hello VM-lemezkép információkat nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-208">Offer information for hello VM image.</span></span> <span data-ttu-id="2fbfc-209">Kép: Azure-galériából telepített lemezképek nincs csak ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-209">This value is only present for images deployed from Azure image gallery.</span></span>
<span data-ttu-id="2fbfc-210">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="2fbfc-210">publisher</span></span> | <span data-ttu-id="2fbfc-211">Hello Virtuálisgép-lemezkép kiadója</span><span class="sxs-lookup"><span data-stu-id="2fbfc-211">Publisher of hello VM image</span></span>
<span data-ttu-id="2fbfc-212">Termékváltozat</span><span class="sxs-lookup"><span data-stu-id="2fbfc-212">sku</span></span> | <span data-ttu-id="2fbfc-213">Adott SKU hello VM-lemezkép</span><span class="sxs-lookup"><span data-stu-id="2fbfc-213">Specific SKU for hello VM image</span></span>  
<span data-ttu-id="2fbfc-214">Verzió</span><span class="sxs-lookup"><span data-stu-id="2fbfc-214">version</span></span> | <span data-ttu-id="2fbfc-215">Hello Virtuálisgép-lemezkép verziója</span><span class="sxs-lookup"><span data-stu-id="2fbfc-215">Version of hello VM image</span></span> 
<span data-ttu-id="2fbfc-216">osType</span><span class="sxs-lookup"><span data-stu-id="2fbfc-216">osType</span></span> | <span data-ttu-id="2fbfc-217">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="2fbfc-217">Linux or Windows</span></span> 
<span data-ttu-id="2fbfc-218">platformUpdateDomain</span><span class="sxs-lookup"><span data-stu-id="2fbfc-218">platformUpdateDomain</span></span> |  <span data-ttu-id="2fbfc-219">[Frissítési tartományok](virtual-machines-windows-manage-availability.md) hello virtuális gép fut.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-219">[Update domain](virtual-machines-windows-manage-availability.md) hello VM is running in</span></span>
<span data-ttu-id="2fbfc-220">platformFaultDomain</span><span class="sxs-lookup"><span data-stu-id="2fbfc-220">platformFaultDomain</span></span> | <span data-ttu-id="2fbfc-221">[Tartalék tartomány](virtual-machines-windows-manage-availability.md) hello virtuális gép fut.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-221">[Fault domain](virtual-machines-windows-manage-availability.md) hello VM is running in</span></span>
<span data-ttu-id="2fbfc-222">vmId</span><span class="sxs-lookup"><span data-stu-id="2fbfc-222">vmId</span></span> | <span data-ttu-id="2fbfc-223">[Egyedi azonosító](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) hello virtuális gép számára</span><span class="sxs-lookup"><span data-stu-id="2fbfc-223">[Unique identifier](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) for hello VM</span></span>
<span data-ttu-id="2fbfc-224">vmSize</span><span class="sxs-lookup"><span data-stu-id="2fbfc-224">vmSize</span></span> | [<span data-ttu-id="2fbfc-225">Virtuálisgép-mérettel</span><span class="sxs-lookup"><span data-stu-id="2fbfc-225">VM size</span></span>](virtual-machines-windows-sizes.md)
<span data-ttu-id="2fbfc-226">IPv4/privateipaddress tulajdonságot</span><span class="sxs-lookup"><span data-stu-id="2fbfc-226">ipv4/privateIpAddress</span></span> | <span data-ttu-id="2fbfc-227">Virtuális gép hello helyi IPv4-címe</span><span class="sxs-lookup"><span data-stu-id="2fbfc-227">Local IPv4 address of hello VM</span></span> 
<span data-ttu-id="2fbfc-228">IPv4-vagy nyilvános</span><span class="sxs-lookup"><span data-stu-id="2fbfc-228">ipv4/publicIpAddress</span></span> | <span data-ttu-id="2fbfc-229">Virtuális gép hello nyilvános IPv4-címe</span><span class="sxs-lookup"><span data-stu-id="2fbfc-229">Public IPv4 address of hello VM</span></span>
<span data-ttu-id="2fbfc-230">alhálózat és címét</span><span class="sxs-lookup"><span data-stu-id="2fbfc-230">subnet/address</span></span> | <span data-ttu-id="2fbfc-231">Virtuális gép hello alhálózati címe</span><span class="sxs-lookup"><span data-stu-id="2fbfc-231">Subnet address of hello VM</span></span>
<span data-ttu-id="2fbfc-232">/ előtagot.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-232">subnet/prefix</span></span> | <span data-ttu-id="2fbfc-233">Példa 24 alhálózati előtag</span><span class="sxs-lookup"><span data-stu-id="2fbfc-233">Subnet prefix, example 24</span></span>
<span data-ttu-id="2fbfc-234">IPv6/IP-cím</span><span class="sxs-lookup"><span data-stu-id="2fbfc-234">ipv6/ipAddress</span></span> | <span data-ttu-id="2fbfc-235">Virtuális gép hello helyi IPv6-cím</span><span class="sxs-lookup"><span data-stu-id="2fbfc-235">Local IPv6 address of hello VM</span></span>
<span data-ttu-id="2fbfc-236">MacAddress</span><span class="sxs-lookup"><span data-stu-id="2fbfc-236">macAddress</span></span> | <span data-ttu-id="2fbfc-237">Virtuális gép mac-cím</span><span class="sxs-lookup"><span data-stu-id="2fbfc-237">VM mac address</span></span> 
<span data-ttu-id="2fbfc-238">scheduledevents</span><span class="sxs-lookup"><span data-stu-id="2fbfc-238">scheduledevents</span></span> | <span data-ttu-id="2fbfc-239">Jelenleg a nyilvános előzetes lásd: [scheduledevents](virtual-machines-scheduled-events.md)</span><span class="sxs-lookup"><span data-stu-id="2fbfc-239">Currently in Public Preview See [scheduledevents](virtual-machines-scheduled-events.md)</span></span>

## <a name="example-scenarios-for-usage"></a><span data-ttu-id="2fbfc-240">Példa használati forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="2fbfc-240">Example Scenarios for usage</span></span>  

### <a name="tracking-vm-running-on-azure"></a><span data-ttu-id="2fbfc-241">Azure-on futó virtuális gép nyomon követése</span><span class="sxs-lookup"><span data-stu-id="2fbfc-241">Tracking VM running on Azure</span></span>

<span data-ttu-id="2fbfc-242">Szolgáltató előfordulhat, hogy a szoftver futó virtuális gépek száma tootrack hello, vagy hello VM tootrack egyediségének igénylő ügynökök.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-242">As a service provider, you may require tootrack hello number of VMs running your software or have agents that need tootrack uniqueness of hello VM.</span></span> <span data-ttu-id="2fbfc-243">toobe képes tooget egyedi Azonosítót a virtuális gépek használata hello `vmId` példány metaadat-szolgáltatásának mezőjét.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-243">toobe able tooget a unique ID for a VM, use hello `vmId` field from Instance Metadata Service.</span></span>

<span data-ttu-id="2fbfc-244">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="2fbfc-244">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

<span data-ttu-id="2fbfc-245">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="2fbfc-245">**Response**</span></span>

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a><span data-ttu-id="2fbfc-246">Tárolók elhelyezését, adat-partíciók alapú tartalék/frissítési tartomány</span><span class="sxs-lookup"><span data-stu-id="2fbfc-246">Placement of containers, data-partitions based Fault/Update domain</span></span> 

<span data-ttu-id="2fbfc-247">Az egyes forgatókönyvek, más adatok replikák elhelyezését van elsődleges fontosságú.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-247">For certain scenarios, placement of different data replicas is of prime importance.</span></span> <span data-ttu-id="2fbfc-248">Például [HDFS replika elhelyezésének](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) vagy tároló elhelyezési keresztül egy [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) igényelheti tooknow hello `platformFaultDomain` és `platformUpdateDomain` hello futó virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-248">For example, [HDFS replica placement](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) or container placement via an [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) may you require tooknow hello `platformFaultDomain` and `platformUpdateDomain` hello VM is running on.</span></span>
<span data-ttu-id="2fbfc-249">Ezek az adatok közvetlenül hello példány metaadat-szolgáltatás segítségével lekérdezheti.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-249">You can query this data directly via hello Instance Metadata Service.</span></span>

<span data-ttu-id="2fbfc-250">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="2fbfc-250">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

<span data-ttu-id="2fbfc-251">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="2fbfc-251">**Response**</span></span>

```
0
```

### <a name="getting-more-information-about-hello-vm-during-support-case"></a><span data-ttu-id="2fbfc-252">További információ a virtuális gép hello során támogatási esetet beolvasása</span><span class="sxs-lookup"><span data-stu-id="2fbfc-252">Getting more information about hello VM during support case</span></span> 

<span data-ttu-id="2fbfc-253">Szolgáltató előfordulhat, hogy kap egy támogatási hívás tooknow helyének hello VM további információt.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-253">As a service provider, you may get a support call where you would like tooknow more information about hello VM.</span></span> <span data-ttu-id="2fbfc-254">Hello ügyfél tooshare kérése a hello számítási metaadatok alapvető információkat a támogatási szakemberek tooknow hello hello típusú virtuális gép Azure biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-254">Asking hello customer tooshare hello compute metadata can provide basic information for hello support professional tooknow about hello kind of VM on Azure.</span></span> 

<span data-ttu-id="2fbfc-255">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="2fbfc-255">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

<span data-ttu-id="2fbfc-256">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="2fbfc-256">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="2fbfc-257">a rendszer hello választ, egy JSON-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-257">hello response is a JSON string.</span></span> <span data-ttu-id="2fbfc-258">a következő példa egy válasz hello pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-258">hello following example response is pretty-printed for readability.</span></span>

```
{
  "compute": {
    "location": "CentralUS",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  }
}
```

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-hello-vm"></a><span data-ttu-id="2fbfc-259">Példák hello VM belül különböző nyelveken használó metaadatok szolgáltatás hívása</span><span class="sxs-lookup"><span data-stu-id="2fbfc-259">Examples of calling metadata service using different languages inside hello VM</span></span> 

<span data-ttu-id="2fbfc-260">Nyelv</span><span class="sxs-lookup"><span data-stu-id="2fbfc-260">Language</span></span> | <span data-ttu-id="2fbfc-261">Példa</span><span class="sxs-lookup"><span data-stu-id="2fbfc-261">Example</span></span> 
---------|----------------
<span data-ttu-id="2fbfc-262">Ruby</span><span class="sxs-lookup"><span data-stu-id="2fbfc-262">Ruby</span></span>     | <span data-ttu-id="2fbfc-263">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.RB</span><span class="sxs-lookup"><span data-stu-id="2fbfc-263">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb</span></span>
<span data-ttu-id="2fbfc-264">Nyissa meg a helyi hálózat</span><span class="sxs-lookup"><span data-stu-id="2fbfc-264">Go Lan</span></span>   | <span data-ttu-id="2fbfc-265">https://github.com/Microsoft/azureimds/BLOB/Master/imdssample.go</span><span class="sxs-lookup"><span data-stu-id="2fbfc-265">https://github.com/Microsoft/azureimds/blob/master/imdssample.go</span></span>            
<span data-ttu-id="2fbfc-266">python</span><span class="sxs-lookup"><span data-stu-id="2fbfc-266">python</span></span>   | <span data-ttu-id="2fbfc-267">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.PY</span><span class="sxs-lookup"><span data-stu-id="2fbfc-267">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py</span></span>
<span data-ttu-id="2fbfc-268">C++</span><span class="sxs-lookup"><span data-stu-id="2fbfc-268">C++</span></span>      | <span data-ttu-id="2fbfc-269">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample-Windows.cpp</span><span class="sxs-lookup"><span data-stu-id="2fbfc-269">https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp</span></span>
<span data-ttu-id="2fbfc-270">C#</span><span class="sxs-lookup"><span data-stu-id="2fbfc-270">C#</span></span>       | <span data-ttu-id="2fbfc-271">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.cs</span><span class="sxs-lookup"><span data-stu-id="2fbfc-271">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs</span></span>
<span data-ttu-id="2fbfc-272">Javascript</span><span class="sxs-lookup"><span data-stu-id="2fbfc-272">Javascript</span></span> | <span data-ttu-id="2fbfc-273">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.js</span><span class="sxs-lookup"><span data-stu-id="2fbfc-273">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js</span></span>
<span data-ttu-id="2fbfc-274">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2fbfc-274">Powershell</span></span> | <span data-ttu-id="2fbfc-275">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.ps1</span><span class="sxs-lookup"><span data-stu-id="2fbfc-275">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1</span></span>
<span data-ttu-id="2fbfc-276">Bash</span><span class="sxs-lookup"><span data-stu-id="2fbfc-276">Bash</span></span>       | <span data-ttu-id="2fbfc-277">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.SH</span><span class="sxs-lookup"><span data-stu-id="2fbfc-277">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh</span></span>
    

## <a name="faq"></a><span data-ttu-id="2fbfc-278">GYIK</span><span class="sxs-lookup"><span data-stu-id="2fbfc-278">FAQ</span></span>
1. <span data-ttu-id="2fbfc-279">Hello hiba azért kapom `400 Bad Request, Required metadata header not specified`.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-279">I am getting hello error `400 Bad Request, Required metadata header not specified`.</span></span> <span data-ttu-id="2fbfc-280">Ez mit jelent?</span><span class="sxs-lookup"><span data-stu-id="2fbfc-280">What does this mean?</span></span>
   * <span data-ttu-id="2fbfc-281">hello példány metaadat-szolgáltatás hello fejlécet igényel `Metadata: true` toobe átadott hello kérelemben.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-281">hello Instance Metadata Service requires hello header `Metadata: true` toobe passed in hello request.</span></span> <span data-ttu-id="2fbfc-282">Ezt a fejlécet benyújtása hello REST-hívást lehetővé teszi, hogy a hozzáférés toohello példány metaadat-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-282">Passing this header in hello REST call allows access toohello Instance Metadata Service.</span></span> 
2. <span data-ttu-id="2fbfc-283">Miért nem jelenik meg a virtuális gép számítási adatai?</span><span class="sxs-lookup"><span data-stu-id="2fbfc-283">Why am I not getting compute information for my VM?</span></span>
   * <span data-ttu-id="2fbfc-284">Hello példány metaadat-szolgáltatás jelenleg csak az Azure Resource Manager eszközzel létrehozott példányokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-284">Currently hello Instance Metadata Service only supports instances created with Azure Resource Manager.</span></span> <span data-ttu-id="2fbfc-285">A jövőbeli hello azt is támogatásáról Cloud Service virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-285">In hello future, we may add support for Cloud Service VMs.</span></span>
3. <span data-ttu-id="2fbfc-286">A virtuális gép Azure Resource Manager használatával létrehozott egy while vissza.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-286">I created my Virtual Machine through Azure Resource Manager a while back.</span></span> <span data-ttu-id="2fbfc-287">Miért nem tudok lásd számítási metaadat-információ?</span><span class="sxs-lookup"><span data-stu-id="2fbfc-287">Why am I not see compute metadata information?</span></span>
   * <span data-ttu-id="2fbfc-288">A 2016. szeptember után létrehozott virtuális gépek hozzáadása egy [címke](../azure-resource-manager/resource-group-using-tags.md) toostart megnéz számítási metaadatok.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-288">For any VMs created after Sep 2016, add a [Tag](../azure-resource-manager/resource-group-using-tags.md) toostart seeing compute metadata.</span></span> <span data-ttu-id="2fbfc-289">(Szeptember 2016 előtt létrehozott) régebbi virtuális gépek hozzáadása extensions vagy adatok lemezek toohello VM toorefresh metaadatok.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-289">For older VMs (created before Sep 2016), add/remove extensions or data disks toohello VM toorefresh metadata.</span></span>
4. <span data-ttu-id="2fbfc-290">Miért jelenik meg hello hiba `500 Internal Server Error`?</span><span class="sxs-lookup"><span data-stu-id="2fbfc-290">Why am I getting hello error `500 Internal Server Error`?</span></span>
   * <span data-ttu-id="2fbfc-291">Próbálja megismételni a kérést a rendszer ki exponenciális vissza alapján.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-291">Please retry your request based on exponential back off system.</span></span> <span data-ttu-id="2fbfc-292">Ha nem szűnik hello Azure ügyfélszolgálatához.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-292">If hello issue persists contact  Azure support.</span></span>
5. <span data-ttu-id="2fbfc-293">Ha ugyanazt a további kérdéseit vagy megjegyzéseit?</span><span class="sxs-lookup"><span data-stu-id="2fbfc-293">Where do I share additional questions/comments?</span></span>
   * <span data-ttu-id="2fbfc-294">A Megjegyzések küldése a http://feedback.azure.com.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-294">Send your comments on http://feedback.azure.com.</span></span>
7. <span data-ttu-id="2fbfc-295">Ez használható lenne a virtuálisgép-méretezési beállítása példányt?</span><span class="sxs-lookup"><span data-stu-id="2fbfc-295">Would this work for Virtual Machine Scale Set Instance?</span></span>
   * <span data-ttu-id="2fbfc-296">Igen metaadat-szolgáltatás esetén méretezési beállítása példányok érhető el.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-296">Yes Metadata service is available for Scale Set Instances.</span></span> 
6. <span data-ttu-id="2fbfc-297">Hogyan kérhet támogatást hello szolgáltatás?</span><span class="sxs-lookup"><span data-stu-id="2fbfc-297">How do I get support for hello service?</span></span>
   * <span data-ttu-id="2fbfc-298">tooget támogatása hello szolgáltatást, hozzon létre egy támogatási probléma Azure-portál a virtuális gép, amelyen még nem tud tooget metaadatok válasz hosszú újrapróbálkozás után hello</span><span class="sxs-lookup"><span data-stu-id="2fbfc-298">tooget support for hello service, create a support issue in Azure portal for hello VM where you are not able tooget metadata response after long retries</span></span> 

   ![Példány metaadatok támogatása](./media/virtual-machines-instancemetadataservice-overview/InstanceMetadata-support.png)
    
## <a name="next-steps"></a><span data-ttu-id="2fbfc-300">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2fbfc-300">Next Steps</span></span>

- <span data-ttu-id="2fbfc-301">További tudnivalók hello [scheduledevents](virtual-machines-scheduled-events.md) API **a nyilvános előzetes** hello példány metaadat-szolgáltatás által biztosított.</span><span class="sxs-lookup"><span data-stu-id="2fbfc-301">Learn more about hello [scheduledevents](virtual-machines-scheduled-events.md) API **In Public Preview** provided by hello Instance Metadata Service.</span></span>
