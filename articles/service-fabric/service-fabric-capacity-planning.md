---
title: "a Service Fabric-alkalmazások tervezése aaaCapacity |} Microsoft Docs"
description: "Ismerteti, hogyan tooidentify hello a Service Fabric-alkalmazás szükséges számítási csomópontok száma"
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: markfuss
editor: 
ms.assetid: 9fa47be0-50a2-4a51-84a5-20992af94bea
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 44a69e9d8ec5efcc43122dc42e8f923ef37378f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="capacity-planning-for-service-fabric-applications"></a>Kapacitástervezés a Service Fabric-alkalmazások
Ez a dokumentum útmutatást ad meg hogyan tooestimate hello összeg erőforrások (CPU-k, memória, lemezes tárolás) kell toorun az Azure Service Fabric-alkalmazások. Gyakori, hogy az erőforrás-követelmények toochange felett idő. Túl sok erőforrás általában a szolgáltatás fejlesztés/tesztelés, és ezután ugyan több erőforrást, éles környezetben módba, és az alkalmazás növekszik időben népszerűvé vált a szükséges. Az alkalmazás tervezésekor gondolja, hogy hello hosszú távú követelmények keresztül, és lehetőségeket, amelyek lehetővé teszik a szolgáltatás tooscale toomeet magas keresletének.

 A Service Fabric-fürt létrehozásakor eldöntheti, milyen típusú virtuális gépek (VM) jött létre hello fürt. Minden virtuális gép csak korlátozott mennyiségű processzor (magok és sebesség), a hálózati sávszélesség, a RAM-MAL és a lemezes tárolás hello formájában erőforrásokat tartalmaz. A szolgáltatás adott idő alatt növekedésével frissítheti tooVMs, amely nagyobb erőforrásokat kínálnak, illetve további virtuális gépek tooyour fürt hozzáadása. Ez utóbbi toodo hello, meg kell tervezővel a szolgáltatás kezdetben, kihasználhatja az beszerzése a toohello fürt dinamikusan hozzáadott új virtuális gépeket.

Egyes szolgáltatások adatok kezelésében az kevés toono hello virtuális. Ezért ezeket a szolgáltatásokat kell összpontosítania elsősorban teljesítményére, ami azt jelenti, hogy hello kiválasztása a kapacitástervezés megfelelő hello virtuális gépek processzor (magok és sebessége). Emellett érdemes megfontolnia hálózati sávszélesség, beleértve a hálózati átvitel milyen gyakran fordul elő, és mennyi adat átvitele. Ha a szolgáltatás tooperform is a szolgáltatás használati növekedése, hozzáadhat további virtuális gépek toohello fürt és hello hálózati érkező kérések elosztása betölteni minden hello virtuális gépek között.

A nagy mennyiségű adat a hello virtuális gépeket kezelő szolgáltatásokra kapacitástervezés kell összpontosítania elsősorban méretét. Ebből kifolyólag alaposan gondolja át hello hello VM RAM memóriával és a lemez tárolási kapacitását. Windows hello virtuális memória felügyeleti rendszer RAM tooapplication kód hasonló lemezterület teszi. Emellett a hello Service Fabric-futtatókörnyezet biztosítja a memória és a mozgási hello ritkán használt adatok toodisk csak gyakran használt adatokkal megőrzi az intelligens lapozás. Alkalmazások így több memóriát a virtuális gép hello fizikailag rendelkezésre állónál használhatják. Teljesítmény, egyszerűen több RAM-MAL rendelkező növeli a óta hello VM további lemezegységet RAM tárolhatja. hello VM választja egy megfelelő méretű toostore hello lemezadatokat, amelyet a virtuális gép hello kell rendelkeznie. Ehhez hasonlóan hello VM elegendő memória tooprovide való hello felügyelni teljesítmény kell rendelkeznie. Ha a szolgáltatás az növekszik adott idő alatt, további virtuális gépek toohello fürt és a partíció hello adatokat felveheti összes hello virtuális gépek között.

## <a name="determine-how-many-nodes-you-need"></a>Határozza meg, hány csomópontok van szüksége
Particionálás a szolgáltatás lehetővé teszi a szolgáltatás kimenő tooscale. A particionálás további információkért lásd: [particionálás Service Fabric](service-fabric-concepts-partitioning.md). Mindegyik partíció hozzá kell férnie egy virtuális, de több (kisméretű) partíció egy virtuális lehet tenni. Igen, több kis partíciót rendelkező nagyobb rugalmasságot biztosít mint rendelkezik néhány nagyobb partíciókat. hello kompromisszum, hogy ha sok partíciók növeli a Service Fabric terhelés, és nem hajtható végre a tranzakciós műveletek közötti partíciók. Emellett további lehetséges a hálózati adatforgalom egy Ha a szolgáltatáskód hibáit gyakran kell tooaccess kódrészletek, amelyek a különböző partíciók élő adatok. A szolgáltatás tervezésekor alaposan gondolja át, egy hatékony particionálási stratégia ezek előnyeit és hátrányait tooarrive.

Tételezzük fel, hogy az alkalmazás rendelkezik, amely az év toogrow tooDB_Size GB várt tároló mérete egyetlen állapotalapú szolgáltatással. További alkalmazások (valamint partíciók) áll hajlandó tooadd túl az adott év növekedési tapasztal.  hello replikációs tényező (RF), a szolgáltatás hatások hello teljes DB_Size a replikák hello számát meghatározó. teljes DB_Size összes replika között hello DB_Size megszorozza replikációs tényező hello.  Node_Size jelöli hello szabad terület/RAM csomópontonként a szolgáltatás kívánt toouse. A legjobb teljesítmény érdekében hello DB_Size kell elférjenek memóriába hello fürt és a Node_Size, amely körül hello RAM a virtuális gép ki kell választani hello. Egy Node_Size hello RAM kapacitás nagyobb kiosztásával hello Service Fabric-futtatókörnyezet által biztosított hello lapozás vannak hagyatkoznia. A teljesítmény, így nem lehet optimális, ha a teljes adat tekinthető toobe működés közbeni (óta majd hello adatok van lapozható vagy kikapcsolása). Azonban számos olyan szolgáltatás, ahol csak egy adatok mekkora hányadát hello az működés közbeni, célszerű költséghatékonyabb.

a maximális teljesítmény szükséges csomópontok száma hello kiszámítása a következőképpen történik:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>A növekedésre fiók
Érdemes lehet a szolgáltatás toogrow, továbbá várt DB_Size hello toohello DB_Size, akkor a megkezdett alapján toocompute hello a csomópontok számát. Csomópontok száma hello majd, nő a szolgáltatást, hogy a rendszer nem túlzott kiosztása csomópontok száma hello növekedésével. De a partíciók számának hello hello van szükség, ha a maximális növekedési, amelyen a szolgáltatás csomópontok száma alapján.

Ez azért van jó toohave néhány további gépek bármikor elérhető, hogy kezelheti bármely váratlan igényeiben jelentkező vagy sikertelen (például, ha néhány virtuális gép leáll).  Hello extra kapacitás az elvárt teljesítményt segítségével kell meghatározni, egyfajta kiindulópontot napjainkban tooreserve néhány további virtuális gépek (5 – 10 % időtartammal extra).

előző hello azt feltételezi, hogy egyetlen állapotalapú szolgáltatás. Ha egynél több állapotalapú szolgáltatásból, hogy tooadd hello társított DB_Size más szolgáltatások hello hello egyenlet be. Azt is megteheti külön-külön az egyes állapotalapú szolgáltatás csomópontok száma hello számíthatja ki.  Előfordulhat, hogy a szolgáltatás replikák és a kívánt nem elosztott terhelésű. Ne feledje, hogy partíciók több adat, mint mások is rendelkezhetnek. A particionálás további információkért lásd: [a gyakorlati tanácsok cikk particionálás](service-fabric-concepts-partitioning.md). Hello előző egyenlet azonban partícióazonosító és másodpéldány független, mert a Service Fabric biztosítja, hogy hello replikák helyezkednek el hello csomópontok közötti optimalizált módon.

## <a name="use-a-spreadsheet-for-cost-calculation"></a>A táblázat használatához a költségszámítás
Most tegyünk néhány valós számok hello képletben. Egy [példa számolótábla](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) bemutatja, hogyan tooplan hello kapacitás az adatok három objektumtípusokat tartalmazó alkalmazást. Az egyes objektumok azt hozzávetőleges mérete és toohave várhatóan hány objektumokat. Azt is választhatja azt szeretnénk, ha az egyes objektumtípusoknál hány replikákat. hello számolótábla hello mekkora memóriát toobe hello fürt tárolt számítja ki.

Azt adja meg a Virtuálisgép-méretet és havi költségét. Alapuló Virtuálisgép-méretet hello, hello számolótábla közli, hogy hello hello csomóponton kell használnia az adatok toophysically elférjen toosplit partíciók minimális száma. Az alkalmazás adott számítási és hálózati forgalmi igényeinek kielégítése partíciók tooaccommodate nagyobb számú lehet felügyelni. hello számolótábla jeleníti meg, egy toosix a hello hello felhasználói profil objektumok kezelt partíciók száma növekedett.

Most ezen adatok alapján hello számolótábla jeleníti meg, hogy fizikailag sikerült-e be a kívánt hello partíciókat és a replikák összes hello adatok 26 csomópontos fürt esetén. Azonban a fürt volna kell sűrűn csomagolt, ezért érdemes néhány további csomópontokat tooaccommodate csomópontok hibáit és a frissítések. hello táblázat is mutatja, hogy több mint 57 csomópontok összevonása nincs további értéke, mert üres csomópontok kellene lennie. Ebben az esetben érdemes lehet fent 57 csomópontok toogo ennek ellenére tooaccommodate csomópontok hibáit és a frissítések. Hello számolótábla toomatch módosíthatja az alkalmazás igényeinek.   

![Számolótáblába, kiszámítása][Image1]

## <a name="next-steps"></a>Következő lépések
Tekintse meg [particionálás Service Fabric szolgáltatások] [ 10] a szolgáltatás partícionálásra vonatkozó további toolearn.

<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-concepts-partitioning.md
