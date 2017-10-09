---
title: "egy Azure Media Services-fiók az Azure-portálon hello aaaCreate |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello hello Azure-portálon az Azure Media Services-fiók létrehozása."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: c551e158-aad6-47b4-931e-b46260b3ee4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: juliako
ms.openlocfilehash: fdad93d5d470fc08380670ec0f6c2d33468b1492
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-media-services-account-using-hello-azure-portal"></a><span data-ttu-id="4a82d-103">Hozzon létre egy Azure Media Services-fiók hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="4a82d-103">Create an Azure Media Services account using hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4a82d-104">Portál</span><span class="sxs-lookup"><span data-stu-id="4a82d-104">Portal</span></span>](media-services-portal-create-account.md)
> * [<span data-ttu-id="4a82d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a82d-105">PowerShell</span></span>](media-services-manage-with-powershell.md)
> * [<span data-ttu-id="4a82d-106">REST</span><span class="sxs-lookup"><span data-stu-id="4a82d-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> <span data-ttu-id="4a82d-107">toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="4a82d-107">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="4a82d-108">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4a82d-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="4a82d-109">hello Azure portál segítségével tooquickly Azure Media Services (AMS)-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4a82d-109">hello Azure portal provides a way tooquickly create an Azure Media Services (AMS) account.</span></span> <span data-ttu-id="4a82d-110">A fiók tooaccess Media Services-meg toostore engedélyezése, titkosítására, kódolására, kezelése és az Azure media adatfolyamként is használhatja.</span><span class="sxs-lookup"><span data-stu-id="4a82d-110">You can use your account tooaccess Media Services that enable you toostore, encrypt, encode, manage, and stream media content in Azure.</span></span> <span data-ttu-id="4a82d-111">Hello a Media Services-fiók létrehozása során, akkor is egy kapcsolódó tárfiók létrehozása (vagy használjon egy meglévőt) a hello, hello Media Services-fiókkal azonos földrajzi régióban.</span><span class="sxs-lookup"><span data-stu-id="4a82d-111">At hello time you create a Media Services account, you also create an associated storage account (or use an existing one) in hello same geographic region as hello Media Services account.</span></span>

<span data-ttu-id="4a82d-112">Ez a cikk ismerteti néhány gyakori fogalmak, és bemutatja, hogyan toocreate egy Media Services fiók hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="4a82d-112">This article explains some common concepts and shows how toocreate a Media Services account with hello Azure portal.</span></span>

## <a name="concepts"></a><span data-ttu-id="4a82d-113">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="4a82d-113">Concepts</span></span>
<span data-ttu-id="4a82d-114">A Media Services szolgáltatásainak eléréséhez két kapcsolódó fiók szükséges:</span><span class="sxs-lookup"><span data-stu-id="4a82d-114">Accessing Media Services requires two associated accounts:</span></span>

* <span data-ttu-id="4a82d-115">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="4a82d-115">A Media Services account.</span></span> <span data-ttu-id="4a82d-116">A biztosít hozzáférést a Media Services felhőalapú tooa készlete, amelyek az Azure-ban elérhető.</span><span class="sxs-lookup"><span data-stu-id="4a82d-116">Your account gives you access tooa set of cloud-based Media Services that are available in Azure.</span></span> <span data-ttu-id="4a82d-117">A Media Services-fiók nem tárol tényleges médiatartalmakat.</span><span class="sxs-lookup"><span data-stu-id="4a82d-117">A Media Services account does not store actual media content.</span></span> <span data-ttu-id="4a82d-118">Ehelyett metaadatokat tárol hello médiatartalmak és médiafeldolgozási feladatokról a fiókban.</span><span class="sxs-lookup"><span data-stu-id="4a82d-118">Instead it stores metadata about hello media content and media processing jobs in your account.</span></span> <span data-ttu-id="4a82d-119">Időben hello hello fiókot létrehoznia válassza ki az elérhető Media Services-régiót.</span><span class="sxs-lookup"><span data-stu-id="4a82d-119">At hello time you create hello account, you select an available Media Services region.</span></span> <span data-ttu-id="4a82d-120">hello választott régió egy adatközpont, amely a fiókhoz tartozó metaadat-bejegyzéseket hello tárolja.</span><span class="sxs-lookup"><span data-stu-id="4a82d-120">hello region you select is a data center that stores hello metadata records for your account.</span></span>
  
* <span data-ttu-id="4a82d-121">Egy Azure-tárfiók.</span><span class="sxs-lookup"><span data-stu-id="4a82d-121">An Azure storage account.</span></span> <span data-ttu-id="4a82d-122">Storage-fiókok hello kell elhelyezni, hello Media Services-fiókkal azonos földrajzi régióban.</span><span class="sxs-lookup"><span data-stu-id="4a82d-122">Storage accounts must be located in hello same geographic region as hello Media Services account.</span></span> <span data-ttu-id="4a82d-123">Egy Media Services-fiók létrehozásakor választhat meglévő tárfiók hello ugyanabban a régióban, vagy létrehozhat egy új tárfiókot a hello ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="4a82d-123">When you create a Media Services account, you can either choose an existing storage account in hello same region, or you can create a new storage account in hello same region.</span></span> <span data-ttu-id="4a82d-124">Egy Media Services-fiók törlése esetén a kapcsolódó tárfiókban lévő hello blobok nem törlődnek.</span><span class="sxs-lookup"><span data-stu-id="4a82d-124">If you delete a Media Services account, hello blobs in your related storage account are not deleted.</span></span>

> [!NOTE]
> <span data-ttu-id="4a82d-125">A különböző régiókban lévő Azure Media Services-funkciók elérhetőségéről további információért lásd [az AMS-funkciók elérhetőségét az egyes adatközpontokban](scenarios-and-availability.md#availability).</span><span class="sxs-lookup"><span data-stu-id="4a82d-125">For information about availability of Azure Media Services features in different regions, see [availability of AMS features across datacenters](scenarios-and-availability.md#availability).</span></span>

## <a name="create-an-ams-account"></a><span data-ttu-id="4a82d-126">AMS-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a82d-126">Create an AMS account</span></span>
<span data-ttu-id="4a82d-127">hello lépéseket ezen témakör megjelenítése hogyan toocreate az AMS-fiók.</span><span class="sxs-lookup"><span data-stu-id="4a82d-127">hello steps in this section show how toocreate an AMS account.</span></span>

1. <span data-ttu-id="4a82d-128">Jelentkezzen be hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4a82d-128">Log in at hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="4a82d-129">Kattintson a **+New** > **Web + Mobile** > **Media Services** elemre.</span><span class="sxs-lookup"><span data-stu-id="4a82d-129">Click **+New** > **Web + Mobile** > **Media Services**.</span></span>
   
    ![Media Services, létrehozás](./media/media-services-create-account/media-services-new1.png)
3. <span data-ttu-id="4a82d-131">A **CREATE MEDIA SERVICES ACCOUNT** (Media Services-fiók létrehozása) részben adja meg a kívánt értékeket.</span><span class="sxs-lookup"><span data-stu-id="4a82d-131">In **CREATE MEDIA SERVICES ACCOUNT** enter required values.</span></span>
   
    ![Media Services, létrehozás](./media/media-services-create-account/media-services-new3.png)
   
   1. <span data-ttu-id="4a82d-133">A **fióknév**, adja meg az új AMS-fiók hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="4a82d-133">In **Account Name**, enter hello name of hello new AMS account.</span></span> <span data-ttu-id="4a82d-134">Egy Media Services-fiók nevének minden kisbetűket és számokat, szóközök nélkül, és 3 too24 karakter hosszúságú.</span><span class="sxs-lookup"><span data-stu-id="4a82d-134">A Media Services account name is all lowercase letters or numbers with no spaces, and is 3 too24 characters in length.</span></span>
   2. <span data-ttu-id="4a82d-135">Az előfizetést hello Azure-előfizetések rendelkezik hozzáféréssel közül választhat.</span><span class="sxs-lookup"><span data-stu-id="4a82d-135">In Subscription, select among hello different Azure subscriptions that you have access to.</span></span>
   3. <span data-ttu-id="4a82d-136">A **erőforráscsoport**, válassza ki az új és meglévő erőforráscsoportokra hello.</span><span class="sxs-lookup"><span data-stu-id="4a82d-136">In **Resource Group**, select hello new or existing resource.</span></span>  <span data-ttu-id="4a82d-137">Az erőforráscsoport közös életciklussal, engedélyekkel és házirendekkel rendelkező erőforrások gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="4a82d-137">A resource group is a collection of resources that share lifecycle, permissions, and policies.</span></span> <span data-ttu-id="4a82d-138">További információkat [itt](../azure-resource-manager/resource-group-overview.md#resource-groups) talál.</span><span class="sxs-lookup"><span data-stu-id="4a82d-138">Learn more [here](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>
   4. <span data-ttu-id="4a82d-139">A **hely**, válassza ki a földrajzi régiót, amelyben lesz használt toostore hello media és metaadat-bejegyzéseket a Media Services-fiókhoz tartozó hello.</span><span class="sxs-lookup"><span data-stu-id="4a82d-139">In **Location**,  select hello geographic region that will be used toostore hello media and metadata records for your Media Services account.</span></span> <span data-ttu-id="4a82d-140">Ebben a régióban rendszer használt tooprocess, majd streamelni a médiafájlokat.</span><span class="sxs-lookup"><span data-stu-id="4a82d-140">This  region will be used tooprocess and stream your media.</span></span> <span data-ttu-id="4a82d-141">Csak hello elérhető Media Services-régiók hello legördülő listában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="4a82d-141">Only hello available Media Services regions appear in hello drop-down list box.</span></span> 
   5. <span data-ttu-id="4a82d-142">A **Tárfiók**, válassza ki a tárolási fiók tooprovide blob-tároló hello médiatartalom a Media Services-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="4a82d-142">In **Storage Account**, select a storage account tooprovide blob storage of hello media content from your Media Services account.</span></span> <span data-ttu-id="4a82d-143">Hello kiválaszthat egy meglévő tárfiókot használ, a Media Services-fiókját, vagy azonos földrajzi régióban hozhat létre egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="4a82d-143">You can select an existing storage account in hello same geographic region as your Media Services account, or you can create a storage account.</span></span> <span data-ttu-id="4a82d-144">Egy új tárfiók létrehozása a hello azonos régióban.</span><span class="sxs-lookup"><span data-stu-id="4a82d-144">A new storage account is created in hello same region.</span></span> <span data-ttu-id="4a82d-145">hello szabályok tárfiók neve hello ugyanaz, mint a Media Services-fiókok.</span><span class="sxs-lookup"><span data-stu-id="4a82d-145">hello rules for storage account names are hello same as for Media Services accounts.</span></span>
      
       <span data-ttu-id="4a82d-146">További információkat a tárhelyről [itt](../storage/common/storage-introduction.md) talál.</span><span class="sxs-lookup"><span data-stu-id="4a82d-146">Learn more about storage [here](../storage/common/storage-introduction.md).</span></span>
   6. <span data-ttu-id="4a82d-147">Válassza ki **PIN-kód toodashboard** toosee hello hello fiók központi telepítés végrehajtási állapotát.</span><span class="sxs-lookup"><span data-stu-id="4a82d-147">Select **Pin toodashboard** toosee hello progress of hello account deployment.</span></span>
4. <span data-ttu-id="4a82d-148">Kattintson a **létrehozása** hello hello képernyő alsó részén.</span><span class="sxs-lookup"><span data-stu-id="4a82d-148">Click **Create** at hello bottom of hello form.</span></span>
   
    <span data-ttu-id="4a82d-149">Hello fiók sikeres létrehozását követően – áttekintés oldalra tölti be.</span><span class="sxs-lookup"><span data-stu-id="4a82d-149">Once hello account is successfully created, overview page loads.</span></span> <span data-ttu-id="4a82d-150">Hello streaming endpoint tábla hello fiókja rendelkeznek egy alapértelmezett streamvégpontból a hello **leállítva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="4a82d-150">In hello streaming endpoint table hello account will have a default streaming endpoint in hello **Stopped** state.</span></span> 

    >[!NOTE]
    ><span data-ttu-id="4a82d-151">Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="4a82d-151">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="4a82d-152">a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="4a82d-152">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 
   
## <a name="toomanage-your-ams-account"></a><span data-ttu-id="4a82d-153">toomanage az AMS-fiók</span><span class="sxs-lookup"><span data-stu-id="4a82d-153">toomanage your AMS account</span></span>

<span data-ttu-id="4a82d-154">toomanage az AMS-fiók (például toohello AMS API programozott módon való kapcsolódás, videók feltöltése, kódolása, a tartalom védelmének konfigurálása, feladatok előrehaladásának figyeléséhez) kiválasztása **beállítások** a bal oldalán található hello portal hello.</span><span class="sxs-lookup"><span data-stu-id="4a82d-154">toomanage your AMS account (for example, connect toohello AMS API programmatically, upload videos, encode assets, configure content protection, monitor job progress) select **Settings** on hello left side of hello portal.</span></span> <span data-ttu-id="4a82d-155">A hello **beállítások**, keresse meg a rendelkezésre álló paneleken hello tooone (például: **API-hozzáférés**, **eszközök**, **feladatok**, **Védelmi tartalom**).</span><span class="sxs-lookup"><span data-stu-id="4a82d-155">From hello **Settings**, navigate tooone of hello available blades (for example: **API access**, **Assets**, **Jobs**, **Content protection**).</span></span>


## <a name="next-steps"></a><span data-ttu-id="4a82d-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4a82d-156">Next steps</span></span>

<span data-ttu-id="4a82d-157">Most már feltölthet fájlokat AMS-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="4a82d-157">You can now upload files into your AMS account.</span></span> <span data-ttu-id="4a82d-158">További információk: [Fájlok feltöltése](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="4a82d-158">For more information, see [Upload files](media-services-portal-upload-files.md).</span></span>

<span data-ttu-id="4a82d-159">Ha programozottan tooaccess AMS API, lásd: [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="4a82d-159">If you plan tooaccess AMS API programmatically, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="4a82d-160">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="4a82d-160">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4a82d-161">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="4a82d-161">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

