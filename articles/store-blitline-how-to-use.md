---
title: "aaaHow toouse Blitline kép feldolgozásra - Azure szolgáltatás útmutató"
description: "Ismerje meg, hogyan toouse hello Blitline szolgáltatást az Azure-alkalmazások tooprocess képek."
services: 
documentationcenter: .net
author: blitline-dev
manager: jason@blitline.com
editor: jason@blitline.com
ms.assetid: 6c711248-0e52-4895-ba9e-8395628de924
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2014
ms.author: support@blitline.com
ms.openlocfilehash: 328fd177e25f45f29f8ad8e142d02b46017a858e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blitline-with-azure-and-azure-storage"></a>Hogyan toouse Blitline Azure és az Azure tárolással
Ez az útmutató alapján meghatározható, hogyan tooaccess Blitline szolgáltatásokat, és hogyan toosubmit feladatok tooBlitline.

## <a name="what-is-blitline"></a>Mi az a Blitline?
Blitline a felhő alapú lemezkép feldolgozási szolgáltatás, amely egy azon részét, hogy toobuild volna áron hello ár vállalati szintű kép feldolgozását, saját magának.

hello tény, hogy lemezkép nincs feldolgozva többször, általában az újonnan létrehozott minden webhely hello alapoktól. A Microsoft vegye figyelembe, ez mivel azt létrehozott őket egy millió alkalommal túl. Egy nap döntöttünk, hogy lehet, hogy a rendszer idő azt csak erre mindenki számára. Tudjuk, hogyan toodo azt, akkor gyors és hatékony, és mentse mindenki működni a hello addig toodo.

További információkért lásd: [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Mi Blitline nincs...
tooclarify mi Blitline a hasznos, ez gyakran könnyebb tooidentify Blitline nem mire soron előtt.

* Blitline nincs HTML widgeteket tooupload képek. Lemezképeket nyilvánosan vagy Blitline tooreach érhető el a korlátozott engedélyekkel kell rendelkeznie.
* Blitline nem végez feldolgozást, például a Aviary.com élő kép
* Blitline nem fogadja el a képek feltöltését, közvetlenül nem lehet leküldéssel terjeszteni a képek tooBlitline. Kell küldje le őket tooAzure tároló vagy más helyen Blitline támogatja, és majd kérje meg a Blitline, ahol toogo megkapja őket.
* Blitline nagymértékben párhuzamos, és nem végez semmilyen szinkron feldolgozásra. Ezért meg kell biztosítják a postback_url, és azt segítségével megállapíthatja, hogy ha igazolnia befejezése feldolgozásra.

## <a name="create-a-blitline-account"></a>Blitline-fiók létrehozása
[!INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-toocreate-a-blitline-job"></a>Hogyan toocreate Blitline feladat
Blitline JSON-toodefine hello műveleteket, amelyeket a lemezkép tootake használja. Ez a JSON néhány egyszerű mezők tevődik össze.

hello legegyszerűbb például a következőképpen történik:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Itt a "src" lemezkép kezeléséért JSON tudunk "... boys.jpeg" és a lemezkép too240x140 majd átméretezése.

hello Alkalmazásazonosító valami megtalálja a **KAPCSOLATINFORMÁCIÓ** vagy **kezelése** Azure fülre. A titkos azonosítója, amely lehetővé teszi a Blitline toorun feladatok is.

"Mentés" paraméter hello hol szeretné tooput hello kép után azt feldolgozta azonosítja. A trivial esetben még nem meghatározott egyet. Ha nincs megadva helykód Blitline fogja tárolni, helyileg (és ideiglenesen) egy egyedi felhő helyen. A hely, ahonnan hello JSON Blitline által visszaadott, amikor hello Blitline képes tooget lesz. hello "lemezképpel" azonosító szükség esetén és tooyou mentésekor tooidentify az adott képet.

További információ a hello található *funkciók* támogatjuk itt: <http://www.blitline.com/docs/functions>

Is találhat hello kapcsolatos feladat beállítások itt: <http://www.blitline.com/docs/api>

Ha elvégezte a JSON toodo van szüksége **POST** azt túl`http://api.blitline.com/job`

Az alábbihoz hasonló JSON vissza jelenik meg:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Igen, akkor Blitline megkapta a kérelmet, azt állította a feldolgozási sorban, és befejezését követően érhető el lesz-e a lemezkép hello: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_azonosítója /CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-toosave-an-image-tooyour-azure-storage-account"></a>Hogyan toosave egy kép tooyour Azure Storage-fiókban
Ha egy Azure Storage-fiókot, könnyedén lehet Blitline leküldéses feldolgozott hello képek az Azure-tárolóba. Egy "azure_destination" hozzáadásával meghatározása a hello helyét és a Blitline toopush engedélyeit.

Például:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Hello CAPITALIZED értékeket a saját kitöltésével elküldheti a JSON toohttp://api.blitline.com/job és hello "src" kép életlenítés szűrő dolgoz fel, és majd leküldött tooyou Azure cél.

### <a name="please-note"></a>Megjegyzés:
hello SAS hello teljes SAS URL-cím, beleértve a célfájl hello hello fájlnevet kell tartalmaznia.

Példa:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Is olvasható Blitline tartozó Azure Storage docs legújabb kiadása hello [Itt](http://www.blitline.com/docs/azure_storage).

## <a name="next-steps"></a>Következő lépések
Látogasson el az egyéb funkciókkal kapcsolatos blitline.com tooread:

* Blitline API-végpont Docs <http://www.blitline.com/docs/api>
* Blitline API-függvények <http://www.blitline.com/docs/functions>
* Blitline API példák <http://www.blitline.com/docs/examples>
* Harmadik része a Nuget könyvtár <http://nuget.org/packages/Blitline.Net>

