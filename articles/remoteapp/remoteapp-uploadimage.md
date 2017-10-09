---
title: "aaaUpload egyéni lemezképként az Azure Remoteappban |} Microsoft Docs"
description: "Ismerje meg, hogyan tooupload egyéni rendszerképet az Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: 299e0510-1a6b-4fdf-914a-3631b061a360
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: ericor
ms.openlocfilehash: 6ad40fe58795ece37f4c7900be01bc713938da87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-custom-image-for-azure-remoteapp"></a><span data-ttu-id="4333c-103">Az Azure RemoteApp egyéni lemezkép feltöltése</span><span class="sxs-lookup"><span data-stu-id="4333c-103">Upload a custom image for Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="4333c-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="4333c-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="4333c-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="4333c-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="4333c-106">Most, hogy létrehozta az egyéni sablon rendszerképet, vagy frissítette a változások, kell tooupload adott kép tooyour Azure RemoteApp Képtár.</span><span class="sxs-lookup"><span data-stu-id="4333c-106">Now that you have created your custom template image or have updated it with changes, you need tooupload that image tooyour Azure RemoteApp image library.</span></span> <span data-ttu-id="4333c-107">Kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="4333c-107">Use these steps.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="4333c-108">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="4333c-108">Before you start</span></span>
1. <span data-ttu-id="4333c-109">Ellenőrizze az egyéni lemezképet megfelel-e hello [képre vonatkozó követelmények](remoteapp-imagereqs.md) és [alkalmazáskövetelményeket](remoteapp-appreqs.md).</span><span class="sxs-lookup"><span data-stu-id="4333c-109">Verify your custom image meets hello [image requirements](remoteapp-imagereqs.md) and [application requirements](remoteapp-appreqs.md).</span></span>
2. <span data-ttu-id="4333c-110">Telepítse a hello [Azure PowerShell modul](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4333c-110">Install hello [Azure PowerShell module](/powershell/azure/overview).</span></span>

## <a name="step-by-step-on-how-tooupload-custom-image"></a><span data-ttu-id="4333c-111">Lépésről lépésre arról, hogy hogyan tooupload egyéni kép</span><span class="sxs-lookup"><span data-stu-id="4333c-111">Step by step on how tooupload custom image</span></span>
1. <span data-ttu-id="4333c-112">Nyissa meg az Azure felügyeleti portálon, és keresse meg a toohello RemoteApp lap.</span><span class="sxs-lookup"><span data-stu-id="4333c-112">Open Azure Management Portal and navigate toohello RemoteApp page.</span></span>
2. <span data-ttu-id="4333c-113">A hello **sablonrendszerképek** lapra, majd **feltöltése** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="4333c-113">On hello **Template images** tab, click **Upload** at hello bottom of hello page.</span></span>
3. <span data-ttu-id="4333c-114">Adjon egy rövid nevet a lemezkép számára, és adja meg a hello tárfiókhely.</span><span class="sxs-lookup"><span data-stu-id="4333c-114">Enter a friendly name for your image and specify hello storage account location.</span></span> <span data-ttu-id="4333c-115">Győződjön meg arról, hello helye hello és a RemoteApp-gyűjtemény vagy egy helyre, ahol egy toocreate ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="4333c-115">Ensure hello location is hello same location as your RemoteApp collection or a location where you want toocreate one.</span></span>
4. <span data-ttu-id="4333c-116">Amikor a rendszer kéri, töltse le a hello parancsfájl tooyour helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4333c-116">When prompted, download hello script tooyour local PC.</span></span>
5. <span data-ttu-id="4333c-117">Hello parancsparaméterek hello szöveg mezőben tooyour vágólapra másolása.</span><span class="sxs-lookup"><span data-stu-id="4333c-117">Copy hello command parameters in hello text box tooyour clipboard.</span></span>
6. <span data-ttu-id="4333c-118">Nyisson meg egy rendszergazda jogú Windows PowerShell-ablakban.</span><span class="sxs-lookup"><span data-stu-id="4333c-118">Open an elevated Windows PowerShell window.</span></span>
7. <span data-ttu-id="4333c-119">A hello emelt szintű Windows PowerShell-ablakot, keresse meg a toohello azonos könyvtárat, amelybe letöltötte hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="4333c-119">From hello elevated Windows PowerShell window, navigate toohello same directory where you downloaded hello script.</span></span>
8. <span data-ttu-id="4333c-120">Beillesztés hello másolt parancs, és nyomja le az **Enter**.</span><span class="sxs-lookup"><span data-stu-id="4333c-120">Paste hello copied command and press **Enter**.</span></span>
   
   <span data-ttu-id="4333c-121">hello feltöltési folyamat akkor kezdődik, és időtartama függhet számos tényező közé tartoznak többek között a hálózati sebesség és a hello lemezkép mérete</span><span class="sxs-lookup"><span data-stu-id="4333c-121">hello upload process will begin and duration may depend on many factors including your network speed and size of hello image</span></span>
9. <span data-ttu-id="4333c-122">Ha a feltöltés nem sikerül miatt hálózati megszakítás és dolgot hasonlóan, mindig folytathatja hello feltöltési folyamat megkezdése.</span><span class="sxs-lookup"><span data-stu-id="4333c-122">If your upload does not succeed because of network interruption or things like that, you can always resume hello upload process you began.</span></span> <span data-ttu-id="4333c-123">tooresume feltöltés, újra hello parancsprogrammal hello azonos parancssorban.</span><span class="sxs-lookup"><span data-stu-id="4333c-123">tooresume an upload, run hello script again using hello same command line.</span></span>

> [!WARNING]
> <span data-ttu-id="4333c-124">Soha ne módosítsa a hello feltöltés parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="4333c-124">Never modify hello upload script.</span></span> <span data-ttu-id="4333c-125">Egyes ellenőrzéséről, hogy a kép megfelel-e hello kép követelményeknek és alkalmazáskövetelmények hello megvalósított tooensure törölték.</span><span class="sxs-lookup"><span data-stu-id="4333c-125">Specific checks have been implemented tooensure that hello image meets hello image requirements and application requirements.</span></span>
> 
> 

## <a name="common-problems"></a><span data-ttu-id="4333c-126">Gyakori problémák</span><span class="sxs-lookup"><span data-stu-id="4333c-126">Common problems</span></span>
* <span data-ttu-id="4333c-127">Győződjön meg arról, hogy a Windows PowerShell, nem az Azure PowerShell használata.</span><span class="sxs-lookup"><span data-stu-id="4333c-127">Make sure you use Windows PowerShell, not Azure PowerShell.</span></span> <span data-ttu-id="4333c-128">Meg kell tooinstall hello Azure PowerShell modul, mivel egyes modulok hello feltöltési folyamat során van szükség.</span><span class="sxs-lookup"><span data-stu-id="4333c-128">You need tooinstall hello Azure PowerShell module because certain modules are needed during hello upload process.</span></span>
* <span data-ttu-id="4333c-129">Soha ne módosítsa a hello parancsfájl, érvényesítést vannak-e a felhasználók kényelme érdekében.</span><span class="sxs-lookup"><span data-stu-id="4333c-129">Never alter hello script, validations are there for your convenience.</span></span>
* <span data-ttu-id="4333c-130">Ha hello vhd-fájl feltöltése során lekérdezi zárolva, hello másolás, vagy helyezze tooa új helyre, és megpróbál feltöltése újra.</span><span class="sxs-lookup"><span data-stu-id="4333c-130">If hello vhd file gets locked out during upload, copy hello file or move it tooa new location and attempt upload again.</span></span> <span data-ttu-id="4333c-131">Előfordulhat, hogy egy Windows-folyamat, amely megakadályozza az feltöltése.</span><span class="sxs-lookup"><span data-stu-id="4333c-131">There might be some Windows process that is preventing upload.</span></span>  

