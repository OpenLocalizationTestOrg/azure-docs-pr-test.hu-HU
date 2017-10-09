---
title: "egy Azure Media Services-fiók az Azure-portálon hello aaaCreate |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello hello Azure-portálon az Azure Media Services-fiók létrehozása."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: c551e158-aad6-47b4-931e-b46260b3ee4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: juliako
ms.openlocfilehash: fdad93d5d470fc08380670ec0f6c2d33468b1492
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-media-services-account-using-hello-azure-portal"></a>Hozzon létre egy Azure Media Services-fiók hello Azure-portál használatával
> [!div class="op_single_selector"]
> * [Portál](media-services-portal-create-account.md)
> * [PowerShell](media-services-manage-with-powershell.md)
> * [REST](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

hello Azure portál segítségével tooquickly Azure Media Services (AMS)-fiók létrehozása. A fiók tooaccess Media Services-meg toostore engedélyezése, titkosítására, kódolására, kezelése és az Azure media adatfolyamként is használhatja. Hello a Media Services-fiók létrehozása során, akkor is egy kapcsolódó tárfiók létrehozása (vagy használjon egy meglévőt) a hello, hello Media Services-fiókkal azonos földrajzi régióban.

Ez a cikk ismerteti néhány gyakori fogalmak, és bemutatja, hogyan toocreate egy Media Services fiók hello Azure-portálon.

## <a name="concepts"></a>Alapelvek
A Media Services szolgáltatásainak eléréséhez két kapcsolódó fiók szükséges:

* Egy Media Services-fiók. A biztosít hozzáférést a Media Services felhőalapú tooa készlete, amelyek az Azure-ban elérhető. A Media Services-fiók nem tárol tényleges médiatartalmakat. Ehelyett metaadatokat tárol hello médiatartalmak és médiafeldolgozási feladatokról a fiókban. Időben hello hello fiókot létrehoznia válassza ki az elérhető Media Services-régiót. hello választott régió egy adatközpont, amely a fiókhoz tartozó metaadat-bejegyzéseket hello tárolja.
  
* Egy Azure-tárfiók. Storage-fiókok hello kell elhelyezni, hello Media Services-fiókkal azonos földrajzi régióban. Egy Media Services-fiók létrehozásakor választhat meglévő tárfiók hello ugyanabban a régióban, vagy létrehozhat egy új tárfiókot a hello ugyanabban a régióban. Egy Media Services-fiók törlése esetén a kapcsolódó tárfiókban lévő hello blobok nem törlődnek.

> [!NOTE]
> A különböző régiókban lévő Azure Media Services-funkciók elérhetőségéről további információért lásd [az AMS-funkciók elérhetőségét az egyes adatközpontokban](scenarios-and-availability.md#availability).

## <a name="create-an-ams-account"></a>AMS-fiók létrehozása
hello lépéseket ezen témakör megjelenítése hogyan toocreate az AMS-fiók.

1. Jelentkezzen be hello [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **+New** > **Web + Mobile** > **Media Services** elemre.
   
    ![Media Services, létrehozás](./media/media-services-create-account/media-services-new1.png)
3. A **CREATE MEDIA SERVICES ACCOUNT** (Media Services-fiók létrehozása) részben adja meg a kívánt értékeket.
   
    ![Media Services, létrehozás](./media/media-services-create-account/media-services-new3.png)
   
   1. A **fióknév**, adja meg az új AMS-fiók hello hello nevét. Egy Media Services-fiók nevének minden kisbetűket és számokat, szóközök nélkül, és 3 too24 karakter hosszúságú.
   2. Az előfizetést hello Azure-előfizetések rendelkezik hozzáféréssel közül választhat.
   3. A **erőforráscsoport**, válassza ki az új és meglévő erőforráscsoportokra hello.  Az erőforráscsoport közös életciklussal, engedélyekkel és házirendekkel rendelkező erőforrások gyűjteménye. További információkat [itt](../azure-resource-manager/resource-group-overview.md#resource-groups) talál.
   4. A **hely**, válassza ki a földrajzi régiót, amelyben lesz használt toostore hello media és metaadat-bejegyzéseket a Media Services-fiókhoz tartozó hello. Ebben a régióban rendszer használt tooprocess, majd streamelni a médiafájlokat. Csak hello elérhető Media Services-régiók hello legördülő listában jelennek meg. 
   5. A **Tárfiók**, válassza ki a tárolási fiók tooprovide blob-tároló hello médiatartalom a Media Services-fiókhoz. Hello kiválaszthat egy meglévő tárfiókot használ, a Media Services-fiókját, vagy azonos földrajzi régióban hozhat létre egy tárfiókot. Egy új tárfiók létrehozása a hello azonos régióban. hello szabályok tárfiók neve hello ugyanaz, mint a Media Services-fiókok.
      
       További információkat a tárhelyről [itt](../storage/common/storage-introduction.md) talál.
   6. Válassza ki **PIN-kód toodashboard** toosee hello hello fiók központi telepítés végrehajtási állapotát.
4. Kattintson a **létrehozása** hello hello képernyő alsó részén.
   
    Hello fiók sikeres létrehozását követően – áttekintés oldalra tölti be. Hello streaming endpoint tábla hello fiókja rendelkeznek egy alapértelmezett streamvégpontból a hello **leállítva** állapotát. 

    >[!NOTE]
    >Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát. 
   
## <a name="toomanage-your-ams-account"></a>toomanage az AMS-fiók

toomanage az AMS-fiók (például toohello AMS API programozott módon való kapcsolódás, videók feltöltése, kódolása, a tartalom védelmének konfigurálása, feladatok előrehaladásának figyeléséhez) kiválasztása **beállítások** a bal oldalán található hello portal hello. A hello **beállítások**, keresse meg a rendelkezésre álló paneleken hello tooone (például: **API-hozzáférés**, **eszközök**, **feladatok**, **Védelmi tartalom**).


## <a name="next-steps"></a>Következő lépések

Most már feltölthet fájlokat AMS-fiókjába. További információk: [Fájlok feltöltése](media-services-portal-upload-files.md).

Ha programozottan tooaccess AMS API, lásd: [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

