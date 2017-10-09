---
title: aaaProtect HLS tartalmakat a Microsoft PlayReady vagy Apple FairPlay - Azure |} Microsoft Docs
description: "Ez a témakör áttekintést nyújt, és bemutatja, hogyan toouse Azure Media Services toodynamically titkosítani a HTTP-Live Streaming (HLS) az Apple FairPlay. Azt is bemutatja, hogyan toouse hello Media Services licenc kézbesítési szolgáltatás toodeliver FairPlay-licenc tooclients."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 91ca451e3e7bf0da1d74dac4c99180f08f39e4ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protect-your-hls-content-with-apple-fairplay-or-microsoft-playready"></a><span data-ttu-id="7deb2-104">A tartalom Apple FairPlay vagy a Microsoft PlayReady HLS védelme</span><span class="sxs-lookup"><span data-stu-id="7deb2-104">Protect your HLS content with Apple FairPlay or Microsoft PlayReady</span></span>
<span data-ttu-id="7deb2-105">Az Azure Media Services lehetővé teszi, hogy Ön toodynamically titkosítani a HTTP-Live Streaming (HLS) a következő formátumok hello segítségével:</span><span class="sxs-lookup"><span data-stu-id="7deb2-105">Azure Media Services enables you toodynamically encrypt your HTTP Live Streaming (HLS) content by using hello following formats:</span></span>  

* <span data-ttu-id="7deb2-106">**AES-128 boríték tiszta kulcsot**</span><span class="sxs-lookup"><span data-stu-id="7deb2-106">**AES-128 envelope clear key**</span></span>

    <span data-ttu-id="7deb2-107">hello teljes adatrészlet használatával titkosítja hello **AES-128 CBC** mód.</span><span class="sxs-lookup"><span data-stu-id="7deb2-107">hello entire chunk is encrypted by using hello **AES-128 CBC** mode.</span></span> <span data-ttu-id="7deb2-108">hello adatfolyam hello visszafejtése natív módon támogatott iOS és OS X-lejátszót.</span><span class="sxs-lookup"><span data-stu-id="7deb2-108">hello decryption of hello stream is supported by iOS and OS X player natively.</span></span> <span data-ttu-id="7deb2-109">További információkért lásd: [AES-128 használata a dinamikus titkosítás és a kulcs kézbesítési szolgáltatás](media-services-protect-with-aes128.md).</span><span class="sxs-lookup"><span data-stu-id="7deb2-109">For more information, see [Using AES-128 dynamic encryption and key delivery service](media-services-protect-with-aes128.md).</span></span>
* <span data-ttu-id="7deb2-110">**Apple FairPlay**</span><span class="sxs-lookup"><span data-stu-id="7deb2-110">**Apple FairPlay**</span></span>

    <span data-ttu-id="7deb2-111">egyes videó hello és hang minták hello segítségével lettek titkosítva **AES-128 CBC** mód.</span><span class="sxs-lookup"><span data-stu-id="7deb2-111">hello individual video and audio samples are encrypted by using hello **AES-128 CBC** mode.</span></span> <span data-ttu-id="7deb2-112">**FairPlay Streaming** (FPS) integrálva van hello eszköz-operációsrendszerek, iOS és az Apple TV natív támogatását.</span><span class="sxs-lookup"><span data-stu-id="7deb2-112">**FairPlay Streaming** (FPS) is integrated into hello device operating systems, with native support on iOS and Apple TV.</span></span> <span data-ttu-id="7deb2-113">OS x Safari FPS hello titkosított adathordozó-bővítmények (EME) felület támogatása használatával lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="7deb2-113">Safari on OS X enables FPS by using hello Encrypted Media Extensions (EME) interface support.</span></span>
* <span data-ttu-id="7deb2-114">**Microsoft PlayReady**</span><span class="sxs-lookup"><span data-stu-id="7deb2-114">**Microsoft PlayReady**</span></span>

<span data-ttu-id="7deb2-115">hello következő kép bemutatja hello **HLS + FairPlay vagy PlayReady a dinamikus titkosítás** munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="7deb2-115">hello following image shows hello **HLS + FairPlay or PlayReady dynamic encryption** workflow.</span></span>

![A dinamikus titkosítás munkafolyamat diagramja](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

<span data-ttu-id="7deb2-117">Ez a témakör bemutatja, hogyan toouse Media Services toodynamically titkosítani az Apple FairPlay HLS tartalmaihoz.</span><span class="sxs-lookup"><span data-stu-id="7deb2-117">This topic demonstrates how toouse Media Services toodynamically encrypt your HLS content with Apple FairPlay.</span></span> <span data-ttu-id="7deb2-118">Azt is bemutatja, hogyan toouse hello Media Services licenc kézbesítési szolgáltatás toodeliver FairPlay-licenc tooclients.</span><span class="sxs-lookup"><span data-stu-id="7deb2-118">It also shows how toouse hello Media Services license delivery service toodeliver FairPlay licenses tooclients.</span></span>

> [!NOTE]
> <span data-ttu-id="7deb2-119">Ha is szeretné tooencrypt a HLS tartalom a PlayReady, toocreate közös tartalomkulcs kell, és rendelje hozzá azt az objektumot.</span><span class="sxs-lookup"><span data-stu-id="7deb2-119">If you also want tooencrypt your HLS content with PlayReady, you need toocreate a common content key and associate it with your asset.</span></span> <span data-ttu-id="7deb2-120">A is kell tooconfigure hello tartalomkulcs hitelesítési szabályzatának, [segítségével PlayReady a dynamic common encryption](media-services-protect-with-drm.md).</span><span class="sxs-lookup"><span data-stu-id="7deb2-120">You also need tooconfigure hello content key’s authorization policy, as described in [Using PlayReady dynamic common encryption](media-services-protect-with-drm.md).</span></span>
>
>

## <a name="requirements-and-considerations"></a><span data-ttu-id="7deb2-121">Követelmények és szempontok</span><span class="sxs-lookup"><span data-stu-id="7deb2-121">Requirements and considerations</span></span>

<span data-ttu-id="7deb2-122">hello következő szükség, a Media Services toodeliver titkosított HLS integrált FairPlay és toodeliver FairPlay-licenc használata esetén:</span><span class="sxs-lookup"><span data-stu-id="7deb2-122">hello following are required when using Media Services toodeliver HLS encrypted with FairPlay, and toodeliver FairPlay licenses:</span></span>

  * <span data-ttu-id="7deb2-123">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="7deb2-123">An Azure account.</span></span> <span data-ttu-id="7deb2-124">További részletek: [Ingyenes Azure-próbafiók](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="7deb2-124">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
  * <span data-ttu-id="7deb2-125">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="7deb2-125">A Media Services account.</span></span> <span data-ttu-id="7deb2-126">toocreate, lásd: [hello Azure-portál használatával Azure Media Services-fiók létrehozása](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="7deb2-126">toocreate one, see [Create an Azure Media Services account using hello Azure portal](media-services-portal-create-account.md).</span></span>
  * <span data-ttu-id="7deb2-127">A regisztráció [Apple fejlesztési Program](https://developer.apple.com/).</span><span class="sxs-lookup"><span data-stu-id="7deb2-127">Sign up with [Apple Development Program](https://developer.apple.com/).</span></span>
  * <span data-ttu-id="7deb2-128">Apple szükséges hello tartalom tulajdonosa tooobtain hello [központi telepítési csomag](https://developer.apple.com/contact/fps/).</span><span class="sxs-lookup"><span data-stu-id="7deb2-128">Apple requires hello content owner tooobtain hello [deployment package](https://developer.apple.com/contact/fps/).</span></span> <span data-ttu-id="7deb2-129">Adja meg, hogy már megvalósította a kulcs biztonsági modul (KSM) a Media Services, és, hogy a kért hello végső FPS csomag.</span><span class="sxs-lookup"><span data-stu-id="7deb2-129">State that you already implemented Key Security Module (KSM) with Media Services, and that you are requesting hello final FPS package.</span></span> <span data-ttu-id="7deb2-130">Nincsenek a végső FPS toogenerate hitelesítő csomagot, majd az beszerzése hello utasításait hello alkalmazás titkos kulcs (KÉRJEN).</span><span class="sxs-lookup"><span data-stu-id="7deb2-130">There are instructions in hello final FPS package toogenerate certification and obtain hello Application Secret Key (ASK).</span></span> <span data-ttu-id="7deb2-131">Kérje meg tooconfigure FairPlay használja.</span><span class="sxs-lookup"><span data-stu-id="7deb2-131">You use ASK tooconfigure FairPlay.</span></span>
  * <span data-ttu-id="7deb2-132">Az Azure Media Services .NET SDK-verzió **3.6.0** vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="7deb2-132">Azure Media Services .NET SDK version **3.6.0** or later.</span></span>

<span data-ttu-id="7deb2-133">a Media Services kulcs kézbesítési oldalon dolgok következő hello kell beállítani:</span><span class="sxs-lookup"><span data-stu-id="7deb2-133">hello following things must be set on Media Services key delivery side:</span></span>

  * <span data-ttu-id="7deb2-134">**Alkalmazás Cert (AC)**: Ez az hello titkos kulcsot tartalmazó .pfx fájlt.</span><span class="sxs-lookup"><span data-stu-id="7deb2-134">**App Cert (AC)**: This is a .pfx file that contains hello private key.</span></span> <span data-ttu-id="7deb2-135">A fájl létrehozásához, és a titkosítás jelszóval.</span><span class="sxs-lookup"><span data-stu-id="7deb2-135">You create this file and encrypt it with a password.</span></span>

       <span data-ttu-id="7deb2-136">A kulcs továbbítási szabályzatban konfigurálásakor meg kell adnia a jelszót és hello .pfx fájlt Base64 formátumban.</span><span class="sxs-lookup"><span data-stu-id="7deb2-136">When you configure a key delivery policy, you must provide that password and hello .pfx file in Base64 format.</span></span>

      <span data-ttu-id="7deb2-137">hello lépések azt mutatják be, hogyan toogenerate .pfx tanúsítvány FairPlay fájlt:</span><span class="sxs-lookup"><span data-stu-id="7deb2-137">hello following steps describe how toogenerate a .pfx certificate file for FairPlay:</span></span>

    1. <span data-ttu-id="7deb2-138">A https://slproweb.com/products/Win32OpenSSL.html OpenSSL telepíteni.</span><span class="sxs-lookup"><span data-stu-id="7deb2-138">Install OpenSSL from https://slproweb.com/products/Win32OpenSSL.html.</span></span>

        <span data-ttu-id="7deb2-139">Nyissa meg a toohello mappára, ahol hello FairPlay tanúsítvány és egyéb fájlokat, az Apple által szállított.</span><span class="sxs-lookup"><span data-stu-id="7deb2-139">Go toohello folder where hello FairPlay certificate and other files delivered by Apple are.</span></span>
    2. <span data-ttu-id="7deb2-140">Futtassa a parancsot követő hello parancssorból hello.</span><span class="sxs-lookup"><span data-stu-id="7deb2-140">Run hello following command from hello command line.</span></span> <span data-ttu-id="7deb2-141">Ez a hello .cer fájl tooa .pem-fájlokat alakítja át.</span><span class="sxs-lookup"><span data-stu-id="7deb2-141">This converts hello .cer file tooa .pem file.</span></span>

        <span data-ttu-id="7deb2-142">"C:\OpenSSL-Win32\bin\openssl.exe" x509-der tájékoztatja-a fairplay.cer-fairplay-out.pem kimenő</span><span class="sxs-lookup"><span data-stu-id="7deb2-142">"C:\OpenSSL-Win32\bin\openssl.exe" x509 -inform der -in fairplay.cer -out fairplay-out.pem</span></span>
    3. <span data-ttu-id="7deb2-143">Futtassa a parancsot követő hello parancssorból hello.</span><span class="sxs-lookup"><span data-stu-id="7deb2-143">Run hello following command from hello command line.</span></span> <span data-ttu-id="7deb2-144">Hello .pem fájl tooa .pfx fájl alakítja hello titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="7deb2-144">This converts hello .pem file tooa .pfx file with hello private key.</span></span> <span data-ttu-id="7deb2-145">hello jelszót hello .pfx fájl majd OpenSSL: kérte.</span><span class="sxs-lookup"><span data-stu-id="7deb2-145">hello password for hello .pfx file is then asked by OpenSSL.</span></span>

        <span data-ttu-id="7deb2-146">"C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-exportálása - kimenő fairplay-out.pfx-inkey privatekey.pem-fairplay-out.pem - passin file:privatekey-pem-pass.txt a</span><span class="sxs-lookup"><span data-stu-id="7deb2-146">"C:\OpenSSL-Win32\bin\openssl.exe" pkcs12 -export -out fairplay-out.pfx -inkey privatekey.pem -in fairplay-out.pem -passin file:privatekey-pem-pass.txt</span></span>
  * <span data-ttu-id="7deb2-147">**Alkalmazás Cert jelszó**: hello jelszót a .pfx fájl hello létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="7deb2-147">**App Cert password**: hello password for creating hello .pfx file.</span></span>
  * <span data-ttu-id="7deb2-148">**Alkalmazás Cert jelszó azonosítója**: hello jelszó, hasonló toohow azokat más Media Services kulcsok töltse fel kell tölteni.</span><span class="sxs-lookup"><span data-stu-id="7deb2-148">**App Cert password ID**: You must upload hello password, similar toohow they upload other Media Services keys.</span></span> <span data-ttu-id="7deb2-149">Használjon hello **ContentKeyType.FairPlayPfxPassword** felsorolási érték tooget hello Media Services-azonosító.</span><span class="sxs-lookup"><span data-stu-id="7deb2-149">Use hello **ContentKeyType.FairPlayPfxPassword** enum value tooget hello Media Services ID.</span></span> <span data-ttu-id="7deb2-150">Ez a szükséges toouse belül hello kulcs kézbesítési házirend-beállításként.</span><span class="sxs-lookup"><span data-stu-id="7deb2-150">This is what they need toouse inside hello key delivery policy option.</span></span>
  * <span data-ttu-id="7deb2-151">**IV**: egy véletlenszerű érték 16 bájt.</span><span class="sxs-lookup"><span data-stu-id="7deb2-151">**iv**: This is a random value of 16 bytes.</span></span> <span data-ttu-id="7deb2-152">Meg kell egyeznie a hello objektumtovábbítási szabályzat iv hello.</span><span class="sxs-lookup"><span data-stu-id="7deb2-152">It must match hello iv in hello asset delivery policy.</span></span> <span data-ttu-id="7deb2-153">Létrehozhat hello iv, és mindkét helyen elhelyezi: hello adategység továbbítási házirendjét és hello kulcs kézbesítési házirend-beállításként.</span><span class="sxs-lookup"><span data-stu-id="7deb2-153">You generate hello iv, and put it in both places: hello asset delivery policy and hello key delivery policy option.</span></span>
  * <span data-ttu-id="7deb2-154">**Kérje meg**: Ez a kulcs érkezik hello hitelesítő hello Apple Developer portálon keresztül generálásakor.</span><span class="sxs-lookup"><span data-stu-id="7deb2-154">**ASK**: This key is received when you generate hello certification by using hello Apple Developer portal.</span></span> <span data-ttu-id="7deb2-155">Minden egyes fejlesztői csapat egyedi KÉRJEN fog kapni.</span><span class="sxs-lookup"><span data-stu-id="7deb2-155">Each development team will receive a unique ASK.</span></span> <span data-ttu-id="7deb2-156">Hello KÉRJEN másolatának mentése, és tárolja biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="7deb2-156">Save a copy of hello ASK, and store it in a safe place.</span></span> <span data-ttu-id="7deb2-157">Később szüksége lesz tooconfigure KÉRJEN FairPlayAsk tooMedia szolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="7deb2-157">You will need tooconfigure ASK as FairPlayAsk tooMedia Services later.</span></span>
  * <span data-ttu-id="7deb2-158">**Kérje meg azonosító**: Ez az azonosító kapjuk meg, kérje meg a Media Services feltöltésekor.</span><span class="sxs-lookup"><span data-stu-id="7deb2-158">**ASK ID**: This ID is obtained when you upload ASK into Media Services.</span></span> <span data-ttu-id="7deb2-159">Kérje meg a hello kell feltöltenie **ContentKeyType.FairPlayAsk** számbavételi érték.</span><span class="sxs-lookup"><span data-stu-id="7deb2-159">You must upload ASK by using hello **ContentKeyType.FairPlayAsk** enum value.</span></span> <span data-ttu-id="7deb2-160">Hello, így hello Media Services Azonosítót ad vissza, és azt, hogy mit kell használni, amikor hello kulcs kézbesítési házirend beállítást.</span><span class="sxs-lookup"><span data-stu-id="7deb2-160">As hello result, hello Media Services ID is returned, and this is what should be used when setting hello key delivery policy option.</span></span>

<span data-ttu-id="7deb2-161">hello következőket kell beállítania a hello FPS ügyféloldali:</span><span class="sxs-lookup"><span data-stu-id="7deb2-161">hello following things must be set by hello FPS client side:</span></span>

  * <span data-ttu-id="7deb2-162">**Alkalmazás Cert (AC)**: egy.cer/.der fájl, amely hello nyilvános kulcsot, mely hello operációs rendszere tooencrypt néhány hasznos tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7deb2-162">**App Cert (AC)**: This is a .cer/.der file that contains hello public key, which hello operating system uses tooencrypt some payload.</span></span> <span data-ttu-id="7deb2-163">A Media Services tooknow információt kell, mert a hello player miatt szükséges.</span><span class="sxs-lookup"><span data-stu-id="7deb2-163">Media Services needs tooknow about it because it is required by hello player.</span></span> <span data-ttu-id="7deb2-164">hello kulcs kézbesítési szolgáltatás visszafejti hello megfelelő titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="7deb2-164">hello key delivery service decrypts it using hello corresponding private key.</span></span>

<span data-ttu-id="7deb2-165">tooplay biztonsági FairPlay titkosított adatfolyam, első lekérni egy valódi kérje meg, és majd a valódi tanúsítvány jön létre.</span><span class="sxs-lookup"><span data-stu-id="7deb2-165">tooplay back a FairPlay encrypted stream, get a real ASK first, and then generate a real certificate.</span></span> <span data-ttu-id="7deb2-166">A folyamat minden három részből hoz létre:</span><span class="sxs-lookup"><span data-stu-id="7deb2-166">That process creates all three parts:</span></span>

  * <span data-ttu-id="7deb2-167">.der fájl</span><span class="sxs-lookup"><span data-stu-id="7deb2-167">.der file</span></span>
  * <span data-ttu-id="7deb2-168">.pfx-fájlt</span><span class="sxs-lookup"><span data-stu-id="7deb2-168">.pfx file</span></span>
  * <span data-ttu-id="7deb2-169">hello .pfx jelszavát</span><span class="sxs-lookup"><span data-stu-id="7deb2-169">password for hello .pfx</span></span>

<span data-ttu-id="7deb2-170">hello következő ügyfelek támogatják a HLS **AES-128 CBC** titkosítási: OS X, az Apple TV IOS Safari böngésző.</span><span class="sxs-lookup"><span data-stu-id="7deb2-170">hello following clients support HLS with **AES-128 CBC** encryption: Safari on OS X, Apple TV, iOS.</span></span>

## <a name="configure-fairplay-dynamic-encryption-and-license-delivery-services"></a><span data-ttu-id="7deb2-171">FairPlay dinamikus titkosítás és licenc licenctovábbítási szolgáltatások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7deb2-171">Configure FairPlay dynamic encryption and license delivery services</span></span>
<span data-ttu-id="7deb2-172">Az alábbiakban hello általános lépéseket a FairPlay eszközök védelmének hello Media Services licenctovábbítási szolgáltatása segítségével, valamint a dinamikus titkosítás használatával.</span><span class="sxs-lookup"><span data-stu-id="7deb2-172">hello following are general steps for protecting your assets with FairPlay by using hello Media Services license delivery service, and also by using dynamic encryption.</span></span>

1. <span data-ttu-id="7deb2-173">Hozzon létre egy eszközt, és a fájlok feltöltése hello objektumba.</span><span class="sxs-lookup"><span data-stu-id="7deb2-173">Create an asset, and upload files into hello asset.</span></span>
2. <span data-ttu-id="7deb2-174">Hello eszköz, amely tartalmazza a hello fájl toohello adaptív sávszélességű MP4 típusú beállításkészlettel kódolása.</span><span class="sxs-lookup"><span data-stu-id="7deb2-174">Encode hello asset that contains hello file toohello adaptive bitrate MP4 set.</span></span>
3. <span data-ttu-id="7deb2-175">Hozzon létre egy tartalomkulcsot, és társítsa a kódolt hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="7deb2-175">Create a content key, and associate it with hello encoded asset.</span></span>  
4. <span data-ttu-id="7deb2-176">Hello tartalomkulcs hitelesítési szabályzatának konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7deb2-176">Configure hello content key’s authorization policy.</span></span> <span data-ttu-id="7deb2-177">Adja meg a hello következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="7deb2-177">Specify hello following:</span></span>

   * <span data-ttu-id="7deb2-178">hello kézbesítési módszert (ebben az esetben az FairPlay).</span><span class="sxs-lookup"><span data-stu-id="7deb2-178">hello delivery method (in this case, FairPlay).</span></span>
   * <span data-ttu-id="7deb2-179">FairPlay házirend-beállítások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7deb2-179">FairPlay policy options configuration.</span></span> <span data-ttu-id="7deb2-180">További részletekért tooconfigure FairPlay, lásd: hello **ConfigureFairPlayPolicyOptions()** hello minta az alábbi metódust.</span><span class="sxs-lookup"><span data-stu-id="7deb2-180">For details on how tooconfigure FairPlay, see hello **ConfigureFairPlayPolicyOptions()** method in hello sample below.</span></span>

     > [!NOTE]
     > <span data-ttu-id="7deb2-181">Általában érdemes választani tooconfigure FairPlay házirend beállításai csak egyszer, mert csak egy hitelesítő és egy KÉRJEN egy készletét van.</span><span class="sxs-lookup"><span data-stu-id="7deb2-181">Usually, you would want tooconfigure FairPlay policy options only once, because you will only have one set of a certification and an ASK.</span></span>
     >
     >
   * <span data-ttu-id="7deb2-182">Korlátozások (nyitott vagy token).</span><span class="sxs-lookup"><span data-stu-id="7deb2-182">Restrictions (open or token).</span></span>
   * <span data-ttu-id="7deb2-183">Információt adott toohello kulcs kézbesítési típust, amely meghatározza, hogyan hello kulcs toohello ügyfél kerül-e.</span><span class="sxs-lookup"><span data-stu-id="7deb2-183">Information specific toohello key delivery type that defines how hello key is delivered toohello client.</span></span>
5. <span data-ttu-id="7deb2-184">Hello objektumtovábbítási szabályzat konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7deb2-184">Configure hello asset delivery policy.</span></span> <span data-ttu-id="7deb2-185">hello továbbítási szabályzat konfigurációjához tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="7deb2-185">hello delivery policy configuration includes:</span></span>

   * <span data-ttu-id="7deb2-186">hello objektumtovábbítási protokoll (HLS).</span><span class="sxs-lookup"><span data-stu-id="7deb2-186">hello delivery protocol (HLS).</span></span>
   * <span data-ttu-id="7deb2-187">a dinamikus titkosítás (közös CBC titkosítás) hello típusa.</span><span class="sxs-lookup"><span data-stu-id="7deb2-187">hello type of dynamic encryption (common CBC encryption).</span></span>
   * <span data-ttu-id="7deb2-188">hello licenc licenckérési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="7deb2-188">hello license acquisition URL.</span></span>

     > [!NOTE]
     > <span data-ttu-id="7deb2-189">Ha azt szeretné, hogy toodeliver FairPlay és egy másik digitális tartalomvédelmi (DRM) rendszeren titkosított adatfolyam, tooconfigure külön továbbítási házirendjeit van:</span><span class="sxs-lookup"><span data-stu-id="7deb2-189">If you want toodeliver a stream that is encrypted with FairPlay and another Digital Rights Management (DRM) system, you have tooconfigure separate delivery policies:</span></span>
     >
     > * <span data-ttu-id="7deb2-190">Egy IAssetDeliveryPolicy tooconfigure dinamikus adaptív Streameléshez HTTP (DASH) a Common Encryption (CENC) (PlayReady + Widevine), és zökkenőmentes a Playreadyvel</span><span class="sxs-lookup"><span data-stu-id="7deb2-190">One IAssetDeliveryPolicy tooconfigure Dynamic Adaptive Streaming over HTTP (DASH) with Common Encryption (CENC) (PlayReady + Widevine), and Smooth with PlayReady</span></span>
     > * <span data-ttu-id="7deb2-191">Egy másik IAssetDeliveryPolicy tooconfigure a HLS FairPlay</span><span class="sxs-lookup"><span data-stu-id="7deb2-191">Another IAssetDeliveryPolicy tooconfigure FairPlay for HLS</span></span>
     >
     >
6. <span data-ttu-id="7deb2-192">Hozzon létre egy OnDemand-lokátor tooget egy adatfolyam-továbbítási URL-címet.</span><span class="sxs-lookup"><span data-stu-id="7deb2-192">Create an OnDemand locator tooget a streaming URL.</span></span>

## <a name="use-fairplay-key-delivery-by-player-apps"></a><span data-ttu-id="7deb2-193">FairPlay kulcs kézbesítési player alkalmazások használatát</span><span class="sxs-lookup"><span data-stu-id="7deb2-193">Use FairPlay key delivery by player apps</span></span>
<span data-ttu-id="7deb2-194">Player alkalmazásokat fejleszthet hello iOS SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="7deb2-194">You can develop player apps by using hello iOS SDK.</span></span> <span data-ttu-id="7deb2-195">toobe képes tooplay FairPlay tartalmat, hogy tooimplement hello licenc exchange protokoll.</span><span class="sxs-lookup"><span data-stu-id="7deb2-195">toobe able tooplay FairPlay content, you have tooimplement hello license exchange protocol.</span></span> <span data-ttu-id="7deb2-196">Ez a protokoll Apple nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="7deb2-196">This protocol is not specified by Apple.</span></span> <span data-ttu-id="7deb2-197">Már be tooeach alkalmazást hogyan toosend kulcs kézbesítési kéri.</span><span class="sxs-lookup"><span data-stu-id="7deb2-197">It is up tooeach app how toosend key delivery requests.</span></span> <span data-ttu-id="7deb2-198">Media Services FairPlay kulcs kézbesítési szolgáltatás hello hello SPC toocome www-form-url kódolt feladás egy vagy több üzenetet, a következő képernyő hello várja:</span><span class="sxs-lookup"><span data-stu-id="7deb2-198">hello Media Services FairPlay key delivery service expects hello SPC toocome as a www-form-url encoded post message, in hello following form:</span></span>

    spc=<Base64 encoded SPC>

> [!NOTE]
> <span data-ttu-id="7deb2-199">Az Azure Media Player nem támogatja a FairPlay lejátszás hello kezdő verzióról.</span><span class="sxs-lookup"><span data-stu-id="7deb2-199">Azure Media Player doesn’t support FairPlay playback out of hello box.</span></span> <span data-ttu-id="7deb2-200">MAC OS x, tooget FairPlay lejátszás hello minta player szerzi be hello Apple developer-fiók.</span><span class="sxs-lookup"><span data-stu-id="7deb2-200">tooget FairPlay playback on MAC OS X, obtain hello sample player from hello Apple developer account.</span></span>
>
>

## <a name="streaming-urls"></a><span data-ttu-id="7deb2-201">Adatfolyam-továbbítási URL-címek</span><span class="sxs-lookup"><span data-stu-id="7deb2-201">Streaming URLs</span></span>
<span data-ttu-id="7deb2-202">Ha az objektum egynél több DRM lett titkosítva, a streamelési URL-cím hello egy titkosítási címke használja: (formátum = "m3u8-aapl" titkosítási = "xxx").</span><span class="sxs-lookup"><span data-stu-id="7deb2-202">If your asset was encrypted with more than one DRM, you should use an encryption tag in hello streaming URL: (format='m3u8-aapl', encryption='xxx').</span></span>

<span data-ttu-id="7deb2-203">a következő szempontok hello vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="7deb2-203">hello following considerations apply:</span></span>

* <span data-ttu-id="7deb2-204">Csak nulla vagy egy titkosítási típus adható meg.</span><span class="sxs-lookup"><span data-stu-id="7deb2-204">Only zero or one encryption type can be specified.</span></span>
* <span data-ttu-id="7deb2-205">hello titkosítási típus toobe hello URL-címben megadott, ha csak egy titkosítási lett alkalmazott toohello eszköz nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7deb2-205">hello encryption type doesn't have toobe specified in hello URL if only one encryption was applied toohello asset.</span></span>
* <span data-ttu-id="7deb2-206">hello titkosítási típus-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="7deb2-206">hello encryption type is case insensitive.</span></span>
* <span data-ttu-id="7deb2-207">a következő típusú titkosítás hello adható meg:</span><span class="sxs-lookup"><span data-stu-id="7deb2-207">hello following encryption types can be specified:</span></span>  
  * <span data-ttu-id="7deb2-208">**cenc**: Common encryption (PlayReady vagy Widevine)</span><span class="sxs-lookup"><span data-stu-id="7deb2-208">**cenc**:  Common encryption (PlayReady or Widevine)</span></span>
  * <span data-ttu-id="7deb2-209">**cbcs-aapl**: FairPlay</span><span class="sxs-lookup"><span data-stu-id="7deb2-209">**cbcs-aapl**: FairPlay</span></span>
  * <span data-ttu-id="7deb2-210">**CBC**: AES envelope titkosítás</span><span class="sxs-lookup"><span data-stu-id="7deb2-210">**cbc**: AES envelope encryption</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="7deb2-211">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7deb2-211">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="7deb2-212">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="7deb2-212">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="7deb2-213">Adja hozzá a következő elemek túl hello**appSettings** az app.config fájlban meghatározott:</span><span class="sxs-lookup"><span data-stu-id="7deb2-213">Add hello following elements too**appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a><span data-ttu-id="7deb2-214">Példa</span><span class="sxs-lookup"><span data-stu-id="7deb2-214">Example</span></span>

<span data-ttu-id="7deb2-215">a következő minta hello hello képességét toouse Media Services toodeliver FairPlay titkosított tartalom mutatja be.</span><span class="sxs-lookup"><span data-stu-id="7deb2-215">hello following sample demonstrates hello ability toouse Media Services toodeliver your content encrypted with FairPlay.</span></span> <span data-ttu-id="7deb2-216">Ez a funkció a .NET-hez verzió 3.6.0 bevezetett hello Azure Media Services SDK-t.</span><span class="sxs-lookup"><span data-stu-id="7deb2-216">This functionality was introduced in hello Azure Media Services SDK for .NET version 3.6.0.</span></span> 

<span data-ttu-id="7deb2-217">Írja felül a Program.cs fájlban hello kód hello kód itt látható.</span><span class="sxs-lookup"><span data-stu-id="7deb2-217">Overwrite hello code in your Program.cs file with hello code shown in this section.</span></span>

>[!NOTE]
><span data-ttu-id="7deb2-218">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="7deb2-218">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="7deb2-219">Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek.</span><span class="sxs-lookup"><span data-stu-id="7deb2-219">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="7deb2-220">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="7deb2-220">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="7deb2-221">Győződjön meg arról, hogy tooupdate változók toopoint toofolders ahol a bemeneti fájlok találhatók.</span><span class="sxs-lookup"><span data-stu-id="7deb2-221">Make sure tooupdate variables toopoint toofolders where your input files are located.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
    using Newtonsoft.Json;
    using System.Security.Cryptography.X509Certificates;

    namespace DynamicEncryptionWithFairPlay
    {
        class Program
        {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        private static readonly Uri _sampleAudience =
            new Uri(ConfigurationManager.AppSettings["Audience"]);

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            bool tokenRestriction = false;
            string tokenTemplateString = null;

            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
            Console.WriteLine("Uploaded asset: {0}", asset.Id);

            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);

            IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
            Console.WriteLine();

            if (tokenRestriction)
            tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
            else
            AddOpenAuthorizationPolicy(key);

            Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
            Console.WriteLine();

            CreateAssetDeliveryPolicy(encodedAsset, key);
            Console.WriteLine("Created asset delivery policy. \n");
            Console.WriteLine();

            if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
            {
            // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
            // back into a TokenRestrictionTemplate class instance.
            TokenRestrictionTemplate tokenTemplate =
                TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

            // Generate a test token based on hello hello data in hello given TokenRestrictionTemplate.
            // Note, you need toopass hello key id Guid because we specified
            // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                        DateTime.UtcNow.AddDays(365));
            Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);

            Console.ReadLine();
        }

        static public IAsset UploadFileAndCreateAsset(string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
            Console.WriteLine("File does not exist.");
            return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
        {
            var encodingPreset = "Adaptive Streaming";

            IJob job = _context.Jobs.Create(String.Format("Encoding {0}", inputAsset.Name));

            var mediaProcessors =
            _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();

            var latestMediaProcessor =
            mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();

            ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
            encodeTask.InputAssets.Add(inputAsset);
            encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }

        static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
        {
            // Create HLS SAMPLE AES encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryptionCbcs);

            // Associate hello key with hello asset.
            asset.ContentKeys.Add(key);

            return key;
        }


        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {
            // Create ContentKeyAuthorizationPolicy with Open restrictions
            // and create authorization policy          

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                    new ContentKeyAuthorizationPolicyRestriction
                    {
                        Name = "Open",
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                        Requirements = null
                    }
                    };


            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();

            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
            ContentKeyDeliveryType.FairPlay,
            restrictions,
            FairPlayConfiguration);


            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate hello content key authorization policy with hello content key.
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                    new ContentKeyAuthorizationPolicyRestriction
                    {
                        Name = "Token Authorization Policy",
                        KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                        Requirements = tokenTemplateString,
                    }
                    };

            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();


            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                   ContentKeyDeliveryType.FairPlay,
                   restrictions,
                   FairPlayConfiguration);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate hello content key authorization policy with hello content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        private static string ConfigureFairPlayPolicyOptions()
        {
            // For testing you can provide all zeroes for ASK bytes together with hello cert from Apple FPS SDK.
            // However, for production you must use a real ASK from Apple bound tooa real prod certificate.
            byte[] askBytes = Guid.NewGuid().ToByteArray();
            var askId = Guid.NewGuid();
            // Key delivery retrieves askKey by askId and uses this key toogenerate hello response.
            IContentKey askKey = _context.ContentKeys.Create(
                        askId,
                        askBytes,
                        "askKey",
                        ContentKeyType.FairPlayASk);

            //Customer password for creating hello .pfx file.
            string pfxPassword = "<customer password for creating hello .pfx file>";
            // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key toogenerate hello response.
            var pfxPasswordId = Guid.NewGuid();
            byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
            IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                        pfxPasswordId,
                        pfxPasswordBytes,
                        "pfxPasswordKey",
                        ContentKeyType.FairPlayPfxPassword);

            // iv - 16 bytes random value, must match hello iv in hello asset delivery policy.
            byte[] iv = Guid.NewGuid().ToByteArray();

            //Specify hello .pfx file created by hello customer.
            var appCert = new X509Certificate2("path toohello .pfx file created by hello customer", pfxPassword, X509KeyStorageFlags.Exportable);

            string FairPlayConfiguration =
            Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                appCert,
                pfxPassword,
                pfxPasswordId,
                askId,
                iv);

            return FairPlayConfiguration;
        }

        static private string GenerateTokenRequirements()
        {
            TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

            template.PrimaryVerificationKey = new SymmetricVerificationKey();
            template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
            template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

            return TokenRestrictionTemplateSerializer.Serialize(template);
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();

            var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);

            FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);

            // Get hello FairPlay license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);

            // hello reason hello below code replaces "https://" with "skd://" is because
            // in hello IOS player sample code which you obtained in Apple developer account,
            // hello player only recognizes a Key URL that starts with skd://.
            // However, if you are using a customized player,
            // you can choose whatever protocol you want.
            // For example, "https".

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                    {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
            };

            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
            assetDeliveryPolicyConfiguration);

            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }


        /// <summary>
        /// Gets hello streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference toohello streaming manifest file from hello  
            // collection of files in hello asset.

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator toohello streaming content on an origin.
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL toohello manifest file.
            return originLocator.Path + assetFile.Name;
        }

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
        }

        static private byte[] GetRandomBuffer(int length)
        {
            var returnValue = new byte[length];

            using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
            {
            rng.GetBytes(returnValue);
            }

            return returnValue;
        }
        }
    }


## <a name="next-steps-media-services-learning-paths"></a><span data-ttu-id="7deb2-222">Következő lépések: Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="7deb2-222">Next steps: Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7deb2-223">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="7deb2-223">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
