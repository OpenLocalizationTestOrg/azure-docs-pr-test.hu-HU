---
title: "Mobile Engagement – első lépések az ajánlott eljárásokkal aaaAzure"
description: "Első lépések útmutató az Azure Mobile Engagement használatának megkezdéséhez és ajánlott eljárások a bevezetéshez"
services: mobile-engagement
documentationcenter: mobile
author: wesmc7777
manager: erikre
editor: 
ms.assetid: dfce1183-6398-466e-aa7e-ed702fb52818
ms.service: mobile-engagement
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/04/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: d3f81c34fe74f741a60ca511caa55c312af73b1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement---getting-started-guide-with-best-practices"></a>Azure Mobile Engagement – Első lépések útmutató ajánlott eljárásokkal
## <a name="overview"></a>Áttekintés
**hello mobileszközök képernyője nagyon zsúfolt terület:** 2013-ban egy tanulmány szerint hello átlagos mobileszközre kellett 27 alkalmazást telepítettek. A felhasználók átlagosan havonta 30 órát foglalkoztak az alkalmazásaikkal. Ezen idő nagy részét a közösségi hálózatok és a játékok vitték el (mintegy 20 órát). 2014-re hello Android piaci a a felhasználók toochoose körülbelül 1,5 millió alkalmazás volt. hello Apple áruházában körülbelül 1,2 millió alkalmazás volt elérhető. A növekvő piacon versengő fejlesztőknek köszönhetően a mobilalkalmazások használata továbbra is növekvő tendenciát mutat. 

hello átlagos mobilfelhasználó telepíti és távolít el alkalmazásokat nagy gyakorisággal attól függően, hogy módosítása érdekében, és az alkalmazáson belüli élményeken. A sorrend toodetermine hello egy alkalmazás sikerességének létfontosságú tooknow válik több mint csak felhasználók számára az alkalmazás telepítéséhez. Fontos, hogy mennyire hasznos az alkalmazás, illetve ha, hogy használati trendje változik tooknow. hello a következő kérdések váltak fontossá:

* Vannak a felhasználók verziótól toofind érdektelen vagy elavult az alkalmazást? 
* Hány felhasználó hagyta abba teljesen az alkalmazás használatát? 
* Növekszik vagy csökken az alkalmazáson belüli vásárlások száma?
* Felhasználók sikertelenek toocomplete munkafolyamatok összefüggő hello alkalmazás vagy tapasztalható? 
* Sikerült megőrizni az alkalmazás hasznosságát és fontosságát friss tartalom tooyour felhasználói alap? 
* Ezek a friss tartalmak lenne az összes felhasználó számára azonos vagy összpontosítanak az alkalmazáson belüli viselkedés alapján felhasználói szegmenseknek hello? 

Válaszok tooquestions hasonló toothese hozzájárulhat az hello élettartamának és bevétel az alkalmazásból. Emellett segíthetnek a felhasználói bázis meghatározásában és megtartásában is. 

Media kapcsolódó alkalmazások általában toohave néhány hello legmagasabb felhasználómegtartási aránnyal. Ennek az egyik oka, hogy folyamatosan friss tartalom toousers ad. Hasznos leküldéses értesítések használatának korai bevezetése irányított tooa felhasználói szegmens általában nagy hatással van az alkalmazás megtartására toohave. 

hello Azure Mobile Engagement program tervezett toohelp kiterjesztése hello élettartamának és megtartási arányának az alkalmazás egy metódus toogather megadásával és elemezheti a hello használata az alkalmazás részletes információkat. Nyújt segítséget a felhasználói bázis megfelelő toobehavior besorolását, és hozzon létre kézbesítéséhez a tooidentified felhasználói szegmenseknek leküldéses értesítésekkel és alkalmazáson belüli üzeneteket küldő, célzott kampányok. A fő teljesítménymutatók (KPI) mérik, hogy a felhasználók mennyire aktívak az alkalmazás különböző aspektusainak a vonatkozásában. Hello metódusokat biztosít az Azure Mobile Engagement ezen KPI-k toodetermine szüksége. Ennek segítségével növelheti a hello visszatérési hello infrastruktúra biztosításával a ráta (ROI) a szükséges tooincrease engagement a mobilalkalmazást. 

Rendelés tooget hello legtöbbet az Azure Mobile Engagement toostart egy jól kidolgozott felhasználóbevonási tervvel kell. A terv segíteni fog hello részletes adatot kell toobe képes toosegment a felhasználó alap azonosításához. Ez alapulhat a viselkedésen és az alkalmazáson belüli élményeken. Ahhoz, hogy a terv sikeres toobe hogy a rendszer a bevált gyakorlat tooclearly hello KPI-t, amely mérni fogja az alkalmazás célkitűzéseit hello határozza meg. Az egyértelműen meghatározott teljesítménymutatók, könnyedén építheti hello szükséges logika az alkalmazás toocollect finom adatok, amelyek a tooanalyze használja, és értékelje ki a KPI-ket. Ez a témakör az ajánlott eljárásokat tartalmazó útmutató a bevonási tervvel használandó hello KPI-k meghatározásához. 

## <a name="step-1-define-your-kpis-toofit-hello-bet-model"></a>1. lépés: A KPI-k toofit hello BET-modell meghatározása
A KPI-k megfelelő meghatározása nehéz feladat-toocomplete is lehet. A különböző iparágak számára tervezett alkalmazások eltérő jellemzőkkel és célkitűzésekkel bírnak. Ez zavarólag tooconfuse a megközelítésre. toohelp Ennek elkerüléséhez célkitűzések és a KPI-ket három fő kategóriába kell sorolni: **üzleti**, **Engagement**, és **műszaki**. Ez az úgynevezett hello **BET-modellnek**.

Egy jó terv általában olyan célkitűzéseket tartalmaz, amelyek mérik az egyes kategóriák hello BET-modell a következő hello hello sikeres hello KPI.

#### <a name="business-kpis"></a>Üzleti KPI-k
Üzleti KPI-k hello legegyszerűbb rész toobuild kell lennie. Ezeket valamilyen formában valószínűleg már meghatározta, amikor megtervezte a mobilalkalmazást. Ezek a KPI-k általában az alkalmazáshoz kapcsolódó bevételek és ROI méréséhez nyújtanak segítséget. hello alábbiakban néhány példát biztosít üzleti KPI-k, előfordulhat, hogy útmutatásként használhat a teljesítménymutatók meghatározásához:

* Médiához kapcsolódó üzleti KPI-k
  * Reklámkattintások száma
  * Oldallátogatások száma felhasználónként
  * Az aktuális előfizetések száma
* Játékokhoz kapcsolódó üzleti KPI-k
  * Alkalmazáson belüli vásárlások száma
  * Átlagos bevétel felhasználónként (ARPU)
  * Alkalmanként játékkal töltött idő
  * Játékkal töltött napok száma és a játékon belüli aktuális szint
* E-kereskedelemhez kapcsolódó üzleti KPI-k
  * Alkalmazás használati napjainak száma
  * Átlagos bevétel felhasználónként (ARPU)
  * Átlagos összeg a kosárban a fizetéskor
  * A legtöbb megtekintést és vásárlást hozó termékkategória
* Banki és biztosítási szolgáltatásokhoz kapcsolódó üzleti KPI-k
  * Számlák száma
  * Aktivált szolgáltatások száma
  * Felkeresett ajánlati oldalak száma
  * Kattintott vagy aktivált figyelmeztetések száma       

#### <a name="engagement-kpis"></a>Bevonási KPI-k
Bevonási KPI olyan a teljesítmény kijelző toomeasure hello engagement a felhasználók. A terület trendjei meghatározásához, hogy az alkalmazás hello megőrzési. Alább néhány példát láthat az ilyen típusú KPI-kre:

* Az elmúlt 7 napban hello aktív felhasználók
* Hello 7 napja inaktív felhasználók száma
* A felhasználók, akik 30 napban nem használták hello alkalmazást száma  

Bizonyos nyilvánvaló külső tényezők befolyásolhatják a terület mutatóit. Például akkor fontolja meg egy mobileszköz toobe felhasználóval történő mindig. Ez nem feltétlenül igaz. Egy általában toohave játékalkalmazásokat munkaszüneti napokat is, amikor a játékosok előfordulhat, hogy többet játszanak, miközben munkába vagy iskolába menniük.   

Jól meghatározott KPI-k ebbe a kategóriába kell segítségével mérheti hello kapcsolat az alkalmazás és az ügyfelek között.

#### <a name="technical-kpis"></a>Műszaki KPI-k
A kategória teljesítménymutató annak meghatározásában segítenek, hogy az alkalmazás megfelelően működik, nem válaszol vagy összeomlik. Ezek a mutatók is mérésére hello az alkalmazás állapotát, és meghatározhatnak olyan használati problémákat, amelyek megakadályozhatják, hogy a felhasználók hello alkalmazással. A kategóriában gyűjtött információkat is tartalmazhatnak, amely lenne megfelelő toomarketing csapatok teljesítményadatok. hello adatokat is hasznos lehet a hibaelhárítás informatikai és a támogatási csapat toohelp nem jelentett hibák azonosítására. 

Néhány példa a műszaki KPI-kre:

* A nem kezelt vagy kezelt kivételek adatai és száma 
* Az utolsó összeomlás időbélyegzője
* A gomb, amelyre utoljára kattintottak, vagy az utoljára felkeresett oldal
* Hello alkalmazás memóriahasználata
* Az alkalmazás képkockasebessége
* Operációs rendszer verziója, amely hello alkalmazás fut.
* Az alkalmazás verziója

Adja meg ezeket a KPI-k toohelp alkalmazás teljesítményének méréséhez és a lehetséges hibák kiszűréséhez. A mutatók csökkenthetik az ügyfeleknek toodeliver egy javítást kell hello idő. Emellett segíthetnek az olyan felhasználói szegmensek meghatározásában, amelyek adott problémákkal szembesültek. Is használhatja, hogy felhasználói szegmentálást toocreate kampányok toodeliver értesítések tekintetében, elérhető frissítésekkel vagy lehetséges promóciókkal toohelp ügyfelek elégedettségének a helyreállítását. 

#### <a name="playbook-exercise-1-create-your-kpi-dashboard"></a>Az útmutató 1. feladata: A KPI-irányítópult létrehozása
A marketingstratégia meghatározásakor a KPI-knek mindegyik fő célkitűzést képviselniük kell. Egyértelműen meghatározott adatpontok, amelyek lehetővé teszik hogy toocollect a fontos adatokat toomonitor a hello végfelhasználói alkalmazás, és hello viselkedését kell őket.

Egy KPI-irányítópultot, amely tartalmazza az alábbi információkat hello összeállítása

1. Mik azok a KPI-k hello hello alkalmazás?
2. Mely adatpontok fogják használni toorepresent egyes KPI-KET?
3. Hol találhatók ezen adatok az alkalmazásban (például képernyő, beállítások, rendszer stb.)?
4. Képes vagy szimulálni egy bevonási folyamatot ezen KPI vonatkozásában?

Használhatja a hello **KPI Builder** munkalapján a [alkalmazástervezési sablonja] [ Media Playbook link] további példákat és útmutatást.

## <a name="step-2-your-engagement-program"></a>2. lépés: Bevonási program
Az alkalmazás egyik legfontosabb összetevőjeként kell tekinteni a kiváló bevonási programra. Ennek mindenképpen tartalmaznia kell egy nagyszerű üdvözlőprogramot, amely végrehajtja a felhasználó hello során az alkalmazás használatának első nap. Ez általában nagyon pozitív hatása bevonására és megtartására az alkalmazás toohave. A tanulmányok kimutatták, hogy egy alkalmazás hello telepítését követő első néhány napban használja felhasználók stop hello többsége. Szeretné, hogy toostrive toomeet, vagy felhasználói elvárása növeli az érdeklődést közben hello felhasználó továbbra is az alkalmazásra koncentrál korai haladhatja meg a. Mindenképpen mutassa be hello kulcs értéke és az alkalmazás előnyeit tooyour ügyfelek. 

![](./media/mobile-engagement-getting-started-best-practices/unsegmented-push-notifications.png)

Leküldéses értesítések hello ajánlott megközelítés tooearly bevonására mobileszköz-felhasználók. Nagy gondot kell fordítani azonban a felhasználók szegmentálására a leküldéses értesítések tekintetében. Amint egy felhasználó úgy érzi, hogy levélszemetet vagy érdektelen értesítéseket kap, annak súlyos hatása lehet. Mindössze néhány kattintással, a felhasználó törölheti az alkalmazást soha nem tooreturn. hello felhasználói kell kapnia általános levélszemét helyett nagy mértékben testre szabott, alkalmazáson belüli értéket.

Ha a felhasználók bevonása sikeres, majd a bevonási program segítségével sikerességének előmozdításához más szempontokból hello alkalmazás.

Például akkor lehetett egy kampányt, amely az aktív felhasználók toorate kér az alkalmazás. Mivel ez a felhasználói szegmens hello legaktívabb és rendelkezik hello legtöbb tapasztalattal az alkalmazást, teheti meg őket toogive hello legpontosabb értékelést. Miután kiváló értékeléseket kap, az segíthet hello organikus letöltések számának növelésében, csökkentve az új felhasználók megszerzésének költségeit az alkalmazást.

#### <a name="engagement-sequence"></a>Bevonási folyamat
A globális bevonási program különböző bevonási folyamatokat tartalmaz. Minden folyamat több célkitűzés célja a tooreach.

###### <a name="life-push-sequence"></a>Életciklushoz kapcsolódó leküldések folyamata
egy életciklushoz kapcsolódó leküldések folyamata hello céljait eltérőek attól függően, hogy hello életciklusát hello felhasználói engagement hello alkalmazással. Egy adott felhasználó lehet új, inaktív vagy nagyon aktív. A bevonási életciklus különböző szakaszaiban a felhasználók számára előnyös lehet a tippek vagy hivatkozások toodocumentation hello formájában friss tartalom. 

Például egy új felhasználót is kell súgó objektumorientált tooan app getting de hasznot húzhatnak a egy új felhasználó ösztönző hasonló toohello hello első indításakor hello alkalmazás a következő...

*"Örülünk toohave meg hozzánk! Ne feledje toologin tooget az 1. ingyenes hónap igénybe vételéhez! "*

###### <a name="behavioral-push-sequence"></a>Viselkedéshez kapcsolódó leküldések folyamata
hello viselkedéshez kapcsolódó leküldések tooincrease használatát a hello alkalmazás számára gyűjtött felhasználói viselkedés alapján.  

Például egy képzeletbeli futballalkalmazás nagyon aktív felhasználójának előnye származhat leküldéses értesítést a következő hello abból...

*„John, Ön komoly futballrajongó! Jelentkezzen be tooour bemutatjuk az NFL szakaszt, és szabad hozzáférés toohello SuperBowl win!"*

###### <a name="alerting-push-sequence"></a>Értesítésekhez kapcsolódó leküldések folyamata
A felhasználók értékelni fogják az érdeklődésüknek megfelelő híreket. Az értesítésekhez kapcsolódó leküldések folyamata a felhasználóval kapcsolatban által egyértelműen kimutatott érdeklődések alapján küldött értesítésekkel növeli a felhasználó aktivitását. Az történhet közvetlenül, ha a felhasználó megadja érdeklődési köreit hello alkalmazásban. Azt is megállapítása volt implicit módon a felhasználó és hello alkalmazás közötti interakció során gyűjtött adatok alapján.

Például az hello felhasználó egy E-kereskedelmi alkalmazás felhasználója rendszeresen vásárol egy adott márkájú kávét, amit egy üzleti KPI-t. hello alábbi értesítéssel így növelhető a felhasználó engagement hello alkalmazással.

*"Nagy, Wes a kedvenc kávémárkája egyik lesz pénztári 25 % hello első hete – 2015. szeptember. Nagyra értékeljük, mint az ügyfél és kívánta toomake meg arról, hogy Ön ismeri."*

###### <a name="rentention-push-sequence"></a>Megtartáshoz kapcsolódó leküldések folyamata
A feladatütemezési célok tooretain felhasználók egy ismétlődő leküldéses értesítési kampányt toohelp a meghajtó, amelyek révén hello app válhat. Ez segítheti a alkalmazás megtartása növelhető, ha hello felhasználó élvezi az interakciókat hello. 

Egy sporttal kapcsolatos alkalmazás felhasználója hello fordulhat elő, például a következő leküldéses értesítést hetente alapján hello kedvenc csapatairól hello:

*"A alkalommal toowin 200 pontot szavaz arra, hogy hello New York Yankees nyer a game ezen a héten szemben a Toronto Blue Jayst!"*

#### <a name="hello-3w-approach"></a>hello 3W megközelítés
Hello különböző leküldéses folyamatok elsajátítása segít a kapcsolattartást a végfelhasználókkal. Azonban továbbra is szükség toouse hello 3W megközelítést az értesítések testre szabásához. hello 3W megközelítést kell, hogy kinek, mit és mikor minden értesítés esetén. Ha egyértelműen megválaszolja ezt a három kérdést, az értesítései megfelelően fognak összpontosítani a bevonásra.

![](./media/mobile-engagement-getting-started-best-practices/who-what-when.png)

###### <a name="who-hello-user-segment-that-will-receive-messages"></a>Ki: felhasználói szegmens, amely az üzeneteket fogja kapni hello
Értesítések tooyour felhasználók küldését figyelembe kell venni egy nagyon érzékeny kommunikációs csatornát. Ellenőrizze, hogy hello értesítések megfelelnek toosend tooa felhasználói szegmens adott felhasználói szegmensnek is hatókörön belüli toohello érdekében. Helytelenül célzott értesítés jó eséllyel toohave rossz hatással lesz a felhasználó. Előfordulhat, hogy ítélik levélszemét vezető tooyour alkalmazás eltávolításához vezethet. 

Használja adott technikai és viselkedési feltételek kombinációját, amikor meghatározza azt a felhasználói szegmenset, amely az értesítéseket meg fogja kapni. A felhasználói szegmens meghatározásának egyszerű példája hasonló toohello utasítás a következő lehet:

"Minden felhasználó, aki elindított hello mobilalkalmazást hello először 3 nappal ezelőtt együtt ténylegesen bejelentkezett nélkül kétszer látogatott el hello bejelentkezési oldal".

Ez az állítás segít azonosítani kell toocollect toosupport egy adott forgatókönyvhöz hello adatokat.

###### <a name="what-hello-message-that-you-will-send"></a>Mi: hello üzenet, amely el szeretne küldeni
**Hangnem**

A kapcsolattartás során olyan hangnemet használjon, amely megfelel a felhasználói szegmens tagjainak. Ez mindenképpen egy jó módszer tooconnect a végfelhasználók és lépteti elő az alkalmazás iránti egy felhasználó. 

**Átirányítás**

Leküldéses értesítés több mint hello alkalmazás megnyitására használható. Ha hello értesítési üzenetet biztosít a olyan környezetben, például a hírek közvetítésére vagy egy termék népszerűsítésére szolgál, az értesítés lehetséges, hogy mélyhivatkozása közvetlen toohello jobb gombbal a tartalom hello alkalmazáson belül. toosupport, létre kell hoznia egy URL-cím sémája toolet hello alkalmazás kezelése hello átirányítása. Amikor a bevonási folyamatokon dolgozik, ez egy fontos lépés, amelyről nem szabad elfelejtkezni.

Az átirányítás más rendszerek vonatkozásában is kezelhető. Például a egy művelet URL-címe esetén a lehetséges tooredirect végfelhasználók toomany más rendszerekkel, többek között a következő hello:

* webhelyre,
* már beállított e-mail-címmel rendelkező postaládába,
* SMS-fiókba,
* tárcsázási szolgáltatásba,
* Közvetlenül toohello alkalmazás-áruház hello alkalmazás értékeléséhez. 

Ez sok lehetőségeket biztosít a tooengage végfelhasználók számára, és hozzon létre automatikus tooimprove teljesítményét.

**Formátum/tartalom**

Különböző típusok és leküldéses értesítési formátumok:

1. **Közlemények** : lehetővé teszi a különböző időpontokban toosend hirdetési üzenetek toousers (alkalmazásból, az alkalmazás vagy bármikor).
2. **Szavazások** : engedélyezte, toogather adatokat azon végfelhasználóktól kérdések kéri a felhasználót. Ezek a válaszok ezután rendelkezésre fognak tootarget végfelhasználók feltételek létrehozásakor.
3. **Adatleküldések** : lehetővé teszi a toosend egy bináris vagy base64 adatok fájl tooupdate hello alkalmazást. hello szereplő adatokat küld adatokat tooyour alkalmazás toopersonalize hello felhasználói élményt az alkalmazásban. Az alkalmazás kell megtervezni toobe toosupport hello adatok található adatokat.
4. **Csempék (csak Windows Phone)** : lehetővé teszi toouse hello a Microsoft leküldéses értesítési szolgáltatásának (MPNS) toosend natív leküldéses értesítés tartalmazó XML-adatok (SDK 0.9.0-s verziója óta támogatott. csempék végső hasznos hello nem haladhatja meg a 32 kilobájtot.). üdvözlőüzenetére közvetlenül a indítópanel csempe megjelenik.
5. **Webes nézet**: A webes nézet webes tartalmat tartalmazó előugró ablak. Az előugró ablak akkor jelenik meg, amikor hello végfelhasználói hello leküldéses értesítési kattintott. A webes nézet lehetővé teszi a toohave több interakciót tesz hello végfelhasználói.

> [!NOTE]
> Győződjön meg arról, hogy hello tartalom leküldéses értesítések, megfelel hello megfelelő platform (iOS, Android, Windows) alkalmazások fejlesztésérére és leküldéses értesítések küldése vonatkozó irányelveket.
> 
> 

###### <a name="when-hello-timing-of-your-campaign"></a>Esetén: a kampány időzítése hello
Ha van hello legjobb idő tooactivate egy elindítani leküldéses értesítéseket használó kampányt? Manuális vagy automatikus legyen? Ismétlődjön? Meghatározó hello megfelelő időpont és ismétlődési gyakoriság alapvető tooengage felhasználók hello legjobb eredmények elérése érdekében. Minden bevonási folyamat és forgatókönyv esetén meg kell adnia mikor lesz hello legjobb idő toosend leküldéses értesítéseket. Néhány lehetséges példa:

![](./media/mobile-engagement-getting-started-best-practices/campaign-timing-examples.png)

Ha naponta sok értesítést küld, komolyan meg kell fontolnia, hogy a felhasználók levélszemétnek tekinthetik-e a kommunikációt. 

Azure Mobile Engagement két módot biztosít toohelp elkerülése érdekében a kommunikáció levélszemétként történő kezelését. Először az finomítsa szegmentálását tooensure tegye nem cél hello ugyanazokat a felhasználókat. Emellett az Azure Mobile Engagement egy „kvóta” funkciót is biztosít. Ezzel a funkcióval korlátozhatók a kampány részeként elküldött értesítések. Például egy alapértelmezett kvótát too5 hetente beállítása biztosítja, hogy a felhasználó, valamint hello kampány felhasználói szegmens részét hetente legfeljebb 5 értesítéseket fogad.

#### <a name="playbook-exercise-2-create-your-engagement-program"></a>Az útmutató 2. feladata: A bevonási program létrehozása
Szánjon némi időt a célkitűzések összegzésére, és hello kampányok meghatározása várt tooplay adott folyamatok használatával. Ellenőrizze, hogy a kampányok hello 3W megközelítés toohello értesítések alkalmaz. 

Használjon hello **bevonási Program** hello munkalapján [alkalmazástervezési sablonja] [ Media Playbook link] további példákat és útmutatást.

## <a name="step-3-app-integration"></a>3. lépés: Alkalmazásintegráció
#### <a name="create-a-tag-plan"></a>Címkézési terv létrehozása
Azure Mobile Engagement toointegrate az alkalmazásba a címkézési tervnek toocreate kell. hello címkézési terv hello projekt sarokköve hello. Meghatározza a marketingspecifikációk, hello hello alkalmazás munkafolyamata és hello valós címkeadatok gyűjtését olyan hello app toomeasure KPI-k hello kapcsolatát. Azt jelzi, hogy milyen elemzési adatokat fog tudni toosee hello portálon. Azt is segítséget nyújt felhasználói szegmensek meghatározásában, és a koncentrált leküldéses értesítések tooengage küldeni a végfelhasználók számára. Ha már definiált hello címkézési tervet, hello kód toointegrate hozzáadása az alkalmazásba használata egyszerű hello Azure Mobile Engagement SDK használatával.

A címkézési tervnek nem szabad mindent címkéznie az alkalmazásban. Csak olyan címkeadatokat szabad tartalmaznia, amelyek a mobilmarketing-stratégiájának a részét képezik. Az adatok valószínűleg eltérőek lesz alkalmazásonként. Hello [alkalmazástervezési sablonja] [ Media Playbook link] megadott által az Azure Mobile Engagement segítséget nyújt, a címkézési terv létrehozásához egy adott módszerrel. Használjon hello **címkézési terv** munkalap egy útmutató toobuilding, a címkézési tervben.

Egy címke szakasz hello munkalapon meghatározásakor kell erősen korlátozott. Ez a nagyon fontos tooavoid zavart. Részletezzen minden olyan várható forgatókönyvet, amelyben az egyes címkék elküldése megtörténik. Ha minden egyes címkék be lesznek ágyazva hello tevékenység hello nevét tartalmazza. Ez az összes szerepeljenek hello **Informative** hello munkalap része. hello címke terv létrehozására szolgáló munkalapnak kell hello a teszteléssel történő ellenőrzés fő referenciájának. 

A hello **adatok toocollect** részben, a fejlesztői csapat kell hello típusok, neveket, értékeket és minden szükséges további adatokat biztosító kulcs/érték párok címkékhez keresés beágyazni kívánt hello alkalmazás.

Azt javasoljuk, hogy hello címkézési terv áttekintését hello projekthez kapcsolódó csapatok mindegyikével. Végezze el a szükséges javításokat, és győződjön meg arról, hogy a marketing- és a fejlesztői csapat mindennel tisztában van.

Hello **munka utasítás** munkalap lehet használt toohelp útmutató hello projekt összes résztvevője számára.

#### <a name="data-types"></a>Adattípusok
Az Azure Mobile Engagement által támogatott általános adattípusok.

###### <a name="devices-and-users"></a>Eszközök és felhasználók
Az Azure Mobile Engagement az egyes eszközök számára létrehozott egyedi azonosítóval azonosítja a felhasználókat. Ezen azonosító neve eszközazonosító hello (vagy deviceid). Úgy, hogy a adott megosztáshoz eszközön futó összes alkalmazás ugyanazon eszközazonosítót használja hello jön létre.

###### <a name="sessions-and-activities"></a>Munkamenetek és tevékenységek
A munkamenet a felhasználó által futtatott hello alkalmazás több példánya. hello munkamenet is a hello indításakor hello felhasználói hello alkalmazás leállásáig tart.

Egy tevékenység készen áll hello app egy munkamenet során előfordulhat, hogy műveletekből logikai csoportosítása. Ez általában egy adott képernyőre hello alkalmazásban, de lehet bármi, amit hello logika hello alkalmazás. Legalább az alkalmazás egyes képernyőit vagy tevékenységeit meg kell címkéznie. Ez lehetővé teszi az Ön toounderstand hello felhasználó által megtett út.

###### <a name="events"></a>Események
Események olyan hello alkalmazással használt tooreport felhasználói beavatkozást. Az események lehetnek azonnali műveletek, például tartalom megosztása vagy videó elindítása. Az események címkézése tudhatja meg, amelyek megmutatják, hogy a felhasználók hogyan használják a hello alkalmazással. 

###### <a name="jobs"></a>Feladatok
Feladatok időtartammal rendelkező műveletek használt tooreport. Néhány példa ilyen műveletekre:

* API-hívások végrehajtása,
* hirdetések megjelenési ideje,
* háttérben futó feladatok időtartama, 
* vásárlási folyamat időtartama,
* videó megtekintése.

###### <a name="errors"></a>Hibák
Hibákat észlelt hello alkalmazás által használt tooreport problémák. Ilyenek például a helytelen felhasználói műveletek vagy sikertelen API-hívások.

###### <a name="application-information"></a>Alkalmazásadatok
Alkalmazás (App-Info) adatok kapcsolódó tootag adatok tooa felhasználói élmény alkalmazással. Egy felhasználó és hello alkalmazás közötti interakció állítja elő. 

Egy adott alkalmazásadat-kulcsnak, az Azure Mobile Engagement csak nyomon követi az hello legfrissebb értéket (az előzményeket nem). Az alkalmazásadatok az alkalmazás vagy a végfelhasználók hello állapotát. Például hello bejelentkezési állapot, vagy a felhasználó kedvenc termékcsoportja.

###### <a name="crash-data"></a>Összeomlási adatok
Összeomlási adatok által automatikusan összegyűjtött hello Mobile Engagement SDK hello alkalmazás által nem kezelt alkalmazáshibákat. Ilyen például egy bekövetkezett nem kezelt kivétel.

###### <a name="extra-data"></a>További adatok
Az eseményeket, hibákat, tevékenységeket és feladatokat paraméterekkel lehet bővíteni. Ez az a fejlesztő hello alkalmazásból származó konkrét adatként biztosíthat további adatokat. Ez a részletes szegmentálás meghatározásához fontos. 

Például egy "cikk" címke értéke hello lehetővé teszi a toosegment végfelhasználók ki tekintette meg, hogy adott cikk alapján. Ugyanakkor nem biztos, hogy ez elegendő. Jobb megoldás lehet, ha ugyanazon „cikk” címke további adatokat is tartalmaz, ilyen lehet például a „hír_kategória” egy tevékenységen belül. Ez hasznos lehet toodetermine dinamikusan hello hello felhasználó kedvenc kategóriáinak. 

A további adatok jelentése kulcs/érték párként történik. Ezen médiaalkalmazás hello példában hello további adatai a "hír_kategória" hello kategória értéke lenne. Például „sport”, „gazdaság” vagy „politika”.

#### <a name="tag-and-sdk-integration"></a>Címke- és SDK-integráció
A részletes utasításokat a termék hello Azure Mobile Engagement SDK-t az alkalmazásba, hajtsa végre a hello [Engagement SDK-integráció](mobile-engagement-windows-store-integrate-engagement.md) dokumentáció az Azure-webhelyen. A célplatform választhat, hogy a lap tetején hello hello hivatkozásait.

Javasoljuk, hogy az Azure Mobile Engagementre épült két alkalmazáshoz hozzon létre projekteket. Egyet a fejlesztési és tesztelési szakaszhoz, és az éles szakaszhoz hello. Az informatikai csapat így előreléphet tesztből átmeneti tooproduction, ha a hello felhasználói tesztelés sikeres volt.

#### <a name="user-acceptance-testing-uat"></a>Felhasználói tesztelés
A felhasználói tesztelés magában foglalja annak ellenőrzését, hogy minden a tervek szerint működik-e. A munkafolyamatok befejezhetők, és a címkézési terv alapján minden adat összegyűjthető:

* Tájékoztatási toodocumented AZME-koncepciók szerint helyen kell lennie
* Minden szükséges információ gyűjtése megtörténik (beleértve a további adatok értékeit és alkalmazásadatok értékeit).
* Elnevezéseket megegyezik a címkézési tervben függően tooyout
* Nincsenek duplán elküldött címkék.

Alaposan tesztelje az alkalmazás beágyazott viselkedését minden hello típusú

* Közlemények, szavazások, adatleküldések alkalmazáson kívül és belül.
* Szöveges/webes nézetek.
* Jelvényfrissítés, kategóriák.

#### <a name="setup"></a>Beállítás
Az Azure Mobile Engagement beállítása nagyon egyszerű. Minden hello dokumentációt kapcsolódó toohello felhasználói felület érhető el a hello Azure Mobile Engagement webhelyén, [hogyan toonavigate hello felhasználói felület](mobile-engagement-user-interface-home.md).

Ajánlott kezdeni hello megfelelő szerepköreivel és szerepkörcsoportjaival a projekt felhasználóinak hello beállítása. Ezzel a megoldással a megfelelő hozzáférési toohello platform az összes felhasználó kezelése. A szerepkörök az alábbiakat foglalhatják magukban:

* Rendszergazdák
* Fejlesztők
* Megtekintők 

Ezt követően:

* A deviceID tootest saját eszköz regisztrálása.
* Nyissa meg a fiók toohello beállításait, és állítsa be a hello időzóna toohave diagramok és értesítések küldési időpontja az időzónájának beállítása.
* Nyissa meg az alkalmazás toohello beállításait, és regisztrálja a hello "App-info" tootarget végfelhasználók megcélzásához szükséges.

További információ a toorun az első leküldéses értesítési kampányt, tekintse át [hogyan tooget használatának és a kimenő tooyour végfelhasználók tooreach kezelése leküldéses értesítések](mobile-engagement-how-tos.md).

## <a name="conclusion"></a>Összegzés
A bevonási programok ismétlődőek, érdemes folyamatosan fejleszteni őket, miközben kísérletezik azzal, hogy mi működik a legjobban az alkalmazás tekintetében. 

Kezdetben a felhasználói élmény fejlesztése során az engagement stratégiák ne próbáljon egy teljes globális stratégiát toobuild. A KPI-k azonosító lépésről lépésre a megoldást, és hogyan tooleverage őket. A bevonási stratégia minden alkalmazás esetén egyedi lesz.

Miután szert tett némi tapasztalattal érdemes lehet a következő tooyour bevonási programok hello hozzáadása:

* Nyomon követés: vélhetően szert tesz felhasználókra, és valószínűleg meghatároz adatgyűjtési forrásokat. Az Azure mobile Engagement csatolt toodata-gyűjtemény adatforrások is lehet. Ez lehetővé teszi minden forrás teljesítményének toomonitor. Ezt az információt fogja érdekes toomaximize beszerzési befektetéseit. 
* A/B tesztelés: Ez fontos része a bevonási programnak. Minden alkalmazás saját adottságokkal rendelkezik. Az A/B tesztelés révén fejlesztheti a bevonási programot.
* Földrajzi helymeghatározás: Ez nagy lehetőség a márkáknak. Köszönjük, hogy toothis szolgáltatás érhet el hello megfelelő helyen és időben. Azt javasoljuk, hogy elegendő adatot a végfelhasználók viselkedésével összegyűjtötte toouse földrajzihely-elindítása előtt ellenőrzi.
* Adatleküldés: Az adatleküldés láthatatlan leküldés. Az adatleküldés lehetővé teszi az alkalmazás testre szabását a végfelhasználói viselkedés alapján. Például ha egy felhasználói szegmens gyakran tekint csúcstechnológiai termék, hello alkalmazás tulajdonosa is küldésével szabhatja testre a csúcstechnológiai tartalmú kezdőlapnak.

## <a name="next-steps"></a>Következő lépések
* [Azure Mobile Engagement-fiók létrehozása](mobile-engagement-create.md).
* Látogasson el [a Mobile Engagement-stratégia kidolgozása](mobile-engagement-define-your-mobile-engagement-strategy.md) toolearn további információk a Mobile Engagement-stratégia meghatározása.

<!--Image references-->


<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
