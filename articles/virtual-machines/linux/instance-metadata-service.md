---
title: "Linux virtuális gépek Azure-példányokon metaadat-szolgáltatás |} Microsoft Docs"
description: "Linux virtuális gép számítási, hálózati, és a közelgő karbantartásokról események adatainak beolvasása rESTful felületet."
services: virtual-machines-linux
documentationcenter: 
author: harijay
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: harijay
ms.openlocfilehash: a61acbe0532ece3a6a26ceb366c12c69db4c304c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-instance-metadata-service-for-linux-vms"></a><span data-ttu-id="8d333-103">Linux virtuális gépek Azure példány metaadat-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="8d333-103">Azure Instance Metadata service for Linux VMs</span></span>


<span data-ttu-id="8d333-104">Az Azure példány metaadat-szolgáltatás kezelése és konfigurálása a virtuális gépek használható virtuálisgép-példányok futtatásával kapcsolatos információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="8d333-104">The Azure Instance Metadata Service provides information about running virtual machine instances that can be used to manage and configure your virtual machines.</span></span>
<span data-ttu-id="8d333-105">Ide tartoznak például Termékváltozat, a hálózati konfigurációs és a közelgő karbantartásokról események.</span><span class="sxs-lookup"><span data-stu-id="8d333-105">This includes information such as SKU, network configuration, and upcoming maintenance events.</span></span> <span data-ttu-id="8d333-106">Milyen típusú információkat érhető el a további információkért lásd: [metaadat-kategóriák](#instance-metadata-data-categories).</span><span class="sxs-lookup"><span data-stu-id="8d333-106">For more information on what type of information is available, see [metadata categories](#instance-metadata-data-categories).</span></span>

<span data-ttu-id="8d333-107">Azure-példány metaadat-szolgáltatás segítségével létrehozott összes infrastruktúra-szolgáltatási virtuális gép számára elérhető REST-végpont a [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="8d333-107">Azure's Instance Metadata Service is a REST Endpoint accessible to all IaaS VMs created via the [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="8d333-108">A végpont egy jól ismert nem irányítható IP-címen érhető el (`169.254.169.254`), amely elérhető csak a virtuális Gépen belül.</span><span class="sxs-lookup"><span data-stu-id="8d333-108">The endpoint is available at a well-known non-routable IP address (`169.254.169.254`) that can be accessed only from within the VM.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8d333-109">Ez a szolgáltatás **általánosan elérhető** globális Azure-régióban.</span><span class="sxs-lookup"><span data-stu-id="8d333-109">This service is  **generally available** in Global Azure Regions.</span></span> <span data-ttu-id="8d333-110">Nyilvános előzetes kormányzati, Kína és német Azure felhőben van.</span><span class="sxs-lookup"><span data-stu-id="8d333-110">It is in Public preview for Government, China, and German Azure Cloud.</span></span> <span data-ttu-id="8d333-111">Rendszeresen teszi közzé a virtuálisgép-példányok új információ a frissítések kap.</span><span class="sxs-lookup"><span data-stu-id="8d333-111">It regularly receives updates to expose new information about virtual machine instances.</span></span> <span data-ttu-id="8d333-112">Ezen a lapon tükrözi a legfrissebb [az adatkategóriákat](#instance-metadata-data-categories) érhető el.</span><span class="sxs-lookup"><span data-stu-id="8d333-112">This page reflects the up-to-date [data categories](#instance-metadata-data-categories) available.</span></span>

## <a name="service-availability"></a><span data-ttu-id="8d333-113">Elérhető szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="8d333-113">Service availability</span></span>
<span data-ttu-id="8d333-114">A szolgáltatás minden általánosan elérhető globális Azure területen érhető el.</span><span class="sxs-lookup"><span data-stu-id="8d333-114">The service is available in all generally available Global Azure regions.</span></span> <span data-ttu-id="8d333-115">A szolgáltatás a kormányzati, Kína vagy Németország régiókban nyilvános előzetes verzió van.</span><span class="sxs-lookup"><span data-stu-id="8d333-115">The service is in public preview  in the Government, China, or Germany regions.</span></span>

<span data-ttu-id="8d333-116">Régiók</span><span class="sxs-lookup"><span data-stu-id="8d333-116">Regions</span></span>                                        | <span data-ttu-id="8d333-117">Rendelkezésre állási?</span><span class="sxs-lookup"><span data-stu-id="8d333-117">Availability?</span></span>
-----------------------------------------------|-----------------------------------------------
[<span data-ttu-id="8d333-118">Minden általánosan elérhető globális Azure-régió</span><span class="sxs-lookup"><span data-stu-id="8d333-118">All Generally Available Global Azure Regions</span></span>](https://azure.microsoft.com/regions/)     | <span data-ttu-id="8d333-119">Általánosan elérhető</span><span class="sxs-lookup"><span data-stu-id="8d333-119">Generally Available</span></span> 
[<span data-ttu-id="8d333-120">Azure Government</span><span class="sxs-lookup"><span data-stu-id="8d333-120">Azure Government</span></span>](https://azure.microsoft.com/overview/clouds/government/)              | <span data-ttu-id="8d333-121">Az előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="8d333-121">In Preview</span></span> 
[<span data-ttu-id="8d333-122">Az Azure Kínában</span><span class="sxs-lookup"><span data-stu-id="8d333-122">Azure China</span></span>](https://www.azure.cn/)                                                           | <span data-ttu-id="8d333-123">Az előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="8d333-123">In Preview</span></span>
[<span data-ttu-id="8d333-124">Az Azure-Németország</span><span class="sxs-lookup"><span data-stu-id="8d333-124">Azure Germany</span></span>](https://azure.microsoft.com/overview/clouds/germany/)                    | <span data-ttu-id="8d333-125">Az előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="8d333-125">In Preview</span></span>

<span data-ttu-id="8d333-126">Ez a táblázat frissülni fog, amikor a szolgáltatás elérhetővé válnak más Azure felhőben.</span><span class="sxs-lookup"><span data-stu-id="8d333-126">This table is updated when the service becomes available in other Azure clouds.</span></span>

<span data-ttu-id="8d333-127">A példány metaadat-szolgáltatás kipróbálására, a virtuális gép létrehozása az [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) vagy a [Azure-portálon](http://portal.azure.com) a fenti régiókban, és kövesse az alábbi példák.</span><span class="sxs-lookup"><span data-stu-id="8d333-127">To try out the Instance Metadata Service, create a VM from [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) or the [Azure portal](http://portal.azure.com) in the above regions and follow the examples below.</span></span>

## <a name="usage"></a><span data-ttu-id="8d333-128">Használat</span><span class="sxs-lookup"><span data-stu-id="8d333-128">Usage</span></span>

### <a name="versioning"></a><span data-ttu-id="8d333-129">Versioning</span><span class="sxs-lookup"><span data-stu-id="8d333-129">Versioning</span></span>
<span data-ttu-id="8d333-130">A példány metaadat-szolgáltatás nem rendszerverzióval ellátott.</span><span class="sxs-lookup"><span data-stu-id="8d333-130">The Instance Metadata Service is versioned.</span></span> <span data-ttu-id="8d333-131">Verziók kötelező, és a jelenlegi verzió: `2017-04-02`.</span><span class="sxs-lookup"><span data-stu-id="8d333-131">Versions are mandatory and the current version is `2017-04-02`.</span></span>

> [!NOTE] 
> <span data-ttu-id="8d333-132">Előző előzetes kiadásaiban ütemezett események {legújabb} támogatott api-verzióval.</span><span class="sxs-lookup"><span data-stu-id="8d333-132">Previous preview releases of scheduled events supported {latest} as the api-version.</span></span> <span data-ttu-id="8d333-133">Ez a formátum már nem támogatott, és a jövőben elavulttá válik.</span><span class="sxs-lookup"><span data-stu-id="8d333-133">This format is no longer supported and will be deprecated in the future.</span></span>

<span data-ttu-id="8d333-134">Azt hozzáadja az újabb verziók, régebbi verziók továbbra is elérhető kompatibilitás, ha a parancsfájlok tartalmazhat függőségeket, meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="8d333-134">As we add newer versions, older versions can still be accessed for compatibility if your scripts have dependencies on specific data formats.</span></span> <span data-ttu-id="8d333-135">Azonban előfordulhat, hogy az aktuális előnézeti version(2017-03-01) nem érhető el a szolgáltatás általánosan elérhetővé válik.</span><span class="sxs-lookup"><span data-stu-id="8d333-135">However, note that the current preview version(2017-03-01) may not be available once the service is generally available.</span></span>

### <a name="using-headers"></a><span data-ttu-id="8d333-136">Fejlécek használata</span><span class="sxs-lookup"><span data-stu-id="8d333-136">Using headers</span></span>
<span data-ttu-id="8d333-137">Ha a példány metaadat-szolgáltatás, meg kell adnia a fejléc `Metadata: true` annak érdekében, hogy nem szándékos átirányítja a kérést.</span><span class="sxs-lookup"><span data-stu-id="8d333-137">When you query the Instance Metadata Service, you must provide the header `Metadata: true` to ensure the request was not unintentionally redirected.</span></span>

### <a name="retrieving-metadata"></a><span data-ttu-id="8d333-138">Metaadatainak lekérése során</span><span class="sxs-lookup"><span data-stu-id="8d333-138">Retrieving metadata</span></span>

<span data-ttu-id="8d333-139">Példány metaadatai nem érhető el a futó virtuális gépek létrehozása/segítségével kezelt [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="8d333-139">Instance metadata is available for running VMs created/managed using [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).</span></span> <span data-ttu-id="8d333-140">Elérhető összes az adatkategóriákat a virtuális gép példány használatával a következő kérelmet:</span><span class="sxs-lookup"><span data-stu-id="8d333-140">Access all data categories for a virtual machine instance using the following request:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> <span data-ttu-id="8d333-141">Minden példány metaadatok lekérdezés-és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="8d333-141">All instance metadata queries are case-sensitive.</span></span>

### <a name="data-output"></a><span data-ttu-id="8d333-142">Kimeneti adatok</span><span class="sxs-lookup"><span data-stu-id="8d333-142">Data output</span></span>
<span data-ttu-id="8d333-143">Alapértelmezés szerint a példány metaadat-szolgáltatás adatokat adja vissza JSON formátumban (`Content-Type: application/json`).</span><span class="sxs-lookup"><span data-stu-id="8d333-143">By default, the Instance Metadata Service returns data in JSON format (`Content-Type: application/json`).</span></span> <span data-ttu-id="8d333-144">Azonban más API-k adhatnak vissza adatokat különböző formátumokban kérésre.</span><span class="sxs-lookup"><span data-stu-id="8d333-144">However, different APIs can return data in different formats if requested.</span></span>
<span data-ttu-id="8d333-145">A következő táblázat a más adatok formátumok API-k támogathatja a hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="8d333-145">The following table is a reference of other data formats APIs may support.</span></span>

<span data-ttu-id="8d333-146">API</span><span class="sxs-lookup"><span data-stu-id="8d333-146">API</span></span> | <span data-ttu-id="8d333-147">Alapértelmezett adatformátum</span><span class="sxs-lookup"><span data-stu-id="8d333-147">Default Data Format</span></span> | <span data-ttu-id="8d333-148">Eltérő formátumban</span><span class="sxs-lookup"><span data-stu-id="8d333-148">Other Formats</span></span>
--------|---------------------|--------------
<span data-ttu-id="8d333-149">/instance</span><span class="sxs-lookup"><span data-stu-id="8d333-149">/instance</span></span> | <span data-ttu-id="8d333-150">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="8d333-150">json</span></span> | <span data-ttu-id="8d333-151">Szöveg</span><span class="sxs-lookup"><span data-stu-id="8d333-151">text</span></span>
<span data-ttu-id="8d333-152">/scheduledevents</span><span class="sxs-lookup"><span data-stu-id="8d333-152">/scheduledevents</span></span> | <span data-ttu-id="8d333-153">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="8d333-153">json</span></span> | <span data-ttu-id="8d333-154">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="8d333-154">none</span></span>

<span data-ttu-id="8d333-155">Egy nem alapértelmezett válaszformátum szeretne használni, adja meg a kért formátumát a kérelem lekérdezési karakterlánc paraméterként.</span><span class="sxs-lookup"><span data-stu-id="8d333-155">To access a non-default response format, specify the requested format as a querystring parameter in the request.</span></span> <span data-ttu-id="8d333-156">Példa:</span><span class="sxs-lookup"><span data-stu-id="8d333-156">For example:</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### <a name="security"></a><span data-ttu-id="8d333-157">Biztonság</span><span class="sxs-lookup"><span data-stu-id="8d333-157">Security</span></span>
<span data-ttu-id="8d333-158">A példány metaadatok szolgáltatás végpontjának értéke csak a futó virtuális gép példány nem irányítható IP-címnek a belülről érhetők el.</span><span class="sxs-lookup"><span data-stu-id="8d333-158">The Instance Metadata Service endpoint is accessible only from within the running virtual machine instance on a non-routable IP address.</span></span> <span data-ttu-id="8d333-159">Emellett a kérelmet egy `X-Forwarded-For` fejléc elutasította a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8d333-159">In addition, any request with a `X-Forwarded-For` header is rejected by the service.</span></span>
<span data-ttu-id="8d333-160">Azt is igénylik, magában foglalja a kérelmek egy `Metadata: true` fejlécének győződjön meg arról, hogy a tényleges kérelem volt közvetlenül tervezett és nem szándékos átirányítási része.</span><span class="sxs-lookup"><span data-stu-id="8d333-160">We also require requests to contain a `Metadata: true` header to ensure that the actual request was directly intended and not a part of unintentional redirection.</span></span> 

### <a name="error"></a><span data-ttu-id="8d333-161">Hiba</span><span class="sxs-lookup"><span data-stu-id="8d333-161">Error</span></span>
<span data-ttu-id="8d333-162">Ha nem található egy adatelem vagy hibás formátumú kérelem, a példány metaadat-szolgáltatás szabványos HTTP-hibák adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8d333-162">If there is a data element not found or a malformed request, the Instance Metadata Service returns standard HTTP errors.</span></span> <span data-ttu-id="8d333-163">Példa:</span><span class="sxs-lookup"><span data-stu-id="8d333-163">For example:</span></span>

<span data-ttu-id="8d333-164">HTTP-állapotkód</span><span class="sxs-lookup"><span data-stu-id="8d333-164">HTTP Status Code</span></span> | <span data-ttu-id="8d333-165">Ok</span><span class="sxs-lookup"><span data-stu-id="8d333-165">Reason</span></span>
----------------|-------
<span data-ttu-id="8d333-166">200 OK</span><span class="sxs-lookup"><span data-stu-id="8d333-166">200 OK</span></span> |
<span data-ttu-id="8d333-167">400 Hibás kérés</span><span class="sxs-lookup"><span data-stu-id="8d333-167">400 Bad Request</span></span> | <span data-ttu-id="8d333-168">Hiányzó `Metadata: true` fejléc</span><span class="sxs-lookup"><span data-stu-id="8d333-168">Missing `Metadata: true` header</span></span>
<span data-ttu-id="8d333-169">404 – Nem található</span><span class="sxs-lookup"><span data-stu-id="8d333-169">404 Not Found</span></span> | <span data-ttu-id="8d333-170">A kért elem does't létezik</span><span class="sxs-lookup"><span data-stu-id="8d333-170">The requested element does't exist</span></span> 
<span data-ttu-id="8d333-171">405 metódus nem engedélyezett</span><span class="sxs-lookup"><span data-stu-id="8d333-171">405 Method Not Allowed</span></span> | <span data-ttu-id="8d333-172">Csak `GET` és `POST` kérelmek támogatottak.</span><span class="sxs-lookup"><span data-stu-id="8d333-172">Only `GET` and `POST` requests are supported</span></span>
<span data-ttu-id="8d333-173">429 túl sok kérelem</span><span class="sxs-lookup"><span data-stu-id="8d333-173">429 Too Many Requests</span></span> | <span data-ttu-id="8d333-174">Az API-t jelenleg legfeljebb 5 lekérdezések száma másodpercenként</span><span class="sxs-lookup"><span data-stu-id="8d333-174">The API currently supports a maximum of 5 queries per second</span></span>
<span data-ttu-id="8d333-175">500 szolgáltatási hiba</span><span class="sxs-lookup"><span data-stu-id="8d333-175">500 Service Error</span></span>     | <span data-ttu-id="8d333-176">Némi várakozás után próbálja meg újra</span><span class="sxs-lookup"><span data-stu-id="8d333-176">Retry after some time</span></span>

### <a name="examples"></a><span data-ttu-id="8d333-177">Példák</span><span class="sxs-lookup"><span data-stu-id="8d333-177">Examples</span></span>

> [!NOTE] 
> <span data-ttu-id="8d333-178">Minden API-válasz olyan JSON-karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="8d333-178">All API responses are JSON strings.</span></span> <span data-ttu-id="8d333-179">Minden, a következő példa válaszok pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="8d333-179">All following example responses  are pretty-printed for readability.</span></span>

#### <a name="retrieving-network-information"></a><span data-ttu-id="8d333-180">Hálózati adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="8d333-180">Retrieving network information</span></span>

<span data-ttu-id="8d333-181">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="8d333-181">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-04-02"
```

<span data-ttu-id="8d333-182">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="8d333-182">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="8d333-183">A válasz egy JSON-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="8d333-183">The response is a JSON string.</span></span> <span data-ttu-id="8d333-184">A következő példa egy válasz pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="8d333-184">The following example response is pretty-printed for readability.</span></span>

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

#### <a name="retrieving-public-ip-address"></a><span data-ttu-id="8d333-185">Nyilvános IP-cím beolvasása</span><span class="sxs-lookup"><span data-stu-id="8d333-185">Retrieving public IP address</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a><span data-ttu-id="8d333-186">Egy példányát minden metaadatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="8d333-186">Retrieving all metadata for an instance</span></span>

<span data-ttu-id="8d333-187">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="8d333-187">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

<span data-ttu-id="8d333-188">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="8d333-188">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="8d333-189">A válasz egy JSON-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="8d333-189">The response is a JSON string.</span></span> <span data-ttu-id="8d333-190">A következő példa egy válasz pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="8d333-190">The following example response is pretty-printed for readability.</span></span>

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

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a><span data-ttu-id="8d333-191">A Windows rendszerű virtuális gép metaadatainak lekérése során</span><span class="sxs-lookup"><span data-stu-id="8d333-191">Retrieving metadata in Windows Virtual Machine</span></span>

<span data-ttu-id="8d333-192">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="8d333-192">**Request**</span></span>

<span data-ttu-id="8d333-193">Példány metaadatok lehet beolvasni a Windows a PowerShell segédprogram segítségével `curl`:</span><span class="sxs-lookup"><span data-stu-id="8d333-193">Instance metadata can be retrieved in Windows via the PowerShell utility `curl`:</span></span> 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

<span data-ttu-id="8d333-194">Vagy a `Invoke-RestMethod` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="8d333-194">Or through the `Invoke-RestMethod` cmdlet:</span></span>
    
```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

<span data-ttu-id="8d333-195">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="8d333-195">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="8d333-196">A válasz egy JSON-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="8d333-196">The response is a JSON string.</span></span> <span data-ttu-id="8d333-197">A következő példa egy válasz pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="8d333-197">The following example response  is pretty-printed for readability.</span></span>

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

## <a name="instance-metadata-data-categories"></a><span data-ttu-id="8d333-198">Példány metaadat az adatkategóriákat</span><span class="sxs-lookup"><span data-stu-id="8d333-198">Instance metadata data categories</span></span>
<span data-ttu-id="8d333-199">A következő az adatkategóriákat a példány metaadatok szolgáltatáson keresztül érhetők el:</span><span class="sxs-lookup"><span data-stu-id="8d333-199">The following data categories are available through the Instance Metadata Service:</span></span>

<span data-ttu-id="8d333-200">Adatok</span><span class="sxs-lookup"><span data-stu-id="8d333-200">Data</span></span> | <span data-ttu-id="8d333-201">Leírás</span><span class="sxs-lookup"><span data-stu-id="8d333-201">Description</span></span>
-----|------------
<span data-ttu-id="8d333-202">location</span><span class="sxs-lookup"><span data-stu-id="8d333-202">location</span></span> | <span data-ttu-id="8d333-203">Azure-régió, a virtuális gép fut.</span><span class="sxs-lookup"><span data-stu-id="8d333-203">Azure Region the VM is running in</span></span>
<span data-ttu-id="8d333-204">név</span><span class="sxs-lookup"><span data-stu-id="8d333-204">name</span></span> | <span data-ttu-id="8d333-205">A virtuális gép neve</span><span class="sxs-lookup"><span data-stu-id="8d333-205">Name of the VM</span></span> 
<span data-ttu-id="8d333-206">az ajánlat</span><span class="sxs-lookup"><span data-stu-id="8d333-206">offer</span></span> | <span data-ttu-id="8d333-207">A VM-lemezkép információkat nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="8d333-207">Offer information for the VM image.</span></span> <span data-ttu-id="8d333-208">Kép: Azure-galériából telepített lemezképek nincs csak ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="8d333-208">This value is only present for images deployed from Azure image gallery.</span></span>
<span data-ttu-id="8d333-209">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="8d333-209">publisher</span></span> | <span data-ttu-id="8d333-210">A Virtuálisgép-lemezkép kiadója</span><span class="sxs-lookup"><span data-stu-id="8d333-210">Publisher of the VM image</span></span>
<span data-ttu-id="8d333-211">Termékváltozat</span><span class="sxs-lookup"><span data-stu-id="8d333-211">sku</span></span> | <span data-ttu-id="8d333-212">A VM-lemezkép adott Termékváltozat</span><span class="sxs-lookup"><span data-stu-id="8d333-212">Specific SKU for the VM image</span></span>  
<span data-ttu-id="8d333-213">Verzió</span><span class="sxs-lookup"><span data-stu-id="8d333-213">version</span></span> | <span data-ttu-id="8d333-214">A Virtuálisgép-lemezkép verziója</span><span class="sxs-lookup"><span data-stu-id="8d333-214">Version of the VM image</span></span> 
<span data-ttu-id="8d333-215">osType</span><span class="sxs-lookup"><span data-stu-id="8d333-215">osType</span></span> | <span data-ttu-id="8d333-216">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="8d333-216">Linux or Windows</span></span> 
<span data-ttu-id="8d333-217">platformUpdateDomain</span><span class="sxs-lookup"><span data-stu-id="8d333-217">platformUpdateDomain</span></span> |  <span data-ttu-id="8d333-218">[Frissítési tartományok](manage-availability.md) a virtuális gép fut.</span><span class="sxs-lookup"><span data-stu-id="8d333-218">[Update domain](manage-availability.md) the VM is running in</span></span>
<span data-ttu-id="8d333-219">platformFaultDomain</span><span class="sxs-lookup"><span data-stu-id="8d333-219">platformFaultDomain</span></span> | <span data-ttu-id="8d333-220">[Tartalék tartomány](manage-availability.md) a virtuális gép fut.</span><span class="sxs-lookup"><span data-stu-id="8d333-220">[Fault domain](manage-availability.md) the VM is running in</span></span>
<span data-ttu-id="8d333-221">vmId</span><span class="sxs-lookup"><span data-stu-id="8d333-221">vmId</span></span> | <span data-ttu-id="8d333-222">[Egyedi azonosító](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="8d333-222">[Unique identifier](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) for the VM</span></span>
<span data-ttu-id="8d333-223">vmSize</span><span class="sxs-lookup"><span data-stu-id="8d333-223">vmSize</span></span> | [<span data-ttu-id="8d333-224">Virtuálisgép-mérettel</span><span class="sxs-lookup"><span data-stu-id="8d333-224">VM size</span></span>](sizes.md)
<span data-ttu-id="8d333-225">IPv4/privateipaddress tulajdonságot</span><span class="sxs-lookup"><span data-stu-id="8d333-225">ipv4/privateIpAddress</span></span> | <span data-ttu-id="8d333-226">A virtuális gép helyi IPv4-címe</span><span class="sxs-lookup"><span data-stu-id="8d333-226">Local IPv4 address of the VM</span></span> 
<span data-ttu-id="8d333-227">IPv4-vagy nyilvános</span><span class="sxs-lookup"><span data-stu-id="8d333-227">ipv4/publicIpAddress</span></span> | <span data-ttu-id="8d333-228">A virtuális gép nyilvános IPv4-cím</span><span class="sxs-lookup"><span data-stu-id="8d333-228">Public IPv4 address of the VM</span></span>
<span data-ttu-id="8d333-229">alhálózat és címét</span><span class="sxs-lookup"><span data-stu-id="8d333-229">subnet/address</span></span> | <span data-ttu-id="8d333-230">A virtuális gép alhálózati cím</span><span class="sxs-lookup"><span data-stu-id="8d333-230">Subnet address of the VM</span></span>
<span data-ttu-id="8d333-231">/ előtagot.</span><span class="sxs-lookup"><span data-stu-id="8d333-231">subnet/prefix</span></span> | <span data-ttu-id="8d333-232">Példa 24 alhálózati előtag</span><span class="sxs-lookup"><span data-stu-id="8d333-232">Subnet prefix, example 24</span></span>
<span data-ttu-id="8d333-233">IPv6/IP-cím</span><span class="sxs-lookup"><span data-stu-id="8d333-233">ipv6/ipAddress</span></span> | <span data-ttu-id="8d333-234">A virtuális gép helyi IPv6-cím</span><span class="sxs-lookup"><span data-stu-id="8d333-234">Local IPv6 address of the VM</span></span>
<span data-ttu-id="8d333-235">MacAddress</span><span class="sxs-lookup"><span data-stu-id="8d333-235">macAddress</span></span> | <span data-ttu-id="8d333-236">Virtuális gép mac-cím</span><span class="sxs-lookup"><span data-stu-id="8d333-236">VM mac address</span></span> 
<span data-ttu-id="8d333-237">scheduledevents</span><span class="sxs-lookup"><span data-stu-id="8d333-237">scheduledevents</span></span> | <span data-ttu-id="8d333-238">Jelenleg a nyilvános előzetes lásd: [scheduledevents](scheduled-events.md)</span><span class="sxs-lookup"><span data-stu-id="8d333-238">Currently in Public Preview See [scheduledevents](scheduled-events.md)</span></span>

## <a name="example-scenarios-for-usage"></a><span data-ttu-id="8d333-239">Példa használati forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="8d333-239">Example scenarios for usage</span></span>  

### <a name="tracking-vm-running-on-azure"></a><span data-ttu-id="8d333-240">Azure-on futó virtuális gép nyomon követése</span><span class="sxs-lookup"><span data-stu-id="8d333-240">Tracking VM running on Azure</span></span>

<span data-ttu-id="8d333-241">Szolgáltató akkor szükség lehet a nyomon követheti a szoftvert futtató virtuális gépek számát, vagy ügynököket, amelyeket a virtuális gép egyediségi nyomon kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="8d333-241">As a service provider, you may require to track the number of VMs running your software or have agents that need to track uniqueness of the VM.</span></span> <span data-ttu-id="8d333-242">Kell helyeznie egy egyedi Azonosítót a virtuális gépek, használja a `vmId` példány metaadat-szolgáltatásának mezőjét.</span><span class="sxs-lookup"><span data-stu-id="8d333-242">To be able to get a unique ID for a VM, use the `vmId` field from Instance Metadata Service.</span></span>

<span data-ttu-id="8d333-243">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="8d333-243">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

<span data-ttu-id="8d333-244">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="8d333-244">**Response**</span></span>

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a><span data-ttu-id="8d333-245">Tárolók elhelyezését, adat-partíciók alapú tartalék/frissítési tartomány</span><span class="sxs-lookup"><span data-stu-id="8d333-245">Placement of containers, data-partitions based fault/update domain</span></span> 

<span data-ttu-id="8d333-246">Az egyes forgatókönyvek, más adatok replikák elhelyezését van elsődleges fontosságú.</span><span class="sxs-lookup"><span data-stu-id="8d333-246">For certain scenarios, placement of different data replicas is of prime importance.</span></span> <span data-ttu-id="8d333-247">Például [HDFS replika elhelyezésének](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) vagy tároló elhelyezési keresztül egy [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) , szükség lehet tudni, hogy a `platformFaultDomain` és `platformUpdateDomain` fut a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="8d333-247">For example, [HDFS replica placement](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) or container placement via an [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) may you require to know the `platformFaultDomain` and `platformUpdateDomain` the VM is running on.</span></span>
<span data-ttu-id="8d333-248">Ezek az adatok közvetlenül a példány metaadatok szolgáltatásával lekérdezheti.</span><span class="sxs-lookup"><span data-stu-id="8d333-248">You can query this data directly via the Instance Metadata Service.</span></span>

<span data-ttu-id="8d333-249">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="8d333-249">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

<span data-ttu-id="8d333-250">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="8d333-250">**Response**</span></span>

```
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a><span data-ttu-id="8d333-251">További információ a virtuális gép támogatási esetet során beolvasása</span><span class="sxs-lookup"><span data-stu-id="8d333-251">Getting more information about the VM during support case</span></span> 

<span data-ttu-id="8d333-252">Szolgáltató akkor jelenhet meg a támogatási hívás hol szeretné tudni, hogy a virtuális gép további információt.</span><span class="sxs-lookup"><span data-stu-id="8d333-252">As a service provider, you may get a support call where you would like to know more information about the VM.</span></span> <span data-ttu-id="8d333-253">Az ügyfél a számítási metaadatok megosztásához kérni a támogatási szakember, milyen típusú Azure virtuális gép tudnia alapvető információkat nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="8d333-253">Asking the customer to share the compute metadata can provide basic information for the support professional to know about the kind of VM on Azure.</span></span> 

<span data-ttu-id="8d333-254">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="8d333-254">**Request**</span></span>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

<span data-ttu-id="8d333-255">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="8d333-255">**Response**</span></span>

> [!NOTE] 
> <span data-ttu-id="8d333-256">A válasz egy JSON-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="8d333-256">The response is a JSON string.</span></span> <span data-ttu-id="8d333-257">A következő példa egy válasz pretty nyomtatott olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="8d333-257">The following example response is pretty-printed for readability.</span></span>

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

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-the-vm"></a><span data-ttu-id="8d333-258">Példák a virtuális Gépen belül különböző nyelveken használó metaadatok szolgáltatás hívása</span><span class="sxs-lookup"><span data-stu-id="8d333-258">Examples of calling metadata service using different languages inside the VM</span></span> 

<span data-ttu-id="8d333-259">Nyelv</span><span class="sxs-lookup"><span data-stu-id="8d333-259">Language</span></span> | <span data-ttu-id="8d333-260">Példa</span><span class="sxs-lookup"><span data-stu-id="8d333-260">Example</span></span> 
---------|----------------
<span data-ttu-id="8d333-261">Ruby</span><span class="sxs-lookup"><span data-stu-id="8d333-261">Ruby</span></span>     | <span data-ttu-id="8d333-262">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.RB</span><span class="sxs-lookup"><span data-stu-id="8d333-262">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb</span></span>
<span data-ttu-id="8d333-263">Nyissa meg a helyi hálózat</span><span class="sxs-lookup"><span data-stu-id="8d333-263">Go Lan</span></span>   | <span data-ttu-id="8d333-264">https://github.com/Microsoft/azureimds/BLOB/Master/imdssample.go</span><span class="sxs-lookup"><span data-stu-id="8d333-264">https://github.com/Microsoft/azureimds/blob/master/imdssample.go</span></span>            
<span data-ttu-id="8d333-265">python</span><span class="sxs-lookup"><span data-stu-id="8d333-265">python</span></span>   | <span data-ttu-id="8d333-266">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.PY</span><span class="sxs-lookup"><span data-stu-id="8d333-266">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py</span></span>
<span data-ttu-id="8d333-267">C++</span><span class="sxs-lookup"><span data-stu-id="8d333-267">C++</span></span>      | <span data-ttu-id="8d333-268">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample-Windows.cpp</span><span class="sxs-lookup"><span data-stu-id="8d333-268">https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp</span></span>
<span data-ttu-id="8d333-269">C#</span><span class="sxs-lookup"><span data-stu-id="8d333-269">C#</span></span>       | <span data-ttu-id="8d333-270">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.cs</span><span class="sxs-lookup"><span data-stu-id="8d333-270">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs</span></span>
<span data-ttu-id="8d333-271">Javascript</span><span class="sxs-lookup"><span data-stu-id="8d333-271">Javascript</span></span> | <span data-ttu-id="8d333-272">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.js</span><span class="sxs-lookup"><span data-stu-id="8d333-272">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js</span></span>
<span data-ttu-id="8d333-273">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8d333-273">Powershell</span></span> | <span data-ttu-id="8d333-274">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.ps1</span><span class="sxs-lookup"><span data-stu-id="8d333-274">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1</span></span>
<span data-ttu-id="8d333-275">Bash</span><span class="sxs-lookup"><span data-stu-id="8d333-275">Bash</span></span>       | <span data-ttu-id="8d333-276">https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.SH</span><span class="sxs-lookup"><span data-stu-id="8d333-276">https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh</span></span>
    

## <a name="faq"></a><span data-ttu-id="8d333-277">GYIK</span><span class="sxs-lookup"><span data-stu-id="8d333-277">FAQ</span></span>
1. <span data-ttu-id="8d333-278">A hiba azért kapom `400 Bad Request, Required metadata header not specified`.</span><span class="sxs-lookup"><span data-stu-id="8d333-278">I am getting the error `400 Bad Request, Required metadata header not specified`.</span></span> <span data-ttu-id="8d333-279">Ez mit jelent?</span><span class="sxs-lookup"><span data-stu-id="8d333-279">What does this mean?</span></span>
   * <span data-ttu-id="8d333-280">A példány metaadat-szolgáltatás a fejlécet igényel `Metadata: true` átadni a kérelemben.</span><span class="sxs-lookup"><span data-stu-id="8d333-280">The Instance Metadata Service requires the header `Metadata: true` to be passed in the request.</span></span> <span data-ttu-id="8d333-281">Ezt a fejlécet a REST-hívást benyújtása lehetővé teszi, hogy a példány metaadat-szolgáltatás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="8d333-281">Passing this header in the REST call allows access to the Instance Metadata Service.</span></span> 
2. <span data-ttu-id="8d333-282">Miért nem jelenik meg a virtuális gép számítási adatai?</span><span class="sxs-lookup"><span data-stu-id="8d333-282">Why am I not getting compute information for my VM?</span></span>
   * <span data-ttu-id="8d333-283">A példány metaadat-szolgáltatás jelenleg csak az Azure Resource Manager eszközzel létrehozott példányokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="8d333-283">Currently the Instance Metadata Service only supports instances created with Azure Resource Manager.</span></span> <span data-ttu-id="8d333-284">A jövőben azt előfordulhat, hogy támogassa az Cloud Service virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="8d333-284">In the future, we may add support for Cloud Service VMs.</span></span>
3. <span data-ttu-id="8d333-285">A virtuális gép Azure Resource Manager használatával létrehozott egy while vissza.</span><span class="sxs-lookup"><span data-stu-id="8d333-285">I created my Virtual Machine through Azure Resource Manager a while back.</span></span> <span data-ttu-id="8d333-286">Miért nem tudok lásd számítási metaadat-információ?</span><span class="sxs-lookup"><span data-stu-id="8d333-286">Why am I not see compute metadata information?</span></span>
   * <span data-ttu-id="8d333-287">A 2016. szeptember után létrehozott virtuális gépek hozzáadása egy [címke](../../azure-resource-manager/resource-group-using-tags.md) jelenítse meg a számítási metaadatok.</span><span class="sxs-lookup"><span data-stu-id="8d333-287">For any VMs created after Sep 2016, add a [Tag](../../azure-resource-manager/resource-group-using-tags.md) to start seeing compute metadata.</span></span> <span data-ttu-id="8d333-288">(Szeptember 2016 előtt létrehozott) régebbi virtuális gépek hozzáadása a virtuális gép metaadatait extensions vagy adatok lemezét.</span><span class="sxs-lookup"><span data-stu-id="8d333-288">For older VMs (created before Sep 2016), add/remove extensions or data disks to the VM to refresh metadata.</span></span>
4. <span data-ttu-id="8d333-289">Miért jelenik meg a hiba `500 Internal Server Error`?</span><span class="sxs-lookup"><span data-stu-id="8d333-289">Why am I getting the error `500 Internal Server Error`?</span></span>
   * <span data-ttu-id="8d333-290">Próbálja megismételni a kérést a rendszer ki exponenciális vissza alapján.</span><span class="sxs-lookup"><span data-stu-id="8d333-290">Please retry your request based on exponential back off system.</span></span> <span data-ttu-id="8d333-291">Ha a probléma továbbra is fennáll, forduljon az Azure támogatási.</span><span class="sxs-lookup"><span data-stu-id="8d333-291">If the issue persists contact  Azure support.</span></span>
5. <span data-ttu-id="8d333-292">Ha ugyanazt a további kérdéseit vagy megjegyzéseit?</span><span class="sxs-lookup"><span data-stu-id="8d333-292">Where do I share additional questions/comments?</span></span>
   * <span data-ttu-id="8d333-293">A Megjegyzések küldése a http://feedback.azure.com.</span><span class="sxs-lookup"><span data-stu-id="8d333-293">Send your comments on http://feedback.azure.com.</span></span>
7. <span data-ttu-id="8d333-294">Ez használható lenne a virtuálisgép-méretezési beállítása példányt?</span><span class="sxs-lookup"><span data-stu-id="8d333-294">Would this work for Virtual Machine Scale Set Instance?</span></span>
   * <span data-ttu-id="8d333-295">Igen metaadat-szolgáltatás esetén méretezési beállítása példányok érhető el.</span><span class="sxs-lookup"><span data-stu-id="8d333-295">Yes Metadata service is available for Scale Set Instances.</span></span> 
6. <span data-ttu-id="8d333-296">Hogyan kérhet támogatást a szolgáltatás?</span><span class="sxs-lookup"><span data-stu-id="8d333-296">How do I get support for the service?</span></span>
   * <span data-ttu-id="8d333-297">Segítségre van szüksége a szolgáltatáshoz, hozzon létre egy támogatási probléma Azure-portálon a virtuális gép, amelyen még nem kérhető le a metaadatokat válasz hosszú újrapróbálkozás után</span><span class="sxs-lookup"><span data-stu-id="8d333-297">To get support for the service, create a support issue in Azure portal for the VM where you are not able to get metadata response after long retries</span></span> 

   ![Példány metaadatok támogatása](./media/instance-metadata-service/InstanceMetadata-support.png)
    
## <a name="next-steps"></a><span data-ttu-id="8d333-299">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8d333-299">Next steps</span></span>

- <span data-ttu-id="8d333-300">További információ a [ütemezett események](scheduled-events.md) API **nyilvános előzetes verziójában** a példány metaadat-szolgáltatás által biztosított.</span><span class="sxs-lookup"><span data-stu-id="8d333-300">Learn more about the [Scheduled Events](scheduled-events.md) API **in public preview** provided by the Instance Metadata service.</span></span>
