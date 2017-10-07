---
title: "aaaUsing Emulator Express toorun és hibakeresése az Azure felhőalapú szolgáltatás a helyi számítógépen |} Microsoft Docs"
description: "Helyi gépen toorun Emulator Express és a hibakeresési egy felhőalapú szolgáltatás használata"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 73108f98-a552-4817-b7a1-551367b71906
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: d89a0fc2dce42b4a2d2f93f9c4ff0482af924ad4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-emulator-express-toorun-and-debug-an-azure-cloud-service-on-a-local-machine"></a><span data-ttu-id="050d4-103">Emulator Express használatával toorun és hibakeresése az Azure felhőalapú szolgáltatás a helyi számítógépen</span><span class="sxs-lookup"><span data-stu-id="050d4-103">Using Emulator Express toorun and debug an Azure cloud service on a local machine</span></span>
<span data-ttu-id="050d4-104">Emulator Express használatával tesztelése, és egy felhőalapú szolgáltatás hibakeresése a Visual Studio futtatása rendszergazdaként nélkül.</span><span class="sxs-lookup"><span data-stu-id="050d4-104">By using Emulator Express, you can test and debug a cloud service without running Visual Studio as an administrator.</span></span> <span data-ttu-id="050d4-105">Beállíthatja a projekt beállítások toouse vagy Emulator Express vagy hello teljes emulátor, attól függően, hogy a felhőszolgáltatás hello követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="050d4-105">You can set your project settings toouse either Emulator Express or hello full emulator, depending on hello requirements of your cloud service.</span></span> <span data-ttu-id="050d4-106">Hello teljes emulátor kapcsolatos további információkért lásd: [egy Azure-alkalmazás futtatását a Compute Emulator hello](storage/common/storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="050d4-106">For more information about hello full emulator, see [Run an Azure Application in hello Compute Emulator](storage/common/storage-use-emulator.md).</span></span>

## <a name="using-emulator-express-in-visual-studio"></a><span data-ttu-id="050d4-107">A Visual Studio Emulator Express használatával</span><span class="sxs-lookup"><span data-stu-id="050d4-107">Using Emulator Express in Visual Studio</span></span>
<span data-ttu-id="050d4-108">Az Azure SDK 2.3 vagy újabb Azure-projekt létrehozásakor automatikusan Emulator Express szolgál.</span><span class="sxs-lookup"><span data-stu-id="050d4-108">When you create an Azure project in Azure SDK 2.3 or later, Emulator Express is automatically used.</span></span> <span data-ttu-id="050d4-109">Egy korábbi hello Azure SDK-val létrehozott meglévő projekteket használja a következő lépéseket tooselect Emulator Express hello:</span><span class="sxs-lookup"><span data-stu-id="050d4-109">For existing projects that were created with an earlier version of hello Azure SDK, use hello following steps tooselect Emulator Express:</span></span>

1. <span data-ttu-id="050d4-110">Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.</span><span class="sxs-lookup"><span data-stu-id="050d4-110">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="050d4-111">A **Megoldáskezelőben**, kattintson a jobb gombbal a projekt hello és, hello helyi menüből válassza ki a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="050d4-111">In **Solution Explorer**, right-click hello project, and, from hello context menu, select **Properties**.</span></span>

1. <span data-ttu-id="050d4-112">Hello projektek tulajdonságai lapon válassza ki a hello **webes** fülre.</span><span class="sxs-lookup"><span data-stu-id="050d4-112">In hello projects properties pages, select hello **Web** tab.</span></span>

    ![Egy Azure-felhőszolgáltatás-projekt tulajdonságai](./media/vs-azure-tools-emulator-express-debug-run/web-properties.png)

1. <span data-ttu-id="050d4-114">A **helyi fejlesztési kiszolgáló**, jelölje be **beállítás használatát az IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="050d4-114">Under **Local Development Server**, select **Use IIS Express option**.</span></span>

1. <span data-ttu-id="050d4-115">A **emulátor**, jelölje be **használata Emulator Express**.</span><span class="sxs-lookup"><span data-stu-id="050d4-115">Under **Emulator**, select **Use Emulator Express**.</span></span>
   
1. <span data-ttu-id="050d4-116">toolaunch hello Emulator Express, futtassa a következő parancsot a parancssorba hello:</span><span class="sxs-lookup"><span data-stu-id="050d4-116">toolaunch hello Emulator Express, run hello following command at a command prompt:</span></span> 

    ```
    csrun.exe /useemulatorexpress
    ```

## <a name="emulator-express-limitations"></a><span data-ttu-id="050d4-117">Emulator Express korlátozásai</span><span class="sxs-lookup"><span data-stu-id="050d4-117">Emulator Express limitations</span></span>
<span data-ttu-id="050d4-118">a következő problémák hello ismert korlátozásai Emulator Express:</span><span class="sxs-lookup"><span data-stu-id="050d4-118">hello following issues are known limitations of Emulator Express:</span></span> 

- <span data-ttu-id="050d4-119">Emulator Express nem kompatibilis az IIS-webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="050d4-119">Emulator Express is not compatible with IIS Web Server.</span></span>
- <span data-ttu-id="050d4-120">A felhőalapú szolgáltatás több szerepkört is tartalmazhat, de minden egyes szerepkör korlátozott tooone példány.</span><span class="sxs-lookup"><span data-stu-id="050d4-120">Your cloud service can contain multiple roles, but each role is limited tooone instance.</span></span>
- <span data-ttu-id="050d4-121">Portszámok 1000 alatt nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="050d4-121">You can't access port numbers below 1000.</span></span> <span data-ttu-id="050d4-122">Ha használ olyan hitelesítésszolgáltatója, amely általában az alábbi 1000 portot használ, szükség lehet toochange ezen érték tooa portszám, amely 1000 fent.</span><span class="sxs-lookup"><span data-stu-id="050d4-122">If you use an authentication provider that normally uses a port below 1000, you might need toochange this value tooa port number that's above 1000.</span></span>
- <span data-ttu-id="050d4-123">Azure Compute Emulator toohello vonatkozó korlátozások is érvényesek tooEmulator Express.</span><span class="sxs-lookup"><span data-stu-id="050d4-123">Any limitations that apply toohello Azure Compute Emulator also apply tooEmulator Express.</span></span> <span data-ttu-id="050d4-124">Például nem lehet üzemelő példányonként legfeljebb 50 szerepkörpéldányokat.</span><span class="sxs-lookup"><span data-stu-id="050d4-124">For example, you can't have more than 50 role instances per deployment.</span></span> <span data-ttu-id="050d4-125">Hello Azure Compute Emulator kapcsolatos további információkért lásd: [egy Azure-alkalmazás futtatását a Compute Emulator hello](http://go.microsoft.com/fwlink/p/?LinkId=623050).</span><span class="sxs-lookup"><span data-stu-id="050d4-125">For more information about hello Azure Compute Emulator, see [Run an Azure Application in hello Compute Emulator](http://go.microsoft.com/fwlink/p/?LinkId=623050).</span></span>

## <a name="next-steps"></a><span data-ttu-id="050d4-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="050d4-126">Next steps</span></span>
[<span data-ttu-id="050d4-127">Hibakeresési Azure cloud services csomag</span><span class="sxs-lookup"><span data-stu-id="050d4-127">Debugging Azure cloud services</span></span>](https://msdn.microsoft.com/library/azure/ee405479.aspx)
