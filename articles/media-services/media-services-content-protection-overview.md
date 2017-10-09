---
title: aaaProtect a tartalmat az Azure Media Services |} Microsoft Docs
description: "Ez a cikk áttekintést a Media Services tartalomvédelem."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: abab7602d71d7357a692976420ca9a988c0d096f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-content-overview"></a>Védett tartalom áttekintése
Microsoft Azure Media Services lehetővé teszi, hogy Ön toosecure hello idő keresztül tárhely, feldolgozás és kézbesítési elhagyják a adathordozókról. A Media Services lehetővé teszi az élő és az igény a tartalom titkosított dinamikusan toodeliver Advanced Encryption Standard (AES) (a 128 bites titkosítási kulcsok használatával) vagy annak bármelyik hello fő DRMs: Microsoft PlayReady, Google Widevine és Apple FairPlay. A Media Services is biztosít egy szolgáltatás, amelynek segítségével a AES-kulcsok, és (PlayReady, Widevine és FairPlay) DRM-licencek tooauthorized ügyfelek. 

a következő kép hello hello tartalomvédelem munkafolyamatok AMS támogató mutatja be. 

![Védelem biztosítása a PlayReadyvel](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát. 

Ez a témakör azt ismerteti, [fogalmakat és terminológiát](media-services-content-protection-overview.md) vonatkozó toounderstanding tartalomvédelem az AMS. hello témakör is tartalmaz [hivatkozások](media-services-content-protection-overview.md#common-scenarios) tootopics, amelyek megjelenítik a hogyan tooachieve tartalom védelmi feladatokat. 

## <a name="dynamic-encryption"></a>A dinamikus titkosítás
A Microsoft Azure Media Services lehetővé teszi a tartalom titkosított dinamikusan AES-kulcs törlése vagy DRM titkosítási toodeliver: Microsoft PlayReady, Google Widevine és Apple FairPlay.

Jelenleg a következő formátumban hello titkosíthatók: HLS, MPEG DASH vagy Smooth Streaming. Progresszív letöltés nem titkosítható.

Ha azt szeretné, a Media Services tooencrypt egy eszköz tooassociate egy titkosítási kulcsot (CommonEncryption vagy EnvelopeEncryption) kell az eszközt, és engedélyezési házirendeket hello kulcs is konfigurálhatja.

Szükség tooconfigure hello adategység továbbítási házirendjét. Ha azt szeretné, hogy a tároló titkosított eszköz toostream, győződjön meg arról, hogy hogyan szeretné toospecify toodeliver, úgy konfigurálja az adategység továbbítási házirendjét.

Ha olyan adatfolyamot kell megadni a Windows Media Player van szükség, Media Services használ-e a megadott hello kulcs toodynamically a tartalom törlése kulcs AES segítségével vagy a DRM titkosítási titkosítására. toodecrypt hello stream, hello player fog igényelni hello kulcs hello kulcs kézbesítési szolgáltatásból. hello felhasználónak van-e toodecide engedélyezett tooget hello kulcs, hello szolgáltatás értékeli az hello kulcshoz megadott hello engedélyezési házirendeket.


## <a name="storage-encryption"></a>Tárolás titkosítása
Tárolás titkosítási tooencrypt használja a tiszta tartalom helyileg az AES 256 bites titkosítás használata, majd töltse fel azt tooAzure helyén tárolás titkosítása. Storage-titkosítással védett adategységek automatikusan titkosítás és egy titkosított fájl rendszer előzetes tooencoding, és ha szükséges újra titkosítani előzetes toouploading egy új kimeneti eszközként helyezve. hello elsődleges használati eset a tárolás titkosítása akkor, ha azt szeretné, hogy a jó minőségű bemeneti médiafájljait erős titkosítással, rest-lemezen toosecure.

A sorrend toodeliver tárolási titkosított eszköz konfigurálnia kell hello adategység továbbítási házirendjét, a Media Services tudja, hogyan szeretné toodeliver a tartalom. Mielőtt az eszköz továbbítható, hello adatfolyam-kiszolgáló eltávolítása hello tárolás titkosítása és adatfolyamokat a tartalom hello segítségével megadott továbbítási házirendjét (például AES, általános titkosítás vagy titkosítás nélkül).

## <a name="common-encryption-cenc"></a>Common encryption (CENC)
Közös titkosítás titkosításához a tartalmaknak a PlayReady vagy / és Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Cbcs-aapl titkosítással
Cbcs-aapl FairPlay a tartalom titkosításához használatos.

## <a name="envelope-encryption"></a>Boríték titkosítás
Használja ezt a beállítást, ha azt szeretné, tooprotect a tartalom AES-128 tiszta kulcs használatával. Ha azt szeretné, hogy egy biztonságosabb beállítás, válassza ki, ebben a témakörben felsorolt hello DRMs egyikének. 

## <a name="licenses-and-keys-delivery-service"></a>Licencek és a kulcsok kézbesítési szolgáltatás
A Media Services (PlayReady, Widevine, FairPlay) DRM-licencek kézbesítéséhez szolgáltatást nyújt, és AES kulcsok tooauthorized ügyfelek törölje. Használhat [hello Azure-portálon](media-services-portal-protect-content.md), REST API-t vagy a Media Services SDK for .NET tooconfigure hitelesítési és engedélyezési házirendeket a licencekkel és a kulcsok.

## <a name="token-restriction"></a>Token korlátozása
hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: Nyissa meg, vagy a token korlátozás. hello token korlátozott házirend által a Secure Token Service (STS) kiadott tokennek kell csatolni. A Media Services hello Simple Web Tokens (SWT) és JSON webes jogkivonat (JWT) formátumú tokeneket támogatja. A Media Services nem nyújt Secure Token szolgáltatásokat. Hozzon létre egy egyéni STS, vagy használja a Microsoft Azure ACS tooissue jogkivonatokat. hello STS kell lennie a megadott hello aláírt jogkivonat konfigurált toocreate hello token korlátozás a konfigurációban megadott kulcs és a probléma jogcímeket. kulcs kézbesítési szolgáltatás visszaadható hello (vagy licencelési) toohello ügyfél Ha hello jogkivonat érvényes, és hello kért hello Media Services-jogcímek az hello token megfelelő azokat konfigurált hello (vagy licencelési).

Hello token korlátozott házirend konfigurálásakor hello elsődleges hitelesítési kulcs, a kibocsátó és a célközönség paramétereket kell megadnia. hello elsődleges hitelesítési kulcsot tartalmazó hello kulcsfontosságú, hogy hello token lett aláírva, illetve kibocsátó hello biztonságos biztonságijogkivonat-szolgáltatás által kiállított hello jogkivonat. hello célközönség (más néven hatókör) hello token hello célját ismerteti, vagy hello erőforrás hello token engedélyezi a hozzáférést. hello Media Services kulcs kézbesítési szolgáltatás ellenőrzi, hogy ezek az értékek hello token értékekre, hello hello sablont.

## <a name="streaming-urls"></a>Adatfolyam-továbbítási URL-címek
Ha az objektum egynél több DRM lett titkosítva, a streamelési URL-cím hello egy titkosítási címke használja: (formátum = "m3u8-aapl" titkosítási = "xxx").

a következő szempontok hello vonatkoznak:

* Csak nulla vagy egy titkosítási típus adható meg.
* Titkosítási típus toobe hello URL-címben megadott, ha csak egy titkosítási lett alkalmazott toohello eszköz nem rendelkezik.
* Titkosítási típus-és nagybetűket.
* a következő típusú titkosítás hello adható meg:  
  * **cenc**: Common encryption (Playready vagy Widevine)
  * **cbcs-aapl**: Fairplay
  * **CBC**: AES envelope titkosítást.

## <a name="common-scenarios"></a>Gyakori forgatókönyvek
hello következő témakörök bemutatják, hogyan tooprotect tartalmat tároló, a fájlmegosztásba dinamikusan titkosított adatfolyam, használja az AMS kulcs/licenctovábbítási szolgáltatásra

* [AES védelme](media-services-protect-with-aes128.md) 
* [PlayReady és/vagy Widevine védelme](media-services-protect-with-drm.md)
* [A HLS tartalom védett Apple FairPlay és/vagy a PlayReady-adatfolyam](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>További helyzetek
* [Hogyan toointegrate Azure PlayReady licenc saját titkosító/streamelési kiszolgáló a szolgáltatás](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
* [CastLabs toodeliver DRM-licencek tooAzure Media Services használatával](media-services-castlabs-integration.md)

>[!NOTE]
>A forgatókönyv külső DRM server(technology) és AMS kapott használatához jelenleg nem támogatott.


## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Kapcsolódó hivatkozások
[Szolgáltatás-és AES az Azure Media Services dinamikus titkosítást a PlayReady bejelentése](http://mingfeiy.com/playready)

[Az Azure Media Services PlayReady licenc kézbesítési árképzési ismertetése](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Hogyan toodebug az AES titkosított Azure Media Services-adatfolyam](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT jogkivonat authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Azure Media Services OWIN MVC-alapú alkalmazások integrálása az Azure Active Directoryban, és korlátozhatja a JWT jogcímei alapján kulcs tartalomkézbesítési](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Használjon Azure ACS tooissue tokeneket](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
