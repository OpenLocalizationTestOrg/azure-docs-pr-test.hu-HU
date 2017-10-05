---
title: "Egyéni lemezkép feltöltése az Azure Remoteappban |} Microsoft Docs"
description: "Útmutató: az Azure RemoteApp egyéni lemezkép feltöltése"
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
ms.openlocfilehash: 5a235fac88d6e95ea294bda197641108acb4a09f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="upload-a-custom-image-for-azure-remoteapp"></a><span data-ttu-id="21129-103">Az Azure RemoteApp egyéni lemezkép feltöltése</span><span class="sxs-lookup"><span data-stu-id="21129-103">Upload a custom image for Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="21129-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="21129-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="21129-105">A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="21129-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="21129-106">Most, hogy létrehozta az egyéni sablon rendszerképet, vagy frissítette a változások, kell, hogy a Rendszerkép feltöltése az Azure RemoteApp Képtár.</span><span class="sxs-lookup"><span data-stu-id="21129-106">Now that you have created your custom template image or have updated it with changes, you need to upload that image to your Azure RemoteApp image library.</span></span> <span data-ttu-id="21129-107">Kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="21129-107">Use these steps.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="21129-108">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="21129-108">Before you start</span></span>
1. <span data-ttu-id="21129-109">Ellenőrizze az egyéni lemezképet megfelel-e a [képre vonatkozó követelmények](remoteapp-imagereqs.md) és [alkalmazáskövetelményeket](remoteapp-appreqs.md).</span><span class="sxs-lookup"><span data-stu-id="21129-109">Verify your custom image meets the [image requirements](remoteapp-imagereqs.md) and [application requirements](remoteapp-appreqs.md).</span></span>
2. <span data-ttu-id="21129-110">Telepítse a [Azure PowerShell modul](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="21129-110">Install the [Azure PowerShell module](/powershell/azure/overview).</span></span>

## <a name="step-by-step-on-how-to-upload-custom-image"></a><span data-ttu-id="21129-111">Lépésről lépésre kapcsolatos egyéni lemezkép feltöltése</span><span class="sxs-lookup"><span data-stu-id="21129-111">Step by step on how to upload custom image</span></span>
1. <span data-ttu-id="21129-112">Nyissa meg az Azure felügyeleti portálon, és nyissa meg a RemoteApp lapot.</span><span class="sxs-lookup"><span data-stu-id="21129-112">Open Azure Management Portal and navigate to the RemoteApp page.</span></span>
2. <span data-ttu-id="21129-113">Az a **sablonrendszerképek** lapra, majd **feltöltése** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="21129-113">On the **Template images** tab, click **Upload** at the bottom of the page.</span></span>
3. <span data-ttu-id="21129-114">Adjon egy rövid nevet a lemezkép számára, és adja meg a tárfiók helyének.</span><span class="sxs-lookup"><span data-stu-id="21129-114">Enter a friendly name for your image and specify the storage account location.</span></span> <span data-ttu-id="21129-115">Győződjön meg arról a hely és a RemoteApp-gyűjteménnyel ugyanazon a helyen vagy egy helyet, ahová hozzon létre egyet.</span><span class="sxs-lookup"><span data-stu-id="21129-115">Ensure the location is the same location as your RemoteApp collection or a location where you want to create one.</span></span>
4. <span data-ttu-id="21129-116">Amikor a rendszer kéri, töltse le a parancsfájlt a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="21129-116">When prompted, download the script to your local PC.</span></span>
5. <span data-ttu-id="21129-117">A parancs paraméterei a szövegmezőben másolása a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="21129-117">Copy the command parameters in the text box to your clipboard.</span></span>
6. <span data-ttu-id="21129-118">Nyisson meg egy rendszergazda jogú Windows PowerShell-ablakban.</span><span class="sxs-lookup"><span data-stu-id="21129-118">Open an elevated Windows PowerShell window.</span></span>
7. <span data-ttu-id="21129-119">Emelt szintű Windows PowerShell ablakában keresse meg az azonos könyvtárat, amelybe letöltötte a parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="21129-119">From the elevated Windows PowerShell window, navigate to the same directory where you downloaded the script.</span></span>
8. <span data-ttu-id="21129-120">Illessze be a másolt parancs, és nyomja le az **Enter**.</span><span class="sxs-lookup"><span data-stu-id="21129-120">Paste the copied command and press **Enter**.</span></span>
   
   <span data-ttu-id="21129-121">A feltöltési folyamat akkor kezdődik, és időtartama függhet számos tényező közé tartoznak többek között a hálózati sebesség és a lemezkép mérete</span><span class="sxs-lookup"><span data-stu-id="21129-121">The upload process will begin and duration may depend on many factors including your network speed and size of the image</span></span>
9. <span data-ttu-id="21129-122">Ha a feltöltés nem sikerül miatt hálózati megszakítás és dolgot hasonlóan, mindig folytathatja a feltöltési folyamat megkezdése.</span><span class="sxs-lookup"><span data-stu-id="21129-122">If your upload does not succeed because of network interruption or things like that, you can always resume the upload process you began.</span></span> <span data-ttu-id="21129-123">Feltöltés folytatásához futtassa a parancsfájlt, az azonos parancssorban segítségével újból.</span><span class="sxs-lookup"><span data-stu-id="21129-123">To resume an upload, run the script again using the same command line.</span></span>

> [!WARNING]
> <span data-ttu-id="21129-124">Soha ne módosítsa a feltöltési parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="21129-124">Never modify the upload script.</span></span> <span data-ttu-id="21129-125">Annak érdekében, hogy a kép megfelel-e a lemezkép-követelmények és alkalmazáskövetelmények valósul ellenőrzésekkel.</span><span class="sxs-lookup"><span data-stu-id="21129-125">Specific checks have been implemented to ensure that the image meets the image requirements and application requirements.</span></span>
> 
> 

## <a name="common-problems"></a><span data-ttu-id="21129-126">Gyakori problémák</span><span class="sxs-lookup"><span data-stu-id="21129-126">Common problems</span></span>
* <span data-ttu-id="21129-127">Győződjön meg arról, hogy a Windows PowerShell, nem az Azure PowerShell használata.</span><span class="sxs-lookup"><span data-stu-id="21129-127">Make sure you use Windows PowerShell, not Azure PowerShell.</span></span> <span data-ttu-id="21129-128">Szeretné telepíteni az Azure PowerShell-modult, mert bizonyos modulok van szükség a feltöltési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="21129-128">You need to install the Azure PowerShell module because certain modules are needed during the upload process.</span></span>
* <span data-ttu-id="21129-129">Soha ne módosítsa a parancsprogram, érvényesítést vannak-e a felhasználók kényelme érdekében.</span><span class="sxs-lookup"><span data-stu-id="21129-129">Never alter the script, validations are there for your convenience.</span></span>
* <span data-ttu-id="21129-130">Ha a vhd-fájl feltöltése során lekérdezi zárolva, másolja a fájlt, vagy újra áthelyezése egy új helyre, és megpróbál feltöltése.</span><span class="sxs-lookup"><span data-stu-id="21129-130">If the vhd file gets locked out during upload, copy the file or move it to a new location and attempt upload again.</span></span> <span data-ttu-id="21129-131">Előfordulhat, hogy egy Windows-folyamat, amely megakadályozza az feltöltése.</span><span class="sxs-lookup"><span data-stu-id="21129-131">There might be some Windows process that is preventing upload.</span></span>  

