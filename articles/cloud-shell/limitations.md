---
title: "aaaAzure felhő rendszerhéj (előzetes verzió) korlátozások |} Microsoft Docs"
description: "Azure Cloud rendszerhéj vonatkozó korlátozások áttekintése"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 8462b0b9850fcde790a386433009439bbab52c0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-of-azure-cloud-shell"></a><span data-ttu-id="83155-103">Azure-felhőbe rendszerhéj korlátozásai</span><span class="sxs-lookup"><span data-stu-id="83155-103">Limitations of Azure Cloud Shell</span></span>
<span data-ttu-id="83155-104">Azure Cloud rendszerhéj rendelkezik hello következő ismert korlátozásai:</span><span class="sxs-lookup"><span data-stu-id="83155-104">Azure Cloud Shell has hello following known limitations:</span></span>

## <a name="system-state-and-persistence"></a><span data-ttu-id="83155-105">Rendszerállapot- és adatmegőrzési</span><span class="sxs-lookup"><span data-stu-id="83155-105">System state and persistence</span></span>
<span data-ttu-id="83155-106">a felhő rendszerhéj munkamenet biztosító hello gép csak átmenetileg létezik, és újrahasznosítása után a munkamenet a 20 percig inaktív.</span><span class="sxs-lookup"><span data-stu-id="83155-106">hello machine that provides your Cloud Shell session is temporary, and it is recycled after your session is inactive for 20 minutes.</span></span> <span data-ttu-id="83155-107">Felhő rendszerhéj egy fájl megosztási toobe csatlakoztatva van szükség.</span><span class="sxs-lookup"><span data-stu-id="83155-107">Cloud Shell requires a file share toobe mounted.</span></span> <span data-ttu-id="83155-108">Ennek eredményeképpen az előfizetés mentése a tárolási erőforrások tooaccess felhő rendszerhéj képes tooset kell lennie.</span><span class="sxs-lookup"><span data-stu-id="83155-108">As a result, your subscription must be able tooset up storage resources tooaccess Cloud Shell.</span></span> <span data-ttu-id="83155-109">Egyéb szempontok a következők:</span><span class="sxs-lookup"><span data-stu-id="83155-109">Other considerations include:</span></span>
* <span data-ttu-id="83155-110">Csatlakoztatott tárolóval, csak azokat a módosításokat, belül a `$Home` könyvtár vagy `clouddrive` directory megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="83155-110">With mounted storage, only modifications within your `$Home` directory or `clouddrive` directory are persisted.</span></span>
* <span data-ttu-id="83155-111">Fájlmegosztások csak belülről lehet csatlakoztatni az [hozzárendelve régió](persisting-shell-storage.md#mount-a-new-clouddrive).</span><span class="sxs-lookup"><span data-stu-id="83155-111">File shares can be mounted only from within your [assigned region](persisting-shell-storage.md#mount-a-new-clouddrive).</span></span>
* <span data-ttu-id="83155-112">Az Azure Files csak a helyileg redundáns tárolás és a georedundáns tárolás fiókokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="83155-112">Azure Files supports only locally redundant storage and geo-redundant storage accounts.</span></span>

## <a name="user-permissions"></a><span data-ttu-id="83155-113">Felhasználói engedélyek</span><span class="sxs-lookup"><span data-stu-id="83155-113">User permissions</span></span>
<span data-ttu-id="83155-114">Engedélyek vannak beállítva, normál felhasználóként sudo hozzáférés nélkül.</span><span class="sxs-lookup"><span data-stu-id="83155-114">Permissions are set as regular users without sudo access.</span></span> <span data-ttu-id="83155-115">Bármely telepítési kívül a `$Home` könyvtár nem fogja megőrizni.</span><span class="sxs-lookup"><span data-stu-id="83155-115">Any installation outside your `$Home` directory will not persist.</span></span>
<span data-ttu-id="83155-116">Bár az egyes parancsok belül hello `clouddrive` könyvtárába, például `git clone`, nem rendelkezik megfelelő engedélyekkel a `$Home` directory engedélye.</span><span class="sxs-lookup"><span data-stu-id="83155-116">Although certain commands within hello `clouddrive` directory, such as `git clone`, do not have proper permissions, your `$Home` directory does have permissions.</span></span>

## <a name="browser-support"></a><span data-ttu-id="83155-117">Webböngésző támogatása</span><span class="sxs-lookup"><span data-stu-id="83155-117">Browser support</span></span>
<span data-ttu-id="83155-118">Felhő rendszerhéj támogatja a Microsoft Edge, a Microsoft Internet Explorer, a Google Chrome, a Mozilla Firefox és az Apple Safari legújabb verziói hello.</span><span class="sxs-lookup"><span data-stu-id="83155-118">Cloud Shell supports hello latest versions of Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox, and Apple Safari.</span></span> <span data-ttu-id="83155-119">Safari privát üzemmódban nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="83155-119">Safari in private mode is not supported.</span></span>

## <a name="copy-and-paste"></a><span data-ttu-id="83155-120">Másolás és beillesztés</span><span class="sxs-lookup"><span data-stu-id="83155-120">Copy and paste</span></span>
<span data-ttu-id="83155-121">CTRL + C és a Ctrl + V, a másolás/beillesztés parancsikonok felhő rendszerhéj Windows gépeken, használja a Ctrl + Insert és a Shift + Insert toocopy és beillesztés, illetve nem működnek.</span><span class="sxs-lookup"><span data-stu-id="83155-121">Ctrl+C and Ctrl+V do not function as copy/paste shortcuts in Cloud Shell on Windows machines, use Ctrl+Insert and Shift+Insert toocopy and paste respectively.</span></span>

<span data-ttu-id="83155-122">Kattintson a jobb gombbal a másolás és beillesztés beállítások, de kattintson a jobb gombbal függvény tulajdonos toobrowser-specifikus vágólap hozzáférésének.</span><span class="sxs-lookup"><span data-stu-id="83155-122">Right-click copy-and-paste options are also available, but right-click function is subject toobrowser-specific clipboard access.</span></span>

## <a name="editing-bashrc"></a><span data-ttu-id="83155-123">.Bashrc szerkesztése</span><span class="sxs-lookup"><span data-stu-id="83155-123">Editing .bashrc</span></span>
<span data-ttu-id="83155-124">Szerkesztés .bashrc, így váratlan hibákat okozhat felhő rendszerhéj elvégzendő járjon el.</span><span class="sxs-lookup"><span data-stu-id="83155-124">Take caution when editing .bashrc, doing so can cause unexpected errors in Cloud Shell.</span></span>

## <a name="bashhistory"></a><span data-ttu-id="83155-125">.bash_history</span><span class="sxs-lookup"><span data-stu-id="83155-125">.bash_history</span></span>
<span data-ttu-id="83155-126">Lehet, hogy az előzmények bash parancsok felhő rendszerhéj munkamenet megszakítása vagy egyidejű munkamenetek miatt inkonzisztens.</span><span class="sxs-lookup"><span data-stu-id="83155-126">Your history of bash commands may be inconsistent because of Cloud Shell session disruption or concurrent sessions.</span></span>

## <a name="usage-limits"></a><span data-ttu-id="83155-127">Használati korlátok</span><span class="sxs-lookup"><span data-stu-id="83155-127">Usage limits</span></span>
<span data-ttu-id="83155-128">Felhő rendszerhéj készült interaktív használati eseteket.</span><span class="sxs-lookup"><span data-stu-id="83155-128">Cloud Shell is intended for interactive use cases.</span></span> <span data-ttu-id="83155-129">Ennek eredményeképpen a hosszan futó nem interaktív munkamenet befejeződik figyelmeztetés nélkül.</span><span class="sxs-lookup"><span data-stu-id="83155-129">As a result, any long-running non-interactive sessions are ended without warning.</span></span>

## <a name="network-connectivity"></a><span data-ttu-id="83155-130">Hálózati kapcsolat</span><span class="sxs-lookup"><span data-stu-id="83155-130">Network connectivity</span></span>
<span data-ttu-id="83155-131">Felhő rendszerhéj bármely késés tulajdonos toolocal internetkapcsolattal, a felhő rendszerhéj bármely küldött utasítások végrehajtásával kimenő tooattempt toocarry továbbra is.</span><span class="sxs-lookup"><span data-stu-id="83155-131">Any latency in Cloud Shell is subject toolocal internet connectivity, Cloud Shell continues tooattempt toocarry out any instructions sent.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83155-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="83155-132">Next steps</span></span>
[<span data-ttu-id="83155-133">Felhő rendszerhéj gyors üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="83155-133">Cloud Shell Quickstart</span></span>](quickstart.md)
