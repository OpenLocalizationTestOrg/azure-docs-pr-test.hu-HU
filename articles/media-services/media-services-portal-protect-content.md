---
title: "aaaConfiguring tartalomvédelem házirendekkel hello Azure portálon |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toouse hello Azure portál tooconfigure tartalomvédelem házirendek. hello a következő cikket is bemutatja hogyan tooenable a dinamikus titkosítás az eszközök."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: 3e7ce6ddaa0e738b5a1e26dafe9eef2df221f039
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-content-protection-policies-using-hello-azure-portal"></a>Tartalomvédelem házirendekkel hello Azure-portál konfigurálása
> [!NOTE]
> toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
> 
> 

## <a name="overview"></a>Áttekintés
Microsoft Azure Media Services (AMS) lehetővé teszi, hogy Ön toosecure hello idő keresztül tárhely, feldolgozás és kézbesítési elhagyják a adathordozókról. A Media Services toodeliver lehetővé teszi a tartalom titkosított dinamikusan az Advanced Encryption Standard (AES) (a 128 bites titkosítási kulcsok használatával), közös titkosítás (CENC) segítségével PlayReady és/vagy a Widevine DRM-Védelemmel, és az Apple FairPlay. 

AMS DRM-licencek kézbesítéséhez szolgáltatást nyújt, és AES kulcsok tooauthorized ügyfelek törölje. hello Azure-portál lehetővé teszi egy toocreate **kulcs/licenc engedélyezési házirend** minden típusú titkosítások használatára.

Ez a cikk bemutatja, hogyan tooconfigure content protection házirendeket és hello Azure-portálon. hello a következő cikket is bemutatja hogyan tooapply a dinamikus titkosítás tooyour eszközök.


> [!NOTE]
> Ha hello Azure klasszikus portál toocreate adatvédelmi szabályzatok, hello házirendek nem szerepelhet hello [Azure-portálon](https://portal.azure.com/). Azonban az összes régi hello házirendek továbbra is létezik. Tekintse meg őket használatával hello Azure Media Services .NET SDK vagy hello [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) eszköz (toosee hello házirendek, kattintson a jobb gombbal a hello eszköz -> információkat (F4) -> kattintson a tartalomkulcs -> a lap megjelenítése Kattintson a hello kulcs). 
> 
> Ha tooencrypt az eszköz új szabályzatokkal, konfigurálja őket a hello Azure-portálon, kattintson a Mentés gombra, és alkalmazza újra a dinamikus titkosítás. 
> 
> 

## <a name="start-configuring-content-protection"></a>Indítsa el a tartalom védelmének beállítása
toouse hello portál toostart konfigurálása tartalomvédelem, a globális tooyour AMS-fiók, hello a következő:

1. A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.
2. Válassza ki **beállítások** > **védelmi tartalom**.

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Kulcs/licenc engedélyezési házirend
AMS több módon azok a felhasználók, akik vagy licencelési kérelmeket támogatja. hello tartalomkulcs-hitelesítési szabályzatot kell Ön által konfigurált és érheti el, ha az ügyfél ahhoz, hogy hello kulcs/licenc toobe delived toohello ügyfél. hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: **nyissa meg a** vagy **token** korlátozás.

hello Azure-portál lehetővé teszi egy toocreate **kulcs/licenc engedélyezési házirend** minden típusú titkosítások használatára.

### <a name="open"></a>Nyílt
Nyissa meg a szoftverkorlátozó azt jelenti, hogy hello rendszer hello kulcs tooanyone kulcs kérést fog továbbítani. Ez a korlátozás tesztelési célokra hasznos lehet. 

### <a name="token"></a>Token
hello token korlátozott házirend által a Secure Token Service (STS) kiadott tokennek kell csatolni. A Media Services hello Simple Web Tokens (SWT) és JSON webes jogkivonat (JWT) formátumú tokeneket támogatja. A Media Services nem nyújt Secure Token szolgáltatásokat. Hozzon létre egy egyéni STS, vagy használja a Microsoft Azure ACS tooissue jogkivonatokat. hello STS kell lennie a megadott hello aláírt jogkivonat konfigurált toocreate hello token korlátozás a konfigurációban megadott kulcs és a probléma jogcímeket. kulcs kézbesítési szolgáltatás visszaadható hello (vagy licencelési) toohello ügyfél Ha hello jogkivonat érvényes, és hello kért hello Media Services-jogcímek az hello token megfelelő azokat konfigurált hello (vagy licencelési).

Hello token korlátozott házirend konfigurálásakor hello elsődleges hitelesítési kulcs, a kibocsátó és a célközönség paramétereket kell megadnia. hello elsődleges hitelesítési kulcsot tartalmazó hello kulcsfontosságú, hogy hello token lett aláírva, illetve kibocsátó hello biztonságos biztonságijogkivonat-szolgáltatás által kiállított hello jogkivonat. hello célközönség (más néven hatókör) hello token hello célját ismerteti, vagy hello erőforrás hello token engedélyezi a hozzáférést. hello Media Services kulcs kézbesítési szolgáltatás ellenőrzi, hogy ezek az értékek hello token értékekre, hello hello sablont.

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>PlayReady-jogosultságsablont
Hello PlayReady jogosultságsablont kapcsolatos részletes információkért lásd: [Media Services PlayReady licenc sablon áttekintése](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Nem állandó
Licenc nem állandó, konfigurálása, ha azt csak használatban van memória közben hello player hello licencet használ.  

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Állandó
Ha a konfigurált hello licenc állandó, a Mentés az állandó tároló a hello ügyfél.

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Widevine-jogosultságsablont
Hello Widevine jogosultságsablont kapcsolatos részletes információkért lásd: [Widevine-licenc sablon áttekintése](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Basic
Ha bejelöli **alapvető**, hello sablon jön létre minden alapértelmezett értéket.

### <a name="advanced"></a>Extra szintű
Widevine-konfigurációk előzetes lehetőségekről részletes ismertetése [ez](media-services-widevine-license-template-overview.md) témakör.

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay-konfiguráció
tooenable FairPlay titkosításhoz, és van szükség tooprovide hello App tanúsítvány alkalmazás titkos kulcs (KÉRJEN) hello FairPlay konfigurációs beállítás használatával. FairPlay konfigurációs és követelmények kapcsolatos részletes információkért lásd: [ez](media-services-protect-hls-with-fairplay.md) cikk.

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-tooyour-asset"></a>A dinamikus titkosítás tooyour eszköz alkalmazása
tootake előnye a dinamikus titkosítás kell tooencode a forrásfájlt adaptív sávszélességű MP4-fájlokat alakítja.

### <a name="select-an-asset-that-you-want-tooencrypt"></a>Válassza ki egy eszközt, amelyet az tooencrypt
toosee az eszközök, válasszon **beállítások** > **eszközök**.

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Az AES vagy DRM titkosítása
Ha lenyomja az **titkosítása** egy eszköz, lehetősége lesz két választási lehetőség: **AES** vagy **DRM**. 

#### <a name="aes"></a>AES
Törölje a jelet titkosítás engedélyezve lesz az összes adatfolyam-továbbítási protokollok AES: Smooth Streaming, HLS vagy MPEG-DASH.

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
Amikor hello DRM lapon válassza ki, lehetősége lesz a tartalomvédelem házirendek más lehetőségek (amely konfigurálnia kell mostanra) + protokollok streameléshez készlete.

* **PlayReady és Widevine rendelkező MPEG-DASH** -rendszer dinamikus titkosítást a PlayReady és Widevine DRMs MPEG-DASH adatfolyam.
* **PlayReady és MPEG-DASH végzett Widevine + a HLS FairPlay** -dinamikusan titkosítja, MPEG-DASH-adatfolyam a PlayReady vagy Widevine DRMs. A HLS-adatfolyamok FairPlay is titkosítja.
* **Csak a Smooth Streaming, HLS, és MPEG-DASH PlayReady** -dinamikusan titkosítani fog Smooth Streaming, HLS, MPEG-DASH-adatfolyamok PlayReady DRM.
* **Csak a MPEG-DASH Widevine** -dinamikusan titkosítani, MPEG-DASH a Widevine DRM-Védelemmel.
* **Csak a HLS FairPlay** -dinamikusan titkosítja a HLS adatfolyam FairPlay.

tooenable FairPlay titkosításhoz, és van szükség tooprovide hello App tanúsítvány alkalmazás titkos kulcs (KÉRJEN) keresztül hello FairPlay konfigurációs beállítás hello tartalomvédelem beállítások panelről.

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Ha hello titkosítás kiválasztása, nyomja meg az **alkalmaz**.

>[!NOTE] 
>Ha azt tervezi, tooplay az AES titkosított HLS a Safari című [ebben a blogban](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Következő lépések
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

