---
title: "Példány metaadat-szolgáltatás Windows aaaAzure |} Microsoft Docs"
description: "RESTful felület tooget információ Windows virtuális gép számítási, hálózati, és a közelgő karbantartásokról események."
services: virtual-machines-windows
documentationcenter: 
author: harijay
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: harijay
ms.openlocfilehash: a33c26b5e9ed650be639598cdb6895fc19ccb605
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-instance-metadata-service-for-windows-vms"></a><span data-ttu-id="7c358-103">Windows virtuális gépek Azure példány metaadat-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7c358-103">Azure Instance Metadata service for Windows VMs</span></span>


<span data-ttu-id="7c358-104">hello Azure példány metaadat-szolgáltatás fut a virtuális gépet állíthat be és használt toomanage lehet virtuálisgép-példányok információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="7c358-104">hello Azure Instance Metadata Service provides information about running virtual machine instances that can be used toomanage and configure your virtual machines.</span></span>
<span data-ttu-id="7c358-105">Ide tartoznak például Termékváltozat, a hálózati konfigurációs és a közelgő karbantartásokról események.</span><span class="sxs-lookup"><span data-stu-id="7c358-105">This includes information such as SKU, network configuration, and upcoming maintenance events.</span></span> <span data-ttu-id="7c358-106">Milyen típusú információkat érhető el a további információkért lásd: [metaadat-kategóriák](#instance-metadata-data-categories).</span><span class="sxs-lookup"><span data-stu-id="7c358-106">For more information on what type of information is available, see [metadata categories](#instance-metadata-data-categories).</span></span>

<span data-ttu-id="7c358-107">Azure példány metaadatok szolgáltatás nem érhető el tooall IaaS virtuális gépeket hello segítségével létrehozott REST-végpont [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="7c358-107">Azure's Instance Metadata Service is a REST Endpoint accessible tooall IaaS VMs created via hello [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="7c358-108">hello végpont egy jól ismert nem irányítható IP-címen érhető el (`169.254.169.254`), amely elérhető csak hello VM belül.</span><span class="sxs-lookup"><span data-stu-id="7c358-108">hello endpoint is available at a well-known non-routable IP address (`169.254.169.254`) that can be accessed only from within hello VM.</span></span>



> [!IMPORTANT]
> <span data-ttu-id="7c358-109">Ez a szolgáltatás **általánosan elérhető** globális Azure-régióban.</span><span class="sxs-lookup"><span data-stu-id="7c358-109">This service is  **generally available** in Global Azure Regions.</span></span> <span data-ttu-id="7c358-110">Nyilvános előzetes kormányzati, Kína és német Azure felhőben van.</span><span class="sxs-lookup"><span data-stu-id="7c358-110">It is in Public preview for Government, China, and German Azure Cloud.</span></span> <span data-ttu-id="7c358-111">Rendszeresen kap frissítéseket tooexpose vonatkozó új információt virtuálisgép-példánya.</span><span class="sxs-lookup"><span data-stu-id="7c358-111">It regularly receives updates tooexpose new information about virtual machine instances.</span></span> <span data-ttu-id="7c358-112">Ezen a lapon tükrözi naprakész hello [az adatkategóriákat](#instance-metadata-data-categories) érhető el.</span><span class="sxs-lookup"><span data-stu-id="7c358-112">This page reflects hello up-to-date [data categories](#instance-metadata-data-categories) available.</span></span>



## <a name="service-availability"></a><span data-ttu-id="7c358-113">Szolgáltatás rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="7c358-113">Service Availability</span></span>
<span data-ttu-id="7c358-114">hello szolgáltatás minden általánosan elérhető globális Azure területen érhető el.</span><span class="sxs-lookup"><span data-stu-id="7c358-114">hello service is available in all generally available Global Azure regions.</span></span> <span data-ttu-id="7c358-115">hello szolgáltatás nyilvános előzetes hello kormányzati, Kína vagy Németország régióban van.</span><span class="sxs-lookup"><span data-stu-id="7c358-115">hello service is in public preview  in hello Government, China, or Germany regions.</span></span>

<span data-ttu-id="7c358-116">Régiók</span><span class="sxs-lookup"><span data-stu-id="7c358-116">Regions</span></span>                                        | <span data-ttu-id="7c358-117">Rendelkezésre állási?</span><span class="sxs-lookup"><span data-stu-id="7c358-117">Availability?</span></span>
-----------------------------------------------|-----------------------------------------------
[<span data-ttu-id="7c358-118">Minden általánosan elérhető globális Azure-régió</span><span class="sxs-lookup"><span data-stu-id="7c358-118">All Generally Available Global Azure Regions</span></span>](https://azure.microsoft.com/regions/)     | <span data-ttu-id="7c358-119">Általánosan elérhető</span><span class="sxs-lookup"><span data-stu-id="7c358-119">Generally Available</span></span> 
[<span data-ttu-id="7c358-120">Azure Government</span><span class="sxs-lookup"><span data-stu-id="7c358-120">Azure Government</span></span>](https://azure.microsoft.com/overview/clouds/government/)              | <span data-ttu-id="7c358-121">Az előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="7c358-121">In Preview</span></span> 
[<span data-ttu-id="7c358-122">Az Azure Kínában</span><span class="sxs-lookup"><span data-stu-id="7c358-122">Azure China</span></span>](https://www.azure.cn/)                                                           | <span data-ttu-id="7c358-123">Az előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="7c358-123">In Preview</span></span>
[<span data-ttu-id="7c358-124">Az Azure-Németország</span><span class="sxs-lookup"><span data-stu-id="7c358-124">Azure Germany</span></span>](https://azure.microsoft.com/overview/clouds/germany/)                    | <span data-ttu-id="7c358-125">Az előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="7c358-125">In Preview</span></span>

<span data-ttu-id="7c358-126">Ez a táblázat frissülni fog, amikor más Azure felhőben hello szolgáltatás elérhetővé válik.</span><span class="sxs-lookup"><span data-stu-id="7c358-126">This table is updated when hello service becomes available in other Azure clouds.</span></span>

<span data-ttu-id="7c358-127">tootry kimenő hello példány metaadat-szolgáltatás, a virtuális gép létrehozása [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) vagy hello [Azure-portálon](http://portal.azure.com) a hello régiók és kövesse hello példák az alábbi felett.</span><span class="sxs-lookup"><span data-stu-id="7c358-127">tootry out hello Instance Metadata Service, create a VM from [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) or hello [Azure portal](http://portal.azure.com) in hello above regions and follow hello examples below.</span></span>

## <a name="usage"></a><span data-ttu-id="7c358-128">Használat</span><span class="sxs-lookup"><span data-stu-id="7c358-128">Usage</span></span>

### <a name="versioning"></a><span data-ttu-id="7c358-129">Versioning</span><span class="sxs-lookup"><span data-stu-id="7c358-129">Versioning</span></span>
<span data-ttu-id="7c358-130">hello példány metaadat-szolgáltatás rendszerverzióval ellátott.</span><span class="sxs-lookup"><span data-stu-id="7c358-130">hello Instance Metadata Service is versioned.</span></span> <span data-ttu-id="7c358-131">Verziók kötelező, és hello verziószámának `2017-04-02`.</span><span class="sxs-lookup"><span data-stu-id="7c358-131">Versions are mandatory and hello current version is `2017-04-02`.</span></span>

> [!NOTE] 
> <span data-ttu-id="7c358-132">Előző előzetes kiadásaiban ütemezett események {legújabb} hello api-verziója támogatott.</span><span class="sxs-lookup"><span data-stu-id="7c358-132">Previous preview releases of scheduled events supported {latest} as hello api-version.</span></span> <span data-ttu-id="7c358-133">Ez a formátum már nem támogatott, és a jövőbeli hello elavulttá válik.</span><span class="sxs-lookup"><span data-stu-id="7c358-133">This format is no longer supported and will be deprecated in hello future.</span></span>

<span data-ttu-id="7c358-134">Azt hozzáadja az újabb verziók, régebbi verziók továbbra is elérhető kompatibilitás, ha a parancsfájlok tartalmazhat függőségeket, meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="7c358-134">As we add newer versions, older versions can still be accessed for compatibility if your scripts have dependencies on specific data formats.</span></span> <span data-ttu-id="7c358-135">Vegye figyelembe azonban, hogy hello aktuális előzetes version(2017-03-01) nem érhető el hello szolgáltatás általánosan elérhetővé válik.</span><span class="sxs-lookup"><span data-stu-id="7c358-135">However, note that hello current preview version(2017-03-01) may not be available once hello service is generally available.</span></span>

### <a name="using-headers"></a><span data-ttu-id="7c358-136">Fejlécek használata</span><span class="sxs-lookup"><span data-stu-id="7c358-136">Using headers</span></span>
<span data-ttu-id="7c358-137">Ha hello példány metaadat-szolgáltatás, meg kell adnia hello fejléc `Metadata: true` tooensure hello kérést nem volt szándékos átirányítja.</span><span class="sxs-lookup"><span data-stu-id="7c358-137">When you query hello Instance Metadata Service, you must provide hello header `Metadata: true` tooensure hello request was not unintentionally redirected.</span></span>

### <a name="retrieving-metadata"></a><span data-ttu-id="7c358-138">Metaadatainak lekérése során</span><span class="sxs-lookup"><span data-stu-id="7c358-138">Retrieving metadata</span></span>

<span data-ttu-id="7c358-139">Példány metaadatai nem érhető el a futó virtuális gépek létrehozása/segítségével kezelt [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="7c358-139">Instance metadata is available for running VMs created/managed using [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="7c358-140">Elérhető összes az adatkategóriákat a virtuális gép példány hello kérelem a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="7c358-140">Access all data categories for a virtual machine instance using hello following request:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> <span data-ttu-id="7c358-141">Minden példány metaadatok lekérdezés-és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="7c358-141">All instance metadata queries are case-sensitive.</span></span>

### <a name="data-output"></a><span data-ttu-id="7c358-142">Kimeneti adatok</span><span class="sxs-lookup"><span data-stu-id="7c358-142">Data output</span></span>
<span data-ttu-id="7c358-143">Alapértelmezés szerint hello példány metaadat-szolgáltatás adatokat adja vissza JSON formátumban (`Content-Type: application/json`).</span><span class="sxs-lookup"><span data-stu-id="7c358-143">By default, hello Instance Metadata Service returns data in JSON format (`Content-Type: application/json`).</span></span> <span data-ttu-id="7c358-144">Azonban más API-k adhatnak vissza adatokat különböző formátumokban kérésre.</span><span class="sxs-lookup"><span data-stu-id="7c358-144">However, different APIs can return data in different formats if requested.</span></span>
<span data-ttu-id="7c358-145">hello következő tábla más adatok formátumú API-k támogathatja a hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="7c358-145">hello following table is a reference of other data formats APIs may support.</span></span>

<span data-ttu-id="7c358-146">API</span><span class="sxs-lookup"><span data-stu-id="7c358-146">API</span></span> | <span data-ttu-id="7c358-147">Alapértelmezett adatformátum</span><span class="sxs-lookup"><span data-stu-id="7c358-147">Default Data Format</span></span> | <span data-ttu-id="7c358-148">Eltérő formátumban</span><span class="sxs-lookup"><span data-stu-id="7c358-148">Other Formats</span></span>
--------|---------------------|--------------
<span data-ttu-id="7c358-149">/instance</span><span class="sxs-lookup"><span data-stu-id="7c358-149">/instance</span></span> | <span data-ttu-id="7c358-150">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="7c358-150">json</span></span> | <span data-ttu-id="7c358-151">Szöveg</span><span class="sxs-lookup"><span data-stu-id="7c358-151">text</span></span>
<span data-ttu-id="7c358-152">/scheduledevents</span><span class="sxs-lookup"><span data-stu-id="7c358-152">/scheduledevents</span></span> | <span data-ttu-id="7c358-153">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="7c358-153">json</span></span> | <span data-ttu-id="7c358-154">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="7c358-154">none</span></span>

<span data-ttu-id="7c358-155">egy nem alapértelmezett válaszformátum tooaccess hello kért formátum hello kérelem lekérdezési karakterlánc paraméterként adja meg.</span><span class="sxs-lookup"><span data-stu-id="7c358-155">tooaccess a non-default response format, specify hello requested format as a querystring parameter in hello request.</span></span> <span data-ttu-id="7c358-156">Példa:</span><span class="sxs-lookup"><span data-stu-id="7c358-156">For example:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### <a name="security"></a><span data-ttu-id="7c358-157">Biztonság</span><span class="sxs-lookup"><span data-stu-id="7c358-157">Security</span></span>
<span data-ttu-id="7c358-158">hello példány metaadatok szolgáltatás végpontjának értéke csak nem irányítható IP-címnek a virtuálisgép-példányt futtató hello belülről érhetők el.</span><span class="sxs-lookup"><span data-stu-id="7c358-158">hello Instance Metadata Service endpoint is accessible only from within hello running virtual machine instance on a non-routable IP address.</span></span> <span data-ttu-id="7c358-159">Emellett a kérelmet egy `X-Forwarded-For` fejléc hello szolgáltatás elutasította.</span><span class="sxs-lookup"><span data-stu-id="7c358-159">In addition, any request with a `X-Forwarded-For` header is rejected by hello service.</span></span>
<span data-ttu-id="7c358-160">Azt is igénylik kérelmek toocontain egy `Metadata: true` fejléc tooensure, amelyek tényleges kérelem hello közvetlenül volt a tervezett és nem szándékos átirányítási része.</span><span class="sxs-lookup"><span data-stu-id="7c358-160">We also require requests toocontain a `Metadata: true` header tooensure that hello actual request was directly intended and not a part of unintentional redirection.</span></span> 

### <a name="error"></a><span data-ttu-id="7c358-161">Hiba</span><span class="sxs-lookup"><span data-stu-id="7c358-161">Error</span></span>
<span data-ttu-id="7c358-162">Ha nem található egy adatelem vagy hibás formátumú kérelem, a példány metaadat-szolgáltatás hello szabványos HTTP-hibák adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7c358-162">If there is a data element not found or a malformed request, hello Instance Metadata Service returns standard HTTP errors.</span></span> <span data-ttu-id="7c358-163">Példa:</span><span class="sxs-lookup"><span data-stu-id="7c358-163">For example:</span></span>

<span data-ttu-id="7c358-164">HTTP-állapotkód</span><span class="sxs-lookup"><span data-stu-id="7c358-164">HTTP Status Code</span></span> | <span data-ttu-id="7c358-165">Ok</span><span class="sxs-lookup"><span data-stu-id="7c358-165">Reason</span></span>
----------------|-------
<span data-ttu-id="7c358-166">200 OK</span><span class="sxs-lookup"><span data-stu-id="7c358-166">200 OK</span></span> |
<span data-ttu-id="7c358-167">400 Hibás kérés</span><span class="sxs-lookup"><span data-stu-id="7c358-167">400 Bad Request</span></span> | <span data-ttu-id="7c358-168">Hiányzó `Metadata: true` fejléc</span><span class="sxs-lookup"><span data-stu-id="7c358-168">Missing `Metadata: true` header</span></span>
<span data-ttu-id="7c358-169">404 – Nem található</span><span class="sxs-lookup"><span data-stu-id="7c358-169">404 Not Found</span></span> | <span data-ttu-id="7c358-170">hello kért elem does't létezik</span><span class="sxs-lookup"><span data-stu-id="7c358-170">hello requested element does't exist</span></span> 
<span data-ttu-id="7c358-171">405 metódus nem engedélyezett</span><span class="sxs-lookup"><span data-stu-id="7c358-171">405 Method Not Allowed</span></span> | <span data-ttu-id="7c358-172">Csak `GET` és `POST` kérelmek támogatottak.</span><span class="sxs-lookup"><span data-stu-id="7c358-172">Only `GET` and `POST` requests are supported</span></span>
<span data-ttu-id="7c358-173">429 túl sok kérelem</span><span class="sxs-lookup"><span data-stu-id="7c358-173">429 Too Many Requests</span></span> | <span data-ttu-id="7c358-174">hello API jelenleg legfeljebb 5 lekérdezések száma másodpercenként</span><span class="sxs-lookup"><span data-stu-id="7c358-174">hello API currently supports a maximum of 5 queries per second</span></span>
<span data-ttu-id="7c358-175">500 szolgáltatási hiba</span><span class="sxs-lookup"><span data-stu-id="7c358-175">500 Service Error</span></span>     | <span data-ttu-id="7c358-176">Némi várakozás után próbálja meg újra</span><span class="sxs-lookup"><span data-stu-id="7c358-176">Retry after some time</span></span>

### <a name="examples"></a><span data-ttu-id="7c358-177">Példák</span><span class="sxs-lookup"><span data-stu-id="7c358-177">Examples</span></span>

> [!NOTE] 
> <span data-ttu-id="7c358-178">Minden API-válasz olyan JSON-karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="7c358-178">All API responses are JSON strings.</span></span> <span data-ttu-id="7c358-179">Minden, a következő példa válaszok pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="7c358-179">All following example responses  are pretty-printed for readability.</span></span>

#### <a name="retrieving-network-information"></a><span data-ttu-id="7c358-180">Hálózati adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="7c358-180">Retrieving network information</span></span>

<span data-ttu-id="7c358-181">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="7c358-181">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-04-02"
```

<span data-ttu-id="7c358-182">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="7c358-182">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="7c358-183">a rendszer hello választ, egy JSON-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="7c358-183">hello response is a JSON string.</span></span> <span data-ttu-id="7c358-184">a következő példa egy válasz hello pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="7c358-184">hello following example response is pretty-printed for readability.</span></span>

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

#### <a name="retrieving-public-ip-address"></a><span data-ttu-id="7c358-185">Nyilvános IP-cím beolvasása</span><span class="sxs-lookup"><span data-stu-id="7c358-185">Retrieving public IP address</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a><span data-ttu-id="7c358-186">Egy példányát minden metaadatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="7c358-186">Retrieving all metadata for an instance</span></span>

<span data-ttu-id="7c358-187">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="7c358-187">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

<span data-ttu-id="7c358-188">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="7c358-188">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="7c358-189">a rendszer hello választ, egy JSON-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="7c358-189">hello response is a JSON string.</span></span> <span data-ttu-id="7c358-190">a következő példa egy válasz hello pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="7c358-190">hello following example response is pretty-printed for readability.</span></span>

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

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a><span data-ttu-id="7c358-191">A Windows rendszerű virtuális gép metaadatainak lekérése során</span><span class="sxs-lookup"><span data-stu-id="7c358-191">Retrieving metadata in Windows virtual machine</span></span>

<span data-ttu-id="7c358-192">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="7c358-192">**Request**</span></span>

<span data-ttu-id="7c358-193">Példány metaadatok lehet beolvasni a Windows hello PowerShell segédprogram segítségével `curl`:</span><span class="sxs-lookup"><span data-stu-id="7c358-193">Instance metadata can be retrieved in Windows via hello PowerShell utility `curl`:</span></span> 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

<span data-ttu-id="7c358-194">Vagy hello `Invoke-RestMethod` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="7c358-194">Or through hello `Invoke-RestMethod` cmdlet:</span></span>
    
```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

<span data-ttu-id="7c358-195">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="7c358-195">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="7c358-196">a rendszer hello választ, egy JSON-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="7c358-196">hello response is a JSON string.</span></span> <span data-ttu-id="7c358-197">a következő példa egy válasz hello pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="7c358-197">hello following example response  is pretty-printed for readability.</span></span>

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

## <a name="instance-metadata-data-categories"></a><span data-ttu-id="7c358-198">Példány metaadat az adatkategóriákat</span><span class="sxs-lookup"><span data-stu-id="7c358-198">Instance metadata data categories</span></span>
<span data-ttu-id="7c358-199">az adatkategóriákat a következő hello hello példány metaadat-szolgáltatás keresztül érhetők el:</span><span class="sxs-lookup"><span data-stu-id="7c358-199">hello following data categories are available through hello Instance Metadata Service:</span></span>

<span data-ttu-id="7c358-200">Adatok</span><span class="sxs-lookup"><span data-stu-id="7c358-200">Data</span></span> | <span data-ttu-id="7c358-201">Leírás</span><span class="sxs-lookup"><span data-stu-id="7c358-201">Description</span></span>
-----|------------
<span data-ttu-id="7c358-202">location</span><span class="sxs-lookup"><span data-stu-id="7c358-202">location</span></span> | <span data-ttu-id="7c358-203">Az Azure régióban hello virtuális gép fut.</span><span class="sxs-lookup"><span data-stu-id="7c358-203">Azure Region hello VM is running in</span></span>
<span data-ttu-id="7c358-204">név</span><span class="sxs-lookup"><span data-stu-id="7c358-204">name</span></span> | <span data-ttu-id="7c358-205">Hello virtuális gép neve</span><span class="sxs-lookup"><span data-stu-id="7c358-205">Name of hello VM</span></span> 
<span data-ttu-id="7c358-206">az ajánlat</span><span class="sxs-lookup"><span data-stu-id="7c358-206">offer</span></span> | <span data-ttu-id="7c358-207">Hello VM-lemezkép információkat nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="7c358-207">Offer information for hello VM image.</span></span> <span data-ttu-id="7c358-208">Kép: Azure-galériából telepített lemezképek nincs csak ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="7c358-208">This value is only present for images deployed from Azure image gallery.</span></span>
<span data-ttu-id="7c358-209">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="7c358-209">publisher</span></span> | <span data-ttu-id="7c358-210">Hello Virtuálisgép-lemezkép kiadója</span><span class="sxs-lookup"><span data-stu-id="7c358-210">Publisher of hello VM image</span></span>
<span data-ttu-id="7c358-211">Termékváltozat</span><span class="sxs-lookup"><span data-stu-id="7c358-211">sku</span></span> | <span data-ttu-id="7c358-212">Adott SKU hello VM-lemezkép</span><span class="sxs-lookup"><span data-stu-id="7c358-212">Specific SKU for hello VM image</span></span>  
<span data-ttu-id="7c358-213">Verzió</span><span class="sxs-lookup"><span data-stu-id="7c358-213">version</span></span> | <span data-ttu-id="7c358-214">Hello Virtuálisgép-lemezkép verziója</span><span class="sxs-lookup"><span data-stu-id="7c358-214">Version of hello VM image</span></span> 
<span data-ttu-id="7c358-215">osType</span><span class="sxs-lookup"><span data-stu-id="7c358-215">osType</span></span> | <span data-ttu-id="7c358-216">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="7c358-216">Linux or Windows</span></span> 
<span data-ttu-id="7c358-217">platformUpdateDomain</span><span class="sxs-lookup"><span data-stu-id="7c358-217">platformUpdateDomain</span></span> |  <span data-ttu-id="7c358-218">[Frissítési tartományok](manage-availability.md) hello virtuális gép fut.</span><span class="sxs-lookup"><span data-stu-id="7c358-218">[Update domain](manage-availability.md) hello VM is running in</span></span>
<span data-ttu-id="7c358-219">platformFaultDomain</span><span class="sxs-lookup"><span data-stu-id="7c358-219">platformFaultDomain</span></span> | <span data-ttu-id="7c358-220">[Tartalék tartomány](manage-availability.md) hello virtuális gép fut.</span><span class="sxs-lookup"><span data-stu-id="7c358-220">[Fault domain](manage-availability.md) hello VM is running in</span></span>
<span data-ttu-id="7c358-221">vmId</span><span class="sxs-lookup"><span data-stu-id="7c358-221">vmId</span></span> | <span data-ttu-id="7c358-222">[Egyedi azonosító](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) hello virtuális gép számára</span><span class="sxs-lookup"><span data-stu-id="7c358-222">[Unique identifier](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) for hello VM</span></span>
<span data-ttu-id="7c358-223">vmSize</span><span class="sxs-lookup"><span data-stu-id="7c358-223">vmSize</span></span> | [<span data-ttu-id="7c358-224">Virtuálisgép-mérettel</span><span class="sxs-lookup"><span data-stu-id="7c358-224">VM size</span></span>](sizes.md)
<span data-ttu-id="7c358-225">IPv4/privateipaddress tulajdonságot</span><span class="sxs-lookup"><span data-stu-id="7c358-225">ipv4/privateIpAddress</span></span> | <span data-ttu-id="7c358-226">Virtuális gép hello helyi IPv4-címe</span><span class="sxs-lookup"><span data-stu-id="7c358-226">Local IPv4 address of hello VM</span></span> 
<span data-ttu-id="7c358-227">IPv4-vagy nyilvános</span><span class="sxs-lookup"><span data-stu-id="7c358-227">ipv4/publicIpAddress</span></span> | <span data-ttu-id="7c358-228">Virtuális gép hello nyilvános IPv4-címe</span><span class="sxs-lookup"><span data-stu-id="7c358-228">Public IPv4 address of hello VM</span></span>
<span data-ttu-id="7c358-229">alhálózat és címét</span><span class="sxs-lookup"><span data-stu-id="7c358-229">subnet/address</span></span> | <span data-ttu-id="7c358-230">Virtuális gép hello alhálózati címe</span><span class="sxs-lookup"><span data-stu-id="7c358-230">Subnet address of hello VM</span></span>
<span data-ttu-id="7c358-231">/ előtagot.</span><span class="sxs-lookup"><span data-stu-id="7c358-231">subnet/prefix</span></span> | <span data-ttu-id="7c358-232">Példa 24 alhálózati előtag</span><span class="sxs-lookup"><span data-stu-id="7c358-232">Subnet prefix, example 24</span></span>
<span data-ttu-id="7c358-233">IPv6/IP-cím</span><span class="sxs-lookup"><span data-stu-id="7c358-233">ipv6/ipAddress</span></span> | <span data-ttu-id="7c358-234">Virtuális gép hello helyi IPv6-cím</span><span class="sxs-lookup"><span data-stu-id="7c358-234">Local IPv6 address of hello VM</span></span>
<span data-ttu-id="7c358-235">MacAddress</span><span class="sxs-lookup"><span data-stu-id="7c358-235">macAddress</span></span> | <span data-ttu-id="7c358-236">Virtuális gép mac-cím</span><span class="sxs-lookup"><span data-stu-id="7c358-236">VM mac address</span></span> 
<span data-ttu-id="7c358-237">scheduledevents</span><span class="sxs-lookup"><span data-stu-id="7c358-237">scheduledevents</span></span> | <span data-ttu-id="7c358-238">Jelenleg a nyilvános előzetes lásd: [scheduledevents](scheduled-events.md)</span><span class="sxs-lookup"><span data-stu-id="7c358-238">Currently in Public Preview See [scheduledevents](scheduled-events.md)</span></span>

## <a name="example-scenarios-for-usage"></a><span data-ttu-id="7c358-239">Példa használati forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="7c358-239">Example scenarios for usage</span></span>  

### <a name="tracking-vm-running-on-azure"></a><span data-ttu-id="7c358-240">Azure-on futó virtuális gép nyomon követése</span><span class="sxs-lookup"><span data-stu-id="7c358-240">Tracking VM running on Azure</span></span>

<span data-ttu-id="7c358-241">Szolgáltató előfordulhat, hogy a szoftver futó virtuális gépek száma tootrack hello, vagy hello VM tootrack egyediségének igénylő ügynökök.</span><span class="sxs-lookup"><span data-stu-id="7c358-241">As a service provider, you may require tootrack hello number of VMs running your software or have agents that need tootrack uniqueness of hello VM.</span></span> <span data-ttu-id="7c358-242">toobe képes tooget egyedi Azonosítót a virtuális gépek használata hello `vmId` példány metaadat-szolgáltatásának mezőjét.</span><span class="sxs-lookup"><span data-stu-id="7c358-242">toobe able tooget a unique ID for a VM, use hello `vmId` field from Instance Metadata Service.</span></span>

<span data-ttu-id="7c358-243">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="7c358-243">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

<span data-ttu-id="7c358-244">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="7c358-244">**Response**</span></span>

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a><span data-ttu-id="7c358-245">Tárolók elhelyezését, adat-partíciók alapú tartalék/frissítési tartomány</span><span class="sxs-lookup"><span data-stu-id="7c358-245">Placement of containers, data-partitions based fault/update domain</span></span> 

<span data-ttu-id="7c358-246">Az egyes forgatókönyvek, más adatok replikák elhelyezését van elsődleges fontosságú.</span><span class="sxs-lookup"><span data-stu-id="7c358-246">For certain scenarios, placement of different data replicas is of prime importance.</span></span> <span data-ttu-id="7c358-247">Például [HDFS replika elhelyezésének](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) vagy tároló elhelyezési keresztül egy [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) igényelheti tooknow hello `platformFaultDomain` és `platformUpdateDomain` hello futó virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="7c358-247">For example, [HDFS replica placement](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) or container placement via an [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) may you require tooknow hello `platformFaultDomain` and `platformUpdateDomain` hello VM is running on.</span></span>
<span data-ttu-id="7c358-248">Ezek az adatok közvetlenül hello példány metaadat-szolgáltatás segítségével lekérdezheti.</span><span class="sxs-lookup"><span data-stu-id="7c358-248">You can query this data directly via hello Instance Metadata Service.</span></span>

<span data-ttu-id="7c358-249">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="7c358-249">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

<span data-ttu-id="7c358-250">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="7c358-250">**Response**</span></span>

```
0
```

### <a name="getting-more-information-about-hello-vm-during-support-case"></a><span data-ttu-id="7c358-251">További információ a virtuális gép hello során támogatási esetet beolvasása</span><span class="sxs-lookup"><span data-stu-id="7c358-251">Getting more information about hello VM during support case</span></span> 

<span data-ttu-id="7c358-252">Szolgáltató előfordulhat, hogy kap egy támogatási hívás tooknow helyének hello VM további információt.</span><span class="sxs-lookup"><span data-stu-id="7c358-252">As a service provider, you may get a support call where you would like tooknow more information about hello VM.</span></span> <span data-ttu-id="7c358-253">Hello ügyfél tooshare kérése a hello számítási metaadatok alapvető információkat a támogatási szakemberek tooknow hello hello típusú virtuális gép Azure biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="7c358-253">Asking hello customer tooshare hello compute metadata can provide basic information for hello support professional tooknow about hello kind of VM on Azure.</span></span> 

<span data-ttu-id="7c358-254">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="7c358-254">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

<span data-ttu-id="7c358-255">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="7c358-255">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="7c358-256">a rendszer hello választ, egy JSON-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="7c358-256">hello response is a JSON string.</span></span> <span data-ttu-id="7c358-257">a következő példa egy válasz hello pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="7c358-257">hello following example response is pretty-printed for readability.</span></span>

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

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-hello-vm"></a><span data-ttu-id="7c358-258">Példák hello VM belül különböző nyelveken használó metaadatok szolgáltatás hívása</span><span class="sxs-lookup"><span data-stu-id="7c358-258">Examples of calling metadata service using different languages inside hello VM</span></span> 

<span data-ttu-id="7c358-259">Nyelv</span><span class="sxs-lookup"><span data-stu-id="7c358-259">Language</span></span> | <span data-ttu-id="7c358-260">Példa</span><span class="sxs-lookup"><span data-stu-id="7c358-260">Example</span></span> 
---------|----------------
<span data-ttu-id="7c358-261">Ruby</span><span class="sxs-lookup"><span data-stu-id="7c358-261">Ruby</span></span>     | <span data-ttu-id="7c358-262">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.RB</span><span class="sxs-lookup"><span data-stu-id="7c358-262">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb</span></span>
<span data-ttu-id="7c358-263">Nyissa meg a helyi hálózat</span><span class="sxs-lookup"><span data-stu-id="7c358-263">Go Lan</span></span>   | <span data-ttu-id="7c358-264">https://github.com/Microsoft/azureimds/BLOB/Master/imdssample.go</span><span class="sxs-lookup"><span data-stu-id="7c358-264">https://github.com/Microsoft/azureimds/blob/master/imdssample.go</span></span>            
<span data-ttu-id="7c358-265">python</span><span class="sxs-lookup"><span data-stu-id="7c358-265">python</span></span>   | <span data-ttu-id="7c358-266">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.PY</span><span class="sxs-lookup"><span data-stu-id="7c358-266">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py</span></span>
<span data-ttu-id="7c358-267">C++</span><span class="sxs-lookup"><span data-stu-id="7c358-267">C++</span></span>      | <span data-ttu-id="7c358-268">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample-Windows.cpp</span><span class="sxs-lookup"><span data-stu-id="7c358-268">https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp</span></span>
<span data-ttu-id="7c358-269">C#</span><span class="sxs-lookup"><span data-stu-id="7c358-269">C#</span></span>       | <span data-ttu-id="7c358-270">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.cs</span><span class="sxs-lookup"><span data-stu-id="7c358-270">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs</span></span>
<span data-ttu-id="7c358-271">Javascript</span><span class="sxs-lookup"><span data-stu-id="7c358-271">Javascript</span></span> | <span data-ttu-id="7c358-272">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.js</span><span class="sxs-lookup"><span data-stu-id="7c358-272">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js</span></span>
<span data-ttu-id="7c358-273">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7c358-273">Powershell</span></span> | <span data-ttu-id="7c358-274">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.ps1</span><span class="sxs-lookup"><span data-stu-id="7c358-274">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1</span></span>
<span data-ttu-id="7c358-275">Bash</span><span class="sxs-lookup"><span data-stu-id="7c358-275">Bash</span></span>       | <span data-ttu-id="7c358-276">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.SH</span><span class="sxs-lookup"><span data-stu-id="7c358-276">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh</span></span>
    

## <a name="faq"></a><span data-ttu-id="7c358-277">GYIK</span><span class="sxs-lookup"><span data-stu-id="7c358-277">FAQ</span></span>
1. <span data-ttu-id="7c358-278">Hello hiba azért kapom `400 Bad Request, Required metadata header not specified`.</span><span class="sxs-lookup"><span data-stu-id="7c358-278">I am getting hello error `400 Bad Request, Required metadata header not specified`.</span></span> <span data-ttu-id="7c358-279">Ez mit jelent?</span><span class="sxs-lookup"><span data-stu-id="7c358-279">What does this mean?</span></span>
   * <span data-ttu-id="7c358-280">hello példány metaadat-szolgáltatás hello fejlécet igényel `Metadata: true` toobe átadott hello kérelemben.</span><span class="sxs-lookup"><span data-stu-id="7c358-280">hello Instance Metadata Service requires hello header `Metadata: true` toobe passed in hello request.</span></span> <span data-ttu-id="7c358-281">Ezt a fejlécet benyújtása hello REST-hívást lehetővé teszi, hogy a hozzáférés toohello példány metaadat-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7c358-281">Passing this header in hello REST call allows access toohello Instance Metadata Service.</span></span> 
2. <span data-ttu-id="7c358-282">Miért nem jelenik meg a virtuális gép számítási adatai?</span><span class="sxs-lookup"><span data-stu-id="7c358-282">Why am I not getting compute information for my VM?</span></span>
   * <span data-ttu-id="7c358-283">Hello példány metaadat-szolgáltatás jelenleg csak az Azure Resource Manager eszközzel létrehozott példányokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="7c358-283">Currently hello Instance Metadata Service only supports instances created with Azure Resource Manager.</span></span> <span data-ttu-id="7c358-284">A jövőbeli hello azt is támogatásáról Cloud Service virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="7c358-284">In hello future, we may add support for Cloud Service VMs.</span></span>
3. <span data-ttu-id="7c358-285">A virtuális gép Azure Resource Manager használatával létrehozott egy while vissza.</span><span class="sxs-lookup"><span data-stu-id="7c358-285">I created my Virtual Machine through Azure Resource Manager a while back.</span></span> <span data-ttu-id="7c358-286">Miért nem tudok lásd számítási metaadat-információ?</span><span class="sxs-lookup"><span data-stu-id="7c358-286">Why am I not see compute metadata information?</span></span>
   * <span data-ttu-id="7c358-287">A 2016. szeptember után létrehozott virtuális gépek hozzáadása egy [címke](../../azure-resource-manager/resource-group-using-tags.md) toostart megnéz számítási metaadatok.</span><span class="sxs-lookup"><span data-stu-id="7c358-287">For any VMs created after Sep 2016, add a [Tag](../../azure-resource-manager/resource-group-using-tags.md) toostart seeing compute metadata.</span></span> <span data-ttu-id="7c358-288">(Szeptember 2016 előtt létrehozott) régebbi virtuális gépek hozzáadása extensions vagy adatok lemezek toohello VM toorefresh metaadatok.</span><span class="sxs-lookup"><span data-stu-id="7c358-288">For older VMs (created before Sep 2016), add/remove extensions or data disks toohello VM toorefresh metadata.</span></span>
4. <span data-ttu-id="7c358-289">Miért jelenik meg hello hiba `500 Internal Server Error`?</span><span class="sxs-lookup"><span data-stu-id="7c358-289">Why am I getting hello error `500 Internal Server Error`?</span></span>
   * <span data-ttu-id="7c358-290">Próbálja megismételni a kérést a rendszer ki exponenciális vissza alapján.</span><span class="sxs-lookup"><span data-stu-id="7c358-290">Please retry your request based on exponential back off system.</span></span> <span data-ttu-id="7c358-291">Ha nem szűnik hello Azure ügyfélszolgálatához.</span><span class="sxs-lookup"><span data-stu-id="7c358-291">If hello issue persists contact  Azure support.</span></span>
5. <span data-ttu-id="7c358-292">Ha ugyanazt a további kérdéseit vagy megjegyzéseit?</span><span class="sxs-lookup"><span data-stu-id="7c358-292">Where do I share additional questions/comments?</span></span>
   * <span data-ttu-id="7c358-293">A Megjegyzések küldése a http://feedback.azure.com.</span><span class="sxs-lookup"><span data-stu-id="7c358-293">Send your comments on http://feedback.azure.com.</span></span>
7. <span data-ttu-id="7c358-294">Ez használható lenne a virtuálisgép-méretezési beállítása példányt?</span><span class="sxs-lookup"><span data-stu-id="7c358-294">Would this work for Virtual Machine Scale Set Instance?</span></span>
   * <span data-ttu-id="7c358-295">Igen metaadat-szolgáltatás esetén méretezési beállítása példányok érhető el.</span><span class="sxs-lookup"><span data-stu-id="7c358-295">Yes Metadata service is available for Scale Set Instances.</span></span> 
6. <span data-ttu-id="7c358-296">Hogyan kérhet támogatást hello szolgáltatás?</span><span class="sxs-lookup"><span data-stu-id="7c358-296">How do I get support for hello service?</span></span>
   * <span data-ttu-id="7c358-297">tooget támogatása hello szolgáltatást, hozzon létre egy támogatási probléma Azure-portál a virtuális gép, amelyen még nem tud tooget metaadatok válasz hosszú újrapróbálkozás után hello</span><span class="sxs-lookup"><span data-stu-id="7c358-297">tooget support for hello service, create a support issue in Azure portal for hello VM where you are not able tooget metadata response after long retries</span></span> 

   ![Példány metaadatok támogatása](./media/instance-metadata-service/InstanceMetadata-support.png)
    
## <a name="next-steps"></a><span data-ttu-id="7c358-299">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7c358-299">Next steps</span></span>

- <span data-ttu-id="7c358-300">További tudnivalók hello [ütemezett események](scheduled-events.md) API **nyilvános előzetes verziójában** hello példány metaadat-szolgáltatás által biztosított.</span><span class="sxs-lookup"><span data-stu-id="7c358-300">Learn more about hello [Scheduled Events](scheduled-events.md) API **in public preview** provided by hello Instance Metadata service.</span></span>
