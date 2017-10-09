---
title: "az Azure Search aaaLucene lekérdezés példák |} Microsoft Docs"
description: "Az intelligens egyeztetésű keresési, közelségi kapcsolat keresési, kifejezés kiemelése, reguláris kifejezés keresési, és helyettesítő karakteres keresés Lucene a lekérdezés szintaxisa."
services: search
documentationcenter: 
author: LiamCa
manager: pablocas
editor: 
tags: Lucene query analyzer syntax
ms.assetid: 147f360d-a5ce-4d7b-a909-c8b65bfb748c
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/21/2017
ms.author: liamca
ms.openlocfilehash: d859cf697ef9485a49e9e0591b68e812ffa55f1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Lucene lekérdezési szintaxis példák az Azure Search lekérdezések létrehozása
Az Azure Search lekérdezések térített használhatja bármelyik hello alapértelmezett [egyszerű lekérdezés szintaxisát](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) vagy hello alternatív [Lucene Lekérdezéselemzőben az Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search). hello Lucene Lekérdezéselemzőben összetett lekérdezési szerkezeteket, például mező hatókörű lekérdezések intelligens egyeztetésű keresési, közelségi kapcsolat keresési, kifejezés kiemelése vagy reguláris kifejezést keresési támogatja.

Ebben a cikkben vehetjük példák hello teljes szintaxis használata esetén, amely tartalmazza a lekérdezési műveletek érhető el.

## <a name="viewing-hello-examples-in-jsfiddle"></a>Hello példák JSFiddle megtekintése

A cikkben szereplő példák hello összes végrehajtható lekérdezések, amelyek egy előre betöltött Search-index futtatni [JSFiddle](https://jsfiddle.net), parancsfájl és HTML teszteléséhez egy online kód szerkesztése. 

toorun őket, kattintson a jobb gombbal a hello lekérdezés példa URL-címek tooopen JSFiddle egy külön böngészőablakban.

> [!NOTE]
> hello alábbi példák kihasználja a rendelkezésre álló feladatok hello által biztosított adatkészlet alapján álló search-index [New York OpenData Város](https://nycopendata.socrata.com/) kezdeményezésére. Ezek az adatok nem tekinthető aktuális vagy teljes. hello indexe egy Microsoft által biztosított védőfal szolgáltatásra. Nem kell egy Azure-előfizetés vagy az Azure Search tootry ezeket a lekérdezéseket.
>


## <a name="how-tooinvoke-full-lucene-parsing"></a>Hogyan tooinvoke teljes Lucene elemzése

Összes hello jelen cikk példái a adja meg a hello **queryType = teljes** keresési feltétel, jelezve, hogy hello teljes szintaxissal hello Lucene Lekérdezéselemzőben kezeli. 

**1. példa** – kattintson a jobb gombbal hello lekérdezés részlet tooopen azt egy új böngészőablakban lapon, hogy a következő betölt JSFiddle, és futtatása lekérdezés hello:

* [& queryType teljes = & Keresés = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

Hello új böngészőablakban hello JavaScript forrás- és a HTML-kimenetében sorrendben egymás mellett. hello parancsfájl (nem csak hello részlet, ahogy az hello hivatkozás) teljes lekérdezés hivatkozik. hello teljes lekérdezés hello URL-címéből minden példa látható. 

Ez a lekérdezés dokumentumok New York City feladatok indexben (nycjobs, a védőfal szolgáltatás betöltött) adja vissza. Kivonatosan mutatja hello lekérdezés határoz meg, csak az üzleti címe ad vissza. hello teljes alapul szolgáló lekérdezés a következőképpen történik:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Hello **searchFields** paraméter hello keresési toojust hello üzleti cím mező korlátozza. Hello **queryType** értéke túl**teljes**, amely arra utasítja az Azure Search toouse hello Lucene Lekérdezéselemzőben ehhez a lekérdezéshez.

> [!NOTE]
> A lekérdezés feldolgozása a háttérben, lásd: [hogyan teljes szöveges keresés működik az Azure Search](search-lucene-query-architecture.md). A keresési paraméterek további információkért lásd: [dokumentumok keresése (Azure Search szolgáltatás REST API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).
>

### <a name="fielded-query-operation"></a>Fielded lekérdezési művelet
A cikkben szereplő példák hello megadásával módosíthatja a **fieldname:searchterm** konstrukció toodefine fielded lekérdezési műveletet, ahol hello mező egyetlen szó, és hello keresési kifejezés is egy szót vagy kifejezést, opcionálisan a logikai operátor. Néhány példa hello következő:

* business_title:(senior NOT junior)
* állapot: ("Győr" és "Új Jersey")

Lehet, hogy tooput idézőjelek között több karakterláncok Ha azt szeretné, hogy mindkét karakterláncok toobe egyetlen egységként, ebben az esetben két különböző város hello hely mezőben keresése hasonlóan értékeli ki. Gondoskodjon arról is, a hello operátor van nagybetű, az nem látható módon és and

hello mező van megadva a **fieldname:searchterm** kereshető mezőnek kell lennie. Lásd: [a Create Index (Azure Search szolgáltatás REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) attribútum használata a definíciók mező leírását.

**2. példa** – kattintson a jobb gombbal hello következő lekérdezés részlet hello üzleti címeket keres távon vezető bennük, de nem kezdő lekérdezés:

* [& queryType teljes = & Keresés = business_title:senior nem kezdő](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="fuzzy-search-example"></a>Intelligens egyeztetésű keresési – példa
Egy intelligens kereséséhez megfelel a feltételeknek, egy hasonló konstrukció rendelkező. / [Lucene dokumentáció](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), intelligens keresések alapuló [Damerau-Levenshtein távolság](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

egy intelligens egyeztetésű keresési toodo hozzáfűzése hello hullámos vonallal "~" szimbólum nem kötelező paraméter, hello Szerkesztés távolság értéke 0 és 2 között egyetlen szó hello végén. Például "kék ~" vagy "kék ~ 1" alakítanák vissza kék kékek és kapcsolás.

**3. példa** – kattintson a jobb gombbal hello következő lekérdezés részlet. Ez a lekérdezés keresi a feladatokat a hello kifejezés társítható (ahol az hibásan van megadva):

* [& queryType teljes = & Keresés = business_title:asosiate ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

> [!Note]
> Intelligens lekérdezések nincsenek [elemzett](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis), amely meglepő, ha várhatóan származó vagy Lemmatizálás lehet. Lexikális elemző csak hajtható végre teljes feltételek (kifejezés lekérdezés vagy kifejezés lekérdezés). Hiányos adatokkal (előtag lekérdezés, helyettesítő karaktereknek, regex lekérdezés, intelligens lekérdezés) lekérdezéstípusok kerülnek közvetlenül toohello lekérdezés konzolfáján hello elemzési fázis kihagyásával. hello csak hiányos lekérdezési kifejezések végzett átalakításában van lowercasing.
>

## <a name="proximity-search-example"></a>Közelségi kapcsolat keresési – példa
Közelségi kapcsolat keresések használt toofind kifejezéssel hasonlókat dokumentumban. Helyezze be a tildével "~" hello végén lévő kifejezést szimbólum hello közelségi kapcsolat határ szóból hello száma követ. Például "Szálloda repülőtéri" egy dokumentumot ~ 5 található hello feltételek Szálloda és repülőtéri belül minden más 5 szavakat.

**4. példa** – kattintson a jobb gombbal hello lekérdezés. Keresse meg a feladatokat a hello kifejezés "vezető elemző", ahol szóközzel elválasztva legfeljebb egy word:

* [& queryType teljes = & Keresés = business_title: "vezető elemző" ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**5. példa** --próbálja ki újra a hello szavakat közötti hello kifejezés "vezető elemző" eltávolítása.

* [& queryType teljes = & Keresés = business_title: "vezető elemző" ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting-examples"></a>Példák kiemelése kifejezés
Egy újabb verzióját, ha hello tartalmaz dokumentum súlyozott kifejezés, relatív toodocuments hello kifejezés nem tartalmazó tooranking kifejezés kiemelése hivatkozik. Ez eltér profilok pontozási abban a pontozási profil növelése az egyes mezők ahelyett, hogy adott kifejezések. hello alábbi példa segíthet hello különbségeket mutatja be.

Vegye figyelembe a pontozási profil növekedhet, megfelel a bizonyos mezőben, például a **genre** hello musicstoreindex példában. Kifejezés kiemelése az oka az lehet, hogy bizonyos keresési feltételek magasabb, mint a többire használt toofurther program. Például "rock ^ 2 elektronikus" hello hello kifejezést tartalmazó dokumentumok fog növelése **genre** magasabb, mint más kereshető mezők hello index mező. Ezenkívül hello keresési kifejezés "rock" tartalmazó dokumentumok fog rangsorolandó magasabb, mint az hello más keresési kifejezés "elektronikus" hello kifejezés program értéket (2) miatt.

a kifejezés, használjon hello kalap, tooboost "^", program tényezővel (szám) keres hello kifejezés hello végén szimbólum. hello magasabb hello program tényező hello több megfelelő hello kifejezés lesz relatív tooother keresési feltételeket. Alapértelmezés szerint a program tényező hello: 1. Hello program tényező pozitívnak kell lennie, bár lehet (például 0,2) kisebb, mint 1.

**6. példa** – kattintson a jobb gombbal hello lekérdezés. Keresse meg a feladatokat a hello kifejezés "számítógép elemző", ahol a szavakat számítógép és az elemzői eredménytelen, de elemző feladatok olyan hello eredmények hello tetején látható.

* [& queryType teljes = & Keresés business_title:computer elemző =](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Példa 7** – próbálja meg újra, az idő kiemelése eredmények hello kifejezés számítógéppel keresztül hello kifejezés elemző mindkét szavak nem léteznek.

* [& queryType teljes = & Keresés = business_title:computer ^ 2 elemző](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression-example"></a>Reguláris kifejezés – példa
A reguláris kifejezés keresési egyezést közötti perjellel "/", mint a dokumentált hello hello tartalom alapján [RegExp szolgáltatást osztály](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

**Példa 8** – kattintson a jobb gombbal hello lekérdezés. Keresési hello kifejezés vezető vagy a beosztott esetében.

* `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

Ebben a példában hello URL-címe nem jelennek meg megfelelően hello lapon. A probléma megoldásához hello URL-címet az alábbi másolja, majd illessze be hello böngésző URL-címet:`http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`

## <a name="wildcard-search-example"></a>Helyettesítő karakteres keresés – példa
Több általában felismerhető szintaxis is használható (\*) vagy egy (?) karakter helyettesítő karakteres kereséssel. Vegye figyelembe a hello Lucene lekérdezés elemző támogatja hello használatát, a szimbólumok egyetlen kifejezés, és nem egy kódot.

**Példa 9** – kattintson a jobb gombbal hello lekérdezés. Keresse meg a feladatokat, amelyekben hello előtag "program" többek között kellene üzleti címek a programozási hello fogalmak és programozói azt.

* [& queryType = teljes & $select = business_title & keresést = business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26queryType=full%26$select=business_title%26search=business_title:prog*)

Nem használhatja a * és? a keresés hello első karakterként szimbólum.

## <a name="next-steps"></a>Következő lépések
Próbáljon meg hello Lucene Lekérdezéselemzőben a kódban. a következő hivatkozások hello azt ismertetik, hogyan .NET-hez és hello REST API lekérdezi-e a Keresés mentése tooset. hello hivatkozások szintaxissal hello alapértelmezett egyszerű, szüksége lesz tooapply adatnézetből származó Ez a cikk toospecify hello **queryType**.

* [A .NET SDK hello használata Azure Search-Index lekérdezése](search-query-dotnet.md)
* [A hello REST API-t használó Azure Search-Index lekérdezése](search-query-rest-api.md)

## <a name="see-also"></a>Lásd még:

 [Hogyan teljes szöveges keresés az Azure Search működik](search-lucene-query-architecture.md)