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
# <a name="update-media-services-after-rolling-storage-access-keys"></a><span data-ttu-id="b613c-103">A Media Services frissítése után a működés közbeni tárelérési kulcsok</span><span class="sxs-lookup"><span data-stu-id="b613c-103">Update Media Services after rolling storage access keys</span></span>

<span data-ttu-id="b613c-104">Amikor létrehoz egy új Azure Media Services (AMS) fiók, akkor is felkérést kap egy Azure Storage fiók, amely tooselect használt toostore a médiatartalom.</span><span class="sxs-lookup"><span data-stu-id="b613c-104">When you create a new Azure Media Services (AMS) account, you are also asked tooselect an Azure Storage account that is used toostore your media content.</span></span> <span data-ttu-id="b613c-105">Egynél több tároló fiókok tooyour Media Services-fiókot is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="b613c-105">You can add more than one storage accounts tooyour Media Services account.</span></span> <span data-ttu-id="b613c-106">Ez a témakör bemutatja, hogyan toorotate tárolási kulcsok.</span><span class="sxs-lookup"><span data-stu-id="b613c-106">This topic shows how toorotate storage keys.</span></span> <span data-ttu-id="b613c-107">Azt is bemutatja, hogyan a tooadd tárfiókok tooa media-fiók.</span><span class="sxs-lookup"><span data-stu-id="b613c-107">It also shows how tooadd storage accounts tooa media account.</span></span> 

<span data-ttu-id="b613c-108">a jelen témakörben ismertetett tooperform hello műveletek kell használni [ARM API-k](https://docs.microsoft.com/rest/api/media/mediaservice) és [Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).</span><span class="sxs-lookup"><span data-stu-id="b613c-108">tooperform hello actions described in this topic, you should be using [ARM APIs](https://docs.microsoft.com/rest/api/media/mediaservice) and [Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).</span></span>  <span data-ttu-id="b613c-109">További információkért lásd: [hogyan toomanage Azure PowerShell és a Resource Manager erőforrások](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b613c-109">For more information, see [How toomanage Azure resources with PowerShell and Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

## <a name="overview"></a><span data-ttu-id="b613c-110">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b613c-110">Overview</span></span>

<span data-ttu-id="b613c-111">Új tárfiók létrehozásakor az Azure létrehoz két 512 bites tárelérési kulcsot, amelyek használt tooauthenticate tooyour tárfiók eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b613c-111">When a new storage account is created, Azure generates two 512-bit storage access keys, which are used tooauthenticate access tooyour storage account.</span></span> <span data-ttu-id="b613c-112">a tárolási kapcsolatok biztonságosabb, ajánlott tooperiodically generálja újra, és a tárelérési kulcs elforgatása tookeep.</span><span class="sxs-lookup"><span data-stu-id="b613c-112">tookeep your storage connections more secure, it is recommended tooperiodically regenerate and rotate your storage access key.</span></span> <span data-ttu-id="b613c-113">Két kulcs (elsődleges és másodlagos) vannak a megadott sorrendben tooenable meg toomaintain kapcsolatok toohello storage-fiók használatával férnek hozzá kulcs közben hello újragenerálja más hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="b613c-113">Two access keys (primary and secondary) are provided in order tooenable you toomaintain connections toohello storage account using one access key while you regenerate hello other access key.</span></span> <span data-ttu-id="b613c-114">Ez az eljárás "működés közbeni hívóbetűk" néven is ismert.</span><span class="sxs-lookup"><span data-stu-id="b613c-114">This procedure is also called "rolling access keys".</span></span>

<span data-ttu-id="b613c-115">A Media Services attól függ, hogy a megadott tooit kulcs.</span><span class="sxs-lookup"><span data-stu-id="b613c-115">Media Services depends on a storage key provided tooit.</span></span> <span data-ttu-id="b613c-116">Hello lokátorokat, amelyek használt toostream, vagy töltse le az eszközök pontosabban hello megadott tárelérési kulcs függ.</span><span class="sxs-lookup"><span data-stu-id="b613c-116">Specifically, hello locators that are used toostream or download your assets depend on hello specified storage access key.</span></span> <span data-ttu-id="b613c-117">Az AMS-fiók létrehozásakor az felveszi egy függőséget hello elsődleges tárelérési kulcs alapértelmezés szerint, de egy olyan felhasználó nevében hello kulcs AMS rendelkező frissíthető.</span><span class="sxs-lookup"><span data-stu-id="b613c-117">When an AMS account is created it takes a dependency on hello primary storage access key by default but as a user you can update hello storage key that AMS has.</span></span> <span data-ttu-id="b613c-118">Győződjön meg arról, hogy toolet Media Services tudja, melyik kulcs toouse a jelen témakörben ismertetett lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="b613c-118">You must make sure toolet Media Services know which key toouse by following steps described in this topic.</span></span>  

>[!NOTE]
> <span data-ttu-id="b613c-119">Ha több tárfiókot, akkor ez a művelet minden egyes storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="b613c-119">If you have multiple storage accounts, you would perform this procedure with each storage account.</span></span> <span data-ttu-id="b613c-120">hello sorrendben, amelyben a tárolás kulcsok elforgatása nincs megoldva.</span><span class="sxs-lookup"><span data-stu-id="b613c-120">hello order in which you rotate storage keys is not fixed.</span></span> <span data-ttu-id="b613c-121">Először elforgatása hello másodlagos kulcs, és ezután hello az elsődleges kulcs, vagy fordítva fordítva.</span><span class="sxs-lookup"><span data-stu-id="b613c-121">You can rotate hello secondary key first and then hello primary key or vice versa.</span></span>
>
> <span data-ttu-id="b613c-122">Lépések végrehajtása előtt a jelen témakörben ismertetett egy éles fiók, győződjön meg arról, hogy tootest őket egy üzem előtti fiók.</span><span class="sxs-lookup"><span data-stu-id="b613c-122">Before executing steps described in this topic on a production account, make sure tootest them on a pre-production account.</span></span>
>

## <a name="steps-toorotate-storage-keys"></a><span data-ttu-id="b613c-123">Lépéseket toorotate tárolási kulcsok</span><span class="sxs-lookup"><span data-stu-id="b613c-123">Steps toorotate storage keys</span></span> 
 
 1. <span data-ttu-id="b613c-124">Változás hello tárfiók elsődleges hívóbetűjét hello powershell-parancsmaggal vagy [Azure](https://portal.azure.com/) portálon.</span><span class="sxs-lookup"><span data-stu-id="b613c-124">Change hello storage account Primary key through hello powershell cmdlet or [Azure](https://portal.azure.com/) portal.</span></span>
 2. <span data-ttu-id="b613c-125">Megfelelő paraméterei tooforce media fiók toopick mentése tárfiókkulcsok szinkronizálása-AzureRmMediaServiceStorageKeys parancsmagot hívja</span><span class="sxs-lookup"><span data-stu-id="b613c-125">Call Sync-AzureRmMediaServiceStorageKeys cmdlet with appropriate params tooforce media account toopick up storage account keys</span></span>
 
    <span data-ttu-id="b613c-126">hello a következő példa bemutatja, hogyan toosync kulcsok toostorage fiókok.</span><span class="sxs-lookup"><span data-stu-id="b613c-126">hello following example shows how toosync keys toostorage accounts.</span></span>
  
         Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. <span data-ttu-id="b613c-127">Várjon egy órát.</span><span class="sxs-lookup"><span data-stu-id="b613c-127">Wait an hour or so.</span></span> <span data-ttu-id="b613c-128">Ellenőrizze, hogy hello adatfolyam-továbbítási forgatókönyv dolgozik.</span><span class="sxs-lookup"><span data-stu-id="b613c-128">Verify hello streaming scenarios are working.</span></span>
 4. <span data-ttu-id="b613c-129">Módosítsa a tárfiók másodlagos kulcsot hello powershell-parancsmagot vagy az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="b613c-129">Change storage account secondary key through hello powershell cmdlet or Azure portal.</span></span>
 5. <span data-ttu-id="b613c-130">Hívja meg a megfelelő paraméterei tooforce media fiók toopick fel új tárfiókkulcsok szinkronizálása-AzureRmMediaServiceStorageKeys powershell.</span><span class="sxs-lookup"><span data-stu-id="b613c-130">Call Sync-AzureRmMediaServiceStorageKeys powershell with appropriate params tooforce media account toopick up new storage account keys.</span></span> 
 6. <span data-ttu-id="b613c-131">Várjon egy órát.</span><span class="sxs-lookup"><span data-stu-id="b613c-131">Wait an hour or so.</span></span> <span data-ttu-id="b613c-132">Ellenőrizze, hogy hello adatfolyam-továbbítási forgatókönyv dolgozik.</span><span class="sxs-lookup"><span data-stu-id="b613c-132">Verify hello streaming scenarios are working.</span></span>
 
### <a name="a-powershell-cmdlet-example"></a><span data-ttu-id="b613c-133">Egy powershell parancsmag – példa</span><span class="sxs-lookup"><span data-stu-id="b613c-133">A powershell cmdlet example</span></span> 

<span data-ttu-id="b613c-134">hello következő példa bemutatja, hogyan tooget hello storage-fiók és szinkronizálás a hello AMS-fiók.</span><span class="sxs-lookup"><span data-stu-id="b613c-134">hello following example demonstrates how tooget hello storage account and sync it with hello AMS account.</span></span>

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-tooadd-storage-accounts-tooyour-ams-account"></a><span data-ttu-id="b613c-135">Lépéseket tooadd tárfiókok tooyour AMS-fiók</span><span class="sxs-lookup"><span data-stu-id="b613c-135">Steps tooadd storage accounts tooyour AMS account</span></span>

<span data-ttu-id="b613c-136">hello következő a témakör bemutatja, hogyan a tooadd tárfiókok tooyour AMS-fiók: [több tároló fiókok tooa Media Services-fiók csatolása](meda-services-managing-multiple-storage-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="b613c-136">hello following topic shows how tooadd storage accounts tooyour AMS account: [Attach multiple storage accounts tooa Media Services account](meda-services-managing-multiple-storage-accounts.md).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="b613c-137">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="b613c-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b613c-138">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="b613c-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a><span data-ttu-id="b613c-139">Nyugták</span><span class="sxs-lookup"><span data-stu-id="b613c-139">Acknowledgments</span></span>
<span data-ttu-id="b613c-140">Szeretnénk tooacknowledge hello személyek felé, hogy ez a dokumentum létrehozása része volt a következő: Cenk Dingiloglu, Milánó Gada Seva Titov.</span><span class="sxs-lookup"><span data-stu-id="b613c-140">We would like tooacknowledge hello following people who contributed towards creating this document: Cenk Dingiloglu, Milan Gada, Seva Titov.</span></span>
