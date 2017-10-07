---
title: "aaaResource Manager és klasszikus üzembe helyezési |} Microsoft Docs"
description: "Hello hello Resource Manager üzembe helyezési modellben és a klasszikus hello (vagy szolgáltatásfelügyelet) közötti különbségeket telepítési modell ismerteti."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 7ae0ffa3-c8da-4151-bdcc-8f4f69290fb4
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: tomfitz
ms.openlocfilehash: fbf1959991b100547a459bf88a29c0afbc8592e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-hello-state-of-your-resources"></a>Az Azure Resource Manager és klasszikus üzembe helyezési: üzembe helyezési modellel megértéséhez, valamint az erőforrások állapotát hello
Ebben a témakörben megismerkedhet a Azure Resource Manager és klasszikus üzembe helyezési modellel, az erőforrások hello állapotát, és ezért az erőforrások közül egy vagy többi hello üzembe helyezése. hello Resource Manager és klasszikus üzembe helyezési modellek határoz meg két különböző módokat telepítése és kezelése az Azure megoldások. Különböző API kétféle módon működik velük, és hello telepített erőforrások tartalmazhat fontos különbség. hello két modell nincsenek teljesen kompatibilis egymással. Ez a témakör ismerteti azokat a különbségeket.

toosimplify hello telepítése és az erőforrások kezelése, a Microsoft azt javasolja, hogy minden új erőforrások erőforrás-kezelő használjon. Ha lehetséges a Microsoft azt javasolja, hogy a meglévő erőforrásokat Resource Manageren keresztül telepíteni.

Ha új tooResource kezelő, érdemes lehet a toofirst felülvizsgálati hello terminológia hello definiált [Azure Resource Manager áttekintése](resource-group-overview.md).

## <a name="history-of-hello-deployment-models"></a>Hello üzembe helyezési modellel előzményei
Azure eredetileg csak a hello klasszikus üzembe helyezési modellben megadott. Ebben a modellben minden erőforrás már létezett a egymástól függetlenül; nem lehetett toogroup kapcsolódó erőforrások együtt. Ehelyett toomanually nyomon követése áll a megoldás vagy az alkalmazás mely erőforrásokat, és ne feledje toomanage kellett a összehangolt őket. toodeploy megoldást, tooeither hozzon létre az egyes erőforrások külön-külön hello klasszikus portálon keresztül, vagy hozzon létre olyan parancsfájlt, amely minden hello erőforrások hello megfelelő sorrendben telepítve volt. toodelete megoldást, kellett toodelete az egyes erőforrások külön-külön. Akkor lehetett könnyen nem vonatkoznak, és kapcsolódó erőforrások hozzáférés-vezérlési házirendek frissítése. Végül akkor nem alkalmazható a következő címkék tooresources toolabel azokat a feltételeket, amelyek segítenek az erőforrások figyelése és kezelése.

A 2014-re az Azure erőforrás-kezelő, amely erőforráscsoport hello fogalma hozzá vezette be. Erőforráscsoport egy olyan tároló, amelyek egy közös életciklussal erőforrások. hello Resource Manager üzembe helyezési modellben számos előnyt kínál:

* Központi telepítése, kezelése és figyelése a megoldás összes hello szolgáltatást egy csoportot, hanem külön-külön kezelése ezeket a szolgáltatásokat.
* Ismételten a teljes életciklus megoldás üzembe helyezése, és lehet abban, hogy az erőforrások telepítése konzisztens lesz.
* Access control tooall erőforrások az erőforráscsoportban alkalmazhat, és ezek a házirendek automatikusan alkalmazzák, amikor új erőforrásokat ad toohello erőforráscsoport.
* Címkékkel láthatja tooresources toologically rendszerezése összes hello erőforrást az előfizetésében.
* A megoldás JavaScript Object Notation (JSON) toodefine hello infrastruktúra is használhatja. a Resource Manager sablonként ismert hello JSON-fájlt.
* Megadhatja, hogy hello függőségek között erőforrásokat, hogy azok hello megfelelő sorrendben legyenek telepítve.

Erőforrás-kezelő lett hozzáadva, ha az összes erőforrás visszamenőleges lett felvéve a toodefault erőforráscsoportok. Ha most a klasszikus üzembe helyezési keresztül egy erőforrás létrehozásához hello erőforrás automatikusan létrejön, hogy a szolgáltatás alapértelmezett erőforráscsoporton belül annak ellenére, hogy nem adta meg a központi telepítés erőforráscsoport. Azonban csak meglévő erőforráscsoporton belül nem jelenti azt, hogy a hello erőforrás megtörtént-e a konvertált toohello Resource Manager modellt. Megnézzük, hogyan kezeli az egyes szolgáltatások a hello két üzembe helyezési modellel hello a következő szakaszban. 

## <a name="understand-support-for-hello-models"></a>Hello modellek támogatása ismertetése
Amikor eldönti, milyen központi telepítési modell toouse erőforrások, számos három forgatókönyvek toobe tudomást.

1. hello szolgáltatás erőforrás-kezelő támogatja, és csak egyetlen típusra biztosítja.
2. hello szolgáltatás Resource Manager támogatja, de két típus - biztosít egy Resource Manager és klasszikus. Ez a forgatókönyv csak a toovirtual gépek, a storage-fiókok és a virtuális hálózatok vonatkozik.
3. hello szolgáltatás nem támogatja a Resource Manager.

toodiscover, hogy a szolgáltatás támogatja az erőforrás-kezelő, lásd: [erőforrás-szolgáltatók és típusok](resource-manager-supported-services.md).

Ha toouse kívánja hello szolgáltatás nem támogatja az erőforrás-kezelő, a klasszikus üzembe helyezési használatával kell folytatja.

Ha hello szolgáltatást támogatja az erőforrás-kezelő és **nem** egy virtuális gépet, a tárfiókhoz vagy a virtuális hálózati erőforrás-kezelő bármely komplikációk nélkül is használható.

A virtuális gépek, a storage-fiókok és a virtuális hálózatok klasszikus telepítési hello erőforrás hozták létre, ha továbbra is kell toooperate rajta klasszikus műveletek révén. Ha hello virtuális gépet, a storage-fiók vagy a virtuális hálózati erőforrás-kezelő központi jött létre, továbbra is kell erőforrás-kezelő műveletekkel. Ezt a különbséget zavaró kaphat, ha az előfizetése tartalmazza majd a Resource Manager és klasszikus üzembe helyezési létrejött erőforrásokat kombinációját. Ez a kombináció erőforrások nem várt eredményeket hozhat létre, mivel nem támogatják a hello erőforrások hello ugyanazokat a műveleteket.

Bizonyos esetekben egy erőforrás-kezelő parancs kérheti le a klasszikus üzembe helyezési létre erőforrásra vonatkozó adatokat, vagy egy hagyományos erőforrás tooanother erőforráscsoport áthelyezésével felügyeleti feladatot hajthat végre. Azonban ezekben az esetekben hello benyomást, hogy hello típus támogatja-e az erőforrás-kezelő műveletek számára. Tegyük fel például, a klasszikus üzembe helyezési hoztak létre virtuális gépet tartalmazó erőforráscsoport. Ha futtatja a Resource Manager PowerShell-parancsot a következő hello:

```powershell
Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines
```

Virtuális gép hello adja vissza:

```powershell
Name              : ExampleClassicVM
ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
ResourceName      : ExampleClassicVM
ResourceType      : Microsoft.ClassicCompute/virtualMachines
ResourceGroupName : ExampleGroup
Location          : westus
SubscriptionId    : {guid}
```

Azonban az erőforrás-kezelő parancsmag hello **Get-AzureRmVM** csak a Resource Manager használatával telepített virtuális gépek adja vissza. hello következő parancs nem ad vissza a klasszikus üzembe helyezési létrehozott hello virtuális géphez.

```powershell
Get-AzureRmVM -ResourceGroupName ExampleGroup
```

Csak erőforrások erőforrás-kezelő támogatási címkék segítségével létrehozott. Címkék tooclassic erőforrások nem alkalmazható.

## <a name="resource-manager-characteristics"></a>Erőforrás-kezelő jellemzői
két hello tisztában toohelp modellek, tekintsük át az erőforrás-kezelő típusok hello jellemzői:

* Hello segítségével létrehozott [Azure-portálon](https://portal.azure.com/).
  
     ![Azure Portal](./media/resource-manager-deployment-model/portal.png)
  
     A számítási, tárolási és hálózati erőforrások lehetősége van hello erőforrás-kezelő vagy a klasszikus telepítési használatával. Válassza ki **erőforrás-kezelő**.
  
     ![Erőforrás-kezelő telepítése](./media/resource-manager-deployment-model/select-resource-manager.png)
* Az Azure PowerShell-parancsmagok hello hello erőforrás-kezelő verziójával létrehozott. Ezek a parancsok hello formátumuk *ige-AzureRmNoun*.

  ```powershell
  New-AzureRmResourceGroupDeployment
  ```

* Hello segítségével létrehozott [Azure Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) REST-műveletek.
* Hello futtatható Azure parancssori felület parancsait létre **arm** mód.
  
  ```azurecli
  azure config mode arm
  azure group deployment create
  ```

* hello erőforrástípus nem tartalmaz **(klasszikus)** hello nevében. hello következő kép bemutatja hello típusú **tárfiók**.
  
    ![webalkalmazásra](./media/resource-manager-deployment-model/resource-manager-type.png)

## <a name="classic-deployment-characteristics"></a>Klasszikus üzembe helyezési jellemzői
Előfordulhat, hogy ismernie is hello klasszikus üzembe helyezési modellel hello szolgáltatás felügyeleti modell.

Erőforrások létrehozása hello klasszikus telepítési modell megosztás hello a következő jellemzőkkel:

* Hello segítségével létrehozott [klasszikus portál](https://manage.windowsazure.com)
  
     ![klasszikus portál](./media/resource-manager-deployment-model/classic-portal.png)
  
     Vagy, hello Azure-portálon, és adja meg, hogy **klasszikus** központi telepítését (számítási, tárolási és hálózati).
  
     ![Klasszikus üzembe helyezési](./media/resource-manager-deployment-model/select-classic.png)
* Hello szolgáltatásfelügyelet verziója hello Azure PowerShell-parancsmagok használatával létre. A parancs nevek hello formátumuk *ige-AzureNoun*.

  ```powershell
  New-AzureVM
  ```

* Hello segítségével létrehozott [szolgáltatásfelügyelet REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) REST-műveletek.
* Futtassa Azure parancssori felület parancsait létre **asm** mód.

  ```azurecli
  azure config mode asm
  azure vm create
  ```
   
* hello erőforrástípus tartalmaz **(klasszikus)** hello nevében. hello következő kép bemutatja hello típusú **(klasszikus) tárfiókot**.
  
    ![klasszikus típusa](./media/resource-manager-deployment-model/classic-type.png)

Használhatja a hello Azure portál toomanage erőforrásokat, amelyek klasszikus üzembe helyezési keresztül lettek létrehozva.

## <a name="changes-for-compute-network-and-storage"></a>A számítási, hálózati és tárolási változások
hello alábbi ábrán látható, számítási, hálózati és adattárolási erőforrásokat a Resource Manager használatával telepített.

![Erőforrás-kezelői architektúra](./media/resource-manager-deployment-model/arm_arch3.png)

Megjegyzés: hello hello erőforrások közötti kapcsolatok követően:

* Minden hello erőforrás létezik erőforráscsoporton belül.
* hello virtuális gép attól függ, hogy a megadott tárfiók hello Storage erőforrás-szolgáltató toostore definiált annak lemezeit a blob storage (kötelező).
* hello virtuális gép hivatkozik egy adott hálózati adapter hello hálózati erőforrás-szolgáltató (kötelező) és a rendelkezésre állási készlet hello számítási erőforrás-szolgáltató az (nem kötelező).
* hello NIC hivatkozások hello virtuális gép IP-címmel (kötelező), hello alhálózati hello virtuális hálózat hello virtuális gép (kötelező), és tooa hálózati biztonsági csoport (nem kötelező).
* a virtuális hálózaton belül hello alhálózat egy hálózati biztonsági csoportot (nem kötelező) hivatkozik.
* hello load balancer példány hello háttérbeli IP-címkészletet, amely tartalmazza az hello hálózati adapter a virtuális gép (nem kötelező) hivatkozik, és a betöltés terheléselosztó nyilvános vagy privát IP-cím (nem kötelező) hivatkozik.

Az alábbiakban hello összetevőket, és azok a klasszikus üzembe helyezési:

![klasszikus architektúrája](./media/resource-manager-deployment-model/arm_arch1.png)

hello klasszikus megoldás a virtuális gépek üzemeltetéséhez tartalmaz:

* Egy szükséges felhőalapú szolgáltatás, amely a virtuális gépeket (számítást) tárolója. Virtuális gépek automatikusan kapnak egy hálózati kártya (NIC), és az Azure által kiosztott IP-címet. Emellett hello felhőszolgáltatás tartalmazza-e a külső terheléselosztási terheléselosztó példány, egy nyilvános IP-cím és alapértelmezett végpontok tooallow távoli és távoli asztal PowerShell Windows-alapú virtuális gépek és a Secure Shell (SSH) forgalomnak a Linux-alapú virtuális gépek.
* A tárolók, hogy virtuális merevlemezek egy virtuális gép, beleértve a hello operációs rendszer, ideiglenes, és további adatlemezt (tárolás) hello szükséges storage-fiók.
* Egy nem kötelező, úgy működik, mint egy további tárolót, amelyben alhálózati struktúra létrehozása és kijelölése hello alhálózat mely hello a virtuális gép is található virtuális hálózat (hálózati).

hello a következő táblázat ismerteti, hogyan működnek együtt a számítási, hálózati és Tárolóerőforrás-szolgáltatók változásai:

| Elem | Klasszikus | Resource Manager |
| --- | --- | --- |
| Felhőszolgáltatás a virtuális gépekhez |A felhőalapú szolgáltatás hello platform és a terheléselosztás rendelkezésre állását is igényelte hello virtuális gépek egy tároló volt. |A felhőalapú szolgáltatás már nem szükséges a virtuális gépek hello új modelljének létrehozását objektum. |
| Virtuális hálózatok |Virtuális hálózat nem kötelező hello virtuális géphez. Ha tartalmazza, hello virtuális hálózat nem állítható rendszerbe a Resource Manager. |A virtuális gépeknek a Resource Manager központilag telepített virtuális hálózat. |
| Tárfiókok |hello a virtuális gépeknek, amelyek hello operációs rendszerhez, ideiglenes, és további adatlemezt hello VHD-k tárolja. |hello virtuális gép igényel a tárolási fiók toostore annak lemezeit a blob Storage tárolóban. |
| Rendelkezésre állási csoportok |Úgy konfigurálja a rendelkezésre állási toohello platform jelzett azonos "AvailabilitySetName" a virtuális gépek hello hello. hello tartalék tartományok maximális száma 2 volt. |A Rendelkezésre állási csoport egy Microsoft.Compute szolgáltató által közzétett erőforrás. Magas rendelkezésre állást igénylő virtuális gépek rendelkezésre állási csoport hello kell szerepelnie. hello tartalék tartományok maximális száma mostantól 3. |
| Affinitáscsoportok |Virtuális hálózatok létrehozásához szükség volt Affinitáscsoportokra. Azonban a regionális virtuális hálózatokba hello bevezetése, amely már nem volt szükség. |toosimplify, hello Affinitáscsoportok koncepciója hello Azure Resource Manageren keresztül közzétett API-kban nem létezik. |
| Terheléselosztás |Egy felhőalapú szolgáltatás létrehozása egy implicit terheléselosztót biztosít hello telepített virtuális gépeket. |hello Load Balancer egy olyan hello Microsoft.Network szolgáltató által közzétett erőforrás. hello toobe terhelésű igénylő virtuális gépek elsődleges hálózati adapterének hello a hello terheléselosztó kell hivatkozik. Egy terheléselosztó lehet külső vagy belső. A terhelés terheléselosztó példánya hello háttérbeli IP-címkészletet, amely tartalmazza az hello hálózati adapter a virtuális gép (nem kötelező) hivatkozik, és a betöltés terheléselosztó nyilvános vagy privát IP-cím (nem kötelező) hivatkozik. [További információk.](../virtual-network/resource-groups-networking.md) |
| Virtuális IP-cím |Cloud Services egy alapértelmezett VIP-t (virtuális IP-cím) jelenik meg, ha egy virtuális Gépet hozzáadnak tooa felhőalapú szolgáltatás. Virtuális IP-cím hello az hello implicit terheléselosztóhoz társított hello cím. |Nyilvános IP-cím hello Microsoft.Network szolgáltató által közzétett erőforrás. Egy nyilvános IP-cím lehet Statikus (Fenntartott) vagy Dinamikus. Dinamikus nyilvános IP-címek tooa terheléselosztóhoz rendelhetők hozzá. A nyilvános IP-címek védelme biztonsági csoportok segítségével biztosítható. |
| Fenntartott IP-címek |Azure-ban és azt egy felhőalapú szolgáltatás tooensure, amely hello IP-cím állandóságát társítható IP-cím foglalhat. |Nyilvános IP-cím létrehozhatók "Statikus" módban, és azt ajánlatok hello egy "fenntartott IP-címet" funkció. Statikus nyilvános IP-címek csak hozzárendelhető tooa terheléselosztó most. |
| Virtuális gépenként megadott nyilvános IP-cím (PIP) |Nyilvános IP-címeket is lehet társítva tooa virtuális gép közvetlenül. |Nyilvános IP-cím hello Microsoft.Network szolgáltató által közzétett erőforrás. Egy nyilvános IP-cím lehet Statikus (Fenntartott) vagy Dinamikus. Azonban csak dinamikus nyilvános IP-címek nem hozzárendelt tooa hálózati illesztő tooget nyilvános IP-cím, virtuális gépenként most. |
| Végpontok |Egy virtuális gép toobe konfigurált bemeneti végpontok szükséges toobe kapcsolat bizonyos portok megnyitása. Az egyik hello legelterjedtebb módja a bemeneti végpontok beállítása toovirtual gépekhez csatlakozó. |Bejövő NAT-szabályok konfigurálhatók a Terheléselosztókon tooachieve hello azonos képességek toohello virtuális gépek csatlakozni adott portok végpontok engedélyezésére. |
| DNS-név |Egy felhőszolgáltatás egy implicit globálisan egyedi DNS-nevet kap. Például: `mycoffeeshop.cloudapp.net`. |A DNS-nevek opcionális paraméterek, amelyek egy nyilvános IP-cím erőforráson adhatók meg. hello FQDN-je hello alábbi-formátum – `<domainlabel>.<region>.cloudapp.azure.com`. |
| Hálózati illesztők |Az elsődleges és másodlagos hálózati adapter és tulajdonságai egy virtuális gép hálózati konfigurációjaként voltak megadva. |A hálózati adapter egy Microsoft.Network szolgáltató által közzétett erőforrás. hello életciklusát hello hálózati adapter nincs virtuális gép tooa kötődik. Hello virtuális géphez hozzárendelt IP-cím (kötelező), hello alhálózat virtuális hálózat hello hello virtuális gép (kötelező), és tooa hálózati biztonsági csoport (nem kötelező) hivatkozik. |

toolearn különböző üzembe helyezési modellel, a virtuális hálózatok csatlakoztatása kapcsolatban lásd: [csatlakoztatja a virtuális hálózatok a különböző üzembe helyezési modellel hello portálon](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-tooresource-manager"></a>Klasszikus tooResource Manager áttelepítése
Ha készen áll a toomigrate áll az erőforrások a klasszikus üzembe helyezési tooResource Manager telepítése, lásd:

1. [Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő](../virtual-machines/windows/migration-classic-resource-manager-deep-dive.md)
2. [IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének támogatott platform](../virtual-machines/windows/migration-classic-resource-manager-overview.md)
3. [IaaS-erőforrásokra át klasszikus tooAzure erőforrás-kezelő Azure PowerShell használatával](../virtual-machines/windows/migration-classic-resource-manager-ps.md)
4. [IaaS-erőforrásokra át klasszikus tooAzure erőforrás-kezelő Azure parancssori felület használatával](../virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Gyakori kérdések
**A virtuális gépek Azure Resource Manager toodeploy klasszikus üzembe helyezési használatával létrehozott virtuális hálózatban hozhat létre?**

Ez nem támogatott. Klasszikus üzembe helyezési használatával létrehozott virtuális hálózatba Azure Resource Manager toodeploy egy virtuális gép nem használható.

**Hozható létre a virtuális gépek hello Azure Resource Manager hello Azure szolgáltatásfelügyeleti API használatával létrehozott felhasználói rendszerképből?**

Ez nem támogatott. Azonban hello VHD-fájlok másolása a hello szolgáltatásfelügyeleti API használatával létrehozott tárfiókból, és vegye fel őket tooa új fiók létrehozása az Azure Resource Manageren keresztül.

**Mi az az előfizetésem hello kvótája hello gyakorolt?**

hello hello virtuális gépek, virtuális hálózatok és hello Azure Resource Manager segítségével létrehozott storage-fiókok kvótái nem azonosak a más kvótákat. Minden előfizetés lekérdezi a kvóták toocreate hello-erőforrások hello új API-k. További hello további kvótákról [Itt](../azure-subscription-service-limits.md).

**Használhatom továbbra toouse a virtuális gépeket, a virtuális hálózatok és a storage-fiókok hello Resource Manager API-k segítségével történő üzembe helyezéséhez automatizált parancsfájlokat?**

Minden hello automatizálási és parancsfájlok, már létrehozott továbbra is toowork hello meglévő virtuális gépek, hello Azure szolgáltatásfelügyelet módban létrehozott virtuális hálózatokat. Azonban hello parancsfájlok sémája toobe frissített toouse hello új hello hello Resource Manager módra ugyanazon erőforrásoknak létrehozásához.

**Hol találhatok példákat az Azure Resource Manager-sablonok?**

Széles választékát kezdősablont található [Azure Resource Manager gyorsindítási sablonok](https://azure.microsoft.com/documentation/templates/).

## <a name="next-steps"></a>Következő lépések
* a sablont, amely meghatározza egy virtuális gépet, a tárfiók és a virtuális hálózat, lásd: hello létrehozása toowalk [Resource Manager sablonokhoz](resource-manager-template-walkthrough.md).
* a sablonok telepítésével toosee hello parancsok lásd [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).

