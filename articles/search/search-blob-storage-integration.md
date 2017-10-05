---
title: "Az Azure Search ad hozzá a Blob Storage |} Microsoft Docs"
description: "Index létrehozása kódból az Azure Search HTTP REST API használatával."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
ms.service: search
ms.topic: article
ms.date: 05/04/2017
ms.author: ashmaka
ms.openlocfilehash: 0cd0cbb05c465d32a9ef02f9350ebdf9ccea36c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="searching-blob-storage-with-azure-search"></a>Keresés a Blob Storage-tárolókban az Azure Search szolgáltatással

A különböző Azure Blob storage-ban tárolt tartalomtípusok teljes megoldására nehéz problémát okozhat. Azonban index és a BLOB tartalmát keresni néhány kattintással Azure Search használatával. Egy Azure Search szolgáltatás kiépítését keres a Blob-tároló igényli. A különböző szolgáltatások korlátait és az Azure Search árképzési szinteket megtalálható a [árképzést ismertető oldalra](https://aka.ms/azspricing).

## <a name="what-is-azure-search"></a>Mi az az Azure Search?
[Az Azure Search](https://aka.ms/whatisazsearch) egy keresési megoldás, amely megkönnyíti, hogy a fejlesztők számára a hatékony teljes szöveges keresés észlel hozzáadása webes és mobilalkalmazásokhoz. Szolgáltatást Azure Search szükségtelenné ajánlat során bármilyen keresési infrastruktúra kezelése érdekében egy [99,9 %-os hasznos üzemidő SLA](https://aka.ms/azuresearchsla).

Azure Search 56 nyelven támogatása speciális, a tartalom elemezheti és intelligens módon kezeli az nyelvspecifikus szerkezeteket. Az Azure Search olyan keresési gazdag felhasználói élmény létrehozásához eszközöket is biztosít. Használata egyszerű, szolgáltatások, mint a jellemzőalapú navigáció, typeahead keresési javaslatok, és kattintson az Azure Search használatával felhasználói felületek elembe kiemelve. Azure Search funkciókkal kapcsolatos további tudnivalókért olvassa el a szolgáltatás [dokumentáció](https://aka.ms/azsearchdocs).

## <a name="crack-open-and-search-through-the-content-of-enterprise-document-formats"></a>Repedés nyissa meg és keressen a tartalom a vállalati dokumentum formátumok keresztül
A támogató [kibontási dokumentum](https://aka.ms/azsblobindexer) az Azure Blob storage Azure Search indexelheti a blobok tárolt fájltípusok számos tartalma:
- PDF
- A Microsoft Office: DOCX/DOC, XLSX/XLS, PPTX/PPT, Adatköltségek (Outlook e-mailek)
- HTML
- Egyszerű szöveges fájlokról

Szöveg és az ilyen típusú metaadatok kibontása, egyszerű keresés több fájl formátumok rendszerektől függetlenül a leginkább releváns dokumentumokban található egyetlen lekérdezés esetén. A tartalom és a Microsoft Office-dokumentumokat, PDF-fájlok és e-mailek metaadatok indexelést is lehet a Blob storage és az Azure Search használatával hatékony nagyvállalati tartalomkezelési megoldás létrehozása.

## <a name="search-through-your-blob-metadata"></a>A blob-metaadatok közötti keresésre
Egy gyakori forgatókönyv, amely megkönnyíti a blobok bármely tartalomtípus végigvesszük az egyéni, felhasználó által definiált blob metaadatait, valamint a rendszer tulajdonságai indexelésre az egyes blobok. Ezzel a módszerrel minden egyes blob adatait indexelt függetlenül dokumentumtípusra a blob, így könnyen rendezheti és a dimenzió összes Blob storage tartalmat.

[Ismerje meg, további információk az indexelő blob-metaadatok.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Kép keresése
Az Azure Search teljes szöveges keresés, jellemzőalapú navigáció és rendezési lehetőségeket is érvényesek a blobok tárolt képek metaadatait.

Ha ezeket a lemezképeket előre feldolgozása használatával a [számítógép Látástechnológiai API](https://www.microsoft.com/cognitive-services/computer-vision-api) a Microsoft kognitív szolgáltatást, majd lehetőség a vizuális tartalom található egyes lemezképek és a kézírás-felismerési beleértve az index. Jelenleg is dolgozunk OCR hozzáadása és egyéb kép feldolgozási funkciói közvetlenül Azure Search, ha ezek a képességek érdekli küldjön egy kérést a a [UserVoice](https://aka.ms/azsuv) vagy [nekünk e-mailben](mailto:azscustquestions@microsoft.com).

## <a name="index-and-search-through-json-blobs"></a>Index és a keresési JSON-blobok keresztül
Az Azure Search beállítható úgy, hogy a kibontani strukturált blobok JSON tartalmazó található. Az Azure Search JSON-blobok olvashatja, és a strukturált tartalom elemzése az Azure Search dokumentum a megfelelő mezőkbe. Az Azure Search is számos blobot, amely tartalmazza JSON-objektumok tömbje és hozzárendelését az egyes elemei egy külön Azure Search-dokumentumot.

Vegye figyelembe, hogy JSON elemzése nem jelenleg konfigurálható a portálon keresztül. [További tudnivalók az Azure Search elemzése JSON.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Első lépések
Az Azure Search közvetlenül a Blob storage portálpaneljéhez blobok is hozzáadhatók.

![](./media/search-blob-storage-integration/blob-blade.png)

Kattintson az "Azure Search hozzáadása" lehetőséget a elindítja a folyamat, ahol válasszon ki egy meglévő Azure Search szolgáltatást, vagy hozzon létre egy új szolgáltatást. Ha új szolgáltatás létrehozása nyílik a tárfiók portál élmény kívül. Szüksége lesz újra keresse meg a tárolási portál panel, és jelölje ki újból az "Azure Search hozzáadása" lehetőséget, ahol kiválaszthatja a létező szolgáltatás.

### <a name="next-steps"></a>Következő lépések
További tudnivalók az Azure Search Blob indexelő a teljes [dokumentáció](https://aka.ms/azsblobindexer).
