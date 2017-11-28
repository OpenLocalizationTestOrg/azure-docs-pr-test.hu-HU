---
title: "aaaConfigure Azure piactér lemezkép beállításainak a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
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
ms.openlocfilehash: bb4b7f1c0cbe967bee724f7ee20f64f8c4ea58ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a><span data-ttu-id="a1989-103">Az Azure DevTest Labs Azure piactér kép beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a1989-103">Configure Azure Marketplace image settings in Azure DevTest Labs</span></span>
<span data-ttu-id="a1989-104">DevTest Labs alapján attól függően, hogy hogyan konfigurálta az Azure piactér képek toobe a tesztkörnyezetben használt Azure piactéren elérhető rendszerkép létrehozása virtuális gépek támogatja.</span><span class="sxs-lookup"><span data-stu-id="a1989-104">DevTest Labs supports creating VMs based on Azure Marketplace images depending on how you have configured Azure Marketplace images toobe used in your lab.</span></span> <span data-ttu-id="a1989-105">Ez a cikk bemutatja, hogyan toospecify, amely, ha bármely, az Azure piactéren elérhető rendszerkép lehet egy, amikor a virtuális gépek létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="a1989-105">This article shows you how toospecify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span> <span data-ttu-id="a1989-106">Ez biztosítja, hogy csak rendelkezik hozzáférési toohello piactéren elérhető rendszerkép van szükségük.</span><span class="sxs-lookup"><span data-stu-id="a1989-106">This ensures that your team only has access toohello Marketplace images they need.</span></span> 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a><span data-ttu-id="a1989-107">Válassza ki, melyik Azure piactéren elérhető rendszerkép vannak engedélyezve, ha a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1989-107">Select which Azure Marketplace images are allowed when creating a VM</span></span>
1. <span data-ttu-id="a1989-108">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="a1989-108">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="a1989-109">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="a1989-109">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="a1989-110">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="a1989-110">From hello list of labs, select hello desired lab.</span></span> 
4. <span data-ttu-id="a1989-111">Hello labor paneljén válassza **konfigurációs és házirendek**.</span><span class="sxs-lookup"><span data-stu-id="a1989-111">On hello lab's blade, select **Configuration and policies**.</span></span>
5. <span data-ttu-id="a1989-112">A tesztlabor a **konfigurációs és házirendek** részen **virtuális gép körrel**, jelölje be **piactéren elérhető rendszerkép**.</span><span class="sxs-lookup"><span data-stu-id="a1989-112">On lab's **Configuration and policies** blade under **Virtual Machine Bases**, select **Marketplace images**.</span></span>
6. <span data-ttu-id="a1989-113">Adja meg, hogy az összes hello minősített Azure piactér képek toobe használható egy új virtuális gép alapjaként.</span><span class="sxs-lookup"><span data-stu-id="a1989-113">Specify whether you want all hello qualified Azure Marketplace images toobe available for use as a base of a new VM.</span></span> <span data-ttu-id="a1989-114">Ha **Igen**, majd az összes hello Azure piactéren elérhető rendszerkép összes hello következő feltételeknek megfelelő engedélyezettek hello laborban:</span><span class="sxs-lookup"><span data-stu-id="a1989-114">If you select **Yes**, then all hello Azure Marketplace images that meet all hello following criteria are allowed in hello lab:</span></span>
   
   * <span data-ttu-id="a1989-115">hello lemezképet hoz létre egy virtuális, **és**</span><span class="sxs-lookup"><span data-stu-id="a1989-115">hello image creates a single VM, **and**</span></span>
   * <span data-ttu-id="a1989-116">hello lemezképet használja az Azure Resource Manager tooprovision virtuális gépek, **és**</span><span class="sxs-lookup"><span data-stu-id="a1989-116">hello image uses Azure Resource Manager tooprovision VMs, **and**</span></span>
   * <span data-ttu-id="a1989-117">hello lemezkép nincs szükség, egy extra licenccsomagban megvásárlásáról</span><span class="sxs-lookup"><span data-stu-id="a1989-117">hello image doesn't require purchasing an extra licensing plan</span></span>
     
    <span data-ttu-id="a1989-118">Ha azt szeretné, hogy nincsenek képek toobe engedélyezett, vagy azt szeretné, hogy mely képek is használható, jelölje be toospecify **nem**.</span><span class="sxs-lookup"><span data-stu-id="a1989-118">If you want no images toobe allowed, or you want toospecify which images can be used, select **No**.</span></span>
     
     ![Beállítás tooallow összes Piactéri lemezképet toobe használt alap képként virtuális gépekhez](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. <span data-ttu-id="a1989-120">Ha **nem** toohello előző lépést, hello **engedélyezett képek/válassza ki az összes** jelölőnégyzet engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="a1989-120">If you select **No** toohello previous step, hello **Allowed images/Select all** checkbox is enabled.</span></span> 
   <span data-ttu-id="a1989-121">Ezzel a kapcsolóval együtt hello Keresés mezőbe tooquickly válassza ki, vagy hello listán megjelenített összes hello elemek kijelölésének megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="a1989-121">You can use this option together with hello search box tooquickly select or deselect all hello items displayed in hello list.</span></span>
   * <span data-ttu-id="a1989-122">Válassza ki a kívánt tooallow virtuális gépek létrehozására egyenként mindegyik lemezkép operációs rendszerének megfelelő jelölőnégyzet bejelölésével hello Azure piactéren elérhető rendszerkép.</span><span class="sxs-lookup"><span data-stu-id="a1989-122">Select hello Azure Marketplace images you want tooallow for VM creation individually by checking each image's corresponding checkbox.</span></span>
   * <span data-ttu-id="a1989-123">Válassza ki semmi a hello lista Ha nem szeretné, hogy tooallow bármely Azure piactér képek toobe hello a tesztkörnyezetben használt.</span><span class="sxs-lookup"><span data-stu-id="a1989-123">Select nothing from hello list if you don't want tooallow any Azure Marketplace images toobe used in hello lab.</span></span>
   
    ![Megadhatja a virtuális gépek mely Azure piactéren elérhető rendszerkép használható alap képként](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="a1989-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1989-125">Next steps</span></span>
<span data-ttu-id="a1989-126">Miután konfigurálta, hogy Azure piactéren elérhető rendszerkép engedélyezettek, ha a virtuális gép létrehozása, hello tovább túl van-e[VM tooyour labor hozzáadása](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="a1989-126">Once you have configured how Azure Marketplace images are allowed when creating a VM, hello next step is too[add a VM tooyour lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

