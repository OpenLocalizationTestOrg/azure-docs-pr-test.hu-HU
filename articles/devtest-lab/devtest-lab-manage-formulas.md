---
title: "a virtuális gépek Azure DevTest Labs toocreate aaaManage képletek |} Microsoft Docs"
description: "Megtudhatja, hogyan tooupdate és eltávolítás Azure DevTest Labs képlet"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 855debe46f3b70cc45ea5d55869663b64e225124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-devtest-labs-formulas"></a><span data-ttu-id="8d859-103">Azure DevTest Labs képletek kezelése</span><span class="sxs-lookup"><span data-stu-id="8d859-103">Manage Azure DevTest Labs formulas</span></span>

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

<span data-ttu-id="8d859-104">Ez a cikk bemutatja, hogyan toocreate base (egyéni lemezképet, Piactéri lemezképhez vagy egy másik képlet) vagy a meglévő virtuális képletet.</span><span class="sxs-lookup"><span data-stu-id="8d859-104">This article illustrates how toocreate a formula from either a base (custom image, Marketplace image, or another formula) or an existing VM.</span></span> <span data-ttu-id="8d859-105">Ez a cikk is végigvezeti Önt meglévő képletek kezelése.</span><span class="sxs-lookup"><span data-stu-id="8d859-105">This article also guides you through managing existing formulas.</span></span>

## <a name="create-a-formula"></a><span data-ttu-id="8d859-106">Létrehozhat egy képletet</span><span class="sxs-lookup"><span data-stu-id="8d859-106">Create a formula</span></span>
<span data-ttu-id="8d859-107">Bárki, aki DevTest Labs *felhasználók* engedélyek képes toocreate virtuális gépek képlettel alapjaként.</span><span class="sxs-lookup"><span data-stu-id="8d859-107">Anyone with DevTest Labs *Users* permissions is able toocreate VMs using a formula as a base.</span></span> <span data-ttu-id="8d859-108">Két módon toocreate képletek:</span><span class="sxs-lookup"><span data-stu-id="8d859-108">There are two ways toocreate formulas:</span></span> 

* <span data-ttu-id="8d859-109">A base - használni, ha szeretné toodefine hello képlet összes hello jellemzőit.</span><span class="sxs-lookup"><span data-stu-id="8d859-109">From a base - Use when you want toodefine all hello characteristics of hello formula.</span></span>
* <span data-ttu-id="8d859-110">Az egy meglévő Virtuálisgép - tesztkörnyezet használja, ha azt szeretné, hogy toocreate képlet beállításai alapján a hello egy meglévő virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="8d859-110">From an existing lab VM - Use when you want toocreate a formula based on hello settings of an existing VM.</span></span>

<span data-ttu-id="8d859-111">Felhasználók és engedélyek hozzáadásával kapcsolatos további információkért lásd: [tulajdonosa és a felhasználók hozzáadása az Azure DevTest Labs](./devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="8d859-111">For more information about adding users and permissions, see [Add owners and users in Azure DevTest Labs](./devtest-lab-add-devtest-user.md).</span></span>

### <a name="create-a-formula-from-a-base"></a><span data-ttu-id="8d859-112">A base létrehozhat egy képletet</span><span class="sxs-lookup"><span data-stu-id="8d859-112">Create a formula from a base</span></span>
<span data-ttu-id="8d859-113">a lépéseket követve hello hello folyamatot, amely egy egyéni lemezképet, Piactéri lemezképhez vagy egy másik képlet képletek létrehozását ismerteti.</span><span class="sxs-lookup"><span data-stu-id="8d859-113">hello following steps guide you through hello process of creating a formula from a custom image, Marketplace image, or another formula.</span></span>

1. <span data-ttu-id="8d859-114">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="8d859-114">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

2. <span data-ttu-id="8d859-115">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="8d859-115">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>

3. <span data-ttu-id="8d859-116">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="8d859-116">From hello list of labs, select hello desired lab.</span></span>  

4. <span data-ttu-id="8d859-117">Hello labor paneljén válassza **képletek (újrafelhasználható körrel)**.</span><span class="sxs-lookup"><span data-stu-id="8d859-117">On hello lab's blade, select **Formulas (reusable bases)**.</span></span>
   
    ![Képletadat menü](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. <span data-ttu-id="8d859-119">A hello **képletek** panelen válassza **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="8d859-119">On hello **Formulas** blade, select **+ Add**.</span></span>
   
    ![A képlet hozzáadása](./media/devtest-lab-create-formulas/add-formula.png)

6. <span data-ttu-id="8d859-121">A hello **base válasszon** panelen, jelölje be hello alapja (egyéni lemezképet, Piactéri lemezképhez vagy képlet), amelyből el kívánja toocreate hello képlet.</span><span class="sxs-lookup"><span data-stu-id="8d859-121">On hello **Choose a base** blade, select hello base (custom image, Marketplace image, or formula) from which you want toocreate hello formula.</span></span>
   
    ![Kiinduló lista](./media/devtest-lab-create-formulas/base-list.png)

7. <span data-ttu-id="8d859-123">A hello **képletet** panelen adja meg a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="8d859-123">On hello **Create formula** blade, specify hello following values:</span></span>
   
    * <span data-ttu-id="8d859-124">**Képlet neve** -adja meg a képlet nevét.</span><span class="sxs-lookup"><span data-stu-id="8d859-124">**Formula name** - Enter a name for your formula.</span></span> <span data-ttu-id="8d859-125">Ez az érték alap képek hello listája jelenik meg a virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="8d859-125">This value is displayed in hello list of base images when you create a VM.</span></span> <span data-ttu-id="8d859-126">Írja be azt, és nem érvényes, ha egy üzenet azt jelzi-e egy érvényes nevet hello követelményei hello nevének érvényességét.</span><span class="sxs-lookup"><span data-stu-id="8d859-126">hello name is validated as you type it, and if not valid, a message indicates hello requirements for a valid name.</span></span>
    * <span data-ttu-id="8d859-127">**Leírás** -adja meg a képlet beszédes leírást.</span><span class="sxs-lookup"><span data-stu-id="8d859-127">**Description** - Enter a meaningful description for your formula.</span></span> <span data-ttu-id="8d859-128">Ezt az értéket a virtuális gép létrehozása esetén hello képlet helyi menüjéből érhető el.</span><span class="sxs-lookup"><span data-stu-id="8d859-128">This value is available from hello formula's context menu when you create a VM.</span></span>
    * <span data-ttu-id="8d859-129">**Felhasználónév** -adjon meg egy felhasználónevet, amely rendszergazdai jogosultságokkal engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="8d859-129">**User name** - Enter a user name that is granted administrator privileges.</span></span>
    * <span data-ttu-id="8d859-130">**Jelszó** – írja be - vagy hello legördülő menüből válassza - hello titkos kulcs (jelszó), amelyet az toouse hello megadott felhasználó társított egy értéket.</span><span class="sxs-lookup"><span data-stu-id="8d859-130">**Password** - Enter - or select from hello dropdown - a value that is associated with hello secret (password) that you want toouse for hello specified user.</span></span> <span data-ttu-id="8d859-131">Hello titkok kapcsolatos további információkért lásd: [Azure DevTest Labs: titkos tárolójának](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).</span><span class="sxs-lookup"><span data-stu-id="8d859-131">For more information about hello secrets, see [Azure DevTest Labs: Personal secret store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).</span></span>
    * <span data-ttu-id="8d859-132">**Virtuális gép lemeztípus** : Adja meg vagy HDD (merevlemez-meghajtóra), vagy mely tárolási lemez típusa (SSD-meghajtóra) SSD tooindicate hello virtuális gépek üzembe helyezve az alapjául szolgáló lemezképhez használata esetén engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="8d859-132">**Virtual machine disk type** - Specify either HDD (hard-disk drive) or SSD (solid-state drive) tooindicate which storage disk type is allowed for hello virtual machines provisioned using this base image.</span></span>
    * <span data-ttu-id="8d859-133">** Virtuális gép mérete ** – hello processzormag, a RAM memória méretét és a merevlemez méretének hello hello VM toocreate meghatározó előre definiált hello elemek közül.</span><span class="sxs-lookup"><span data-stu-id="8d859-133">** Virtual machine size** - Select one of hello predefined items that specify hello processor cores, RAM size, and hello hard drive size of hello VM toocreate.</span></span> 
    * <span data-ttu-id="8d859-134">**Az összetevők** -válassza tooopen hello **vegye fel az összetevők** , amelyben akkor válassza ki, és beállíthatja, amelyet az tooadd toohello alapjául szolgáló lemezképhez hello összetevők paneljén.</span><span class="sxs-lookup"><span data-stu-id="8d859-134">**Artifacts** - Select tooopen hello **Add artifacts** blade, in which you select and configure hello artifacts that you want tooadd toohello base image.</span></span> <span data-ttu-id="8d859-135">Az összetevők kapcsolatos további információkért lásd: [kezelése Virtuálisgép-összetevők a Azure DevTest Labs szolgáltatásban](./devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="8d859-135">For more information about artifacts, see [Manage VM artifacts in Azure DevTest Labs](./devtest-lab-add-vm-with-artifacts.md).</span></span>
    * <span data-ttu-id="8d859-136">**Speciális beállítások** -válassza tooopen hello **speciális** hello a következő beállítások konfigurálására szolgáló panel:</span><span class="sxs-lookup"><span data-stu-id="8d859-136">**Advanced settings** - Select tooopen hello **Advanced** blade where you configure hello following settings:</span></span>
        * <span data-ttu-id="8d859-137">**Virtuális hálózati** -adja meg a hello virtuális hálózat szükséges.</span><span class="sxs-lookup"><span data-stu-id="8d859-137">**Virtual network** - Specify hello desired virtual network.</span></span>
        * <span data-ttu-id="8d859-138">**Alhálózati** -szükséges hello alhálózatot adjon meg.</span><span class="sxs-lookup"><span data-stu-id="8d859-138">**Subnet** - Specify hello desired subnet.</span></span>    
        * <span data-ttu-id="8d859-139">**IP-címkonfigurációt** -adja meg, ha hello Public, Private vagy megosztott IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="8d859-139">**IP address configuration** - Specify if you want hello Public, Private, or Shared IP addresses.</span></span> <span data-ttu-id="8d859-140">További információ a megosztott IP-címek: [megértése megosztott IP-címek az Azure DevTest Labs](./devtest-lab-shared-ip.md).</span><span class="sxs-lookup"><span data-stu-id="8d859-140">For more information about shared IP addresses, see [Understand shared IP addresses in Azure DevTest Labs](./devtest-lab-shared-ip.md).</span></span>
        * <span data-ttu-id="8d859-141">**Ellenőrizze a gép claimable** -és a gép "claimable" azt jelenti, hogy a rendszer nem hozzárendel tulajdonjoga hello létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="8d859-141">**Make this machine claimable** - Making a machine "claimable" means that it will not be assigned ownership at hello time of creation.</span></span> <span data-ttu-id="8d859-142">Ehelyett a labor felhasználók fognak képes tootake tulajdonosi ("jogcím") hello gép hello labor panelen.</span><span class="sxs-lookup"><span data-stu-id="8d859-142">Instead lab users will be able tootake ownership ("claim") hello machine in hello lab's blade.</span></span>     
    * <span data-ttu-id="8d859-143">**Kép** – Ez a mező neve hello alapjául szolgáló lemezképhez kiválasztott hello előző panelen.</span><span class="sxs-lookup"><span data-stu-id="8d859-143">**Image** - This field displays name of hello base image you selected on hello previous blade.</span></span> 
     
       ![Képlet létrehozása](./media/devtest-lab-create-formulas/create-formula.png)

8. <span data-ttu-id="8d859-145">Válassza ki **létrehozása** toocreate hello képlet.</span><span class="sxs-lookup"><span data-stu-id="8d859-145">Select **Create** toocreate hello formula.</span></span>

9. <span data-ttu-id="8d859-146">Hello képlet létrehozásakor hello hello listája megjeleníti **képletek** panelen.</span><span class="sxs-lookup"><span data-stu-id="8d859-146">When hello formula has been created, it displays in hello list on hello **Formulas** blade.</span></span>

### <a name="create-a-formula-from-a-vm"></a><span data-ttu-id="8d859-147">Létrehozhat egy képletet a virtuális gépről</span><span class="sxs-lookup"><span data-stu-id="8d859-147">Create a formula from a VM</span></span>
<span data-ttu-id="8d859-148">hello következő lépések végigvezetik hello egy meglévő virtuális Gépen alapuló képlet létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="8d859-148">hello following steps guide you through hello process of creating a formula based on an existing VM.</span></span> 

> [!NOTE]
> <span data-ttu-id="8d859-149">egy virtuális géptől képlet toocreate, hello VM kell létrehozni után, 2016. március 30..</span><span class="sxs-lookup"><span data-stu-id="8d859-149">toocreate a formula from a VM, hello VM must have been created after March 30, 2016.</span></span> 
> 
> 

1. <span data-ttu-id="8d859-150">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="8d859-150">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="8d859-151">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="8d859-151">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="8d859-152">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="8d859-152">From hello list of labs, select hello desired lab.</span></span>  
4. <span data-ttu-id="8d859-153">A hello labor **áttekintése** panel, amelyen toocreate hello képlet kívánja válassza hello VM.</span><span class="sxs-lookup"><span data-stu-id="8d859-153">On hello lab's **Overview** blade, select hello VM from which you wish toocreate hello formula.</span></span>
   
    ![Labs virtuális gépek](./media/devtest-lab-create-formulas/my-vms.png)
5. <span data-ttu-id="8d859-155">Hello virtuális gép paneljén válassza **képletet (újrafelhasználható alap)**.</span><span class="sxs-lookup"><span data-stu-id="8d859-155">On hello VM's blade, select **Create formula (reusable base)**.</span></span>
   
    ![Képlet létrehozása](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. <span data-ttu-id="8d859-157">A hello **képletet** panelen adjon meg egy **neve** és **leírása** az új képlet.</span><span class="sxs-lookup"><span data-stu-id="8d859-157">On hello **Create formula** blade, enter a **Name** and **Description** for your new formula.</span></span>
   
    ![Képletadat panel létrehozása](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. <span data-ttu-id="8d859-159">Válassza ki **OK** toocreate hello képlet.</span><span class="sxs-lookup"><span data-stu-id="8d859-159">Select **OK** toocreate hello formula.</span></span>

## <a name="modify-a-formula"></a><span data-ttu-id="8d859-160">A képlet módosítása</span><span class="sxs-lookup"><span data-stu-id="8d859-160">Modify a formula</span></span>
<span data-ttu-id="8d859-161">toomodify képlet, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8d859-161">toomodify a formula, follow these steps:</span></span>

1. <span data-ttu-id="8d859-162">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="8d859-162">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="8d859-163">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="8d859-163">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="8d859-164">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="8d859-164">From hello list of labs, select hello desired lab.</span></span>  
4. <span data-ttu-id="8d859-165">Hello labor paneljén válassza **képletek (újrafelhasználható körrel)**.</span><span class="sxs-lookup"><span data-stu-id="8d859-165">On hello lab's blade, select **Formulas (reusable bases)**.</span></span>
   
    ![Képletadat menü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. <span data-ttu-id="8d859-167">A hello **labor képletek** panelen, jelölje be hello képlet toomodify kívánja.</span><span class="sxs-lookup"><span data-stu-id="8d859-167">On hello **Lab formulas** blade, select hello formula you wish toomodify.</span></span>
6. <span data-ttu-id="8d859-168">A hello **képlet frissítése** panelen szükséges hello Szerkesztés, és válassza ki **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="8d859-168">On hello **Update formula** blade, make hello desired edits, and select **Update**.</span></span>

## <a name="delete-a-formula"></a><span data-ttu-id="8d859-169">Képlet törlése</span><span class="sxs-lookup"><span data-stu-id="8d859-169">Delete a formula</span></span>
<span data-ttu-id="8d859-170">toodelete képlet, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8d859-170">toodelete a formula, follow these steps:</span></span>

1. <span data-ttu-id="8d859-171">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="8d859-171">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="8d859-172">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="8d859-172">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="8d859-173">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="8d859-173">From hello list of labs, select hello desired lab.</span></span>  
4. <span data-ttu-id="8d859-174">A tesztkörnyezet hello **beállítások** panelen válassza **képletek**.</span><span class="sxs-lookup"><span data-stu-id="8d859-174">On hello lab **Settings** blade, select **Formulas**.</span></span>
   
    ![Képletadat menü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. <span data-ttu-id="8d859-176">A hello **labor képletek** panelen, jelölje be hello három pont toohello sarkában hello képlet toodelete kívánja.</span><span class="sxs-lookup"><span data-stu-id="8d859-176">On hello **Lab formulas** blade, select hello ellipsis toohello right of hello formula you wish toodelete.</span></span>
   
    ![Képletadat menü](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. <span data-ttu-id="8d859-178">Hello képlet helyi menüben, válassza ki a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="8d859-178">On hello formula's context menu, select **Delete**.</span></span>
   
    ![Képletadat helyi menü](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. <span data-ttu-id="8d859-180">Válassza ki **Igen** toohello törlési megerősítés.</span><span class="sxs-lookup"><span data-stu-id="8d859-180">Select **Yes** toohello deletion confirmation dialog.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="8d859-181">Kapcsolódó blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="8d859-181">Related blog posts</span></span>
* [<span data-ttu-id="8d859-182">Egyéni lemezképek vagy képletek?</span><span class="sxs-lookup"><span data-stu-id="8d859-182">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a><span data-ttu-id="8d859-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8d859-183">Next steps</span></span>
<span data-ttu-id="8d859-184">Miután létrehozta a képlet használható virtuális gép létrehozásakor, hello tovább túl van-e[VM tooyour labor hozzáadása](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="8d859-184">Once you have created a formula for use when creating a VM, hello next step is too[add a VM tooyour lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

