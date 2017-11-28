---
title: "A .NET ContentKeys létrehozása"
description: "Ismerje meg, amelyek biztonságos hozzáférést biztosítanak az eszközök tartalomkulcs létrehozása."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 225b05e5-7d30-409c-b5b7-3ef0634310c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 3280a6fcde59bae360da7cb9fea4bb649f984e43
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-contentkeys-with-net"></a><span data-ttu-id="3f6b1-103">A .NET ContentKeys létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f6b1-103">Create ContentKeys with .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3f6b1-104">REST</span><span class="sxs-lookup"><span data-stu-id="3f6b1-104">REST</span></span>](media-services-rest-create-contentkey.md)
> * [<span data-ttu-id="3f6b1-105">.NET</span><span class="sxs-lookup"><span data-stu-id="3f6b1-105">.NET</span></span>](media-services-dotnet-create-contentkey.md)
> 
> 

<span data-ttu-id="3f6b1-106">A Media Services titkosított eszközök teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="3f6b1-106">Media Services enables you to create and deliver encrypted assets.</span></span> <span data-ttu-id="3f6b1-107">A **ContentKey** biztonságos hozzáférést biztosít a **eszköz**s.</span><span class="sxs-lookup"><span data-stu-id="3f6b1-107">A **ContentKey** provides secure access to your **Asset**s.</span></span> 

<span data-ttu-id="3f6b1-108">Amikor létrehoz egy új eszközt (például előtt [fájlok feltöltése](media-services-dotnet-upload-files.md)), a következő titkosítási beállításokat adhat meg: **StorageEncrypted**, **CommonEncryptionProtected**, vagy **EnvelopeEncryptionProtected**.</span><span class="sxs-lookup"><span data-stu-id="3f6b1-108">When you create a new asset (for example, before you [upload files](media-services-dotnet-upload-files.md)), you can specify the following encryption options: **StorageEncrypted**, **CommonEncryptionProtected**, or **EnvelopeEncryptionProtected**.</span></span> 

<span data-ttu-id="3f6b1-109">Eszközök kézbesítése az ügyfelek számára, amikor is [dinamikusan legyen titkosítva eszközök konfigurálása](media-services-dotnet-configure-asset-delivery-policy.md) valamelyik, a következő két titkosítások használatára: **DynamicEnvelopeEncryption** vagy **DynamicCommonEncryption**.</span><span class="sxs-lookup"><span data-stu-id="3f6b1-109">When you deliver assets to your clients, you can [configure for assets to be dynamically encrypted](media-services-dotnet-configure-asset-delivery-policy.md) with one of the following two encryptions: **DynamicEnvelopeEncryption** or **DynamicCommonEncryption**.</span></span>

<span data-ttu-id="3f6b1-110">Titkosított eszközöknek kell társítani **ContentKey**s.</span><span class="sxs-lookup"><span data-stu-id="3f6b1-110">Encrypted assets have to be associated with **ContentKey**s.</span></span> <span data-ttu-id="3f6b1-111">A cikkből megtudhatja, hogyan hozzon létre egy tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="3f6b1-111">This article describes how to create a content key.</span></span>

> [!NOTE]
> <span data-ttu-id="3f6b1-112">Amikor hoz létre egy új **StorageEncrypted** eszközök a Media Services .NET SDK használatával a **ContentKey** az automatikusan létrehozott és az eszköz kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="3f6b1-112">When creating a new **StorageEncrypted** asset using the Media Services .NET SDK , the **ContentKey** is automatically created and linked with the asset.</span></span>
> 
> 

## <a name="contentkeytype"></a><span data-ttu-id="3f6b1-113">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="3f6b1-113">ContentKeyType</span></span>
<span data-ttu-id="3f6b1-114">A tartalom létrehozása a értékeket, hogy kell-e állítva mikor kulcsa a tartalom írja be.</span><span class="sxs-lookup"><span data-stu-id="3f6b1-114">One of the values that you must set when create a content key is the content key type.</span></span> <span data-ttu-id="3f6b1-115">A következő értékek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="3f6b1-115">Choose from one of the following values.</span></span> 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }

## <span data-ttu-id="3f6b1-116"><a id="envelope_contentkey"></a>Boríték típus ContentKey létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f6b1-116"><a id="envelope_contentkey"></a>Create envelope type ContentKey</span></span>
<span data-ttu-id="3f6b1-117">A következő kódrészletet hoz létre egy tartalomkulcsot a boríték titkosítási típus.</span><span class="sxs-lookup"><span data-stu-id="3f6b1-117">The following code snippet creates a content key of the envelope encryption type.</span></span> <span data-ttu-id="3f6b1-118">Ezután a kulcs társít a megadott eszköz.</span><span class="sxs-lookup"><span data-stu-id="3f6b1-118">It then associates the key with the specified asset.</span></span>

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

        asset.ContentKeys.Add(key);

        return key;
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

<span data-ttu-id="3f6b1-119">Hívás</span><span class="sxs-lookup"><span data-stu-id="3f6b1-119">call</span></span>

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



## <span data-ttu-id="3f6b1-120"><a id="common_contentkey"></a>Általános típus ContentKey létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f6b1-120"><a id="common_contentkey"></a>Create common type ContentKey</span></span>
<span data-ttu-id="3f6b1-121">A következő kódrészletet hoz létre egy tartalomkulcsot a közös titkosítási típus.</span><span class="sxs-lookup"><span data-stu-id="3f6b1-121">The following code snippet creates a content key of the common encryption type.</span></span> <span data-ttu-id="3f6b1-122">Ezután a kulcs társít a megadott eszköz.</span><span class="sxs-lookup"><span data-stu-id="3f6b1-122">It then associates the key with the specified asset.</span></span>

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
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
<span data-ttu-id="3f6b1-123">Hívás</span><span class="sxs-lookup"><span data-stu-id="3f6b1-123">call</span></span>

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


## <a name="media-services-learning-paths"></a><span data-ttu-id="3f6b1-124">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="3f6b1-124">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3f6b1-125">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="3f6b1-125">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

