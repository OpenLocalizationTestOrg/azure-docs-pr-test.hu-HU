---
title: "Tartalom a Microsoft PlayReady vagy Apple FairPlay - Azure HLS védelme |} Microsoft Docs"
description: "Ez a témakör áttekintést nyújt, és dinamikusan titkosítani a HTTP-Live Streaming (HLS) Apple FairPlay az Azure Media Services használatát ismerteti. Azt is bemutatja, hogyan használja a Media Services licenctovábbítási szolgáltatása képes biztosítani a FairPlay-licenc ügyfelek számára."
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
ms.openlocfilehash: 895d6307b1cef74e195cc2ffd8dbef4196e97b1f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="protect-your-hls-content-with-apple-fairplay-or-microsoft-playready"></a><span data-ttu-id="50231-104">A tartalom Apple FairPlay vagy a Microsoft PlayReady HLS védelme</span><span class="sxs-lookup"><span data-stu-id="50231-104">Protect your HLS content with Apple FairPlay or Microsoft PlayReady</span></span>
<span data-ttu-id="50231-105">Az Azure Media Services lehetővé teszi, hogy dinamikusan titkosítani a HTTP-Live Streaming (HLS) a következő formátum használatával:</span><span class="sxs-lookup"><span data-stu-id="50231-105">Azure Media Services enables you to dynamically encrypt your HTTP Live Streaming (HLS) content by using the following formats:</span></span>  

* <span data-ttu-id="50231-106">**AES-128 boríték tiszta kulcsot**</span><span class="sxs-lookup"><span data-stu-id="50231-106">**AES-128 envelope clear key**</span></span>

    <span data-ttu-id="50231-107">A teljes rendszer használatával titkosítja a **AES-128 CBC** mód.</span><span class="sxs-lookup"><span data-stu-id="50231-107">The entire chunk is encrypted by using the **AES-128 CBC** mode.</span></span> <span data-ttu-id="50231-108">Az adatfolyam visszafejtése natív módon támogatott iOS és OS X-lejátszót.</span><span class="sxs-lookup"><span data-stu-id="50231-108">The decryption of the stream is supported by iOS and OS X player natively.</span></span> <span data-ttu-id="50231-109">További információkért lásd: [AES-128 használata a dinamikus titkosítás és a kulcs kézbesítési szolgáltatás](media-services-protect-with-aes128.md).</span><span class="sxs-lookup"><span data-stu-id="50231-109">For more information, see [Using AES-128 dynamic encryption and key delivery service](media-services-protect-with-aes128.md).</span></span>
* <span data-ttu-id="50231-110">**Apple FairPlay**</span><span class="sxs-lookup"><span data-stu-id="50231-110">**Apple FairPlay**</span></span>

    <span data-ttu-id="50231-111">Az egyes video- és minták segítségével lettek titkosítva a **AES-128 CBC** mód.</span><span class="sxs-lookup"><span data-stu-id="50231-111">The individual video and audio samples are encrypted by using the **AES-128 CBC** mode.</span></span> <span data-ttu-id="50231-112">**FairPlay Streaming** (FPS) integrálva van az eszköz-operációsrendszerek, iOS és az Apple TV natív támogatását.</span><span class="sxs-lookup"><span data-stu-id="50231-112">**FairPlay Streaming** (FPS) is integrated into the device operating systems, with native support on iOS and Apple TV.</span></span> <span data-ttu-id="50231-113">OS x Safari FPS használatával a titkosított adathordozó-bővítmények (EME) felület támogatása lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="50231-113">Safari on OS X enables FPS by using the Encrypted Media Extensions (EME) interface support.</span></span>
* <span data-ttu-id="50231-114">**Microsoft PlayReady**</span><span class="sxs-lookup"><span data-stu-id="50231-114">**Microsoft PlayReady**</span></span>

<span data-ttu-id="50231-115">Az alábbi képen látható a **HLS + FairPlay vagy PlayReady a dinamikus titkosítás** munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="50231-115">The following image shows the **HLS + FairPlay or PlayReady dynamic encryption** workflow.</span></span>

![A dinamikus titkosítás munkafolyamat diagramja](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

<span data-ttu-id="50231-117">Ez a témakör bemutatja, hogyan használja a Media Services dinamikus titkosítást az Apple FairPlay HLS tartalmaihoz.</span><span class="sxs-lookup"><span data-stu-id="50231-117">This topic demonstrates how to use Media Services to dynamically encrypt your HLS content with Apple FairPlay.</span></span> <span data-ttu-id="50231-118">Azt is bemutatja, hogyan használja a Media Services licenctovábbítási szolgáltatása képes biztosítani a FairPlay-licenc ügyfelek számára.</span><span class="sxs-lookup"><span data-stu-id="50231-118">It also shows how to use the Media Services license delivery service to deliver FairPlay licenses to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="50231-119">Ha szeretné titkosítani a HLS a PlayReady, szeretné hozzon létre egy közös tartalomkulcsot, és rendelje hozzá azt az objektumot.</span><span class="sxs-lookup"><span data-stu-id="50231-119">If you also want to encrypt your HLS content with PlayReady, you need to create a common content key and associate it with your asset.</span></span> <span data-ttu-id="50231-120">Azt is konfigurálnia kell a tartalomkulcs hitelesítési szabályzatának, a [segítségével PlayReady a dynamic common encryption](media-services-protect-with-drm.md).</span><span class="sxs-lookup"><span data-stu-id="50231-120">You also need to configure the content key’s authorization policy, as described in [Using PlayReady dynamic common encryption](media-services-protect-with-drm.md).</span></span>
>
>

## <a name="requirements-and-considerations"></a><span data-ttu-id="50231-121">Követelmények és szempontok</span><span class="sxs-lookup"><span data-stu-id="50231-121">Requirements and considerations</span></span>

<span data-ttu-id="50231-122">A következőkre szükség FairPlay titkosított HLS továbbítására, és képes biztosítani a FairPlay-licenc Media Services használata közben:</span><span class="sxs-lookup"><span data-stu-id="50231-122">The following are required when using Media Services to deliver HLS encrypted with FairPlay, and to deliver FairPlay licenses:</span></span>

  * <span data-ttu-id="50231-123">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="50231-123">An Azure account.</span></span> <span data-ttu-id="50231-124">További részletek: [Ingyenes Azure-próbafiók](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="50231-124">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
  * <span data-ttu-id="50231-125">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="50231-125">A Media Services account.</span></span> <span data-ttu-id="50231-126">Szeretne létrehozni egyet, lásd: [hozzon létre egy Azure Media Services-fiók az Azure portál használatával](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="50231-126">To create one, see [Create an Azure Media Services account using the Azure portal](media-services-portal-create-account.md).</span></span>
  * <span data-ttu-id="50231-127">A regisztráció [Apple fejlesztési Program](https://developer.apple.com/).</span><span class="sxs-lookup"><span data-stu-id="50231-127">Sign up with [Apple Development Program](https://developer.apple.com/).</span></span>
  * <span data-ttu-id="50231-128">Apple szükséges a tartalom tulajdonosa az beszerzése a [központi telepítési csomag](https://developer.apple.com/contact/fps/).</span><span class="sxs-lookup"><span data-stu-id="50231-128">Apple requires the content owner to obtain the [deployment package](https://developer.apple.com/contact/fps/).</span></span> <span data-ttu-id="50231-129">Adja meg, hogy már megvalósította a kulcs biztonsági modul (KSM) a Media Services, és a végső FPS csomag kérelmet.</span><span class="sxs-lookup"><span data-stu-id="50231-129">State that you already implemented Key Security Module (KSM) with Media Services, and that you are requesting the final FPS package.</span></span> <span data-ttu-id="50231-130">A végső FPS csomagban hitelesítésszolgáltató létrehozása, és szerezze be a titkos kulcs (?) utasításokat is.</span><span class="sxs-lookup"><span data-stu-id="50231-130">There are instructions in the final FPS package to generate certification and obtain the Application Secret Key (ASK).</span></span> <span data-ttu-id="50231-131">Kérje meg FairPlay konfigurálására használt.</span><span class="sxs-lookup"><span data-stu-id="50231-131">You use ASK to configure FairPlay.</span></span>
  * <span data-ttu-id="50231-132">Az Azure Media Services .NET SDK-verzió **3.6.0** vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="50231-132">Azure Media Services .NET SDK version **3.6.0** or later.</span></span>

<span data-ttu-id="50231-133">A következő műveleteket kell állítani a Media Services kulcs kézbesítési oldalon:</span><span class="sxs-lookup"><span data-stu-id="50231-133">The following things must be set on Media Services key delivery side:</span></span>

  * <span data-ttu-id="50231-134">**Alkalmazás Cert (AC)**: egy .pfx-fájl, amely tartalmazza a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="50231-134">**App Cert (AC)**: This is a .pfx file that contains the private key.</span></span> <span data-ttu-id="50231-135">A fájl létrehozásához, és a titkosítás jelszóval.</span><span class="sxs-lookup"><span data-stu-id="50231-135">You create this file and encrypt it with a password.</span></span>

       <span data-ttu-id="50231-136">A kulcs továbbítási szabályzatban konfigurálásakor meg kell adnia, hogy a jelszó és a .pfx fájl Base64 formátumban.</span><span class="sxs-lookup"><span data-stu-id="50231-136">When you configure a key delivery policy, you must provide that password and the .pfx file in Base64 format.</span></span>

      <span data-ttu-id="50231-137">A következő lépések azt ismertetik, hogyan FairPlay .pfx tanúsítványfájl létrehozására:</span><span class="sxs-lookup"><span data-stu-id="50231-137">The following steps describe how to generate a .pfx certificate file for FairPlay:</span></span>

    1. <span data-ttu-id="50231-138">A https://slproweb.com/products/Win32OpenSSL.html OpenSSL telepíteni.</span><span class="sxs-lookup"><span data-stu-id="50231-138">Install OpenSSL from https://slproweb.com/products/Win32OpenSSL.html.</span></span>

        <span data-ttu-id="50231-139">Nyissa meg azt a mappát, amelyben a FairPlay tanúsítvány és egyéb Apple kézbesítette fájlokat is.</span><span class="sxs-lookup"><span data-stu-id="50231-139">Go to the folder where the FairPlay certificate and other files delivered by Apple are.</span></span>
    2. <span data-ttu-id="50231-140">Futtassa az alábbi parancsot a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="50231-140">Run the following command from the command line.</span></span> <span data-ttu-id="50231-141">A .cer fájl konvertál egy .pem fájlt.</span><span class="sxs-lookup"><span data-stu-id="50231-141">This converts the .cer file to a .pem file.</span></span>

        <span data-ttu-id="50231-142">"C:\OpenSSL-Win32\bin\openssl.exe" x509-der tájékoztatja-a fairplay.cer-fairplay-out.pem kimenő</span><span class="sxs-lookup"><span data-stu-id="50231-142">"C:\OpenSSL-Win32\bin\openssl.exe" x509 -inform der -in fairplay.cer -out fairplay-out.pem</span></span>
    3. <span data-ttu-id="50231-143">Futtassa az alábbi parancsot a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="50231-143">Run the following command from the command line.</span></span> <span data-ttu-id="50231-144">A titkos kulcsot tartalmazó .pfx fájlba alakítja a .pem fájl.</span><span class="sxs-lookup"><span data-stu-id="50231-144">This converts the .pem file to a .pfx file with the private key.</span></span> <span data-ttu-id="50231-145">OpenSSL majd felkéri, a .pfx fájlhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="50231-145">The password for the .pfx file is then asked by OpenSSL.</span></span>

        <span data-ttu-id="50231-146">"C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-exportálása - kimenő fairplay-out.pfx-inkey privatekey.pem-fairplay-out.pem - passin file:privatekey-pem-pass.txt a</span><span class="sxs-lookup"><span data-stu-id="50231-146">"C:\OpenSSL-Win32\bin\openssl.exe" pkcs12 -export -out fairplay-out.pfx -inkey privatekey.pem -in fairplay-out.pem -passin file:privatekey-pem-pass.txt</span></span>
  * <span data-ttu-id="50231-147">**Alkalmazás Cert jelszó**: A jelszót a .pfx fájl létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="50231-147">**App Cert password**: The password for creating the .pfx file.</span></span>
  * <span data-ttu-id="50231-148">**Alkalmazás Cert jelszó azonosítója**: a jelszó, hogyan azok feltölteni más Media Services kulcsok hasonló kell feltöltenie.</span><span class="sxs-lookup"><span data-stu-id="50231-148">**App Cert password ID**: You must upload the password, similar to how they upload other Media Services keys.</span></span> <span data-ttu-id="50231-149">Használja a **ContentKeyType.FairPlayPfxPassword** Felsorolásérték lekérni a Media Services-azonosító.</span><span class="sxs-lookup"><span data-stu-id="50231-149">Use the **ContentKeyType.FairPlayPfxPassword** enum value to get the Media Services ID.</span></span> <span data-ttu-id="50231-150">Ez akkor szükséges a kulcs kézbesítési házirend-beállításként belül használja.</span><span class="sxs-lookup"><span data-stu-id="50231-150">This is what they need to use inside the key delivery policy option.</span></span>
  * <span data-ttu-id="50231-151">**IV**: egy véletlenszerű érték 16 bájt.</span><span class="sxs-lookup"><span data-stu-id="50231-151">**iv**: This is a random value of 16 bytes.</span></span> <span data-ttu-id="50231-152">Meg kell egyeznie a iv az adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="50231-152">It must match the iv in the asset delivery policy.</span></span> <span data-ttu-id="50231-153">Ön hozza létre a iv, és mindkét helyen elhelyezi: az adategység továbbítási házirendjét és a kulcs kézbesítési házirend-beállításként.</span><span class="sxs-lookup"><span data-stu-id="50231-153">You generate the iv, and put it in both places: the asset delivery policy and the key delivery policy option.</span></span>
  * <span data-ttu-id="50231-154">**Kérje meg**: Ez a kulcs érkezik, ha a hitelesítésszolgáltató létrehozásakor az Apple fejlesztői portáljáról használatával.</span><span class="sxs-lookup"><span data-stu-id="50231-154">**ASK**: This key is received when you generate the certification by using the Apple Developer portal.</span></span> <span data-ttu-id="50231-155">Minden egyes fejlesztői csapat egyedi KÉRJEN fog kapni.</span><span class="sxs-lookup"><span data-stu-id="50231-155">Each development team will receive a unique ASK.</span></span> <span data-ttu-id="50231-156">A KÉRJEN másolatának mentése, és tárolja biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="50231-156">Save a copy of the ASK, and store it in a safe place.</span></span> <span data-ttu-id="50231-157">Szüksége lesz, kérje meg a Media Services később FairPlayAsk konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="50231-157">You will need to configure ASK as FairPlayAsk to Media Services later.</span></span>
  * <span data-ttu-id="50231-158">**Kérje meg azonosító**: Ez az azonosító kapjuk meg, kérje meg a Media Services feltöltésekor.</span><span class="sxs-lookup"><span data-stu-id="50231-158">**ASK ID**: This ID is obtained when you upload ASK into Media Services.</span></span> <span data-ttu-id="50231-159">Kérje meg a kell feltöltenie az **ContentKeyType.FairPlayAsk** számbavételi érték.</span><span class="sxs-lookup"><span data-stu-id="50231-159">You must upload ASK by using the **ContentKeyType.FairPlayAsk** enum value.</span></span> <span data-ttu-id="50231-160">Ennek eredményeképpen a Media Services Azonosítót ad vissza, és azt kell használni, ha a beállítás a kulcs kézbesítési házirend-beállításként.</span><span class="sxs-lookup"><span data-stu-id="50231-160">As the result, the Media Services ID is returned, and this is what should be used when setting the key delivery policy option.</span></span>

<span data-ttu-id="50231-161">A következő műveleteket kell beállítania a FPS ügyféloldali:</span><span class="sxs-lookup"><span data-stu-id="50231-161">The following things must be set by the FPS client side:</span></span>

  * <span data-ttu-id="50231-162">**Alkalmazás Cert (AC)**: egy.cer/.der fájl, amely tartalmazza a nyilvános kulcsot, amely néhány hasznos titkosítására használja az operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="50231-162">**App Cert (AC)**: This is a .cer/.der file that contains the public key, which the operating system uses to encrypt some payload.</span></span> <span data-ttu-id="50231-163">A Media Services kell tudnia, vagy mert a Windows Media Player van szükség.</span><span class="sxs-lookup"><span data-stu-id="50231-163">Media Services needs to know about it because it is required by the player.</span></span> <span data-ttu-id="50231-164">A kulcs kézbesítési szolgáltatás visszafejti a megfelelő titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="50231-164">The key delivery service decrypts it using the corresponding private key.</span></span>

<span data-ttu-id="50231-165">FairPlay titkosított adatfolyam lejátszását, első lekérni egy valódi kérje meg, és majd a valódi tanúsítvány jön létre.</span><span class="sxs-lookup"><span data-stu-id="50231-165">To play back a FairPlay encrypted stream, get a real ASK first, and then generate a real certificate.</span></span> <span data-ttu-id="50231-166">A folyamat minden három részből hoz létre:</span><span class="sxs-lookup"><span data-stu-id="50231-166">That process creates all three parts:</span></span>

  * <span data-ttu-id="50231-167">.der fájl</span><span class="sxs-lookup"><span data-stu-id="50231-167">.der file</span></span>
  * <span data-ttu-id="50231-168">.pfx-fájlt</span><span class="sxs-lookup"><span data-stu-id="50231-168">.pfx file</span></span>
  * <span data-ttu-id="50231-169">a .pfx jelszavát</span><span class="sxs-lookup"><span data-stu-id="50231-169">password for the .pfx</span></span>

<span data-ttu-id="50231-170">A következő ügyfelek támogatják a HLS **AES-128 CBC** titkosítási: OS X, az Apple TV IOS Safari böngésző.</span><span class="sxs-lookup"><span data-stu-id="50231-170">The following clients support HLS with **AES-128 CBC** encryption: Safari on OS X, Apple TV, iOS.</span></span>

## <a name="configure-fairplay-dynamic-encryption-and-license-delivery-services"></a><span data-ttu-id="50231-171">FairPlay dinamikus titkosítás és licenc licenctovábbítási szolgáltatások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="50231-171">Configure FairPlay dynamic encryption and license delivery services</span></span>
<span data-ttu-id="50231-172">Az alábbi lépések általános FairPlay a eszközök védelmére, a Media Services licenctovábbítási szolgáltatása segítségével, valamint a dinamikus titkosítás használatával.</span><span class="sxs-lookup"><span data-stu-id="50231-172">The following are general steps for protecting your assets with FairPlay by using the Media Services license delivery service, and also by using dynamic encryption.</span></span>

1. <span data-ttu-id="50231-173">Hozzon létre egy eszközt, és a fájlok feltöltése az objektumba.</span><span class="sxs-lookup"><span data-stu-id="50231-173">Create an asset, and upload files into the asset.</span></span>
2. <span data-ttu-id="50231-174">Az adaptív sávszélességű MP4 típusú beállításkészlettel fájlt tartalmazó objektum kódolása.</span><span class="sxs-lookup"><span data-stu-id="50231-174">Encode the asset that contains the file to the adaptive bitrate MP4 set.</span></span>
3. <span data-ttu-id="50231-175">Hozzon létre egy tartalomkulcsot, és társítsa a kódolt objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="50231-175">Create a content key, and associate it with the encoded asset.</span></span>  
4. <span data-ttu-id="50231-176">Konfigurálja a tartalomkulcs hitelesítési szabályzatát.</span><span class="sxs-lookup"><span data-stu-id="50231-176">Configure the content key’s authorization policy.</span></span> <span data-ttu-id="50231-177">Adja meg az alábbiakat:</span><span class="sxs-lookup"><span data-stu-id="50231-177">Specify the following:</span></span>

   * <span data-ttu-id="50231-178">A kézbesítési módszert (ebben az esetben az FairPlay).</span><span class="sxs-lookup"><span data-stu-id="50231-178">The delivery method (in this case, FairPlay).</span></span>
   * <span data-ttu-id="50231-179">FairPlay házirend-beállítások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="50231-179">FairPlay policy options configuration.</span></span> <span data-ttu-id="50231-180">FairPlay konfigurálásával kapcsolatos részletekért lásd: a **ConfigureFairPlayPolicyOptions()** módszer az alábbi minta.</span><span class="sxs-lookup"><span data-stu-id="50231-180">For details on how to configure FairPlay, see the **ConfigureFairPlayPolicyOptions()** method in the sample below.</span></span>

     > [!NOTE]
     > <span data-ttu-id="50231-181">Általában célszerű FairPlay házirend beállításainak konfigurálása csak egyszer, mert csak egy hitelesítő és egy KÉRJEN egy készletét van.</span><span class="sxs-lookup"><span data-stu-id="50231-181">Usually, you would want to configure FairPlay policy options only once, because you will only have one set of a certification and an ASK.</span></span>
     >
     >
   * <span data-ttu-id="50231-182">Korlátozások (nyitott vagy token).</span><span class="sxs-lookup"><span data-stu-id="50231-182">Restrictions (open or token).</span></span>
   * <span data-ttu-id="50231-183">A kulcs kézbesítési típust, amely meghatározza, hogyan a kulcs kézbesítve lenne az ügyfél-specifikus adatait.</span><span class="sxs-lookup"><span data-stu-id="50231-183">Information specific to the key delivery type that defines how the key is delivered to the client.</span></span>
5. <span data-ttu-id="50231-184">Konfigurálja az adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="50231-184">Configure the asset delivery policy.</span></span> <span data-ttu-id="50231-185">A továbbítási szabályzat konfigurációjához tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="50231-185">The delivery policy configuration includes:</span></span>

   * <span data-ttu-id="50231-186">A továbbítási protokoll (HLS).</span><span class="sxs-lookup"><span data-stu-id="50231-186">The delivery protocol (HLS).</span></span>
   * <span data-ttu-id="50231-187">A dinamikus titkosítás (közös CBC titkosítás) típusú.</span><span class="sxs-lookup"><span data-stu-id="50231-187">The type of dynamic encryption (common CBC encryption).</span></span>
   * <span data-ttu-id="50231-188">A licenc licenckérési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="50231-188">The license acquisition URL.</span></span>

     > [!NOTE]
     > <span data-ttu-id="50231-189">Ha azt szeretné, hogy FairPlay és egy másik digitális tartalomvédelmi (DRM) rendszeren titkosított adatfolyam, hogy külön továbbítási házirendjeit konfigurálnia:</span><span class="sxs-lookup"><span data-stu-id="50231-189">If you want to deliver a stream that is encrypted with FairPlay and another Digital Rights Management (DRM) system, you have to configure separate delivery policies:</span></span>
     >
     > * <span data-ttu-id="50231-190">Egy IAssetDeliveryPolicy konfigurálása a dinamikus adaptív Streameléshez HTTP (DASH) a Common Encryption (CENC) (PlayReady + Widevine), és a PlayReady Smooth keresztül</span><span class="sxs-lookup"><span data-stu-id="50231-190">One IAssetDeliveryPolicy to configure Dynamic Adaptive Streaming over HTTP (DASH) with Common Encryption (CENC) (PlayReady + Widevine), and Smooth with PlayReady</span></span>
     > * <span data-ttu-id="50231-191">Egy másik IAssetDeliveryPolicy HLS FairPlay konfigurálása</span><span class="sxs-lookup"><span data-stu-id="50231-191">Another IAssetDeliveryPolicy to configure FairPlay for HLS</span></span>
     >
     >
6. <span data-ttu-id="50231-192">Hozzon létre egy OnDemand-lokátort a streamelési URL-cím beszerzése.</span><span class="sxs-lookup"><span data-stu-id="50231-192">Create an OnDemand locator to get a streaming URL.</span></span>

## <a name="use-fairplay-key-delivery-by-player-apps"></a><span data-ttu-id="50231-193">FairPlay kulcs kézbesítési player alkalmazások használatát</span><span class="sxs-lookup"><span data-stu-id="50231-193">Use FairPlay key delivery by player apps</span></span>
<span data-ttu-id="50231-194">Az IOS SDK player alkalmazásokat fejleszthet.</span><span class="sxs-lookup"><span data-stu-id="50231-194">You can develop player apps by using the iOS SDK.</span></span> <span data-ttu-id="50231-195">Nem fogja tudni FairPlay tartalom lejátszása, be kell megvalósítani a licenc exchange protokoll.</span><span class="sxs-lookup"><span data-stu-id="50231-195">To be able to play FairPlay content, you have to implement the license exchange protocol.</span></span> <span data-ttu-id="50231-196">Ez a protokoll Apple nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="50231-196">This protocol is not specified by Apple.</span></span> <span data-ttu-id="50231-197">Esetén a minden alkalmazás kulcs kézbesítési kérelem küldése.</span><span class="sxs-lookup"><span data-stu-id="50231-197">It is up to each app how to send key delivery requests.</span></span> <span data-ttu-id="50231-198">A Media Services FairPlay kulcs kézbesítési szolgáltatás vár a SPC lesz üzenetként www-form-url kódolt feladás egy vagy több, a következő formában:</span><span class="sxs-lookup"><span data-stu-id="50231-198">The Media Services FairPlay key delivery service expects the SPC to come as a www-form-url encoded post message, in the following form:</span></span>

    spc=<Base64 encoded SPC>

> [!NOTE]
> <span data-ttu-id="50231-199">Az Azure Media Player FairPlay lejátszás kívül a mezőben nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="50231-199">Azure Media Player doesn’t support FairPlay playback out of the box.</span></span> <span data-ttu-id="50231-200">Ahhoz, hogy FairPlay lejátszás MAC OS x, szerezze be a minta player az Apple developer-fiók.</span><span class="sxs-lookup"><span data-stu-id="50231-200">To get FairPlay playback on MAC OS X, obtain the sample player from the Apple developer account.</span></span>
>
>

## <a name="streaming-urls"></a><span data-ttu-id="50231-201">Adatfolyam-továbbítási URL-címek</span><span class="sxs-lookup"><span data-stu-id="50231-201">Streaming URLs</span></span>
<span data-ttu-id="50231-202">Ha az objektum egynél több DRM lett titkosítva, egy titkosítási címke használjon az adatfolyam-továbbítási URL-cím: (formátum = "m3u8-aapl" titkosítási = "xxx").</span><span class="sxs-lookup"><span data-stu-id="50231-202">If your asset was encrypted with more than one DRM, you should use an encryption tag in the streaming URL: (format='m3u8-aapl', encryption='xxx').</span></span>

<span data-ttu-id="50231-203">A következők érvényesek:</span><span class="sxs-lookup"><span data-stu-id="50231-203">The following considerations apply:</span></span>

* <span data-ttu-id="50231-204">Csak nulla vagy egy titkosítási típus adható meg.</span><span class="sxs-lookup"><span data-stu-id="50231-204">Only zero or one encryption type can be specified.</span></span>
* <span data-ttu-id="50231-205">A titkosítási típus nem kell az URL-cím adható meg, ha csak egy titkosítási alkalmazta az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="50231-205">The encryption type doesn't have to be specified in the URL if only one encryption was applied to the asset.</span></span>
* <span data-ttu-id="50231-206">A titkosítási típus-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="50231-206">The encryption type is case insensitive.</span></span>
* <span data-ttu-id="50231-207">A következő titkosítási típusok adhatók meg:</span><span class="sxs-lookup"><span data-stu-id="50231-207">The following encryption types can be specified:</span></span>  
  * <span data-ttu-id="50231-208">**cenc**: Common encryption (PlayReady vagy Widevine)</span><span class="sxs-lookup"><span data-stu-id="50231-208">**cenc**:  Common encryption (PlayReady or Widevine)</span></span>
  * <span data-ttu-id="50231-209">**cbcs-aapl**: FairPlay</span><span class="sxs-lookup"><span data-stu-id="50231-209">**cbcs-aapl**: FairPlay</span></span>
  * <span data-ttu-id="50231-210">**CBC**: AES envelope titkosítás</span><span class="sxs-lookup"><span data-stu-id="50231-210">**cbc**: AES envelope encryption</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="50231-211">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="50231-211">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="50231-212">Állítsa be a fejlesztési környezetet, és töltse fel az app.config fájlt a kapcsolatadatokkal a [.NET-keretrendszerrel történő Media Services-fejlesztést](media-services-dotnet-how-to-use.md) ismertető dokumentumban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="50231-212">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="50231-213">Adja hozzá a következő elemeket az app.config fájlban megadott **appSettings** szakaszhoz:</span><span class="sxs-lookup"><span data-stu-id="50231-213">Add the following elements to **appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a><span data-ttu-id="50231-214">Példa</span><span class="sxs-lookup"><span data-stu-id="50231-214">Example</span></span>

<span data-ttu-id="50231-215">A következő példa bemutatja teszi a Media Services használatát a FairPlay titkosított tartalom.</span><span class="sxs-lookup"><span data-stu-id="50231-215">The following sample demonstrates the ability to use Media Services to deliver your content encrypted with FairPlay.</span></span> <span data-ttu-id="50231-216">Ez a funkció az Azure Media Services SDK bevezetett a .NET-hez 3.6.0 verziója.</span><span class="sxs-lookup"><span data-stu-id="50231-216">This functionality was introduced in the Azure Media Services SDK for .NET version 3.6.0.</span></span> 

<span data-ttu-id="50231-217">Írja felül a Program.cs fájlban található kódot az itt látható kóddal.</span><span class="sxs-lookup"><span data-stu-id="50231-217">Overwrite the code in your Program.cs file with the code shown in this section.</span></span>

>[!NOTE]
><span data-ttu-id="50231-218">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="50231-218">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="50231-219">Ha mindig ugyanazokat a napokat/hozzáférési engedélyeket használja (például olyan keresők szabályzatait, amelyek hosszú ideig érvényben maradnak, vagyis nem feltöltött szabályzatokat), a szabályzatazonosítónak is ugyanannak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="50231-219">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="50231-220">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="50231-220">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="50231-221">Módosítsa úgy a változókat, hogy a bemeneti fájlok tárolásához Ön által használt mappákra mutassanak.</span><span class="sxs-lookup"><span data-stu-id="50231-221">Make sure to update variables to point to folders where your input files are located.</span></span>

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
        // Read values from the App.config file.
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
            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
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

            // Generate a test token based on the the data in the given TokenRestrictionTemplate.
            // Note, you need to pass the key id Guid because we specified
            // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                        DateTime.UtcNow.AddDays(365));
            Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
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

            // Associate the key with the asset.
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

            // Associate the content key authorization policy with the content key.
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

            // Associate the content key authorization policy with the content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        private static string ConfigureFairPlayPolicyOptions()
        {
            // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK.
            // However, for production you must use a real ASK from Apple bound to a real prod certificate.
            byte[] askBytes = Guid.NewGuid().ToByteArray();
            var askId = Guid.NewGuid();
            // Key delivery retrieves askKey by askId and uses this key to generate the response.
            IContentKey askKey = _context.ContentKeys.Create(
                        askId,
                        askBytes,
                        "askKey",
                        ContentKeyType.FairPlayASk);

            //Customer password for creating the .pfx file.
            string pfxPassword = "<customer password for creating the .pfx file>";
            // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
            var pfxPasswordId = Guid.NewGuid();
            byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
            IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                        pfxPasswordId,
                        pfxPasswordBytes,
                        "pfxPasswordKey",
                        ContentKeyType.FairPlayPfxPassword);

            // iv - 16 bytes random value, must match the iv in the asset delivery policy.
            byte[] iv = Guid.NewGuid().ToByteArray();

            //Specify the .pfx file created by the customer.
            var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);

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

            // Get the FairPlay license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);

            // The reason the below code replaces "https://" with "skd://" is because
            // in the IOS player sample code which you obtained in Apple developer account,
            // the player only recognizes a Key URL that starts with skd://.
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

            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }


        /// <summary>
        /// Gets the streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference to the streaming manifest file from the  
            // collection of files in the asset.

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator to the streaming content on an origin.
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL to the manifest file.
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


## <a name="next-steps-media-services-learning-paths"></a><span data-ttu-id="50231-222">Következő lépések: Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="50231-222">Next steps: Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="50231-223">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="50231-223">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
