---
title: "aaaAnalyze az adathordozó használatával hello Azure portálon |} Microsoft Docs"
description: "Ez a témakör ismerteti hogyan tooprocess a Médiaelemzés media processzor (felügyeleti csomagok) használatával médiatartalmakat hello Azure-portálon."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: d549c0315cd58c3771420379316b4f2ad3c2cea6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-media-using-hello-azure-portal"></a>Elemezze az adathordozó hello Azure-portál használatával
> [!NOTE]
> toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

## <a name="overview"></a>Áttekintés
Azure Services Médiaelemzés beszéd- és vizuális összetevők (a vállalati méretű, megfelelőség, biztonsági és globális reach), amelyek megkönnyítik a szervezetek és vállalatok számára tooderive gyakorlatban használható elemzések készítsenek gyűjteménye. További részletes Azure Media Services Analytics áttekintése: [ez](media-services-analytics-overview.md) témakör. 

Ez a témakör ismerteti hogyan tooprocess a Médiaelemzés media processzor (felügyeleti csomagok) használatával médiatartalmakat hello Azure-portálon. Media Analytics felügyeleti csomagok MP4- vagy JSON-fájlok előállításához. Ha egy media processzor MP4-fájlokat, hello fájl fokozatosan lehet letölteni. Ha egy media processzor egy JSON-fájl, hello fájlt letöltheti hello Azure blob Storage tárolóban. 

## <a name="choose-an-asset-that-you-want-tooanalyze"></a>Válasszon egy eszköz, amelyet az tooanalyze
1. A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.
2. A hello **beállítások** ablakban válassza ki **eszközök**.  
   .
    ![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze001.png)
3. Jelölje be hello eszköz, amelyet tooanalyze, és nyomja le az hello **elemzés** gombra.
   
    ![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. A hello **folyamat media eszköz Media Analytics** ablakban, a select hello processzor. 
   
    hello rest hello cikk azt ismerteti, miért és hogyan toouse minden processzor. 
5. Nyomja le az **létrehozása** toohello feladat indítása.

## <a name="azure-media-indexer"></a>Azure Media Indexer
Hello **Azure Media Indexer** media processzor lehetővé teszi toomake médiafájlok és a tartalom kereshető, valamint készítése lezárt feliratok nyomon követi. Ez a szakasz tájékoztatást nyújt a néhány, a felügyeleti csomag megadható beállítások.

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Nyelv
hello természetes nyelvű toobe hello multimédiás fájlban ismerhető fel. Például angol és spanyol. 

### <a name="captions"></a>Feliratok
Kiválaszthatja a fejléc formátuma nem hoz létre a tartalomról. Az indexelő feladat feliratfájlokat fájlok hozhat létre a következő formátumok hello:  

* **SZÁMI**
* **TTML**
* **WebVTT**

Bezárt felirat (CC) fájlok az alábbi formátumokban lehet használt toomake hang, és a videó elérhető toopeople nyújtanak segítséget fogyatékkal fájlokat.

### <a name="aib-file"></a>AIB fájl
Válassza ezt a lehetőséget, ha meg szeretné toogenerate hello hang Index Blob fájlt az például hello egyéni SQL Server IFilter. További információkért lásd: [ez](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blog.

### <a name="keywords"></a>Kulcsszavak
Válassza ezt a lehetőséget, ha azt szeretné, hogy toogenerate egy kulcsszavak XML-fájlt. Ez a fájl kulcsszavak hello beszéd tartalom, kinyert gyakorisággal és oszlopeltolási információ tartalmazza.

### <a name="job-name"></a>Feladat neve
Egy rövid nevet, amely lehetővé teszi a hello feladat azonosításához. [Ez](media-services-portal-check-job-progress.md) a cikk ismerteti, hogyan figyelheti a feladat előrehaladását hello. 

### <a name="output-file"></a>Kimeneti fájlja
Egy rövid nevet, amely lehetővé teszi a hello kimeneti tartalmat. 

## <a name="azure-media-hyperlapse"></a>Azure Media Hyperlapse
Az Azure Media Hyperlapse egy felügyeleti csomag által létrehozott zökkenőmentes idő letelt videók első, aki vagy művelet – kamera tartalomról.  További információ [ebben](media-services-hyperlapse-content.md) a témakörben érhető el. Ez a szakasz tájékoztatást nyújt a néhány, a felügyeleti csomag megadható beállítások.

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Gyorsaság
Adja meg hello sebesség mely toospeed hello bemeneti videó fel. hello eredménye egy stabil és az idő letelt verzióinak hello bemeneti videó.

### <a name="job-name"></a>Feladat neve
Egy rövid nevet, amely lehetővé teszi a hello feladat azonosításához. [Ez](media-services-portal-check-job-progress.md) a cikk ismerteti, hogyan figyelheti a feladat előrehaladását hello. 

### <a name="output-file"></a>Kimeneti fájlja
Egy rövid nevet, amely lehetővé teszi a hello kimeneti tartalmat. 

## <a name="azure-media-face-detector"></a>Azure Media Face Detector
Hello **Azure Media Arcfelismerési érzékelő** media processzor (MP) lehetővé teszi a toocount, nyomon követése típusú áthelyezések, és még a mérőműszer célközönség részvételét és reakciót arcfelismerést kifejezések keresztül. Ez a szolgáltatás két funkciókat tartalmazza: 

* **Arcfelismerési észlelése**
  
    Arcfelismerési észlelési talál, és nyomon követi a videó emberi lapjaira. Több lapokat észlelhető, és ezt követően nyomon követhetők egy JSON-fájl által visszaadott hello ideje és helye metaadatokkal körül, mozgás. Nyomon követése, során a program megpróbálja toogive egy egységes azonosító toohello azonos szembesülhetnek, amíg hello személy van Navigálás a képernyőn, még akkor is, ha kényszerítő vagy röviden hagyja hello keret.
  
  > [!NOTE]
  > A szolgáltatások nem végez arcfelismerést. Személy hello keret hagyja, vagy a válik fedhetik túl hosszú kap egy új Azonosítót amikor azok tér vissza.
  > 
  > 
* **Érzelemfelismerés**
  
    Érzelemfelismerés hello Arcfelismerési észlelési Media processzor ad vissza elemzés több érzelmi attribútum hello lapok észlel, például Boldogsága, sadness, félelem, utasítás és egyéb választható összetevőként. 

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Észlelési mód
A következő módok hello egyik hello processzor használhatja:

* arcfelismerési észlelése
* egy oldallal érzelemfelismerés
* összesített érzelemfelismerés

### <a name="job-name"></a>Feladat neve
Egy rövid nevet, amely lehetővé teszi a hello feladat azonosításához. [Ez](media-services-portal-check-job-progress.md) a cikk ismerteti, hogyan figyelheti a feladat előrehaladását hello. 

### <a name="output-file"></a>Kimeneti fájlja
Egy rövid nevet, amely lehetővé teszi a hello kimeneti tartalmat. 

## <a name="azure-media-motion-detector"></a>Azure Media Motion Detector
Hello **Azure Media mozgásérzékelő** media processzor (MP) lehetővé teszi, hogy Ön tooefficiently azonosítani egy egyébként hosszú és Eseménytelen video házirendsablonokkal szakaszait. Mozgásérzékelés statikus kamera felvételei tooidentify szakaszok hello videó hol mozgásérzékelési jelentkezik is használhatók. A metaadatok időbélyegeket és a régió, ahol hello esemény történt határolókeret hello tartalmazó JSON-fájlt hoz létre.

Cél biztonsági videó hírcsatornák, ez a technológia képes toocategorize mozgásérzékelési kapcsolódó eseményeket és vakriasztások például árnyékok és megvilágítási módosításokat. Ez lehetővé teszi a kamera hírcsatornák toogenerate biztonsági riasztások alatt címünkre végtelen irreleváns események, ugyanakkor nem képes tooextract perc múlva a rendkívül hosszú felügyeleti videók érdeklő nélkül.

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video Thumbnails
A processzor segítségével létre videók kijelölésével automatikusan érdekes kódtöredékek hello forrás videó. Ez akkor hasznos, ha azt szeretné, hogy milyen hosszú videó tooexpect gyors áttekintést tooprovide. Részletes útmutatást és példákat, lásd: [használata Azure Media Videoindexképek tooCreate egy videó összegzésének](media-services-video-summarization.md)

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Feladat neve
Egy rövid nevet, amely lehetővé teszi a hello feladat azonosításához. [Ez](media-services-portal-check-job-progress.md) a cikk ismerteti, hogyan figyelheti a feladat előrehaladását hello. 

### <a name="output-file"></a>Kimeneti fájlja
Egy rövid nevet, amely lehetővé teszi a hello kimeneti tartalmat. 

## <a name="next-steps"></a>Következő lépések
Nézet Media Services tanulási útvonalai.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

