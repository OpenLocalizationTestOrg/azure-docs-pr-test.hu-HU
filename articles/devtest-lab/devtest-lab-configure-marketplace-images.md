---
title: "Azure piactér kép beállításainak konfigurálása az Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Mely Azure piactéren elérhető rendszerkép is használható, ha a virtuális gép létrehozása az Azure DevTest Labs konfigurálása"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 804c6af2-17e9-4320-af3a-f454bd398379
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: 5f888c9d92a9164cc7d3d1aed66c29a724b365d7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a><span data-ttu-id="770ec-103">Az Azure DevTest Labs Azure piactér kép beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="770ec-103">Configure Azure Marketplace image settings in Azure DevTest Labs</span></span>
<span data-ttu-id="770ec-104">DevTest Labs létrehozása virtuális gépek Azure piactéren elérhető rendszerkép attól függően, hogy hogyan konfigurálta az Azure piactéren elérhető rendszerkép alapján használhatók a labor támogatja.</span><span class="sxs-lookup"><span data-stu-id="770ec-104">DevTest Labs supports creating VMs based on Azure Marketplace images depending on how you have configured Azure Marketplace images to be used in your lab.</span></span> <span data-ttu-id="770ec-105">Ez a cikk bemutatja, hogyan határozhatja meg, ha bármely, az Azure piactéren elérhető rendszerkép lehet egy, amikor a virtuális gépek létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="770ec-105">This article shows you how to specify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span> <span data-ttu-id="770ec-106">Ez biztosítja, hogy a csoport csak fér hozzá a piactéren elérhető rendszerkép van szükségük.</span><span class="sxs-lookup"><span data-stu-id="770ec-106">This ensures that your team only has access to the Marketplace images they need.</span></span> 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a><span data-ttu-id="770ec-107">Válassza ki, melyik Azure piactéren elérhető rendszerkép vannak engedélyezve, ha a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="770ec-107">Select which Azure Marketplace images are allowed when creating a VM</span></span>
1. <span data-ttu-id="770ec-108">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="770ec-108">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="770ec-109">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** a listából.</span><span class="sxs-lookup"><span data-stu-id="770ec-109">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="770ec-110">Válassza ki a kívánt labor labs listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="770ec-110">From the list of labs, select the desired lab.</span></span> 
4. <span data-ttu-id="770ec-111">A labor paneljén válassza **konfigurációs és házirendek**.</span><span class="sxs-lookup"><span data-stu-id="770ec-111">On the lab's blade, select **Configuration and policies**.</span></span>
5. <span data-ttu-id="770ec-112">A tesztlabor a **konfigurációs és házirendek** részen **virtuális gép körrel**, jelölje be **piactéren elérhető rendszerkép**.</span><span class="sxs-lookup"><span data-stu-id="770ec-112">On lab's **Configuration and policies** blade under **Virtual Machine Bases**, select **Marketplace images**.</span></span>
6. <span data-ttu-id="770ec-113">Adja meg, hogy minden a minősített Azure piactéren elérhető rendszerkép alapjaként egy új virtuális gép számára elérhető legyen.</span><span class="sxs-lookup"><span data-stu-id="770ec-113">Specify whether you want all the qualified Azure Marketplace images to be available for use as a base of a new VM.</span></span> <span data-ttu-id="770ec-114">Ha **Igen**, majd az Azure piactéren lemezképeket, hogy megfelel a következő feltételek engedélyezettek a laborban:</span><span class="sxs-lookup"><span data-stu-id="770ec-114">If you select **Yes**, then all the Azure Marketplace images that meet all the following criteria are allowed in the lab:</span></span>
   
   * <span data-ttu-id="770ec-115">A kép létrehoz egy virtuális, **és**</span><span class="sxs-lookup"><span data-stu-id="770ec-115">The image creates a single VM, **and**</span></span>
   * <span data-ttu-id="770ec-116">A kép Azure Resource Managert használja a virtuális gépek, a rendelkezésre **és**</span><span class="sxs-lookup"><span data-stu-id="770ec-116">The image uses Azure Resource Manager to provision VMs, **and**</span></span>
   * <span data-ttu-id="770ec-117">A kép nincs szükség, egy extra licenccsomagban megvásárlásáról</span><span class="sxs-lookup"><span data-stu-id="770ec-117">The image doesn't require purchasing an extra licensing plan</span></span>
     
    <span data-ttu-id="770ec-118">Ha azt szeretné, hogy a kép nem számára engedélyezett, vagy meg szeretné határozni mely képek is használható, jelölje be **nem**.</span><span class="sxs-lookup"><span data-stu-id="770ec-118">If you want no images to be allowed, or you want to specify which images can be used, select **No**.</span></span>
     
     ![A beállítást, minden piactéren elérhető rendszerkép virtuális gépek portbesorolása alap képként](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. <span data-ttu-id="770ec-120">Ha **nem** az előző lépésben a **engedélyezett képek/válassza ki az összes** jelölőnégyzet engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="770ec-120">If you select **No** to the previous step, the **Allowed images/Select all** checkbox is enabled.</span></span> 
   <span data-ttu-id="770ec-121">Segítségével ezt a beállítást a keresőmezőbe együtt gyorsan válassza ki, vagy kapcsolja ki a listában megjelenített összes elemet.</span><span class="sxs-lookup"><span data-stu-id="770ec-121">You can use this option together with the search box to quickly select or deselect all the items displayed in the list.</span></span>
   * <span data-ttu-id="770ec-122">Válassza ki az Azure piactéren elérhető rendszerkép szeretné engedélyezni a virtuális gép létrehozásához egyenként mindegyik lemezkép operációs rendszerének megfelelő jelölőnégyzet bejelölésével.</span><span class="sxs-lookup"><span data-stu-id="770ec-122">Select the Azure Marketplace images you want to allow for VM creation individually by checking each image's corresponding checkbox.</span></span>
   * <span data-ttu-id="770ec-123">Válassza ki semmi a listából, ha nem kívánja a laborban használandó bármely Azure piactéren elérhető rendszerkép engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="770ec-123">Select nothing from the list if you don't want to allow any Azure Marketplace images to be used in the lab.</span></span>
   
    ![Megadhatja a virtuális gépek mely Azure piactéren elérhető rendszerkép használható alap képként](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="770ec-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="770ec-125">Next steps</span></span>
<span data-ttu-id="770ec-126">Miután konfigurálta, hogy Azure piactéren elérhető rendszerkép engedélyezettek, ha a virtuális gép létrehozása, a következő lépés, hogy [a virtuális gépek hozzáadása a labor](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="770ec-126">Once you have configured how Azure Marketplace images are allowed when creating a VM, the next step is to [add a VM to your lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

