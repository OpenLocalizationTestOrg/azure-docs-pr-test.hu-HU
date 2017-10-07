---
title: "AAA\"hello Azure-portál használatával Media Services-fiók fájlokat feltöltés |} Microsoft dokumentumok\""
description: "Ez az oktatóanyag végigvezeti hello fájlok feltöltése a Media Services-fiók hello Azure-portál használatával"
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
ms.openlocfilehash: 4ce1e133c72854532735ba7c72a43c92a75bc240
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-hello-azure-portal"></a>Fájlok feltöltése a Media Services-fiók hello Azure-portál használatával
> [!div class="op_single_selector"]
> * [Portál](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> 
> [!NOTE]
> toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/). 
> 


A Media Services szolgáltatásban a digitális fájlok feltöltése egy adategységbe történik. hello eszköz tartalmazhat videó, hang, képeket, miniatűröket, szöveg nyomon követi és feliratfájlokat fájlokat (és mindezen fájlok metaadatait hello.) Miután hello fájlok feltöltése után a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő.


## <a name="upload-files"></a>Fájlok feltöltése

>[!NOTE]
>Nincs a Media Services feldolgozás támogatott korlátot toohello fájl maximális méretét. Ellenőrizze a [ez](media-services-quotas-and-limitations.md) témakör hello méretű fájlt választhat vonatkozó további információért.
>

1. A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.
2. A hello **beállítások** panelen kattintson a **eszközök**.
   
    ![Fájlok feltöltése](./media/media-services-portal-vod-get-started/media-services-upload.png)
3. Kattintson a hello **feltöltése** gombra.
   
    Hello **töltse fel a video asset** ablak jelenik meg.
   
   > [!NOTE]
   > Tetszőleges méretű fájlt választhat.
   > 
   > 
4. Keresse meg a szükséges toohello videót a számítógépen, válassza ki azt, és kattintson az OK gombra.  
   
    hello feltöltés elindul, és megtekintheti a hello fájlnév hello folyamatban.  

Hello feltöltése után látni fogja a hello új objektum bekerül hello **eszközök** ablak. 

## <a name="next-steps"></a>Következő lépések
Most már kódolhatja a feltöltött adategységeket. További információ: [Encode Assets](media-services-portal-encode.md) (Adategységek kódolása).

Használhatja az Azure Functions tootrigger egy kódolási feladat, a konfigurált hello tároló érkező fájl alapján. További információkért tekintse meg [ezt a mintát](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

