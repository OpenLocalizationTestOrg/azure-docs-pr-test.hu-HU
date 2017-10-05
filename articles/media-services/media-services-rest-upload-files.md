---
title: "Fájlok feltöltése a Media Services-fiók használatával REST |} Microsoft Docs"
description: "Útmutató a médiatartalom feltölti a Media Services létrehozásával és feltöltésével eszközök."
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
ms.openlocfilehash: 955356ffe6fc524c1528364add7e2c2a336137b7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a><span data-ttu-id="7e80c-103">Fájlok feltöltése a Media Services-fiók használatával REST</span><span class="sxs-lookup"><span data-stu-id="7e80c-103">Upload files into a Media Services account using REST</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7e80c-104">.NET</span><span class="sxs-lookup"><span data-stu-id="7e80c-104">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="7e80c-105">REST</span><span class="sxs-lookup"><span data-stu-id="7e80c-105">REST</span></span>](media-services-rest-upload-files.md)
> * [<span data-ttu-id="7e80c-106">Portal</span><span class="sxs-lookup"><span data-stu-id="7e80c-106">Portal</span></span>](media-services-portal-upload-files.md)
> 
> 

<span data-ttu-id="7e80c-107">A Media Services szolgáltatásban a digitális fájlok feltöltése egy adategységbe történik.</span><span class="sxs-lookup"><span data-stu-id="7e80c-107">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="7e80c-108">A [eszköz](https://docs.microsoft.com/rest/api/media/operations/asset) entitás tartalmazhat videót, hang, képek, miniatűröket, szöveg nyomon követi és feliratfájlokat fájlokat (és a mindezen fájlok metaadatait.)  Ha a fájlok feltöltése az objektumba, a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-felhő.</span><span class="sxs-lookup"><span data-stu-id="7e80c-108">The [Asset](https://docs.microsoft.com/rest/api/media/operations/asset) entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and the metadata about these files.)  Once the files are uploaded into the asset, your content is stored securely in the cloud for further processing and streaming.</span></span> 

> [!NOTE]
> <span data-ttu-id="7e80c-109">A következők érvényesek:</span><span class="sxs-lookup"><span data-stu-id="7e80c-109">The following considerations apply:</span></span>
> 
> * <span data-ttu-id="7e80c-110">A Media Services a IAssetFile.Name tulajdonság értékét használja, amikor az adatfolyam-tartalmak (például http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) a URL-címek kiépítéséhez Emiatt százalék-kódolás nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="7e80c-110">Media Services uses the value of the IAssetFile.Name property when building URLs for the streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="7e80c-111">Értékét a **neve** tulajdonság nem lehet a következő [százalék kódolás-fenntartott karakterek](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ".</span><span class="sxs-lookup"><span data-stu-id="7e80c-111">The value of the **Name** property cannot have any of the following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="7e80c-112">Emellett csak lehet egy "." a fájlnévkiterjesztés.</span><span class="sxs-lookup"><span data-stu-id="7e80c-112">Also, there can only be one '.' for the file name extension.</span></span>
> * <span data-ttu-id="7e80c-113">A név hossza nem lehet hosszabb 260 karakternél.</span><span class="sxs-lookup"><span data-stu-id="7e80c-113">The length of the name should not be greater than 260 characters.</span></span>
> * <span data-ttu-id="7e80c-114">A Media Services által feldolgozható maximális támogatott fájlméret korlátozott.</span><span class="sxs-lookup"><span data-stu-id="7e80c-114">There is a limit to the maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="7e80c-115">A fájlméretre vonatkozó korlátozással kapcsolatban további információt [ebben](media-services-quotas-and-limitations.md) a témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="7e80c-115">Please see [this](media-services-quotas-and-limitations.md) topic for details about the file size limitation.</span></span>
> 

<span data-ttu-id="7e80c-116">A következő alapvető munkafolyamattal eszközök feltöltése a következő szakaszokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="7e80c-116">The basic workflow for uploading Assets is divided into the following sections:</span></span>

* <span data-ttu-id="7e80c-117">Egy eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e80c-117">Create an Asset</span></span>
* <span data-ttu-id="7e80c-118">Az objektum titkosítására használható (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="7e80c-118">Encrypt an Asset (Optional)</span></span>
* <span data-ttu-id="7e80c-119">A blob storage-fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="7e80c-119">Upload a file to blob storage</span></span>

<span data-ttu-id="7e80c-120">AMS is lehetővé teszi az eszközök tömeges feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="7e80c-120">AMS also enables you to upload assets in bulk.</span></span> <span data-ttu-id="7e80c-121">További információ [itt](media-services-rest-upload-files.md#upload_in_bulk) érhető el.</span><span class="sxs-lookup"><span data-stu-id="7e80c-121">For more information, see [this](media-services-rest-upload-files.md#upload_in_bulk) section.</span></span>

> [!NOTE]
> <span data-ttu-id="7e80c-122">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="7e80c-122">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="7e80c-123">További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="7e80c-123">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>
> 

## <a name="connect-to-media-services"></a><span data-ttu-id="7e80c-124">Kapcsolódás a Media Services szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="7e80c-124">Connect to Media Services</span></span>

<span data-ttu-id="7e80c-125">Az AMS API-hoz kapcsolódáshoz információkért lásd: [elérni az Azure Media Services API-t az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="7e80c-125">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="7e80c-126">Sikeresen csatlakoztassa a https://media.windows.net, adja meg egy másik Media Services URI 301 átirányítást fog kapni.</span><span class="sxs-lookup"><span data-stu-id="7e80c-126">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="7e80c-127">Meg kell nyitnia az új URI későbbi hívásokat.</span><span class="sxs-lookup"><span data-stu-id="7e80c-127">You must make subsequent calls to the new URI.</span></span>

## <a name="upload-assets"></a><span data-ttu-id="7e80c-128">Töltse fel az eszközök</span><span class="sxs-lookup"><span data-stu-id="7e80c-128">Upload assets</span></span>

### <a name="create-an-asset"></a><span data-ttu-id="7e80c-129">Egy eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e80c-129">Create an asset</span></span>

<span data-ttu-id="7e80c-130">Az eszköz egy olyan tároló, típusok vagy a Media Services, beleértve a videó, hang, képeket, miniatűröket, szöveges nyomon követi és feliratfájlokat fájlok objektumokat.</span><span class="sxs-lookup"><span data-stu-id="7e80c-130">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="7e80c-131">A REST API-ban az eszköz létrehozásához POST kérést küld a Media Services és az eszköz minden tulajdonságadatokat helyezi el a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="7e80c-131">In the REST API, creating an Asset requires sending POST request to Media Services and placing any property information about your asset in the request body.</span></span>

<span data-ttu-id="7e80c-132">A tulajdonságokat, amelyeket megadhat egy eszköz létrehozása esetén egyik **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="7e80c-132">One of the properties that you can specify when creating an asset is **Options**.</span></span> <span data-ttu-id="7e80c-133">**Beállítások** számbavételi érték, amely leírja a titkosítási beállításokat, hogy egy eszköz hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="7e80c-133">**Options** is an enumeration value that describes the encryption options that an Asset can be created with.</span></span> <span data-ttu-id="7e80c-134">Érvényes értéket a értékeket az alábbi listán, nem az értékek egy kombinációját kell.</span><span class="sxs-lookup"><span data-stu-id="7e80c-134">A valid value is one of the values from the list below, not a combination of values.</span></span> 

* <span data-ttu-id="7e80c-135">**Nincs** = **0**: titkosítás nélkül fog történni.</span><span class="sxs-lookup"><span data-stu-id="7e80c-135">**None** = **0**: No encryption will be used.</span></span> <span data-ttu-id="7e80c-136">Ez az alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="7e80c-136">This is the default value.</span></span> <span data-ttu-id="7e80c-137">Vegye figyelembe, hogy ez a beállítás használatakor a tartalom nem védett átvitel, sem tárolás közben.</span><span class="sxs-lookup"><span data-stu-id="7e80c-137">Note that when using this option your content is not protected in transit or at rest in storage.</span></span>
    <span data-ttu-id="7e80c-138">Ha egy MP4-fájlt progresszív letöltés útján tervez továbbítani, használja ezt a lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="7e80c-138">If you plan to deliver an MP4 using progressive download, use this option.</span></span> 
* <span data-ttu-id="7e80c-139">**StorageEncrypted** = **1**: Adja meg, ha a fájlok AES-256 bites titkosítással feltöltés és tárolás titkosítását.</span><span class="sxs-lookup"><span data-stu-id="7e80c-139">**StorageEncrypted** = **1**: Specify if you want for your files to be encrypted with AES-256 bit encryption for upload and storage.</span></span>
  
    <span data-ttu-id="7e80c-140">Ha az adategységen tárolótitkosítást alkalmaz, konfigurálnia kell az adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="7e80c-140">If your asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="7e80c-141">További információ: [objektumtovábbítási szabályzat konfigurálása](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="7e80c-141">For more information see [Configuring asset delivery policy](media-services-rest-configure-asset-delivery-policy.md).</span></span>
* <span data-ttu-id="7e80c-142">**CommonEncryptionProtected** = **2**: Adja meg, ha meg feltölteni egy közös titkosítási módszerrel (például PlayReady) védett fájlokkal.</span><span class="sxs-lookup"><span data-stu-id="7e80c-142">**CommonEncryptionProtected** = **2**: Specify if you are uploading files protected with a common encryption method (such as PlayReady).</span></span> 
* <span data-ttu-id="7e80c-143">**EnvelopeEncryptionProtected** = **4**: Adja meg, ha AES fájlok titkosított HLS meg feltölteni.</span><span class="sxs-lookup"><span data-stu-id="7e80c-143">**EnvelopeEncryptionProtected** = **4**: Specify if you are uploading HLS encrypted with AES files.</span></span> <span data-ttu-id="7e80c-144">Megjegyzés: ehhez a fájlokat a Transform Manager használatával kell kódolni és titkosítani.</span><span class="sxs-lookup"><span data-stu-id="7e80c-144">Note that the files must have been encoded and encrypted by Transform Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="7e80c-145">Ha az eszköz titkosítást fog használni, létre kell hoznia egy **ContentKey** , és az eszköz a következő témakörben leírtak szerint:[létrehozása egy ContentKey](media-services-rest-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="7e80c-145">If your asset will use encryption, you must create a **ContentKey** and link it to your asset as described in the following topic:[How to create a ContentKey](media-services-rest-create-contentkey.md).</span></span> <span data-ttu-id="7e80c-146">Vegye figyelembe, hogy a fájlok feltöltése az objektumba, után frissítenie kell a titkosítási tulajdonságok a a **AssetFile** entitás során kapott értékekkel a **eszköz** titkosítás.</span><span class="sxs-lookup"><span data-stu-id="7e80c-146">Note that after you upload the files into the asset, you need to update the encryption properties on the **AssetFile** entity with the values you got during the **Asset** encryption.</span></span> <span data-ttu-id="7e80c-147">Használatával teheti a **EGYESÍTÉSE** HTTP-kérelem.</span><span class="sxs-lookup"><span data-stu-id="7e80c-147">Do it by using the **MERGE** HTTP request.</span></span> 
> 
> 

<span data-ttu-id="7e80c-148">A következő példa bemutatja, hogyan hozzon létre egy eszközt.</span><span class="sxs-lookup"><span data-stu-id="7e80c-148">The following example shows how to create an asset.</span></span>

<span data-ttu-id="7e80c-149">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="7e80c-149">**HTTP Request**</span></span>

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

<span data-ttu-id="7e80c-150">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="7e80c-150">**HTTP Response**</span></span>

<span data-ttu-id="7e80c-151">Ha sikeres, a következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="7e80c-151">If successful, the following is returned:</span></span>

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

### <a name="create-an-assetfile"></a><span data-ttu-id="7e80c-152">Hozzon létre egy AssetFile</span><span class="sxs-lookup"><span data-stu-id="7e80c-152">Create an AssetFile</span></span>
<span data-ttu-id="7e80c-153">A [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entitás egy blob-tárolóban tárolt video- vagy fájlt jelöli.</span><span class="sxs-lookup"><span data-stu-id="7e80c-153">The [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="7e80c-154">Egy eszköz fájl mindig társítva van egy eszköz, és egy eszköz egy vagy több eszköz fájlt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7e80c-154">An asset file is always associated with an asset, and an asset may contain one or many asset files.</span></span> <span data-ttu-id="7e80c-155">A Media Services kódoló feladat sikertelen lesz, ha egy eszköz fájl objektumhoz nincs társítva egy digitális fájlhoz egy blob-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7e80c-155">The Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="7e80c-156">Vegye figyelembe, hogy a **AssetFile** példány és a tényleges médiafájl két különböző objektum.</span><span class="sxs-lookup"><span data-stu-id="7e80c-156">Note that the **AssetFile** instance and the actual media file are two distinct objects.</span></span> <span data-ttu-id="7e80c-157">A AssetFile példány media fájl metaadatainak tartalmaz, míg a médiafájl tartalmazza a tényleges médiatartalmakat.</span><span class="sxs-lookup"><span data-stu-id="7e80c-157">The AssetFile instance contains metadata about the media file, while the media file contains the actual media content.</span></span>

<span data-ttu-id="7e80c-158">Miután a digitális adathordozójának fájl feltöltése a blob-tárolóba, szüksége lesz a **EGYESÍTÉSE** frissíti a AssetFile a médiafájl információit tartalmazó (lásd a témakör későbbi részében) HTTP-kérelem.</span><span class="sxs-lookup"><span data-stu-id="7e80c-158">After you upload your digital media file into a blob container, you will use the **MERGE** HTTP request to update the AssetFile with information about your media file (as shown later in the topic).</span></span> 

<span data-ttu-id="7e80c-159">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="7e80c-159">**HTTP Request**</span></span>

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

<span data-ttu-id="7e80c-160">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="7e80c-160">**HTTP Response**</span></span>

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

### <a name="creating-the-accesspolicy-with-write-permission"></a><span data-ttu-id="7e80c-161">A AccessPolicy létrehozása írási engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="7e80c-161">Creating the AccessPolicy with write permission.</span></span>

>[!NOTE]
><span data-ttu-id="7e80c-162">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="7e80c-162">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="7e80c-163">Ha mindig ugyanazokat a napokat/hozzáférési engedélyeket használja (például olyan keresők szabályzatait, amelyek hosszú ideig érvényben maradnak, vagyis nem feltöltött szabályzatokat), a szabályzatazonosítónak is ugyanannak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7e80c-163">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="7e80c-164">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="7e80c-164">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="7e80c-165">Fájlok feltöltése a blob-tárolóba, mielőtt írásra, hogy egy eszköz házirend jogosultságok a hozzáférés beállítása.</span><span class="sxs-lookup"><span data-stu-id="7e80c-165">Before uploading any files into blob storage, set the access policy rights for writing to an asset.</span></span> <span data-ttu-id="7e80c-166">Ehhez a AccessPolicies entitáskészlet HTTP-kérelmek POST.</span><span class="sxs-lookup"><span data-stu-id="7e80c-166">To do that, POST an HTTP request to the AccessPolicies entity set.</span></span> <span data-ttu-id="7e80c-167">Adjon meg egy DurationInMinutes számot a létrehozása után, vagy egy belső kiszolgálót 500 hibaüzenetet kap vissza válaszként.</span><span class="sxs-lookup"><span data-stu-id="7e80c-167">Define a DurationInMinutes value upon creation or you will receive a 500 Internal Server error message back in response.</span></span> <span data-ttu-id="7e80c-168">A AccessPolicies további információkért lásd: [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span><span class="sxs-lookup"><span data-stu-id="7e80c-168">For more information on AccessPolicies, see [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span></span>

<span data-ttu-id="7e80c-169">A következő példa bemutatja, hogyan hozzon létre egy AccessPolicy:</span><span class="sxs-lookup"><span data-stu-id="7e80c-169">The following example shows how to create an AccessPolicy:</span></span>

<span data-ttu-id="7e80c-170">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="7e80c-170">**HTTP Request**</span></span>

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

<span data-ttu-id="7e80c-171">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="7e80c-171">**HTTP Request**</span></span>

    If successful, the following response is returned:

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

### <a name="get-the-upload-url"></a><span data-ttu-id="7e80c-172">A feltöltés URL-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="7e80c-172">Get the Upload URL</span></span>
<span data-ttu-id="7e80c-173">A tényleges feltöltés URL-címet kap, hozzon létre egy SAS-kereső.</span><span class="sxs-lookup"><span data-stu-id="7e80c-173">To receive the actual upload URL, create a SAS Locator.</span></span> <span data-ttu-id="7e80c-174">Keresők meghatározása a kezdési idő és a csatlakozási végpont típusú ügyfelek számára, szeretné, hogy egy eszköz lévő fájlok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="7e80c-174">Locators define the start time and type of connection endpoint for clients that want to access Files in an Asset.</span></span> <span data-ttu-id="7e80c-175">Létrehozhat több lokátor entitás egy adott AccessPolicy és eszköz párhoz különböző ügyfélkérelmek és kell kezelni.</span><span class="sxs-lookup"><span data-stu-id="7e80c-175">You can create multiple Locator entities for a given AccessPolicy and Asset pair to handle different client requests and needs.</span></span> <span data-ttu-id="7e80c-176">Egyes a Lokátorokat segítségével a StartTime érték és a AccessPolicy DurationInMinutes értéke határozza meg egy URL-cím használható idő hosszúsága.</span><span class="sxs-lookup"><span data-stu-id="7e80c-176">Each of these Locators use the StartTime value plus the DurationInMinutes value of the AccessPolicy to determine the length of time a URL can be used.</span></span> <span data-ttu-id="7e80c-177">További információkért lásd: [lokátor](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="7e80c-177">For more information, see [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>

<span data-ttu-id="7e80c-178">A SAS URL-cím formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="7e80c-178">A SAS URL has the following format:</span></span>

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

<span data-ttu-id="7e80c-179">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="7e80c-179">Some considerations apply:</span></span>

* <span data-ttu-id="7e80c-180">Egy adott eszközhöz társított egyszerre legfeljebb öt egyedi keresők tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="7e80c-180">You cannot have more than five unique Locators associated with a given Asset at one time.</span></span> <span data-ttu-id="7e80c-181">További információkért tekintse meg a lokátor.</span><span class="sxs-lookup"><span data-stu-id="7e80c-181">For more information, see Locator.</span></span>
* <span data-ttu-id="7e80c-182">Ha szeretné azonnal töltse fel a fájlokat, akkor a StartTime érték az aktuális időpont előtt öt percet kell beállítania.</span><span class="sxs-lookup"><span data-stu-id="7e80c-182">If you need to upload your files immediately, you should set your StartTime value to five minutes before the current time.</span></span> <span data-ttu-id="7e80c-183">Ennek az az oka lehet óra eltérésére az ügyfélszámítógép és a Media Services között.</span><span class="sxs-lookup"><span data-stu-id="7e80c-183">This is because there may be clock skew between your client machine and Media Services.</span></span> <span data-ttu-id="7e80c-184">Ezenkívül a StartTime érték a következő dátum és idő formátumban kell lennie: éééé-hh-SSz (például "2014-05-23T17:53:50Z").</span><span class="sxs-lookup"><span data-stu-id="7e80c-184">Also, your StartTime value must be in the following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>    
* <span data-ttu-id="7e80c-185">Előfordulhat, hogy a 30-40 második késleltetése, ha használható a lokátor létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="7e80c-185">There may be a 30-40 second delay after a Locator is created to when it is available for use.</span></span> <span data-ttu-id="7e80c-186">A probléma a SAS URL-cím és a forrás keresők egyaránt vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="7e80c-186">This issue applies to both SAS URL and Origin Locators.</span></span>

<span data-ttu-id="7e80c-187">A következő példa bemutatja, hogyan egy SAS URL-cím lokátor létrehozása a kérés törzsében ("1" egy SAS-kereső) és egy az Igényalapú származási kereső "2" típusú tulajdonság által meghatározott módon.</span><span class="sxs-lookup"><span data-stu-id="7e80c-187">The following example shows how to create a SAS URL Locator, as defined by the Type property in the request body ("1" for a SAS locator and "2" for an On-Demand origin locator).</span></span> <span data-ttu-id="7e80c-188">A **elérési** visszaadott tulajdonsága tartalmazza az URL-címet, fel kell töltenie a fájlt kell használnia.</span><span class="sxs-lookup"><span data-stu-id="7e80c-188">The **Path** property returned contains the URL that you must use to upload your file.</span></span>

<span data-ttu-id="7e80c-189">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="7e80c-189">**HTTP Request**</span></span>

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

<span data-ttu-id="7e80c-190">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="7e80c-190">**HTTP Response**</span></span>

<span data-ttu-id="7e80c-191">Ha sikeres, a következő választ ad vissza:</span><span class="sxs-lookup"><span data-stu-id="7e80c-191">If successful, the following response is returned:</span></span>

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

### <a name="upload-a-file-into-a-blob-storage-container"></a><span data-ttu-id="7e80c-192">Egy blob storage tárolóba-fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="7e80c-192">Upload a file into a blob storage container</span></span>
<span data-ttu-id="7e80c-193">Miután a AccessPolicy és lokátor beállítása, a rendszer a tényleges fájl feltöltése, egy Azure Blob Storage tárolóban, az Azure Storage REST API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="7e80c-193">Once you have the AccessPolicy and Locator set, the actual file is uploaded to an Azure Blob Storage container using the Azure Storage REST APIs.</span></span> <span data-ttu-id="7e80c-194">A fájlok blokkblobként kell feltöltenie.</span><span class="sxs-lookup"><span data-stu-id="7e80c-194">You must upload the files as block blobs.</span></span> <span data-ttu-id="7e80c-195">Lapblobokat nem Azure Media Services által támogatott.</span><span class="sxs-lookup"><span data-stu-id="7e80c-195">Page blobs are not supported by Azure Media Services.</span></span>  

> [!NOTE]
> <span data-ttu-id="7e80c-196">Hozzá kell adni a fájlnevet a lokátor feltölteni kívánt fájl **elérési** érték érkezett az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="7e80c-196">You must add the file name for the file you want to upload to the Locator **Path** value received in the previous section.</span></span> <span data-ttu-id="7e80c-197">Például https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="7e80c-197">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="7e80c-198">.</span><span class="sxs-lookup"><span data-stu-id="7e80c-198">.</span></span> <span data-ttu-id="7e80c-199">.</span><span class="sxs-lookup"><span data-stu-id="7e80c-199">.</span></span> <span data-ttu-id="7e80c-200">.</span><span class="sxs-lookup"><span data-stu-id="7e80c-200">.</span></span> 
> 
> 

<span data-ttu-id="7e80c-201">Az Azure storage blobs munkavégzés további információkért lásd: [Blob szolgáltatás REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="7e80c-201">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

### <a name="update-the-assetfile"></a><span data-ttu-id="7e80c-202">Frissítés a AssetFile</span><span class="sxs-lookup"><span data-stu-id="7e80c-202">Update the AssetFile</span></span>
<span data-ttu-id="7e80c-203">Most, hogy a fájl feltöltése a FileAsset méret (és más) adatainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="7e80c-203">Now that you've uploaded your file, update the FileAsset size (and other) information.</span></span> <span data-ttu-id="7e80c-204">Példa:</span><span class="sxs-lookup"><span data-stu-id="7e80c-204">For example:</span></span>

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


<span data-ttu-id="7e80c-205">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="7e80c-205">**HTTP Response**</span></span>

<span data-ttu-id="7e80c-206">Ha sikeres, a következőket adja vissza: HTTP/1.1 204 nem tartalom</span><span class="sxs-lookup"><span data-stu-id="7e80c-206">If successful, the following is returned: HTTP/1.1 204 No Content</span></span>

### <a name="delete-the-locator-and-accesspolicy"></a><span data-ttu-id="7e80c-207">A lokátor és AccessPolicy törlése</span><span class="sxs-lookup"><span data-stu-id="7e80c-207">Delete the Locator and AccessPolicy</span></span>
<span data-ttu-id="7e80c-208">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="7e80c-208">**HTTP Request**</span></span>

    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="7e80c-209">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="7e80c-209">**HTTP Response**</span></span>

<span data-ttu-id="7e80c-210">Ha sikeres, a következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="7e80c-210">If successful, the following is returned:</span></span>

    HTTP/1.1 204 No Content 
    ...

<span data-ttu-id="7e80c-211">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="7e80c-211">**HTTP Request**</span></span>

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="7e80c-212">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="7e80c-212">**HTTP Response**</span></span>

<span data-ttu-id="7e80c-213">Ha sikeres, a következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="7e80c-213">If successful, the following is returned:</span></span>

    HTTP/1.1 204 No Content 
    ...

## <span data-ttu-id="7e80c-214"><a id="upload_in_bulk"></a>Eszközök tömeges feltöltése</span><span class="sxs-lookup"><span data-stu-id="7e80c-214"><a id="upload_in_bulk"></a>Upload assets in bulk</span></span>
### <a name="create-the-ingestmanifest"></a><span data-ttu-id="7e80c-215">A IngestManifest létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e80c-215">Create the IngestManifest</span></span>
<span data-ttu-id="7e80c-216">A IngestManifest egy olyan tároló, azon eszközök, eszköz fájlok és statisztikai adatainak tömeges választásával dolgozhat fel, a csoport állapotának meghatározására használható.</span><span class="sxs-lookup"><span data-stu-id="7e80c-216">The IngestManifest is a container for a set of assets, asset files, and statistic information that can be used to determine the progress of bulk ingesting for the set.</span></span>

<span data-ttu-id="7e80c-217">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="7e80c-217">**HTTP Request**</span></span>

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

### <a name="create-assets"></a><span data-ttu-id="7e80c-218">Eszközök létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e80c-218">Create assets</span></span>
<span data-ttu-id="7e80c-219">Mielőtt létrehozná a IngestManifestAsset, létrehozásához szükséges az eszköz, amely segítségével tömegesen választásával dolgozhat fel befejeződik.</span><span class="sxs-lookup"><span data-stu-id="7e80c-219">Before creating the IngestManifestAsset, you need to create the Asset that will be completed using bulk ingesting.</span></span> <span data-ttu-id="7e80c-220">Az eszköz egy olyan tároló, típusok vagy a Media Services, beleértve a videó, hang, képeket, miniatűröket, szöveges nyomon követi és feliratfájlokat fájlok objektumokat.</span><span class="sxs-lookup"><span data-stu-id="7e80c-220">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="7e80c-221">A REST API-ban az eszköz létrehozásához egy HTTP POST kérést küld a Microsoft Azure Media Services és az eszköz minden tulajdonságadatokat helyezi el a kérés törzsében. Ebben a példában az eszköz jön létre, a kérelem törzsében található StorageEncrption(1) funkcióval.</span><span class="sxs-lookup"><span data-stu-id="7e80c-221">In the REST API, creating an Asset requires sending a HTTP POST request to Microsoft Azure Media Services and placing any property information about your asset in the request body.In this example, the Asset is created using the StorageEncrption(1) option included with the request body.</span></span>

<span data-ttu-id="7e80c-222">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="7e80c-222">**HTTP Response**</span></span>

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

### <a name="create-the-ingestmanifestassets"></a><span data-ttu-id="7e80c-223">A IngestManifestAssets létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e80c-223">Create the IngestManifestAssets</span></span>
<span data-ttu-id="7e80c-224">IngestManifestAssets belül egy IngestManifest eszközök tömeges választásával dolgozhat fel használt jelölik.</span><span class="sxs-lookup"><span data-stu-id="7e80c-224">IngestManifestAssets represent Assets within an IngestManifest that are used with bulk ingesting.</span></span> <span data-ttu-id="7e80c-225">A alapvetően az eszköz kapcsolódik a jegyzékfájlban.</span><span class="sxs-lookup"><span data-stu-id="7e80c-225">The basically link the asset to the manifest.</span></span> <span data-ttu-id="7e80c-226">Az Azure Media Services belső a fájl feltöltése a IngestManifestAsset társított IngestManifestFiles gyűjtemény alapján figyeli.</span><span class="sxs-lookup"><span data-stu-id="7e80c-226">Azure Media Services watches internally for the file upload based on IngestManifestFiles collection associated to the IngestManifestAsset.</span></span> <span data-ttu-id="7e80c-227">Ha ezek a fájlok feltöltése után az eszköz befejeződött.</span><span class="sxs-lookup"><span data-stu-id="7e80c-227">Once these files are uploaded, the asset is completed.</span></span> <span data-ttu-id="7e80c-228">Létrehozhat egy új IngestManifestAsset egy HTTP POST-kérelmet.</span><span class="sxs-lookup"><span data-stu-id="7e80c-228">You can create a new IngestManifestAsset with a HTTP POST request.</span></span> <span data-ttu-id="7e80c-229">A kérelem törzsében szereplő közé tartozik a IngestManifest azonosítója és az eszköz azonosítója, amely a IngestManifestAsset kell összeköt tömeges választásával dolgozhat fel.</span><span class="sxs-lookup"><span data-stu-id="7e80c-229">In the request body, include the IngestManifest Id and the Asset Id that the IngestManifestAsset should link together for bulk ingesting.</span></span>

<span data-ttu-id="7e80c-230">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="7e80c-230">**HTTP Response**</span></span>

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


### <a name="create-the-ingestmanifestfiles-for-each-asset"></a><span data-ttu-id="7e80c-231">Az egyes eszközök a IngestManifestFiles létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e80c-231">Create the IngestManifestFiles for each Asset</span></span>
<span data-ttu-id="7e80c-232">Egy IngestManifestFile tömeges választásával dolgozhat fel, az eszközhöz tartozó részeként lesz feltöltve tényleges video- vagy blob objektumot jelöli.</span><span class="sxs-lookup"><span data-stu-id="7e80c-232">An IngestManifestFile represents an actual video or audio blob object that will be uploaded as part of bulk ingesting for an asset.</span></span> <span data-ttu-id="7e80c-233">Titkosítási kapcsolódó tulajdonságok esetén nincs szükség, kivéve, ha az eszköz egy titkosítási beállítást használja.</span><span class="sxs-lookup"><span data-stu-id="7e80c-233">Encryption related properties are not required unless the asset is using an encryption option.</span></span> <span data-ttu-id="7e80c-234">A jelen szakaszban használt példa bemutatja, a korábban létrehozott eszköz használó StorageEncryption egy IngestManifestFile létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7e80c-234">The example used in this section demonstrates creating an IngestManifestFile that uses StorageEncryption for the Asset previously created.</span></span>

<span data-ttu-id="7e80c-235">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="7e80c-235">**HTTP Response**</span></span>

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

### <a name="upload-the-files-to-blob-storage"></a><span data-ttu-id="7e80c-236">A fájlok feltöltése a Blob Storage</span><span class="sxs-lookup"><span data-stu-id="7e80c-236">Upload the Files to Blob Storage</span></span>
<span data-ttu-id="7e80c-237">A nagy sebességű ügyfélalkalmazás képes az eszköz fájlok feltöltése a blob storage tárolót a IngestManifest BlobStorageUriForUpload tulajdonsága által biztosított Uri használható.</span><span class="sxs-lookup"><span data-stu-id="7e80c-237">You can use any high speed client application capable of uploading the asset files to the blob storage container Uri provided by the BlobStorageUriForUpload property of the IngestManifest.</span></span> <span data-ttu-id="7e80c-238">Egy figyelmet a jelentősebb nagy sebességű feltöltési szolgáltatás [Aspera igény szerinti Azure alkalmazáshoz](http://go.microsoft.com/fwlink/?LinkId=272001).</span><span class="sxs-lookup"><span data-stu-id="7e80c-238">One notable high speed upload service is [Aspera On Demand for Azure Application](http://go.microsoft.com/fwlink/?LinkId=272001).</span></span>

### <a name="monitor-bulk-ingest-progress"></a><span data-ttu-id="7e80c-239">A figyelő tömeges betöltési folyamatban</span><span class="sxs-lookup"><span data-stu-id="7e80c-239">Monitor Bulk Ingest Progress</span></span>
<span data-ttu-id="7e80c-240">Az előrehaladást tömeges választásával dolgozhat fel egy IngestManifest műveletek a IngestManifest statisztika tulajdonságának lekérdezésével.</span><span class="sxs-lookup"><span data-stu-id="7e80c-240">You can monitor the progress of bulk ingesting operations for an IngestManifest by polling the Statistics property of the IngestManifest.</span></span> <span data-ttu-id="7e80c-241">Hogy a tulajdonság egy összetett típus [IngestManifestStatistics](https://docs.microsoft.com/rest/api/media/operations/ingestmanifeststatistics).</span><span class="sxs-lookup"><span data-stu-id="7e80c-241">That property is a complex type, [IngestManifestStatistics](https://docs.microsoft.com/rest/api/media/operations/ingestmanifeststatistics).</span></span> <span data-ttu-id="7e80c-242">A statisztika tulajdonság lekérdezésére, igényelnie HTTP GET átadja a IngestManifest azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="7e80c-242">To poll the Statistics property, submit a HTTP GET request passing the IngestManifest Id.</span></span>

## <a name="create-contentkeys-used-for-encryption"></a><span data-ttu-id="7e80c-243">A titkosításhoz használt ContentKeys létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e80c-243">Create ContentKeys used for encryption</span></span>
<span data-ttu-id="7e80c-244">Ha az eszköz titkosítást fog használni, létre kell hoznia az adategység-fájloknak létrehozása előtt titkosításhoz használandó ContentKey.</span><span class="sxs-lookup"><span data-stu-id="7e80c-244">If your asset will use encryption, you must create the ContentKey to be used for encryption before creating the asset files.</span></span> <span data-ttu-id="7e80c-245">A tárolás titkosítását a következő tulajdonságok tartozhatnak a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="7e80c-245">For storage encryption, the following properties should be included in the request body.</span></span>

| <span data-ttu-id="7e80c-246">Kérelem törzse tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7e80c-246">Request body property</span></span> | <span data-ttu-id="7e80c-247">Leírás</span><span class="sxs-lookup"><span data-stu-id="7e80c-247">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7e80c-248">Azonosító</span><span class="sxs-lookup"><span data-stu-id="7e80c-248">Id</span></span> |<span data-ttu-id="7e80c-249">A ContentKey azonosítója, amely azt ragozott formáival létrehozása a következő formátumban "nb:kid:UUID:<NEW GUID>".</span><span class="sxs-lookup"><span data-stu-id="7e80c-249">The ContentKey Id which we generate ourselves using the following format, “nb:kid:UUID:<NEW GUID>”.</span></span> |
| <span data-ttu-id="7e80c-250">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="7e80c-250">ContentKeyType</span></span> |<span data-ttu-id="7e80c-251">Ez az a tartalom írja be a tartalom kulcs egész szám lehet.</span><span class="sxs-lookup"><span data-stu-id="7e80c-251">This is the content key type as an integer for this content key.</span></span> <span data-ttu-id="7e80c-252">Az érték 1 storage-titkosítás továbbítja azt.</span><span class="sxs-lookup"><span data-stu-id="7e80c-252">We pass the value 1 for storage encryption.</span></span> |
| <span data-ttu-id="7e80c-253">EncryptedContentKey</span><span class="sxs-lookup"><span data-stu-id="7e80c-253">EncryptedContentKey</span></span> |<span data-ttu-id="7e80c-254">Létrehozhatunk egy új tartalom kulcs értéke pedig 256 bites (32 bájt) értéket.</span><span class="sxs-lookup"><span data-stu-id="7e80c-254">We create a new content key value which is a 256-bit (32 byte) value.</span></span> <span data-ttu-id="7e80c-255">A kulcs titkosított a tárolási titkosítási X.509 tanúsítvány, amely által egy HTTP GET kérelem végrehajtása a GetProtectionKeyId és GetProtectionKey metódusok nem beolvasni a Microsoft Azure Media Services használatával.</span><span class="sxs-lookup"><span data-stu-id="7e80c-255">The key is encrypted using the storage encryption X.509 certificate which we retrieve from Microsoft Azure Media Services by executing a HTTP GET request for the GetProtectionKeyId and GetProtectionKey Methods.</span></span> |
| <span data-ttu-id="7e80c-256">ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="7e80c-256">ProtectionKeyId</span></span> |<span data-ttu-id="7e80c-257">Ez az a védelmi tároló titkosítási X.509-tanúsítvány, amely a tartalom kulcs titkosításához használt kulcs azonosítója.</span><span class="sxs-lookup"><span data-stu-id="7e80c-257">This is the protection key id for the storage encryption X.509 certificate that was used to encrypt our content key.</span></span> |
| <span data-ttu-id="7e80c-258">ProtectionKeyType</span><span class="sxs-lookup"><span data-stu-id="7e80c-258">ProtectionKeyType</span></span> |<span data-ttu-id="7e80c-259">Ez egy, a védelem a tartalom kulcs titkosításához használt kulcs a titkosítási típus.</span><span class="sxs-lookup"><span data-stu-id="7e80c-259">This is the encryption type for the protection key that was used to encrypt the content key.</span></span> <span data-ttu-id="7e80c-260">Ez az érték a fenti példában StorageEncryption(1).</span><span class="sxs-lookup"><span data-stu-id="7e80c-260">This value is StorageEncryption(1) for our example.</span></span> |
| <span data-ttu-id="7e80c-261">Ellenőrzőösszeg</span><span class="sxs-lookup"><span data-stu-id="7e80c-261">Checksum</span></span> |<span data-ttu-id="7e80c-262">Az MD5 számított ellenőrzőösszeg a tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="7e80c-262">The MD5 calculated checksum for the content key.</span></span> <span data-ttu-id="7e80c-263">A tartalom azonosítója a tartalom kulccsal titkosítja számítja ki.</span><span class="sxs-lookup"><span data-stu-id="7e80c-263">It is computed by encrypting the content Id with the content key.</span></span> <span data-ttu-id="7e80c-264">A mintakód bemutatja, hogyan ellenőrzőösszeg számítása.</span><span class="sxs-lookup"><span data-stu-id="7e80c-264">The example code demonstrates how to calculate the checksum.</span></span> |

<span data-ttu-id="7e80c-265">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="7e80c-265">**HTTP Response**</span></span>

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

### <a name="link-the-contentkey-to-the-asset"></a><span data-ttu-id="7e80c-266">Az eszköz a ContentKey csatolása</span><span class="sxs-lookup"><span data-stu-id="7e80c-266">Link the ContentKey to the Asset</span></span>
<span data-ttu-id="7e80c-267">A ContentKey úgy, hogy a HTTP POST-kérelmet küld egy vagy több eszköz társítva.</span><span class="sxs-lookup"><span data-stu-id="7e80c-267">The ContentKey is associated to one or more assets by sending a HTTP POST request.</span></span> <span data-ttu-id="7e80c-268">A következő kérelme, mert az azonosítója. példa az eszköznek a példa ContentKey csatolása példa</span><span class="sxs-lookup"><span data-stu-id="7e80c-268">The following request is an example to link the example ContentKey to the example asset by Id.</span></span>

<span data-ttu-id="7e80c-269">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="7e80c-269">**HTTP Response**</span></span>

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

<span data-ttu-id="7e80c-270">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="7e80c-270">**HTTP Response**</span></span>

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net

## <a name="next-steps"></a><span data-ttu-id="7e80c-271">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e80c-271">Next steps</span></span>

<span data-ttu-id="7e80c-272">Most már kódolhatja a feltöltött adategységeket.</span><span class="sxs-lookup"><span data-stu-id="7e80c-272">You can now encode your uploaded assets.</span></span> <span data-ttu-id="7e80c-273">További információ: [Encode Assets](media-services-portal-encode.md) (Adategységek kódolása).</span><span class="sxs-lookup"><span data-stu-id="7e80c-273">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="7e80c-274">Emellett az Azure Functions használatával is elindíthatja a kódolási feladatokat a konfigurált tárolóba érkező fájlok alapján.</span><span class="sxs-lookup"><span data-stu-id="7e80c-274">You can also use Azure Functions to trigger an encoding job based on a file arriving in the configured container.</span></span> <span data-ttu-id="7e80c-275">További információkért tekintse meg [ezt a mintát](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="7e80c-275">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="7e80c-276">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="7e80c-276">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7e80c-277">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="7e80c-277">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[How to Get a Media Processor]: media-services-get-media-processor.md

