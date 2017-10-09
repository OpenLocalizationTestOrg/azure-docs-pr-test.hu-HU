---
title: "IntelliJ aaaManage storage-fiókok használatával hello Azure-kezelővel |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage az Azure storage accounts IntelliJ hello Azure Explorer használatával."
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
ms.openlocfilehash: b094bdcd2fa2782cb12b67c96ac406fbe4c1aa3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-storage-accounts-by-using-hello-azure-explorer-for-intellij"></a><span data-ttu-id="de638-103">Az IntelliJ hello Azure Explorer használatával storage-fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="de638-103">Manage storage accounts by using hello Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="de638-104">hello Azure-kezelővel hello Azure eszköztára IntelliJ része, a Java fejlesztők egy könnyen kezelhető megoldás biztosít a fiókjuk Azure storage-fiók kezeléséhez hello IntelliJ integrált fejlesztési környezeti (IDE) belül.</span><span class="sxs-lookup"><span data-stu-id="de638-104">hello Azure Explorer, which is part of hello Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing storage accounts in their Azure account from inside hello IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a><span data-ttu-id="de638-105">A storage-fiók létrehozása az intellij-t</span><span class="sxs-lookup"><span data-stu-id="de638-105">Create a storage account in IntelliJ</span></span>

<span data-ttu-id="de638-106">a storage-fiókok hello Azure Explorer használatával toocreate hello a következő:</span><span class="sxs-lookup"><span data-stu-id="de638-106">toocreate a storage account by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="de638-107">Jelentkezzen be Azure-fiók tooyour hello [hello Azure eszköztára Eclipse bejelentkezési utasítások].</span><span class="sxs-lookup"><span data-stu-id="de638-107">Sign in tooyour Azure account by using hello [Sign-in instructions for hello Azure Toolkit for Eclipse].</span></span> 

2. <span data-ttu-id="de638-108">A hello **Azure Explorer** megtekintéséhez bontsa ki a hello **Azure** csomópontot, kattintson a jobb gombbal **Tárfiókok**, és kattintson a **Storage-fióklétrehozása**.</span><span class="sxs-lookup"><span data-stu-id="de638-108">In hello **Azure Explorer** view, expand hello **Azure** node, right-click **Storage Accounts**, and then click **Create Storage Account**.</span></span>

   ![A Tárfiók parancs létrehozása][CS01]

3. <span data-ttu-id="de638-110">A hello **Storage-fiók létrehozása** párbeszédpanelen adja meg az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="de638-110">In hello **Create Storage Account** dialog box, specify hello following options:</span></span>

   ![Hozzon létre új Tárfiók párbeszédpanel][CS02]

   * <span data-ttu-id="de638-112">**Név**: hello új tárfiók hello nevét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="de638-112">**Name**: Specifies hello name for hello new storage account.</span></span>

   * <span data-ttu-id="de638-113">**Fiók kind**: hello adja meg a tárolási fiók toocreate (például "Blob tároló").</span><span class="sxs-lookup"><span data-stu-id="de638-113">**Account kind**: Specifies hello type of storage account toocreate (for example, "Blob storage").</span></span> <span data-ttu-id="de638-114">További információkért lásd: [tudnivalók az Azure storage-fiókok].</span><span class="sxs-lookup"><span data-stu-id="de638-114">For more information, see [About Azure storage accounts].</span></span> 

   * <span data-ttu-id="de638-115">**Teljesítmény**: Megadja, hogy mely tárolási toouse ajánlat hello kijelölt közzétételi (például "Prémium").</span><span class="sxs-lookup"><span data-stu-id="de638-115">**Performance**: Specifies which storage account offering toouse from hello selected publisher (for example, "Premium").</span></span> <span data-ttu-id="de638-116">További információkért lásd: [az Azure storage méretezhetőségi és Teljesítménycélok].</span><span class="sxs-lookup"><span data-stu-id="de638-116">For more information, see [Azure storage scalability and performance targets].</span></span> 

   * <span data-ttu-id="de638-117">**Replikációs**: hello replikációs hello tárfiókhoz (például "Zónaredundáns") határozza meg.</span><span class="sxs-lookup"><span data-stu-id="de638-117">**Replication**: Specifies hello replication for hello storage account (for example, "Zone-Redundant").</span></span> <span data-ttu-id="de638-118">További információkért lásd: [az Azure storage replikációs].</span><span class="sxs-lookup"><span data-stu-id="de638-118">For more information, see [Azure storage replication].</span></span> 

   * <span data-ttu-id="de638-119">**Előfizetés**: hello Azure-előfizetést, amelyet az toouse hello új tárfiók határozza meg.</span><span class="sxs-lookup"><span data-stu-id="de638-119">**Subscription**: Specifies hello Azure subscription that you want toouse for hello new storage account.</span></span>

   * <span data-ttu-id="de638-120">**Hely**: hello helyet a tárfiók létrehozásának helyét (például "Nyugati US") határoz meg.</span><span class="sxs-lookup"><span data-stu-id="de638-120">**Location**: Specifies hello location where your storage account will be created (for example, "West US").</span></span>

   * <span data-ttu-id="de638-121">**Erőforráscsoport**: hello erőforrás csoportot a virtuális gép határozza meg.</span><span class="sxs-lookup"><span data-stu-id="de638-121">**Resource Group**: Specifies hello resource group for your virtual machine.</span></span> <span data-ttu-id="de638-122">Válasszon egyet az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="de638-122">Select one of hello following options:</span></span>
      * <span data-ttu-id="de638-123">**Új**: megadhatja, hogy kívánja-e toocreate egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="de638-123">**Create new**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="de638-124">**Meglévő**: meghatározza, hogy az erőforráscsoportok az Azure-fiókjával társított listából kiválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="de638-124">**Use existing**: Specifies that you will select from a list of resource groups that are associated with your Azure account.</span></span>

4. <span data-ttu-id="de638-125">Ha megadott hello megelőző beállítások mindegyikével, kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="de638-125">When you have specified all of hello preceding options, click **OK**.</span></span>

## <a name="create-a-storage-container-in-intellij"></a><span data-ttu-id="de638-126">A tároló létrehozása az intellij-t</span><span class="sxs-lookup"><span data-stu-id="de638-126">Create a storage container in IntelliJ</span></span>

<span data-ttu-id="de638-127">egy tároló hello Azure Explorer használatával toocreate hello a következő:</span><span class="sxs-lookup"><span data-stu-id="de638-127">toocreate a storage container by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="de638-128">A hello Azure terület Intézőbeli nézetében, kattintson a jobb gombbal hello tárfiókot, ha szeretné, hogy a tároló toocreate, és kattintson a **létrehozás blob tároló**.</span><span class="sxs-lookup"><span data-stu-id="de638-128">In hello Azure Explorer view, right-click hello storage account where you want toocreate a container, and then click **Create blob container**.</span></span>

   ![A blob-tároló parancs létrehozása][CC01]

2. <span data-ttu-id="de638-130">A hello **létrehozás blob tároló** párbeszédpanelen adja meg a tároló hello nevét, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="de638-130">In hello **Create blob container** dialog box, specify hello name for your container, and then click **OK**.</span></span> <span data-ttu-id="de638-131">A tároló elnevezésével kapcsolatos további információkért lásd: [elnevezési és a tárolók, blobok és metaadatok hivatkozó].</span><span class="sxs-lookup"><span data-stu-id="de638-131">For more information about naming storage containers, see [Naming and referencing containers, blobs, and metadata].</span></span>

   ![Hozzon létre tárolási tároló párbeszédpanel][CC02]

## <a name="delete-a-storage-container-in-intellij"></a><span data-ttu-id="de638-133">Az intellij-t a tároló törlése</span><span class="sxs-lookup"><span data-stu-id="de638-133">Delete a storage container in IntelliJ</span></span>

<span data-ttu-id="de638-134">egy tároló hello Azure Explorer használatával toodelete hello a következő:</span><span class="sxs-lookup"><span data-stu-id="de638-134">toodelete a storage container by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="de638-135">Hello Azure terület Intézőbeli nézetében, kattintson a jobb gombbal a hello tárolót, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="de638-135">In hello Azure Explorer view, right-click hello storage container, and then click **Delete**.</span></span>

   ![Tárolási tároló parancs törlése][DC01]

2. <span data-ttu-id="de638-137">Hello ablak, kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="de638-137">In hello confirmation window, click **Yes**.</span></span>

   ![Tárolási tároló ablak törlése][DC02]

## <a name="delete-a-storage-account-in-intellij"></a><span data-ttu-id="de638-139">Az IntelliJ a tárfiók törlése</span><span class="sxs-lookup"><span data-stu-id="de638-139">Delete a storage account in IntelliJ</span></span>

<span data-ttu-id="de638-140">a storage-fiókok hello Azure Explorer használatával toodelete hello a következő:</span><span class="sxs-lookup"><span data-stu-id="de638-140">toodelete a storage account by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="de638-141">A hello **Azure Explorer** nézetben kattintson a jobb gombbal a hello tárfiók, és válassza ki **törlése**.</span><span class="sxs-lookup"><span data-stu-id="de638-141">In hello **Azure Explorer** view, right-click hello storage account, and then select **Delete**.</span></span>

   ![Felhasználóifiók-menüjéből tároló törlése][DS01]

2. <span data-ttu-id="de638-143">Hello ablak, kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="de638-143">In hello confirmation window, click **Yes**.</span></span>

   ![Törölje a tárolási fiók ablak][DS02]

## <a name="next-steps"></a><span data-ttu-id="de638-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de638-145">Next steps</span></span>
<span data-ttu-id="de638-146">Az Azure storage-fiókok, méretek és árképzési kapcsolatos további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="de638-146">For more information about Azure storage accounts, sizes, and pricing, see hello following resources:</span></span>

* <span data-ttu-id="de638-147">[Azure Storage bemutatása tooMicrosoft]</span><span class="sxs-lookup"><span data-stu-id="de638-147">[Introduction tooMicrosoft Azure Storage]</span></span>
* <span data-ttu-id="de638-148">[tudnivalók az Azure storage-fiókok]</span><span class="sxs-lookup"><span data-stu-id="de638-148">[About Azure storage accounts]</span></span>
* <span data-ttu-id="de638-149">Azure storage-fiókok méretének</span><span class="sxs-lookup"><span data-stu-id="de638-149">Azure storage-account sizes</span></span>
  * <span data-ttu-id="de638-150">[Windows Azure storage-fiók mérete]</span><span class="sxs-lookup"><span data-stu-id="de638-150">[Sizes for Windows storage accounts in Azure]</span></span>
  * <span data-ttu-id="de638-151">[Az Azure storage-fiókok Linux mérete]</span><span class="sxs-lookup"><span data-stu-id="de638-151">[Sizes for Linux storage accounts in Azure]</span></span>
* <span data-ttu-id="de638-152">Az Azure storage-fiók díjszabása</span><span class="sxs-lookup"><span data-stu-id="de638-152">Azure storage-account pricing</span></span>
  * <span data-ttu-id="de638-153">[A Windows storage-fiók díjszabása]</span><span class="sxs-lookup"><span data-stu-id="de638-153">[Windows storage-account pricing]</span></span>
  * <span data-ttu-id="de638-154">[Linux-tárfiók díjszabása]</span><span class="sxs-lookup"><span data-stu-id="de638-154">[Linux storage-account pricing]</span></span>

<span data-ttu-id="de638-155">A Java IDEs Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="de638-155">For more information about Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="de638-156">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="de638-156">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="de638-157">[What's new in hello Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="de638-157">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="de638-158">[Hello Azure eszköztára Eclipse telepítése]</span><span class="sxs-lookup"><span data-stu-id="de638-158">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="de638-159">[hello Azure eszköztára Eclipse bejelentkezési utasítások]</span><span class="sxs-lookup"><span data-stu-id="de638-159">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="de638-160">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="de638-160">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="de638-161">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="de638-161">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="de638-162">[What's new in hello IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="de638-162">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="de638-163">[Az IntelliJ hello Azure eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="de638-163">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="de638-164">[Bejelentkezési utasításokat hello Azure eszközkészlet az intellij-t]</span><span class="sxs-lookup"><span data-stu-id="de638-164">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="de638-165">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="de638-165">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="de638-166">Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="de638-166">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

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

[Azure Storage bemutatása tooMicrosoft]: /azure/storage/storage-introduction
[tudnivalók az Azure storage-fiókok]: /azure/storage/storage-create-storage-account
[az Azure storage replikációs]: /azure/storage/storage-redundancy
[Az Azure storage méretezhetőségi és teljesítménycéloknak]: /azure/storage/storage-scalability-targets
[elnevezési és a tárolók, blobok és metaadatok hivatkozó]: http://go.microsoft.com/fwlink/?LinkId=255555

[Windows Azure storage-fiók mérete]: /azure/virtual-machines/virtual-machines-windows-sizes
[Az Azure storage-fiókok Linux mérete]: /azure/virtual-machines/virtual-machines-linux-sizes
[A Windows storage-fiók díjszabása]: /pricing/details/virtual-machines/windows/
[Linux-tárfiók díjszabása]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png
