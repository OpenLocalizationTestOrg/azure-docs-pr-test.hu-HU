---
title: "egy Azure-Media Encoder Standard használatával objektum aaaHow tooencode |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Media Encoder Standard tooencode media Azure Media Services a tartalmat. Kódminták REST API-t használja."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2a7273c6-8a22-4f82-9bfe-4509ff32d4a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: b766bafded7ee98eda3e6ef149c31d5d8fe406fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencode-an-asset-by-using-media-encoder-standard"></a>Hogyan tooencode egy eszköz Media Encoder Standard használatával
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
> * [REST](media-services-rest-encode-asset.md)
> * [Portál](media-services-portal-encode.md)
>
>

## <a name="overview"></a>Áttekintés
toodeliver digitális videót hello interneten keresztül, kell tömöríteni hello adathordozót. Digitális videofájlok nagyok, és lehet, hogy túl nagy toodeliver hello interneten keresztül, vagy az ügyfelek eszközök toodisplay megfelelően. Kódolás az tömörítés videó és hang, így az ügyfelek megtekintheti a media hello folyamat.

Kódolási feladatok olyan hello leggyakoribb feldolgozási műveletek Azure Media Services. Kódolási feladatok tooconvert médiafájlok hoz létre egy kódolási tooanother. Amikor kódolására, hello Media Services beépített kódoló (Media Encoder Standard) is használhatja. Egy Media Services partner által biztosított kódoló is használható. Külső kódolók hello Azure piactéren keresztül érhetők el. Hello részleteit kódolási feladatokhoz a kódoló-készlet karakterláncokat használatával, vagy előre definiált konfigurációs fájlok használatával adhatja meg. a rendelkezésre álló készletek toosee hello típusú lásd [feladat készletek a Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).

Minden egyes feladat egy vagy, hogy a feldolgozási hello típusától függően további feladatok tooaccomplish szeretné. Hello REST API-t, keresztül hozhat létre feladatokat és azok kapcsolódó feladatok az alábbi két módszer egyikével:

* Feladatok lehet meghatározott beágyazott hello feladatok navigációs tulajdonság a feladat entitások keresztül.
* OData kötegfeldolgozási használja.

Javasoljuk, hogy mindig kódolása egy adaptív sávszélességű MP4 állítsa be a forrásfájlokat, és alakítsa át hello beállítása toohello kívánt formátum használatával [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md).

Ha az kimeneti adategységen tárolótitkosítást alkalmaz, konfigurálnia kell a hello adategység továbbítási házirendjét. További információkért lásd: [objektumtovábbítási szabályzat konfigurálása](media-services-rest-configure-asset-delivery-policy.md).

## <a name="considerations"></a>Megfontolandó szempontok

A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre. További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).

Mielőtt media processzorok hivatkozik, győződjön meg arról, hogy rendelkezik-e hello helyes adathordozók processzor azonosítóját. További információkért lásd: [media processzorok beolvasása](media-services-rest-get-media-processor.md).

## <a name="connect-toomedia-services"></a>Connect tooMedia szolgáltatások

Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást. Meg kell nyitnia a további hívások toohello új URI.

## <a name="create-a-job-with-a-single-encoding-task"></a>Hozzon létre egy feladatot egyetlen kódolási feladat
> [!NOTE]
> Dolgozunk a hello Media Services REST API-t, ha hello következők érvényesek:
>
> A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre. További információkért lásd: [Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).
>
> Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást. Meg kell nyitnia a további hívások toohello új URI. Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).
>
> Ha JSON használatával és toouse hello megadásával **__metadata** hello kérelem (már például tooreferences csatolt objektum van hátra), meg kell adni a hello kulcsszó **elfogadás** fejléc túl[részletes JSON formátumban ](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Fogadja el: az application/json; odata = részletes.
>
>

hello a következő példa bemutatja, hogyan toocreate és a feladás egy vagy több feladat is egy feladatot állíthatja tooencode videó adott feloldási és minőségi. A Media Encoder Standard kódol, használhatja a megadott feladat konfigurációs készletek [Itt](http://msdn.microsoft.com/library/mt269960).

A kérelem:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net

    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "Adaptive Streaming", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Válasz:

    HTTP/1.1 201 Created

    . . .

### <a name="set-hello-output-assets-name"></a>– Hello kimeneti eszköz neve
hello a következő példa bemutatja, hogyan tooset hello assetName attribútum:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## <a name="considerations"></a>Megfontolandó szempontok
* TaskBody tulajdonságok literális XML toodefine hello száma bemeneti vagy kimeneti eszközök hello tevékenység által használt kell használnia. hello feladat témakör hello XML Schema definíció hello XML számára.
* Hello TaskBody definícióját, az egyes belső értékének <inputAsset> és <outputAsset> JobInputAsset(value) vagy JobOutputAsset(value) be kell állítani.
* Kimeneti adategységek feladatonként. Egy JobOutputAsset(x) csak egyszer használható egy feladatot a feladatok kimeneteként.
* A tevékenység bemeneti eszközként JobInputAsset vagy JobOutputAsset adhat meg.
* Feladatok kell nem alkotnak hurkot.
* hello érték paraméter átadására tooJobInputAsset vagy JobOutputAsset hello index értékét az adott eszköz számára jelöli. hello tényleges eszközök hello InputMediaAssets és hello entitás feladatdefiníció OutputMediaAssets navigációs tulajdonságainak vannak definiálva.
* A Media Services OData v3 épül, mert egyes eszközöket a hello InputMediaAssets hello és OutputMediaAssets navigációs tulajdonság gyűjtemények hivatkozott keresztül a "__metadata: uri" név-érték pár.
* InputMediaAssets tooone vagy további eszközök, a Media Services továbbítja. OutputMediaAssets hello rendszer hozza létre. Egy meglévő eszköz nem hivatkozó.
* OutputMediaAssets elnevezheti hello assetName attribútum használatával. Ha ez az attribútum nincs jelen, akkor hello OutputMediaAsset hello nevét bármilyen hello belső szöveges értékét hello <outputAsset> elem utótaggal hello feladat nevét, és közül hello feladatazonosító érték (hello esetet, ahol hello Name tulajdonság nincs megadva). Például ha assetName értékének beállítása túl "Sample", majd hello OutputMediaAsset Name tulajdonsága túl "Sample." Azonban ha assetName értékének beállítása nem, de adta meg az hello feladat neve túl "NewJob", majd hello OutputMediaAsset neve lenne "_NewJob JobOutputAsset (érték)."

## <a name="create-a-job-with-chained-tasks"></a>Hozzon létre egy feladatot láncolt feladatok
Számos alkalmazás forgatókönyvben fejlesztők szeretné toocreate feldolgozási feladatok sorozata. A Media Services szolgáltatásban láncolt feladatok sorozatát hozhat létre. Minden tevékenység különböző feldolgozó lépéseket hajtja végre, és különböző media processzorok használhatja. hello kapcsolt feladatok is egy eszköz kiosztják a feladat tooanother hello eszköz lineáris sorozatát feladatok végrehajtására. Azonban végrehajtott feladatokban hello feladatokat is nem szükséges toobe sorrendje egy sorozatban. Láncolt feladatot hoz létre, amikor hello kapcsolt **ITask** objektumok létrehozásakor egyetlen **IJob** objektum.

> [!NOTE]
> Jelenleg legfeljebb 30 tevékenységek maximális száma feladat. Ha több mint 30 feladatok kell toochain, hozzon létre több feladat toocontain hello feladatokat.
>
>

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


### <a name="considerations"></a>Megfontolandó szempontok
tooenable feladat láncolás:

* Egy feladat legalább két tevékenységet kell rendelkeznie.
* Legalább egy tevékenységet, amelynek a bemeneti érték egy másik feladat hello feladat kimenetének hello kell lennie.

## <a name="use-odata-batch-processing"></a>Használhatja a kötegelt feldolgozást OData
hello a következő példa bemutatja, hogyan toouse OData a batch-feldolgozási toocreate egy feladatot, és a feladatokat. Kötegfeldolgozási információkért lásd: [nyitott Data (OData) protokollnak kötegelt feldolgozása](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net


    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--



## <a name="create-a-job-by-using-a-jobtemplate"></a>Hozzon létre egy feladatot a JobTemplate használatával
Ha feldolgozni a több eszköz használatával gyakori feladatokat, a JobTemplate toospecify hello alapértelmezett feladat készletek használata vagy a feladatok tooset hello sorrendjét.

hello a következő példa bemutatja, hogyan toocreate egy JobTemplate egy TaskTemplate, amely a soron belül definiált. hello TaskTemplate Media Encoder Standard hello hello MediaProcessor tooencode hello eszköz fájlt használja. Azonban más MediaProcessors használható is.

    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }


> [!NOTE]
> Egyéb Media Services entitások, ellentétben minden TaskTemplate egy új GUID azonosító megadása és elhelyezheti a kérelem törzsében szereplő hello taskTemplateId és azonosító tulajdonsággal. hello tartalom azonosítási séma hajtsa végre az Azure Media Services entitások azonosítása ismertetett hello séma. JobTemplates is, nem lehet frissíteni. Ehelyett hozzon létre egy újat a frissített módosításokat.
>
>

Ha sikeres, a következő válasz hello adja vissza:

    HTTP/1.1 201 Created

    . . .


a következő példa azt mutatja meg hogyan hello toocreate egy feladatot, amely JobTemplate azonosítóra hivatkozik:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}


Ha sikeres, a következő válasz hello adja vissza:

    HTTP/1.1 201 Created

    . . .



## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Következő lépések
Most, hogy tudja, hogyan toocreate egy eszköz, a feladat tooencode: [hogyan toocheck feladat folyamatban van a Media Services](media-services-rest-check-job-progress.md).

## <a name="see-also"></a>Lásd még:
[Media processzorok beolvasása](media-services-rest-get-media-processor.md)
