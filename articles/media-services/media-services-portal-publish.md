---
title: "  Az Azure portálon tartalom közzététele |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti a közzétételi és az Azure portál tartalmaihoz."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 92c364eb-5a5f-4f4e-8816-b162c031bb40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 68a2fbdda0996cf4ba5ea3b09816bf845af756f4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="publish-content-with-the-azure-portal"></a>Az Azure portálon tartalom közzététele
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-publish.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [REST](media-services-rest-deliver-streaming-content.md)
> 
> 

## <a name="overview"></a>Áttekintés
> [!NOTE]
> Az oktatóanyag elvégzéséhez egy Azure-fiókra lesz szüksége. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Ahhoz, hogy átadhassa a tartalmak streamelésére vagy letöltésére használható URL-címet a felhasználónak, először „közzé kell tennie” az objektumot. Ehhez létre kell hoznia egy lokátort. Az objektumban található fájlokhoz a lokátorok biztosítanak hozzáférést. A Media Services két lokátortípust támogat: 

* Streamelési (OnDemandOrigin) lokátorokat, amelyek adaptív streameléshez (például MPEG DASH, HLS vagy Smooth Streaming adatok streameléséhez) használhatók. A streamelési lokátorok létrehozásához az objektumnak tartalmaznia kell egy .ism-fájlt. 
* Progresszív (SAS-) lokátorokat, amelyek a videó progresszív letöltésen keresztül történő továbbítására használatosak.

A streamelési URL-cím formátuma alább látható. Ez Smooth Streaming-objektumok lejátszására használható.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS-streamelési URL-cím létrehozásához fűzze hozzá a következőt az URL-címhez: (format=m3u8-aapl).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH-streamelési URL-cím létrehozásához fűzze hozzá a következőt az URL-címhez: (format=mpd-time-csf).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Az SAS URL-cím formátuma a következő:

    {blob container name}/{asset name}/{file name}/{SAS signature}

További információkért lásd: [tartalom áttekintése kézbesítéséhez](media-services-deliver-content-overview.md).

> [!NOTE]
> 2015 márciusa előtt a portál kétéves lejárati idővel hozta létre a lokátorokat.  
> 
> 

A lokátor lejárati idejének módosításához használjon [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) vagy [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-t. Ne feledje, hogy a SAS-lokátor lejárati idejének módosításával az URL-cím is megváltozik.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Az objektum portál segítségével történő közzététele
Az objektumnak a portál segítségével történő közzétételéhez tegye a következőket:

1. Az [Azure-portálon](https://portal.azure.com/) válassza ki Azure Media Services-fiókját.
2. Válassza a **Settgings (Beállítások)** > **Assets (Objektumok)** lehetőséget.
3. Válassza ki a közzétenni kívánt objektumot.
4. Kattintson a **Publish** (Közzététel) gombra.
5. Válassza ki a lokátor típusát.
6. Nyomja meg az **Add** (Hozzáadás) gombot.
   
    ![Közzététel](./media/media-services-portal-vod-get-started/media-services-publish1.png)

A rendszer hozzáadja az URL-címet a **Published URLs** (Közzétett URL-címek) listához.

## <a name="play-content-from-the-portal"></a>Tartalom lejátszása a portálról
Az Azure Portalon talál egy tartalomlejátszót, amellyel tesztelheti a videót.

Kattintson a kívánt videóra, majd a **Lejátszás** gombra.

![Közzététel](./media/media-services-portal-vod-get-started/media-services-play.png)

Vegye figyelembe a következőket:

* Ellenőrizze, hogy közzétette-e a videót.
* A **Media Player** az alapértelmezett streamvégpontból játssza le a fájlokat. Ha egy nem alapértelmezett streamvégpontból szeretne lejátszani valamit, rákattintva másolja az URL-címet, és használjon másik lejátszót. Például az [Azure Media Services lejátszót](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
* A streamvégpontján, amelyről streaming rendszernek kell futnia.  

## <a name="next-steps"></a>Következő lépések
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

