---
title: "Az intellij-t az Azure-kezelővel a storage-fiókok kezelése |} Microsoft Docs"
description: "Útmutató az Azure storage-fiókok kezelése az intellij-t az Azure-kezelővel használatával."
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
ms.openlocfilehash: a1b56cc2751fc43a1ad6917eca77eec460f26694
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="a3e76-103">Az intellij-t az Azure-kezelővel a storage-fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="a3e76-103">Manage storage accounts by using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="a3e76-104">Az Azure-kezelővel, amely az IntelliJ Azure eszköztára része, Java fejlesztők egy könnyen kezelhető megoldás biztosít az Azure-fiók a belül az IntelliJ integrált fejlesztési környezeti (IDE) storage-fiók kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="a3e76-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing storage accounts in their Azure account from inside the IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a><span data-ttu-id="a3e76-105">A storage-fiók létrehozása az intellij-t</span><span class="sxs-lookup"><span data-stu-id="a3e76-105">Create a storage account in IntelliJ</span></span>

<span data-ttu-id="a3e76-106">A storage-fiók létrehozása az Azure-kezelővel használatával, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a3e76-106">To create a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="a3e76-107">Jelentkezzen be az Azure-fiók használatával a [Eclipse Azure eszköztára bejelentkezési utasítások].</span><span class="sxs-lookup"><span data-stu-id="a3e76-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for Eclipse].</span></span> 

2. <span data-ttu-id="a3e76-108">Az a **Azure Explorer** megtekintéséhez bontsa ki a **Azure** csomópontot, kattintson a jobb gombbal **Tárfiókok**, és kattintson a **Storage-fiók létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a3e76-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Storage Accounts**, and then click **Create Storage Account**.</span></span>

   ![A Tárfiók parancs létrehozása][CS01]

3. <span data-ttu-id="a3e76-110">Az a **Storage-fiók létrehozása** párbeszédpanelen adja meg a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="a3e76-110">In the **Create Storage Account** dialog box, specify the following options:</span></span>

   ![Hozzon létre új Tárfiók párbeszédpanel][CS02]

   * <span data-ttu-id="a3e76-112">**Név**: az új tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="a3e76-112">**Name**: Specifies the name for the new storage account.</span></span>

   * <span data-ttu-id="a3e76-113">**Fiók kind**: határozza meg a tárfiók létrehozása (például "Blob-tároló").</span><span class="sxs-lookup"><span data-stu-id="a3e76-113">**Account kind**: Specifies the type of storage account to create (for example, "Blob storage").</span></span> <span data-ttu-id="a3e76-114">További információkért lásd: [tudnivalók az Azure storage-fiókok].</span><span class="sxs-lookup"><span data-stu-id="a3e76-114">For more information, see [About Azure storage accounts].</span></span> 

   * <span data-ttu-id="a3e76-115">**Teljesítmény**: Adja meg a használt tárfiók ajánlat (például "Prémium") a kijelölt közzétételi használatára.</span><span class="sxs-lookup"><span data-stu-id="a3e76-115">**Performance**: Specifies which storage account offering to use from the selected publisher (for example, "Premium").</span></span> <span data-ttu-id="a3e76-116">További információkért lásd: [az Azure storage méretezhetőségi és Teljesítménycélok].</span><span class="sxs-lookup"><span data-stu-id="a3e76-116">For more information, see [Azure storage scalability and performance targets].</span></span> 

   * <span data-ttu-id="a3e76-117">**Replikációs**: Adja meg a tárfiók (például "Zónaredundáns") replikációját.</span><span class="sxs-lookup"><span data-stu-id="a3e76-117">**Replication**: Specifies the replication for the storage account (for example, "Zone-Redundant").</span></span> <span data-ttu-id="a3e76-118">További információkért lásd: [az Azure storage replikációs].</span><span class="sxs-lookup"><span data-stu-id="a3e76-118">For more information, see [Azure storage replication].</span></span> 

   * <span data-ttu-id="a3e76-119">**Előfizetés**: Adja meg az új tárfiók használni kívánt Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="a3e76-119">**Subscription**: Specifies the Azure subscription that you want to use for the new storage account.</span></span>

   * <span data-ttu-id="a3e76-120">**Hely**: Adja meg a helyet, ahol a tárfiók létrejön-e (például "Nyugati US").</span><span class="sxs-lookup"><span data-stu-id="a3e76-120">**Location**: Specifies the location where your storage account will be created (for example, "West US").</span></span>

   * <span data-ttu-id="a3e76-121">**Erőforráscsoport**: Adja meg a virtuális géphez tartozó erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="a3e76-121">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="a3e76-122">A következő lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="a3e76-122">Select one of the following options:</span></span>
      * <span data-ttu-id="a3e76-123">**Új**: megadhatja, hogy kívánja-e egy új erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a3e76-123">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="a3e76-124">**Meglévő**: meghatározza, hogy az erőforráscsoportok az Azure-fiókjával társított listából kiválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="a3e76-124">**Use existing**: Specifies that you will select from a list of resource groups that are associated with your Azure account.</span></span>

4. <span data-ttu-id="a3e76-125">Ha az előző közül az összes megadott, kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="a3e76-125">When you have specified all of the preceding options, click **OK**.</span></span>

## <a name="create-a-storage-container-in-intellij"></a><span data-ttu-id="a3e76-126">A tároló létrehozása az intellij-t</span><span class="sxs-lookup"><span data-stu-id="a3e76-126">Create a storage container in IntelliJ</span></span>

<span data-ttu-id="a3e76-127">A tároló létrehozása az Azure-kezelővel használatával, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a3e76-127">To create a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="a3e76-128">Az Azure-kezelővel nézetben kattintson a jobb gombbal a tárfiókot, hol szeretné létrehozni a tárolót, és kattintson a **létrehozás blob tároló**.</span><span class="sxs-lookup"><span data-stu-id="a3e76-128">In the Azure Explorer view, right-click the storage account where you want to create a container, and then click **Create blob container**.</span></span>

   ![A blob-tároló parancs létrehozása][CC01]

2. <span data-ttu-id="a3e76-130">Az a **létrehozás blob tároló** párbeszédpanelen adja meg a tároló a nevét, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="a3e76-130">In the **Create blob container** dialog box, specify the name for your container, and then click **OK**.</span></span> <span data-ttu-id="a3e76-131">A tároló elnevezésével kapcsolatos további információkért lásd: [elnevezési és a tárolók, blobok és metaadatok hivatkozó].</span><span class="sxs-lookup"><span data-stu-id="a3e76-131">For more information about naming storage containers, see [Naming and referencing containers, blobs, and metadata].</span></span>

   ![Hozzon létre tárolási tároló párbeszédpanel][CC02]

## <a name="delete-a-storage-container-in-intellij"></a><span data-ttu-id="a3e76-133">Az intellij-t a tároló törlése</span><span class="sxs-lookup"><span data-stu-id="a3e76-133">Delete a storage container in IntelliJ</span></span>

<span data-ttu-id="a3e76-134">A tároló törlése az Azure Explorerrel, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a3e76-134">To delete a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="a3e76-135">Az Azure-kezelővel nézetben kattintson a jobb gombbal a tárolóra, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="a3e76-135">In the Azure Explorer view, right-click the storage container, and then click **Delete**.</span></span>

   ![Tárolási tároló parancs törlése][DC01]

2. <span data-ttu-id="a3e76-137">Kattintson a megerősítés ablakban **Igen**.</span><span class="sxs-lookup"><span data-stu-id="a3e76-137">In the confirmation window, click **Yes**.</span></span>

   ![Tárolási tároló ablak törlése][DC02]

## <a name="delete-a-storage-account-in-intellij"></a><span data-ttu-id="a3e76-139">Az IntelliJ a tárfiók törlése</span><span class="sxs-lookup"><span data-stu-id="a3e76-139">Delete a storage account in IntelliJ</span></span>

<span data-ttu-id="a3e76-140">A tárfiók törlése az Azure-kezelővel használatával, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a3e76-140">To delete a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="a3e76-141">Az a **Azure Explorer** nézetben kattintson a jobb gombbal a tárfiók, és válassza ki **törlése**.</span><span class="sxs-lookup"><span data-stu-id="a3e76-141">In the **Azure Explorer** view, right-click the storage account, and then select **Delete**.</span></span>

   ![Felhasználóifiók-menüjéből tároló törlése][DS01]

2. <span data-ttu-id="a3e76-143">Kattintson a megerősítés ablakban **Igen**.</span><span class="sxs-lookup"><span data-stu-id="a3e76-143">In the confirmation window, click **Yes**.</span></span>

   ![Törölje a tárolási fiók ablak][DS02]

## <a name="next-steps"></a><span data-ttu-id="a3e76-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3e76-145">Next steps</span></span>
<span data-ttu-id="a3e76-146">További információ az Azure storage-fiókok méretek és a díjszabás, lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="a3e76-146">For more information about Azure storage accounts, sizes, and pricing, see the following resources:</span></span>

* <span data-ttu-id="a3e76-147">[A Microsoft Azure Storage bemutatása]</span><span class="sxs-lookup"><span data-stu-id="a3e76-147">[Introduction to Microsoft Azure Storage]</span></span>
* <span data-ttu-id="a3e76-148">[tudnivalók az Azure storage-fiókok]</span><span class="sxs-lookup"><span data-stu-id="a3e76-148">[About Azure storage accounts]</span></span>
* <span data-ttu-id="a3e76-149">Azure storage-fiókok méretének</span><span class="sxs-lookup"><span data-stu-id="a3e76-149">Azure storage-account sizes</span></span>
  * <span data-ttu-id="a3e76-150">[Windows Azure storage-fiók mérete]</span><span class="sxs-lookup"><span data-stu-id="a3e76-150">[Sizes for Windows storage accounts in Azure]</span></span>
  * <span data-ttu-id="a3e76-151">[Az Azure storage-fiókok Linux mérete]</span><span class="sxs-lookup"><span data-stu-id="a3e76-151">[Sizes for Linux storage accounts in Azure]</span></span>
* <span data-ttu-id="a3e76-152">Az Azure storage-fiók díjszabása</span><span class="sxs-lookup"><span data-stu-id="a3e76-152">Azure storage-account pricing</span></span>
  * <span data-ttu-id="a3e76-153">[A Windows storage-fiók díjszabása]</span><span class="sxs-lookup"><span data-stu-id="a3e76-153">[Windows storage-account pricing]</span></span>
  * <span data-ttu-id="a3e76-154">[Linux-tárfiók díjszabása]</span><span class="sxs-lookup"><span data-stu-id="a3e76-154">[Linux storage-account pricing]</span></span>

<span data-ttu-id="a3e76-155">A Java IDEs Azure eszközök gazdag kapcsolatos további információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="a3e76-155">For more information about Azure Toolkits for Java IDEs, see the following resources:</span></span>

* <span data-ttu-id="a3e76-156">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="a3e76-156">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a3e76-157">[What's new in Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="a3e76-157">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a3e76-158">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="a3e76-158">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a3e76-159">[Eclipse Azure eszköztára bejelentkezési utasítások]</span><span class="sxs-lookup"><span data-stu-id="a3e76-159">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a3e76-160">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="a3e76-160">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="a3e76-161">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="a3e76-161">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a3e76-162">[What's new in IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="a3e76-162">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a3e76-163">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="a3e76-163">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a3e76-164">[Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t]</span><span class="sxs-lookup"><span data-stu-id="a3e76-164">[Sign-in instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a3e76-165">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="a3e76-165">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="a3e76-166">Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="a3e76-166">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="a3e76-167">[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="a3e76-167">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="a3e76-168">[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="a3e76-168">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="a3e76-169">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="a3e76-169">[Create a Hello World web app for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="a3e76-170">[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="a3e76-170">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="a3e76-171">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="a3e76-171">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="a3e76-172">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="a3e76-172">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="a3e76-173">[Eclipse Azure eszköztára bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="a3e76-173">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="a3e76-174">[Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="a3e76-174">[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="a3e76-175">[What's new in Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="a3e76-175">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="a3e76-176">[What's new in IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="a3e76-176">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="a3e76-177">[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="a3e76-177">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="a3e76-178">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="a3e76-178">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

<span data-ttu-id="a3e76-179">[A Microsoft Azure Storage bemutatása]: /azure/storage/storage-introduction</span><span class="sxs-lookup"><span data-stu-id="a3e76-179">[Introduction to Microsoft Azure Storage]: /azure/storage/storage-introduction</span></span>
<span data-ttu-id="a3e76-180">[tudnivalók az Azure storage-fiókok]: /azure/storage/storage-create-storage-account</span><span class="sxs-lookup"><span data-stu-id="a3e76-180">[About Azure storage accounts]: /azure/storage/storage-create-storage-account</span></span>
<span data-ttu-id="a3e76-181">[az Azure storage replikációs]: /azure/storage/storage-redundancy</span><span class="sxs-lookup"><span data-stu-id="a3e76-181">[Azure storage replication]: /azure/storage/storage-redundancy</span></span>
<span data-ttu-id="a3e76-182">[Az Azure storage méretezhetőségi és teljesítménycéloknak]: /azure/storage/storage-scalability-targets</span><span class="sxs-lookup"><span data-stu-id="a3e76-182">[Azure storage scalability and Performance Targets]: /azure/storage/storage-scalability-targets</span></span>
<span data-ttu-id="a3e76-183">[elnevezési és a tárolók, blobok és metaadatok hivatkozó]: http://go.microsoft.com/fwlink/?LinkId=255555</span><span class="sxs-lookup"><span data-stu-id="a3e76-183">[Naming and referencing containers, blobs, and metadata]: http://go.microsoft.com/fwlink/?LinkId=255555</span></span>

<span data-ttu-id="a3e76-184">[Windows Azure storage-fiók mérete]: /azure/virtual-machines/virtual-machines-windows-sizes</span><span class="sxs-lookup"><span data-stu-id="a3e76-184">[Sizes for Windows storage accounts in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes</span></span>
<span data-ttu-id="a3e76-185">[Az Azure storage-fiókok Linux mérete]: /azure/virtual-machines/virtual-machines-linux-sizes</span><span class="sxs-lookup"><span data-stu-id="a3e76-185">[Sizes for Linux storage accounts in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes</span></span>
<span data-ttu-id="a3e76-186">[A Windows storage-fiók díjszabása]: /pricing/details/virtual-machines/windows/</span><span class="sxs-lookup"><span data-stu-id="a3e76-186">[Windows storage-account pricing]: /pricing/details/virtual-machines/windows/</span></span>
<span data-ttu-id="a3e76-187">[Linux-tárfiók díjszabása]: /pricing/details/virtual-machines/linux/</span><span class="sxs-lookup"><span data-stu-id="a3e76-187">[Linux storage-account pricing]: /pricing/details/virtual-machines/linux/</span></span>

<!-- IMG List -->

[CS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png
