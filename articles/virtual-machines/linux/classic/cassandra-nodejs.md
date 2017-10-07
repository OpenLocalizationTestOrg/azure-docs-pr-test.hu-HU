---
title: az Azure-on Linux Cassandra aaaRun |} Microsoft Docs
description: "Hogyan toorun egy Cassandra fürt Linux Azure Virtual Machines egy Node.js-alkalmazás"
services: virtual-machines-linux
documentationcenter: nodejs
author: tomarcher
manager: routlaw
editor: 
tags: azure-service-management
ms.assetid: 30de1f29-e97d-492f-ae34-41ec83488de0
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 381ca301bbe88d3740cf182f9c44fada5b9ba7cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="running-cassandra-with-linux-on-azure-and-accessing-it-from-nodejs"></a>A linuxos Cassandra futtatása az Azure-ban és az alkalmazás Node.js-ből való elérése
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Tekintse meg a Resource Manager-sablonok [alapszintű Datastax vállalati](https://azure.microsoft.com/documentation/templates/datastax) és [Spark-fürt és Cassandra a CentOS](https://azure.microsoft.com/documentation/templates/spark-and-cassandra-on-centos/).

## <a name="overview"></a>Áttekintés
Microsoft Azure a egy megnyitott felhőalapú platform, amely a Microsoft fut, és a nem Microsoft szoftvert, amely operációs rendszerek, alkalmazás-kiszolgálókat, üzenetküldési köztes, valamint SQL és a NoSQL-adatbázisok mindkét kereskedelmi és nyissa meg a forrás-modellek. A nyilvános felhők, beleértve az Azure rugalmas szolgáltatások igényel gondos tervezéssel és szándékos architektúra mindkét alkalmazáskiszolgálók jól tárolási rétegek. Cassandra által elosztott tároló-architektúra természetes segítségével magas rendelkezésre állású rendszerek, amelyek a fürt hibák hibatűrő fejlesztése során. Cassandra egy felhőhöz méretezett NoSQL-adatbázis által Apache szoftver Foundation költséget cassandra.apache.org; Cassandra Java nyelven van megírva, és ezért mind a Windows, Linux futtatja platformokon.

hello Ez a cikk célja a Microsoft Azure virtuális gépek és virtuális hálózatok egyetlen és több data center fürtként Ubuntu tooshow Cassandra telepítés. hello fürttelepítés optimalizált termelési számítási feladatokhoz hatókörén kívül esik a cikk több lemez a csomópont-konfiguráció szükséges, megfelelő körgyűrűs topológia tervezési és toosupport hello modellezési szükséges replikációs, az adatok konzisztenciájának, átviteli sebesség és magas rendelkezésre állási követelményeinek.

Ez a cikk vesz egy alapvető megközelítés tooshow, mi is szerepet kap az épület hello Cassandra fürt képest Docker, Chef vagy Puppet, amely hello infrastruktúra telepítése sokkal egyszerűbbé teheti.  

## <a name="hello-deployment-models"></a>üzembe helyezési modellel hello
A Microsoft Azure hálózatkezelés lehetővé teszi, hogy a elkülönített titkos fürtök esetén, amelyek hello hozzáférést lehet korlátozott tooattain finom nyomtatott hálózati biztonsági hello telepítését.  Mivel ez a cikk hello Cassandra telepítési alapvető szinten megjelenítésével kapcsolatos, nem tárgyaljuk hello konzisztenciaszint és hello optimális tárolókialakítás átviteli sebesség eléréséhez. A képzeletbeli fürt hálózati követelményei hello listája itt található:

* Külső rendszerek Cassandra adatbázis belül vagy kívül Azure nem tud hozzáférni.
* Cassandra fürt toobe thrift-forgalom egy terheléselosztó mögött van
* Minden adatközpontot továbbfejlesztett fürt rendelkezésre állási csoportok két csomópontja Cassandra telepítése
* Zárolását hello fürt úgy, hogy csak az alkalmazás kiszolgálófarm hozzáférés toohello adatbázis közvetlenül van
* Eltérő SSH nyilvános hálózati végpontok
* Minden egyes Cassandra csomópont kell egy rögzített belső IP-cím

Cassandra telepített tooa egyetlen Azure-régiót vagy toomultiple régiók hello munkaterhelés hello elosztott jellege alapján lehet. Több régióban üzembe helyezési modellel lehet tooserve kihasználhatók a végfelhasználók szorosabb tooa adott földrajzi keresztül hello Cassandra ugyanabban az infrastruktúrában. Cassandra meg beépített csomópont replikációs vesz többszörös főkiszolgáló hello szinkronizálás több különböző adatközponthoz származó ír és egységes hello adatok tooapplications ablak jeleníti. Több területi központi telepítés is segíthetnek a hello kockázatcsökkentés a hello szélesebb körű Azure szolgáltatás-kimaradások számát. Cassandra tartozó hangolható konzisztencia és a replikációs topológia segít az alkalmazások különböző Helyreállítási igényeket.

### <a name="single-region-deployment"></a>Egyetlen régión központi telepítés
Program először egy régió egyetlen központi telepítési és betakarítás hello learnings több területi modell létrehozásához. Az Azure virtuális hálózat lesz elkülönített használt toocreate alhálózatok, hogy a fent említett hello hálózatbiztonsági követelményei érheti el.  hello egyetlen régión központi telepítés létrehozása bemutatott hello folyamat használja az Ubuntu 14.04 LTS és Cassandra 2.08; azonban hello folyamat könnyen lehet elfogadott toohello más Linux Variant adattípusban. hello közé tartoznak a következők hello rendszeres hello egyetlen régión telepítési jellemzőit.  

**Magas rendelkezésre állás:** hello Cassandra csomópontok látható az 1. ábra telepített hello tootwo rendelkezésre állási készletek, hogy hello csomópont a magas rendelkezésre állású több tartalék tartományok között van elosztva. Virtuális gépek minden egyes rendelkezésre állási csoport attribútummal megfeleltetve too2 tartalék tartományok.  A Microsoft Azure által használt hello fogalma tartalék tartomány toomanage állásidő (pl. a hardver- vagy hibák) közben (pl. gazdagép vagy vendég operációs rendszer javítás vagy frissítés, alkalmazásfrissítések) frissítési tartomány hello fogalma nem tervezett kezelésére állásidő ütemezett használt. Ellenőrizze a [vész-helyreállítási és magas rendelkezésre állás, az Azure-alkalmazások](http://msdn.microsoft.com/library/dn251004.aspx) hello szerepkör hiba és a frissítési tartományok magas rendelkezésre állás eléréséhez.

![Egyetlen régión központi telepítés](./media/cassandra-nodejs/cassandra-linux1.png)

1. ábra: Egy régiót központi telepítés

Vegye figyelembe, hogy írásának hello időpontban Azure nem engedélyezi a virtuális gépek tooa adott tartalék tartomány; csoportja hello explicit leképezése Következésképpen még hello telepítési modell 1. ábrán látható, a valószínű statisztikailag, hogy az összes hello virtuális gép csatlakoztatott tootwo tartalék tartományok helyett négy lehetnek.

**Terheléselosztás Thrift forgalmi betöltése:** Thrift ügyfél függvénytárainak belül hello webkiszolgáló toohello fürt csatlakoznak a belső terheléselosztót. Ehhez hello folyamata hello belső terheléselosztó toohello "data" alhálózat hozzáadása (lásd az 1. ábra) hello Cassandra fürtöt hello felhőszolgáltatás hello környezetében. Belső terheléselosztó hello van definiálva, miután minden csomópont van szükség, hello megjegyzések egy elosztott terhelésű készlet a korábban hozzáadott hello elosztott terhelésű végpont toobe meghatározott terheléselosztó neve. Lásd: [Azure belső terheléselosztás ](../../../load-balancer/load-balancer-internal-overview.md)további részleteket.

**Fürt magok:** fontos tooselect hello legtöbb magas rendelkezésre állású csomópontok az mag, az új csomópontok hello kezdőérték csomópontok toodiscover hello topológia hello fürt fognak kommunikálni. Egyes rendelkezésre állási csoport egy csomópont kijelölt kezdőérték csomópontok tooavoid hibaérzékeny pontok kialakulását.

**Replikációs tényező és Konzisztenciaszint:** Cassandra a beépített magas rendelkezésre állású és az adatok tartóssága jellemzőek hello replikációs tényező (RF - példányszámot az egyes sorok hello fürtön tárolt), és a Konzisztenciaszint (száma replikák toobe hello eredmény toohello hívó visszatérése előtt olvasása/írása). Replikációs tényező hello kulcstérértesítések használatával (hasonlóan tooa relációs adatbázis) létrehozása közben megadott, mivel hello konzisztenciaszint hello CRUD-lekérdezés elküldése során van megadva. Lásd a Cassandra dokumentációját a [konfigurálásával konzisztenciájának](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) konzisztencia részletek és hello képlet kvórum számításhoz.

Cassandra kétféle adatok integritásának modellek – konzisztencia és a végleges konzisztencia; hello replikációs tényező és Konzisztenciaszint együtt meghatározzák, ha hello adatok lesz konzisztens, amint egy írási művelet befejeződik, vagy idővel konzisztenssé lesz. Például KVÓRUM megadó Konzisztenciaszint mindig lesz hello biztosítja az adatok konzisztencia során minden konzisztencia szint alatt szükséges tooattain megírva replikák toobe hello száma, hogy idővel konzisztenssé adatok eredményezi KVÓRUM (pl. egy).

3-KVÓRUM replikációs tényezővel a fent látható hello 8 csomópontos fürt (2 csomópontok vannak olvasása vagy írása konzisztencia) olvasási/írási konzisztenciaszint, hibatűrését hello elméleti megszűnését hello legtöbb 1 csomópont egy replikációs csoport hello alkalmazás indítása előtt: hello hiba észre. A parancs feltételezi, hogy minden hello kulcs szóközt kell jól kiegyensúlyozott olvasási/írási kérések.  Az alábbiakban hello hello paraméterek telepített hello fürt használjuk:

Egyetlen régión Cassandra fürtkonfiguráció:

| Fürt paraméter | Érték | Megjegyzések |
| --- | --- | --- |
| (N) csomópontok száma |8 |Hello fürtben található csomópontok száma |
| Replikációs tényező (RF) |3 |Adott sor replikák száma |
| Konzisztenciaszint (írás) |QUORUM[(RF/2) +1) = 2] hello eredmény hello a képlet lefelé kerekíti |Írja a hello legtöbb 2 replikával hello válasz toohello hívó elküldése előtt 3. a replika idővel konzisztenssé módon írása. |
| Konzisztenciaszint (olvasás) |KVÓRUM [(RF/2) + 1 = 2] hello képlet hello eredményét lefelé kerekíti |Beolvassa a 2-replikával válasz toohello hívó elküldése előtt. |
| Replikációs stratégia |NetworkTopologyStrategy lásd [adatreplikáció](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) Cassandra dokumentációjában talál további információt a |Hello telepítési topológia megértette és replikák helyezi a csomóponton, hogy az összes hello replika nem végül a hello azonos állvány |
| Snitch |Lásd a GossipingPropertyFileSnitch [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) Cassandra dokumentációjában talál további információt a |NetworkTopologyStrategy snitch toounderstand hello topológia egy fogalom használja. GossipingPropertyFileSnitch a minden egyes csomópont toodata center és állvány leképezésekor nagyobb ellenőrzést biztosít. hello fürt pletykákat toopropagate ezt az információt használja. Ez az jóval egyszerűbbé válik a dinamikus IP beállítás relatív tooPropertyFileSnitch |

**Cassandra fürt Azure szempontjai:** Microsoft Azure virtuális gépek funkció Azure Blob Storage tárolót használja a lemez megőrzéséhez; Az Azure Storage minden lemez a magas tartósság 3 replikák menti. Ez azt jelenti, hogy minden egyes soraiban levő adatok, a Cassandra táblába 3 replikák már tárolja, és ezért adatkonzisztencia van már hozott megvagyunk akkor is, ha hello replikációs tényező (RF) 1. hello fő problémája replikációs tényező 1 folyamatban, hogy az alkalmazás hello Leállás következik, még akkor is, ha egy önálló Cassandra csomópont meghibásodik. Azonban ha egy csomópont le a hello problémákat (például a hardver, a szoftver rendszerhibák) ismeri fel az Azure Fabric Controller, az kiépíti a hely használatával új csomópontjának azonos tárolóeszközöket hello. Kiépítés egy új csomópont tooreplace hello régi egy eltarthat néhány percig.  Tervezett karbantartás tevékenységek, például a vendég operációs rendszer módosításokat, hasonlóképpen Cassandra frissíti, és alkalmazás Azure Fabric Controller hajtja végre a működés közbeni frissítés hello csomópontok hello fürtben.  Működés közbeni frissítés is is igénybe vehet néhány csomópont le egyszerre, és ezért hello fürt problémákat tapasztalhat a néhány partíciók rövid idejű leállást. Hello adatok azonban nem lesznek toohello beépített Azure adattároló redundanciája, amely miatt megszakadt.  

A rendszerek, amelyek nem igényelnek magas rendelkezésre állású tooAzure telepített (pl. körülbelül 99,9 amely egyenértékű too8.76 óra/év; lásd: [magas rendelkezésre állású](http://en.wikipedia.org/wiki/High_availability) részletek) rendelkező RF képes toorun esetleg = 1 és Konzisztenciaszint = egy.  A magas rendelkezésre állású KÖVETELMÉNYŰ RF alkalmazások = 3 és Konzisztenciaszint = KVÓRUM hello hello csomópontok egy hello replikák közül az egyik állásidő lesz működését. RF = 1, a hagyományos telepítések (pl. a helyszíni) toohello esetleges adatvesztés eredő problémák, mint például a lemezek meghibásodása miatt nem használható.   

## <a name="multi-region-deployment"></a>Több területi központi telepítés
Bármely külső tooling Cassandra tartozó adatok-center-kompatibilis replikációs és konzisztencia-modell segítségével hello több területi telepítés nélkül hello hello kezdő verzióról a fent leírt van szükség. Ez eltér meglehetősen hello hagyományos, relációs adatbázisok ahol hello beállítása az adatbázis-tükrözésre több főkiszolgálós írások elég bonyolult lehet. Állítson be több régióban Cassandra hello használati forgatókönyvei többek között a következő hello is segítséget nyújt:

**Közelségi kapcsolat alapú központi telepítés:** több-bérlős alkalmazásokhoz, és törölje a jelet a bérlő felhasználók-az-régió, is származott, alacsony késleltetésű hello több területi fürt által. Például egy tanulási felügyeleti rendszerek oktatási intézményeknél a az USA keleti régiója, és az USA nyugati régiója régiók tooserve hello megfelelő kampuszok az elosztott fürtök telepítése tranzakciós, továbbá az elemzés. hello adatok lehetnek helyileg konzisztens hello idő olvasása és írása, és idővel konzisztenssé mindkét hello régiók között. További példák mint az adathordozó terjesztési, elektronikus kereskedelmi és semmit, és koncentrált földrajzi felhasználói alap látja, hogy egy megfelelő használati eset a központi telepítési modell.

**Magas rendelkezésre állás:** redundancia szoftverek és hardverek magas rendelkezésre állás elérése kulcsfontosságú tényező; a részleteket lásd a Microsoft Azure épület megbízható a felhőalapú rendszerek. A Microsoft Azure-hello csak megbízható igaz redundancia elérésének módja több területi fürt üzembe helyezésével. Egy aktív-aktív vagy aktív-passzív módban alkalmazások telepíthetők, és hello régiók egyikéhez sem nem működik, ha Azure Traffic Manager irányíthatja a forgalmat toohello aktív terület.  Hello egyetlen régión központi telepítésére, ha hello rendelkezésre állás 99,9, a két-régió központi telepítés el tud érni hello képlettel kiszámított 99.9999 a rendelkezésre állási: (1-(1-0.999) * (1-0.999)) * 100); Lásd a dokumentum részletes fent hello.

**Vész-helyreállítási:** több területi Cassandra fürt, ha megfelelően, is képes elviselni katasztrofális data center kimaradások esetén. Ha egy régió nem működik, hello központilag telepített alkalmazás tooother régiók indításához hello végfelhasználók szolgáltató. Bármely más üzleti folytonossági megvalósításokhoz, például a hello alkalmazásnak hello adatok hello aszinkron feldolgozási eredő adatvesztést a hibatűrő toobe. Cassandra azonban hello helyreállítási sokkal zavartalanabbá, mint a hagyományos adatbázis helyreállítási folyamatok által igénybe vett hello idő. 2. ábrán minden régióban hello tipikus több régióban üzembe helyezési modellben a nyolc csomópont látható. Mindkét régiók a következők tükör lemezképeket minden más hello azonos szimmetriasíkjához; valós tervek hello munkaterhelésének típusát (pl. tranzakciós vagy analitikai), a helyreállítási Időkorlát, a RTO, a adatkonzisztenciát és a rendelkezésre állási követelményektől függenek.

![Több régióban központi telepítés](./media/cassandra-nodejs/cassandra-linux2.png)

2. ábra: Több területi Cassandra központi telepítés

### <a name="network-integration"></a>Hálózati integráció
Beállítja a virtuális gépek két régiókban található telepített tooprivate hálózatok egymáshoz VPN-alagúton használatával kommunikál. hello VPN-alagút két szoftver átjáró hello hálózati telepítési folyamat során létesített kapcsolatot. Mindkét memóriaterületnél hasonló hálózati architektúra szempontjából "web" és "data" alhálózat; Az Azure hálózatkezelés lehetővé teszi, hogy a lehető legtöbb alhálózatok igény szerint hello létrehozása, és alkalmazni a hozzáférés-vezérlési listákat, igényei szerint hálózati biztonság. Hello fürtjének topológiája tervezésekor többek adatok center kommunikációs késés és hello gazdasági hatása hello hálózati forgalmat kell toobe számít.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Adatok konzisztenciájának több adatközpont központi telepítéshez
Az elosztott központi telepítések kell toobe tisztában hello fürt topológia gyakorolt átviteli sebesség és a magas rendelkezésre állású legyen. hello RF és Konzisztenciaszint kell toobe, ilyen módon, amely a kvórum hello kijelölt összes hello adatközpontok hello rendelkezésre állását sem függ.
A rendszer, amelyet a magas konzisztencia, a LOCAL_QUORUM konzisztenciaszint (az olvasási és írási) fog győződjön meg arról, hogy hello helyi beolvassa és írási műveletek teljesülnek a helyi hello csomópontok, adatok replikálása aszinkron módon történik toohello távoli adatközpontokban.  2. táblázat hello leírt hello több területi fürt részletes Microsoft Word hello konfiguráció összegzése látható.

**A két-régió Cassandra fürtkonfiguráció**

| Fürt paraméter | Érték | Megjegyzések |
| --- | --- | --- |
| (N) csomópontok száma |8 + 8 |Hello fürtben található csomópontok száma |
| Replikációs tényező (RF) |3 |Adott sor replikák száma |
| Konzisztenciaszint (írás) |LOCAL_QUORUM [(sum(RF)/2) +1) = 4] hello képlet hello eredményét lefelé kerekíti |2 csomópontok lesz írva toohello első adatközpont szinkron módon történik; hello további 2 csomópontok kvórum szükség lesz írva aszinkron módon toohello 2. az Adatközpont. |
| Konzisztenciaszint (olvasás) |LOCAL_QUORUM ((RF/2) + 1) = 2 hello képlet hello eredményét lefelé kerekíti |Olvasási kérések teljesülnek a csak egy régió tartozik; 2 csomópontja hátsó toohello ügyfél hello válasz elküldése előtt olvasható. |
| Replikációs stratégia |NetworkTopologyStrategy lásd [adatreplikáció](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) Cassandra dokumentációjában talál további információt a |Hello telepítési topológia megértette és replikák helyezi a csomóponton, hogy az összes hello replika nem végül a hello azonos állvány |
| Snitch |Lásd a GossipingPropertyFileSnitch [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) Cassandra dokumentációjában talál további információt a |NetworkTopologyStrategy snitch toounderstand hello topológia egy fogalom használja. GossipingPropertyFileSnitch a minden egyes csomópont toodata center és állvány leképezésekor nagyobb ellenőrzést biztosít. hello fürt pletykákat toopropagate ezt az információt használja. Ez az jóval egyszerűbbé válik a dinamikus IP beállítás relatív tooPropertyFileSnitch |

## <a name="hello-software-configuration"></a>hello SZOFTVERKONFIGURÁCIÓJÁNAK összeállítása
a következő szoftververziók hello hello központi telepítése során használhatók:

<table>
<tr><th>Szoftver</th><th>Forrás</th><th>Verzió</th></tr>
<tr><td>JRE    </td><td>[8 JRE](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA    </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu    </td><td>[A Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 LTS</td></tr>
</table>

Az Oracle-licencet, toosimplify hello központi telepítés, az összes szükséges szoftverek toohello asztal lemezképpel hello Ubuntu sablon azt létrehozni prekurzor toohello fürtként később feltöltése hello letöltési manuális elfogadását JRE letöltésének szükséges. központi telepítés.

Töltse le a fenti szoftverek hello könyvtárba, amely egy jól ismert letöltési (pl. Windows %TEMP%/downloads vagy legtöbb Linux terjesztésekről vagy Mac ~/Downloads) hello helyi számítógépen.

### <a name="create-ubuntu-vm"></a>UBUNTU VIRTUÁLIS GÉP LÉTREHOZÁSA
Ebben a lépésben hello folyamat létrehozunk Ubuntu kép hello előfeltételt jelentő szoftvereket a úgy, hogy hello lemezkép felhasználható számos Cassandra csomópont történő üzembe helyezéséhez.  

#### <a name="step-1-generate-ssh-key-pair"></a>1. lépés: Az SSH-kulcspárral létrehozása
Azure egy nyilvános kulcsot, amely PEM vagy DER kódolású idő kiépítés hello: X509 kér. Milyen található hello utasításokat követve nyilvános/titkos kulcspárt létre tooUse SSH Linux Azure-on. Ha azt tervezi, toouse putty.exe Windows vagy Linux SSH-ügyfélként, hogy tooconvert hello PEM kódolású RSA titkos kulcs tooPPK formátum használatával puttygen.exe; hello utasításokat a fenti weblap hello találhatók.

#### <a name="step-2-create-ubuntu-template-vm"></a>2. lépés: Sablon Ubuntu virtuális gép
toocreate hello sablon virtuális Gépet, jelentkezzen be hello Azure klasszikus portál és -felhasználási hello a következő feladatütemezési: kattintson az új, számítási, virtuális gép, FROM GYŰJTEMÉNYE, UBUNTU, Ubuntu Server 14.04 LTS, majd a hello jobbra mutató nyílra. Egy oktatóanyag, amely leírja, hogyan toocreate Linux virtuális gép: hozzon létre egy virtuális gép futó Linux.

Adja meg a következő információ hello "virtuálisgép-konfiguráció" képernyőn #1 hello:

<table>
<tr><th>MEZŐ NEVE              </td><td>       MEZŐÉRTÉK               </td><td>         MEGJEGYZÉSEK                </td><tr>
<tr><td>VERZIÓ KIADÁSI DÁTUM    </td><td> Válassza ki a megfelelő hello legördülő dátuma</td><td></td><tr>
<tr><td>VIRTUÁLIS GÉP NEVE    </td><td> esetén-sablon                   </td><td> Ez a virtuális gép hello hello állomásnevét </td><tr>
<tr><td>RÉTEG                     </td><td> STANDARD                           </td><td> Hello alapértelmezett hagyja              </td><tr>
<tr><td>MÉRET                     </td><td> A1                              </td><td>Virtuális gép alapján hello IO válassza hello kell; erre a célra hagyja hello alapértelmezett </td><tr>
<tr><td> ÚJ FELHASZNÁLÓ NEVE             </td><td> localadmin                       </td><td> "rendszergazda" az egyetlen foglalt felhasználónévvel Ubuntu 12. xx és után</td><tr>
<tr><td> HITELESÍTÉS         </td><td> Jelölje be jelölőnégyzetet                 </td><td>Ellenőrizze, hogy ha azt szeretné toosecure SSH-kulcs </td><tr>
<tr><td> TANÚSÍTVÁNY             </td><td> hello nyilvános kulcsú tanúsítvány fájlneve </td><td> Korábban létrehozott hello nyilvános kulcs használata</td><tr>
<tr><td> Új jelszó    </td><td> erős jelszó </td><td> </td><tr>
<tr><td> Jelszó megerősítése    </td><td> erős jelszó </td><td></td><tr>
</table>

Adja meg a következő információ hello "virtuálisgép-konfiguráció" képernyőn #2 hello:

<table>
<tr><th>MEZŐ NEVE             </th><th> MEZŐÉRTÉK                       </th><th> MEGJEGYZÉSEK                                 </th></tr>
<tr><td> A FELHŐALAPÚ SZOLGÁLTATÁS    </td><td> Új felhőalapú szolgáltatás létrehozása    </td><td>Felhőszolgáltatás egy tároló számítási erőforrásokhoz, mint a virtuális gépek</td></tr>
<tr><td> FELHŐALAPÚ SZOLGÁLTATÁS DNS-NÉV    </td><td>ubuntu-template.cloudapp.net    </td><td>Adjon meg egy gép független terheléselosztó neve</td></tr>
<tr><td> RÉGIÓ/AFFINITÁSCSOPORT/VIRTUÁLIS HÁLÓZAT </td><td>    USA nyugati régiója    </td><td> Válasszon ki egy régiót, ahol a webalkalmazások hello Cassandra fürt eléréséhez</td></tr>
<tr><td>TÁRFIÓK </td><td>    Használhatja az alapértelmezettet    </td><td>Az adott hello alapértelmezett tárfiókot vagy egy előre létrehozott tárfiók használata</td></tr>
<tr><td>A RENDELKEZÉSRE ÁLLÁSI CSOPORT </td><td>    None </td><td>    Hagyja üresen</td></tr>
<tr><td>VÉGPONTOK    </td><td>Használhatja az alapértelmezettet </td><td>    Hello alapértelmezett SSH-konfiguráció használata </td></tr>
</table>

Kattintson a jobbra mutató nyílra hello alapértelmezett hagyja a #3 üdvözlő képernyőt, és kattintson a hello "ellenőrzés" gombra toocomplete hello virtuális gép üzembe helyezési folyamat. Néhány perc elteltével hello virtuális gép neve "ubuntu-sablonnal hello" a "fut" állapotba kell lennie.

### <a name="install-hello-necessary-software"></a>Hello szükséges SZOFTVEREK telepítése
#### <a name="step-1-upload-tarballs"></a>1. lépés: Feltöltés tarballs
A szolgáltatáskapcsolódási pont vagy pscp, másolása hello korábban letöltött szoftverfrissítések túl ~ / letöltések directory hello parancs formátuma a következő használatával:

##### <a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp kiszolgáló-jre-8u5-linux-x64.tar.gzlocaladmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz
Ismételje meg a fent parancs hello JRE is mint hello Cassandra bits.

#### <a name="step-2-prepare-hello-directory-structure-and-extract-hello-archives"></a>2. lépés: Készítse elő a hello könyvtárszerkezetét és hello archívum kibontása
Jelentkezzen be a virtuális gép hello és hello könyvtárstruktúrát létrehozása, és bontsa ki a szoftver felügyelőként hello bash parancsfájlt az alábbi:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change hello ownership toohello service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile tooadd JRE toohello PATH"
    echo "installation is complete"


Ha vim ablakba be ezt a parancsfájlt, győződjön meg arról, hogy tooremove hello kocsivissza visszatérési ("\r") a következő parancs hello használata:

    tr -d '\r' <infile.sh >outfile.sh

#### <a name="step-3-edit-etcprofile"></a>3. lépés: Stb/profil szerkesztése
Hozzáfűzendő hello következő hello végén:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

#### <a name="step-4-install-jna-for-production-systems"></a>4. lépés: Telepítse JNA éles rendszerek esetén
Használjon hello következő parancsot a feladatütemezési: hello következő parancsot a telepítés jna-3.2.7.jar és jna-platform-3.2.7.jar too/usr/share.java directory sudo apt-get telepíti a java-libjna

Hozza létre a szimbolikus csatolást $CASS_HOME/lib könyvtárban, hogy Cassandra indítási parancsfájl megtalálhassa a JAR-fájlok kivételével:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

#### <a name="step-5-configure-cassandrayaml"></a>5. lépés: Cassandra.yaml konfigurálása
Minden virtuális gép tooreflect konfiguráció [azt fogja végeznünk ez hello tényleges kiépítése során] összes hello virtuális gép által igényelt cassandra.yaml szerkesztése:

<table>
<tr><th>Mező neve   </th><th> Érték  </th><th>    Megjegyzések </th></tr>
<tr><td>fürtnév </td><td>    "CustomerService"    </td><td> Hello nevet válasszon, amely tükrözi a központi telepítés</td></tr>
<tr><td>listen_address    </td><td>[hagyja üresen a mezőt]    </td><td> Törölje a "localhost" </td></tr>
<tr><td>rpc_addres   </td><td>[hagyja üresen a mezőt]    </td><td> Törölje a "localhost" </td></tr>
<tr><td>magok    </td><td>"10.1.2.4, 10.1.2.6, 10.1.2.8"    </td><td>Összes rendszer naplólemezként jelöli magok hello IP-címek listáját.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Ez hello NetworkTopologyStrateg behívásakor hello adatközpontban és a virtuális gép hello hello állvány használja</td></tr>
</table>

#### <a name="step-6-capture-hello-vm-image"></a>6. lépés: Hello Virtuálisgép-lemezkép rögzítése
Jelentkezzen be hello virtuális gép hello állomásnév (hk-cas-template.cloudapp.net) és hello titkos SSH-kulcsot korábban hozott létre. Lásd: hogyan hogyan használatával toolog hello parancs ssh vagy putty.exe részletezi az Azure Linux SSH tooUse.

A következő feladatütemezési műveletek toocapture hello lemezkép hello hajtható végre:

##### <a name="1-deprovision"></a>1. Deprovision
Hello paranccsal "sudo waagent-deprovision + felhasználói" tooremove virtuálisgép-példány egyedi adatok. Tekintse át a vonatkozó [hogyan tooCapture Linux virtuális gépek](capture-image.md) sablonként tooUse több részletet hello lemezkép rögzítését.

##### <a name="2-shutdown-hello-vm"></a>2: leállítási hello méretű VM
Győződjön meg arról, hogy hello a virtuális gép ki van jelölve, és az alsó eszköztár hello hello LEÁLLÍTÁSI hivatkozásra.

##### <a name="3-capture-hello-image"></a>3: hello lemezképet
Győződjön meg arról, hogy hello a virtuális gép ki van jelölve, és az alsó eszköztár hello hello rögzítési hivatkozásra. Hello következő képernyőn nevezze el egy kép (pl. hk-cas-2-08-ub-14-04-2014071), a megfelelő RENDSZERKÉP leírása, és kattintson a hello "ellenőrzés" jel toofinish hello rögzítési folyamat.

Ez eltarthat néhány másodpercig és hello kép elérhetőnek kell lennie hello kép gyűjteménye a saját LEMEZKÉPEK szakaszában. hello forrás virtuális gép automatikusan után törlődni fognak hello lemezkép rögzítése sikerült. 

## <a name="single-region-deployment-process"></a>Egyetlen régión telepítési folyamata
**1. lépés:, Hozzon létre virtuális hálózati hello** bejelentkezni hello Azure-portálon, és hozzon létre egy virtuális hálózat (klasszikus) hello attribútumokkal hello a következő táblázatban látható. Lásd: [hozzon létre egy virtuális hálózat (klasszikus) Azure-portálon hello](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) hello folyamat részletes leírást.      

<table>
<tr><th>VM-attribútum neve</th><th>Érték</th><th>Megjegyzések</th></tr>
<tr><td>Név</td><td>vnet-esetén-nyugati-us</td><td></td></tr>
<tr><td>Régió</td><td>USA nyugati régiója</td><td></td></tr>
<tr><td>DNS-kiszolgálók</td><td>None</td><td>Figyelmen kívül hagyja ezt a DNS-kiszolgáló nem használjuk</td></tr>
<tr><td>Címterület</td><td>10.1.0.0/16</td><td></td></tr>    
<tr><td>Kezdő IP-Címét</td><td>10.1.0.0</td><td></td></tr>    
<tr><td>CIDR </td><td>/16 (65531)</td><td></td></tr>
</table>

Adja hozzá a következő alhálózatok hello:

<table>
<tr><th>Név</th><th>Kezdő IP-Címét</th><th>CIDR</th><th>Megjegyzések</th></tr>
<tr><td>webalkalmazás</td><td>10.1.1.0</td><td>/24 (251)</td><td>Hello webfarm-alhálózatot</td></tr>
<tr><td>Adatok</td><td>10.1.2.0</td><td>/24 (251)</td><td>Adatbázis-csomópont hello alhálózatot</td></tr>
</table>

Adatok és a webes alhálózatok védetté tehetők a hálózati biztonsági csoportokkal hello érvényességének, amelyeknek ez a cikk a hatókörén kívül esik.  

**2. lépés: Kiépítése virtuális gépek** korábban létrehozott hello lemezképet használ, rendszer létrehozása a következő virtuális gépek hello felhőben hello server "hk-c-svc-nyugati" és azok toohello megfelelő alhálózatokban kötési alább látható módon:

<table>
<tr><th>Számítógép neve    </th><th>Alhálózat    </th><th>IP-cím    </th><th>Rendelkezésre állási csoport</th><th>DC/Rack</th><th>Kezdőérték?</th></tr>
<tr><td>HK-c1-nyugati-us    </td><td>Adatok    </td><td>10.1.2.4    </td><td>HK-c-aset-1    </td><td>DC = WESTUS állvány = rack1 </td><td>Igen</td></tr>
<tr><td>HK-c2-nyugati-us    </td><td>Adatok    </td><td>10.1.2.5    </td><td>HK-c-aset-1    </td><td>DC = WESTUS állvány = rack1    </td><td>Nem </td></tr>
<tr><td>HK-c3-nyugati-us    </td><td>Adatok    </td><td>10.1.2.6    </td><td>HK-c-aset-1    </td><td>DC = WESTUS állvány = rack2    </td><td>Igen</td></tr>
<tr><td>HK-c4-nyugati-us    </td><td>Adatok    </td><td>10.1.2.7    </td><td>HK-c-aset-1    </td><td>DC = WESTUS állvány = rack2    </td><td>Nem </td></tr>
<tr><td>HK-c5-nyugati-us    </td><td>Adatok    </td><td>10.1.2.8    </td><td>HK-c-aset-2    </td><td>DC = WESTUS állvány = rack3    </td><td>Igen</td></tr>
<tr><td>HK-c6-nyugati-us    </td><td>Adatok    </td><td>10.1.2.9    </td><td>HK-c-aset-2    </td><td>DC = WESTUS állvány = rack3    </td><td>Nem </td></tr>
<tr><td>HK-c7-nyugati-us    </td><td>Adatok    </td><td>10.1.2.10    </td><td>HK-c-aset-2    </td><td>DC = WESTUS állvány = rack4    </td><td>Igen</td></tr>
<tr><td>HK-c8-nyugati-us    </td><td>Adatok    </td><td>10.1.2.11    </td><td>HK-c-aset-2    </td><td>DC = WESTUS állvány = rack4    </td><td>Nem </td></tr>
<tr><td>HK-F1-nyugati-us    </td><td>webalkalmazás    </td><td>10.1.1.4    </td><td>HK-w-aset-1    </td><td>                       </td><td>N/A</td></tr>
<tr><td>HK-w2-nyugati-us    </td><td>webalkalmazás    </td><td>10.1.1.5    </td><td>HK-w-aset-1    </td><td>                       </td><td>N/A</td></tr>
</table>

Hello fent található virtuális gépek listájára létrehozásához a következő folyamat hello van szükség:

1. Az adott üres felhőalapú szolgáltatás létrehozása
2. Hozzon létre egy virtuális Gépet hello korábban rögzített lemezképből, és csatlakoztassa azt toohello; korábban létrehozott virtuális hálózatban Ismételje meg ezt a hello virtuális gépen
3. Adja hozzá a belső terheléselosztó toohello felhőalapú szolgáltatás, és csatlakoztassa azt toohello "data" alhálózat
4. A korábban létrehozott virtuális gépek hozzáadása egy elosztott terhelésű végpont egy elosztott terhelésű készlet csatlakoztatott korábban létrehozott toohello belső terheléselosztó keresztül thrift-forgalom

hello fent folyamat használja a klasszikus Azure portálon; hajtható végre a Windows-számítógépen (Ha még nem rendelkezik hozzáférési tooa Windows gép Azure virtuális gép egy használata), használja a következő PowerShell-parancsfájl tooprovision hello 8 virtuális gépeinek automatikusan.

**Lista 1: Virtuális gépek rendszerbe állításához PowerShell-parancsfájl**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into hello current Powershell session before proceeding
        #hello process: 1. create Azure Storage account, 2. create virtual network, 3.create hello VM template, 2. crate a list of VMs from hello template

        #fundamental variables - change these tooreflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores hello list of azure vm configuration objects
        #create hello list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add add hello thrift endpoint toohello internal load balancer for all hello VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**3. lépés: Cassandra konfigurálása az egyes virtuális gépek**

Jelentkezzen be a virtuális gép hello és hello következőket hajthatja végre:

* $CASS_HOME/conf/cassandra-rackdc.properties toospecify hello data center és állvány tulajdonságainak szerkesztése:
  
       dc =EASTUS, rack =rack1
* Cassandra.yaml tooconfigure kezdőérték csomópontok alábbi szerkesztése:
  
       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**4. lépés: Indítsa el a hello virtuális gépek és hello fürt tesztelése**

Jelentkezzen be valamelyik hello csomópontja (pl. hk-c1-nyugati-us) és a következő parancs toosee hello állapotának hello fürt futtatási hello:

       nodetool –h 10.1.2.4 –p 7199 status

Hello megjelenítési hasonló toohello egy alatt a 8 csomópontos fürtök kell megjelennie:

<table>
<tr><th>status</th><th>Cím    </th><th>Betöltés    </th><th>Tokenek    </th><th>Tulajdonos </th><th>Állomás azonosítója    </th><th>Állvány</th></tr>
<tr><th>VISSZAVONÁSA    </td><td>10.1.2.4     </td><td>87.81 KB    </td><td>256    </td><td>38.0%    </td><td>GUID (eltávolítani)</td><td>rack1</td></tr>
<tr><th>VISSZAVONÁSA    </td><td>10.1.2.5     </td><td>41.08 KB    </td><td>256    </td><td>68.9%    </td><td>GUID (eltávolítani)</td><td>rack1</td></tr>
<tr><th>VISSZAVONÁSA    </td><td>10.1.2.6     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (eltávolítani)</td><td>rack2</td></tr>
<tr><th>VISSZAVONÁSA    </td><td>10.1.2.7     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (eltávolítani)</td><td>rack2</td></tr>
<tr><th>VISSZAVONÁSA    </td><td>10.1.2.8     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (eltávolítani)</td><td>rack3</td></tr>
<tr><th>VISSZAVONÁSA    </td><td>10.1.2.9     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (eltávolítani)</td><td>rack3</td></tr>
<tr><th>VISSZAVONÁSA    </td><td>10.1.2.10     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (eltávolítani)</td><td>rack4</td></tr>
<tr><th>VISSZAVONÁSA    </td><td>10.1.2.11     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (eltávolítani)</td><td>rack4</td></tr>
</table>

## <a name="test-hello-single-region-cluster"></a>Teszt hello fürtön régió
A következő lépéseket tootest hello fürt hello használata:

1. Belső terheléselosztó hello hello IP-cím (pl. lekérésére hello Powershell parancs Get-AzureInternalLoadbalancer parancsmag használatával,  10.1.2.101). hello hello parancs szintaxisa alább látható: Get-AzureLoadbalancer – szolgáltatásnév "hk-c-svc-nyugati-us" [hello belső terheléselosztó IP-címének együtt hello részleteit jeleníti meg]
2. Jelentkezzen be a hello webfarm (pl. hk-F1-nyugati-us) virtuális gép használata a Putty vagy ssh
3. Végrehajtás $CASS_HOME/bin/cqlsh 10.1.2.101 9160
4. Ha hello fürt nem működik a következő CQL parancsok tooverify hello használata:
   
     Kulcstérértesítések használatával hozzon létre customers_ks rendelkező replikációs = {"class": "SimpleStrategy", "replication_factor": 3};   HASZNÁLJA a customers_ks;   Hozzon létre a tábla Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   Helyezze be a Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   Helyezze be a Customers(customer_id, firstname, lastname) értékek (2, "Jane", "Doe").
   
     Válassza ki * ügyfelek;

Például hello alatt megjelenítésre kell megjelennie:

<table>
  <tr><th> customer_id </th><th> Utónév </th><th> Vezetéknév </th></tr>
  <tr><td> 1 </td><td> John </td><td> DOE </td></tr>
  <tr><td> 2 </td><td> Jane </td><td> DOE </td></tr>
</table>

Vegye figyelembe, hogy a 4. lépésben létrehozott hello kulcstérértesítések használatával SimpleStrategy használ egy replication_factor 3. SimpleStrategy ajánlott egyetlen data center központi telepítések mivel több adatok NetworkTopologyStrategy center központi telepítések. A / 3 replication_factor csomópont hibák tolerancia rendszerében.

## <a id="tworegion"></a>Több területi telepítési folyamata
Lesz használhatják fel a hello egyetlen régión telepítése befejeződött, és ismételje meg ugyanezt a folyamatot hello hello második régió telepítése. hello kulcs hello egyetlen és több régióban telepítési közötti különbség hello VPN-alagút beállítást régió közti kommunikációhoz; rendszer hello hálózati telepítéssel, hello virtuális gépek telepítéséhez és konfigurálásához Cassandra.

### <a name="step-1-create-hello-virtual-network-at-hello-2nd-region"></a>1. lépés: Hozzon létre virtuális hálózati hello hello régió 2.
Jelentkezzen be a klasszikus Azure portálon hello, és hozzon létre egy virtuális hálózati hello attribútumok megjelenítése hello táblában. Lásd: [Cloud-Only virtuális hálózat konfigurálása a klasszikus Azure portálon hello](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) hello folyamat részletes leírást.      

<table>
<tr><th>Attribútum neve    </th><th>Érték    </th><th>Megjegyzések</th></tr>
<tr><td>Név    </td><td>vnet-esetén-keleti-us</td><td></td></tr>
<tr><td>Régió    </td><td>USA keleti régiója</td><td></td></tr>
<tr><td>DNS-kiszolgálók        </td><td></td><td>Figyelmen kívül hagyja ezt a DNS-kiszolgáló nem használjuk</td></tr>
<tr><td>A pont-pont VPN konfigurálása</td><td></td><td>        Figyelmen kívül hagyja ezt</td></tr>
<tr><td>Webhelyek közötti virtuális magánhálózat konfigurálása</td><td></td><td>        Figyelmen kívül hagyja ezt</td></tr>
<tr><td>Címterület    </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>Kezdő IP-Címét    </td><td>10.2.0.0    </td><td></td></tr>
<tr><td>CIDR    </td><td>/16 (65531)</td><td></td></tr>
</table>

Adja hozzá a következő alhálózatok hello:

<table>
<tr><th>Név    </th><th>Kezdő IP-Címét    </th><th>CIDR    </th><th>Megjegyzések</th></tr>
<tr><td>webalkalmazás    </td><td>10.2.1.0    </td><td>/24 (251)    </td><td>Hello webfarm-alhálózatot</td></tr>
<tr><td>Adatok    </td><td>10.2.2.0    </td><td>/24 (251)    </td><td>Adatbázis-csomópont hello alhálózatot</td></tr>
</table>


### <a name="step-2-create-local-networks"></a>2. lépés: A helyi hálózatok létrehozása
Az Azure virtuális hálózatban egy helyi hálózaton egy proxy címtartományt, amely leképezhető tooa magánfelhőbe vagy egy másik Azure-régió, többek között távoli hely. A proxy címtartomány kötött tooa távoli átjáró jobb hálózati célok útválasztási hálózati toohello. Lásd: [konfigurálja a VNet tooVNet kapcsolat](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) hello útmutatásért VNET – VNET-kapcsolatot létesít.

Hozzon létre két helyi hálózat következő adatok hello száma:

| Hálózati név | VPN-átjáró címét | Címterület | Megjegyzések |
| --- | --- | --- | --- |
| HK-lnet-Map-to-East-us |23.1.1.1 |10.2.0.0/16 |Hello helyi hálózat létrehozásakor adjon címet az átjáró helyőrző. hello valós átjárócímet ki van töltve, hello átjáró létrehozása után. Győződjön meg arról, hogy hello címterület pontosan megegyezik hello megfelelő távoli virtuális hálózat; Ebben az esetben hello virtuális hálózat létrehozása hello USA keleti régiójában. |
| HK-lnet-Map-to-West-us |23.2.2.2 |10.1.0.0/16 |Hello helyi hálózat létrehozásakor adjon címet az átjáró helyőrző. hello valós átjárócímet ki van töltve, hello átjáró létrehozása után. Győződjön meg arról, hogy hello címterület pontosan megegyezik hello megfelelő távoli virtuális hálózat; Ebben az esetben hello virtuális hálózat létrehozása hello USA nyugati régiója régióban. |

### <a name="step-3-map-local-network-toohello-respective-vnets"></a>3. lépés: Térkép "Helyi" hálózati toohello megfelelő Vnetek
Hello a klasszikus Azure portálon minden egyes virtuális hálózat kiválasztása, kattintson a "Beállítása", "Connect toohello helyi hálózat" Ellenőrizze, majd hello a következő adatok hello / helyi hálózatok kiválasztása:

| Virtual Network | Helyi hálózati |
| --- | --- |
| HK-vnet-nyugati-us |HK-lnet-Map-to-East-us |
| HK-vnet-keleti-us |HK-lnet-Map-to-West-us |

### <a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>4. lépés: A VNET1 és VNET2 átjárók létrehozása
Mindkét hello virtuális hálózatok hello irányítópultról kattintson létrehozása ÁTJÁRÓN, amely akkor indul el, hello VPN-átjáró létesítésének folyamatát kell használnia. Miután néhány percig, amíg az összes virtuális hálózat irányítópult hello megjelenjen-e hello tényleges átjárócímet.

### <a name="step-5-update-local-networks-with-hello-respective-gateway-addresses"></a>5. lépés: Frissítés "Helyi" hálózatok hello megfelelő "Gateway" címek
Mindkét hello helyi hálózatok tooreplace hello helyőrző átjáró IP-cím hello valós IP-címével csak hello kiépített átjárók szerkesztése. A következő hozzárendelési hello használata:

<table>
<tr><th>Helyi hálózati    </th><th>Virtuális hálózati átjáró</th></tr>
<tr><td>HK-lnet-Map-to-East-us </td><td>A hk-vnet-nyugati-us átjáró</td></tr>
<tr><td>HK-lnet-Map-to-West-us </td><td>A hk-vnet-keleti-us átjáró</td></tr>
</table>

### <a name="step-6-update-hello-shared-key"></a>6. lépés: Hello megosztott kulcs frissítése
A következő Powershell parancsfájl tooupdate hello IPSec-kulcs minden VPN-átjáró [hello szakét kulcs használata mindkét hello átjárók] használata hello: Set-AzureVNetGatewayKey - VNetName hk-vnet-keleti-us - LocalNetworkSiteName hk-lnet-map-to-west-us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - VNetName hk-vnet-nyugati-us - LocalNetworkSiteName hk-lnet-map-to-east-us - SharedKey D9E76BKK

### <a name="step-7-establish-hello-vnet-to-vnet-connection"></a>7. lépés: Hello VNET – VNET-kapcsolatot létrehozni.
A klasszikus Azure portálon hello használja a hello "IRÁNYÍTÓPULT" menü mindkét hello virtuális hálózatok tooestablish átjárók közötti kapcsolat. Hello alsó eszköztár hello "Csatlakozás" menüpontok használja. Néhány perc múlva hello irányítópult megjelenjen-e hello kapcsolódási adatait grafikusan.

### <a name="step-8-create-hello-virtual-machines-in-region-2"></a>8. lépés: A régió #2 hello virtuális gépek létrehozása
Hello Ubuntu lemezkép létrehozása ugyanazon lépések vagy hello kép VHD fájl toohello Azure storage-fiók #2 régióban található, másolja a következő hello #1 régió telepítésben szerint, és hello lemezkép létrehozásához. Ezzel a lemezképpel, és hozzon létre egy új felhőalapú szolgáltatás hk-c-svc-keleti-nekünk a virtuális gépek listájának következő hello:

| Számítógép neve | Alhálózat | IP-cím | Rendelkezésre állási csoport | DC/Rack | Kezdőérték? |
| --- | --- | --- | --- | --- | --- |
| HK-c1-keleti-us |Adatok |10.2.2.4 |HK-c-aset-1 |DC = EASTUS állvány = rack1 |Igen |
| HK-c2-keleti-us |Adatok |10.2.2.5 |HK-c-aset-1 |DC = EASTUS állvány = rack1 |Nem |
| HK-c3-keleti-us |Adatok |10.2.2.6 |HK-c-aset-1 |DC = EASTUS állvány = rack2 |Igen |
| HK-c5-keleti-us |Adatok |10.2.2.8 |HK-c-aset-2 |DC = EASTUS állvány = rack3 |Igen |
| HK-c6-keleti-us |Adatok |10.2.2.9 |HK-c-aset-2 |DC = EASTUS állvány = rack3 |Nem |
| HK-c7-keleti-us |Adatok |10.2.2.10 |HK-c-aset-2 |DC = EASTUS állvány = rack4 |Igen |
| HK-c8-keleti-us |Adatok |10.2.2.11 |HK-c-aset-2 |DC = EASTUS állvány = rack4 |Nem |
| HK-F1-keleti-us |webalkalmazás |10.2.1.4 |HK-w-aset-1 |N/A |N/A |
| HK-w2-keleti-us |webalkalmazás |10.2.1.5 |HK-w-aset-1 |N/A |N/A |

Kövesse hello ugyanaz, mint #1 régió utasításokat, de a 10.2.xxx.xxx címterület használata.

### <a name="step-9-configure-cassandra-on-each-vm"></a>9. lépés: Cassandra konfigurálása az egyes virtuális gépek
Jelentkezzen be a virtuális gép hello és hello következőket hajthatja végre:

1. $CASS_HOME/conf/cassandra-rackdc.properties toospecify hello data center és állvány tulajdonságainak szerkesztése hello formátumban: dc = EASTUS állvány = rack1
2. Cassandra.yaml tooconfigure kezdőérték csomópontok szerkesztése: magok: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

### <a name="step-10-start-cassandra"></a>10. lépés: Indítsa el a Cassandra
Jelentkezzen be az összes virtuális Géphez, és indítsa el a Cassandra hello háttérben hello a következő parancs futtatásával: $CASS_HOME/bin/cassandra

## <a name="test-hello-multi-region-cluster"></a>Teszt hello több területi fürt
Mostanra Cassandra lett telepített too16-csomópont minden Azure régióban 8 csomópont. Ezek a csomópontok azonos fürt hello közös fürt neve és hello kezdőérték csomópont-konfiguráció hello szerepelnek. A következő folyamat tootest hello fürt hello használata:

### <a name="step-1-get-hello-internal-load-balancer-ip-for-both-hello-regions-using-powershell"></a>1. lépés: Hello belső terheléselosztó IP lekérése mindkét hello régió PowerShell használatával
* "Hk-c-svc-nyugati-us" Get-AzureInternalLoadbalancer - szolgáltatásnév
* "Hk-c-svc-keleti-us" Get-AzureInternalLoadbalancer - szolgáltatásnév  
  
    Vegye figyelembe a hello IP-cím (pl. - 10.1.2.101, kelet - nyugati 10.2.2.101) jelenik meg.

### <a name="step-2-execute-hello-following-in-hello-west-region-after-logging-into-hk-w1-west-us"></a>2. lépés: A végrehajtást hello következő hello nyugati terület hk-F1-nyugati-us való bejelentkezés után
1. Végrehajtás $CASS_HOME/bin/cqlsh 10.1.2.101 9160
2. A következő CQL parancsok hello hajtható végre:
   
     Kulcstérértesítések használatával hozzon létre customers_ks rendelkező replikációs = {"class": "NetworkToplogyStrategy", "WESTUS": 3, "EASTUS": 3};   HASZNÁLJA a customers_ks;   Hozzon létre a tábla Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   Helyezze be a Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   Helyezze be a Customers(customer_id, firstname, lastname) értékek (2, "Jane", "Doe").   Válassza ki * ügyfelek;

Például hello alatt megjelenítésre kell megjelennie:

| customer_id | Utónév | Vezetéknév |
| --- | --- | --- |
| 1 |John |DOE |
| 2 |Jane |DOE |

### <a name="step-3-execute-hello-following-in-hello-east-region-after-logging-into-hk-w1-east-us"></a>3. lépés: A végrehajtást hello következő hello keleti terület hk-F1-keleti-us való bejelentkezés után:
1. Végrehajtás $CASS_HOME/bin/cqlsh 10.2.2.101 9160
2. A következő CQL parancsok hello hajtható végre:
   
     HASZNÁLJA a customers_ks;   Hozzon létre a tábla Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   Helyezze be a Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   Helyezze be a Customers(customer_id, firstname, lastname) értékek (2, "Jane", "Doe").   Válassza ki * ügyfelek;

Azonos jeleníti meg, mint hello nyugati régiójában hello kell megjelennie:

| customer_id | Utónév | Vezetéknév |
| --- | --- | --- |
| 1 |John |DOE |
| 2 |Jane |DOE |

Néhány további Beszúrások hajtható végre, és, hogy azok beszerezni replikált toowest-us hello fürt része.

## <a name="test-cassandra-cluster-from-nodejs"></a>Cassandra Tesztfürthöz Node.js-ből
Hello Linux virtuális gépek egyike segítségével jön létre hello "web" réteg korábban, azt fogja végrehajtani egy egyszerű Node.js parancsfájl tooread hello korábban beszúrt adatok

**1. lépés: A Node.js és Cassandra ügyfél telepítése**

1. Telepítse a Node.js és npm
2. Telepítse a csomag "cassandra-ügyfél csomópont" npm segítségével
3. A következő parancsfájl hello rendszerhéj-parancssorba, mely megjeleníti a json-karakterláncban hello hello beolvasott adatok hello hajtható végre:
   
        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };
   
        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed toocreate Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }
   
        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed toocreate column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }
   
        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);
   
           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }
   
        //update will also insert hello record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }
   
        //read hello two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }
   
        //exectue hello code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)

## <a name="conclusion"></a>Összegzés
Microsoft Azure a rugalmas platform, amely lehetővé teszi a Microsoft, valamint nyílt forráskódú szoftvereket hello fut, amint azt a ebben a gyakorlatban. Magas rendelkezésre állású Cassandra fürtök egyetlen adatközpontba több tartalék tartományokban hello fürtcsomópontok terjednek hello keresztül telepíthető. Cassandra fürtök különféle régiókban földrajzilag távoli Azure katasztrófa igazoló rendszerekhez is telepíthető. Azure és Cassandra együtt lehetővé teszi, hogy hello építése jól skálázható, magas rendelkezésre állású és a vészhelyreállítás helyreállítható felhőszolgáltatások szükséges mai internet által méret szolgáltatások.  

## <a name="references"></a>Referencia
* [http://cassandra.Apache.org](http://cassandra.apache.org)
* [http://www.datastax.com](http://www.datastax.com)
* [http://www.nodejs.org](http://www.nodejs.org)

