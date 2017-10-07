---
title: "aaaAzure tárolási teljesítményének és méretezhetőségének ellenőrzőlista |} Microsoft Docs"
description: "Egy ellenőrzőlista használható az Azure Storage performant-alkalmazások fejlesztésével bevált gyakorlatát."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 959d831b-a4fd-4634-a646-0d2c0c462ef8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: c2970c055d460070288d1810e4a77a7f056a4137
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-storage-performance-and-scalability-checklist"></a>A Microsoft Azure Storage teljesítmény- és méretezhetőségi ellenőrzőlistája
## <a name="overview"></a>Áttekintés
Óta hello Microsoft Azure Storage szolgáltatás hello verziójában Microsoft performant módon ezen szolgáltatások használatára vonatkozó bevált gyakorlatok számos fejlesztett ki, és ez a cikk szolgál tooconsolidate hello egyik legfontosabb ellenőrzőlista stílusú listává. hello Ez a cikk célja toohelp alkalmazásfejlesztők annak ellenőrzése, hogy azok bevált gyakorlat az Azure Storage és toohelp-érdemes megfontolni a bevezetése egyéb bevált gyakorlatok azonosítása őket. Ez a cikk nem kísérli meg toocover minden lehetséges teljesítményének és méretezhetőségének optimalizálás – a művelet hatása a kis- és tágabb értelemben véve nem alkalmazható nem tartalmazza. az alkalmazás viselkedését hello toohello mértékben előre jelezhető tervezés közben ezeket a következőket kell figyelembe venni a korai hasznos tookeep tooavoid terveket, amely a teljesítménybeli problémákat.  

Minden Azure Storage használatával alkalmazásfejlesztő kell érvénybe hello idő tooread ebben a cikkben, és ellenőrizze, hogy saját alkalmazás egyes hello bizonyítása az alábbi eljárásokat a következő.  

## <a name="checklist"></a>Feladatlista
Ez a cikk bizonyítása hello eljárások azokat a csoportokat a következő hello rendezi. Bevált gyakorlatok alkalmazható:  

* Minden Azure Storage szolgáltatás (BLOB, táblák, üzenetsorok és fájlok)
* Blobok
* Táblák
* Üzenetsorok  

| Kész | Terület | Kategória | Kérdés |
| --- | --- | --- | --- |
| &nbsp; | All Services (Minden szolgáltatás) |Méretezhetőségi célok |[Az alkalmazás, amelynek célja tooavoid közeledik hello méretezhetőségi célok?](#subheading1) |
| &nbsp; | All Services (Minden szolgáltatás) |Méretezhetőségi célok |[Az jobban az elnevezési egyezmény tervezett tooenable terheléselosztás?](#subheading47) |
| &nbsp; | All Services (Minden szolgáltatás) |Hálózat |[Rendelkeznek-e ügyféloldali ügyféleszközök elég nagy sávszélességet és alacsony késési igényű tooachieve hello teljesítményt?](#subheading2) |
| &nbsp; | All Services (Minden szolgáltatás) |Hálózat |[Ügyféloldali ügyféleszközök rendelkeznek elég kiváló minőségű hivatkozást?](#subheading3) |
| &nbsp; | All Services (Minden szolgáltatás) |Hálózat |["Közelében" hello storage-fiókban található hello ügyfélalkalmazás?](#subheading4) |
| &nbsp; | All Services (Minden szolgáltatás) |Tartalomterjesztés |[Használja a CDN a tartalmak terjesztéséhez?](#subheading5) |
| &nbsp; | All Services (Minden szolgáltatás) |Közvetlen ügyfélhozzáférés |[Használ SAS, valamint a CORS tooallow közvetlen hozzáférést toostorage proxy helyett?](#subheading6) |
| &nbsp; | All Services (Minden szolgáltatás) |Gyorsítótárazás |[A gyorsítótárazási alkalmazásadatok ismételten használt és módosítások ritkán van?](#subheading7) |
| &nbsp; | All Services (Minden szolgáltatás) |Gyorsítótárazás |[Az alkalmazás kötegelés van a frissítések (ügyféloldali gyorsítótárazás őket, és majd töltse fel a nagyobb készletek)?](#subheading8) |
| &nbsp; | All Services (Minden szolgáltatás) |.NET-konfiguráció |[Rendelkezik-e konfigurálva az ügyfél toouse egyidejű kapcsolat elegendő számú?](#subheading9) |
| &nbsp; | All Services (Minden szolgáltatás) |.NET-konfiguráció |[Állított .NET toouse szálak elegendő számú?](#subheading10) |
| &nbsp; | All Services (Minden szolgáltatás) |.NET-konfiguráció |[.NET 4.5-ös verziójának használatával, vagy később, amely javult a szemétgyűjtés?](#subheading11) |
| &nbsp; | All Services (Minden szolgáltatás) |Párhuzamos végrehajtás |[Biztosították, hogy párhuzamossági korlátozódik megfelelően, hogy az ügyfél képességek vagy a hello méretezhetőségi célok nem túlterhelés?](#subheading12) |
| &nbsp; | All Services (Minden szolgáltatás) |Eszközök |[Rendszer Microsoft hello legújabb verzióját használja a megadott ügyfél-kódtárak és eszközök?](#subheading13) |
| &nbsp; | All Services (Minden szolgáltatás) |Újrapróbálkozások |[Azok az exponenciális leállítási használatával újra hibák és időtúllépéseket okoz szabályozás házirend?](#subheading14) |
| &nbsp; | All Services (Minden szolgáltatás) |Újrapróbálkozások |[Az alkalmazás elkerülő újrapróbálkozások van – Újrapróbálkozást lehetővé nem tevő hibákat?](#subheading15) |
| &nbsp; | Blobok |Méretezhetőségi célok |[Rendelkezik egy adott objektum egyidejűleg hozzáférő ügyfelek nagy számú?](#subheading46) |
| &nbsp; | Blobok |Méretezhetőségi célok |[Az alkalmazás hello sávszélességet vagy a műveletek a méretezhetőség cél egyetlen BLOB tartózkodik?](#subheading16) |
| &nbsp; | Blobok |Blobok másolása |[Másolási blobok vannak hatékony módon?](#subheading17) |
| &nbsp; | Blobok |Blobok másolása |[AzCopy használunk a blobok tömeges másolatot?](#subheading18) |
| &nbsp; | Blobok |Blobok másolása |[Azure Import/Export tootransfer nagyon nagy adatkötetek használ?](#subheading19) |
| &nbsp; | Blobok |Metaadatok |[Akkor tárolja a gyakran használt BLOB metaadatait a metaadatok?](#subheading20) |
| &nbsp; | Blobok |Gyors feltöltése |[Mielőtt gyorsan tooupload egy blob, akkor feltölteni párhuzamos blokkok?](#subheading21) |
| &nbsp; | Blobok |Gyors feltöltése |[Mielőtt tooupload sok blobok gyorsan, akkor feltölteni párhuzamosan blobok?](#subheading22) |
| &nbsp; | Blobok |Javítsa ki a Blob típushoz |[Használ lapblobokat vagy adott esetben a blokkblobok?](#subheading23) |
| &nbsp; | Táblák |Méretezhetőségi célok |[Vannak, megközelíti hello méretezhetőségi célok másodpercenként entitások?](#subheading24) |
| &nbsp; | Táblák |Konfiguráció |[Használ JSON-ban a tábla kérelmek?](#subheading25) |
| &nbsp; | Táblák |Konfiguráció |[Kikapcsolta-e kis kérelmek teljesítésének tooimprove hello Nagle?](#subheading26) |
| &nbsp; | Táblák |Táblák és -partíciók |[Rendelkezik meg megfelelően particionálva az adatokat?](#subheading27) |
| &nbsp; | Táblák |Működés közbeni partíciók |[Fűzi, és csak illesztenie minták elkerülésére?](#subheading28) |
| &nbsp; | Táblák |Működés közbeni partíciók |[A Beszúrás/frissítés sok partíciót között van elosztva?](#subheading29) |
| &nbsp; | Táblák |Lekérdezés hatóköre |[Rendelkezik a séma tooallow a legtöbb esetben és tábla lekérdezések toobe használja takarékosan használ pont lekérdezések toobe tervezett?](#subheading30) |
| &nbsp; | Táblák |Lekérdezés sűrűség |[A lekérdezések általában csak vizsgálat, és térjen vissza a sorok, amelyekre az alkalmazás fogja használni?](#subheading31) |
| &nbsp; | Táblák |Korlátozza az adatokat adott vissza |[Nem szükséges szűrési tooavoid adatszolgáltató entitások használ?](#subheading32) |
| &nbsp; | Táblák |Korlátozza az adatokat adott vissza |[Nem szükséges tulajdonságok visszaadó leképezése tooavoid használ?](#subheading33) |
| &nbsp; | Táblák |Denormalization |[Rendelkezik denormalizált az adatok úgy, hogy ne hatékony lekérdezések és több olvasási kéréseket tooget adatok közben?](#subheading34) |
| &nbsp; | Táblák |Insert/Update/Delete |[Tranzakciós kell toobe vagy elvégezhető hello azonos kérelmek kötegelés tooreduce üzenetváltások idő?](#subheading35) |
| &nbsp; | Táblák |Insert/Update/Delete |[Vannak, szükségtelenné téve az entitás csak toodetermine beolvasása e toocall beszúrásához vagy frissítéséhez?](#subheading36) |
| &nbsp; | Táblák |Insert/Update/Delete |[Tekintette tárolja azokat a gyakran letöltött adatok együtt egyetlen entitás több entitás helyett tulajdonságként?](#subheading37) |
| &nbsp; | Táblák |Insert/Update/Delete |[Mindig veszi együtt, és kötegekben (pl. idő adatsor) csak írható entitások akkor tekintette a blobs használata helyett táblák?](#subheading38) |
| &nbsp; | Üzenetsorok |Méretezhetőségi célok |[Üzenetek / másodperc vonatkoznak, megközelíti hello méretezhetőségi célok?](#subheading39) |
| &nbsp; | Üzenetsorok |Konfiguráció |[Kikapcsolta-e kis kérelmek teljesítésének tooimprove hello Nagle?](#subheading40) |
| &nbsp; | Üzenetsorok |Üzenet mérete |[Azok az üzenetek kompakt tooimprove hello teljesítmény hello várólista?](#subheading41) |
| &nbsp; | Üzenetsorok |Tömeges beolvasása |[Egy "Get" művelettel több üzenetek fogadása?](#subheading42) |
| &nbsp; | Üzenetsorok |Lekérdezési gyakorisága |[Gyakran elegendő az alkalmazás késés érzékelt tooreduce hello lekérdezési?](#subheading43) |
| &nbsp; | Üzenetsorok |Üzenet frissítése |[Használ UpdateMessage toostore folyamatban lévő üzenetek feldolgozása kerülése tooreprocess teljes üdvözlőüzenetére hiba esetén?](#subheading44) |
| &nbsp; | Üzenetsorok |Architektúra |[Használja a várólisták toomake a teljes alkalmazás több méretezhető lekapcsolva hosszan futó munkaterhelések hello kritikus elérési út és a skála majd egymástól függetlenül?](#subheading45) |

## <a name="allservices"></a>Minden szolgáltatás
Ez a rész felsorolja a bevált gyakorlat, amelyek megfelelő toohello használata hello Azure Storage szolgáltatás (BLOB, táblák, üzenetsorok vagy fájlok).  

### <a name="subheading1"></a>Méretezhetőségi célok
Hello Azure Storage szolgáltatás mindegyikén méretezhetőségi célok kapacitás (GB), a tranzakciók sebességét és a sávszélesség. Ha az alkalmazás megközelíti vagy meghaladja a bármely hello méretezhetőségi célok, észlelhetnek megnövekedett tranzakció késések vagy a sávszélesség-szabályozás. Ha egy társzolgáltatás azelőtt gyorsítja fel, az alkalmazás, hello szolgáltatás megkezdi az tooreturn "503-as kiszolgáló elfoglalt" vagy "500 művelet időtúllépése" hibakódok néhány storage-tranzakció. Ez a szakasz ismerteti mind hello általános megközelítés toodealing méretezhetőségi célok és a sávszélesség méretezhetőségi célok különösen. A későbbi fejezetek, amely az egyéni tárolószolgáltatások kezelését ismertetik a méretezhetőségi célok hello környezetben, hogy adott szolgáltatás:  

* [A BLOB sávszélesség és a kérelmek / másodperc](#subheading16)
* [Táblaentitásokat / másodperc](#subheading24)
* [Várólista-üzenetek / másodperc](#subheading39)  

#### <a name="sub1bandwidth"></a>Sávszélesség méretezhetőség cél minden szolgáltatáshoz
Hello írásának időpontjában, hello sávszélesség hello USA egy georedundáns tárolás (GRS) fiók célpontjai 10 Gigabit / másodperc (Gbps) – érkező (adatok küldése toohello tárfiók) és a kimenő forgalom (hello tárfiókból küldött adatok) 20 GB/s. A helyileg redundáns tárolás (LRS) fiók hello korlátok magasabbak – 20 GB/s a bemenő és kimenő forgalom esetében 30 GB/s.  Nemzetközi sávszélességkorlátok alacsonyabb lehet, és megtalálható a [méretezhetőségi célok lap](http://msdn.microsoft.com/library/azure/dn249410.aspx).  Hello redundancia tárolásáról további információkért lásd: hello hivatkozások [hasznos források](#sub1useful) alatt.  

#### <a name="what-toodo-when-approaching-a-scalability-target"></a>Milyen toodo, amikor eléri a méretezhetőség cél
Ha az alkalmazás hello méretezhetőségi célok egyetlen tárfiók közeledik, fontolja meg bevezetése hello a következő módszerek egyikét:  

* Vizsgálja felül az alkalmazás tooapproach okozó hello munkaterhelés, vagy túlteljesíti hello méretezhetőség cél. Is tervez, másképp kisebb sávszélességet vagy a kapacitás, illetve kevesebb tranzakciók toouse?
* Ha egy alkalmazás haladhatja meg a hello méretezhetőségi célok egyikét, érdemes létrehozni storage-fiókok és a partíció az alkalmazásadatok ezeket több tárfiókok között. Ha ezt a mintát használja, akkor meg arról, hogy toodesign az alkalmazás, hogy a jövőbeni terheléselosztás hello további tárfiókokat adhat hozzá. Írásának időpontjában az egyes Azure-előfizetések legfeljebb too100 tárfiókok tartalmazhat.  Storage-fiókok is a használati tárolt adatokat, a végrehajtott tranzakciók vagy az átvitt adatok tekintetében eltérő költség nélkül.
* Ha az alkalmazás találatok hello sávszélesség célozza, érdemes lehet hello tooreduce hello szükséges sávszélesség toosend hello adatok toohello tárolási ügyfélszolgáltatás az adatok tömörítése.  Vegye figyelembe, hogy a sávszélességet, és hálózati teljesítmény javításához, azt is beállíthatja, hogy néhány negatív hatások.  Azt ki kell értékelnie hello teljesítményre gyakorolt hatását a toohello további feldolgozás követelményei tömörítése és kibontása hello ügyfél adatok miatt. Emellett a tömörített adatok tárolása teheti nehezebb tootroubleshoot problémák óta nehezebb tooview tárolt adatok szabványos eszközökkel lehet.
* Ha az alkalmazás hello méretezhetőségi célok találatok, majd győződjön meg arról, hogy egy exponenciális leállítási újrapróbálkozások használ (lásd: [újrapróbálkozások](#subheading14)).  Annak jobb toomake meg arról, hogy soha nem megközelíti az hello méretezhetőségi célok (egyikével hello módszerek fent), de ezzel biztosíthatja, hogy az alkalmazás nem csak tartsa gyorsan újrapróbálkozás biztosít, ami még rosszabb szabályozás hello.  

#### <a name="useful-resources"></a>Hasznos segédanyagok
a következő hivatkozások hello méretezhetőségi célok részletesebben is biztosítanak:

* Lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md) méretezhetőségi célok kapcsolatos információkat.
* Lásd: [Azure Storage replikációs](storage-redundancy.md) és hello blogbejegyzés [Azure tárolási redundancia lehetőségek és az írásvédett Georedundáns redundáns tárolás](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx) tárolási redundancia beállításokkal kapcsolatos információkért.
* Az Azure szolgáltatások árazással kapcsolatos aktuális információkért lásd: [Azure-beli árakról](https://azure.microsoft.com/pricing/overview/).  

### <a name="subheading47"></a>Partíció elnevezési egyezmény
Az Azure Storage használja a tartomány particionálási séma tooscale és a terhelés elosztása hello rendszerbe. hello partíciókulcs tartományok használt toopartition adatokat pedig a tartományok terhelésű hello rendszer között. Ez azt jelenti, hogy elnevezési konvenciókat például lexikális rendezés (pl. msftpayroll, msftperformance, msftemployees stb) vagy időbélyegeket (log20160101, log20160102, log20160102, stb.) használatával lesz alkalmasnak alatt esetlegesen közös elhelyezésű hello toohello partíciók ugyanazon partíció a kiszolgálón, amíg egy terheléselosztási művelet felosztja a őket ki kisebb tartományt. Például a tárolóban lévő összes BLOB szolgáltatható egyetlen kiszolgáló amíg hello a blobok terhelése szükséges további újraelosztás hello partíció tartományait. Ehhez hasonlóan a nevük lexikális sorrendbe rendezve enyhén betöltött fiókoknak csoport kezdeményezheti a egyetlen kiszolgáló hello csak egy betöltése vagy az összes ezeket a fiókokat szükségesek őket toobe felosztása több partíció-kiszolgálón. Minden terheléselosztási művelet hatással lehet a hello késés tárolási hívások hello művelet során. hello rendszer képes toohandle forgalom tooa partíció hirtelen kapacitásnövelés korlátozza az egypartíciós kiszolgáló hello méretezhetőség hello terheléselosztási művelet csak megrúgni-a, és újra egyensúlyba hozza a hello kulcs partíciótartomány.  

Néhány ajánlott eljárások tooreduce hello gyakorisága az ilyen műveletek követésével.  

* Vizsgálja meg a hello elnevezési konvenciót kell használni a fiókok, tárolók, blobok, táblák és várólisták, kiszűrése alapos figyelmet igényel. Fontolja meg a kivonatoló függvénnyel az igényeinek leginkább megfelelő 3 számjegyű kivonatát a fióknevek előtag.  
* Időbélyeg helyi időre vagy numerikus azonosítók használata esetén az adatok rendezéséhez nem használ egy csak append (vagy csak illesztenie) forgalmat tooensure lesz. Ezek a minták nincsenek megfelelő-e a tartomány-alapú particionálási rendszert, és sikerült átfutási tooall hello forgalom folyamatos tooa egyetlen partícióra és a korlátozó hello rendszer hatékonyan terheléselosztás. Például ha naponta egy blob objektumot az időbélyegzőnek ÉÉÉÉHHNN, majd az összes hello adatforgalmat például napi művelet használó műveletek tooa egyetlen objektumon, amelynek egyetlen partíció-kiszolgáló által kiszolgált irányítja. Tekintse meg e hello blob korlátokat és partíciónként korlátozza megfelel az igényeinek, és vegye figyelembe, ez a művelet ossza több blobokat, ha szükséges. Ehhez hasonlóan idő adatsorozat adat tárolása a táblák, ha minden hello forgalom lehet hello kulcs névtér utolsó része toohello irányítva. Időbélyeg helyi időre vagy numerikus azonosítót kell használnia, ha előtag hello azonosító 3 számjegyű kivonatát, illetve időbélyegeket előtag hello másodperc részét hello idő például ssyyyymmdd hello esetében. Ha listázása, és lekérdezi-műveleteket rendszeresen hajtja végre, válassza ki a kivonatoló függvényt, amely korlátozza a lekérdezések száma. Egyéb esetben véletlenszerű előtag elegendő lehet.  
* Az Azure Storage használt séma particionálás hello további információkért olvassa el a hello SOSP paper [Itt](http://sigops.org/sosp/sosp11/current/2011-Cascais/printable/11-calder.pdf).

### <a name="networking"></a>Hálózat
Amíg hello API függetlenül attól, hogy hívja, gyakran hello fizikai hálózati megkötések hello alkalmazás jelentős hatást a teljesítményre. hello alábbiakban ismertetjük azok néhány felhasználók szembesülhetnek korlátozások.  

#### <a name="client-network-capability"></a>Ügyfél hálózati funkció
##### <a name="subheading2"></a>Átviteli sebesség
A sávszélesség-hello probléma legtöbbször hello ügyfél hello képességeit. Például egy tárfiók kezelni tud a 10 GB/s, vagy több érkező közben (lásd: [sávszélesség méretezhetőségi célok](#sub1bandwidth)), hálózati sebesség hello a "Kicsi" Azure feldolgozói szerepkör példánya csak képes a körülbelül 100 MB/s. Nagyobb méretű Azure-példányokon a hálózati adapterek nagyobb kapacitással rendelkező rendelkezik, ezért érdemes egy nagyobb példányt, vagy több virtuális gép használ, ha nagyobb hálózati korlátok egyetlen számítógépről van szüksége. Ha egy társzolgáltatás on-premises alkalmazásból fér hozzá, akkor hello ugyanaz a szabály vonatkozik: hello hálózati képességekről hello ügyféleszköz- és hello hálózati kapcsolat toohello Azure tárolási helye az és vagy-javítása céljából igény szerint vagy Tervezze meg az alkalmazás toowork belül képességeit.  

##### <a name="subheading3"></a>Hivatkozás minősége
Csakúgy, mint bármely hálózathasználatot, vegye figyelembe, hogy hálózati körülmények hibák és a csomagveszteség lassú-e a hatékony átviteli sebességet.  WireShark vagy NetMon segítséget nyújthat a probléma diagnosztizálásához.  

##### <a name="useful-resources"></a>Hasznos segédanyagok
További információ a virtuálisgép-méretek és a hozzárendelt sávszélesség: [Windows Virtuálisgép-méretek](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vagy [Linux Virtuálisgép-méretek](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

#### <a name="subheading4"></a>Hely
Minden elosztott környezetben hello ügyfél közelében toohello kiszolgáló elhelyezése nyújt a hello lehető legjobb teljesítményt. Az Azure Storage elérése hello legkisebb mértékű késleltetést, hello ajánlott az ügyfél helye hello belül azonos Azure-régiót. Például ha egy Azure Storage használó Azure-webhelyet, akkor kell helyezni őket mindkét (például US nyugati vagy Ázsia délkeleti) egyetlen régión belül. Ez csökkenti a hello késést és a költségeket, hello – hello írásának időpontjában, sávszélesség-használat egy szabad.  

Ha az alkalmazások nem található Azure-ban (például a mobileszköz-alkalmazások vagy a helyi vállalati szolgáltatások), majd újra ügyfél hello tárfiókot, elérő toohello eszközt közeli régióban helyezi el lesz általában késés csökkentésére. Ha az ügyfelek széles körű terjesztése (a példában, néhány Észak-Amerikában, és néhány, az Európai), akkor érdemes több tárfiókot használ: egy Észak-amerikai régió, a másik egy Európai régióban található. Ez segít tooreduce késés mindkét régióban lévő felhasználók számára. Ez a megközelítés esetén általában egyszerűbb tooimplement hello hello alkalmazás adattárolókhoz adott tooindividual felhasználók, és nem igényel storage-fiókok közötti adatreplikálás.  A széles körű tartalmak terjesztéséhez CDN ajánlott – hello további részleteket a következő szakaszban talál.  

### <a name="subheading5"></a>Tartalom terjesztése
Egyes esetekben az alkalmazás igényeinek tooserve hello azonos tartalom toomany felhasználók (például a termék bemutató videó hello kezdőlap webhely használt), vagy található hello azonos, vagy több. Ebben a forgatókönyvben egy Content Delivery Network (CDN) például az Azure CDN-t kell használnia, hello CDN volna használják, az Azure storage hello hello adatok forrása. Egy Azure Storage-fiókot, amely egy régió létezik, és, hogy nem továbbítanak tartalmat, kisebb késést tooother régiók, eltérően Azure CDN hello világ több adatközpontokban kiszolgálót használ. Emellett a CDN általában támogathat egyetlen tárfiók sokkal nagyobb kimenő korlátokat.  

Azure CDN kapcsolatos további információkért lásd: [Azure CDN](https://azure.microsoft.com/services/cdn/).  

### <a name="subheading6"></a>SAS és a CORS használatával
Tooauthorize kód például a JavaScriptek egy webes böngésző vagy a mobiltelefon tooaccess alkalmazásadatok az Azure Storage van szüksége, amikor egyik módszer-e az alkalmazás webes szerepkör proxyként toouse: hello felhasználó eszközén végzi a hitelesítést hello webes szerepkör, amely a bekapcsolása hello tároló szolgáltatás végzi a hitelesítést. Ezzel a módszerrel elkerülhetők a teszi ki a tárfiók kulcsait a nem biztonságos eszközökön. Azonban ez elhelyez egy nagy terhelés hello webes szerepkör hello felhasználó-eszköz között továbbított összes hello adatot, mert a hello társzolgáltatás kell haladnia hello webes szerepkör. Elkerülheti a webes szerepkör alapján proxyként hello társzolgáltatás megosztott hozzáférési aláírásokkal (SAS), egyes esetekben a fejlécek eltérő eredetű erőforrások megosztása (CORS) együtt. Az SAS segítségével engedélyezheti a felhasználói eszköz toomake kérelmek közvetlen tooa storage szolgáltatást egy korlátozott hozzáférési jogkivonat segítségével. Például ha egy felhasználó azt szeretné, hogy egy fényképező tooyour alkalmazás tooupload, a webes szerepkör is létrehozó és küldő toohello felhasználó eszközén egy SAS-jogkivonatot, amely engedélyezi a következő 30 perc (amely után hello SAS-jogkivonat lejár) toowrite tooa adott engedélyblobja vagy hello tárolója.

Általában a böngésző nem engedélyezi a JavaScript egy webhelyet egy tartományi tooperform bizonyos műveletek, például a "PUT" tooanother tartomány által üzemeltetett lapon. Például, ha a webes szerepkör "contosomarketing.cloudapp.net" és a kívánt toouse ügyfél ügyféloldali JavaScript tooupload egy blob tooyour tárfiók "contosoproducts.blob.core.windows.net" működteti, hello böngésző "azonos származási policy" fog megtiltják a a műveletet. A CORS egy funkciója, amely lehetővé teszi a hello cél tartományában (Ez esetben hello a tárfiók) toocommunicate toohello böngésző megbízhatónak hello forrástartomány (a kis hello webes szerepkör) származó kérelmek.  

Mindkét technológiát segítségével elkerülhető szükségtelen terhelés (és a szűk keresztmetszeteket) a webalkalmazáshoz.  

#### <a name="useful-resources"></a>Hasznos segédanyagok
További információ a SAS: [megosztott hozzáférési aláírásokkal, 1. rész: Understanding hello SAS-modell](../storage-dotnet-shared-access-signature-part-1.md).  

További információ a CORS: [hello Azure Storage szolgáltatásainak Cross-Origin Resource Sharing (CORS) támogatása](http://msdn.microsoft.com/library/azure/dn535601.aspx).  

### <a name="caching"></a>Gyorsítótárazás
#### <a name="subheading7"></a>Adatok beolvasása
Adatok lekérése egy szolgáltatás egyszer általában jobb, mint az első azt kétszer. Fontolja meg egy webes szerepkör, amely már rendelkezik 50MB blob beolvasni hello tárolási szolgáltatás tooserve tartalom tooa felhasználóként fut MVC webalkalmazás hello példát. hello alkalmazás sikerült beolvasásához, hogy ugyanazt a blob minden alkalommal, amikor a felhasználó azt kéri, vagy azt sikerült gyorsítótárazása azt helyileg toodisk és újbóli hello gyorsítótárazott verzió a következő felhasználói kéréseket. Továbbá amikor egy felhasználó hello adatokat kér, hello alkalmazás problémát sikerült feltételes fejlécet beállítani a módosítás időpontja, amelyek a volna hello teljes blob lekérése, ha még nem lett módosítva. A táblaentitásokat a azonos mintát tooworking is alkalmazhatja.  

Bizonyos esetekben dönthet úgy, hogy az alkalmazás-emeléssel felveheti azt, és a során ez idő alatt hello alkalmazást nem kell toocheck hello blob módosításakor beolvasása után rövid ideig érvényes marad, hogy a hello blob.

Konfiguráció, a keresési és az egyéb adatok mindig hello alkalmazás által használt rendszer deduplikációra kiválóan alkalmas jelöltek a gyorsítótárazáshoz.  

Például hogyan tooget egy blob tulajdonságai toodiscover hello utolsó módosításának dátuma használata a .NET, lásd: [Set, a Tulajdonságok beolvasása és a metaadatok](../blobs/storage-properties-metadata.md). Feltételes letöltéseire vonatkozó további információkért lásd: [feltételesen frissítése a helyi másolat készítése a Blob](http://msdn.microsoft.com/library/azure/dd179371.aspx).  

#### <a name="subheading8"></a>Adatok kötegelt feltöltése
Néhány alkalmazás esetben összesíteni az adatokat helyileg, és majd rendszeres időközönként töltse fel egy kötegben minden adat azonnal feltöltése helyett. Például egy webes alkalmazás előfordulhat, hogy tartsa a tevékenységek naplófájl: hello alkalmazás sikerült vagy feltöltése minden tevékenység részleteinek akkor fordul elő, a tábla egységként (amelyhez szükséges számos tárolási műveletek), vagy azt sikerült tevékenység részletei tooa helyi naplófájl mentése, majd rendszeres időközönként töltse fel az összes tevékenység részletei tagolt fájl tooa blob-ként. Ha minden naplóbejegyzés 1KB-nál, feltöltheti a több ezer (tölthet fel egy tranzakción belül mérete too64MB a blob) egy "Put Blob" tranzakción belül. Természetesen hello helyi számítógép összeomlik előzetes toohello feltöltési, potenciálisan elvész néhány naplóadatok: hello alkalmazásfejlesztő kell az ügyféleszközön hello lehetőségét tervezi vagy annak feltöltése sikertelen.  Ha hello tevékenység adatok toobe timespans (csak egyetlen tevékenység) le van szüksége, majd blobok javasoltak táblák keresztül.

### <a name="net-configuration"></a>.NET-konfiguráció
Ha a .NET-keretrendszer használatával hello, ez a szakasz számos gyors konfigurációs beállítások, amelyeket felhasználhat toomake jelentős teljesítményjavulást eredményezhet.  Ha használ egyéb nyelvek, toosee ellenőrizze, hogy a hasonló fogalmak alkalmazza a kiválasztott nyelven.  

#### <a name="subheading9"></a>Alapértelmezett korlát növelése
A .NET, a következő kód hello növeli a hello alapértelmezett kapcsolathoz megadott korlátot (amely általában 2 ügyfél környezetben vagy 10 kiszolgálói környezetben) too100. Általában célszerű hello érték tooapproximately hello a szálak számát, amelyet az alkalmazás.  

```csharp
ServicePointManager.DefaultConnectionLimit = 100; //(Or More)  
```

Meg kell adni a kapcsolatokat megnyitása előtt hello kapcsolathoz megadott korlátot.  

Más programozási nyelven tekintse meg a nyelvi dokumentáció toodetermine hogyan tooset hello kapcsolat korlátozására.  

További információkért lásd: hello blogbejegyzésben [webszolgáltatások: létesített egyidejű kapcsolatok](http://blogs.msdn.com/b/darrenj/archive/2005/03/07/386655.aspx).  

#### <a name="subheading10"></a>Növelje a szálkészlet Min szálak, ha aszinkron és szinkron kód használatával
Ez a kód megnöveli a hello szál készlet min szálak:  

```csharp
ThreadPool.SetMinThreads(100,100); //(Determine hello right number for your application)  
```

További információkért lásd: [ThreadPool.SetMinThreads metódus](http://msdn.microsoft.com/library/system.threading.threadpool.setminthreads%28v=vs.110%29.aspx).  

#### <a name="subheading11"></a>.NET 4.5 szemétgyűjtés előnyeinek kihasználása
Használjon .NET 4.5-ös vagy újabb hello ügyfél alkalmazás tootake előnyeit, teljesítménnyel kapcsolatos fejlesztések a kiszolgáló szemétgyűjtés.

További információkért lásd: hello cikk [az Áttekintés a teljesítménnyel kapcsolatos fejlesztések a .NET 4.5](http://msdn.microsoft.com/magazine/hh882452.aspx).  

### <a name="subheading12"></a>Unbounded párhuzamossági
Párhuzamossági kiváló teljesítmény lehetnek, körültekintően adatokkal unbounded párhuzamossági (szálak, illetve párhuzamos kérelmek hello száma korlátozva) tooupload vagy letöltési, több munkavállalók tooaccess használatával több partíciót (tárolók, várólisták, vagy tábla partíciók) a hello ugyanazon tárfiók vagy tooaccess hello elemei egyazon partícióra kerüljenek. Ha hello párhuzamossági unbounded, az alkalmazás haladhatja meg a hello ügyfél eszközök képességei, vagy hello storage-fiók méretezhetőségi célok hosszabb késések eredményez, és a szabályozás.  

### <a name="subheading13"></a>Storage Ügyfélkódtáraival és eszközök
Mindig használjon hello legújabb Microsoft által biztosított ügyfél kódtárak és eszközök. Hello írásának időpontjában nincsenek .NET, Windows Phone, Windows-Futtatókörnyezetű, Java és C++ elérhető klienskódtárait, valamint más nyelvekre preview szalagtárak. Ezenkívül a Microsoft közzétette PowerShell-parancsmagok és az Azure Storage használatához Azure parancssori felület parancsait. Microsoft aktívan házon belül fejlesztett alkalmazásokra teljesítménnyel szem előtt ezeket az eszközöket, toodate hello legújabb verziót a mentést biztosít, és biztosítja azokat kezelni, belső bizonyítása teljesítmény eljárások hello számos.  

### <a name="retries"></a>Újrapróbálkozások
#### <a name="subheading14"></a>Sávszélesség-szabályozás/ServerBusy
Bizonyos esetekben hello tároló szolgáltatás szabályozása az alkalmazás vagy előfordulhat, hogy egyszerűen nem tooserve hello kérelem miatt toosome átmeneti állapotról és történhet "503-as kiszolgáló elfoglalt" üzenet vagy az "500 időtúllépése" visszaadása.  Ez akkor fordulhat elő, ha az alkalmazás felé közelít hello méretezhetőségi célok bármelyikét, vagy ha hello rendszer van újraelosztás a particionált adatok tooallow nagyobb átviteli sebesség eléréséhez.  hello ügyfélalkalmazás általában újra kell próbálkoznia ilyen hibát okozó hello művelet: hello azonos később kérelem kísérlet sikeres lehet. Ha hello társzolgáltatás van az alkalmazás szabályozását, mert a méretezhetőségi célok túllépte, vagy ha a hello szolgáltatást a valamilyen más okból nem tooserve hello irányuló kérelem volt, az agresszív újrapróbálkozások általában ellenőrizze hello probléma ami még rosszabb. Emiatt az exponenciális vissza (hello szalagtárak alapértelmezett toothis ügyfélviselkedést) ki kell használnia. Például az alkalmazás előfordulhat, hogy 2 másodperc, majd 4 másodperc 10 másodpercet, majd a 30 másodperc után próbálkozzon újra, és majd teljesen feladták. Ezt a viselkedést eredményezi, az alkalmazás jelentősen csökkenti annak terhelésétől hello szolgáltatás helyett súlyosbodott problémákat.  

Vegye figyelembe, hogy kapcsolódási hibák követően újra megkísérelhető a azonnal, mivel nincsenek hello eredménye sávszélesség-szabályozás, illetve átmeneti várt toobe.  

#### <a name="subheading15"></a>– Újrapróbálkozást lehetővé nem tevő hibák
hello klienskódtárak mely hibák újrapróbálkozási-, és nem ismerik. Ha saját kódot hello storage REST API-t, ne feledje azonban, hogy nem kell újra hibák vannak a: például a 400-as válasz azt jelzi, hogy hello ügyfélalkalmazás (hibás kérés) nem lehet feldolgozni, mert a kérés érkezett, nem várt formában. Küldje el újra a kéréssel hatására hello a minden alkalommal ugyanaz a válasz, nincs újrapróbálkozás azt a pont. Elleni hello storage REST API-t saját kód írását, vegye figyelembe, hogy milyen hello hibakódjai középérték és hello megfelelő módon tooretry (vagy nem) esetében.  

#### <a name="useful-resources"></a>Hasznos segédanyagok
Tárolási hibakódokkal kapcsolatban további információkért lásd: [állapotát és hibakódok](http://msdn.microsoft.com/library/azure/dd179382.aspx) hello Microsoft Azure-webhelyen.  

## <a name="blobs"></a>Blobok
Az eljárások bizonyítása hozzáadása toohello [minden szolgáltatás](#allservices) a fentiekben ismertetett eljárásokat bizonyítása hello következő kifejezetten toohello blob szolgáltatás alkalmazása.  

### <a name="blob-specific-scalability-targets"></a>A BLOB-specifikus méretezhetőségi célok
#### <a name="subheading46"></a>Egy adott objektum eléréséhez egyidejűleg több ügyfél
Ha egy adott objektum egyidejűleg hozzáférő ügyfelek nagy számú szüksége lesz egy objektum és a storage méretezhetőségi célok tooconsider. hello pontos számát egy adott objektum hozzáférő ügyfelek fogja például hello objektum egyszerre kérő ügyfelek hello objektum mérete hello hello száma tényezőktől függően változnak, hálózati feltételek stb.

Ha hello objektum keresztül terjeszthető például képek és videók CDN kiszolgálása egy webhelyről, majd a CDN-t kell használnia. Lásd: [Itt](#subheading5).

Más esetekben, például bizalmas adatokat hello esetén tudományos szimulációja két választási lehetősége van. hello először toostagger a számítási hozzáférési ilyen objektum hello érhető el egy meghatározott időtartamra vonatkozóan válaszidővel egyidejűleg használatban. Azt is megteheti ideiglenesen másolhatja hello objektum toomultiple tárfiókok hello és növelhetik a teljes IOPS objektumonként és tárfiókok között. A korlátozott tesztelése észleltünk, hogy körülbelül 25 virtuális gépek egyidejűleg tölthető le 100GB blob párhuzamos (a virtuális gépek lett parallelizing hello letöltési 32 szálat használ). Ha tooaccess hello objektum kellene 100 ügyféllel, először tooa második tárfiók másolásához rendelkezik hello első 50 virtuális gépek hozzáférési hello első blob és hello második 50 virtuális gépek hozzáférési hello második blob. Eredmények függ az alkalmazások viselkedését, tesztelje a tervezés során. 

#### <a name="subheading16"></a>Sávszélesség és a műveletek másodpercenkénti Blob
Olvassa el, vagy írható tooa egyetlen blob-, mentése tooa legfeljebb 60 MB/s (Ez körülbelül 480 MB/s ami hello képességek sok ügyfél oldalán hálózatok meghaladja az (beleértve a hello hello ügyféleszközön a fizikai hálózati Adapterre). Emellett egyetlen blob támogatja too500 kérelmek száma másodpercenként. Ha több ügyféllel tooread igénylő hello azonos blob, és előfordulhat, hogy meghaladja ezt a korlátot, fontolja meg a CDN hello blob terjesztéséhez.  

A blobok cél átviteli kapcsolatos további információkért lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md).  

### <a name="copying-and-moving-blobs"></a>Másolásának és áthelyezésének Blobok
#### <a name="subheading17"></a>A Blob másolása
hello storage REST API 2012-02-12-es verzió hello hasznos képességét toocopy blobok bevezetett fiókok között: ügyfélalkalmazás utasítani hello tárolási szolgáltatás toocopy blob más forrásból (valószínűleg a során eltérő tárfiók), és hagyja hello szolgáltatás árnyékmásolata hello aszinkron módon történik. Ez jelentősen csökkenti más tárfiókok az adatok áttelepítése esetén, mert nem kell toodownload és hello adatok feltöltése hello alkalmazáshoz szükséges hello sávszélesség.  

Egy figyelembe azonban, hogy másolás storage-fiókok között, ha nincs idő garancia hello példány befejeződik, ha a. Ha az alkalmazásnak toocomplete blob gyorsan másolja az ellenőrzése alatt, valószínűleg jobb toocopy hello blob tooa VM letöltheti és toohello cél feltöltése.  Ebben a helyzetben teljes kiszámíthatóságot győződjön meg arról, hogy történik-e hello másolási hello futó virtuális gép által azonos Azure-régió, vagy pedig hálózati körülmények lehet (és valószínűleg fog) hatással a másolási teljesítményére.  Ezenkívül kísérheti hello egy aszinkron példány programozott módon.  

Figyelje meg, hogy ugyanazt a tárfiókot maga általában gyorsan befejeződött hello belül másolja.  

További információkért lásd: [másolási Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx).  

#### <a name="subheading18"></a>AzCopy használata
hello Azure Storage csapat kiadott egy parancssori eszközt "AzCopy", amely jelentette toohelp a tömeges, valamint a tárfiókok között számos blobok átvitele.  Ezt az eszközt ebben a forgatókönyvben van optimalizálva, és nagy átviteli sebesség érhető el.  A tömeges feltöltés, a letöltésről és a másolással használata javasolt. További információk toolearn és le is, lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md).  

#### <a name="subheading19"></a>Az Azure Import/Export szolgáltatás
A rendkívül nagy mennyiségű adatok (legfeljebb 1 TB-os) a hello Azure Storage kínál hello Import/Export szolgáltatás, amely lehetővé teszi a feltöltés és letöltés a blob storage által szállítási merevlemez-meghajtókat.  Helyezte el az adatokat egy merevlemezre és elküldi a feltöltésének tooMicrosoft, vagy egy üres merevlemez-meghajtóról tooMicrosoft toodownload adatküldéshez.  További információkért lásd: [hello a Microsoft Azure Import/Export szolgáltatás tooTransfer adatok tooBlob Storage használata](../storage-import-export-service.md).  Ez lehet sokkal hatékonyabb, mint feltöltése/letöltése az adatok mennyiségét hello hálózaton keresztül.  

### <a name="subheading20"></a>Metaadatok
hello blob szolgáltatás támogatja a head kérelmek, amelyek magukban foglalhatják hello blob metaadatait. Például ha az alkalmazás hello EXIF adatokat fénykép szükséges, az olvashat be hello fénykép és bontsa ki. toosave sávszélesség és a teljesítmény javítása érdekében az alkalmazás sikerült hello EXIF adat tárolása hello blob metaadatai feltöltéskor hello alkalmazás hello fénykép: majd le hello EXIF adatokat a metaadatok csak olyan HEAD kérelem jelentős sávszélesség mentése illetve hello feldolgozási időt tooextract hello EXIF adatokat minden egyes alkalommal hello blob olvasható. Akkor hasznos, ahol csak kell hello metaadatok, és nem hello teljes tartalma blob forgatókönyvekben.  Vegye figyelembe, hogy a metaadatok csak 8 KB-os tárolható egy blob (hello szolgáltatás nem fogadja el a kérelmet toostore ennél nagyobb), így ha hello adatok nem fér el ez a méret, akkor előfordulhat, hogy nem tudja toouse kell ezt a módszert használja.  

A példa bemutatja, hogyan tooget egy blob metaadatok használatával a .NET, lásd: [Set, a Tulajdonságok beolvasása és a metaadatok](../blobs/storage-properties-metadata.md).  

### <a name="uploading-fast"></a>Gyors feltöltése
gyors tooupload blobokat, hello első kérdés tooanswer van: Ön egy blob vagy nagy feltöltésével?  Hello alatt útmutatást toodetermine hello helyes metódus toouse a forgatókönyvtől függően használja.  

#### <a name="subheading21"></a>Egy nagy blob gyors feltöltése
nagy egyetlen blob gyorsan tooupload, az ügyfélalkalmazás kell töltse fel a blokkok vagy lapok párhuzamos (az egyes blobok és hello tárfiók egész hello méretezhetőségi célok szem előtt tartva alatt).  Ne feledje, hogy hello hivatalos Microsoft által biztosított RTM Storage ügyfélkódtáraival (.NET, Java) hello képességét toodo ez.  Az egyes hello szalagtárak használja a hello megadott objektumtulajdonság tooset hello szint egyidejű alatt:  

* .NET: Set ParallelOperationThreadCount a egy BlobRequestOptions objektum toobe használt.
* Java/Android: BlobRequestOptions.setConcurrentRequestCount() használata
* NODE.js: ParallelOperationThreadCount vagy hello lehetőségek és hello blob szolgáltatás használható.
* C++: Hello blob_request_options::set_parallelism_factor módszert használja.

#### <a name="subheading22"></a>Sok blobok gyors feltöltése
számos gyorsan, blobok tooupload párhuzamosan blobok feltöltése. Ez a gyorsabb, mint a feltöltés egyetlen blobok egyszerre a párhuzamos blokk feltöltések mert hello feltöltés terjedése több partíciót hello tárolási szolgáltatás között. Egy blob csak 60 MB/s (körülbelül 480 Mbps sebességű) átviteli támogatja. Hello írásának időpontjában egy Egyesült államokbeli LRS fiók legfeljebb támogat too20 GB/s érkező, amely jóval több, mint a hello egy egyedi blob által támogatott.  [AzCopy](#subheading18) párhuzamos feltöltések alapértelmezés szerint hajt végre, és ehhez a forgatókönyvhöz ajánlott.  

### <a name="subheading23"></a>Hello megfelelő típusú blob kiválasztása
Az Azure Storage támogatja a blob kétféle: *lap* blobok és *blokk* blobokat. Adott használati forgatókönyvek esetében a választott blob típushoz hello teljesítményét és méretezhetőségét, a megoldás hatással lesz. Blokkblobok megfelelőek, ha azt szeretné, tooupload nagy mennyiségű adat hatékonyan: például ügyfélalkalmazás esetleg tooupload fénykép vagy videó tooblob tárolási. Lapblobokat megfelelőek, ha hello alkalmazásnak kell tooperform véletlenszerű írások hello adatokon: például Azure virtuális merevlemezek lapblobokat tárolódnak.  

További információkért lásd: [ismertetése Blokkblobokat, hozzáfűző blobokat és Lapblobokat](http://msdn.microsoft.com/library/azure/ee691964.aspx).  

## <a name="tables"></a>Táblák
Az eljárások bizonyítása hozzáadása toohello [minden szolgáltatás](#allservices) a fentiekben ismertetett eljárásokat bizonyítása hello következő kifejezetten toohello tábla szolgáltatás alkalmazása.  

### <a name="subheading24"></a>Tábla vonatkozó méretezhetőségi célok
Továbbá toohello sávszélességgel kapcsolatos korlátozásai egy teljes tárfiókja, a kell hello a következő egyedi méretezhetőségi korlátot.  Vegye figyelembe, hogy hello rendszer egyenleg betölti a a forgalom növekedése, de ha a forgalom hirtelen felszakadásáig, akkor előfordulhat, hogy nem kell tudni tooget átviteli kötet azonnal.  Ha a minta van felszakadásáig, számíthat toosee sávszélesség-szabályozás és/vagy időtúllépések alatt hello kapacitásnövelés hello tárolási szolgáltatásként automatikusan ki a tábla egyenlegének betöltése.  Jobb eredményeket hello rendszer tooload egyenlege megfelelően nyújtja lassan általában ramping rendelkezik.  

#### <a name="entities-per-second-account"></a>Entitások / másodperc (fiók)
hello skálázhatósági korlátot táblák eléréséhez too20, egy olyan fiók másodpercenként 000 entitások (1KB minden) működik.  Általában minden entitás beszúrt, frissítése, törlése, vagy beolvasott számát, ez a cél felé.  Ezért egy 100 entitást tartalmazó kötegelt Beszúrás 100 entitást számít.  Egy lekérdezést, amely 1000 entitások vizsgálja, és visszaadja az 5 1000 entitások számít.  

#### <a name="entities-per-second-partition"></a>Entitások / másodperc (partíció:)
Hello belül egyetlen partícióra, a méretezhetőség cél táblák elérése: 2000 entitások (1KB minden) másodpercenként, használatával hello azonos számbavételi hello előző szakaszban leírtak szerint.  

### <a name="configuration"></a>Konfiguráció
Ez a rész felsorolja használható toomake jelentős teljesítményjavulást eredményezhet a hello table szolgáltatás több gyors konfigurációs beállítások:  

#### <a name="subheading25"></a>JSON használata
Tárolási verziójú 2013-08-15 kezdve hello table szolgáltatás támogatja a JSON hello XML-alapú AtomPub formátum helyett a táblabeli adatok átviteléhez. Ez csökkentheti a tartalom méretét szerint 75 %, és jelentősen növelheti az alkalmazás hello teljesítményét.

További információkért lásd: hello utáni [Microsoft Azure-táblákat: bevezetéséről JSON](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/05/windows-azure-tables-introducing-json.aspx) és [Table szolgáltatási műveletek az adattartalom formátuma](http://msdn.microsoft.com/library/azure/dn535600.aspx).

#### <a name="subheading26"></a>Nagle kikapcsolása
Nagle tartozó algoritmus széles körben egy azt jelenti, hogy tooimprove hálózati teljesítményt, megvalósítva TCP/IP-hálózatokon keresztül. Azonban nincs optimalizálva, minden körülmények között (például magas interaktív környezetekben). Az Azure Storage Nagle tartozó algoritmus negatív hatással rendelkezik kérelmek toohello tábla és a queue szolgáltatások hello teljesítményét, és tiltsa le, ha lehetséges.  

További információkért tekintse meg a következő blogbejegyzésben található [Nagle tartozó algoritmus nem rövid kis kérelmek felé](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx), amely ismerteti, miért Nagle tartozó algoritmus tábla- és várólista kérelmek rosszul kommunikál, és mutatja hogyan toodisable azt az ügyfél az alkalmazás.  

### <a name="schema"></a>Séma
Hogyan jelöl, és az adatok lekérdezése hello legnagyobb egyetlen tényező, amely befolyásolja a hello table szolgáltatás hello teljesítményét. Míg minden más, ez a szakasz ismerteti néhány általános bevált gyakorlatokat vonatkoznak:  

* Táblatervezés
* Hatékony lekérdezések
* Hatékony adatfrissítések  

#### <a name="subheading27"></a>Táblák és -partíciók
Táblák partíciókra oszlik meg. Minden entitás tárolt partícióra megosztások hello ugyanazzal a partíciókulccsal, és rendelkezik egy egyedi sorazonosítóként kulcs tooidentify azt, hogy a partíción belül. Partíciók előnyt nyújtanak, de méretezhetőségének korlátai is vezethet.  

* Előnyei: Entitásokat is frissítheti a hello azonos too100 külön tárolási műveletek (4MB teljes mérete legfeljebb) másolatot tartalmazó egyetlen, atomi, kötegelt tranzakció partíciójához. Feltéve, hogy ugyanannyi hello entitások toobe beolvasni, a lekérdezheti is belül egyetlen partícióra adatok (de mutatunk be további ajánlások tábla adatok lekérdezése) partíciók kiterjedő adatok hatékonyabban.
* A méretezhetőségi korlátot: egyetlen partícióra tárolt hozzáférés tooentities nem lehet elosztott terhelésű mert partíciók támogatja a atomi kötegelt tranzakciókat. Emiatt hello méretezhetőség egyedi tábla partícióhoz célja alacsonyabb, mint a hello table szolgáltatás egészére.  

Miatt a következő jellemzőkkel táblák és-partíciók el kell fogadnia a következő tervezési alapelvek hello:  

* Az ügyfélalkalmazás gyakran frissíteni vagy ugyanazon logikai egység munka kell elhelyezkedniük hello kérdezhetők le adatokat hello egyazon partícióra kerüljenek.  Ennek az lehet az oka az alkalmazás van összesítése írási műveleteket, vagy azt szeretné, hogy atomi kötegműveletek tootake előnyeit.  Is egy olyan partíciót adatok hatékonyabban lekérhetők az egyetlen lekérdezés mint adat partíciók között.
* Az ügyfélalkalmazás nem beszúrásához vagy frissítéséhez vagy adatok lekérdezés hello ugyanazon logikai egység munka (egyetlen lekérdezés vagy kötegelt frissítése) kell elhelyezni, a különböző partíciók.  Egy fontos: az, hogy nincs nincs korlát toohello számú partíciós kulcsok található egyetlen táblában, így több millió partíciós kulcsok esetében nem jelent problémát, és nem lesz hatással a teljesítményre.  Például ha az alkalmazás rendelkező felhasználói bejelentkezés népszerű webhely, hello felhasználói azonosítót használja, mint hello partíciókulcs lehet érdemes választani.  

#### <a name="hot-partitions"></a>Működés közbeni partíciók
A működés közbeni partíció egyik fogadó egy hello forgalom tooan fiók le aránytalanul nagy részét, és nem kell az elosztott terhelésű mert egyetlen partíció.  Általában a működés közbeni partíciók létrejönnek két módszer egyikével:  

##### <a name="subheading28"></a>Csak hozzáfűzés és illesztenie csak minták
hello "Hozzáfűzése csak" minta egyike ahol összes (vagy szinte minden) hello forgalom tooa PK megadott növeli, és csökkenti a függően toohello aktuális idő.  Például akkor, ha az alkalmazás használt hello aktuális dátumot a naplóadatok partíciókulcsként.  Ennek eredményeképpen minden hello Beszúrások toohello a tábla utolsó partíciója fog, és hello rendszer nem lehet terhelést elosztani, mert hello írási műveletek mindegyike a táblázat további folyamatos toohello vége.  Ha hello forgalom toothat partíció meghaladják hello partíció szintű méretezhetőség célként, majd azt eredményez szabályozási.  Jobb tooensure elküldött forgalom toomultiple partíciók, célszerű a tooenable load balance hello keresztül a tábla kéri.  

##### <a name="subheading29"></a>Nagy forgalmú adatok
Ha a particionálási sémát eredményezi egy olyan partíciót, amelyek csak adatok, mint a többi partíció sokkal használt, is találkozhat sávszélesség-szabályozás, az adott partíció megközelíti hello méretezhetőség célja egy olyan partíciót.  Jobb toomake meg arról, hogy a partíció sémát eredményezi nincs egyetlen partition megközelíti hello méretezhetőségi célok.  

#### <a name="querying"></a>Lekérdezése
Ez a szakasz ismerteti a bevált gyakorlat hello table szolgáltatás lekérdezése.  

##### <a name="subheading30"></a>Lekérdezés hatóköre
Számos különböző módokon toospecify hello entitások tooquery.  hello az alábbiakban az egyes hello használatát tárgyalja.  

Általában elkerülése érdekében a vizsgálatok (lekérdezések nagyobb, mint egyetlen entitás), de kell beolvasni, ha próbálja tooorganize az adatokat, hogy a vizsgálatok vagy a felesleges entitások jelentős mennyiségű visszaadó nélkül kell hello adatainak beolvasása.  

###### <a name="point-queries"></a>Pont lekérdezések
Egy pont lekérdezés lekéri pontosan egy entitást. Ennek érdekében hello partíciókulcs és hello entitás tooretrieve sor kulcsa. Ezeket a lekérdezéseket, nagyon hatékony, és ajánlott őket, amikor csak lehetséges.  

###### <a name="partition-queries"></a>Partíció lekérdezések
A partíciólekérdezés osztja meg a közös partíciókulcsot adatok olyan készlete, amely. Általában hello lekérdezés határozza meg a sor értékek tartománya vagy néhány entitás tulajdonság értéktartománya hozzáadása tooa partíciós kulcs. Ezek pont lekérdezések-nél kevésbé hatékonyak, és takarékosan.  

###### <a name="table-queries"></a>Tábla lekérdezések
A lekérdezés, amely nem ugyanazt a közös partíciókulcsot az entitások készletének, amely. Ezeket a lekérdezéseket nem hatékony, és lehetőség szerint kerülje azokat.  

##### <a name="subheading31"></a>Lekérdezés sűrűség
Egy másik lekérdezés hatékonyságát kulcsfontosságú tényező beolvasott toofind hello visszaadott beállított entitások összehasonlított toohello számát adja vissza a entitások hello számát jelenti. Ha az alkalmazást, hogy csak 1 % hello adatok megosztások, hello lekérdezés megvizsgálja összes adja vissza egy entitás 100 entitást tulajdonság értéke szűrőt tartalmazó tábla lekérdezést hajt végre. hello tábla méretezhetőségi célok tárgyalt korábban az összes kapcsolódó beolvasott entitások toohello száma, és nem hello visszaadott entitások száma: lekérdezés kis sűrűségű könnyen okozhat hello tábla szolgáltatás toothrottle az alkalmazás, mert olyan sok kell vizsgálata entitások tooretrieve hello entitásra keres.  Lásd az alábbi szakasz hello a [denormalization](#subheading34) további információt a tooavoid ez.  

##### <a name="limiting-hello-amount-of-data-returned"></a>Mennyi az adatok visszaadásához hello korlátozása
###### <a name="subheading32"></a>Szűrés
Ha tudja, hogy a lekérdezés entitások ne hello ügyfélalkalmazás visszatér, fontolja meg egy szűrő tooreduce hello mérete hello beállítása adott vissza. Közben hello entitások nem toohello ügyfél továbbra is száma hello méretezhetőségi korlátok felé, az alkalmazások teljesítményéről fog miatt hello kisebb hálózati terhelés méretének növelése és a hello arról, hogy az ügyfélalkalmazás kell folyamat entitások csökkentett száma .  A Megjegyzés fent látható [lekérdezés sűrűség](#subheading31), azonban – hello méretezhetőségi célok vonatkoznak-e a beolvasott, entitások toohello száma, a lekérdezés, amely a számos entitás továbbra is eredményezhet sávszélesség-szabályozás, még akkor is, ha néhány entitásokat ad vissza.  

###### <a name="subheading33"></a>Leképezése
Ha az ügyfélalkalmazást igényel, csak korlátozott számú tulajdonságok a hello entitásokból a táblázatban, leképezés toolimit hello hello visszaadott adatok méretétől is használhatja. Csakúgy, mint a szűrés, ez segít tooreduce hálózati terhelés és ügyfél feldolgozása.  

##### <a name="subheading34"></a>Denormalization
Ellentétben a relációs adatbázisok használata hello hatékonyan a tábla adatainak lekérdezése bevált gyakorlatok vezethet toodenormalizing adatait. Ez azt jelenti, hogy ugyanazokat az adatokat a több egység duplikálását hello (egy toofind hello adatok segítségével minden kulcs), hogy a lekérdezés toofind hello adatok hello ügyféloldali igények, ahelyett, hogy entitások toofind tooscan nagyszámú kell beolvasni entitások toominimize hello száma hello adatoknak az alkalmazás kell.  Például az elektronikus kereskedelmi webhely, érdemes lehet toofind egy rendezési mindkét hello ügyfél-azonosítója (, adja meg a megrendelések) és hello dátum (engedi rendelések napon).  A Table Storage esetében ajánlott toostore hello entitás (vagy egy hivatkozás tooit) kétszer – egyszer az ügyfél-azonosító, egyszer toofacilitate találja hello dátum szerint táblanév PK és RK toofacilitate találja.  

#### <a name="insertupdatedelete"></a>Insert/Update/Delete
Ez a szakasz ismerteti a bevált eljárásokat a hello table szolgáltatás tárolt entitásokat módosítását.  

##### <a name="subheading35"></a>Kötegelés
Kötegelt tranzakciókat ismert, entitás csoport tranzakciók (ETG) az Azure Storage; minden hello műveletet egy ETG belül egyetlen partícióra található egyetlen táblában kell lennie. Ahol lehetséges, használjon ETGs tooperform Beszúrások, a frissítések és törlések kötegekben. Ez csökkenti a kiszolgálókkal való adatváltások számát hello számát az ügyfél alkalmazás toohello kiszolgálóról, csökkenti (egy ETG más célra egyetlen tranzakció számít, és tartalmazhat too100 tárolási műveletek mentése) számlázható tranzakciók hello száma, és lehetővé teszi a atomi frissítések (sikeres minden műveletnél vagy az összes sikertelen egy ETG belül). Például a mobil eszközök nagy késleltetésű környezetek nagy mértékben előnyösek, ETGs használatával.  

##### <a name="subheading36"></a>Upsert
Használjon tábla **Upsert** műveletek lehetőség. Két típusa van **Upsert**, mindkettőnek hatékonyabb, mint egy hagyományos lehet **beszúrása** és **frissítés** műveletek:  

* **InsertOrMerge**: ezzel, ha azt szeretné, hogy tooupload hello entitás tulajdonságok részhalmazát, de nem biztos benne, hogy hello entitás már létezik. Ha hello entitás létezik, a hívás frissíti hello szereplő hello tulajdonságok **Upsert** műveletet, és minden meglévő tulajdonságainak bízza, mint azok, ha hello entitás nem létezik, hello új entitás beilleszti. Ez hasonló toousing leképezése egy lekérdezésben abban, hogy csak a tooupload hello tulajdonságokhoz módosítani kell.
* **InsertOrReplace**: ezt használja, ha azt szeretné, hogy egy teljesen új entitás tooupload, de azt nem biztos benne, hogy létezik-e. Ön csak akkor használható Ha ismeri a adott hello újonnan feltöltött entitás nem teljes mértékben megfelelő, mert a régi entitás hello teljesen felülírja. Például azt szeretné, hogy tooupdate hello entitás, amely tárolja a felhasználó aktuális helye függetlenül-e hello alkalmazás korábban találhatók meg a helyadatok hello felhasználó; Új hely entitás befejeződött, és nincs szükség az adatok minden korábbi entitás hello.

##### <a name="subheading37"></a>Az Adatsorozatban tárolja egyetlen entitás
Egyes esetekben az alkalmazás tárolja, hogy gyakran szükséges adatokat tooretrieve egyszerre több: például egy alkalmazás előfordulhat, hogy nyomon CPU-használat idővel rendelés tooplot hello adatok a működés közbeni diagram hello utolsó 24 órában. Egy megoldás, toohave egy olyan táblát, óránként rendelkező entitás minden egyes entitás egy adott órát jelölő és a CPU-használat hello tárolása, hogy egy óra. tooplot ezeket az adatokat, hello alkalmazás kell tooretrieve hello entitások okozó hello hello adatait legutóbbi 24 órában.  

Azt is megteheti, az alkalmazás sikerült tárolni hello CPU-használat minden órában egyetlen entitás külön tulajdonságként: tooupdate minden órában, az alkalmazás használhat egyetlen **InsertOrMerge Upsert** hello tooupdate hello érték meghívása legutóbbi órában. tooplot hello adatokat, a hello alkalmazást csak kell tooretrieve 24, így egy nagyon hatékony lekérdezés helyett egyetlen entitás (lásd fentebb vitafórum a [hatókör lekérdezése](#subheading30)).

##### <a name="subheading38"></a>A blobok strukturált adatok tárolására
Egyes esetekben strukturált adatok érzi, például táblázatokban, el kell, de az entitások tartományok mindig olvassa együtt, és kötegelt szúrható be.  Egy jó példa erre, egy naplófájlt.  Ebben az esetben a batch-naplók néhány percig, tegye őket, és majd mindig keres, valamint egyszerre több percnyi.  Ebben az esetben a teljesítmény, ez a beállítás nagyobb toouse blobok helyett táblák, mivel Ön is jelentősen csökkentheti a objektumok írt/visszaadott hello számát, valamint általában a kérelmek száma, amelyeket hello.  

## <a name="queues"></a>Üzenetsorok
Az eljárások bizonyítása hozzáadása toohello [minden szolgáltatás](#allservices) a fentiekben ismertetett eljárásokat bizonyítása hello következő kifejezetten toohello várólista szolgáltatás alkalmazása.  

### <a name="subheading39"></a>Méretezhetőségi korlátok
Egy adott sorba körülbelül 2000 üzenetek (1KB minden) (egyes AddMessage GetMessage és DeleteMessage száma itt üzenetként) másodpercenként feldolgozásához. Ha ez nem elegendő az alkalmazáshoz, több üzenetsorok használata, és köszönőüzenetei elosztva.  

A jelenlegi méretezhetőségi célok megtekintése [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md).  

### <a name="subheading40"></a>Nagle kikapcsolása
Hello című rész ismerteti, amelyek hello Nagle algoritmus tábla-konfigurációban – hello Nagle algoritmus hello teljesítmény várólista kérelmek általában hibás, és tiltsa le azt.  

### <a name="subheading41"></a>Üzenet mérete
Várólista teljesítményének és méretezhetőségének csökken a üzenet méretének növekedése. Az üzenet csak hello hello fogadó kell kell elhelyezni.  

### <a name="subheading42"></a>Kötegelt lekérése
Too32 üzenetek várólistából való várólistából egy művelettel kérheti le. Ez csökkentheti használatával hello száma hello ügyfélalkalmazás, ami különösen hasznos az olyan környezetekben, mobileszközök, például a nagy késleltetésű.  

### <a name="subheading43"></a>Várólista lekérdezési időköz
A legtöbb alkalmazás kérdezze le az üzenetek várólistából való várólistából, amely lehet hello legnagyobb forrásokból tranzakciók az adott alkalmazáshoz. Válassza ki a lekérdezési időköz georeplikációt: lekérdezés túl gyakran okozhat az alkalmazás tooapproach hello méretezhetőségi célok hello várólista. Azonban 200 000 tranzakciók $0,01 (hello írásának időpontjában), a lekérdezési követően egy hónapig másodpercenként volna költség költségeket, így kevesebb mint 15 cent egyprocesszoros nincs általában egy tényező, amely befolyásolja a kiválasztott lekérdezési időközt.  

Naprakész költség információkért lásd: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/).  

### <a name="subheading44"></a>UpdateMessage
Használhat **UpdateMessage** tooincrease hello láthatatlansági időtúllépés vagy tooupdate állapotadatokat üzenet. Ez nem hatékony, ne feledje, hogy minden egyes **UpdateMessage** művelet módszert hello méretezhetőség cél felé számolnak. Azonban ez nem egy sokkal hatékonyabb módszert alkalmaz, mint hogy olyan munkafolyamatot, amely továbbítja a feladat egy olyan sort toohello a a következő lépésre hello feladat befejezését követően. Hello segítségével **UpdateMessage** művelet lehetővé teszi, hogy az alkalmazás toosave hello feladat toohello állapotüzenetet, és majd folytathatja a munkát, újra Üzenetsor-kezelés üdvözlőüzenetére hello feladat minden alkalommal, amikor egy lépés befejezése hello a következő lépéshez helyett.  

További információkért lásd: hello cikk [hogyan: hello aszinkron üzenet tartalmának módosítása](../queues/storage-dotnet-how-to-use-queues.md#change-the-contents-of-a-queued-message).  

### <a name="subheading45"></a>Alkalmazás-architektúra
Az alkalmazás méretezhető architektúra várólisták toomake kell használnia. hello következő egyes módszereket, várólisták toomake az alkalmazás több méretezhető sorolja fel:  

* Várólisták toocreate várakozási sora munkahelyi feldolgozásához használja, és a munkaterhelések simítja az alkalmazásban. Például hogy sikerült sorba felhasználók tooperform processzor intenzív munkahelyi például feltöltött képek átméretezése érkező kérelmeket.
* Az alkalmazás részei várólisták toodecouple használhatja, hogy egymástól függetlenül méretezheti őket. Például egy előtér-webkiszolgáló sikerült helyezzen el felméréseket, a felhasználók egy várólista újabb elemzés és tárolására. Érdemes felvenni több feldolgozói szerepkör példányok tooprocess hello várólista adatokat szükség szerint.  

## <a name="conclusion"></a>Összegzés
Ez a cikk ismertet néhány általános, hello bizonyítása eljárások az Azure Storage használata esetén a teljesítmény optimalizálása. Azt minden alkalmazás fejlesztői tooassess ösztönözze az egyes eljárások fent hello alkalmazásokban, és vegye figyelembe a ható hello javaslatok tooget kiváló teljesítményt az Azure Storage használó alkalmazások esetén.
