---
title: "aaaAnalyze élcsomópontok teljesítményének Azure CDN |} Microsoft Docs"
description: "A Microsoft Azure CDN élcsomópontok teljesítményének elemzése. Peremhálózati teljesítmény Analytics hello CDN biztosít a részletes információkat forgalom és a sávszélesség-használat."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 8cc596a7-3e01-4f76-af7b-a05a1421517e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 35361d1c5e27fc6b8536c29e33c2ed217ee4d17e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Az élcsomópontok teljesítményének elemzése a Microsoft Azure CDN szolgáltatásban
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Áttekintés
Peremhálózati teljesítmény analytics hello CDN biztosít a részletes információkat forgalom és a sávszélesség-használat. Ez az információ majd lehet használt toogenerate trendekkel statisztika, amelyek lehetővé teszik toogain ismereteket hogyan az eszközök gyorsítótárazott és kézbesített tooyour ügyfelek folyamatban van. Viszont így tooform, hogyan toooptimize hello kézbesítését saját tartalmait és toodetermine milyen problémák stratégia kell toobetter emelés hello CDN megoldásra. Emiatt nem csak lesz képes tooimprove adatok kézbesítési teljesítmény, de a program kell tudni tooreduce CDN költségeit.

> [!NOTE]
> Minden jelentés UTC/GMT notation egy dátum/idő megadásakor használhatók.
> 
> 

## <a name="reports-and-log-collection"></a>Jelentések és naplógyűjtést
CDN-tevékenységek adatait hello peremhálózati teljesítmény Analytics modul kell gyűjteni előtt hozhat létre a jelentések rajta. Az adatgyűjtési folyamat akkor fordul elő, ha naponta, és hozzá van rendelve hello tevékenység hello előző nap során történt. Ez azt jelenti, hogy egy jelentés statisztika hello nap statisztikák minta képviselő hello időben lett feldolgozva, azonban nem feltétlenül tartalmaznak hello az adatok teljes készletének hello aktuális nap. hello elsődleges ezekre a jelentésekre funkciója tooassess teljesítményét. Ezek nem használható számlázási célokra vagy a pontos numerikus statisztika.

> [!NOTE]
> hello nyers adatok, amelyből peremhálózati teljesítmény elemzési jelentések létrehozása legalább 90 napig érhető el.
> 
> 

## <a name="dashboard"></a>Irányítópult
hello peremhálózati teljesítmény elemzések irányítópultján nyomon követi az aktuális és korábbi CDN forgalmat egy diagram és a statisztika keresztül. Az irányítópult toodetect legutóbbi és a hosszú távú trendeket a CDN-adatforgalom hello teljesítményre használhat a fiókhoz.

Ez az irányítópult áll:

* Interaktív diagram, amely lehetővé teszi a hello képi megjelenítés metrikáit és a trendeket.
* Az idősor, amely egy logika annak a hosszú távú biztosít metrikáit és a trendeket.
* Alapvető metrikákat, és hogyan statisztikai adatok a CDN hálózati növeli a webhelyek forgalmát mért összesített teljesítményt, használatát és hatékonyságát.

### <a name="accessing-hello-edge-performance-dashboard"></a>Hello peremhálózati teljesítmény irányítópult elérése
1. A CDN-profil panelje hello, kattintson a hello **kezelése** gombra.
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    Megnyílik a hello CDN felügyeleti portálon.
2. Hello az egérmutatót **Analytics** fülre, majd az egérmutatót hello **peremhálózati teljesítési Analytics** menü.  Kattintson a **irányítópult**.
   
    hello peremhálózati csomópont elemzések irányítópultján jelenik meg.

### <a name="chart"></a>A diagram
az irányítópulton hello egy diagram, amely egy metrika hello keresztül nyomon követi a közvetlenül a alatt megjelenő hello ütemterv kiválasztott időszakra vonatkozóan.  Amely másolatot toohello grafikonokon utolsó két éves CDN tevékenység ütemterv közvetlenül hello diagram alatt jelenik meg.

#### <a name="using-hello-chart"></a>Hello diagram használatával
* Alapértelmezés szerint hello gyorsítótár hatékonyságát aránya az elmúlt 30 napban hello forrásadatok lesz.
* Ez a diagram adatok alapon naponta jön létre.
* A hello vonaldiagram naponta fölé határidő jelzi hello metrika dátum- és hello érték.
* Kattintson a hétvégéket kiemelése tootoggle könnyű szürke függőleges sávok, amelyek megfelelnek a hétvégéket hello diagram alakzatot az átfedés. Átmeneti területre az ilyen típusú esetén hasznos forgalmi minták azonosítása hétvégéket keresztül.
* Kattintson az előző hello átfedés nézet egy év ezelőtt tootoggle évi tevékenység hello keresztül azonos időszak hello diagram-kiszolgálóra. Az ilyen típusú összehasonlító hosszú távú CDN használati minták betekintést nyújt. hello jobb felső sarkában hello diagram jelmagyarázat, amely jelzi az egyes vonaldiagram hello szín kódját tartalmazza.

#### <a name="updating-hello-chart"></a>Hello diagram frissítése
* Időtartomány: Hello következő hajtsa végre:
  * Válassza ki a kívánt régiót hello hello idővonalon. hello diagram, amely megfelel a kijelölt időszakban toohello adatokkal frissül.
  * Kattintson duplán az összes rendelkezésre álló előzményadatokat mentése tooa legfeljebb két évre hello diagram toodisplay.
* Metrika: Kattintson a Tovább szükség toohello metrika megjelenő hello diagram ikon. hello diagram és hello ütemterv hello megfelelő mérőszám adatokkal frissülnek.

### <a name="key-metrics-and-statistics"></a>Alapvető metrikákat és statisztika
#### <a name="efficiency-metrics"></a>Hatékonyságát metrikák
hello metrikákat célja toosee e növelhető a gyorsítótár hatékonyságát. gyorsítótár hatékonysága származó hello fő előnyei a következők:

* Csökkentett terhelés hello forrás kiszolgálón, amely azt eredményezheti, hogy:
  * Web server jobb teljesítményt.
  * Csökkenti a működési költségeket.
* Továbbfejlesztett adatok kézbesítési gyorsítás, mivel további kérelmeket szolgáltató hello CDN-ről.

| Mező | Leírás |
| --- | --- |
| Gyorsítótár hatékonyságát |Azt jelzi, hogy hello százalékos az átvitt adatok, amelyek a gyorsítótárból állítása és kiszolgálása között. A metrika intézkedéseket, ha egy gyorsítótárazott verziója hello a kért tartalom kiszolgálásának közvetlenül a hello CDN (peremhálózati kiszolgálóinak) toorequesters (pl. webböngésző) |
| Találati arány |Jelzi, hogy volt kiszolgálása a gyorsítótárból kérelem hello arányát. A metrika intézkedéseket, ha egy gyorsítótárazott verziója hello a kért tartalom kiszolgálásának közvetlenül a hello CDN (peremhálózati kiszolgálóinak) toorequesters (pl. webböngésző). |
| a távoli memória - nincs gyorsítótár Config % |Azt jelzi, hogy a forrás kiszolgálók toohello CDN (peremhálózati kiszolgálóinak), amelyek nem kerülnek a gyorsítótárba hello áthidaló gyorsítótár funkció (HTTP szabálymotor) eredményeként állítása és kiszolgálása között forgalmat hello aránya. |
| %-a távoli memória - gyorsítótárban lejárt |Jelzi a hello arányát az, hogy a forrás kiszolgálók toohello CDN (peremhálózati kiszolgálóinak) állítása és kiszolgálása között forgalmat miatt elavult tartalom ismételt érvényesítése. |

#### <a name="usage-metrics"></a>Használati metrikák
hello metrikákat célja a következő költség-kivágáshoz intézkedések hello tooprovide betekintést:

* Minimalizálja a működési költségeket a hello CDN.
* Gyorsítótár hatékonysága és a tömörítés révén CDN költségek csökkentését.

> [!NOTE]
> Forgalom mennyiségi számát és a százalékos arányt tesz lehetővé a számítások lett megadva, és csak lehet, hogy hello teljes forgalom egy része megjelenítése nagy mennyiségű ügyfél forgalom jelölik.
> 
> 

| Mező | Leírás |
| --- | --- |
| Kimenő Ave bájtok |Azt jelzi, hogy az egyes kérelmek helyről hello CDN (peremhálózati kiszolgálóinak) toohello kérelmező (pl. webböngésző) átvitt bájtok átlagos számát hello. |
| Egyetlen gyorsítótár Config bájt sebessége |Azt jelzi, hogy hello CDN (peremhálózati kiszolgálóinak) toohello kérelmező (pl. webböngésző), amelyek nem kerülnek a gyorsítótárba miatt toohello áthidaló gyorsítótár szolgáltatás származó forgalmat hello százalékát. |
| Tömörített bájt arány |Azt jelzi, hello CDN (peremhálózati kiszolgálóinak) toorequesters (pl. webböngésző) által küldött forgalmat hello százaléka tömörített formátumban. |
| Kimenő bájtok |Azt jelzi, hogy hello adatmennyiség bájtban megadva, a hello CDN (peremhálózati kiszolgálóinak) toohello kérelmező (pl. webböngésző) kapott. |
| A bájtok |Azt jelzi, hogy hello adatmennyiség bájtban (pl. webböngésző) engedélyezését toohello CDN (peremhálózati kiszolgálóinak) által küldött. |
| Bájtok távoli |Azt jelzi, hogy hello adatmennyiség bájtban megadva, a CDN és a felhasználói származási kiszolgálók toohello CDN (peremhálózati kiszolgálóinak) által küldött. |

#### <a name="performance-metrics"></a>Teljesítménymértékeket
hello metrikákat célja tootrack az forgalom CDN összteljesítményre.

| Mező | Leírás |
| --- | --- |
| Átviteli sebesség |Azt jelzi, hogy hello átlagos száma, amelynél tartalom hello CDN tooa kérelmező lett továbbítva. |
| Időtartam |Azt jelzi, hello átlagos idő, ezredmásodpercben tartott toodeliver egy eszköz tooa kérelmező (pl. webböngésző). |
| Tömörített kérelmek gyakorisága |Azt jelzi, hello százalékos aránya a hello CDN (peremhálózati kiszolgálóinak) toohello kérelmező (pl. webböngésző) kapott találatok tömörített formátumban. |
| 4xx Hibaarány |Azt jelzi, hogy által generált 4xx állapotkódot találatok hello százalékát. |
| 5XX Hibaarány |Azt jelzi, hogy által generált 5xx állapotkódot találatok hello százalékát. |
| A találatok |A CDN-tartalom kérelmek hello számát jelöli. |

#### <a name="secure-traffic-metrics"></a>Biztonságos forgalom metrikák
hello metrikákat célja tootrack CDN teljesítmény a HTTPS-forgalmat.

| Mező | Leírás |
| --- | --- |
| Biztonságos gyorsítótár hatékonyságát |Azt jelzi, hogy a HTTPS-kéréseket volt kiszolgálása a gyorsítótárból átvitt adatok hello százalékát. Ez a metrika méri, amikor hello gyorsítótárazott verzióját a kért tartalom kiszolgálásának hello CDN (peremhálózati kiszolgálóinak) toorequesters (pl. webböngésző) közvetlenül a HTTPS-KAPCSOLATON keresztül. |
| Biztonságos átviteli sebessége |Azt jelzi, ahol tartalom hello CDN (peremhálózati kiszolgálóinak) toorequesters (például a webkiszolgálók) lett továbbítva hello átlagos HTTPS-KAPCSOLATON keresztül. |
| Biztonságos végrehajtásának átlagos időtartama |Azt jelzi, hello átlagos idő, ezredmásodpercben tartott toodeliver egy eszköz tooa kérelmező (pl. webböngésző) HTTPS-KAPCSOLATON keresztül. |
| Biztonságos találatok |CDN-tartalom HTTPS-kéréseket hello számát jelzi. |
| Kimenő biztonságos bájtok |Azt jelzi, hogy HTTPS-forgalmat a hello CDN (peremhálózati kiszolgálóinak) toohello kérelmező (pl. webböngésző) kapott bájtokban hello mennyiségét. |

## <a name="reports"></a>Jelentések
Ez a modul az egyes jelentések tartalmaz egy diagram és a sávszélesség és a forgalom használati metrikák különböző típusú statisztikák (pl. HTTP-állapotkódok, gyorsítótár állapotkódokat alállapotkódok kérés URL-címe, stb.). Lehet, hogy hogyan tartalom kiszolgált tooyour ügyfelek és toofine-hangolási CDN viselkedésének tooimprove adatok szállítási teljesítménye mélyebben használt toodelve.

### <a name="accessing-hello-edge-performance-reports"></a>Hello peremhálózati teljesítmény jelentések használata
1. A CDN-profil panelje hello, kattintson a hello **kezelése** gombra.
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    Megnyílik a hello CDN felügyeleti portálon.
2. Hello az egérmutatót **Analytics** fülre, majd az egérmutatót hello **peremhálózati teljesítési Analytics** menü.  Kattintson a **HTTP nagy objektum**.
   
    hello peremhálózati csomópont elemzési jelentések képernyő jelenik meg.

| Jelentés | Leírás |
| --- | --- |
| Napi összegzése |Lehetővé teszi tooview napi forgalom trendek megadott idő alatt. Ez a diagram minden sávján egy adott dátumot jelöli. hello mérete hello sáv jelzi hello által történt a dátumnak a találatok száma. |
| Óránkénti összegzése |Lehetővé teszi tooview óránkénti forgalom trendek megadott idő alatt. Ez a diagram minden sávján egy adott napon egy órát jelöli. hello mérete hello sáv jelzi, hogy az óra során bekövetkezett találatok száma hello. |
| Protokollok |Hello HTTP és HTTPS protokollok közötti forgalom hello bontását jeleníti meg. Fánk diagram azt jelzi, hogy által az egyes protokoll történt találatok hello százalékát. |
| HTTP-metódus |Lehetővé teszi egy gyors logika, amely HTTP-metódus vannak tooget éppen használt toorequest adatait. Hello leggyakrabban használt HTTP-kérelem metódus általában GET, HEAD és POST. Fánk diagram azt jelzi, hogy által az egyes HTTP-kérési metódust történt találatok hello százalékát. |
| URL-címek |Tartalmazza a hello felső 10 kért URL-címek megjelenítő grafikon. A sáv jelenik meg minden egyes URL-CÍMÉT. hello sáv hello magassága azt jelzi, hogy által adott URL generált keresztül hello időtartam hány találatok hello jelentés tárgyát. Hello top 100 statisztikáit. a kért URL-címek közvetlenül a ehhez a diagramhoz alább láthatók. |
| CNAME |Tartalmaz egy jelentés időtartamának hello keresztül hello felső 10 használt CNAME toorequest eszközök megjelenítő grafikon. A kért hello top 100 statisztikája CNAME közvetlenül a ehhez a diagramhoz alább láthatók. |
| Források |Tartalmaz egy grafikonon, amely hello első 10 CDN vagy ügyfél eredeti kiszolgálók, amelyből eszközök kért egy meghatározott időtartamra vonatkozóan jeleníti meg. A kért hello top 100 statisztikája CDN vagy ügyfél eredeti kiszolgálók közvetlenül a ehhez a diagramhoz alább láthatók. Ügyfél eredeti kiszolgálók meghatározott hello könyvtárnév beállítás hello neve azonosítja. |
| Földrajzi POP |Mutatja, hogy mekkora a forgalom alatt keresztül történik egy adott pont nyújtó jelenléti (POP). hello rövidítését jelöli a CDN hálózat POP. |
| Ügyfelek |Tartalmaz egy grafikonon, amely hello felső 10 ügyfél által kért eszközök egy meghatározott időtartamra vonatkozóan jeleníti meg. Ez a jelentés hello céljából, az összes kérelmekkel hello azonos IP-cím között útmutatóul szolgálnak a hello toobe ugyanazt az ügyfelet. Statisztika hello top 100 ügyfelek közvetlenül ehhez a diagramhoz alatt jelennek meg. Ez a jelentés az letöltési tevékenység minták a felső ügyfelek meghatározására szolgál. |
| Gyorsítótár-állapotok |Részletes információkat a gyorsítótár-viselkedést felfedheti javítására megközelítés biztosítja hello általános végfelhasználói élmény. Hello leggyorsabb teljesítmény találatot eredményező gyorsítótárbeli kereséseinek származik, mert adatok kézbesítési sebességű minimalizálja a gyorsítótárbeli és lejárt találatok is optimalizálhatja. |
| Nincs részletek |Tartalmazza a diagramját, amelyek hello felső 10 URL-címek eszközök, amelynek gyorsítótár tartalom frissessége nincs bejelölve egy meghatározott időtartamra vonatkozóan jeleníti meg. Statisztika hello top 100 URL-címek esetén az ilyen típusú eszközök közvetlenül ehhez a diagramhoz alatt jelennek meg. |
| CONFIG_NOCACHE részletei |Tartalmazza az eszközök, amelyek toohello felhasználói CDN konfigurálása miatt nem lettek gyorsítótárazva hello első 10 URL-címek megjelenítő grafikon. Az ilyen típusú eszközök közvetlenül forráskiszolgálóról hello volt kiszolgált. Statisztika hello top 100 URL-címek esetén az ilyen típusú eszközök közvetlenül ehhez a diagramhoz alatt jelennek meg. |
| UNCACHEABLE részletei |Tartalmazza a felső 10 URL hello toorequest fejléc adatok miatt nem gyorsítótárazható eszközök megjelenítő grafikon. Statisztika hello top 100 URL-címek esetén az ilyen típusú eszközök közvetlenül ehhez a diagramhoz alatt jelennek meg. |
| TCP_HIT részletei |Tartalmazza a felső 10 URL hello gyorsítótárból azonnal szolgáltatott eszközök megjelenítő grafikon. Statisztika hello top 100 URL-címek esetén az ilyen típusú eszközök közvetlenül ehhez a diagramhoz alatt jelennek meg. |
| TCP_MISS részletei |Tartalmazza a felső 10 URL hello TCP_MISS gyorsítótár állapottal rendelkező eszközök megjelenítő grafikon. Statisztika hello top 100 URL-címek esetén az ilyen típusú eszközök közvetlenül ehhez a diagramhoz alatt jelennek meg. |
| TCP_EXPIRED_HIT részletei |Tartalmazza a felső 10 URL hello elavult eszközök hello POP-ről volt szolgáltatott megjelenítő grafikon. Statisztika hello top 100 URL-címek esetén az ilyen típusú eszközök közvetlenül ehhez a diagramhoz alatt jelennek meg. |
| TCP_EXPIRED_MISS részletei |Tartalmazza a felső 10 URL hello elavult eszközöket, amelynek új verzió kellett hello forráskiszolgálóról beolvasott toobe megjelenítő grafikon. Statisztika hello top 100 URL-címek esetén az ilyen típusú eszközök közvetlenül ehhez a diagramhoz alatt jelennek meg. |
| TCP_CLIENT_REFRESH_MISS részletei |A sávdiagram az eszközök lekérése egy forráskiszolgálóról miatt hello ügyfél tooa no-cache kérése hello felső 10 URL megjelenítő tartalmazza. Statisztika hello top 100 URL-címek esetén az ilyen típusú kérelmek közvetlenül a diagram alatt jelennek meg. |
| Ügyfél kéréstípusok |A HTTP-ügyfelek (például böngészők) által végrehajtott kérelmek hello típusát jelzi. Ez a jelentés tartalmaz egy logika biztosít toohow kéréseket kezeli fánk diagram. Az egyes kérelmek sávszélesség és a forgalmi információk hello diagram alatt jelennek meg. |
| Felhasználói ügynök |Tartalmaz egy oszlopdiagramot hello felső 10 felhasználói ügynökök toorequest keresztül a CDN a tartalom megjelenítése. A felhasználói ügynök általában egy webes böngésző, a media player vagy a mobiltelefon böngészővel. Hello top 100 felhasználói ügynökök statisztikája közvetlenül a diagram alatt jelennek meg. |
| Hivatkozó kérelmei |A CDN keresztül elért hello felső 10 hivatkozó kérelmei toocontent megjelenítése oszlopdiagramon tartalmazza. A hivatkozó általában hello weblap vagy az erőforrás, amely a tartalom tooyour hello URL-CÍMÉT. Részletes információk hello top 100 hivatkozó kérelmei előírt hello graph alatt. |
| A tömörítési típusok |E tömörített azokat a peremhálózati kiszolgáló által kért eszközök megszakad fánk diagram tartalmazza. a tömörített eszközök hello százalékos használt tömörítési típust hello oszlanak meg. Részletes információk hello graph alább az egyes tömörítési típus és állapotát. |
| Fájltípus |Egy oszlopdiagramot hello felső 10 fájltípusokat keresztül a CDN a fiókjához kért megjelenítő tartalmazza. Ez a jelentés hello céljából, fájl van definiálva típus hello eszköz fájlnévkiterjesztés és Internet az adathordozó típusa (pl. .html \[text/html\], .htm \[text/html\], .aspx \[text/html\]stb.). Részletes információkat biztosít a hello top 100 hello graph alább. |
| Egyedi fájlok |Diagramját, amelyek egy adott napon egy adott időtartamra vonatkozóan kért egyedi eszközök teljes száma hello tevékenységtérkép tartalmazza. |
| Jogkivonat hitelesítési összegzése |A tortadiagram, amely gyors áttekintést nyújt a kért eszközök jogkivonat-alapú hitelesítés által védett e tartalmazza. A megkísérelt hitelesítés toohello eredményei alapján hello diagram védett eszközök jelennek meg. |
| Jogkivonat hitelesítési megtagadása részletei |Tartalmaz egy oszlopdiagramot, amely lehetővé teszi tooview hello felső 10 kérelmek volt tooToken-alapú hitelesítés miatt megtagadva. |
| A HTTP válaszkódot. |Bontásban hello HTTP-állapotkódok (pl. 200 OK, 403 Tiltott, 404 nem található, stb.), amely volt tooyour HTTP ügyfelek kézbesítette peremhálózati kiszolgálókról. A tortadiagram lehetővé teszi a tooquickly értékeléséhez, hogyan lettek kiszolgált az eszközök. Részletes statisztikai adatok biztosított minden válaszkód hello graph alatt. |
| 404-es hibák |Tartalmaz egy oszlopdiagramot, amely lehetővé teszi tooview hello felső 10 kérelmek 404-es nem található válaszkódot eredményezett. |
| 403-as hiba |Tartalmaz egy oszlopdiagramot, amely lehetővé teszi tooview hello felső 10 kérelmek egy tiltott 403-as válaszkódot eredményezett. A tiltott 403-as válaszkódot megtagadta a kérelmet egy ügyfél forrás vagy a POP-ra a peremhálózati kiszolgáló következik be. |
| 4xx hibák |Egy oszlopdiagramot, amely lehetővé teszi tooview hello felső 10 kérelmek hello 400 közé válaszkódot eredményező tartalmazza. Ez a jelentés nem tartoznak azok a 403-as nem található, vagy tiltott 404-es válaszkódot. Általában 4xx válaszkód következik be, amikor egy kérelem egy ügyfél hiba miatt megtagadva. |
| 504-es számú hiba |Egy oszlopdiagramot, amely lehetővé teszi tooview hello felső 10 kérelmek átjáró időtúllépése 504-es számú válaszkódot eredményező tartalmazza. Átjáró időtúllépése 504-es számú válaszkódot időtúllépés egy HTTP-proxy egy másik kiszolgálóval toocommunicate közben következik be. A CDN hello esetben 504-es számú átjáró időtúllépése válaszkód általában előtt következik be egy biztonsági kiszolgálót egy ügyfél eredeti kiszolgálóra nem tooestablish kommunikál. |
| 502 hibák |Egy oszlopdiagramot, amely lehetővé teszi tooview hello felső 10 kérelmek hibás átjáró 502 válaszkódot eredményező tartalmazza. 502 Hibás átjáró válaszkód egy kiszolgáló és a HTTP-proxy egy HTTP protokoll hiba esetén következik be. A CDN hello esetben hibás átjáró 502 válaszkód általában akkor fordul elő, amikor egy ügyfél eredeti kiszolgálóra adja vissza egy érvénytelen válasz tooan biztonsági kiszolgálót. A válasz érvénytelen, ha nem lehetett elemezni, vagy nem teljes. |
| 5XX hibák |Egy oszlopdiagramot, amely lehetővé teszi tooview hello felső 10 kérelmek hello 500 közé válaszkódot eredményező tartalmazza.  Ez a jelentés nem 502 Hibás átjáró és az átjáró időtúllépése 504-es számú válaszkódot. |

## <a name="see-also"></a>Lásd még:
* [Az Azure CDN áttekintése](cdn-overview.md)
* [A Microsoft Azure CDN valós idejű statisztikák](cdn-real-time-stats.md)
* [Alapértelmezett HTTP működés használata hello szabályok felülbírálása](cdn-rules-engine.md)
* [Speciális HTTP-jelentések](cdn-advanced-http-reports.md)

