---
title: "aaaManaging Azure Cloud Explorer erőforrások |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Cloud Explorer toobrowse és a Visual Studio Azure-erőforrások kezeléséhez."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 6347dc53-f497-49d5-b29b-e8b9f0e939d7
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/25/2017
ms.author: kraigb
ms.openlocfilehash: 8a81660074d5d04be063df9e25076b7a97586514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-resources-associated-with-your-azure-accounts-in-visual-studio-cloud-explorer"></a><span data-ttu-id="397ee-103">A Visual Studio Cloud Explorer Azure fiókokhoz tartozó hello erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="397ee-103">Manage hello resources associated with your Azure accounts in Visual Studio Cloud Explorer</span></span>
<span data-ttu-id="397ee-104">Cloud Explorer lehetővé teszi, hogy Ön tooview az Azure-erőforrások és csoportok, vizsgálja meg az tulajdonságait, és a műveleteket kulcs fejlesztői diagnosztika a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="397ee-104">Cloud Explorer enables you tooview your Azure resources and resource groups, inspect their properties, and perform key developer diagnostics actions from within Visual Studio.</span></span> 

<span data-ttu-id="397ee-105">Például a hello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040), Cloud Explorer épül hello Azure Resource Manager-készletben.</span><span class="sxs-lookup"><span data-stu-id="397ee-105">Like hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), Cloud Explorer is built on hello Azure Resource Manager stack.</span></span> <span data-ttu-id="397ee-106">Ezért Cloud Explorer megértette erőforrások, például Azure erőforráscsoport-sablonok és a Logic Apps alkalmazásokat és API-alkalmazások például az Azure-szolgáltatásokat, valamint a [szerepköralapú hozzáférés-vezérlés](active-directory/role-based-access-control-configure.md) (RBAC).</span><span class="sxs-lookup"><span data-stu-id="397ee-106">Therefore, Cloud Explorer understands resources such as Azure resource groups and Azure services such as Logic apps and API apps, and it supports [role-based access control](active-directory/role-based-access-control-configure.md) (RBAC).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="397ee-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="397ee-107">Prerequisites</span></span>
- <span data-ttu-id="397ee-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/) a hello **Azure számítási** kiválasztva, vagy a Visual Studio korábbi verzióját a hello [Microsoft Azure SDK for .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).</span><span class="sxs-lookup"><span data-stu-id="397ee-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/) with hello **Azure workload** selected, or an earlier version of Visual Studio with hello [Microsoft Azure SDK for .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).</span></span>
- <span data-ttu-id="397ee-109">A Microsoft Azure-fiók – Ha nincs fiókja, akkor [regisztráljon egy ingyenes próbaverzióra](http://go.microsoft.com/fwlink/?LinkId=623901) vagy [aktiválhatja a Visual Studio előfizetői előnyeit](http://go.microsoft.com/fwlink/?LinkId=623901).</span><span class="sxs-lookup"><span data-stu-id="397ee-109">Microsoft Azure account - If you don't have an account, you can [sign up for a free trial](http://go.microsoft.com/fwlink/?LinkId=623901) or [activate your Visual Studio subscriber benefits](http://go.microsoft.com/fwlink/?LinkId=623901).</span></span>

> [!NOTE]
> <span data-ttu-id="397ee-110">Válassza ki a Cloud Explorer tooview **nézet** > **Cloud Explorer** hello menüsávjában.</span><span class="sxs-lookup"><span data-stu-id="397ee-110">tooview Cloud Explorer, select **View** > **Cloud Explorer** on hello menu bar.</span></span>   
> 
> 

## <a name="add-an-azure-account-toocloud-explorer"></a><span data-ttu-id="397ee-111">Egy Azure-fiók tooCloud Explorer hozzáadása</span><span class="sxs-lookup"><span data-stu-id="397ee-111">Add an Azure account tooCloud Explorer</span></span>
<span data-ttu-id="397ee-112">tooview hello erőforrások társított Azure-fiókot, először hozzá kell hello fiók tooCloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="397ee-112">tooview hello resources associated with an Azure account, you must first add hello account tooCloud Explorer.</span></span> 

1. <span data-ttu-id="397ee-113">A **Cloud Explorer**, jelölje be **Azure-fiók beállításai**.</span><span class="sxs-lookup"><span data-stu-id="397ee-113">In **Cloud Explorer**, select **Azure account settings**.</span></span>

    ![Cloud Explorer Azure fiók beállításainak ikonja](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. <span data-ttu-id="397ee-115">Válassza ki **új fiókot felveszi**.</span><span class="sxs-lookup"><span data-stu-id="397ee-115">Select **Add new account**.</span></span> 

    ![Cloud Explorer-fiók hozzáadása hivatkozás](media/vs-azure-tools-resources-managing-with-cloud-explorer/add-account-link.png)

1. <span data-ttu-id="397ee-117">Jelentkezzen be Azure-fiók azt szeretné, amelynek erőforrásait toohello toobrowse.</span><span class="sxs-lookup"><span data-stu-id="397ee-117">Log in toohello Azure account whose resources you want toobrowse.</span></span> 

1. <span data-ttu-id="397ee-118">Miután bejelentkezett az Azure-fiók tooan, azzal a fiókkal társított hello előfizetések jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="397ee-118">Once logged in tooan Azure account, hello subscriptions associated with that account display.</span></span> <span data-ttu-id="397ee-119">Válassza ki a kívánt toobrowse, és válassza ki hello fiók-előfizetések jelölőnégyzeteit hello **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="397ee-119">Select hello check boxes for hello account subscriptions you want toobrowse and then select **Apply**.</span></span> 
 
    ![Cloud Explorer: válassza ki az Azure-előfizetések toodisplay](media/vs-azure-tools-resources-managing-with-cloud-explorer/select-subscriptions.png)

1. <span data-ttu-id="397ee-121">Azt szeretné, amelynek erőforrásait hello előfizetések kiválasztása után toobrowse, ezen előfizetések és az erőforrások megjelenítése hello Cloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="397ee-121">After selecting hello subscriptions whose resources you want toobrowse, those subscriptions and resources display in hello Cloud Explorer.</span></span>

    ![Cloud Explorer erőforrás Azure-fiókot listázása](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-listed.png)

## <a name="remove-an-azure-account-from-cloud-explorer"></a><span data-ttu-id="397ee-123">Távolítsa el az Azure-fiók Cloud Explorer</span><span class="sxs-lookup"><span data-stu-id="397ee-123">Remove an Azure account from Cloud Explorer</span></span> 

1. <span data-ttu-id="397ee-124">A **Cloud Explorer**, jelölje be **Azure-fiók beállításai**.</span><span class="sxs-lookup"><span data-stu-id="397ee-124">In **Cloud Explorer**, select **Azure account settings**.</span></span>

    ![Cloud Explorer Azure fiók beállításainak ikonja](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. <span data-ttu-id="397ee-126">Válassza ki a következő toohello fiók tooremove, **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="397ee-126">Next toohello account you want tooremove, select **Remove**.</span></span>

    ![Cloud Explorer Azure fiók beállításainak ikonja](media/vs-azure-tools-resources-managing-with-cloud-explorer/remove-account.png)

## <a name="view-resource-types-or-resource-groups"></a><span data-ttu-id="397ee-128">Erőforrástípus vagy erőforráscsoportok megtekintésére</span><span class="sxs-lookup"><span data-stu-id="397ee-128">View resource types or resource groups</span></span>
<span data-ttu-id="397ee-129">tooview az Azure-erőforrások, dönthet úgy, vagy **erőforrástípusok** vagy **erőforráscsoportok** nézet.</span><span class="sxs-lookup"><span data-stu-id="397ee-129">tooview your Azure resources, you can choose either **Resource Types** or **Resource Groups** view.</span></span>

1. <span data-ttu-id="397ee-130">A **Cloud Explorer**, válassza ki az erőforrás nézet legördülő lista hello.</span><span class="sxs-lookup"><span data-stu-id="397ee-130">In **Cloud Explorer**, select hello resource view dropdown.</span></span>

    ![Cloud Explorer legördülő lista tooselect hello szükséges erőforrások megtekintése](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-view-dropdown.png)

1. <span data-ttu-id="397ee-132">Hello helyi menüből válassza ki a kívánt hello nézetet:</span><span class="sxs-lookup"><span data-stu-id="397ee-132">From hello context menu, select hello desired view:</span></span> 

    - <span data-ttu-id="397ee-133">**Erőforrástípusok** nézet – hello közös nézet hello használt [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040), az Azure-erőforrások kategóriába sorolt, például webes alkalmazásokat, a storage-fiókok és a virtuális gépek típus szerint jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="397ee-133">**Resource Types** view - hello common view used on hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), shows your Azure resources categorized by their type, such as web apps, storage accounts, and virtual machines.</span></span> 
    - <span data-ttu-id="397ee-134">**Erőforráscsoportok** - hello Azure erőforráscsoport, amellyel fontosságúak társított szerint kategorizálja Azure-erőforrások megtekintése.</span><span class="sxs-lookup"><span data-stu-id="397ee-134">**Resource Groups** view - Categorizes Azure resources by hello Azure resource group with which they're associated.</span></span> <span data-ttu-id="397ee-135">Egy erőforráscsoportot az Azure-erőforrások, általában egy adott alkalmazás által használt csomag egyik gyermekszoftver.</span><span class="sxs-lookup"><span data-stu-id="397ee-135">A resource group is a bundle of Azure resources, typically used by a specific application.</span></span> <span data-ttu-id="397ee-136">További információ az Azure erőforráscsoport-sablonok, toolearn lásd [Azure Resource Manager áttekintése](./azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="397ee-136">toolearn more about Azure resource groups, see [Azure Resource Manager Overview](./azure-resource-manager/resource-group-overview.md).</span></span>

    <span data-ttu-id="397ee-137">hello következő kép bemutatja hello összehasonlítása két erőforrás nézeteinek társítása:</span><span class="sxs-lookup"><span data-stu-id="397ee-137">hello following image shows a comparison of hello two resource views:</span></span>

    ![Cloud Explorer erőforrás nézetek összehasonlítása](media/vs-azure-tools-resources-managing-with-cloud-explorer/resource-views-comparison.png)

## <a name="view-and-navigate-resources-in-cloud-explorer"></a><span data-ttu-id="397ee-139">Megtekintheti, és keresse meg a Cloud Explorer erőforrások</span><span class="sxs-lookup"><span data-stu-id="397ee-139">View and navigate resources in Cloud Explorer</span></span>
<span data-ttu-id="397ee-140">Azure-erőforrás toonavigate tooan és megtekintse az adataikat a Cloud Explorer, bontsa ki a hello elem típusa vagy tartozó erőforráscsoport, és válassza a hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="397ee-140">toonavigate tooan Azure resource and view its information in Cloud Explorer, expand hello item's type or associated resource group and then select hello resource.</span></span> <span data-ttu-id="397ee-141">Erőforrás kiválasztásakor információk megjelennek-e a két hello lapok - **műveletek** és **tulajdonságok** - Cloud Explorer hello alján.</span><span class="sxs-lookup"><span data-stu-id="397ee-141">When you select a resource, information appears in hello two tabs - **Actions** and **Properties** - at hello bottom of Cloud Explorer.</span></span> 

- <span data-ttu-id="397ee-142">**Műveletek** lapon - listák hello elvégezhető műveletekhez nyújtanak a Cloud Explorer hello kiválasztott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="397ee-142">**Actions** tab - Lists hello actions you can take in Cloud Explorer for hello selected resource.</span></span> <span data-ttu-id="397ee-143">Is megtekintheti ezeket a beállításokat kattintson a jobb gombbal a hello erőforrás tooview a helyi menüből.</span><span class="sxs-lookup"><span data-stu-id="397ee-143">You can also view these options by right-clicking hello resource tooview its context menu.</span></span>

- <span data-ttu-id="397ee-144">**Tulajdonságok** lapon - hello tulajdonságainak megjelenítése hello erőforrás, például a típusa, területi beállítás és az erőforrás csoport, amelyhez társítva.</span><span class="sxs-lookup"><span data-stu-id="397ee-144">**Properties** tab - Shows hello properties of hello resource, such as its type, locale, and resource group with which it is associated.</span></span>

<span data-ttu-id="397ee-145">hello alábbi ábrán láthatók az egyes lapokon az egy App Service egy példa összehasonlítása:</span><span class="sxs-lookup"><span data-stu-id="397ee-145">hello following image shows an example comparison of what you see on each tab for an App Service:</span></span>

![](./media/vs-azure-tools-resources-managing-with-cloud-explorer/actions-and-properties.png)

<span data-ttu-id="397ee-146">Minden erőforrás van hello művelet **nyissa meg a portál**.</span><span class="sxs-lookup"><span data-stu-id="397ee-146">Every resource has hello action **Open in portal**.</span></span> <span data-ttu-id="397ee-147">Amikor úgy dönt, hogy ez a művelet, Cloud Explorer jeleníti meg hello kiválasztott erőforrás hello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="397ee-147">When you choose this action, Cloud Explorer displays hello selected resource in hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span> <span data-ttu-id="397ee-148">Hello **nyissa meg a portál** szolgáltatás toodeeply beágyazott erőforrások navigáláshoz lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="397ee-148">hello **Open in portal** feature is handy for navigating toodeeply nested resources.</span></span>

<span data-ttu-id="397ee-149">További műveletek és a tulajdonságértékek is megjelenhetnek hello Azure erőforráscsoport alapján.</span><span class="sxs-lookup"><span data-stu-id="397ee-149">Additional actions and property values may also appear based on hello Azure resource.</span></span> <span data-ttu-id="397ee-150">Például webes alkalmazásokat és a logic apps is hello műveletek **nyissa meg böngészőben** és **Hibakereső csatlakoztatása** továbbá túl**nyissa meg a portál**.</span><span class="sxs-lookup"><span data-stu-id="397ee-150">For example, web apps and logic apps also have hello actions **Open in browser** and **Attach debugger** in addition too**Open in portal**.</span></span> <span data-ttu-id="397ee-151">Műveletek tooopen szerkesztők jelennek meg, ha úgy dönt, hogy a tárolási fiók blob, üzenetsor vagy tábla.</span><span class="sxs-lookup"><span data-stu-id="397ee-151">Actions tooopen editors appear when you choose a storage account blob, queue, or table.</span></span> <span data-ttu-id="397ee-152">Az Azure-alkalmazások is rendelkeznek **URL-cím** és **állapot** tulajdonságok, míg a tárolási erőforrások kulcs és a kapcsolati karakterlánc tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="397ee-152">Azure apps have **URL** and **Status** properties, while storage resources have key and connection string properties.</span></span>

## <a name="find-resources-in-cloud-explorer"></a><span data-ttu-id="397ee-153">A Cloud Explorer erőforrások keresése</span><span class="sxs-lookup"><span data-stu-id="397ee-153">Find resources in Cloud Explorer</span></span>
<span data-ttu-id="397ee-154">a megadott név megadásával az Azure-fiók előfizetések toolocate erőforrások adja meg a hello nevét, a hello **keresési** Cloud Explorer párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="397ee-154">toolocate resources with a specific name in your Azure account subscriptions, enter hello name in hello **Search** box in Cloud Explorer.</span></span>

![A Cloud Explorer erőforrások keresése](./media/vs-azure-tools-resources-managing-with-cloud-explorer/search-for-resources.png)

<span data-ttu-id="397ee-156">Hello karakter beírásakor **keresési** mezőbe csak azok a karakterek megfelelő erőforrások jelennek meg a hello erőforrások fájában.</span><span class="sxs-lookup"><span data-stu-id="397ee-156">As you enter characters in hello **Search** box, only resources that match those characters appear in hello resource tree.</span></span>
