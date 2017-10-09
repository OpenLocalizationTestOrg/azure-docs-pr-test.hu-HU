---
title: "Azure-portálon aaaConfigure Tartalomkulcs hitelesítési szabályzatának használatával hello |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconfigure a tartalomkulcs az engedélyezési házirend."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 157fb691b7f71f4889228817e1dc64555e327d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-content-key-authorization-policy"></a>Konfigurálja a Tartalomkulcs-hitelesítési házirend
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Áttekintés
A Microsoft Azure Media Services lehetővé teszi a toodeliver MPEG-DASH, Smooth Streaming vagy HTTP-Live-Streaming (HLS) streamjeit az Advanced Encryption Standard (AES) (a 128 bites titkosítási kulcsok használatával) vagy [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS azt is lehetővé teszi, hogy a akkor toodeliver DASH-streameket Widevine DRM titkosítva. Mind a PlayReady, mind a Widevine titkosítása hello Common Encryption (ISO/IEC 23001-7 CENC) megadását.

A Media Services is biztosít a **kulcs/Licenctovábbítási szolgáltatása** , amelyekről az ügyfelek AES-kulcsok úgy szerezheti be, vagy a PlayReady vagy Widevine-licencek tooplay hello titkosított tartalmat.

Ez a témakör bemutatja, hogyan toouse hello Azure portál tooconfigure hello tartalomkulcs-hitelesítési szabályzatot. hello kulcs később használható toodynamically a tartalmak titkosításához. Vegye figyelembe, hogy jelenleg titkosíthatja hello a következő formátumban: HLS, MPEG DASH vagy Smooth Streaming. Progresszív letöltés nem titkosítható.

Amikor egy player olyan adatfolyamra, amely dinamikusan titkosított toobe van beállítva, a Media Services által használt konfigurált hello kulcs toodynamically titkosítása a tartalmat, AES vagy DRM titkosítással történik. toodecrypt hello stream, hello player fog igényelni hello kulcs hello kulcs kézbesítési szolgáltatásból. hello felhasználónak van-e toodecide engedélyezett tooget hello kulcs, hello szolgáltatás értékeli az hello kulcshoz megadott hello engedélyezési házirendeket.

Ha a terv toohave több tartalomkulcs, és szeretné, hogy toospecify egy **kulcs/Licenctovábbítási szolgáltatása** eltérő hello Media Services kulcs kézbesítési szolgáltatás URL-CÍMÉT használja a Media Services .NET SDK-t vagy a REST API-k.

[Media Services .NET SDK használatával Tartalomkulcs hitelesítési szabályzatának konfigurálása](media-services-dotnet-configure-content-key-auth-policy.md)

[Media Services REST API használatával Tartalomkulcs hitelesítési szabályzatának konfigurálása](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Vegye figyelembe a következőket:
* Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás, az adatfolyam-továbbítási végpontjának tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart toobe rendelkezik hello **futtató** állapotát. 
* Az eszköz tartalmaznia kell egy adaptív sávszélességű MP4 vagy Smooth Streaming-fájlsorozattá készletét. További információkért lásd: [kódolása egy eszköz](media-services-encode-asset.md).
* hello kulcs kézbesítési szolgáltatás 15 percig gyorsítótárazza a ContentKeyAuthorizationPolicy és a kapcsolódó objektumok (házirend-beállítások és korlátozásai).  Ha hozzon létre egy ContentKeyAuthorizationPolicy és toouse "Token" korlátozás, adja meg, majd tesztelje, és frissítse a hello házirend túl "nyissa meg" korlátozás, mielőtt hello kapcsolók toohello "Megnyitás" házirendverzió hello házirend nagyjából 15 percig tart.

## <a name="how-to-configure-hello-key-authorization-policy"></a>Hogyan: hello hitelesítési házirend konfigurálása
tooconfigure hello hitelesítési szabályzatot, jelölje be hello **TARTALOMVÉDELEM** lap.

A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat. hello tartalomkulcs-hitelesítési házirend rendelkezhet **nyissa meg a**, **token**, vagy **IP** engedélyezési korlátozások (**IP** konfigurálható REST- vagy .NET SDK-val).

### <a name="open-restriction"></a>Nyissa meg a szoftverkorlátozó
Hello **nyissa meg a** korlátozás azt jelenti, hogy hello rendszer hello kulcs tooanyone kulcs kérést fog továbbítani. Ez a korlátozás tesztelési célokra hasznos lehet.

![OpenPolicy][open_policy]

### <a name="token-restriction"></a>Token korlátozása
toochoose hello token korlátozott házirend, nyomja meg az hello **TOKEN** gombra.

Hello **token** korlátozott házirend által kiadott tokennek kell fűzni a **Secure Token Service** (STS). A Media Services hello támogatja a jogkivonatokat **Simple Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) formátumú és **JSON Web Token** (JWT) formátumú. További információ: [JWT jogkivonat hitelesítési](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Nem biztosít a Media Services **Token szolgáltatások biztonságos**. Hozzon létre egy egyéni STS, vagy használja a Microsoft Azure ACS tooissue jogkivonatokat. hello STS kell lennie a megadott hello aláírt jogkivonat konfigurált toocreate hello token korlátozás a konfigurációban megadott kulcs és a probléma jogcímeket. hello Media Services kulcs kézbesítési szolgáltatás visszaadható hello titkosítási kulcs toohello ügyfél Ha hello jogkivonat érvényes, és hello hello jogkivonatában lévő jogcímeket egyeznek hello tartalomkulcsot konfigurálva. További információkért lásd: [használata Azure ACS tooissue jogkivonatok](http://mingfeiy.com/acs-with-key-services).

Hello konfigurálásakor **TOKEN** korlátozott házirend, meg kell adni az értékeket **ellenőrző kulcs**, **kibocsátó** és **célközönség**. hello elsődleges hitelesítési kulcsot tartalmazó hello kulcsfontosságú, hogy hello token lett aláírva, illetve kibocsátó hello biztonságos biztonságijogkivonat-szolgáltatás által kiállított hello jogkivonat. hello célközönség (más néven hatókör) hello token hello célját ismerteti, vagy hello erőforrás hello token engedélyezi a hozzáférést. hello Media Services kulcs kézbesítési szolgáltatás ellenőrzi, hogy ezek az értékek hello token értékekre, hello hello sablont.

### <a name="playready"></a>PlayReady
Ha az a tartalmak védelmére **PlayReady**, egy hello dolog van szüksége az engedélyezési házirendben toospecify hello PlayReady licencsablon definiáló XML-karakterlánc. Alapértelmezés szerint a következő házirend hello van beállítva:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1"><LicenseTemplates> <PlayReadyLicenseTemplate> <AllowTestDevices>igaz</AllowTestDevices> <ContentKey i:type="ContentEncryptionKeyFromHeader" /> <LicenseType>Nonpersistent</LicenseType> <PlayRight> <AllowPassingVideoContentToUnknownOutput>engedélyezett</AllowPassingVideoContentToUnknownOutput> </PlayRight> </PlayReadyLicenseTemplate> </LicenseTemplates></PlayReadyLicenseResponseTemplate>

Kattinthat a hello **importálni a házirend XML-** gombra, és adjon meg egy másik-XML fájlok, amelyek megfelel a megadott XML-séma toohello [Itt](media-services-playready-license-template-overview.md).

## <a name="next-step"></a>Következő lépés
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

