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
# <a name="configuring-content-protection-policies-using-hello-azure-portal"></a><span data-ttu-id="bff76-104">Tartalomvédelem házirendekkel hello Azure-portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bff76-104">Configuring content protection policies using hello Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="bff76-105">toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="bff76-105">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="bff76-106">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bff76-106">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="bff76-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="bff76-107">Overview</span></span>
<span data-ttu-id="bff76-108">Microsoft Azure Media Services (AMS) lehetővé teszi, hogy Ön toosecure hello idő keresztül tárhely, feldolgozás és kézbesítési elhagyják a adathordozókról.</span><span class="sxs-lookup"><span data-stu-id="bff76-108">Microsoft Azure Media Services (AMS) enables you toosecure your media from hello time it leaves your computer through storage, processing, and delivery.</span></span> <span data-ttu-id="bff76-109">A Media Services toodeliver lehetővé teszi a tartalom titkosított dinamikusan az Advanced Encryption Standard (AES) (a 128 bites titkosítási kulcsok használatával), közös titkosítás (CENC) segítségével PlayReady és/vagy a Widevine DRM-Védelemmel, és az Apple FairPlay.</span><span class="sxs-lookup"><span data-stu-id="bff76-109">Media Services allows you toodeliver your content encrypted dynamically with Advanced Encryption Standard (AES) (using 128-bit encryption keys), common encryption (CENC) using PlayReady and/or Widevine DRM, and Apple FairPlay.</span></span> 

<span data-ttu-id="bff76-110">AMS DRM-licencek kézbesítéséhez szolgáltatást nyújt, és AES kulcsok tooauthorized ügyfelek törölje.</span><span class="sxs-lookup"><span data-stu-id="bff76-110">AMS provides a service for delivering DRM licenses and AES clear keys tooauthorized clients.</span></span> <span data-ttu-id="bff76-111">hello Azure-portál lehetővé teszi egy toocreate **kulcs/licenc engedélyezési házirend** minden típusú titkosítások használatára.</span><span class="sxs-lookup"><span data-stu-id="bff76-111">hello Azure portal enables you toocreate one **key/license authorization policy** for all types of encryptions.</span></span>

<span data-ttu-id="bff76-112">Ez a cikk bemutatja, hogyan tooconfigure content protection házirendeket és hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="bff76-112">This article demonstrates how tooconfigure content protection policies with hello Azure portal.</span></span> <span data-ttu-id="bff76-113">hello a következő cikket is bemutatja hogyan tooapply a dinamikus titkosítás tooyour eszközök.</span><span class="sxs-lookup"><span data-stu-id="bff76-113">hello article also shows how tooapply dynamic encryption tooyour assets.</span></span>


> [!NOTE]
> <span data-ttu-id="bff76-114">Ha hello Azure klasszikus portál toocreate adatvédelmi szabályzatok, hello házirendek nem szerepelhet hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="bff76-114">If you used hello Azure classic portal toocreate protection policies, hello policies may not appear in hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="bff76-115">Azonban az összes régi hello házirendek továbbra is létezik.</span><span class="sxs-lookup"><span data-stu-id="bff76-115">However, all hello old polices still exist.</span></span> <span data-ttu-id="bff76-116">Tekintse meg őket használatával hello Azure Media Services .NET SDK vagy hello [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) eszköz (toosee hello házirendek, kattintson a jobb gombbal a hello eszköz -> információkat (F4) -> kattintson a tartalomkulcs -> a lap megjelenítése Kattintson a hello kulcs).</span><span class="sxs-lookup"><span data-stu-id="bff76-116">You can examine them using hello Azure Media Services .NET SDK or hello [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) tool (toosee hello policies, right-click on hello asset -> Display information (F4)->click on Content keys tab-> click on hello key).</span></span> 
> 
> <span data-ttu-id="bff76-117">Ha tooencrypt az eszköz új szabályzatokkal, konfigurálja őket a hello Azure-portálon, kattintson a Mentés gombra, és alkalmazza újra a dinamikus titkosítás.</span><span class="sxs-lookup"><span data-stu-id="bff76-117">If you want tooencrypt your asset using new policies, configure them with hello Azure portal, click save, and reapply dynamic encryption.</span></span> 
> 
> 

## <a name="start-configuring-content-protection"></a><span data-ttu-id="bff76-118">Indítsa el a tartalom védelmének beállítása</span><span class="sxs-lookup"><span data-stu-id="bff76-118">Start configuring content protection</span></span>
<span data-ttu-id="bff76-119">toouse hello portál toostart konfigurálása tartalomvédelem, a globális tooyour AMS-fiók, hello a következő:</span><span class="sxs-lookup"><span data-stu-id="bff76-119">toouse hello portal toostart configuring content protection, global tooyour AMS account, do hello following:</span></span>

1. <span data-ttu-id="bff76-120">A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="bff76-120">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="bff76-121">Válassza ki **beállítások** > **védelmi tartalom**.</span><span class="sxs-lookup"><span data-stu-id="bff76-121">Select **Settings** > **Content protection**.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a><span data-ttu-id="bff76-123">Kulcs/licenc engedélyezési házirend</span><span class="sxs-lookup"><span data-stu-id="bff76-123">Key/license authorization policy</span></span>
<span data-ttu-id="bff76-124">AMS több módon azok a felhasználók, akik vagy licencelési kérelmeket támogatja.</span><span class="sxs-lookup"><span data-stu-id="bff76-124">AMS supports multiple ways of authenticating users who make key or license requests.</span></span> <span data-ttu-id="bff76-125">hello tartalomkulcs-hitelesítési szabályzatot kell Ön által konfigurált és érheti el, ha az ügyfél ahhoz, hogy hello kulcs/licenc toobe delived toohello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="bff76-125">hello content key authorization policy must be configured by you and met by your client in order for hello key/license toobe delived toohello client.</span></span> <span data-ttu-id="bff76-126">hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: **nyissa meg a** vagy **token** korlátozás.</span><span class="sxs-lookup"><span data-stu-id="bff76-126">hello content key authorization policy could have one or more authorization restrictions: **open** or **token** restriction.</span></span>

<span data-ttu-id="bff76-127">hello Azure-portál lehetővé teszi egy toocreate **kulcs/licenc engedélyezési házirend** minden típusú titkosítások használatára.</span><span class="sxs-lookup"><span data-stu-id="bff76-127">hello Azure portal enables you toocreate one **key/license authorization policy** for all types of encryptions.</span></span>

### <a name="open"></a><span data-ttu-id="bff76-128">Nyílt</span><span class="sxs-lookup"><span data-stu-id="bff76-128">Open</span></span>
<span data-ttu-id="bff76-129">Nyissa meg a szoftverkorlátozó azt jelenti, hogy hello rendszer hello kulcs tooanyone kulcs kérést fog továbbítani.</span><span class="sxs-lookup"><span data-stu-id="bff76-129">Open restriction means that hello system will deliver hello key tooanyone who makes a key request.</span></span> <span data-ttu-id="bff76-130">Ez a korlátozás tesztelési célokra hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="bff76-130">This restriction might be useful for test purposes.</span></span> 

### <a name="token"></a><span data-ttu-id="bff76-131">Token</span><span class="sxs-lookup"><span data-stu-id="bff76-131">Token</span></span>
<span data-ttu-id="bff76-132">hello token korlátozott házirend által a Secure Token Service (STS) kiadott tokennek kell csatolni.</span><span class="sxs-lookup"><span data-stu-id="bff76-132">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="bff76-133">A Media Services hello Simple Web Tokens (SWT) és JSON webes jogkivonat (JWT) formátumú tokeneket támogatja.</span><span class="sxs-lookup"><span data-stu-id="bff76-133">Media Services supports tokens in hello Simple Web Tokens (SWT) format and JSON Web Token (JWT) format.</span></span> <span data-ttu-id="bff76-134">A Media Services nem nyújt Secure Token szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="bff76-134">Media Services does not provide Secure Token Services.</span></span> <span data-ttu-id="bff76-135">Hozzon létre egy egyéni STS, vagy használja a Microsoft Azure ACS tooissue jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="bff76-135">You can create a custom STS or leverage Microsoft Azure ACS tooissue tokens.</span></span> <span data-ttu-id="bff76-136">hello STS kell lennie a megadott hello aláírt jogkivonat konfigurált toocreate hello token korlátozás a konfigurációban megadott kulcs és a probléma jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="bff76-136">hello STS must be configured toocreate a token signed with hello specified key and issue claims that you specified in hello token restriction configuration.</span></span> <span data-ttu-id="bff76-137">kulcs kézbesítési szolgáltatás visszaadható hello (vagy licencelési) toohello ügyfél Ha hello jogkivonat érvényes, és hello kért hello Media Services-jogcímek az hello token megfelelő azokat konfigurált hello (vagy licencelési).</span><span class="sxs-lookup"><span data-stu-id="bff76-137">hello Media Services key delivery service will return hello requested key (or license) toohello client if hello token is valid and hello claims in hello token match those configured for hello key (or license).</span></span>

<span data-ttu-id="bff76-138">Hello token korlátozott házirend konfigurálásakor hello elsődleges hitelesítési kulcs, a kibocsátó és a célközönség paramétereket kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="bff76-138">When configuring hello token restricted policy, you must specify hello primary verification key, issuer, and audience parameters.</span></span> <span data-ttu-id="bff76-139">hello elsődleges hitelesítési kulcsot tartalmazó hello kulcsfontosságú, hogy hello token lett aláírva, illetve kibocsátó hello biztonságos biztonságijogkivonat-szolgáltatás által kiállított hello jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="bff76-139">hello primary verification key contains hello key that hello token was signed with, issuer is hello secure token service that issues hello token.</span></span> <span data-ttu-id="bff76-140">hello célközönség (más néven hatókör) hello token hello célját ismerteti, vagy hello erőforrás hello token engedélyezi a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="bff76-140">hello audience (sometimes called scope) describes hello intent of hello token or hello resource hello token authorizes access to.</span></span> <span data-ttu-id="bff76-141">hello Media Services kulcs kézbesítési szolgáltatás ellenőrzi, hogy ezek az értékek hello token értékekre, hello hello sablont.</span><span class="sxs-lookup"><span data-stu-id="bff76-141">hello Media Services key delivery service validates that these values in hello token match hello values in hello template.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a><span data-ttu-id="bff76-143">PlayReady-jogosultságsablont</span><span class="sxs-lookup"><span data-stu-id="bff76-143">PlayReady rights template</span></span>
<span data-ttu-id="bff76-144">Hello PlayReady jogosultságsablont kapcsolatos részletes információkért lásd: [Media Services PlayReady licenc sablon áttekintése](media-services-playready-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bff76-144">For detailed information about hello PlayReady rights template, see [Media Services PlayReady License Template Overview](media-services-playready-license-template-overview.md).</span></span>

### <a name="non-persistent"></a><span data-ttu-id="bff76-145">Nem állandó</span><span class="sxs-lookup"><span data-stu-id="bff76-145">Non persistent</span></span>
<span data-ttu-id="bff76-146">Licenc nem állandó, konfigurálása, ha azt csak használatban van memória közben hello player hello licencet használ.</span><span class="sxs-lookup"><span data-stu-id="bff76-146">If you configure license as non-persistent, it is only held in memory while hello player is using hello license.</span></span>  

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a><span data-ttu-id="bff76-148">Állandó</span><span class="sxs-lookup"><span data-stu-id="bff76-148">Persistent</span></span>
<span data-ttu-id="bff76-149">Ha a konfigurált hello licenc állandó, a Mentés az állandó tároló a hello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="bff76-149">If you configure hello license  as persistent, it is saved in persistent storage on hello client.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a><span data-ttu-id="bff76-151">Widevine-jogosultságsablont</span><span class="sxs-lookup"><span data-stu-id="bff76-151">Widevine rights template</span></span>
<span data-ttu-id="bff76-152">Hello Widevine jogosultságsablont kapcsolatos részletes információkért lásd: [Widevine-licenc sablon áttekintése](media-services-widevine-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bff76-152">For detailed information about hello Widevine rights template, see [Widevine License Template Overview](media-services-widevine-license-template-overview.md).</span></span>

### <a name="basic"></a><span data-ttu-id="bff76-153">Basic</span><span class="sxs-lookup"><span data-stu-id="bff76-153">Basic</span></span>
<span data-ttu-id="bff76-154">Ha bejelöli **alapvető**, hello sablon jön létre minden alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="bff76-154">When you select **Basic**, hello template will be created with all defaults values.</span></span>

### <a name="advanced"></a><span data-ttu-id="bff76-155">Extra szintű</span><span class="sxs-lookup"><span data-stu-id="bff76-155">Advanced</span></span>
<span data-ttu-id="bff76-156">Widevine-konfigurációk előzetes lehetőségekről részletes ismertetése [ez](media-services-widevine-license-template-overview.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="bff76-156">For detailed explanation about advance option of Widevine configurations, see [this](media-services-widevine-license-template-overview.md) topic.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a><span data-ttu-id="bff76-158">FairPlay-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="bff76-158">FairPlay configuration</span></span>
<span data-ttu-id="bff76-159">tooenable FairPlay titkosításhoz, és van szükség tooprovide hello App tanúsítvány alkalmazás titkos kulcs (KÉRJEN) hello FairPlay konfigurációs beállítás használatával.</span><span class="sxs-lookup"><span data-stu-id="bff76-159">tooenable FairPlay encryption, you need tooprovide hello App Certificate and Application Secret Key (ASK) through hello FairPlay Configuration option.</span></span> <span data-ttu-id="bff76-160">FairPlay konfigurációs és követelmények kapcsolatos részletes információkért lásd: [ez](media-services-protect-hls-with-fairplay.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="bff76-160">For detailed information about FairPlay configuration and requirements, see [this](media-services-protect-hls-with-fairplay.md) article.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-tooyour-asset"></a><span data-ttu-id="bff76-162">A dinamikus titkosítás tooyour eszköz alkalmazása</span><span class="sxs-lookup"><span data-stu-id="bff76-162">Apply dynamic encryption tooyour asset</span></span>
<span data-ttu-id="bff76-163">tootake előnye a dinamikus titkosítás kell tooencode a forrásfájlt adaptív sávszélességű MP4-fájlokat alakítja.</span><span class="sxs-lookup"><span data-stu-id="bff76-163">tootake advantage of dynamic encryption, you need tooencode your source file into a set of adaptive-bitrate MP4 files.</span></span>

### <a name="select-an-asset-that-you-want-tooencrypt"></a><span data-ttu-id="bff76-164">Válassza ki egy eszközt, amelyet az tooencrypt</span><span class="sxs-lookup"><span data-stu-id="bff76-164">Select an asset that you want tooencrypt</span></span>
<span data-ttu-id="bff76-165">toosee az eszközök, válasszon **beállítások** > **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="bff76-165">toosee all your assets, select **Settings** > **Assets**.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a><span data-ttu-id="bff76-167">Az AES vagy DRM titkosítása</span><span class="sxs-lookup"><span data-stu-id="bff76-167">Encrypt with AES or DRM</span></span>
<span data-ttu-id="bff76-168">Ha lenyomja az **titkosítása** egy eszköz, lehetősége lesz két választási lehetőség: **AES** vagy **DRM**.</span><span class="sxs-lookup"><span data-stu-id="bff76-168">Once you press **Encrypt** on an asset, you are presented wtih two choices: **AES** or **DRM**.</span></span> 

#### <a name="aes"></a><span data-ttu-id="bff76-169">AES</span><span class="sxs-lookup"><span data-stu-id="bff76-169">AES</span></span>
<span data-ttu-id="bff76-170">Törölje a jelet titkosítás engedélyezve lesz az összes adatfolyam-továbbítási protokollok AES: Smooth Streaming, HLS vagy MPEG-DASH.</span><span class="sxs-lookup"><span data-stu-id="bff76-170">AES clear key encryption will be enabled on all streaming protocols: Smooth Streaming, HLS, and MPEG-DASH.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a><span data-ttu-id="bff76-172">DRM</span><span class="sxs-lookup"><span data-stu-id="bff76-172">DRM</span></span>
<span data-ttu-id="bff76-173">Amikor hello DRM lapon válassza ki, lehetősége lesz a tartalomvédelem házirendek más lehetőségek (amely konfigurálnia kell mostanra) + protokollok streameléshez készlete.</span><span class="sxs-lookup"><span data-stu-id="bff76-173">When you select hello DRM tab, you are presented with different choices of content protection policies (which you must have configured by now) + a set of streaming protocols.</span></span>

* <span data-ttu-id="bff76-174">**PlayReady és Widevine rendelkező MPEG-DASH** -rendszer dinamikus titkosítást a PlayReady és Widevine DRMs MPEG-DASH adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="bff76-174">**PlayReady and Widevine with MPEG-DASH** - will dynamically encrypt your MPEG-DASH stream with PlayReady and Widevine DRMs.</span></span>
* <span data-ttu-id="bff76-175">**PlayReady és MPEG-DASH végzett Widevine + a HLS FairPlay** -dinamikusan titkosítja, MPEG-DASH-adatfolyam a PlayReady vagy Widevine DRMs.</span><span class="sxs-lookup"><span data-stu-id="bff76-175">**PlayReady and Widevine with MPEG-DASH + FairPlay with HLS** - will dynamically encrypt you MPEG-DASH stream with PlayReady and Widevine DRMs.</span></span> <span data-ttu-id="bff76-176">A HLS-adatfolyamok FairPlay is titkosítja.</span><span class="sxs-lookup"><span data-stu-id="bff76-176">Will also encrypt your HLS streams with FairPlay.</span></span>
* <span data-ttu-id="bff76-177">**Csak a Smooth Streaming, HLS, és MPEG-DASH PlayReady** -dinamikusan titkosítani fog Smooth Streaming, HLS, MPEG-DASH-adatfolyamok PlayReady DRM.</span><span class="sxs-lookup"><span data-stu-id="bff76-177">**PlayReady only with Smooth Streaming, HLS and MPEG-DASH** - will dynamically encrypt Smooth Streaming, HLS, MPEG-DASH streams with PlayReady DRM.</span></span>
* <span data-ttu-id="bff76-178">**Csak a MPEG-DASH Widevine** -dinamikusan titkosítani, MPEG-DASH a Widevine DRM-Védelemmel.</span><span class="sxs-lookup"><span data-stu-id="bff76-178">**Widevine only with MPEG-DASH** - will dynamically encrypt you MPEG-DASH with Widevine DRM.</span></span>
* <span data-ttu-id="bff76-179">**Csak a HLS FairPlay** -dinamikusan titkosítja a HLS adatfolyam FairPlay.</span><span class="sxs-lookup"><span data-stu-id="bff76-179">**FairPlay only with HLS** - will dynamically encrypt your HLS stream with FairPlay.</span></span>

<span data-ttu-id="bff76-180">tooenable FairPlay titkosításhoz, és van szükség tooprovide hello App tanúsítvány alkalmazás titkos kulcs (KÉRJEN) keresztül hello FairPlay konfigurációs beállítás hello tartalomvédelem beállítások panelről.</span><span class="sxs-lookup"><span data-stu-id="bff76-180">tooenable FairPlay encryption, you need tooprovide hello App Certificate and Application Secret Key (ASK) through hello FairPlay Configuration option of hello Content Protection settings blade.</span></span>

![Tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection009.png)

<span data-ttu-id="bff76-182">Ha hello titkosítás kiválasztása, nyomja meg az **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="bff76-182">Once you make hello encryption selection, press **Apply**.</span></span>

>[!NOTE] 
><span data-ttu-id="bff76-183">Ha azt tervezi, tooplay az AES titkosított HLS a Safari című [ebben a blogban](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span><span class="sxs-lookup"><span data-stu-id="bff76-183">If you are planning tooplay an AES encrypted HLS in Safari, see [this blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bff76-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bff76-184">Next steps</span></span>
<span data-ttu-id="bff76-185">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="bff76-185">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="bff76-186">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="bff76-186">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

