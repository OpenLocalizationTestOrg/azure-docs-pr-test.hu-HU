---
title: "AES-128 aaaUsing dinamikus titkosítás és a kulcs kézbesítési szolgáltatás |} Microsoft Docs"
description: "A Microsoft Azure Media Services lehetővé teszi, hogy Ön toodeliver a 128 bites AES titkosítási kulccsal titkosított tartalom. A Media Services hello kulcs kézbesítési szolgáltatás letölti a titkosítási kulcsok tooauthorized felhasználók is biztosít. Ez a témakör bemutatja, hogyan toodynamically titkosítani az AES-128 és hello kulcs kézbesítési szolgáltatás használata."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: cb1b413ec2ba79f7437464099cf72236ab93f312
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a><span data-ttu-id="0d728-105">AES-128 dinamikus titkosítás és a kulcs kézbesítési szolgáltatás használatával</span><span class="sxs-lookup"><span data-stu-id="0d728-105">Using AES-128 dynamic encryption and key delivery service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0d728-106">.NET</span><span class="sxs-lookup"><span data-stu-id="0d728-106">.NET</span></span>](media-services-protect-with-aes128.md)
> * [<span data-ttu-id="0d728-107">Java</span><span class="sxs-lookup"><span data-stu-id="0d728-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="0d728-108">PHP</span><span class="sxs-lookup"><span data-stu-id="0d728-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="0d728-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0d728-109">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="0d728-110">Lásd: [ez](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) videó megtudhatja, hogyan tooprotect médiatartalmak tartalom AES titkosítással.</span><span class="sxs-lookup"><span data-stu-id="0d728-110">See [this](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video for an overview of how tooprotect your Media Content with AES encryption.</span></span>
> 
> 

<span data-ttu-id="0d728-111">A Microsoft Azure Media Services lehetővé teszi toodeliver Http-Live-Streaming (HLS), és zökkenőmentes adatfolyamok titkosítva az Advanced Encryption Standard (AES) (a 128 bites titkosítási kulcsok használatával).</span><span class="sxs-lookup"><span data-stu-id="0d728-111">Microsoft Azure Media Services enables you toodeliver Http-Live-Streaming (HLS) and Smooth Streams encrypted with Advanced Encryption Standard (AES) (using 128-bit encryption keys).</span></span> <span data-ttu-id="0d728-112">A Media Services hello kulcs kézbesítési szolgáltatás letölti a titkosítási kulcsok tooauthorized felhasználók is biztosít.</span><span class="sxs-lookup"><span data-stu-id="0d728-112">Media Services also provides hello Key Delivery service that delivers encryption keys tooauthorized users.</span></span> <span data-ttu-id="0d728-113">Ha azt szeretné, a Media Services tooencrypt egy eszköz tooassociate hello eszközt titkosítási kulcsot kell, és engedélyezési házirendeket hello kulcs is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="0d728-113">If you want for Media Services tooencrypt an asset, you need tooassociate an encryption key with hello asset and also configure authorization policies for hello key.</span></span> <span data-ttu-id="0d728-114">Ha olyan adatfolyamot kell megadni a Windows Media Player van szükség, Media Services használ-e a megadott hello kulcs toodynamically titkosítani az AES titkosítással történik.</span><span class="sxs-lookup"><span data-stu-id="0d728-114">When a stream is requested by a player, Media Services uses hello specified key toodynamically encrypt your content using AES encryption.</span></span> <span data-ttu-id="0d728-115">toodecrypt hello stream, hello player fog igényelni hello kulcs hello kulcs kézbesítési szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="0d728-115">toodecrypt hello stream, hello player will request hello key from hello key delivery service.</span></span> <span data-ttu-id="0d728-116">hello felhasználónak van-e toodecide engedélyezett tooget hello kulcs, hello szolgáltatás értékeli az hello kulcshoz megadott hello engedélyezési házirendeket.</span><span class="sxs-lookup"><span data-stu-id="0d728-116">toodecide whether or not hello user is authorized tooget hello key, hello service evaluates hello authorization policies that you specified for hello key.</span></span>

<span data-ttu-id="0d728-117">A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="0d728-117">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="0d728-118">hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: Nyissa meg, vagy a token korlátozás.</span><span class="sxs-lookup"><span data-stu-id="0d728-118">hello content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span> <span data-ttu-id="0d728-119">hello token korlátozott házirend által a Secure Token Service (STS) kiadott tokennek kell csatolni.</span><span class="sxs-lookup"><span data-stu-id="0d728-119">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="0d728-120">A Media Services hello támogatja a jogkivonatokat [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) formátumú és [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) formátumú.</span><span class="sxs-lookup"><span data-stu-id="0d728-120">Media Services supports tokens in hello [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) format and [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) format.</span></span> <span data-ttu-id="0d728-121">További információkért lásd: [hello tartalomkulcs hitelesítési szabályzatának konfigurálása](media-services-protect-with-aes128.md#configure_key_auth_policy).</span><span class="sxs-lookup"><span data-stu-id="0d728-121">For more information, see [Configure hello content key’s authorization policy](media-services-protect-with-aes128.md#configure_key_auth_policy).</span></span>

<span data-ttu-id="0d728-122">tootake előnye a dinamikus titkosítás toohave többszörös sávszélességű MP4-fájlok vagy többféle sávszélességű Smooth Streaming-forrásfájlokat tartalmazó objektumot kell.</span><span class="sxs-lookup"><span data-stu-id="0d728-122">tootake advantage of dynamic encryption, you need toohave an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="0d728-123">Szükség tooconfigure hello objektumtovábbítási szabályzat hello eszköz (lásd a témakör későbbi részében).</span><span class="sxs-lookup"><span data-stu-id="0d728-123">You also need tooconfigure hello delivery policy for hello asset (described later in this topic).</span></span> <span data-ttu-id="0d728-124">Majd a streamelési URL-cím hello hello formátummegadás alapján, hello Igényalapú Streamelési kiszolgáló biztosítja, hogy hello adatfolyam a rendszer a kiválasztott hello protokollal.</span><span class="sxs-lookup"><span data-stu-id="0d728-124">Then, based on hello format specified in hello streaming URL, hello On-Demand Streaming server will ensure that hello stream is delivered in hello protocol you have chosen.</span></span> <span data-ttu-id="0d728-125">Ennek eredményeképpen csak akkor kell toostore és fizetési hello fájlokat egyetlen tárolási formátumban és a Media Services szolgáltatás elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ hello.</span><span class="sxs-lookup"><span data-stu-id="0d728-125">As a result, you only need toostore and pay for hello files in single storage format and Media Services service will build and serve hello appropriate response based on requests from a client.</span></span>

<span data-ttu-id="0d728-126">Ez a témakör, amely védett médiafájlok továbbításával foglalkoznak hasznos toodevelopers lenne.</span><span class="sxs-lookup"><span data-stu-id="0d728-126">This topic would be useful toodevelopers that work on applications that deliver protected media.</span></span> <span data-ttu-id="0d728-127">hello a témakör bemutatja, hogyan tooconfigure hello-e kulcs kézbesítési szolgáltatás vonatkozó szabályzatokat úgy, hogy csak az arra jogosult ügyfelek kaphassák meg hello titkosítási kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="0d728-127">hello topic shows you how tooconfigure hello key delivery service with authorization policies so that only authorized clients could receive hello encryption keys.</span></span> <span data-ttu-id="0d728-128">Azt is bemutatja, hogyan toouse a dinamikus titkosítás.</span><span class="sxs-lookup"><span data-stu-id="0d728-128">It also shows how toouse dynamic encryption.</span></span>


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a><span data-ttu-id="0d728-129">AES-128, a dinamikus titkosítás és a kulcs kézbesítési szolgáltatás munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="0d728-129">AES-128 Dynamic Encryption and Key Delivery Service Workflow</span></span>

<span data-ttu-id="0d728-130">Az alábbiakban hello általános lépéseket, hogy kell tooperform AES, hello Media Services kulcs szállítási szolgáltatás használatával, és a dinamikus titkosítás használata az eszközök titkosításához.</span><span class="sxs-lookup"><span data-stu-id="0d728-130">hello following are general steps that you would need tooperform when encrypting your assets with AES, using hello Media Services key delivery service, and also using dynamic encryption.</span></span>

1. <span data-ttu-id="0d728-131">[Hozzon létre egy eszközt, majd fájlok feltöltése hello objektumba](media-services-protect-with-aes128.md#create_asset).</span><span class="sxs-lookup"><span data-stu-id="0d728-131">[Create an asset and upload files into hello asset](media-services-protect-with-aes128.md#create_asset).</span></span>
2. <span data-ttu-id="0d728-132">[Hello fájl toohello adaptív sávszélességű MP4-készletet tartalmazó hello objektum kódolása](media-services-protect-with-aes128.md#encode_asset).</span><span class="sxs-lookup"><span data-stu-id="0d728-132">[Encode hello asset containing hello file toohello adaptive bitrate MP4 set](media-services-protect-with-aes128.md#encode_asset).</span></span>
3. <span data-ttu-id="0d728-133">[Hozzon létre egy tartalomkulcsot, és azt társítsa a kódolt hello eszköz](media-services-protect-with-aes128.md#create_contentkey).</span><span class="sxs-lookup"><span data-stu-id="0d728-133">[Create a content key and associate it with hello encoded asset](media-services-protect-with-aes128.md#create_contentkey).</span></span> <span data-ttu-id="0d728-134">A Media Services szolgáltatásban a hello tartalomkulcs tartalmazza a hello objektum titkosítási kulcsát.</span><span class="sxs-lookup"><span data-stu-id="0d728-134">In Media Services, hello content key contains hello asset’s encryption key.</span></span>
4. <span data-ttu-id="0d728-135">[Hello tartalomkulcs hitelesítési szabályzatának konfigurálása](media-services-protect-with-aes128.md#configure_key_auth_policy).</span><span class="sxs-lookup"><span data-stu-id="0d728-135">[Configure hello content key’s authorization policy](media-services-protect-with-aes128.md#configure_key_auth_policy).</span></span> <span data-ttu-id="0d728-136">hello tartalomkulcs-hitelesítési szabályzatot kell állította be, és ahhoz, hogy hello tartalom kulcs toobe kézbesített toohello ügyfél hello ügyfél teljesíti.</span><span class="sxs-lookup"><span data-stu-id="0d728-136">hello content key authorization policy must be configured by you and met by hello client in order for hello content key toobe delivered toohello client.</span></span>
5. <span data-ttu-id="0d728-137">[Az objektum továbbítási szabályzatát hello konfigurálása](media-services-protect-with-aes128.md#configure_asset_delivery_policy).</span><span class="sxs-lookup"><span data-stu-id="0d728-137">[Configure hello delivery policy for an asset](media-services-protect-with-aes128.md#configure_asset_delivery_policy).</span></span> <span data-ttu-id="0d728-138">hello továbbítási szabályzat konfigurációjához tartalmazza: licenckérési URL-cím és-inicializálási vektor (IV) (az AES-128 szükséges hello azonos IV toobe megadott titkosításához és visszafejtéséhez), objektumtovábbítási protokoll (például MPEG DASH, HLS, Smooth Streaming vagy az összes), hello típusa a dinamikus titkosítás (például a boríték vagy a dinamikus titkosítás nélkül).</span><span class="sxs-lookup"><span data-stu-id="0d728-138">hello delivery policy configuration includes: key acquisition URL and Initialization Vector (IV) (AES 128 requires hello same IV toobe supplied when encrypting and decrypting), delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all), hello type of dynamic encryption (for example, envelope or no dynamic encryption).</span></span>

    <span data-ttu-id="0d728-139">Tudta alkalmazni a különböző házirend tooeach protokollt a következő hello azonos eszköz.</span><span class="sxs-lookup"><span data-stu-id="0d728-139">You could apply different policy tooeach protocol on hello same asset.</span></span> <span data-ttu-id="0d728-140">Beállíthat például PlayReady titkosítási tooSmooth/DASH és AES Envelope tooHLS.</span><span class="sxs-lookup"><span data-stu-id="0d728-140">For example, you could apply PlayReady encryption tooSmooth/DASH and AES Envelope tooHLS.</span></span> <span data-ttu-id="0d728-141">Nincsenek megadva a továbbítási szabályzatban protokollok (például hozzáadhat egy szabályzatban, amely csak HLS hello protokoll) le lesz tiltva streaming.</span><span class="sxs-lookup"><span data-stu-id="0d728-141">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as hello protocol) will be blocked from streaming.</span></span> <span data-ttu-id="0d728-142">hello kivétel toothis jelent, ha egyáltalán nem állít be objektumtovábbítási szabályzatot egyáltalán.</span><span class="sxs-lookup"><span data-stu-id="0d728-142">hello exception toothis is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="0d728-143">Ezt követően hello törölje a jelet minden protokoll engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="0d728-143">Then, all protocols will be allowed in hello clear.</span></span>

6. <span data-ttu-id="0d728-144">[Hozzon létre egy OnDemand-kereső](media-services-protect-with-aes128.md#create_locator) a rendezés tooget egy adatfolyam-továbbítási URL-címet.</span><span class="sxs-lookup"><span data-stu-id="0d728-144">[Create an OnDemand locator](media-services-protect-with-aes128.md#create_locator) in order tooget a streaming URL.</span></span>

<span data-ttu-id="0d728-145">hello témakör is látható [hogyan ügyfélalkalmazás is kérhet egy kulcs hello kulcs kézbesítési szolgáltatás](media-services-protect-with-aes128.md#client_request).</span><span class="sxs-lookup"><span data-stu-id="0d728-145">hello topic also shows [how a client application can request a key from hello key delivery service](media-services-protect-with-aes128.md#client_request).</span></span>

<span data-ttu-id="0d728-146">A teljes .NET található [példa](media-services-protect-with-aes128.md#example) hello hello témakör végén.</span><span class="sxs-lookup"><span data-stu-id="0d728-146">You will find a complete .NET [example](media-services-protect-with-aes128.md#example) at hello end of hello topic.</span></span>

<span data-ttu-id="0d728-147">a következő kép hello hello fentiekben leírt munkafolyamatot mutatja be.</span><span class="sxs-lookup"><span data-stu-id="0d728-147">hello following image demonstrates hello workflow described above.</span></span> <span data-ttu-id="0d728-148">Itt hello jogkivonat-hitelesítéshez használt.</span><span class="sxs-lookup"><span data-stu-id="0d728-148">Here hello token is used for authentication.</span></span>

![Védelem 128 bites AES-titkosítással](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

<span data-ttu-id="0d728-150">Ez a témakör többi hello részletes magyarázatokat, kódmintákat és olyan hivatkozásokat tootopics, amelyek bemutatják, hogyan tooachieve hello a fent leírt biztosít.</span><span class="sxs-lookup"><span data-stu-id="0d728-150">hello rest of this topic provides detailed explanations, code examples, and links tootopics that show you how tooachieve hello tasks described above.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="0d728-151">Aktuális korlátozások</span><span class="sxs-lookup"><span data-stu-id="0d728-151">Current limitations</span></span>
<span data-ttu-id="0d728-152">Objektumtovábbítási szabályzat hozzáadásakor vagy módosításakor törölnie kell az ahhoz tartozó meglévő lokátort (ha van), majd létre kell hoznia egy új lokátort.</span><span class="sxs-lookup"><span data-stu-id="0d728-152">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>

## <span data-ttu-id="0d728-153"><a id="create_asset"></a>Hozzon létre egy eszközt, majd fájlok feltöltése hello objektumba</span><span class="sxs-lookup"><span data-stu-id="0d728-153"><a id="create_asset"></a>Create an asset and upload files into hello asset</span></span>
<span data-ttu-id="0d728-154">A sorrend toomanage, kódolásához és streameléséhez a videók, akkor először fel kell töltenie a tartalom a Microsoft Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="0d728-154">In order toomanage, encode, and stream your videos, you must first upload your content into Microsoft Azure Media Services.</span></span> <span data-ttu-id="0d728-155">A feltöltést követően a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő.</span><span class="sxs-lookup"><span data-stu-id="0d728-155">Once uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span> 

<span data-ttu-id="0d728-156">További információk: [Upload Files into a Media Services account](media-services-dotnet-upload-files.md) (Fájlok feltöltése a Media Services-fiókba).</span><span class="sxs-lookup"><span data-stu-id="0d728-156">For detailed information, see [Upload Files into a Media Services account](media-services-dotnet-upload-files.md).</span></span>

## <span data-ttu-id="0d728-157"><a id="encode_asset"></a>Hello eszköz tartalmazó hello fájl toohello adaptív sávszélességű MP4 típusú beállításkészlettel kódolása</span><span class="sxs-lookup"><span data-stu-id="0d728-157"><a id="encode_asset"></a>Encode hello asset containing hello file toohello adaptive bitrate MP4 set</span></span>
<span data-ttu-id="0d728-158">A dinamikus titkosítás szüksége toocreate többszörös sávszélességű MP4-fájlok vagy többféle sávszélességű Smooth Streaming-forrásfájlokat tartalmazó objektumot.</span><span class="sxs-lookup"><span data-stu-id="0d728-158">With dynamic encryption all you need is toocreate an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="0d728-159">Majd hello hello jegyzékben megadott formátumnak alapján kérelem darabolható, vagy igény szerinti adatfolyam-kiszolgáló biztosítja a megjelenő hello adatfolyam a kiválasztott protokollal hello hello.</span><span class="sxs-lookup"><span data-stu-id="0d728-159">Then, based on hello specified format in hello manifest or fragment request, hello On-Demand Streaming server will ensure that you receive hello stream in hello protocol you have chosen.</span></span> <span data-ttu-id="0d728-160">Ennek eredményeképpen csak akkor kell toostore és fizetési hello fájlokat egyetlen tárolási formátumban és a Media Services szolgáltatás elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ hello.</span><span class="sxs-lookup"><span data-stu-id="0d728-160">As a result, you only need toostore and pay for hello files in single storage format and Media Services service will build and serve hello appropriate response based on requests from a client.</span></span> <span data-ttu-id="0d728-161">További információkért lásd: hello [dinamikus becsomagolás áttekintése](media-services-dynamic-packaging-overview.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="0d728-161">For more information, see hello [Dynamic Packaging Overview](media-services-dynamic-packaging-overview.md) topic.</span></span>

>[!NOTE]
><span data-ttu-id="0d728-162">Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="0d728-162">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="0d728-163">a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="0d728-163">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 
>
><span data-ttu-id="0d728-164">Emellett toobe képes toouse dinamikus csomagolás és a dinamikus titkosítás az eszköz tartalmaznia kell egy adaptív sávszélességű MP4 vagy Smooth Streaming-fájlsorozattá készletét.</span><span class="sxs-lookup"><span data-stu-id="0d728-164">Also, toobe able toouse dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>

<span data-ttu-id="0d728-165">Útmutatást tooencode, lásd: [hogyan tooencode keresztül Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span><span class="sxs-lookup"><span data-stu-id="0d728-165">For instructions on how tooencode, see [How tooencode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>

## <span data-ttu-id="0d728-166"><a id="create_contentkey"></a>Hozzon létre egy tartalomkulcsot, és azt társítsa a kódolt hello eszköz</span><span class="sxs-lookup"><span data-stu-id="0d728-166"><a id="create_contentkey"></a>Create a content key and associate it with hello encoded asset</span></span>
<span data-ttu-id="0d728-167">A Media Services szolgáltatásban hello tartalomkulcs tartalmazza, amelyet az eszköz tooencrypt hello kulcs együtt.</span><span class="sxs-lookup"><span data-stu-id="0d728-167">In Media Services, hello content key contains hello key that you want tooencrypt an asset with.</span></span>

<span data-ttu-id="0d728-168">További információk: [Create content key](media-services-dotnet-create-contentkey.md) (Tartalomkulcs létrehozása).</span><span class="sxs-lookup"><span data-stu-id="0d728-168">For detailed information, see [Create content key](media-services-dotnet-create-contentkey.md).</span></span>

## <span data-ttu-id="0d728-169"><a id="configure_key_auth_policy"></a>Hello tartalomkulcs hitelesítési szabályzatának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0d728-169"><a id="configure_key_auth_policy"></a>Configure hello content key’s authorization policy</span></span>
<span data-ttu-id="0d728-170">A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="0d728-170">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="0d728-171">hello tartalomkulcs-hitelesítési szabályzatot kell Ön által konfigurált és érheti el, ha hello ügyfél (a lejátszó) ahhoz, hogy hello kulcs toobe toohello ügyfél kézbesíteni.</span><span class="sxs-lookup"><span data-stu-id="0d728-171">hello content key authorization policy must be configured by you and met by hello client (player) in order for hello key toobe delivered toohello client.</span></span> <span data-ttu-id="0d728-172">hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: Nyissa meg a, lexikális elem: korlátozás vagy IP-korlátozás.</span><span class="sxs-lookup"><span data-stu-id="0d728-172">hello content key authorization policy could have one or more authorization restrictions: open, token restriction, or IP restriction.</span></span>

<span data-ttu-id="0d728-173">További információk: [A tartalomkulcs hitelesítési szabályzatának létrehozása](media-services-dotnet-configure-content-key-auth-policy.md).</span><span class="sxs-lookup"><span data-stu-id="0d728-173">For detailed information, see [Configure Content Key Authorization Policy](media-services-dotnet-configure-content-key-auth-policy.md).</span></span>

## <span data-ttu-id="0d728-174"><a id="configure_asset_delivery_policy"></a>Objektumtovábbítási szabályzat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0d728-174"><a id="configure_asset_delivery_policy"></a>Configure asset delivery policy</span></span>
<span data-ttu-id="0d728-175">Az objektum továbbítási szabályzatát hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0d728-175">Configure hello delivery policy for your asset.</span></span> <span data-ttu-id="0d728-176">Néhány dolog, amely az eszköz továbbítási szabályzat konfigurációjához hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="0d728-176">Some things that hello asset delivery policy configuration includes:</span></span>

* <span data-ttu-id="0d728-177">hello kulcs licenckérési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="0d728-177">hello Key acquisition URL.</span></span> 
* <span data-ttu-id="0d728-178">hello inicializálási vektor (IV) toouse hello boríték titkosításhoz.</span><span class="sxs-lookup"><span data-stu-id="0d728-178">hello Initialization Vector (IV) toouse for hello envelope encryption.</span></span> <span data-ttu-id="0d728-179">AES-128 hello azonos IV toobe megadni, ha az titkosításához és visszafejtéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="0d728-179">AES 128 requires hello same IV toobe supplied when encrypting and decrypting.</span></span> 
* <span data-ttu-id="0d728-180">hello objektumtovábbítási protokoll (például MPEG DASH, HLS, Smooth Streaming vagy all).</span><span class="sxs-lookup"><span data-stu-id="0d728-180">hello asset delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all).</span></span>
* <span data-ttu-id="0d728-181">a dinamikus titkosítás (például AES envelope) hello típus vagy a dinamikus titkosítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="0d728-181">hello type of dynamic encryption (for example, AES envelope) or no dynamic encryption.</span></span> 

<span data-ttu-id="0d728-182">További információk: [Objektumtovábbítási szabályzat konfigurálása ](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="0d728-182">For detailed information, see [Configure asset delivery policy ](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <span data-ttu-id="0d728-183"><a id="create_locator"></a>Hozzon létre egy OnDemand-lokátor a rendelés tooget streamelési URL-cím</span><span class="sxs-lookup"><span data-stu-id="0d728-183"><a id="create_locator"></a>Create an OnDemand streaming locator in order tooget a streaming URL</span></span>
<span data-ttu-id="0d728-184">Szüksége lesz tooprovide a felhasználó az URL-címe Smooth, DASH vagy HLS streamelési hello.</span><span class="sxs-lookup"><span data-stu-id="0d728-184">You will need tooprovide your user with hello streaming URL for Smooth, DASH or HLS.</span></span>

> [!NOTE]
> <span data-ttu-id="0d728-185">Objektumtovábbítási szabályzat hozzáadásakor vagy módosításakor törölnie kell az ahhoz tartozó meglévő lokátort (ha van), majd létre kell hoznia egy új lokátort.</span><span class="sxs-lookup"><span data-stu-id="0d728-185">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
> 
> 

<span data-ttu-id="0d728-186">Hogyan toopublish egy eszköz és -buildek a streamelési URL-cím: kapcsolatos utasításokat [streamelési URL-cím összeállítása](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="0d728-186">For instructions on how toopublish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="get-a-test-token"></a><span data-ttu-id="0d728-187">Tesztjogkivonat lekérése</span><span class="sxs-lookup"><span data-stu-id="0d728-187">Get a test token</span></span>
<span data-ttu-id="0d728-188">Tesztjogkivonat lekérése hello hello kulcshitelesítési szabályzatban használt jogkivonat korlátozás alapján.</span><span class="sxs-lookup"><span data-stu-id="0d728-188">Get a test token based on hello token restriction that was used for hello key authorization policy.</span></span>

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on hello data in hello given TokenRestrictionTemplate.
    //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
    //so you have tooadd it in front of hello token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);

<span data-ttu-id="0d728-189">Használhatja a hello [AMS-lejátszóval](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest az adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="0d728-189">You can use hello [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest your stream.</span></span>

## <span data-ttu-id="0d728-190"><a id="client_request"></a>Hogyan is az ügyfél kérhet egy kulcs hello kulcs kézbesítési szolgáltatás?</span><span class="sxs-lookup"><span data-stu-id="0d728-190"><a id="client_request"></a>How can your client request a key from hello key delivery service?</span></span>
<span data-ttu-id="0d728-191">Hello a korábbi lépésben hello URL-CÍMMEL tooa jegyzékfájl kialakítani.</span><span class="sxs-lookup"><span data-stu-id="0d728-191">In hello previous step, you constructed hello URL that points tooa manifest file.</span></span> <span data-ttu-id="0d728-192">Az ügyfélnek kell tooextract hello szükséges információkat hello streaming rendelés toomake egy kérelem toohello kulcs kézbesítési szolgáltatás a jegyzékfájlt.</span><span class="sxs-lookup"><span data-stu-id="0d728-192">Your client needs tooextract hello necessary information from hello streaming manifest files in order toomake a request toohello key delivery service.</span></span>

### <a name="manifest-files"></a><span data-ttu-id="0d728-193">Fájlok</span><span class="sxs-lookup"><span data-stu-id="0d728-193">Manifest files</span></span>
<span data-ttu-id="0d728-194">hello ügyfélnek kell tooextract hello URL-címe (tartalmazó is tartalomkulcsot azonosítója (kid)) értéket hello jegyzékfájlt.</span><span class="sxs-lookup"><span data-stu-id="0d728-194">hello client needs tooextract hello URL (that also contains content key Id (kid)) value from hello manifest file.</span></span> <span data-ttu-id="0d728-195">hello ügyfél fogják megpróbálni tooget hello titkosítási kulcs hello kulcs kézbesítési szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="0d728-195">hello client will then try tooget hello encryption key from hello key delivery service.</span></span> <span data-ttu-id="0d728-196">hello ügyfél is tooextract hello IV értéket kell, és használja azt a következő kódrészletet hello stream.hello visszafejtéséhez látható hello <Protection> hello Smooth Streaming jegyzékfájl elemet.</span><span class="sxs-lookup"><span data-stu-id="0d728-196">hello client also needs tooextract hello IV value and use it do decrypt hello stream.hello following snippet shows hello <Protection> element of hello Smooth Streaming manifest.</span></span>

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

<span data-ttu-id="0d728-197">HLS hello esetben hello legfelső szintű jegyzékfájl lebontva szegmens fájlokat.</span><span class="sxs-lookup"><span data-stu-id="0d728-197">In hello case of HLS, hello root manifest is broken into segment files.</span></span> 

<span data-ttu-id="0d728-198">Például hello legfelső szintű jegyzékfájl van: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl), és a szegmens fájlnevek listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0d728-198">For example, hello root manifest is: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) and it contains a list of segment file names.</span></span>

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

<span data-ttu-id="0d728-199">Ha hello szegmens fájlok megnyitása szövegszerkesztőben (például http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should tartalmaz #EXT-X-kulcsot, amely azt jelzi, hogy hello fájl titkosítva van.</span><span class="sxs-lookup"><span data-stu-id="0d728-199">If you open one of hello segment files in text editor (for example, http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should contain #EXT-X-KEY which indicates that hello file is encrypted.</span></span>

    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST

>[!NOTE] 
><span data-ttu-id="0d728-200">Ha azt tervezi, tooplay az AES titkosított HLS a Safari című [ebben a blogban](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span><span class="sxs-lookup"><span data-stu-id="0d728-200">If you are planning tooplay an AES encrypted HLS in Safari, see [this blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span></span>

### <a name="request-hello-key-from-hello-key-delivery-service"></a><span data-ttu-id="0d728-201">Hello kulcs hello kulcs kézbesítési szolgáltatás kérése</span><span class="sxs-lookup"><span data-stu-id="0d728-201">Request hello key from hello key delivery service</span></span>

<span data-ttu-id="0d728-202">hello következő kód bemutatja, hogyan toosend egy kérelem toohello Media Services kulcs kézbesítési szolgáltatás egy kulcs kézbesítés URI-azonosítóhoz (hello jegyzékfájl kinyert) használata, valamint a jogkivonatot (a jelen témakör nem szolgáltatással kapcsolatban, hogyan tooget egyszerű webes jogkivonatokat kérhetnek a Secure Token Service).</span><span class="sxs-lookup"><span data-stu-id="0d728-202">hello following code shows how toosend a request toohello Media Services key delivery service using a key delivery Uri (that was extracted from hello manifest) and a token (this topic does not talk about how tooget Simple Web Tokens from a Secure Token Service).</span></span>

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);

        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;

        var response = request.GetResponse();

        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }

        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();

        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }

## <a name="protect-your-content-with-aes-128-using-net"></a><span data-ttu-id="0d728-203">Az AES-128 tartalomvédelemre .NET használatával</span><span class="sxs-lookup"><span data-stu-id="0d728-203">Protect your content with AES-128 using .NET</span></span>

### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="0d728-204">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0d728-204">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="0d728-205">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="0d728-205">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="0d728-206">Adja hozzá a következő elemek túl hello**appSettings** az app.config fájlban meghatározott:</span><span class="sxs-lookup"><span data-stu-id="0d728-206">Add hello following elements too**appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <span data-ttu-id="0d728-207"><a id="example"></a>Példa</span><span class="sxs-lookup"><span data-stu-id="0d728-207"><a id="example"></a>Example</span></span>

<span data-ttu-id="0d728-208">Írja felül a Program.cs fájlban hello kód hello kód itt látható.</span><span class="sxs-lookup"><span data-stu-id="0d728-208">Overwrite hello code in your Program.cs file with hello code shown in this section.</span></span>
 
>[!NOTE]
><span data-ttu-id="0d728-209">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="0d728-209">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="0d728-210">Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek.</span><span class="sxs-lookup"><span data-stu-id="0d728-210">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="0d728-211">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="0d728-211">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="0d728-212">Győződjön meg arról, hogy tooupdate változók toopoint toofolders ahol a bemeneti fájlok találhatók.</span><span class="sxs-lookup"><span data-stu-id="0d728-212">Make sure tooupdate variables toopoint toofolders where your input files are located.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Security.Cryptography;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace AESDynamicEncryptionAndKeyDeliverySvc
    {
        class Program
        {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // A Uri describing hello issuer of hello token.  
        // Must match hello value in hello token for hello token toobe considered valid.
        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        // hello Audience or Scope of hello token.  
        // Must match hello value in hello token for hello token toobe considered valid.
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

            IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
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

            // Generate a test token based on hello data in hello given TokenRestrictionTemplate.
            // Note, you need toopass hello key id Guid because we specified 
            // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

            //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
            //so you have tooadd it in front of hello token string. 
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
            Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use hello bit.ly/aesplayer Flash player tootest hello URL 
            // (with open authorization policy). 
            // Paste hello URL and click hello Update button tooplay hello video. 
            //
            string URL = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
            Console.WriteLine();
            Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
            Console.WriteLine();

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
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);


            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass tooit hello name of hello 
            // processor toouse for hello specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with hello encoding details, using a string preset.
            // In this case "Adaptive Streaming" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify hello input asset toobe encoded.
            task.InputAssets.Add(asset);
            // Add an output asset toocontain hello results of hello job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means hello output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }

        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }

        static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
        {
            // Create envelope encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                keyId,
                contentKey,
                "ContentKey",
                ContentKeyType.EnvelopeEncryption);

            // Associate hello key with hello asset.
            asset.ContentKeys.Add(key);

            return key;
        }

        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {
            // Create ContentKeyAuthorizationPolicy with Open restrictions 
            // and create authorization policy             
            IContentKeyAuthorizationPolicy policy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Open Authorization Policy").Result;

            List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

            ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
            Name = "HLS Open Authorization Policy",
            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
            Requirements = null // no requirements needed for HLS
        };

            restrictions.Add(restriction);

            IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy",
            ContentKeyDeliveryType.BaselineHttp,
            restrictions,
            "");

            policy.Options.Add(policyOption);

            // Add ContentKeyAutorizationPolicy tooContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            IContentKeyAuthorizationPolicy policy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("HLS token restricted authorization policy").Result;

            List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

            ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "Token Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString
            };

            restrictions.Add(restriction);

            //You could have multiple options 
            IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "Token option for HLS",
            ContentKeyDeliveryType.BaselineHttp,
            restrictions,
            null  // no key delivery data is needed for HLS
            );

            policy.Options.Add(policyOption);

            // Add ContentKeyAutorizationPolicy tooContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);

            return tokenTemplateString;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);

            string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));

            // When configuring delivery policy, you can choose tooassociate it
            // with a key acquisition URL that has a KID appended or
            // or a key acquisition URL that does not have a KID appended  
            // in which case a content key can be reused. 

            // EnvelopeKeyAcquisitionUrl:  contains a key ID in hello key URL.
            // EnvelopeBaseKeyAcquisitionUrl:  hello URL does not contains a key ID

            // hello following policy configuration specifies: 
            // key url that will have KID=<Guid> appended toohello envelope and
            // hello Initialization Vector (IV) toouse for hello envelope encryption.

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
            };

            IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
                AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                assetDeliveryPolicyConfiguration);

            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
        }

        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference toohello streaming manifest file from hello  
            // collection of files in hello asset. 

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                EndsWith(".ism")).
                FirstOrDefault();

            // Create a 30-day readonly access policy. 
            // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
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

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
        }

        static private byte[] GetRandomBuffer(int size)
        {
            byte[] randomBytes = new byte[size];
            using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
            {
            rng.GetBytes(randomBytes);
            }

            return randomBytes;
        }
        }
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="0d728-213">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="0d728-213">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0d728-214">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="0d728-214">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

