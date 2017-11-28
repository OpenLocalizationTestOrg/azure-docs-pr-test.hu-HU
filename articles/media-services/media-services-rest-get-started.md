---
title: "aaaGet a többi segítségével igény szerinti tartalomtovábbítás használatába |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello végrehajtási egy on igény szerinti tartalomtovábbító alkalmazást az Azure Media Services REST API használatával."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 88194b59-e479-43ac-b179-af4f295e3780
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: f270ed59e9ae9745e8403ec6e19d5c3533fc82b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-rest"></a><span data-ttu-id="8ea2f-103">Ismerkedés a többi segítségével igény szerinti tartalomtovábbítás</span><span class="sxs-lookup"><span data-stu-id="8ea2f-103">Get started with delivering content on demand using REST</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="8ea2f-104">A gyors üzembe helyezés végigvezeti hello végrehajtási egy Video-on-Demand (VoD) tartalomtovábbító alkalmazást Azure Media Services (AMS) REST API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-104">This quickstart walks you through hello steps of implementing a Video-on-Demand (VoD) content delivery application using Azure Media Services (AMS) REST APIs.</span></span>

<span data-ttu-id="8ea2f-105">hello útmutató bemutatja a Media Services alapvető munkafolyamatait hello és hello leggyakoribb programozási objektumokat és a Media Services-fejlesztés szükséges feladatok.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-105">hello tutorial introduces hello basic Media Services workflow and hello most common programming objects and tasks required for Media Services development.</span></span> <span data-ttu-id="8ea2f-106">A hello hello az oktatóanyag befejezése után képes toostream kell vagy fokozatosan letölteni egy feltöltött, kódolt és letöltött példa médiafájlt lesz.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-106">At hello completion of hello tutorial, you will be able toostream or progressively download a sample media file that you uploaded, encoded, and downloaded.</span></span>

<span data-ttu-id="8ea2f-107">hello következő kép bemutatja a leggyakrabban használt hello objektumok hello Media Services OData modellre VoD-alkalmazások fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-107">hello following image shows some of hello most commonly used objects when developing VoD applications against hello Media Services OData model.</span></span>

<span data-ttu-id="8ea2f-108">Kattintson a hello kép tooview, teljes méret.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-108">Click hello image tooview it full size.</span></span>  

<a href="./media/media-services-rest-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-rest-get-started/media-services-overview-object-model-small.png"></a> 

## <a name="prerequisites"></a><span data-ttu-id="8ea2f-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8ea2f-109">Prerequisites</span></span>
<span data-ttu-id="8ea2f-110">hello következő előfeltételeket szükséges toostart fejlesztés a Media Services REST API-khoz.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-110">hello following prerequisites are required toostart developing with Media Services with REST APIs.</span></span>

* <span data-ttu-id="8ea2f-111">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-111">An Azure account.</span></span> <span data-ttu-id="8ea2f-112">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="8ea2f-113">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-113">A Media Services account.</span></span> <span data-ttu-id="8ea2f-114">egy Media Services-fiók toocreate lásd [hogyan tooCreate Media Services-fiók](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-114">toocreate a Media Services account, see [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="8ea2f-115">Megismernie az toodevelop Media Services REST API-t.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-115">Understanding of how toodevelop with Media Services REST API.</span></span> <span data-ttu-id="8ea2f-116">További információkért lásd: [Media Services REST API – áttekintés](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-116">For more information, see [Media Services REST API overview](media-services-rest-how-to-use.md).</span></span>
* <span data-ttu-id="8ea2f-117">Az Ön által választott küldő HTTP-kérések és válaszok alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-117">An application of your choice that can send HTTP requests and responses.</span></span> <span data-ttu-id="8ea2f-118">Ez az oktatóanyag használja [Fiddler](http://www.telerik.com/download/fiddler).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-118">This tutorial uses [Fiddler](http://www.telerik.com/download/fiddler).</span></span>

<span data-ttu-id="8ea2f-119">a következő feladatok hello láthatók a gyors üzembe helyezés.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-119">hello following tasks are shown in this quickstart.</span></span>

1. <span data-ttu-id="8ea2f-120">Indítsa el az adatfolyam-továbbítási végpontra (hello Azure-portál használatával).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-120">Start streaming endpoint (using hello Azure portal).</span></span>
2. <span data-ttu-id="8ea2f-121">Csatlakozás toohello Media Services-fiók REST API-t.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-121">Connect toohello Media Services account with REST API.</span></span>
3. <span data-ttu-id="8ea2f-122">Hozzon létre egy új eszközt, és a REST API-t egy videofájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-122">Create a new asset and upload a video file with REST API.</span></span>
4. <span data-ttu-id="8ea2f-123">Kódolja hello forrásfájl az adaptív sávszélességű MP4-fájlsorozattá REST API-t.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-123">Encode hello source file into a set of adaptive bitrate MP4 files with REST API.</span></span>
5. <span data-ttu-id="8ea2f-124">Hello objektum közzététele, és a streamelési és a progresszív letöltési URL-címet a REST API-t.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-124">Publish hello asset and get streaming and progressive download URLs with REST API.</span></span>
6. <span data-ttu-id="8ea2f-125">Tartalom lejátszása</span><span class="sxs-lookup"><span data-stu-id="8ea2f-125">Play your content.</span></span>

>[!NOTE]
><span data-ttu-id="8ea2f-126">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-126">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="8ea2f-127">Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-127">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="8ea2f-128">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-128">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="8ea2f-129">Ebben a témakörben használt AMS REST entitások kapcsolatos részletekért lásd: [Azure Media Services REST API-referencia](/rest/api/media/services/azure-media-services-rest-api-reference).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-129">For details about AMS REST entities used in this topic, see [Azure Media Services REST API Reference](/rest/api/media/services/azure-media-services-rest-api-reference).</span></span> <span data-ttu-id="8ea2f-130">Lásd még [Azure Media Services alapfogalmaiért](media-services-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-130">Also, see [Azure Media Services concepts](media-services-concepts.md).</span></span>

>[!NOTE]
><span data-ttu-id="8ea2f-131">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-131">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="8ea2f-132">További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-132">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="start-streaming-endpoints-using-hello-azure-portal"></a><span data-ttu-id="8ea2f-133">Indítsa el az adatfolyam-végpontok hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="8ea2f-133">Start streaming endpoints using hello Azure portal</span></span>

<span data-ttu-id="8ea2f-134">Ha az Azure Media Services egyik leggyakoribb forgatókönyve hello elkötelezett használatával adatfolyam adaptív sávszélességű streamelést működik.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-134">When working with Azure Media Services one of hello most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="8ea2f-135">A Media Services dinamikus becsomagolást biztosít, amely lehetővé teszi toodeliver az adaptív sávszélességű MP4-kódolású tartalmak anélkül, hogy előre csomagolt toostore (MPEG DASH, HLS, Smooth Streaming), a Media Services just-in-time, által támogatott streamformátumok Ezekbe a formátumokba egyes verzióit.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-135">Media Services provides dynamic packaging, which allows you toodeliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having toostore pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="8ea2f-136">Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-136">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="8ea2f-137">a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-137">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span>

<span data-ttu-id="8ea2f-138">toostart hello streamvégpontra, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-138">toostart hello streaming endpoint, do hello following:</span></span>

1. <span data-ttu-id="8ea2f-139">Jelentkezzen be hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-139">Log in at hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="8ea2f-140">Hello-beállítások ablakában kattintson a Streaming végpontok.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-140">In hello Settings window, click Streaming endpoints.</span></span>
3. <span data-ttu-id="8ea2f-141">Kattintson a hello alapértelmezett streamvégpontra.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-141">Click hello default streaming endpoint.</span></span>

    <span data-ttu-id="8ea2f-142">hello alapértelmezett STREAMING ENDPOINT részletek ablak.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-142">hello DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="8ea2f-143">Kattintson a hello Start ikonra.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-143">Click hello Start icon.</span></span>
5. <span data-ttu-id="8ea2f-144">Kattintson a hello Mentés gombra toosave a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-144">Click hello Save button toosave your changes.</span></span>

## <span data-ttu-id="8ea2f-145"><a id="connect"></a>Csatlakozás toohello Media Services-fiók REST API-hoz</span><span class="sxs-lookup"><span data-stu-id="8ea2f-145"><a id="connect"></a>Connect toohello Media Services account with REST API</span></span>

<span data-ttu-id="8ea2f-146">Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-146">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="8ea2f-147">Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-147">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="8ea2f-148">Meg kell nyitnia a további hívások toohello új URI.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-148">You must make subsequent calls toohello new URI.</span></span>

<span data-ttu-id="8ea2f-149">Például ha az tooconnect próbálja, után kapott hello következő:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-149">For example, if after trying tooconnect, you got hello following:</span></span>

    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

<span data-ttu-id="8ea2f-150">A következő API-hívások toohttps://wamsbayclus001rest-hs.cloudapp.net/api/ tegye meg.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-150">You should post your subsequent API calls toohttps://wamsbayclus001rest-hs.cloudapp.net/api/.</span></span>

## <span data-ttu-id="8ea2f-151"><a id="upload"></a>Hozzon létre egy új eszközt, és a REST API-t egy videofájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="8ea2f-151"><a id="upload"></a>Create a new asset and upload a video file with REST API</span></span>

<span data-ttu-id="8ea2f-152">A Media Services szolgáltatásban a digitális fájlok feltöltése egy adategységbe történik.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-152">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="8ea2f-153">Hello **eszköz** entitás tartalmazhat videót, hang, képek, miniatűröket, szöveg nyomon követi és feliratfájlokat fájlokat (és mindezen fájlok metaadatait hello.)  Miután hello objektumba hello fájlok feltöltése után a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-153">hello **Asset** entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.)  Once hello files are uploaded into hello asset, your content is stored securely in hello cloud for further processing and streaming.</span></span>

<span data-ttu-id="8ea2f-154">Hello értékek tooprovide lehet, ha egy eszköz létrehozása adategység-létrehozási lehetőségek egyikét.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-154">One of hello values that you have tooprovide when creating an asset is asset creation options.</span></span> <span data-ttu-id="8ea2f-155">Hello **beállítások** tulajdonsága egy számbavételi érték, amely leírja, hogy egy eszköz hozhatók létre hello titkosítási lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-155">hello **Options** property is an enumeration value that describes hello encryption options that an Asset can be created with.</span></span> <span data-ttu-id="8ea2f-156">Érvényes értéket kell az hello listán, nem a lista értékeinek kombinációja hello értékek egyikét:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-156">A valid value is one of hello values from hello list below, not a combination of values from this list:</span></span>

* <span data-ttu-id="8ea2f-157">**Nincs** = **0** – nincs titkosítás.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-157">**None** = **0** - No encryption is used.</span></span> <span data-ttu-id="8ea2f-158">Ez a beállítás használatakor a tartalom nem védett átvitel, sem tárolás közben.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-158">When using this option your content is not protected in transit or at rest in storage.</span></span>
    <span data-ttu-id="8ea2f-159">Ha egy MP4 toodeliver fájlt progresszív letöltés útján tervez, használja ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-159">If you plan toodeliver an MP4 using progressive download, use this option.</span></span>
* <span data-ttu-id="8ea2f-160">**StorageEncrypted** = **1** - titkosítja a tiszta tartalom helyileg az AES-256 bites titkosítás használata és tooAzure helyén tárolás titkosítása feltöltését.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-160">**StorageEncrypted** = **1** - Encrypts your clear content locally using AES-256 bit encryption and then uploads it tooAzure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="8ea2f-161">Storage-titkosítással védett adategységek automatikusan titkosítás és egy titkosított fájl rendszer előzetes tooencoding, és ha szükséges újra titkosítani előzetes toouploading egy új kimeneti eszközként helyezve.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-161">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior tooencoding, and optionally re-encrypted prior toouploading back as a new output asset.</span></span> <span data-ttu-id="8ea2f-162">a tárolás titkosítása hello elsődleges használati eset az, amikor toosecure a kiváló minőségű bemeneti médiafájljait erős titkosítással aktívan a lemezen.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-162">hello primary use case for Storage Encryption is when you want toosecure your high-quality input media files with strong encryption at rest on disk.</span></span>
* <span data-ttu-id="8ea2f-163">**CommonEncryptionProtected** = **2** -használja ezt a beállítást, ha már titkosítva és védve általános titkosítás vagy a PlayReady DRM által (például Smooth Streaming tartalom védett PlayReady DRM).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-163">**CommonEncryptionProtected** = **2** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="8ea2f-164">**EnvelopeEncryptionProtected** = **4** – használja ezt a beállítást, ha AES által titkosított HLS.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-164">**EnvelopeEncryptionProtected** = **4** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="8ea2f-165">hello fájlokat kell kódolni és Transform Manager használatával titkosított.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-165">hello files must have been encoded and encrypted by Transform Manager.</span></span>

### <a name="create-an-asset"></a><span data-ttu-id="8ea2f-166">Egy eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ea2f-166">Create an asset</span></span>
<span data-ttu-id="8ea2f-167">Az eszköz egy olyan tároló, típusok vagy a Media Services, beleértve a videó, hang, képeket, miniatűröket, szöveges nyomon követi és feliratfájlokat fájlok objektumokat.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-167">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="8ea2f-168">A REST API-t, az eszköz létrehozása a FELADÁS egy vagy több küldenie kell hello kérhetnek tooMedia szolgáltatásokat, és helyezi el az eszköz minden tulajdonságadatokat hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-168">In hello REST API, creating an Asset requires sending POST request tooMedia Services and placing any property information about your asset in hello request body.</span></span>

<span data-ttu-id="8ea2f-169">a következő példa azt mutatja meg hogyan hello toocreate eszközként.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-169">hello following example shows how toocreate an asset.</span></span>

<span data-ttu-id="8ea2f-170">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-170">**HTTP Request**</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45

    {"Name":"BigBuckBunny.mp4", "Options":"0"}


<span data-ttu-id="8ea2f-171">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-171">**HTTP Response**</span></span>

<span data-ttu-id="8ea2f-172">Ha sikeres, hello következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-172">If successful, hello following is returned:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

### <a name="create-an-assetfile"></a><span data-ttu-id="8ea2f-173">Hozzon létre egy AssetFile</span><span class="sxs-lookup"><span data-stu-id="8ea2f-173">Create an AssetFile</span></span>
<span data-ttu-id="8ea2f-174">Hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entitás egy blob-tárolóban tárolt video- vagy fájlt jelöli.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-174">hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="8ea2f-175">Egy eszköz fájl mindig társítva van egy eszköz, és egy eszköz tartalmazhat egy vagy több AssetFiles.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-175">An asset file is always associated with an asset, and an asset may contain one or many AssetFiles.</span></span> <span data-ttu-id="8ea2f-176">hello Media Services kódoló feladat sikertelen lesz, ha egy eszköz fájl objektumhoz nincs társítva egy digitális fájlhoz egy blob-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-176">hello Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="8ea2f-177">Miután a digitális adathordozójának fájl feltöltése a blob-tárolóba, hello az **EGYESÍTÉSE** HTTP kérelem tooupdate hello AssetFile a adatainak media (hello témakör későbbi részében szerint).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-177">After you upload your digital media file into a blob container, you use hello **MERGE** HTTP request tooupdate hello AssetFile with information about your media file (as shown later in hello topic).</span></span>

<span data-ttu-id="8ea2f-178">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-178">**HTTP Request**</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 164

    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


<span data-ttu-id="8ea2f-179">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-179">**HTTP Response**</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-hello-accesspolicy-with-write-permission"></a><span data-ttu-id="8ea2f-180">Írási engedéllyel rendelkező hello AccessPolicy létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ea2f-180">Creating hello AccessPolicy with write permission</span></span>
<span data-ttu-id="8ea2f-181">Fájlok feltöltése a blob-tárolóba, mielőtt házirend jogosultságok tooan eszköz írásra hello hozzáférés beállítása.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-181">Before uploading any files into blob storage, set hello access policy rights for writing tooan asset.</span></span> <span data-ttu-id="8ea2f-182">Ezt követően egy HTTP-kérelem toohello AccessPolicies entitás utáni toodo beállítása.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-182">toodo that, POST an HTTP request toohello AccessPolicies entity set.</span></span> <span data-ttu-id="8ea2f-183">Adjon meg egy DurationInMinutes számot a létrehozása után, vagy egy belső kiszolgálót 500 hibaüzenetet kapja vissza válaszként.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-183">Define a DurationInMinutes value upon creation or you receive a 500 Internal Server error message back in response.</span></span> <span data-ttu-id="8ea2f-184">A AccessPolicies további információkért lásd: [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-184">For more information on AccessPolicies, see [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span></span>

<span data-ttu-id="8ea2f-185">a következő példa azt mutatja meg hogyan hello toocreate egy AccessPolicy:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-185">hello following example shows how toocreate an AccessPolicy:</span></span>

<span data-ttu-id="8ea2f-186">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-186">**HTTP Request**</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74

    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"}

<span data-ttu-id="8ea2f-187">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-187">**HTTP Response**</span></span>

<span data-ttu-id="8ea2f-188">Ha sikeres, a következő válasz hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-188">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

### <a name="get-hello-upload-url"></a><span data-ttu-id="8ea2f-189">Hello feltöltése URL-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="8ea2f-189">Get hello Upload URL</span></span>

<span data-ttu-id="8ea2f-190">tooreceive hello tényleges feltöltési URL-cím, egy SAS-kereső létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-190">tooreceive hello actual upload URL, create a SAS Locator.</span></span> <span data-ttu-id="8ea2f-191">Keresők ügyfelek számára, szeretné, hogy egy eszköz tooaccess fájlok hello kezdési ideje és a csatlakozási végpont típusa határozza meg.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-191">Locators define hello start time and type of connection endpoint for clients that want tooaccess Files in an Asset.</span></span> <span data-ttu-id="8ea2f-192">Létrehozhat több lokátor entitás egy adott AccessPolicy és eszköz pár toohandle különböző ügyfél, kérések és a vonatkozó igényeket.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-192">You can create multiple Locator entities for a given AccessPolicy and Asset pair toohandle different client requests and needs.</span></span> <span data-ttu-id="8ea2f-193">A keresők mindegyike használ, hello StartTime érték plus hello DurationInMinutes értékének hello AccessPolicy toodetermine hello mennyi ideig egy URL-cím használható.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-193">Each of these Locators uses hello StartTime value plus hello DurationInMinutes value of hello AccessPolicy toodetermine hello length of time a URL can be used.</span></span> <span data-ttu-id="8ea2f-194">További információkért lásd: [lokátor](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-194">For more information, see [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>

<span data-ttu-id="8ea2f-195">Egy SAS URL-címnek a következő formátumban hello:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-195">A SAS URL has hello following format:</span></span>

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

<span data-ttu-id="8ea2f-196">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-196">Some considerations apply:</span></span>

* <span data-ttu-id="8ea2f-197">Egy adott eszközhöz társított egyszerre legfeljebb öt egyedi keresők tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-197">You cannot have more than five unique Locators associated with a given Asset at one time.</span></span> <span data-ttu-id="8ea2f-198">További információkért tekintse meg a lokátor.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-198">For more information, see Locator.</span></span>
* <span data-ttu-id="8ea2f-199">Ha tooupload a fájlok azonnal, a StartTime érték toofive perc hello aktuális időpont előtt kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-199">If you need tooupload your files immediately, you should set your StartTime value toofive minutes before hello current time.</span></span> <span data-ttu-id="8ea2f-200">Ennek az az oka lehet óra eltérésére az ügyfélszámítógép és a Media Services között.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-200">This is because there may be clock skew between your client machine and Media Services.</span></span> <span data-ttu-id="8ea2f-201">A StartTime érték is, kell lennie a következő dátum és idő formátumban hello: éééé-hh-SSz (például "2014-05-23T17:53:50Z").</span><span class="sxs-lookup"><span data-stu-id="8ea2f-201">Also, your StartTime value must be in hello following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>    
* <span data-ttu-id="8ea2f-202">Előfordulhat, hogy a 30-40 második késleltetés akkor érhető el használatra toowhen egy kereső létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-202">There may be a 30-40 second delay after a Locator is created toowhen it is available for use.</span></span> <span data-ttu-id="8ea2f-203">A probléma tooboth SAS URL-cím és a forrás-keresők vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-203">This issue applies tooboth SAS URL and Origin Locators.</span></span>

<span data-ttu-id="8ea2f-204">További információ a SAS keresők: [ez](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-204">For more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.</span></span>

<span data-ttu-id="8ea2f-205">hello a következő példa bemutatja, hogyan toocreate SAS URL-cím lokátor, által definiált hello hello kérés törzsében ("1" egy SAS-kereső) és egy az Igényalapú származási kereső "2" típusú tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-205">hello following example shows how toocreate a SAS URL Locator, as defined by hello Type property in hello request body ("1" for a SAS locator and "2" for an On-Demand origin locator).</span></span> <span data-ttu-id="8ea2f-206">Hello **elérési** tulajdonság visszaadott hello URL-címet, hogy fájlt kell használnia tooupload a tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-206">hello **Path** property returned contains hello URL that you must use tooupload your file.</span></span>

<span data-ttu-id="8ea2f-207">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-207">**HTTP Request**</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178

    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


<span data-ttu-id="8ea2f-208">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-208">**HTTP Response**</span></span>

<span data-ttu-id="8ea2f-209">Ha sikeres, a következő válasz hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-209">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a><span data-ttu-id="8ea2f-210">Egy blob storage tárolóba-fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="8ea2f-210">Upload a file into a blob storage container</span></span>
<span data-ttu-id="8ea2f-211">Miután hello AccessPolicy és a lokátor-készlet, a fájl tényleges hello feltöltött tooan Azure blob storage tárolót hello Azure Storage REST API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-211">Once you have hello AccessPolicy and Locator set, hello actual file is uploaded tooan Azure blob storage container using hello Azure Storage REST APIs.</span></span> <span data-ttu-id="8ea2f-212">Hello fájlok blokkblobként kell feltöltenie.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-212">You must upload hello files as block blobs.</span></span> <span data-ttu-id="8ea2f-213">Lapblobokat nem Azure Media Services által támogatott.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-213">Page blobs are not supported by Azure Media Services.</span></span>  

> [!NOTE]
> <span data-ttu-id="8ea2f-214">Hozzá kell adnia a hello fájlnév hello fájlt keresi tooupload toohello lokátor **elérési** hello korábbi szakaszában kapott érték.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-214">You must add hello file name for hello file you want tooupload toohello Locator **Path** value received in hello previous section.</span></span> <span data-ttu-id="8ea2f-215">Például https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="8ea2f-215">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="8ea2f-216">.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-216">.</span></span> <span data-ttu-id="8ea2f-217">.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-217">.</span></span> <span data-ttu-id="8ea2f-218">.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-218">.</span></span>
>
>

<span data-ttu-id="8ea2f-219">Az Azure storage blobs munkavégzés további információkért lásd: [Blob szolgáltatás REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-219">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

### <a name="update-hello-assetfile"></a><span data-ttu-id="8ea2f-220">Frissítés hello AssetFile</span><span class="sxs-lookup"><span data-stu-id="8ea2f-220">Update hello AssetFile</span></span>
<span data-ttu-id="8ea2f-221">Most, hogy a fájl feltöltése hello FileAsset méret (és más) adatainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-221">Now that you've uploaded your file, update hello FileAsset size (and other) information.</span></span> <span data-ttu-id="8ea2f-222">Példa:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-222">For example:</span></span>

    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


<span data-ttu-id="8ea2f-223">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-223">**HTTP Response**</span></span>

<span data-ttu-id="8ea2f-224">Ha sikeres, hello következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-224">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

## <a name="delete-hello-locator-and-accesspolicy"></a><span data-ttu-id="8ea2f-225">Hello lokátor és AccessPolicy törlése</span><span class="sxs-lookup"><span data-stu-id="8ea2f-225">Delete hello Locator and AccessPolicy</span></span>
<span data-ttu-id="8ea2f-226">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-226">**HTTP Request**</span></span>

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="8ea2f-227">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-227">**HTTP Response**</span></span>

<span data-ttu-id="8ea2f-228">Ha sikeres, hello következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-228">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

<span data-ttu-id="8ea2f-229">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-229">**HTTP Request**</span></span>

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

<span data-ttu-id="8ea2f-230">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-230">**HTTP Response**</span></span>

<span data-ttu-id="8ea2f-231">Ha sikeres, hello következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-231">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

## <span data-ttu-id="8ea2f-232"><a id="encode"></a>Hello forrásfájl kódolása adaptív sávszélességű MP4-fájlsorozattá készletére</span><span class="sxs-lookup"><span data-stu-id="8ea2f-232"><a id="encode"></a>Encode hello source file into a set of adaptive bitrate MP4 files</span></span>

<span data-ttu-id="8ea2f-233">Választásával dolgozhat fel, a Media Services szolgáltatásban adathordozó eszközök kódolható, transmuxed teljesítményjellemzőit, és így tovább, miután előtt biztosítását tooclients.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-233">After ingesting Assets into Media Services, media can be encoded, transmuxed, watermarked, and so on, before it is delivered tooclients.</span></span> <span data-ttu-id="8ea2f-234">Ezek a tevékenységek ütemezett, és több háttér szerepkör példányok tooensure nagy teljesítményt és rendelkezésre állás futtatni.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-234">These activities are scheduled and run against multiple background role instances tooensure high performance and availability.</span></span> <span data-ttu-id="8ea2f-235">Ezeket a tevékenységeket feladatoknak nevezzük, és minden feladat áll hello hello objektumfájlt valódi munkát atomi feladatokhoz (további információkért lásd: [feladat](/rest/api/media/services/job), [feladat](/rest/api/media/services/task) leírása).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-235">These activities are called Jobs and each Job is composed of atomic Tasks that do hello actual work on hello Asset file (for more information, see [Job](/rest/api/media/services/job), [Task](/rest/api/media/services/task) descriptions).</span></span>

<span data-ttu-id="8ea2f-236">Mivel korábban már említettük, ha működik az Azure Media Services egyik hello leggyakoribb forgatókönyve az adaptív sávszélességű streamelési tooyour ügyfelek elkötelezett.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-236">As was mentioned earlier, when working with Azure Media Services one of hello most common scenarios is delivering adaptive bitrate streaming tooyour clients.</span></span> <span data-ttu-id="8ea2f-237">A Media Services tudja dinamikusan csomagolni adaptív sávszélességű MP4-fájlok készlete hello a következő formátumok egyikét: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-237">Media Services can dynamically package a set of adaptive bitrate MP4 files into one of hello following formats: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span>

<span data-ttu-id="8ea2f-238">a következő szakasz hello bemutatja, hogyan toocreate egy feladatot, amely tartalmaz egy kódolási feladat.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-238">hello following section shows how toocreate a job that contains one encoding task.</span></span> <span data-ttu-id="8ea2f-239">hello feladat Megadja, hogy tootranscode hello mezzazine-fájlt használatával adaptív sávszélességű MP4 készletére **Media Encoder Standard**.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-239">hello task specifies tootranscode hello mezzanine file into a set of adaptive bitrate MP4s using **Media Encoder Standard**.</span></span> <span data-ttu-id="8ea2f-240">hello szakasz azt is bemutatja, hogyan toomonitor hello feladat feldolgozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-240">hello section also shows how toomonitor hello job processing progress.</span></span> <span data-ttu-id="8ea2f-241">Ha hello feladat befejeződött, lehetővé válik a szükséges tooget hozzáférés tooyour eszközöket képes toocreate lokátorokat.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-241">When hello job is complete, you would be able toocreate locators that are needed tooget access tooyour assets.</span></span>

### <a name="get-a-media-processor"></a><span data-ttu-id="8ea2f-242">Egy media processzor beolvasása</span><span class="sxs-lookup"><span data-stu-id="8ea2f-242">Get a media processor</span></span>
<span data-ttu-id="8ea2f-243">A Media Services szolgáltatásban a media processzor összetevő, amely egy meghatározott feldolgozási feladatot, például a kódolása, titkosítása vagy visszafejtése közben médiatartalmak konverzióval, kezeli.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-243">In Media Services, a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="8ea2f-244">A kódolási feladat ebben az oktatóanyagban szereplő hello fogjuk toouse hello Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-244">For hello encoding task shown in this tutorial, we are going toouse hello Media Encoder Standard.</span></span>

<span data-ttu-id="8ea2f-245">a következő kód kérelmek hello kódoló azonosító hello.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-245">hello following code requests hello encoder's id.</span></span>

<span data-ttu-id="8ea2f-246">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-246">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="8ea2f-247">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-247">**HTTP Response**</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a><span data-ttu-id="8ea2f-248">Feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ea2f-248">Create a job</span></span>
<span data-ttu-id="8ea2f-249">Minden egyes feladat egy vagy, hogy a feldolgozási hello típusától függően további feladatok tooaccomplish szeretné.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-249">Each Job can have one or more Tasks depending on hello type of processing that you want tooaccomplish.</span></span> <span data-ttu-id="8ea2f-250">Hello REST API-t, használatával hozhat létre feladatokat és azok kapcsolódó feladatok az alábbi két módszer egyikével: feladatok lehet meghatározott beágyazott hello feladatok navigációs tulajdonság a feladat entitásokat vagy OData kötegfeldolgozási révén.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-250">Through hello REST API, you can create Jobs and their related Tasks in one of two ways: Tasks can be defined inline through hello Tasks navigation property on Job entities, or through OData batch processing.</span></span> <span data-ttu-id="8ea2f-251">Media Services SDK hello kötegfeldolgozási használja.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-251">hello Media Services SDK uses batch processing.</span></span> <span data-ttu-id="8ea2f-252">Hello olvashatóság érdekében hello kódpéldák ebben a témakörben, azonban a feladatok nem definiált beágyazott.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-252">However, for hello readability of hello code examples in this topic, tasks are defined inline.</span></span> <span data-ttu-id="8ea2f-253">Kötegfeldolgozási információkért lásd: [nyitott Data (OData) protokollnak kötegelt feldolgozása](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-253">For information on batch processing, see [Open Data Protocol (OData) Batch Processing](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span></span>

<span data-ttu-id="8ea2f-254">hello a következő példa bemutatja, hogyan toocreate és a feladás egy vagy több feladat is egy feladatot állíthatja tooencode videó adott feloldási és minőségi.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-254">hello following example shows you how toocreate and post a Job with one Task set tooencode a video at a specific resolution and quality.</span></span> <span data-ttu-id="8ea2f-255">a következő dokumentáció hello megtalálható hello az összes hello [készletek feladat](http://msdn.microsoft.com/library/mt269960) hello Media Encoder Standard processzor által támogatott.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-255">hello following documentation section contains hello list of all hello [task presets](http://msdn.microsoft.com/library/mt269960) supported by hello Media Encoder Standard processor.</span></span>  

<span data-ttu-id="8ea2f-256">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-256">**HTTP Request**</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"Adaptive Streaming",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

<span data-ttu-id="8ea2f-257">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-257">**HTTP Response**</span></span>

<span data-ttu-id="8ea2f-258">Ha sikeres, a következő válasz hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-258">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  

             ]
          }
       }
    }


<span data-ttu-id="8ea2f-259">Van néhány fontos dolgot toonote feladat kérésének:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-259">There are a few important things toonote in any Job request:</span></span>

* <span data-ttu-id="8ea2f-260">TaskBody tulajdonságok literális XML toodefine hello számú bemeneti vagy kimeneti eszközök hello tevékenység által használt kell használnia.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-260">TaskBody properties MUST use literal XML toodefine hello number of input, or output assets that are used by hello Task.</span></span> <span data-ttu-id="8ea2f-261">hello feladat témakör hello XML Schema definíció hello XML számára.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-261">hello Task topic contains hello XML Schema Definition for hello XML.</span></span>
* <span data-ttu-id="8ea2f-262">Hello TaskBody definícióját, az egyes belső értékének <inputAsset> és <outputAsset> JobInputAsset(value) vagy JobOutputAsset(value) be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-262">In hello TaskBody definition, each inner value for <inputAsset> and <outputAsset> must be set as JobInputAsset(value) or JobOutputAsset(value).</span></span>
* <span data-ttu-id="8ea2f-263">Kimeneti adategységek feladatonként.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-263">A task can have multiple output assets.</span></span> <span data-ttu-id="8ea2f-264">Egy JobOutputAsset(x) csak egyszer használható egy feladatot a feladatok kimeneteként.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-264">One JobOutputAsset(x) can only be used once as an output of a task in a job.</span></span>
* <span data-ttu-id="8ea2f-265">A tevékenység bemeneti eszközként JobInputAsset vagy JobOutputAsset adhat meg.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-265">You can specify JobInputAsset or JobOutputAsset as an input asset of a task.</span></span>
* <span data-ttu-id="8ea2f-266">Feladatok kell nem alkotnak hurkot.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-266">Tasks must not form a cycle.</span></span>
* <span data-ttu-id="8ea2f-267">hello érték paraméter átadására tooJobInputAsset vagy JobOutputAsset hello index értékét jelöli az adott eszköz számára.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-267">hello value parameter that you pass tooJobInputAsset or JobOutputAsset represents hello index value for an Asset.</span></span> <span data-ttu-id="8ea2f-268">hello tényleges eszközök hello InputMediaAssets és hello entitás feladatdefiníció OutputMediaAssets navigációs tulajdonságainak vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-268">hello actual Assets are defined in hello InputMediaAssets and OutputMediaAssets navigation properties on hello Job entity definition.</span></span>

> [!NOTE]
> <span data-ttu-id="8ea2f-269">A Media Services OData v3 épül, mert egyes eszközöket a InputMediaAssets hello és OutputMediaAssets navigációs tulajdonság gyűjtemények hivatkozott keresztül a "__metadata: uri" név-érték pár.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-269">Because Media Services is built on OData v3, hello individual assets in InputMediaAssets and OutputMediaAssets navigation property collections are referenced through a "__metadata : uri" name-value pair.</span></span>
>
>

* <span data-ttu-id="8ea2f-270">InputMediaAssets tooone vagy a Media Services létrehozott további eszközök képezi le.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-270">InputMediaAssets maps tooone or more Assets you have created in Media Services.</span></span> <span data-ttu-id="8ea2f-271">OutputMediaAssets hello rendszer hozza létre.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-271">OutputMediaAssets are created by hello system.</span></span> <span data-ttu-id="8ea2f-272">Egy meglévő eszköz azok többé ne hivatkozzanak.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-272">They do not reference an existing asset.</span></span>
* <span data-ttu-id="8ea2f-273">OutputMediaAssets elnevezheti hello assetName attribútum használatával.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-273">OutputMediaAssets can be named using hello assetName attribute.</span></span> <span data-ttu-id="8ea2f-274">Ha ez az attribútum nincs jelen, akkor hello OutputMediaAsset hello nevét bármilyen hello belső szöveges értékét hello <outputAsset> elem utótaggal hello feladat nevét, és közül hello feladatazonosító érték (hello esetet, ahol hello Name tulajdonság nincs megadva).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-274">If this attribute is not present, then hello name of hello OutputMediaAsset is whatever hello inner text value of hello <outputAsset> element is with a suffix of either hello Job Name value, or hello Job Id value (in hello case where hello Name property isn't defined).</span></span> <span data-ttu-id="8ea2f-275">Például ha egy érték a assetName túl "Sample", majd OutputMediaAsset Name tulajdonság van beállítva hello túl "Sample".</span><span class="sxs-lookup"><span data-stu-id="8ea2f-275">For example, if you set a value for assetName too"Sample", then hello OutputMediaAsset Name property would be set too"Sample".</span></span> <span data-ttu-id="8ea2f-276">Azonban ha nem állított assetName értékét, de adta meg az hello feladat neve túl "NewJob", majd hello OutputMediaAsset neve lesz "JobOutputAsset (érték) _NewJob".</span><span class="sxs-lookup"><span data-stu-id="8ea2f-276">However, if you did not set a value for assetName, but did set hello job name too"NewJob", then hello OutputMediaAsset Name would be "JobOutputAsset(value)_NewJob".</span></span>

    <span data-ttu-id="8ea2f-277">hello a következő példa bemutatja, hogyan tooset hello assetName attribútum:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-277">hello following example shows how tooset hello assetName attribute:</span></span>

        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"
* <span data-ttu-id="8ea2f-278">tooenable feladat láncolás:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-278">tooenable task chaining:</span></span>

  * <span data-ttu-id="8ea2f-279">Egy feladat legalább két tevékenységet kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-279">A job must have at least two tasks</span></span>
  * <span data-ttu-id="8ea2f-280">Legalább egy tevékenységet, amelynek a bemeneti érték egy másik feladat hello feladat kimenetének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-280">There must be at least one task whose input is output of another task in hello job.</span></span>

<span data-ttu-id="8ea2f-281">További információ: [egy kódolási feladat létrehozása a Media Services REST API hello](media-services-rest-encode-asset.md).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-281">For more information see, [Creating an Encoding Job with hello Media Services REST API](media-services-rest-encode-asset.md).</span></span>

### <a name="monitor-processing-progress"></a><span data-ttu-id="8ea2f-282">A figyelő feldolgozása folyamatban van</span><span class="sxs-lookup"><span data-stu-id="8ea2f-282">Monitor Processing Progress</span></span>
<span data-ttu-id="8ea2f-283">Hello-State tulajdonsága, használatával lekérhető hello feladat állapotát, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-283">You can retrieve hello Job status by using hello State property, as shown in hello following example.</span></span>

<span data-ttu-id="8ea2f-284">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-284">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


<span data-ttu-id="8ea2f-285">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-285">**HTTP Response**</span></span>

<span data-ttu-id="8ea2f-286">Ha sikeres, a következő válasz hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-286">If successful, hello following response is returned:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT

    {"d":{"State":2}}


### <a name="cancel-a-job"></a><span data-ttu-id="8ea2f-287">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="8ea2f-287">Cancel a job</span></span>
<span data-ttu-id="8ea2f-288">A Media Services lehetővé teszi a futó feladatok keresztül hello CancelJob függvény toocancel.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-288">Media Services allows you toocancel running jobs through hello CancelJob function.</span></span> <span data-ttu-id="8ea2f-289">Ez a hívás 400 hibakódot ad vissza, ha toocancel egy feladat állapotában visszavonásakor, visszavonás, hiba, vagy befejeződött.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-289">This call returns a 400 error code if you try toocancel a Job when its state is canceled, canceling, error, or finished.</span></span>

<span data-ttu-id="8ea2f-290">a következő példa azt mutatja meg hogyan hello toocall CancelJob.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-290">hello following example shows how toocall CancelJob.</span></span>

<span data-ttu-id="8ea2f-291">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-291">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


<span data-ttu-id="8ea2f-292">Ha sikeres, nem az üzenet törzse 204 válaszkód is visszaad.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-292">If successful, a 204 response code is returned with no message body.</span></span>

> [!NOTE]
> <span data-ttu-id="8ea2f-293">Hello feladatazonosító URL-kódolni kell (általában nb:jid:UUID: somevalue) történő átadásakor legyen, mint egy paraméterrel tooCancelJob.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-293">You must URL-encode hello job id (normally nb:jid:UUID: somevalue) when passing it in as a parameter tooCancelJob.</span></span>
>
>

### <a name="get-hello-output-asset"></a><span data-ttu-id="8ea2f-294">Hello kimeneti eszköz beolvasása</span><span class="sxs-lookup"><span data-stu-id="8ea2f-294">Get hello output asset</span></span>
<span data-ttu-id="8ea2f-295">hello következő kód bemutatja, hogyan toorequest hello kimeneti eszköz azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-295">hello following code shows how toorequest hello output asset Id.</span></span>

<span data-ttu-id="8ea2f-296">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-296">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="8ea2f-297">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="8ea2f-297">**HTTP Response**</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <span data-ttu-id="8ea2f-298"><a id="publish_get_urls"></a>Hello objektum közzététele, és a streamelési és a progresszív letöltési URL-címet a REST API-n</span><span class="sxs-lookup"><span data-stu-id="8ea2f-298"><a id="publish_get_urls"></a>Publish hello asset and get streaming and progressive download URLs with REST API</span></span>

<span data-ttu-id="8ea2f-299">toostream vagy egy eszköz letöltése, akkor először kell túl "közzététele" egy kereső létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-299">toostream or download an asset, you first need too"publish" it by creating a locator.</span></span> <span data-ttu-id="8ea2f-300">Lokátorok biztosítanak hozzáférést toofiles hello eszköz szerepel.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-300">Locators provide access toofiles contained in hello asset.</span></span> <span data-ttu-id="8ea2f-301">Media Services két lokátortípust támogat: OnDemandOrigin keresők használt toostream media (például MPEG DASH, HLS vagy Smooth Streaming) és a hozzáférési aláírás (SAS) lokátortípus, toodownload médiafájlok használt.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-301">Media Services supports two types of locators: OnDemandOrigin locators, used toostream media (for example, MPEG DASH, HLS, or Smooth Streaming) and Access Signature (SAS) locators, used toodownload media files.</span></span> <span data-ttu-id="8ea2f-302">További információ a SAS keresők: [ez](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-302">For more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.</span></span>

<span data-ttu-id="8ea2f-303">Miután létrehozott egy hello lokátorokat, hozhat létre hello URL-címek, amelyek használt toostream, vagy töltse le a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-303">Once you create hello locators, you can build hello URLs that are used toostream or download your files.</span></span>

>[!NOTE]
><span data-ttu-id="8ea2f-304">Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-304">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="8ea2f-305">a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-305">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span>

<span data-ttu-id="8ea2f-306">Smooth Streaming egy adatfolyam-továbbítási URL-címnek a hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-306">A streaming URL for Smooth Streaming has hello following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="8ea2f-307">Egy HLS streamelési URL-címet a következő formátumban hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-307">A streaming URL for HLS has hello following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="8ea2f-308">MPEG DASH-továbbítási egy URL-formátum a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-308">A streaming URL for MPEG DASH has hello following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


<span data-ttu-id="8ea2f-309">A használt SAS URL-cím toodownload fájlok hello formátuma a következő esetében:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-309">A SAS URL used toodownload files has hello following format:</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

<span data-ttu-id="8ea2f-310">Ez a szakasz bemutatja, hogyan tooperform hello következő feladatok szükséges túl "közzététele" az eszközök.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-310">This section shows how tooperform hello following tasks necessary too"publish" your assets.</span></span>  

* <span data-ttu-id="8ea2f-311">Olvasási engedéllyel rendelkező hello AccessPolicy létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ea2f-311">Creating hello AccessPolicy with read permission</span></span>
* <span data-ttu-id="8ea2f-312">Letölti a tartalmat egy SAS URL-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ea2f-312">Creating a SAS URL for downloading content</span></span>
* <span data-ttu-id="8ea2f-313">Hozzon létre egy forrás URL-CÍMÉT egy adatfolyam-tartalom</span><span class="sxs-lookup"><span data-stu-id="8ea2f-313">Creating an origin URL for streaming content</span></span>

### <a name="creating-hello-accesspolicy-with-read-permission"></a><span data-ttu-id="8ea2f-314">Olvasási engedéllyel rendelkező hello AccessPolicy létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ea2f-314">Creating hello AccessPolicy with read permission</span></span>
<span data-ttu-id="8ea2f-315">Letöltése, illetve bármely médiatartalom streaming előtt először egy AccessPolicy olvasási engedély megadása, és hozzon létre hello megfelelő lokátor entitás hello típust megadó kézbesítési mechanizmus az ügyfelek számára a kívánt tooenable.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-315">Before downloading or streaming any media content, first define an AccessPolicy with read permissions and create hello appropriate Locator entity that specifies hello type of delivery mechanism you want tooenable for your clients.</span></span> <span data-ttu-id="8ea2f-316">Hello tulajdonságok érhetők el a további információkért lásd: [AccessPolicy Entitástulajdonságok](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-316">For more information on hello properties available, see [AccessPolicy Entity Properties](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).</span></span>

<span data-ttu-id="8ea2f-317">hello a következő példa bemutatja, hogyan toospecify hello AccessPolicy olvasási engedéllyel az adott eszköz.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-317">hello following example shows how toospecify hello AccessPolicy for read permissions for a given Asset.</span></span>

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue

    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

<span data-ttu-id="8ea2f-318">Ha sikeres, a 201 sikeres vissza hello AccessPolicy entitás létrehozott leíró.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-318">If successful, a 201 success code is returned describing hello AccessPolicy entity that you created.</span></span> <span data-ttu-id="8ea2f-319">Ezt követően az hello AccessPolicy azonosító együtt hello hello eszköz hello fájlt (például egy kimeneti eszköz) toodeliver toocreate hello lokátor entitást tartalmazó objektum azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-319">You then use hello AccessPolicy Id along with hello Asset Id of hello asset that contains hello file you want toodeliver(such as an output asset) toocreate hello Locator entity.</span></span>

> [!NOTE]
> <span data-ttu-id="8ea2f-320">Ez a munkafolyamat alapvető van hello ugyanaz, mint a fájlt feltölteni, amikor egy eszköz választásával dolgozhat fel, (a korábban a jelen témakörben bemutatott volt).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-320">This basic workflow is hello same as uploading a file when ingesting an Asset (as was discussed earlier in this topic).</span></span> <span data-ttu-id="8ea2f-321">Is például fájlok feltöltése, ha Ön (vagy az ügyfelek) tooaccess kell a fájlok azonnal, állítsa be a StartTime érték toofive perc hello aktuális ideje előtt.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-321">Also, like uploading files, if you (or your clients) need tooaccess your files immediately, set your StartTime value toofive minutes before hello current time.</span></span> <span data-ttu-id="8ea2f-322">Ez a művelet szükség, mert előfordulhatnak óra eltérésére hello ügyfél és a Media Services között.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-322">This action is necessary because there may be clock skew between hello client and Media Services.</span></span> <span data-ttu-id="8ea2f-323">hello StartTime érték hello a következő dátum és idő formátumban kell megadni: éééé-hh-SSz (például "2014-05-23T17:53:50Z").</span><span class="sxs-lookup"><span data-stu-id="8ea2f-323">hello StartTime value must be in hello following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>
>
>

### <a name="creating-a-sas-url-for-downloading-content"></a><span data-ttu-id="8ea2f-324">Letölti a tartalmat egy SAS URL-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ea2f-324">Creating a SAS URL for downloading content</span></span>
<span data-ttu-id="8ea2f-325">hello a következő kód bemutatja, hogyan tooget lehet használt toodownload egy médiafájlt URL-Címeket létrehozni és korábban feltöltött.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-325">hello following code shows how tooget a URL that can be used toodownload a media file created and uploaded previously.</span></span> <span data-ttu-id="8ea2f-326">hello AccessPolicy engedélycsoport rendelkezik-e olvasási és hello lokátor elérési út tooa SAS letöltési URL-CÍMRE hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-326">hello AccessPolicy has read permissions set and hello Locator path refers tooa SAS download URL.</span></span>

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

<span data-ttu-id="8ea2f-327">Ha sikeres, a következő válasz hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-327">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


<span data-ttu-id="8ea2f-328">visszaadott hello **elérési** tulajdonság hello SAS URL-címet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-328">hello returned **Path** property contains hello SAS URL.</span></span>

> [!NOTE]
> <span data-ttu-id="8ea2f-329">Tárolási titkosított tartalom letöltéséhez esetén kell manuálisan előtt azt visszafejteni, vagy a hello tárolási visszafejtési MediaProcessor egy feldolgozási feladat toooutput a feldolgozott hello egyértelmű tooan OutputAsset fájlokat, és töltse le az adott eszközre.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-329">If you download storage encrypted content, you must manually decrypt it before rendering it, or use hello Storage Decryption MediaProcessor in a processing task toooutput processed files in hello clear tooan OutputAsset and then download from that Asset.</span></span> <span data-ttu-id="8ea2f-330">A feldolgozási további információkért lásd: hello Media Services REST API-t egy kódolási feladat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-330">For more information on processing, see Creating an Encoding Job with hello Media Services REST API.</span></span> <span data-ttu-id="8ea2f-331">SAS URL-cím keresők nem lehet frissíteni, azok létrehozását követően.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-331">Also, SAS URL Locators cannot be updated after they have been created.</span></span> <span data-ttu-id="8ea2f-332">Például nem használhat újra hello frissített StartTime értékkel azonos Lokátort.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-332">For example, you cannot reuse hello same Locator with an updated StartTime value.</span></span> <span data-ttu-id="8ea2f-333">Ez az SAS URL-címek jönnek létre hello módon miatt.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-333">This is because of hello way SAS URLs are created.</span></span> <span data-ttu-id="8ea2f-334">Ha egy eszköz tooaccess letölthető egy kereső lejárta után, akkor egy új StartTime egy újat kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-334">If you want tooaccess an asset for download after a Locator has expired, then you must create a new one with a new StartTime.</span></span>
>
>

### <a name="download-files"></a><span data-ttu-id="8ea2f-335">Fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="8ea2f-335">Download files</span></span>
<span data-ttu-id="8ea2f-336">Miután hello AccessPolicy és a lokátor-készlet, letöltheti a fájlokat a hello Azure Storage REST API-k.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-336">Once you have hello AccessPolicy and Locator set, you can download files using hello Azure Storage REST APIs.</span></span>  

> [!NOTE]
> <span data-ttu-id="8ea2f-337">Hozzá kell adnia a hello fájlnév hello fájlt keresi toodownload toohello lokátor **elérési** hello korábbi szakaszában kapott érték.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-337">You must add hello file name for hello file you want toodownload toohello Locator **Path** value received in hello previous section.</span></span> <span data-ttu-id="8ea2f-338">Például https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="8ea2f-338">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="8ea2f-339">.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-339">.</span></span> <span data-ttu-id="8ea2f-340">.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-340">.</span></span> <span data-ttu-id="8ea2f-341">.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-341">.</span></span>
>
>

<span data-ttu-id="8ea2f-342">Az Azure storage blobs munkavégzés további információkért lásd: [Blob szolgáltatás REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-342">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

<span data-ttu-id="8ea2f-343">Miatt hello korábban végrehajtott feladat kódolás (kódolása adaptív MP4 állítsa be) hogy több MP4-fájlokat fokozatosan lehet letölteni.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-343">As a result of hello encoding job that you performed earlier (encoding into Adaptive MP4 set), you have multiple MP4 files that you can progressively download.</span></span> <span data-ttu-id="8ea2f-344">Példa:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-344">For example:</span></span>    

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a><span data-ttu-id="8ea2f-345">Hozzon létre egy adatfolyam-továbbítási URL-címet egy adatfolyam-tartalom</span><span class="sxs-lookup"><span data-stu-id="8ea2f-345">Creating a streaming URL for streaming content</span></span>
<span data-ttu-id="8ea2f-346">a következő kód bemutatja hogyan hello toocreate URL-címet a streamelési Lokátorok létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-346">hello following code shows how toocreate a streaming URL Locator:</span></span>

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

<span data-ttu-id="8ea2f-347">Ha sikeres, a következő válasz hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="8ea2f-347">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

<span data-ttu-id="8ea2f-348">egy Smooth Streaming forrás URL-címet egy adatfolyam-továbbítási media Player toostream, hozzá kell fűzni a hello Smooth Streaming manifest fájl "/ manifest" nevű hello hello Path tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="8ea2f-348">toostream a Smooth Streaming origin URL in a streaming media player, you must append hello Path property with hello name of hello Smooth Streaming manifest file, followed by "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

<span data-ttu-id="8ea2f-349">HLS, toostream hozzáfűzése (formátum = m3u8-aapl) után hello "/ manifest".</span><span class="sxs-lookup"><span data-stu-id="8ea2f-349">toostream HLS, append (format=m3u8-aapl) after hello "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

<span data-ttu-id="8ea2f-350">toostream MPEG DASH, hozzáfűző (formátum mpd-idő-csf =) után hello "/ manifest".</span><span class="sxs-lookup"><span data-stu-id="8ea2f-350">toostream MPEG DASH, append (format=mpd-time-csf) after hello "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <span data-ttu-id="8ea2f-351"><a id="play"></a>Tartalom lejátszása</span><span class="sxs-lookup"><span data-stu-id="8ea2f-351"><a id="play"></a>Play your content</span></span>
<span data-ttu-id="8ea2f-352">toostream videó, használhat [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-352">toostream you video, use [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

<span data-ttu-id="8ea2f-353">tootest progresszív letöltés, illessze be egy URL-címet a böngészőjébe (például az Internet Explorer, az Chrome, a Safari).</span><span class="sxs-lookup"><span data-stu-id="8ea2f-353">tootest progressive download, paste a URL into a browser (for example, IE, Chrome, Safari).</span></span>

## <a name="next-steps-media-services-learning-paths"></a><span data-ttu-id="8ea2f-354">Következő lépések: Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="8ea2f-354">Next Steps: Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="8ea2f-355">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="8ea2f-355">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
