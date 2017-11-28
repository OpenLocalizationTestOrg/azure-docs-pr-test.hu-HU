---
title: "A tartalom titkosított a tárolás titkosítása AMS REST API használatával"
description: "Ismerje meg, hogy a tartalom titkosítása a tárolás titkosítása AMS REST API-k használatával."
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
ms.openlocfilehash: 1979f5bf5e8cab88dab5fba49018afacf24504b3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="encrypting-your-content-with-storage-encryption"></a><span data-ttu-id="70878-103">A tárolás titkosítása tartalom titkosítása</span><span class="sxs-lookup"><span data-stu-id="70878-103">Encrypting your content with storage encryption</span></span>

<span data-ttu-id="70878-104">Helyileg az AES-256 bites titkosítás használata a tartalom titkosításához, és ezután töltse fel az Azure Storage azt tárolására szolgáló titkosítása erősen ajánlott.</span><span class="sxs-lookup"><span data-stu-id="70878-104">It is highly recommended to encrypt your content locally using AES-256 bit encryption and then upload it to Azure Storage where it will be stored encrypted at rest.</span></span>

<span data-ttu-id="70878-105">Ez a cikk áttekintést AMS tárolás titkosítása, és bemutatja, hogyan töltse fel a tárolási titkosított tartalom:</span><span class="sxs-lookup"><span data-stu-id="70878-105">This article gives an overview of AMS storage encryption and shows you how to upload the storage encrypted content:</span></span>

* <span data-ttu-id="70878-106">Hozzon létre egy tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="70878-106">Create a content key.</span></span>
* <span data-ttu-id="70878-107">Hozzon létre egy eszközt.</span><span class="sxs-lookup"><span data-stu-id="70878-107">Create an Asset.</span></span> <span data-ttu-id="70878-108">Állítsa be a AssetCreationOption StorageEncryption, amikor az eszköz hoz létre.</span><span class="sxs-lookup"><span data-stu-id="70878-108">Set the AssetCreationOption to StorageEncryption when creating the Asset.</span></span>
  
     <span data-ttu-id="70878-109">Titkosított eszközök tartalomkulcs társítani kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="70878-109">Encrypted assets have to be associated with content keys.</span></span>
* <span data-ttu-id="70878-110">A tartalomkulcs csatolása az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="70878-110">Link the content key to the asset.</span></span>  
* <span data-ttu-id="70878-111">Állítsa be a titkosítás kapcsolatos AssetFile entitásokat paramétereket.</span><span class="sxs-lookup"><span data-stu-id="70878-111">Set the encryption related parameters on the AssetFile entities.</span></span>

## <a name="considerations"></a><span data-ttu-id="70878-112">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="70878-112">Considerations</span></span> 

<span data-ttu-id="70878-113">Ha egy tárolási titkosított eszköz kézbesíteni szeretné, konfigurálnia kell az adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="70878-113">If you want to deliver a storage encrypted asset, you must configure the asset’s delivery policy.</span></span> <span data-ttu-id="70878-114">Mielőtt az eszköz továbbítható, a streamelési kiszolgáló a tárolás titkosítása eltávolítja, és az adatfolyamokat, a tartalom a megadott objektumtovábbítási szabályzat segítségével.</span><span class="sxs-lookup"><span data-stu-id="70878-114">Before your asset can be streamed, the streaming server removes the storage encryption and streams your content using the specified delivery policy.</span></span> <span data-ttu-id="70878-115">További információkért lásd: [konfigurálása az adategység továbbítási házirendjeit](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="70878-115">For more information, see [Configuring Asset Delivery Policies](media-services-rest-configure-asset-delivery-policy.md).</span></span>

<span data-ttu-id="70878-116">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="70878-116">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="70878-117">További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="70878-117">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span> 

## <a name="connect-to-media-services"></a><span data-ttu-id="70878-118">Kapcsolódás a Media Services szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="70878-118">Connect to Media Services</span></span>

<span data-ttu-id="70878-119">Az AMS API-hoz kapcsolódáshoz információkért lásd: [elérni az Azure Media Services API-t az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="70878-119">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="70878-120">Sikeresen csatlakoztassa a https://media.windows.net, adja meg egy másik Media Services URI 301 átirányítást fog kapni.</span><span class="sxs-lookup"><span data-stu-id="70878-120">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="70878-121">Meg kell nyitnia az új URI későbbi hívásokat.</span><span class="sxs-lookup"><span data-stu-id="70878-121">You must make subsequent calls to the new URI.</span></span>

## <a name="storage-encryption-overview"></a><span data-ttu-id="70878-122">Tárolás titkosítási – áttekintés</span><span class="sxs-lookup"><span data-stu-id="70878-122">Storage encryption overview</span></span>
<span data-ttu-id="70878-123">Az AMS tárolás titkosítása vonatkozik **AES-Parancsra** mód a titkosítás a teljes fájlt.</span><span class="sxs-lookup"><span data-stu-id="70878-123">The AMS storage encryption applies **AES-CTR** mode encryption to the entire file.</span></span>  <span data-ttu-id="70878-124">AES-Parancsra módja a blokktitkosításon, amelyek titkosíthatják a tetszőleges hosszúságú adatok kitöltési szükségessége nélkül.</span><span class="sxs-lookup"><span data-stu-id="70878-124">AES-CTR mode is a block cipher that can encrypt arbitrary length data without need for padding.</span></span> <span data-ttu-id="70878-125">Egy számláló blokkot, amelynek a AES algoritmust, majd a XOR-ing AES kimenete az adatok titkosítására vagy visszafejtésére titkosításával működik.</span><span class="sxs-lookup"><span data-stu-id="70878-125">It operates by encrypting a counter block with the AES algorithm and then XOR-ing the output of AES with the data to encrypt or decrypt.</span></span>  <span data-ttu-id="70878-126">A számláló blokk használt összeállított úgy, hogy a számláló értéke 0 és 7 bájtja a InitializationVector értékét, és a számláló értéke 8-15 bájtja értéke nulla.</span><span class="sxs-lookup"><span data-stu-id="70878-126">The counter block used is constructed by copying the value of the InitializationVector to bytes 0 to 7 of the counter value and bytes 8 to 15 of the counter value are set to zero.</span></span> <span data-ttu-id="70878-127">A 16 bájtos számláló blokk bájt 8-15 (azaz a legkisebb helyiértékű bájt) kell használni, mint egy egyszerű 64 bites előjel nélküli egész szám, amely minden ezt követő adatblokk eggyel növeli dolgoz fel, és hálózati maradnak.</span><span class="sxs-lookup"><span data-stu-id="70878-127">Of the 16 byte counter block, bytes 8 to 15 (i.e. the least significant bytes) are used as a simple 64 bit unsigned integer that is incremented by one for each subsequent block of data processed and is kept in network byte order.</span></span> <span data-ttu-id="70878-128">Vegye figyelembe, hogy a beállítást, ha az egész eléri a maximális értéket (0xFFFFFFFFFFFFFFFF) alaphelyzetbe állítását növekvő a blokk számláló nulla (bájt 8-15) nem befolyásolja a többi 64 bites a számláló (azaz a 0 és 7 bájt).</span><span class="sxs-lookup"><span data-stu-id="70878-128">Note that if this integer reaches the maximum value (0xFFFFFFFFFFFFFFFF) then incrementing it resets the block counter to zero (bytes 8 to 15) without affecting the other 64 bits of the counter (i.e. bytes 0 to 7).</span></span>   <span data-ttu-id="70878-129">Ahhoz, hogy az AES-Parancsra mód titkosítási biztonságának fenntartásához, egy adott kulcs azonosítót minden tartalomkulcsot InitializationVector értékét kell minden egyes fájl egyedi, és fájlok legfeljebb 2 ^ 64 blokkok hossza.</span><span class="sxs-lookup"><span data-stu-id="70878-129">In order to maintain the security of the AES-CTR mode encryption, the InitializationVector value for a given Key Identifier for each content key shall be unique for each file and files shall be less than 2^64 blocks in length.</span></span>  <span data-ttu-id="70878-130">Ez azért szükséges, hogy a számláló értéke nem fel újra egy adott kulccsal.</span><span class="sxs-lookup"><span data-stu-id="70878-130">This is to ensure that a counter value is never reused with a given key.</span></span> <span data-ttu-id="70878-131">A Parancsra móddal kapcsolatos további információkért lásd: [a wiki lapon](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (a a wikicikket "Nonce" helyett "InitializationVector" kifejezést használja).</span><span class="sxs-lookup"><span data-stu-id="70878-131">For more information about the CTR mode, see [this wiki page](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (the wiki article uses the term "Nonce" instead of "InitializationVector").</span></span>

<span data-ttu-id="70878-132">Használjon **tárolás titkosítása** a tiszta tartalom helyileg használatával az AES-256 bit a titkosítási és majd töltse fel az Azure Storage helyén titkosítása.</span><span class="sxs-lookup"><span data-stu-id="70878-132">Use **Storage Encryption** to encrypt your clear content locally using AES-256 bit encryption and then upload it to Azure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="70878-133">Storage-titkosítással védett adategységek automatikusan a titkosítás és helyezni egy titkosított fájlrendszerbe kódolás előtt, és egy új kimeneti eszközként feltöltés előtt esetleg újra titkosítja.</span><span class="sxs-lookup"><span data-stu-id="70878-133">Assets protected with storage encryption are automatically unencrypted and placed in an encrypted file system prior to encoding, and optionally re-encrypted prior to uploading back as a new output asset.</span></span> <span data-ttu-id="70878-134">A tárolás titkosítása elsődleges használati eset az, amikor biztonságossá tételéhez a kiváló minőségű bemeneti médiafájljait erős titkosítással aktívan a lemezen.</span><span class="sxs-lookup"><span data-stu-id="70878-134">The primary use case for storage encryption is when you want to secure your high quality input media files with strong encryption at rest on disk.</span></span>

<span data-ttu-id="70878-135">A tárolási titkosított eszköz kezelni, konfigurálnia kell az adategység továbbítási házirendjét, a Media Services tudja, hogyan kívánja a tartalom.</span><span class="sxs-lookup"><span data-stu-id="70878-135">In order to deliver a storage encrypted asset, you must configure the asset’s delivery policy so Media Services knows how you want to deliver your content.</span></span> <span data-ttu-id="70878-136">Mielőtt az eszköz továbbítható, a streamelési kiszolgáló eltávolítja a tárolás titkosítása, és az adatfolyamokat a tartalmat a megadott továbbítási házirendjét (például AES, általános titkosítás vagy titkosítás nélkül) használatával.</span><span class="sxs-lookup"><span data-stu-id="70878-136">Before your asset can be streamed, the streaming server removes the storage encryption and streams your content using the specified delivery policy (for example, AES, common encryption, or no encryption).</span></span>

## <a name="create-contentkeys-used-for-encryption"></a><span data-ttu-id="70878-137">A titkosításhoz használt ContentKeys létrehozása</span><span class="sxs-lookup"><span data-stu-id="70878-137">Create ContentKeys used for encryption</span></span>
<span data-ttu-id="70878-138">A titkosított eszközökre kell lennie a tárolási titkosítási kulcs.</span><span class="sxs-lookup"><span data-stu-id="70878-138">Encrypted assets have to be associated with Storage Encryption key.</span></span> <span data-ttu-id="70878-139">A tartalomkulcs használt titkosítási az adategység-fájloknak létrehozása előtt létre kell hoznia.</span><span class="sxs-lookup"><span data-stu-id="70878-139">You must create the content key to be used for encryption before creating the asset files.</span></span> <span data-ttu-id="70878-140">Ez a szakasz ismerteti, hogyan hozzon létre egy tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="70878-140">This section describes how to create a content key.</span></span>

<span data-ttu-id="70878-141">Az alábbi lépések általános generálásához tartalomkulcs, amely a titkosítani kívánt eszközök fog társítani.</span><span class="sxs-lookup"><span data-stu-id="70878-141">The following are general steps for generating content keys that you will associate with assets that you want to be encrypted.</span></span> 

1. <span data-ttu-id="70878-142">A tárolás titkosítása véletlenszerűen 32 bájtos AES kulcs létrehozása.</span><span class="sxs-lookup"><span data-stu-id="70878-142">For storage encryption, randomly generate a 32-byte AES key.</span></span> 
   
    <span data-ttu-id="70878-143">Ez lesz a tartalomkulcsot az adategységhez, ami azt jelenti, hogy az adott eszközhöz hozzárendelt összes fájl szükség lesz az azonos tartalomkulcsot a visszafejtés során.</span><span class="sxs-lookup"><span data-stu-id="70878-143">This will be the content key for your asset, which means all files associated with that asset will need to use the same content key during decryption.</span></span> 
2. <span data-ttu-id="70878-144">Hívja a [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) és [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) módszerek megszerezni a helyes X.509-tanúsítvány használatával titkosítja a tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="70878-144">Call the [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) and [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) methods to get the correct X.509 Certificate that must be used to encrypt your content key.</span></span>
3. <span data-ttu-id="70878-145">A tartalomkulcs X.509-tanúsítvány nyilvános kulcsával titkosítja.</span><span class="sxs-lookup"><span data-stu-id="70878-145">Encrypt your content key with the public key of the X.509 Certificate.</span></span> 
   
   <span data-ttu-id="70878-146">Media Services .NET SDK RSA-t használ a OAEP típusú végrehajtásakor a titkosítást.</span><span class="sxs-lookup"><span data-stu-id="70878-146">Media Services .NET SDK uses RSA with OAEP when doing the encryption.</span></span>  <span data-ttu-id="70878-147">A .NET példáját láthatja az [EncryptSymmetricKeyData függvény](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span><span class="sxs-lookup"><span data-stu-id="70878-147">You can see a .NET example in the [EncryptSymmetricKeyData function](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span></span>
4. <span data-ttu-id="70878-148">Hozzon létre egy ellenőrzőösszeget számítja ki a kulcs azonosítója és a tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="70878-148">Create a checksum value calculated using the key identifier and content key.</span></span> <span data-ttu-id="70878-149">A következő .NET típusú példát a GUID része a kulcsazonosító és egyértelműen tartalomkulcsot ellenőrzőösszeg számítja ki.</span><span class="sxs-lookup"><span data-stu-id="70878-149">The following .NET example calculates the checksum using the GUID part of the key identifier and the clear content key.</span></span>

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting the KID
            // with the content key.
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

1. <span data-ttu-id="70878-150">Hozzon létre a tartalomkulcsot a a **EncryptedContentKey** (karakterlánccá base64-kódolású), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**, és **ellenőrzőösszeg** értékeket az előző lépésben kapott.</span><span class="sxs-lookup"><span data-stu-id="70878-150">Create the Content key with the **EncryptedContentKey** (converted to base64-encoded string), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**, and **Checksum** values you have received in previous steps.</span></span>

    <span data-ttu-id="70878-151">A tárolás titkosítását a következő tulajdonságok tartozhatnak a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="70878-151">For storage encryption, the following properties should be included in the request body.</span></span>

    <span data-ttu-id="70878-152">Kérelem törzse tulajdonság</span><span class="sxs-lookup"><span data-stu-id="70878-152">Request body property</span></span>    | <span data-ttu-id="70878-153">Leírás</span><span class="sxs-lookup"><span data-stu-id="70878-153">Description</span></span>
    ---|---
    <span data-ttu-id="70878-154">Azonosító</span><span class="sxs-lookup"><span data-stu-id="70878-154">Id</span></span> | <span data-ttu-id="70878-155">A ContentKey azonosítója, amely azt ragozott formáival létrehozása a következő formátumban "nb:kid:UUID:<NEW GUID>".</span><span class="sxs-lookup"><span data-stu-id="70878-155">The ContentKey Id which we generate ourselves using the following format, “nb:kid:UUID:<NEW GUID>”.</span></span>
    <span data-ttu-id="70878-156">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="70878-156">ContentKeyType</span></span> | <span data-ttu-id="70878-157">Ez az a tartalom írja be a tartalom kulcs egész szám lehet.</span><span class="sxs-lookup"><span data-stu-id="70878-157">This is the content key type as an integer for this content key.</span></span> <span data-ttu-id="70878-158">Az érték 1 storage-titkosítás továbbítja azt.</span><span class="sxs-lookup"><span data-stu-id="70878-158">We pass the value 1 for storage encryption.</span></span>
    <span data-ttu-id="70878-159">EncryptedContentKey</span><span class="sxs-lookup"><span data-stu-id="70878-159">EncryptedContentKey</span></span> | <span data-ttu-id="70878-160">Létrehozhatunk egy új tartalom kulcs értéke pedig 256 bites (32 bájt) értéket.</span><span class="sxs-lookup"><span data-stu-id="70878-160">We create a new content key value which is a 256-bit (32 byte) value.</span></span> <span data-ttu-id="70878-161">A kulcs titkosított a tárolási titkosítási X.509 tanúsítvány, amely által egy HTTP GET kérelem végrehajtása a GetProtectionKeyId és GetProtectionKey metódusok nem beolvasni a Microsoft Azure Media Services használatával.</span><span class="sxs-lookup"><span data-stu-id="70878-161">The key is encrypted using the storage encryption X.509 certificate which we retrieve from Microsoft Azure Media Services by executing a HTTP GET request for the GetProtectionKeyId and GetProtectionKey Methods.</span></span> <span data-ttu-id="70878-162">Tegyük fel, tekintse meg a következő .NET-kódot: a **EncryptSymmetricKeyData** definiált metódus [Itt](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span><span class="sxs-lookup"><span data-stu-id="70878-162">As an example, see the following .NET code: the  **EncryptSymmetricKeyData** method defined [here](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span></span>
    <span data-ttu-id="70878-163">ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="70878-163">ProtectionKeyId</span></span> | <span data-ttu-id="70878-164">Ez az a védelmi tároló titkosítási X.509-tanúsítvány, amely a tartalom kulcs titkosításához használt kulcs azonosítója.</span><span class="sxs-lookup"><span data-stu-id="70878-164">This is the protection key id for the storage encryption X.509 certificate that was used to encrypt our content key.</span></span>
    <span data-ttu-id="70878-165">ProtectionKeyType</span><span class="sxs-lookup"><span data-stu-id="70878-165">ProtectionKeyType</span></span> | <span data-ttu-id="70878-166">Ez egy, a védelem a tartalom kulcs titkosításához használt kulcs a titkosítási típus.</span><span class="sxs-lookup"><span data-stu-id="70878-166">This is the encryption type for the protection key that was used to encrypt the content key.</span></span> <span data-ttu-id="70878-167">Ez az érték a fenti példában StorageEncryption(1).</span><span class="sxs-lookup"><span data-stu-id="70878-167">This value is StorageEncryption(1) for our example.</span></span>
    <span data-ttu-id="70878-168">Ellenőrzőösszeg</span><span class="sxs-lookup"><span data-stu-id="70878-168">Checksum</span></span> |<span data-ttu-id="70878-169">Az MD5 számított ellenőrzőösszeg a tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="70878-169">The MD5 calculated checksum for the content key.</span></span> <span data-ttu-id="70878-170">A tartalom azonosítója a tartalom kulccsal titkosítja számítja ki.</span><span class="sxs-lookup"><span data-stu-id="70878-170">It is computed by encrypting the content Id with the content key.</span></span> <span data-ttu-id="70878-171">A mintakód bemutatja, hogyan ellenőrzőösszeg számítása.</span><span class="sxs-lookup"><span data-stu-id="70878-171">The example code demonstrates how to calculate the checksum.</span></span>


### <a name="retrieve-the-protectionkeyid"></a><span data-ttu-id="70878-172">A ProtectionKeyId beolvasása</span><span class="sxs-lookup"><span data-stu-id="70878-172">Retrieve the ProtectionKeyId</span></span>
<span data-ttu-id="70878-173">A következő példa bemutatja, hogyan beolvasni a ProtectionKeyId, egy tanúsítvány-ujjlenyomat, a tanúsítványt a tartalomkulcsot titkosításakor kell használnia.</span><span class="sxs-lookup"><span data-stu-id="70878-173">The following example shows how to retrieve the ProtectionKeyId, a certificate thumbprint, for the certificate you must use when encrypting your content key.</span></span> <span data-ttu-id="70878-174">Hajtsa végre ezt a lépést, győződjön meg arról, hogy már rendelkezik a megfelelő tanúsítvány a gépen.</span><span class="sxs-lookup"><span data-stu-id="70878-174">Do this step to make sure that you already have the appropriate certificate on your machine.</span></span>

<span data-ttu-id="70878-175">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="70878-175">Request:</span></span>

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="70878-176">Válasz:</span><span class="sxs-lookup"><span data-stu-id="70878-176">Response:</span></span>

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

### <a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a><span data-ttu-id="70878-177">A ProtectionKey lekérdezni a ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="70878-177">Retrieve the ProtectionKey for the ProtectionKeyId</span></span>
<span data-ttu-id="70878-178">A következő példa bemutatja, hogyan lehet lekérni az X.509-tanúsítvány a ProtectionKeyId az előző lépésben kapott.</span><span class="sxs-lookup"><span data-stu-id="70878-178">The following example shows how to retrieve the X.509 certificate using the ProtectionKeyId you received in the previous step.</span></span>

<span data-ttu-id="70878-179">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="70878-179">Request:</span></span>

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net

<span data-ttu-id="70878-180">Válasz:</span><span class="sxs-lookup"><span data-stu-id="70878-180">Response:</span></span>

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

### <a name="create-the-content-key"></a><span data-ttu-id="70878-181">A tartalomkulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="70878-181">Create the content key</span></span>
<span data-ttu-id="70878-182">Az X.509 tanúsítvány lekérése és a tartalom kulcs titkosításához használt nyilvános kulcsát, akkor létre kell hoznia egy **ContentKey** entitás és a tulajdonság értékek ennek megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="70878-182">After you have retrieved the X.509 certificate and used its public key to encrypt your content key, create a **ContentKey** entity and set its property values accordingly.</span></span>

<span data-ttu-id="70878-183">A tartalom létrehozása a értékeket, hogy kell-e állítva mikor kulcs egy típus.</span><span class="sxs-lookup"><span data-stu-id="70878-183">One of the values that you must set when create the content key is the type.</span></span> <span data-ttu-id="70878-184">Esetén a tárolás titkosítása értéke "1".</span><span class="sxs-lookup"><span data-stu-id="70878-184">In case of the storage encryption, the value is '1'.</span></span> 

<span data-ttu-id="70878-185">A következő példa bemutatja, hogyan hozhat létre egy **ContentKey** rendelkező egy **ContentKeyType** tárolás titkosítása ("1") beállítása és a **ProtectionKeyType** azt jelzi, hogy a védelem kulcs azonosítója az X.509 tanúsítvány ujjlenyomata "0" értékre állítva.</span><span class="sxs-lookup"><span data-stu-id="70878-185">The following example shows how to create a **ContentKey** with a **ContentKeyType** set for storage encryption ("1") and the **ProtectionKeyType** set to "0" to indicate that the protection key Id is the X.509 certificate thumbprint.</span></span>  

<span data-ttu-id="70878-186">Kérés</span><span class="sxs-lookup"><span data-stu-id="70878-186">Request</span></span>

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

<span data-ttu-id="70878-187">Válasz:</span><span class="sxs-lookup"><span data-stu-id="70878-187">Response:</span></span>

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

## <a name="create-an-asset"></a><span data-ttu-id="70878-188">Egy eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="70878-188">Create an asset</span></span>
<span data-ttu-id="70878-189">A következő példa bemutatja, hogyan hozzon létre egy eszközt.</span><span class="sxs-lookup"><span data-stu-id="70878-189">The following example shows how to create an asset.</span></span>

<span data-ttu-id="70878-190">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="70878-190">**HTTP Request**</span></span>

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

<span data-ttu-id="70878-191">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="70878-191">**HTTP Response**</span></span>

<span data-ttu-id="70878-192">Ha sikeres, a következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="70878-192">If successful, the following is returned:</span></span>

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

## <a name="associate-the-contentkey-with-an-asset"></a><span data-ttu-id="70878-193">Egy eszköz a ContentKey társítása</span><span class="sxs-lookup"><span data-stu-id="70878-193">Associate the ContentKey with an Asset</span></span>
<span data-ttu-id="70878-194">Miután létrehozta a ContentKey, rendelje hozzá azt az objektumot az $links művelet használatával a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="70878-194">After creating the ContentKey, associate it with your Asset using the $links operation, as shown in the following example:</span></span>

<span data-ttu-id="70878-195">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="70878-195">Request:</span></span>

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

<span data-ttu-id="70878-196">Válasz:</span><span class="sxs-lookup"><span data-stu-id="70878-196">Response:</span></span>

    HTTP/1.1 204 No Content 

## <a name="create-an-assetfile"></a><span data-ttu-id="70878-197">Hozzon létre egy AssetFile</span><span class="sxs-lookup"><span data-stu-id="70878-197">Create an AssetFile</span></span>
<span data-ttu-id="70878-198">A [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entitás egy blob-tárolóban tárolt video- vagy fájlt jelöli.</span><span class="sxs-lookup"><span data-stu-id="70878-198">The [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="70878-199">Egy eszköz fájl mindig társítva van egy eszköz, és egy eszköz egy vagy több eszköz fájlt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="70878-199">An asset file is always associated with an asset, and an asset may contain one or many asset files.</span></span> <span data-ttu-id="70878-200">A Media Services kódoló feladat sikertelen lesz, ha egy eszköz fájl objektumhoz nincs társítva egy digitális fájlhoz egy blob-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="70878-200">The Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="70878-201">Vegye figyelembe, hogy a **AssetFile** példány és a tényleges médiafájl két különböző objektum.</span><span class="sxs-lookup"><span data-stu-id="70878-201">Note that the **AssetFile** instance and the actual media file are two distinct objects.</span></span> <span data-ttu-id="70878-202">A AssetFile példány media fájl metaadatainak tartalmaz, míg a médiafájl tartalmazza a tényleges médiatartalmakat.</span><span class="sxs-lookup"><span data-stu-id="70878-202">The AssetFile instance contains metadata about the media file, while the media file contains the actual media content.</span></span>

<span data-ttu-id="70878-203">A digitális adathordozójának fájl feltöltése a blob-tárolóba, után fogja használni a **EGYESÍTÉSE** HTTP-kérelem a AssetFile frissítheti a adatainak media (ebben a témakörben nem látható).</span><span class="sxs-lookup"><span data-stu-id="70878-203">After you upload your digital media file into a blob container, you will use the **MERGE** HTTP request to update the AssetFile with information about your media file (not shown in this topic).</span></span> 

<span data-ttu-id="70878-204">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="70878-204">**HTTP Request**</span></span>

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

<span data-ttu-id="70878-205">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="70878-205">**HTTP Response**</span></span>

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
