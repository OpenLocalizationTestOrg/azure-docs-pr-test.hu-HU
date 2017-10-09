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
# <a name="create-contentkeys-with-net"></a>A .NET ContentKeys létrehozása
> [!div class="op_single_selector"]
> * [REST](media-services-rest-create-contentkey.md)
> * [.NET](media-services-dotnet-create-contentkey.md)
> 
> 

A Media Services lehetővé teszi a toocreate és titkosított eszközök. A **ContentKey** biztosít a biztonságos hozzáférést tooyour **eszköz**s. 

Amikor létrehoz egy új eszközt (például előtt [fájlok feltöltése](media-services-dotnet-upload-files.md)), megadhatja a titkosítási beállítások a következő hello: **StorageEncrypted**, **CommonEncryptionProtected**, vagy **EnvelopeEncryptionProtected**. 

Ha eszközök tooyour ügyfelek kézbesíti, akkor [dinamikusan titkosított eszközök toobe konfigurálása](media-services-dotnet-configure-asset-delivery-policy.md) hello két titkosítások használatára a következő egyike: **DynamicEnvelopeEncryption** vagy  **DynamicCommonEncryption**.

Titkosított eszközök rendelkeznek társított toobe **ContentKey**s. Ez a cikk ismerteti, hogyan toocreate egy tartalomkulcsot.

> [!NOTE]
> Amikor hoz létre egy új **StorageEncrypted** eszköz használatával hello Media Services .NET SDK hello **ContentKey** az automatikusan létrehozott és hello eszköz kapcsolódik.
> 
> 

## <a name="contentkeytype"></a>ContentKeyType
A tartalom létrehozása, hogy kell-e állítva mikor hello értékek egyike kulcsa hello tartalom kulcs típusa. Hello a következő értékek közül választhat. 

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

## <a id="envelope_contentkey"></a>Boríték típus ContentKey létrehozása
hello következő kódrészletet hoz létre egy tartalomkulcsot hello boríték titkosítási típus. Az hello megadott eszköz hello kulcs majd társítja.

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

Hívás

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



## <a id="common_contentkey"></a>Általános típus ContentKey létrehozása
hello következő kódrészletet hoz létre egy tartalomkulcsot hello közös titkosítási típus. Az hello megadott eszköz hello kulcs majd társítja.

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
Hívás

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

