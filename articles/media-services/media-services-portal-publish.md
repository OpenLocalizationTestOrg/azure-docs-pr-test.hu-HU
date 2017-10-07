---
title: "AAA\"hello Azure-portálon a tartalom közzététele |} Microsoft dokumentumok\""
description: "Ez az oktatóanyag végigvezeti hello közzétételi hello Azure-portálon a tartalmat."
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
ms.openlocfilehash: a7a3867a6939b4b9da883176c6cc20c99d6c54e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-content-with-hello-azure-portal"></a>Az Azure-portálon hello tartalom közzététele
> [!div class="op_single_selector"]
> * [Portál](media-services-portal-publish.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [REST](media-services-rest-deliver-streaming-content.md)
> 
> 

## <a name="overview"></a>Áttekintés
> [!NOTE]
> toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

tooprovide a felhasználó egy URL-cím használt toostream vagy letölteni a tartalmat, akkor először kell túl "közzététele" az eszköz egy kereső létrehozásával. Lokátorok biztosítanak hozzáférést toofiles hello eszköz szerepel. A Media Services két lokátortípust támogat: 

* Streamelési (OnDemandOrigin) lokátorokat, amelyek adaptív streameléshez (például toostream MPEG DASH, HLS vagy Smooth Streaming) használt. toocreate a streamelési lokátorok létrehozásához az eszköz tartalmaznia kell egy .ism-fájlt. 
* Progresszív (SAS-) lokátorokat, amelyek a videó progresszív letöltésen keresztül történő továbbítására használatosak.

A streamelési URL-cím formátuma a következő hello rendelkezik, és használata tooplay Smooth Streaming-objektumok.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

toobuild HLS-streamelési URL-cím, egy hozzáfűző (formátum = m3u8-aapl) toohello URL-CÍMÉT.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

egy streamelési URL-cím, MPEG DASH toobuild hozzáfűzése (formátum mpd-idő-csf =) toohello URL-CÍMÉT.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Egy SAS URL-címnek a következő formátumban hello.

    {blob container name}/{asset name}/{file name}/{SAS signature}

További információkért lásd: [tartalom áttekintése kézbesítéséhez](media-services-deliver-content-overview.md).

> [!NOTE]
> Ha korábban használt hello portál toocreate keresők 2015. márciusi, keresők kétéves lejárati dátummal jöttek létre.  
> 
> 

a lokátor, használja a lejárati dátum tooupdate [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) vagy [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-k. Vegye figyelembe, hogy egy SAS-lokátor lejárati dátuma hello frissítésekor módosítja hello URL-CÍMÉT.

### <a name="toouse-hello-portal-toopublish-an-asset"></a>toouse hello portál toopublish egy eszköz
toouse hello portál toopublish egy eszköz hello a következő:

1. A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.
2. Válassza a **Settgings (Beállítások)** > **Assets (Objektumok)** lehetőséget.
3. Válassza ki a megjeleníteni kívánt toopublish hello eszközt.
4. Kattintson a hello **közzététel** gombra.
5. Válassza ki a hello lokátor típusát.
6. Nyomja meg az **Add** (Hozzáadás) gombot.
   
    ![Közzététel](./media/media-services-portal-vod-get-started/media-services-publish1.png)

hello URL-cím hozzáadódik toohello listája **közzétett URL-címek**.

## <a name="play-content-from-hello-portal"></a>Hello portálról tartalom lejátszása
hello Azure-portálon talál egy tartalomlejátszót használható tootest a videót.

Kattintson a kívánt hello videó, majd hello **lejátszása** gombra.

![Közzététel](./media/media-services-portal-vod-get-started/media-services-play.png)

Vegye figyelembe a következőket:

* Ellenőrizze, hogy a videó hello is közzé lett téve.
* Ez **Media player** hello alapértelmezett streamvégpontból játssza le. Ha azt szeretné, hogy a nem alapértelmezett tooplay az adatfolyam-továbbítási végpontra, kattintson a toocopy hello URL-címet, és használjon másik lejátszót. Például az [Azure Media Services lejátszót](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
* adatfolyam-továbbítási végpontra, amelyből streaming hello rendszernek kell futnia.  

## <a name="next-steps"></a>Következő lépések
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

