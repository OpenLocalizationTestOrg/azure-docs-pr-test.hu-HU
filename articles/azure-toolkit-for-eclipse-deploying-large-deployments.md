---
title: "Nagy központi telepítése"
description: "Megtudhatja, hogyan telepítheti az Azure-eszközkészlet használata az Eclipse nagy méretű telepítéséhez."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 5e18bace-5df0-4af8-ad86-6151ea8bd823
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: e12e379e2b6727653e2377b1760c3745596a1e9c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploying-large-deployments"></a><span data-ttu-id="23e0b-103">Nagy központi telepítése</span><span class="sxs-lookup"><span data-stu-id="23e0b-103">Deploying Large Deployments</span></span>
<span data-ttu-id="23e0b-104">Ha a telepítés túl nagy ahhoz, hogy az alapértelmezett approot mappában található, használhatja a helyi tároló egyik erőforrásához, a központi telepítés gyökérmappa a JDK és az alkalmazáskiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="23e0b-104">If your deployment is too large to be contained in the default approot folder, you can use a local storage resource as the deployment root folder for your JDK and application server.</span></span>

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a><span data-ttu-id="23e0b-105">A helyi tároló egyik erőforrásához használandó telepítési gyökérmappájába nagy méretű telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="23e0b-105">To use a local storage resource as the deployment root folder for large deployments</span></span>
1. <span data-ttu-id="23e0b-106">Hozzon létre egy új helyi tároló-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="23e0b-106">Create a new local storage resource.</span></span> <span data-ttu-id="23e0b-107">Az erőforrás neve nem lényeges.</span><span class="sxs-lookup"><span data-stu-id="23e0b-107">The name of the resource does not matter.</span></span> <span data-ttu-id="23e0b-108">Tároló-erőforrások a szerepkör szintjén van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="23e0b-108">Storage resources are defined at the role level.</span></span> <span data-ttu-id="23e0b-109">A leggyorsabban úgy tudja elérni a helyi tároló konfigurációs párbeszédpanel, ahol létrehozhat egy új helyi tároló-erőforrás van a következő lépések segítségével: kattintson a jobb gombbal a szerepkör a **Project Explorer** nézet (bontsa ki az Azure-projekt csomópont Ha a szerepkör nem látható), kattintson **Azure**, és kattintson a **helyi tároló**.</span><span class="sxs-lookup"><span data-stu-id="23e0b-109">The quickest way to access the local storage configuration dialog, from which you could create a new local storage resource, is by using the following steps: Right-click the role in the **Project Explorer** view (expand your Azure project node if you don't see the role), click **Azure**, and then click **Local Storage**.</span></span> <span data-ttu-id="23e0b-110">Belül a **helyi tároló** párbeszédpanel, kattintson a **Hozzáadás** válasszal létrehozhat egy új helyi tárterület.</span><span class="sxs-lookup"><span data-stu-id="23e0b-110">Within the **Local Storage** dialog, click **Add** to create a new local storage resource.</span></span>

2. <span data-ttu-id="23e0b-111">A kívánt méretet állítsa legalább 2048 MB-ra (semmit kevesebb problémái lehetnek ugyanazt a fájlt méretét, akkor tapasztal a approot).</span><span class="sxs-lookup"><span data-stu-id="23e0b-111">Set the desired size to at least 2048 MB (anything less may cause the same file size problems as you would encounter in the approot).</span></span>

3. <span data-ttu-id="23e0b-112">Győződjön meg arról, hogy **tiszta tartalmát, amikor a szerepkör példánya újraindul** be van jelölve; ez segít a központi telepítés ügyfélindítási logikája a szerepkörpéldány esetén az erőforrás már meglévő fájlok való ütközések futásának megakadályozása felhasználását.</span><span class="sxs-lookup"><span data-stu-id="23e0b-112">Ensure that **Clean the contents when the role instance is recycled** is checked; this will help prevent the deployment's startup logic from running into conflicts with pre-existing files in the resource when the role instance is recycled.</span></span>

4. <span data-ttu-id="23e0b-113">Győződjön meg arról, hogy a **környezeti változó tárolja az erőforrás elérési útja a telepítés utáni** a karakterlánc értéke **DEPLOYROOT**.</span><span class="sxs-lookup"><span data-stu-id="23e0b-113">Ensure that the **Environment variable storing the resource's directory path after deployment** value is set to the string **DEPLOYROOT**.</span></span> <span data-ttu-id="23e0b-114">A helyi tároló-erőforrás párbeszédpanel az alábbihoz hasonlóan fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="23e0b-114">Your local storage resource dialog will look similar to the following.</span></span>

   ![][ic667943]

<span data-ttu-id="23e0b-115">Azt is megteheti Ha **DEPLOYROOT** , a *neve* a helyi erőforrások és, ne módosítsa az automatikusan generált környezeti változó neve (amely úgy lesz beállítva, **DEPLOYROOT_ Elérési út** ebben az esetben), működő lenne, az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="23e0b-115">Alternatively, if you use **DEPLOYROOT** as the *name* of your local resource and you do not change the automatically-generated environment variable name (which will be set to **DEPLOYROOT_PATH** in that case), that would work for your application as well.</span></span>

<span data-ttu-id="23e0b-116">A helyi tároló egyik erőforrásához létrehozásával kapcsolatos további információk találhatók [helyi tároló tulajdonságainak][Local storage properties].</span><span class="sxs-lookup"><span data-stu-id="23e0b-116">Additional information about creating a local storage resource can be found at [Local storage properties][Local storage properties].</span></span>

## <a name="see-also"></a><span data-ttu-id="23e0b-117">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="23e0b-117">See Also</span></span>
<span data-ttu-id="23e0b-118">[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="23e0b-118">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="23e0b-119">[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="23e0b-119">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="23e0b-120">[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="23e0b-120">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="23e0b-121">Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="23e0b-121">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
