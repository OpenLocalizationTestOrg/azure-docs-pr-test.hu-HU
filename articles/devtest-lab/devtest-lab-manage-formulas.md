---
title: "A virtuális gépek létrehozásához Azure DevTest Labs szolgáltatásban képletek kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan frissítése és eltávolítása az Azure DevTest Labs képletek"
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
ms.openlocfilehash: bfdab5def50158f9b764bbb1e50c2624cc6d5fb3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-devtest-labs-formulas"></a><span data-ttu-id="a37f4-103">Azure DevTest Labs képletek kezelése</span><span class="sxs-lookup"><span data-stu-id="a37f4-103">Manage Azure DevTest Labs formulas</span></span>

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

<span data-ttu-id="a37f4-104">Ez a cikk bemutatja, hogyan képlet base (egyéni lemezképet, Piactéri lemezképhez vagy egy másik képlet) vagy egy meglévő virtuális gép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a37f4-104">This article illustrates how to create a formula from either a base (custom image, Marketplace image, or another formula) or an existing VM.</span></span> <span data-ttu-id="a37f4-105">Ez a cikk is végigvezeti Önt meglévő képletek kezelése.</span><span class="sxs-lookup"><span data-stu-id="a37f4-105">This article also guides you through managing existing formulas.</span></span>

## <a name="create-a-formula"></a><span data-ttu-id="a37f4-106">Létrehozhat egy képletet</span><span class="sxs-lookup"><span data-stu-id="a37f4-106">Create a formula</span></span>
<span data-ttu-id="a37f4-107">Bárki, aki DevTest Labs *felhasználók* engedélyek létre tudja hozni a virtuális gépek képlettel alapjaként.</span><span class="sxs-lookup"><span data-stu-id="a37f4-107">Anyone with DevTest Labs *Users* permissions is able to create VMs using a formula as a base.</span></span> <span data-ttu-id="a37f4-108">Képletek létrehozása két módja van:</span><span class="sxs-lookup"><span data-stu-id="a37f4-108">There are two ways to create formulas:</span></span> 

* <span data-ttu-id="a37f4-109">A base - használja, ha be szeretné állítani a képlet jellemzői.</span><span class="sxs-lookup"><span data-stu-id="a37f4-109">From a base - Use when you want to define all the characteristics of the formula.</span></span>
* <span data-ttu-id="a37f4-110">Az egy meglévő Virtuálisgép - tesztkörnyezet használja, ha szeretne létrehozni egy képletet egy meglévő virtuális gép beállításai alapján.</span><span class="sxs-lookup"><span data-stu-id="a37f4-110">From an existing lab VM - Use when you want to create a formula based on the settings of an existing VM.</span></span>

<span data-ttu-id="a37f4-111">Felhasználók és engedélyek hozzáadásával kapcsolatos további információkért lásd: [tulajdonosa és a felhasználók hozzáadása az Azure DevTest Labs](./devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="a37f4-111">For more information about adding users and permissions, see [Add owners and users in Azure DevTest Labs](./devtest-lab-add-devtest-user.md).</span></span>

### <a name="create-a-formula-from-a-base"></a><span data-ttu-id="a37f4-112">A base létrehozhat egy képletet</span><span class="sxs-lookup"><span data-stu-id="a37f4-112">Create a formula from a base</span></span>
<span data-ttu-id="a37f4-113">A következő lépések végigvezetik egy egyéni lemezképet, Piactéri lemezképhez vagy egy másik képlet képlet létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="a37f4-113">The following steps guide you through the process of creating a formula from a custom image, Marketplace image, or another formula.</span></span>

1. <span data-ttu-id="a37f4-114">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="a37f4-114">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

2. <span data-ttu-id="a37f4-115">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** a listából.</span><span class="sxs-lookup"><span data-stu-id="a37f4-115">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>

3. <span data-ttu-id="a37f4-116">Válassza ki a kívánt labor labs listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="a37f4-116">From the list of labs, select the desired lab.</span></span>  

4. <span data-ttu-id="a37f4-117">A labor paneljén válassza **képletek (újrafelhasználható körrel)**.</span><span class="sxs-lookup"><span data-stu-id="a37f4-117">On the lab's blade, select **Formulas (reusable bases)**.</span></span>
   
    ![Képletadat menü](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. <span data-ttu-id="a37f4-119">Az a **képletek** panelen válassza **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="a37f4-119">On the **Formulas** blade, select **+ Add**.</span></span>
   
    ![A képlet hozzáadása](./media/devtest-lab-create-formulas/add-formula.png)

6. <span data-ttu-id="a37f4-121">Az a **base válasszon** panelen válassza ki, amelyből el kívánja az alaposztály (egyéni lemezképet, Piactéri lemezképhez vagy képlet) létrehozása a képlet.</span><span class="sxs-lookup"><span data-stu-id="a37f4-121">On the **Choose a base** blade, select the base (custom image, Marketplace image, or formula) from which you want to create the formula.</span></span>
   
    ![Kiinduló lista](./media/devtest-lab-create-formulas/base-list.png)

7. <span data-ttu-id="a37f4-123">Az a **képletet** panelen adja meg a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="a37f4-123">On the **Create formula** blade, specify the following values:</span></span>
   
    * <span data-ttu-id="a37f4-124">**Képlet neve** -adja meg a képlet nevét.</span><span class="sxs-lookup"><span data-stu-id="a37f4-124">**Formula name** - Enter a name for your formula.</span></span> <span data-ttu-id="a37f4-125">Ez az érték alap képek listája jelenik meg a virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="a37f4-125">This value is displayed in the list of base images when you create a VM.</span></span> <span data-ttu-id="a37f4-126">A név van hitelesítve, írja be azt, és nem érvényes, ha egy üzenet azt jelzi-e egy érvényes nevet a követelményei.</span><span class="sxs-lookup"><span data-stu-id="a37f4-126">The name is validated as you type it, and if not valid, a message indicates the requirements for a valid name.</span></span>
    * <span data-ttu-id="a37f4-127">**Leírás** -adja meg a képlet beszédes leírást.</span><span class="sxs-lookup"><span data-stu-id="a37f4-127">**Description** - Enter a meaningful description for your formula.</span></span> <span data-ttu-id="a37f4-128">Ezt az értéket a virtuális gép létrehozása esetén a képlet helyi menüjéből érhető el.</span><span class="sxs-lookup"><span data-stu-id="a37f4-128">This value is available from the formula's context menu when you create a VM.</span></span>
    * <span data-ttu-id="a37f4-129">**Felhasználónév** -adjon meg egy felhasználónevet, amely rendszergazdai jogosultságokkal engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="a37f4-129">**User name** - Enter a user name that is granted administrator privileges.</span></span>
    * <span data-ttu-id="a37f4-130">**Jelszó** – írja be - vagy a legördülő listából válassza ki - társított a titkos kulcsot (jelszó), amely a megadott felhasználó használni kívánt értéket.</span><span class="sxs-lookup"><span data-stu-id="a37f4-130">**Password** - Enter - or select from the dropdown - a value that is associated with the secret (password) that you want to use for the specified user.</span></span> <span data-ttu-id="a37f4-131">A titkos kulcsok kapcsolatos további információkért lásd: [Azure DevTest Labs: titkos tárolójának](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).</span><span class="sxs-lookup"><span data-stu-id="a37f4-131">For more information about the secrets, see [Azure DevTest Labs: Personal secret store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).</span></span>
    * <span data-ttu-id="a37f4-132">**Virtuális gép lemeztípus** : Adja meg vagy HDD (merevlemez-meghajtóra), vagy SSD (SSD-meghajtóra), milyen típusú jelzi a virtuális gépek üzembe helyezve az alapjául szolgáló lemezképhez használata esetén engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="a37f4-132">**Virtual machine disk type** - Specify either HDD (hard-disk drive) or SSD (solid-state drive) to indicate which storage disk type is allowed for the virtual machines provisioned using this base image.</span></span>
    * <span data-ttu-id="a37f4-133">** Virtuális gép mérete ** – válasszon ki egy előre meghatározott elemek adja meg a Processzormagok, RAM memória méretét és a merevlemez mérete a virtuális gép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a37f4-133">** Virtual machine size** - Select one of the predefined items that specify the processor cores, RAM size, and the hard drive size of the VM to create.</span></span> 
    * <span data-ttu-id="a37f4-134">**Az összetevők** - megnyitásához válassza a **vegye fel az összetevők** panel, ahol válassza ki, és az alapjául szolgáló lemezképhez hozzáadni kívánt az összetevők konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a37f4-134">**Artifacts** - Select to open the **Add artifacts** blade, in which you select and configure the artifacts that you want to add to the base image.</span></span> <span data-ttu-id="a37f4-135">Az összetevők kapcsolatos további információkért lásd: [kezelése Virtuálisgép-összetevők a Azure DevTest Labs szolgáltatásban](./devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="a37f4-135">For more information about artifacts, see [Manage VM artifacts in Azure DevTest Labs](./devtest-lab-add-vm-with-artifacts.md).</span></span>
    * <span data-ttu-id="a37f4-136">**Speciális beállítások** – Itt adhatja meg megnyitni a **speciális** panel, ahol konfigurálhatja a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="a37f4-136">**Advanced settings** - Select to open the **Advanced** blade where you configure the following settings:</span></span>
        * <span data-ttu-id="a37f4-137">**Virtuális hálózati** -adja meg a kívánt virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="a37f4-137">**Virtual network** - Specify the desired virtual network.</span></span>
        * <span data-ttu-id="a37f4-138">**Alhálózati** -adja meg a kívánt alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="a37f4-138">**Subnet** - Specify the desired subnet.</span></span>    
        * <span data-ttu-id="a37f4-139">**IP-címkonfigurációt** -adja meg, ha a Public, Private vagy megosztott IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="a37f4-139">**IP address configuration** - Specify if you want the Public, Private, or Shared IP addresses.</span></span> <span data-ttu-id="a37f4-140">További információ a megosztott IP-címek: [megértése megosztott IP-címek az Azure DevTest Labs](./devtest-lab-shared-ip.md).</span><span class="sxs-lookup"><span data-stu-id="a37f4-140">For more information about shared IP addresses, see [Understand shared IP addresses in Azure DevTest Labs](./devtest-lab-shared-ip.md).</span></span>
        * <span data-ttu-id="a37f4-141">**Ellenőrizze a gép claimable** -és a gép "claimable" azt jelenti, hogy a rendszer nem hozzárendel tulajdonjoga létrehozásának időpontjában.</span><span class="sxs-lookup"><span data-stu-id="a37f4-141">**Make this machine claimable** - Making a machine "claimable" means that it will not be assigned ownership at the time of creation.</span></span> <span data-ttu-id="a37f4-142">Ehelyett labor felhasználók fognak tudni tulajdonba ("jogcím") a gép a labor panelen.</span><span class="sxs-lookup"><span data-stu-id="a37f4-142">Instead lab users will be able to take ownership ("claim") the machine in the lab's blade.</span></span>     
    * <span data-ttu-id="a37f4-143">**Kép** – Ez a mező neve az alapjául szolgáló lemezképhez, választotta az előző panel.</span><span class="sxs-lookup"><span data-stu-id="a37f4-143">**Image** - This field displays name of the base image you selected on the previous blade.</span></span> 
     
       ![Képlet létrehozása](./media/devtest-lab-create-formulas/create-formula.png)

8. <span data-ttu-id="a37f4-145">Válassza ki **létrehozása** a képlet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a37f4-145">Select **Create** to create the formula.</span></span>

9. <span data-ttu-id="a37f4-146">A képlet létrehozásakor megjeleníti a listában a **képletek** panelen.</span><span class="sxs-lookup"><span data-stu-id="a37f4-146">When the formula has been created, it displays in the list on the **Formulas** blade.</span></span>

### <a name="create-a-formula-from-a-vm"></a><span data-ttu-id="a37f4-147">Létrehozhat egy képletet a virtuális gépről</span><span class="sxs-lookup"><span data-stu-id="a37f4-147">Create a formula from a VM</span></span>
<span data-ttu-id="a37f4-148">A következő lépések végigvezetik a meglévő virtuális alapuló képlet létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="a37f4-148">The following steps guide you through the process of creating a formula based on an existing VM.</span></span> 

> [!NOTE]
> <span data-ttu-id="a37f4-149">A képletet a virtuális gépről, a virtuális gép kell létrehozni után 2016. március 30.</span><span class="sxs-lookup"><span data-stu-id="a37f4-149">To create a formula from a VM, the VM must have been created after March 30, 2016.</span></span> 
> 
> 

1. <span data-ttu-id="a37f4-150">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="a37f4-150">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="a37f4-151">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** a listából.</span><span class="sxs-lookup"><span data-stu-id="a37f4-151">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="a37f4-152">Válassza ki a kívánt labor labs listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="a37f4-152">From the list of labs, select the desired lab.</span></span>  
4. <span data-ttu-id="a37f4-153">A tesztlabor a **áttekintése** panelen válassza ki a virtuális gép, amelyből létre szeretne hozni a képlet.</span><span class="sxs-lookup"><span data-stu-id="a37f4-153">On the lab's **Overview** blade, select the VM from which you wish to create the formula.</span></span>
   
    ![Labs virtuális gépek](./media/devtest-lab-create-formulas/my-vms.png)
5. <span data-ttu-id="a37f4-155">A virtuális gép paneljén válassza **képletet (újrafelhasználható alap)**.</span><span class="sxs-lookup"><span data-stu-id="a37f4-155">On the VM's blade, select **Create formula (reusable base)**.</span></span>
   
    ![Képlet létrehozása](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. <span data-ttu-id="a37f4-157">A a **képletet** panelen adjon meg egy **neve** és **leírás** az új képlet.</span><span class="sxs-lookup"><span data-stu-id="a37f4-157">On the **Create formula** blade, enter a **Name** and **Description** for your new formula.</span></span>
   
    ![Képletadat panel létrehozása](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. <span data-ttu-id="a37f4-159">Válassza ki **OK** a képlet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a37f4-159">Select **OK** to create the formula.</span></span>

## <a name="modify-a-formula"></a><span data-ttu-id="a37f4-160">A képlet módosítása</span><span class="sxs-lookup"><span data-stu-id="a37f4-160">Modify a formula</span></span>
<span data-ttu-id="a37f4-161">A képlet módosításához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a37f4-161">To modify a formula, follow these steps:</span></span>

1. <span data-ttu-id="a37f4-162">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="a37f4-162">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="a37f4-163">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** a listából.</span><span class="sxs-lookup"><span data-stu-id="a37f4-163">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="a37f4-164">Válassza ki a kívánt labor labs listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="a37f4-164">From the list of labs, select the desired lab.</span></span>  
4. <span data-ttu-id="a37f4-165">A labor paneljén válassza **képletek (újrafelhasználható körrel)**.</span><span class="sxs-lookup"><span data-stu-id="a37f4-165">On the lab's blade, select **Formulas (reusable bases)**.</span></span>
   
    ![Képletadat menü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. <span data-ttu-id="a37f4-167">Az a **labor képletek** panelen válassza ki a módosítani kívánt képletet.</span><span class="sxs-lookup"><span data-stu-id="a37f4-167">On the **Lab formulas** blade, select the formula you wish to modify.</span></span>
6. <span data-ttu-id="a37f4-168">Az a **képlet frissítése** panelen elvégezni a kívánt módosításokat, és válassza ki **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="a37f4-168">On the **Update formula** blade, make the desired edits, and select **Update**.</span></span>

## <a name="delete-a-formula"></a><span data-ttu-id="a37f4-169">Képlet törlése</span><span class="sxs-lookup"><span data-stu-id="a37f4-169">Delete a formula</span></span>
<span data-ttu-id="a37f4-170">A képlet törléséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a37f4-170">To delete a formula, follow these steps:</span></span>

1. <span data-ttu-id="a37f4-171">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="a37f4-171">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="a37f4-172">Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** a listából.</span><span class="sxs-lookup"><span data-stu-id="a37f4-172">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="a37f4-173">Válassza ki a kívánt labor labs listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="a37f4-173">From the list of labs, select the desired lab.</span></span>  
4. <span data-ttu-id="a37f4-174">A tesztlabor a **beállítások** panelen válassza **képletek**.</span><span class="sxs-lookup"><span data-stu-id="a37f4-174">On the lab **Settings** blade, select **Formulas**.</span></span>
   
    ![Képletadat menü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. <span data-ttu-id="a37f4-176">Az a **labor képletek** panelen válassza ki a törölni kívánt képlet jobb a három pont.</span><span class="sxs-lookup"><span data-stu-id="a37f4-176">On the **Lab formulas** blade, select the ellipsis to the right of the formula you wish to delete.</span></span>
   
    ![Képletadat menü](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. <span data-ttu-id="a37f4-178">Válassza ki a képletet helyi menüben, **törlése**.</span><span class="sxs-lookup"><span data-stu-id="a37f4-178">On the formula's context menu, select **Delete**.</span></span>
   
    ![Képletadat helyi menü](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. <span data-ttu-id="a37f4-180">Válassza ki **Igen** a törlési megerősítés párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a37f4-180">Select **Yes** to the deletion confirmation dialog.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="a37f4-181">Kapcsolódó blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="a37f4-181">Related blog posts</span></span>
* [<span data-ttu-id="a37f4-182">Egyéni lemezképek vagy képletek?</span><span class="sxs-lookup"><span data-stu-id="a37f4-182">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a><span data-ttu-id="a37f4-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a37f4-183">Next steps</span></span>
<span data-ttu-id="a37f4-184">Miután létrehozta a képlet használható virtuális gép létrehozásakor, a következő lépés, hogy [a virtuális gépek hozzáadása a labor](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="a37f4-184">Once you have created a formula for use when creating a VM, the next step is to [add a VM to your lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

