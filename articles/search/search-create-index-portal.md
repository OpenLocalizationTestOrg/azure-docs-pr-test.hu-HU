---
title: "AAA \"(portál – Azure Search) index létrehozása |} Microsoft dokumentumok\""
description: "Hozzon létre hello Azure portál használatával."
services: search
manager: jhubbard
author: heidisteen
documentationcenter: 
ms.assetid: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/20/2017
ms.author: heidist
ms.openlocfilehash: 4c5d663499bf73a8883690aa9482b6ba59d1ddf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-index-using-hello-azure-portal"></a>Hozzon létre egy Azure Search-index hello Azure portál használatával
> [!div class="op_single_selector"]
> * [Áttekintés](search-what-is-an-index.md)
> * [Portál](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Az Azure portál tooprototype hello beépített index designer használja, vagy hozzon létre egy [search-index](search-what-is-an-index.md) toorun meg az Azure Search szolgáltatás. 

## <a name="prerequisites"></a>Előfeltételek

Ez a cikk feltételezi, hogy egy [Azure-előfizetés](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) és [Azure Search szolgáltatás](search-create-service-portal.md).  

## <a name="find-your-search-service"></a>Keresse meg a keresési szolgáltatáshoz
1. Jelentkezzen be az Azure portálon toohello, és tekintse át a hello [keresési az előfizetéshez tartozó szolgáltatások](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Válassza ki az Azure Search szolgáltatást.

## <a name="name-hello-index"></a>Hello index neve

1. Kattintson a hello **index hozzáadása** gombra a parancssávon hello hello oldal hello tetején.
2. Nevezze el az Azure Search-index. 
   * Betűvel kezdődhet.
   * Csak kisbetűk, számjegyek és kötőjelek használja ("-").
   * Korlátozza a hello too60 karaktert.

  hello indexnév hello végponti URL-cím kapcsolatok toohello index és az Azure Search REST API hello HTTP-kérelmek küldéséhez használt részévé válik.

## <a name="define-hello-fields-of-your-index"></a>Az index hello mezők megadása

Index összeállítás tartalmaz egy *gyűjtemény mezők* hello kereshető adatokat, amely meghatározza az indexben. Pontosabban adja meg azt jelzi, hogy külön-külön feltöltése hello struktúra. hello mezők gyűjteménybe szükséges és választható mezőket, nevű és adta-e, az index attribútumok toodetermine hogyan hello mező használható.

1. A hello **Index hozzáadása** panelen kattintson a **mezők >** tooslide hello mező definition panel megnyitásához. 

2. Fogadja el a generált hello *kulcs* Edm.String típusú mező. Alapértelmezés szerint hello mező neve *azonosító* azonban mindaddig, amíg hello karakterlánc megfelel a átnevezheti [elnevezési szabályait](https://docs.microsoft.com/rest/api/searchservice/Naming-rules). A következő kulcsmező minden Azure Search-index esetén kötelező, és karakterláncnak kell lennie.

3. Adja hozzá a mezők toofully fel kell töltenie hello dokumentumok adja meg. Ha dokumentumok áll egy *azonosító*, *Szálloda neve*, *cím*, *Város*, és *régió*, hozzon létre egy megfelelő mező mindegyiknél hello index. Felülvizsgálati hello [kialakítási útmutató az alábbi részben hello](#design) segítségre attribútum.

4. Opcionálisan adja hozzá a mezőket használt belső szűrőkifejezéseket. Attribútumok használatát a hello mező a keresési műveletek tooexclude mezők állítható be.

5. Ha befejezte, kattintson a **OK** toosave és hello index létrehozása.

## <a name="tips-for-adding-fields"></a>Tippek a mezők hozzáadása

Az index létrehozása hello portálon intenzív billentyűzet. Ez a munkafolyamat következő minimalizálása érdekében a lépéseket:

1. Először is hozhat létre hello mezőlista nevet, és az adattípus beállítása.

2. A következő hello jelölőnégyzetek használata minden egyes attribútum toobulk hello tetején hello beállítás, minden mezőbe, és szelektív törölje hello jelölőnégyzetéből kevés mezőből áll, amelyek nem igényelnek. Például karakterlánc mező kitöltése általában kereshetők. Így előfordulhat, hogy kattintson **lekérhető** és **kereshető** tooboth hello visszatérési értékei hello mezőben a találatok között, valamint lehetővé teszik a teljes szöveges keresés hello mező. 

<a name="design"></a>
## <a name="design-guidance-for-setting-attributes"></a>Tervezési útmutatást nyújt attribútumainak beállítása

Habár bármikor hozzáadhat új mezőket, meglévő mező definíciók zárolta hello index hello élettartamát. Emiatt fejlesztők általában használó hello portal egyszerű indexek létrehozása, ötleteket tesztelése és hello portál lapjai toolook fel egy beállítás használatával. Egy index terv keresztül gyakori iteráció egy sokkal hatékonyabb, ha a kódalapú megközelítés követi, hogy könnyen használható hello index.

Elemzőkkel és a javaslattevők társítva a mezőket, hello index mentése előtt. Lehet, hogy tooclick minden lapokra lap tooadd nyelvi elemzőkkel vagy javaslattevők tooyour Indexdefiníció keresztül.

A karakterlánc típusú gyakran fel van tüntetve **kereshető** és **lekérhető**.

Használt mezőkkel toonarrow találatok között szerepelnek **rendezhető**, **Filterable**, és **kategorizálható**.

A mező attribútumok határozza meg, hogyan mező használata esetén például, hogy használatba a teljes szöveges keresés, jellemzőalapú navigációs, rendezési műveletet, és így tovább. hello a következő táblázat ismerteti az összes attribútumot.

|Attribútum|Leírás|  
|---------------|-----------------|  
|**kereshető**|Teljes szöveges kereshető, tulajdonos toolexical elemzés például szóhatároló indexelés során. Ha például a "napfényes" kereshető mező tooa érték beállításához belső azt lesznek osztva a hello egyedi jogkivonatok "moziba" és "day". További információkért lásd: [hogyan teljes szöveges keresés works](search-lucene-query-architecture.md).|  
|**szűrhető**|A hivatkozott **$filter** lekérdezések. Típusú szűrhető mezők `Edm.String` vagy `Collection(Edm.String)` nem kerülnek szóhatároló, így csak pontosan megegyezik az összehasonlítást. Például, ha a mező f túl beállítása "napfényes", `$filter=f eq 'sunny'` megkeresi, nincs találat, de `$filter=f eq 'sunny day'` lesz. |  
|**rendezhető**|Alapértelmezés szerint hello rendszer rendezése az eredmények pontszám, de be lehet állítani a rendezési a hello dokumentumokban lévő mezők alapján. Típusú mezők `Collection(Edm.String)` nem lehet **rendezhető**. |  
|**kategorizálható**|A keresési eredmények kategória (például egy adott városban szállodák) találati számát tartalmazó bemutató jellemzően használt. Ez a beállítás nem használható típusú mezők `Edm.GeographyPoint`. Típusú mezők `Edm.String` , amelyek **szűrhető**, **rendezhető**, vagy **kategorizálható** legfeljebb 32 kilobájt hosszabb használható. További információkért lásd: [a Create Index (REST API-t)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**kulcs**|A dokumentumok hello index belül egyedi azonosítója Pontosan egy mezőt ki kell választani, hello kulcsmező és típusúnak kell lennie `Edm.String`.|  
|**lekérhető**|Meghatározza, hogy a keresési eredmény hello mező adhatók vissza. Ez akkor hasznos, ha azt szeretné, hogy egy mező toouse (például *nyereség margó*) szűrőként, rendezés, vagy pontozási mechanizmussal, de elvégezhető nem kívánt hello mező toobe látható toohello végfelhasználói. Ennek az attribútumnak kell lennie `true` a `key` mezőket.|  

## <a name="create-hello-hotels-index-used-in-example-api-sections"></a>Példa API szakaszokban használt hello szállodák index létrehozása

Az Azure Search API dokumentációja tartalmaz egy egyszerű kiemeli kódpéldák *szállodák* index. Az alábbi hello képernyőképek, lásd: hello Indexdefiníció, beleértve az index definícióját, során megadott hello francia nyelvi elemzőt amely létrehozhatja a gyakorlat során hello portálon.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next-steps"></a>Következő lépések

Miután létrehozta az Azure Search-index, áthelyezheti toohello következő lépés: [kereshető adatokat feltölteni hello indexbe](search-what-is-data-import.md).

Másik lehetőségként is készíthet, indexek mélyebb betekintést. Ezenkívül toohello mezők gyűjteményében, index is megadja elemzőkkel, javaslattevők, pontozási profilokat, és a CORS-beállítások. hello portal lapokra lapokat biztosít hello leggyakoribb elemek meghatározásához: mezők, lekérdezések és javaslattevők. toocreate vagy egyéb elemek módosítása, hello REST API-t vagy a .NET SDK-t használja.

## <a name="see-also"></a>Lásd még:

 [A teljes szöveges keresés működése](search-lucene-query-architecture.md)  
 [Keresési szolgáltatás REST API](https://docs.microsoft.com/rest/api/searchservice/) [.NET SDK-val](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

