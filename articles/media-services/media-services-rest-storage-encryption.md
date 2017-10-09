---
title: "aaaEncrypting a tartalmaknak a tárolás titkosítása AMS REST API használatával"
description: "Megtudhatja, hogyan tooencrypt a tartalmaknak a tárolás titkosítása AMS REST API-k használatával."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a0a79f3d-76a1-4994-9202-59b91a2230e0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: d5f8cb8dd1dcded76c9fededccc772d8102ccbad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encrypting-your-content-with-storage-encryption"></a><span data-ttu-id="c7dfb-103">A tárolás titkosítása tartalom titkosítása</span><span class="sxs-lookup"><span data-stu-id="c7dfb-103">Encrypting your content with storage encryption</span></span>

<span data-ttu-id="c7dfb-104">Lehetőleg tooencrypt a tartalomhoz helyileg az AES-256 algoritmussal titkosítási bit, és azt a rendszer hol tárolja tárolás titkosítása tooAzure feltöltése.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-104">It is highly recommended tooencrypt your content locally using AES-256 bit encryption and then upload it tooAzure Storage where it will be stored encrypted at rest.</span></span>

<span data-ttu-id="c7dfb-105">Ez a cikk áttekintést AMS tárolás titkosítása, és bemutatja, hogyan tooupload hello tárolási titkosított tartalom:</span><span class="sxs-lookup"><span data-stu-id="c7dfb-105">This article gives an overview of AMS storage encryption and shows you how tooupload hello storage encrypted content:</span></span>

* <span data-ttu-id="c7dfb-106">Hozzon létre egy tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-106">Create a content key.</span></span>
* <span data-ttu-id="c7dfb-107">Hozzon létre egy eszközt.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-107">Create an Asset.</span></span> <span data-ttu-id="c7dfb-108">Állítsa be a hello AssetCreationOption tooStorageEncryption hello eszköz létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-108">Set hello AssetCreationOption tooStorageEncryption when creating hello Asset.</span></span>
  
     <span data-ttu-id="c7dfb-109">Titkosított eszközök kapcsolódó tartalom toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-109">Encrypted assets have toobe associated with content keys.</span></span>
* <span data-ttu-id="c7dfb-110">Hivatkozás hello tartalom kulcs toohello eszköz.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-110">Link hello content key toohello asset.</span></span>  
* <span data-ttu-id="c7dfb-111">Állítsa be hello titkosítási kapcsolatos paramétereket az hello AssetFile entitásokra.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-111">Set hello encryption related parameters on hello AssetFile entities.</span></span>

## <a name="considerations"></a><span data-ttu-id="c7dfb-112">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="c7dfb-112">Considerations</span></span> 

<span data-ttu-id="c7dfb-113">Ha azt szeretné, hogy a tároló titkosított eszköz toodeliver, konfigurálnia kell a hello adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-113">If you want toodeliver a storage encrypted asset, you must configure hello asset’s delivery policy.</span></span> <span data-ttu-id="c7dfb-114">Mielőtt az eszköz továbbítható, hello adatfolyam-kiszolgáló eltávolítása hello tárolás titkosítása és adatfolyamokat a tartalom hello segítségével megadott továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-114">Before your asset can be streamed, hello streaming server removes hello storage encryption and streams your content using hello specified delivery policy.</span></span> <span data-ttu-id="c7dfb-115">További információkért lásd: [konfigurálása az adategység továbbítási házirendjeit](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="c7dfb-115">For more information, see [Configuring Asset Delivery Policies](media-services-rest-configure-asset-delivery-policy.md).</span></span>

<span data-ttu-id="c7dfb-116">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-116">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="c7dfb-117">További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="c7dfb-117">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span> 

## <a name="connect-toomedia-services"></a><span data-ttu-id="c7dfb-118">Connect tooMedia szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="c7dfb-118">Connect tooMedia Services</span></span>

<span data-ttu-id="c7dfb-119">Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="c7dfb-119">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="c7dfb-120">Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-120">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="c7dfb-121">Meg kell nyitnia a további hívások toohello új URI.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-121">You must make subsequent calls toohello new URI.</span></span>

## <a name="storage-encryption-overview"></a><span data-ttu-id="c7dfb-122">Tárolás titkosítási – áttekintés</span><span class="sxs-lookup"><span data-stu-id="c7dfb-122">Storage encryption overview</span></span>
<span data-ttu-id="c7dfb-123">hello AMS tárolás titkosítása vonatkozik **AES-Parancsra** mód titkosítási toohello teljes fájlt.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-123">hello AMS storage encryption applies **AES-CTR** mode encryption toohello entire file.</span></span>  <span data-ttu-id="c7dfb-124">AES-Parancsra módja a blokktitkosításon, amelyek titkosíthatják a tetszőleges hosszúságú adatok kitöltési szükségessége nélkül.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-124">AES-CTR mode is a block cipher that can encrypt arbitrary length data without need for padding.</span></span> <span data-ttu-id="c7dfb-125">Egy számláló blokkot, amelynek hello AES algoritmus és XOR-ing hello kimenet az AES hello adatok tooencrypt titkosításával működik, illetve visszafejteni.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-125">It operates by encrypting a counter block with hello AES algorithm and then XOR-ing hello output of AES with hello data tooencrypt or decrypt.</span></span>  <span data-ttu-id="c7dfb-126">hello számláló blokk használt összeállított hello InitializationVector toobytes 0 too7 hello számláló értékének hello értékének másolásával és a 8 bájtos too15 hello számláló értékének toozero.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-126">hello counter block used is constructed by copying hello value of hello InitializationVector toobytes 0 too7 of hello counter value and bytes 8 too15 of hello counter value are set toozero.</span></span> <span data-ttu-id="c7dfb-127">Hello 16 bájtos számláló blokk, 8 bájtos too15 (azaz hello legkisebb helyiértékű bájt) kell használni, mint egy egyszerű 64 bites előjel nélküli egész szám, amely minden ezt követő adatblokk adatfeldolgozási eggyel növekszik, és van hálózati tartani.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-127">Of hello 16 byte counter block, bytes 8 too15 (i.e. hello least significant bytes) are used as a simple 64 bit unsigned integer that is incremented by one for each subsequent block of data processed and is kept in network byte order.</span></span> <span data-ttu-id="c7dfb-128">Figyelje meg, hogy ha az egész hello maximális értékének elérésekor (0xFFFFFFFFFFFFFFFF) majd növekvő hello blokk számláló toozero (8 bájtos too15) visszaállítja az nem befolyásolja a többi hello hello számláló (vagyis 0 bájt too7) 64 bites.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-128">Note that if this integer reaches hello maximum value (0xFFFFFFFFFFFFFFFF) then incrementing it resets hello block counter toozero (bytes 8 too15) without affecting hello other 64 bits of hello counter (i.e. bytes 0 too7).</span></span>   <span data-ttu-id="c7dfb-129">Rendelés toomaintain hello biztonsági hello AES-Parancsra mód titkosítási, hello InitializationVector érték egy adott kulcsazonosító minden tartalomkulcsot az egyes fájlok egyedinek kell lennie, és fájlok legfeljebb 2 ^ 64 blokkok hossza.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-129">In order toomaintain hello security of hello AES-CTR mode encryption, hello InitializationVector value for a given Key Identifier for each content key shall be unique for each file and files shall be less than 2^64 blocks in length.</span></span>  <span data-ttu-id="c7dfb-130">Ez a tooensure, hogy a számláló értéke soha nem használja fel újra egy adott kulccsal.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-130">This is tooensure that a counter value is never reused with a given key.</span></span> <span data-ttu-id="c7dfb-131">Hello Parancsra móddal kapcsolatos további információkért lásd: [a wiki lapon](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (hello wikicikket használ hello kifejezés "Nonce" helyett "InitializationVector").</span><span class="sxs-lookup"><span data-stu-id="c7dfb-131">For more information about hello CTR mode, see [this wiki page](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (hello wiki article uses hello term "Nonce" instead of "InitializationVector").</span></span>

<span data-ttu-id="c7dfb-132">Használjon **tárolás titkosítása** tooencrypt a tiszta tartalom helyileg az AES-256 algoritmussal bit a titkosítási, majd töltse fel tooAzure helyén tárolás titkosítása.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-132">Use **Storage Encryption** tooencrypt your clear content locally using AES-256 bit encryption and then upload it tooAzure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="c7dfb-133">Storage-titkosítással védett adategységek automatikusan titkosítás és egy titkosított fájl rendszer előzetes tooencoding, és ha szükséges újra titkosítani előzetes toouploading egy új kimeneti eszközként helyezve.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-133">Assets protected with storage encryption are automatically unencrypted and placed in an encrypted file system prior tooencoding, and optionally re-encrypted prior toouploading back as a new output asset.</span></span> <span data-ttu-id="c7dfb-134">hello elsődleges használati eset a tárolás titkosítása akkor, ha azt szeretné, hogy a jó minőségű bemeneti médiafájljait erős titkosítással, rest-lemezen toosecure.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-134">hello primary use case for storage encryption is when you want toosecure your high quality input media files with strong encryption at rest on disk.</span></span>

<span data-ttu-id="c7dfb-135">A sorrend toodeliver tárolási titkosított eszköz konfigurálnia kell hello adategység továbbítási házirendjét, a Media Services tudja, hogyan szeretné toodeliver a tartalom.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-135">In order toodeliver a storage encrypted asset, you must configure hello asset’s delivery policy so Media Services knows how you want toodeliver your content.</span></span> <span data-ttu-id="c7dfb-136">Mielőtt az eszköz továbbítható, hello adatfolyam-kiszolgáló eltávolítása hello tárolás titkosítása és adatfolyamokat a tartalom hello segítségével megadott továbbítási házirendjét (például AES, általános titkosítás vagy titkosítás nélkül).</span><span class="sxs-lookup"><span data-stu-id="c7dfb-136">Before your asset can be streamed, hello streaming server removes hello storage encryption and streams your content using hello specified delivery policy (for example, AES, common encryption, or no encryption).</span></span>

## <a name="create-contentkeys-used-for-encryption"></a><span data-ttu-id="c7dfb-137">A titkosításhoz használt ContentKeys létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7dfb-137">Create ContentKeys used for encryption</span></span>
<span data-ttu-id="c7dfb-138">Titkosított eszközök tárolás titkosítása kulcshoz tartozó toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-138">Encrypted assets have toobe associated with Storage Encryption key.</span></span> <span data-ttu-id="c7dfb-139">Hello tartalom kulcs toobe titkosítja az eszköz fájlok hello létrehozása előtt létre kell hoznia.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-139">You must create hello content key toobe used for encryption before creating hello asset files.</span></span> <span data-ttu-id="c7dfb-140">Ez a szakasz ismerteti, hogyan toocreate egy tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-140">This section describes how toocreate a content key.</span></span>

<span data-ttu-id="c7dfb-141">hello az alábbiakban általános lépésekkel végezhető tartalomkulcs fog társítani eszközök titkosított toobe generálásához.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-141">hello following are general steps for generating content keys that you will associate with assets that you want toobe encrypted.</span></span> 

1. <span data-ttu-id="c7dfb-142">A tárolás titkosítása véletlenszerűen 32 bájtos AES kulcs létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-142">For storage encryption, randomly generate a 32-byte AES key.</span></span> 
   
    <span data-ttu-id="c7dfb-143">Ez az objektum, amely azt jelenti, hogy a hozzárendelt összes fájl hello tartalomkulcsot lesz az adott eszközre fog kell toouse hello azonos tartalomkulcsot a visszafejtés során.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-143">This will be hello content key for your asset, which means all files associated with that asset will need toouse hello same content key during decryption.</span></span> 
2. <span data-ttu-id="c7dfb-144">Hello hívás [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) és [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) módszerek tooget hello kell lennie a használt tooencrypt megfelelő X.509-tanúsítvány a tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-144">Call hello [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) and [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) methods tooget hello correct X.509 Certificate that must be used tooencrypt your content key.</span></span>
3. <span data-ttu-id="c7dfb-145">Hello nyilvános kulcsával hello X.509 tanúsítvány a tartalom kulcs titkosításához.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-145">Encrypt your content key with hello public key of hello X.509 Certificate.</span></span> 
   
   <span data-ttu-id="c7dfb-146">Media Services .NET SDK RSA végrehajtásakor hello titkosítási OAEP típusú használ.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-146">Media Services .NET SDK uses RSA with OAEP when doing hello encryption.</span></span>  <span data-ttu-id="c7dfb-147">Láthatja, hogy a .NET típusú példát a hello [EncryptSymmetricKeyData függvény](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span><span class="sxs-lookup"><span data-stu-id="c7dfb-147">You can see a .NET example in hello [EncryptSymmetricKeyData function](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span></span>
4. <span data-ttu-id="c7dfb-148">Hozzon létre egy ellenőrzőösszeget hello kulcsazonosító és tartalomkulcsot számítja.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-148">Create a checksum value calculated using hello key identifier and content key.</span></span> <span data-ttu-id="c7dfb-149">.NET típusú példát a következő hello kiszámítja a hello kulcsazonosító hello GUID része hello ellenőrzőösszeg és hello törölje tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-149">hello following .NET example calculates hello checksum using hello GUID part of hello key identifier and hello clear content key.</span></span>

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting hello KID
            // with hello content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }

1. <span data-ttu-id="c7dfb-150">Hozzon létre hello tartalomkulcsot hello **EncryptedContentKey** (átalakítás toobase64 kódolású karakterlánc) **ProtectionKeyId**, **ProtectionKeyType**,  **ContentKeyType**, és **ellenőrzőösszeg** értékeket az előző lépésben kapott.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-150">Create hello Content key with hello **EncryptedContentKey** (converted toobase64-encoded string), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**, and **Checksum** values you have received in previous steps.</span></span>

    <span data-ttu-id="c7dfb-151">A tárolás titkosítása hello következő tulajdonságok tartozhatnak hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-151">For storage encryption, hello following properties should be included in hello request body.</span></span>

    <span data-ttu-id="c7dfb-152">Kérelem törzse tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c7dfb-152">Request body property</span></span>    | <span data-ttu-id="c7dfb-153">Leírás</span><span class="sxs-lookup"><span data-stu-id="c7dfb-153">Description</span></span>
    ---|---
    <span data-ttu-id="c7dfb-154">Azonosító</span><span class="sxs-lookup"><span data-stu-id="c7dfb-154">Id</span></span> | <span data-ttu-id="c7dfb-155">hello ContentKey azonosítója, amely azt készítése ragozott formáival használatával a következő hello formázásához "nb:kid:UUID:<NEW GUID>".</span><span class="sxs-lookup"><span data-stu-id="c7dfb-155">hello ContentKey Id which we generate ourselves using hello following format, “nb:kid:UUID:<NEW GUID>”.</span></span>
    <span data-ttu-id="c7dfb-156">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="c7dfb-156">ContentKeyType</span></span> | <span data-ttu-id="c7dfb-157">Ez az hello kulcs tartalomtípus a tartalom kulcs egész szám lehet.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-157">This is hello content key type as an integer for this content key.</span></span> <span data-ttu-id="c7dfb-158">Azt adja át a tárolás titkosítása hello értéke 1.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-158">We pass hello value 1 for storage encryption.</span></span>
    <span data-ttu-id="c7dfb-159">EncryptedContentKey</span><span class="sxs-lookup"><span data-stu-id="c7dfb-159">EncryptedContentKey</span></span> | <span data-ttu-id="c7dfb-160">Létrehozhatunk egy új tartalom kulcs értéke pedig 256 bites (32 bájt) értéket.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-160">We create a new content key value which is a 256-bit (32 byte) value.</span></span> <span data-ttu-id="c7dfb-161">hello kulcs Titkosított hello tárolási titkosítási X.509 tanúsítvány, amely által egy HTTP GET kérelem hello GetProtectionKeyId és GetProtectionKey metódusok végrehajtása nem beolvasni a Microsoft Azure Media Services használatával.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-161">hello key is encrypted using hello storage encryption X.509 certificate which we retrieve from Microsoft Azure Media Services by executing a HTTP GET request for hello GetProtectionKeyId and GetProtectionKey Methods.</span></span> <span data-ttu-id="c7dfb-162">Tegyük fel, tekintse meg a következő kód .NET hello: hello **EncryptSymmetricKeyData** definiált metódus [Itt](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span><span class="sxs-lookup"><span data-stu-id="c7dfb-162">As an example, see hello following .NET code: hello  **EncryptSymmetricKeyData** method defined [here](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span></span>
    <span data-ttu-id="c7dfb-163">ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="c7dfb-163">ProtectionKeyId</span></span> | <span data-ttu-id="c7dfb-164">Ez az hello védelmi hello storage encryption X.509 tanúsítvány, de a használt tooencrypt azonosítója a tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-164">This is hello protection key id for hello storage encryption X.509 certificate that was used tooencrypt our content key.</span></span>
    <span data-ttu-id="c7dfb-165">ProtectionKeyType</span><span class="sxs-lookup"><span data-stu-id="c7dfb-165">ProtectionKeyType</span></span> | <span data-ttu-id="c7dfb-166">Ez az hello titkosítási típus hello védelmi kulcs, de a használt tooencrypt hello tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-166">This is hello encryption type for hello protection key that was used tooencrypt hello content key.</span></span> <span data-ttu-id="c7dfb-167">Ez az érték a fenti példában StorageEncryption(1).</span><span class="sxs-lookup"><span data-stu-id="c7dfb-167">This value is StorageEncryption(1) for our example.</span></span>
    <span data-ttu-id="c7dfb-168">Ellenőrzőösszeg</span><span class="sxs-lookup"><span data-stu-id="c7dfb-168">Checksum</span></span> |<span data-ttu-id="c7dfb-169">hello MD5 számított ellenőrzőösszege hello tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-169">hello MD5 calculated checksum for hello content key.</span></span> <span data-ttu-id="c7dfb-170">Hello tartalmat azonosító hello tartalomkulcsot titkosításával számítja ki.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-170">It is computed by encrypting hello content Id with hello content key.</span></span> <span data-ttu-id="c7dfb-171">hello példakód bemutatja, hogyan toocalculate hello ellenőrzőösszeg.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-171">hello example code demonstrates how toocalculate hello checksum.</span></span>


### <a name="retrieve-hello-protectionkeyid"></a><span data-ttu-id="c7dfb-172">Hello ProtectionKeyId beolvasása</span><span class="sxs-lookup"><span data-stu-id="c7dfb-172">Retrieve hello ProtectionKeyId</span></span>
<span data-ttu-id="c7dfb-173">hello a következő példa bemutatja, hogyan tooretrieve hello ProtectionKeyId, egy tanúsítvány-ujjlenyomat, hello tanúsítványt kell használni a tartalom kulcs titkosításához.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-173">hello following example shows how tooretrieve hello ProtectionKeyId, a certificate thumbprint, for hello certificate you must use when encrypting your content key.</span></span> <span data-ttu-id="c7dfb-174">Hajtsa végre a lépés toomake meg arról, hogy már rendelkezik a megfelelő tanúsítvánnyal hello a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-174">Do this step toomake sure that you already have hello appropriate certificate on your machine.</span></span>

<span data-ttu-id="c7dfb-175">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="c7dfb-175">Request:</span></span>

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="c7dfb-176">Válasz:</span><span class="sxs-lookup"><span data-stu-id="c7dfb-176">Response:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

### <a name="retrieve-hello-protectionkey-for-hello-protectionkeyid"></a><span data-ttu-id="c7dfb-177">A hello ProtectionKeyId ProtectionKey hello beolvasása</span><span class="sxs-lookup"><span data-stu-id="c7dfb-177">Retrieve hello ProtectionKey for hello ProtectionKeyId</span></span>
<span data-ttu-id="c7dfb-178">hello következő példa bemutatja, hogyan tooretrieve hello X.509-tanúsítvány hello ProtectionKeyId kapott hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-178">hello following example shows how tooretrieve hello X.509 certificate using hello ProtectionKeyId you received in hello previous step.</span></span>

<span data-ttu-id="c7dfb-179">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="c7dfb-179">Request:</span></span>

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net

<span data-ttu-id="c7dfb-180">Válasz:</span><span class="sxs-lookup"><span data-stu-id="c7dfb-180">Response:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-hello-content-key"></a><span data-ttu-id="c7dfb-181">Hello tartalomkulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7dfb-181">Create hello content key</span></span>
<span data-ttu-id="c7dfb-182">Hello X.509 tanúsítvány lekérése és használja a nyilvános kulcs tooencrypt a tartalomkulcsot, akkor létre kell hoznia egy **ContentKey** entitás és a tulajdonság értékek ennek megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-182">After you have retrieved hello X.509 certificate and used its public key tooencrypt your content key, create a **ContentKey** entity and set its property values accordingly.</span></span>

<span data-ttu-id="c7dfb-183">Hello tartalom hello típus kulcs létrehozása, hogy kell-e állítva mikor hello értékek egyike.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-183">One of hello values that you must set when create hello content key is hello type.</span></span> <span data-ttu-id="c7dfb-184">Hello tárolás titkosítását, ha a hello értéke "1".</span><span class="sxs-lookup"><span data-stu-id="c7dfb-184">In case of hello storage encryption, hello value is '1'.</span></span> 

<span data-ttu-id="c7dfb-185">a következő példa azt mutatja meg hogyan hello toocreate egy **ContentKey** rendelkező egy **ContentKeyType** beállítása a tárolás titkosítása ("1") és hello **ProtectionKeyType** beállítása túl "0" amely védelmi kulcs azonosítója hello tooindicate hello X.509 tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-185">hello following example shows how toocreate a **ContentKey** with a **ContentKeyType** set for storage encryption ("1") and hello **ProtectionKeyType** set too"0" tooindicate that hello protection key Id is hello X.509 certificate thumbprint.</span></span>  

<span data-ttu-id="c7dfb-186">Kérés</span><span class="sxs-lookup"><span data-stu-id="c7dfb-186">Request</span></span>

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }

<span data-ttu-id="c7dfb-187">Válasz:</span><span class="sxs-lookup"><span data-stu-id="c7dfb-187">Response:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a><span data-ttu-id="c7dfb-188">Egy eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7dfb-188">Create an asset</span></span>
<span data-ttu-id="c7dfb-189">a következő példa azt mutatja meg hogyan hello toocreate eszközként.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-189">hello following example shows how toocreate an asset.</span></span>

<span data-ttu-id="c7dfb-190">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="c7dfb-190">**HTTP Request**</span></span>

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"Name":"BigBuckBunny" "Options":1}

<span data-ttu-id="c7dfb-191">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="c7dfb-191">**HTTP Response**</span></span>

<span data-ttu-id="c7dfb-192">Ha sikeres, hello következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="c7dfb-192">If successful, hello following is returned:</span></span>

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
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

## <a name="associate-hello-contentkey-with-an-asset"></a><span data-ttu-id="c7dfb-193">Egy eszköz hello ContentKey társítása</span><span class="sxs-lookup"><span data-stu-id="c7dfb-193">Associate hello ContentKey with an Asset</span></span>
<span data-ttu-id="c7dfb-194">Miután létrehozta a hello ContentKey, társíthatja hello $links művelettel, az eszköz a hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="c7dfb-194">After creating hello ContentKey, associate it with your Asset using hello $links operation, as shown in hello following example:</span></span>

<span data-ttu-id="c7dfb-195">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="c7dfb-195">Request:</span></span>

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

<span data-ttu-id="c7dfb-196">Válasz:</span><span class="sxs-lookup"><span data-stu-id="c7dfb-196">Response:</span></span>

    HTTP/1.1 204 No Content 

## <a name="create-an-assetfile"></a><span data-ttu-id="c7dfb-197">Hozzon létre egy AssetFile</span><span class="sxs-lookup"><span data-stu-id="c7dfb-197">Create an AssetFile</span></span>
<span data-ttu-id="c7dfb-198">Hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entitás egy blob-tárolóban tárolt video- vagy fájlt jelöli.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-198">hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="c7dfb-199">Egy eszköz fájl mindig társítva van egy eszköz, és egy eszköz egy vagy több eszköz fájlt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-199">An asset file is always associated with an asset, and an asset may contain one or many asset files.</span></span> <span data-ttu-id="c7dfb-200">hello Media Services kódoló feladat sikertelen lesz, ha egy eszköz fájl objektumhoz nincs társítva egy digitális fájlhoz egy blob-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-200">hello Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="c7dfb-201">Vegye figyelembe, hogy hello **AssetFile** példány és a hello tényleges media fájl is két különböző objektum.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-201">Note that hello **AssetFile** instance and hello actual media file are two distinct objects.</span></span> <span data-ttu-id="c7dfb-202">hello AssetFile példány hello media fájlról metaadatot tartalmaz, amíg hello médiafájl tartalmaz hello tényleges médiatartalmakat.</span><span class="sxs-lookup"><span data-stu-id="c7dfb-202">hello AssetFile instance contains metadata about hello media file, while hello media file contains hello actual media content.</span></span>

<span data-ttu-id="c7dfb-203">Miután a digitális adathordozójának fájl feltöltése a blob-tárolóba, szüksége lesz a hello **EGYESÍTÉSE** HTTP kérelem tooupdate hello AssetFile a adatainak media (ebben a témakörben nem látható).</span><span class="sxs-lookup"><span data-stu-id="c7dfb-203">After you upload your digital media file into a blob container, you will use hello **MERGE** HTTP request tooupdate hello AssetFile with information about your media file (not shown in this topic).</span></span> 

<span data-ttu-id="c7dfb-204">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="c7dfb-204">**HTTP Request**</span></span>

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
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }

<span data-ttu-id="c7dfb-205">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="c7dfb-205">**HTTP Response**</span></span>

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
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
