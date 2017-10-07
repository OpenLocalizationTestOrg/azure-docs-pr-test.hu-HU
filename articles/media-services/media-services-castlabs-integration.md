---
title: aaaUsing castLabs toodeliver Widevine-licencek tooAzure Media Services |} Microsoft Docs
description: "Ez a cikk ismerteti, hogyan használhatja az Azure Media Services (AMS) toodeliver olyan adatfolyamra, amely dinamikusan titkosítja a PlayReady vagy a Widevine DRMs AMS. hello PlayReady licenc Media Services PlayReady licenckiszolgáló származik, és Widevine-licenc castLabs licenckiszolgáló hozta."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 2a9a408a-a995-49e1-8d8f-ac5b51e17d40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: Mingfeiy;willzhan;Juliako
ms.openlocfilehash: 80d2778fb283a96361e7e511990a36c2f551a310
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-castlabs-toodeliver-widevine-licenses-tooazure-media-services"></a>CastLabs toodeliver Widevine-licencek tooAzure Media Services használatával
> [!div class="op_single_selector"]
> * [Axinom](media-services-axinom-integration.md)
> * [castLabs](media-services-castlabs-integration.md)
> 
> 

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan használhatja az Azure Media Services (AMS) toodeliver olyan adatfolyamra, amely dinamikusan titkosítja a PlayReady vagy a Widevine DRMs AMS. hello PlayReady licenc származik a licenckiszolgáló Media Services PlayReady és Widevine-licenc hozta **castLabs** licenckiszolgáló.

adatfolyam-védett tartalom tooplayback CENC (PlayReady és/vagy Widevine), használható [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Lásd: [AMP dokumentum](http://amp.azure.net/libs/amp/latest/docs/) részleteiről.

hello a következő ábra azt mutatja be, a magas szintű Azure Media Services és a castLabs integrációs architektúra.

![Integráció](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

## <a name="typical-system-set-up"></a>Tipikus rendszer beállítása
* A médiatartalom AMS tárolja.
* Kulcs azonosítók tartalom kulcsok castLabs, mind az AMS vannak tárolva.
* castLabs és AMS egyaránt rendelkezik beépített tokent használó hitelesítés. hello a következő szakaszok ismertetik a hitelesítési tokenek. 
* Amikor egy ügyfél toostream hello videó, dinamikusan hello tartalom titkosítása a **Common Encryption** (CENC) és AMS tooSmooth dinamikusan csomagolni, Streaming és kötőjel. Azt is biztosítanak a HLS streamelési protokoll PlayReady M2TS elemi adatfolyam titkosítását.
* PlayReady-licenc AMS licenckiszolgáló kéri le a rendszer, és Widevine-licenc castLabs licenckiszolgáló kéri le a rendszer. 
* Media Player automatikusan eldönti, milyen licenc toofetch hello ügyfél platform képességei alapján. 

## <a name="authentication-token-generation-for-getting-a-license"></a>Hitelesítési jogkivonatok létrehozásához a licenc beolvasásakor
CastLabs, mind az AMS támogatja (JSON Web Token) JWT jogkivonat formátumától tooauthorize egy licencet. 

### <a name="jwt-token-in-ams"></a>Az AMS JWT jogkivonat
hello a következő táblázat ismerteti az AMS JWT jogkivonat. 

| Kiállító | A kiválasztott kibocsátó karakterláncnak a következőről hello Secure Token Service (STS) |
| --- | --- |
| Célközönség |A célközönség-karakterláncnak a következőről hello használt STS |
| Jogcímek |Jogcímek egy készletét |
| NotBefore |Hello token érvényességének kezdő |
| Lejár |Hello token érvényességének vége |
| SigningCredentials |hello kulcs, amelyet használ PlayReady licenckiszolgáló, castLabs licenckiszolgáló és STS, akkor lehet, hogy szimmetrikus vagy aszimmetrikus kulcs. |

### <a name="jwt-token-in-castlabs"></a>A castLabs JWT jogkivonat
a következő táblázat hello castLabs a JWT jogkivonat ismerteti. 

| Név | Leírás |
| --- | --- |
| optData |Egy adatokat tartalmazó JSON-karakterlánc. |
| CRT |A JSON karakterlánc kapcsolatos információkat tartalmazó hello eszköz, a licencelési adatokat, és a lejátszás jogok. |
| IAT |hello aktuális datetime epoch. |
| jti |Ez a token (összes jogkivonat csak egyszer használható hello castLabs rendszerben) kapcsolatos egyedi azonosítója. |

## <a name="sample-solution-set-up"></a>A minta megoldás beállítása
Hello [megoldás minta](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) két projektet tartalmaz:

* Egy konzolalkalmazást, amely a PlayReady és Widevine már feldolgozott eszközként használt tooset DRM korlátozásait.
* A webes alkalmazás, amely átadja a jogkivonatokat, ami az STS szolgáltatással nagyon egyszerűsített verziója sikerült tekinthető meg.

toouse hello konzolalkalmazást:

1. Hello app.config toosetup AMS hitelesítő adatokat, a castLabs hitelesítő adatokat, a STS konfigurációs és a megosztott kulcs módosítása.
2. Töltse fel az eszköz az AMS.
3. Get hello UUID hello a feltöltött eszköz, és módosítsa a sor 32 hello Program.cs fájlban:
   
      var objIAsset = _context. Assets.Where (x = > x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf"). FirstOrDefault();
4. Használjon egy AssetId hello eszköz elnevezési hello castLabs rendszerben (sor 44 hello Program.cs fájlban).
   
   Meg kell adni a AssetId **castLabs**; toobe egyedi alfanumerikus karakterlánc szükséges.
5. Hello program futtatása.

toouse hello webes alkalmazás (STS):

1. Változás hello web.config toosetup castlabs kereskedelmi Azonosítót, hello STS konfigurációs és hello megosztott kulcsot.
2. Webhelyek tooAzure telepítése.
3. Keresse meg a toohello webhelyet.

## <a name="playing-back-a-video"></a>Vissza a videó lejátszása
tooplayback common encryption (PlayReady és/vagy Widevine) titkosítva videó, használhatja a hello [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Hello Konzolalkalmazás futtatásakor annál hello Tartalomazonosítót kulcs és hello Manifest URL-CÍMÉT.

1. Nyisson meg egy új lapot, és indítsa el a STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2. Nyissa meg túl[Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3. Illessze be a streamelési URL-cím hello.
4. Kattintson a hello **speciális beállítások** jelölőnégyzetet.
5. A hello **védelmi** legördülő menüben válassza a PlayReady és/vagy Widevine.
6. Illessze be a portáltól kapott az STS hello Token szövegmezőjének hello jogkivonat. 
   
   hello castLab licenckiszolgáló nem kell hello "tulajdonosi =" hello token elé előtag. Ezért távolítsa el, amely hello token elküldése előtt.
7. Hello player frissítése.
8. videó hello kell játszott.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

