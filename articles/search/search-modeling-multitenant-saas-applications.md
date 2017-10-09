---
title: "több vállalat kiszolgálása az Azure Search aaaModeling |} Microsoft Docs"
description: "További információk a közös tervminták több-bérlős SaaS-alkalmazásokhoz az Azure Search használata során."
services: search
manager: jhubbard
author: ashmaka
documentationcenter: 
ms.assetid: 72e9696a-553b-47dc-9e05-a82db0ebf094
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/26/2016
ms.author: ashmaka
ms.openlocfilehash: dd46cda772d32566b9aaa18d407f12fdf178bd43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>A kialakítási minták a több-bérlős SaaS-alkalmazásokhoz és az Azure Search
Egy több-bérlős alkalmazás egyike hello biztosító szolgáltatásokat és képességeket tooany ugyanannyi bérlők számára nem látható, vagy a semmilyen más bérlővel hello adatmegosztásra. Ez a dokumentum ismerteti a bérlő elkülönítési stratégiák több-bérlős alkalmazásokhoz az Azure Search beépített.

## <a name="azure-search-concepts"></a>Az Azure Search fogalmak
Keresés,--szolgáltatás megoldásként Azure Search lehetővé teszi a fejlesztők tooadd hatékony keresési tooapplications észlel bármilyen infrastruktúra kezelése és a keresési szakértő váljon nélkül. Adatok feltöltése toohello szolgáltatás, és majd hello felhőben tárolt. Egyszerű kérelmek toohello Azure Search API használatával, hello adatok majd módosíthatók és keres. Hello szolgáltatás áttekintése itt található: [Ez a cikk](http://aka.ms/whatisazsearch). Ismertetése előtt kialakítási minta, fontos fontos toounderstand néhány olyan fogalmat, az Azure Search.

### <a name="search-services-indexes-fields-and-documents"></a>Keresési szolgáltatások, az indexek, a mezők és a dokumentumok
Azure Search használata esetén egy előfizet tooa *keresési szolgáltatás*. Mert adatai feltöltött tooAzure keresési, tárolása pedig egy *index* hello keresési szolgáltatáson belül. Egy önálló szolgáltatásként az indexek száma lehet. toouse hello ismerős fogalmai adatbázisok hello keresési szolgáltatás lehet likened tooa adatbázis szolgáltatás hello az indexek adatbázisban lévő likened tootables lehetnek.

A keresési szolgáltatás belüli egyes index saját séma, testreszabható számos által definiált van *mezők*. Adatok kerülnek tooan Azure Search-index hello képernyőn egyéni *dokumentumok*. Minden egyes dokumentum feltöltött tooa adott index kell lennie, és bele kell férnie a sémát, hogy indexeli. Azure Search használatával adatok keresésekor hello teljes szöveges keresési lekérdezések alapján egy adott index adják ki.  toocompare ezen fogalmakat toothose adatbázis mezők lehet likened toocolumns egy tábla, és dokumentumokat likened toorows lehet.

### <a name="scalability"></a>Méretezhetőség
Bármely Azure Search szolgáltatást, a hello Standard [tarifacsomag](https://azure.microsoft.com/pricing/details/search/) méretezheti a két dimenziókban: tárolási és rendelkezésre állás.

* *Partíciók* tooincrease hello tárolása egy keresési szolgáltatás lehet hozzáadni.
* *Replikák* tooa szolgáltatás tooincrease hello kapacitásának kezelni tud a keresési szolgáltatás lehet hozzáadni.

Hozzáadása és eltávolítása a partíciókat és a replikákról lehetővé teszi hello keresési szolgáltatás toogrow hello mennyiségű adat és a forgalom hello terhelésekig hello kapacitását. A rendezés egy keresési szolgáltatás tooachieve az olvasási [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), két replika van szükség. A rendezés egy szolgáltatás tooachieve az olvasási és írási [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), három replikák van szükség.

### <a name="service-and-index-limits-in-azure-search"></a>Az Azure Search szolgáltatás és az index korlátok
Nincsenek különböző néhány [tarifacsomagok](https://azure.microsoft.com/pricing/details/search/) az Azure Search hello rétegek mindegyikén különböző [korlátozásai és a kvóták](search-limits-quotas-capacity.md). Ezek a korlátozások némelyike: hello szolgáltatásiszint-, némelyek hello index-szinten, és hello partíció-szintjén vannak.

|  | Basic | Standard1 | Standard2 | Standard3 | Standard3 HD |
| --- | --- | --- | --- | --- | --- |
| Maximális replikák-szolgáltatás |3 |12 |12 |12 |12 |
| Maximális partícióból szolgáltatás |1 |12 |12 |12 |1 |
| Maximális keresési egységek (replikák * partíciók) szolgáltatás |3 |36 |36 |36 |36 (legfeljebb 3 partíció) |
| Maximális dokumentumok-szolgáltatás |1 millió |180 millió |720 millió |1.4 milliárd |600 millió |
| Maximális tárolási szolgáltatás |2 GB |300 GB |1,2 TB |2,4 TB |600 GB |
| Maximális dokumentumok partíciónként |1 millió |15 millió |60 millió |120 millió |200 millió |
| Maximális tárolási partíciónként |2 GB |25 GB |100 GB |200 GB |200 GB |
| Maximális indexek-szolgáltatás |5 |50 |200 |200 |3000 (legfeljebb 1000 indexek/partíció) |

#### <a name="s3-high-density"></a>S3 Nagy sűrűségű "
Azure Search S3 tarifacsomag a beállítás a kifejezetten a több-bérlős forgatókönyvek készült hello nagy sűrűségű (HD) módban van. Sok esetben az is szükséges toosupport nagyszámú kisebb bérlők alatt egy egyszerűség és a költséghatékonyság hatékonyságát egyetlen szolgáltatás tooachieve hello előnyeit.

S3 HD lehetővé teszi a sok kisméretű indexek toobe érinti egy egyetlen search szolgáltatás hello felügyelete alatt álló hello képességét tooscale kimenő indexek partíciók használatával a hello képességét toohost egyetlen szolgáltatás további indexek kereskedelmi hello.

S3 szolgáltatásnak konkrétan, 1 és 200 indexek üzemeltető együtt sikerült másolatot too1.4 milliárd dokumentumok között lehet. Egy S3 HD a hello egyrészről lehetővé teszi az egyedi indexek tooonly feljebb too1 millió dokumentumot, de be too1000 indexeket 200 millió partíciónként azoknak a teljes dokumentum számával (felfelé too3000 szolgáltatásonként) partíciónként kezelni tud (felfelé too600 millió-szolgáltatás esetében).

## <a name="considerations-for-multitenant-applications"></a>Több-bérlős alkalmazásokhoz szempontjai
Több-bérlős alkalmazások kell hatékony terjesztésére erőforrások közötti hello bérlők között hello adatvédelmi bizonyos fokú megőrzésével különböző bérlők. Ilyen alkalmazás hello architektúra tervezésekor, van néhány szempontok:

* *Bérlők elkülönítésének:* alkalmazásfejlesztők tootake megfelelő intézkedéseket tooensure, hogy nem a bérlők számára nem engedélyezett vagy kívánt access toohello adatokat a többi bérlő nem szükséges. Adatvédelem hello szempontjából, túl bérlői elkülönítési stratégiák a megosztott erőforrások és a védelmet a zajos szomszédok hatékony kezelése szükséges.
* *A felhő erőforrás költség:* módon és bármely alkalmazás, szoftveres megoldások költség versenyképes egy több-bérlős alkalmazás összetevőjeként kell maradnia.
* *Műveletek a könnyű:* fontos szempont a több-bérlős architektúrák fejlesztésekor hello hello alkalmazás műveletek gyakorolt és összetettségét. Az Azure Search rendelkezik egy [99,9 %-os SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* *Globális erőforrásigényét:* több-Bérlős alkalmazások esetleg tooeffectively szolgálnak bérlők számára, amely hello földgömb különböző pontjain.
* *Méretezhetőség:* alkalmazásfejlesztők tooconsider hogyan azok egyeztetése alkalmazás összetettségi elég alacsony szintű karbantartásáért, valamint a hello alkalmazás tooscale megtervezése a bérlők száma között kell, és hello bérlők adatok méretétől és munkaterhelés.

Az Azure Search kínál, amelyek idővel használt tooisolate bérlők adatok és alkalmazások és szolgáltatások néhány határokat.

## <a name="modeling-multitenancy-with-azure-search"></a>Több vállalat kiszolgálása az Azure Search modellezési
Egy több-bérlős forgatókönyv hello esetben hello alkalmazásfejlesztő egy vagy több keresési szolgáltatást használ, és osztani a bérlők között a szolgáltatások, indexeket, vagy mindkettőt. Az Azure Search rendelkezik néhány gyakori minták a modellezési egy több-bérlős forgatókönyv:

1. *Bérlőnként index:* mindegyik bérlő rendelkezik egy keresési szolgáltatáson belül, amely más bérlők meg van osztva a saját index.
2. *Bérlőnként szolgáltatás:* mindegyik bérlő rendelkezik saját dedikált Azure Search szolgáltatással, adatokat és a munkaterhelések elkülönítése legmagasabb szintű kínál.
3. *Mindkét vegyesen:* nagyobb, a több aktív bérlők rendelt dedikált szolgáltatások, amíg kisebb bérlők megosztott szolgáltatások az egyedi indexek vannak rendelve.

## <a name="1-index-per-tenant"></a>1. Bérlőnként index
![Egy portrayal hello index / bérlői modell](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

Az index egyes bérlői modell több bérlő helyezkednek el egyetlen Azure Search szolgáltatás ahol mindegyik bérlő rendelkezik saját index.

Bérlők adatok elkülönítési érhető el, mert minden keresési kérelmeket, és az Azure Search index szinten kiállított dokumentum műveletek. Hello alkalmazásréteg nincs hello szükség ezekre a műveletekre toodirect különböző bérlők forgalom toohello megfelelő indexek hello miközben is erőforrások hello szolgáltatás szintjén keresztüli egyetlen bérlő számára.

Hello index / bérlői modell kulcsattribútum hello mostantól hello alkalmazás fejlesztői toooversubscribe hello egy keresési szolgáltatás kapacitása hello alkalmazás bérlők között. Ha egy munkaterhelés egyenetlen eloszlását, bérlők számára is optimális kombinációja hello legyen elosztva a hello bérlők rendelkezik egy keresési szolgáltatás indexeli tooaccommodate nagyon aktív, az erőforrás-igényes bérlők számos hosszú végének közben egyszerre kevésbé aktív bérlők. hello kompromisszum hello nem képes hello modell toohandle olyan helyzetekben, ahol mindegyik bérlő egyidejű nagyon aktív.

hello index / bérlői modell hello alapot biztosít egy változó költség modell, ahol az egész Azure Search szolgáltatást vásárolt kezdeti és majd ezt követően feltölti a bérlők számára. Ez lehetővé teszi a nem használt kapacitás toobe kijelölt kísérletek és ingyenes fiókot.

Az alkalmazások globális tárhely hello index / bérlői modell nem lehet leghatékonyabb hello. Ha egy alkalmazás bérlők különböző pontjain hello földgolyó méretét, akkor egy külön szolgáltatási mindegyik régióhoz, amely előfordulhat, hogy duplikálja költségek azok szükség lehet.

Az Azure Search lehetővé teszi, hogy hello bővített hello egyedi indexek és indexek toogrow hello teljes száma. Ha egy megfelelő árképzési szint választja, és partíciók és replikák felveheti teljes keresőszolgáltatás toohello, amikor hello szolgáltatáson belül egyedi index növekedésének storage vagy a forgalom túl nagy.

Hello indexek száma túl nagy az egyetlen szolgáltatás növekszik, ha egy másik szolgáltatás rendelkezik kiépített toobe tooaccommodate hello új bérlők számára. Ha indexek toobe léptetik keresési szolgáltatások között, hogy új szolgáltatással bővül, hello adatok hello indexből rendelkezik-e manuálisan másolta át egy index toohello más toobe, az Azure Search nem érhető el egy index toobe áthelyezték.

## <a name="2-service-per-tenant"></a>2. Bérlőnként szolgáltatás
![Egy portrayal hello szolgáltatás / bérlői modell](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

A szolgáltatás-/-bérlős architektúrák mindegyik bérlő saját keresési szolgáltatás rendelkezik.

Ebben a modellben hello alkalmazás éri el a bérlők elszigetelését hello maximális szintjét. Minden szolgáltatás van dedikált, tárolási és átviteli sebesség kezelésére keresési kérelmet, valamint a külön API-kulcsokat.

Alkalmazások esetében, ahol mindegyik bérlő rendelkezik egy nagy erőforrásigényét vagy hello munkaterhelés a bérlő tootenant kevés variancia hello szolgáltatás / bérlői modell tényleges választási esetén, mert erőforrások megosztása nem különböző bérlők munkaterhelések között.

A szolgáltatás egyes bérlői modellek hello előnye, hogy egy előre jelezhető, rögzített költség modell is biztosít. Van egy teljes keresési szolgáltatás nincs kezdeti beruházást addig, amíg nincs egy bérlő toofill, azonban hello költség-/-bérlő értéke magasabb, mint az index egyes bérlői modell.

hello szolgáltatás / bérlői modell esetén egy globális tárhely alkalmazások hatékony választani. A földrajzilag elosztott bérlők esetén könnyen toohave az egyes bérlők szolgáltatás hello megfelelő régió.

Ebben a mintában skálázás hello kihívás merülhetnek fel, ha az egyes bérlők elszaporodó a szolgáltatás. Az Azure Search jelenleg nem támogatja, minden adatot kell manuálisan toobe tooa új szolgáltatás IP-címek egy keresési szolgáltatás frissítése hello.

## <a name="3-mixing-both-models"></a>3. Mindkét modellt keverése
Több vállalat kiszolgálása modellezési egy másik mintát index / bérlői és a szolgáltatás egy bérlő stratégiák van keverése.

Úgy hello két minták, egy alkalmazás legnagyobb bérlők lefoglalhatja dedikált szolgáltatások míg hosszú vége, az aktív, kisebb bérlők kevésbé hello indexek lefoglalhatja a megosztott szolgáltatás. Ez a modell biztosítja, hogy hello legnagyobb bérlők következetesen nagy teljesítményű hello szolgáltatásból származó bármely zajos szomszédok kisebb bérlők hello tooprotect elősegítik.

Azonban ezt a stratégiát végrehajtási támaszkodik előrelátás előrejelzésére, mely a bérlők egy dedikált szolgáltatás vagy egy megosztott szolgáltatás indexe van szükség. Alkalmazás összetettsége hello kell toomanage mindkét az több-bérlős modellek fokozódik.

## <a name="achieving-even-finer-granularity"></a>Még nagyobb részletességgel elérése
hello kialakítási minták toomodel több-bérlős forgatókönyvek az Azure Search fent egy egységes hatókörhöz, ahol mindegyik bérlő teljes példánya egy alkalmazás feltételezi. Alkalmazások azonban néha kezelik a sok kisebb hatókörét.

Ha a szolgáltatás-/-bérlők és az index / bérlői modellek nem elég hatókörök, akkor lehetséges toomodel egy index tooachieve egy granularitási még nagyobb fokú.

egyetlen index eltérően viselkednek a különböző ügyfél-végpontok toohave, egy mező lehet hozzáadott tooan index összes lehetséges ügyfél a megadott érték jelzi. Minden alkalommal, amikor egy ügyfél hívja az Azure Search tooquery vagy index módosítása, hello kód hello ügyfélalkalmazás határozza meg a megfelelő értéket hello Azure keresési mező [szűrő](https://msdn.microsoft.com/library/azure/dn798921.aspx) időben funkció.

Ez a metódus lehet külön felhasználói fiókokat, külön jogosultsági szintek és akár teljesen különálló alkalmazások használt tooachieve funkcióit.

> [!NOTE]
> Hello megközelítéssel a fent leírt tooconfigure egy egyetlen index tooserve több bérlő érinti hello relevanciájának a keresési eredmények. Keresési relevanciájának pontszámok arra az esetre vonatkoznak egy index szintű hatókörben, nem bérlői szintű hatókör, így egyetlen bérlő számára adatokat hello relevanciájának pontszámok alapul szolgáló statisztikákról, például a kifejezés gyakoriság van beépítve.
> 
> 

## <a name="next-steps"></a>Következő lépések
Az Azure Search esetén számos alkalmazás kényszerítő választani [hello szolgáltatás robusztus képességeivel kapcsolatos további](http://aka.ms/whatisazsearch). Ha kiértékelése hello különböző kialakítási minták több-bérlős alkalmazásokhoz, javasoljuk, hogy a hello [különböző árképzési szinteket](https://azure.microsoft.com/pricing/details/search/) és a megfelelő hello [szolgáltatási korlátait](search-limits-quotas-capacity.md) toobest személyessé tétele az Azure Search toofit alkalmazások és szolgáltatások és a különböző méretű architektúráinak megfelelően.

Bármely kérdések az Azure Search és a több-bérlős forgatókönyvek irányítható tooazuresearch_contact@microsoft.com.

