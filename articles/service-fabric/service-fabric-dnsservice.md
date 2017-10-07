---
title: "Service Fabric DNS-szolgáltatás aaaAzure |} Microsoft Docs"
description: "Használja a Service Fabric DNS-szolgáltatás felderítéséhez a mikroszolgáltatások hello fürtben található."
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
ms.openlocfilehash: fa536f0e41f52c4942702d0a1bdcd3ed7d418d6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dns-service-in-azure-service-fabric"></a><span data-ttu-id="402ba-103">Az Azure Service Fabric DNS-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="402ba-103">DNS Service in Azure Service Fabric</span></span>
<span data-ttu-id="402ba-104">hello DNS-szolgáltatás egy opcionális-szolgáltatás, amely a fürt toodiscover engedélyezheti az egyéb szolgáltatások hello DNS protokoll használatát.</span><span class="sxs-lookup"><span data-stu-id="402ba-104">hello DNS Service is an optional system service that you can enable in your cluster toodiscover other services using hello DNS protocol.</span></span>

<span data-ttu-id="402ba-105">Számos szolgáltatás, főleg indexelése szolgáltatások lehet egy meglévő URL-címet, és képes tooresolve őket hello szabványos DNS protokoll (helyett hello szolgáltatás protokoll) használatával kívánatos, különösen a "növekedési és az eltolás mértékét megadó" forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="402ba-105">Many services, especially containerized services, can have an existing URL name, and being able tooresolve them using hello standard DNS protocol (rather than hello Naming Service protocol) is desirable, particularly in "lift and shift" scenarios.</span></span> <span data-ttu-id="402ba-106">hello DNS-szolgáltatás lehetővé teszi, hogy Ön toomap DNS-nevek tooa szolgáltatás neve, és ezért a végpont IP-címeket feloldani.</span><span class="sxs-lookup"><span data-stu-id="402ba-106">hello DNS service enables you toomap DNS names tooa service name and hence resolve endpoint IP addresses.</span></span> 

<span data-ttu-id="402ba-107">a maps pedig hello szolgáltatás tooreturn hello szolgáltatási végpont által megoldott DNS-nevek tooservice nevek hello DNS-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="402ba-107">hello DNS service maps DNS names tooservice names, which in turn are resolved by hello Naming Service tooreturn hello service endpoint.</span></span> <span data-ttu-id="402ba-108">hello DNS-név hello szolgáltatás létrehozása a hello időpontjában valósul meg.</span><span class="sxs-lookup"><span data-stu-id="402ba-108">hello DNS name for hello service is provided at hello time of creation.</span></span> 

![Szolgáltatás-végpontok][0]

## <a name="enabling-hello-dns-service"></a><span data-ttu-id="402ba-110">Hello DNS szolgáltatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="402ba-110">Enabling hello DNS service</span></span>
<span data-ttu-id="402ba-111">Először tooenable hello DNS-szolgáltatás szükséges a fürt.</span><span class="sxs-lookup"><span data-stu-id="402ba-111">First you need tooenable hello DNS service in your cluster.</span></span> <span data-ttu-id="402ba-112">Hello sablon lekérése hello fürt, amelyet az toodeploy.</span><span class="sxs-lookup"><span data-stu-id="402ba-112">Get hello template for hello cluster that you want toodeploy.</span></span> <span data-ttu-id="402ba-113">Vagy használjon hello is [mintasablon használatával](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) vagy Resource Manager-sablon létrehozása.</span><span class="sxs-lookup"><span data-stu-id="402ba-113">You can either use hello [sample templates](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype)  or create a Resource Manager template.</span></span> <span data-ttu-id="402ba-114">Hello DNS-szolgáltatás engedélyezéséhez az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="402ba-114">You can enable hello DNS service with hello following steps:</span></span>

1. <span data-ttu-id="402ba-115">Ellenőrizze, hogy hello `apiversion` értéke túl`2017-07-01-preview` a hello `Microsoft.ServiceFabric/clusters` erőforrás, és ha nem, frissítheti a következő kódrészletet hello látható:</span><span class="sxs-lookup"><span data-stu-id="402ba-115">Check that hello `apiversion` is set too`2017-07-01-preview` for hello `Microsoft.ServiceFabric/clusters` resource, and if not, update it as shown in hello following snippet:</span></span>

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. <span data-ttu-id="402ba-116">Most lehetővé teszi a hello DNS-szolgáltatás hello következő hozzáadásával `addonFeatures` szakasz után hello `fabricSettings` szakaszban, ahogy az alábbi részlet hello:</span><span class="sxs-lookup"><span data-stu-id="402ba-116">Now enable hello DNS service by adding hello following `addonFeatures` section after hello `fabricSettings` section as shown in hello following snippet:</span></span> 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. <span data-ttu-id="402ba-117">Miután a fürt sablon a módosítások megelőző hello frissít, alkalmazza azokat, és lehetővé teszik a frissítés befejeződött hello.</span><span class="sxs-lookup"><span data-stu-id="402ba-117">Once you have updated your cluster template with hello preceding changes, apply them and let hello upgrade complete.</span></span> <span data-ttu-id="402ba-118">Művelet befejeződése után hello DNS-rendszer szolgáltatás elindul a fürtön is nevezett `fabric:/System/DnsService` rendszer szolgáltatás szakaszban hello Service Fabric explorer.</span><span class="sxs-lookup"><span data-stu-id="402ba-118">Once complete, hello DNS system service starts running in your cluster that is called `fabric:/System/DnsService` under system service section in hello Service Fabric explorer.</span></span> 

<span data-ttu-id="402ba-119">Azt is megteheti a fürt létrehozása hello időpontjában hello DNS-szolgáltatás hello portálon keresztül is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="402ba-119">Alternatively, you can enable hello DNS service through hello portal at hello time of cluster creation.</span></span> <span data-ttu-id="402ba-120">hello DNS-szolgáltatás a hello négyzet bejelölésével engedélyezhető `Include DNS service` a hello `Cluster configuration` menü, ahogy az alábbi képernyőfelvétel a hello:</span><span class="sxs-lookup"><span data-stu-id="402ba-120">hello DNS service can be enabled by checking hello box for `Include DNS service` in hello `Cluster configuration` menu as shown in hello following screenshot:</span></span>

![Hello portálon keresztül a DNS szolgáltatás engedélyezése][2]


## <a name="setting-hello-dns-name-for-your-service"></a><span data-ttu-id="402ba-122">A szolgáltatás DNS-nevét hello beállítása</span><span class="sxs-lookup"><span data-stu-id="402ba-122">Setting hello DNS name for your service</span></span>
<span data-ttu-id="402ba-123">Ha hello DNS-szolgáltatás fut a fürtön, állíthatja be a szolgáltatás egy DNS-nevet vagy deklaratív módon az alapértelmezett szolgáltatások hello `ApplicationManifest.xml` vagy Powershell-parancsok használatával.</span><span class="sxs-lookup"><span data-stu-id="402ba-123">Once hello DNS service is running in your cluster, you can set a DNS name for your services either declaratively for default services in hello `ApplicationManifest.xml` or through Powershell commands.</span></span>

### <a name="setting-hello-dns-name-for-a-default-service-in-hello-applicationmanifestxml"></a><span data-ttu-id="402ba-124">Hello ApplicationManifest.xml hello DNS-név egy alapértelmezett szolgáltatás beállítása</span><span class="sxs-lookup"><span data-stu-id="402ba-124">Setting hello DNS name for a default service in hello ApplicationManifest.xml</span></span>
<span data-ttu-id="402ba-125">Nyissa meg a projektet a Visual Studio vagy a kedvenc szerkesztőt, és nyissa meg a hello `ApplicationManifest.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="402ba-125">Open your project in Visual Studio, or your favorite editor, and open hello `ApplicationManifest.xml` file.</span></span> <span data-ttu-id="402ba-126">Nyissa meg toohello alapértelmezett szolgáltatások szakaszt, és minden egyes szolgáltatás Hozzáadás hello `ServiceDnsName` attribútum.</span><span class="sxs-lookup"><span data-stu-id="402ba-126">Go toohello default services section, and for each service add hello `ServiceDnsName` attribute.</span></span> <span data-ttu-id="402ba-127">hello a következő példa bemutatja, hogyan a tooset hello hello szolgáltatás DNS-neve túl`service1.application1`</span><span class="sxs-lookup"><span data-stu-id="402ba-127">hello following example shows how tooset hello DNS name of hello service too`service1.application1`</span></span>

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
<span data-ttu-id="402ba-128">Hello alkalmazás lett telepítve, ha a Service Fabric explorer hello hello szolgáltatáspéldány hello DNS-neve az adott példány látható, a hello a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="402ba-128">Once hello application is deployed, hello service instance in hello Service Fabric explorer shows hello DNS name for this instance, as shown in hello following figure:</span></span> 

![Szolgáltatás-végpontok][1]

### <a name="setting-hello-dns-name-for-a-service-using-powershell"></a><span data-ttu-id="402ba-130">DNS-nevét hello Powershell szolgáltatás beállítása</span><span class="sxs-lookup"><span data-stu-id="402ba-130">Setting hello DNS name for a service using Powershell</span></span>
<span data-ttu-id="402ba-131">Beállíthatja a hello DNS-név szolgáltatás hello segítségével létrehozásakor `New-ServiceFabricService` Powershell.</span><span class="sxs-lookup"><span data-stu-id="402ba-131">You can set hello DNS name for a service when creating it using hello `New-ServiceFabricService` Powershell.</span></span> <span data-ttu-id="402ba-132">hello alábbi példa létrehoz egy új állapotmentes szolgáltatások hello DNS-név`service1.application1`</span><span class="sxs-lookup"><span data-stu-id="402ba-132">hello following example creates a new stateless service with hello DNS name `service1.application1`</span></span>

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

## <a name="using-dns-in-your-services"></a><span data-ttu-id="402ba-133">A szolgáltatások a DNS-sel</span><span class="sxs-lookup"><span data-stu-id="402ba-133">Using DNS in your services</span></span>
<span data-ttu-id="402ba-134">Ha egynél több szolgáltatást telepít, az egyéb szolgáltatások toocommunicate hello végpontjai található egy DNS-név használatával.</span><span class="sxs-lookup"><span data-stu-id="402ba-134">If you deploy more than one service, you can find hello endpoints of other services toocommunicate with  by using a DNS name.</span></span> <span data-ttu-id="402ba-135">hello DNS-szolgáltatás is csak megfelelő toostateless szolgáltatások, mivel hello DNS protokoll állapotalapú szolgáltatások nem tudnak kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="402ba-135">hello DNS service is only applicable toostateless services, since hello DNS protocol cannot communicate with stateful services.</span></span> <span data-ttu-id="402ba-136">Állapotalapú szolgáltatások esetén használható hello beépített fordított proxy http hívások toocall egy adott szolgáltatáshoz partíció.</span><span class="sxs-lookup"><span data-stu-id="402ba-136">For stateful services, you can use hello built-in reverse proxy for http calls toocall a particular service partition.</span></span>

<span data-ttu-id="402ba-137">hello következő kód bemutatja, hogyan toocall egy másik szolgáltatás, amely egyszerűen csak egy normál http hívás ahol meg kell adnia hello port és a választható elérési hello URL-cím részeként.</span><span class="sxs-lookup"><span data-stu-id="402ba-137">hello following code shows how toocall another service, which is simply a regular http call where you provide hello port and any optional path as part of hello URL.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="402ba-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="402ba-138">Next steps</span></span>
<span data-ttu-id="402ba-139">További információk a szolgáltatások közötti kommunikáció hello fürtön belül [csatlakozzon és szolgáltatásokkal kommunikálni](service-fabric-connect-and-communicate-with-services.md)</span><span class="sxs-lookup"><span data-stu-id="402ba-139">Learn more about service communication within hello cluster with  [connect and communicate with services](service-fabric-connect-and-communicate-with-services.md)</span></span>

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG
