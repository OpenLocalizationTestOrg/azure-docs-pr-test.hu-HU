---
title: "AES-128 dinamikus titkosítás és a kulcs kézbesítési szolgáltatás használatával |} Microsoft Docs"
description: "A Microsoft Azure Media Services lehetővé teszi, hogy a 128 bites AES titkosítási kulccsal titkosított tartalom. Media Services is biztosít a kulcs kézbesítési szolgáltatás letölti a titkosítási kulcsok engedéllyel rendelkező felhasználók számára. Ez a témakör bemutatja, hogyan dinamikusan titkosítani az AES-128, és a kulcs kézbesítési szolgáltatás használata."
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
ms.openlocfilehash: ae1b36c26e688e74eb8fcc1a4cdbd3be0c014c08
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a><span data-ttu-id="3b340-105">AES-128 dinamikus titkosítás és a kulcs kézbesítési szolgáltatás használatával</span><span class="sxs-lookup"><span data-stu-id="3b340-105">Using AES-128 dynamic encryption and key delivery service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3b340-106">.NET</span><span class="sxs-lookup"><span data-stu-id="3b340-106">.NET</span></span>](media-services-protect-with-aes128.md)
> * [<span data-ttu-id="3b340-107">Java</span><span class="sxs-lookup"><span data-stu-id="3b340-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="3b340-108">PHP</span><span class="sxs-lookup"><span data-stu-id="3b340-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="3b340-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3b340-109">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="3b340-110">Lásd: [ez](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) videó megtudhatja, hogyan védi meg a média tartalom AES titkosítással.</span><span class="sxs-lookup"><span data-stu-id="3b340-110">See [this](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video for an overview of how to protect your Media Content with AES encryption.</span></span>
> 
> 

<span data-ttu-id="3b340-111">A Microsoft Azure Media Services lehetővé teszi, hogy Http-Live-Streaming (HLS), és zökkenőmentes adatfolyamok titkosítva az Advanced Encryption Standard (AES) (a 128 bites titkosítási kulcsok használatával).</span><span class="sxs-lookup"><span data-stu-id="3b340-111">Microsoft Azure Media Services enables you to deliver Http-Live-Streaming (HLS) and Smooth Streams encrypted with Advanced Encryption Standard (AES) (using 128-bit encryption keys).</span></span> <span data-ttu-id="3b340-112">Media Services is biztosít a kulcs kézbesítési szolgáltatás letölti a titkosítási kulcsok engedéllyel rendelkező felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="3b340-112">Media Services also provides the Key Delivery service that delivers encryption keys to authorized users.</span></span> <span data-ttu-id="3b340-113">Ha azt szeretné, a Media Services az objektum titkosítására, meg kell rendelje hozzá egy titkosítási kulcsot az eszköz és engedélyezési házirendeket, a kulcs is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="3b340-113">If you want for Media Services to encrypt an asset, you need to associate an encryption key with the asset and also configure authorization policies for the key.</span></span> <span data-ttu-id="3b340-114">Ha olyan adatfolyamot kell megadni a Windows Media Player van szükség, a Media Services megadott kulcsot használja az dinamikusan titkosítani az AES titkosítással.</span><span class="sxs-lookup"><span data-stu-id="3b340-114">When a stream is requested by a player, Media Services uses the specified key to dynamically encrypt your content using AES encryption.</span></span> <span data-ttu-id="3b340-115">Az adatfolyam visszafejtése, a Windows Media player a kulcs kézbesítési szolgáltatás fog igényelni a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="3b340-115">To decrypt the stream, the player will request the key from the key delivery service.</span></span> <span data-ttu-id="3b340-116">Döntse el, hogy a felhasználó jogosult-e a kulcs eléréséhez, hogy a szolgáltatás értékeli az engedélyezési házirendeket, amelyek a kulcshoz megadott.</span><span class="sxs-lookup"><span data-stu-id="3b340-116">To decide whether or not the user is authorized to get the key, the service evaluates the authorization policies that you specified for the key.</span></span>

<span data-ttu-id="3b340-117">A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="3b340-117">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="3b340-118">A tartalomkulcs-hitelesítési szabályzat egy vagy több hitelesítési korlátozást tartalmazhat: ezek lehetnek nyitott vagy jogkivonat-korlátozások.</span><span class="sxs-lookup"><span data-stu-id="3b340-118">The content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span> <span data-ttu-id="3b340-119">A tokennel korlátozott szabályzatokhoz a Secure Token Service (Biztonsági jegykiadó szolgáltatás, STS) által kiadott tokennek kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="3b340-119">The token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="3b340-120">A Media Services a [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) és a [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) formátumú tokeneket támogatja.</span><span class="sxs-lookup"><span data-stu-id="3b340-120">Media Services supports tokens in the [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) format and [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) format.</span></span> <span data-ttu-id="3b340-121">További információkért lásd: [a tartalomkulcs hitelesítési szabályzatának konfigurálása](media-services-protect-with-aes128.md#configure_key_auth_policy).</span><span class="sxs-lookup"><span data-stu-id="3b340-121">For more information, see [Configure the content key’s authorization policy](media-services-protect-with-aes128.md#configure_key_auth_policy).</span></span>

<span data-ttu-id="3b340-122">A dinamikus titkosítás által nyújtott előnyök kihasználásához többszörös sávszélességű MP4-fájlokat vagy Smooth Streaming-forrásfájlokat tartalmazó objektummal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="3b340-122">To take advantage of dynamic encryption, you need to have an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="3b340-123">Azt is konfigurálnia kell az eszköz (lásd a témakör későbbi részében) továbbítási szabályzatát.</span><span class="sxs-lookup"><span data-stu-id="3b340-123">You also need to configure the delivery policy for the asset (described later in this topic).</span></span> <span data-ttu-id="3b340-124">Ezt követően az igényalapú streamelési kiszolgáló a streamelési URL-címben megadott formátumnak megfelelően gondoskodik arról, hogy a rendszer a kiválasztott protokollal továbbítsa a streamet.</span><span class="sxs-lookup"><span data-stu-id="3b340-124">Then, based on the format specified in the streaming URL, the On-Demand Streaming server will ensure that the stream is delivered in the protocol you have chosen.</span></span> <span data-ttu-id="3b340-125">Így elég egyetlen tárolási formátumban tárolni a fájlokat (és kifizetni a tárhelyüket), a Media Services szolgáltatás elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ.</span><span class="sxs-lookup"><span data-stu-id="3b340-125">As a result, you only need to store and pay for the files in single storage format and Media Services service will build and serve the appropriate response based on requests from a client.</span></span>

<span data-ttu-id="3b340-126">Ez a témakör hasznos lehet a fejlesztők számára, amely védett médiafájlok továbbításával foglalkoznak.</span><span class="sxs-lookup"><span data-stu-id="3b340-126">This topic would be useful to developers that work on applications that deliver protected media.</span></span> <span data-ttu-id="3b340-127">A témakörben elsajátíthatja, hogy a kulcs kézbesítési szolgáltatás konfigurálása engedélyezési házirendeket, hogy csak az arra jogosult ügyfelek kaphassák meg a titkosítási kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="3b340-127">The topic shows you how to configure the key delivery service with authorization policies so that only authorized clients could receive the encryption keys.</span></span> <span data-ttu-id="3b340-128">Azt is bemutatja, hogyan dinamikus titkosítás használatához.</span><span class="sxs-lookup"><span data-stu-id="3b340-128">It also shows how to use dynamic encryption.</span></span>


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a><span data-ttu-id="3b340-129">AES-128, a dinamikus titkosítás és a kulcs kézbesítési szolgáltatás munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="3b340-129">AES-128 Dynamic Encryption and Key Delivery Service Workflow</span></span>

<span data-ttu-id="3b340-130">A következőkben általános lépéseket kell végrehajtani, ha AES, a Media Services kulcs kézbesítési szolgáltatás segítségével, és a dinamikus titkosítás használata az eszközök titkosításához.</span><span class="sxs-lookup"><span data-stu-id="3b340-130">The following are general steps that you would need to perform when encrypting your assets with AES, using the Media Services key delivery service, and also using dynamic encryption.</span></span>

1. <span data-ttu-id="3b340-131">[Hozzon létre egy eszközt, majd fájlok feltöltése az objektumba](media-services-protect-with-aes128.md#create_asset).</span><span class="sxs-lookup"><span data-stu-id="3b340-131">[Create an asset and upload files into the asset](media-services-protect-with-aes128.md#create_asset).</span></span>
2. <span data-ttu-id="3b340-132">[A fájl az adaptív sávszélességű MP4-készletet tartalmazó objektum kódolása](media-services-protect-with-aes128.md#encode_asset).</span><span class="sxs-lookup"><span data-stu-id="3b340-132">[Encode the asset containing the file to the adaptive bitrate MP4 set](media-services-protect-with-aes128.md#encode_asset).</span></span>
3. <span data-ttu-id="3b340-133">[Hozzon létre egy tartalomkulcsot, majd társítsa a kódolt objektumhoz](media-services-protect-with-aes128.md#create_contentkey).</span><span class="sxs-lookup"><span data-stu-id="3b340-133">[Create a content key and associate it with the encoded asset](media-services-protect-with-aes128.md#create_contentkey).</span></span> <span data-ttu-id="3b340-134">A Media Services szolgáltatásban a tartalomkulcs tartalmazza az objektum titkosítási kulcsát.</span><span class="sxs-lookup"><span data-stu-id="3b340-134">In Media Services, the content key contains the asset’s encryption key.</span></span>
4. <span data-ttu-id="3b340-135">[A tartalomkulcs hitelesítési szabályzatának konfigurálása](media-services-protect-with-aes128.md#configure_key_auth_policy).</span><span class="sxs-lookup"><span data-stu-id="3b340-135">[Configure the content key’s authorization policy](media-services-protect-with-aes128.md#configure_key_auth_policy).</span></span> <span data-ttu-id="3b340-136">Ahhoz, hogy az ügyfél megkaphassa a tartalomkulcsot, Önnek be kell állítania a tartalomkulcs-hitelesítési szabályzatot, amelynek az ügyfélnek meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="3b340-136">The content key authorization policy must be configured by you and met by the client in order for the content key to be delivered to the client.</span></span>
5. <span data-ttu-id="3b340-137">[Konfigurálja az az objektum továbbítási szabályzatát](media-services-protect-with-aes128.md#configure_asset_delivery_policy).</span><span class="sxs-lookup"><span data-stu-id="3b340-137">[Configure the delivery policy for an asset](media-services-protect-with-aes128.md#configure_asset_delivery_policy).</span></span> <span data-ttu-id="3b340-138">A továbbítási szabályzat konfigurációjához tartalmazza: licenckérési URL-cím és-inicializálási vektor (IV) (az AES-128 szükséges adni, ha titkosítása és visszafejtése azonos IV), objektumtovábbítási protokoll (például MPEG DASH, HLS, Smooth Streaming vagy az összes), a a dinamikus titkosítás (például a boríték vagy a dinamikus titkosítás nélkül).</span><span class="sxs-lookup"><span data-stu-id="3b340-138">The delivery policy configuration includes: key acquisition URL and Initialization Vector (IV) (AES 128 requires the same IV to be supplied when encrypting and decrypting), delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all), the type of dynamic encryption (for example, envelope or no dynamic encryption).</span></span>

    <span data-ttu-id="3b340-139">Az adott objektum különböző protokolljaira akár eltérő szabályzatokat is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="3b340-139">You could apply different policy to each protocol on the same asset.</span></span> <span data-ttu-id="3b340-140">Beállíthatja például, hogy a PlayReady-titkosítás csak a Smooth/DASH-re vonatkozzon, az AES Envelope pedig csak a HLS-re.</span><span class="sxs-lookup"><span data-stu-id="3b340-140">For example, you could apply PlayReady encryption to Smooth/DASH and AES Envelope to HLS.</span></span> <span data-ttu-id="3b340-141">A továbbítási szabályzatban meg nem határozott protokollok streameléshez való használatát a rendszer nem engedélyezi (ilyen lehet például, ha csupán egyetlen szabályzatot állít be, amely kizárólag a HLS-protokoll használatát tartalmazza).</span><span class="sxs-lookup"><span data-stu-id="3b340-141">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as the protocol) will be blocked from streaming.</span></span> <span data-ttu-id="3b340-142">Kivételt jelent, ha egyáltalán nem állít be objektumtovábbítási szabályzatot.</span><span class="sxs-lookup"><span data-stu-id="3b340-142">The exception to this is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="3b340-143">Ebben az esetben a rendszer az összes protokollt engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="3b340-143">Then, all protocols will be allowed in the clear.</span></span>

6. <span data-ttu-id="3b340-144">[Hozzon létre egy OnDemand-kereső](media-services-protect-with-aes128.md#create_locator) egy adatfolyam-továbbítási URL-cím beszerzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="3b340-144">[Create an OnDemand locator](media-services-protect-with-aes128.md#create_locator) in order to get a streaming URL.</span></span>

<span data-ttu-id="3b340-145">Ez a témakör azt is bemutatja [hogyan ügyfélalkalmazás is kérhet egy kulcsot a fő kézbesítési szolgáltatás](media-services-protect-with-aes128.md#client_request).</span><span class="sxs-lookup"><span data-stu-id="3b340-145">The topic also shows [how a client application can request a key from the key delivery service](media-services-protect-with-aes128.md#client_request).</span></span>

<span data-ttu-id="3b340-146">A teljes .NET található [példa](media-services-protect-with-aes128.md#example) a témakör végén.</span><span class="sxs-lookup"><span data-stu-id="3b340-146">You will find a complete .NET [example](media-services-protect-with-aes128.md#example) at the end of the topic.</span></span>

<span data-ttu-id="3b340-147">Az alábbi képen a fentiekben leírt munkafolyamatot láthatja.</span><span class="sxs-lookup"><span data-stu-id="3b340-147">The following image demonstrates the workflow described above.</span></span> <span data-ttu-id="3b340-148">Itt a tokenes hitelesítést használtuk.</span><span class="sxs-lookup"><span data-stu-id="3b340-148">Here the token is used for authentication.</span></span>

![Védelem 128 bites AES-titkosítással](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

<span data-ttu-id="3b340-150">A témakör további részében részletes magyarázatokat, kódmintákat és olyan témakörökre mutató hivatkozásokat talál, amelyek segítenek elérni a fent leírt célokat.</span><span class="sxs-lookup"><span data-stu-id="3b340-150">The rest of this topic provides detailed explanations, code examples, and links to topics that show you how to achieve the tasks described above.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="3b340-151">Aktuális korlátozások</span><span class="sxs-lookup"><span data-stu-id="3b340-151">Current limitations</span></span>
<span data-ttu-id="3b340-152">Objektumtovábbítási szabályzat hozzáadásakor vagy módosításakor törölnie kell az ahhoz tartozó meglévő lokátort (ha van), majd létre kell hoznia egy új lokátort.</span><span class="sxs-lookup"><span data-stu-id="3b340-152">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>

## <span data-ttu-id="3b340-153"><a id="create_asset"></a>Hozzon létre egy eszközt, majd fájlok feltöltése az objektumba</span><span class="sxs-lookup"><span data-stu-id="3b340-153"><a id="create_asset"></a>Create an asset and upload files into the asset</span></span>
<span data-ttu-id="3b340-154">A videók kezeléséhez, kódolásához és streameléséhez először fel kell töltenie tartalmait a Microsoft Azure Media Services szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="3b340-154">In order to manage, encode, and stream your videos, you must first upload your content into Microsoft Azure Media Services.</span></span> <span data-ttu-id="3b340-155">A feltöltést követően tartalmai a biztonságos felhőtárhelyre kerülnek további feldolgozás és streamelés céljából.</span><span class="sxs-lookup"><span data-stu-id="3b340-155">Once uploaded, your content is stored securely in the cloud for further processing and streaming.</span></span> 

<span data-ttu-id="3b340-156">További információk: [Upload Files into a Media Services account](media-services-dotnet-upload-files.md) (Fájlok feltöltése a Media Services-fiókba).</span><span class="sxs-lookup"><span data-stu-id="3b340-156">For detailed information, see [Upload Files into a Media Services account](media-services-dotnet-upload-files.md).</span></span>

## <span data-ttu-id="3b340-157"><a id="encode_asset"></a>Az adaptív sávszélességű MP4 típusú beállításkészlettel fájlt tartalmazó objektum kódolása</span><span class="sxs-lookup"><span data-stu-id="3b340-157"><a id="encode_asset"></a>Encode the asset containing the file to the adaptive bitrate MP4 set</span></span>
<span data-ttu-id="3b340-158">A dinamikus titkosítás segítségével mindössze egy többszörös sávszélességű MP4-fájlokat vagy Smooth Streaming-forrásfájlokat tartalmazó objektumot kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="3b340-158">With dynamic encryption all you need is to create an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="3b340-159">Ezt követően a jegyzék vagy töredék kérelem, az Igényalapú Streamelési megadott formátumnak megfelelően kiszolgáló biztosítja az adatfolyam kapni a kiválasztott protokollal.</span><span class="sxs-lookup"><span data-stu-id="3b340-159">Then, based on the specified format in the manifest or fragment request, the On-Demand Streaming server will ensure that you receive the stream in the protocol you have chosen.</span></span> <span data-ttu-id="3b340-160">Így elég egyetlen tárolási formátumban tárolni a fájlokat (és kifizetni a tárhelyüket), a Media Services szolgáltatás elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ.</span><span class="sxs-lookup"><span data-stu-id="3b340-160">As a result, you only need to store and pay for the files in single storage format and Media Services service will build and serve the appropriate response based on requests from a client.</span></span> <span data-ttu-id="3b340-161">További információkért lásd a [Dynamic Packaging Overview](media-services-dynamic-packaging-overview.md) (A dinamikus becsomagolás áttekintése) című témakört.</span><span class="sxs-lookup"><span data-stu-id="3b340-161">For more information, see the [Dynamic Packaging Overview](media-services-dynamic-packaging-overview.md) topic.</span></span>

>[!NOTE]
><span data-ttu-id="3b340-162">Az AMS-fiók létrehozásakor a rendszer hozzáad egy **alapértelmezett** streamvégpontot a fiókhoz **Leállítva** állapotban.</span><span class="sxs-lookup"><span data-stu-id="3b340-162">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="3b340-163">A tartalom streamelésének megkezdéséhez, valamint a dinamikus csomagolás és a dinamikus titkosítás kihasználásához a tartalomstreameléshez használt streamvégpontnak **Fut** állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3b340-163">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 
>
><span data-ttu-id="3b340-164">Is hogy fogja tudni használni a dinamikus csomagolás és a dinamikus titkosítás az objektumot kell foglal magában adaptív sávszélességű MP4 vagy Smooth Streaming-fájlsorozattá.</span><span class="sxs-lookup"><span data-stu-id="3b340-164">Also, to be able to use dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>

<span data-ttu-id="3b340-165">A kódolással kapcsolatos utasításokért lásd: [How to encode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) (Objektum kódolása a Media Encoder Standard használatával).</span><span class="sxs-lookup"><span data-stu-id="3b340-165">For instructions on how to encode, see [How to encode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>

## <span data-ttu-id="3b340-166"><a id="create_contentkey"></a>Tartalomkulcs létrehozása és társítása a kódolt objektumhoz</span><span class="sxs-lookup"><span data-stu-id="3b340-166"><a id="create_contentkey"></a>Create a content key and associate it with the encoded asset</span></span>
<span data-ttu-id="3b340-167">A Media Services szolgáltatásban a tartalomkulcs tartalmazza az objektum titkosítására használható kulcsot.</span><span class="sxs-lookup"><span data-stu-id="3b340-167">In Media Services, the content key contains the key that you want to encrypt an asset with.</span></span>

<span data-ttu-id="3b340-168">További információk: [Create content key](media-services-dotnet-create-contentkey.md) (Tartalomkulcs létrehozása).</span><span class="sxs-lookup"><span data-stu-id="3b340-168">For detailed information, see [Create content key](media-services-dotnet-create-contentkey.md).</span></span>

## <span data-ttu-id="3b340-169"><a id="configure_key_auth_policy"></a>A tartalomkulcs engedélyezési házirendjének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3b340-169"><a id="configure_key_auth_policy"></a>Configure the content key’s authorization policy</span></span>
<span data-ttu-id="3b340-170">A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="3b340-170">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="3b340-171">Ahhoz, hogy az ügyfél (a lejátszó) megkaphassa a kulcsot, Önnek be kell állítania a tartalomkulcs-hitelesítési szabályzatot, amelynek az ügyfélnek meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="3b340-171">The content key authorization policy must be configured by you and met by the client (player) in order for the key to be delivered to the client.</span></span> <span data-ttu-id="3b340-172">A tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: Nyissa meg a, lexikális elem: korlátozás vagy IP-korlátozás.</span><span class="sxs-lookup"><span data-stu-id="3b340-172">The content key authorization policy could have one or more authorization restrictions: open, token restriction, or IP restriction.</span></span>

<span data-ttu-id="3b340-173">További információk: [A tartalomkulcs hitelesítési szabályzatának létrehozása](media-services-dotnet-configure-content-key-auth-policy.md).</span><span class="sxs-lookup"><span data-stu-id="3b340-173">For detailed information, see [Configure Content Key Authorization Policy](media-services-dotnet-configure-content-key-auth-policy.md).</span></span>

## <span data-ttu-id="3b340-174"><a id="configure_asset_delivery_policy"></a>Objektumtovábbítási szabályzat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3b340-174"><a id="configure_asset_delivery_policy"></a>Configure asset delivery policy</span></span>
<span data-ttu-id="3b340-175">Konfigurálja az objektum továbbítási szabályzatát.</span><span class="sxs-lookup"><span data-stu-id="3b340-175">Configure the delivery policy for your asset.</span></span> <span data-ttu-id="3b340-176">Az objektumtovábbítási szabályzat konfigurálásához többek között az alábbiak tartoznak:</span><span class="sxs-lookup"><span data-stu-id="3b340-176">Some things that the asset delivery policy configuration includes:</span></span>

* <span data-ttu-id="3b340-177">A kulcs licenckérési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="3b340-177">The Key acquisition URL.</span></span> 
* <span data-ttu-id="3b340-178">Az inicializálási vektor (IV) a boríték titkosítási használatára.</span><span class="sxs-lookup"><span data-stu-id="3b340-178">The Initialization Vector (IV) to use for the envelope encryption.</span></span> <span data-ttu-id="3b340-179">AES-128 adni, ha titkosítása és visszafejtése azonos IV igényel.</span><span class="sxs-lookup"><span data-stu-id="3b340-179">AES 128 requires the same IV to be supplied when encrypting and decrypting.</span></span> 
* <span data-ttu-id="3b340-180">Az adategység-továbbítási protokoll (pl. MPEG DASH, HLS, Smooth Streaming vagy ezek mindegyike).</span><span class="sxs-lookup"><span data-stu-id="3b340-180">The asset delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all).</span></span>
* <span data-ttu-id="3b340-181">A dinamikus titkosítás (például AES envelope) típusú vagy a dinamikus titkosítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="3b340-181">The type of dynamic encryption (for example, AES envelope) or no dynamic encryption.</span></span> 

<span data-ttu-id="3b340-182">További információk: [Objektumtovábbítási szabályzat konfigurálása ](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="3b340-182">For detailed information, see [Configure asset delivery policy ](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <span data-ttu-id="3b340-183"><a id="create_locator"></a>OnDemand-lokátor létrehozása a streamelési URL-cím lekérése érdekében</span><span class="sxs-lookup"><span data-stu-id="3b340-183"><a id="create_locator"></a>Create an OnDemand streaming locator in order to get a streaming URL</span></span>
<span data-ttu-id="3b340-184">A felhasználók rendelkezésére kell bocsátania a Smooth, DASH vagy HLS streamelési URL-címét.</span><span class="sxs-lookup"><span data-stu-id="3b340-184">You will need to provide your user with the streaming URL for Smooth, DASH or HLS.</span></span>

> [!NOTE]
> <span data-ttu-id="3b340-185">Objektumtovábbítási szabályzat hozzáadásakor vagy módosításakor törölnie kell az ahhoz tartozó meglévő lokátort (ha van), majd létre kell hoznia egy új lokátort.</span><span class="sxs-lookup"><span data-stu-id="3b340-185">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
> 
> 

<span data-ttu-id="3b340-186">További információk az objektumok közzétételéről és a streamelési URL-cím létrehozásáról: [Build a streaming URL](media-services-deliver-streaming-content.md) (Streamelési URL-cím létrehozása).</span><span class="sxs-lookup"><span data-stu-id="3b340-186">For instructions on how to publish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="get-a-test-token"></a><span data-ttu-id="3b340-187">Tesztjogkivonat lekérése</span><span class="sxs-lookup"><span data-stu-id="3b340-187">Get a test token</span></span>
<span data-ttu-id="3b340-188">Kérje le a kulcshitelesítési szabályzatban használt jogkivonat-korlátozásoknak megfelelő tesztjogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="3b340-188">Get a test token based on the token restriction that was used for the key authorization policy.</span></span>

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

<span data-ttu-id="3b340-189">A stream kipróbálásához használja az [AMS-lejátszót](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="3b340-189">You can use the [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) to test your stream.</span></span>

## <span data-ttu-id="3b340-190"><a id="client_request"></a>Hogyan is az ügyfél kérhet egy kulcsot a fő kézbesítési szolgáltatás?</span><span class="sxs-lookup"><span data-stu-id="3b340-190"><a id="client_request"></a>How can your client request a key from the key delivery service?</span></span>
<span data-ttu-id="3b340-191">Az előző lépésben összeállított jegyzékfájlt mutató URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="3b340-191">In the previous step, you constructed the URL that points to a manifest file.</span></span> <span data-ttu-id="3b340-192">Az ügyfél a szükséges információk kinyerése adatfolyam fájlok ahhoz, hogy a kulcs kézbesítési szolgáltatás indítson egy lekérdezést kell.</span><span class="sxs-lookup"><span data-stu-id="3b340-192">Your client needs to extract the necessary information from the streaming manifest files in order to make a request to the key delivery service.</span></span>

### <a name="manifest-files"></a><span data-ttu-id="3b340-193">Fájlok</span><span class="sxs-lookup"><span data-stu-id="3b340-193">Manifest files</span></span>
<span data-ttu-id="3b340-194">Az ügyfélnek kell az URL (tartalmazó is tartalomkulcsot azonosítója (kid)) bontsa ki a jegyzékfájl közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="3b340-194">The client needs to extract the URL (that also contains content key Id (kid)) value from the manifest file.</span></span> <span data-ttu-id="3b340-195">Az ügyfél a titkosítási kulcs beszerzése a kulcs kézbesítési szolgáltatás majd megpróbálja.</span><span class="sxs-lookup"><span data-stu-id="3b340-195">The client will then try to get the encryption key from the key delivery service.</span></span> <span data-ttu-id="3b340-196">Az ügyfél kell bontsa ki a IV érték és az visszafejteni az adatfolyam használata. Az alábbi kódrészletben látható a <Protection> a Smooth Streaming jegyzékfájl elemet.</span><span class="sxs-lookup"><span data-stu-id="3b340-196">The client also needs to extract the IV value and use it do decrypt the stream.The following snippet shows the <Protection> element of the Smooth Streaming manifest.</span></span>

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

<span data-ttu-id="3b340-197">HLS, ha a legfelső szintű jegyzékfájl lebontva szegmens fájlokat.</span><span class="sxs-lookup"><span data-stu-id="3b340-197">In the case of HLS, the root manifest is broken into segment files.</span></span> 

<span data-ttu-id="3b340-198">Például a legfelső szintű jegyzékfájl van: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl), és a szegmens fájlnevek listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3b340-198">For example, the root manifest is: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) and it contains a list of segment file names.</span></span>

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

<span data-ttu-id="3b340-199">Ha egy szegmens fájlt (például http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should contain szövegszerkesztőben megnyitása #EXT X-kulcs, amely azt jelzi, hogy a fájl titkosítva van.</span><span class="sxs-lookup"><span data-stu-id="3b340-199">If you open one of the segment files in text editor (for example, http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should contain #EXT-X-KEY which indicates that the file is encrypted.</span></span>

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
><span data-ttu-id="3b340-200">Ha azt tervezi, számára, hogy az AES titkosított HLS a Safari című [ebben a blogban](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span><span class="sxs-lookup"><span data-stu-id="3b340-200">If you are planning to play an AES encrypted HLS in Safari, see [this blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span></span>

### <a name="request-the-key-from-the-key-delivery-service"></a><span data-ttu-id="3b340-201">A kulcs kér a kulcs kézbesítési szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="3b340-201">Request the key from the key delivery service</span></span>

<span data-ttu-id="3b340-202">A következő kód bemutatja, hogyan kérelmet küld a Media Services kulcs kézbesítési szolgáltatás egy kulcs kézbesítés URI-azonosítóhoz (a jegyzékfájl kinyert) használata, valamint a jogkivonatot (a jelen témakör nem konzultáljon a Secure Token Service Simple Web Tokens tudhat).</span><span class="sxs-lookup"><span data-stu-id="3b340-202">The following code shows how to send a request to the Media Services key delivery service using a key delivery Uri (that was extracted from the manifest) and a token (this topic does not talk about how to get Simple Web Tokens from a Secure Token Service).</span></span>

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

## <a name="protect-your-content-with-aes-128-using-net"></a><span data-ttu-id="3b340-203">Az AES-128 tartalomvédelemre .NET használatával</span><span class="sxs-lookup"><span data-stu-id="3b340-203">Protect your content with AES-128 using .NET</span></span>

### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="3b340-204">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3b340-204">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="3b340-205">Állítsa be a fejlesztési környezetet, és töltse fel az app.config fájlt a kapcsolatadatokkal a [.NET-keretrendszerrel történő Media Services-fejlesztést](media-services-dotnet-how-to-use.md) ismertető dokumentumban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="3b340-205">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="3b340-206">Adja hozzá a következő elemeket az app.config fájlban megadott **appSettings** szakaszhoz:</span><span class="sxs-lookup"><span data-stu-id="3b340-206">Add the following elements to **appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <span data-ttu-id="3b340-207"><a id="example"></a>Példa</span><span class="sxs-lookup"><span data-stu-id="3b340-207"><a id="example"></a>Example</span></span>

<span data-ttu-id="3b340-208">Írja felül a Program.cs fájlban található kódot az itt látható kóddal.</span><span class="sxs-lookup"><span data-stu-id="3b340-208">Overwrite the code in your Program.cs file with the code shown in this section.</span></span>
 
>[!NOTE]
><span data-ttu-id="3b340-209">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="3b340-209">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="3b340-210">Ha mindig ugyanazokat a napokat/hozzáférési engedélyeket használja (például olyan keresők szabályzatait, amelyek hosszú ideig érvényben maradnak, vagyis nem feltöltött szabályzatokat), a szabályzatazonosítónak is ugyanannak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3b340-210">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="3b340-211">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="3b340-211">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="3b340-212">Módosítsa úgy a változókat, hogy a bemeneti fájlok tárolásához Ön által használt mappákra mutassanak.</span><span class="sxs-lookup"><span data-stu-id="3b340-212">Make sure to update variables to point to folders where your input files are located.</span></span>

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
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // A Uri describing the issuer of the token.  
        // Must match the value in the token for the token to be considered valid.
        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        // The Audience or Scope of the token.  
        // Must match the value in the token for the token to be considered valid.
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
            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
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

            // Generate a test token based on the data in the given TokenRestrictionTemplate.
            // Note, you need to pass the key id Guid because we specified 
            // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

            //The GenerateTestToken method returns the token without the word “Bearer” in front
            //so you have to add it in front of the token string. 
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
            Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use the bit.ly/aesplayer Flash player to test the URL 
            // (with open authorization policy). 
            // Paste the URL and click the Update button to play the video. 
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
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with the encoding details, using a string preset.
            // In this case "Adaptive Streaming" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
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

            // Associate the key with the asset.
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

            // Add ContentKeyAutorizationPolicy to ContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
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

            // Add ContentKeyAutorizationPolicy to ContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);

            return tokenTemplateString;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);

            string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));

            // When configuring delivery policy, you can choose to associate it
            // with a key acquisition URL that has a KID appended or
            // or a key acquisition URL that does not have a KID appended  
            // in which case a content key can be reused. 

            // EnvelopeKeyAcquisitionUrl:  contains a key ID in the key URL.
            // EnvelopeBaseKeyAcquisitionUrl:  the URL does not contains a key ID

            // The following policy configuration specifies: 
            // key url that will have KID=<Guid> appended to the envelope and
            // the Initialization Vector (IV) to use for the envelope encryption.

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

            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
        }

        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference to the streaming manifest file from the  
            // collection of files in the asset. 

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                EndsWith(".ism")).
                FirstOrDefault();

            // Create a 30-day readonly access policy. 
            // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
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


## <a name="media-services-learning-paths"></a><span data-ttu-id="3b340-213">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="3b340-213">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3b340-214">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="3b340-214">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

