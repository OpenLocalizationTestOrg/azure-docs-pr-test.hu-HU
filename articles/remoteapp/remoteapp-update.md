---
title: "aaaUpdate az Azure RemoteApp-gyűjteménnyel |} Microsoft Docs"
description: "Megtudhatja, hogyan tooupdate Azure RemoteApp-gyűjteménye"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: e553d432-e581-48fe-b996-c432357eb64a
ms.service: remoteapp
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 849d7abfdfad4dbe6a235d2a28c71f7943eb812c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-a-collection-in-azure-remoteapp"></a><span data-ttu-id="a3efd-103">Az Azure Remoteappban egy gyűjtemény frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="a3efd-103">Update a collection in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a3efd-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="a3efd-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="a3efd-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="a3efd-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="a3efd-106">Egy időpontot, mindenképpen, ha van szükség tooupdate hello alkalmazások vagy lemezkép az Azure RemoteApp-gyűjteménnyel fog érkezni.</span><span class="sxs-lookup"><span data-stu-id="a3efd-106">There will come a time, inevitably, when you need tooupdate hello apps or image in your Azure RemoteApp collection.</span></span> <span data-ttu-id="a3efd-107">Használata az Azure RemoteApp előfizetés felhőalapú vagy hibrid gyűjtemény hello rendszerképeket egyik minden frissítések kezeli Azure RemoteApp magát, így könnyen hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="a3efd-107">If you are using one of hello images included with your Azure RemoteApp subscription, in either a cloud or hybrid collection, any and all updates are handled by Azure RemoteApp itself, so you can rest easy.</span></span>

<span data-ttu-id="a3efd-108">Azonban ha egy egyéni lemezképet (vagy teljesen új beépített, vagy a lemezképek módosításával létrehozott) használ, is feladata hello rendszerkép és az alkalmazások karbantartása.</span><span class="sxs-lookup"><span data-stu-id="a3efd-108">However, if you are using a custom image (either that you built from scratch or that you created by modifying one of our images), you are in charge of maintaining hello image and apps.</span></span> <span data-ttu-id="a3efd-109">Ha a lemezkép vagy azon belül hello alkalmazások bármelyikét tooupdate kell, kell toocreate hello lemezképet, majd cserélje le hello meglévő lemezkép új, frissített verziójának új frissített lemezképpel a gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="a3efd-109">If you need tooupdate your image or any of hello apps inside it, you need toocreate a new, updated version of hello image, and then replace hello existing image in your collection with this new updated image.</span></span>

<span data-ttu-id="a3efd-110">Igen hogyan továbblépne: a gyűjtemény frissítése?</span><span class="sxs-lookup"><span data-stu-id="a3efd-110">So, how do you go about updating your collection?</span></span> <span data-ttu-id="a3efd-111">Meglehetősen egyszerű:</span><span class="sxs-lookup"><span data-stu-id="a3efd-111">It's fairly straightforward:</span></span>

1. <span data-ttu-id="a3efd-112">A gyűjteményben használt lemezképet hello frissítése.</span><span class="sxs-lookup"><span data-stu-id="a3efd-112">Update hello image that you used in your collection.</span></span> <span data-ttu-id="a3efd-113">Bármely javítások vagy a szükséges frissítéseket alkalmazza, és mentse az új nevet.</span><span class="sxs-lookup"><span data-stu-id="a3efd-113">Apply any patches or updates needed, and then save it with a new name.</span></span>
2. <span data-ttu-id="a3efd-114">[Töltse fel](remoteapp-uploadimage.md) vagy [importálása](remoteapp-image-on-azurevm.md) adott kép tooRemoteApp.</span><span class="sxs-lookup"><span data-stu-id="a3efd-114">[Upload](remoteapp-uploadimage.md) or [import](remoteapp-image-on-azurevm.md) that image tooRemoteApp.</span></span>
3. <span data-ttu-id="a3efd-115">Most hello gyűjtemény oldalon kattintson **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="a3efd-115">Now, on hello collection page, click **Update**.</span></span>
4. <span data-ttu-id="a3efd-116">Új kép hello választhat hello **sablonlemezkép** listája.</span><span class="sxs-lookup"><span data-stu-id="a3efd-116">Choose hello new image from hello **Template Image** list.</span></span>
5. <span data-ttu-id="a3efd-117">Ide tartozik hello legbonyolultabb - toodecide hogyan kell azokat a felhasználókat, hello gyűjtemény jelenleg használ egy alkalmazást a toodeal.</span><span class="sxs-lookup"><span data-stu-id="a3efd-117">Here's hello tricky part - you need toodecide how toodeal with any users that are currently using an app in hello collection.</span></span> <span data-ttu-id="a3efd-118">Hello a következő lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="a3efd-118">You have hello following choices:</span></span>
   
   * <span data-ttu-id="a3efd-119">**Hello frissítés utáni a 60 perc haladék a felhasználóknak**.</span><span class="sxs-lookup"><span data-stu-id="a3efd-119">**Give users 60 minutes after hello update**.</span></span> <span data-ttu-id="a3efd-120">Amint hello frissítése befejeződött, az Azure RemoteApp egy üzenet tooany aktív felhasználók kapnak a munkahelyi és a napló ki, és jelentkezzen be oda toosave jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a3efd-120">As soon as hello update is finished, Azure RemoteApp will display a message tooany active users telling them toosave their work and log off and log back in.</span></span> <span data-ttu-id="a3efd-121">60 perc után nem jelentkezik ki, aktív felhasználók automatikusan ki lesz léptetve.</span><span class="sxs-lookup"><span data-stu-id="a3efd-121">After 60 minutes, any active users who have not logged off will be automatically logged off.</span></span> <span data-ttu-id="a3efd-122">Felhasználók közvetlenül jelentkezhetnek be újra.</span><span class="sxs-lookup"><span data-stu-id="a3efd-122">Users can immediately log back on.</span></span>
   * <span data-ttu-id="a3efd-123">**Felhasználók azonnali kijelentkeztetése**.</span><span class="sxs-lookup"><span data-stu-id="a3efd-123">**Sign users out immediately**.</span></span> <span data-ttu-id="a3efd-124">Amint hello frissítése befejeződött, jelentkezzen ki minden felhasználó figyelmeztetés nélkül automatikusan.</span><span class="sxs-lookup"><span data-stu-id="a3efd-124">As soon as hello update is finished, log off all users automatically without any warning.</span></span> <span data-ttu-id="a3efd-125">Ha ezt a lehetőséget választja, a felhasználók adatok elveszhetnek.</span><span class="sxs-lookup"><span data-stu-id="a3efd-125">If you choose this option, users might lose data.</span></span> <span data-ttu-id="a3efd-126">Azonban ezeket újra csatlakozhat toohello alkalmazás azonnal.</span><span class="sxs-lookup"><span data-stu-id="a3efd-126">However, they can reconnect toohello app immediately.</span></span>
6. <span data-ttu-id="a3efd-127">Kattintson a hello pipa toostart hello frissítés.</span><span class="sxs-lookup"><span data-stu-id="a3efd-127">Click hello check mark toostart hello update.</span></span>

