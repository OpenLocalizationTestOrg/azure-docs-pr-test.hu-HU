---
title: "Virtuális gépek felügyelni az eclipse-ben az Azure-kezelővel használatával |} Microsoft Docs"
description: "Útmutató az Azure virtuális gépek kezeléséhez az Azure-kezelővel az eclipse-ben."
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
ms.openlocfilehash: 9784e8af9c530078afee06f08a23403a44b0762f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-eclipse"></a><span data-ttu-id="93fd5-103">Virtuális gépek kezeléséhez az Azure-kezelővel az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="93fd5-103">Manage virtual machines by using the Azure Explorer for Eclipse</span></span>

<span data-ttu-id="93fd5-104">Az Azure-kezelővel, amely része az Azure-eszközkészlet az eclipse-ben, Java fejlesztők egy könnyen kezelhető megoldás biztosít a felhőszolgáltatóknak a virtuális gépek a Azure fiók belül az Eclipse integrált fejlesztési környezeti (IDE).</span><span class="sxs-lookup"><span data-stu-id="93fd5-104">The Azure Explorer, which is part of the Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside the Eclipse integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a><span data-ttu-id="93fd5-105">Virtuális gép létrehozása az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="93fd5-105">Create a virtual machine in Eclipse</span></span>

<span data-ttu-id="93fd5-106">Virtuális gép létrehozása az Azure Explorerrel, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="93fd5-106">To create a virtual machine by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="93fd5-107">Jelentkezzen be az Azure-fiók használatával a [Eclipse Azure eszköztára bejelentkezési utasítások].</span><span class="sxs-lookup"><span data-stu-id="93fd5-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for Eclipse].</span></span>

2. <span data-ttu-id="93fd5-108">Az a **Azure Explorer** megtekintéséhez bontsa ki a **Azure** csomópontot, kattintson a jobb gombbal **virtuális gépek**, és kattintson a **hozzon létre virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="93fd5-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span>

   <span data-ttu-id="93fd5-109">![A virtuális gép létrehozása parancs][CR01]</span><span class="sxs-lookup"><span data-stu-id="93fd5-109">![The Create VM command][CR01]</span></span>  
   <span data-ttu-id="93fd5-110">A **hozzon létre új virtuális gépet** varázsló megnyílik.</span><span class="sxs-lookup"><span data-stu-id="93fd5-110">The **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="93fd5-111">Az a **válasszon egy előfizetést** ablakban jelölje ki az előfizetését, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="93fd5-111">In the **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span>

   ![A Válasszon egy előfizetés ablak][CR02]

4. <span data-ttu-id="93fd5-113">Az a **válassza ki a virtuálisgép-lemezkép** ablak, írja be a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="93fd5-113">In the **Select a Virtual Machine Image** window, enter the following information:</span></span>

   * <span data-ttu-id="93fd5-114">**Hely**: határozza meg, ahol létrejön a virtuális gép (például *USA nyugati régiója*).</span><span class="sxs-lookup"><span data-stu-id="93fd5-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span>

   * <span data-ttu-id="93fd5-115">**A Publisher**: Adja meg a kiadó, hozott létre a lemezképet, amellyel a virtuális gép létrehozása (például *Microsoft*).</span><span class="sxs-lookup"><span data-stu-id="93fd5-115">**Publisher**: Specifies the publisher that created the image you'll use to create your virtual machine (for example, *Microsoft*).</span></span>

   * <span data-ttu-id="93fd5-116">**Ajánlat**: Adja meg a virtuális gép használata a kijelölt közzétételi ajánlat (például *JDK*).</span><span class="sxs-lookup"><span data-stu-id="93fd5-116">**Offer**: Specifies the virtual machine offering to use from the selected publisher (for example, *JDK*).</span></span>

   * <span data-ttu-id="93fd5-117">**SKU**: Adja meg a Raktározásiegység (SKU) a kijelölt ajánlat használata (például *JDK_8*).</span><span class="sxs-lookup"><span data-stu-id="93fd5-117">**Sku**: Specifies the stockkeeping unit (SKU) to use from the selected offering (for example, *JDK_8*).</span></span>

   * <span data-ttu-id="93fd5-118">**Verzió #**: Adja meg a kiválasztott Termékváltozat használandó verziójának.</span><span class="sxs-lookup"><span data-stu-id="93fd5-118">**Version #**: Specifies which version of the selected SKU to use.</span></span>

    ![Válassza ki a virtuálisgép-lemezkép ablak][CR03]

5. <span data-ttu-id="93fd5-120">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="93fd5-120">Click **Next**.</span></span>

6. <span data-ttu-id="93fd5-121">Az a **virtuális gép alapbeállítások** ablak, írja be a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="93fd5-121">In the **Virtual Machine Basic Settings** window, enter the following information:</span></span>

   * <span data-ttu-id="93fd5-122">**Virtuális gép neve**: Adja meg a nevet az új virtuális gépet, amely kizárólag betűvel kezdődhet, és csak betűket, számokat és kötőjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="93fd5-122">**Virtual Machine Name**: Specifies the name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="93fd5-123">**Méret**: Adja meg a magok száma és a virtuális gép lefoglalni memóriát.</span><span class="sxs-lookup"><span data-stu-id="93fd5-123">**Size**: Specifies the number of cores and memory to allocate for your virtual machine.</span></span>

   * <span data-ttu-id="93fd5-124">**Felhasználónév**: Adja meg a rendszergazdai fiók kezeléséhez a virtuális gép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="93fd5-124">**User name**: Specifies the administrator account to create for managing your virtual machine.</span></span>

   * <span data-ttu-id="93fd5-125">**Jelszó** és **megerősítése**: a rendszergazdai fiók jelszavát adja meg.</span><span class="sxs-lookup"><span data-stu-id="93fd5-125">**Password** and **Confirm**: Specifies the password for your administrator account.</span></span>

    ![A virtuális gép alapbeállítások ablak][CR04]

7. <span data-ttu-id="93fd5-127">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="93fd5-127">Click **Next**.</span></span>

8. <span data-ttu-id="93fd5-128">Az a **új Tárfiók létrehozása** ablak, írja be a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="93fd5-128">In the **Create New Storage Account** window, enter the following information:</span></span>

   * <span data-ttu-id="93fd5-129">**Erőforráscsoport**: Adja meg a virtuális géphez tartozó erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="93fd5-129">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="93fd5-130">A következő lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="93fd5-130">Select one of the following options:</span></span>
      * <span data-ttu-id="93fd5-131">**Új**: megadhatja, hogy kívánja-e egy új erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="93fd5-131">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="93fd5-132">**Meglévő**: meghatározza, hogy szeretné-e válasszon egy erőforráscsoportot, amely már társítva van az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="93fd5-132">**Use existing**: Specifies that you want to select a resource group that is already associated with your Azure account.</span></span>

      ![Az új Tárfiók létrehozása párbeszédpanel][CR05]

   * <span data-ttu-id="93fd5-134">**A tárfiók**: Adja meg a tárfiók a virtuális gép tárolására használandó.</span><span class="sxs-lookup"><span data-stu-id="93fd5-134">**Storage account**: Specifies the storage account to use for storing your virtual machine.</span></span> <span data-ttu-id="93fd5-135">Meglévő tárfiók használata, vagy hozzon létre egy új fiókot.</span><span class="sxs-lookup"><span data-stu-id="93fd5-135">You can use an existing storage account or create a new account.</span></span>

   * <span data-ttu-id="93fd5-136">**Virtuális hálózati** és **alhálózati**: Adja meg a virtuális hálózat és az alhálózatot, amely a virtuális gép csatlakozni fog.</span><span class="sxs-lookup"><span data-stu-id="93fd5-136">**Virtual Network** and **Subnet**: Specifies the virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="93fd5-137">Használhat egy meglévő hálózati és alhálózati, vagy létrehozhat egy új hálózati és alhálózati.</span><span class="sxs-lookup"><span data-stu-id="93fd5-137">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="93fd5-138">Ha **hozzon létre új**, a következő párbeszédpanel jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="93fd5-138">If you select **Create new**, the following dialog box is displayed:</span></span>

      ![Az új virtuális hálózat létrehozása párbeszédpanel][CR06]

9. <span data-ttu-id="93fd5-140">Az a **kapcsolódó erőforrások** ablak, írja be a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="93fd5-140">In the **Associated Resources** window, enter the following information:</span></span>

   * <span data-ttu-id="93fd5-141">**Nyilvános IP-cím**: egy külső hálózat irányába néző IP-cím a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="93fd5-141">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="93fd5-142">Ha szeretné, hozzon létre egy új IP-címet, vagy ha a virtuális gép nem fog működni a nyilvános IP-cím, választhat **(nincs)**.</span><span class="sxs-lookup"><span data-stu-id="93fd5-142">You can choose to create a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span>

   * <span data-ttu-id="93fd5-143">**Hálózati biztonsági csoport**: Adja meg a virtuális gép egy választható hálózati tűzfal.</span><span class="sxs-lookup"><span data-stu-id="93fd5-143">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="93fd5-144">Kiválaszthatja, hogy egy meglévő tűzfal, vagy ha a virtuális gép nem használja a hálózati tűzfal, akkor választhat **(nincs)**.</span><span class="sxs-lookup"><span data-stu-id="93fd5-144">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span>

   * <span data-ttu-id="93fd5-145">**A rendelkezésre állási csoport**: határozza meg, opcionális rendelkezésre állási készlet, hogy a virtuális gép tartozhatnak.</span><span class="sxs-lookup"><span data-stu-id="93fd5-145">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="93fd5-146">Válasszon egy meglévő rendelkezésre állási csoportot, vagy hozzon létre egy új rendelkezésre állási készletet, vagy ha a virtuális gép nem fog tartozni egy rendelkezésre állási csoportot, válassza **(nincs)**.</span><span class="sxs-lookup"><span data-stu-id="93fd5-146">You can select an existing availability set or create a new availability set or, if your virtual machine will not belong to an availability set, you can select **(None)**.</span></span>

   ![A kapcsolódó erőforrások ablak][CR07]

9. <span data-ttu-id="93fd5-148">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="93fd5-148">Click **Finish**.</span></span>  
  <span data-ttu-id="93fd5-149">Az új virtuális gépet az Azure Explorer eszköz ablakban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="93fd5-149">Your new virtual machine is displayed in the Azure Explorer tool window.</span></span>

   ![Új virtuális gép][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a><span data-ttu-id="93fd5-151">A virtuális gép újraindításához az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="93fd5-151">Restart a virtual machine in Eclipse</span></span>

<span data-ttu-id="93fd5-152">Az eclipse-ben az Azure-kezelővel használatával egy virtuális gép újraindításához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="93fd5-152">To restart a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="93fd5-153">Az a **Azure Explorer** nézetben kattintson a jobb gombbal a virtuális gépet, és válassza ki **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="93fd5-153">In the **Azure Explorer** view, right-click the virtual machine, and then select **Restart**.</span></span>

   ![A virtuális gép újraindítási parancsnak][RE01]

2. <span data-ttu-id="93fd5-155">Kattintson a megerősítés ablakban **Igen**.</span><span class="sxs-lookup"><span data-stu-id="93fd5-155">In the confirmation window, click **Yes**.</span></span>

   ![Az újraindítás ablak][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a><span data-ttu-id="93fd5-157">Az eclipse-ben a virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="93fd5-157">Shut down a virtual machine in Eclipse</span></span>

<span data-ttu-id="93fd5-158">Az eclipse-ben az Azure-kezelővel használatával állítsa le a futó virtuális gépek, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="93fd5-158">To shut down a running virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="93fd5-159">Az a **Azure Explorer** nézetben kattintson a jobb gombbal a virtuális gépet, és válassza ki **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="93fd5-159">In the **Azure Explorer** view, right-click the virtual machine, and then select **Shutdown**.</span></span>

   ![A virtuális gép leállítási parancs][SH01]

2. <span data-ttu-id="93fd5-161">Kattintson a megerősítés ablakban **Igen**.</span><span class="sxs-lookup"><span data-stu-id="93fd5-161">In the confirmation window, click **Yes**.</span></span>

   ![A virtuális gép leállítási ablak][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a><span data-ttu-id="93fd5-163">Az eclipse-ben a virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="93fd5-163">Delete a virtual machine in Eclipse</span></span>

<span data-ttu-id="93fd5-164">Törölje a virtuális gép az eclipse-ben az Azure-kezelővel használatával, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="93fd5-164">To delete a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="93fd5-165">Az a **Azure Explorer** nézetben kattintson a jobb gombbal a virtuális gépet, és válassza ki **törlése**.</span><span class="sxs-lookup"><span data-stu-id="93fd5-165">In the **Azure Explorer** view, right-click the virtual machine, and then select **Delete**.</span></span>

   ![A virtuális gép Delete parancs esetében][DE01]

2. <span data-ttu-id="93fd5-167">Kattintson a megerősítés ablakban **Igen**.</span><span class="sxs-lookup"><span data-stu-id="93fd5-167">In the confirmation window, click **Yes**.</span></span>

   ![A virtuális gép törlése ablak][DE02]

## <a name="next-steps"></a><span data-ttu-id="93fd5-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="93fd5-169">Next steps</span></span>
<span data-ttu-id="93fd5-170">Az Azure virtuális gépek méretét és a díjszabással kapcsolatos további információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="93fd5-170">For more information about Azure virtual-machine sizes and pricing, see the following resources:</span></span>

* <span data-ttu-id="93fd5-171">Az Azure virtuális gépek méretét</span><span class="sxs-lookup"><span data-stu-id="93fd5-171">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="93fd5-172">[Az Azure-ban a Windows virtuális gépek méretei]</span><span class="sxs-lookup"><span data-stu-id="93fd5-172">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="93fd5-173">[Az Azure Linux virtuális gépek méretei]</span><span class="sxs-lookup"><span data-stu-id="93fd5-173">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="93fd5-174">Az Azure virtuális gépek – díjszabás</span><span class="sxs-lookup"><span data-stu-id="93fd5-174">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="93fd5-175">[Windows virtuális gépek – díjszabás]</span><span class="sxs-lookup"><span data-stu-id="93fd5-175">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="93fd5-176">[Linux virtuális gépek – díjszabás]</span><span class="sxs-lookup"><span data-stu-id="93fd5-176">[Linux virtual-machine pricing]</span></span>

<span data-ttu-id="93fd5-177">A Java IDEs az Azure eszközök gazdag kapcsolatos további információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="93fd5-177">For more information about the Azure Toolkits for Java IDEs, see the following resources:</span></span>

* <span data-ttu-id="93fd5-178">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="93fd5-178">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="93fd5-179">[What's new in Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="93fd5-179">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="93fd5-180">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="93fd5-180">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="93fd5-181">[Eclipse Azure eszköztára bejelentkezési utasítások]</span><span class="sxs-lookup"><span data-stu-id="93fd5-181">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="93fd5-182">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="93fd5-182">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="93fd5-183">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="93fd5-183">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="93fd5-184">[What's new in IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="93fd5-184">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="93fd5-185">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="93fd5-185">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="93fd5-186">[Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t]</span><span class="sxs-lookup"><span data-stu-id="93fd5-186">[Sign-in instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="93fd5-187">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="93fd5-187">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="93fd5-188">Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="93fd5-188">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="93fd5-189">[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="93fd5-189">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="93fd5-190">[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="93fd5-190">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="93fd5-191">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="93fd5-191">[Create a Hello World web app for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="93fd5-192">[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="93fd5-192">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="93fd5-193">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="93fd5-193">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="93fd5-194">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="93fd5-194">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="93fd5-195">[Eclipse Azure eszköztára bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="93fd5-195">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="93fd5-196">[Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="93fd5-196">[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="93fd5-197">[What's new in Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="93fd5-197">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="93fd5-198">[What's new in IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="93fd5-198">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="93fd5-199">[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="93fd5-199">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="93fd5-200">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="93fd5-200">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

<span data-ttu-id="93fd5-201">[Az Azure-ban a Windows virtuális gépek méretei]: /azure/virtual-machines/virtual-machines-windows-sizes</span><span class="sxs-lookup"><span data-stu-id="93fd5-201">[Sizes for Windows virtual machines in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes</span></span>
<span data-ttu-id="93fd5-202">[Az Azure Linux virtuális gépek méretei]: /azure/virtual-machines/virtual-machines-linux-sizes</span><span class="sxs-lookup"><span data-stu-id="93fd5-202">[Sizes for Linux virtual machines in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes</span></span>
<span data-ttu-id="93fd5-203">[Windows virtuális gépek – díjszabás]: /pricing/details/virtual-machines/windows/</span><span class="sxs-lookup"><span data-stu-id="93fd5-203">[Windows virtual-machine pricing]: /pricing/details/virtual-machines/windows/</span></span>
<span data-ttu-id="93fd5-204">[Linux virtuális gépek – díjszabás]: /pricing/details/virtual-machines/linux/</span><span class="sxs-lookup"><span data-stu-id="93fd5-204">[Linux virtual-machine pricing]: /pricing/details/virtual-machines/linux/</span></span>

<!-- IMG List -->

[RE01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR08.png
