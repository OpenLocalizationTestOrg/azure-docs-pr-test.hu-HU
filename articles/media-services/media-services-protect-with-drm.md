---
title: "PlayReady és/vagy Widevine Dynamic Common Encryption használata | Microsoft Docs"
description: "A Microsoft Azure Media Services lehetővé teszi, hogy MPEG-DASH, Smooth Streaming vagy HTTP-Live-Streaming (HLS) típusú streamjeit Microsoft PlayReady DRM-védelemmel lássa el. Ezenfelül Widevine DRM-védelemmel ellátott DASH-továbbítást is kínál. Ez a témakör bemutatja, hogyan állíthat be dinamikus titkosítást a PlayReady vagy a Widevine DRM segítségével."
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
ms.openlocfilehash: 6cfb7b558b8dce511d517e69c022765feae245fa
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="using-playready-andor-widevine-dynamic-common-encryption"></a><span data-ttu-id="43c1f-105">A PlayReady és/vagy Widevine Dynamic Common Encryption titkosítás használata</span><span class="sxs-lookup"><span data-stu-id="43c1f-105">Using PlayReady and/or Widevine dynamic common encryption</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="43c1f-106">.NET</span><span class="sxs-lookup"><span data-stu-id="43c1f-106">.NET</span></span>](media-services-protect-with-drm.md)
> * [<span data-ttu-id="43c1f-107">Java</span><span class="sxs-lookup"><span data-stu-id="43c1f-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="43c1f-108">PHP</span><span class="sxs-lookup"><span data-stu-id="43c1f-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
>
>

<span data-ttu-id="43c1f-109">A Microsoft Azure Media Services lehetővé teszi, hogy MPEG-DASH, Smooth Streaming vagy HTTP-Live-Streaming (HLS) típusú streamjeit [Microsoft PlayReady DRM-védelemmel](https://www.microsoft.com/playready/overview/) lássa el.</span><span class="sxs-lookup"><span data-stu-id="43c1f-109">Microsoft Azure Media Services enables you to deliver MPEG-DASH, Smooth Streaming, and HTTP-Live-Streaming (HLS) streams protected with [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span></span> <span data-ttu-id="43c1f-110">Ezenfelül arra is lehetőséget kínál, hogy titkosított DASH-streameket továbbítson Widevine DRM-licencek segítségével.</span><span class="sxs-lookup"><span data-stu-id="43c1f-110">It also enables you to deliver encrypted DASH streams with Widevine DRM licenses.</span></span> <span data-ttu-id="43c1f-111">Mind a PlayReady, mind a Widevine titkosítása a Common Encryption (ISO/IEC 23001-7 CENC) szabvány specifikációi szerint történik.</span><span class="sxs-lookup"><span data-stu-id="43c1f-111">Both PlayReady and Widevine are encrypted per the Common Encryption (ISO/IEC 23001-7 CENC) specification.</span></span> <span data-ttu-id="43c1f-112">Az AssetDeliveryConfiguration Widevine használatára történő beállításához használja az [AMS .NET SDK-t](https://www.nuget.org/packages/windowsazure.mediaservices/) (a 3.5.1-es vagy újabb verziót), vagy a REST API-t.</span><span class="sxs-lookup"><span data-stu-id="43c1f-112">You can use [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (starting with the version 3.5.1) or REST API to configure your AssetDeliveryConfiguration to use Widevine.</span></span>

<span data-ttu-id="43c1f-113">A Media Services része egy szolgáltatás, amelynek segítségével PlayReady vagy Widevine DRM-licenceket továbbíthat.</span><span class="sxs-lookup"><span data-stu-id="43c1f-113">Media Services provides a service for delivering PlayReady and Widevine DRM licenses.</span></span> <span data-ttu-id="43c1f-114">A Media Services ezenfelül API-kat is tartalmaz, amelyek segítségével beállíthatja azokat a jogokat és korlátozásokat, amelyeket szeretne betartatni a PlayReady vagy a Widevine DRM-futtatókörnyezettel, amikor egy felhasználó védett tartalmakat játszik le.</span><span class="sxs-lookup"><span data-stu-id="43c1f-114">Media Services also provides APIs that let you configure the rights and restrictions that you want for the PlayReady or Widevine DRM runtime to enforce when a user plays back protected content.</span></span> <span data-ttu-id="43c1f-115">Amikor a felhasználók DRM-védelemmel rendelkező tartalmat kérnek, a lejátszóalkalmazás licencet kér az AMS-licencelési szolgáltatástól.</span><span class="sxs-lookup"><span data-stu-id="43c1f-115">When a user requests a DRM protected content, the player application will request a license from the AMS license service.</span></span> <span data-ttu-id="43c1f-116">Az AMS-licencelési szolgáltatás akkor adja meg a licencet, ha a kérelmező felhasználó megkapta a megfelelő jogosultságokat.</span><span class="sxs-lookup"><span data-stu-id="43c1f-116">The AMS license service will issue a license to the player if it is authorized.</span></span> <span data-ttu-id="43c1f-117">A PlayReady- vagy Widevine-licencek tartalmazzák a feloldási kulcsot, amelynek segítségével az ügyféllejátszó képes feloldani a titkosítást, majd streamelni a kért tartalmakat.</span><span class="sxs-lookup"><span data-stu-id="43c1f-117">A PlayReady or Widevine license contains the decryption key that can be used by the client player to decrypt and stream the content.</span></span>

<span data-ttu-id="43c1f-118">A Widevine-licencek továbbításának támogatásához a következő AMS-partnereket is használhatja: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) vagy [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="43c1f-118">You can also use the following AMS partners to help you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span> <span data-ttu-id="43c1f-119">További információk: integráció az [Axinom](media-services-axinom-integration.md) és a [castLabs](media-services-castlabs-integration.md) rendszerekkel.</span><span class="sxs-lookup"><span data-stu-id="43c1f-119">For more information, see: integration with [Axinom](media-services-axinom-integration.md) and [castLabs](media-services-castlabs-integration.md).</span></span>

<span data-ttu-id="43c1f-120">A Media Services szolgáltatásban több különböző módot is beállíthat, amelyek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="43c1f-120">Media Services supports multiple ways of authorizing users who make key requests.</span></span> <span data-ttu-id="43c1f-121">A tartalomkulcs-hitelesítési szabályzat egy vagy több hitelesítési korlátozást tartalmazhat: ezek lehetnek nyitott vagy jogkivonat-korlátozások.</span><span class="sxs-lookup"><span data-stu-id="43c1f-121">The content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span> <span data-ttu-id="43c1f-122">A tokennel korlátozott szabályzatokhoz a Secure Token Service (Biztonsági jegykiadó szolgáltatás, STS) által kiadott tokennek kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="43c1f-122">The token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="43c1f-123">A Media Services a [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) és a [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) formátumú tokeneket támogatja.</span><span class="sxs-lookup"><span data-stu-id="43c1f-123">Media Services supports tokens in the [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) format and [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) format.</span></span> <span data-ttu-id="43c1f-124">További információk: A tartalomkulcs hitelesítési szabályzatának konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="43c1f-124">For more information, see Configure the content key’s authorization policy.</span></span>

<span data-ttu-id="43c1f-125">A dinamikus titkosítás által nyújtott előnyök kihasználásához többszörös sávszélességű MP4-fájlokat vagy Smooth Streaming-forrásfájlokat tartalmazó objektummal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="43c1f-125">To take advantage of dynamic encryption, you need to have an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="43c1f-126">Ezenfelül be kell állítania az objektumhoz tartozó továbbítási szabályzatokat is (ennek módját a témakör későbbi részében írjuk le).</span><span class="sxs-lookup"><span data-stu-id="43c1f-126">You also need to configure the delivery policies for the asset (described later in this topic).</span></span> <span data-ttu-id="43c1f-127">Ezt követően az igényalapú streamelési kiszolgáló a streamelési URL-címben megadott formátumnak megfelelően gondoskodik arról, hogy a rendszer a kiválasztott protokollal továbbítsa a streamet.</span><span class="sxs-lookup"><span data-stu-id="43c1f-127">Then, based on the format specified in the streaming URL, the On-Demand Streaming server will ensure that the stream is delivered in the protocol you have chosen.</span></span> <span data-ttu-id="43c1f-128">Így elég egyetlen tárolási formátumban tárolni a fájlokat (és kifizetni a tárhelyüket), a Media Services elkészíti és kiszolgálja az ügyféltől érkező kéréseknek megfelelő HTTP-választ.</span><span class="sxs-lookup"><span data-stu-id="43c1f-128">As a result, you only need to store and pay for the files in a single storage format and Media Services will build and serve the appropriate HTTP response based on each request from a client.</span></span>

<span data-ttu-id="43c1f-129">Ez a témakör azon fejlesztők számára lehet hasznos, akik többféle DRM-mel (például PlayReady és Widevine) védett médiafájlok továbbításával foglalkoznak.</span><span class="sxs-lookup"><span data-stu-id="43c1f-129">This topic would be useful to developers that work on applications that deliver media protected with multiple DRMs, such as PlayReady and Widevine.</span></span> <span data-ttu-id="43c1f-130">A témakör leírja, hogyan konfigurálhatja a PlayReady-licenctovábbítási szolgáltatásra vonatkozó szabályzatokat úgy, hogy csak az arra jogosult ügyfelek kaphassák meg a PlayReady- és Widevine-licenceket.</span><span class="sxs-lookup"><span data-stu-id="43c1f-130">The topic shows you how to configure the PlayReady license delivery service with authorization policies so that only authorized clients could receive PlayReady or Widevine licenses.</span></span> <span data-ttu-id="43c1f-131">Ezenfelül azt is bemutatja, hogyan használja a dinamikus titkosítás funkciót a PlayReady vagy a Widevine DRM-mel a DASH-en keresztül.</span><span class="sxs-lookup"><span data-stu-id="43c1f-131">It also shows how to use dynamic encryption encryption with PlayReady or Widevine DRM over DASH.</span></span>

>[!NOTE]
><span data-ttu-id="43c1f-132">Az AMS-fiók létrehozásakor a rendszer hozzáad egy **alapértelmezett** streamvégpontot a fiókhoz **Leállítva** állapotban.</span><span class="sxs-lookup"><span data-stu-id="43c1f-132">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="43c1f-133">A tartalom streamelésének megkezdéséhez, valamint a dinamikus csomagolás és a dinamikus titkosítás kihasználásához a tartalomstreameléshez használt streamvégpontnak **Fut** állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="43c1f-133">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 

## <a name="download-sample"></a><span data-ttu-id="43c1f-134">Minta letöltése</span><span class="sxs-lookup"><span data-stu-id="43c1f-134">Download sample</span></span>
<span data-ttu-id="43c1f-135">A cikkben leírt mintát [innen](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm) töltheti le.</span><span class="sxs-lookup"><span data-stu-id="43c1f-135">You can download the sample described in this article from [here](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).</span></span>

## <a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a><span data-ttu-id="43c1f-136">A Dynamic Common Encryption és DRM-licenctovábbítási szolgáltatások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43c1f-136">Configuring Dynamic Common Encryption and DRM License Delivery Services</span></span>

<span data-ttu-id="43c1f-137">A következőkben általános lépéseket olvashat, amelyeket el kell végeznie ahhoz, hogy PlayReady-védelemmel lássa el objektumait a Media Services licenctovábbítási szolgáltatása, valamint a dinamikus titkosítás használata mellett.</span><span class="sxs-lookup"><span data-stu-id="43c1f-137">The following are general steps that you would need to perform when protecting your assets with PlayReady, using the Media Services license delivery service, and also using dynamic encryption.</span></span>

1. <span data-ttu-id="43c1f-138">Hozzon létre egy objektumot, és töltse fel a fájlokat az objektumba.</span><span class="sxs-lookup"><span data-stu-id="43c1f-138">Create an asset and upload files into the asset.</span></span>
2. <span data-ttu-id="43c1f-139">Kódolja a fájlt tartalmazó objektumot az adaptív sávszélességű MP4 típusú beállításkészlettel.</span><span class="sxs-lookup"><span data-stu-id="43c1f-139">Encode the asset containing the file to the adaptive bitrate MP4 set.</span></span>
3. <span data-ttu-id="43c1f-140">Hozzon létre egy tartalomkulcsot, majd társítsa a kódolt objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="43c1f-140">Create a content key and associate it with the encoded asset.</span></span> <span data-ttu-id="43c1f-141">A Media Services szolgáltatásban a tartalomkulcs tartalmazza az objektum titkosítási kulcsát.</span><span class="sxs-lookup"><span data-stu-id="43c1f-141">In Media Services, the content key contains the asset’s encryption key.</span></span>
4. <span data-ttu-id="43c1f-142">Konfigurálja a tartalomkulcs hitelesítési szabályzatát.</span><span class="sxs-lookup"><span data-stu-id="43c1f-142">Configure the content key’s authorization policy.</span></span> <span data-ttu-id="43c1f-143">Ahhoz, hogy az ügyfél megkaphassa a tartalomkulcsot, Önnek be kell állítania a tartalomkulcs-hitelesítési szabályzatot, amelynek az ügyfélnek meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="43c1f-143">The content key authorization policy must be configured by you and met by the client in order for the content key to be delivered to the client.</span></span>

    <span data-ttu-id="43c1f-144">A tartalomkulcs-hitelesítési szabályzat létrehozásakor a következőket kell beállítania: továbbítási módszer (PlayReady vagy Widevine), korlátozások (nyitott vagy token), valamint azon információk, amelyek azt határozzák meg, hogy a rendszer hogyan továbbítja a kulcsot az ügyfélnek ([PlayReady-](media-services-playready-license-template-overview.md) vagy [Widevine-](media-services-widevine-license-template-overview.md)licencsablon).</span><span class="sxs-lookup"><span data-stu-id="43c1f-144">When creating the content key authorization policy, you need to specify the following: delivery method (PlayReady or Widevine), restrictions (open or token), and information specific to the key delivery type that defines how the key is delivered to the client ([PlayReady](media-services-playready-license-template-overview.md) or [Widevine](media-services-widevine-license-template-overview.md) license template).</span></span>

5. <span data-ttu-id="43c1f-145">Konfigurálja az objektum továbbítási szabályzatát.</span><span class="sxs-lookup"><span data-stu-id="43c1f-145">Configure the delivery policy for an asset.</span></span> <span data-ttu-id="43c1f-146">A továbbítási szabályzat konfigurációs lehetőségei között a következők találhatók: továbbítási protokoll (pl. MPEG DASH, HLS, Smooth Streaming vagy ezek mindegyike), a dinamikus titkosítás típusa (pl. Common Encryption), a PlayReady- vagy Widevine-licenckérési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="43c1f-146">The delivery policy configuration includes: delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all), the type of dynamic encryption (for example, Common Encryption), PlayReady or Widevine license acquisition URL.</span></span>

    <span data-ttu-id="43c1f-147">Az adott objektum különböző protokolljaira akár eltérő szabályzatokat is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="43c1f-147">You could apply different policy to each protocol on the same asset.</span></span> <span data-ttu-id="43c1f-148">Beállíthatja például, hogy a PlayReady-titkosítás csak a Smooth/DASH-re vonatkozzon, az AES Envelope pedig csak a HLS-re.</span><span class="sxs-lookup"><span data-stu-id="43c1f-148">For example, you could apply PlayReady encryption to Smooth/DASH and AES Envelope to HLS.</span></span> <span data-ttu-id="43c1f-149">A továbbítási szabályzatban meg nem határozott protokollok streameléshez való használatát a rendszer nem engedélyezi (ilyen lehet például, ha csupán egyetlen szabályzatot állít be, amely kizárólag a HLS-protokoll használatát tartalmazza).</span><span class="sxs-lookup"><span data-stu-id="43c1f-149">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as the protocol) will be blocked from streaming.</span></span> <span data-ttu-id="43c1f-150">Kivételt jelent, ha egyáltalán nem állít be objektumtovábbítási szabályzatot.</span><span class="sxs-lookup"><span data-stu-id="43c1f-150">The exception to this is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="43c1f-151">Ebben az esetben a rendszer az összes protokollt engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="43c1f-151">Then, all protocols will be allowed in the clear.</span></span>

6. <span data-ttu-id="43c1f-152">Hozzon létre egy OnDemand-lokátort a streamelési URL-cím lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="43c1f-152">Create an OnDemand locator in order to get a streaming URL.</span></span>

<span data-ttu-id="43c1f-153">A témakör végén teljes .NET típusú példát talál.</span><span class="sxs-lookup"><span data-stu-id="43c1f-153">You will find a complete .NET example at the end of the topic.</span></span>

<span data-ttu-id="43c1f-154">Az alábbi képen a fentiekben leírt munkafolyamatot láthatja.</span><span class="sxs-lookup"><span data-stu-id="43c1f-154">The following image demonstrates the workflow described above.</span></span> <span data-ttu-id="43c1f-155">Itt a tokenes hitelesítést használtuk.</span><span class="sxs-lookup"><span data-stu-id="43c1f-155">Here the token is used for authentication.</span></span>

![Védelem biztosítása a PlayReadyvel](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

<span data-ttu-id="43c1f-157">A témakör további részében részletes magyarázatokat, kódmintákat és olyan témakörökre mutató hivatkozásokat talál, amelyek segítenek elérni a fent leírt célokat.</span><span class="sxs-lookup"><span data-stu-id="43c1f-157">The rest of this topic provides detailed explanations, code examples, and links to topics that show you how to achieve the tasks described above.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="43c1f-158">Aktuális korlátozások</span><span class="sxs-lookup"><span data-stu-id="43c1f-158">Current limitations</span></span>
<span data-ttu-id="43c1f-159">Objektumtovábbítási szabályzat hozzáadásakor vagy módosításakor törölnie kell az ahhoz tartozó lokátort (ha van), majd létre kell hoznia egy új lokátort.</span><span class="sxs-lookup"><span data-stu-id="43c1f-159">If you add or update an asset delivery policy, you must delete the associated locator (if any) and create a new locator.</span></span>

<span data-ttu-id="43c1f-160">Az Azure Media Services szolgáltatással végzett Widevine-titkosításra vonatkozó korlátozások: a funkció jelenleg nem támogatja több tartalomkulcs használatát.</span><span class="sxs-lookup"><span data-stu-id="43c1f-160">Limitation when encrypting with Widevine with Azure Media Services: currently, multiple content keys are not supported.</span></span>

## <a name="create-an-asset-and-upload-files-into-the-asset"></a><span data-ttu-id="43c1f-161">Objektum létrehozása, majd fájlok feltöltése az objektumba</span><span class="sxs-lookup"><span data-stu-id="43c1f-161">Create an asset and upload files into the asset</span></span>
<span data-ttu-id="43c1f-162">A videók kezeléséhez, kódolásához és streameléséhez először fel kell töltenie tartalmait a Microsoft Azure Media Services szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="43c1f-162">In order to manage, encode, and stream your videos, you must first upload your content into Microsoft Azure Media Services.</span></span> <span data-ttu-id="43c1f-163">A feltöltést követően tartalmai a biztonságos felhőtárhelyre kerülnek további feldolgozás és streamelés céljából.</span><span class="sxs-lookup"><span data-stu-id="43c1f-163">Once uploaded, your content is stored securely in the cloud for further processing and streaming.</span></span>

<span data-ttu-id="43c1f-164">További információk: [Upload Files into a Media Services account](media-services-dotnet-upload-files.md) (Fájlok feltöltése a Media Services-fiókba).</span><span class="sxs-lookup"><span data-stu-id="43c1f-164">For detailed information, see [Upload Files into a Media Services account](media-services-dotnet-upload-files.md).</span></span>

## <a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a><span data-ttu-id="43c1f-165">A fájlt tartalmazó objektum kódolása az adaptív sávszélességű MP4 típusú beállításkészlettel</span><span class="sxs-lookup"><span data-stu-id="43c1f-165">Encode the asset containing the file to the adaptive bitrate MP4 set</span></span>
<span data-ttu-id="43c1f-166">A dinamikus titkosítás segítségével mindössze egy többszörös sávszélességű MP4-fájlokat vagy Smooth Streaming-forrásfájlokat tartalmazó objektumot kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="43c1f-166">With dynamic encryption all you need is to create an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="43c1f-167">Ezt követően az igényalapú streamelési kiszolgáló a jegyzékfájlban és a töredékkérésben megadott formátumnak megfelelően gondoskodik arról, hogy a rendszer a kiválasztott protokollal biztosítsa a streamet az Ön számára.</span><span class="sxs-lookup"><span data-stu-id="43c1f-167">Then, based on the specified format in the manifest and fragment request, the On-Demand Streaming server will ensure that you receive the stream in the protocol you have chosen.</span></span> <span data-ttu-id="43c1f-168">Így elég egyetlen tárolási formátumban tárolni a fájlokat (és kifizetni a tárhelyüket), a Media Services szolgáltatás elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ.</span><span class="sxs-lookup"><span data-stu-id="43c1f-168">As a result, you only need to store and pay for the files in single storage format and Media Services service will build and serve the appropriate response based on requests from a client.</span></span> <span data-ttu-id="43c1f-169">További információkért lásd a [Dynamic Packaging Overview](media-services-dynamic-packaging-overview.md) (A dinamikus becsomagolás áttekintése) című témakört.</span><span class="sxs-lookup"><span data-stu-id="43c1f-169">For more information, see the [Dynamic Packaging Overview](media-services-dynamic-packaging-overview.md) topic.</span></span>

<span data-ttu-id="43c1f-170">A kódolással kapcsolatos utasításokért lásd: [How to encode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) (Objektum kódolása a Media Encoder Standard használatával).</span><span class="sxs-lookup"><span data-stu-id="43c1f-170">For instructions on how to encode, see [How to encode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>

## <span data-ttu-id="43c1f-171"><a id="create_contentkey"></a>Tartalomkulcs létrehozása és társítása a kódolt objektumhoz</span><span class="sxs-lookup"><span data-stu-id="43c1f-171"><a id="create_contentkey"></a>Create a content key and associate it with the encoded asset</span></span>
<span data-ttu-id="43c1f-172">A Media Services szolgáltatásban a tartalomkulcs tartalmazza az objektum titkosítására használható kulcsot.</span><span class="sxs-lookup"><span data-stu-id="43c1f-172">In Media Services, the content key contains the key that you want to encrypt an asset with.</span></span>

<span data-ttu-id="43c1f-173">További információk: [Create content key](media-services-dotnet-create-contentkey.md) (Tartalomkulcs létrehozása).</span><span class="sxs-lookup"><span data-stu-id="43c1f-173">For detailed information, see [Create content key](media-services-dotnet-create-contentkey.md).</span></span>

## <span data-ttu-id="43c1f-174"><a id="configure_key_auth_policy"></a>A tartalomkulcs engedélyezési házirendjének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43c1f-174"><a id="configure_key_auth_policy"></a>Configure the content key’s authorization policy</span></span>
<span data-ttu-id="43c1f-175">A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="43c1f-175">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="43c1f-176">Ahhoz, hogy az ügyfél (a lejátszó) megkaphassa a kulcsot, Önnek be kell állítania a tartalomkulcs-hitelesítési szabályzatot, amelynek az ügyfélnek meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="43c1f-176">The content key authorization policy must be configured by you and met by the client (player) in order for the key to be delivered to the client.</span></span> <span data-ttu-id="43c1f-177">A tartalomkulcs-hitelesítési szabályzat egy vagy több hitelesítési korlátozást tartalmazhat: ezek lehetnek nyitott vagy jogkivonat-korlátozások.</span><span class="sxs-lookup"><span data-stu-id="43c1f-177">The content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span>

<span data-ttu-id="43c1f-178">További információk: [A tartalomkulcs hitelesítési szabályzatának létrehozása](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).</span><span class="sxs-lookup"><span data-stu-id="43c1f-178">For detailed information, see [Configure Content Key Authorization Policy](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).</span></span>

## <span data-ttu-id="43c1f-179"><a id="configure_asset_delivery_policy"></a>Objektumtovábbítási szabályzat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43c1f-179"><a id="configure_asset_delivery_policy"></a>Configure asset delivery policy</span></span>
<span data-ttu-id="43c1f-180">Konfigurálja az objektum továbbítási szabályzatát.</span><span class="sxs-lookup"><span data-stu-id="43c1f-180">Configure the delivery policy for your asset.</span></span> <span data-ttu-id="43c1f-181">Az objektumtovábbítási szabályzat konfigurálásához többek között az alábbiak tartoznak:</span><span class="sxs-lookup"><span data-stu-id="43c1f-181">Some things that the asset delivery policy configuration includes:</span></span>

* <span data-ttu-id="43c1f-182">A DRM-licenckérési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="43c1f-182">The DRM license acquisition URL.</span></span>
* <span data-ttu-id="43c1f-183">Az adategység-továbbítási protokoll (pl. MPEG DASH, HLS, Smooth Streaming vagy ezek mindegyike).</span><span class="sxs-lookup"><span data-stu-id="43c1f-183">The asset delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all).</span></span>
* <span data-ttu-id="43c1f-184">A dinamikus titkosítás típusa (ebben az esetben Common Encrpytion).</span><span class="sxs-lookup"><span data-stu-id="43c1f-184">The type of dynamic encryption (in this case, Common Encryption).</span></span>

<span data-ttu-id="43c1f-185">További információk: [Objektumtovábbítási szabályzat konfigurálása ](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="43c1f-185">For detailed information, see [Configure asset delivery policy ](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <span data-ttu-id="43c1f-186"><a id="create_locator"></a>OnDemand-lokátor létrehozása a streamelési URL-cím lekérése érdekében</span><span class="sxs-lookup"><span data-stu-id="43c1f-186"><a id="create_locator"></a>Create an OnDemand streaming locator in order to get a streaming URL</span></span>
<span data-ttu-id="43c1f-187">A felhasználók rendelkezésére kell bocsátania a Smooth, DASH vagy HLS streamelési URL-címét.</span><span class="sxs-lookup"><span data-stu-id="43c1f-187">You will need to provide your user with the streaming URL for Smooth, DASH or HLS.</span></span>

> [!NOTE]
> <span data-ttu-id="43c1f-188">Objektumtovábbítási szabályzat hozzáadásakor vagy módosításakor törölnie kell az ahhoz tartozó meglévő lokátort (ha van), majd létre kell hoznia egy új lokátort.</span><span class="sxs-lookup"><span data-stu-id="43c1f-188">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
>
>

<span data-ttu-id="43c1f-189">További információk az objektumok közzétételéről és a streamelési URL-cím létrehozásáról: [Build a streaming URL](media-services-deliver-streaming-content.md) (Streamelési URL-cím létrehozása).</span><span class="sxs-lookup"><span data-stu-id="43c1f-189">For instructions on how to publish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="get-a-test-token"></a><span data-ttu-id="43c1f-190">Tesztjogkivonat lekérése</span><span class="sxs-lookup"><span data-stu-id="43c1f-190">Get a test token</span></span>
<span data-ttu-id="43c1f-191">Kérje le a kulcshitelesítési szabályzatban használt jogkivonat-korlátozásoknak megfelelő tesztjogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="43c1f-191">Get a test token based on the token restriction that was used for the key authorization policy.</span></span>

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string.
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);


<span data-ttu-id="43c1f-192">A stream kipróbálásához használja az [AMS-lejátszót](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="43c1f-192">You can use the [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) to test your stream.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="43c1f-193">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43c1f-193">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="43c1f-194">Állítsa be a fejlesztési környezetet, és töltse fel az app.config fájlt a kapcsolatadatokkal a [.NET-keretrendszerrel történő Media Services-fejlesztést](media-services-dotnet-how-to-use.md) ismertető dokumentumban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="43c1f-194">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="43c1f-195">Adja hozzá a következő elemeket az app.config fájlban megadott **appSettings** szakaszhoz:</span><span class="sxs-lookup"><span data-stu-id="43c1f-195">Add the following elements to **appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a><span data-ttu-id="43c1f-196">Példa</span><span class="sxs-lookup"><span data-stu-id="43c1f-196">Example</span></span>

<span data-ttu-id="43c1f-197">Az alábbi mintában azokat a funkciókat mutatjuk be, amelyeket az Azure Media Services SDK for .Net 3.5.2-es verziója vezetett be (azaz a funkciót, amelynek segítségével Widevine-licencsablon állítható be, majd a Widevine-licenc lekérhető az Azure Media Services szolgáltatásból).</span><span class="sxs-lookup"><span data-stu-id="43c1f-197">The following sample demonstrates functionality that was introduced in Azure Media Services SDK for .Net -Version 3.5.2 (specifically, the ability to define a Widevine license template and request a Widevine license from Azure Media Services).</span></span>

<span data-ttu-id="43c1f-198">Írja felül a Program.cs fájlban található kódot az itt látható kóddal.</span><span class="sxs-lookup"><span data-stu-id="43c1f-198">Overwrite the code in your Program.cs file with the code shown in this section.</span></span>

>[!NOTE]
><span data-ttu-id="43c1f-199">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="43c1f-199">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="43c1f-200">Ha mindig ugyanazokat a napokat/hozzáférési engedélyeket használja (például olyan keresők szabályzatait, amelyek hosszú ideig érvényben maradnak, vagyis nem feltöltött szabályzatokat), a szabályzatazonosítónak is ugyanannak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="43c1f-200">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="43c1f-201">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="43c1f-201">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="43c1f-202">Módosítsa úgy a változókat, hogy a bemeneti fájlok tárolásához Ön által használt mappákra mutassanak.</span><span class="sxs-lookup"><span data-stu-id="43c1f-202">Make sure to update variables to point to folders where your input files are located.</span></span>

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

            IContentKey key = CreateCommonTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
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

            // Generate a test token based on the the data in the given TokenRestrictionTemplate.
            // Note, you need to pass the key id Guid because we specified
            // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                        DateTime.UtcNow.AddDays(365));
            Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
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

            IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
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

            // Associate the content key authorization policy with the content key
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
            // The following code configures PlayReady License Template using .NET classes
            // and returns the XML string.

            //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user.
            //It contains a field for a custom data string between the license server and the application
            //(may be useful for custom app logic) as well as a list of one or more license templates.
            PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

            // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
            // to be returned to the end users.
            //It contains the data on the content key in the license and any rights or restrictions to be
            //enforced by the PlayReady DRM runtime when using the content key.
            PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
            //Configure whether the license is persistent (saved in persistent storage on the client)
            //or non-persistent (only held in memory while the player is using the license).  
            licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

            // AllowTestDevices controls whether test devices can use the license or not.  
            // If true, the MinimumSecurityLevel property of the license
            // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
            licenseTemplate.AllowTestDevices = true;

            // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class.
            // It grants the user the ability to playback the content subject to the zero or more restrictions
            // configured in the license and on the PlayRight itself (for playback specific policy).
            // Much of the policy on the PlayRight has to do with output restrictions
            // which control the types of outputs that the content can be played over and
            // any restrictions that must be put in place when using a given output.
            // For example, if the DigitalVideoOnlyContentRestriction is enabled,
            //then the DRM runtime will only allow the video to be displayed over digital outputs
            //(analog video outputs won’t be allowed to pass the content).

            //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience.
            // If the output protections are configured too restrictive,
            // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

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
            // Get the PlayReady license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

            // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
            // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
            // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption
            // to append /? KID =< keyId > to the end of the url when creating the manifest.
            // As a result Widevine license acquisition URL will have KID appended twice,
            // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.

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

            // In this case we only specify Dash streaming protocol in the delivery policy,
            // All other protocols will be blocked from streaming.
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
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


## <a name="next-step"></a><span data-ttu-id="43c1f-203">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="43c1f-203">Next step</span></span>
<span data-ttu-id="43c1f-204">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="43c1f-204">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="43c1f-205">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="43c1f-205">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="43c1f-206">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="43c1f-206">See also</span></span>
<span data-ttu-id="43c1f-207">[CENC with Multi-DRM and Access Control](media-services-cenc-with-multidrm-access-control.md) (CENC többszörös DRM-mel és hozzáférés-vezérléssel)</span><span class="sxs-lookup"><span data-stu-id="43c1f-207">[CENC with Multi-DRM and Access Control](media-services-cenc-with-multidrm-access-control.md)</span></span>

<span data-ttu-id="43c1f-208">[Configure Widevine packaging with AMS](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services) (Widevine-csomagolás konfigurálása az AMS-szel)</span><span class="sxs-lookup"><span data-stu-id="43c1f-208">[Configure Widevine packaging with AMS](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)</span></span>

<span data-ttu-id="43c1f-209">[Announcing Google Widevine license delivery services in Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/) (A Google Widevine-licenctovábbítási szolgáltatás megjelenése az Azure Media Services-ben)</span><span class="sxs-lookup"><span data-stu-id="43c1f-209">[Announcing Google Widevine license delivery services in Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)</span></span>
