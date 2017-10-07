---
title: "egy Media Services-fiókhoz használó többi aaaUpload fájlok |} Microsoft Docs"
description: "Ismerje meg, hogyan tooget multimédiás tartalom a Media Services létrehozásával és feltöltésével eszközök."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 41df7cbe-b8e2-48c1-a86c-361ec4e5251f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 2a92cecdc32d747d7a478946f069c15931eb32b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a><span data-ttu-id="d355b-103">Fájlok feltöltése a Media Services-fiók használatával REST</span><span class="sxs-lookup"><span data-stu-id="d355b-103">Upload files into a Media Services account using REST</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d355b-104">.NET</span><span class="sxs-lookup"><span data-stu-id="d355b-104">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="d355b-105">REST</span><span class="sxs-lookup"><span data-stu-id="d355b-105">REST</span></span>](media-services-rest-upload-files.md)
> * [<span data-ttu-id="d355b-106">Portál</span><span class="sxs-lookup"><span data-stu-id="d355b-106">Portal</span></span>](media-services-portal-upload-files.md)
> 
> 

<span data-ttu-id="d355b-107">A Media Services szolgáltatásban a digitális fájlok feltöltése egy adategységbe történik.</span><span class="sxs-lookup"><span data-stu-id="d355b-107">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="d355b-108">Hello [eszköz](https://docs.microsoft.com/rest/api/media/operations/asset) entitás tartalmazhat videót, hang, képek, miniatűröket, szöveg nyomon követi és feliratfájlokat fájlokat (és mindezen fájlok metaadatait hello.)  Miután hello objektumba hello fájlok feltöltése után a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő.</span><span class="sxs-lookup"><span data-stu-id="d355b-108">hello [Asset](https://docs.microsoft.com/rest/api/media/operations/asset) entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.)  Once hello files are uploaded into hello asset, your content is stored securely in hello cloud for further processing and streaming.</span></span> 

> [!NOTE]
> <span data-ttu-id="d355b-109">a következő szempontok hello vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="d355b-109">hello following considerations apply:</span></span>
> 
> * <span data-ttu-id="d355b-110">Media Services hello hello IAssetFile.Name tulajdonság értékét használja, amikor a hello adatfolyam-tartalmat (például http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) URL-címek kiépítéséhez Emiatt százalék-kódolás nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="d355b-110">Media Services uses hello value of hello IAssetFile.Name property when building URLs for hello streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="d355b-111">hello értékének hello **neve** tulajdonság nem lehet hello következő [százalék kódolás-fenntartott karakterek](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ".</span><span class="sxs-lookup"><span data-stu-id="d355b-111">hello value of hello **Name** property cannot have any of hello following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="d355b-112">Emellett csak lehet egy "." hello fájlnévkiterjesztés.</span><span class="sxs-lookup"><span data-stu-id="d355b-112">Also, there can only be one '.' for hello file name extension.</span></span>
> * <span data-ttu-id="d355b-113">hello hello nevének hossza nem lehet hosszabb 260 karakternél.</span><span class="sxs-lookup"><span data-stu-id="d355b-113">hello length of hello name should not be greater than 260 characters.</span></span>
> * <span data-ttu-id="d355b-114">Nincs a Media Services feldolgozás támogatott korlátot toohello fájl maximális méretét.</span><span class="sxs-lookup"><span data-stu-id="d355b-114">There is a limit toohello maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="d355b-115">Ellenőrizze a [ez](media-services-quotas-and-limitations.md) témakör hello méretű fájlt választhat vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="d355b-115">Please see [this](media-services-quotas-and-limitations.md) topic for details about hello file size limitation.</span></span>
> 

<span data-ttu-id="d355b-116">hello alapvető munkafolyamata eszközök feltöltése a következő szakaszok hello oszlik:</span><span class="sxs-lookup"><span data-stu-id="d355b-116">hello basic workflow for uploading Assets is divided into hello following sections:</span></span>

* <span data-ttu-id="d355b-117">Egy eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="d355b-117">Create an Asset</span></span>
* <span data-ttu-id="d355b-118">Az objektum titkosítására használható (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="d355b-118">Encrypt an Asset (Optional)</span></span>
* <span data-ttu-id="d355b-119">A fájltároló tooblob feltöltése</span><span class="sxs-lookup"><span data-stu-id="d355b-119">Upload a file tooblob storage</span></span>

<span data-ttu-id="d355b-120">AMS is lehetővé teszi tooupload eszközök tömeges.</span><span class="sxs-lookup"><span data-stu-id="d355b-120">AMS also enables you tooupload assets in bulk.</span></span> <span data-ttu-id="d355b-121">További információ [itt](media-services-rest-upload-files.md#upload_in_bulk) érhető el.</span><span class="sxs-lookup"><span data-stu-id="d355b-121">For more information, see [this](media-services-rest-upload-files.md#upload_in_bulk) section.</span></span>

> [!NOTE]
> <span data-ttu-id="d355b-122">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="d355b-122">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="d355b-123">További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="d355b-123">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>
> 

## <a name="connect-toomedia-services"></a><span data-ttu-id="d355b-124">Connect tooMedia szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d355b-124">Connect tooMedia Services</span></span>

<span data-ttu-id="d355b-125">Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="d355b-125">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="d355b-126">Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást.</span><span class="sxs-lookup"><span data-stu-id="d355b-126">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="d355b-127">Meg kell nyitnia a további hívások toohello új URI.</span><span class="sxs-lookup"><span data-stu-id="d355b-127">You must make subsequent calls toohello new URI.</span></span>

## <a name="upload-assets"></a><span data-ttu-id="d355b-128">Töltse fel az eszközök</span><span class="sxs-lookup"><span data-stu-id="d355b-128">Upload assets</span></span>

### <a name="create-an-asset"></a><span data-ttu-id="d355b-129">Egy eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="d355b-129">Create an asset</span></span>

<span data-ttu-id="d355b-130">Az eszköz egy olyan tároló, típusok vagy a Media Services, beleértve a videó, hang, képeket, miniatűröket, szöveges nyomon követi és feliratfájlokat fájlok objektumokat.</span><span class="sxs-lookup"><span data-stu-id="d355b-130">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="d355b-131">A REST API-t, az eszköz létrehozása a FELADÁS egy vagy több küldenie kell hello kérhetnek tooMedia szolgáltatásokat, és helyezi el az eszköz minden tulajdonságadatokat hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="d355b-131">In hello REST API, creating an Asset requires sending POST request tooMedia Services and placing any property information about your asset in hello request body.</span></span>

<span data-ttu-id="d355b-132">Megadható, amikor egy eszköz létrehozása hello tulajdonságok valamelyikét **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="d355b-132">One of hello properties that you can specify when creating an asset is **Options**.</span></span> <span data-ttu-id="d355b-133">**Beállítások** számbavételi érték, amely leírja, hogy egy eszköz hozhatók létre hello titkosítási lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d355b-133">**Options** is an enumeration value that describes hello encryption options that an Asset can be created with.</span></span> <span data-ttu-id="d355b-134">Érvényes értéket a hello listán, nem értékek kombinációja hello értékek egyike.</span><span class="sxs-lookup"><span data-stu-id="d355b-134">A valid value is one of hello values from hello list below, not a combination of values.</span></span> 

* <span data-ttu-id="d355b-135">**Nincs** = **0**: titkosítás nélkül fog történni.</span><span class="sxs-lookup"><span data-stu-id="d355b-135">**None** = **0**: No encryption will be used.</span></span> <span data-ttu-id="d355b-136">Ez az alapértelmezett érték hello.</span><span class="sxs-lookup"><span data-stu-id="d355b-136">This is hello default value.</span></span> <span data-ttu-id="d355b-137">Vegye figyelembe, hogy ez a beállítás használatakor a tartalom nem védett átvitel, sem tárolás közben.</span><span class="sxs-lookup"><span data-stu-id="d355b-137">Note that when using this option your content is not protected in transit or at rest in storage.</span></span>
    <span data-ttu-id="d355b-138">Ha egy MP4 toodeliver fájlt progresszív letöltés útján tervez, használja ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="d355b-138">If you plan toodeliver an MP4 using progressive download, use this option.</span></span> 
* <span data-ttu-id="d355b-139">**StorageEncrypted** = **1**: Adja meg, ha az AES-256 bites titkosítás feltöltés és tárolás titkosított fájlok toobe számára.</span><span class="sxs-lookup"><span data-stu-id="d355b-139">**StorageEncrypted** = **1**: Specify if you want for your files toobe encrypted with AES-256 bit encryption for upload and storage.</span></span>
  
    <span data-ttu-id="d355b-140">Ha az adategységen tárolótitkosítást alkalmaz, konfigurálnia kell az adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="d355b-140">If your asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="d355b-141">További információ: [objektumtovábbítási szabályzat konfigurálása](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="d355b-141">For more information see [Configuring asset delivery policy](media-services-rest-configure-asset-delivery-policy.md).</span></span>
* <span data-ttu-id="d355b-142">**CommonEncryptionProtected** = **2**: Adja meg, ha meg feltölteni egy közös titkosítási módszerrel (például PlayReady) védett fájlokkal.</span><span class="sxs-lookup"><span data-stu-id="d355b-142">**CommonEncryptionProtected** = **2**: Specify if you are uploading files protected with a common encryption method (such as PlayReady).</span></span> 
* <span data-ttu-id="d355b-143">**EnvelopeEncryptionProtected** = **4**: Adja meg, ha AES fájlok titkosított HLS meg feltölteni.</span><span class="sxs-lookup"><span data-stu-id="d355b-143">**EnvelopeEncryptionProtected** = **4**: Specify if you are uploading HLS encrypted with AES files.</span></span> <span data-ttu-id="d355b-144">Vegye figyelembe, hogy hello fájlokat kell kódolni és titkosítani Transform Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="d355b-144">Note that hello files must have been encoded and encrypted by Transform Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="d355b-145">Ha az eszköz titkosítást fog használni, létre kell hoznia egy **ContentKey** és hivatkozás létrehozása tooyour eszköz hello a következő témakörben leírtak szerint:[hogyan toocreate egy ContentKey](media-services-rest-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="d355b-145">If your asset will use encryption, you must create a **ContentKey** and link it tooyour asset as described in hello following topic:[How toocreate a ContentKey](media-services-rest-create-contentkey.md).</span></span> <span data-ttu-id="d355b-146">Megjegyzés: hello objektumba hello fájlok feltöltése, után újra kell tooupdate hello titkosítási tulajdonságainak hello **AssetFile** hello során kapott hello értékekkel entitás **eszköz** titkosítás.</span><span class="sxs-lookup"><span data-stu-id="d355b-146">Note that after you upload hello files into hello asset, you need tooupdate hello encryption properties on hello **AssetFile** entity with hello values you got during hello **Asset** encryption.</span></span> <span data-ttu-id="d355b-147">Úgy teheti meg, hogy hello **EGYESÍTÉSE** HTTP-kérelem.</span><span class="sxs-lookup"><span data-stu-id="d355b-147">Do it by using hello **MERGE** HTTP request.</span></span> 
> 
> 

<span data-ttu-id="d355b-148">a következő példa azt mutatja meg hogyan hello toocreate eszközként.</span><span class="sxs-lookup"><span data-stu-id="d355b-148">hello following example shows how toocreate an asset.</span></span>

<span data-ttu-id="d355b-149">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="d355b-149">**HTTP Request**</span></span>

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"Name":"BigBuckBunny.mp4"}

<span data-ttu-id="d355b-150">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="d355b-150">**HTTP Response**</span></span>

<span data-ttu-id="d355b-151">Ha sikeres, hello következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="d355b-151">If successful, hello following is returned:</span></span>

    HTP/1.1 201 Created
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
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

### <a name="create-an-assetfile"></a><span data-ttu-id="d355b-152">Hozzon létre egy AssetFile</span><span class="sxs-lookup"><span data-stu-id="d355b-152">Create an AssetFile</span></span>
<span data-ttu-id="d355b-153">Hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entitás egy blob-tárolóban tárolt video- vagy fájlt jelöli.</span><span class="sxs-lookup"><span data-stu-id="d355b-153">hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="d355b-154">Egy eszköz fájl mindig társítva van egy eszköz, és egy eszköz egy vagy több eszköz fájlt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d355b-154">An asset file is always associated with an asset, and an asset may contain one or many asset files.</span></span> <span data-ttu-id="d355b-155">hello Media Services kódoló feladat sikertelen lesz, ha egy eszköz fájl objektumhoz nincs társítva egy digitális fájlhoz egy blob-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="d355b-155">hello Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="d355b-156">Vegye figyelembe, hogy hello **AssetFile** példány és a hello tényleges media fájl is két különböző objektum.</span><span class="sxs-lookup"><span data-stu-id="d355b-156">Note that hello **AssetFile** instance and hello actual media file are two distinct objects.</span></span> <span data-ttu-id="d355b-157">hello AssetFile példány hello media fájlról metaadatot tartalmaz, amíg hello médiafájl tartalmaz hello tényleges médiatartalmakat.</span><span class="sxs-lookup"><span data-stu-id="d355b-157">hello AssetFile instance contains metadata about hello media file, while hello media file contains hello actual media content.</span></span>

<span data-ttu-id="d355b-158">A digitális adathordozójának fájl feltöltése a blob-tárolóba, után használandó hello **EGYESÍTÉSE** HTTP kérelem tooupdate hello AssetFile a adatainak media (hello témakör későbbi részében szerint).</span><span class="sxs-lookup"><span data-stu-id="d355b-158">After you upload your digital media file into a blob container, you will use hello **MERGE** HTTP request tooupdate hello AssetFile with information about your media file (as shown later in hello topic).</span></span> 

<span data-ttu-id="d355b-159">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="d355b-159">**HTTP Request**</span></span>

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    Content-Length: 164

    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }

<span data-ttu-id="d355b-160">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="d355b-160">**HTTP Response**</span></span>

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

### <a name="creating-hello-accesspolicy-with-write-permission"></a><span data-ttu-id="d355b-161">Hello AccessPolicy létrehozása írási engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="d355b-161">Creating hello AccessPolicy with write permission.</span></span>

>[!NOTE]
><span data-ttu-id="d355b-162">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="d355b-162">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="d355b-163">Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek.</span><span class="sxs-lookup"><span data-stu-id="d355b-163">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="d355b-164">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="d355b-164">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="d355b-165">Fájlok feltöltése a blob-tárolóba, mielőtt házirend jogosultságok tooan eszköz írásra hello hozzáférés beállítása.</span><span class="sxs-lookup"><span data-stu-id="d355b-165">Before uploading any files into blob storage, set hello access policy rights for writing tooan asset.</span></span> <span data-ttu-id="d355b-166">Ezt követően egy HTTP-kérelem toohello AccessPolicies entitás utáni toodo beállítása.</span><span class="sxs-lookup"><span data-stu-id="d355b-166">toodo that, POST an HTTP request toohello AccessPolicies entity set.</span></span> <span data-ttu-id="d355b-167">Adjon meg egy DurationInMinutes számot a létrehozása után, vagy egy belső kiszolgálót 500 hibaüzenetet kap vissza válaszként.</span><span class="sxs-lookup"><span data-stu-id="d355b-167">Define a DurationInMinutes value upon creation or you will receive a 500 Internal Server error message back in response.</span></span> <span data-ttu-id="d355b-168">A AccessPolicies további információkért lásd: [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span><span class="sxs-lookup"><span data-stu-id="d355b-168">For more information on AccessPolicies, see [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span></span>

<span data-ttu-id="d355b-169">a következő példa azt mutatja meg hogyan hello toocreate egy AccessPolicy:</span><span class="sxs-lookup"><span data-stu-id="d355b-169">hello following example shows how toocreate an AccessPolicy:</span></span>

<span data-ttu-id="d355b-170">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="d355b-170">**HTTP Request**</span></span>

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

<span data-ttu-id="d355b-171">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="d355b-171">**HTTP Request**</span></span>

    If successful, hello following response is returned:

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

### <a name="get-hello-upload-url"></a><span data-ttu-id="d355b-172">Hello feltöltése URL-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="d355b-172">Get hello Upload URL</span></span>
<span data-ttu-id="d355b-173">tooreceive hello tényleges feltöltési URL-cím, egy SAS-kereső létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d355b-173">tooreceive hello actual upload URL, create a SAS Locator.</span></span> <span data-ttu-id="d355b-174">Keresők ügyfelek számára, szeretné, hogy egy eszköz tooaccess fájlok hello kezdési ideje és a csatlakozási végpont típusa határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d355b-174">Locators define hello start time and type of connection endpoint for clients that want tooaccess Files in an Asset.</span></span> <span data-ttu-id="d355b-175">Létrehozhat több lokátor entitás egy adott AccessPolicy és eszköz pár toohandle különböző ügyfél, kérések és a vonatkozó igényeket.</span><span class="sxs-lookup"><span data-stu-id="d355b-175">You can create multiple Locator entities for a given AccessPolicy and Asset pair toohandle different client requests and needs.</span></span> <span data-ttu-id="d355b-176">A keresők mindegyikének használja hello StartTime érték plus hello DurationInMinutes értékének hello AccessPolicy toodetermine hello mennyi ideig egy URL-cím használható.</span><span class="sxs-lookup"><span data-stu-id="d355b-176">Each of these Locators use hello StartTime value plus hello DurationInMinutes value of hello AccessPolicy toodetermine hello length of time a URL can be used.</span></span> <span data-ttu-id="d355b-177">További információkért lásd: [lokátor](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="d355b-177">For more information, see [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>

<span data-ttu-id="d355b-178">Egy SAS URL-címnek a következő formátumban hello:</span><span class="sxs-lookup"><span data-stu-id="d355b-178">A SAS URL has hello following format:</span></span>

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

<span data-ttu-id="d355b-179">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="d355b-179">Some considerations apply:</span></span>

* <span data-ttu-id="d355b-180">Egy adott eszközhöz társított egyszerre legfeljebb öt egyedi keresők tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="d355b-180">You cannot have more than five unique Locators associated with a given Asset at one time.</span></span> <span data-ttu-id="d355b-181">További információkért tekintse meg a lokátor.</span><span class="sxs-lookup"><span data-stu-id="d355b-181">For more information, see Locator.</span></span>
* <span data-ttu-id="d355b-182">Ha tooupload a fájlok azonnal, a StartTime érték toofive perc hello aktuális időpont előtt kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="d355b-182">If you need tooupload your files immediately, you should set your StartTime value toofive minutes before hello current time.</span></span> <span data-ttu-id="d355b-183">Ennek az az oka lehet óra eltérésére az ügyfélszámítógép és a Media Services között.</span><span class="sxs-lookup"><span data-stu-id="d355b-183">This is because there may be clock skew between your client machine and Media Services.</span></span> <span data-ttu-id="d355b-184">A StartTime érték is, kell lennie a következő dátum és idő formátumban hello: éééé-hh-SSz (például "2014-05-23T17:53:50Z").</span><span class="sxs-lookup"><span data-stu-id="d355b-184">Also, your StartTime value must be in hello following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>    
* <span data-ttu-id="d355b-185">Előfordulhat, hogy a 30-40 második késleltetés akkor érhető el használatra toowhen egy kereső létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="d355b-185">There may be a 30-40 second delay after a Locator is created toowhen it is available for use.</span></span> <span data-ttu-id="d355b-186">A probléma tooboth SAS URL-cím és a forrás-keresők vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="d355b-186">This issue applies tooboth SAS URL and Origin Locators.</span></span>

<span data-ttu-id="d355b-187">hello a következő példa bemutatja, hogyan toocreate SAS URL-cím lokátor, által definiált hello hello kérés törzsében ("1" egy SAS-kereső) és egy az Igényalapú származási kereső "2" típusú tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="d355b-187">hello following example shows how toocreate a SAS URL Locator, as defined by hello Type property in hello request body ("1" for a SAS locator and "2" for an On-Demand origin locator).</span></span> <span data-ttu-id="d355b-188">Hello **elérési** tulajdonság visszaadott hello URL-címet, hogy fájlt kell használnia tooupload a tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d355b-188">hello **Path** property returned contains hello URL that you must use tooupload your file.</span></span>

<span data-ttu-id="d355b-189">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="d355b-189">**HTTP Request**</span></span>

    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }

<span data-ttu-id="d355b-190">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="d355b-190">**HTTP Response**</span></span>

<span data-ttu-id="d355b-191">Ha sikeres, a következő válasz hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="d355b-191">If successful, hello following response is returned:</span></span>

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

### <a name="upload-a-file-into-a-blob-storage-container"></a><span data-ttu-id="d355b-192">Egy blob storage tárolóba-fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="d355b-192">Upload a file into a blob storage container</span></span>
<span data-ttu-id="d355b-193">Miután hello AccessPolicy és a lokátor-készlet, a fájl tényleges hello feltöltött tooan Azure Blob Storage tárolóban hello Azure Storage REST API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="d355b-193">Once you have hello AccessPolicy and Locator set, hello actual file is uploaded tooan Azure Blob Storage container using hello Azure Storage REST APIs.</span></span> <span data-ttu-id="d355b-194">Hello fájlok blokkblobként kell feltöltenie.</span><span class="sxs-lookup"><span data-stu-id="d355b-194">You must upload hello files as block blobs.</span></span> <span data-ttu-id="d355b-195">Lapblobokat nem Azure Media Services által támogatott.</span><span class="sxs-lookup"><span data-stu-id="d355b-195">Page blobs are not supported by Azure Media Services.</span></span>  

> [!NOTE]
> <span data-ttu-id="d355b-196">Hozzá kell adnia a hello fájlnév hello fájlt keresi tooupload toohello lokátor **elérési** hello korábbi szakaszában kapott érték.</span><span class="sxs-lookup"><span data-stu-id="d355b-196">You must add hello file name for hello file you want tooupload toohello Locator **Path** value received in hello previous section.</span></span> <span data-ttu-id="d355b-197">Például https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="d355b-197">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="d355b-198">.</span><span class="sxs-lookup"><span data-stu-id="d355b-198">.</span></span> <span data-ttu-id="d355b-199">.</span><span class="sxs-lookup"><span data-stu-id="d355b-199">.</span></span> <span data-ttu-id="d355b-200">.</span><span class="sxs-lookup"><span data-stu-id="d355b-200">.</span></span> 
> 
> 

<span data-ttu-id="d355b-201">Az Azure storage blobs munkavégzés további információkért lásd: [Blob szolgáltatás REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="d355b-201">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

### <a name="update-hello-assetfile"></a><span data-ttu-id="d355b-202">Frissítés hello AssetFile</span><span class="sxs-lookup"><span data-stu-id="d355b-202">Update hello AssetFile</span></span>
<span data-ttu-id="d355b-203">Most, hogy a fájl feltöltése hello FileAsset méret (és más) adatainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="d355b-203">Now that you've uploaded your file, update hello FileAsset size (and other) information.</span></span> <span data-ttu-id="d355b-204">Példa:</span><span class="sxs-lookup"><span data-stu-id="d355b-204">For example:</span></span>

    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


<span data-ttu-id="d355b-205">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="d355b-205">**HTTP Response**</span></span>

<span data-ttu-id="d355b-206">Ha sikeres, hello következő adja vissza: HTTP/1.1 204 nem tartalom</span><span class="sxs-lookup"><span data-stu-id="d355b-206">If successful, hello following is returned: HTTP/1.1 204 No Content</span></span>

### <a name="delete-hello-locator-and-accesspolicy"></a><span data-ttu-id="d355b-207">Hello lokátor és AccessPolicy törlése</span><span class="sxs-lookup"><span data-stu-id="d355b-207">Delete hello Locator and AccessPolicy</span></span>
<span data-ttu-id="d355b-208">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="d355b-208">**HTTP Request**</span></span>

    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="d355b-209">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="d355b-209">**HTTP Response**</span></span>

<span data-ttu-id="d355b-210">Ha sikeres, hello következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="d355b-210">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content 
    ...

<span data-ttu-id="d355b-211">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="d355b-211">**HTTP Request**</span></span>

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="d355b-212">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="d355b-212">**HTTP Response**</span></span>

<span data-ttu-id="d355b-213">Ha sikeres, hello következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="d355b-213">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content 
    ...

## <span data-ttu-id="d355b-214"><a id="upload_in_bulk"></a>Eszközök tömeges feltöltése</span><span class="sxs-lookup"><span data-stu-id="d355b-214"><a id="upload_in_bulk"></a>Upload assets in bulk</span></span>
### <a name="create-hello-ingestmanifest"></a><span data-ttu-id="d355b-215">Hello IngestManifest létrehozása</span><span class="sxs-lookup"><span data-stu-id="d355b-215">Create hello IngestManifest</span></span>
<span data-ttu-id="d355b-216">hello IngestManifest egy olyan tároló, azon eszközök, eszköz-fájlok és statisztikai információkat használt toodetermine hello előrehaladását választásával a tömeges hello készlet dolgozhat fel.</span><span class="sxs-lookup"><span data-stu-id="d355b-216">hello IngestManifest is a container for a set of assets, asset files, and statistic information that can be used toodetermine hello progress of bulk ingesting for hello set.</span></span>

<span data-ttu-id="d355b-217">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="d355b-217">**HTTP Request**</span></span>

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue

    { "Name" : "ExampleManifestREST" }

### <a name="create-assets"></a><span data-ttu-id="d355b-218">Eszközök létrehozása</span><span class="sxs-lookup"><span data-stu-id="d355b-218">Create assets</span></span>
<span data-ttu-id="d355b-219">Mielőtt létrehozna hello IngestManifestAsset, kell toocreate hello eszköz, amely segítségével tömegesen választásával dolgozhat fel befejeződik.</span><span class="sxs-lookup"><span data-stu-id="d355b-219">Before creating hello IngestManifestAsset, you need toocreate hello Asset that will be completed using bulk ingesting.</span></span> <span data-ttu-id="d355b-220">Az eszköz egy olyan tároló, típusok vagy a Media Services, beleértve a videó, hang, képeket, miniatűröket, szöveges nyomon követi és feliratfájlokat fájlok objektumokat.</span><span class="sxs-lookup"><span data-stu-id="d355b-220">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="d355b-221">A hello REST API-t egy eszköz létrehozásához egy HTTP POST kérelem tooMicrosoft Azure Media Services küldésekor, és az eszköz minden tulajdonságadatokat elhelyezéséhez hello kérés törzsében. Ebben a példában hello eszköz jön létre, hello kérelemtörzset mellékelt hello StorageEncrption(1) beállítás használatával.</span><span class="sxs-lookup"><span data-stu-id="d355b-221">In hello REST API, creating an Asset requires sending a HTTP POST request tooMicrosoft Azure Media Services and placing any property information about your asset in hello request body.In this example, hello Asset is created using hello StorageEncrption(1) option included with hello request body.</span></span>

<span data-ttu-id="d355b-222">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="d355b-222">**HTTP Response**</span></span>

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue

    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

### <a name="create-hello-ingestmanifestassets"></a><span data-ttu-id="d355b-223">Hello IngestManifestAssets létrehozása</span><span class="sxs-lookup"><span data-stu-id="d355b-223">Create hello IngestManifestAssets</span></span>
<span data-ttu-id="d355b-224">IngestManifestAssets belül egy IngestManifest eszközök tömeges választásával dolgozhat fel használt jelölik.</span><span class="sxs-lookup"><span data-stu-id="d355b-224">IngestManifestAssets represent Assets within an IngestManifest that are used with bulk ingesting.</span></span> <span data-ttu-id="d355b-225">hello alapvetően hello eszköz toohello jegyzékfájl hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="d355b-225">hello basically link hello asset toohello manifest.</span></span> <span data-ttu-id="d355b-226">Az Azure Media Services belső hello fájlfeltöltés IngestManifestFiles hozzárendelt gyűjteményre toohello IngestManifestAsset alapján figyeli.</span><span class="sxs-lookup"><span data-stu-id="d355b-226">Azure Media Services watches internally for hello file upload based on IngestManifestFiles collection associated toohello IngestManifestAsset.</span></span> <span data-ttu-id="d355b-227">Ezeket a fájlokat a feltöltést követően hello eszköz befejeződött.</span><span class="sxs-lookup"><span data-stu-id="d355b-227">Once these files are uploaded, hello asset is completed.</span></span> <span data-ttu-id="d355b-228">Létrehozhat egy új IngestManifestAsset egy HTTP POST-kérelmet.</span><span class="sxs-lookup"><span data-stu-id="d355b-228">You can create a new IngestManifestAsset with a HTTP POST request.</span></span> <span data-ttu-id="d355b-229">A kérelem törzsében hello például a hello IngestManifest azonosítója és a hello eszköz azonosítója, hogy hello IngestManifestAsset kell összeköt tömeges választásával dolgozhat fel.</span><span class="sxs-lookup"><span data-stu-id="d355b-229">In hello request body, include hello IngestManifest Id and hello Asset Id that hello IngestManifestAsset should link together for bulk ingesting.</span></span>

<span data-ttu-id="d355b-230">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="d355b-230">**HTTP Response**</span></span>

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


### <a name="create-hello-ingestmanifestfiles-for-each-asset"></a><span data-ttu-id="d355b-231">Minden eszköz hello IngestManifestFiles létrehozása</span><span class="sxs-lookup"><span data-stu-id="d355b-231">Create hello IngestManifestFiles for each Asset</span></span>
<span data-ttu-id="d355b-232">Egy IngestManifestFile tömeges választásával dolgozhat fel, az eszközhöz tartozó részeként lesz feltöltve tényleges video- vagy blob objektumot jelöli.</span><span class="sxs-lookup"><span data-stu-id="d355b-232">An IngestManifestFile represents an actual video or audio blob object that will be uploaded as part of bulk ingesting for an asset.</span></span> <span data-ttu-id="d355b-233">Titkosítási kapcsolódó tulajdonságok esetén nincs szükség, kivéve, ha hello eszköz egy titkosítási beállítást használja.</span><span class="sxs-lookup"><span data-stu-id="d355b-233">Encryption related properties are not required unless hello asset is using an encryption option.</span></span> <span data-ttu-id="d355b-234">Ebben a szakaszban használt hello példa bemutatja, hogy az eszköz korábban létrehozott hello használó StorageEncryption egy IngestManifestFile létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d355b-234">hello example used in this section demonstrates creating an IngestManifestFile that uses StorageEncryption for hello Asset previously created.</span></span>

<span data-ttu-id="d355b-235">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="d355b-235">**HTTP Response**</span></span>

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue

    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }

### <a name="upload-hello-files-tooblob-storage"></a><span data-ttu-id="d355b-236">Hello fájlok tooBlob tárolási feltöltése</span><span class="sxs-lookup"><span data-stu-id="d355b-236">Upload hello Files tooBlob Storage</span></span>
<span data-ttu-id="d355b-237">A nagy sebességű ügyfélalkalmazás képes feltöltése hello eszköz fájlok toohello blob storage tárolót hello hello IngestManifest BlobStorageUriForUpload tulajdonsága által megadott URI-azonosítót is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d355b-237">You can use any high speed client application capable of uploading hello asset files toohello blob storage container Uri provided by hello BlobStorageUriForUpload property of hello IngestManifest.</span></span> <span data-ttu-id="d355b-238">Egy figyelmet a jelentősebb nagy sebességű feltöltési szolgáltatás [Aspera igény szerinti Azure alkalmazáshoz](http://go.microsoft.com/fwlink/?LinkId=272001).</span><span class="sxs-lookup"><span data-stu-id="d355b-238">One notable high speed upload service is [Aspera On Demand for Azure Application](http://go.microsoft.com/fwlink/?LinkId=272001).</span></span>

### <a name="monitor-bulk-ingest-progress"></a><span data-ttu-id="d355b-239">A figyelő tömeges betöltési folyamatban</span><span class="sxs-lookup"><span data-stu-id="d355b-239">Monitor Bulk Ingest Progress</span></span>
<span data-ttu-id="d355b-240">Kísérheti hello tömeges választásával dolgozhat fel egy IngestManifest műveletek hello IngestManifest hello statisztika tulajdonságának lekérdezésével.</span><span class="sxs-lookup"><span data-stu-id="d355b-240">You can monitor hello progress of bulk ingesting operations for an IngestManifest by polling hello Statistics property of hello IngestManifest.</span></span> <span data-ttu-id="d355b-241">Hogy a tulajdonság egy összetett típus [IngestManifestStatistics](https://docs.microsoft.com/rest/api/media/operations/ingestmanifeststatistics).</span><span class="sxs-lookup"><span data-stu-id="d355b-241">That property is a complex type, [IngestManifestStatistics](https://docs.microsoft.com/rest/api/media/operations/ingestmanifeststatistics).</span></span> <span data-ttu-id="d355b-242">toopoll hello statisztika tulajdonság hello IngestManifest azonosító továbbításához HTTP GET kérelmet küldeni.</span><span class="sxs-lookup"><span data-stu-id="d355b-242">toopoll hello Statistics property, submit a HTTP GET request passing hello IngestManifest Id.</span></span>

## <a name="create-contentkeys-used-for-encryption"></a><span data-ttu-id="d355b-243">A titkosításhoz használt ContentKeys létrehozása</span><span class="sxs-lookup"><span data-stu-id="d355b-243">Create ContentKeys used for encryption</span></span>
<span data-ttu-id="d355b-244">Ha az eszköz titkosítást fog használni, hello ContentKey toobe titkosítja az eszköz fájlok hello létrehozása előtt létre kell hoznia.</span><span class="sxs-lookup"><span data-stu-id="d355b-244">If your asset will use encryption, you must create hello ContentKey toobe used for encryption before creating hello asset files.</span></span> <span data-ttu-id="d355b-245">A tárolás titkosítása hello következő tulajdonságok tartozhatnak hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="d355b-245">For storage encryption, hello following properties should be included in hello request body.</span></span>

| <span data-ttu-id="d355b-246">Kérelem törzse tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d355b-246">Request body property</span></span> | <span data-ttu-id="d355b-247">Leírás</span><span class="sxs-lookup"><span data-stu-id="d355b-247">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d355b-248">Azonosító</span><span class="sxs-lookup"><span data-stu-id="d355b-248">Id</span></span> |<span data-ttu-id="d355b-249">hello ContentKey azonosítója, amely azt készítése ragozott formáival használatával a következő hello formázásához "nb:kid:UUID:<NEW GUID>".</span><span class="sxs-lookup"><span data-stu-id="d355b-249">hello ContentKey Id which we generate ourselves using hello following format, “nb:kid:UUID:<NEW GUID>”.</span></span> |
| <span data-ttu-id="d355b-250">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="d355b-250">ContentKeyType</span></span> |<span data-ttu-id="d355b-251">Ez az hello kulcs tartalomtípus a tartalom kulcs egész szám lehet.</span><span class="sxs-lookup"><span data-stu-id="d355b-251">This is hello content key type as an integer for this content key.</span></span> <span data-ttu-id="d355b-252">Azt adja át a tárolás titkosítása hello értéke 1.</span><span class="sxs-lookup"><span data-stu-id="d355b-252">We pass hello value 1 for storage encryption.</span></span> |
| <span data-ttu-id="d355b-253">EncryptedContentKey</span><span class="sxs-lookup"><span data-stu-id="d355b-253">EncryptedContentKey</span></span> |<span data-ttu-id="d355b-254">Létrehozhatunk egy új tartalom kulcs értéke pedig 256 bites (32 bájt) értéket.</span><span class="sxs-lookup"><span data-stu-id="d355b-254">We create a new content key value which is a 256-bit (32 byte) value.</span></span> <span data-ttu-id="d355b-255">hello kulcs Titkosított hello tárolási titkosítási X.509 tanúsítvány, amely által egy HTTP GET kérelem hello GetProtectionKeyId és GetProtectionKey metódusok végrehajtása nem beolvasni a Microsoft Azure Media Services használatával.</span><span class="sxs-lookup"><span data-stu-id="d355b-255">hello key is encrypted using hello storage encryption X.509 certificate which we retrieve from Microsoft Azure Media Services by executing a HTTP GET request for hello GetProtectionKeyId and GetProtectionKey Methods.</span></span> |
| <span data-ttu-id="d355b-256">ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="d355b-256">ProtectionKeyId</span></span> |<span data-ttu-id="d355b-257">Ez az hello védelmi hello storage encryption X.509 tanúsítvány, de a használt tooencrypt azonosítója a tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="d355b-257">This is hello protection key id for hello storage encryption X.509 certificate that was used tooencrypt our content key.</span></span> |
| <span data-ttu-id="d355b-258">ProtectionKeyType</span><span class="sxs-lookup"><span data-stu-id="d355b-258">ProtectionKeyType</span></span> |<span data-ttu-id="d355b-259">Ez az hello titkosítási típus hello védelmi kulcs, de a használt tooencrypt hello tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="d355b-259">This is hello encryption type for hello protection key that was used tooencrypt hello content key.</span></span> <span data-ttu-id="d355b-260">Ez az érték a fenti példában StorageEncryption(1).</span><span class="sxs-lookup"><span data-stu-id="d355b-260">This value is StorageEncryption(1) for our example.</span></span> |
| <span data-ttu-id="d355b-261">Ellenőrzőösszeg</span><span class="sxs-lookup"><span data-stu-id="d355b-261">Checksum</span></span> |<span data-ttu-id="d355b-262">hello MD5 számított ellenőrzőösszege hello tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="d355b-262">hello MD5 calculated checksum for hello content key.</span></span> <span data-ttu-id="d355b-263">Hello tartalmat azonosító hello tartalomkulcsot titkosításával számítja ki.</span><span class="sxs-lookup"><span data-stu-id="d355b-263">It is computed by encrypting hello content Id with hello content key.</span></span> <span data-ttu-id="d355b-264">hello példakód bemutatja, hogyan toocalculate hello ellenőrzőösszeg.</span><span class="sxs-lookup"><span data-stu-id="d355b-264">hello example code demonstrates how toocalculate hello checksum.</span></span> |

<span data-ttu-id="d355b-265">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="d355b-265">**HTTP Response**</span></span>

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue

    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-hello-contentkey-toohello-asset"></a><span data-ttu-id="d355b-266">Hivatkozás hello ContentKey toohello eszköz</span><span class="sxs-lookup"><span data-stu-id="d355b-266">Link hello ContentKey toohello Asset</span></span>
<span data-ttu-id="d355b-267">hello ContentKey társítva tooone vagy további eszközök úgy, hogy a HTTP POST-kérelmet küld.</span><span class="sxs-lookup"><span data-stu-id="d355b-267">hello ContentKey is associated tooone or more assets by sending a HTTP POST request.</span></span> <span data-ttu-id="d355b-268">hello következő kérelme, mert egy példa toolink hello példa ContentKey toohello példa az eszköznek azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="d355b-268">hello following request is an example toolink hello example ContentKey toohello example asset by Id.</span></span>

<span data-ttu-id="d355b-269">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="d355b-269">**HTTP Response**</span></span>

    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue

    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

<span data-ttu-id="d355b-270">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="d355b-270">**HTTP Response**</span></span>

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net

## <a name="next-steps"></a><span data-ttu-id="d355b-271">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d355b-271">Next steps</span></span>

<span data-ttu-id="d355b-272">Most már kódolhatja a feltöltött adategységeket.</span><span class="sxs-lookup"><span data-stu-id="d355b-272">You can now encode your uploaded assets.</span></span> <span data-ttu-id="d355b-273">További információ: [Encode Assets](media-services-portal-encode.md) (Adategységek kódolása).</span><span class="sxs-lookup"><span data-stu-id="d355b-273">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="d355b-274">Használhatja az Azure Functions tootrigger egy kódolási feladat, a konfigurált hello tároló érkező fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="d355b-274">You can also use Azure Functions tootrigger an encoding job based on a file arriving in hello configured container.</span></span> <span data-ttu-id="d355b-275">További információkért tekintse meg [ezt a mintát](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="d355b-275">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="d355b-276">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="d355b-276">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d355b-277">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="d355b-277">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[How tooGet a Media Processor]: media-services-get-media-processor.md

