---
title: "aaaManage virtuális gépek Azure-kezelőjét az Eclipse hello |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage az Azure virtuális gépek használatával hello Azure-kezelőjét az eclipse-ben."
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
ms.openlocfilehash: aa83ec7546a0a8514842723b51ce8a5af81f98f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machines-by-using-hello-azure-explorer-for-eclipse"></a><span data-ttu-id="6367f-103">Virtuális gépek felügyelni Eclipse hello Azure Explorer használatával</span><span class="sxs-lookup"><span data-stu-id="6367f-103">Manage virtual machines by using hello Azure Explorer for Eclipse</span></span>

<span data-ttu-id="6367f-104">hello Azure-kezelővel, amely hello Azure eszköztára Eclipse része, Java-fejlesztők számára, egy könnyen használható megoldást biztosít a felhőszolgáltatóknak a virtuális gépeket az Azure-fiók a hello Eclipse integrált fejlesztési környezeti (IDE) belül.</span><span class="sxs-lookup"><span data-stu-id="6367f-104">hello Azure Explorer, which is part of hello Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside hello Eclipse integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a><span data-ttu-id="6367f-105">Virtuális gép létrehozása az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="6367f-105">Create a virtual machine in Eclipse</span></span>

<span data-ttu-id="6367f-106">a virtuális gép hello Azure Explorer használatával toocreate hello a következő:</span><span class="sxs-lookup"><span data-stu-id="6367f-106">toocreate a virtual machine by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="6367f-107">Jelentkezzen be Azure-fiók tooyour hello [hello Azure eszköztára Eclipse bejelentkezési utasítások].</span><span class="sxs-lookup"><span data-stu-id="6367f-107">Sign in tooyour Azure account by using hello [Sign-in instructions for hello Azure Toolkit for Eclipse].</span></span>

2. <span data-ttu-id="6367f-108">A hello **Azure Explorer** megtekintéséhez bontsa ki a hello **Azure** csomópontot, kattintson a jobb gombbal **virtuális gépek**, és kattintson a **hozzon létre virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="6367f-108">In hello **Azure Explorer** view, expand hello **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span>

   <span data-ttu-id="6367f-109">![Hozzon létre virtuális gép parancs hello][CR01]</span><span class="sxs-lookup"><span data-stu-id="6367f-109">![hello Create VM command][CR01]</span></span>  
   <span data-ttu-id="6367f-110">Hello **hozzon létre új virtuális gépet** varázsló megnyílik.</span><span class="sxs-lookup"><span data-stu-id="6367f-110">hello **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="6367f-111">A hello **válasszon egy előfizetést** ablakban jelölje ki az előfizetését, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="6367f-111">In hello **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span>

   ![Válasszon egy előfizetés ablak hello][CR02]

4. <span data-ttu-id="6367f-113">A hello **válassza ki a virtuálisgép-lemezkép** ablak, írja be a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="6367f-113">In hello **Select a Virtual Machine Image** window, enter hello following information:</span></span>

   * <span data-ttu-id="6367f-114">**Hely**: határozza meg, ahol létrejön a virtuális gép (például *USA nyugati régiója*).</span><span class="sxs-lookup"><span data-stu-id="6367f-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span>

   * <span data-ttu-id="6367f-115">**A Publisher**: hello publisher fogjuk toocreate a virtuális gép hello kép létrehozott határozza meg (például *Microsoft*).</span><span class="sxs-lookup"><span data-stu-id="6367f-115">**Publisher**: Specifies hello publisher that created hello image you'll use toocreate your virtual machine (for example, *Microsoft*).</span></span>

   * <span data-ttu-id="6367f-116">**Ajánlat**: hello virtuális gép toouse ajánlat hello kijelölt közzétételi határozza meg (például *JDK*).</span><span class="sxs-lookup"><span data-stu-id="6367f-116">**Offer**: Specifies hello virtual machine offering toouse from hello selected publisher (for example, *JDK*).</span></span>

   * <span data-ttu-id="6367f-117">**SKU**: hello raktározási egységhez toouse hello kijelölt ajánlat az határozza meg (például *JDK_8*).</span><span class="sxs-lookup"><span data-stu-id="6367f-117">**Sku**: Specifies hello stockkeeping unit (SKU) toouse from hello selected offering (for example, *JDK_8*).</span></span>

   * <span data-ttu-id="6367f-118">**Verzió #**: Adja meg a kiválasztott hello verziójának SKU toouse.</span><span class="sxs-lookup"><span data-stu-id="6367f-118">**Version #**: Specifies which version of hello selected SKU toouse.</span></span>

    ![hello válassza ki a virtuálisgép-lemezkép ablak][CR03]

5. <span data-ttu-id="6367f-120">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="6367f-120">Click **Next**.</span></span>

6. <span data-ttu-id="6367f-121">A hello **virtuális gép alapbeállítások** ablak, írja be a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="6367f-121">In hello **Virtual Machine Basic Settings** window, enter hello following information:</span></span>

   * <span data-ttu-id="6367f-122">**Virtuális gép neve**: hello neve, az új virtuális gépen, amely kizárólag betűvel kezdődhet, és csak betűket, számokat és kötőjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="6367f-122">**Virtual Machine Name**: Specifies hello name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="6367f-123">**Méret**: hello magok száma és a virtuális gép memória tooallocate határozza meg.</span><span class="sxs-lookup"><span data-stu-id="6367f-123">**Size**: Specifies hello number of cores and memory tooallocate for your virtual machine.</span></span>

   * <span data-ttu-id="6367f-124">**Felhasználónév**: hello rendszergazdai fiók toocreate kezelését a virtuális gép határozza meg.</span><span class="sxs-lookup"><span data-stu-id="6367f-124">**User name**: Specifies hello administrator account toocreate for managing your virtual machine.</span></span>

   * <span data-ttu-id="6367f-125">**Jelszó** és **megerősítése**: az Ön rendszergazdai fiókjának hello jelszót ad meg.</span><span class="sxs-lookup"><span data-stu-id="6367f-125">**Password** and **Confirm**: Specifies hello password for your administrator account.</span></span>

    ![hello virtuális gép alapbeállítások ablak][CR04]

7. <span data-ttu-id="6367f-127">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="6367f-127">Click **Next**.</span></span>

8. <span data-ttu-id="6367f-128">A hello **új Tárfiók létrehozása** ablak, írja be a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="6367f-128">In hello **Create New Storage Account** window, enter hello following information:</span></span>

   * <span data-ttu-id="6367f-129">**Erőforráscsoport**: hello erőforrás csoportot a virtuális gép határozza meg.</span><span class="sxs-lookup"><span data-stu-id="6367f-129">**Resource Group**: Specifies hello resource group for your virtual machine.</span></span> <span data-ttu-id="6367f-130">Válasszon egyet az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="6367f-130">Select one of hello following options:</span></span>
      * <span data-ttu-id="6367f-131">**Új**: megadhatja, hogy kívánja-e toocreate egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="6367f-131">**Create new**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="6367f-132">**Meglévő**: megadhatja, hogy kívánja-e tooselect egy erőforráscsoport, amely már társítva van az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="6367f-132">**Use existing**: Specifies that you want tooselect a resource group that is already associated with your Azure account.</span></span>

      ![hello új Tárfiók létrehozása párbeszédpanel][CR05]

   * <span data-ttu-id="6367f-134">**A tárfiók**: hello tárolási fiók toouse a virtuális gép tárolásához adja meg.</span><span class="sxs-lookup"><span data-stu-id="6367f-134">**Storage account**: Specifies hello storage account toouse for storing your virtual machine.</span></span> <span data-ttu-id="6367f-135">Meglévő tárfiók használata, vagy hozzon létre egy új fiókot.</span><span class="sxs-lookup"><span data-stu-id="6367f-135">You can use an existing storage account or create a new account.</span></span>

   * <span data-ttu-id="6367f-136">**Virtuális hálózati** és **alhálózati**: hello virtuális hálózat és az alhálózatot, amely a virtuális gép csatlakozni fog.</span><span class="sxs-lookup"><span data-stu-id="6367f-136">**Virtual Network** and **Subnet**: Specifies hello virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="6367f-137">Használhat egy meglévő hálózati és alhálózati, vagy létrehozhat egy új hálózati és alhálózati.</span><span class="sxs-lookup"><span data-stu-id="6367f-137">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="6367f-138">Ha **hozzon létre új**, hello a következő párbeszédpanel jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="6367f-138">If you select **Create new**, hello following dialog box is displayed:</span></span>

      ![hello új virtuális hálózat létrehozása párbeszédpanel][CR06]

9. <span data-ttu-id="6367f-140">A hello **kapcsolódó erőforrások** ablak, írja be a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="6367f-140">In hello **Associated Resources** window, enter hello following information:</span></span>

   * <span data-ttu-id="6367f-141">**Nyilvános IP-cím**: egy külső hálózat irányába néző IP-cím a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="6367f-141">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="6367f-142">Választhat toocreate egy új IP-címet, vagy ha a virtuális gép nem fog működni a nyilvános IP-cím, választhat **(nincs)**.</span><span class="sxs-lookup"><span data-stu-id="6367f-142">You can choose toocreate a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span>

   * <span data-ttu-id="6367f-143">**Hálózati biztonsági csoport**: Adja meg a virtuális gép egy választható hálózati tűzfal.</span><span class="sxs-lookup"><span data-stu-id="6367f-143">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="6367f-144">Kiválaszthatja, hogy egy meglévő tűzfal, vagy ha a virtuális gép nem használja a hálózati tűzfal, akkor választhat **(nincs)**.</span><span class="sxs-lookup"><span data-stu-id="6367f-144">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span>

   * <span data-ttu-id="6367f-145">**A rendelkezésre állási csoport**: határozza meg, opcionális rendelkezésre állási készlet, hogy a virtuális gép tartozhatnak.</span><span class="sxs-lookup"><span data-stu-id="6367f-145">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="6367f-146">Válasszon egy meglévő rendelkezésre állási csoportot, vagy hozzon létre egy új rendelkezésre állási készletet, vagy ha a virtuális gép nem fog tartozni tooan rendelkezésre állási csoportot, válassza **(nincs)**.</span><span class="sxs-lookup"><span data-stu-id="6367f-146">You can select an existing availability set or create a new availability set or, if your virtual machine will not belong tooan availability set, you can select **(None)**.</span></span>

   ![hello kapcsolódó erőforrások ablak][CR07]

9. <span data-ttu-id="6367f-148">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="6367f-148">Click **Finish**.</span></span>  
  <span data-ttu-id="6367f-149">Az új virtuális gép hello Azure Explorer eszköz ablakban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6367f-149">Your new virtual machine is displayed in hello Azure Explorer tool window.</span></span>

   ![Új virtuális gép][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a><span data-ttu-id="6367f-151">A virtuális gép újraindításához az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="6367f-151">Restart a virtual machine in Eclipse</span></span>

<span data-ttu-id="6367f-152">az eclipse-ben hello Azure Explorer használatával egy virtuális gép toorestart hello a következő:</span><span class="sxs-lookup"><span data-stu-id="6367f-152">toorestart a virtual machine by using hello Azure Explorer in Eclipse, do hello following:</span></span>

1. <span data-ttu-id="6367f-153">A hello **Azure Explorer** nézetben, majd válassza ki és kattintson a jobb gombbal a virtuális gép hello **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="6367f-153">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Restart**.</span></span>

   ![virtuális gép újraindítási parancsnak hello][RE01]

2. <span data-ttu-id="6367f-155">Hello ablak, kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="6367f-155">In hello confirmation window, click **Yes**.</span></span>

   ![hello újraindítás ablak][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a><span data-ttu-id="6367f-157">Az eclipse-ben a virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="6367f-157">Shut down a virtual machine in Eclipse</span></span>

<span data-ttu-id="6367f-158">hello Azure Explorer használatával az eclipse-ben futó virtuális gép le tooshut hello a következő:</span><span class="sxs-lookup"><span data-stu-id="6367f-158">tooshut down a running virtual machine by using hello Azure Explorer in Eclipse, do hello following:</span></span>

1. <span data-ttu-id="6367f-159">A hello **Azure Explorer** nézetben, majd válassza ki és kattintson a jobb gombbal a virtuális gép hello **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="6367f-159">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Shutdown**.</span></span>

   ![hello virtuális gép leállítási parancs][SH01]

2. <span data-ttu-id="6367f-161">Hello ablak, kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="6367f-161">In hello confirmation window, click **Yes**.</span></span>

   ![virtuális gép leállítási hello ablak][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a><span data-ttu-id="6367f-163">Az eclipse-ben a virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="6367f-163">Delete a virtual machine in Eclipse</span></span>

<span data-ttu-id="6367f-164">az eclipse-ben hello Azure Explorer használatával egy virtuális gép toodelete hello a következő:</span><span class="sxs-lookup"><span data-stu-id="6367f-164">toodelete a virtual machine by using hello Azure Explorer in Eclipse, do hello following:</span></span>

1. <span data-ttu-id="6367f-165">A hello **Azure Explorer** nézetben, majd válassza ki és kattintson a jobb gombbal a virtuális gép hello **törlése**.</span><span class="sxs-lookup"><span data-stu-id="6367f-165">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Delete**.</span></span>

   ![hello virtuális gép Delete parancs esetében][DE01]

2. <span data-ttu-id="6367f-167">Hello ablak, kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="6367f-167">In hello confirmation window, click **Yes**.</span></span>

   ![hello törölni a virtuális gépet ablak][DE02]

## <a name="next-steps"></a><span data-ttu-id="6367f-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6367f-169">Next steps</span></span>
<span data-ttu-id="6367f-170">Az Azure virtuális gépek méretét és a díjszabással kapcsolatos további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="6367f-170">For more information about Azure virtual-machine sizes and pricing, see hello following resources:</span></span>

* <span data-ttu-id="6367f-171">Az Azure virtuális gépek méretét</span><span class="sxs-lookup"><span data-stu-id="6367f-171">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="6367f-172">[Az Azure-ban a Windows virtuális gépek méretei]</span><span class="sxs-lookup"><span data-stu-id="6367f-172">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="6367f-173">[Az Azure Linux virtuális gépek méretei]</span><span class="sxs-lookup"><span data-stu-id="6367f-173">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="6367f-174">Az Azure virtuális gépek – díjszabás</span><span class="sxs-lookup"><span data-stu-id="6367f-174">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="6367f-175">[Windows virtuális gépek – díjszabás]</span><span class="sxs-lookup"><span data-stu-id="6367f-175">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="6367f-176">[Linux virtuális gépek – díjszabás]</span><span class="sxs-lookup"><span data-stu-id="6367f-176">[Linux virtual-machine pricing]</span></span>

<span data-ttu-id="6367f-177">A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="6367f-177">For more information about hello Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="6367f-178">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="6367f-178">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="6367f-179">[What's new in hello Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="6367f-179">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="6367f-180">[Hello Azure eszköztára Eclipse telepítése]</span><span class="sxs-lookup"><span data-stu-id="6367f-180">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="6367f-181">[hello Azure eszköztára Eclipse bejelentkezési utasítások]</span><span class="sxs-lookup"><span data-stu-id="6367f-181">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="6367f-182">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="6367f-182">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="6367f-183">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="6367f-183">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="6367f-184">[What's new in hello IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="6367f-184">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="6367f-185">[Az IntelliJ hello Azure eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="6367f-185">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="6367f-186">[Bejelentkezési utasításokat hello Azure eszközkészlet az intellij-t]</span><span class="sxs-lookup"><span data-stu-id="6367f-186">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="6367f-187">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="6367f-187">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="6367f-188">Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="6367f-188">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Hello Azure eszköztára Eclipse telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ hello Azure eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[hello Azure eszköztára Eclipse bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezési utasításokat hello Azure eszközkészlet az intellij-t]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's new in hello Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md
[What's new in hello IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/

[Az Azure-ban a Windows virtuális gépek méretei]: /azure/virtual-machines/virtual-machines-windows-sizes
[Az Azure Linux virtuális gépek méretei]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows virtuális gépek – díjszabás]: /pricing/details/virtual-machines/windows/
[Linux virtuális gépek – díjszabás]: /pricing/details/virtual-machines/linux/

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
