---
title: "aaaUpdate Media Services után tároló működés közbeni hívóbetűk |} Microsoft Docs"
description: "Ez a cikk útmutatást hogyan tooupdate Media Services után tároló működés közbeni hívóbetűk biztosítják."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a892ebb0-0ea0-4fc8-b715-60347cc5c95b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: milanga;cenkdin;juliako
ms.openlocfilehash: 26fa7a75a73397842aaebda59516a00f68ab97f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-media-services-after-rolling-storage-access-keys"></a>A Media Services frissítése után a működés közbeni tárelérési kulcsok

Amikor létrehoz egy új Azure Media Services (AMS) fiók, akkor is felkérést kap egy Azure Storage fiók, amely tooselect használt toostore a médiatartalom. Egynél több tároló fiókok tooyour Media Services-fiókot is hozzáadhat. Ez a témakör bemutatja, hogyan toorotate tárolási kulcsok. Azt is bemutatja, hogyan a tooadd tárfiókok tooa media-fiók. 

a jelen témakörben ismertetett tooperform hello műveletek kell használni [ARM API-k](https://docs.microsoft.com/rest/api/media/mediaservice) és [Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).  További információkért lásd: [hogyan toomanage Azure PowerShell és a Resource Manager erőforrások](../azure-resource-manager/powershell-azure-resource-manager.md).

## <a name="overview"></a>Áttekintés

Új tárfiók létrehozásakor az Azure létrehoz két 512 bites tárelérési kulcsot, amelyek használt tooauthenticate tooyour tárfiók eléréséhez. a tárolási kapcsolatok biztonságosabb, ajánlott tooperiodically generálja újra, és a tárelérési kulcs elforgatása tookeep. Két kulcs (elsődleges és másodlagos) vannak a megadott sorrendben tooenable meg toomaintain kapcsolatok toohello storage-fiók használatával férnek hozzá kulcs közben hello újragenerálja más hozzáférési kulcsot. Ez az eljárás "működés közbeni hívóbetűk" néven is ismert.

A Media Services attól függ, hogy a megadott tooit kulcs. Hello lokátorokat, amelyek használt toostream, vagy töltse le az eszközök pontosabban hello megadott tárelérési kulcs függ. Az AMS-fiók létrehozásakor az felveszi egy függőséget hello elsődleges tárelérési kulcs alapértelmezés szerint, de egy olyan felhasználó nevében hello kulcs AMS rendelkező frissíthető. Győződjön meg arról, hogy toolet Media Services tudja, melyik kulcs toouse a jelen témakörben ismertetett lépéseket követve.  

>[!NOTE]
> Ha több tárfiókot, akkor ez a művelet minden egyes storage-fiók. hello sorrendben, amelyben a tárolás kulcsok elforgatása nincs megoldva. Először elforgatása hello másodlagos kulcs, és ezután hello az elsődleges kulcs, vagy fordítva fordítva.
>
> Lépések végrehajtása előtt a jelen témakörben ismertetett egy éles fiók, győződjön meg arról, hogy tootest őket egy üzem előtti fiók.
>

## <a name="steps-toorotate-storage-keys"></a>Lépéseket toorotate tárolási kulcsok 
 
 1. Változás hello tárfiók elsődleges hívóbetűjét hello powershell-parancsmaggal vagy [Azure](https://portal.azure.com/) portálon.
 2. Megfelelő paraméterei tooforce media fiók toopick mentése tárfiókkulcsok szinkronizálása-AzureRmMediaServiceStorageKeys parancsmagot hívja
 
    hello a következő példa bemutatja, hogyan toosync kulcsok toostorage fiókok.
  
         Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. Várjon egy órát. Ellenőrizze, hogy hello adatfolyam-továbbítási forgatókönyv dolgozik.
 4. Módosítsa a tárfiók másodlagos kulcsot hello powershell-parancsmagot vagy az Azure portálon keresztül.
 5. Hívja meg a megfelelő paraméterei tooforce media fiók toopick fel új tárfiókkulcsok szinkronizálása-AzureRmMediaServiceStorageKeys powershell. 
 6. Várjon egy órát. Ellenőrizze, hogy hello adatfolyam-továbbítási forgatókönyv dolgozik.
 
### <a name="a-powershell-cmdlet-example"></a>Egy powershell parancsmag – példa 

hello következő példa bemutatja, hogyan tooget hello storage-fiók és szinkronizálás a hello AMS-fiók.

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-tooadd-storage-accounts-tooyour-ams-account"></a>Lépéseket tooadd tárfiókok tooyour AMS-fiók

hello következő a témakör bemutatja, hogyan a tooadd tárfiókok tooyour AMS-fiók: [több tároló fiókok tooa Media Services-fiók csatolása](meda-services-managing-multiple-storage-accounts.md).

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>Nyugták
Szeretnénk tooacknowledge hello személyek felé, hogy ez a dokumentum létrehozása része volt a következő: Cenk Dingiloglu, Milánó Gada Seva Titov.
