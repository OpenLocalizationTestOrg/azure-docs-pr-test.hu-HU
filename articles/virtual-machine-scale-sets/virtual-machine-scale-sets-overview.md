---
title: "aaaAzure virtulálisgép-skálázási készletekben áttekintése |} Microsoft Docs"
description: "További információk az Azure virtuálisgép-méretezési csoportokról"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 788b2d1636e0bf4ef3fbf94aed9b3303c5fafa82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Mik azok a virtuálisgép-méretezési csoportok az Azure-ban?
Virtuálisgép-méretezési csoportok olyan Azure számítási erőforrás toodeploy használja és az azonos virtuális gépek kezelésére is. A konfigurált összes virtuális gép hello azonos, méretezési csoportok tervezett toosupport igaz automatikus skálázás, és nincs előre kiépítése virtuális gépek szükség. Így könnyebben toobuild nagyméretű szolgáltatásokról, amelyek nagy számítási, a big Data típusú adatok és a tárolóalapú munkafolyamatok.

Tooscale számítási erőforrásokat, és a skála igénylő alkalmazásokhoz műveletek implicit módon hiba és a frissítési tartományok közötti elosztását. Beállítja egy további bemutatása tooscale, részletek toohello [Azure blog közlemény](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

A méretezési csoportokkal kapcsolatban további információkat tudhat meg az alábbi videókból:

* [Mark Russinovich talks Azure Scale Sets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/) (Mark Russinovich ismerteti az Azure-alapú méretezési csoportokat)  
* [Virtual Machine Scale Sets with Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman) (A virtuálisgép-méretezési csoportokról Guy Bowerman mesél)

## <a name="creating-and-managing-scale-sets"></a>Méretezési csoportok létrehozása és kezelése
Létrehozhat egy méretezési hello beállított [Azure-portálon](https://portal.azure.com) kiválasztásával **új** , és írja be **méretezési** a hello keresősáv. **Virtuálisgép-méretezési csoport** hello eredmények szerepel. Ott hello kötelező mezők toocustomize töltse ki, és telepítheti a méretezési készlet. Akkor is beállítások tooset alapvető automatikus skálázási szabályok hello portálon CPU-használat alapján.

A méretezési csoportok megadását és üzembe helyezését – az egyedi Azure Resource Manager-alapú virtuális gépekhez hasonlóan – JSON-sablonok és [REST API-k](https://msdn.microsoft.com/library/mt589023.aspx) segítségével is elvégezheti. Ezért lehetőség van bármilyen szabványos Azure Resource Manager-alapú üzembe helyezési módszer használatára. A sablonokról további információkat az [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) (Azure Resource Manager-sablonok készítése) című témakörben talál.

Egy példa sablont a virtuálisgép-méretezési készlet beállítja hello található [Azure gyors üzembe helyezési sablonokat GitHub-tárházban](https://github.com/Azure/azure-quickstart-templates). (Keresse meg a sablonok **vmss** hello címben.)

Hello gyors üzembe helyezés sablon példákat, a "Központi telepítés tooAzure" gombra, a hello fontos az egyes sablon hivatkozások toohello portál telepítési szolgáltatásokhoz. toodeploy hello méretezés beállítása, hello gombra, és töltse ki a paramétereket, amelyek szükségesek a hello portál. 

## <a name="scaling-a-scale-set-out-and-in"></a>Méretezési csoport horizontális fel- és leskálázása
Módosíthatja a skála hello Azure-portálon a beállításához kattintson a hello hello kapacitás **méretezés** szakaszában **beállítások**. 

toochange méretezési kapacitás hello parancssorban, használja a hello **méretezési** parancsot [Azure CLI](https://github.com/Azure/azure-cli). Ez a parancs tooset 10 virtuális gépek méretezési készlet tooa kapacitású például használja:

```bash
az vmss scale -g resourcegroupname -n scalesetname --new-capacity 10 
```

tooset hello virtuális gépek számát terjedő skálán beállítása PowerShell használatával, használja a hello **frissítés-AzureRmVmss** parancs:

```PowerShell
$vmss = Get-AzureRmVmss -ResourceGroupName resourcegroupname -VMScaleSetName scalesetname  
$vmss.Sku.Capacity = 10
Update-AzureRmVmss -ResourceGroupName resourcegroupname -Name scalesetname -VirtualMachineScaleSet $vmss
```

a skála lévő virtuális gépek tooincrease vagy csökken hello száma beállítása az Azure Resource Manager-sablon használatával, hello módosítása **kapacitás** tulajdonság, és helyezze üzembe újra hello sablont. Az egyszerűség kedvéért rendelkező Azure automatikus skálázás, vagy a saját egyéni méretezési réteg Ha adott Azure automatikus skálázás kell toodefine egyedi méretezés események nem támogatja a toowrite könnyen toointegrate-méretezési csoportok segítségével. 

Ha az Azure Resource Manager sablon toochange hello kapacitás ismételt üzembe helyezéssel, sokkal kisebb tartalmazó sablon csak hello meghatározhatja **SKU** hello csomagot tulajdonság frissítése kapacitás. [Például:](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing)

## <a name="autoscale"></a>Automatikus méretezés

A méretezési opcionálisan konfigurálható automatikus skálázási beállításokat az Azure-portálon hello létrehozásakor. virtuális gépek száma hello majd is növelhető vagy csökkenthető, átlagos CPU-használat alapján. 

Sablonok beállítása hello számos méretezése hello [Azure gyors üzembe helyezési sablonokat](https://github.com/Azure/azure-quickstart-templates) automatikus skálázási beállítás megadása. Automatikus skálázási beállításokat tooan méretezési meglévő is hozzáadhat. Az Azure PowerShell-parancsfájl például processzoralapú automatikus skálázás tooa méretezési csoport hozzáadása:

```PowerShell

$subid = "yoursubscriptionid"
$rgname = "yourresourcegroup"
$vmssname = "yourscalesetname"
$location = "yourlocation" # e.g. southcentralus

$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Increase -ScaleActionValue 1
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator LessThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Decrease -ScaleActionValue 1
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "autoprofile1"
Add-AzureRmAutoscaleSetting -Location $location -Name "autosetting1" -ResourceGroup $rgname -TargetResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -AutoscaleProfiles $profile1
```

Található érvényes metrikák tooscale listáját a [támogatott Azure-figyelő metrikák](../monitoring-and-diagnostics/monitoring-supported-metrics.md) hello "Microsoft.Compute/virtualMachineScaleSets." fejléc alatt Automatikus skálázás további beállítások is elérhetők, többek között ütemezés alapján automatikus skálázás és webhookokkal toointegrate használatával riasztási rendszerekkel.

## <a name="monitoring-your-scale-set"></a>A méretezési csoport figyelése
Hello [Azure-portálon](https://portal.azure.com) listák méretezés állítja be, és azok tulajdonságait mutatja. hello portál is támogatja a felügyeleti műveleteket. Végrehajthat felügyeleti műveleteket a méretezési csoportokon és méretezési csoportok egyes virtuális gépein is. hello portal emellett egy testreszabható erőforrás-használati diagramot. 

Ha toosee kell, és az alapul szolgáló egy Azure-erőforrás a JSON-definícióból hello szerkesztéséhez is használhatja [Azure erőforrás-kezelő](https://resources.azure.com). Méretezési csoportok hello Microsoft.Compute Azure erőforrás-szolgáltató az erőforrás. Erről a helyről a következő hivatkozások hello kibontásával megtekintheti őket:

**Előfizetések** > **saját előfizetés** > **resourceGroups** > **szolgáltatók** > **Microsoft.Compute** > **virtualMachineScaleSets** > **saját méretezési csoport** &gt; stb.

## <a name="scale-set-scenarios"></a>Méretezési csoportok használatának esetei
Ez a szakasz a méretezési csoportok használatának néhány tipikus esetét sorolja fel. Ezek az esetek néhány magasabb szintű Azure-szolgáltatás (például a Batch, a Service Fabric és a Container Service) használatakor merülhetnek fel.

* **RDP vagy SSH tooconnect tooscale beállítani példányok**: egy méretezési csoport egy virtuális hálózaton belül jön létre, és hello méretezési csoportban lévő egyes virtuális gépeken nem jutnak a nyilvános IP-címek alapértelmezés szerint. Ez a szabályzat Ezzel elkerülheti hello költség, valamint kezelési terhet külön nyilvános IP-cím lefoglalása a címek tooall hello csomópontok a számítási rácsban. Ha külső kapcsolatok tooscale állítsa be a virtuális gépek közvetlen kell, konfigurálhatja a méretezési készlet tooautomatically hozzárendelése nyilvános IP címek toonew virtuális gépeket. Másik lehetőségként csatlakoztathatja tooVMs más erőforrásaiból a virtuális hálózat, amely kiosztható nyilvános IP-címet, például terheléselosztókról, illetve önálló virtuális gépek. 
* **NAT-szabályok használatával kapcsolódnak a tooVMs**: egy nyilvános IP-cím létrehozása, tooa terheléselosztó rendelje hozzá, és adja meg a bejövő NAT-készlete. Ezek a műveletek leképezik a hello IP-cím tooa port a hello méretezési csoportban lévő virtuális gép. Példa:
  
  | Forrás | Forrásport | Cél | Célport |
  | --- | --- | --- | --- |
  |  Nyilvános IP-cím |50000-es port |vmss\_0 |22-es port |
  |  Nyilvános IP-cím |50001-es port |vmss\_1 |22-es port |
  |  Nyilvános IP-cím |50002-es port |vmss\_2 |22-es port |
  
   A [ebben a példában](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat), a NAT-szabályok meghatározott tooenable egy SSH kapcsolat tooevery Virtuálisgép-méretezési csoportban lévő, egyetlen nyilvános IP-cím használatával cím.
  
   [Ez a példa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat) hello azonos RDP és a Windows.
* **"Jumpbox" használatával kapcsolódnak a tooVMs**: a méretezési és egy különálló virtuális gép létrehozásakor azonos virtuális hálózat, hello hello önálló virtuális Gépet és hello méretezési VM kapcsolódhatnak egy másik, a saját belső IP-címek, tooone virtuális hello által definiált hálózati vagy az alhálózat. Ha egy nyilvános IP-cím létrehozása, és rendelje hozzá toohello önálló virtuális gép, RDP és az SSH tooconnect toohello önálló virtuális gép is használhatja. Majd csatlakoztathatja az, hogy gép tooyour méretezési példányok. Bizonyára észrevette, hogy egy egyszerű méretezési csoport eleve biztonságosabb, mint alapértelmezett konfigurációjában egy nyilvános IP-címmel rendelkező, egyszerű különálló virtuális gép.
  
   [Ez a sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox) például egy egyszerű méretezési csoportot helyez üzembe önálló virtuális géppel. 
* **Terheléselosztási tooscale set példányok**: Ha egy ciklikus multiplexeléssel használatával toodeliver munkahelyi tooa számítási fürtön található virtuális gépek, konfigurálható egy Azure terheléselosztó réteg-4 terheléselosztási szabályok ennek megfelelően. Mintavételt meghatározhatja, hogy az alkalmazás fut. történő megpingelésével tooverify portok a megadott protokoll, időközök és vonatkozó kérelem elérési útját. Az [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) szintén támogatja a méretezési csoportokat, illetve a 7. rétegbeli és az annál kifinomultabb terheléselosztási forgatókönyveket.
  
   [Ez a példa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) hoz létre egy méretezési használó Apache webkiszolgálók fut, és egy toobalance hello terheléselosztó, amely az egyes virtuális gépek számára. (Nézze hello Microsoft.Network/loadBalancers erőforrástípus és networkProfile és a virtualMachineScaleSet extensionProfile.)

   [Ez a linuxos példa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway) és [ez a windowsos példa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway) az Application Gatewayt használja.  

* **Méretezési csoport üzembe helyezése számítási fürtként egy PaaS-fürtkezelőben**: A méretezési csoportokat néha következő generációs feldolgozói szerepkörnek is nevezik. Bár egy érvényes leírást azt futtassa zavaró méretezési hello kockázatát set funkciók Azure Cloud Services szolgáltatások. A méretezési csoportok bizonyos értelemben valódi feldolgozói szerepkört vagy feldolgozói erőforrást biztosítanak. Használhatók platform- és futásidő-független, testre szabható és az Azure Resource Manager IaaS szolgáltatásába integrálható általános számítási erőforrásként.
  
   Egy Cloud Services-beli feldolgozói szerepkör a platform-/futásidő-támogatás tekintetében korlátozott (csak Windows platformú lemezképekkel használható). Magában foglal azonban olyan szolgáltatásokat is, mint a virtuális IP-címek felcserélése, konfigurálható frissítési beállítások, futásidő- és alkalmazástelepítés-specifikus beállítások. Ezek a szolgáltatások vagy nem érhetők *még* el a virtuálisgép-méretezési csoportokban, vagy más magasabb szintű PaaS-szolgáltatások, például az Azure Service Fabric részeként érhetők el. A méretezési csoportok PaaS-szolgáltatást támogató infrastruktúrának is tekinthetők. A [Service Fabrichoz](https://azure.microsoft.com/services/service-fabric/) hasonló PaaS-megoldások erre az infrastruktúrára épülnek.
  
   A megközelítést bemutató [példában](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos) az [Azure Container Service](https://azure.microsoft.com/services/container-service/) egy tárolóvezénylővel rendelkező méretezési csoportokon alapuló fürtöt helyez üzembe.

## <a name="scale-set-performance-and-scale-guidance"></a>A virtuálisgép-méretezési csoportok teljesítményével és skálázásával kapcsolatos útmutató
* A méretezési támogatja too1, 000 virtuális gép mentése. Ha hoz létre, és töltse fel a saját egyéni Virtuálisgép-lemezképek, hello határértéke 100. A nagy méretezési csoportok használatánál megfontolandó szempontokról a [nagyméretű virtuálisgép-méretezési csoportok használatát](virtual-machine-scale-sets-placement-groups.md) ismertető cikkben olvashat.
* Nem rendelkezik toopre-létrehozása az Azure storage fiókok toouse méretezési készlet. Skálázási készletek támogatási Azure felügyelt lemezek, amelyek semlegesítsék teljesítmény-maillel kapcsolatos lemezeket a tárfiók hello száma. További információ: [Azure virtuálisgép-méretezési csoportok és felügyelt lemezek](virtual-machine-scale-sets-managed-disks.md).
* Érdemes megfontolni az Azure Prémium Storage használatát az Azure Storage helyett a gyorsabb és kiszámíthatóbb VM-üzembehelyezési idők és a jobb I/O-teljesítmény érdekében.
* hello core kvóta hello régióban, amelyben telepít virtuális gépeket hozhat létre hello számának korlátozása. Szükség lehet toocontact ügyfél-támogatási tooincrease a számítási határt, még akkor is, ha magas vonatkozó felső korlátját: való használathoz az Azure Felhőszolgáltatások ma rendelkezik. tooquery a kvótát, az Azure CLI paranccsal: `azure vm list-usage`. Vagy futtassa ezt a PowerShell-parancsot: `Get-AzureRmVMUsage`.

## <a name="frequently-asked-questions-for-scale-sets"></a>A méretezési csoportokkal kapcsolatos gyakori kérdések
**K.** Hány virtuális gépet tartalmazhat egy méretezési csoport?

**V.** A méretezési lehet 0 too1, 000 virtuális gépek platform képek, vagy 0 too100 virtuális gépek alapján egyéni lemezképek. 

**K.** Támogatott az adatlemezek használata a méretezési csoportokon belül?

**V.** Igen. A méretezési csoport egy csatolt adatok lemezek konfigurációs tooall virtuális gépek hello készlet alkalmazó adhat meg. További információ: [Azure-beli méretezési csoportok és csatlakoztatott adatlemezek](virtual-machine-scale-sets-attached-disks.md). Egyéb adattárolási lehetőségek:

* Azure Files (SMB-megosztásos meghajtók)
* Operációs rendszer meghajtója
* Ideiglenes meghajtó (helyi, nem Azure Storage-alapú)
* Azure-adatszolgáltatás (például Azure-táblák, Azure-blobok)
* Külső adatszolgáltatás (például távoli adatbázis)

**K.** Mely Azure-régiók támogatják a méretezési csoportokat?

**V.** Mindegyik régió támogatja a méretezési csoportokat.

**K.** Hogyan lehet egyéni rendszerképekből méretezési csoportot létrehozni?

**V.** Hozzon létre egy felügyelt lemezt az egyéni rendszerkép VHD-fájlja alapján, és hivatkozzon arra a méretezési csoport sablonjában. [Például:](https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os)

**K.** Ha csökkenthető a méretezési kapacitás a 20 too15, amelyeket a virtuális gépek?

**V.** Virtuális gépek hello méretezési frissítési tartományok és a tartalék tartományok toomaximize rendelkezésre állási készlet egyenletesen el lesznek távolítva. Virtuális gépek hello legmagasabb azonosítók először törlődnek.

**K.** Mi történik, ha majd növelje a 15 too18 hello kapacitását?

**V.** Ha növeli a kapacitás too18, 3 új virtuális gépek jönnek létre. Minden alkalommal hello VM Példányazonosító értéke eggyel növekszik, hello előző legnagyobb érték (például 20, 21, 22-es). A virtuális gépek a tartalék és frissítési tartományok között oszlanak el.

**K.** Ha több bővítményt használok egy méretezési csoportban, van lehetőség végrehajtási sorrend kényszerítésére?

**V.** Közvetlenül nincs, de hello customScript bővítménnyel a parancsfájl egy másik bővítmény toofinish várhat. A bővítmény műveleti sorrend a hello blogbejegyzésben további útmutatást kaphat [bővítmény alkalmazás-előkészítés az Azure Virtuálisgép-méretezési készlet](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**K.** Használhatok virtuálisgép-méretezési csoportokat Azure rendelkezésre állási csoportokkal?

**V.** Igen. A méretezési csoport egy implicit rendelkezésre állási csoport 5 tartalék és 5 frissítési tartománnyal. Skálázási készletek 100-nál több virtuális gépek több span *elhelyezési csoportok*, amelyeket egyenértékű toomultiple rendelkezésre állási készletek. További információ az elhelyezési csoportokról: [Nagyméretű virtuálisgép-méretezési csoportok használata](virtual-machine-scale-sets-placement-groups.md). A virtuális gépek rendelkezésre állási csoportok létezhet hello ugyanaz a virtuális gépek méretezési és virtuális hálózathoz. Általános konfigurációs tooput vezérlő csomópont virtuális gépek, (amelyek gyakran egyedi konfigurációs) egy rendelkezésre állási beállítása és adatcsomópontokat put hello méretezési csoportban lévő.

További válaszok tooquestions méretezésének beállítja hello található [Azure virtuálisgép-skálázási készletekben gyakran ismételt kérdések](virtual-machine-scale-sets-faq.md).
