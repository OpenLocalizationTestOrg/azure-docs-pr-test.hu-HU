---
title: Azure Blob Storage az Azure Search aaaIndexing
description: "Ismerje meg, hogyan tooindex Azure Blob Storage és hogyan nyerhet ki a szöveget az Azure Search dokumentumok"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 2a5968f4-6768-4e16-84d0-8b995592f36a
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/22/2017
ms.author: eugenesh
ms.openlocfilehash: 1bdd34e66a4a9192ed88cacbc7b8456d0dcdfeb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-documents-in-azure-blob-storage-with-azure-search"></a>Az Azure Blob Storage tárolóban az Azure Search dokumentumok indexelő
Ez a cikk bemutatja, hogyan toouse Azure Search tooindex dokumentumok (például PDF-fájlok, a Microsoft Office-dokumentumok, és számos egyéb gyakori formátumok) Azure Blob storage-ban tárolt. Első lépésként beállítása és konfigurálása a blob indexelő hello alapjait ismerteti. Ezt követően kínál a mélyebb feltárása viselkedések és forgatókönyvek valószínűleg tooencounter áll.

## <a name="supported-document-formats"></a>A dokumentum formátumokat támogatja
hello blob indexelő lehet szöveg kinyerése a következő dokumentum formátumok hello:

* PDF
* A Microsoft Office formátumok: DOCX/DOC, XLSX/XLS, PPTX/PPT, Adatköltségek (Outlook e-mailek)  
* HTML
* XML
* A ZIP-
* EML
* RTF
* Egyszerű szöveges fájlt (lásd még: [indexelő egyszerű szöveges](#IndexingPlainText))
* JSON (lásd: [indexelő JSON-blobok](search-howto-index-json-blobs.md))
* CSV (lásd: [indexelő CSV-blobok](search-howto-index-csv-blobs.md) előzetes verziójú funkciók)

> [!IMPORTANT]
> Fürt megosztott kötetei szolgáltatás és a JSON-tömbök támogatása jelenleg előzetes verzió. Ezek a formátumok érhetők el, csak verziójával **2016 09-01. dátumú előnézeti** a REST API-t vagy a verziójával 2.x preview hello a .NET SDK hello. Adjon ne feledje, az előzetes API-k tesztelésében és értékelésében készült, és nem használható üzemi környezetben.
>
>

## <a name="setting-up-blob-indexing"></a>Blobindexelés beállítása
Beállíthat egy Azure Blob Storage indexelő használatával:

* [Azure Portal](https://ms.portal.azure.com)
* Az Azure Search [REST API-n](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Az Azure Search [.NET SDK-val](https://aka.ms/search-sdk)

> [!NOTE]
> Egyes szolgáltatások (például mező leképezések) még nem állnak rendelkezésre a hello portálon, és programozási nyelven toobe rendelkezik.
>
>

Itt bemutatjuk hello folyamata hello REST API használatával.

### <a name="step-1-create-a-data-source"></a>1. lépés:, Hozzon létre egy adatforrást
Egy adatforrás határozza meg, mely adatokat tooindex, a szükséges hitelesítő adatokat tooaccess hello adatokat és a házirendek tooefficiently – hello adatokban (új, módosított vagy törölt sorai) a változásokat. Egy adatforrás használható által hello több indexelő ugyanaz a keresési szolgáltatáshoz.

Hello adatforrás blobindexelés, a következő kötelező tulajdonság hello kell rendelkeznie:

* **név** hello hello adatforrás belül a keresési szolgáltatás egyedi neve.
* **típus** kell `azureblob`.
* **hitelesítő adatok** hello tárolási fiók kapcsolati karakterláncát, hello `credentials.connectionString` paraméter. Lásd: [hogyan toospecify hitelesítő adatok](#Credentials) alábbi részleteket.
* **tároló** határozza meg a tárfiók egy tárolót. Alapértelmezés szerint valamennyi BLOB hello tárolóban található lekérhető. Ha csak egy adott virtuális könyvtár tooindex blobokat, megadhatja a használata nem kötelező hello könyvtárhoz **lekérdezés** paraméter.

egy adatforrás toocreate:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Hozzon létre adatforrást API hello bővebben lásd: [adatforrás létrehozása](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="how-toospecify-credentials"></a>Hogyan toospecify hitelesítő adatok ####

Megadhatja, hogy hello hitelesítő adatok hello blob tároló az alábbi módszerek valamelyikével:

- **Teljes hozzáférés tárolási fiók kapcsolati karakterlánc**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>`. Letölthető hello kapcsolati karakterlánc hello Azure-portálon lépjen a storage-fiók panelen toohello > Beállítások > kulcsok (a klasszikus tárfiókokkal) vagy a beállítások > hozzáférési kulcsokkal (az Azure Resource Manager storage-fiókok).
- **Tárolási fiók közös hozzáférésű jogosultságkódot** (SAS) kapcsolódási karakterlánc: `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<hello signature>&spr=https&se=<hello validity end time>&srt=co&ss=b&sp=rl` hello SAS rendelkeznie kell a listában, és olvasási engedéllyel a tárolók és objektumok hello (ebben az esetben blobok).
-  **Tároló közös hozzáférésű jogosultságkódot**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<hello signature>&se=<hello validity end time>&sp=rl` hello SAS hello listában, és olvasási jogosultságokkal hello tároló kell rendelkeznie.

További információt a tárhelyen megosztott hozzáférési aláírásokkal, lásd: [megosztott hozzáférési aláírásokkal használatával](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Ha SAS-hitelesítő adatokat használ, akkor a lejárati idejük tooupdate hello az adatforrás hitelesítő adatainak rendszeres időközönként a megújított aláírások tooprevent. Ha SAS-hitelesítő adatok lejárnak, hello indexelő sikertelen lesz, és egy ehhez hasonló hibaüzenetet túl`Credentials provided in hello connection string are invalid or have expired.`.  

### <a name="step-2-create-an-index"></a>2. lépés: Az index létrehozása
hello index hello mezők egy dokumentumot, attribútumok, adja meg, és egyéb hoz létre, hogy alakzat hello keresési élményt biztosít.

Itt hogyan toocreate egy index ugyanezzel a kereshető `content` toostore hello szöveg blobok kinyert mezőben:   

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }

Indexek létrehozásával kapcsolatban bővebben lásd: [Index létrehozása](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="step-3-create-an-indexer"></a>3. lépés:, Hozzon létre egy indexelőt
Az indexelő csatlakoznak az adatforrás egy cél search-index, és egy tooautomate hello az Adatfrissítés ütemezéséről biztosít.

Hello index és az adatforrás létrehozása után készen áll a toocreate hello indexelő Ön:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Az indexelő minden két órát fog futni (ütemezési időköz értéke túl "PT2H"). az indexelő toorun 30 percenként túl hello időköz beállítása "PT30M". hello legrövidebb támogatott időköz értéke 5 perc. hello ütemezésének megadása nem kötelező – Ha nincs megadva, az indexelő futása csak egyszer, amikor létrejön. Azonban bármikor indexelő igény szerinti is futtathatja.   

További részleteket a hello indexelő API létrehozása, tekintse meg [létrehozása indexelő](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="how-azure-search-indexes-blobs"></a>Hogyan indexeli az Azure Search a blobok

Attól függően, hogy hello [indexelő konfigurációs](#PartsOfBlobToIndex), hello blob indexelő indexelheti csak a tárolási metaadatok (akkor hasznos, ha csak ítélt készül hello metaadatok, és nem szükséges tooindex hello BLOB tartalmát), tároló és a tartalom metaadatainak vagy mindkettő metaadatok és a szöveges tartalom. Alapértelmezés szerint a hello indexelő metaadatok és a tartalom kibontása.

> [!NOTE]
> Alapértelmezés szerint blobok JSON vagy a fürt megosztott kötetei szolgáltatás például tartalmazó strukturált indexeli, egyetlen adattömb szöveg. Ha tooindex-blobok JSON és a fürt megosztott kötetei szolgáltatás strukturált módon, lásd: [indexelő JSON-blobok](search-howto-index-json-blobs.md) és [indexelő CSV-blobok](search-howto-index-csv-blobs.md) az előzetes funkciók.
>
> Egy összetett vagy beágyazott dokumentum (például ZIP-archívum létrehozása, vagy egy Word-dokumentumot beágyazott Outlook e-mail mellékleteket tartalmazó) is indexelt egyetlen-dokumentumként.

* hello szöveges hello dokumentum tartalmának kibontása nevű mezőnek `content`.

> [!NOTE]
> Az Azure Search korlátozza attól függően, hogy az IP-címek hello bontja ki szöveg: 32000 karakterek szabad réteg, 64 000 az alapszintű és a 4 millió Standard, a Standard S2 és a Standard S3 rétege számára. Hello indexelő állapot válasz csonkolt dokumentumokra vonatkozó figyelmeztetés tartalmazza.  

* E hello blob, a felhasználó által megadott metaadat-tulajdonságainak bármilyen kibontása pontosan.
* Standard blob metaadat-tulajdonságainak ki kell olvasni a következő mezők hello:

  * **metaadatok\_tárolási\_neve** (Edm.String) – hello hello blob fájlnevét. Például ha egy blob /my-container/my-folder/subfolder/resume.pdf, hello Ez a mező értéke `resume.pdf`.
  * **metaadatok\_tárolási\_elérési** (Edm.String) – hello hello blob, beleértve a hello tárfiók teljes URI-címe. Például: `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`
  * **metaadatok\_tárolási\_tartalom\_típus** (Edm.String) - tartalomtípus meg van adva hello kóddal akkor tooupload hello blob használt. Például: `application/octet-stream`.
  * **metaadatok\_tárolási\_utolsó\_módosított** (Edm.DateTimeOffset) - utolsó módosítás hello BLOB időbélyegző. Az Azure Search használja a Timestamp típusú megváltozott tooidentify blobokat, tooavoid újraindexelés mindent hello kezdeti indexelő után.
  * **metaadatok\_tárolási\_mérete** (Edm.Int64) - blob mérete bájtban.
  * **metaadatok\_tárolási\_tartalom\_md5** (Edm.String) - MD5 kivonatoló hello blobtartalom, ha elérhető.
* Metaadatok tulajdonságok adott tooeach dokumentumformátum kibontása felsorolt hello mezőkbe [Itt](#ContentSpecificMetadata).

Nem kell toodefine mezők az összes fent tulajdonságok hello a keresési index, mert csak rögzítése hello tulajdonságok van szüksége az alkalmazáshoz.

> [!NOTE]
> Gyakran a meglévő index hello mezőnevek hello mezőnevek dokumentum kibontási során eltérő lesz. Használhat **hozzárendelések mezőben** toomap hello Azure Search toohello alkotó mezőnevek az search-index által megadott tulajdonságnév. Látni fogja a mező hozzárendelések alábbi példát.
>
>

<a name="DocumentKeys"></a>
### <a name="defining-document-keys-and-field-mappings"></a>Dokumentum kulcsok és a mezők leképezésének meghatározása
Az Azure Search hello dokumentum kulcs egyedileg azonosít egy dokumentumot. Minden keresési index Edm.String típusú pontosan egy kulcsmező kell rendelkeznie. hello kulcs mező kitöltése kötelező nyilvántartott egyes dokumentumok hozzáadott toohello index (ténylegesen hello csak kötelező mező).  

Alaposan gondolja át melyik kibontott mezőt kell leképezni toohello kulcsmező az index. hello alternatívák::

* **metaadatok\_tárolási\_neve** – Ez lehet, hogy egy kényelmes jelölt, de vegye figyelembe, hogy 1) hello nevek nem feltétlenül egyedi, mivel előfordulhat, hogy különböző mappák neve azonos hello blobok és 2) hello neve karaktereket tartalmazhat, amelyek a dokumentum kulcsok, például kötőjelek érvénytelenek. Hello segítségével érvénytelen karaktereket az kezelését is `base64Encode` [leképezési függvény mezőben](search-indexer-field-mappings.md#base64EncodeFunction) – Ha így tesz, ne feledje tooencode dokumentum kulcsok, amikor például a keresési meghívja átadja őket az API-ban. (Például a .NET használható hello [UrlTokenEncode metódus](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) erre a célra).
* **metaadatok\_tárolási\_elérési** – hello teljes elérési útja a biztosítja a egyediségi, de hello elérési útját mindenképpen tartalmaz `/` karakterek, amelyek [egy dokumentum kulcs érvénytelen](https://docs.microsoft.com/rest/api/searchservice/naming-rules).  A fenti, lehetősége van hello hello kulcsok használatával hello kódolási `base64Encode` [függvény](search-indexer-field-mappings.md#base64EncodeFunction).
* Ha a fenti hello lehetőségek egyike megfelelő Önnek, egy egyéni metaadat-tulajdonság toohello blobok is hozzáadhat. Ezt a beállítást, azonban szükséges a blob feltöltési folyamat tooadd adott metaadatok tulajdonság tooall blobokat. Hello kulcs értéke kötelezően megadandó tulajdonság, összes BLOB, amelyek nem rendelkeznek az adott tulajdonsághoz sikertelen lesz indexelve toobe.

> [!IMPORTANT]
> Ha nincs explicit leképezés hello kulcsmező hello index, automatikusan használja-e Azure Search `metadata_storage_path` , és a base-64 hello kódolja kulcsértékei (hello második lehetőség fenti).
>
>

Ehhez a példához tegyük válasszon hello `metadata_storage_name` hello dokumentum kulcsként mezőjét. Tételezzük is fel, hogy az index nevű kulcs mezőt tartalmaz `key` és mező `fileSize` hello dokumentum méretének tárolásához. a kívánt módon működjenek, a toowire folyamatot adja meg a következő mező hozzárendelések létrehozásakor vagy frissítésekor az indexelő hello:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

toobring az összes együtt, az alábbiakban hogyan adhat mező leképezések és engedélyezése base 64 kódolás kulcsok a meglévő indexelőt:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }

> [!NOTE]
> toolearn mezők leképezését kapcsolatos további információkért lásd: [Ez a cikk](search-indexer-field-mappings.md).
>
>

<a name="WhichBlobsAreIndexed"></a>
## <a name="controlling-which-blobs-are-indexed"></a>Mely blobok indexelt vezérlése
Szabályozhatja, hogy mely blobok indexelt, és amely kimarad.

### <a name="index-only-hello-blobs-with-specific-file-extensions"></a>Csak hello blobok meghatározott fájlnév-kiterjesztések index
Csak hello blobok hello fájlnévkiterjesztésekkel hello használatával megadja a indexelheti `indexedFileNameExtensions` indexelő konfigurációs paraméter. hello értéke a kívánt fájlkiterjesztéseket (a kezdő pont) vesszővel tagolt listáját tartalmazó karakterlánc. Például tooindex csak hello. PDF és. Blobok DOCX, tegye a következőket:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions"></a>Blobok meghatározott fájlnév-kiterjesztésű kizárása
Kizárhat meghatározott fájlnév-kiterjesztések blobok hello segítségével indexelésének `excludedFileNameExtensions` konfigurációs paraméter. hello értéke a kívánt fájlkiterjesztéseket (a kezdő pont) vesszővel tagolt listáját tartalmazó karakterlánc. Például minden tooindex blobok a hello kivételével. PNG és. JPEG-bővítményeket, tegye a következőket:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Ha mindkét `indexedFileNameExtensions` és `excludedFileNameExtensions` paraméterek szerepelnek, az Azure Search először ellenőrzi, hogy az `indexedFileNameExtensions`, majd a `excludedFileNameExtensions`. Ez azt jelenti, hogy ha hello ugyanazon fájl kiterjesztése mindkét listája szerepel, akkor nem kerülnek bele az indexelő.

### <a name="dealing-with-unsupported-content-types"></a>Nem támogatott tartalomtípus kezelésével

Alapértelmezés szerint hello blob indexelője, amint fordul egy blobot a tartalom nem támogatott típust (például egy képet). Természetesen használhatja hello `excludedFileNameExtensions` bizonyos paraméter tooskip tartalomtípus. Azonban szükség lehet tooindex blobok összes hello lehetséges tartalomtípusok előre ismerete nélkül. Amikor a rendszer észlelt egy nem támogatott tartalomtípus, indexelő toocontinue hello beállítása `failOnUnsupportedContentType` konfigurációs paramétere túl`false`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }

### <a name="ignoring-parsing-errors"></a>A rendszer figyelmen kívül hagyja az elemzési hibák

Az Azure Search dokumentum kibontási logika nem tökéletes megoldás, ezért sikertelen lesz, néha tooparse dokumentumok tartalom típusa támogatott, például a. DOCX vagy. PDF-FÁJLT. Ha nem szeretné, hogy ebben az esetben indexelő toointerrupt hello, állítsa be a hello `maxFailedItems` és `maxFailedItemsPerBatch` konfigurációs paraméterek toosome ésszerű értékeket. Példa:

    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }

<a name="PartsOfBlobToIndex"></a>
## <a name="controlling-which-parts-of-hello-blob-are-indexed"></a>Hello blob részeket indexelt vezérlése

Hello blobok részeket indexelt hello segítségével szabályozhatja `dataToExtract` konfigurációs paraméter. A következő értékek hello is igénybe vehet:

* `storageMetadata`-határozza meg, hogy csak hello [szabványos blob tulajdonságait és a felhasználó által megadott metaadatok](../storage/blobs/storage-properties-metadata.md) indexelt.
* `allMetadata`-határozza meg, hogy tárolási metaadatai és hello [tartalomtípus adott metaadatokat](#ContentSpecificMetadata) kibontott hello blobból tartalom indexelt.
* `contentAndMetadata`-Adja meg a metaadatok és a szöveges tartalom hello blob kinyert indexelt. Ez az alapértelmezett érték hello.

Például tooindex csak hello tárolási metaadatoknál használja:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }

### <a name="using-blob-metadata-toocontrol-how-blobs-are-indexed"></a>Hogyan blobok indexelt blob metaadatai toocontrol használatával

a fent leírt hello konfigurációs paraméterek tooall blobok alkalmazni. Egyes esetekben érdemes lehet toocontrol hogyan *egyes blobok* indexelt. Ehhez adja hozzá a következő hello blob-metaadatok tulajdonságok és értékek:

| Tulajdonság neve | Tulajdonság értéke | Magyarázat |
| --- | --- | --- |
| AzureSearch_Skip |"true" |Arra utasítja a hello blob indexelő toocompletely skip hello blob. Sem a metaadatok, sem a tartalom kivonása kísérlet történik. Ez akkor hasznos, ha egy adott blob ismételten meghibásodik, és megszakítja hello indexelési folyamat. |
| AzureSearch_SkipContent |"true" |Ez megegyezik a `"dataToExtract" : "allMetadata"` ismertetett beállítás [fent](#PartsOfBlobToIndex) hatókörön belüli tooa adott blob. |

## <a name="incremental-indexing-and-deletion-detection"></a>Növekményes indexelő és törlési észlelése
Ha beállít egy blob indexelő toorun ütemezés szerint, azt újra indexek csak hello megváltozott blobot hello blob alapján `LastModified` időbélyegző.

> [!NOTE]
> Nem rendelkezik toospecify egy módosítás szabályzat – növekményes indexelő engedélyezve van, automatikusan.

toosupport törlése dokumentumok, a "soft törlés" módszert használja. Ha törli a végleges hello blobok, megfelelő dokumentumok nem törlődik hello search-index. Ehelyett használjon hello a következő lépéseket:  

1. Adja hozzá az egyéni metaadat tulajdonság toohello blob tooindicate tooAzure keresési logikailag el kell-e hagyni
2. Hello az adatforrás egy helyreállítható törlési szabályzat konfigurálása
3. Hello indexelő hello blob (eredményobjektumokról hello indexelő állapot API) rendelkezik feldolgozását követően hello blob fizikailag törlése

Például a következő házirend hello úgy ítéli meg, a blob toobe törölve, ha olyan metadata tulajdonsággal rendelkezik `IsDeleted` hello értékű `true`:

    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",     
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }   

## <a name="indexing-large-datasets"></a>Az indexelő nagy adatkészletek

Blobok indexelő időigényes folyamat lehet. A blobok tooindex több millió esetében esetben felgyorsíthatja az adatok particionálása és párhuzamos használatával több indexelők tooprocess hello adatok indexelése. Itt látható, hogyan állíthat be ezt:

- Az adatok particionálása több blob tárolók vagy virtuális mappák
- Állítson be több Azure keresési adatforrások, egy tárolót vagy mappa egy. toopoint tooa blob mappa, használjon hello `query` paraméter:

    ```
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

- Hozzon létre egy megfelelő indexelőt minden adatforrás. Az összes hello indexelők is pont toohello azonos cél search-index.  

- A szolgáltatás egy keresési egység egy indexelő futtathatja egy adott időpontban. Csak akkor hasznos, ha ténylegesen párhuzamos futtatása több indexelők létrehozása a fent leírt módon. toorun párhuzamosan több indexelők kiterjesztése a keresőszolgáltatása megfelelő számú partíciót és a replikák létrehozásával. Például ha a keresési szolgáltatás 6 keresési egységek (például 2 partíció x 3 replikák), majd 6 indexelők is futtatható egyidejűleg, egy six-fold hello indexelési teljesítmény növekedését eredményezi. További információ a méretezés és a kapacitástervezést, toolearn lásd: [erőforrás szintjeinek lekérdezési és indexelési munkaterhelések az Azure Search méretezése](search-capacity-planning.md).

## <a name="indexing-documents-along-with-related-data"></a>Az indexelő dokumentumok kapcsolódó adatokkal együtt

Érdemes lehet túl "összeállítása" dokumentumok az indexben több forrásból. Például érdemes lehet blobok toomerge szöveg más Cosmos DB tárolt metaadatok. Leküldéses indexelési API különböző indexelők együtt túl kialakításához több részeiből dokumentumok keresése hello még akkor is használhatja. 

A toowork összes indexelő és más olyan összetevők kell tooagree hello dokumentum kulcs. Részletes útmutató, tekintse meg a külső cikket: [dokumentumok egyesíthető más adatokat az Azure Search ](http://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

<a name="IndexingPlainText"></a>
## <a name="indexing-plain-text"></a>Az indexelő egyszerű szöveg 

Összes blobok egyszerű szöveget tartalmazhat hello azonos kódolását, ha jelentősen növelheti az indexelő teljesítményét használatával **mód elemzése szöveg**. elemzési módot, set hello toouse szöveg `parsingMode` konfigurációs tulajdonság túl`text`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }

Alapértelmezés szerint hello `UTF-8` feltételezett kódolását. egy másik kódolást toospecify hello használata `encoding` konfigurációs tulajdonság: 

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }


<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>Tartalom típusra vonatkozó metaadatokhoz tulajdonságai
hello következő táblázat összefoglalja az egyes végzett feldolgozás, és Azure Search által kibontott hello metaadatok tulajdonságait ismerteti.

| Dokumentum formátuma / tartalomtípus | Adott metaadatokat tartalomtípus-tulajdonságok | Feldolgozási részletek |
| --- | --- | --- |
| HTML (`text/html`) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |Sáv HTML-kódot, és bontsa ki a szöveget |
| PDF (`application/pdf`) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Bontsa ki a szöveget, beleértve a beágyazott dokumentumok (kivéve a lemezképek) |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Bontsa ki a szöveget, beleértve a beágyazott dokumentumok |
| DOC (application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Bontsa ki a szöveget, beleértve a beágyazott dokumentumok |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Bontsa ki a szöveget, beleértve a beágyazott dokumentumok |
| XLS (kérelem/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Bontsa ki a szöveget, beleértve a beágyazott dokumentumok |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Bontsa ki a szöveget, beleértve a beágyazott dokumentumok |
| PPT (kérelem/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Bontsa ki a szöveget, beleértve a beágyazott dokumentumok |
| ÜZENET (kérelem/vnd.ms-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Bontsa ki a szöveget, beleértve a mellékletek |
| ZIP (kérelem/zip) |`metadata_content_type` |Szöveg kinyerése hello archívumban található összes dokumentum |
| XML (application/xml) |`metadata_content_type`</br>`metadata_content_encoding`</br> |XML-címke sáv és szöveg |
| JSON (application/json) |`metadata_content_type`</br>`metadata_content_encoding` |Szöveg<br/>Megjegyzés: Ha több dokumentum mezőit a JSON blob tooextract van szüksége, tekintse meg [indexelő JSON-blobok](search-howto-index-json-blobs.md) részletek |
| EML (üzenet/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Bontsa ki a szöveget, beleértve a mellékletek |
| RTF (kérelem/rtf) |`metadata_content_type`</br>`metadata_author`</br>`metadata_character_count`</br>`metadata_creation_date`</br>`metadata_page_count`</br>`metadata_word_count`</br> | Szöveg|
| Egyszerű szöveges (egyszerű szöveg) |`metadata_content_type`</br>`metadata_content_encoding`</br> | Szöveg|


## <a name="help-us-make-azure-search-better"></a>Segítsen az Azure Search továbbfejlesztésében
Ha a szolgáltatás-kérelmek vagy ötleteket javításai, ossza meg velünk a a [UserVoice webhelyén](https://feedback.azure.com/forums/263029-azure-search/).
