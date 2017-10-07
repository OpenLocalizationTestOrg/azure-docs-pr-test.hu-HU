---
title: "Azure-írásvédett Georedundáns tárolás (RA-GRS) használatával magas rendelkezésre álló alkalmazások aaaDesigning |} Microsoft Docs"
description: "Hogyan toouse Azure-RA-GRS tárolási tooarchitect egy magas rendelkezésre állású rugalmas alkalmazás elég toohandle kimaradások esetén."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 8f040b0f-8926-4831-ac07-79f646f31926
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 1/19/2017
ms.author: robinsh
ms.openlocfilehash: e4a9fe7ef33eecd894408b3c1ef59920a248d1bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="designing-highly-available-applications-using-ra-grs"></a>RA-GRS használatával magas rendelkezésre álló alkalmazások megtervezése

A felhőalapú infrastruktúrák közös szolgáltatása a magas rendelkezésre állású platform biztosítják az alkalmazások tárolására szolgáló. A felhőalapú alkalmazások fejlesztők figyelembe kell vennie gondosan hogyan tooleverage a platform toodeliver magas rendelkezésre állású alkalmazások tootheir felhasználók. Ez a cikk kifejezetten a fejlesztők használatát hello Azure Storage írásvédett Georedundáns redundáns tárolás (RA-GRS) toomake foglalkozik a több elérhető alkalmazások.

A redundancia érdekében – LRS (helyileg redundáns tárolás), a zrs-t (zóna redundáns tárolás), a GRS (Georedundáns tárolás) és az RA-GRS (írásvédett Georedundáns tárolás) négy lehetősége van. Toodiscuss GRS és az RA-GRS ebben a cikkben fogjuk. A GRS az adatok három példányban tárolják hello storage-fiók beállítása során kiválasztott hello elsődleges régióban. Három további másolatokat aszinkron módon karbantartása az Azure által megadott másodlagos régióba. RA-GRS van hello ugyanaz, mint a Georedundáns, azzal a különbséggel, hogy a felhasználó rendelkezik olvasási jogosultsággal toohello másodlagos másolatával. Hello különböző Azure Storage redundancia beállításokkal kapcsolatos további információkért lásd: [Azure Storage replikációs](https://docs.microsoft.com/en-us/azure/storage/storage-redundancy). hello replikációs cikket is hello párosítása hello elsődleges és másodlagos régiók jeleníti meg.

Nincsenek kódtöredékek szerepelni fog ebben a cikkben, és egy hivatkozás tooa teljes mintát, töltse le, és futtassa hello végén.

## <a name="key-features-of-ra-grs"></a>RA-GRS a kulcsfontosságú szolgáltatásokat

Előtt kapcsolatos döntésről toouse RA-GRS storage most szolgáltatással kapcsolatban a tulajdonságok és viselkedését.

* Az Azure Storage fenntart egy csak olvasható hello adatokat tárolja, az elsődleges régió egy másodlagos régióban; a fentiek szerint hello társzolgáltatás hello másodlagos régióba hello helyét határozza meg.

* hello írásvédett másolat [idővel konzisztenssé](https://en.wikipedia.org/wiki/Eventual_consistency) hello elsődleges régióban hello adatokkal.

* A blobot, táblát és üzenetsort, másodlagos régió hello lekérheti a *utolsó szinkronizálás* érték, amely jelzi, hogy mikor történt a legutóbbi replikáció hello hello elsődleges toohello másodlagos régióba. (Ez nem támogatott az Azure File storage, amely jelenleg nem rendelkezik az RA-GRS redundancia.)

* A Storage ügyféloldali kódtára toointeract hello vagy hello elsődleges vagy másodlagos régióban hello adatokkal is használhatja. Is átirányíthatja olvasási kérések automatikusan toohello másodlagos régióba Ha egy olvasási kérést toohello elsődleges régió időkorlátja lejár.

* Egy hello kisegítő hello adatok hello elsődleges régióban érintő jelentős probléma esetén hello Azure csapata indíthatnak földrajzi-feladatátvétel, amikor hello DNS-bejegyzések toohello elsődleges régió mutató lesz módosított toopoint toohello másodlagos régióba.

* Egy földrajzi feladatátvétel esetén Azure lesz jelöljön ki egy új másodlagos helyet hello adatok toothat helyre replikálni, majd hello másodlagos DNS-bejegyzések tooit mutasson. hello másodlagos végpont nem érhető el, amíg hello tárfiók replikálása befejeződött. További információkért lásd: [egy Azure Storage tervezett kimaradás esetén milyen toodo](https://docs.microsoft.com/en-us/azure/storage/storage-disaster-recovery-guidance).

## <a name="application-design-considerations-when-using-ra-grs"></a>RA-GRS használatakor alkalmazás kialakítási szempontok

Ez a cikk hello fő célja tooshow, hogyan toodesign toofunction továbbra is (bár a korlátozott kapacitás) még akkor is, az elsődleges adatok hello jelentős katasztrófa következik hello esemény alkalmazás center. Ehhez az alkalmazás toohandle átmeneti vagy hosszú futású problémák rendelkező hello másodlagos régióban való váltás tooread, ha probléma van, és mód visszakapcsolását hello elsődleges régióban elérhető esetén újra.

### <a name="using-eventually-consistent-data"></a>Idővel konzisztenssé adatok használata

Ez a javasolt megoldás feltételezi, hogy azt rendben tooreturn mi elavult adat toohello hívó alkalmazás lehet. Mivel hello másodlagos adatok idővel konzisztenssé, akkor előfordulhat, hogy hello adatok bejegyzésre kerültek-toohello elsődleges, de hello frissítés toohello másodlagos kellett nem fejezte be a replikálása, amikor elsődleges régió hello elérhetetlenné vált.

Például az ügyfél sikerült elküldeni a sikeres frissítést, és elsődleges hello sikerült folytassa hello frissítés csak másodlagos propagált toohello. Ebben az esetben hello ügyfél kéri tooread hello vissza adatokat, részesül hello elavult adat frissítése hello adatok helyett. Döntse el, ha ez elfogadható, és ha igen, hogyan hello ügyfél fog üzenet. Láthatja, hogyan toocheck hello utolsó szinkronizálás hello másodlagos lévő adatokon, később ezen cikk toosee másodlagos hello naprakész esetén is.

### <a name="handling-services-separately-or-all-together"></a>Kezelési szolgáltatások, külön-külön vagy együtt

Amíg nem valószínű, lehetőség egy szolgáltatás toobecome nem érhető el a míg hello más szolgáltatások továbbra is teljes körűen működik. Akkor is leíró hello újrapróbálkozások és írásvédett módban az egyes szolgáltatási külön-külön (BLOB, üzenetsorok, táblák), vagy minden hello tárolási szolgáltatások általános újrapróbálásainak együtt kezelheti.

Például ha az alkalmazás üzenetsorokat és blobokat használ, dönthet külön kódot toohandle Újrapróbálkozást lehetővé tevő hibák tooput minden egyes. Majd ha ismételt próbálkozással lekérni hello blob szolgáltatástól, de hello queue szolgáltatás továbbra is működik, az alkalmazás, amely kezeli a blobok csak hello részét csökkenhet. Ha úgy dönt, minden társzolgáltatás újrapróbálja általános toohandle és egy hívás toohello blob szolgáltatás egy újrapróbálkozást lehetővé tevő hibát ad vissza, majd kérelmek tooboth hello blob és hello várólista szolgáltatás csökkenhet.

Végül Ez függ az alkalmazás hello összetettsége. Nem toohandle hello hibák szolgáltatás döntse el, de ehelyett tooredirect olvasási kérések összes tárolási szolgáltatások toohello másodlagos régióba és hello alkalmazás futtatásához csak olvasható módban, ha névváltozást észlel a társzolgáltatás probléma hello elsődleges régióban.

### <a name="other-considerations"></a>Egyéb szempontok

A rendszer hello egyéb szempontok, ez a cikk többi hello ismertetik.

*   Az olvasási kérések hello áramköri megszakító minta használatával újrapróbálkozások kezelése

*   Idővel konzisztenssé adatok és hello utolsó szinkronizálás

*   Tesztelés

## <a name="running-your-application-in-read-only-mode"></a>Az alkalmazás csak olvasható módban fut

toouse RA-GRS tárolási, képes toohandle mindkét sikertelen olvasási kéréseket kell lenniük, és nem sikerült a frissítési kérelmek (ebben az esetben a beszúrások, a frissítések és törlések tehát frissítés). Ha hello elsődleges data center meghiúsul, olvassa el a kérelmek lehet átirányított toohello másodlagos adatközpont, de a frissítési kérelmek nem lehetséges, mert hello másodlagos csak olvasható. Emiatt kell néhány módon toorun az alkalmazás csak olvasható módban.

Megadhatja például, hogy a jelzőt, amely minden frissítési kérelmek toohello társzolgáltatás elküldés előtt ellenőrizni kell. Ha egy hello frissítési kérelmek, hagyja ki, és térjen vissza az ügyfél egy megfelelő választ toohello. Akkor is szeretné, hogy toodisable bizonyos funkciók elemet hello probléma megoldásáig, és értesítse a felhasználókat, hogy ezek nem átmenetileg nem érhető el.

Ha külön-külön toohandle hibák az egyes szolgáltatásokhoz, is szüksége lesz toohandle hello képességét toorun az alkalmazás csak olvasható módban szolgáltatás. Csak olvasható jelzők minden egyes szolgáltatás engedélyezhető legyen, és le van tiltva, és kezelni hello megfelelő jelző hello megfelelő helyen, a kódban.

Képes toorun alatt az alkalmazás csak olvasható módban van egy másik oldalán juttatás – biztosít hello képességét tooensure korlátozott üzemmódban a súlyos alkalmazás frissítése során. Indíthat el az alkalmazás toorun csak olvasható módban és pont toohello másodlagos adatközpont, biztosítva, hogy senki sem adataihoz fér hozzá hello hello elsődleges régió frissítések létrehozása idejére.

## <a name="handling-updates-when-running-in-read-only-mode"></a>Frissítések kezelése, csak olvasható módban történő futtatásakor

Számos módon toohandle frissítési kérelmek csak olvasható módban történő futtatásakor. Átfogó azt ne fedje ez, de általában többféle annak a figyelembe venni.

1.  Tooyour felhasználói válaszol, és közölje vele, jelenleg nem elfogadja frissítések. Például a kapcsolattartási rendszer sikerült engedélyezése az ügyfelek tooaccess kapcsolattartási adatokat, de nem a módosításokat.

2.  A frissítések, egy másik régióban is sorba helyezni. Ebben az esetben, ehhez írja be a függőben lévő frissítési kérelmek tooa sor egy másik régióban található, és már rendelkezik egy módon tooprocess ezeket a kérelmeket, miután újra online állapotba hello elsődleges adatközpont kerül. Ha ebben a forgatókönyvben hagyja, hello ügyfél tudja, hogy a kért hello update várakozik későbbi feldolgozás céljából.

3.  Írhat a frissítések tooa tárfiók más régióban. Majd ha hello elsődleges adatközpont ismét online elérhető lesz, akkor egy módon toomerge hello elsődleges adatokká, attól függően, hogy hello adatok szerkezete hello frissítések. Például külön fájlok hello nevét egy dátum-/ időbélyeg létrehozásakor, másolhatja ezen fájlok hátsó toohello elsődleges régióban. Ez a módszer egyes munkaterhelések, például a naplózás és az iOT.

## <a name="handling-retries"></a>Az újrapróbálkozások kezelése

Hogyan tudja megállapítani, mely hibák Újrapróbálkozást lehetővé tevő? Ez határozza meg hello a storage ügyféloldali kódtára. Például 404 hibaüzenetet (az erőforrás nem található) nincs Újrapróbálkozást lehetővé tevő mert újrapróbálkozás, nem valószínű tooresult a sikeres. A hello, ugyanakkor 500 hiba oka az újrapróbálkozást lehetővé tevő kiszolgálóhiba, és egyszerűen lehet, hogy átmeneti jellegű probléma. További részletekért tekintse meg a hello [nyissa meg a hello ExponentialRetry osztály forráskódja](https://github.com/Azure/azure-storage-net/blob/87b84b3d5ee884c7adc10e494e2c7060956515d0/Lib/Common/RetryPolicies/ExponentialRetry.cs) hello .NET a storage ügyféloldali kódtára a. (Hello ShouldRetry metódus keressen.)

### <a name="read-requests"></a>Olvasási kérések

Olvasási kérések lehet átirányított toosecondary tárolási, ha az elsődleges storage probléma van. Szerint azt a fentiekben leírtuk a [idővel azonos adatokat használó](#using-eventually-consistent-data), azt kell elfogadható az alkalmazás toopotentially elavult adatokat olvasni. Ha hello tárolási ügyfél könyvtár tooaccess RA-GRS adatokat használ, hello újrapróbálási viselkedése olvasási kérelem hello értékének beállításával megadhatja **LocationMode** tulajdonság tooone hello alábbi:

*   **PrimaryOnly** (hello alapértelmezett)

*   **PrimaryThenSecondary**

*   **SecondaryOnly**

*   **SecondaryThenPrimary**

Hello beállításakor **LocationMode** túl**PrimaryThenSecondary**, ha hello kezdeti olvasási kérelem toohello elsődleges végpont Újrapróbálkozást lehetővé tevő hiba miatt nem sikerül, akkor hello ügyfél automatikusan lehetővé teszi egy másik olvasása másodlagos végponti toohello kérelmet. Ha hello hiba: a kiszolgáló időkorlátja, majd hello ügyfél lesz a hello időtúllépés tooexpire toowait előtt hello szolgáltatás megkapja a Újrapróbálkozást lehetővé tevő hiba.

Amikor arról dönt van alapvetően két esetben tooconsider hogyan toorespond tooa Újrapróbálkozást lehetővé tevő hiba:

*   Ez egy elkülönített probléma és kérelmeknél toohello elsődleges végpont nem ad vissza egy újrapróbálkozást lehetővé tevő hiba. Egy példa, ahol ez akkor fordulhat elő, amikor egy átmeneti hálózati hiba.

    Ilyen esetben van nincs jelentős teljesítményét rendelkező **LocationMode** túl beállítása**PrimaryThenSecondary** , ez csak akkor fordul elő ritkán.

*   A probléma legalább egy hello tárolószolgáltatások hello elsődleges régióban és minden későbbi kérelmeket toothat szolgáltatás hello elsődleges régióban valószínűleg tooreturn Újrapróbálkozást lehetővé tevő hibák egy ideig. Egy példa erre, ha hello elsődleges régió nem teljesen érhető el.

    Ebben a forgatókönyvben nincs rendszer teljesítményét, mert az olvasási kéréseket fog előbb a hello elsődleges végpont, hello időtúllépés tooexpire várja meg, majd toohello másodlagos végponti váltani.

Ezek a forgatókönyvek esetén meg kell határoznia, amely nincs hello elsődleges végpont folyamatban lévő problémát, és küldése az összes olvasási kérések közvetlen toohello másodlagos végpont úgy, hogy hello **LocationMode** tulajdonság túl **SecondaryOnly**. Ilyenkor meg kell változtatni hello alkalmazás toorun csak olvasható módban is. Ezt a módszert nevezik hello [áramköri megszakító mintát](https://msdn.microsoft.com/library/dn589784.aspx).

### <a name="update-requests"></a>Frissítési kérelmek

hello áramköri megszakító mintát alkalmazott tooupdate kérelmeket is lehet. Azonban a frissítési kérelmet nem lehet átirányított toosecondary tárolóról, ami csak olvasható. Ezeket a kéréseket a hello hagyja **LocationMode** tulajdonsága túl**PrimaryOnly** (hello alapértelmezett). toohandle ezeket a hibákat a metrika toothese kérések – például olyan sorok esetén – 10 hibák alkalmazni, és a küszöbérték teljesülésekor váltás hello alkalmazás csak olvasható módba. Használhat hello ugyanazokat a módszereket tooupdate üzemmódban, mint a következő szakaszban hello hello áramköri megszakító mintát kapcsolatos az alábbiakban visszaküldésére használatos.

## <a name="circuit-breaker-pattern"></a>Áramköri megszakító minta

Hello áramköri megszakító minta használatával az alkalmazás képes megakadályozni azt egy művelet várható toofail ismételten megpróbálná. Ezen idő foglalnak, amíg hello műveletet a rendszer ismét megkísérli exponenciálisan helyett hello alkalmazás toocontinue toorun lehetővé teszi. Azt is észleli, ha hello hiba kijavítása, amelynél hello alkalmazása próbálkozhat hello műveletet.

### <a name="how-tooimplement-hello-circuit-breaker-pattern"></a>Hogyan tooimplement hello áramköri megszakító minta

tooidentify elsődleges végpont folyamatban lévő hibát, a figyelheti, hogy milyen gyakran hello ügyfél Újrapróbálkozást lehetővé tevő hibát észlel. Mivel minden egyes eset különböző, hogy rendelkezik-e toodecide hello küszöbérték a toouse szánt hello döntési tooswitch toohello másodlagos végponti és hello alkalmazás futtatásához csak olvasható módban. Például sikertelen eldöntheti, tooperform hello kapcsoló Ha 10 hibák szerepel egy nem sikeres rendelkező sor. Egy másik példa tooswitch esetén 90 %-a 2 perces időtartamra hello kérelem nem.

A hello első esetben egyszerűen egy hello hibáinak száma tárolhatja, és ha elérése előtt sikeres hello maximális, beállíthatja hello száma hátsó toozero. Hello a második forgatókönyvben toouse egyirányú tooimplement hello MemoryCache objektumban (.NET). Az egyes kérelmek CacheItem toohello gyorsítótár hozzáadása, hello érték toosuccess (1) beállítása sikertelen (0), illetve hello lejárati idő too2 perc beállítása most (vagy bármilyen az időkorlát). Egy bejegyzést lejárati idő elérésekor a rendszer automatikusan eltávolította a hello bejegyzést. Ez biztosítja a működés közbeni 2 perces ablak. Minden egyes kérelem toohello tárolási szolgáltatásként, akkor először használja a Linq lekérdezés hello MemoryCache objektum toocalculate hello százalékos sikerességi hello értékek megengedő és hello számát elosztjuk. Amikor hello százalékos sikerességi bizonyos küszöb (például 10 %) alá csökken, állítsa be a hello **LocationMode** tulajdonsága túl az olvasási kérésekre**SecondaryOnly** és váltás hello alkalmazás csak olvasható módba előtt a folytatás.

hibák hello küszöbértéket toodetermine használatos toomake hello kapcsoló eltérőek lehetnek az alkalmazás szolgáltatási tooservice, érdemes lehet minősítené konfigurálható paraméterek. Ez akkor is, ha úgy dönt, toohandle Újrapróbálkozást lehetővé tevő észlelt hibákat az egyes szolgáltatások külön vagy számít, korábban bemutatott.

Meg kell vizsgálni, hogyan toohandle több példányát egy alkalmazást, és milyen toodo, ha észleli a Újrapróbálkozást lehetővé tevő hibákat minden egyes példányában. Például előfordulhat, hogy fut az alkalmazás betöltése hello 20 virtuális gép. Tegye mindegyik példány külön-külön kezeli? Ha egy példány indításakor problémák, így szeretné toolimit hello válasz toojust egy példányhoz, vagy az tootry toohave összes példányát a válaszolnak hello azonos módon, ha probléma van egy példány? Hello példányok külön kezelése sokkal egyszerűbb, mint a toocoordinate hello válasz közben ezek között, de ennek módja az alkalmazás architektúra függ.

### <a name="options-for-monitoring-hello-error-frequency"></a>Figyelheti hello hiba gyakorisága

A sorrend toodetermine hello elsődleges régióban újrapróbálkozások gyakorisága hello figyelésre, amikor tooswitch keresztül toohello másodlagos régióban, és módosítsa az alkalmazás toorun csak olvasható módban hello három fő lehetőség van.

*   Adja hozzá a kezelőt hello [ **újrapróbálkozás** ](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.operationcontext.retrying.aspx) esemény a hello [ **OperationContext** ](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.operationcontext.aspx) tooyour tárolási kérelmeket át – Ez a hello objektum módszer jelenik meg ebben a cikkben, és a minta kísérő hello. Ezek az események érvényesítést, amikor hello ügyfél kérelmet újrapróbálkozik, így már milyen gyakran tootrack hello ügyfél elsődleges végpont Újrapróbálkozást lehetővé tevő hibát észlel.

    ```csharp 
    operationContext.Retrying += (sender, arguments) =>
    {
        // Retrying in hello primary region
        if (arguments.Request.Host == primaryhostname)
            ...
    };
    ```

*   A hello [ **Evaluate** ](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.evaluate.aspx) egyéni újrapróbálkozási házirendje metódust, bármikor futtatható, egyéni kód ha ismételt próbálkozással kerül sor. Ha ismételt próbálkozással történik, is által biztosított lehetőséget toomodify az újrapróbálási viselkedése hello hozzáadása toorecording a.

    ```csharp 
    public RetryInfo Evaluate(RetryContext retryContext,
    OperationContext operationContext)
    {
        var statusCode = retryContext.LastRequestResult.HttpStatusCode;
        if (retryContext.CurrentRetryCount >= this.maximumAttempts
        || ((statusCode &gt;= 300 && statusCode &lt; 500 && statusCode != 408)
        || statusCode == 501 // Not Implemented
        || statusCode == 505 // Version Not Supported
            ))
        {
        // Do not retry
            return null;
        }

        // Monitor retries in hello primary location
        ...

        // Determine RetryInterval and TargetLocation
        RetryInfo info =
            CreateRetryInfo(retryContext.CurrentRetryCount);

        return info;
    }
    ```

*   hello harmadik módszer olyan egyéni figyelő-összetevőt az alkalmazás, amely folyamatosan Pingeli az elsődleges tárolási végpont helyőrző tooimplement kérések (például Olvasás kis blob) toodetermine állapotára olvassa. A volna tarthat néhány forrás, de nem jelentős időt. Ha a probléma, amely eléri a küszöbérték felderített, majd elvégeznie hello kapcsoló túl**SecondaryOnly** és írásvédett módban.

Egy bizonyos ponton érdemes tooswitch hátsó toousing hello elsődleges végpont és a frissítések engedélyezése. Ha a fent felsorolt hello első két módszer egyikével, sikerült egyszerűen át hátsó toohello elsődleges végpont, és frissítési mód engedélyezése tetszőlegesen kiválasztott mennyi idő vagy a műveletek végrehajtását követően. Majd engedélyezheti azt keresztül hello újrapróbálkozási logika nyissa meg újra. Ha hello ki lett javítva, akkor lesz toouse hello elsődleges végpont továbbra is, és frissítések engedélyezése. Ha a probléma továbbra is van, akkor még egyszer vált vissza toohello másodlagos végponti és írásvédett módban után hello feltételek beállítása sikertelen.

Hello harmadik forgatókönyv, ha a pingelés hello elsődleges tárolási végpont válik sikeres újra, indíthat el hello visszaváltás túl**PrimaryOnly** , és folytassa a frissítések engedélyezése.

## <a name="handling-eventually-consistent-data"></a>Idővel konzisztenssé adatok kezelése

RA-GRS működését tekintve a tranzakciók replikálásához hello elsődleges toohello másodlagos régióba. A replikálási folyamat biztosítja, hogy a másodlagos régióban hello hello adatok *idővel konzisztenssé*. Ez azt jelenti, hogy az összes hello tranzakció hello elsődleges régióban végül megjelennek a hello másodlagos régióban, de lehet egy lag csak akkor jelennek meg, és, hogy nincs garancia hello tranzakciók érkeznek hello azonos sorrendben, mint a hello másodlagos régióban ahol eredetileg kérelmezték, hello elsődleges régióban. Ha a tranzakciók hello másodlagos régióban, sorrendje nem érkeznek meg *előfordulhat, hogy* fontolja meg az adatok hello másodlagos régióba toobe inkonzisztens állapotban, amíg hello szolgáltatás ki.

hello alábbi táblázat szemlélteti, mi történne akkor, amikor egy alkalmazott toomake hello részleteit frissíti rá, hogy hello tagja *rendszergazdák* szerepkör. Hello szakét ebben a példában, az ehhez szükséges frissítenie hello **alkalmazott** entitás és a frissítés egy **rendszergazdai szerepkör** entitás rendszergazdák hello száma számaival együtt. Figyelje meg, hogyan hello frissítéseket alkalmazza a rendszer nem megfelelő sorrendben hello másodlagos régióban.

| **Idő** | **Tranzakció**                                            | **Replikáció**                       | **Utolsó szinkronizálás** | **Eredménye** |
|----------|------------------------------------------------------------|---------------------------------------|--------------------|------------| 
| T0       | Tranzakció A: <br> Alkalmazott beszúrása <br> az elsődleges entitás |                                   |                    | A tranzakció tooprimary, szúrja be.<br> még nem replikált. |
| A T1       |                                                            | A tranzakció <br> replikált<br> másodlagos | A T1 | A tranzakció toosecondary replikálja. <br>Utolsó szinkronizálás ideje frissíteni.    |
| T2       | B tranzakció<br>Frissítés<br> alkalmazott entitás<br> az elsődleges  |                                | A T1                 | Tranzakció B tooprimary, írása<br> még nem replikált.  |
| A T3       | Tranzakció C:<br> Frissítés <br>Rendszergazda<br>a szerepkör entitás<br>elsődleges |                    | A T1                 | Tranzakció tooprimary, írása C<br> még nem replikált.  |
| *T4*     |                                                       | Tranzakció C <br>replikált<br> másodlagos | A T1         | Tranzakció C toosecondary replikálja.<br>Nincs frissítve, mert LastSyncTime <br>B tranzakció még nincs replikálva.|
| *T5*     | Olvassa el az entitások <br>másodlagos                           |                                  | A T1                 | Hello alkalmazott elavult értéket kap <br> entitás, mert a tranzakció B nem <br> még replikált. Hello új értéket kap<br> rendszergazdai szerepkör entitás mert C<br> a rendszer replikálja. Utolsó szinkronizálás ideje még nem<br> lett frissítése, mert a tranzakció B<br> a rendszer nem replikálja. Beállíthatja a<br>rendszergazdai szerepkör entitás inkonzisztens. <br>mivel hello entitás dátum/idő után <br>Utolsó szinkronizálás hello. |
| *T6*     |                                                      | B tranzakció<br> replikált<br> másodlagos | T6                 | *T6* – C – az összes tranzakció van <br>van replikálva, a legutóbbi szinkronizálás ideje<br> frissül. |

Ebben a példában feltételezzük hello ügyfél kapcsolók tooreading hello másodlagos régióban, T5. Akkor is sikeresen beolvasta a hello **rendszergazdai szerepkör** jelenleg entitás, de hello entitás hello rendszergazdák azon számát, amely nincs összhangban hello számú értéket tartalmaz **alkalmazott** entitások a rendszergazdák hello másodlagos régióban, amely van megjelölve, most. Az ügyfél egyszerűen lehetett megjeleníteni ezt az értéket, hello kockázata, hogy a rendszer inkonzisztens adatokat. Másik lehetőségként hello ügyfél kísérletet toodetermine adott hello **rendszergazdai szerepkör** hello sorrendje nem történt, és ezután megadja az hello felhasználói erről a potenciálisan inkonzisztens állapotban van.

arról, hogy rendelkezik-e esetleg ellentmondásos adatok toorecognize, hello ügyfél használhatja hello hello értékének *utolsó szinkronizálás* , hogy kaphat a bármikor a társzolgáltatás lekérdezésével. Igen, akkor hello időpontja hello adatok hello másodlagos régióban utolsó következetes és hello szolgáltatást kellett alkalmazásakor minden hello tranzakciók előzetes toothat pont időben. A hello fenti példában, miután hello szolgáltatás által a hello **alkalmazott** entitás hello másodlagos régióban, hello utolsó szinkronizálás értéke túl*T1*. A leválasztást *T1* amíg hello szolgáltatás hogyan frissíti hello **alkalmazott** entitás hello másodlagos régióban, ha túl*T6*. Ha hello ügyfél lekérdezi a hello utolsó szinkronizálási idő, amikor olvassa be a hello entitás *T5*, azt is összehasonlíthatja hello időbélyeg hello entitás. Ha hello időbélyeg hello entitás későbbre esik, mint hello időpontja, utolsó szinkronizálás majd hello entitás potenciálisan inkonzisztens állapotban van, és is igénybe vehet, függetlenül az hello megfelelő lépéseket az alkalmazás. Ez a mező használatához arról, hogy mikor hello utolsó frissítés toohello elsődleges befejeződött.

## <a name="testing"></a>Tesztelés

Fontos, hogy az alkalmazás Újrapróbálkozást lehetővé tevő hiba várt viselkedik tootest. Például, hogy az alkalmazás kapcsolók toohello másodlagos hello tootest van szüksége, és csak olvasható módba, ha problémát észlel, és váltás amikor hello elsődleges régió ismét elérhetővé válik. toodo, kell egy módon toosimulate Újrapróbálkozást lehetővé tevő hibákat, és szabályozhatja, hogy milyen gyakran előforduló.

Használhat [Fiddler](http://www.telerik.com/fiddler) toointercept, és módosítsa a parancsfájl a HTTP-válaszokat. Ez a parancsfájl azonosítsa az elsődleges végpont érkező válaszokat, és hello HTTP-állapot kódját tooone módosítása, hogy a Storage ügyféloldali kódtára felismeri Újrapróbálkozást lehetővé tevő hiba hello. A kódrészletet mutat be egy egyszerű, amely elfogja a választ tooread kéréseket a meghatározott hello Fiddler parancsfájl **employeedata** tábla tooreturn 502 állapota:

```java
static function OnBeforeResponse(oSession: Session) {
    ...
    if ((oSession.hostname == "\[yourstorageaccount\].table.core.windows.net")
      && (oSession.PathAndQuery.StartsWith("/employeedata?$filter"))) {
        oSession.responseCode = 502;
    }
}
```

Nem sikerült kiterjesztése a példa toointercept kérelmek szélesebb köre, és csak a hello módosítása **responseCode** a némelyikük toobetter szimulálása egy valós forgatókönyv. Fiddler parancsfájlok testreszabásával kapcsolatos további információkért lásd: [módosítását olyan kérésre vagy válaszra](http://docs.telerik.com/fiddler/KnowledgeBase/FiddlerScript/ModifyRequestOrResponse) hello Fiddler dokumentációjában található.

Ha az alkalmazás csak tooread mód konfigurálható átállításához hello küszöbértékeket, könnyebben tootest hello viselkedés nem éles tranzakció kötetekkel lesz.

## <a name="next-steps"></a>Következő lépések

* Olvasási hozzáférés-Georedundancia kapcsolatos további információkért például hogyan hello LastSyncTime be van állítva, egy másik példát talál [Windows Azure tárolási redundancia lehetőségek és az írásvédett Georedundáns redundáns tárolás](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

* Egy teljes mintát jelenít meg, hogyan toomake hello hello elsődleges és másodlagos végpontok közötti oda-vissza kapcsoló, lásd: [Azure-minták – használatával hello áramköri megszakító minta az RA-GRS tárolással rendelkező](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs).
