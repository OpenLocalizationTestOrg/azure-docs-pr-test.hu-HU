---
title: "Service Fabric fürt erőforrás-kezelő aaaIntroducing hello |} Microsoft Docs"
description: "Egy bevezető toohello Service Fabric fürt erőforrás-kezelő."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: cfab735b-923d-4246-a2a8-220d4f4e0c64
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: e815925880e2f3a755294de1dcfb9b88fbdde08a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introducing-hello-service-fabric-cluster-resource-manager"></a>A Service Fabric fürt erőforrás-kezelő hello bemutatása
Hagyományosan kezelése az informatikai rendszerek vagy online szolgáltatások célja, hogy az adott fizikai vagy virtuális gépek toothose adott szolgáltatások vagy a rendszer. Szolgáltatások volt tervezett rétegek szerint. A "web" réteghez és egy "data" vagy "tároló" réteget lenne. Alkalmazások kellene egy üzenetkezelési réteget, ahol a kérelmek növelésére és csökkentésére, valamint egy dedikált toocaching gépek készletét érkezett. Minden egyes réteg vagy Terheléstípus kellett a konkrét gépek dedikált tooit: hello adatbázis kapott néhány, gépek dedikált tooit, hello webkiszolgálók néhány. Ha egy munkaterhelés bizonyos típusú hello gépek állapotukkal a toorun túl gyakran használt adatok miatt, majd hozzá, hogy azonos konfigurációs toothat tartozó további gépeket. Azonban nem minden munkaterhelések így könnyen sikerült terjeszthető ki – különösen az adatréteg hello cserélje általában a gépek, a nagyobb gépek. Egyszerű. Ha egy gép sikertelen volt, hello a teljes alkalmazás a része, alacsonyabb kapacitással futott, amíg nem sikerült visszaállítani a hello gép. Továbbra is meglehetősen egyszerű (Ha ez nem szükségszerűen szórakoztató).

Most már azonban hello world szolgáltatás és szoftver architektúra megváltozott. Gyakori, hogy az alkalmazások alkalmazó kibővített kialakítást is. A tárolók vagy mikroszolgáltatások alkalmazásokat (vagy mindkettő) közös. Most csak néhány gépek továbbra is lehet, amíg azok nem futtatja a munkaterhelés csak egyetlen példányát. Ezek akkor is futtathatnak több különböző terhelésekhez hello: ugyanannyi időt vesz igénybe. Most már rendelkezik különböző szolgáltatások (nincs fel egy teljes számítógép alatt érkezett erőforrások), több tucatnyi akár több száz ezek a szolgáltatások különböző példányai. Minden elnevezett példány rendelkezik egy vagy több példány vagy a replikákat a magas rendelkezésre állású (HA). Attól függően, hogy ezeket a munkaterheléseket, és hogyan foglalt hello méretű több száz vagy ezer gépek előfordulhat magát. 

Hirtelen kezelése a környezet többé már nem így egyszerűen munkaterhelések néhány gépek dedikált toosingle szűrőtípusok kezelését. A kiszolgálók virtuális, illetve neve nem lehet (a mindsets rendelkezik váltott [Hobbiállatok toocattle](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) követően). Konfigurációs értéke kisebb hello gépek és hello szolgáltatásoknak bővebben. Hardver, amely dedikált tooa egyetlen példány futhat a munkaterhelés nagyjából egy korábbi hello dolog. Maguk szolgáltatások váltak kis elosztott rendszerek, amelyek több több kisebb kódrészletek hagyományos hardvereken.

Az alkalmazás már nincs elosztva a több rétegek monoliths sorozata, mert a számos további kombinációk toodeal most már rendelkezik. Akik úgy dönt, hogy milyen típusú munkaterheléseket futtatható hardver, vagy hogy hány? Mely munkaterhelések is jól működik hello azonos hardver-, és amelyek ütköznek? Amikor olyan gép lefelé hogyan tudja, hogy mi az, hogy a gép nem futott? Ki van feladata meggyőződött arról, hogy munkaterhelés újra futásának indításakor? Várjon hello (?) a virtuálisgép-toocome vissza, vagy hajtsa végre a munkaterhelések automatikusan áthelyezze a feladatokat tooother gépeket, és csak olyan fut? Az emberi beavatkozás szükséges? Mi a helyzet a frissítéseket az ebben a környezetben?

A fejlesztők és operátorok foglalkozó ebben a környezetben az oktatóanyagban módosítjuk összetettség toowant kezeléséhez. A binge bérbeadása, majd próbálkozzon toohide hello összetettsége személyekkel valószínűleg nem hello megfelelő választ, így Mi a teendő?

## <a name="introducing-orchestrators"></a>Orchestrators bemutatása
Egy "Orchestrator" hello általános kifejezés, amely segítségével a rendszergazdák az ilyen típusú környezetben kezelése szoftvereken a rendszer. Orchestrators összetevői hello, amely a kérelmek igénybe vehet, például "Szeretnék a saját környezetben futó szolgáltatás öt példánya." Próbálnak toomake hello környezet egyezés szükséges hello állapot, függetlenül attól, hogy mi történik.

(Nem emberek) orchestrators, mi a művelet végrehajtása, és a gép meghibásodik, vagy egy munkaterhelés valamilyen nem várt okból leáll. A legtöbb orchestrators csupán foglalkoznak. Egyéb szolgáltatások rendelkeznek-e új frissítések kezelése, és a hálózatierőforrás-fogyasztás és irányítási foglalkozó központi telepítések kezelt. Minden orchestrators készül alapvetően néhány konfigurációs hello környezetben kívánt állapotának karbantartása. Azt szeretné, hogy toobe képes tootell az orchestrator mi azt szeretné, és annak hello gyakori emelő. Az Aurora Mesos, Docker Datacenter/Docker Swarm, Kubernetes és a Service Fabric fölött példák összes orchestrators. Ezek orchestrators folyamatban van a valós munkaterhelések éles környezetben aktívan fejlett toomeet hello igényeinek. 

## <a name="orchestration-as-a-service"></a>Vezénylési szolgáltatásként
hello fürt erőforrás-kezelő, amely kezeli a Service Fabric vezénylési hello rendszerösszetevő. hello fürt erőforrás-kezelő feladat három részből oszlik:

1. Szabályok érvényesítése
2. A környezet optimalizálása
3. Útmutatás nyújtása a más folyamatokkal

toosee hello erőforrás-kezelő fürt működése, a következő Microsoft Virtual Academy videó figyelési hello:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=d4tka66yC_5706218965">
<img src="./media/service-fabric-cluster-resource-manager-introduction/ConceptsAndDemoVid.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="what-it-isnt"></a>Mi nem
A hagyományos N rétegből álló alkalmazások, mindig fennáll a [terheléselosztó](https://en.wikipedia.org/wiki/Load_balancing_(computing)). Általában ez volt, a hálózati terheléselosztó (NLB) vagy egy alkalmazás terhelés terheléselosztó (ALB) attól függően, ahol ez volt a hello hálózati vermet. Néhány terheléselosztók meg az F5 BigIP ajánlat például hardveralapú, mások olyan szoftveres például a Microsoft által a hálózati Terheléselosztás. Más környezetekben megjelenhet valami haproxy esetén, nginx, Istio vagy ügyféloldali ebben a szerepkörben. Ezek architektúrákban hello terheléselosztás folyamat tooensure állapot nélküli munkaterheléseket fogadása (körülbelül) hello munkahelyi olyan mértékű. Terheléselosztás kapcsolatos olyan stratégiák változatos betölteni. Néhány terheléselosztók elküldése minden más hívási tooa másik kiszolgálón. Mások biztosítja, hogy a rögzítési munkamenet/tölcsérútvonalak. Speciális terheléselosztók tényleges betöltést becslés vagy hívás a várt és jelenlegi gép terhelés alapján jelentéskészítési tooroute használja.

Webes/munkavégző réteg hello hálózati terheléselosztók vagy üzenet próbált útválasztók tooensure maradt nagyjából kiegyensúlyozott. Terheléselosztás hello adatrétegbeli kapcsolatos olyan stratégiák különböző és hello adatok tárolási mechanizmus a függő volt. Adatok horizontális gyorsítótárazáshoz, olyan felügyelt nézetek, a tárolt eljárások és az egyéb tároló-specifikus mechanizmusok hello adatrétegbeli terheléselosztás hivatkozni.

Ezek stratégiák némelyike érdekes, amíg hello Service Fabric fürt erőforrás-kezelő nincs semmi például egy hálózati terheléselosztó vagy a gyorsítótár. Hálózati terheléselosztó frontends frontends közötti forgalom elosztásával egyensúlyba kerüljön. Service Fabric fürt erőforrás-kezelő hello van egy másik stratégia. Alapvetően, helyezi át a Service Fabric *szolgáltatások* toowhere, akkor a legtöbb érthető, a várt forgalom hello vagy nem tölthető be toofollow. Akkor lehet, hogy át például szolgáltatások toonodes, hogy vannak-e hello szolgáltatást nem végez alkalmazásfejlesztőre jelenleg cold vannak. hello csomópontok cold lehet, mert hello jelen sikerült törölték vagy áthelyezték máshol. Másik példaként a fürt erőforrás-kezelő hello is helyezze át egy másik lapra a gép szolgáltatás. Lehet, hogy hello gép frissítése toobe kapcsolatban, vagy fut rajta hello szolgáltatás túlterhelt tooa csúcs az igények fogyasztás miatt. Alernatively, erőforrás-követelmények hello szolgáltatást is növekedett. Ennek eredményeképpen nincs elegendő erőforrás a gép toocontinue is fut a. 

Hello fürt erőforrás-kezelő felelős körül szolgáltatások áthelyezésére, mert egy másik szolgáltatás képest set toowhat egy hálózati terheléselosztó találjuk tartalmaz. Ennek az az oka hálózati terheléselosztók szolgáltatások már vannak, a hálózati forgalom toowhere kézbesíti, akkor is, ha nem az ideális hello szolgáltatást futtató helyről. hello Service Fabric fürt erőforrás-kezelő funkcióit használja annak biztosítására, hogy hello fürt hello erőforrások hatékony ki van használva alapvetően különböző stratégiákat.

## <a name="next-steps"></a>Következő lépések
- Hello architektúra és információk folyamata belül hello fürt erőforrás-kezelő információkért tekintse meg [Ez a cikk](service-fabric-cluster-resource-manager-architecture.md)
- hello fürt erőforrás-kezelő számos lehetőséget leíró hello fürt rendelkezik. toofind további információk a metrikákat, tekintse meg a cikk a [leíró a Service Fabric-fürt](service-fabric-cluster-resource-manager-cluster-description.md)
- A szolgáltatások konfigurálásáról [szolgáltatások konfigurálásával kapcsolatos tudnivalók](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)
- Adatok gyűjtése le hogyan kezeli a Service Fabric fürt erőforrás-kezelő hello használat és a kapacitás hello fürtben. További metrikák és hogyan tooconfigure őket kivétel toolearn [Ez a cikk](service-fabric-cluster-resource-manager-metrics.md)
- hello fürt erőforrás-kezelő szolgáltatás Hálófelügyeleti képességek működik. adott integrációról további információkat toofind olvasási [Ez a cikk](service-fabric-cluster-resource-manager-management-integration.md)
- toofind meg arról, hogyan hello fürt erőforrás-kezelő felügyeli, és elosztja a terhelést hello fürtben, tekintse meg hello a cikk a [terheléselosztás](service-fabric-cluster-resource-manager-balancing.md)
