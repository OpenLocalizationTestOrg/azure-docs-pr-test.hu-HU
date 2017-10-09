---
title: "IntelliJ aaaManage virtuális gépek használatával hello Azure-kezelővel |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage az Azure virtuális gépek használatával hello Azure-kezelőjét az intellij-t."
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
ms.openlocfilehash: a73dd4f73b311dd3413f6712e3b76c36ee464de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machines-by-using-hello-azure-explorer-for-intellij"></a><span data-ttu-id="965c8-103">Virtuális gépek felügyelni az IntelliJ hello Azure Explorer használatával</span><span class="sxs-lookup"><span data-stu-id="965c8-103">Manage virtual machines by using hello Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="965c8-104">hello Azure Explorer, az IntelliJ Azure eszköztára hello részét képező Java fejlesztők egy könnyen kezelhető megoldás biztosít a felhőszolgáltatóknak a virtuális gépek az Azure-fiók a hello IntelliJ integrált fejlesztési környezeti (IDE) belül.</span><span class="sxs-lookup"><span data-stu-id="965c8-104">hello Azure Explorer, which is part of hello Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside hello IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-intellij"></a><span data-ttu-id="965c8-105">Virtuális gép létrehozása az intellij-t</span><span class="sxs-lookup"><span data-stu-id="965c8-105">Create a virtual machine in IntelliJ</span></span>

<span data-ttu-id="965c8-106">a virtuális gép hello Azure Explorer használatával toocreate hello a következő:</span><span class="sxs-lookup"><span data-stu-id="965c8-106">toocreate a virtual machine by using hello Azure Explorer, do hello following:</span></span> 

1. <span data-ttu-id="965c8-107">Jelentkezzen be Azure-fiók tooyour hello lépéseket a hello [bejelentkezési utasításokat hello Azure eszközkészlet az IntelliJ] cikk.</span><span class="sxs-lookup"><span data-stu-id="965c8-107">Sign in tooyour Azure account by using hello steps in hello [Sign-in instructions for hello Azure Toolkit for IntelliJ] article.</span></span>

2. <span data-ttu-id="965c8-108">A hello **Azure Explorer** megtekintéséhez bontsa ki a hello **Azure** csomópontot, kattintson a jobb gombbal **virtuális gépek**, és kattintson a **hozzon létre virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="965c8-108">In hello **Azure Explorer** view, expand hello **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span> 

   <span data-ttu-id="965c8-109">![Hozzon létre virtuális gép parancs hello][CR01]</span><span class="sxs-lookup"><span data-stu-id="965c8-109">![hello Create VM command][CR01]</span></span>  
    <span data-ttu-id="965c8-110">Hello **hozzon létre új virtuális gépet** varázsló megnyílik.</span><span class="sxs-lookup"><span data-stu-id="965c8-110">hello **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="965c8-111">A hello **válasszon egy előfizetést** ablakban jelölje ki az előfizetését, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="965c8-111">In hello **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span> 

   ![Válasszon egy előfizetés ablak hello][CR02]

4. <span data-ttu-id="965c8-113">A hello **válassza ki a virtuálisgép-lemezkép** ablak, írja be a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="965c8-113">In hello **Select a Virtual Machine Image** window, enter hello following information:</span></span>

   * <span data-ttu-id="965c8-114">**Hely**: határozza meg, ahol létrejön a virtuális gép (például *USA nyugati régiója*).</span><span class="sxs-lookup"><span data-stu-id="965c8-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span> 

   * <span data-ttu-id="965c8-115">**Kép ajánlott**: meghatározza, hogy fog választhat egy lemezképet egy gyakran használt képek rövidített listájának.</span><span class="sxs-lookup"><span data-stu-id="965c8-115">**Recommended image**: Specifies that you will choose an image from an abbreviated list of commonly used images.</span></span>

   * <span data-ttu-id="965c8-116">**Kép: egyéni**: meghatározza a fog választani egy egyéni lemezképet, adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="965c8-116">**Custom image**: Specifies that you will choose a custom image by providing hello following information:</span></span>

      * <span data-ttu-id="965c8-117">**A Publisher**: Adja meg a virtuális géphez használni kívánt kép hello létrehozott hello közzétevő (például *Microsoft*).</span><span class="sxs-lookup"><span data-stu-id="965c8-117">**Publisher**: Specifies hello publisher that created hello image that you will use for your virtual machine (for example, *Microsoft*).</span></span>

      * <span data-ttu-id="965c8-118">**Ajánlat**: hello virtuális gép toouse ajánlat hello kijelölt közzétételi határozza meg (például *JDK*).</span><span class="sxs-lookup"><span data-stu-id="965c8-118">**Offer**: Specifies hello virtual machine offering toouse from hello selected publisher (for example, *JDK*).</span></span>

      * <span data-ttu-id="965c8-119">**SKU**: hello raktározási egységhez toouse hello kijelölt ajánlat az határozza meg (például *JDK_8*).</span><span class="sxs-lookup"><span data-stu-id="965c8-119">**Sku**: Specifies hello stockkeeping unit (SKU) toouse from hello selected offering (for example, *JDK_8*).</span></span>

      * <span data-ttu-id="965c8-120">**Verzió #**: Adja meg a kiválasztott hello verziójának SKU toouse.</span><span class="sxs-lookup"><span data-stu-id="965c8-120">**Version #**: Specifies which version of hello selected SKU toouse.</span></span>

   ![hello válassza ki a virtuálisgép-lemezkép ablak][CR03]

5. <span data-ttu-id="965c8-122">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="965c8-122">Click **Next**.</span></span> 

6. <span data-ttu-id="965c8-123">A hello **virtuális gép alapbeállítások** ablak, írja be a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="965c8-123">In hello **Virtual Machine Basic Settings** window, enter hello following information:</span></span>

   * <span data-ttu-id="965c8-124">**Virtuális gép neve**: hello neve, az új virtuális gépen, amely kizárólag betűvel kezdődhet, és csak betűket, számokat és kötőjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="965c8-124">**Virtual machine name**: Specifies hello name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="965c8-125">**Méret**: hello magok száma és a virtuális gép memória tooallocate határozza meg.</span><span class="sxs-lookup"><span data-stu-id="965c8-125">**Size**: Specifies hello number of cores and memory tooallocate for your virtual machine.</span></span>

   * <span data-ttu-id="965c8-126">**Felhasználónév**: hello rendszergazdai fiók toocreate kezelését a virtuális gép határozza meg.</span><span class="sxs-lookup"><span data-stu-id="965c8-126">**User name**: Specifies hello administrator account toocreate for managing your virtual machine.</span></span>

   * <span data-ttu-id="965c8-127">**Jelszó** és **megerősítése**: az Ön rendszergazdai fiókjának hello jelszót ad meg.</span><span class="sxs-lookup"><span data-stu-id="965c8-127">**Password** and **Confirm**: Specifies hello password for your administrator account.</span></span>

   ![hello virtuális gép alapbeállítások ablak][CR04]

7. <span data-ttu-id="965c8-129">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="965c8-129">Click **Next**.</span></span> 

8. <span data-ttu-id="965c8-130">A hello **kapcsolódó erőforrások** ablak, írja be a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="965c8-130">In hello **Associated Resources** window, enter hello following information:</span></span>

   * <span data-ttu-id="965c8-131">**Erőforráscsoport**: hello erőforrás csoportot a virtuális gép határozza meg.</span><span class="sxs-lookup"><span data-stu-id="965c8-131">**Resource group**: Specifies hello resource group for your virtual machine.</span></span> <span data-ttu-id="965c8-132">Válasszon egyet az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="965c8-132">Select one of hello following options:</span></span>
      * <span data-ttu-id="965c8-133">**Új**: megadhatja, hogy kívánja-e toocreate egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="965c8-133">**Create new**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="965c8-134">**Meglévő**: megadhatja, hogy kívánja-e tooselect az erőforráscsoportok az Azure-fiókjával társított listájából.</span><span class="sxs-lookup"><span data-stu-id="965c8-134">**Use existing**: Specifies that you want tooselect from a list of resource groups that are associated with your Azure account.</span></span>

       ![hello kapcsolódó erőforrások ablak][CR07]

   * <span data-ttu-id="965c8-136">**A tárfiók**: hello tárolási fiók toouse a virtuális gép tárolásához adja meg.</span><span class="sxs-lookup"><span data-stu-id="965c8-136">**Storage account**: Specifies hello storage account toouse for storing your virtual machine.</span></span> <span data-ttu-id="965c8-137">Válasszon egy meglévő tárfiókot használ, vagy hozzon létre egy új fiókot.</span><span class="sxs-lookup"><span data-stu-id="965c8-137">You can choose an existing storage account or create a new account.</span></span> <span data-ttu-id="965c8-138">Ha úgy dönt, **hozzon létre új**, hello a következő párbeszédpanel jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="965c8-138">If you choose **Create New**, hello following dialog box appears:</span></span>

      ![hello Storage-fiók létrehozása párbeszédpanel][CR05]

   * <span data-ttu-id="965c8-140">**Virtuális hálózati** és **alhálózati**: hello virtuális hálózat és az alhálózatot, amely a virtuális gép csatlakozni fog.</span><span class="sxs-lookup"><span data-stu-id="965c8-140">**Virtual Network** and **Subnet**: Specifies hello virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="965c8-141">Használhat egy meglévő hálózati és alhálózati, vagy létrehozhat egy új hálózati és alhálózati.</span><span class="sxs-lookup"><span data-stu-id="965c8-141">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="965c8-142">Ha **hozzon létre új**, hello a következő párbeszédpanel jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="965c8-142">If you select **Create new**, hello following dialog box appears:</span></span>

      ![hello virtuális hálózat létrehozása párbeszédpanel][CR06]

   * <span data-ttu-id="965c8-144">**Nyilvános IP-cím**: egy külső hálózat irányába néző IP-cím a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="965c8-144">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="965c8-145">Választhat toocreate egy új IP-címet, vagy ha a virtuális gép nem fog működni a nyilvános IP-cím, választhat **(nincs)**.</span><span class="sxs-lookup"><span data-stu-id="965c8-145">You can choose toocreate a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span> 

   * <span data-ttu-id="965c8-146">**Hálózati biztonsági csoport**: Adja meg a virtuális gép egy választható hálózati tűzfal.</span><span class="sxs-lookup"><span data-stu-id="965c8-146">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="965c8-147">Kiválaszthatja, hogy egy meglévő tűzfal, vagy ha a virtuális gép nem használja a hálózati tűzfal, akkor választhat **(nincs)**.</span><span class="sxs-lookup"><span data-stu-id="965c8-147">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span> 

   * <span data-ttu-id="965c8-148">**A rendelkezésre állási csoport**: határozza meg, opcionális rendelkezésre állási készlet, hogy a virtuális gép tartozhatnak.</span><span class="sxs-lookup"><span data-stu-id="965c8-148">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="965c8-149">Válassza ki a rendelkezésre állási készlet, létrehozhat egy új rendelkezésre állási csoport vagy, ha a virtuális gép nem fog tartozni tooan rendelkezésre állási csoportot, válassza ki a **(nincs)**.</span><span class="sxs-lookup"><span data-stu-id="965c8-149">You can select an existing availability set, create a new availability set or, if your virtual machine will not belong tooan availability set, select **(None)**.</span></span>

9. <span data-ttu-id="965c8-150">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="965c8-150">Click **Finish**.</span></span>  
    <span data-ttu-id="965c8-151">Az új virtuális gép hello Azure eszköz ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="965c8-151">Your new virtual machine appears in hello Azure Explorer tool window.</span></span> 

   ![Új virtuális gép hello Azure Explorer megtekintése][CR08]

## <a name="restart-a-virtual-machine-in-intellij"></a><span data-ttu-id="965c8-153">Az intellij-t egy virtuális gép újraindítása</span><span class="sxs-lookup"><span data-stu-id="965c8-153">Restart a virtual machine in IntelliJ</span></span>

<span data-ttu-id="965c8-154">az intellij-t, hello Azure Explorer használatával egy virtuális gép toorestart hello a következő:</span><span class="sxs-lookup"><span data-stu-id="965c8-154">toorestart a virtual machine by using hello Azure Explorer in IntelliJ, do hello following:</span></span>

1. <span data-ttu-id="965c8-155">A hello **Azure Explorer** nézetben, majd válassza ki és kattintson a jobb gombbal a virtuális gép hello **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="965c8-155">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Restart**.</span></span>

   ![virtuális gép újraindítási parancsnak hello][RE01]

2. <span data-ttu-id="965c8-157">Hello ablak, kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="965c8-157">In hello confirmation window, click **Yes**.</span></span> 

   ![hello indítsa újra a virtuális gép ablak][RE02]

## <a name="shut-down-a-virtual-machine-in-intellij"></a><span data-ttu-id="965c8-159">Az intellij-t egy virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="965c8-159">Shut down a virtual machine in IntelliJ</span></span>

<span data-ttu-id="965c8-160">le a futó virtuális gépek IntelliJ, hello Azure Explorer használatával tooshut hello a következő:</span><span class="sxs-lookup"><span data-stu-id="965c8-160">tooshut down a running virtual machine by using hello Azure Explorer in IntelliJ, do hello following:</span></span>

1. <span data-ttu-id="965c8-161">A hello **Azure Explorer** nézetben, majd válassza ki és kattintson a jobb gombbal a virtuális gép hello **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="965c8-161">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Shutdown**.</span></span>

   ![hello virtuális gép leállítási parancs][SH01]

2. <span data-ttu-id="965c8-163">Hello ablak, kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="965c8-163">In hello confirmation window, click **Yes**.</span></span> 

   ![Állítsa le a virtuális gép ablak hello][SH02]

## <a name="delete-a-virtual-machine-in-intellij"></a><span data-ttu-id="965c8-165">Az intellij-t egy virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="965c8-165">Delete a virtual machine in IntelliJ</span></span>

<span data-ttu-id="965c8-166">az intellij-t, hello Azure Explorer használatával egy virtuális gép toodelete hello a következő:</span><span class="sxs-lookup"><span data-stu-id="965c8-166">toodelete a virtual machine by using hello Azure Explorer in IntelliJ, do hello following:</span></span>

1. <span data-ttu-id="965c8-167">A hello **Azure Explorer** nézetben, majd válassza ki és kattintson a jobb gombbal a virtuális gép hello **törlése**.</span><span class="sxs-lookup"><span data-stu-id="965c8-167">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Delete**.</span></span>

   ![hello virtuális gép Delete parancs esetében][DE01]

2. <span data-ttu-id="965c8-169">Hello ablak, kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="965c8-169">In hello confirmation window, click **Yes**.</span></span> 

   ![hello törlése a virtuális gép ablak][DE02]

## <a name="next-steps"></a><span data-ttu-id="965c8-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="965c8-171">Next steps</span></span>
<span data-ttu-id="965c8-172">Az Azure virtuális gépek méretét és a díjszabással kapcsolatos további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="965c8-172">For more information about Azure virtual-machine sizes and pricing, see hello following resources:</span></span>

* <span data-ttu-id="965c8-173">Az Azure virtuális gépek méretét</span><span class="sxs-lookup"><span data-stu-id="965c8-173">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="965c8-174">[Az Azure-ban a Windows virtuális gépek méretei]</span><span class="sxs-lookup"><span data-stu-id="965c8-174">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="965c8-175">[Az Azure Linux virtuális gépek méretei]</span><span class="sxs-lookup"><span data-stu-id="965c8-175">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="965c8-176">Az Azure virtuális gépek – díjszabás</span><span class="sxs-lookup"><span data-stu-id="965c8-176">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="965c8-177">[Windows virtuális gépek – díjszabás]</span><span class="sxs-lookup"><span data-stu-id="965c8-177">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="965c8-178">[Linux virtuális gépek – díjszabás]</span><span class="sxs-lookup"><span data-stu-id="965c8-178">[Linux virtual-machine pricing]</span></span>

<span data-ttu-id="965c8-179">A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="965c8-179">For more information about hello Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="965c8-180">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="965c8-180">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="965c8-181">[What's new in hello Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="965c8-181">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="965c8-182">[Hello Azure eszköztára Eclipse telepítése]</span><span class="sxs-lookup"><span data-stu-id="965c8-182">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="965c8-183">[Hello Azure eszköztára Eclipse bejelentkezési utasítások]</span><span class="sxs-lookup"><span data-stu-id="965c8-183">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="965c8-184">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="965c8-184">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="965c8-185">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="965c8-185">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="965c8-186">[What's new in hello IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="965c8-186">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="965c8-187">[Az IntelliJ hello Azure eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="965c8-187">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="965c8-188">[bejelentkezési utasításokat hello Azure eszközkészlet az IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="965c8-188">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="965c8-189">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="965c8-189">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="965c8-190">Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="965c8-190">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Hello Azure eszköztára Eclipse telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ hello Azure eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Hello Azure eszköztára Eclipse bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[bejelentkezési utasításokat hello Azure eszközkészlet az IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's new in hello Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md
[What's new in hello IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/

[Az Azure-ban a Windows virtuális gépek méretei]: /azure/virtual-machines/virtual-machines-windows-sizes
[Az Azure Linux virtuális gépek méretei]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows virtuális gépek – díjszabás]: /pricing/details/virtual-machines/windows/
[Linux virtuális gépek – díjszabás]: /pricing/details/virtual-machines/linux/


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
