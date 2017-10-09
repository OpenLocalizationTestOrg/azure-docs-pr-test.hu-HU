---
title: "aaaAzure Data Lake Store Storm teljesítményének hangolása irányelvek |} Microsoft Docs"
description: "Azure Data Lake Store Storm teljesítményének hangolása irányelvek"
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: 5412fd46cf2373f5877030913df4fe1fc6f5473a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-storm-on-hdinsight-and-azure-data-lake-store"></a>Útmutató a Storm on HDInsight és az Azure Data Lake Store teljesítményhangolása

Ismerje meg, hello tényezőket kell figyelembe venni, amikor egy Azure Storm-topológia hello teljesítményének beállításakor. Például akkor fontos toounderstand hello jellemzői hello által végzett munka hello spoutokkal kapcsolatban és hello boltokhoz (hello munkahelyi-e i/o- vagy memóriaigényes). Ez a cikk foglalkozik számos különböző teljesítményének hangolása útmutatást, beleértve a gyakori problémák elhárításához.

## <a name="prerequisites"></a>Előfeltételek

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Egy Azure Data Lake Store-fiók**. Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md).
* **Egy Azure HDInsight fürt** a Data Lake Store-fiók hozzáférési tooa. Lásd: [HDInsight-fürtök létrehozása a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Ellenőrizze, hogy engedélyezte a távoli asztal hello fürt.
* **A Data Lake Store egy Storm-fürt futtató**. További információkért lásd: [HDInsight alatt futó Storm](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-overview).
* **Teljesítményhangolás irányelvek a Data Lake Store**.  Általános teljesítmény fogalmakat, lásd: [Data Lake Store teljesítmény hangolása útmutatást](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance).  

## <a name="tune-hello-parallelism-of-hello-topology"></a>Hello topológia párhuzamosságát hello hangolása

Előfordulhat, hogy képes tooimprove teljesítmény szerint növekvő hello párhuzamossági az i/o-tooand hello Data Lake Store-ból. A Storm-topológia tartozik egy hello párhuzamossági meghatározó konfigurációk:
* Munkavégző folyamatok (hello munkavállalók egyenlően elosztott virtuális gépek hello) száma.
* Spout végrehajtó példányok száma.
* Bolt végrehajtó példányok száma.
* Spout feladatok száma.
* Bolt feladatok száma.

Például egy fürtön 4 virtuális gépek és 4 munkavégző folyamatok, 32 spout végrehajtója és 32 spout feladatokat, és 256 bolt végrehajtója és 512 bolt feladatok, vegye figyelembe a következőket hello:

Minden egyes felügyelő, amely a munkavégző csomópont, a Java virtuális gép (JVM) folyamat egyetlen worker rendelkezik. A JVM folyamat 4 spout szál és 64 bolt szál kezeli. Minden egyes szál belül feladatok egymás után futnak. Konfigurációs megelőző hello minden spout szál 1 tevékenység rendelkezik, és minden egyes bolt szál 2 tevékenységet tartalmaz.

A Storm az alábbiakban hello érintett összetevők, és rendelkezik párhuzamossági hello szint hatásuk:
* hello átjárócsomópont (a Storm hívott Nimbus) használt toosubmit és feladatok kezelése. Ezek a csomópontok befolyásolni párhuzamossági hello fokát.
* hello felügyelő csomópontok. A Hdinsightban Ez megfelel a tooa munkavégző csomópont Azure virtuális Gépen.
* hello munkavégző feladatokat az hello virtuális gépeken futó Storm folyamatokat is. Minden egyes munkavégző feladat tooa JVM-példány megfelel. Storm hello szám osztja el a munkavégző folyamatok adhat meg toohello munkavégző csomópontokhoz lehetőség szerint egyenletes.
* Spout és boltok végrehajtó példányok. Minden egyes végrehajtó példány hello munkavállalók (JVMs) belül futó tooa szál felel meg.
* A Storm feladatok. Ezek a logikai feladatok, hogy minden egyes szálait futtatható. Ez nem módosítja hello szintű párhuzamosság, így ki kell értékelnie, hogy több feladat végrehajtója / kell-e.

### <a name="get-hello-best-performance-from-data-lake-store"></a>Hello legjobb teljesítmény eléréséhez a Data Lake Store-ból

A Data Lake Store használatakor hello legjobb teljesítmény érdekében elérhetővé Ha hello a következő:
* Nyílt meg kis hozzáfűzi a nagyobb méretű (ideális esetben 4 MB).
* Hajtsa végre, mint a sok egyidejű kéréssel. Minden egyes bolt szál blokkoló olvasások végez műveletet, mert kívánt toohave valahol 8 – 12 szálak száma core hello tartományán belül. Így hello hálózati adapter és hello CPU is alkalmazhatja. A nagyobb virtuális gépek lehetővé teszi több egyidejű kérelmet.  

### <a name="example-topology"></a>Példa topológia

Tegyük fel, hogy rendelkezik egy 8 munkavégző csomópontot tartalmazó fürtben D13v2 Azure virtuális gépen. A virtuális gép rendelkezik 8 magos, így között hello 8 munkavégző csomópontokhoz, 64 teljes mag van.

Tegyük fel, 8 bolt szálak száma core végezzük. A megadott 64 mag, amely azt jelenti, hogy azt szeretnénk, ha 512 teljes bolt végrehajtó példányok (Ez azt jelenti, hogy a szál). Ebben az esetben tegyük fel, azt egy JVM-et virtuális gépenként kezdődnie, és főként a hello szál párhuzamossági hello JVM tooachieve egyidejűségi belül használja. Ez azt jelenti, hogy 8 munkavégző feladatokat (egy Azure virtuális gépenként), és 512 bolt végrehajtója kell. Ebben a konfigurációban a Storm megpróbál toodistribute hello munkavállalók egyenletesen munkavégző csomópontokhoz (más néven a felügyelő csomópontok) keresztül jogosultságot ad az egyes feldolgozó csomópontok 1 JVM-et. Most belül hello felügyelők, Storm megpróbál toodistribute hello végrehajtója egyenletesen felügyelők között, minden felügyelő (Ez azt jelenti, hogy JVM) amely 8 szálait egyes.

## <a name="tune-additional-parameters"></a>További paraméterek hangolása
Miután hello alapszintű topológia, érdemes lehet dönthet a tootweak hello paraméterek egyikét:
* **Egyes feldolgozó csomópontok JVMs száma.** Ha nagy adatszerkezet (például egy keresési tábla) a memóriában, minden egyes JVM állomás szükséges egy külön példányát. Azt is megteheti használhatja hello adatszerkezet sok szálakon Ha kevesebb JVMs rendelkezik. Hello bolt i/o JVMs hello száma tegye az ugyanennyi egy között ezen JVMs hozzáadott szálak számának hello különbség. Az egyszerűség kedvéért egy jó ötlet toohave egy / munkavégző JVM-et. Attól függően, hogy milyen módon tartalmaz a bolt vagy milyen alkalmazás feldolgozása az Ön igényel, ha esetleg toochange ezt a számot.
* **Spout végrehajtója száma.** Hello előző példában boltokhoz tooData Lake Store írásra, mert hello spoutokkal kapcsolatban nincs közvetlenül érintett toohello bolt teljesítményét. Azonban attól függően, hogy feldolgozás vagy i/o történik a hello spout jó ötlet hello mennyisége tootune hello spoutok a legjobb teljesítmény érdekében. Győződjön meg arról, hogy rendelkezik-e elegendő spoutokkal kapcsolatban toobe képes tookeep hello boltokhoz foglalt. hello kimeneti sebességű hello spoutokkal kapcsolatban meg kell felelnie hello boltokhoz hello átviteli sebességgel. hello tényleges konfiguráció hello spout függ.
* **Feladatok száma.** Minden egyes bolt egyetlen szálon futtatja. További feladatok / bolt nem ad meg semmilyen további egyidejűségi. hello csak azok előnyös, ha a folyamat hello rekordot igazolása időt vesz igénybe a bolt végrehajtási idő nagy részét. Egy jó ötlet toogroup sok rekordokat a nagyobb hozzáfűzése a hello bolt nyugtázást elküldése előtt. Így a legtöbb esetben több feladat nincs további előnye adja meg.
* **Helyi vagy véletlen csoportosítása.** Ha ez a beállítás engedélyezve van, rekordokat küldött toobolts hello belül azonos munkavégző folyamatot. Ez csökkenti a folyamatok közötti kommunikációt és a hálózati hívások. Ez a legtöbb topológiák javasolt.

A alapvető forgatókönyv, az jó kiindulási pont. A saját adatok tootweak hello megelőző paraméterek tooachieve optimális teljesítményének tesztelése

## <a name="tune-hello-spout"></a>Hello spout hangolása

A következő beállítások tootune hello spout hello módosíthatja.

- **Rekord időtúllépés: topology.message.timeout.secs**. Ez a beállítás hello mennyiségű üzenet toocomplete szükséges idő határozza meg, és fogadni nyugtázása, nem sikertelen volt.

- **Munkavégző folyamatok maximális memória: worker.childopts**. Ezzel a beállítással adható meg a további parancssori paraméterek toohello Java munkavállalók. a beállítás itt leggyakrabban használt hello XmX, amely megadja, hogy hello lefoglalt memória maximális tooa JVM halommemória.

- **Függőben lévő maximális spout: topology.max.spout.pending**. Ez a beállítás meghatározza, hogy rekordokat, amelyek a lehetnek repülési (hello topológia összes csomópontjának, még nem nyugtázott) spout szálankénti bármikor hello száma.

 Egy jó számítási toodo mérete tooestimate hello egyes a rekordokat. Majd mérje fel, hogy mennyi memória egy spout szál. hello tooa szál, ez az érték osztva lefoglalt teljes memória biztosítani fogja hello maximális spout paraméter függőben lévő hello felső korlátja.

## <a name="tune-hello-bolt"></a>Hello bolt hangolása
TooData Lake Store ír be, amikor mérete szinkronizálási szabály (puffer hello ügyféloldalon) beállításához too4 MB. A kiürítési vagy hsync() csak akkor, ha hello puffer mérete hello ezt az értéket, majd történik. a virtuális gép hello munkavégző hello Data Lake Store illesztőprogramja automatikusan végzi a pufferelés, kivéve, ha explicit módon hajtsa végre egy hsync().

hello alapértelmezett Data Lake Store Storm bolt mérete szinkronizálási házirend paraméter tartozik (fileBufferSize), amely használt tootune ezt a paramétert.

I/O-igényes topológia esetén a jó ötlet toohave minden egyes bolt szál tooits saját fájlt, és tooset egy fájl Elforgatás-házirend írása (fileRotationSize). Amikor hello fájl elér egy adott méretet, hello adatfolyam automatikusan ki van ürítve nevével, és egy új fájlt. hello ajánlott Elforgatás mérete 1 GB-os.

### <a name="handle-tuple-data"></a>Rekord adatok kezelése

A Storm egy spout érvényes tooa rekordot a amíg explicit módon nyugtázta hello bolt. Ha egy rekord hello bolt által beolvasása megtörtént, de még nem nyugtázták, hello spout előfordulhat, hogy nem rendelkezik állandóként létrehozni a Data Lake Store háttérből. A nyugtázott egy rekord, miután hello spout hello bolt a adatmegőrzési garantálható, és is törölje hello forrásadatok bármilyen forrásból történt a olvasásakor.  

A legjobb teljesítmény elérése érdekében a Data Lake Store rendelkezik hello bolt rekordot adatok 4 MB-os puffer. Jegyezze meg egy 4 MB-os írhatóként end vissza a Data Lake Store toohello. Hello adatok sikeresen írásbeli toohello tároló után (hívó hflush()) hello bolt is jelenti hello adatok hátsó toohello spout. Ez az, hogy milyen hello példa boltok esetében megadott ide biztosítja. Egyúttal elfogadható toohold előtt hello hflush() kezdeményezték a rekordokat és hello nagyobb számú nyugtázott rekordokat. Azonban ez növeli a felhőszolgáltató közötti átviteléhez hello spout toohold van szüksége, és ezért növekszik hello JVM szükséges memóriamennyiség rekordjainak hello számát.

> [!NOTE]
Alkalmazások esetében gyakrabban (a adatok mérete MB-nál kisebb 4) a követelmény tooacknowledge rekordokat más nem teljesítményének javítására szolgál. Azonban, amely hatással lehet a hello i/o átviteli toohello tároló háttér. Gondosan mérjük ez kompromisszumot hello bolt i/o-teljesítmény ellen.

Ha hello beérkezési sebessége rekordokat nem magas, hello 4 MB-os puffer tart egy hosszú ideig toofill, érdemes kiküszöböléséhez ezt úgy:
* Szögek hello számának csökkentése, így nincsenek kevesebb pufferek toofill.
* Minden kiürítéseinek x vagy minden y ezredmásodperc rendelkező időalapú vagy count-alapú szabályzat, ahol egy hflush() van elindítva, és eddig halmozott hello rekordokat ismernek vissza.

Fontos megjegyezni, hogy hello átviteli ebben az esetben nem éri az események lassú sebesség, maximális átviteli sebesség célja nem hello legnagyobb ennek ellenére is. Ezek azok mérséklési hello egy rekordot tooflow – toohello tárolják a szükséges teljes idő csökkentése érdekében. Ez lehet, hogy számít, ha azt szeretné, hogy még egy kis események száma a valós idejű folyamat. Is vegye figyelembe, hogy ha a bejövő rekordot aránya alacsony, kell módosíthatja hello topology.message.timeout_secs paraméter, így hello rekordokat nem túllépi az időkorlátot, miközben azok első pufferelt vagy nem dolgozható fel.

## <a name="monitor-your-topology-in-storm"></a>A topológia a Storm figyelése  
A topológia futása közben, a figyelheti hello Storm felhasználói felületén. Az alábbiakban hello fő paraméterek toolook::

* **Teljes folyamat végrehajtása késés.** Ez az hello átlagos idő, egy rekord toobe hello spout által kibocsátott, hello bolt által feldolgozott és nyugtázott vesz igénybe.

* **Teljes bolt folyamat várakozási ideje** Ez az hello átlagos feldolgozási idő által hello rekordot hello bolt, amíg nem kap nyugtázást.

* **Teljes bolt késés hajtható végre.** Ez a hello átlagos a hello hello bolt által felhasznált idő metódus hajtható végre.

* **Hibák száma.** Ez hivatkozik, amely nem sikerült a teljes feldolgozása előtt azok túllépte az időkorlátot toobe rekordokat toohello száma.

* **A kapacitás.** Ez a méri, hogy foglalt a rendszer. Ha ez a szám 1, a boltokhoz gyors a következőkre dolgozik. Ha kevesebb mint 1, növelje a hello párhuzamosságát. Ha nagyobb, mint 1, csökkentse a hello párhuzamossági.

## <a name="troubleshoot-common-problems"></a>Gyakori hibák elhárítása
Az alábbiakban néhány gyakori hibaelhárítási forgatókönyveket.
* **Sok listának vannak időtúllépés miatt.** Tekintse meg minden egyes csomópontja hello topológia toodetermine hello szűk esetén. hello leggyakoribb ennek oka, hogy hello boltokhoz nem tud tookeep mentése hello spoutokkal kapcsolatban. Ennek eredménye tootuples eltömődés hello belső puffer várakozási toobe feldolgozása közben. Vegye figyelembe a hello időtúllépési érték növelésével vagy csökkentésével hello maximális spout függőben.

* **Nincs, a magas teljes folyamat végrehajtása késés, de egy kis bolt folyamat várakozási ideje.** Ebben az esetben is lehet, hogy hello rekordokat nem alatt ismernek elég gyorsan. Ellenőrizze, hogy vannak-e elegendő acknowledgers. Egy másik lehetőség, hogy várnak hello várólistában túl hosszú a hello boltok feldolgozás indítása előtt. Hello maximális spout függőben lévő csökkentése.

* **Nincs a magas bolt késés hajtható végre.** Ez azt jelenti, hogy a bolt metódusában hello az execute() metódus túl sokáig tart. Optimalizálni hello kódot, vagy írási méretek tekintse meg, és ürítse ki a viselkedést.

### <a name="data-lake-store-throttling"></a>Data Lake Store-szabályozás
Kattintson a Data Lake Store által biztosított sávszélesség hello korlátai, ha a feladat sikertelen jelenhet meg. Tekintse meg a feladat naplókat hibák szabályozás. Hello párhuzamossági tároló méretének növelésével csökkenthető.    

Ha Ön első szabályozott, toocheck hello hibakeresési naplózás hello ügyféloldalon engedélyezése:

1. A **Ambari** > **Storm** > **Config** > **storm-worker-log4j speciális**, módosítása  **&lt;gyökér szintű = "Infó"&gt;**  túl**&lt;gyökér szintű = "debug"&gt;**. Indítsa újra az összes hello csomópontok/szolgáltatást hello konfigurációs tootake hatást.
2. A figyelő hello Storm-topológia bejelentkezik a feldolgozó csomópontok (alatt /var/log/storm/worker-artifacts /&lt;TopologyName&gt;/&lt;port&gt;/worker.log) a Data Lake Store szabályozási kivételeket.

## <a name="next-steps"></a>Következő lépések
További teljesítményhangolás, a Storm hivatkozható [ebben a blogban](https://blogs.msdn.microsoft.com/shanyu/2015/05/14/performance-tuning-for-hdinsight-storm-and-microsoft-azure-eventhubs/).

Egy további példa toorun, lásd: [erre a Githubon](https://github.com/hdinsight/storm-performance-automation).
