---
title: az Azure-on aaaIntroduction toomicroservices |} Microsoft Docs
description: "Miért egy mikroszolgáltatások módszert használja a felhőalapú alkalmazások számára fontos a modern alkalmazások fejlesztését, és hogyan Azure Service Fabric biztosít egy platform tooachieve ez áttekintése."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: fae2be85-0ab4-4cd3-9d1f-e0d95fe1959b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: msfussell
ms.openlocfilehash: b11920b9105e7575390e8fcf0d1ef6ab3c632978
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="why-a-microservices-approach-toobuilding-applications"></a>Miért egy mikroszolgáltatások közelítse toobuilding alkalmazások?
Szoftverfejlesztők, mert jelenleg nincs újdonságunk a ahogyan szerintünk kapcsolatos faktoring egy alkalmazás összetevő részre. Hello központi paradigma objektum tájolás, a szoftver absztrakt entitással egészült ki, és a componentization. Napjainkban a factorization általában osztályok és a megosztott szalagtárakkal és technológiai réteg kapcsolódási tootake hello formában. Általában a rétegzett megközelítés használatban van egy háttér-tároló, középszintű üzleti logika és előtér felhasználói felület (UI). Mi *rendelkezik* megváltoztatni hello elmúlt néhány évben, hogy azt, mint a fejlesztők számára, létre elosztott alkalmazásokat, amelyek hello felhőalapú és hello üzleti vezérlik.

hello változó üzleti igényeinek a következők:

* A szolgáltatás épül, és a skála tooreach ügyfelek új földrajzi régióban (például) működik.
* Funkciók és képességek toobe képes toorespond toocustomer iránti igények kielégítése érdekében gyors módon gyorsabb kézbesítését.
* Továbbfejlesztett Erőforrás kihasználtsága tooreduce költségek.

Ezek az üzleti igények is érinthetik *hogyan* azt olyan alkalmazásokat hozhat létre.

További információ az Azure toomicroservices hello megközelítés [Mikroszolgáltatások: egy alkalmazás fordulat hello felhő technológiával](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservice-design-approach"></a>Mikroszolgáltatási tervezett módszert alkalmaz és egységes
Minden alkalmazás verzióinformációk. Sikeres alkalmazásokat fejleszteni őket hasznos toopeople. Sikertelen kérelmek nem fejlődnek, és végül elavultak. hello kérdés válik: mennyi ismeri a követelményekkel kapcsolatos ma, és mi ezek lesz a jövőbeni hello? Tételezzük fel például, hogy egy-egy részleg jelentéskészítési alkalmazást fejleszt. Biztos, hogy a hello alkalmazás a vállalat hello hatókörén belül maradnak, és hello jelentések rövid élettartamú. A választott módszer eltér, tegyük fel például, felépítése egy szolgáltatás, amely biztosítja a felhasználók millióinak videó tartalom tootens. 

Egyes esetekben első valami hello ajtó, mint a koncepció igazolása hello vezetői tényező, amíg hello alkalmazás később újratervezett tudja. Nincs kevés pontot túlzott mérnöki valami soha nem leggyakrabban használt. Az hello szokásos mérnöki kompromisszum. A hello ugyanakkor, ha a vállalatok szolgáltatással kapcsolatban épület, a hello felhő, hello elvárása növekedés és használati. hello probléma, hogy a növekedési és a skála előre nem látható. Szeretnénk tudni tooprototype toobe gyorsan is, hogy az egy elérési utat toodeal jövőbeli sikerrel dolgozunk tudatában. Ez a hello lean indítási megközelítés: build, mérik, ismerje meg, és többször.

Során hello ügyfél-kiszolgáló éra azt a többrétegű alkalmazások létrehozásához az egyes rétegek egyes technológiák használatával toofocus arra irányult. hello kifejezés *egységes* alkalmazás mutatkozott az ezen módszerek. hello felületek arra irányult toobe hello fizetősökbe, és nagyobb mértékben összekapcsolt tervezési összetevők belül minden egyes réteg között használt. A fejlesztők és a szalagtárak össze és néhány futtatható fájljainak és DLL-ek az egymáshoz kapcsolódó osztályok beleszámítja. 

Nincsenek előnyöket toosuch egy egységes tervezett módszert alkalmaz. Gyakran egyszerűbb toodesign, és nem rendelkezik összetevők közötti gyorsabb hívások, mert az adott hívások gyakran folyamatközi kommunikáció (IPC) alatt. Emellett egy termék, amely azt toobe további személyek-erőforrások hatékony mindenki teszteli. hello hátránya, hogy egy rétegzett rétegek közötti szoros kapcsoló van, de egyes összetevőket nem lehet méretezni. Ha tooperform javítások és a frissítések, mások toowait rendelkezik toofinish vizsgálati. Agilis nehezebb toobe.

Mikroszolgáltatások cím ezek downsides és több üzleti követelmények megelőző hello szorosan igazodnak, de előnyei és a források is rendelkeznek. a következők mikroszolgáltatások hello előnyei, hogy minden egyes általában foglalja az egyszerűbb üzleti működését, ami felfelé vagy lefelé teszt méretezhető, telepítéséhez és felügyeletéhez egymástól függetlenül. Egyik fontos mikroszolgáltatási megközelítés előnye, hogy csoportok alakítják több mint üzleti forgatókönyvek szerint technológia, amely rétegzett megközelítés hello javasolja. A gyakorlatban kisebb egy ügyfél alapuló mikroszolgáltatási fejlesztése és bármely választ technológiák használata. 

Más szóval hello szervezet toostandardize műszaki toomaintain mikroszolgáltatási alkalmazások nem szükséges. Az egyes csoportok, hogy saját szolgáltatások teheti meg szabálykészletében mi számukra team szakértői alapján, vagy mi az leginkább megfelelő toosolve hello probléma. A gyakorlatban ajánlott technológiák, például egy adott NoSQL tárolja, vagy a webes alkalmazás-keretrendszer, érdemes.

hello hátránya az mikroszolgáltatások érkezik kezelése külön entitásokat és kezelésével összetettebb központi telepítéseket és versioning nőtt hello száma. Hello mikroszolgáltatások közötti hálózati forgalom növeli a valamint hello megfelelő hálózati késések fordulnak elő. Chatty, részletes szolgáltatásokat nagyszámú egy receptet a teljesítmény-nightmare. Eszközök toohelp nélkül megtekintheti ezeket a függőségeket, a merevlemez túl "Lásd a" hello teljes rendszer. 

Szabványok ellenőrizze hello mikroszolgáltatási megközelítés működnek-e hogyan toocommunicate, és csak a hello műveleteket hibatűrő szükséges szolgáltatás, nem pedig kemény szerződések megállapodjanak. Ezek a szerződések előre hello megtervezésére, fontos toodefine mert adatszolgáltatások frissítése egymástól függetlenül is. Egy másik leírás szereznek a mikroszolgáltatások megközelítés való érték "minden részletre kiterjedő szolgáltatásorientált architektúra (SOA)."

***A legegyszerűbb hello mikroszolgáltatások létrehozására tervezett módszert alkalmaz arra készül, egy leválasztott összevonási szolgáltatások, a független módosítások tooeach, és elfogadott szabványnak kommunikációhoz.***

Mivel további felhőalapú alkalmazások előállítása, személyek felderítése, amely a felbontás ellen hello általános alkalmazás független, forgatókönyv-arra irányul, hogy a szolgáltatás az jobban hosszú távú megközelítés.

## <a name="comparison-between-application-development-approaches"></a>Alkalmazás-fejlesztői módszerek összehasonlítása
![A Service Fabric platform alkalmazásfejlesztés][Image1]

1) Egy egységes alkalmazást tartományspecifikus funkciót tartalmaz, és általában osztóval működési rétegek, például webes, üzleti és adat.

2) Több kiszolgáló virtuális gépek klónozásával méretezni egységes app/tárolók.

3) Egy mikroszolgáltatási alkalmazás elválasztja kisebb szolgáltatások külön funkciókat.

4) hello mikroszolgáltatások megközelítés méretezik által végzett központi telepítése minden egyes szolgáltatás egymástól függetlenül, ezek a szolgáltatások példánya létre keresztül kiszolgálók vagy virtuális gépek/tárolók.

Az egy mikroszolgáltatási tervezése megoldás, nem az összes projektet, melyekben egy panacea, de jobban igazodnak a korábban leírt hello üzleti célkitűzéseiket. Egységes módszer kezdve elfogadható, ha tudja, hogy hello lehetőség toorework hello kódot később mikroszolgáltatások kialakítást lehet. Gyakrabban egységes alkalmazás kezdődnie, és lassan bontsa szakaszokban hello funkcionális területet kell toobe, méretezhető, vagy a nagy kezdve.

toosummarize, hello mikroszolgáltatási megoldás, toocompose az alkalmazás a sok kisméretű szolgáltatások. hello szolgáltatások a gépet fürtön belül üzembe helyezett tárolók futnak. Kisebb csoportok egy szolgáltatás, amely egy olyan forgatókönyvet fejlesztése és egymástól függetlenül, verzió tesztelése, üzembe helyezését, és minden egyes szolgáltatás méretezését, úgy, hogy az alkalmazás teljes hello fejlődnek is.

## <a name="what-is-a-microservice"></a>Mi az a mikroszolgáltatási?
Nincsenek mikroszolgáltatások különböző meghatározása. Ha hello interneten keres, látni fogja, amelyek a saját nézőpontok és a definíciók biztosítanak számos hasznos források. Azonban a következő mikroszolgáltatások jellemzői hello többsége széles körben megegyezésre:

* Egy ügyfél vagy üzleti forgatókönyv foglalják magukban. Mi az a hello problémamegoldó?
* Egy kis mérnöki csapata által fejlesztett.
* Bármely programozási nyelv, és tetszőleges-keretrendszerével.
* (Opcionális) állapot, amelyek mindkettő egymástól függetlenül rendszerverzióval ellátott, telepített, és a kiterjesztett és kód állnak.
* Jól meghatározott felületet és protokollokon keresztül kommunikál a többi mikroszolgáltatások létrehozására.
* A nevek egyediek (URL) használt tooresolve azok helyétől.
* Következetes és hibák hello jelenléte elérhető marad.

A következő jellemzőkkel foglalhatja össze:

***Mikroszolgáltatási alkalmazások álló kis, egymástól függetlenül rendszerverzióval ellátott és méretezhető ügyfél tárgyalt szolgáltatás, amely a jól meghatározott felülethez szabványos protokollokon keresztül kommunikálnak egymással.***

Azt a kezelt hello először között eltelt hello szakasz megelőző, és bontsa ki a és tisztázása hello mások.

### <a name="written-in-any-programming-language-and-use-any-framework"></a>Bármely programozási nyelv, és bármely keretrendszer használata
A fejlesztők azt kell megjelennie szabad toochoose egy nyelvet vagy keretrendszert, amely azt szeretnénk, attól függően, hogy a képességek vagy hello szolgáltatás hello igényeinek. Az egyes szolgáltatások hello teljesítménybeli előnyökben mindenekelőtt más c++ előfordulhat, hogy érték. Az egyéb szolgáltatások hello felügyelt könnyű fejlesztés jegyében készült C# vagy Java legfontosabb lehet. Bizonyos esetekben szükség lehet egy adott partner könyvtár toouse tárolási technológiát, vagy azt jelenti, hogy érdekében, hogy csökkenjen hello szolgáltatás tooclients.

Miután kiválasztotta a technológia, sikerül toohello működési vagy a életciklusának kezelésére és a méretezésről hello szolgáltatást.

### <a name="allows-code-and-state-toobe-independently-versioned-deployed-and-scaled"></a>Lehetővé teszi a kódot és az állapot toobe egymástól függetlenül rendszerverzióval ellátott, telepített és méretezhető
Azonban úgy dönt, toowrite a mikroszolgáltatások, hello kódból és opcionálisan hello állapot kell egymástól függetlenül telepítése, frissítése és méretezése. Ez az ténylegesen egyik hello nehezebben problémák toosolve, mert technológiák tooyour választott le ismét. A méretezés, megértése, hogyan toopartition (és horizontális skálázás) egyaránt hello kódot és az állapot, kihívást. Hello kódot és az állapot különálló technológia, amely közös ma, használja a mikroszolgáltatási hello telepítési parancsfájljainak kell toobe képes toocope a skálázás két. Ez a helyzet is kapcsolatos agilitást és a rugalmasságot, néhány hello mikroszolgáltatások tooupgrade anélkül frissítheti az összes egyszerre.

Adatszolgáltató toohello egységes mikroszolgáltatási megközelítést és egy kis ideig, hello alábbi ábrán látható hello különbségek hello megközelítés toostoring állapotban.

#### <a name="state-storage-between-application-styles"></a>Állapot tárolási alkalmazás stílusok között
![A Service Fabric platform állapot tárolásához][Image2]

***hello egységes módszer hello bal oldali egy önálló adatbázis és az egyes technológiák álló rendelkezik.***

***hello mikroszolgáltatások megközelítés a jobb oldali hello egy grafikonon összekapcsolt mikroszolgáltatások, ahol a állapot általában hatókörön belüli toohello mikroszolgáltatási és a különböző technológiákkal, azzal.***

Egységes megközelítéssel, általában hello alkalmazás egyetlen adatbázist használ. hello előnye, hogy a rendszer, így könnyen toodeploy egyetlen helyen. Minden egyes összetevő rendelkezhet egy egyetlen tábla toostore állapotában. Szükséges toostrictly külön állapotát, amely kihívást jelent. Mindenképpen temptations tooadd új oszlop tooan meglévő felhasználói tábla, tegye a csatlakozzon a táblák között, és függőségek hello tárolási réteg létrehozása. Miután ez akkor fordul elő, az egyes összetevők nem lehet méretezni. 

Hello mikroszolgáltatások módszert használja minden egyes szolgáltatás kezeli, és eltárolja a saját állapotot. Minden szolgáltatás kód és állapota is együtt toomeet hello iránti igények kielégítése érdekében hello szolgáltatás skálázás felelős. A hátránya, hogy ha egy szükséges toocreate nézetek vagy lekérdezéseket készítenek, az alkalmazásadatok, kell tooquery között különböző állapotot tárolja. Általában ez megoldódott azzal, hogy egy külön mikroszolgáltatási, amelyek között mikroszolgáltatások gyűjteménye képet alkot. Ha tooperform hello adatok több rögtönzött lekérdezései, minden mikroszolgáltatási vegye figyelembe az adatok tooa vámraktározási adatszolgáltatás offline elemzéséhez írása.

Versioning az adott toohello telepített verziója egy mikroszolgáltatási úgy, hogy több, különböző verziót telepíteni, és futtassa egymás mellett. Versioning címek hello forgatókönyvek, amennyiben egy újabb verziója egy mikroszolgáltatási sikertelen frissítés során, és hátsó tooan tooroll korábbi verzióját. hello más forgatókönyv verziószámozásának hajt végre A/B-stílusú tesztelése, ahol a különböző felhasználók tapasztalható hello szolgáltatás különböző verzióit. Közös tooupgrade egy mikroszolgáltatási ügyfelek tootest új funkciók adott csoportja számára például, mielőtt további széles körben. Után mikroszolgáltatások életciklusának kezelését Ez most számos lehetőséget kínál, toocommunication közöttük.

### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>Jól meghatározott felületet és protokollokon keresztül más mikroszolgáltatások kommunikációja
Ez a témakör figyelmet kevés itt, mert kapcsolatos szolgáltatásorientált architektúra, amely közzé lett téve keresztül hello elmúlt 10 évben kiterjedt irodalom szokások ismerteti. Általában szolgáltatások közötti kommunikáció használ a többi megközelítés HTTP és a TCP protokollt és XML- vagy JSON hello szerializálási formátum szerint. Felület szempontból akkor kapcsolatos munkastílus hello webes tervezett módszert alkalmaz. De semmi nem leáll, a bináris protokollok vagy a saját adatok formátum használatával. Készíteni személyek toohave a mikroszolgáltatások használata, ha nyilvánosan elérhetővé nehezebb időpontot.

### <a name="has-a-unique-name-url-used-tooresolve-its-location"></a>Rendelkezik egy egyedi nevet (URL) használt tooresolve helyére
Ne feledje, hogy hogyan azt tartsa arról, hogy hello mikroszolgáltatási megközelítés hasonlít hello webes? Hello webkiszolgáló, például a mikroszolgáltatási kell toobe megcímezhető bárhol fut-e. Ha vannak a számítógép gépek gondolat, és melyik fut egy adott mikroszolgáltatási, dolgot gyorsan rossz lépjen. 

Hello azonos módon, hogy a DNS egy adott URL-cím tooa adott gép mutat, a mikroszolgáltatási kell toohave egy egyedi nevet úgy, hogy az aktuális helyén felderíthető. Mikroszolgáltatások létrehozására, amelyek miatt a futó hello infrastruktúra független megcímezhető nevet kell adni. Ez azt jelenti, hogy van egy közötti interakció hogyan a szolgáltatás telepítve van és hogyan felderítésekor, mert a szolgáltatás beállításjegyzékbeli toobe kell. Egyaránt Ha egy gép meghibásodik, hello beállításszolgáltatás kell meghatározzák a hello szolgáltatás most futtató. 

Ekkor velünk toohello a következő témakörben: rugalmasságának megvalósítása és konzisztenciáját.

### <a name="remains-consistent-and-available-in-hello-presence-of-failures"></a>Következetes és hibák hello jelenléte elérhető marad
Váratlan hibák kezelésével egyike hello nagyon nehéz problémák toosolve, különösen az elosztott rendszer. Nagy részét hello kódot, amely azt a fejlesztők írható kezeli a következő kivételeket, és ez is ahol hello legtöbb idő telhet el a tesztelése. hello probléma bonyolultabb, mint programozás toohandle hibák. Mi történik, ha hello hello mikroszolgáltatási futtató megtörtént? Nem csak toodetect a mikroszolgáltatási hiba (rögzített probléma önállóan) van szüksége, de emellett szükség van valamilyen toorestart a mikroszolgáltatási. 

Egy mikroszolgáltatási toobe rugalmas toofailures kell, és gyakran indítsa újra a rendelkezésre állási okokból egy másik számítógépen. A mentett nevében hello mikroszolgáltatási, ahol hello mikroszolgáltatási helyre tudja állítani az ilyen állapotú, és hogy hello mikroszolgáltatási képes toorestart sikeresen toohello állapotának is tartalmaz. Más szóval kell toobe rugalmasság a hello számítási (hello folyamat újraindítja), valamint a rugalmasság hello vagy data (adatok elvesztését és hello adatot nem konzisztensek maradnak).

a hibatűrés hello problémák vannak jóváírása során más helyzetekben, például, ha hiba fordulhat elő, egy alkalmazás frissítése során. hello mikroszolgáltatási hello üzembe helyezési rendszerrel, használata nem szükséges toorecover. Toothen is kell annak eldöntése, hogy továbbra is toomove előre toohello újabb verzióra, vagy inkább visszaállítása tooa előző verzió toomaintain konzisztens állapotú legyen. Például elég gépek csatlakoznak-e elérhető tookeep soron és hogyan hello mikroszolgáltatási korábbi verzióinak toorecover kell figyelembe veendő toobe kérdéseket. Ezek a döntések ehhez a hello mikroszolgáltatási tooemit egészségügyi adatokat toobe képes toomake.

### <a name="reports-health-and-diagnostics"></a>Jelentések állapot és diagnosztika
Tűnhet nyilvánvaló, és gyakran kihagyott van, de egy mikroszolgáltatási jelenteniük kell az állapot és diagnosztika. Ellenkező esetben nincs műveletek szempontból kevés betekintést. A diagnosztikai adatok között független szolgáltatások és a gép óra dönt toomake logika hello esemény rendelés van kihívást foglalkozik. A hello azonos módon, hogy Ön a szolgáltatóosztályokkal egy mikroszolgáltatási elfogadott protokollok és adatok formátumú, nem jelenik meg szabványügyi hogyan toolog állapotának és végső soron egy eseményben végül diagnosztikai eseményei tárolni kérdez le, és tekintse meg a szükséges. Egy mikroszolgáltatások módszert használja, a kulcs esetében, hogy a különböző csapatok megállapodni egy egyetlen naplózási formátum. Kell toobe egy egységes módszer tooviewing diagnosztikai események hello alkalmazás egészének.

Állapot abban különbözik a diagnosztika. Állapotfigyelő tárgya hello mikroszolgáltatási reporting az aktuális állapot tootake megfelelő műveleteket. Egy jó példa a frissítés és a központi telepítési módszerek toomaintain rendelkezésre állási dolgoznak. Bár a tooa folyamat összeomlása miatt jelenleg nem megfelelő vagy számítógép-újraindítás, hello szolgáltatás továbbra is előfordulhat, hogy működésbe. hello dolog van szüksége a ami még rosszabb toomake van frissít. hello legjobb megoldás toodo vizsgálat először, vagy várja meg hello mikroszolgáltatási toorecover. Az egy mikroszolgáltatási állapotával kapcsolatos események segítsen tájékozott döntést hozni, és öngyógyító szolgáltatások érvényben, segítséget.

## <a name="service-fabric-as-a-microservices-platform"></a>A Service Fabric mikroszolgáltatások platformként
Az Azure Service Fabric kiderült a Microsoft által átmenet mezőben termékek, amely általában egységes stílus, toodelivering szolgáltatások küldjenek. hello élmény felépítése, és működési nagy szolgáltatások, például az Azure SQL Database és Azure Cosmos DB, a Service Fabric alakú. hello platform továbbfejlődtek idővel egyre több szolgáltatások elfogadott azt. Fontos a Service Fabric kellett toorun, nem csak az Azure-ban, hanem a Windows Server-telepítések önálló.

***hello célja a Service Fabric toosolve hello rögzített problémák kialakításához és futtatásához egy szolgáltatás, és infrastruktúra erőforrásainak hatékonyan, használatát, így a csapatok egy mikroszolgáltatások megközelítéssel üzleti problémák megoldhatók.***

A Service Fabric nyújt három területek toohelp meg alkalmazások összeállítását, amelyek egy mikroszolgáltatások módszert használja:

* Egy platform, amely biztosítja a rendszer szolgáltatások toodeploy, frissítés, észleli, és indítsa újra a sikertelen szolgáltatásokat, szolgáltatások felderítése, üzenetek, állapot kezelése és állapotának figyeléséhez. A rendszer szolgáltatások engedélyezése korábban leírt mikroszolgáltatások hello jellemzői számos érvényben.
* Képes toodeploy alkalmazások vagy fut, az tárolókat, és feldolgozza. A Service Fabric, a tároló és a folyamat az orchestrator.
* Hatékony programozási API-k, olyan alkalmazásokat, mikroszolgáltatások hozhat létre toohelp: [ASP.NET Core Reliable Actors és Reliable Services](service-fabric-choose-framework.md). Kiválaszthatja a kód toobuild a mikroszolgáltatási. Azonban ezen API-k ellenőrizze hello feladat egyszerűbb, és azok integrálása hello platform mélyebb ismeretére. Így például állapot és diagnosztika információkat kaphat, vagy beépített magas rendelkezésre állású előnyeit élvezheti.

***A Service Fabric a hogyan hoz létre a szolgáltatás független, és bármely technológiával. Azonban biztosítja a beépített programozási API-k, amelyek révén könnyebben toobuild mikroszolgáltatások létrehozására.***

### <a name="migrating-existing-applications-tooservice-fabric"></a>Meglévő alkalmazások tooService háló áttelepítése
A kulcs megközelítés tooService háló tooreuse meglévő kódot, mely tudja majd utat korszerűsítenek az új mikroszolgáltatások létrehozására. Tooapplication korszerűsítése öt szakaszból áll, és elindíthatja, és bármikor hello szakaszból leállítása. Ezek a;

1) A hagyományos egységes alkalmazás igénybe
2) Növekedési és az eltolás mértékét megadó – a Service Fabric-tárolók és a Vendég végrehajtható fájlok toohost meglévő kódot használja.
3) Korszerűsítése - mellett a meglévő indexelése kód hozzáadott új mikroszolgáltatások létrehozására. 
4) Innovációját - felosztása hello egységes mikroszolgáltatások tisztán igények alapján.
5) Átalakítja az mikroszolgáltatások - hello átalakítása egységes alkalmazások meglévő vagy új greenfield alkalmazások létrehozásához.

![Áttelepítési tooMicroservices][Image3]

Fontos tooemphasis ebben az esetben is **indítása és leállítása, ezek a szakaszok**, nem kényszerül toomoved toohello következő szakaszára. Most nézzük példák az alábbi szakaszok.

**Emelje fel, és az eltolás mértékét megadó** - vállalat nagy számú emelésre van, és a meglévő egységes alkalmazások időeltolású tárolók toofor be az alábbi okok miatt két;

- Költség csökkentése érdekében vagy fizetési tooconsolidation és eltávolításának magasabb sűrűség meglévő hardver- vagy futó alkalmazásokat. 
- Egységes telepítési szerződés fejlesztésre, működésre között.

Érthető költségek csökkentése és a Microsofton belül folyamatban van a meglévő alkalmazások nagy számú egyszerűen dolláros toomillions indexelése. Egységes telepítési nehezebben tooevaluate, de egyaránt fontos. Azt jelzi, hogy a fejlesztők is még szabad toochoose hello technológia, hogy a csomagok kell őket, de hello műveletek csak egy egyetlen konkrét módszert meghatározni toodeploy elfogadja és ezeket az alkalmazásokat kezeléséhez. Kiküszöböli az hello műveletek a sok különböző technológiák hello összetettsége toodeal rendelkezik, vagy fejlesztők tooonly kényszerítése válasszon bizonyos néhányat a meglévők közül. Gyakorlatilag minden egyes alkalmazás van indexelése az önálló telepítési lemezképeket.

Számos szervezet itt leállítása. Tárolók hello előnyei már rendelkeznek, és a Service Fabric hello teljes kezelést biztosít központi telepítés, frissítés, versioning, visszagörgetése, egészségügyi figyelési stb.

**Korszerűsítése** -hello hozzáadása az új szolgáltatások meglévő indexelése kóddal együtt. Új kód toowrite fog, esetén ajánlott toodecide tootake kis lépésekkel hello mikroszolgáltatások útvonalon. Lehet, hozzáadása egy új REST API-végpont vagy egy új üzleti logikát. Ezzel a módszerrel, indítsa el a hello út épület új mikroszolgáltatások létrehozására és gyakorlat fejlesztéséhez és telepítéséhez őket.

**Innovációját** -megjegyzése azokat eredeti változó üzleti igényeinek ebben a cikkben hello elején hello mikroszolgáltatások megközelítés eredményező? Ez a szakasz hello döntési van, ezek azonban toomy aktuális alkalmazás, és ha igen, toostart hello monolit felosztásával, vagy innovating kell. Itt például akkor, ha egy adatbázist a feldolgozás szűk keresztmetszetek válik, mivel használatban van egy munkafolyamat-várólista. Hello számának növelése hello munkafolyamat kérelmek működik elosztott a skálázási igények toobe. Így az adott hello alkalmazást, amely nem adott adathordozónevek skálázás, vagy meg kell tooupdate több gyakran, egy mikroszolgáltatási ossza ki ez, és innovációját. 

**Átalakítja az mikroszolgáltatások** – Ez a ahol az alkalmazás teljesen álló (vagy bontja fel a rendszer a) mikroszolgáltatások létrehozására. Itt tooreach, végrehajtott hello mikroszolgáltatások út. Megkezdheti itt, de toodo Ez egy mikroszolgáltatások platform toohelp nélkül, jelentős befektetési. 

### <a name="are-microservices-right-for-my-application"></a>Azok a mikroszolgáltatások közvetlenül az alkalmazáshoz?
Lehetséges. Azt tapasztalta, hogy több és több csapat a Microsoft hello felhő üzleti okokból toobuild megkezdődött, mivel ezek is tudjuk mikroszolgáltatási hasonló alkalmazni hello előnyei volt. A Bing, például rendelkezik lett fejlesztése a keresési évig mikroszolgáltatások létrehozására. A más csapatokkal hello mikroszolgáltatások megközelítés az volt új. Csoportok megállapította, hogy rögzített problémák toosolve kívül erőssége core területére. Ezért a Service Fabric szerzett vontatási szolgáltatások számára választott hello technológia.

Service Fabric hello célját tooreduce hello összetett szolgáltatásokkal, az alkalmazások egy mikroszolgáltatási módszert használja, így nem kell toogo keresztül, számos költséges redesigns. Kezdje kis lépésekkel, ha szükséges, szolgáltatások érvényteleníthető, újakat vehet fel és fejleszteni az ügyfél használati hello megközelítés. Is tudjuk, hogy sok egyéb problémák vannak még toobe a legtöbb fejlesztő több könnyű toomake mikroszolgáltatások lehet megoldani. Tárolók és hello szereplő programozási modell példák ebben az irányban kis lépés, és azt, hogy további innovációinak fog megjelenni toomake ez biztosan egyszerűbb.
 
<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="next-steps"></a>Következő lépések
* [A Service Fabric-terminológia áttekintése](service-fabric-technical-overview.md)
* [Mikroszolgáltatások: Egy alkalmazás fordulat hello felhő technológiával](https://azure.microsoft.com/en-us/blog/microservices-an-application-revolution-powered-by-the-cloud/)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
[Image3]: media/service-fabric-overview-microservices/microservices-migration.png
