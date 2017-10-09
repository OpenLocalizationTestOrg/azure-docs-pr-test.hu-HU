---
title: a .NET ContentKeys aaaCreate
description: "Ismerje meg, hogyan férhetnek hozzá a tartalom kulcsok toocreate biztosító biztonságos tooAssets."
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
ms.openlocfilehash: 35909c64e8393e228be75c464a034ffc40122952
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-contentkeys-with-net"></a><span data-ttu-id="b6df3-103">A .NET ContentKeys létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6df3-103">Create ContentKeys with .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b6df3-104">REST</span><span class="sxs-lookup"><span data-stu-id="b6df3-104">REST</span></span>](media-services-rest-create-contentkey.md)
> * [<span data-ttu-id="b6df3-105">.NET</span><span class="sxs-lookup"><span data-stu-id="b6df3-105">.NET</span></span>](media-services-dotnet-create-contentkey.md)
> 
> 

<span data-ttu-id="b6df3-106">A Media Services lehetővé teszi a toocreate és titkosított eszközök.</span><span class="sxs-lookup"><span data-stu-id="b6df3-106">Media Services enables you toocreate and deliver encrypted assets.</span></span> <span data-ttu-id="b6df3-107">A **ContentKey** biztosít a biztonságos hozzáférést tooyour **eszköz**s.</span><span class="sxs-lookup"><span data-stu-id="b6df3-107">A **ContentKey** provides secure access tooyour **Asset**s.</span></span> 

<span data-ttu-id="b6df3-108">Amikor létrehoz egy új eszközt (például előtt [fájlok feltöltése](media-services-dotnet-upload-files.md)), megadhatja a titkosítási beállítások a következő hello: **StorageEncrypted**, **CommonEncryptionProtected**, vagy **EnvelopeEncryptionProtected**.</span><span class="sxs-lookup"><span data-stu-id="b6df3-108">When you create a new asset (for example, before you [upload files](media-services-dotnet-upload-files.md)), you can specify hello following encryption options: **StorageEncrypted**, **CommonEncryptionProtected**, or **EnvelopeEncryptionProtected**.</span></span> 

<span data-ttu-id="b6df3-109">Ha eszközök tooyour ügyfelek kézbesíti, akkor [dinamikusan titkosított eszközök toobe konfigurálása](media-services-dotnet-configure-asset-delivery-policy.md) hello két titkosítások használatára a következő egyike: **DynamicEnvelopeEncryption** vagy  **DynamicCommonEncryption**.</span><span class="sxs-lookup"><span data-stu-id="b6df3-109">When you deliver assets tooyour clients, you can [configure for assets toobe dynamically encrypted](media-services-dotnet-configure-asset-delivery-policy.md) with one of hello following two encryptions: **DynamicEnvelopeEncryption** or **DynamicCommonEncryption**.</span></span>

<span data-ttu-id="b6df3-110">Titkosított eszközök rendelkeznek társított toobe **ContentKey**s.</span><span class="sxs-lookup"><span data-stu-id="b6df3-110">Encrypted assets have toobe associated with **ContentKey**s.</span></span> <span data-ttu-id="b6df3-111">Ez a cikk ismerteti, hogyan toocreate egy tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="b6df3-111">This article describes how toocreate a content key.</span></span>

> [!NOTE]
> <span data-ttu-id="b6df3-112">Amikor hoz létre egy új **StorageEncrypted** eszköz használatával hello Media Services .NET SDK hello **ContentKey** az automatikusan létrehozott és hello eszköz kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="b6df3-112">When creating a new **StorageEncrypted** asset using hello Media Services .NET SDK , hello **ContentKey** is automatically created and linked with hello asset.</span></span>
> 
> 

## <a name="contentkeytype"></a><span data-ttu-id="b6df3-113">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="b6df3-113">ContentKeyType</span></span>
<span data-ttu-id="b6df3-114">A tartalom létrehozása, hogy kell-e állítva mikor hello értékek egyike kulcsa hello tartalom kulcs típusa.</span><span class="sxs-lookup"><span data-stu-id="b6df3-114">One of hello values that you must set when create a content key is hello content key type.</span></span> <span data-ttu-id="b6df3-115">Hello a következő értékek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="b6df3-115">Choose from one of hello following values.</span></span> 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is hello default value.</remarks>
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

## <span data-ttu-id="b6df3-116"><a id="envelope_contentkey"></a>Boríték típus ContentKey létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6df3-116"><a id="envelope_contentkey"></a>Create envelope type ContentKey</span></span>
<span data-ttu-id="b6df3-117">hello következő kódrészletet hoz létre egy tartalomkulcsot hello boríték titkosítási típus.</span><span class="sxs-lookup"><span data-stu-id="b6df3-117">hello following code snippet creates a content key of hello envelope encryption type.</span></span> <span data-ttu-id="b6df3-118">Az hello megadott eszköz hello kulcs majd társítja.</span><span class="sxs-lookup"><span data-stu-id="b6df3-118">It then associates hello key with hello specified asset.</span></span>

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

<span data-ttu-id="b6df3-119">Hívás</span><span class="sxs-lookup"><span data-stu-id="b6df3-119">call</span></span>

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



## <span data-ttu-id="b6df3-120"><a id="common_contentkey"></a>Általános típus ContentKey létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6df3-120"><a id="common_contentkey"></a>Create common type ContentKey</span></span>
<span data-ttu-id="b6df3-121">hello következő kódrészletet hoz létre egy tartalomkulcsot hello közös titkosítási típus.</span><span class="sxs-lookup"><span data-stu-id="b6df3-121">hello following code snippet creates a content key of hello common encryption type.</span></span> <span data-ttu-id="b6df3-122">Az hello megadott eszköz hello kulcs majd társítja.</span><span class="sxs-lookup"><span data-stu-id="b6df3-122">It then associates hello key with hello specified asset.</span></span>

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

        // Associate hello key with hello asset.
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
<span data-ttu-id="b6df3-123">Hívás</span><span class="sxs-lookup"><span data-stu-id="b6df3-123">call</span></span>

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


## <a name="media-services-learning-paths"></a><span data-ttu-id="b6df3-124">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="b6df3-124">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b6df3-125">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="b6df3-125">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

