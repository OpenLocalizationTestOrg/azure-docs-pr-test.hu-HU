---
title: "Fájlok feltöltése Media Services-fiókba az Azure Portalon | Microsoft Docs"
description: "Ez az oktatóprogram végigvezeti azon lépéseken, amelyek segítségével fájlokat tölthet fel egy Media Services-fiókba az Azure Portalon."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 7ddfe44918b358a1749640d1c93dba490855cc5a
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/18/2017
---
# <a name="upload-files-to-a-media-services-account-in-the-azure-portal"></a>Fájlok feltöltése Media Services-fiókba az Azure Portalon
> [!div class="op_single_selector"]
> * [Portál](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> 
> [!NOTE]
> Az oktatóanyag elvégzéséhez egy Azure-fiókra lesz szüksége. További részletek: [Ingyenes Azure-próbafiók](https://azure.microsoft.com/pricing/free-trial/). 
> 

Az Azure Media Services szolgáltatásban a digitális fájlok feltöltése egy objektumba történik. Az objektum tartalmazhat videót, hangot, képeket, miniatűröket, szövegsávokat és feliratfájlokat (valamint mindezen fájlok metaadatait). A fájlok feltöltése után a tartalom a felhőben lesz biztonságosan tárolva további feldolgozás és streamelés céljából.

> [!NOTE]
> A Media Services a fájlok feldolgozása esetében felső fájlméret-korlátozást alkalmaz. További részletek a fájlméret-korlátozásokról: [Media Services – Kvóták és korlátozások](media-services-quotas-and-limitations.md).
>

## <a name="upload-files"></a>Fájlok feltöltése
1. Az [Azure-portálon](https://portal.azure.com/) válassza ki Azure Media Services-fiókját.
2. Válassza a **Settgings (Beállítások)** > **Assets (Objektumok)** lehetőséget. Ezután válassza ki a **Feltöltés** gombot.
   
    ![Fájlok feltöltése](./media/media-services-portal-vod-get-started/media-services-upload.png)
   
    Megjelenik az **Upload a video asset** (Videóobjektum feltöltése) ablak.
   
   > [!NOTE]
   > A Media Services nem korlátozza a feltölteni kívánt videók méretét.
 
3. Keresse meg a számítógépén a feltölteni kívánt videót. Jelölje ki a videót, majd válassza az **OK** lehetőséget.  
   
    Megkezdődik a feltöltés. A folyamat előrehaladását a fájlnév alatt követheti nyomon.  

A feltöltést követően az új objektum megjelenik az **Objektumok** panelen. 

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Következő lépések
* Ismerkedjen meg a [feltöltött objektumok kódolásának](media-services-portal-encode.md) módjával.

* Emellett az Azure Functions használatával is elindíthatja a kódolási feladatokat a fájloknak a konfigurált tárolóba történő érkezésekor. További információkért tekintse meg a [Media Services: Az Azure Media Services integrálása Azure Functions- és Logic Apps- alkalmazásokkal](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/) cikkben található mintát.


