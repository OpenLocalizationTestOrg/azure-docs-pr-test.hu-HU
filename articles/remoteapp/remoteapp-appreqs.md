---
title: "Azure RemoteApp alkalmazás követelményei |} Microsoft Docs"
description: "Tudnivalók az Azure Remoteappban használni kívánt alkalmazások követelményeit:"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 4427eef6-288a-49e1-97eb-fee67d99f26a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 13d42df97ea2b090180f5865a4eac25945f9f34c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="app-requirements"></a><span data-ttu-id="36726-103">Alkalmazáskövetelmények</span><span class="sxs-lookup"><span data-stu-id="36726-103">App requirements</span></span>
> [!IMPORTANT]
> <span data-ttu-id="36726-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="36726-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="36726-105">A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="36726-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="36726-106">Az Azure RemoteApp támogatja az adatfolyam-továbbítási 32 bites vagy 64 bites Windows-alapú alkalmazások a Windows Server 2012 R2-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="36726-106">Azure RemoteApp supports streaming 32-bit or 64-bit Windows-based applications from a Windows Server 2012 R2 image.</span></span> <span data-ttu-id="36726-107">A legtöbb meglévő 32 bites vagy 64 bites Windows-alapú alkalmazások futnak, "adott állapotban" az Azure RemoteApp (távoli asztali szolgáltatások vagy a korábban a Terminálszolgáltatások néven ismert) környezetben.</span><span class="sxs-lookup"><span data-stu-id="36726-107">Most existing 32-bit or 64-bit Windows-based applications run "as is" in Azure RemoteApp (Remote Desktop Services or formerly known as Terminal Services) environment.</span></span> <span data-ttu-id="36726-108">Azonban fut, és működését között eltérés van – egyes alkalmazások megfelelően működni, és hajtsa végre, míg mások azonban nem.</span><span class="sxs-lookup"><span data-stu-id="36726-108">However, there is a difference between running and running well - some applications function correctly and perform well, while others do not.</span></span> <span data-ttu-id="36726-109">A következő információkat nyújt útmutatást a távoli asztali szolgáltatások környezetben futó alkalmazások fejlesztéséhez és teszteléséhez kompatibilitás-ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="36726-109">The following information provides guidance for developing applications in a Remote Desktop Services environment and testing to ensure compatibility.</span></span>

<span data-ttu-id="36726-110">Tipp: Dolgozunk, működő néhány olyan alkalmazások létrehozásával meg.</span><span class="sxs-lookup"><span data-stu-id="36726-110">Tip: We're working on creating some working examples of apps for you.</span></span> <span data-ttu-id="36726-111">Új témakörök ismertetik, Microsoft Access QuickBooks és App-V használatával RemoteApp láthatja.</span><span class="sxs-lookup"><span data-stu-id="36726-111">You'll see new topics that discuss using Microsoft Access, QuickBooks, and App-V in RemoteApp.</span></span>

## <a name="requirements"></a><span data-ttu-id="36726-112">Követelmények</span><span class="sxs-lookup"><span data-stu-id="36726-112">Requirements</span></span>
<span data-ttu-id="36726-113">Három követelménynek, ha követi, segítséget az alkalmazás jól RemoteApp fut:</span><span class="sxs-lookup"><span data-stu-id="36726-113">These three requirements, if followed, help your application run well in RemoteApp:</span></span>

1. <span data-ttu-id="36726-114">Alkalmazások, amelyek minden [Windows asztali alkalmazások esetén a Hardvertanúsítvány követelményeit](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) és követi a [irányelvek programozási távoli asztali szolgáltatások](https://msdn.microsoft.com/library/aa383490.aspx) RemoteApp teljes kompatibilitás fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="36726-114">Applications that meet all [Certification requirements for Windows desktop apps](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) and adhere to [Remote Desktop Services programming guidelines](https://msdn.microsoft.com/library/aa383490.aspx) will have complete compatibility with RemoteApp.</span></span>
2. <span data-ttu-id="36726-115">Alkalmazások kell soha nem tárolnak adatokat helyileg a lemezkép vagy elvesznek, RemoteApp-példány.</span><span class="sxs-lookup"><span data-stu-id="36726-115">Applications should never store data locally on the image or RemoteApp instances that can be lost.</span></span>  <span data-ttu-id="36726-116">A RemoteApp-gyűjtemény létrehozása után a példányok klónozva vannak állapot nélküli és alkalmazások csak tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="36726-116">After you create a RemoteApp collection, the instances are cloned and are stateless and should only contain applications.</span></span> <span data-ttu-id="36726-117">Külső forrás vagy a felhasználó profiljában adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="36726-117">Store data in an external source or within the user's profile.</span></span>
3. <span data-ttu-id="36726-118">Egyéni lemezképek soha nem kell tartalmaznia, amely akkor szakadhat meg adatok.</span><span class="sxs-lookup"><span data-stu-id="36726-118">Custom images should never contain data that can be lost.</span></span>  

## <a name="testing-your-apps"></a><span data-ttu-id="36726-119">Az alkalmazások tesztelése</span><span class="sxs-lookup"><span data-stu-id="36726-119">Testing your apps</span></span>
<span data-ttu-id="36726-120">Használja az alkalmazások tesztelése az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="36726-120">Use these steps to testing applications:</span></span>

1. <span data-ttu-id="36726-121">Windows Server 2012 R2 és az alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="36726-121">Install Windows Server 2012 R2 and your application</span></span>
2. <span data-ttu-id="36726-122">A Távoli asztal engedélyezése</span><span class="sxs-lookup"><span data-stu-id="36726-122">Enable Remote Desktop</span></span>
3. <span data-ttu-id="36726-123">Hozzon létre két felhasználói fiókokat, a "a" felhasználó és a "b" felhasználó, a távoli asztal biztonsági csoportba való felvételekor mindkét felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="36726-123">Create two user accounts, UserA and UserB, adding both user accounts to the Remote Desktop security group.</span></span>
4. <span data-ttu-id="36726-124">Több munkamenet kompatibilitás-ellenőrzés létrehozásával a két egyidejű távoli asztali szolgáltatások munkamenet támadóknak a Számítógéphez az alkalmazás indítása közben.</span><span class="sxs-lookup"><span data-stu-id="36726-124">Check multi-session compatibility by establishing two simultaneous RDS sessions to the PC while launching the application.</span></span>
5. <span data-ttu-id="36726-125">Ellenőrizze az alkalmazás viselkedését</span><span class="sxs-lookup"><span data-stu-id="36726-125">Validate app behavior</span></span>

## <a name="application-development-guidelines"></a><span data-ttu-id="36726-126">Alkalmazás fejlesztői útmutató</span><span class="sxs-lookup"><span data-stu-id="36726-126">Application development guidelines</span></span>
<span data-ttu-id="36726-127">Használja az alábbi útmutatást a RemoteApp-alkalmazások fejlesztésével.</span><span class="sxs-lookup"><span data-stu-id="36726-127">Use the following guidelines for developing applications for RemoteApp.</span></span>

### <a name="multiple-users"></a><span data-ttu-id="36726-128">Több felhasználó</span><span class="sxs-lookup"><span data-stu-id="36726-128">Multiple users</span></span>
* <span data-ttu-id="36726-129">Telepítés egy [alkalmazás egy felhasználó ](https://msdn.microsoft.com/library/aa380661.aspx)problémákat okozhat az többfelhasználós környezetben.</span><span class="sxs-lookup"><span data-stu-id="36726-129">Installing an [application for a single user ](https://msdn.microsoft.com/library/aa380661.aspx)can create problems in a multiuser environment.</span></span>
* <span data-ttu-id="36726-130">Alkalmazások kell [kapcsolatos információkat tárolja](https://msdn.microsoft.com/library/aa383452.aspx) felhasználóspecifikus helyen, elkülönítve a globális adatokat az összes felhasználóra érvényes.</span><span class="sxs-lookup"><span data-stu-id="36726-130">Applications should [store user-specific information](https://msdn.microsoft.com/library/aa383452.aspx) in user-specific locations, separately from global information that applies to all users.</span></span>
* <span data-ttu-id="36726-131">RemoteApp használja több [kernel objektumok névterek](https://msdn.microsoft.com/library/aa382954.aspx); a globális névtér elsősorban az ügyfél/kiszolgáló alkalmazások services használja.</span><span class="sxs-lookup"><span data-stu-id="36726-131">RemoteApp uses multiple [namespaces for kernel objects](https://msdn.microsoft.com/library/aa382954.aspx); a global namespace is used primarily by services in client/server applications.</span></span>
* <span data-ttu-id="36726-132">Már nem biztonságos feltételeztük, hogy a számítógép nevét vagy a [IP-cím](https://msdn.microsoft.com/library/aa382942.aspx) a számítógéphez rendelt társítva egy-egy felhasználóhoz, mert több felhasználó is kell bejelentkeznie egyszerre egy távoli asztali munkamenetgazda (távoli asztali munkamenetgazda) kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="36726-132">It is not safe to assume that the computer name or the [IP address](https://msdn.microsoft.com/library/aa382942.aspx) assigned to the computer are associated with a single user because multiple users can be logged on simultaneously to a Remote Desktop Session Host (RD Session Host) server.</span></span>

### <a name="performance"></a><span data-ttu-id="36726-133">Teljesítmény</span><span class="sxs-lookup"><span data-stu-id="36726-133">Performance</span></span>
* <span data-ttu-id="36726-134">Tiltsa le a [grafikus hatások](https://msdn.microsoft.com/library/aa380822.aspx) csak az alkalmazás hozzáadni a RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="36726-134">Disable [graphic effects](https://msdn.microsoft.com/library/aa380822.aspx) before you add your app to RemoteApp.</span></span>
* <span data-ttu-id="36726-135">Minden felhasználó CPU rendelkezésre állásának maximalizálását, vagy tiltsa le a [feladatok háttérben ](https://msdn.microsoft.com/library/aa380665.aspx) , vagy hozzon létre, amelyek nincsenek erőforrás-igényes feladatok hatékony háttérben.</span><span class="sxs-lookup"><span data-stu-id="36726-135">To maximize CPU availability for all users, either disable [background tasks ](https://msdn.microsoft.com/library/aa380665.aspx) or create efficient background tasks that are not resource-intensive.</span></span>
* <span data-ttu-id="36726-136">Hangolja és alkalmazás egyensúlyba kell [használati szál](https://msdn.microsoft.com/library/aa383520.aspx) többfelhasználós, többprocesszoros környezethez.</span><span class="sxs-lookup"><span data-stu-id="36726-136">You should tune and balance application [thread usage](https://msdn.microsoft.com/library/aa383520.aspx) for a multiuser, multiprocessor environment.</span></span>
* <span data-ttu-id="36726-137">A teljesítmény optimalizálása érdekében ajánlott az alkalmazások [észlelése](https://msdn.microsoft.com/library/aa380798.aspx) e futtatnak, az ügyfél.</span><span class="sxs-lookup"><span data-stu-id="36726-137">To optimize performance, it is good practice for applications to [detect](https://msdn.microsoft.com/library/aa380798.aspx) whether they are running in a client session.</span></span>

