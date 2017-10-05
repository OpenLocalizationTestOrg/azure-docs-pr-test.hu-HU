---
title: "Virtuális gépek felügyelni az intellij-t az Azure-kezelővel használatával |} Microsoft Docs"
description: "Útmutató az Azure virtuális gépek felügyelni az intellij-t az Azure-kezelővel használatával."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 9197580407b3509fbf9a842e1fee1e6348478c34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="07d92-103">Az intellij-t az Azure-kezelővel használatával virtuális gépek kezelése</span><span class="sxs-lookup"><span data-stu-id="07d92-103">Manage virtual machines by using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="07d92-104">Az Azure-kezelővel, amely az IntelliJ Azure eszköztára része, Java fejlesztők egy könnyen kezelhető megoldás biztosít a felhőszolgáltatóknak a virtuális gépek a Azure fiók belül az IntelliJ integrált fejlesztési környezeti (IDE).</span><span class="sxs-lookup"><span data-stu-id="07d92-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside the IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-intellij"></a><span data-ttu-id="07d92-105">Virtuális gép létrehozása az intellij-t</span><span class="sxs-lookup"><span data-stu-id="07d92-105">Create a virtual machine in IntelliJ</span></span>

<span data-ttu-id="07d92-106">Virtuális gép létrehozása az Azure Explorerrel, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="07d92-106">To create a virtual machine by using the Azure Explorer, do the following:</span></span> 

1. <span data-ttu-id="07d92-107">A lépések az Azure-fiókjával jelentkezzen be a [bejelentkezési utasításokat az Azure-eszközkészlet az IntelliJ] cikk.</span><span class="sxs-lookup"><span data-stu-id="07d92-107">Sign in to your Azure account by using the steps in the [Sign-in instructions for the Azure Toolkit for IntelliJ] article.</span></span>

2. <span data-ttu-id="07d92-108">Az a **Azure Explorer** megtekintéséhez bontsa ki a **Azure** csomópontot, kattintson a jobb gombbal **virtuális gépek**, és kattintson a **hozzon létre virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="07d92-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span> 

   <span data-ttu-id="07d92-109">![A virtuális gép létrehozása parancs][CR01]</span><span class="sxs-lookup"><span data-stu-id="07d92-109">![The Create VM command][CR01]</span></span>  
    <span data-ttu-id="07d92-110">A **hozzon létre új virtuális gépet** varázsló megnyílik.</span><span class="sxs-lookup"><span data-stu-id="07d92-110">The **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="07d92-111">Az a **válasszon egy előfizetést** ablakban jelölje ki az előfizetését, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="07d92-111">In the **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span> 

   ![A Válasszon egy előfizetés ablak][CR02]

4. <span data-ttu-id="07d92-113">Az a **válassza ki a virtuálisgép-lemezkép** ablak, írja be a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="07d92-113">In the **Select a Virtual Machine Image** window, enter the following information:</span></span>

   * <span data-ttu-id="07d92-114">**Hely**: határozza meg, ahol létrejön a virtuális gép (például *USA nyugati régiója*).</span><span class="sxs-lookup"><span data-stu-id="07d92-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span> 

   * <span data-ttu-id="07d92-115">**Kép ajánlott**: meghatározza, hogy fog választhat egy lemezképet egy gyakran használt képek rövidített listájának.</span><span class="sxs-lookup"><span data-stu-id="07d92-115">**Recommended image**: Specifies that you will choose an image from an abbreviated list of commonly used images.</span></span>

   * <span data-ttu-id="07d92-116">**Kép: egyéni**: meghatározza a fog választani egy egyéni lemezképet, adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="07d92-116">**Custom image**: Specifies that you will choose a custom image by providing the following information:</span></span>

      * <span data-ttu-id="07d92-117">**A Publisher**: a közzétevő a lemezképet a virtuális géphez használni kívánt létrehozott határozza meg (például *Microsoft*).</span><span class="sxs-lookup"><span data-stu-id="07d92-117">**Publisher**: Specifies the publisher that created the image that you will use for your virtual machine (for example, *Microsoft*).</span></span>

      * <span data-ttu-id="07d92-118">**Ajánlat**: Adja meg a virtuális gép használata a kijelölt közzétételi ajánlat (például *JDK*).</span><span class="sxs-lookup"><span data-stu-id="07d92-118">**Offer**: Specifies the virtual machine offering to use from the selected publisher (for example, *JDK*).</span></span>

      * <span data-ttu-id="07d92-119">**SKU**: Adja meg a Raktározásiegység (SKU) a kijelölt ajánlat használata (például *JDK_8*).</span><span class="sxs-lookup"><span data-stu-id="07d92-119">**Sku**: Specifies the stockkeeping unit (SKU) to use from the selected offering (for example, *JDK_8*).</span></span>

      * <span data-ttu-id="07d92-120">**Verzió #**: Adja meg a kiválasztott Termékváltozat használandó verziójának.</span><span class="sxs-lookup"><span data-stu-id="07d92-120">**Version #**: Specifies which version of the selected SKU to use.</span></span>

   ![Válassza ki a virtuálisgép-lemezkép ablak][CR03]

5. <span data-ttu-id="07d92-122">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="07d92-122">Click **Next**.</span></span> 

6. <span data-ttu-id="07d92-123">Az a **virtuális gép alapbeállítások** ablak, írja be a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="07d92-123">In the **Virtual Machine Basic Settings** window, enter the following information:</span></span>

   * <span data-ttu-id="07d92-124">**Virtuális gép neve**: Adja meg a nevet az új virtuális gépet, amely kizárólag betűvel kezdődhet, és csak betűket, számokat és kötőjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="07d92-124">**Virtual machine name**: Specifies the name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="07d92-125">**Méret**: Adja meg a magok száma és a virtuális gép lefoglalni memóriát.</span><span class="sxs-lookup"><span data-stu-id="07d92-125">**Size**: Specifies the number of cores and memory to allocate for your virtual machine.</span></span>

   * <span data-ttu-id="07d92-126">**Felhasználónév**: Adja meg a rendszergazdai fiók kezeléséhez a virtuális gép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="07d92-126">**User name**: Specifies the administrator account to create for managing your virtual machine.</span></span>

   * <span data-ttu-id="07d92-127">**Jelszó** és **megerősítése**: a rendszergazdai fiók jelszavát adja meg.</span><span class="sxs-lookup"><span data-stu-id="07d92-127">**Password** and **Confirm**: Specifies the password for your administrator account.</span></span>

   ![A virtuális gép alapbeállítások ablak][CR04]

7. <span data-ttu-id="07d92-129">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="07d92-129">Click **Next**.</span></span> 

8. <span data-ttu-id="07d92-130">Az a **kapcsolódó erőforrások** ablak, írja be a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="07d92-130">In the **Associated Resources** window, enter the following information:</span></span>

   * <span data-ttu-id="07d92-131">**Erőforráscsoport**: Adja meg a virtuális géphez tartozó erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="07d92-131">**Resource group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="07d92-132">A következő lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="07d92-132">Select one of the following options:</span></span>
      * <span data-ttu-id="07d92-133">**Új**: megadhatja, hogy kívánja-e egy új erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="07d92-133">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="07d92-134">**Meglévő**: meghatározza, hogy szeretné-e az erőforráscsoportok az Azure-fiókjával társított listájáról.</span><span class="sxs-lookup"><span data-stu-id="07d92-134">**Use existing**: Specifies that you want to select from a list of resource groups that are associated with your Azure account.</span></span>

       ![A kapcsolódó erőforrások ablak][CR07]

   * <span data-ttu-id="07d92-136">**A tárfiók**: Adja meg a tárfiók a virtuális gép tárolására használandó.</span><span class="sxs-lookup"><span data-stu-id="07d92-136">**Storage account**: Specifies the storage account to use for storing your virtual machine.</span></span> <span data-ttu-id="07d92-137">Válasszon egy meglévő tárfiókot használ, vagy hozzon létre egy új fiókot.</span><span class="sxs-lookup"><span data-stu-id="07d92-137">You can choose an existing storage account or create a new account.</span></span> <span data-ttu-id="07d92-138">Ha úgy dönt, **hozzon létre új**, a következő párbeszédpanel jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="07d92-138">If you choose **Create New**, the following dialog box appears:</span></span>

      ![A Storage-fiók létrehozása párbeszédpanel][CR05]

   * <span data-ttu-id="07d92-140">**Virtuális hálózati** és **alhálózati**: Adja meg a virtuális hálózat és az alhálózatot, amely a virtuális gép csatlakozni fog.</span><span class="sxs-lookup"><span data-stu-id="07d92-140">**Virtual Network** and **Subnet**: Specifies the virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="07d92-141">Használhat egy meglévő hálózati és alhálózati, vagy létrehozhat egy új hálózati és alhálózati.</span><span class="sxs-lookup"><span data-stu-id="07d92-141">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="07d92-142">Ha **hozzon létre új**, a következő párbeszédpanel jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="07d92-142">If you select **Create new**, the following dialog box appears:</span></span>

      ![A virtuális hálózat létrehozása párbeszédpanel][CR06]

   * <span data-ttu-id="07d92-144">**Nyilvános IP-cím**: egy külső hálózat irányába néző IP-cím a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="07d92-144">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="07d92-145">Ha szeretné, hozzon létre egy új IP-címet, vagy ha a virtuális gép nem fog működni a nyilvános IP-cím, választhat **(nincs)**.</span><span class="sxs-lookup"><span data-stu-id="07d92-145">You can choose to create a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span> 

   * <span data-ttu-id="07d92-146">**Hálózati biztonsági csoport**: Adja meg a virtuális gép egy választható hálózati tűzfal.</span><span class="sxs-lookup"><span data-stu-id="07d92-146">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="07d92-147">Kiválaszthatja, hogy egy meglévő tűzfal, vagy ha a virtuális gép nem használja a hálózati tűzfal, akkor választhat **(nincs)**.</span><span class="sxs-lookup"><span data-stu-id="07d92-147">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span> 

   * <span data-ttu-id="07d92-148">**A rendelkezésre állási csoport**: határozza meg, opcionális rendelkezésre állási készlet, hogy a virtuális gép tartozhatnak.</span><span class="sxs-lookup"><span data-stu-id="07d92-148">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="07d92-149">Válassza ki a rendelkezésre állási készlet, létrehozhat egy új rendelkezésre állási csoport vagy, ha a virtuális gép nem fog tartozni egy rendelkezésre állási csoportot, válassza ki a **(nincs)**.</span><span class="sxs-lookup"><span data-stu-id="07d92-149">You can select an existing availability set, create a new availability set or, if your virtual machine will not belong to an availability set, select **(None)**.</span></span>

9. <span data-ttu-id="07d92-150">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="07d92-150">Click **Finish**.</span></span>  
    <span data-ttu-id="07d92-151">Az új virtuális gépet az Azure Explorer eszköz ablakban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="07d92-151">Your new virtual machine appears in the Azure Explorer tool window.</span></span> 

   ![Új virtuális gépet az Azure-kezelővel nézetben][CR08]

## <a name="restart-a-virtual-machine-in-intellij"></a><span data-ttu-id="07d92-153">Az intellij-t egy virtuális gép újraindítása</span><span class="sxs-lookup"><span data-stu-id="07d92-153">Restart a virtual machine in IntelliJ</span></span>

<span data-ttu-id="07d92-154">Az intellij-t az Azure-kezelővel használatával egy virtuális gép újraindításához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="07d92-154">To restart a virtual machine by using the Azure Explorer in IntelliJ, do the following:</span></span>

1. <span data-ttu-id="07d92-155">Az a **Azure Explorer** nézetben kattintson a jobb gombbal a virtuális gépet, és válassza ki **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="07d92-155">In the **Azure Explorer** view, right-click the virtual machine, and then select **Restart**.</span></span>

   ![A virtuális gép újraindítási parancsnak][RE01]

2. <span data-ttu-id="07d92-157">Kattintson a megerősítés ablakban **Igen**.</span><span class="sxs-lookup"><span data-stu-id="07d92-157">In the confirmation window, click **Yes**.</span></span> 

   ![A virtuális gép újraindítása ablak][RE02]

## <a name="shut-down-a-virtual-machine-in-intellij"></a><span data-ttu-id="07d92-159">Az intellij-t egy virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="07d92-159">Shut down a virtual machine in IntelliJ</span></span>

<span data-ttu-id="07d92-160">Az intellij-t az Azure-kezelővel használatával állítsa le a futó virtuális gépek, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="07d92-160">To shut down a running virtual machine by using the Azure Explorer in IntelliJ, do the following:</span></span>

1. <span data-ttu-id="07d92-161">Az a **Azure Explorer** nézetben kattintson a jobb gombbal a virtuális gépet, és válassza ki **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="07d92-161">In the **Azure Explorer** view, right-click the virtual machine, and then select **Shutdown**.</span></span>

   ![A virtuális gép leállítási parancs][SH01]

2. <span data-ttu-id="07d92-163">Kattintson a megerősítés ablakban **Igen**.</span><span class="sxs-lookup"><span data-stu-id="07d92-163">In the confirmation window, click **Yes**.</span></span> 

   ![A virtuális gép ablak Leállítás][SH02]

## <a name="delete-a-virtual-machine-in-intellij"></a><span data-ttu-id="07d92-165">Az intellij-t egy virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="07d92-165">Delete a virtual machine in IntelliJ</span></span>

<span data-ttu-id="07d92-166">Törölje a virtuális gép az intellij-t az Azure-kezelővel használatával, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="07d92-166">To delete a virtual machine by using the Azure Explorer in IntelliJ, do the following:</span></span>

1. <span data-ttu-id="07d92-167">Az a **Azure Explorer** nézetben kattintson a jobb gombbal a virtuális gépet, és válassza ki **törlése**.</span><span class="sxs-lookup"><span data-stu-id="07d92-167">In the **Azure Explorer** view, right-click the virtual machine, and then select **Delete**.</span></span>

   ![A virtuális gép Delete parancs esetében][DE01]

2. <span data-ttu-id="07d92-169">Kattintson a megerősítés ablakban **Igen**.</span><span class="sxs-lookup"><span data-stu-id="07d92-169">In the confirmation window, click **Yes**.</span></span> 

   ![A virtuális gép törlése ablak][DE02]

## <a name="next-steps"></a><span data-ttu-id="07d92-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="07d92-171">Next steps</span></span>
<span data-ttu-id="07d92-172">Az Azure virtuális gépek méretét és a díjszabással kapcsolatos további információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="07d92-172">For more information about Azure virtual-machine sizes and pricing, see the following resources:</span></span>

* <span data-ttu-id="07d92-173">Az Azure virtuális gépek méretét</span><span class="sxs-lookup"><span data-stu-id="07d92-173">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="07d92-174">[Az Azure-ban a Windows virtuális gépek méretei]</span><span class="sxs-lookup"><span data-stu-id="07d92-174">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="07d92-175">[Az Azure Linux virtuális gépek méretei]</span><span class="sxs-lookup"><span data-stu-id="07d92-175">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="07d92-176">Az Azure virtuális gépek – díjszabás</span><span class="sxs-lookup"><span data-stu-id="07d92-176">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="07d92-177">[Windows virtuális gépek – díjszabás]</span><span class="sxs-lookup"><span data-stu-id="07d92-177">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="07d92-178">[Linux virtuális gépek – díjszabás]</span><span class="sxs-lookup"><span data-stu-id="07d92-178">[Linux virtual-machine pricing]</span></span>

<span data-ttu-id="07d92-179">A Java IDEs az Azure eszközök gazdag kapcsolatos további információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="07d92-179">For more information about the Azure Toolkits for Java IDEs, see the following resources:</span></span>

* <span data-ttu-id="07d92-180">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="07d92-180">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="07d92-181">[What's new in Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="07d92-181">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="07d92-182">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="07d92-182">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="07d92-183">[Az Eclipse Azure eszköztára bejelentkezési utasítások]</span><span class="sxs-lookup"><span data-stu-id="07d92-183">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="07d92-184">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="07d92-184">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="07d92-185">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="07d92-185">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="07d92-186">[What's new in IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="07d92-186">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="07d92-187">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="07d92-187">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="07d92-188">[bejelentkezési utasításokat az Azure-eszközkészlet az IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="07d92-188">[Sign-in instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="07d92-189">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="07d92-189">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="07d92-190">Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="07d92-190">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="07d92-191">[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="07d92-191">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="07d92-192">[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="07d92-192">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="07d92-193">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="07d92-193">[Create a Hello World web app for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="07d92-194">[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="07d92-194">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="07d92-195">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="07d92-195">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="07d92-196">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="07d92-196">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="07d92-197">[Az Eclipse Azure eszköztára bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="07d92-197">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="07d92-198">[bejelentkezési utasításokat az Azure-eszközkészlet az IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="07d92-198">[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="07d92-199">[What's new in Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="07d92-199">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="07d92-200">[What's new in IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="07d92-200">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="07d92-201">[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="07d92-201">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="07d92-202">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="07d92-202">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

<span data-ttu-id="07d92-203">[Az Azure-ban a Windows virtuális gépek méretei]: /azure/virtual-machines/virtual-machines-windows-sizes</span><span class="sxs-lookup"><span data-stu-id="07d92-203">[Sizes for Windows virtual machines in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes</span></span>
<span data-ttu-id="07d92-204">[Az Azure Linux virtuális gépek méretei]: /azure/virtual-machines/virtual-machines-linux-sizes</span><span class="sxs-lookup"><span data-stu-id="07d92-204">[Sizes for Linux virtual machines in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes</span></span>
<span data-ttu-id="07d92-205">[Windows virtuális gépek – díjszabás]: /pricing/details/virtual-machines/windows/</span><span class="sxs-lookup"><span data-stu-id="07d92-205">[Windows virtual-machine pricing]: /pricing/details/virtual-machines/windows/</span></span>
<span data-ttu-id="07d92-206">[Linux virtuális gépek – díjszabás]: /pricing/details/virtual-machines/linux/</span><span class="sxs-lookup"><span data-stu-id="07d92-206">[Linux virtual-machine pricing]: /pricing/details/virtual-machines/linux/</span></span>


<!-- IMG List -->

[RE01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR08.png
