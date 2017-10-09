---
title: "az Azure CDN aaaAnalyze kihasználtságának statisztikai adatai speciális HTTP-jelentések |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate speciális HTTP-jelentések a Microsoft Azure CDN szolgáltatás használata. Ezek a jelentések részletes információkkal CDN tevékenység."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ef90adc1-580e-4955-8ff1-bde3f3cafc5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 3184ec00d089613e25c62762f93043cb4ba68394
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-usage-statistics-with-azure-cdn-advanced-http-reports"></a>Használati statisztikák elemezheti Azure CDN speciális HTTP-jelentések
## <a name="overview"></a>Áttekintés
Ez a dokumentum ismerteti a Microsoft Azure CDN speciális HTTP jelentéskészítés. Ezek a jelentések részletes információkkal CDN tevékenység.

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Speciális HTTP-jelentések használata
1. A CDN-profil panelje hello, kattintson a hello **kezelése** gombra.
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-advanced-http-reports/cdn-manage-btn.png)
   
    Megnyílik a hello CDN felügyeleti portálon.
2. Hello az egérmutatót **Analytics** fülre, majd az egérmutatót hello **speciális HTTP-jelentések** menü.  Kattintson a **HTTP nagy Platform**.
   
    ![CDN management portal - jelentések Speciális menüben](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)
   
    A jelentés beállítások jelennek meg.

## <a name="geography-reports-map-based"></a>A földrajzi jelentések (térkép-alapú)
A térkép tooindicate hasznát hello régiók, amelyről a tartalmat kért öt jelentés áll. Ezek a jelentések a következők: globális térkép, az Amerikai Egyesült Államok térkép, Kanada térkép, Európa térkép és Ázsia Csendes-óceáni térkép.

Az egyes térkép-alapú jelentések rangsorolja földrajzi entitások (azaz országok, állapotok és tartományok) régió származó találatok toohello százaléka alapján történik. Emellett a térkép kerül a hello helyek, amelyről a tartalmat kért toohelp megjelenítése. Képes toodo, minden egyes régió színkódolás által toohello mennyisége igény szerint észlelt az adott régióban. Bárka árnyékolt régiók a tartalom alacsonyabb igény szerint azt jelzik, amíg sötétebb régiók jelzi a tartalmak iránti igénye nagyobb mértékű.

Minden egyes régió forgalom és a sávszélesség tájékoztatáshoz közvetlenül hello térkép alább. Ez lehetővé teszi tooview hello teljes száma a találatok, találatok hello százaléka, hello teljes (gigabájtban) átvitt adatok mennyiségét, és az átvitt adatok hello százaléka mindegyik régióhoz. Megtekintheti a leírásukat az egyes metrikákat. Végül ha egy régiót (azaz ország, állam vagy megye) mutat, hello nevét és a találatok hello régióban történt hello százalékát jelenik meg elemleírásként.

Egy rövid leírást az egyes földrajzi térképen-alapú jelentés alább.

| Jelentés neve. | Leírás |
| --- | --- |
| A globális térkép |Ez a jelentés lehetővé teszi tooview hello világszerte igény szerint a CDN-tartalom. Az egyes országok Itt a színkódolás hello world térkép tooindicate hello százalékán régió származó találatok. |
| Az Amerikai Egyesült Államok térkép |Ez a jelentés lehetővé teszi a CDN-tartalom tooview hello igény szerint az Amerikai Egyesült Államokban hello. Az egyes a térkép tooindicate hello százalékán régió származó találatok színekkel jelölve témájával. |
| Kanadában térkép |Ez a jelentés lehetővé teszi a CDN-tartalom Kanadában tooview hello igény szerint. Minden tartomány a térkép tooindicate hello százalékán régió származó találatok színekkel jelölve témájával. |
| Európa térkép |Ez a jelentés lehetővé teszi a CDN-tartalom Európában tooview hello igény szerint. Az egyes országok a térkép tooindicate hello százalékán régió származó találatok színekkel jelölve témájával. |
| Ázsia Csendes-óceáni térkép |Ez a jelentés lehetővé teszi a CDN-tartalom Ázsiában tooview hello igény szerint. Az egyes országok a térkép tooindicate hello százalékán régió származó találatok színekkel jelölve témájával. |

## <a name="geography-reports-bar-charts"></a>A földrajzi jelentések (sávdiagramok)
Nincsenek két további jelentéseket statisztikai szerint toogeography, felső városokat és felső országokban. Ezek a jelentések rangsorolja városokat és országok, toohello azokban a régiókban származó találatok száma alapján történik. Az ilyen típusú jelentés generálásához, akkor a sávdiagram jelzi, hello felső 10 várost vagy országok, amely a kért tartalom egy adott platform keresztül. A sávdiagram lehetővé teszi a tooquickly értékeléséhez hello régiók, amelyek létrehozzák hello legnagyobb száma a tartalomhoz.

hello bal oldalán hello graph (y-tengely) azt jelzi, hogy hány találatok történt hello megadott régión belül. Közvetlenül a hello grafikon (x-tengely) alatt található címke hello felső 10 régióban.

### <a name="using-hello-bar-charts"></a>Hello sávdiagramok használatával
* Ha az egérmutatót egy sávot, hello nevét és hello hello régióban történt találatok száma jelenik meg elemleírásként.
* hello hozzájuk tartozó hello felső városokat jelentés nevét, az állam/megye és ország rövidítése által a város azonosítja.
* Ha hello vagy régiót (azaz, az állam/megye), amelyből a kérelem forrása nem állapítható meg, majd azt jelzi, hogy azok ismeretlen. Ha hello ország ismeretlen, akkor a két kérdőjel (azaz??), fog megjelenni.
* A jelentésben metrikák "Európa" vagy "Ázsia/Csendes-óceáni terület." hello Ezen elemekre nem jogosultak az összes IP-címek ilyen régiókban tooprovide statisztikai adatokat. Ehelyett csak vonatkoznak, amelyek helyezkednek el Európa vagy Ázsia/Csendes-óceáni tooa adott város vagy ország helyett IP-címekről érkeznek toorequests.

hello adat használt toogenerate hello sávdiagram alatt tekintheti meg. Nem található hello találatok, hello százalékos hello (gigabájtban) átvitt adatok mennyiségét, a találatok száma és az átvitt adatok hello százalékos felső 250 régiók hello. Megtekintheti a leírásukat az egyes metrikákat.

Rövid leírása az alábbi jelentések mindkét típusú valósul meg.

| Jelentés neve. | Leírás |
| --- | --- |
| Felső város tartozik. |Ez a jelentés rangsorolja városokat toohello régió származó találatok száma alapján történik. |
| Felső országok |Ez a jelentés rangsorolja országok toohello régió származó találatok száma alapján történik. |

## <a name="daily-summary"></a>Napi összegzése
hello napi összegző jelentés lehetővé teszi azon találatok és a naponta egy adott platformon keresztül továbbított adatok tooview hello száma. Ez az információ használható tooquickly megfejteni a behatolók CDN tevékenység kombinációját. Például a jelentés segítségével napokat tapasztalt magasabb vagy alacsonyabb, mint a várt forgalom észlelésére.

Esetén az ilyen típusú jelentés létrehozása, a sávdiagram nyújtják platform-specifikus igényű tapasztalt toohello összegként vizuális jelzést naponta keresztül hello hello jelentés által lefedett időszakot. Azt fogja ehhez a sáv megjelenítése minden nap hello jelentésben. Például a neve "Elmúlt hét" hello időszak kiválasztása a sávdiagram hét sávokkal hoz létre. Minden egyes sáv jelzi az adott napon észlelt találatok hello teljes száma.

hello bal oldalán hello graph (y-tengely) azt jelzi, hány találatok történt hello megadott dátum. Közvetlenül hello grafikon (x-tengely) alatt található hello dátumát címkéjét (formátum: éééé-hh-nn) hello jelentésben szereplő minden nap.

> [!TIP]
> Ha az egérmutatót egy sávot, elemleírásként hello által történt a dátumnak a találatok száma jelenik meg.
> 
> 

hello adat használt toogenerate hello sávdiagram alatt tekintheti meg. Nem található hello találatok összesített száma és hello (gigabájtban) átvitt adatok mennyiségét hello jelentés által lefedett minden nap.

## <a name="by-hour"></a>Óránként
hello óra által a jelentés lehetővé teszi tooview hello találatok és száma óránként egy adott platformon keresztül továbbított adatok. Ez az információ használható tooquickly megfejteni a behatolók CDN tevékenység kombinációját. Például a jelentés segítségével hello időszakok hello nap során, hogy a magasabb vagy alacsonyabb, mint a várt forgalom észlelésére.

Esetén az ilyen típusú jelentés létrehozásakor, a sávdiagram észlelt óránként keresztül hello hello jelentés által lefedett időszakot platform-specifikus igényű toohello összegként vizuális jelzést ad meg. Azt fogja ehhez minden órában hello jelentés által lefedett sáv megjelenítése. Például kiválasztása egy 24 órás időszakban hoz létre a sávdiagram erdőtopológia kezelésre négy sávokkal. Minden egyes sáv jelzi, hogy órán belül észlelt találatok hello teljes száma.

hello bal oldalán hello graph (y-tengely) azt jelzi, hogy hány találatok hello megadott órát történt. Közvetlenül alatt hello graph (x-tengely), dátum/idő/hello jelző címke található (formátum: éééé-hh-nn óó: PP) számára hello jelentésben szereplő minden egyes órában. Időt jelenti a 24 órás formátumban, és segítségével hello UTC/GMT időzóna van megadva.

> [!TIP]
> Ha az egérmutatót egy sávot, hello előfordult adott időtartam alatt a találatok száma elemleírásként jelenik meg.
> 
> 

hello adat használt toogenerate hello sávdiagram alatt tekintheti meg. Nincs hello találatok összesített száma és talál hello (gigabájtban) átvitt adatok mennyisége az hello jelentés által lefedett óránként.

## <a name="by-file"></a>Fájl segítségével
a kért eszközök hello által fájl a jelentés lehetővé teszi, hogy egy adott platform hello alatt leggyakrabban felmerülő meg tooview hello igény szerint és hello forgalom mennyisége. Az ilyen típusú jelentés generálásához, akkor a sávdiagram jön létre, hello felső 10 leginkább kért eszközök keresztül megadott időszakban hello.

> [!NOTE]
> Ez a jelentés hello céljából a peremhálózati CNAME URL-címei konvertált tootheir egyenértékű CDN URL-címeket. Ez lehetővé teszi, hogy egy pontos egyeztetési hello találatok összesített száma a társított hello CDN vagy peremhálózati CNAME használt URL-cím toorequest függetlenül az eszköz azt.
> 
> 

hello bal oldalán hello graph (y-tengely) keresztül megadott időszakban hello hello minden eszköz-kérelmek számát jelzi. Közvetlenül hello grafikon (x-tengely) alatt található egy címke, amely minden hello felső 10 kért eszközhöz hello fájl nevét jelöli.

hello adat használt toogenerate hello sávdiagram alatt tekintheti meg. Hiba a következő információkat az egyes hello felső 250 kért eszközök hello megtalálja: relatív elérési út, hello száma találatok, a találatok hello százaléka, a hello továbbított adatmennyiséget (gigabájtban), és az átvitt adatok hello (%).

## <a name="by-file-detail"></a>Fájl részletei
hello által részletei jelentés lehetővé teszi egy adott eszközhöz egy adott platformon keresztül felmerülő igény szerint és hello forgalom mennyisége tooview hello. Hello: Ez a jelentés legfelső beállítás hello részleteit a fájl. Ezt a beállítást a kiválasztott platformon hello a legtöbbek által kért eszközök listáját tartalmazza. A sorrend toogenerate által részletei jelentés tooselect hello kívánt eszköz hello fájl részletei a beállítás a kell. Amely után a sávdiagram napi igény keresztül megadott időszakban hello okozó hello mennyisége jelzi.

hello bal oldalán hello graph (y-tengely) azt jelzi, hogy hello, hogy egy eszköz észlelt egy adott napon kérelmek teljes száma. Közvetlenül hello grafikon (x-tengely) alatt található hello dátumát címkéjét (formátum: éééé-hh-nn) mely CDN igény hello az eszköz történt.

hello adat használt toogenerate hello sávdiagram alatt tekintheti meg. Nem található hello találatok összesített száma és hello (gigabájtban) átvitt adatok mennyiségét hello jelentés által lefedett minden nap.

## <a name="by-file-type"></a>Fájltípus
hello fájl típus szerint a jelentés lehetővé teszi, hogy Ön tooview hello igény szerint és hello forgalom mennyisége felmerült a fájl típusa. Az ilyen típusú jelentés generálásához, akkor fánk diagram hello százalékos hello felső 10 fájltípusok állítja elő a találatok jelzi.

> [!TIP]
> Ha az egérmutatót a szelet hello fánk a diagramon, hello Internet médiatípusa, hogy megjelenik-e a fájl típusa elemleírásként.
> 
> 

hello adat használt toogenerate hello fánk diagram alatt tekintheti meg. Nem lesz keresse a hello fájl neve bővítmény/Internet médiatípus, hello találatok összesített száma, a találatok hello százalékos, hello (gigabájtban) átvitt adatok mennyiségét, és hello az egyes hello átvitt adatok százalékos felső 250 fájltípusokat.

## <a name="by-directory"></a>Directory
hello Directory által a jelentés lehetővé teszi egy adott könyvtár tartalmat platformon keresztül felmerülő igény szerint és hello forgalom mennyisége tooview hello. Az ilyen típusú jelentés generálásához, akkor a sávdiagram jelzi, hello hello felső 10 könyvtárakban található tartalom által generált találatok teljes száma.

### <a name="using-hello-bar-chart"></a>Hello sávdiagram használatával
* A sáv rámutat tooview hello relatív elérési út toohello megfelelő könyvtárat.
* Egy könyvtár almappájában tárolt tartalmat nem számít, igény szerinti Directory számításakor. A számítás kizárólag támaszkodik hello hello tényleges könyvtárban tárolt tartalom létrehozott kérelmek száma.
* Ez a jelentés hello céljából a peremhálózati CNAME URL-címei konvertált tootheir egyenértékű CDN URL-címeket. Ez lehetővé teszi, hogy az összes statisztika pontos egyeztetési egy eszközhöz társított hello CDN vagy peremhálózati CNAME használt URL-cím toorequest függetlenül azt.

hello hello graph (y-tengely) bal oldalán jelzi hello teljes számát a felső 10 címtárakban tárolt hello a tartalomhoz. Minden egyes hello diagram sávján egy könyvtárat jelöl. Sáv fel a rendszer toomatch színkódolás használata hello hello felső 250 teljes könyvtárak szakaszban felsorolt tooa könyvtár.

hello adat használt toogenerate hello sávdiagram alatt tekintheti meg. Hiba a következő információkat az egyes hello felső 250 könyvtárak hello megtalálja: relatív elérési út, hello száma találatok, a találatok hello százaléka, a hello továbbított adatmennyiséget (gigabájtban), és az átvitt adatok hello (%).

## <a name="by-browser"></a>Böngésző
hello böngésző által a jelentés lehetővé teszi a böngészők felmérésére volt használt toorequest tartalom tooview. A kördiagram esetén az ilyen típusú jelentés generálásához, hello felső 10 böngészők által kezelt kérelem hello százaléka jelzi.

### <a name="using-hello-pie-chart"></a>A tortadiagram hello használata
* A hello kördiagram tooview szelet rámutat egy böngésző neve és verziója.
* Ez a jelentés hello céljából minden egyedi böngészőverzió kombinációja tekinthető egy másik böngészőben.
* "Egyéb" nevű hello szelet azt jelzi, hogy minden más böngészők és verziók által kezelt kérelem hello százaléka.

hello adat használt toogenerate hello kördiagram alatt tekintheti meg. Nincs megtalálja hello böngésző típusa/verziószámot, hello találatok összesített száma és az egyes hello felső 250 böngészők találatok hello százalékát.

## <a name="by-referrer"></a>Hivatkozó által
hello által hivatkozó jelentés lehetővé teszi tooview hello felső hivatkozó kérelmei toocontent hello kiválasztott platformon. A hivatkozó azt jelzi, hogy egy kérelem lett képzésére szolgáló hello állomásnév. Esetén az ilyen típusú jelentés létrehozásakor, a sávdiagram fogja jelezni, igény szerinti (azaz a találatok) hello mennyisége hello felső 10 hivatkozó kérelmei által generált.

hello bal oldalán hello graph (y-tengely) azt jelzi, hogy hello, hogy egy eszköz észlelt az egyes hivatkozó kérelmek teljes száma. Minden egyes hello diagram sávján egy hivatkozó jelöli. Sáv fel a rendszer toomatch színkódolás használata hello tooa hivatkozó hello felső 250 hivatkozó szakaszban felsorolt.

hello adat használt toogenerate hello sávdiagram alatt tekintheti meg. Nincs hello URL-címe, hello találatok összesített száma, és az egyes hello felső 250 hivatkozó kérelmei generált találatok hello százalékát fogja található.

## <a name="by-download"></a>Letöltés
hello le a jelentés lehetővé teszi a tooanalyze letöltési minták a legtöbbek által kért tartalomhoz. hello felső hello jelentés tartalmazza a sávdiagram, hogy összehasonlítja próbált letöltések hello a befejezett letöltött felső 10 kért eszközök. Minden egyes sáv van színekkel jelölve témájával van toowhether szerint, egy kísérlet letöltési (kék) vagy egy befejezett letöltés (zöld).

> [!NOTE]
> Ez a jelentés hello céljából a peremhálózati CNAME URL-címei konvertált tootheir egyenértékű CDN URL-címeket. Ez lehetővé teszi, hogy az összes statisztika pontos egyeztetési egy eszközhöz társított hello CDN vagy peremhálózati CNAME használt URL-cím toorequest függetlenül azt.
> 
> 

hello bal oldalán hello graph (y-tengely) azt jelzi, hogy minden hello felső 10 kért eszközhöz hello fájlnév. Közvetlenül alatt hello graph (x-tengely) hello teljes próbált/befejezett letöltések számát jelző címke található.

Közvetlenül alatt hello sávdiagram, a következő információk hello jelenik hello felső 250 kért eszközök: relatív elérési utat (például fájlnév), amikor a letöltött toocompletion hello ennyiszer kérték és hello volt hello száma egy teljes letöltés eredményezett kérelmek százalékos aránya

> [!TIP]
> A CDN nem arról tájékoztatja a HTTP-ügyfél (pl. webböngésző) Ha az eszköz teljesen letöltötte. Ennek eredményeképpen tudunk toocalculate, hogy az eszköz teljesen letöltött függően toostatus kódok és bájttartomány kérelmeket. hello thing először azt keresni, amikor a számítás elvégzése e 200 OK állapotkód hello kérelem eredménye. Ha igen, azt tekintse meg bájttartomány kérelmek tooensure, hogy hello teljes eszköz terjed ki. Végül azt összehasonlítása hello mennyisége hello kért eszköz toohello átvitt adatok mérete. Ha hello esetén az átvitt adatmennyiség egyenlő tooor hello fájl mérete nagyobb és hello bájttartomány kérelmek nem az, hogy az eszköz megfelelő, akkor hello találat megszámlálandó teljes letölthető.
> 
> Ez a jelentés jellege interpretive toohello, miatt tartsa szem előtt tartva hello követő pontokat, előfordulhat, hogy az alter hello konzisztencia, és ez a jelentés pontosságára.
> 
> * Forgalmi minták nem tudták pontosan előre jelezni, amikor a felhasználó-ügynökök eltérően viselkednek. Ez nagyobb, mint 100 %-os befejezett letöltési eredményeket hozhat.
> * HTTP progresszív letöltés előnyeit eszközök nem fogják pontosan képviselheti Ez a jelentés. Ez a videó toousers pozicionálási toodifferent pozíciók miatt.
> 
> 

## <a name="by-404-errors"></a>404-es hibák által
hello 404-es hibák által a jelentés lehetővé teszi a tooidentify hello típusú tartalmat, amely legfeljebb ennyi 404-es nem található állapotkódok hello hoz létre. hello felső hello jelentés tartalmazza a sávdiagram hello első 10-eszközök, amelynek a 404-es nem található állapotkódot adott vissza. A sávdiagram összehasonlítja hello kérelmek száma összesen az által, amelyek a 404-es nem található állapotkódja azokat az eszközöket. Minden itt a színkódolás. Egy sárga sáv szolgál, hogy a kérelem hello tooindicate 404-es nem található állapotkód eredményezett. Egy piros sáv használt tooindicate hello kérelmek teljes száma hello eszköz.

> [!NOTE]
> Ez a jelentés hello céljából vegye figyelembe a hello következőket:
> 
> * Találat függetlenül állapotkód eszköz kérésének jelöli.
> * Peremhálózati CNAME URL-címei konvertált tootheir egyenértékű CDN URL-címeket. Ez lehetővé teszi, hogy az összes statisztika pontos egyeztetési egy eszközhöz társított hello CDN vagy peremhálózati CNAME használt URL-cím toorequest függetlenül azt.
> 
> 

hello bal oldalán hello graph (y-tengely) azt jelzi, hogy hello fájlnév minden hello felső 10 kért eszközhöz, amely a 404-es nem található állapotkód eredményezett. Közvetlenül hello grafikon (x-tengely) alatt található címkékhez, amelyek jelzik, hogy hello kérelmek száma összesen és hello kérelmek száma, amelyek a 404-es nem található állapotkód eredményezett.

Közvetlenül alatt hello sávdiagram, a következő információk hello jelenik hello felső 250 kért eszközök: relatív elérési út (többek között a következőket fájl nevét), hello kérelmek száma, amelyek a 404-es nem található állapotkód, hello összesített száma, amelyek eszköz hello eredményezett volt a kért, és a kérelem a 404-es nem található állapotkód eredményező százaléka hello.

## <a name="see-also"></a>Lásd még:
* [Az Azure CDN áttekintése](cdn-overview.md)
* [A Microsoft Azure CDN valós idejű statisztikák](cdn-real-time-stats.md)
* [Alapértelmezett HTTP működés használata hello szabályok felülbírálása](cdn-rules-engine.md)
* [Peremhálózati teljesítményének elemzése](cdn-edge-performance.md)

