---
title: "az Azure Médiaelemzés használatával forgatókönyv aaaRedact lapok |} Microsoft Docs"
description: "Ez a témakör ismerteti részletesen hogyan toorun egy teljes kivonási munkafolyamat Azure Media Services Explorer (AMSE) és az Azure Media Redactor Vizualizálója (nyílt forráskódú eszköz) használatával."
services: media-services
documentationcenter: 
author: Lichard
manager: cfowler
editor: 
ms.assetid: d6fa21b8-d80a-41b7-80c1-ff1761bc68f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/03/2017
ms.author: rli; juliako;
ms.openlocfilehash: ab28f4052b73fdb74fcd5766235eab35402a0c9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a>Az Azure Médiaelemzés használatával forgatókönyv lapok kivonása

## <a name="overview"></a>Áttekintés

**Az Azure Media Redactor** van egy [Azure Médiaelemzés használatával](media-services-analytics-overview.md) media processzor (MP), amely méretezhető arcfelismerési kivonási hello felhőben nyújt. Arcfelismerési kivonási lehetővé teszi, hogy Ön toomodify a rendelés tooblur felületei kijelölt személyek a videó. Érdemes lehet toouse hello arcfelismerési kivonási szolgáltatás nyilvános biztonsági és hírek media forgatókönyvekben. Több lapokat tartalmazó felvételei, néhány percet is igénybe vehet óra tooredact manuálisan, de a szolgáltatás hello arcfelismerési a kivonási folyamat néhány egyszerű lépésben szükséges. További információkért lásd: [ez](https://azure.microsoft.com/blog/azure-media-redactor/) blog.

Vonatkozó további információért **Azure Media Redactor**, lásd: hello [Arcfelismerési kivonási áttekintése](media-services-face-redaction.md) témakör.

Ez a témakör ismerteti részletesen hogyan toorun egy teljes kivonási munkafolyamat Azure Media Services Explorer (AMSE) és az Azure Media Redactor Vizualizálója (nyílt forráskódú eszköz) használatával.

Hello **Azure Media Redactor** felügyeleti csomag jelenleg előzetes verzió. Érhető el az összes Azure-régiók, valamint Amerikai Egyesült államokbeli kormányzati és Kína adatközpontokban. Ez az előnézet jelenleg díjmentesen. Hello a jelenlegi kiadásban 10 perces korlátozva van a feldolgozott videó hossza.

További információkért lásd: [ez](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blog.

## <a name="azure-media-services-explorer-workflow"></a>Az Azure Media Services Explorer munkafolyamat

hello legegyszerűbb módja tooget használatába Redactor toouse hello nyílt forráskódú AMSE eszköz a githubon. Keresztül egyszerűsített munkafolyamat futtatása **kombinált** mód, ha már nem kell használni toohello jegyzet json vagy hello arcfelismerési jpg lemezképet.

### <a name="download-and-setup"></a>A letöltés és telepítés

1. Töltse le az AMSE eszköz hello [Itt](https://github.com/Azure/Azure-Media-Services-Explorer).
1. Jelentkezzen be a szolgáltatás kulccsal Media Services-fiók tooyour.

    tooobtain hello fióknevet és a kulcsadatokat, nyissa meg toohello [Azure-portálon](https://portal.azure.com/) válassza ki az AMS-fiók. Válassza a beállítások > kulcsok. hello kezelése kulcsok windows hello-fiók nevét jeleníti meg, és hello elsődleges és másodlagos kulcsot. Másolja a hello fióknevet és a hello elsődleges kulcs.

![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a>Először adja át – mód elemzése

1. A médiafájl keresztül eszköz feltöltés –> feltöltési, sem a fogd és vidd segítségével. 
1. Kattintson a jobb gombbal, és dolgozza fel a médiafájl Médiaelemzés használatával Azure Media Redactor –> –> elemzési módot. 


![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

hello kimeneti egy jegyzetek json-fájl, arc helyadatok, valamint minden észlelt felületen jpg tartalmazza. 

![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

###<a name="second-pass--redact-mode"></a>Második fázis – mód kivonása

1. Töltse fel az eredeti video asset toohello hello első fázis kimenetét, és állítsa be elsődleges eszközként. 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. (Választható) Töltse fel egy "Dance_idlist.txt" fájlt, amely hello tooredact kívánja azonosítók Sortöréssel elválasztott listáját tartalmazza. 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. (Választható) Hogy bármely módosításokat toohello annotations.json fájl például növelése hello határoló mező határait. 
4. Hello kimeneti eszköz az első fázis hello kattintson a jobb gombbal, válassza ki a hello Redactor, futtassa a hello **Redact** mód. 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. Töltse le, vagy a megosztási hello végső kivont kimenetet eszköz. 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

##<a name="azure-media-redactor-visualizer-open-source-tool"></a>Azure Media Redactor Vizualizálója nyílt forráskódú eszköz

Egy nyílt forráskódú [vizualizálója eszköz](https://github.com/Microsoft/azure-media-redactor-visualizer) nemrég kezdte hello jegyzetek formátumú elemzése, és hello kimeneti használatával kialakított toohelp fejlesztők.

Hello tárház rendelés toorun hello projektben klónozását kell toodownload FFMPEG a saját [hivatalos webhely](https://ffmpeg.org/download.html).

Ha egy fejlesztő tooparse hello JSON jegyzet adatokat próbált, nyissa meg Models.MetaData minta kód példákat.

### <a name="set-up-hello-tool"></a>Hello eszköz beállítása

1.  Töltse le és hello teljes megoldás felépítéséhez. 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  Töltse le a FFMPEG [Itt](https://ffmpeg.org/download.html). Ez a projekt eredetileg fejlesztettek verzió be1d324 (2016-10-04) a statikus hivatkozást. 
3.  Másolja a ffmpeg.exe és ffprobe.exe toohello AzureMediaRedactor.exe megegyező kimeneti mappában. 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. Futtassa a AzureMediaRedactor.exe. 

### <a name="use-hello-tool"></a>Hello eszközzel

1. A videó az Azure Media Services-fiókban hello Redactor felügyeleti csomag az elemzési módot a feldolgozni. 
2. Töltse le a hello eredeti videofájl és a kivonási hello hello kimenete, mert a feladat elemzése. 
3. Hello vizualizálója alkalmazás futtatásához, és válassza ki a fenti hello fájlok. 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. Tekintse meg a fájlt. Válassza ki azt az oldal akkor tooblur hello oldalsávon a hello keresztül szeretné jobb. 
    
    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  hello alsó szövegmező hello arcfelismerési azonosítók frissíteni fogja. Ezek az azonosítók "idlist.txt" nevű fájl létrehozása Sortöréssel elválasztott listáját. 

    >[!NOTE]
    > ANSI hello idlist.txt kell menteni. A Jegyzettömb toosave ANSI használható.
    
6.  Töltse fel a fájl toohello kimeneti adategységen 1. lépésben. Töltse fel a hello eredeti videó toothis eszköz is, és állítsa be elsődleges eszközként. 
7.  Ez az eszköz "Redact" mód tooget hello végső kivont videó kivonási feladat fut. 

## <a name="next-steps"></a>Következő lépések 

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Kapcsolódó hivatkozások
[Azure Media Services elemző áttekintése](media-services-analytics-overview.md)

[Az Azure Médiaelemzés bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

[Az Azure Médiaelemzés használatával Arcfelismerési kivonási bejelentése](https://azure.microsoft.com/blog/azure-media-redactor/)
