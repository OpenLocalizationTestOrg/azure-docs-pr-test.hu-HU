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
# <a name="dns-service-in-azure-service-fabric"></a>Az Azure Service Fabric DNS-szolgáltatás
hello DNS-szolgáltatás egy opcionális-szolgáltatás, amely a fürt toodiscover engedélyezheti az egyéb szolgáltatások hello DNS protokoll használatát.

Számos szolgáltatás, főleg indexelése szolgáltatások lehet egy meglévő URL-címet, és képes tooresolve őket hello szabványos DNS protokoll (helyett hello szolgáltatás protokoll) használatával kívánatos, különösen a "növekedési és az eltolás mértékét megadó" forgatókönyvek. hello DNS-szolgáltatás lehetővé teszi, hogy Ön toomap DNS-nevek tooa szolgáltatás neve, és ezért a végpont IP-címeket feloldani. 

a maps pedig hello szolgáltatás tooreturn hello szolgáltatási végpont által megoldott DNS-nevek tooservice nevek hello DNS-szolgáltatás. hello DNS-név hello szolgáltatás létrehozása a hello időpontjában valósul meg. 

![Szolgáltatás-végpontok][0]

## <a name="enabling-hello-dns-service"></a>Hello DNS szolgáltatás engedélyezése
Először tooenable hello DNS-szolgáltatás szükséges a fürt. Hello sablon lekérése hello fürt, amelyet az toodeploy. Vagy használjon hello is [mintasablon használatával](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) vagy Resource Manager-sablon létrehozása. Hello DNS-szolgáltatás engedélyezéséhez az alábbi lépésekkel hello:

1. Ellenőrizze, hogy hello `apiversion` értéke túl`2017-07-01-preview` a hello `Microsoft.ServiceFabric/clusters` erőforrás, és ha nem, frissítheti a következő kódrészletet hello látható:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Most lehetővé teszi a hello DNS-szolgáltatás hello következő hozzáadásával `addonFeatures` szakasz után hello `fabricSettings` szakaszban, ahogy az alábbi részlet hello: 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. Miután a fürt sablon a módosítások megelőző hello frissít, alkalmazza azokat, és lehetővé teszik a frissítés befejeződött hello. Művelet befejeződése után hello DNS-rendszer szolgáltatás elindul a fürtön is nevezett `fabric:/System/DnsService` rendszer szolgáltatás szakaszban hello Service Fabric explorer. 

Azt is megteheti a fürt létrehozása hello időpontjában hello DNS-szolgáltatás hello portálon keresztül is engedélyezheti. hello DNS-szolgáltatás a hello négyzet bejelölésével engedélyezhető `Include DNS service` a hello `Cluster configuration` menü, ahogy az alábbi képernyőfelvétel a hello:

![Hello portálon keresztül a DNS szolgáltatás engedélyezése][2]


## <a name="setting-hello-dns-name-for-your-service"></a>A szolgáltatás DNS-nevét hello beállítása
Ha hello DNS-szolgáltatás fut a fürtön, állíthatja be a szolgáltatás egy DNS-nevet vagy deklaratív módon az alapértelmezett szolgáltatások hello `ApplicationManifest.xml` vagy Powershell-parancsok használatával.

### <a name="setting-hello-dns-name-for-a-default-service-in-hello-applicationmanifestxml"></a>Hello ApplicationManifest.xml hello DNS-név egy alapértelmezett szolgáltatás beállítása
Nyissa meg a projektet a Visual Studio vagy a kedvenc szerkesztőt, és nyissa meg a hello `ApplicationManifest.xml` fájlt. Nyissa meg toohello alapértelmezett szolgáltatások szakaszt, és minden egyes szolgáltatás Hozzáadás hello `ServiceDnsName` attribútum. hello a következő példa bemutatja, hogyan a tooset hello hello szolgáltatás DNS-neve túl`service1.application1`

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
Hello alkalmazás lett telepítve, ha a Service Fabric explorer hello hello szolgáltatáspéldány hello DNS-neve az adott példány látható, a hello a következő ábrán látható módon: 

![Szolgáltatás-végpontok][1]

### <a name="setting-hello-dns-name-for-a-service-using-powershell"></a>DNS-nevét hello Powershell szolgáltatás beállítása
Beállíthatja a hello DNS-név szolgáltatás hello segítségével létrehozásakor `New-ServiceFabricService` Powershell. hello alábbi példa létrehoz egy új állapotmentes szolgáltatások hello DNS-név`service1.application1`

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

## <a name="using-dns-in-your-services"></a>A szolgáltatások a DNS-sel
Ha egynél több szolgáltatást telepít, az egyéb szolgáltatások toocommunicate hello végpontjai található egy DNS-név használatával. hello DNS-szolgáltatás is csak megfelelő toostateless szolgáltatások, mivel hello DNS protokoll állapotalapú szolgáltatások nem tudnak kommunikálni. Állapotalapú szolgáltatások esetén használható hello beépített fordított proxy http hívások toocall egy adott szolgáltatáshoz partíció.

hello következő kód bemutatja, hogyan toocall egy másik szolgáltatás, amely egyszerűen csak egy normál http hívás ahol meg kell adnia hello port és a választható elérési hello URL-cím részeként.

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

## <a name="next-steps"></a>Következő lépések
További információk a szolgáltatások közötti kommunikáció hello fürtön belül [csatlakozzon és szolgáltatásokkal kommunikálni](service-fabric-connect-and-communicate-with-services.md)

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG
