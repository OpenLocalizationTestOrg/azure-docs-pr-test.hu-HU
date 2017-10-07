---
title: "az erőforrás-kezelő fürt leírása aaaCluster |} Microsoft Docs"
description: "A Service Fabric-fürt leíró tartalék tartományok, a frissítési tartományok, a csomópont tulajdonságait és a csomópont kapacitások hello fürt erőforrás-kezelő megadásával."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 55f8ab37-9399-4c9a-9e6c-d2d859de6766
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: f2822075976bd54402af5ad56991b5b360dfb1d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="describing-a-service-fabric-cluster"></a>Ismertető a service fabric-fürt
Service Fabric fürt erőforrás-kezelő hello leíró egy fürt több mechanizmus biztosít. Futásidőben hello fürt erőforrás-kezelő a információk tooensure magas rendelkezésre állású hello szolgáltatásokat futtató hello fürt használja. Miközben a fontos szabályok, azt is megkísérli a toooptimize hello hálózatierőforrás-fogyasztás hello fürtön belül.

## <a name="key-concepts"></a>Fő fogalmak
hello fürt erőforrás-kezelő fürt leíró több funkciót támogatja:

* Tartalék tartományok
* Frissítési tartományok
* Csomópont tulajdonságai
* Csomópont-kapacitás

## <a name="fault-domains"></a>Tartalék tartományok
Tartalék tartomány bármely terület koordinált hiba. Egyetlen számítógépen egy tartalék tartományt, (mivel meghiúsulhatnak, hogy a power ellátási hibák toodrive hibák toobad NIC belső vezérlőprogram a saját a különböző okok miatt). Azonos Ethernet-kapcsoló szerepelnek csatlakoztatott toohello hello azonos tartalék tartományban, mint a gépek gépek megosztása egyetlen forrás áramkimaradás vagy egy helyen. Mivel a hardveres hibák toooverlap természetes, tartalék tartományok eredendően hierarchikus és jelentésekként jelennek meg a Service Fabric URI.

Fontos, hogy tartalék tartományok megfelelően vannak beállítva, mert a Service Fabric ezen információk toosafely hely szolgáltatásokat használ. A Service Fabric nem szeretné, hogy tooplace szolgáltatások úgy, hogy (hello hiba néhány összetevő által okozott) tartalék tartomány hello elvesztését eredményezi egy szolgáltatás toogo le. Hello Azure Service Fabric hello tartalék tartomány adatokat hello környezet toocorrectly által biztosított használja környezetben konfigurálja az Ön nevében hello hello fürt csomópontja. A Service Fabric önálló, a tartalék tartományok hello időben vannak meghatározva hello fürt beállítása 

> [!WARNING]
> Fontos, hogy hello tartalék tartomány adatokat megadva a háló tooService pontos. Tételezzük fel például, hogy a Service Fabric-fürt csomópontja öt fizikai állomáson futnak, 10 virtuális gépeken belül futnak. Ebben az esetben, annak ellenére, hogy nincsenek 10 virtuális gépet, nincs csak 5 különböző (a legfelső szintű) tartományok fault. Ugyanazon a fizikai gazdagépen hatására hello megosztása virtuális gépek tooshare hello legfelső szintű azonos tartalék tartományban, mert a virtuális gépek élmény hello koordinált hiba, ha a fizikai gazdagép meghibásodik.  
>
> Mivel a Service Fabric hello tartalék tartomány egy csomópont nem toochange vár. Más módszerek, például a biztosítsa a magas rendelkezésre állású virtuális gépek, hello [magas rendelkezésre ÁLLÁSÚ virtuális gépek](https://technet.microsoft.com/en-us/library/cc967323.aspx), egy gazdagép tooanother a virtuális gépek transzparens migrálását. Ezek a mechanizmusok nem konfigurálja újra, és értesíthetők a virtuális gép hello kódot futtató hello. Így azok **nem támogatott** , környezetekben a Service Fabric rendszert futtató fürtöket. A Service Fabric hello csak magas rendelkezésre állású technológia alkalmazott kell lennie. Például a virtuális gép élő áttelepítést, a mechanizmusok San hálózatok, vagy mások számára nem szükséges. Ha a Service Fabric, ezek a mechanizmusok együtt használható _csökkentése_ alkalmazás rendelkezésre állásának és megbízhatóságának vezetnek nagyobb fokú összetettségével jár, mert hiba központi adatforrások hozzáadása, és megbízhatósági és rendelkezésre állási stratégiát, amely ütközik a Service Fabric a felhasználását. 
>
>

Az alábbi ábra hello hello entitásokhoz Ez hozzájárul a tooFault tartományok és a lista összes hello különböző tartalék tartományok, amelyek azt szín. Ebben a példában tudunk adatközpontok ("DC"), például rackszekrények ("R") és panelen (a "B"). Kapcsolódását Ha minden panel egynél több virtuális gép, lehetnek réteget a hello tartalék tartomány hierarchia.

<center>
![Tartalék tartományok keresztül szervezett csomópontja][Image1]
</center>

Futásidőben a Service Fabric fürt erőforrás-kezelő hello hello tartalék tartományok hello fürt leálltnak, és elrendezések tervez. állapot-nyilvántartó replikák hello, vagy egy adott szolgáltatáshoz állapotmentes példányai vannak kerül terjesztésre, így külön tartalék tartományok vannak. Hello szolgáltatás osztja a tartalék tartományok között hello szolgáltatás elérhetősége hello nem sérül, amikor egy tartalék tartomány meghibásodna hello hierarchia bármely szinten biztosítja.

A Service Fabric-fürt erőforrás-kezelő nem fontos hány rétegek hello tartalék tartomány hierarchia szerepelnek. Azonban, hogy bármely hello hierarchia egy részének hello megszűnését nem befolyásolja a benne futó szolgáltatások tooensure megkísérli. 

Ez nem ajánlott, ha egyes szintjein mélység hello tartalék tartomány hierarchia a csomópontok azonos számú hello. Ha a tartalék tartományok "fáját" hello egyenetlen a fürtben, azt megnehezíti a hello fürt erőforrás-kezelő toofigure hello legjobb foglalási szolgáltatások ki. Imbalanced tartalék tartományok elrendezések jelenti azt, hogy egyes tartományok hello adatvesztés hatása hello több, mint a más tartományokban szolgáltatások rendelkezésre állását. Emiatt hello fürt erőforrás-kezelő között két célt levágása: szolgáltatások helyez szeretnének toouse hello gépek az adott "nehéz" tartományban, és, hogy a tartomány hello megszűnését problémákhoz nem szeretnének más tartományok tooplace szolgáltatások. 

Mire imbalanced tartományok figyelni például? Hello az alábbi ábrán megmutatjuk, két különböző fürt elrendezés. Hello első példában a hello csomópontok egyenletesen különböző pontjain hello tartalék tartományok. Hello második példában egy tartalék tartomány más tartalék tartományok rendelkezik mint hello számos további csomópontokat. 

<center>
![Két különböző fürt elrendezések][Image2]
</center>

Az Azure-ban, amelyek a tartalék tartomány csomópontot tartalmaz hello választott van kezelve. Azonban attól függően, hogy hello kiépítése csomópontok száma továbbra is fejezheti a tartalék tartományok többinél azokat a további csomópontokkal. Tegyük fel például, öt tartalék tartományok hello fürt rendelkezik, de egy adott NodeType hét csomópontok létrehozni. Ebben az esetben hello első két tartalék tartományok végül több csomópontot. Ha továbbra is toodeploy további NodeType tulajdonságok értéke csak néhány osztályt, hello probléma rosszabb lekérdezi. Javasoljuk, hogy hello ezért az egyes csomóponttípusok csomópontok száma legyen hello tartalék tartományainak számát.

## <a name="upgrade-domains"></a>Frissítési tartományok
Frissítési tartományok egy másik szolgáltatás, amely segít a Service Fabric fürt erőforrás-kezelő hello hello elrendezés hello fürt ismertetése. Frissítési tartományok megadása a következő hello frissített csomópontok beállítása ugyanannyi időt vesz igénybe. Frissítési tartományok súgó hello fürt erőforrás-kezelő megértéséhez, valamint levezényelni a kezelési műveletek, például a frissítéseket.

Frissítési tartományok sokkal vannak, például a tartalék tartományok, de néhány fontosabb különbségeket. Első lépésként koordinált hardverhibák területéhez tartalék tartományok definiálása. Frissítési tartományt, a hello ugyanakkor, házirend által meghatározott. Toodecide hány azt szeretné, helyett hello környezet alatt meghatározni az beszerzése. Tetszőleges számú frissítési tartományok csomópontok módon lehet. Egy másik tartalék tartományok és a frissítési tartományok közötti különbség, hogy a frissítési tartományok nincsenek hierarchikus. Ehelyett azok több mint egy egyszerű címke. 

hello alábbi ábrán látható három frissítési tartományok szétteríti a három tartalék tartományok. Emellett egy lehetséges három különböző replikáinak állapotalapú szolgáltatás elhelyezésének, ahol minden egyes fejeződik be a különböző tartalék és frissítési tartományok jelenít meg. Az elhelyezési lehetővé teszi, hogy a szolgáltatás frissítésének hello közel tartalék tartomány hello megszűnését, és továbbra is fennáll az hello kódja és adatai egy példányát.  

<center>
![A tartalék és verziófrissítési elhelyezése][Image3]
</center>

Vannak előnyei és hátrányai toohaving nagy számú frissítési tartomány. További frissítési tartományok azt jelenti, hogy egyes hello frissítés lépéseinek részletesebb, és ezért hatással van a csomópont vagy a szolgáltatások kevesebb. Ennek eredményeképpen kevesebb szolgáltatások rendelkeznek toomove egyszerre, kisebb a forgalom bevezetéséről hello rendszerbe. Ez általában tooimprove megbízhatóság, mert kisebb hello szolgáltatás által bevezetett hello frissítés során bármilyen problémát kihatással van. További frissítési tartományok azt is jelenti, kell kevesebb, mint a más csomópontok toohandle hello hatása hello elérhető puffer frissíteni. Például ha öt frissítési tartományok, minden egyes csomópontján hello vannak kezelése nagyjából 20 %-a forgalmat. Ha tootake frissítési tartomány le kell a frissítéshez, a terhelés általában kell toogo valahol. Négy fennmaradó frissítési tartományban vannak, mivel minden egyes hely hello teljes forgalom körülbelül 5 %-át kell rendelkeznie. További frissítési tartományok azt jelenti, hogy kevesebb puffer hello fürt hello csomópontján van szüksége. Tegyük fel, hogy ha 10 frissítési tartománya volt helyette. Ebben az esetben egyes UD volna csak végző hello teljes forgalom körülbelül 10 %-át. Ha a frissítési lépéseket hello fürt keresztül, minden tartomány csak kellene toohave hely készül 1.1 % hello teljes forgalom számára. További frissítési tartományok általában lehetővé teszi toorun magasabb kihasználtsága, mivel kevesebb lefoglalt kapacitás van szüksége a csomópontokat. hello esetében is igaz tartalék tartományok.  

hello hátránya, hogy hány frissítési tartományok, hogy az egyes frissítések tootake hosszabb. A Service Fabric egy megvárja-e egy rövid idő alatt után egy frissítési tartomány befejeződött, és tooupgrade hello elindítása előtt ellenőrzi. Ezek a késleltetések bevezetett hello frissítése előtt hello frissítés előrehalad észlelése problémák engedélyezése. hello kompromisszumot nem elfogadható, mert megakadályozza, hogy rossz módosítások érintő hello szolgáltatás túl sok egyszerre.

Túl kevés frissítési tartományok rendelkezik sok negatív hatásai – minden egyes frissítési tartomány nem működik, amíg a frissítés alatt áll egy nagy része a teljes kapacitásának nem érhető el. Például ha csak három frissítési tartományok készítésének körülbelül 1/3 a teljes szolgáltatás vagy a fürt kapacitás le egyszerre. Hogy nagy részét a szolgáltatás le egyszerre nem kívánatos, mivel Ön toohave elegendő kapacitással rendelkeznek a toohandle hello fürtmunkaterhelés hello többi. Adott puffer azt jelenti, hogy normál működés során azokat a csomópontokat, kisebb, mint egyébként betöltött karbantartása. Ez növeli a szolgáltatást futtató hello költségét.

Nincs valós toohello összesített számának korlátozása hiba vagy a frissítési tartományok egy olyan környezetben, vagy korlátozza a hogyan átfedik van. Említett, van néhány gyakori minták:

- Tartalék tartományok és a frissítési tartományok leképezése 1:1
- Egy frissítési tartomány-csomópontokban (fizikai vagy virtuális OS példány)
- Ha hello tartalék tartományok és a frissítési tartományok alkotják a mátrix általában fut le hello átlói gépekkel "csíkozott" vagy "mátrix" modell

<center>
![Hiba és a frissítési tartomány elrendezések][Image4]
</center>

Nem ajánlott mely elrendezés toochoose választ, a mindegyike rendelkezik néhány előnyei és hátrányai. Például hello 1FD:1UD modell egyszerű tooset legfeljebb lesz. hello csomópont modell 1 frissítési tartományonkénti például milyen személyek használt legtöbb. Frissítések során minden egyes csomópont önállóan frissül. Ez a hasonló toohow kis csoportja gépek múltbeli hello a manuálisan frissített. 

hello leggyakoribb modell hello FD/UD mátrix, ahol hello FDs és UDs egy táblát, és csomópontok kerülnek mentén átlós hello indítása. Ez az alapértelmezés szerint a Service Fabric-fürtök az Azure-ban használt hello modell. A sok csomópontokkal rendelkező fürtök mindent fejeződik be a like hello sűrű mátrix fenti keresése.

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Hiba és a frissítési tartomány típusmegkötéseket és az eredményül kapott viselkedéstől
hello fürt erőforrás-kezelő szolgáltatás tookeep hiba és a frissítési tartományok korlátozásként között elosztott terhelésű hello desire kezeli. További információk a korlátozások található [Ez a cikk](service-fabric-cluster-resource-manager-management-integration.md). Hiba és a frissítési tartomány megkötések állapot hello: "egy adott szolgáltatáshoz partíció soha nem kell különbséget *nagyobb, mint egy* hello (állapotmentes szolgáltatások példányok vagy állapotalapú szolgáltatási replikák) szolgáltatás objektumok száma tartományok közötti." Ez megakadályozza, hogy bizonyos helyezi át, vagy a szabályok, amelyek megszegnek ennél a határértéknél.

Nézzük például. Tegyük fel, hogy rendelkezik-e öt tartalék tartományok és öt frissítési tartományok hat csomóponttal rendelkező fürt.

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | | |N3 | | |
| **UD3** | | | |N4 | |
| **UD4** | | | | |N5 |

Most tegyük fel, a szolgáltatás egy TargetReplicaSetSize (vagy, egy állapot nélküli szolgáltatáshoz az InstanceCount) létrehozhatunk öt. hello replikák N1-N5 léphet. N6 valójában soha nem használt függetlenül attól, hány szolgáltatások, például a hoz létre. De miért? Nézzük hello különbségének hello aktuális elrendezést, és mi történne N6 van kiválasztva.

A Microsoft készült, és hiba és a frissítési tartomány / replikák száma hello hello elrendezés itt található:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** | |R2 | | | |1 |
| **UD2** | | |R3 | | |1 |
| **UD3** | | | |R4 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Ebben az elrendezésben kiegyensúlyozott tartalék tartomány és a frissítési tartományi csomópontok tekintetében. Azt is kiegyensúlyozott hiba és a frissítési tartomány / replikák hello számát. Egyes tartományok csomópontok azonos számú hello és replikák azonos számú hello.

Most mi történne helyett N2 kellett használtuk N6 vizsgáljuk meg. Hogyan lesz replikák hello terjeszthetők majd?

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R5 | | | | |1 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |2 |0 |1 |1 |1 |- |

Ebben az elrendezésben megsérti a tartalék tartomány korlátozás hello definíciója. FD0 két replika van, míg FD1 nulla, felhasználásának hello különbség FD0 és FD1 összesen két. Ezzel az elrendezéssel hello fürt erőforrás-kezelő nem érhető el. Hasonlóképpen, ha azt kivételezett N2 és N6 (helyett N1 és N2) azt visszajelzést kap:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** | | | | | |0 |
| **UD1** |R5 |R1 | | | |2 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Ebben az elrendezésben kiegyensúlyozott tartalék tartományok tekintetében. Azonban most azt van a szabályt sértő hello frissítési tartomány korlátozás. Ennek az az oka UD0 rendelkezik nulla replikákat, míg két UD1 tartozik. Ezért az elrendezés is érvénytelen, és nem tárolható hello fürt erőforrás-kezelő által. 

## <a name="configuring-fault-and-upgrade-domains"></a>Hiba és a frissítési tartományok konfigurálása
Tartalék tartományok és a frissítési tartományok definiálása történik meg automatikusan az Azure Service Fabric központi telepítések üzemeltetett. A Service Fabric szerzi be, és hello környezet az adatokat az Azure-ból használja.

Ha saját fürt létrehozása folyamatban (vagy egy adott topológia a fejlesztési toorun), megadhatja hello tartalék tartomány és a frissítési tartomány információkat saját maga. Ebben a példában meghatároztuk a kilenc csomópont helyi fejlesztési fürtök három "adatközpontok" (mindnek három rackszekrények) is. A fürt ezen három üzemeltetésében csíkozott három frissítési tartomány is van. Hello konfigurációját nem éri el: 

ClusterManifest.xml

```xml
  <Infrastructure>
    <!-- IsScaleMin indicates that this cluster runs on one-box /one single server -->
    <WindowsServer IsScaleMin="true">
      <NodeList>
        <Node NodeName="Node01" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType01" FaultDomain="fd:/DC01/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node02" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType02" FaultDomain="fd:/DC01/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node03" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType03" FaultDomain="fd:/DC01/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node04" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType04" FaultDomain="fd:/DC02/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node05" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType05" FaultDomain="fd:/DC02/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node06" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType06" FaultDomain="fd:/DC02/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node07" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType07" FaultDomain="fd:/DC03/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node08" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType08" FaultDomain="fd:/DC03/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node09" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType09" FaultDomain="fd:/DC03/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
      </NodeList>
    </WindowsServer>
  </Infrastructure>
```

az önálló verziója telepítéseinek művelet keresztül

```json
"nodes": [
  {
    "nodeName": "vm1",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm2",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm3",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm4",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm5",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm6",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm7",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm8",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm9",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD3"
  }
],
```

> [!NOTE]
> Fürtök keresztül Azure Resource Manager meghatározásakor tartalék tartományok és a frissítési tartományok Azure rendeli hozzá. Hello meghatározása a csomóponttípusok és a virtuálisgép-méretezési csoportok az Azure Resource Manager sablon, ezért nem tartalmazza a tartalék tartomány és a frissítési tartomány adatait.
>

## <a name="node-properties-and-placement-constraints"></a>Csomópont tulajdonságai és elhelyezési korlátozás
Egyes esetekben (a gyakorlatban, hello legtöbbször ennek) fog toowant tooensure, amelyek bizonyos munkaterhelések csak bizonyos típusú hello fürt csomópontja. Például bizonyos alkalmazások és szolgáltatások igényelhet Feldolgozóegységekkel vagy SSD-k, míg mások számára nem engedélyezett. A nagy célcsoport-kezelési hardver tooparticular munkaterhelések példája ott szinte minden n szintű architektúra. Egyes gépek szolgál, előtér- vagy a kiszolgáló oldalán található hello alkalmazás API hello és elérhetőségi toohello ügyfelek vagy hello internet. Eltérő gépeken, gyakran különböző hardvererőforrás, a hello munkahelyi hello számítási és tárolási rétegek kezelni. Ezek rendszerint _nem_ közvetlenül elérhetővé tett tooclients vagy hello internet. A Service Fabric vár, hogy vannak-e esetben, amikor adott munkaterhelések kell-e az adott hardverkonfigurációk toorun. Példa:

* egy meglévő n szintű alkalmazást új "vissza, és vette" a Service Fabric környezetbe
* a munkaterhelés a adott hardverekhez toorun szeretne a teljesítmény, a méretezési vagy a biztonsági elkülönítés okok
* A munkaterhelés különítve a többi munkaterhelését házirend vagy az erőforrás felhasználási okokból kell lennie.

toosupport ezek a konfigurációk, rendezése Service Fabric rendelkezik, amely alkalmazott toonodes címkék első osztályú fogalmát. Ezekkel a címkékkel nevezzük **csomópont tulajdonságai**. **Egy elhelyezési korlátozás** hello utasítások csatolt tooindividual szolgáltatások, válasszon egy vagy több csomópont tulajdonságai. Egy elhelyezési korlátozás határozza meg, ahol szolgáltatásainak futnia kell. hello beállítása megkötéseket bővíthető – bármely kulcs/érték pár is működik. 

<center>
![A fürt különböző terhelésekhez elrendezés][Image5]
</center>

### <a name="built-in-node-properties"></a>A beépített csomópont tulajdonságai
A Service Fabric néhány alapértelmezett csomópont nélkül használható lesz automatikusan toodefine rendelkező hello felhasználói tulajdonságok határozza meg azokat. hello alapértelmezett tulajdonságok definiált minden egyes csomóponton: hello **NodeType** és hello **csomópontnév**. Így például lehet írni, mint egy elhelyezési korlátozás `"(NodeType == NodeType03)"`. Általában találtunk, amelyeknek a NodeType toobe leggyakrabban használt hello tulajdonságok egyikét. Ez a beállítás akkor hasznos, mivel a gép típusú megfelel 1:1. Minden számítógép típusú tooa típusú egy hagyományos n szintű alkalmazásban felel meg.

<center>
![Korlátozza és a csomópont tulajdonságai][Image6]
</center>

## <a name="placement-constraint-and-node-property-syntax"></a>Elhelyezési korlátozás és a csomópont tulajdonság szintaxis 
hello csomópont tulajdonságban megadott hello érték string, bool, lehet, vagy hosszú aláírással. hello nyilatkozatát hello szolgáltatást a neve egy elhelyezési *megkötés* óta korlátozza, ahol hello szolgáltatás futtathatók hello fürtben. hello megkötés hello másik csomópont-tulajdonságok hello fürtben működő logikai utasítás lehet. hello érvényes választók a logikai utasításokat a következők:

1) adott utasítások létrehozásához feltételes ellenőrzése

| Utasítás | Szintaxis |
| --- |:---:|
| "egyenlő" | "==" |
| "nem egyenlő" | "!=" |
| "nagyobb, mint" | ">" |
| "nagyobb, mint vagy egyenlő" | ">=" |
| "kisebb, mint" | "<" |
| "kisebb vagy egyenlő, mint" | "<=" |

2) logikai utasításokat a csoportosítási és logikai műveletek

| Utasítás | Szintaxis |
| --- |:---:|
| "és" | "&&" |
| "vagy" | "&#124;&#124;" |
| "nem" | "!" |
| "csoportot egyetlen utasításként" | "()" |

Az alábbiakban néhány olyan alapvető típusmegkötésein utasításokat.

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

Ahol hello teljes elhelyezési korlátozás utasítás kiértékelése túl "True" csak csomópontokat lehet rá hello szolgáltatást. Csomópontot, amely nincs meghatározva tulajdonsága nem egyezik a tartalmazó tulajdonság elhelyezési korlátozás.

Tegyük fel, hogy a következő tulajdonságok definiálva egy adott csomóponttípus csomópont hello:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType01">
      <PlacementProperties>
        <Property Name="HasSSD" Value="true"/>
        <Property Name="NodeColor" Value="green"/>
        <Property Name="SomeProperty" Value="5"/>
      </PlacementProperties>
    </NodeType>
```

az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök. 

> [!NOTE]
> Az Azure Resource Manager sablon hello csomópontban típus általában paraméteres. Az alábbihoz hasonlóan fog kinézni "[parameters('vmNodeType1Name')]" helyett "NodeType01".
>

```json
"nodeTypes": [
    {
        "name": "NodeType01",
        "placementProperties": {
            "HasSSD": "true",
            "NodeColor": "green",
            "SomeProperty": "5"
        },
    }
],
```

Szolgáltatás elhelyezésének hozhat létre *megkötések* egy szolgáltatás, például az alábbiak szerint:

C#

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
serviceDescription.PlacementConstraints = "(HasSSD == true && SomeProperty >= 4)";
// add other required servicedescription fields
//...
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceType -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementConstraint "HasSSD == true && SomeProperty >= 4"
```

Összes csomópontja NodeType01 érvényesek, ha igény szerint kiválaszthatja, hogy csomóponttípus hello megkötés "(NodeType == NodeType01)".

Ritkán használt adatok dolgot hello szolgáltatás placement Constraints korlátozásokat kapcsolatos egyik, hogy azok dinamikusan frissíthető futásidőben. Ezért ha kell, egy szolgáltatás Navigálás hello fürtben, hozzáadhat és eltávolítja a követelmények, stb. A Service Fabric gondoskodik arról, hogy hello szolgáltatás marad, és elérhető még ha ilyen jellegű módosítások végzett.

C#:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

PowerShell:

```posh
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

Minden elnevezett példány különböző Placement Constraints korlátozásokat vannak megadva. Frissítések mindig szükség van hello helye (felülírása) mi korábban lett megadva.

hello fürt definition hello tulajdonságainak meghatározása a csomóponton. A csomópont tulajdonságainak módosításához szükséges konfigurációs Fürtfrissítés. A csomópont tulajdonságainak frissítéséhez minden érintett csomópont toorestart tooreport új tulajdonságát. Ezek a működés közbeni frissítés a Service Fabric kezeli.

## <a name="describing-and-managing-cluster-resources"></a>Leíró, és a fürt erőforrások kezelése
Erőforrás-felhasználás hello fürt egyik legfontosabb feladatok bármely orchestrator toohelp hello kezelése. Fürt-erőforrások kezelése azt több különböző fogalom. Először is hiba van biztosításához, hogy a gépek nem túlterhelt. Ez azt jelenti, hogy meggyőződött arról, hogy, hogy gépek kezelni tud, mint szolgáltatás nem fut-e. Második nincs terheléselosztási és optimalizálási, ami kritikus fontosságú toorunning szolgáltatások hatékony. Költség hatályos vagy a teljesítmény-és nagybetűket szolgáltatásajánlatok nem engedélyezheti néhány csomópontok toobe kiemelt cold vannak. Működés közbeni csomópontok tooresource versenyt és a gyenge teljesítményt, és a cold csomópontok kihasználatlan jelentik erőforrások és a költségek vezethet. 

A Service Fabric jelöli az erőforrásokhoz, mint `Metrics`. Adatok gyűjtése le a logikai és fizikai erőforrás, amelyet az toodescribe tooService háló. Metrikák Példák többek között a "WorkQueueDepth" vagy "MemoryInMb". Csomópontok hello fizikai erőforrásokat, amelyek képesek felügyelni a Service Fabric kapcsolatos információkért lásd: [erőforrás irányítás](service-fabric-resource-governance.md). Egyéni metrikák és a használatukat konfigurálásával kapcsolatos további információkért lásd: [Ez a cikk](service-fabric-cluster-resource-manager-metrics.md)

Adatok gyűjtése le eltérő adatközpontokon korlátozások és a csomópont tulajdonságait. Csomópont tulajdonságai statikus leírók hello csomópontok magukat. Metrikák csomópontok rendelkező erőforrásokat és, hogy a szolgáltatások felhasználásához csomóponton futtatásakor ismertetik. A csomópont-tulajdonságok "HasSSD" lehet, és tootrue, és hamis értéket kell beállítani. hello szabad lemezterület, hogy az SSD és a szolgáltatások mennyi felhasznált például a "DriveSpaceInMb" metrika lenne. 

Fontos, hogy akárcsak egy elhelyezési korlátozás és a csomópont tulajdonságait, hello Service Fabric fürt erőforrás-kezelő nem ismeri a milyen hello neveket hello metrikák középérték toonote. Metrika nevében csak karakterláncok számítanak. Egy jó gyakorlat toodeclare egységek hello metrika neve nem egyértelmű lehet létrehozott részeként is.

## <a name="capacity"></a>Kapacitás
Ha kikapcsolt minden erőforrás *terheléselosztási*, a Service Fabric-fürt erőforrás-kezelő továbbra is biztosítja, hogy nincs csomópont véget ért a kapacitás keresztül. Kapacitás meghaladása kezelése használata lehetséges, kivéve, ha hello fürtön nincs elég hely, vagy hello munkaterhelés nagyobb, mint egyetlen csomópont. Egy másik *megkötés* adott hello fürt erőforrás-kezelő használja toounderstand milyen egy erőforrást egy csomópont van. Fennmaradó kapacitás is nyomon követi hello fürt egésze. Hello kapacitás és a hello fogyasztás hello szolgáltatás szintű metrikák vannak kifejezve. Így például hello metrika "ClientConnections" lehet, és egy adott csomópont lehet egy "ClientConnections" 32768 tartozó kapacitás. Más csomópontok más korlátok néhány szolgáltatás fut az adott csomópont is mondja ki azt az jelenleg fel 32256 hello mérőszám "ClientConnections" lehet.

Hello fürt erőforrás-kezelő futásidőben, nyomon követi a fennmaradó kapacitás hello fürt és a csomóponton. A rendelés tootrack kapacitás hello fürt erőforrás-kezelő kivonja a csomópont-kapacitás hello szolgáltatást futtató minden egyes szolgáltatás használati. Az információ hello Service Fabric fürt erőforrás-kezelő kitalálja, hogy hol tooplace vagy helyezze át a replikákat, hogy a csomópontok kapacitása nem ismerteti.

<center>
![Fürtcsomópontok és a kapacitás][Image7]
</center>

C#:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
ServiceLoadMetricDescription metric = new ServiceLoadMetricDescription();
metric.Name = "ClientConnections";
metric.PrimaryDefaultLoad = 1024;
metric.SecondaryDefaultLoad = 0;
metric.Weight = ServiceLoadMetricWeight.High;
serviceDescription.Metrics.Add(metric);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ClientConnections,High,1024,0)
```

Hello a fürtjegyzékben megadott kapacitások látható:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType03">
      <Capacities>
        <Capacity Name="ClientConnections" Value="65536"/>
      </Capacities>
    </NodeType>
```

az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök. 

```json
"nodeTypes": [
    {
        "name": "NodeType03",
        "capacities": {
            "ClientConnections": "65536",
        }
    }
],
```

Általában az adott szolgáltatás módosításának betöltése dinamikusan. Tegyük fel például, hogy a replika betöltése "ClientConnections" 1024 too2048, de azt kiszolgálón már futott, akkor csak az adott mérőszám fennmaradó 512 kapacitás kellett hello csomópont változása. Most már a adott másodpéldány vagy a példány elhelyezési érvénytelen, mert nincs elég hely a csomóponton. Fürt erőforrás-kezelő hello tookick rendelkezik, és vissza a kapacitás alatt hello csomópont beolvasása. Hello csomópont, amelyhez kapacitás keresztül egy vagy több hello replikák és példányok tér át, hogy tooother csomópontok terhelése csökkenti. Replikák áthelyezésekor hello fürt erőforrás-kezelő megpróbál ilyen mozgást toominimize hello költségét. A mozgás költsége ismertet [Ez a cikk](service-fabric-cluster-resource-manager-movement-cost.md) több kapcsolatos hello fürt erőforrás-kezelő stratégiák tartozó kiegyenlítését és szabályok leírt [Itt](service-fabric-cluster-resource-manager-metrics.md).

## <a name="cluster-capacity"></a>Fürt kapacitás
Igen, hogyan nem hello Service Fabric fürt erőforrás-kezelő megtartása hello általános fürt nem túl teljes? A dinamikus terhelés nincs sokkal lehet hasznos. Szolgáltatások lehet a terhelés csúcs hello fürt erőforrás-kezelő által végrehajtott műveletek függetlenül. Emiatt a fürt bőven ma belső magasságnak lehet underpowered válásának Ön famous holnap. Említett, vannak bizonyos vezérlők, amely a tooprevent problémák vannak bővíthetőség. hello elsőként tehetünk ennek az új típusoktól, amelyek hello fürt toobecome teljes hello létrehozásának megakadályozása.

Tegyük fel például, hogy állapot nélküli szolgáltatás, és néhány terhelés társítva van. Tegyük fel, hogy hello szolgáltatás ügyel hello "DiskSpaceInMb" metrika. Tételezzük is fel, hogy a rendszer folyamatban lévő tooconsume öt egységeinek hello szolgáltatást mindegyik példányhoz "DiskSpaceInMb". Három alkalmazáspéldányra toocreate hello szolgáltatást szeretné. Remek! Úgy, hogy azt jelenti, hogy a "DiskSpaceInMb" toobe 15 egységek kell ahhoz, hogy velünk hello fürt tooeven van kell ezeket szolgáltatáspéldány képes toocreate. hello fürt erőforrás-kezelő folyamatosan kiszámítja hello kapacitás, és mindegyik metrikát fogyasztásának így meg tudja határozni hello fennmaradó kapacitás hello fürtben. Ha nincs elég hely, erőforrás-kezelő fürt elutasítások hello hello létrehozása szolgáltatás hívása.

Hello követelmény, amely csak 15 egység érhető el, mert ez a terület rendelhető számos különböző módja. Például lehet egy fennmaradó egység 15 különböző csomópontokon kapacitás vagy három fennmaradó egységek öt különböző csomópontokon kapacitást. Ha a fürt erőforrás-kezelő hello is átrendezéséhez dolgot úgy érhető el öt egység három csomóponton helyezi hello szolgáltatást. Átrendezése hello fürt általában lehetőség, kivéve, ha hello fürt majdnem megtelt, vagy valamilyen okból kifolyólag nem lehet összevont hello meglévő szolgáltatások.

## <a name="buffered-capacity"></a>A pufferelt kapacitás
A pufferelt kapacitása hello erőforrás-kezelő fürt egy másik szolgáltatása. Lehetővé teszi a Foglalás hello egy részét az általános csomópont kapacitását. A kapacitás puffer csak használt tooplace szolgáltatások frissítéseket és a csomópont hibák alatt. A metrika az összes csomópont pufferelt kapacitás globálisan van meghatározva. hello hello szolgáltatás számára fenntartott kapacitás választott értéke hiba és hello fürt rendelkezik frissítési tartományok száma hello függvényében. Több hiba és frissítési tartományok azt jelenti, hogy kevesebb kiválaszthatja a pufferelt kapacitást. Ha több tartományban vannak, számíthat a fürt toobe kisebb mennyiségű nem érhető el frissítéseket és a hibák alatt. Kapacitás pufferelt megadása csak értelme, ha akkor is megadott hello csomópont-kapacitás olyan metrikajelentés.

Íme egy példa hogyan toospecify pufferelt kapacitás:

ClusterManifest.xml

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="SomeMetric" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök:

```json
"fabricSettings": [
  {
    "name": "NodeBufferPercentage",
    "parameters": [
      {
          "name": "SomeMetric",
          "value": "0.15"
      },
      {
          "name": "SomeOtherMetric",
          "value": "0.20"
      }
    ]
  }
]
```

az új szolgáltatások hello létrehozás sikertelen lesz, amikor hello fürt olyan metrikajelentés pufferelt kapacitás kívül esik. Megakadályozza az új szolgáltatások toopreserve hello puffer hello létrehozása biztosítja, hogy frissítéseket és a hibák nem okozhat csomópontok toogo kapacitás keresztül. A pufferelt kapacitás nem kötelező, de ajánlott a fürt, amely meghatározza egy olyan metrikajelentés kapacitás.

hello fürt erőforrás-kezelő teszi közzé a terhelés kapcsolatos információkat. Mindegyik metrikát az alábbiakat: 
  - hello pufferelt beállításait
  - hello teljes kapacitás
  - hello aktuális felhasználás
  - vagy nem mindegyik metrikát tekinthető e rendszerrel
  - hello szórás statisztikája
  - hello csomópontokat, amelyeknek hello rendelkezik a legtöbb és legalább betöltése  
  
Az alábbiakban egy példa kimenet látható:

```posh
PS C:\Users\user> Get-ServiceFabricClusterLoadInformation
LastBalancingStartTimeUtc : 9/1/2016 12:54:59 AM
LastBalancingEndTimeUtc   : 9/1/2016 12:54:59 AM
LoadMetricInformation     :
                            LoadMetricName        : Metric1
                            IsBalancedBefore      : False
                            IsBalancedAfter       : False
                            DeviationBefore       : 0.192450089729875
                            DeviationAfter        : 0.192450089729875
                            BalancingThreshold    : 1
                            Action                : NoActionNeeded
                            ActivityThreshold     : 0
                            ClusterCapacity       : 189
                            ClusterLoad           : 45
                            ClusterRemainingCapacity : 144
                            NodeBufferPercentage  : 10
                            ClusterBufferedCapacity : 170
                            ClusterRemainingBufferedCapacity : 125
                            ClusterCapacityViolation : False
                            MinNodeLoadValue      : 0
                            MinNodeLoadNodeId     : 3ea71e8e01f4b0999b121abcbf27d74d
                            MaxNodeLoadValue      : 15
                            MaxNodeLoadNodeId     : 2cc648b6770be1bc9824fa995d5b68b1
```

## <a name="next-steps"></a>Következő lépések
* Hello architektúra és információk folyamata belül hello fürt erőforrás-kezelő információkért tekintse meg [Ez a cikk](service-fabric-cluster-resource-manager-architecture.md)
* Töredezettségmentesítés metrikák meghatározása a csomópont helyett ezzel az egyirányú tooconsolidate terhelése. toolearn hogyan tooconfigure töredezettségmentesítés, tekintse meg a túl[Ez a cikk](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
* Indítsa el a hello elejétől és [egy bevezető toohello Service Fabric fürt erőforrás-kezelő beolvasása](service-fabric-cluster-resource-manager-introduction.md)
* toofind meg arról, hogyan hello fürt erőforrás-kezelő felügyeli, és elosztja a terhelést hello fürtben, tekintse meg hello a cikk a [terheléselosztás](service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png
