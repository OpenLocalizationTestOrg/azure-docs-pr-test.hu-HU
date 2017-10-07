---
title: "az Azure Media Services-fiók Azure StorSimple aaaUpload fájlok |} Microsoft Docs"
description: "Ez a cikk rövid áttekintést nyújt az Azure StorSimple Data Managerről. hello cikket is hivatkozásokat tartalmaz, amelyek bemutatják, hogyan tootutorials StorSimple tooextract adatait, és töltse fel eszközök tooan Azure Media Services-fiók."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 1dd09328-262b-43ef-8099-73241b49a925
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/27/2017
ms.author: juliako
ms.openlocfilehash: 7e9712aa480106bbd5fcc63eaecf0418b24a8bef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-an-azure-media-services-account-from-azure-storsimple"></a>Fájlok feltöltése Azure Media Services-fiókba az Azure StorSimple-ből

Ez a cikk rövid áttekintést nyújt az Azure StorSimple Data Managerről. hello cikket is hivatkozásokat tartalmaz, amelyek bemutatják, hogyan tootutorials StorSimple tooextract adatait, majd töltse fel ezeket az adatokat, az eszközök tooan Azure Media Services (AMS) fiók.

> 
> [!NOTE]
> Az Azure StorSimple Data Manager jelenleg privát előzetes verzióban érhető el. 
> 

## <a name="overview"></a>Áttekintés

A Media Services szolgáltatásban a digitális fájlok feltöltése egy adategységbe történik. hello eszköz tartalmazhat videó, hang, képeket, miniatűröket, szöveg nyomon követi és feliratfájlokat fájlokat (és mindezen fájlok metaadatait hello.) Miután hello fájlok feltöltése után a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő.

[Az Azure StorSimple](https://docs.microsoft.com/azure/storsimple/) által használt felhőbeli tárhelyén hello kiterjesztése a helyszíni megoldás, és automatikusan tiers adatok között hello a helyszíni és felhőalapú tárolására. hello StorSimple eszköz dedupes, és tömöríti az adatokat, így nagy fájlok toohello felhő küldési nagyon hatékony toohello felhő elküldés előtt. Hello [StorSimple adatkezelő](../storsimple/storsimple-data-manager-overview.md) szolgáltatást biztosít az API-k, amelyek lehetővé teszik az Ön tooextract adatok a StorSimple és mutatni az AMS-eszközök.

## <a name="get-started"></a>Bevezetés

1. [Media Services-fiók létrehozása](media-services-portal-create-account.md) szeretné tootransfer hello eszközök.
2. Regisztrálhat adatkezelő előzetes verziójára, a hello [StorSimple adatkezelő](../storsimple/storsimple-data-manager-overview.md) cikk.
3. Hozzon létre egy StorSimple Data Manager-fiókot.
4. Hozzon létre egy adatátalakítási feladatot, amely a futtatásakor adatokat gyűjt egy StorSimple-eszközről, és továbbítja azokat objektumokként egy AMS-fiókba. 

    Amikor hello feladat elindul, a tároló várólista jön létre. A sorba az átalakított blobokkal kapcsolatos üzenetek kerülnek, amint a blobok elkészültek. a várólista nevét hello van hello ugyanaz, mint a hello feladatdefiníció hello nevét. A várólista toodetermine használható amikor eszköz, készen áll, és hívja meg a kívánt Media Services művelet toorun rajta. Például a várólista tootrigger egy Azure-függvény, amely rendelkezik a szükséges kód hello a Media Services azt is használhatja.

## <a name="see-also"></a>Lásd még:

[Használjon .net SDK hello hello adatkezelő tootrigger feladatok](../storsimple/storsimple-data-manager-dotnet-jobs.md)

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Következő lépések

Most már kódolhatja a feltöltött adategységeket. További információ: [Encode Assets](media-services-portal-encode.md) (Adategységek kódolása).
