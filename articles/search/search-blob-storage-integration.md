---
title: "aaaAdding Azure Search tooBlob tárolási |} Microsoft Docs"
description: "Index létrehozása kódban hello Azure Search HTTP REST API használatával."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
ms.service: search
ms.topic: article
ms.date: 05/04/2017
ms.author: ashmaka
ms.openlocfilehash: a3801790067bf169693b500e83799286b7387a77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="searching-blob-storage-with-azure-search"></a>Keresés a Blob Storage-tárolókban az Azure Search szolgáltatással

Teljes Azure Blob storage-ban tárolt tartalomtípusok hello számos lehet egy nehéz probléma toosolve. Azonban index, és keresse a blobot, amely néhány kattintással tartalmának hello Azure Search használatával. Egy Azure Search szolgáltatás kiépítését keres a Blob-tároló igényli. hello különböző szolgáltatásra vonatkozó korlátozások és az Azure Search tarifacsomagok található hello [árképzést ismertető oldalra](https://aka.ms/azspricing).

## <a name="what-is-azure-search"></a>Mi az az Azure Search?
[Az Azure Search](https://aka.ms/whatisazsearch) egy keresési megoldás, amely megkönnyíti a fejlesztők tooadd robusztus teljes szöveges keresés észlel tooweb és mobilalkalmazásokhoz. Szolgáltatást Azure Search eltávolítja hello kell toomanage bármely keresési infrastruktúra ajánlat során egy [99,9 %-os hasznos üzemidő SLA](https://aka.ms/azuresearchsla).

Azure Search 56 nyelven támogatása speciális, a tartalom elemezheti és intelligens módon kezeli az nyelvspecifikus szerkezeteket. Az Azure Search keresési gazdag felhasználói élmény is biztosít hello eszközök toobuild. Egyszerű tooadd szolgáltatások, mint a jellemzőalapú navigáció, az typeahead keresési javaslatok és az Azure Search használatával találati kijelölő toouser felületeinek. Azure Search funkciókkal kapcsolatos toolearn, hello szolgáltatás olvasható [dokumentáció](https://aka.ms/azsearchdocs).

## <a name="crack-open-and-search-through-hello-content-of-enterprise-document-formats"></a>Repedés nyissa meg és keressen keresztül a vállalati dokumentum formátumok hello tartalom
A támogató [kibontási dokumentum](https://aka.ms/azsblobindexer) az Azure Blob storage Azure Search indexelheti a blobok tárolt fájltípusok számos hello tartalma:
- PDF
- A Microsoft Office: DOCX/DOC, XLSX/XLS, PPTX/PPT, Adatköltségek (Outlook e-mailek)
- HTML
- Egyszerű szöveges fájlokról

Szöveg- és az ilyen típusú metaadatok kivonja a könnyen toosearch több fájl formátumok egy egyetlen lekérdezés toofind hello legfontosabb dokumentumok rendszerektől függetlenül is. A Microsoft Office-dokumentumok, PDF-fájlok, és e-mailek, az lehetséges toobuild tartalom és hello metaadatok hello Indexeljük Blob-tároló és az Azure Search használatával hatékony nagyvállalati tartalomkezelési megoldás.

## <a name="search-through-your-blob-metadata"></a>A blob-metaadatok közötti keresésre
Egy gyakori forgatókönyv, amely megkönnyíti a blobok bármely tartalomtípus keresztül egyszerűen toosort van tooindex hello egyéni, felhasználó által definiált blob metaadatait, valamint a hello Rendszertulajdonságok az egyes blobok. Ezzel a módszerrel minden egyes blob adatait indexelt függetlenül hello típusú dokumentumot hello blob, így könnyen rendezheti és a dimenzió összes Blob storage tartalmat.

[Ismerje meg, további információk az indexelő blob-metaadatok.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Kép keresése
Az Azure Search teljes szöveges keresés, jellemzőalapú navigáció és rendezési szolgáltatásokkal mostantól lehet blobok tárolt képek alkalmazott toohello metaadatait.

Ha ezeket a lemezképeket előre feldolgozása hello segítségével [számítógép Látástechnológiai API](https://www.microsoft.com/cognitive-services/computer-vision-api) a Microsoft kognitív szolgáltatást, akkor az lehetséges tooindex hello vizuális tartalom található az egyes képet, és a kézírás-felismerési így. Jelenleg is dolgozunk és a többi kép feldolgozási képességek közvetlen tooAzure keresés, ha ezek a képességek érdekli küldjön egy kérést a hozzáadása a [UserVoice](https://aka.ms/azsuv) vagy [nekünk e-mailben](mailto:azscustquestions@microsoft.com).

## <a name="index-and-search-through-json-blobs"></a>Index és a keresési JSON-blobok keresztül
Az Azure Search strukturált konfigurált tooextract tartalom található a blobok JSON tartalmazó lehet. Az Azure Search olvashatja a JSON-blobok és strukturált hello tartalom elemzése az Azure Search dokumentum hello megfelelő mezőkbe. Az Azure Search is igénybe vehet, amely tartalmazza a JSON-objektumok tömbje, és képezze le minden elem tooa külön Azure Search dokumentum blobok is.

Vegye figyelembe, hogy JSON elemzése nem jelenleg konfigurálható hello portálon keresztül. [További tudnivalók az Azure Search elemzése JSON.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Első lépések
Az Azure Search közvetlenül a hello Blob storage portál panel tooblobs adhatók hozzá.

![](./media/search-blob-storage-integration/blob-blade.png)

Kattintson a "Azure Search hozzáadása" lehetőséget hello indít, a folyamat, ahol válasszon ki egy meglévő Azure Search szolgáltatást, vagy hozzon létre egy új szolgáltatást. Ha új szolgáltatás létrehozása nyílik a tárfiók portál élmény kívül. Szüksége lesz a toore-toohello tárolási portál panel keresse meg és jelölje ki újból a hello "Azure Search hozzáadása" lehetőséget, ahol kiválaszthatja a meglévő hello szolgáltatást.

### <a name="next-steps"></a>Következő lépések
További információ a teljes hello Azure keresési Blob indexelő hello [dokumentáció](https://aka.ms/azsblobindexer).
