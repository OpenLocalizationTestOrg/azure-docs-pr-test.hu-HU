---
title: "Az Azure portál használatával tartalomvédelem-szabályzatok konfigurálása |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan használhatja az Azure-portálon tartalomvédelem szabályzatok konfigurálására. A cikk azt is bemutatja, hogyan engedélyezheti az eszközök dinamikus titkosítást."
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
ms.openlocfilehash: 67b3fa9936daebeafb7e87fe3a7b0c7e0105b3b3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-content-protection-policies-using-the-azure-portal"></a><span data-ttu-id="6b362-104">Az Azure portál használatával tartalomvédelem-szabályzatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6b362-104">Configuring content protection policies using the Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="6b362-105">Az oktatóanyag elvégzéséhez egy Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="6b362-105">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="6b362-106">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6b362-106">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="6b362-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6b362-107">Overview</span></span>
<span data-ttu-id="6b362-108">A Microsoft Azure Media Services (AMS) lehetővé teszi a media a tárhely, feldolgozás és kézbesítési keresztül elhagyja óta.</span><span class="sxs-lookup"><span data-stu-id="6b362-108">Microsoft Azure Media Services (AMS) enables you to secure your media from the time it leaves your computer through storage, processing, and delivery.</span></span> <span data-ttu-id="6b362-109">A Media Services lehetővé teszi, hogy a tartalom titkosított dinamikusan az Advanced Encryption Standard (AES) (a 128 bites titkosítási kulcsok használatával), közös titkosítás (CENC) segítségével PlayReady és/vagy a Widevine DRM-Védelemmel, és az Apple FairPlay.</span><span class="sxs-lookup"><span data-stu-id="6b362-109">Media Services allows you to deliver your content encrypted dynamically with Advanced Encryption Standard (AES) (using 128-bit encryption keys), common encryption (CENC) using PlayReady and/or Widevine DRM, and Apple FairPlay.</span></span> 

<span data-ttu-id="6b362-110">AMS DRM-licencek kézbesítéséhez szolgáltatást nyújt, és AES törölje az arra jogosult ügyfelek kulccsal.</span><span class="sxs-lookup"><span data-stu-id="6b362-110">AMS provides a service for delivering DRM licenses and AES clear keys to authorized clients.</span></span> <span data-ttu-id="6b362-111">Az Azure-portálon lehetővé teszi, hogy hozzon létre egyet **kulcs/licenc engedélyezési házirend** minden típusú titkosítások használatára.</span><span class="sxs-lookup"><span data-stu-id="6b362-111">The Azure portal enables you to create one **key/license authorization policy** for all types of encryptions.</span></span>

<span data-ttu-id="6b362-112">Ez a cikk bemutatja, hogyan tartalomvédelem-szabályzatok konfigurálása az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="6b362-112">This article demonstrates how to configure content protection policies with the Azure portal.</span></span> <span data-ttu-id="6b362-113">A cikk azt is bemutatja, hogyan alkalmazni a dinamikus titkosítás.</span><span class="sxs-lookup"><span data-stu-id="6b362-113">The article also shows how to apply dynamic encryption to your assets.</span></span>


> [!NOTE]
> <span data-ttu-id="6b362-114">Ha a klasszikus Azure portálon adatvédelmi szabályzatok létrehozásához használt, a házirendek nem szerepelhet a [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6b362-114">If you used the Azure classic portal to create protection policies, the policies may not appear in the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="6b362-115">Azonban az összes a régi házirendek továbbra is létezik.</span><span class="sxs-lookup"><span data-stu-id="6b362-115">However, all the old polices still exist.</span></span> <span data-ttu-id="6b362-116">Tekintse meg őket az Azure Media Services .NET SDK használatával vagy a [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) eszköz (a szabályzatok megtekintéséhez kattintson a jobb gombbal az eszközre a megjelenített információk (F4) -> -> tartalomkulcs lapon kattintson a -> kattintson a kulcs).</span><span class="sxs-lookup"><span data-stu-id="6b362-116">You can examine them using the Azure Media Services .NET SDK or the [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) tool (to see the policies, right-click on the asset -> Display information (F4)->click on Content keys tab-> click on the key).</span></span> 
> 
> <span data-ttu-id="6b362-117">Ha szeretné titkosítani az eszköz új szabályzatokkal, konfigurálja őket az Azure portálon, kattintson a Mentés gombra, és alkalmazza újra a dinamikus titkosítás.</span><span class="sxs-lookup"><span data-stu-id="6b362-117">If you want to encrypt your asset using new policies, configure them with the Azure portal, click save, and reapply dynamic encryption.</span></span> 
> 
> 

## <a name="start-configuring-content-protection"></a><span data-ttu-id="6b362-118">Indítsa el a tartalom védelmének beállítása</span><span class="sxs-lookup"><span data-stu-id="6b362-118">Start configuring content protection</span></span>
<span data-ttu-id="6b362-119">A portál segítségével indítsa el a tartalom védelmének konfigurálása, globális az AMS-fiók, a következő módon:</span><span class="sxs-lookup"><span data-stu-id="6b362-119">To use the portal to start configuring content protection, global to your AMS account, do the following:</span></span>

1. <span data-ttu-id="6b362-120">Az [Azure-portálon](https://portal.azure.com/) válassza ki Azure Media Services-fiókját.</span><span class="sxs-lookup"><span data-stu-id="6b362-120">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="6b362-121">Válassza ki **beállítások** > **védelmi tartalom**.</span><span class="sxs-lookup"><span data-stu-id="6b362-121">Select **Settings** > **Content protection**.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a><span data-ttu-id="6b362-123">Kulcs/licenc engedélyezési házirend</span><span class="sxs-lookup"><span data-stu-id="6b362-123">Key/license authorization policy</span></span>
<span data-ttu-id="6b362-124">AMS több módon azok a felhasználók, akik vagy licencelési kérelmeket támogatja.</span><span class="sxs-lookup"><span data-stu-id="6b362-124">AMS supports multiple ways of authenticating users who make key or license requests.</span></span> <span data-ttu-id="6b362-125">A tartalomkulcs-hitelesítési szabályzatot kell Ön által konfigurált és érheti el, ha az ügyfél delived kell ahhoz, hogy a kulcs/licencfeltételeket ügyfélprogramba.</span><span class="sxs-lookup"><span data-stu-id="6b362-125">The content key authorization policy must be configured by you and met by your client in order for the key/license to be delived to the client.</span></span> <span data-ttu-id="6b362-126">A tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: **nyissa meg a** vagy **token** korlátozás.</span><span class="sxs-lookup"><span data-stu-id="6b362-126">The content key authorization policy could have one or more authorization restrictions: **open** or **token** restriction.</span></span>

<span data-ttu-id="6b362-127">Az Azure-portálon lehetővé teszi, hogy hozzon létre egyet **kulcs/licenc engedélyezési házirend** minden típusú titkosítások használatára.</span><span class="sxs-lookup"><span data-stu-id="6b362-127">The Azure portal enables you to create one **key/license authorization policy** for all types of encryptions.</span></span>

### <a name="open"></a><span data-ttu-id="6b362-128">Nyílt</span><span class="sxs-lookup"><span data-stu-id="6b362-128">Open</span></span>
<span data-ttu-id="6b362-129">Nyissa meg a szoftverkorlátozó azt jelenti, hogy a rendszer számára, akik egy kulcs kérést fog továbbítani a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="6b362-129">Open restriction means that the system will deliver the key to anyone who makes a key request.</span></span> <span data-ttu-id="6b362-130">Ez a korlátozás tesztelési célokra hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="6b362-130">This restriction might be useful for test purposes.</span></span> 

### <a name="token"></a><span data-ttu-id="6b362-131">Token</span><span class="sxs-lookup"><span data-stu-id="6b362-131">Token</span></span>
<span data-ttu-id="6b362-132">A tokennel korlátozott szabályzatokhoz a Secure Token Service (Biztonsági jegykiadó szolgáltatás, STS) által kiadott tokennek kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="6b362-132">The token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="6b362-133">A Media Services Simple Web Tokens (SWT) és a JSON webes jogkivonat (JWT) formátumú tokeneket támogatja.</span><span class="sxs-lookup"><span data-stu-id="6b362-133">Media Services supports tokens in the Simple Web Tokens (SWT) format and JSON Web Token (JWT) format.</span></span> <span data-ttu-id="6b362-134">A Media Services nem nyújt Secure Token szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="6b362-134">Media Services does not provide Secure Token Services.</span></span> <span data-ttu-id="6b362-135">Hozzon létre egy egyéni STS, vagy probléma jogkivonatokat a Microsoft Azure ACS kihasználja.</span><span class="sxs-lookup"><span data-stu-id="6b362-135">You can create a custom STS or leverage Microsoft Azure ACS to issue tokens.</span></span> <span data-ttu-id="6b362-136">Az STS be kell állítani a megadott kulcs és a probléma JOGCÍMEKKEL, amely a token korlátozás konfigurációjában megadott aláírt jogkivonat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6b362-136">The STS must be configured to create a token signed with the specified key and issue claims that you specified in the token restriction configuration.</span></span> <span data-ttu-id="6b362-137">A Media Services kulcs kézbesítési szolgáltatás visszaáll a kért (vagy licencelési) az ügyfél, ha a jogkivonat érvényes, és a jogcímek, az token találat azokat konfigurált kulcsot (vagy licenc).</span><span class="sxs-lookup"><span data-stu-id="6b362-137">The Media Services key delivery service will return the requested key (or license) to the client if the token is valid and the claims in the token match those configured for the key (or license).</span></span>

<span data-ttu-id="6b362-138">Ha a házirend konfigurálása a token korlátozott, az elsődleges hitelesítési kulcs, a kibocsátó és a célközönség paramétereket kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="6b362-138">When configuring the token restricted policy, you must specify the primary verification key, issuer, and audience parameters.</span></span> <span data-ttu-id="6b362-139">Az elsődleges hitelesítési kulcs, amely a token aláírt kulcsot tartalmazza, a kibocsátó a biztonságos biztonságijogkivonat-szolgáltatás, amely kibocsátja a jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="6b362-139">The primary verification key contains the key that the token was signed with, issuer is the secure token service that issues the token.</span></span> <span data-ttu-id="6b362-140">A célközönség (más néven hatókör) ismerteti a jogkivonat a leképezést, vagy az erőforrás a token engedélyezi a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="6b362-140">The audience (sometimes called scope) describes the intent of the token or the resource the token authorizes access to.</span></span> <span data-ttu-id="6b362-141">A Media Services kulcs kézbesítési szolgáltatás ellenőrzi, hogy ezek az értékek a token egyeznek-e a sablonban szereplő értékeket.</span><span class="sxs-lookup"><span data-stu-id="6b362-141">The Media Services key delivery service validates that these values in the token match the values in the template.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a><span data-ttu-id="6b362-143">PlayReady-jogosultságsablont</span><span class="sxs-lookup"><span data-stu-id="6b362-143">PlayReady rights template</span></span>
<span data-ttu-id="6b362-144">A PlayReady jogosultságsablont kapcsolatos részletes információkért lásd: [Media Services PlayReady licenc sablon áttekintése](media-services-playready-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6b362-144">For detailed information about the PlayReady rights template, see [Media Services PlayReady License Template Overview](media-services-playready-license-template-overview.md).</span></span>

### <a name="non-persistent"></a><span data-ttu-id="6b362-145">Nem állandó</span><span class="sxs-lookup"><span data-stu-id="6b362-145">Non persistent</span></span>
<span data-ttu-id="6b362-146">Licenc nem állandó, konfigurálása, ha azt csak használatban van memória amíg a Windows Media player használja a licenc.</span><span class="sxs-lookup"><span data-stu-id="6b362-146">If you configure license as non-persistent, it is only held in memory while the player is using the license.</span></span>  

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a><span data-ttu-id="6b362-148">Állandó</span><span class="sxs-lookup"><span data-stu-id="6b362-148">Persistent</span></span>
<span data-ttu-id="6b362-149">Ha a licenc állandó, konfigurálása, a Mentés az állandó tároló a az ügyfél.</span><span class="sxs-lookup"><span data-stu-id="6b362-149">If you configure the license  as persistent, it is saved in persistent storage on the client.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a><span data-ttu-id="6b362-151">Widevine-jogosultságsablont</span><span class="sxs-lookup"><span data-stu-id="6b362-151">Widevine rights template</span></span>
<span data-ttu-id="6b362-152">A Widevine jogosultságsablont kapcsolatos részletes információkért lásd: [Widevine-licenc sablon áttekintése](media-services-widevine-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6b362-152">For detailed information about the Widevine rights template, see [Widevine License Template Overview](media-services-widevine-license-template-overview.md).</span></span>

### <a name="basic"></a><span data-ttu-id="6b362-153">Basic</span><span class="sxs-lookup"><span data-stu-id="6b362-153">Basic</span></span>
<span data-ttu-id="6b362-154">Ha bejelöli **alapvető**, a sablon összes alapértelmezett érték jön létre.</span><span class="sxs-lookup"><span data-stu-id="6b362-154">When you select **Basic**, the template will be created with all defaults values.</span></span>

### <a name="advanced"></a><span data-ttu-id="6b362-155">Extra szintű</span><span class="sxs-lookup"><span data-stu-id="6b362-155">Advanced</span></span>
<span data-ttu-id="6b362-156">Widevine-konfigurációk előzetes lehetőségekről részletes ismertetése [ez](media-services-widevine-license-template-overview.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="6b362-156">For detailed explanation about advance option of Widevine configurations, see [this](media-services-widevine-license-template-overview.md) topic.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a><span data-ttu-id="6b362-158">FairPlay-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="6b362-158">FairPlay configuration</span></span>
<span data-ttu-id="6b362-159">FairPlay-titkosítás engedélyezéséhez meg kell adnia a App tanúsítvány és a alkalmazás titkos kulcs (KÉRJEN) keresztül a FairPlay konfigurációs beállítás.</span><span class="sxs-lookup"><span data-stu-id="6b362-159">To enable FairPlay encryption, you need to provide the App Certificate and Application Secret Key (ASK) through the FairPlay Configuration option.</span></span> <span data-ttu-id="6b362-160">FairPlay konfigurációs és követelmények kapcsolatos részletes információkért lásd: [ez](media-services-protect-hls-with-fairplay.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="6b362-160">For detailed information about FairPlay configuration and requirements, see [this](media-services-protect-hls-with-fairplay.md) article.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a><span data-ttu-id="6b362-162">A dinamikus titkosítás alkalmazása az eszközre</span><span class="sxs-lookup"><span data-stu-id="6b362-162">Apply dynamic encryption to your asset</span></span>
<span data-ttu-id="6b362-163">A dinamikus titkosítás előnyeit, a forrásfájl kódolása adaptív sávszélességű MP4-fájlokká be kell.</span><span class="sxs-lookup"><span data-stu-id="6b362-163">To take advantage of dynamic encryption, you need to encode your source file into a set of adaptive-bitrate MP4 files.</span></span>

### <a name="select-an-asset-that-you-want-to-encrypt"></a><span data-ttu-id="6b362-164">Válassza ki egy eszközt, hogy titkosítani szeretné</span><span class="sxs-lookup"><span data-stu-id="6b362-164">Select an asset that you want to encrypt</span></span>
<span data-ttu-id="6b362-165">Az eszközök megtekintéséhez válasszon **beállítások** > **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="6b362-165">To see all your assets, select **Settings** > **Assets**.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a><span data-ttu-id="6b362-167">Az AES vagy DRM titkosítása</span><span class="sxs-lookup"><span data-stu-id="6b362-167">Encrypt with AES or DRM</span></span>
<span data-ttu-id="6b362-168">Ha lenyomja az **titkosítása** egy eszköz, lehetősége lesz két választási lehetőség: **AES** vagy **DRM**.</span><span class="sxs-lookup"><span data-stu-id="6b362-168">Once you press **Encrypt** on an asset, you are presented wtih two choices: **AES** or **DRM**.</span></span> 

#### <a name="aes"></a><span data-ttu-id="6b362-169">AES</span><span class="sxs-lookup"><span data-stu-id="6b362-169">AES</span></span>
<span data-ttu-id="6b362-170">Törölje a jelet titkosítás engedélyezve lesz az összes adatfolyam-továbbítási protokollok AES: Smooth Streaming, HLS vagy MPEG-DASH.</span><span class="sxs-lookup"><span data-stu-id="6b362-170">AES clear key encryption will be enabled on all streaming protocols: Smooth Streaming, HLS, and MPEG-DASH.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a><span data-ttu-id="6b362-172">DRM</span><span class="sxs-lookup"><span data-stu-id="6b362-172">DRM</span></span>
<span data-ttu-id="6b362-173">A DRM lapon kiválasztásakor lehetősége lesz a tartalomvédelem házirendek más lehetőségek (amely konfigurálnia kell mostanra) + protokollok streameléshez készlete.</span><span class="sxs-lookup"><span data-stu-id="6b362-173">When you select the DRM tab, you are presented with different choices of content protection policies (which you must have configured by now) + a set of streaming protocols.</span></span>

* <span data-ttu-id="6b362-174">**PlayReady és Widevine rendelkező MPEG-DASH** -rendszer dinamikus titkosítást a PlayReady és Widevine DRMs MPEG-DASH adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="6b362-174">**PlayReady and Widevine with MPEG-DASH** - will dynamically encrypt your MPEG-DASH stream with PlayReady and Widevine DRMs.</span></span>
* <span data-ttu-id="6b362-175">**PlayReady és MPEG-DASH végzett Widevine + a HLS FairPlay** -dinamikusan titkosítja, MPEG-DASH-adatfolyam a PlayReady vagy Widevine DRMs.</span><span class="sxs-lookup"><span data-stu-id="6b362-175">**PlayReady and Widevine with MPEG-DASH + FairPlay with HLS** - will dynamically encrypt you MPEG-DASH stream with PlayReady and Widevine DRMs.</span></span> <span data-ttu-id="6b362-176">A HLS-adatfolyamok FairPlay is titkosítja.</span><span class="sxs-lookup"><span data-stu-id="6b362-176">Will also encrypt your HLS streams with FairPlay.</span></span>
* <span data-ttu-id="6b362-177">**Csak a Smooth Streaming, HLS, és MPEG-DASH PlayReady** -dinamikusan titkosítani fog Smooth Streaming, HLS, MPEG-DASH-adatfolyamok PlayReady DRM.</span><span class="sxs-lookup"><span data-stu-id="6b362-177">**PlayReady only with Smooth Streaming, HLS and MPEG-DASH** - will dynamically encrypt Smooth Streaming, HLS, MPEG-DASH streams with PlayReady DRM.</span></span>
* <span data-ttu-id="6b362-178">**Csak a MPEG-DASH Widevine** -dinamikusan titkosítani, MPEG-DASH a Widevine DRM-Védelemmel.</span><span class="sxs-lookup"><span data-stu-id="6b362-178">**Widevine only with MPEG-DASH** - will dynamically encrypt you MPEG-DASH with Widevine DRM.</span></span>
* <span data-ttu-id="6b362-179">**Csak a HLS FairPlay** -dinamikusan titkosítja a HLS adatfolyam FairPlay.</span><span class="sxs-lookup"><span data-stu-id="6b362-179">**FairPlay only with HLS** - will dynamically encrypt your HLS stream with FairPlay.</span></span>

<span data-ttu-id="6b362-180">FairPlay-titkosítás engedélyezéséhez meg kell adnia a App tanúsítvány és a alkalmazás titkos kulcs (KÉRJEN) keresztül a FairPlay konfigurációs beállítást, a tartalom védelmi beállítások panelről.</span><span class="sxs-lookup"><span data-stu-id="6b362-180">To enable FairPlay encryption, you need to provide the App Certificate and Application Secret Key (ASK) through the FairPlay Configuration option of the Content Protection settings blade.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection009.png)

<span data-ttu-id="6b362-182">Miután elvégezte a kijelölt titkosítás, nyomja le az **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="6b362-182">Once you make the encryption selection, press **Apply**.</span></span>

>[!NOTE] 
><span data-ttu-id="6b362-183">Ha azt tervezi, számára, hogy az AES titkosított HLS a Safari című [ebben a blogban](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span><span class="sxs-lookup"><span data-stu-id="6b362-183">If you are planning to play an AES encrypted HLS in Safari, see [this blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b362-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6b362-184">Next steps</span></span>
<span data-ttu-id="6b362-185">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="6b362-185">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6b362-186">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="6b362-186">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

