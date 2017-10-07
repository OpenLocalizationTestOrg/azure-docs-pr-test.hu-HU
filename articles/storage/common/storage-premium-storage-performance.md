---
title: "Prémium szintű Azure Storage: Teljesítmény kialakítása |} Microsoft Docs"
description: "Prémium szintű Azure Storage használatával nagy teljesítményű alkalmazások tervezése. Prémium szintű Storage nagy teljesítményű, alacsony késésű támogatása az Azure virtuális gépeken futó I/O-igényes munkaterhelések kínál."
services: storage
documentationcenter: na
author: aungoo-msft
manager: tadb
editor: tysonn
ms.assetid: e6a409c3-d31a-4704-a93c-0a04fdc95960
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: aungoo
ms.openlocfilehash: dde3e60ae4c8387150b65f0715166b5d549891e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-premium-storage-design-for-high-performance"></a>Prémium szintű Azure Storage: Nagy teljesítményű kialakítása
## <a name="overview"></a>Áttekintés
Ez a cikk útmutatást nyújt a prémium szintű Azure Storage használatával nagy teljesítményű alkalmazások létrehozásához. Ez a dokumentum teljesítmény bevált gyakorlatok alkalmazható tootechnologies az alkalmazás által használt együtt hello utasításokat is használhatja. tooillustrate hello útmutatást, az SQL Serveren megtalálható a prémium szintű Storage példaként, ez a dokumentum rendelkezik használtuk.

Amíg oldjuk teljesítmény forgatókönyvek hello tárolási réteg ebben a cikkben, szüksége lesz a toooptimize hello alkalmazásréteg. Például ha a prémium szintű Azure Storage SharePoint-Farm üzemeltet, használhatja hello SQL Server példák a jelen cikk toooptimize hello adatbázis-kiszolgálóhoz. Emellett hello SharePoint-Farm webkiszolgáló és az alkalmazás server tooget hello legtöbb teljesítményének optimalizálásához.

Ez a cikk segít a prémium szintű Azure Storage alkalmazás teljesítményének optimalizálására kapcsolatos gyakori kérdéseket a következő válasz

* Hogyan toomeasure az alkalmazások teljesítménye?  
* Miért meg nem jelenik meg várt nagy teljesítményű?  
* Mely tényezők befolyásolják az alkalmazások teljesítményéről, a prémium szintű Storage?  
* Hogyan hajtsa végre ezeket a tényezők befolyásolják a prémium szintű Storage az alkalmazás teljesítményének?  
* Hogyan is optimalizálhatja az IOPS, sávszélesség és a késleltetés?  

Adtunk ezeket az irányelveket kifejezetten a prémium szintű Storage, mert prémium szintű Storage munkaterheléseinek magas teljesítmény-és nagybetűket. Példák adtunk meg, ahol. Ezek szabványos tárolólemezek az infrastruktúra-szolgáltatási virtuális gépeken futó irányelvek tooapplications némelyike is alkalmazhatja.

Mielőtt elkezdené, ha új tooPremium tárolási, elolvashatja hello [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../storage-premium-storage.md) és [Azure Storage méretezhetőségi és Performance Targets](storage-scalability-targets.md)cikkeket.

## <a name="application-performance-indicators"></a>Alkalmazás teljesítménymutatók
Azt ellenőrzéséhez, hogy helyes-e használatával teljesítmény mutatókat, például egy alkalmazás hajt végre, hogy milyen gyorsan alkalmazás feldolgozása egy felhasználói kérelem mennyi adatot az alkalmazás kérés feldolgozása, hány kér az alkalmazás feldolgozása az egy adott időtartam, hogy mennyi ideig a felhasználó rendelkezik-e toowait tooget választ a kérés elküldése után. hello szakkifejezések a teljesítménymutatók, IOPS, az átviteli vagy a sávszélesség és a késleltetés.

Ebben a szakaszban ismertetjük hello közös teljesítménymutatók a prémium szintű Storage hello környezetében. A következő szakaszban, Alkalmazáskövetelményeket összegyűjtéséhez hello, megtudhatja, hogyan toomeasure ezek teljesítménymutatók az alkalmazáshoz. Később az alkalmazások teljesítményének optimalizálása megtudhatja, hogyan hello tényezők a teljesítmény mutatók és javaslatok toooptimize érintő őket.

## <a name="iops"></a>IO
IOPS-érték számát, hogy az alkalmazás által küldött toohello tárolólemezek egy második kéri. Egy bemeneti/kimeneti műveleti olvashatók a sémaadatok, vagy szekvenciális vagy véletlenszerű írását. OLTP alkalmazások, például egy online kereskedelmi webhelyen sok egyidejű felhasználói kérelmek azonnal tooprocess kell. hello felhasználói kérelmek insert és igényes adatbázis-tranzakciók, mely hello alkalmazás gyorsan kell dolgoznia frissítése. Ezért OLTP alkalmazásoknak nagyon magas iops-érték. Ilyen alkalmazásokat kezeléséhez a kis- és véletlenszerű I/O kérelmek több millió. Ha egy kérelmet, meg kell kialakítása hello alkalmazás infrastruktúra toooptimize IOPS. A hello későbbi szakaszában, *alkalmazások teljesítményének optimalizálása*, tárgyaljuk részletesen, figyelembe kell vennie tooget összes hello tényezők magas iops-érték.

Ha csatlakoztat egy prémium szintű tároló lemez tooyour nagy méretű VM Azure rendelkezések meg IOPS garantált számos hello lemez specifikáció szerint. Például egy P50 lemez 7500 IOPS látja el. Minden nagy méretű Virtuálisgép-méretet is rendelkezik egy adott IOPS-korláttal is kiálló tárolókkal. Például egy szabványos GS5 virtuális gép rendelkezik 80000 IOPS korlátozza.

## <a name="throughput"></a>Teljesítmény
Átviteli vagy a sávszélesség nem hello adatmennyiséget, hogy az alkalmazás által küldött toohello tárolólemezek egy megadott időszakban. Az alkalmazás IO-egység nagy méretű bemeneti/kimeneti műveleteket hajt végre, ha magas teljesítményt igényel. Adatraktár alkalmazások általában tooissue vizsgálat számításigényes műveletek nagy részének adatok elérése egyszerre és általában a tömeges műveletek végrehajtására. Ez azt jelenti az ilyen alkalmazások magasabb teljesítményt igényelnek. Ha egy kérelmet, az infrastruktúra toooptimize kell tervezésekor átviteli sebesség eléréséhez. Hello a következő szakaszban arról lesz szó részletes hello tényezők ez tooachieve kell hangolását.

Ha csatlakoztat egy prémium szintű tároló lemez tooa nagy méretű virtuális gép, Azure rendelkezések átviteli adott lemez specifikáció szerint. Például egy P50 lemez második lemezenként átviteli kiépítését 250 MB. Minden nagy méretű Virtuálisgép-méretet is kiálló tárolókkal adott átviteli korlátozás is rendelkezik. Például szabványos GS5 virtuális Gépnek legyen 2000 MB maximális átviteli sebességgel száma másodpercenként. 

Nincs olyan átviteli sebesség és IOPS, ahogy az alábbi képlet hello közötti kapcsolat.

![](media/storage-premium-storage-performance/image1.png)

Ezért fontos toodetermine hello optimális átviteli sebesség és IOPS értékeket, az alkalmazás által is. Próbálja ki valamelyik toooptimize, más hello is lekérdezi hatással. Egy későbbi szakasz ismerteti *alkalmazások teljesítményének optimalizálása*, iops-érték és a teljesítmény optimalizálása a további részleteket ismertetik.

## <a name="latency"></a>Késés
Késés hello időt egy alkalmazás tooreceive egyetlen kérelem, elküldi a toohello tárolólemezek és küldhet hello válasz toohello ügyfél. Ez a kritikus mérték hozzáadása tooIOPS és az átvitel egy alkalmazás teljesítménye. hello késés egy prémium szintű storage-lemez hello időt vesz igénybe egy kérelem tooretrieve hello információinak, és közölje tooyour alkalmazás vissza. Prémium szintű Storage konzisztens kis késleltetést biztosít. Ha engedélyezi a csak olvasható-állomás gyorsítótárazását a prémium szintű storage lemezeken, sokkal alacsonyabb olvasási késése kaphat. Ismertetjük lemez gyorsítótárazása a szakasz későbbi részében részletesebben a *alkalmazások teljesítményének optimalizálása*.

Ha az alkalmazás tooget optimalizálása magasabb iops-érték és a teljesítményt, az hatással hello késleltetés az alkalmazás. Hello az alkalmazások teljesítményének hangolása, után kiértékelésének eredménye mindig hello hello alkalmazás tooavoid késését váratlan nagy késleltetésű viselkedését.

## <a name="gather-application-performance-requirements"></a>Alkalmazás teljesítmény követelmények összegyűjtése
prémium szintű Azure Storage futó nagy teljesítményű alkalmazások tervezésének első lépése hello, toounderstand hello teljesítménye az alkalmazás követelményeinek. Miután összegyűjtötte teljesítménykövetelményeknek, optimalizálhatja a tooachieve hello legoptimálisabb alkalmazásteljesítmény.

Hello előző szakaszban a Microsoft hello közös teljesítménymutatók, IOPS, az átviteli sebesség és a késleltetés ismertetése. Meg kell adnia a teljesítménymutatók közül kritikus tooyour alkalmazás toodeliver hello szükséges felhasználói élményt. Magas iops értéket számít, például a legtöbb tooOLTP alkalmazások másodpercenként több millió tranzakciók feldolgozása. Mivel a magas teljesítmény fontos a nagy mennyiségű adat feldolgozása egy második Data Warehouse-alkalmazások. Valós idejű alkalmazások, például élő videoadatfolyam webhelyek elengedhetetlen a rendkívül alacsony késleltetés.

A következő mértékcsoport hello maximális hálózatiteljesítmény-igények teljes élettartamuk az alkalmazás. A kezdés hello minta ellenőrzőlista alatt használják. Rekord hello maximális teljesítménykövetelményeknek során normális, maximális és a munkaidőn kívüli munkaterhelés időszakokat. Az összes munkaterhelés szint követelményeinek azonosításával képes toodetermine lesz az alkalmazás általános teljesítmény-követelmény hello. Például az elektronikus kereskedelmi webhely hello normál munkaterhelését lesz hello tranzakciók működik a legtöbb nap során. hello maximális munkaterhelést hello webhely lesz hello tranzakciók során ünnepi vagy különleges pénztári események szolgál. hello maximális munkaterhelést általában tapasztalt korlátozott időtartamra, de megkövetelheti az alkalmazás tooscale két vagy több alkalommal fordult elő a normál működés. Hello 50. percentilis, 90 PERCENTILIS és 99 PERCENTILIS követelmények megállapítása. Ez segít a hello teljesítménykövetelményeknek bármely kiugró kiszűrhetők, és a próbálkozások összpontosíthat optimalizálja a hello megfelelő értékeket.

**Alkalmazás teljesítmény követelmények ellenőrzőlista**

| **Teljesítménnyel kapcsolatos követelmények** | **50. percentilis** | **90 százalékos érték** | **99 PERCENTILIS** |
| --- | --- | --- | --- |
| Legfeljebb Másodpercenkénti tranzakciók | | | |
| % Olvasási műveletek | | | |
| % Írási műveletek | | | |
| % Véletlenszerű műveletek | | | |
| % Egymást követő műveletek | | | |
| IO-kérelem méret | | | |
| Átlagos átviteli sebessége | | | |
| Legfeljebb Teljesítmény | | | |
| Perc. Késés | | | |
| Átlagos késleltetése | | | |
| Legfeljebb CPU | | | |
| Átlagos CPU | | | |
| Legfeljebb Memory (Memória) | | | |
| Átlagos memória | | | |
| Várólistamélység | | | |

> [!NOTE]
> Vegye figyelembe ezeket a számokat az alkalmazás a várt jövőbeli növekedésére alapján méretezés. Egy jó ötlet tooplan növekedésének megfelelően időben, mert nehezebb toochange hello infrastruktúra később a teljesítmény fokozása lehet.
>
>

Ha egy meglévő alkalmazást, és szeretné, hogy toomove tooPremium tárolási, először létre hello ellenőrzőlista fent hello meglévő alkalmazáshoz. Ezt követően összeállítása a prémium szintű Storage és tervezési irányelveket leírtak alapján hello alkalmazás az alkalmazás egy prototípus *alkalmazások teljesítményének optimalizálása* a jelen dokumentum későbbi szakaszában. hello következő szakasz toogather hello TELJESÍTMÉNYMÉRÉSEK használható hello eszközöket írja le.

Létrehoz egy ellenőrzőlista hasonló tooyour meglévő alkalmazást hello prototípus. Benchmarking eszközökkel hello munkaterhelések szimulálhatja és mérheti hello prototípus alkalmazás teljesítményére. Hello című rész a [Benchmarking](#benchmarking) további toolearn. Így ellenőrizheti, hogy prémium szintű Storage egyezik-e, vagy művelet túllépje a alkalmazás teljesítményre vonatkozó követelmények módon. Akkor is létrehozható hello azonos irányelveket az éles alkalmazáshoz.

### <a name="counters-toomeasure-application-performance-requirements"></a>Teljesítményszámlálók toomeasure alkalmazás hálózatiteljesítmény-igények
az alkalmazás legjobb módja toomeasure teljesítménybeli követelményei hello, hello kiszolgálón hello operációs rendszer által biztosított toouse teljesítményfigyelő eszközöket. A Windows Teljesítményfigyelő és iostat Linux használhatja. Ezek az eszközök megfelelő tooeach mérték, tekintse meg a fenti szakaszban hello számlálók rögzíti. Ezek a számlálók értékeit hello rögzíteni kell, amikor az alkalmazás fut, a normál, maximális és a munkaidőn kívüli munkaterhelések.

processzor, memória, és minden egyes logikai lemez és a kiszolgáló fizikai lemez hello teljesítményszámlálók érhetők el. Prémium szintű storage lemezek használatakor egy virtuális gép hello fizikai lemez számlálók prémium szintű storage lemezek, és logikai lemez számlálók hello prémium szintű storage lemezeken létrehozott minden kötet. Az alkalmazás munkaterhelés üzemeltető lemezek hello hello értékek rögzíteni kell. Ha van egy tooone társítás logikai és fizikai lemezek közötti, olvassa el a lemez számlálók toophysical; Ellenkező esetben tekintse meg a logikai lemez számlálók toohello. Linux hello iostat parancs lemez- és CPU-kihasználtság jelentést hoz létre. hello lemezhasználati jelentést kell készítenie a fizikai eszköz vagy a partíció statisztikai biztosít. Ha az adatokhoz és a naplófájlhoz az adatbázis-kiszolgáló külön lemezeken, az adatgyűjtést mindkét lemezhez. Alábbi táblázat ismerteti a lemezek, a processzor és memóriájára vonatkozóan:

| A számláló | Leírás | Teljesítményfigyelő | Iostat |
| --- | --- | --- | --- |
| **IOPS vagy tranzakciók száma másodpercenként** |I/o-kérelmek száma másodpercenként toohello tároló lemez ki. |Lemezolvasások/mp <br> Lemezírás/mp |TP-k <br> r/s <br> w/s |
| **Lemez olvasása és írása** |%-át olvasási és írási hello lemezen végzett műveleteket. |% Lemez olvasási idő <br> % Lemezre írási ideje |r/s <br> w/s |
| **Átviteli sebesség** |Írni vagy olvasni toohello lemezre másodpercenként adatok mennyisége. |Lemez sebessége olvasott bájt/mp <br> Lemez írási bájtok/s |kB_read/s <br> kB_wrtn/s |
| **Késés** |Teljes idő toocomplete lemez IO kérelmet. |Átlagos lemez mp/Olvasás <br> Átlagos lemez mp/írás |await <br> svctm |
| **IO-méret** |i/o-kérelmek hello mérete toohello tárolólemezek ad ki. |Átlagos lemezátviteli/beolvasott bájtok száma <br> Átlagos írási bájtok/írás |avgrq-sz |
| **Várólistamélység** |A lekérdezések várakozási toobe űrlap olvasása vagy írása történik toohello tároló lemez függőben lévő i/o száma. |Lemezvárólista jelenlegi hossza |avgqu-sz |
| **Max. Memória** |Zökkenőmentes a szükséges toorun alkalmazás memória mérete |% Előjegyzett memória |Vmstat használata |
| **Max. PROCESSZOR** |Zökkenőmentes a szükséges toorun alkalmazás CPU mérete |Processzor kihasználtsága |% haszn. |

További információ [iostat](http://linuxcommand.org/man_pages/iostat1.html) és [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).

## <a name="optimizing-application-performance"></a>Az alkalmazások teljesítményének optimalizálása
prémium szintű Storage futó alkalmazást teljesítményét befolyásoló hello fő tényezők jellegét a IO kérelmeket, Virtuálisgép-méretet, a lemez mérete, lemezek, a lemez gyorsítótár, a többszálas végrehajtás és várólistamélység száma. Ezek a tényezők némelyike hello rendszer által biztosított forgatógombját vezérelhető. A legtöbb alkalmazás nem kaphat egy beállítás tooalter hello IO-méret és várólistamélység közvetlenül. Például az SQL Server használatakor nem választható, hello I/O méret- és várólista mélységét. SQL Server legtöbb teljesítmény hello optimális I/O méret- és várólista mélysége értékek tooget hello választja ki. Így toomeet szükségleteknek megfelelő erőforrásokat oszthat is fontos toounderstand hello hatásait mindkét típusú tényezők az alkalmazás teljesítményére.

Ebben a szakaszban tekintse meg a toohello alkalmazás követelmények ellenőrzőlista létrehozott, tooidentify mennyi kell toooptimize, az alkalmazás teljesítményét. Alapján, hogy fogja tudni toodetermine amely tényezők ebben a szakaszban ismertetett meg kell tootune. toowitness hello az alkalmazásteljesítmény megtekintéséhez futtassa eszközök az Alkalmazásbeállítás a teljesítménymérésre minden tényező gyakorolt hatásait. Tekintse meg a toohello [Benchmarking](#Benchmarking) szakasz hello lépéseket toorun közös teljesítménymérésre eszközök a Windows és Linux virtuális gépek a jelen cikk végén.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>Egy pillanat alatt IOPS, az átviteli sebesség és a késleltetés optimalizálása
hello az alábbi táblázat foglalja össze az összes hello teljesítmény tényezők és hello lépéseket toooptimize IOPS, az átviteli sebesség és késés. hello az összegzés a következő részekben egyes tényező sokkal több mélységét.

| &nbsp; | **IOPS** | **Átviteli sebesség** | **Késés** |
| --- | --- | --- | --- |
| **Példa** |Vállalati OLTP alkalmazás igénylő nagyon magas tranzakciók második sebessége. |Vállalati adatraktározási alkalmazás feldolgozási nagymennyiségű adat. |Közel valós idejű a azonnali válaszok toouser kérések, például online játékok igénylő alkalmazásokhoz. |
| Teljesítmény tényezők | &nbsp; | &nbsp; | &nbsp; |
| **IO-méret** |Kisebb IO-méretet eredményez magasabb iops-érték. |IO-mérete nagyobb tooyields nagyobb átviteli sebességgel. | &nbsp;|
| **Virtuálisgép-mérettel** |A Virtuálisgép-méretet, amely nagyobb, mint az alkalmazás követelményeinek IOPS nyújt használja. Virtuálisgép-méretek és az IOPS-korlátok vonatkoznak itt talál. |A Virtuálisgép-méretet használata átviteli korlát nagyobb, mint az alkalmazás követelményeinek. Virtuálisgép-méretek és az átviteli sebességének korlátai itt talál. |Használja a virtuális gép méretét, hogy ajánlatok méretezési korlátok nagyobb, mint az alkalmazás követelményeinek. Virtuálisgép-méretek és azok korlátok itt talál. |
| **Lemez mérete** |A lemez mérete nagyobb, mint az alkalmazás követelményeinek IOPS által használható. Lemez méretét és az IOPS-korlátok vonatkoznak itt talál. |A lemez mérete nagyobb, mint az alkalmazás követelményeinek átviteli korlátot használja. Lemez méretét és az átviteli sebességének korlátai itt talál. |Használja a lemez méretét, hogy ajánlatok méretezési korlátok nagyobb, mint az alkalmazás követelményeinek. Lemezméret és azok korlátok itt talál. |
| **Virtuális gép és a lemez méretkorlátai** |Hello Virtuálisgép-méretet választott IOPS-korláttal prémium tárolólemezek által vezérelt teljes IOPS csatolt tooit nagyobbnak kell lennie. |Átviteli sebesség korlát hello Virtuálisgép-méretet választott prémium tárolólemezek által vezérelt teljes átviteli sebesség csatolt tooit nagyobbnak kell lennie. |Hello Virtuálisgép-méretet választott méretkorlátai teljes méretkorlátai csatolt prémium tárolólemezek nagyobbnak kell lennie. |
| **A lemezes gyorsítótárazás** |Csak olvasható gyorsítótárának engedélyezése a prémium szintű tároló lemez is van olvasási műveletek nehéz tooget magasabb olvasási iops-érték. | &nbsp; |Prémium szintű storage lemezeken készen nehéz műveletek tooget nagyon alacsony olvasási késések fordulnak elő a csak olvasható gyorsítótárának engedélyezése. |
| **Lemez csíkozást** |Több lemez használata, és együtt tooget egy kombinált magasabb iops-érték és átviteli kapacitás paritásos őket. Vegye figyelembe, hogy a kombinált felső határ az egyes virtuális gép hello csatolt premium lemezek kombinált határértékeinek hello-nél nagyobb számot kell lennie. | &nbsp; | &nbsp; |
| **Paritásos mérete** |Paritásos mérete kisebb véletlenszerű kisméretű I/O minta OLTP alkalmazások látható. SQL-kiszolgáló OLTP alkalmazás például használja a 64 KB-os méret paritásos. |Adatraktár alkalmazások látható szekvenciális nagy IO minta nagyobb paritásos mérete. Például használható a paritásos mérete 256 KB-os SQL Server Data warehouse alkalmazás. | &nbsp; |
| **Többszálas** |Használja a többszálas toopush nagyobb számú kérelmek tooPremium, amelyek toohigher IOPS tárolási és átviteli sebességet. SQL-kiszolgálón állítsa például egy magas MAXDOP érték tooallocate további processzorok tooSQL kiszolgáló. | &nbsp; | &nbsp; |
| **Várólistamélység** |Nagyobb várólistamélység magasabb IOPS adja eredményül. |Nagyobb várólistamélység nagyobb átviteli teljesítményt eredményez. |Kisebb várólistamélység kisebb késések adja eredményül. |

## <a name="nature-of-io-requests"></a>IO-kérelmek jellege
IO-kérelmet, amely az alkalmazás végrehajtását fogja bemeneti/kimeneti műveleti egység. Azonosító hello jellege IO kérelmeket, véletlenszerű vagy egymást követő, olvassa el, vagy írási, kisebb vagy nagyobb, segítenek meghatározni az alkalmazás követelményeinek hello teljesítményét. IO-kérelmeket, toomake hello jobb döntéseket az alkalmazás-infrastruktúra tervezésekor nagyon fontos toounderstand hello jellege.

IO-méret egyike hello több fontos tényezők, melyeknek. IO-méret hello mérete hello az alkalmazás által generált hello bemeneti/kimeneti műveleti kérelem. IO-méret hello jelentős hatással van a teljesítményre, különösen a hello IOPS és alkalmazás hello sávszélesség képes tooachieve. hello következő képlet mutatja IOPS, hello kapcsolatát IO-méret és a sávszélesség/átviteli sebességet.  
    ![](media/storage-premium-storage-performance/image1.png)

Bizonyos alkalmazások lehetővé teszik a IO mérete, amíg az egyes alkalmazások azonban nem tooalter. Például az SQL Server hello optimális IO-méret maga határozza meg, és nem biztosít a felhasználók bármely forgatógombját toochange azt. A hello ugyanakkor, Oracle biztosít egy paraméter, [DB\_BLOKKOLÁSA\_mérete](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) használatával, amely konfigurálható hello hello adatbázis i/o-kérés méretét.

Ha használ egy alkalmazást, amely akkor toochange hello IO-méret nem teszi lehetővé, használja a hello útmutatást Ez a cikk toooptimize hello teljesítményét, amely a leginkább releváns tooyour alkalmazás KPI. Például:

* OLTP okoz a kis- és véletlenszerű I/O kérelmek több millió. ilyen típusú IO toohandle kér, alakítson ki az kell az alkalmazás-infrastruktúra tooget magasabb iops-érték.  
* Egy alkalmazás adatraktározási nagy és a szekvenciális I/O kérelmeket állít elő. toohandle ezek írja be a IO kérelmek, kell hogy alakítson ki az alkalmazás-infrastruktúra tooget nagyobb sávszélességet vagy átviteli sebesség.

Ha egy alkalmazás, amely lehetővé teszi a toochange hello I/O méret, használ használni a tapasztalatok hello IO méretezés továbbá tooother teljesítmény-irányelvek szerint

* IO-mérete kisebb tooget magasabb iops-érték. Például 8 KB-os OLTP alkalmazásokra vonatkozóan.  
* IO-mérete nagyobb tooget nagyobb sávszélesség/átviteli sebességgel. Például 1024 KB data warehouse alkalmazáshoz.

Íme egy példa a hogyan számolhatja hello IOPS és átviteli/sávszélesség az alkalmazáshoz. Érdemes lehet egy alkalmazás P30 lemezt használ. hello maximális IOPS és átviteli/sávszélesség P30 lemez érhető el az 5000 iops teljesítményt és 200 MB másodpercenként kulcsattribútumokkal. Most ha az alkalmazás maximális IOPS hello P30 lemez, és használjon 8 KB-os, például kisebb IO-méretet hello fogja sávszélesség eredő hello képes tooget 40 MB másodpercenkénti számát. Azonban ha az alkalmazás hello maximális átviteli sebesség/sávszélesség P30 lemezről, és IO nagyobb mint 1024 KB méretű használ, hello eredményül kapott IOPS lesz kisebb, 200 iops-érték. Ezért hangolására hello I/O méret, úgy, hogy mind az alkalmazás iops-érték és átviteli/sávszélesség követelménynek megfelel-e. Az alábbi táblázat összefoglalja a hello különböző IO méretű és a megfelelő IOPS és átviteli P30 lemez.

| Alkalmazás követelményeinek | I/o-mérete | IO | Átviteli sebesség/sávszélesség |
| --- | --- | --- | --- |
| Maximális iops-érték |8 KB |5,000 |40 MB / s |
| Maximális átviteli sebesség |1024 KB |200 |200 MB / s |
| Maximális átviteli sebesség + magas iops-érték |64 KB |3,200 |200 MB / s |
| Maximális iops-érték + magas teljesítmény |32 KB |5,000 |160 MB / s |

tooget IOPS és egy egyetlen prémium szintű tároló lemez hello maximális értéknél nagyobb sávszélességet használja több premium lemezek csíkozott együtt. Például paritásos két P30 lemezek tooget egy 10 000 IOPS kombinált IOPS vagy egy kombinált átviteli sebesség 400 MB / s. Hello a következő szakaszban leírtak a Virtuálisgép-méretet, amely támogatja a kombinált hello lemez iops-érték és az átvitel kell használnia.

> [!NOTE]
> Növeli vagy IOPS, illetve más átviteli hello is fokozza, ellenőrizze, hogy nem kattint az átviteli vagy IOPS-korlátok vonatkoznak hello lemezének vagy a virtuális gép amikor vagy az egyik növelését.
>
>

toowitness hello IO-méret gyakorolt alkalmazások teljesítménye, összehasonlítási eszközök futtathatja a virtuális gép és a lemezek. Hozzon létre több tesztelési futtatják, és különböző IO-méret minden futtatási toosee hello hatás. Tekintse meg a toohello [Benchmarking](#Benchmarking) szakaszban olvashat a jelen cikk végén hello.

## <a name="high-scale-vm-sizes"></a>Nagy méretű VM-méretek
Amikor elkezdi az alkalmazások fejlesztése, hello első dolog toodo egyik, válassza ki a virtuális gép toohost az alkalmazást. Prémium szintű Storage nagy skálázási Virtuálisgép-méretek, futtatható egy magas helyi lemez i/o-teljesítmény és nagyobb számítási teljesítményt igénylő alkalmazásokhoz tartalmaz. A virtuális gépek hello helyi lemez gyorsabb processzorok, memória-core magasabb arány és egy Solid-State meghajtót (SSD) adja meg. Prémium szintű Storage támogató magas méretezési virtuális gépek példák hello DS, DSv2 és GS adatsorozat virtuális gépeket.

Magas méretezési virtuális gépek különböző méretű mag, memória, az operációs rendszer és ideiglenes lemezméretet eltérő számú érhetők el. Minden egyes Virtuálisgép-méretet is rendelkezik, hogy csatolhat a virtuális gép toohello adatlemezek maximális száma. Ezért hello a kiválasztott Virtuálisgép-méretet érinti, mennyi feldolgozás, a memória és a tárolási kapacitás érhető el az alkalmazást. Hello számítási és tárolási költségű is érinti. Például az alábbiakban hello legnagyobb Virtuálisgép-méretet a DS-ben több, DSv2 adatsorozat és GS több hello jellemzői:

| Virtuális gép mérete | Processzormagok | Memory (Memória) | Virtuális gép mérete | Legfeljebb Az adatlemezek | Gyorsítótár mérete | IO | Gyorsítótár IO sávszélességkorlátok |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS14 |16 |112 GB |AZ OPERÁCIÓS RENDSZER 1023 GB-OS = <br> Helyi SSD 224 GB = |32 |576 GB |50 000 IOPS <br> 512 MB / s |4000 IOPS és 33 MB / s |
| Standard_GS5 |32 |448 GB |AZ OPERÁCIÓS RENDSZER 1023 GB-OS = <br> Helyi SSD 896 GB = |64 |4224 GB |80000 IOPS <br> 2000 MB / s |5000 iops teljesítményt és 50 MB / s |

tooview teljes listáját és az összes elérhető Azure Virtuálisgép-méretek, tekintse meg a túl[Windows Virtuálisgép-méretek](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vagy [Linux Virtuálisgép-méretek](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Válassza ki a virtuális gép méretét, amely megfelel, és a skála tooyour szükséges alkalmazás teljesítménykövetelményekhez. Toothis, továbbá a Virtuálisgép-méretek kiválasztásakor a következő szempontokat figyelembe venni.

*Méretkorlátai*  
hello maximális IOPS-korlátok vonatkoznak virtuális gépenként, és lemezenként különböző és egymástól független. Győződjön meg arról, hogy hello alkalmazás befolyásoló tényezők IOPS hello VM, valamint a hello premium lemezek csatolt tooit hello határain belül. Ellenkező esetben az alkalmazások teljesítményének szabályozása fog tapasztalni.

Tegyük fel tegyük fel, hogy egy alkalmazás legfeljebb 4000 IOPS mérete. tooachieve e, hogy rendelkezés a virtuális gép DS1 P30 lemez. hello P30 lemezre mentése too5, 000 IOPS biztosíthat. Hello DS1 VM azonban korlátozott too3, 200 iops-érték. Következésképpen hello alkalmazásteljesítmény fog kell korlátozza hello VM határérték 3,200 IOPS, és a teljesítmény csökkenését lesz. tooprevent ebben az esetben válassza ki a virtuális gépek és lemezméret, amelynek mindkét találkozik alkalmazás követelményeinek.

*A művelet költsége*  
Sok esetben az is elképzelhető, hogy a prémium szintű Storage használatával művelet teljes költsége alacsonyabb, mint a standard szintű tárolást használ.

Vegyük példaként 16 000 IOPS igénylő alkalmazás. tooachieve a teljesítmény, szüksége lesz egy szabványos\_biztosíthat 16000 32 standard tárolási 1 TB méretű lemezek használatával a maximális IOPS D14 Azure IaaS virtuális. Minden 1TB standard tároló lemez legfeljebb 500 IOPS érhető el. hello becsült, a virtuális gép havi költségét $1,570 lesz. hello havi költségét 32 szabványos tárolólemezek $1,638 lesz. hello becsült, teljes havi költségét $3,208 lesz.

Azonban ha Ön hello azonos alkalmazás a prémium szintű Storage, szüksége lesz egy kisebb Virtuálisgép-méretet és kevesebb a prémium szintű storage lemezt, így csökkenti az általános költségeket hello. Szabványos\_DS13 VM négy P30 lemezekkel hello 16000 IOPS követelménynek megfelel. hello DS13 VM 25,600 a maximális iops-érték tartozik, és minden P30 lemezhez tartozik egy maximális IOPS az 5000. A teljes, ez a konfiguráció érhető el 5000 x 4 = 20 000 iops-érték. hello becsült, a virtuális gép havi költségét $1,003 lesz. prémium szintű storage-lemezek négy P30 havi költségét hello $544.34 lesz. hello becsült, teljes havi költségét $1,544 lesz.

Az alábbi táblázat összefoglalja a hello költség részletes információkat ebben a forgatókönyvben a Standard és prémium szintű Storage.

| &nbsp; | **Standard** | **Prémium** |
| --- | --- | --- |
| **Havonta VM költsége** |$1,570.58 (szabványos\_D14) |$1,003.66 (szabványos\_DS13) |
| **A lemezekkel kapcsolatos költségek havonta** |$1,638.40 (32 x 1 TB-os lemezeken) |$544.34 (4 x P30 lemez) |
| **Teljes költség szempontjából, havonta** |$3,208.98 |$1,544.34 |

*Linux Disztribúciókkal*  

Prémium szintű Azure Storage hello kap a Windows és Linux rendszerű virtuális gépek teljesítménye azonos szinten. Sok változatban is elkészíti a Linux disztribúciókkal támogatott, és hello teljes listáját megtekintheti [Itt](../../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Fontos, hogy a különböző disztribúciókkal jobbak toonote olyan ajánlott különböző típusú munkaterheléseket. Attól függően, hogy a munkaterhelés futó hello distro különböző teljesítményszintet jelenik meg. Az alkalmazással hello Linux disztribúciókkal tesztelése, és válassza ki a legjobban működő hello.

Prémium szintű Storage a Linux operációs rendszert futtató, ellenőrizze az hello a legújabb frissítéseket a szükséges illesztőprogramok tooensure magas teljesítmény.

## <a name="premium-storage-disk-sizes"></a>Prémium szintű tároló lemez mérete
Prémium szintű Storage jelenleg kínál hét mérete. Minden lemez mérethatár különböző méretezési IOPS, sávszélesség és tárolására. Válassza ki a megfelelő prémium szintű tároló lemez méretét hello hello követelményeit és hello nagy méretű virtuális gép méretétől függően. hello az alábbi táblázat hello hét lemezt méretű és képességeit. P4 és P6 méretek jelenleg csak a felügyelt lemezek támogatott.

| Prémium szintű lemezek típusa  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Lemezméret           | 32 GB | 64 GB | 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPS-érték lemezenként       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Adattovábbítás lemezenként | 25 MB / s  | 50 MB / s  | 100 MB / s | 150 MB / s | 200 MB / s | 250 MB / s | 250 MB / s | 


Hello lemezen határozza meg, hogy hány lemezek választott méretét. Használhat egyetlen P50 lemez vagy több P10 lemezek toomeet az alkalmazás követelményeinek. Alább felsorolt hello választott meghozásakor fiókokkal kapcsolatos megfontolások figyelembe.

*(Iops-érték és átviteli) méretkorlátai*  
hello iops-érték és az átviteli Sebességkorlát minden prémium szintű lemez méretének hello VM skálázási korlátai különböző és független származik. Győződjön meg arról, hogy a teljes IOPS hello és átviteli hello lemezekből hello méretezési határain belül kiválasztott Virtuálisgép-méretet.

Ha például egy alkalmazás mérete legfeljebb 250 MB/s átviteli sebesség és DS4 virtuális gép egyetlen P30 lemezt használ. DS4 VM hello too256 MB/s átviteli sebesség is feladták. Azonban egyetlen P30 lemez rendelkezik 200 MB/s átviteli sebesség korlátot. Következésképpen hello alkalmazás korlátozott, 200 MB/s toohello lemez korlát miatt lehet. tooovercome ezt a határt, egynél több adat lemezek toohello virtuális gép kiépítése, vagy a lemezek tooP40 vagy P50 átméretezése.

> [!NOTE]
> Hello gyorsítótár által kiszolgált olvasás nem szerepelnek az IOPS hello lemez és a teljesítményt, ezért nem tulajdonos toodisk korlátok. Gyorsítótár a külön iops-érték és átviteli felső határ az egyes virtuális gép van.
>
> Például kezdetben az olvasási és írási műveletek a következők 60MB/s és 40MB/s kulcsattribútumokkal. Idővel hello gyorsítótár bemelegedett, és több és több hello olvasások hello gyorsítótárból szolgál. Ezt követően kaphat a hello lemezről magasabb írási teljesítmény.
>
>

*Lemezek száma*  
Szüksége lesz, mivel felméri alkalmazáskövetelményeket lemezek hello számát határozza meg. Minden egyes Virtuálisgép-méretet a hello számára, hogy csatolhat a virtuális gép toohello lemez is van korlátozva. Ez általában a magok kétszer hello száma. Győződjön meg arról, hogy hello úgy dönt, hogy a szükséges lemez hello számú Virtuálisgép-méretet.

Ne feledje, hogy hello prémium szintű Storage, hogy nagyobb teljesítményt képest képességek tooStandard tárolólemezek. Ezért, ha az alkalmazás Azure IaaS virtuális Standard tárolási tooPremium Storage használatával telepít, valószínűleg szüksége lesz kevesebb premium lemezek tooachieve hello azonos vagy nagyobb teljesítményt az alkalmazáshoz.

## <a name="disk-caching"></a>A lemezes gyorsítótárazás
Magas méretezési virtuális gépeket, amely kihasználja a prémium szintű Azure Storage BlobCache nevezett többrétegű gyorsítótárazási technológiát rendelkezik. BlobCache hello RAM a virtuális gép és a helyi SSD használ a gyorsítótárazáshoz. Ez a gyorsítótár hello prémium szintű Storage állandó és hello virtuális gép helyi lemezek érhető el. Alapértelmezés szerint a gyorsítótár beállítás értéke tooRead/írás operációsrendszer-lemezek és a prémium szintű Storage üzemeltetett adatlemezek csak olvasható. Rendelkező lemez hello prémium szintű Storage lemezeken, hello nagy méretű virtuális gépek érhető el a rendkívül magas szintű teljesítmény, mint a hello alapul szolgáló lemez teljesítménye.

> [!WARNING]
> Az Azure-lemezek hello gyorsítótár beállításainak megváltoztatása szervezőről, és újra csatolja az hello céllemezt. Ha hello operációsrendszer-lemez, hello virtuális gép újraindul. Állítsa le az összes alkalmazások és szolgáltatások, előfordulhat, hogy a megszakítás előtt hello lemezgyorsítótár-beállítás módosítása nem érinti.
>
>

toolearn BlobCache működésével kapcsolatos további információkért tekintse meg a toohello belül [prémium szintű Azure Storage](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) blogbejegyzést.

Lemezek megfelelő halmazát hello fontos tooenable gyorsítótárában. E kell gyorsítótárazása lemezen prémium lemezen, vagy nem függ hello munkaterhelés mintát, hogy a lemez végző. Az alábbi táblázat hello alapértelmezett operációsrendszer- és adatlemezek-gyorsítótárának beállításait.

| **Lemeztípus** | **Alapértelmezett gyorsítótár beállítás** |
| --- | --- |
| Operációsrendszer-lemez |ReadWrite |
| Adatlemez |None |

Következő vannak hello adatlemezek, ajánlott lemez-gyorsítótárának beállításait

| **Gyorsítótárazása lemezen** | **Ha ajánlást toouse ezt a beállítást** |
| --- | --- |
| None |Gazdagép-gyorsítótár beállítása None a csak írható és írási műveleteket. |
| csak olvasható |Gazdagép-gyorsítótár beállítása csak olvasható a csak olvasható és írható-olvasható. |
| ReadWrite |Csak akkor, ha az alkalmazás megfelelően kezeli írása ReadWrite gyorsítótárazott toopersistent adatlemezek szükség szerint gazdagép-gyorsítótár konfigurálása |

*Csak olvasható*  
Csak olvasható, a prémium szintű Storage-adatok gyorsítótárazása lemezek konfigurálásával olvasási kis késleltetésű eléréséhez, és nagyon magas IOPS olvasási és az átvitel lekérése az alkalmazáshoz. Ez a két okok miatt

1. Olvasás a gyorsítótárból, amelyek a Virtuálisgép-memória hello és a helyi SSD végre, és sokkal gyorsabb, mint olvasások hello adatlemez, amelyek a hello Azure blob storage-ból.  
2. Prémium szintű Storage nem tartoznak bele hello olvasási kiszolgálása a gyorsítótárból, hello lemez iops-érték és az átvitel felé. Az alkalmazás ezért képes tooachieve magasabb összes IOPS és átviteli sebességet.

*ReadWrite*  
Alapértelmezés szerint a hello OS lemezeken lehetnek, ReadWrite gyorsítótárazás engedélyezve van. Támogatja a gyorsítótárazást az adatokat, valamint a lemezek ReadWrite nemrég jelentek meg. Ha ReadWrite gyorsítótárazást használ, a megfelelő módon toowrite hello adatok gyorsítótár toopersistent lemezekből kell rendelkeznie. Például írása az SQL Server kezeli gyorsítótárazza toohello állandó tároló adatlemezek önállóan. ReadWrite gyorsítótár használata olyan alkalmazás, amely nem kezeli a tárolásakor hello szükséges adatok vezethet toodata adatvesztés, ha hello VM összeomlik.

Tegyük fel, ezek irányelvek tooSQL kiszolgáló alkalmazása hello következő tevékenységek végrehajtásával prémium szintű Storage fut

1. "ReadOnly" gyorsítótár konfigurálása prémium szintű storage lemezeken adatok-fájlokat tartalmazó.  
   a.  gyors hello alacsonyabb hello SQL Server lekérdezés gyorsítótárideje olvassa be az óta lapok sokkal gyorsabb lekért hello képest gyorsítótár toodirectly hello adatok lemezekről.  
   b.  Olvasási kiszolgálás a gyorsítótárból, azt jelenti, hogy van további átviteli sebesség érhető el prémium adatlemezek. SQL Server is használja a további átviteli további lapok és más műveletek, például a biztonsági mentés/visszaállítás felé, a batch-terhelések és index újraépíti.  
2. "None" konfigurálása prémium szintű storage gyorsítótárában lemezek üzemeltetési hello naplófájlokat.  
   a.  Naplófájlok elsősorban írási műveleteket műveletek rendelkezik. Ezért azokat nem tudják igénybe venni hello ReadOnly gyorsítótár.

## <a name="disk-striping"></a>Lemez csíkozást
Ha egy virtuális gép csatlakozik a több prémium szintű storage állandó lemezek esetében hello lemezek nagy méretű lehet csíkozott együtt tooaggregate az IOPs, az sávszélesség és a tárolási kapacitást.

A Windows együtt a tárolóhelyek toostripe lemezek is használhatók. A készletbe konfigurálnia kell egy oszlopot az egyes lemezek. Ellenkező esetben hello csíkozott kötetek általános teljesítménye is lehet kisebb, mint a várt forgalom hello lemezeken toouneven terjesztési miatt.

Fontos: A kiszolgáló-kezelő felhasználói felületén, állíthatók hello mentése csíkozott kötetek too8 oszlopok teljes száma. Több mint 8 lemez csatlakoztatása, használjon PowerShell toocreate hello kötet. PowerShell használatával beállíthatja az oszlopok száma hello egyenlő toohello lemezeinek száma miatt. Például, ha egy egyetlen paritásos készlet; 16 lemezek vannak Adja meg a 16 ilyen oszlop hello *numberofcolumns tulajdonsághoz* hello paramétere *New-VirtualDisk* PowerShell-parancsmagot.

Linux, hello MDADM segédprogram toostripe lemezek használhatók együtt. A részletes lépéseket Linux csíkozást lemezeinek tekintse meg a túl[szoftver RAID konfigurálása Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

*Paritásos mérete*  
A lemez csíkozást egy fontos konfigurációs érték hello paritásos méretét. hello paritásos méretét vagy a blokkméret nem hello legkisebb adattömb, alkalmazás csíkozott kötetek meg lehet oldani. hello paritásos mérete konfigurálnia hello típusát és az alkalmazás a kérelem mintát függ. Ha úgy dönt, hogy hello helytelen paritásos mérete, előfordulhat, tooIO hibás illesztés hibákat, ami az alkalmazás toodegraded teljesítményét.

Például ha az alkalmazás által generált IO kérelmet hello lemez paritásos mérete nagyobb, hello tárolórendszer írja azt között a paritásos egység határok több lemezen. Azt az időt tooaccess adatokat, ha egynél több paritásos egységek toocomplete hello kérelem között tooseek lesz. ilyen viselkedést összegző hatásának hello toosubstantial teljesítménycsökkenést eredményezhet. A hello ugyanakkor, ha hello IO kérelem mérete kisebb, mint paritásos mérete, és ha véletlenszerű ideiglenesek, hello IO kérelmek előfordulhat, hogy egyezzen a hello azonos lemez a szűk keresztmetszetet, és végső soron a hello IO teljesítmény terhelése.

Munkaterhelés hello típusától függően az alkalmazás fut, válassza ki a megfelelő paritásos méretét. A véletlenszerű kisméretű I/O kéréseket használja a paritásos mérete kisebb lesz. Mivel a nagy szekvenciális I/O kérelmek paritásos nagyobb méretű használja. Ismerje meg hello paritásos mérete javaslatok hello alkalmazás a prémium szintű Storage fog futni. Az SQL Server 64 KB-os OLTP-munkaterhelések és 256 KB-os adatok vámraktározási munkaterhelések paritásos méretének beállítása. Lásd: [teljesítmény az Azure virtuális gépeken futó SQL Server ajánlott eljárásai](../../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md#disks-guidance) további toolearn.

> [!NOTE]
> Akkor is paritásos együtt legfeljebb 32 prémium szintű storage DS több virtuális gép és 64 prémium szintű storage lemezek a GS több virtuális gép.
>
>

## <a name="multi-threading"></a>Többszálú
Azure van kialakítva, prémium szintű Storage platform toobe nagymértékben párhuzamos. Ezért a többszálas alkalmazások egyszálas alkalmazásnál jobb teljesítményt éri el. A többszálas alkalmazások felosztja a feladatai be több szálak között, és növeli a hatékonyságot terjesztése hello virtuális gép által a futtatása és a lemez erőforrások toohello maximális.

Például ha az alkalmazás egy processzormag két szálat használ a virtuális gép fut, hello CPU válthat hello két szállal tooachieve hatékonyságát. Egy szál a lemez IO toocomplete a vár, amíg hello CPU válthat toohello más szál. Ezzel a módszerrel több mint egy szál volna két szállal érhető el. Ha hello VM több processzormag, továbbá csökkenti futási időt mivel minden egyes core feladatokat is végre párhuzamosan.

Nem fogja tudni toochange hello módon vásárolt alkalmazás valósít meg egyetlen szálkezelési vagy többszálas. Például SQL-kiszolgáló nem képes Többprocesszoros és Többmagos kezelésére. SQL Server azonban úgy dönt, milyen feltételek mellett azt fogja használni, egy vagy több szál tooprocess lekérdezést. Ez lekérdezéseinek futtatásához, és használatával többszálas indexeket építenek. Egy lekérdezést, amely magában foglalja a nagy táblák illesztése és az adatok rendezése toohello felhasználói visszatérése előtt, az SQL Server valószínűleg több szál fogja használni. A felhasználó azonban nem tudja ellenőrizni, hogy az SQL Server használatával egyetlen szálon vagy több szál lekérdezést hajtanak.

Konfigurációs beállítások, hogy a többszálas vagy párhuzamos tooinfluence módosíthatja az alkalmazások feldolgozását. Például az SQL Server esetén is hello legnagyobb mértékben az párhuzamossági konfigurációs. Ez a beállítás nevű MAXDOP, lehetővé teszi tooconfigure hello processzorok maximális száma az SQL Server párhuzamos feldolgozása közben használható. Egyéni lekérdezések vagy indexművelet MAXDOP konfigurálható. Ez akkor hasznos, ha toobalance erőforrást a rendszer kívánt alkalmazáshoz kritikus teljesítmény.

Például, hogy az alkalmazás SQL Server használatával nagy lekérdezés és hello az indexművelet végrehajtása történik ugyanannyi időt vesz igénybe. Tételezzük fel, hogy tényleg hello index művelet toobe további képest performant toohello nagy lekérdezés. Ebben az esetben hello index művelet toobe hello hello lekérdezés MAXDOP értéke nagyobb MAXDOP értékét állíthatja be. Így az SQL Server rendelkezik, amelyek kihasználhatják a hello index művelet képest toohello processzorok száma azt is mozgósíthatnak a processzorok több toohello nagy lekérdezés. Ne feledje, hogy nem szabályozhatja hello a szálak számát, az SQL Server minden művelethez fogja használni. Szabályozhatja az éppen kijelölt processzorok maximális száma hello többszálas.

További információ [szintű párhuzamosság fok](https://technet.microsoft.com/library/ms188611.aspx) az SQL Server kiszolgálón. Ismerje meg ezek a beállítások, amelyek befolyásolják az alkalmazások és konfigurációk toooptimize teljesítményét a többszálas.

## <a name="queue-depth"></a>Várólistamélység
várólistamélység vagy a várólista hossza hello, vagy várólista-méret függőben lévő I/O kérelmek száma hello hello rendszerben. várólistamélység hello értéke határozza meg, hány I/O műveletek az alkalmazás is sor fel, melyik hello tárolólemezek fog dolgoz fel. Minden hello három alkalmazás teljesítménymutatók, amiről beszéltünk, ez a cikk ti, IOPS, az átviteli sebesség és a késleltetés az általa érintett.

Feldolgozási sor mélysége és a többszálas szorosan kapcsolódó. hello várólistamélység érték azt jelzi, hogy mennyit többszálas lehet hello alkalmazás érhető el. Ha hello várólistamélység túl nagy, alkalmazás végrehajtható további műveletek egyidejűleg, ez azt jelenti, több többszálas. Ha hello várólistamélység kicsi, annak ellenére, hogy az alkalmazás többszálas, nem lesz elég kéréseket a párhuzamos végrehajtás egymás mellett.

Általában ki hello forgalomban alkalmazások nem teszik lehetővé toochange hello várólistamélység, mert ha beállított helytelenül jó-nál több kárt fog tenni. Alkalmazások hello jobb oldali értéktengely várólista mélysége tooget hello optimális teljesítményt állítja be. Azonban hogy van-e fontos toounderstand a fogalom, teljesítménnyel kapcsolatos problémák elhárítása az alkalmazás. Várólistamélység hello hatásait azt is láthatja, ha összehasonlítási eszközök futtatásához a rendszerre.

Egyes alkalmazások beállítások tooinfluence hello Várólistamélységének meghatározásához adjon meg. Hello MAXDOP (legnagyobb mértékben párhuzamossági) beállítást az SQL Server például az előző szakaszban ismertetett. MAXDOP egy módon tooinfluence várólistamélység és többszálas, bár nem módosíthatók közvetlenül a hello várólistamélység értéket az SQL Server.

*Magas várólistamélység*  
Magas várólistamélység sorok hello további műveletek fel. hello lemez tudja hello következő kérés időben a várólistán. Következésképpen hello lemez időben műveletek ütemezése és az optimális sorrendben dolgozza fel őket. Mivel hello alkalmazás által küldött kérelmek toohello több lemez, hello lemez további párhuzamos IOs tud feldolgozni. Végső soron a hello alkalmazás képes tooachieve lesz magasabb iops-érték. Alkalmazás feldolgozása további kérelmeket, mert hello hello alkalmazás teljes átviteli sebesség is növeli.

Általában az alkalmazás érhető el maximális átviteli sebesség a 8-16 + kiugró IO-csatlakoztatott lemez. Ha egy várólistamélység, alkalmazás nem elég IOs toohello rendszer küldését, és egy adott időszakban kisebb mennyiségű fogja feldolgozni. Ez azt jelenti kevesebb átviteli sebességet.

Például az SQL Server beállítás hello MAXDOP érték, egy lekérdezés túl "4" arról értesíti az SQL Server-toofour magok tooexecute hello lekérdezés mentése használhat. SQL Server ajánlott várólista mélységének értékét és hello hány hello lekérdezés-végrehajtás magos határozza meg.

*Optimális Várólistamélységének*  
Nagyon magas várólista mélységének értékét is rendelkezik a hátrányai is. Ha várólista mélység értéke túl magas, hello alkalmazás megpróbál toodrive nagyon magas iops-érték. Ha alkalmazás állandó lemezek, amelyek elegendő kiosztott IOPS van, ez negatívan befolyásoló kódokat is alkalmazáskéséseknek. A következő képlet mutatja az iops-érték, a késés és a várólistamélység hello kapcsolatát.  
    ![](media/storage-premium-storage-performance/image6.png)

Ne konfigurálja a Várólistamélységet tooany nagy értékű, de tooan optimális érték, amely elegendő IOPS hello alkalmazás által biztosított késések befolyásolása nélkül. Például akkor, ha hello alkalmazás késés toobe 1 ezredmásodperces, hello várólistamélység szükség tooachieve 5000 IOPS-érték, QD = 5000 x 0,001 = 5.

*Várólistamélység csíkozott kötetek*  
A csíkozott kötetek karbantartása elég magas várólistamélység úgy, hogy minden lemezhez tartozik egy maximális várólistamélység külön-külön. Vegye figyelembe például olyan alkalmazás, amely a leküldéses értesítések egy várólistamélység 2 és 4 lemez található hello paritásos. hello két IO kérelmek tootwo lemezek kerül, és két lemez fennmaradó tétlen. Hello várólistamélység ezért konfigurálni úgy, hogy az összes hello lemez lehet foglalt. Az alábbi képlet mutatja, hogyan toodetermine hello várólistamélység csíkozott köteteken tárolni.  
    ![](media/storage-premium-storage-performance/image7.png)

## <a name="throttling"></a>Szabályozás
Prémium szintű Storage Azure rendelkezések megadott iops-érték és átviteli száma attól függően, hogy hello Virtuálisgép-méretek és a lemez mérete választja. Az alkalmazás megpróbál toodrive IOPS vagy átviteli milyen hello VM vagy a lemez kezelni tud, működés felső korlátjának fent, amikor prémium szintű Storage szabályozás fogja azt. Ez akkor jelentkezik, az alkalmazás teljesítménye hello formájában. Ez is nagyobb késleltetéssel járhat jelenti azt, csökkentheti a teljesítményt, illetve nem csökkentheti iops-érték. Ha prémium szintű Storage nem szabályozás, az alkalmazás teljesen feladatátadáshoz alapján meghaladó mi erőforrásaihoz képesek elérése érdekében. Igen, kapcsolatos tooavoid teljesítményproblémák miatt toothrottling, mindig rendelkezés elegendő erőforrással az alkalmazáshoz. Vegye figyelembe, mi ismertettük hello Virtuálisgép-méretek és a lemez mérete a fenti szakaszban. Hello legjobb módja toofigure teljesítménymérésre milyen erőforrásokat kell toohost az alkalmazást.

## <a name="benchmarking"></a>Teljesítménymérésre
Teljesítménymérésre az szimulálva az alkalmazás a különböző terhelésekhez és a mérési hello alkalmazások teljesítménye az egyes munkaterhelésekhez tartozó hello folyamat. Egy korábbi szakaszában leírt hello lépéseket követve, összegyűjtötte hello alkalmazás teljesítménykövetelményekhez. Teljesítménymérésre hello virtuális gépeken hello alkalmazást futtató eszközök futtatásával határozza meg az alkalmazás érhető el prémium szintű Storage hello teljesítményszintet. Ez a szakasz azt biztosít egy szabványos DS14 virtuális Gépet, a prémium szintű Azure Storage lemezek kiépített teljesítménymérésre példái.

Rendelkezik használtuk közös összehasonlítási eszközök Iometer és FIO, a Windows és Linux kulcsattribútumokkal. Ezek az eszközök elindítanak egy éleshez hasonlító munkaterhelés, majd a mértéket hello rendszerteljesítmény szimulálva több szál. Hello eszközökkel is konfigurálhatja például blokk méretét és a várólista mélység, amely általában nem módosítható az alkalmazás paramétereit. Ez lehetővé teszi további rugalmasságot toodrive hello maximális teljesítmény nagy méretű virtuális gép különböző típusú alkalmazások és szolgáltatások a premium lemezek üzembe. minden egyes összehasonlítási eszköz olvashat toolearn [Iometer](http://www.iometer.org/) és [FIO](http://freecode.com/projects/fio).

toofollow hello példák az alábbi, hozzon létre egy szabványos DS14 virtuális Gépet, és csatolja a 11 prémium szintű Storage lemezek toohello virtuális gép. Hello 11 lemezek-állomás gyorsítótárazását, "None" 10 lemezek konfigurálásához, valamint azokat a NoCacheWrites nevű kötet paritásos. Állomás gyorsítótárazását "ReadOnly", a többi lemezen hello konfigurálása, és hozzon létre egy kötetet CacheReads meghívásra ezt a lemezt. A telepítő lehetősége lesz képes toosee hello maximális olvasási és írási teljesítményt szabványos DS14 virtuális gép alapján. A DS14 virtuális gép létrehozása a premium lemezek kapcsolatos részletes lépéseket lásd túl[létrehozása és használata a prémium szintű Storage fiókot a virtuális gép adatlemezt](../storage-premium-storage.md).

*Hello gyorsítótár előkészítése*  
csak olvasható-állomás gyorsítótárazását hello lemezzel lesz képes toogive hello lemez időkorlátjánál magasabb iops-érték. tooget a maximális teljesítmény gyorsítótárából olvasta be hello gazdagépet, először meg ezt a lemezt hello gyorsítótárát kezeléséből kell. Ez biztosítja, hogy mely összehasonlítási eszköz találkozik CacheReads köteten olvasási IOs hello ténylegesen találatok hello gyorsítótárat, és nem hello lemez közvetlenül. hello gyorsítótár találatok eredményez további IOPS hello a gyorsítótár engedélyezve van a lemezen.

> **Fontos:**  
> Meg kell kezeléséből hello gyorsítótár futtatása teljesítménymérésre, minden alkalommal, amikor a virtuális gép újraindítása előtt.
>
>

#### <a name="iometer"></a>Iometer
[Töltse le a hello Iometer eszköz](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) a virtuális gép hello.

*Fájl tesztelése*  
Iometer teszt teljesítménymérésre hello futtató rendszer hello köteten tárolt teszt fájlt használ. Az olvasási műveletek meghajtók, és ez az írási műveletek tesztelési toomeasure hello lemezen iops-érték és az átvitel. Iometer hoz létre a fájl tesztelése, ha nincs megadva egy. Hozzon létre egy 200 GB-os tesztfájl iobw.tst nevű hello CacheReads és NoCacheWrites köteteken.

*Access specifikációi*  
hello specifikációk, kérelem I/O méret, olvasási/írási %, % véletlenszerű/szekvenciális konfigurált hello "Hozzáférési specifikációk" lapján Iometer. Hozzon létre egy hozzáférési leírása az alábbiakban hello forgatókönyvek. Hello access specifikációi létrehozása és a "Mentés" megfelelő nevet hasonlóan – RandomWrites\_8 KB-os, RandomReads\_8 KB-os. Válassza ki a megfelelő specification hello hello tesztkörnyezet futtatásakor.

Maximális IOPS írási forgatókönyv vonatkozó hozzáférési példa az alábbi  
    ![](media/storage-premium-storage-performance/image8.png)

*Maximális IOPS vizsgálati előírások*  
toodemonstrate maximális IOPs, használja a kisebb kérés méretét. Használja a 8 KB-os kérés méretét, és hozzon létre specifikációk véletlenszerű írási műveletek és olvasási műveletek.

| Hozzáférési leírása | Kérelem mérete | Véletlenszerű % | Olvasási % |
| --- | --- | --- | --- |
| RandomWrites\_8 KB-os |8 KB-OS |100 |0 |
| RandomReads\_8 KB-os |8 KB-OS |100 |100 |

*Maximális átviteli sebesség vizsgálati előírások*  
toodemonstrate használata a kérelem mérete nagyobb maximális átviteli sebességet. Használja a 64 KB-os kérés méretét, és hozzon létre specifikációk véletlenszerű írása és olvasása.

| Hozzáférési leírása | Kérelem mérete | Véletlenszerű % | Olvasási % |
| --- | --- | --- | --- |
| RandomWrites\_64 K |64K |100 |0 |
| RandomReads\_64 K |64K |100 |100 |

*Hello Iometer teszt futtatása*  
Hello lépésekkel telepítheti be gyorsítótár toowarm alatt

1. Hozzon létre két access specifikációi az alábbi értékek

   | Név | Kérelem mérete | Véletlenszerű % | Olvasási % |
   | --- | --- | --- | --- |
   | RandomWrites\_1 MB |1MB |100 |0 |
   | RandomReads\_1 MB |1MB |100 |100 |
2. A gyorsítótár lemez a következő paraméterekkel inicializálása hello Iometer teszt futtatása. Használjon három munkaszál hello célkötet és egy várólistamélység 128. Állítsa be hello hello teszt too2hrs hello "Tesztelése Setup" lap "Futásidejű" időtartama.

   | Forgatókönyv | Célkötet | Név | Időtartam |
   | --- | --- | --- | --- |
   | Gyorsítótár lemez inicializálása |CacheReads |RandomWrites\_1 MB |2hrs |
3. A gyorsítótár lemez a következő paraméterekkel előkészítése hello Iometer teszt futtatása. Használjon három munkaszál hello célkötet és egy várólistamélység 128. Állítsa be hello hello teszt too2hrs hello "Tesztelése Setup" lap "Futásidejű" időtartama.

   | Forgatókönyv | Célkötet | Név | Időtartam |
   | --- | --- | --- | --- |
   | Meleg gyorsítótár lemezre mentése |CacheReads |RandomReads\_1 MB |2hrs |

Ha gyorsítótár lemez tárolóhelyspecifikus, lépjen az alább felsorolt hello Tesztelési forgatókönyvek. toorun hello Iometer teszteket, használja a legalább három worker threads **minden** céloz kötet. Az egyes munkavégző szál hello célkötet kiválasztása, várólistamélység beállítása, és választanak mentett hello vizsgálati előírások, az alábbi toorun hello megfelelő tesztkörnyezet hello táblázatban látható módon. Ezek a tesztek futtatásakor hello táblázatban is látható kívánt eredmény elérése érdekében az iops-érték és a teljesítményt. Minden esetben egy kisméretű I/O mérete 8 KB-os és egy magas várólista mélysége 128 szolgál.

| Tesztkörnyezet | Célkötet | Név | eredménye |
| --- | --- | --- | --- |
| Legfeljebb Olvasási IOPS |CacheReads |RandomWrites\_8 KB-os |50 000 IOPS |
| Legfeljebb IOPS írása |NoCacheWrites |RandomReads\_8 KB-os |64 000 IOPS |
| Legfeljebb Kombinált IOPS |CacheReads |RandomWrites\_8 KB-os |100 000 IOPS |
| NoCacheWrites |RandomReads\_8 KB-os | &nbsp; | &nbsp; |
| Legfeljebb Olvassa el a MB/s |CacheReads |RandomWrites\_64 K |524 bájtnyi MB/s |
| Legfeljebb Írás a MB/s |NoCacheWrites |RandomReads\_64 K |524 bájtnyi MB/s |
| Kombinált MB/s |CacheReads |RandomWrites\_64 K |1000 MB/s |
| NoCacheWrites |RandomReads\_64 K | &nbsp; | &nbsp; |

Az alábbiakban hello pillanatképei Iometer teszteredmények kombinált IOPS és átviteli forgatókönyvek.

*Egyesített írási és olvasási maximális iops-érték*  
![](media/storage-premium-storage-performance/image9.png)

*Egyesített írási és olvasási maximális átviteli sebesség*  
![](media/storage-premium-storage-performance/image10.png)

### <a name="fio"></a>FIO
FIO egy népszerű eszköz toobenchmark tárolót a Linux virtuális gépek hello. Hello rugalmasságot tooselect IO mérete eltér, soros vagy véletlenszerű olvasási és írási rendelkezik. Az indít munkavégző szál, vagy folyamatok tooperform hello megadott i/o-műveletek. Megadhatja, hogy minden egyes munkavégző szál feladat fájlok használatával kell végrehajtania i/o-műveletek hello típusú. Létrehoztunk egy feladat fájl egy forgatókönyvet mutatja be az alábbi hello példák. A feladat fájlok toobenchmark különböző munkaterheléseinek prémium szintű Storage hello specifikációk módosíthatja. Standard DS 14 virtuális gép futó használjuk hello példákban **Ubuntu**. Azonos telepítő hello hello elején leírt használata hello [szakasz teljesítménymérésre](#Benchmarking) és meleg mentése hello gyorsítótár hello teljesítménymérésre tesztek futtatása előtt.

Mielőtt elkezdené, [FIO letöltése](https://github.com/axboe/fio) , és telepíteni a virtuális gép.

Futtassa a parancsot az Ubuntu, a következő hello

```
apt-get install fio
```

Vezetői olvasási műveletek hello lemezeken használjuk az írási műveletek vezetéséhez négy munkaszál és négy munkaszál. hello írási munkavállalók fog kell befolyásoló tényezők forgalom hello "nocache lehet" köteten, a gyorsítótár beállítása túl 10 lemezekkel rendelkező "None". hello olvasási munkavállalók fog kell befolyásoló tényezők forgalom hello "readcache" köteten, amelynek 1 gyorsítótár-készlettel túl "ReadOnly".

*Maximális írási IOPS*  
Hozzon létre hello feladat fájlt az alábbi előírások tooget írási maximális Gyakoriságnál. Nevezze el a "fiowrite.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Megjegyzés: hello hajtsa végre a korábbi szakaszokban ismertetett hello tervezési irányelveket összhangban legyenek főbb tényezőket. Ezen specifikációk alapvető toodrive maximális iops-ről,  

* Magas várólistamélység 256.  
* Kis blokkméret 8 KB-os.  
* Több szál véletlenszerű írási műveleteket végez.

Futtassa a következő parancs tookick ki FIO tesztelése 30 másodpercig hello hello  

```
sudo fio --runtime 30 fiowrite.ini
```

Hello teszt végrehajtása közben fogja tudni toosee hello száma hoz létre, IOPS hello virtuális gép és a Premium lemezek írási. Ahogy az alábbi minta hello, hello DS14 VM elkötelezett a maximális IOPS legfeljebb 50 000 IOPS írási.  
    ![](media/storage-premium-storage-performance/image11.png)

*Maximális olvasási IOPS*  
Hozzon létre hello feladat fájlt az alábbi előírások tooget olvasási maximális Gyakoriságnál. Nevezze el a "fioread.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Megjegyzés: hello hajtsa végre a korábbi szakaszokban ismertetett hello tervezési irányelveket összhangban legyenek főbb tényezőket. Ezen specifikációk alapvető toodrive maximális iops-ről,

* Magas várólistamélység 256.  
* Kis blokkméret 8 KB-os.  
* Több szál véletlenszerű írási műveleteket végez.

Futtassa a következő parancs tookick ki FIO tesztelése 30 másodpercig hello hello

```
sudo fio --runtime 30 fioread.ini
```

Hello teszt végrehajtása közben olvasási IOPS hello méretű képes toosee hello száma is, majd a Premium lemezek hoz létre. Ahogy az alábbi minta hello, hello DS14 VM elkötelezett több mint 64 000 olvasási iops-érték. Ez a hello lemez és a hello gyorsítótár teljesítménye.  
    ![](media/storage-premium-storage-performance/image12.png)

*Maximális olvasási és írási IOPS*  
Hozzon létre hello feladat fájlt az alábbi előírások tooget maximális olvasási és írási IOPS együtt. Nevezze el a "fioreadwrite.ini".

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Megjegyzés: hello hajtsa végre a korábbi szakaszokban ismertetett hello tervezési irányelveket összhangban legyenek főbb tényezőket. Ezen specifikációk alapvető toodrive maximális iops-ről,

* Magas várólista mélysége 128.  
* Kis blokkméret 4 KB-os.  
* Véletlenszerű végrehajtása több szál beolvassa és.

Futtassa a következő parancs tookick ki FIO tesztelése 30 másodpercig hello hello

```
sudo fio --runtime 30 fioreadwrite.ini
```

Hello teszt végrehajtása közben képes toosee hello számú kombinált olvasható, és írási IOPS hello a virtuális gép és a Premium lemezek hoz létre. Ahogy az alábbi minta hello, hello DS14 VM elkötelezett több mint 100 000 kombinált olvasási és írási iops-érték. Ez a hello lemez és a hello gyorsítótár teljesítménye.  
    ![](media/storage-premium-storage-performance/image13.png)

*Maximális átviteli sebesség kombinált*  
maximális tooget hello kombinált olvasási és írási teljesítményt, a nagyobb blokkméret és nagy várólistamélység használata több szál olvasási és írási műveletek végrehajtása. Egy 64 KB-os blokkméretet és 128 várólistamélység is használhatja.

## <a name="next-steps"></a>Következő lépések
Ismerje meg a prémium szintű Azure Storage:

* [Premium Storage: Nagy teljesítményű tárolási szolgáltatás Azure-beli virtuális gépek számítási feladataihoz](../storage-premium-storage.md)  

SQL Server-felhasználók számára olvassa el az SQL Server teljesítményét gyakorlati tanácsok a cikkek:

* [Teljesítmény ajánlott eljárások az SQL Server Azure virtuális gépeken](../../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md)
* [Prémium szintű Storage nyújt az SQL Server Azure virtuális gép a lehető legjobb teljesítmény](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx)
