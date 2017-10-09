---
pageTitle: Synonyms in Azure Search (preview) | Microsoft Docs
description: "Hello szinonimák (előzetes verzió) szolgáltatás, hello Azure Search REST API felfedett előzetes dokumentációjában talál."
services: search
documentationCenter: 
authors: mhko
manager: pablocas
editor: 
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/07/2016
ms.author: nateko
ms.openlocfilehash: 2695139d2b298fa2e7c1814715fdf96729f594ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="synonyms-in-azure-search-preview"></a>Szinonimák az Azure Search (előzetes verzió)

A keresőmotorok szinonimák egyenértékű feltételeket, amely implicit módon bontsa ki a lekérdezés hello hatókör nélkül adja meg a hello kifejezés tooactually rendelkező hello felhasználói társítani. Például adott hello kifejezés "kutya" és "canine" és "ételadagot", "kutya", "canine" vagy "ételadagot" tartalmazó dokumentumokat szinonimát társítását hello hello-lekérdezés hatókörén belül is tartozik.

Az Azure Search szinonimát bővítése időben történik. Szinonimát maps tooa szolgáltatás nem működőképes tooexisting műveletekkel adhat hozzá. Hozzáadhat egy **synonymMaps** tulajdonság tooa mező definition toorebuild hello index nélkül. További információkért lásd: [Index frissítése](https://docs.microsoft.com/rest/api/searchservice/update-index).

## <a name="feature-availability"></a>Szolgáltatások rendelkezésre állása

a szolgáltatás jelenleg a hello szinonimák előzetes megtekintéséhez, és csak a támogatott hello legújabb előzetes api-verzió (api-version = 2016 09-01. dátumú előnézeti). Jelenleg nincs Azure Portal-támogatás. Hello API-verzió van megadva hello kérés, mert már lehetséges toocombine általánosan elérhető (GA) és a hello API-k előzetes ugyanahhoz az alkalmazáshoz. Azonban előnézeti API-k nincsenek az SLA-t és a szolgáltatások módosíthatja, ezért nem javasoljuk azok az éles környezetben.

## <a name="how-toouse-synonyms-in-azure-search"></a>Az Azure-ban toouse szinonimák a keresés

Az Azure Search szinonimát támogatási alapján határozza meg, és töltse fel a tooyour szolgáltatás szinonimát leképezések. A maps képeznek egy független erőforrás (például az adatforrásokat, illetve indexek), és használhatja a keresési szolgáltatás index bármely kereshető mező.

Szinonimát képezi le, és indexek egymástól függetlenül megmaradjanak. Miután szinonimát térképre határozza meg, és töltse fel tooyour szolgáltatás, engedélyezheti nevű új tulajdonsággal hozzáadásával hello szinonimát szolgáltatás mező **synonymMaps** hello mező definícióban. Létrehozása, frissítése és törlése egy szinonima térkép azonban mindig a teljes dokumentum művelet, ami azt jelenti, hogy nem hozható létre, frissítése vagy törlése Növekményesen hello szinonimát térkép részeit. Egy Újrabetöltés még egy-egy bejegyzésnek frissítése szükséges.

Szinonimák beépítése a keresőalkalmazás két lépésből áll:

1.  Szinonimát térkép tooyour keresési szolgáltatás keresztül hello az alábbi API-k hozzáadása.  

2.  Adja meg a kereshető mező toouse hello szinonimát térkép hello index definícióját.

### <a name="synonymmaps-resource-apis"></a>SynonymMaps erőforrás API-k

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a>Adja hozzá, vagy frissítse a szolgáltatás alatt szinonimát térkép, POST használatával, vagy HELYEZZE.

Szinonimát maps feltöltése toohello szolgáltatás POST vagy PUT keresztül. Minden egyes szabály hello új sor karaktert ("\n") kell elválasztani. Too5, egy ingyenes szolgáltatás szinonimát térképre 000 szabálynál és más termékváltozatok 10 000 szabályokat adhat meg. Minden egyes szabály legfeljebb too20 bővítések tartalmazhat.

Ebben az előzetesben szinonimát leképezéseket formátumúnak kell lennie hello Apache Solr alábbiakban ismertetett. Ha egy meglévő szinonimát szótár más formátumú, és szeretné toouse azt közvetlenül, tudassa velünk, a [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

Létrehozhat új szinonimát leképezés HTTP POST, mint például a következő hello használata:

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

Azt is megteheti használja a PUT és hello szinonimát leképezésnév meg hello URI. Ha hello szinonimát leképezés nem létezik, a rendszer létrehozza.

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a>Apache Solr szinonimát formátumban

hello Solr formátumhoz egyenértékű és explicit szinonimát leképezéseket. Leképezési szabályokat követi toohello nyílt forráskódú szinonimát szűrő megadását Apache Solr, a jelen dokumentumban ismertetett: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). Az alábbiakban az egyenértékű szinonimák minta szabályt.
```
              USA, United States, United States of America
```

A fenti hello szabálynak, a keresési lekérdezés "USA" túl bont ki "USA" vagy "Az Amerikai Egyesült Államok" vagy "Az Amerikai Egyesült Államok".

Explicit leképezési nyíl megjelölt "= >". Megadása esetén a kifejezés, amely megfelel a hello bal oldalán keresési lekérdezés sorozatát "= >" hello alternatívák hello jobb oldalán a helyébe. Az alábbi hello szabály megadott, keresőkifejezések "Washington", "Wash." vagy a "WA" összes felülíródik túl "WA". Explicit leképezése csak megadott hello irányban vonatkozik, és nem újraírási hello lekérdezés "WA" túl ebben az esetben a "Washington".
```
              Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a>Lista szinonimát leképezi a szolgáltatás alatt.

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a>A szolgáltatás alatt szinonimát térkép beolvasása.

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a>A szolgáltatás alatt szinonimák térképre törlése.

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

### <a name="configure-a-searchable-field-toouse-hello-synonym-map-in-hello-index-definition"></a>Adja meg a kereshető mező toouse hello szinonimát térkép hello index definícióját.

Új mező tulajdonság **synonymMaps** lehet használt toospecify egy szinonima térkép toouse kereshető mező. Szinonimát maps szolgáltatás szintű erőforrás, és az index hello szolgáltatásban minden mező szerint lehet hivatkozni.

    POST https://[servicename].search.windows.net/indexes?api-version=2016-09-01-Preview
    api-key: [admin key]

    {
       "name":"myindex",
       "fields":[
          {
             "name":"id",
             "type":"Edm.String",
             "key":true
          },
          {
             "name":"name",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"en.lucene",
             "synonymMaps":[
                "mysynonymmap"
             ]
          },
          {
             "name":"name_jp",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"ja.microsoft",
             "synonymMaps":[
                "japanesesynonymmap"
             ]
          }
       ]
    }

**synonymMaps** kereshető mezők hello típusa "Edm.String" vagy "Collection(Edm.String)" adható meg.

> [!NOTE]
> Ebben az előzetesben mezőben leképezése egy szinonima csak tartozhat. Ha szeretné toouse több szinonimát maps, tudassa velünk, a [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="impact-of-synonyms-on-other-search-features"></a>Egyéb keresési funkciókat a szinonimák hatása

hello szinonimák szolgáltatás újraírja hello eredeti lekérdezés szinonimák a hello vagy operátor. Emiatt találati kiemelése és profilok pontozási kezeli a hello eredeti kifejezés és szinonimák egyenértékűként.

Szinonimát szolgáltatás toosearch lekérdezések vonatkozik, és nem vonatkozik a toofilters értékkorlátozást. Ehhez hasonlóan javaslatok csak alapulnak hello eredeti kifejezés; találat szinonimát hello válaszban nem jelennek meg.

Szinonimát bővítések csak akkor érvényesíthetők toowildcard keresőkifejezéseket; előtag, az intelligens egyeztetésű, és a regex feltételek nem bontja ki.

## <a name="tips-for-building-a-synonym-map"></a>Tippek az szinonimát térképre felépítése

- Egy rövid, tetszetős szinonimát leképezés hatékonyabb, mint a lehetséges megfeleltetésekben teljesnek. Túl nagy vagy összetett szótárak tooparse hosszabb ideig eltarthat, és hatással hello lekérdezés-késleltetés, hello lekérdezés toomany szinonimák tárolásához. Ahelyett, hogy a becslés, amellyel feltételek használhatók, kaphat a tényleges feltételek hello keresztül egy [keresési forgalom elemző jelentés](search-traffic-analytics.md).

- Egy előzetes és az érvényesítés a gyakorlatban, engedélyezése, és majd használja, ez a jelentés tooprecisely határozza meg, mely feltételek fog hasznot húzhatnak a szinonima egyezés, és folytassa toouse azt, hogy a szinonima térképre van előállító jobb eredményének ellenőrzése. Hello előre megadott jelentésben, hello tartalmazó csempék éppen "leggyakoribb keresési lekérdezések" és "nulla-eredmény keresési lekérdezések" ad hello szükséges információkat.

- Létrehozhat több szinonimát maps keresési alkalmazás (például úgy, hogy ha az alkalmazás támogatja a többnyelvű vásárlói bázisunk nyelv). Jelenleg a mező csak egyikét használhatja őket. A mező synonymMaps tulajdonság bármikor frissítheti.

## <a name="next-steps"></a>Következő lépések

- Ha egy meglévő index (nem éles) fejlesztői környezetben, kísérletezhet egy kis szótár toosee hogyan szinonimák hello hozzáadása változik hello keresési élményt biztosít, beleértve a profilok pontozási találati kiemelése és javaslatok hatással.

- [Engedélyezze a keresési forgalom analytics](search-traffic-analytics.md) használata hello előre Power BI-jelentés mely kifejezések toolearn hello legtöbb, és mely azokat, visszatérési dokumentumot. Hello szótár tooinclude szinonimák rendszer lekérdezések, amelyek az indexben toodocuments feloldása kell volt képes ezek insights felvértezve, vizsgálja felül.
