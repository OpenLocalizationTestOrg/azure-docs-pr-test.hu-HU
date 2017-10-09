---
title: "aaaUsing PlayReady és/vagy Widevine a dynamic common encryption |} Microsoft Docs"
description: "A Microsoft Azure Media Services lehetővé teszi, hogy Ön toodeliver MPEG-DASH, Smooth Streaming vagy Http-Live-Streaming (HLS) streamjeit Microsoft PlayReady DRM. Emellett lehetővé teszi a Widevine DRM-Védelemmel ellátott DASH toodelivery. Ez a témakör bemutatja, hogyan toodynamically titkosítása a PlayReady vagy Widevine DRM-Védelemmel."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 548d1a12-e2cb-45fe-9307-4ec0320567a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 0475e6ec80dcf39eb4e5c4ad4d17f821502951bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-playready-andor-widevine-dynamic-common-encryption"></a><span data-ttu-id="54ead-105">A PlayReady és/vagy Widevine Dynamic Common Encryption titkosítás használata</span><span class="sxs-lookup"><span data-stu-id="54ead-105">Using PlayReady and/or Widevine dynamic common encryption</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="54ead-106">.NET</span><span class="sxs-lookup"><span data-stu-id="54ead-106">.NET</span></span>](media-services-protect-with-drm.md)
> * [<span data-ttu-id="54ead-107">Java</span><span class="sxs-lookup"><span data-stu-id="54ead-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="54ead-108">PHP</span><span class="sxs-lookup"><span data-stu-id="54ead-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
>
>

<span data-ttu-id="54ead-109">A Microsoft Azure Media Services lehetővé teszi a toodeliver MPEG-DASH, Smooth Streaming vagy HTTP-Live-Streaming (HLS) streamjeit [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span><span class="sxs-lookup"><span data-stu-id="54ead-109">Microsoft Azure Media Services enables you toodeliver MPEG-DASH, Smooth Streaming, and HTTP-Live-Streaming (HLS) streams protected with [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span></span> <span data-ttu-id="54ead-110">Emellett lehetővé teszi a Widevine DRM-licencek toodeliver titkosított DASH-streameket.</span><span class="sxs-lookup"><span data-stu-id="54ead-110">It also enables you toodeliver encrypted DASH streams with Widevine DRM licenses.</span></span> <span data-ttu-id="54ead-111">Mind a PlayReady, mind a Widevine titkosítása hello Common Encryption (ISO/IEC 23001-7 CENC) megadását.</span><span class="sxs-lookup"><span data-stu-id="54ead-111">Both PlayReady and Widevine are encrypted per hello Common Encryption (ISO/IEC 23001-7 CENC) specification.</span></span> <span data-ttu-id="54ead-112">Használhat [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (3.5.1 hello verziójával kezdődően) vagy a REST API tooconfigure az AssetDeliveryConfiguration toouse Widevine.</span><span class="sxs-lookup"><span data-stu-id="54ead-112">You can use [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (starting with hello version 3.5.1) or REST API tooconfigure your AssetDeliveryConfiguration toouse Widevine.</span></span>

<span data-ttu-id="54ead-113">A Media Services része egy szolgáltatás, amelynek segítségével PlayReady vagy Widevine DRM-licenceket továbbíthat.</span><span class="sxs-lookup"><span data-stu-id="54ead-113">Media Services provides a service for delivering PlayReady and Widevine DRM licenses.</span></span> <span data-ttu-id="54ead-114">A Media Services is biztosít, amelyek lehetővé teszik a hello jogok konfigurálása API-k és korlátozásokat, amelyeket használni szeretne hello PlayReady vagy Widevine DRM futásidejű tooenforce felhasználó lejátssza védett tartalmakat.</span><span class="sxs-lookup"><span data-stu-id="54ead-114">Media Services also provides APIs that let you configure hello rights and restrictions that you want for hello PlayReady or Widevine DRM runtime tooenforce when a user plays back protected content.</span></span> <span data-ttu-id="54ead-115">Ha egy felhasználó egy DRM védett tartalmat igényel, hello lejátszóalkalmazás fog licencet kér hello AMS-licencelési szolgáltatástól.</span><span class="sxs-lookup"><span data-stu-id="54ead-115">When a user requests a DRM protected content, hello player application will request a license from hello AMS license service.</span></span> <span data-ttu-id="54ead-116">hello AMS-licencelési szolgáltatástól egy licenc toohello player állít ki, ha engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="54ead-116">hello AMS license service will issue a license toohello player if it is authorized.</span></span> <span data-ttu-id="54ead-117">A PlayReady vagy Widevine-licencek tartalmazza, amelyek segítségével hello ügyfél player toodecrypt és adatfolyam hello tartalom hello visszafejtési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="54ead-117">A PlayReady or Widevine license contains hello decryption key that can be used by hello client player toodecrypt and stream hello content.</span></span>

<span data-ttu-id="54ead-118">Használhatja a következő AMS-partnereket toohelp Widevine-licencek átadná hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="54ead-118">You can also use hello following AMS partners toohelp you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span> <span data-ttu-id="54ead-119">További információk: integráció az [Axinom](media-services-axinom-integration.md) és a [castLabs](media-services-castlabs-integration.md) rendszerekkel.</span><span class="sxs-lookup"><span data-stu-id="54ead-119">For more information, see: integration with [Axinom](media-services-axinom-integration.md) and [castLabs](media-services-castlabs-integration.md).</span></span>

<span data-ttu-id="54ead-120">A Media Services szolgáltatásban több különböző módot is beállíthat, amelyek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="54ead-120">Media Services supports multiple ways of authorizing users who make key requests.</span></span> <span data-ttu-id="54ead-121">hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: Nyissa meg, vagy a token korlátozás.</span><span class="sxs-lookup"><span data-stu-id="54ead-121">hello content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span> <span data-ttu-id="54ead-122">hello token korlátozott házirend által a Secure Token Service (STS) kiadott tokennek kell csatolni.</span><span class="sxs-lookup"><span data-stu-id="54ead-122">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="54ead-123">A Media Services hello támogatja a jogkivonatokat [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) formátumú és [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) formátumú.</span><span class="sxs-lookup"><span data-stu-id="54ead-123">Media Services supports tokens in hello [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) format and [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) format.</span></span> <span data-ttu-id="54ead-124">További információkért lásd: Configure hello tartalomkulcs engedélyezési házirendjét.</span><span class="sxs-lookup"><span data-stu-id="54ead-124">For more information, see Configure hello content key’s authorization policy.</span></span>

<span data-ttu-id="54ead-125">tootake előnye a dinamikus titkosítás toohave többszörös sávszélességű MP4-fájlok vagy többféle sávszélességű Smooth Streaming-forrásfájlokat tartalmazó objektumot kell.</span><span class="sxs-lookup"><span data-stu-id="54ead-125">tootake advantage of dynamic encryption, you need toohave an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="54ead-126">Szükség tooconfigure hello továbbítási házirendjeit hello eszköz (lásd a témakör későbbi részében).</span><span class="sxs-lookup"><span data-stu-id="54ead-126">You also need tooconfigure hello delivery policies for hello asset (described later in this topic).</span></span> <span data-ttu-id="54ead-127">Majd a streamelési URL-cím hello hello formátummegadás alapján, hello Igényalapú Streamelési kiszolgáló biztosítja, hogy hello adatfolyam a rendszer a kiválasztott hello protokollal.</span><span class="sxs-lookup"><span data-stu-id="54ead-127">Then, based on hello format specified in hello streaming URL, hello On-Demand Streaming server will ensure that hello stream is delivered in hello protocol you have chosen.</span></span> <span data-ttu-id="54ead-128">Ennek eredményeképpen csak akkor kell toostore és fizetési hello fájlokat egy egyetlen tárolási formátumban és a Media Services elkészíti és kiszolgálja hello ügyféltől érkező kéréseknek megfelelő HTTP-válasz.</span><span class="sxs-lookup"><span data-stu-id="54ead-128">As a result, you only need toostore and pay for hello files in a single storage format and Media Services will build and serve hello appropriate HTTP response based on each request from a client.</span></span>

<span data-ttu-id="54ead-129">Ez a témakör, amely többféle DRM, például PlayReady és Widevine-mel védett médiafájlok továbbításával foglalkoznak hasznos toodevelopers lenne.</span><span class="sxs-lookup"><span data-stu-id="54ead-129">This topic would be useful toodevelopers that work on applications that deliver media protected with multiple DRMs, such as PlayReady and Widevine.</span></span> <span data-ttu-id="54ead-130">hello a témakör bemutatja, hogyan tooconfigure hello-e PlayReady-licenctovábbítási szolgáltatásra vonatkozó szabályzatokat úgy, hogy csak arra jogosult ügyfelek kaphassák a PlayReady és Widevine-licenceket.</span><span class="sxs-lookup"><span data-stu-id="54ead-130">hello topic shows you how tooconfigure hello PlayReady license delivery service with authorization policies so that only authorized clients could receive PlayReady or Widevine licenses.</span></span> <span data-ttu-id="54ead-131">Azt is bemutatja, hogyan toouse dinamikus titkosítás funkciót a PlayReady vagy Widevine DRM-Védelemmel keresztüli kötőjel.</span><span class="sxs-lookup"><span data-stu-id="54ead-131">It also shows how toouse dynamic encryption encryption with PlayReady or Widevine DRM over DASH.</span></span>

>[!NOTE]
><span data-ttu-id="54ead-132">Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="54ead-132">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="54ead-133">a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="54ead-133">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 

## <a name="download-sample"></a><span data-ttu-id="54ead-134">Minta letöltése</span><span class="sxs-lookup"><span data-stu-id="54ead-134">Download sample</span></span>
<span data-ttu-id="54ead-135">Letöltheti a cikkben leírt hello mintát [Itt](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).</span><span class="sxs-lookup"><span data-stu-id="54ead-135">You can download hello sample described in this article from [here](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).</span></span>

## <a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a><span data-ttu-id="54ead-136">A Dynamic Common Encryption és DRM-licenctovábbítási szolgáltatások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="54ead-136">Configuring Dynamic Common Encryption and DRM License Delivery Services</span></span>

<span data-ttu-id="54ead-137">Az alábbiakban hello általános lépéseket, hogy kell tooperform a PlayReady, hello Media Services licenctovábbítási szolgáltatása segítségével, és a dinamikus titkosítás használata eszközök védelme esetén.</span><span class="sxs-lookup"><span data-stu-id="54ead-137">hello following are general steps that you would need tooperform when protecting your assets with PlayReady, using hello Media Services license delivery service, and also using dynamic encryption.</span></span>

1. <span data-ttu-id="54ead-138">Hozzon létre egy eszközt, majd fájlok feltöltése hello objektumba.</span><span class="sxs-lookup"><span data-stu-id="54ead-138">Create an asset and upload files into hello asset.</span></span>
2. <span data-ttu-id="54ead-139">Hello eszköz tartalmazó hello fájl toohello adaptív sávszélességű MP4 típusú beállításkészlettel kódolása.</span><span class="sxs-lookup"><span data-stu-id="54ead-139">Encode hello asset containing hello file toohello adaptive bitrate MP4 set.</span></span>
3. <span data-ttu-id="54ead-140">Hozzon létre egy tartalomkulcsot, majd társítsa a kódolt hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="54ead-140">Create a content key and associate it with hello encoded asset.</span></span> <span data-ttu-id="54ead-141">A Media Services szolgáltatásban a hello tartalomkulcs tartalmazza a hello objektum titkosítási kulcsát.</span><span class="sxs-lookup"><span data-stu-id="54ead-141">In Media Services, hello content key contains hello asset’s encryption key.</span></span>
4. <span data-ttu-id="54ead-142">Hello tartalomkulcs hitelesítési szabályzatának konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="54ead-142">Configure hello content key’s authorization policy.</span></span> <span data-ttu-id="54ead-143">hello tartalomkulcs-hitelesítési szabályzatot kell állította be, és ahhoz, hogy hello tartalom kulcs toobe kézbesített toohello ügyfél hello ügyfél teljesíti.</span><span class="sxs-lookup"><span data-stu-id="54ead-143">hello content key authorization policy must be configured by you and met by hello client in order for hello content key toobe delivered toohello client.</span></span>

    <span data-ttu-id="54ead-144">Hello tartalomkulcs-hitelesítési házirend létrehozásakor toospecify hello következőkre lesz szüksége: kézbesítési módszer (PlayReady vagy Widevine), korlátozások (nyitott vagy token), és információt adott toohello kulcs kézbesítési típust, amely meghatározza, hogyan kerül hello kulcs toohello ügyfél ([PlayReady](media-services-playready-license-template-overview.md) vagy [Widevine](media-services-widevine-license-template-overview.md) licencsablon).</span><span class="sxs-lookup"><span data-stu-id="54ead-144">When creating hello content key authorization policy, you need toospecify hello following: delivery method (PlayReady or Widevine), restrictions (open or token), and information specific toohello key delivery type that defines how hello key is delivered toohello client ([PlayReady](media-services-playready-license-template-overview.md) or [Widevine](media-services-widevine-license-template-overview.md) license template).</span></span>

5. <span data-ttu-id="54ead-145">Az objektum továbbítási szabályzatát hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="54ead-145">Configure hello delivery policy for an asset.</span></span> <span data-ttu-id="54ead-146">hello továbbítási szabályzat konfigurációjához tartalmazza: továbbítási protokoll (például MPEG DASH, HLS, Smooth Streaming vagy az összes), a dinamikus titkosítás (például Common Encryption) és a PlayReady vagy Widevine-licenc licenckérési URL-cím típusú hello.</span><span class="sxs-lookup"><span data-stu-id="54ead-146">hello delivery policy configuration includes: delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all), hello type of dynamic encryption (for example, Common Encryption), PlayReady or Widevine license acquisition URL.</span></span>

    <span data-ttu-id="54ead-147">Tudta alkalmazni a különböző házirend tooeach protokollt a következő hello azonos eszköz.</span><span class="sxs-lookup"><span data-stu-id="54ead-147">You could apply different policy tooeach protocol on hello same asset.</span></span> <span data-ttu-id="54ead-148">Beállíthat például PlayReady titkosítási tooSmooth/DASH és AES Envelope tooHLS.</span><span class="sxs-lookup"><span data-stu-id="54ead-148">For example, you could apply PlayReady encryption tooSmooth/DASH and AES Envelope tooHLS.</span></span> <span data-ttu-id="54ead-149">Nincsenek megadva a továbbítási szabályzatban protokollok (például hozzáadhat egy szabályzatban, amely csak HLS hello protokoll) le lesz tiltva streaming.</span><span class="sxs-lookup"><span data-stu-id="54ead-149">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as hello protocol) will be blocked from streaming.</span></span> <span data-ttu-id="54ead-150">hello kivétel toothis jelent, ha egyáltalán nem állít be objektumtovábbítási szabályzatot egyáltalán.</span><span class="sxs-lookup"><span data-stu-id="54ead-150">hello exception toothis is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="54ead-151">Ezt követően hello törölje a jelet minden protokoll engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="54ead-151">Then, all protocols will be allowed in hello clear.</span></span>

6. <span data-ttu-id="54ead-152">Hozzon létre egy OnDemand-kereső rendelés tooget egy adatfolyam-továbbítási URL-címet.</span><span class="sxs-lookup"><span data-stu-id="54ead-152">Create an OnDemand locator in order tooget a streaming URL.</span></span>

<span data-ttu-id="54ead-153">Hello hello témakör végén teljes .NET típusú példát talál.</span><span class="sxs-lookup"><span data-stu-id="54ead-153">You will find a complete .NET example at hello end of hello topic.</span></span>

<span data-ttu-id="54ead-154">a következő kép hello hello fentiekben leírt munkafolyamatot mutatja be.</span><span class="sxs-lookup"><span data-stu-id="54ead-154">hello following image demonstrates hello workflow described above.</span></span> <span data-ttu-id="54ead-155">Itt hello jogkivonat-hitelesítéshez használt.</span><span class="sxs-lookup"><span data-stu-id="54ead-155">Here hello token is used for authentication.</span></span>

![Védelem biztosítása a PlayReadyvel](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

<span data-ttu-id="54ead-157">Ez a témakör többi hello részletes magyarázatokat, kódmintákat és olyan hivatkozásokat tootopics, amelyek bemutatják, hogyan tooachieve hello a fent leírt biztosít.</span><span class="sxs-lookup"><span data-stu-id="54ead-157">hello rest of this topic provides detailed explanations, code examples, and links tootopics that show you how tooachieve hello tasks described above.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="54ead-158">Aktuális korlátozások</span><span class="sxs-lookup"><span data-stu-id="54ead-158">Current limitations</span></span>
<span data-ttu-id="54ead-159">Ha hozzáadásakor vagy módosításakor az adategység továbbítási házirendjét, törölnie kell hello tartozó lokátort (ha van ilyen), majd hozzon létre egy új lokátort.</span><span class="sxs-lookup"><span data-stu-id="54ead-159">If you add or update an asset delivery policy, you must delete hello associated locator (if any) and create a new locator.</span></span>

<span data-ttu-id="54ead-160">Az Azure Media Services szolgáltatással végzett Widevine-titkosításra vonatkozó korlátozások: a funkció jelenleg nem támogatja több tartalomkulcs használatát.</span><span class="sxs-lookup"><span data-stu-id="54ead-160">Limitation when encrypting with Widevine with Azure Media Services: currently, multiple content keys are not supported.</span></span>

## <a name="create-an-asset-and-upload-files-into-hello-asset"></a><span data-ttu-id="54ead-161">Hozzon létre egy eszközt, majd fájlok feltöltése hello objektumba</span><span class="sxs-lookup"><span data-stu-id="54ead-161">Create an asset and upload files into hello asset</span></span>
<span data-ttu-id="54ead-162">A sorrend toomanage, kódolásához és streameléséhez a videók, akkor először fel kell töltenie a tartalom a Microsoft Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="54ead-162">In order toomanage, encode, and stream your videos, you must first upload your content into Microsoft Azure Media Services.</span></span> <span data-ttu-id="54ead-163">A feltöltést követően a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő.</span><span class="sxs-lookup"><span data-stu-id="54ead-163">Once uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span>

<span data-ttu-id="54ead-164">További információk: [Upload Files into a Media Services account](media-services-dotnet-upload-files.md) (Fájlok feltöltése a Media Services-fiókba).</span><span class="sxs-lookup"><span data-stu-id="54ead-164">For detailed information, see [Upload Files into a Media Services account](media-services-dotnet-upload-files.md).</span></span>

## <a name="encode-hello-asset-containing-hello-file-toohello-adaptive-bitrate-mp4-set"></a><span data-ttu-id="54ead-165">Hello eszköz tartalmazó hello fájl toohello adaptív sávszélességű MP4 típusú beállításkészlettel kódolása</span><span class="sxs-lookup"><span data-stu-id="54ead-165">Encode hello asset containing hello file toohello adaptive bitrate MP4 set</span></span>
<span data-ttu-id="54ead-166">A dinamikus titkosítás szüksége toocreate többszörös sávszélességű MP4-fájlok vagy többféle sávszélességű Smooth Streaming-forrásfájlokat tartalmazó objektumot.</span><span class="sxs-lookup"><span data-stu-id="54ead-166">With dynamic encryption all you need is toocreate an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="54ead-167">Ezt követően hello hello jegyzékben megadott formátumnak alapján és kérelem darabolható, igény szerinti adatfolyam-kiszolgáló biztosítja a megjelenő hello adatfolyam a kiválasztott protokollal hello hello.</span><span class="sxs-lookup"><span data-stu-id="54ead-167">Then, based on hello specified format in hello manifest and fragment request, hello On-Demand Streaming server will ensure that you receive hello stream in hello protocol you have chosen.</span></span> <span data-ttu-id="54ead-168">Ennek eredményeképpen csak akkor kell toostore és fizetési hello fájlokat egyetlen tárolási formátumban és a Media Services szolgáltatás elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ hello.</span><span class="sxs-lookup"><span data-stu-id="54ead-168">As a result, you only need toostore and pay for hello files in single storage format and Media Services service will build and serve hello appropriate response based on requests from a client.</span></span> <span data-ttu-id="54ead-169">További információkért lásd: hello [dinamikus becsomagolás áttekintése](media-services-dynamic-packaging-overview.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="54ead-169">For more information, see hello [Dynamic Packaging Overview](media-services-dynamic-packaging-overview.md) topic.</span></span>

<span data-ttu-id="54ead-170">Útmutatást tooencode, lásd: [hogyan tooencode keresztül Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span><span class="sxs-lookup"><span data-stu-id="54ead-170">For instructions on how tooencode, see [How tooencode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>

## <span data-ttu-id="54ead-171"><a id="create_contentkey"></a>Hozzon létre egy tartalomkulcsot, és azt társítsa a kódolt hello eszköz</span><span class="sxs-lookup"><span data-stu-id="54ead-171"><a id="create_contentkey"></a>Create a content key and associate it with hello encoded asset</span></span>
<span data-ttu-id="54ead-172">A Media Services szolgáltatásban hello tartalomkulcs tartalmazza, amelyet az eszköz tooencrypt hello kulcs együtt.</span><span class="sxs-lookup"><span data-stu-id="54ead-172">In Media Services, hello content key contains hello key that you want tooencrypt an asset with.</span></span>

<span data-ttu-id="54ead-173">További információk: [Create content key](media-services-dotnet-create-contentkey.md) (Tartalomkulcs létrehozása).</span><span class="sxs-lookup"><span data-stu-id="54ead-173">For detailed information, see [Create content key](media-services-dotnet-create-contentkey.md).</span></span>

## <span data-ttu-id="54ead-174"><a id="configure_key_auth_policy"></a>Hello tartalomkulcs hitelesítési szabályzatának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="54ead-174"><a id="configure_key_auth_policy"></a>Configure hello content key’s authorization policy</span></span>
<span data-ttu-id="54ead-175">A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="54ead-175">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="54ead-176">hello tartalomkulcs-hitelesítési szabályzatot kell Ön által konfigurált és érheti el, ha hello ügyfél (a lejátszó) ahhoz, hogy hello kulcs toobe toohello ügyfél kézbesíteni.</span><span class="sxs-lookup"><span data-stu-id="54ead-176">hello content key authorization policy must be configured by you and met by hello client (player) in order for hello key toobe delivered toohello client.</span></span> <span data-ttu-id="54ead-177">hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: Nyissa meg, vagy a token korlátozás.</span><span class="sxs-lookup"><span data-stu-id="54ead-177">hello content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span>

<span data-ttu-id="54ead-178">További információk: [A tartalomkulcs hitelesítési szabályzatának létrehozása](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).</span><span class="sxs-lookup"><span data-stu-id="54ead-178">For detailed information, see [Configure Content Key Authorization Policy](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).</span></span>

## <span data-ttu-id="54ead-179"><a id="configure_asset_delivery_policy"></a>Objektumtovábbítási szabályzat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="54ead-179"><a id="configure_asset_delivery_policy"></a>Configure asset delivery policy</span></span>
<span data-ttu-id="54ead-180">Az objektum továbbítási szabályzatát hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="54ead-180">Configure hello delivery policy for your asset.</span></span> <span data-ttu-id="54ead-181">Néhány dolog, amely az eszköz továbbítási szabályzat konfigurációjához hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="54ead-181">Some things that hello asset delivery policy configuration includes:</span></span>

* <span data-ttu-id="54ead-182">hello DRM licenc licenckérési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="54ead-182">hello DRM license acquisition URL.</span></span>
* <span data-ttu-id="54ead-183">hello objektumtovábbítási protokoll (például MPEG DASH, HLS, Smooth Streaming vagy all).</span><span class="sxs-lookup"><span data-stu-id="54ead-183">hello asset delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all).</span></span>
* <span data-ttu-id="54ead-184">a dinamikus titkosítás (az ebben az esetben Common Encrpytion) hello típusa.</span><span class="sxs-lookup"><span data-stu-id="54ead-184">hello type of dynamic encryption (in this case, Common Encryption).</span></span>

<span data-ttu-id="54ead-185">További információk: [Objektumtovábbítási szabályzat konfigurálása ](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="54ead-185">For detailed information, see [Configure asset delivery policy ](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <span data-ttu-id="54ead-186"><a id="create_locator"></a>Hozzon létre egy OnDemand-lokátor a rendelés tooget streamelési URL-cím</span><span class="sxs-lookup"><span data-stu-id="54ead-186"><a id="create_locator"></a>Create an OnDemand streaming locator in order tooget a streaming URL</span></span>
<span data-ttu-id="54ead-187">Szüksége lesz tooprovide a felhasználó az URL-címe Smooth, DASH vagy HLS streamelési hello.</span><span class="sxs-lookup"><span data-stu-id="54ead-187">You will need tooprovide your user with hello streaming URL for Smooth, DASH or HLS.</span></span>

> [!NOTE]
> <span data-ttu-id="54ead-188">Objektumtovábbítási szabályzat hozzáadásakor vagy módosításakor törölnie kell az ahhoz tartozó meglévő lokátort (ha van), majd létre kell hoznia egy új lokátort.</span><span class="sxs-lookup"><span data-stu-id="54ead-188">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
>
>

<span data-ttu-id="54ead-189">Hogyan toopublish egy eszköz és -buildek a streamelési URL-cím: kapcsolatos utasításokat [streamelési URL-cím összeállítása](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="54ead-189">For instructions on how toopublish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="get-a-test-token"></a><span data-ttu-id="54ead-190">Tesztjogkivonat lekérése</span><span class="sxs-lookup"><span data-stu-id="54ead-190">Get a test token</span></span>
<span data-ttu-id="54ead-191">Tesztjogkivonat lekérése hello hello kulcshitelesítési szabályzatban használt jogkivonat korlátozás alapján.</span><span class="sxs-lookup"><span data-stu-id="54ead-191">Get a test token based on hello token restriction that was used for hello key authorization policy.</span></span>

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on hello data in hello given TokenRestrictionTemplate.
    //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
    //so you have tooadd it in front of hello token string.
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);


<span data-ttu-id="54ead-192">Használhatja a hello [AMS-lejátszóval](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest az adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="54ead-192">You can use hello [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest your stream.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="54ead-193">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="54ead-193">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="54ead-194">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="54ead-194">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="54ead-195">Adja hozzá a következő elemek túl hello**appSettings** az app.config fájlban meghatározott:</span><span class="sxs-lookup"><span data-stu-id="54ead-195">Add hello following elements too**appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a><span data-ttu-id="54ead-196">Példa</span><span class="sxs-lookup"><span data-stu-id="54ead-196">Example</span></span>

<span data-ttu-id="54ead-197">hello következő mutatja be .NET-keretrendszerhez készült Azure Media Services SDK-ban bevezetett funkcióinak-verzió 3.5.2 (pontosabban hello képességét toodefine egy Widevine sablon licenc és Widevine licencet kér az Azure Media Services).</span><span class="sxs-lookup"><span data-stu-id="54ead-197">hello following sample demonstrates functionality that was introduced in Azure Media Services SDK for .Net -Version 3.5.2 (specifically, hello ability toodefine a Widevine license template and request a Widevine license from Azure Media Services).</span></span>

<span data-ttu-id="54ead-198">Írja felül a Program.cs fájlban hello kód hello kód itt látható.</span><span class="sxs-lookup"><span data-stu-id="54ead-198">Overwrite hello code in your Program.cs file with hello code shown in this section.</span></span>

>[!NOTE]
><span data-ttu-id="54ead-199">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="54ead-199">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="54ead-200">Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek.</span><span class="sxs-lookup"><span data-stu-id="54ead-200">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="54ead-201">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="54ead-201">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="54ead-202">Győződjön meg arról, hogy tooupdate változók toopoint toofolders ahol a bemeneti fájlok találhatók.</span><span class="sxs-lookup"><span data-stu-id="54ead-202">Make sure tooupdate variables toopoint toofolders where your input files are located.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
    using Newtonsoft.Json;

    namespace DynamicEncryptionWithDRM
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

            IContentKey key = CreateCommonTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
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

            // You can use hello http://amsplayer.azurewebsites.net/azuremediaplayer.html player tootest streams.
            // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);

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

            IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} too{1}",
                        inputAsset.Name,
                        encodingPreset));

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


        static public IContentKey CreateCommonTypeContentKey(IAsset asset)
        {

            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryption);

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

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with no restrictions").
                Result;


            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
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

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);

            // Associate hello content key authorization policy with hello content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
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

        static private string ConfigurePlayReadyLicenseTemplate()
        {
            // hello following code configures PlayReady License Template using .NET classes
            // and returns hello XML string.

            //hello PlayReadyLicenseResponseTemplate class represents hello template for hello response sent back toohello end user.
            //It contains a field for a custom data string between hello license server and hello application
            //(may be useful for custom app logic) as well as a list of one or more license templates.
            PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

            // hello PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
            // toobe returned toohello end users.
            //It contains hello data on hello content key in hello license and any rights or restrictions toobe
            //enforced by hello PlayReady DRM runtime when using hello content key.
            PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
            //Configure whether hello license is persistent (saved in persistent storage on hello client)
            //or non-persistent (only held in memory while hello player is using hello license).  
            licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

            // AllowTestDevices controls whether test devices can use hello license or not.  
            // If true, hello MinimumSecurityLevel property of hello license
            // is set too150.  If false (hello default), hello MinimumSecurityLevel property of hello license is set too2000.
            licenseTemplate.AllowTestDevices = true;

            // You can also configure hello Play Right in hello PlayReady license by using hello PlayReadyPlayRight class.
            // It grants hello user hello ability tooplayback hello content subject toohello zero or more restrictions
            // configured in hello license and on hello PlayRight itself (for playback specific policy).
            // Much of hello policy on hello PlayRight has toodo with output restrictions
            // which control hello types of outputs that hello content can be played over and
            // any restrictions that must be put in place when using a given output.
            // For example, if hello DigitalVideoOnlyContentRestriction is enabled,
            //then hello DRM runtime will only allow hello video toobe displayed over digital outputs
            //(analog video outputs won’t be allowed toopass hello content).

            //IMPORTANT: These types of restrictions can be very powerful but can also affect hello consumer experience.
            // If hello output protections are configured too restrictive,
            // hello content might be unplayable on some clients. For more information, see hello PlayReady Compliance Rules document.

            // For example:
            //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

            responseTemplate.LicenseTemplates.Add(licenseTemplate);

            return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
        }

        private static string ConfigureWidevineLicenseTemplate()
        {
            var template = new WidevineMessage
            {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                    new ContentKeySpecs
                    {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                    }
                },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
            };

            string configuration = JsonConvert.SerializeObject(template);
            return configuration;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            // Get hello PlayReady license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

            // GetKeyDeliveryUrl for Widevine attaches hello KID toohello URL.
            // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
            // hello WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption
            // tooappend /? KID =< keyId > toohello end of hello url when creating hello manifest.
            // As a result Widevine license acquisition URL will have KID appended twice,
            // so we need tooremove hello KID that in hello URL when we call GetKeyDeliveryUrl.

            Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
            UriBuilder uriBuilder = new UriBuilder(widevineUrl);
            uriBuilder.Query = String.Empty;
            widevineUrl = uriBuilder.Uri;

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                    {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}

            };

            // In this case we only specify Dash streaming protocol in hello delivery policy,
            // All other protocols will be blocked from streaming.
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
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


## <a name="next-step"></a><span data-ttu-id="54ead-203">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="54ead-203">Next step</span></span>
<span data-ttu-id="54ead-204">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="54ead-204">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="54ead-205">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="54ead-205">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="54ead-206">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="54ead-206">See also</span></span>
<span data-ttu-id="54ead-207">[CENC with Multi-DRM and Access Control](media-services-cenc-with-multidrm-access-control.md) (CENC többszörös DRM-mel és hozzáférés-vezérléssel)</span><span class="sxs-lookup"><span data-stu-id="54ead-207">[CENC with Multi-DRM and Access Control](media-services-cenc-with-multidrm-access-control.md)</span></span>

<span data-ttu-id="54ead-208">[Configure Widevine packaging with AMS](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services) (Widevine-csomagolás konfigurálása az AMS-szel)</span><span class="sxs-lookup"><span data-stu-id="54ead-208">[Configure Widevine packaging with AMS](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)</span></span>

<span data-ttu-id="54ead-209">[Announcing Google Widevine license delivery services in Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/) (A Google Widevine-licenctovábbítási szolgáltatás megjelenése az Azure Media Services-ben)</span><span class="sxs-lookup"><span data-stu-id="54ead-209">[Announcing Google Widevine license delivery services in Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)</span></span>
