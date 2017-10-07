---
title: "aaaDevelop videólejátszó alkalmazások"
description: "hello témakör hivatkozásokat tartalmaz tooPlayer keretrendszerek és beépülő modulok használható toodevelop a saját ügyfélalkalmazásait a Media Services médiafolyamot médiafolyamainak fogadására."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 55e419fc-4c39-4902-9c62-f41cfcd86c6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: juliako
ms.openlocfilehash: a66daa4f006a1f05271cc9ed6a02ea7ee460321d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-video-player-applications"></a>Videolejátszó alkalmazások készítése
## <a name="overview"></a>Áttekintés
Azure Media Services biztosította hello eszközök segítségével kell toocreate gazdag, dinamikus ügyféloldali lejátszóalkalmazások a legtöbb platformra, többek között: iOS-eszközök, Android-eszközök, Windows, Windows Phone, Xbox és dekóder jelölőnégyzetéből. Ez a témakör a hivatkozások tooSDKs és lejátszó-Keretrendszerekhez, amelyeket felhasználhat toodevelop médiafolyamot az Azure Media Services médiafolyamainak fogadására a saját ügyfélalkalmazásait is biztosít.

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát. 
 
## <a name="azure-media-player"></a>Azure Media Player
[Az Azure Media Player](http://aka.ms/ampinfo) egy webes videólejátszó épül tooplay hátsó multimédiás tartalom a Microsoft Azure Media Services a böngészők és az eszközök széles választékát. Az Azure Media Player használja az ipari szabványok, például a HTML5-ös, a Media forrás Extensions (MSE) és a titkosított adathordozó-bővítmények (EME) tooprovide bővített adaptív adatfolyam-továbbítási élményt. Ha ezeknek a szabványoknak nem érhetők el, az eszközön, vagy a böngészőben, Azure Media Player Flash és használja a Silverlight tartalék technológia. Hello hanglejátszáshoz használt technológia, függetlenül a fejlesztők egy egységes JavaScript-felület tooaccess API-k fog rendelkezni. Ez lehetővé teszi az Azure Media Services toobe sok wide-eszközök és bármely extra erőfeszítést böngészőkben lejátszott által kiszolgált tartalmat.

Microsoft Azure Media Services lehetővé teszi DASH, Smooth Streaming, a kiszolgált tartalom toobe és HLS-streamelési formátumok tooplay biztonsági tartalom. Azure Media Player figyelembe kell venni a különböző formátumokba, és automatikusan betöltött szerepe hello hello platform/böngésző képességei alapján ajánlott hivatkozásra. A Microsoft Azure Media Services is lehetővé teszi az eszközök dinamikus titkosítást a PlayReady-titkosítás vagy AES-128 bites titkosítás boríték. Az Azure Media Player lehetővé teszi, hogy PlayReady visszafejtése, és az AES-128 bites titkosított tartalom abban az esetben, ha megfelelően konfigurálva. 

További információ:

* [Azure Media Player](http://aka.ms/ampinfo)
* [Az Azure Media Player dokumentáció](http://aka.ms/ampdocs) 
* [Az Azure Media Player első lépések Blog](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
* [Iratkozzon fel mentése az Azure Media Player a legújabb hello toodate toostay](http://aka.ms/ampsignup)
* [Adja hozzá az új funkciókra vonatkozó kérések, ötleteket, visszajelzés](http://aka.ms/ampuservoice) 

## <a name="other-tools-for-creating-player-applications"></a>Player alkalmazások létrehozására vonatkozó egyéb eszközök
A következő SDK-k hello bármelyikét is használhatja:

* [Adatfolyam-továbbítási ügyfél SDK sima](http://www.iis.net/downloads/microsoft/smooth-streaming) 
* [Zökkenőmentes adatfolyam Windows Áruházbeli alkalmazás](media-services-build-smooth-streaming-apps.md)
* [Microsoft Media Platform: Player keretrendszer](http://playerframework.codeplex.com/) 
* [HTML5 Player keretrendszer dokumentáció](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
* [Microsoft Smooth Streaming beépülő modul a OSMF](https://www.microsoft.com/download/details.aspx?id=36057) 
* [Licencelési Microsoft® zökkenőmentes adatfolyam ügyfél Kit eljárás](http://aka.ms/sspk) 
* [XBOX videó alkalmazásfejlesztés](http://xbox.create.msdn.com/) 

## <a name="advertising"></a>Hirdetési
Az Azure Media Services támogatást nyújt a Windows Media Platform hello keresztül ad beszúrási: lejátszó-Keretrendszerekhez. Ad-támogatással rendelkező lejátszó-keretrendszerekhez Windows 8, a Silverlight, a Windows Phone 8 és az iOS-eszközök érhetők el. Minden egyes player keretrendszer minta kódot tartalmaz, amely bemutatja, hogyan tooimplement a lejátszóalkalmazás. Nincsenek a media beilleszthet ads három különböző típusú:

Lineáris – teljes keret hirdetések hello fő videó felfüggesztése

Nem lineáris – jelennek meg hello fő videó lejátszása átfedő hirdetések, általában egy embléma vagy más hello player belül elhelyezett statikus kép

Kiegészítő – hello player kívül megjelenített ads

ADs hello fő videó idősorán bármikor helyezhető. Meg kell állapítani, hogy hello player amikor tooplay hello ad, és amely ads tooplay. Ebben az esetben a szabványos XML alapú fájlok készletből: videó Ad szolgáltatás sablon (VAST), a digitális videót több Ad lista (VMAP), a Media absztrakt alkalmazás-előkészítés sablon (OSZLOPOS) és a digitális videót Player Ad felület Definition (VPAID). NAGY fájlok adja meg, milyen ads toodisplay. VMAP fájlok adja meg, mikor tooplay különböző hirdetések és HATALMAS XML kódot tartalmaz. A fájlok OSZLOPOS egy másik módja toosequence ads is tartalmazó túlnyomó XML. VPAID fájlok meghatározása hello videólejátszó és hello ad vagy ad-kiszolgáló közötti illesztőfelületet szolgáltasson. További információkért lásd: [beszúrása Ads](https://msdn.microsoft.com/library/dn387398.aspx).

Feliratok és élő adatfolyam-továbbítási videók ads támogatással kapcsolatos információkért lásd: [támogatott kódolt feliratok és Ad beszúrási szabványok](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Lásd még:
[MPEG-DASH adaptív streamelt videók beágyazása DASH.js-sel rendelkező HTML5-alkalmazásba](media-services-embed-mpeg-dash-in-html5.md)

[GitHub-tárházban dash.js](https://github.com/Dash-Industry-Forum/dash.js)

