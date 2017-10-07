---
title: "Service Fabric programozott skálázás aaaAzure |} Microsoft Docs"
description: "Egy bejövő vagy kimenő Azure Service Fabric-fürt méretezése programozott módon, a toocustom eseményindítók szerint"
services: service-fabric
documentationcenter: .net
author: mjrousos
manager: jonjung
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: mikerou
ms.openlocfilehash: a0c6499b1a143a173006248cf8a15380632637e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a>A Service Fabric-fürt méretezése programozott módon 

Az Azure Service Fabric-fürt méretezése alapjai dokumentációjában ismertetett a [fürtméretezés](./service-fabric-cluster-scale-up-down.md). Hogy a cikk ismerteti, hogyan Service Fabric-fürtök épülő virtuálisgép-méretezési csoportok és a manuális vagy automatikus skálázása szabályokkal is méretezhető. Ez a dokumentum speciális forgatókönyvekhez koordináló Azure skálázási műveletek programozási módszerek megvizsgálja. 

## <a name="reasons-for-programmatic-scaling"></a>Programozott méretezéshez okok
A sok esetben skálázás manuális vagy automatikus skálázása szabályok segítségével található jó megoldás. Más esetekben azonban nem feltétlenül hello jobbra igazítása. Lehetséges hátrányai toothese módszerek a következők:

- Manuális skálázás megköveteli a toolog és explicit módon műveletek skálázás kérelem. Ha a skálázási műveletek szükségesek gyakran vagy időpontban előre nem látható, ez a megközelítés nem lehet jó megoldás.
- Automatikus méretezése szabályok eltávolítása egy virtuálisgép-méretezési csoport egy példányát, ha azok nem automatikusan eltávolítása Tudásbázis az adott csomópont tartozó hello Service Fabric-fürt kivéve hello csomóponttípus ezüst vagy arany a tartóssági szint. Automatikus méretezése szabályok hello léptékű szint beállítása (és nem a Service Fabric szint hello) működik, mert automatikus méretezése szabályok is távolítsa el a Service Fabric-csomópont őket szabályosan leállítása nélkül. Goromba csomópont eltávolítása fog hagy "szellemrekord" a Service Fabric-csomópont állapota méretezési a műveletek után. Egy adott (vagy egy szolgáltatás) kell tooperiodically karbantartása hello Service Fabric-fürtöt az eltávolított csomópont állapota.
  - Vegye figyelembe, hogy az arany vagy ezüst tartóssági szint csomóponttípus automatikusan be lesz tisztítása csomópontokat eltávolítani.  
- Bár vannak [sok metrikák](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) automatikus méretezése szabályok által támogatott, akkor még mindig korlátozott. Ha a forgatókönyv nem vonatkozik a készletben lévő egyes metrika alapján méretezéshez, majd automatikus méretezése szabályok nem lehet jó választás.

E tényezők alapján, érdemes tooimplement több testreszabott automatikus méretezési modellek. 

## <a name="scaling-apis"></a>Méretezési API-k
Az Azure API-k léteznek, amelyek lehetővé teszik az alkalmazások tooprogrammatically együttműködnek a virtuálisgép-méretezési csoportok és a Service Fabric-fürtök. Meglévő automatikus méretezése lehetőségek nem működnek az adott esetben, ha ezen API-k legyen lehetséges tooimplement egyéni méretezési logika. 

A "Kezdőlap-végrehajtott" automatikus skálázás egyik módszer tooimplementing funkció tooadd van egy új állapotmentes szolgáltatások toohello Service Fabric application toomanage skálázás műveletek. Hello szolgáltatáson belül `RunAsync` módszer, eseményindítók készlete is megállapítja, hogy skálázás szükség (többek között a paraméterek maximális fürt például ellenőrzése és a méretezés cooldowns).   

virtuális gép méretezési készlet kapcsolati használt API hello (mindkét toocheck hello aktuális száma a virtuálisgép-példányok és toomodify azt) hello van [Folyékonyan beszél Azure felügyeleti számítási könyvtár](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/). hello Folyékonyan beszél számítási kódtár biztosít egy könnyen használható API-t használják a virtuálisgép-méretezési készlet.

hello Service Fabric-fürt magát, a toointeract használja [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).

Természetesen kód skálázás hello szolgáltatás toorun nem szükséges a hello fürt toobe méretezhető. Mindkét `IAzure` és `FabricClient` kapcsolódhatnak tootheir társított Azure-erőforrások távolról, így hello szolgáltatás skálázás könnyen lehet egy konzolalkalmazás vagy a Windows-szolgáltatás fut a külső hello Service Fabric-alkalmazás. 

## <a name="credential-management"></a>Hitelesítő adatok kezelése
Egy szolgáltatás toohandle skálázás rendszerekben az egyik kihívás az, hogy kell-e képes tooaccess virtuális gép méretezési készlet erőforrások egy interaktív bejelentkezési azonosító nélküli hello szolgáltatást. A Service Fabric hello elérése fürt esetén könnyen hello szolgáltatás skálázás módosítja a saját Service Fabric-alkalmazás, de a hitelesítő adatok szükséges tooaccess hello méretezési készlet. a toolog, használhatja a [egyszerű](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) hello létre [Azure CLI 2.0](https://github.com/azure/azure-cli).

Egy egyszerű hello lépésekkel hozhatja létre:

1. Jelentkezzen be az Azure parancssori felület toohello (`az login`) hozzáférési toohello virtuálisgép-méretezési rendelkező felhasználóként beállítása
2. Az egyszerű hello szolgáltatás létrehozása`az ad sp create-for-rbac`
    1. Jegyezze fel a hello appId (más néven "ügyfél-azonosító" máshol), a neve, a jelszót és a bérlői későbbi használatra.
    2. Konfigurálnia kell az előfizetési azonosító, amely tekinthetők meg`az account list`

hello Folyékonyan beszél számítási könyvtár jelentkezhetnek be ezeket a hitelesítő adatokat az alábbiak szerint (vegye figyelembe, hogy core Folyékonyan beszél Azure típusok hasonlóan `IAzure` a hello [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) csomag):

```C#
var credentials = new AzureCredentials(new ServicePrincipalLoginInformation {
                ClientId = AzureClientId,
                ClientSecret = 
                AzureClientKey }, AzureTenantId, AzureEnvironment.AzureGlobalCloud);
IAzure AzureClient = Azure.Authenticate(credentials).WithSubscription(AzureSubscriptionId);

if (AzureClient?.SubscriptionId == AzureSubscriptionId)
{
    ServiceEventSource.Current.ServiceMessage(Context, "Successfully logged into Azure");
}
else
{
    ServiceEventSource.Current.ServiceMessage(Context, "ERROR: Failed toologin tooAzure");
}
```

Miután bejelentkezett, méretezési készlet példányszáma lekérdezhetők, keresztül `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.

## <a name="scaling-out"></a>Méretezés
Folyékonyan beszél hello használata Azure számítási SDK, példányok felveheti toohello virtuálisgép-méretezési pár hívások - beállítása

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

Azt is megteheti virtuális gép méretezési készlet méretét is felügyelhetők a PowerShell-parancsmagokkal. [`Get-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmss)lekérheti hello virtuálisgép-méretezési készlet objektumot. hello aktuális kapacitás fogja tárolni hello `.sku.capacity` tulajdonság. Miután hello kapacitás toohello módosítása a keresett, állítsa be az Azure-ban hello virtuálisgép-méretezési frissíthető hello [ `Update-AzureRmVmss` ](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/update-azurermvmss) parancsot.

Ha a csomópont manuális hozzáadása egy méretezési példány hozzáadása egy új Service Fabric csomópont hello méretezési sablon tartalmazza az összes meg szükséges toostart kell bővítmények tooautomatically csatlakoztassa az új példányok toohello Service Fabric-fürt. 

## <a name="scaling-in"></a>A méretezés

A méretezés akkor hasonló tooscaling ki. hello tényleges virtuálisgép-méretezési csoport változtatások vannak gyakorlatilag hello azonos. De volt korábban bemutatott, a Service Fabric csak automatikusan törli a szükségtelenné az eltávolított csomópontokat a tartós arany vagy ezüst. Így az hello bronz-tartóssági méretezési a helyzet, a Service Fabric hello szükséges toointeract is a fürt tooshut hello csomópont toobe eltávolított le, majd a tooremove állapotában.

Hello csomópont Felkészülés leállítási magában foglalja a hello csomópont toobe keresése eltávolítása (hello legutóbb hozzáadott csomópont) és inaktiválása. Nem kezdőérték csomópontok újabb csomópontok található összehasonlításával `NodeInstanceId`. 

```C#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n => n.NodeInstanceId)
        .FirstOrDefault();
```

Vegye figyelembe, hogy *kezdőérték* csomópontok látszólag nem tooalways hajtsa végre a nagyobb példány azonosítók először eltávolításuk hello egyezmény.

Miután hello csomópont toobe eltávolítani megtalálható, kikapcsolható, és eltávolított használatával hello azonos `FabricClient` példány és hello `IAzure` példányának regisztrációját a korábbi.

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove hello node from hello Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up tooa timeout) for hello node toogracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Mint meg, a PowerShell-parancsmagok a módosítását a virtuálisgép-méretezési készlet kapacitása is használható itt egy parancsfájl-kezelési megoldás hatékonyabb, ha. Miután hello virtuálisgép-példányt eltávolítják, a Service Fabric-csomópont állapota lehet eltávolítani.

```C#
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="potential-drawbacks"></a>Lehetséges hátrányai

Ahogyan az kódrészleteket megelőző hello, biztosít a saját méretezési szolgáltatás létrehozása a hello legmagasabb szintű vezérlő és az alkalmazás keresztül testreszabhatóság miatt a skálázás viselkedését. Ez lehet hasznos, ha pontosan meghatározhatja hogyan vagy ha egy alkalmazás méretezi-e a bejövő vagy kimenő igénylő forgatókönyvek. Azonban ez a vezérlő tartalmaz a kompromisszummal jár, kód összetettségét. Ezzel a megközelítéssel, az azt jelenti, hogy kell-e kód, amely nem triviális skálázás tooown.

Hogyan meg kell megközelíti a Service Fabric skálázás attól függ, hogy a forgatókönyvéhez. Skálázás ritka, hello képességét tooadd, vagy távolítsa el a csomópontok manuálisan akkor elegendő. Az összetettebb forgatókönyveket automatikus méretezése szabályok és SDK-k hello képességét tooscale programozott módon kitettségének kínál az hatékony megoldások.

## <a name="next-steps"></a>Következő lépések

megkezdődött a saját automatikus skálázás logikát megvalósító tooget ismerkedésre hello fogalmakat és hasznos API-k a következő:

- [Manuális vagy automatikus skálázása szabályokkal skálázás](./service-fabric-cluster-scale-up-down.md)
- [A .NET-hez Folyékonyan beszél Azure kezelési kódtárakat](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (hasznos a Service Fabric-fürt alapul szolgáló virtuálisgép-méretezési csoportok való kommunikáció)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (hasznos a Service Fabric-fürt és csomópontjainak való kommunikáció)
