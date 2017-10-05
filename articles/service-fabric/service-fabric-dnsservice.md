---
title: "Az Azure Service Fabric DNS-szolgáltatás |} Microsoft Docs"
description: "A Service Fabric dns szolgáltatás mikroszolgáltatások létrehozására az a fürtben található felderítésének segítségével."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/27/2017
ms.author: msfussell
ms.openlocfilehash: 9871bc5aa4e74ab0faef401d67c4e9558eb5e14b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="dns-service-in-azure-service-fabric"></a><span data-ttu-id="59bbc-103">Az Azure Service Fabric DNS-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="59bbc-103">DNS Service in Azure Service Fabric</span></span>
<span data-ttu-id="59bbc-104">A DNS szolgáltatás nem egy választható-szolgáltatás, amely a fürt felderíteni a más szolgáltatásokkal, DNS protokoll használatával engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="59bbc-104">The DNS Service is an optional system service that you can enable in your cluster to discover other services using the DNS protocol.</span></span>

<span data-ttu-id="59bbc-105">Számos szolgáltatás, főleg indexelése szolgáltatások lehet egy meglévő URL-címet, és tudnak majd hárítsa el őket a szabványos DNS protokoll (ahelyett, hogy a szolgáltatás protokoll) használatával kívánatos, különösen a "növekedési és az eltolás mértékét megadó" forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="59bbc-105">Many services, especially containerized services, can have an existing URL name, and being able to resolve them using the standard DNS protocol (rather than the Naming Service protocol) is desirable, particularly in "lift and shift" scenarios.</span></span> <span data-ttu-id="59bbc-106">A DNS-szolgáltatás lehetővé teszi, hogy a DNS-nevek hozzárendelése egy szolgáltatásnevet, és ezért a végpont IP-címeket feloldani.</span><span class="sxs-lookup"><span data-stu-id="59bbc-106">The DNS service enables you to map DNS names to a service name and hence resolve endpoint IP addresses.</span></span> 

<span data-ttu-id="59bbc-107">A DNS-szolgáltatás DNS-nevek viszont szünteti meg a szolgáltatás a szolgáltatás végpontjának vissza szolgáltatásnevek rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="59bbc-107">The DNS service maps DNS names to service names, which in turn are resolved by the Naming Service to return the service endpoint.</span></span> <span data-ttu-id="59bbc-108">A szolgáltatás DNS-nevét a létrehozás időpontjában valósul meg.</span><span class="sxs-lookup"><span data-stu-id="59bbc-108">The DNS name for the service is provided at the time of creation.</span></span> 

![Szolgáltatás-végpontok][0]

## <a name="enabling-the-dns-service"></a><span data-ttu-id="59bbc-110">A DNS szolgáltatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="59bbc-110">Enabling the DNS service</span></span>
<span data-ttu-id="59bbc-111">Először lehetővé kell tenni a DNS-szolgáltatás a fürtön.</span><span class="sxs-lookup"><span data-stu-id="59bbc-111">First you need to enable the DNS service in your cluster.</span></span> <span data-ttu-id="59bbc-112">Szerezze be a sablon a fürt, amely számára telepíteni kívánja.</span><span class="sxs-lookup"><span data-stu-id="59bbc-112">Get the template for the cluster that you want to deploy.</span></span> <span data-ttu-id="59bbc-113">Használhatja a [mintasablon használatával](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) vagy Resource Manager-sablon létrehozása.</span><span class="sxs-lookup"><span data-stu-id="59bbc-113">You can either use the [sample templates](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype)  or create a Resource Manager template.</span></span> <span data-ttu-id="59bbc-114">Engedélyezheti a DNS-szolgáltatás a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="59bbc-114">You can enable the DNS service with the following steps:</span></span>

1. <span data-ttu-id="59bbc-115">Ellenőrizze, hogy a `apiversion` értékre van állítva `2017-07-01-preview` a a `Microsoft.ServiceFabric/clusters` erőforrás, és ha nem, frissítheti a következő kódrészletben látható:</span><span class="sxs-lookup"><span data-stu-id="59bbc-115">Check that the `apiversion` is set to `2017-07-01-preview` for the `Microsoft.ServiceFabric/clusters` resource, and if not, update it as shown in the following snippet:</span></span>

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. <span data-ttu-id="59bbc-116">Most lehetővé teszi a DNS-szolgáltatás a következő `addonFeatures` szakasz után a `fabricSettings` szakaszban, ahogy az a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="59bbc-116">Now enable the DNS service by adding the following `addonFeatures` section after the `fabricSettings` section as shown in the following snippet:</span></span> 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. <span data-ttu-id="59bbc-117">A fürt sablon előző módosításainak frissítése után alkalmazza őket, és lehetővé teszik a frissítés befejeződött.</span><span class="sxs-lookup"><span data-stu-id="59bbc-117">Once you have updated your cluster template with the preceding changes, apply them and let the upgrade complete.</span></span> <span data-ttu-id="59bbc-118">Művelet befejeződése után a DNS-rendszer szolgáltatás elindul a fürtön is nevezett `fabric:/System/DnsService` rendszer szolgáltatás szakaszban a Service Fabric explorer.</span><span class="sxs-lookup"><span data-stu-id="59bbc-118">Once complete, the DNS system service starts running in your cluster that is called `fabric:/System/DnsService` under system service section in the Service Fabric explorer.</span></span> 

<span data-ttu-id="59bbc-119">Azt is megteheti a DNS-szolgáltatás a portálon keresztül is engedélyezheti a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="59bbc-119">Alternatively, you can enable the DNS service through the portal at the time of cluster creation.</span></span> <span data-ttu-id="59bbc-120">A négyzet bejelölésével engedélyezheti a DNS-szolgáltatás `Include DNS service` a a `Cluster configuration` menü a következő képernyőfelvételen látható módon:</span><span class="sxs-lookup"><span data-stu-id="59bbc-120">The DNS service can be enabled by checking the box for `Include DNS service` in the `Cluster configuration` menu as shown in the following screenshot:</span></span>

![A portálon keresztül a DNS szolgáltatás engedélyezése][2]


## <a name="setting-the-dns-name-for-your-service"></a><span data-ttu-id="59bbc-122">A szolgáltatás DNS-nevének beállítása</span><span class="sxs-lookup"><span data-stu-id="59bbc-122">Setting the DNS name for your service</span></span>
<span data-ttu-id="59bbc-123">Miután a DNS-szolgáltatás fut a fürtön, beállíthat egy DNS-nevet a szolgáltatások deklaratív módon az alapértelmezett szolgáltatások vagy a `ApplicationManifest.xml` vagy Powershell-parancsok használatával.</span><span class="sxs-lookup"><span data-stu-id="59bbc-123">Once the DNS service is running in your cluster, you can set a DNS name for your services either declaratively for default services in the `ApplicationManifest.xml` or through Powershell commands.</span></span>

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a><span data-ttu-id="59bbc-124">A DNS-nevét, egy alapértelmezett szolgáltatás a ApplicationManifest.xml beállítását</span><span class="sxs-lookup"><span data-stu-id="59bbc-124">Setting the DNS name for a default service in the ApplicationManifest.xml</span></span>
<span data-ttu-id="59bbc-125">Nyissa meg a projektet a Visual Studio vagy a kedvenc szerkesztőt, és nyissa meg a `ApplicationManifest.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="59bbc-125">Open your project in Visual Studio, or your favorite editor, and open the `ApplicationManifest.xml` file.</span></span> <span data-ttu-id="59bbc-126">Nyissa meg az alapértelmezett szolgáltatások szakaszt, és minden egyes szolgáltatás hozzáadása a `ServiceDnsName` attribútum.</span><span class="sxs-lookup"><span data-stu-id="59bbc-126">Go to the default services section, and for each service add the `ServiceDnsName` attribute.</span></span> <span data-ttu-id="59bbc-127">A következő példa bemutatja, hogyan kell beállítani a szolgáltatás DNS-neve`service1.application1`</span><span class="sxs-lookup"><span data-stu-id="59bbc-127">The following example shows how to set the DNS name of the service to `service1.application1`</span></span>

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
<span data-ttu-id="59bbc-128">Ha az alkalmazás központi telepítése, a szolgáltatáspéldány a Service Fabric Explorer ebben az esetben a DNS-nevét jeleníti meg az alábbi ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="59bbc-128">Once the application is deployed, the service instance in the Service Fabric explorer shows the DNS name for this instance, as shown in the following figure:</span></span> 

![Szolgáltatás-végpontok][1]

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a><span data-ttu-id="59bbc-130">A DNS-nevét, a Powershell szolgáltatás beállítása</span><span class="sxs-lookup"><span data-stu-id="59bbc-130">Setting the DNS name for a service using Powershell</span></span>
<span data-ttu-id="59bbc-131">A szolgáltatás DNS-neve használatával létrehozásakor beállíthatja a `New-ServiceFabricService` Powershell.</span><span class="sxs-lookup"><span data-stu-id="59bbc-131">You can set the DNS name for a service when creating it using the `New-ServiceFabricService` Powershell.</span></span> <span data-ttu-id="59bbc-132">Az alábbi példa létrehoz egy új állapotmentes szolgáltatást a DNS-névvel`service1.application1`</span><span class="sxs-lookup"><span data-stu-id="59bbc-132">The following example creates a new stateless service with the DNS name `service1.application1`</span></span>

```powershell
    New-ServiceFabricService `
    -Stateless `
    -PartitionSchemeSingleton `
    -ApplicationName `fabric:/application1 `
    -ServiceName fabric:/application1/service1 `
    -ServiceTypeName Service1Type `
    -InstanceCount 1 `
    -ServiceDnsName service1.application1
```

## <a name="using-dns-in-your-services"></a><span data-ttu-id="59bbc-133">A szolgáltatások a DNS-sel</span><span class="sxs-lookup"><span data-stu-id="59bbc-133">Using DNS in your services</span></span>
<span data-ttu-id="59bbc-134">Ha egynél több szolgáltatást telepít, a többi szolgáltatás egy DNS-név használatával kommunikálni végpontok is megtalálhatja.</span><span class="sxs-lookup"><span data-stu-id="59bbc-134">If you deploy more than one service, you can find the endpoints of other services to communicate with  by using a DNS name.</span></span> <span data-ttu-id="59bbc-135">A DNS-szolgáltatás tulajdonság csak állapotmentes szolgáltatások vonatkozik, mert a DNS protokoll állapotalapú szolgáltatások nem tudnak kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="59bbc-135">The DNS service is only applicable to stateless services, since the DNS protocol cannot communicate with stateful services.</span></span> <span data-ttu-id="59bbc-136">Állapotalapú szolgáltatások esetén a http-hívásokban beépített fordított proxy használatával hívható meg egy adott szolgáltatáshoz partíció.</span><span class="sxs-lookup"><span data-stu-id="59bbc-136">For stateful services, you can use the built-in reverse proxy for http calls to call a particular service partition.</span></span>

<span data-ttu-id="59bbc-137">A következő kód bemutatja, hogyan hívhatja meg egy másik szolgáltatás, amely egyszerűen csak egy normál http hívás ahol meg kell adnia a port és bármilyen opcionális elérési utat az URL-cím részeként.</span><span class="sxs-lookup"><span data-stu-id="59bbc-137">The following code shows how to call another service, which is simply a regular http call where you provide the port and any optional path as part of the URL.</span></span>

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="59bbc-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59bbc-138">Next steps</span></span>
<span data-ttu-id="59bbc-139">További információ a fürtön belül a szolgáltatások közötti kommunikáció [csatlakozzon és szolgáltatásokkal kommunikálni](service-fabric-connect-and-communicate-with-services.md)</span><span class="sxs-lookup"><span data-stu-id="59bbc-139">Learn more about service communication within the cluster with  [connect and communicate with services](service-fabric-connect-and-communicate-with-services.md)</span></span>

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG
