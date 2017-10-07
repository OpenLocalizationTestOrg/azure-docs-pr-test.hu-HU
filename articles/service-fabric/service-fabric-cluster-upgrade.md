---
title: "az Azure Service Fabric-fürt aaaUpgrade |} Microsoft Docs"
description: "Frissítés hello Service Fabric-kód és/vagy konfiguráció, amelyen a Service Fabric-fürt, beállítása a fürt frissítési üzemmódban van; tanúsítványok hozzáadása az alkalmazás-porttal rendelkezik, ennek során az operációsrendszer-javítások, frissítése, és így tovább. Mi a számíthat, ha hello frissítések történik?"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/10/2017
ms.author: chackdan
ms.openlocfilehash: 94ac3833ec0810f79de06ecb50f254028fa90408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-an-azure-service-fabric-cluster"></a>Az Azure Service Fabric-fürt frissítése
> [!div class="op_single_selector"]
> * [Azure-fürttel](service-fabric-cluster-upgrade.md)
> * [Önálló fürthöz](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

Bármely, modern rendszer megtervezése a bővíthetőség kulcs tooachieving hosszú távú sikert a termék. Az Azure Service Fabric-fürt, amely a saját, de részben Microsoft által felügyelt erőforrás. A cikkből megtudhatja, mi automatikusan kezeli, és mi beállíthatja saját maga.

## <a name="controlling-hello-fabric-version-that-runs-on-your-cluster"></a>A fürtön futó hello fabric-verzió vezérlése
Beállíthatja a fürt tooreceive automatikus háló frissítések, azokat a Microsoft által kiadott, vagy válassza ki a fürt toobe egy támogatott fabric-verzió.

Ehhez a beállítás hello "upgrademode érték" fürtkonfiguráció hello portálon vagy az erőforrás-kezelő hello létrehozása során, vagy később egy élő fürthöz 

> [!NOTE]
> Győződjön meg arról, hogy tookeep a mindig támogatott háló verzióját futtató fürtre. És azt egy új verzióját a service fabric hello kiadása értesítés, hello előző verziója legalább a dátumnak a 60 nap után végéhez van megjelölve. hello hello új kiadásokat történik bejelentés [hello szolgáltatás háló csapat blogjában](https://blogs.msdn.microsoft.com/azureservicefabric/). hello új kiadásban elérhető toochoose majd. 
> 
> 

14 napja előzetes toohello lejárati hello kiadás a fürt fut, a health esemény jön létre, amely a fürt elhelyezi a figyelmeztetési állapot. hello fürt figyelmeztetési állapotban marad, amíg nem frissíti az tooa támogatott fabric-verzió.

### <a name="setting-hello-upgrade-mode-via-portal"></a>Hello a portálon keresztül frissítési mód beállítása
Beállíthat hello fürt tooautomatic vagy manuális hello fürt létrehozásakor.

![Create_Manualmode][Create_Manualmode]

Hello fürt tooautomatic állíthatja be, vagy manuális élő fürtön lévő, élmény hello segítségével kezelheti. 

#### <a name="upgrading-tooa-new-version-on-a-cluster-that-is-set-toomanual-mode-via-portal"></a>Egy fürtön, amelyek portálon keresztül tooManual mód beállítása a tooa új verziójának frissítése.
tooupgrade tooa új verziója, toodo szüksége hello legördülő listából válassza ki a hello elérhető verzióra, és mentse. Fabric-frissítésnek hello lekérdezi kezdődött el automatikusan. hello fürt állapotházirendeket (kombinációjából csomópont hello egészségügyi összes hello hello fürtben futó alkalmazások és) betartása tooduring hello frissítését.

Ha a fürt állapotházirendeket hello nem teljesülnek, hello frissítés vissza lesz állítva. Görgessen lefelé a dokumentum további hogyan tooread tooset ezen egyéni házirendek. 

Hello visszaállítási eredményező hello problémák kijavítása, amennyiben szükséges tooinitiate hello frissítés ebben az esetben ugyanaz, mint korábban lépések következő hello.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-hello-upgrade-mode-via-a-resource-manager-template"></a>A Resource Manager-sablon használatával hello a frissítési mód beállítása
Adja hozzá a hello "upgrademode érték" konfigurációs toohello Microsoft.ServiceFabric/clusters erőforrás-definícióban, és set hello "clusterCodeVersion" tooone a hello támogatott fabric-verziók alább látható módon, és hello sablon telepítését. hello érvényes "upgrademode érték" értékei a "Manual" vagy "Automatikus"

![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-tooa-new-version-on-a-cluster-that-is-set-toomanual-mode-via-a-resource-manager-template"></a>Egy fürthöz, amelyet a Resource Manager-sablon használatával tooManual mód beállítása a tooa új verziójának frissítése.
Manuális üzemmódban tooupgrade tooa új verziója, hello fürt esetén módosítja hello "clusterCodeVersion" tooa támogatott verzióját, és telepítse azt. hello sablon hello fürttelepítésben hello Fabric frissítés megrúgni lekérdezi kezdődött el automatikusan. hello fürt állapotházirendeket (kombinációjából csomópont hello egészségügyi összes hello hello fürtben futó alkalmazások és) betartása tooduring hello frissítését.

Ha a fürt állapotházirendeket hello nem teljesülnek, hello frissítés vissza lesz állítva. Görgessen lefelé a dokumentum további hogyan tooread tooset ezen egyéni házirendek. 

Hello visszaállítási eredményező hello problémák kijavítása, amennyiben szükséges tooinitiate hello frissítés ebben az esetben ugyanaz, mint korábban lépések következő hello.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Az összes elérhető verzió listáját beszerzése az adott előfizetéshez tartozó minden környezetben
Futtassa a következő parancs hello, és egy kimeneti hasonló toothis kapja meg.

"supportExpiryUtc" közli a Ha egy adott kiadás lejárati ideje, vagy lejárt. hello legújabb kiadás nem rendelkezik érvényes dátum - értéke a "9999-12-31T23:59:59.9999999", amely csak azt jelenti, hogy hello lejárati még nem állították.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/locations/{{location}}/clusterVersions?api-version=2016-09-01

Example: https://management.azure.com/subscriptions/1857f442-3bce-4b96-ad95-627f76437a67/providers/Microsoft.ServiceFabric/locations/eastus/clusterVersions?api-version=2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-hello-cluster-upgrade-mode-is-automatic"></a>Háló frissítési működés, ha automatikus hello fürt frissítési mód
A Microsoft fenntartja a hello háló kódot és az Azure-fürtben lévő futtató konfiguráció. Automatikus figyelt frissítéseket toohello szoftver szükség szerint alapon végezzük. Ezek a frissítések lehet kódot, konfigurációs vagy mindkettőt. a toomake meg arról, hogy az alkalmazás szenved, nincs hatása vagy toothese frissítések miatt gyakorolt minimális hatás mellett, a következő fázisok hello végezzük hello frissítések:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>1. fázis: Frissítés történik az összes fürt házirendek használatával
Ebben a fázisban hello frissítések folytatható egy frissítési tartományt egyszerre, és hello hello fürtben futó alkalmazások továbbra is toorun állásidő nélkül. hello fürt állapotházirendeket (kombinációjából csomópont hello egészségügyi összes hello hello fürtben futó alkalmazások és) betartása tooduring hello frissítését.

Ha a fürt állapotházirendeket hello nem teljesülnek, hello frissítés vissza lesz állítva. Ezután egy e-mailt küld hello előfizetés toohello tulajdonosa. hello e-mail hello a következő információkat tartalmazza:

* Az értesítési tooroll vissza egy Fürtfrissítés le kellett.
* Javasolt javító műveleteket, ha van ilyen.
* hello hány napig (n), amíg azt hajtható végre a 2. szakasza.

A Microsoft próbálja tooexecute hello azonos néhány alkalommal frissíteni, abban az esetben, ha a megjelenjenek infrastruktúra okok miatt nem sikerült. Hello után n naponta hello dátum hello e-mailt küldték, a Folytatás tooPhase 2.

Ha hello fürt állapotházirendeket teljesülnek, a hello frissítés van sikeresnek minősül, és befejezettként. Ez akkor fordulhat elő hello kezdeti frissítés vagy frissítési ismétlések hello bármelyikét során ebben a fázisban. Nincs e-mailes megerősítés sikeres futtató van. Ez a tooavoid egy kivétel toonormal kell tekinteni, túl sok e-mailek--küld egy e-maileket küld. A legtöbb hello fürt frissítések toosucceed várhatóan az alkalmazások rendelkezésre állásáról befolyásolása nélkül.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>2. fázis: Egy frissítés végrehajtása csak alapértelmezett állapotházirendeket használatával
Ebben a fázisban hello állapotházirendeket meg úgy, hogy a megfelelő hello frissítés hello elején volt alkalmazások hello számát hello azonos hello hello frissítési eljárás ideje alatt továbbra is. 1. fázis, mint 2. szakasza hello frissítések folytatható egy frissítési tartományt egyszerre, és hello hello fürtben futó alkalmazások folytatni toorun állásidő nélkül. hello fürt állapotházirendeket (kombinációjából csomópont hello egészségügyi összes hello hello fürtben futó alkalmazások és) hello frissítés betartását toofor hello időtartama.

Ha hello fürt házirendek érvényben nem teljesülnek, hello frissítés vissza lesz állítva. Ezután egy e-mailt küld hello előfizetés toohello tulajdonosa. hello e-mail hello a következő információkat tartalmazza:

* Az értesítési tooroll vissza egy Fürtfrissítés le kellett.
* Javasolt javító műveleteket, ha van ilyen.
* hello hány napig (n) csak akkor hajtható végre a 3.

A Microsoft próbálja tooexecute hello azonos néhány alkalommal frissíteni, abban az esetben, ha a megjelenjenek infrastruktúra okok miatt nem sikerült. Egy emlékeztető e-mailt küld néhány napos n naponta mentése előtt. Hello után n naponta hello dátum hello e-mailt küldték, folytatás tooPhase 3. a 2. szakasza kapni hello e-mailek súlyosan kell venni, és javító műveleteket kell tenni.

Ha hello fürt állapotházirendeket teljesülnek, a hello frissítés van sikeresnek minősül, és befejezettként. Ez akkor fordulhat elő hello kezdeti frissítés vagy frissítési ismétlések hello bármelyikét során ebben a fázisban. Nincs e-mailes megerősítés sikeres futtató van.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>3. fázis: A frissítés végrehajtása túl szigorú házirendek használatával
Ebben a fázisban a állapotházirendeket hello futó alkalmazások állapotát hello helyett hello frissítés körétől vannak. Ebben a fázisban végül fürt nagyon kevés frissítés. A fürt toothis fázis kap, ha van esély arra, hogy az alkalmazás akkor kerül sérült állapotba, és/vagy rendelkezésre állási elvesznek.

Hasonló toohello más két fázis a 3 frissítések folytassa a műveletet egy frissítési tartományt egyszerre.

Ha a fürt állapotházirendeket hello nem teljesülnek, hello frissítés vissza lesz állítva. A Microsoft próbálja tooexecute hello azonos néhány alkalommal frissíteni, abban az esetben, ha a megjelenjenek infrastruktúra okok miatt nem sikerült. Ezt követően hello fürt rögzítve van, így már nem kap támogatási és/vagy a frissítéseket.

Az információ egy e-mailt küld toohello előfizetés tulajdonosa, valamint hello javító műveleteket. A fürtök tooget várhatóan nem olyan állapotba, ha nem a 3.

Ha hello fürt állapotházirendeket teljesülnek, a hello frissítés van sikeresnek minősül, és befejezettként. Ez akkor fordulhat elő hello kezdeti frissítés vagy frissítési ismétlések hello bármelyikét során ebben a fázisban. Nincs e-mailes megerősítés sikeres futtató van.

## <a name="cluster-configurations-that-you-control"></a>Vezérelt fürtkonfigurációk
Ezenkívül toohello képességét tooset hello fürt frissítési mód, az alábbiakban egy élő fürthöz módosíthatja hello konfigurációkat.

### <a name="certificates"></a>Tanúsítványok
Hozzáadhat új, vagy a tanúsítványok hello fürt és az ügyfél hello portálon keresztül egyszerűen törölje. Tekintse meg a túl[Ez a dokumentum részletes információkra van szüksége](service-fabric-cluster-security-update-certs-azure.md)

![Képernyőkép a tanúsítvány-ujjlenyomatok a hello Azure-portálon.][CertificateUpgrade]

### <a name="application-ports"></a>Alkalmazás-portok
Alkalmazás portok hello terheléselosztó erőforrás-tulajdonságok hello csomóponttípus társított megváltoztatásával módosíthatja. Hello portálon is használhatja, vagy az erőforrás-kezelő a PowerShell segítségével közvetlenül.

Új port típusú csomópont összes virtuális tooopen hello a következő:

1. Adjon hozzá egy új mintavételi toohello megfelelő terheléselosztót.
   
    Ha a fürt hello portál használatával telepítette, hello terheléselosztók elnevezése "LB-neve hello erőforrás csoport-NodeTypename", az egyes csomóponttípusok egyet. Mivel hello load balancer nevek csak egy erőforráscsoporton belül egyedi, célszerű, ha egy adott erőforrás csoportban keresse meg őket.
   
    ![Képernyőkép a hozzáadása egy mintavételi tooa terheléselosztó hello portálon.][AddingProbes]
2. Új szabály toohello terheléselosztó felvételéhez.
   
    Adjon hozzá egy új szabály toohello terheléselosztó azonos hello mintavételi hello előző lépésben létrehozott használatával.
   
    ![Új szabály tooa terheléselosztó hozzáadása hello portálon.][AddingLBRules]

### <a name="placement-properties"></a>Elhelyezési tulajdonságok
Az egyes csomóponttípusok hello toouse szeretné az alkalmazásokat az egyéni elhelyezési tulajdonságokat adhat hozzá. A NodeType egy alapértelmezett tulajdonság, amely explicit módon hozzáadná nélkül is használható.

> [!NOTE]
> Részletes hello használatára korlátozza, csomópont tulajdonságait, és hogyan toodefine őket, tekintse meg a Service Fabric fürt erőforrás-kezelő dokumentum hello toohello szakaszának "Elhelyezési megkötések és csomópont tulajdonságok" a [a fürt leíró ](service-fabric-cluster-resource-manager-cluster-description.md).
> 
> 

### <a name="capacity-metrics"></a>A kapacitás metrikák
Az egyes csomóponttípusok hello megjeleníteni kívánt toouse az alkalmazások tooreport terhelés egyéni kapacitási mérőszámokat adhat hozzá. A kapacitás metrikák tooreport terhelésének hello használata további tájékoztatást toohello Service Fabric fürt erőforrás-kezelő dokumentumok [a fürt leíró](service-fabric-cluster-resource-manager-cluster-description.md) és [metrikák és a](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Frissítési beállítások fabric - irányelveinek
Megadhat egyéni irányelveinek fabric frissítéséhez. Ha a fürt tooAutomatic fabric frissítéskezelésének meg, ezek a házirendek alkalmazott toohello 1. fázis hello automatikus fabric frissítéskezelésének beolvasása.
Ha meg van a fürt manuális háló frissítéseket, akkor ezek a házirendek érvényben minden alkalommal, amikor választja kiváltó hello rendszer tookick hello fabric-frissítésnek a fürt ki új verzió. Ha nem bírál felül hello házirendek, hello alapértelmezett beállításokat használja.

Adjon meg egyéni állapotházirendeket hello, vagy áttekintéséhez hello jelenlegi beállításai alapján hello "fabric-frissítésnek" panelen válassza ki a hello speciális beállítások frissítése. Tekintse át az alábbi képen való hello. 

![Egyéni házirendek kezelése][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>A fürt háló beállításainak testreszabása
Tekintse meg a túl[service fabric-fürt hálóbeállításokat](service-fabric-cluster-fabric-settings.md) mi, és hogyan szabhatja azokat.

### <a name="os-patches-on-hello-vms-that-make-up-hello-cluster"></a>A virtuális gépek hello fürtöt alkotó hello operációsrendszer-javítások
Tekintse meg a túl[javítás Vezénylési alkalmazás](service-fabric-patch-orchestration-application.md) amelyek központilag telepíthetők a Windows Update szolgáltatásból a fürt tooinstall javítások tartva hello elérhető szolgáltatások összes hello orchestrated módon. 

### <a name="os-upgrades-on-hello-vms-that-make-up-hello-cluster"></a>Az operációs rendszer frissítései a hello hello fürtöt alkotó virtuális gépek
Ha frissítenie kell az operációsrendszer-lemezképek hello hello fürt hello virtuális gépeken, tegye azt egy virtuális gép egyszerre. Az Ön felelőssége ehhez a frissítéshez--jelenleg nincs automatizálva ehhez.

## <a name="next-steps"></a>Következő lépések
* Megtudhatja, hogyan toocustomize néhány hello [szolgáltatás fabric fürt háló beállításai](service-fabric-cluster-fabric-settings.md)
* Ismerje meg, hogyan túl[a fürt vagy horizontális](service-fabric-cluster-scale-up-down.md)
* További tudnivalók [alkalmazásfrissítések](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
