---
title: "az első Windows virtuális gép IIS aaaInstall |} Microsoft Docs"
description: "Az első kísérletezhet az IIS telepítése és megnyitása a 80-as portot a virtuális gép Windows hello Azure-portálon."
keywords: 
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6334ea45-db6b-4e08-abb5-1f98927ebc34
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: cynthn
ms.openlocfilehash: 7cfed6197df058c4569d111ee88961da7c6fe0b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a><span data-ttu-id="6a739-103">A szerepkör telepítése a Windows virtuális gépén kísérletezhet</span><span class="sxs-lookup"><span data-stu-id="6a739-103">Experiment with installing a role on your Windows VM</span></span>
<span data-ttu-id="6a739-104">Miután az első virtuális gép (VM) be és fut, a továbblépés tooinstalling szoftvereinket és szolgáltatásainkat.</span><span class="sxs-lookup"><span data-stu-id="6a739-104">Once you have your first virtual machine (VM) up and running, you can move on tooinstalling software and services.</span></span> <span data-ttu-id="6a739-105">Ebben az oktatóanyagban fogjuk a hello Windows Server virtuális gép tooinstall IIS toouse Kiszolgálókezelő.</span><span class="sxs-lookup"><span data-stu-id="6a739-105">For this tutorial, we are going toouse Server Manager on hello Windows Server VM tooinstall IIS.</span></span> <span data-ttu-id="6a739-106">Ezután létrehozunk egy hálózati biztonsági csoport (NSG) hello Azure portál tooopen port 80 tooIIS forgalom használ.</span><span class="sxs-lookup"><span data-stu-id="6a739-106">Then, we will create a Network Security Group (NSG) using hello Azure portal tooopen port 80 tooIIS traffic.</span></span> 

<span data-ttu-id="6a739-107">Ha még nem hozott létre az első virtuális gép, meg kell lépjen vissza túl[az első Windows rendszerű virtuális gép létrehozása az Azure-portálon hello](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Ez az oktatóanyag folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="6a739-107">If you haven't already created your first VM, you should go back too[Create your first Windows virtual machine in hello Azure portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before continuing with this tutorial.</span></span>

## <a name="make-sure-hello-vm-is-running"></a><span data-ttu-id="6a739-108">Győződjön meg arról, hogy hello virtuális gép fut.</span><span class="sxs-lookup"><span data-stu-id="6a739-108">Make sure hello VM is running</span></span>
1. <span data-ttu-id="6a739-109">Nyissa meg hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6a739-109">Open hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6a739-110">Hello központ menüben kattintson a **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="6a739-110">On hello hub menu, click **Virtual Machines**.</span></span> <span data-ttu-id="6a739-111">Válassza ki a virtuális gép hello hello listából.</span><span class="sxs-lookup"><span data-stu-id="6a739-111">Select hello virtual machine from hello list.</span></span>
3. <span data-ttu-id="6a739-112">Ha hello állapota **leállítva (Deallocated)**, hello kattintson **Start** hello gombjára **Essentials** hello VM panel.</span><span class="sxs-lookup"><span data-stu-id="6a739-112">If hello status is **Stopped (Deallocated)**, click hello **Start** button on hello **Essentials** blade of hello VM.</span></span> <span data-ttu-id="6a739-113">Ha hello állapota **futtató**, továbbléphet a következő lépésben toohello.</span><span class="sxs-lookup"><span data-stu-id="6a739-113">If hello status is **Running**, you can move on toohello next step.</span></span>

## <a name="connect-toohello-virtual-machine-and-sign-in"></a><span data-ttu-id="6a739-114">Csatlakoztassa toohello virtuális gépet, és jelentkezzen be</span><span class="sxs-lookup"><span data-stu-id="6a739-114">Connect toohello virtual machine and sign in</span></span>
1. <span data-ttu-id="6a739-115">Hello központ menüben kattintson a **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="6a739-115">On hello hub menu, click **Virtual Machines**.</span></span> <span data-ttu-id="6a739-116">Válassza ki a virtuális gép hello hello listából.</span><span class="sxs-lookup"><span data-stu-id="6a739-116">Select hello virtual machine from hello list.</span></span>
2. <span data-ttu-id="6a739-117">Virtuális gép hello hello paneljén kattintson **Connect**.</span><span class="sxs-lookup"><span data-stu-id="6a739-117">On hello blade for hello virtual machine, click **Connect**.</span></span> <span data-ttu-id="6a739-118">Ez a hoz létre, és letölti a Remote Desktop Protocol fájlt (.rdp-fájl), mint a helyi tooconnect tooyour gépek.</span><span class="sxs-lookup"><span data-stu-id="6a739-118">This creates and downloads a Remote Desktop Protocol file (.rdp file) that is like a shortcut tooconnect tooyour machine.</span></span> <span data-ttu-id="6a739-119">Érdemes lehet toosave hello fájl tooyour asztali egyszerűen hozzáférhetnek.</span><span class="sxs-lookup"><span data-stu-id="6a739-119">You might want toosave hello file tooyour desktop for easy access.</span></span> <span data-ttu-id="6a739-120">**Nyissa meg** a fájl tooconnect tooyour virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6a739-120">**Open** this file tooconnect tooyour VM.</span></span>
   
    ![Képernyőfelvétel a hello Azure portál ábrázoló hogyan tooconnect tooyour méretű VM](./media/hero-role/connect.png)
3. <span data-ttu-id="6a739-122">Megjelenik egy figyelmeztetés, hogy hello RDP-fájl közzétevője ismeretlen.</span><span class="sxs-lookup"><span data-stu-id="6a739-122">You get a warning that hello .rdp is from an unknown publisher.</span></span> <span data-ttu-id="6a739-123">Ez nem jelent problémát.</span><span class="sxs-lookup"><span data-stu-id="6a739-123">This is normal.</span></span> <span data-ttu-id="6a739-124">Hello távoli asztal ablakában kattintson **Connect** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="6a739-124">In hello Remote Desktop window, click **Connect** toocontinue.</span></span>
   
    ![Képernyőkép az ismeretlen közzétevőre vonatkozó figyelmeztetésről](./media/hero-role/rdp-warn.png)
4. <span data-ttu-id="6a739-126">Hello Windows rendszerbiztonsági ablakban, a típus hello felhasználónév és jelszó létrehozásakor létrehozott helyi fiók hello hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6a739-126">In hello Windows Security window, type hello username and password for hello local account that you created when you created hello VM.</span></span> <span data-ttu-id="6a739-127">hello felhasználónév van megadva: *vmname*&#92; *felhasználónév*, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="6a739-127">hello username is entered as *vmname*&#92;*username*, then click **OK**.</span></span>
   
    ![Képernyőfelvétel a hello virtuális gép nevét, a felhasználónév és jelszó megadása](./media/hero-role/credentials.png)
5. <span data-ttu-id="6a739-129">Megjelenik egy figyelmeztetés hello tanúsítvány nem ellenőrizhető.</span><span class="sxs-lookup"><span data-stu-id="6a739-129">You get a warning that hello certificate cannot be verified.</span></span> <span data-ttu-id="6a739-130">Ez nem jelent problémát.</span><span class="sxs-lookup"><span data-stu-id="6a739-130">This is normal.</span></span> <span data-ttu-id="6a739-131">Kattintson a **Igen** tooverify hello hello virtuális gép identitását, és a bejelentkezés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="6a739-131">Click **Yes** tooverify hello identity of hello virtual machine and finish logging on.</span></span>
   
   ![Képernyőkép üzenetről hello VM hello identitásának ellenőrzése](./media/hero-role/cert-warning.png)

<span data-ttu-id="6a739-133">Ha fut, tootrouble tooconnect meg, tekintse meg [hibaelhárítása távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gépek](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6a739-133">If you run in tootrouble when you try tooconnect, see [Troubleshoot Remote Desktop connections tooa Windows-based Azure Virtual Machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="install-iis-on-your-vm"></a><span data-ttu-id="6a739-134">Az IIS telepítése a virtuális gépre</span><span class="sxs-lookup"><span data-stu-id="6a739-134">Install IIS on your VM</span></span>
<span data-ttu-id="6a739-135">Most, hogy a virtuális gép toohello van bejelentkezve, egy kiszolgálói szerepkör telepítjük, így több kísérletezhet.</span><span class="sxs-lookup"><span data-stu-id="6a739-135">Now that you are logged in toohello VM, we will install a server role so that you can experiment more.</span></span>

1. <span data-ttu-id="6a739-136">Ha még nincs megnyitva, nyissa meg a **Kiszolgálókezelőt**.</span><span class="sxs-lookup"><span data-stu-id="6a739-136">Open **Server Manager** if it isn't already open.</span></span> <span data-ttu-id="6a739-137">Kattintson a hello **Start** menüben, majd kattintson **Kiszolgálókezelő**.</span><span class="sxs-lookup"><span data-stu-id="6a739-137">Click hello **Start** menu, and then click **Server Manager**.</span></span>
2. <span data-ttu-id="6a739-138">A **Kiszolgálókezelő**, jelölje be **helyi kiszolgáló** hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="6a739-138">In **Server Manager**, select **Local Server** from hello left pane.</span></span> 
3. <span data-ttu-id="6a739-139">Hello menüben válasszon ki **kezelése** > **szerepkörök és szolgáltatások hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6a739-139">In hello menu, select **Manage** > **Add Roles and Features**.</span></span>
4. <span data-ttu-id="6a739-140">Hello hozzáadása szerepkörök és szolgáltatások varázsló hello **telepítési típus** lapon, válassza ki **szerepköralapú vagy szolgáltatásalapú telepítés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="6a739-140">In hello Add Roles and Features Wizard, on hello **Installation Type** page, choose **Role-based or feature-based installation**, and then click **Next**.</span></span>
   
    ![Képernyőkép ábrázoló hello hozzáadása szerepkörök és szolgáltatások varázsló lapján a telepítési típus](./media/hero-role/role-wizard.png)
5. <span data-ttu-id="6a739-142">Válassza ki a virtuális gép hello hello kiszolgálókészletből, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="6a739-142">Select hello VM from hello server pool and click **Next**.</span></span>
6. <span data-ttu-id="6a739-143">A hello **kiszolgálói szerepkörök** lapon jelölje be **webkiszolgáló (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="6a739-143">On hello **Server Roles** page, select **Web Server (IIS)**.</span></span>
   
    ![Képernyőkép ábrázoló hello hozzáadása szerepkörök és szolgáltatások varázsló lapon kiszolgálói szerepköre tekintetében](./media/hero-role/add-iis.png)
7. <span data-ttu-id="6a739-145">Hello előugró IIS szükséges szolgáltatások hozzáadása, ügyeljen arra, hogy **felügyeleti eszközök** van kijelölve, majd kattintson **szolgáltatások hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6a739-145">In hello pop-up about adding features needed for IIS, make sure that **Include management tools** is selected and then click **Add Features**.</span></span> <span data-ttu-id="6a739-146">Előugró hello befejeződésekor kattintson **következő** hello varázslóban.</span><span class="sxs-lookup"><span data-stu-id="6a739-146">When hello pop-up closes, click **Next** in hello wizard.</span></span>
   
    ![Képernyőfelvétel: előugró tooconfirm hello IIS szerepkör hozzáadása](./media/hero-role/confirm-add-feature.png)
8. <span data-ttu-id="6a739-148">Hello szolgáltatások lapon kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="6a739-148">On hello features page, click **Next**.</span></span>
9. <span data-ttu-id="6a739-149">A hello **webkiszolgálói szerepkör (IIS)** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="6a739-149">On hello **Web Server Role (IIS)** page, click **Next**.</span></span> 
10. <span data-ttu-id="6a739-150">A hello **szerepkör-szolgáltatások** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="6a739-150">On hello **Role Services** page, click **Next**.</span></span> 
11. <span data-ttu-id="6a739-151">A hello **megerősítő** kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="6a739-151">On hello **Confirmation** page, click **Install**.</span></span> 
12. <span data-ttu-id="6a739-152">Ha hello telepítés befejeződött, kattintson a **Bezárás** hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="6a739-152">When hello installation is complete, click **Close** on hello wizard.</span></span>

## <a name="open-port-80"></a><span data-ttu-id="6a739-153">A 80-as port megnyitása</span><span class="sxs-lookup"><span data-stu-id="6a739-153">Open port 80</span></span>
<span data-ttu-id="6a739-154">Ahhoz, hogy a virtuális gép tooaccept érkező bejövő forgalmat a 80-as porton keresztül, egy bejövő forgalomra vonatkozó szabály toohello hálózati biztonsági csoport tooadd van szüksége.</span><span class="sxs-lookup"><span data-stu-id="6a739-154">In order for your VM tooaccept inbound traffic over port 80, you need tooadd an inbound rule toohello network security group.</span></span> 

1. <span data-ttu-id="6a739-155">Nyissa meg hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6a739-155">Open hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6a739-156">A **virtuális gépek** válassza ki a létrehozott virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="6a739-156">In **Virtual machines** select hello VM that you created.</span></span>
3. <span data-ttu-id="6a739-157">Hello virtuális gépek beállításaiban, válassza ki a **hálózati illesztőt** és majd hello az válassza ki a meglévő hálózati illesztő.</span><span class="sxs-lookup"><span data-stu-id="6a739-157">In hello virtual machines settings, select **Network interfaces** and then select hello existing network interface.</span></span>
   
    ![Képernyőfelvétel a hello hello virtuális gép beállítása hálózati illesztők](./media/hero-role/network-interface.png)
4. <span data-ttu-id="6a739-159">A **Essentials** hello hálózati adapter, kattintson a hello **hálózati biztonsági csoport**.</span><span class="sxs-lookup"><span data-stu-id="6a739-159">In **Essentials** for hello network interface, click hello **Network security group**.</span></span>
   
    ![Képernyőfelvétel a hello Essentials szakaszban hello hálózati adapter](./media/hero-role/select-nsg.png)
5. <span data-ttu-id="6a739-161">A hello **Essentials** hello NSG paneljén rendelkeznie kell egy meglévő alapértelmezett bejövő szabály **alapértelmezett engedélyezése rdp** így toolog a virtuális gép toohello.</span><span class="sxs-lookup"><span data-stu-id="6a739-161">In hello **Essentials** blade for hello NSG, you should have one existing default inbound rule for **default-allow-rdp** which allows you toolog in toohello VM.</span></span> <span data-ttu-id="6a739-162">Egy másik bejövő forgalomra vonatkozó szabály tooallow IIS forgalom adhat.</span><span class="sxs-lookup"><span data-stu-id="6a739-162">You will add another inbound rule tooallow IIS traffic.</span></span> <span data-ttu-id="6a739-163">Kattintson a **Bejövő biztonsági szabály** elemre.</span><span class="sxs-lookup"><span data-stu-id="6a739-163">Click **Inbound security rule**.</span></span>
   
    ![Képernyőfelvétel a hello NSG hello Essentials szakaszban](./media/hero-role/inbound.png)
6. <span data-ttu-id="6a739-165">A **Bejövő biztonsági szabályok** területen kattintson a **Hozzáadás** elemre.</span><span class="sxs-lookup"><span data-stu-id="6a739-165">In **Inbound security rules**, click **Add**.</span></span>
   
    ![Képernyőfelvétel: hello gomb tooadd biztonsági szabály](./media/hero-role/add-rule.png)
7. <span data-ttu-id="6a739-167">A **Bejövő biztonsági szabályok** területen kattintson a **Hozzáadás** elemre.</span><span class="sxs-lookup"><span data-stu-id="6a739-167">In **Inbound security rules**, click **Add**.</span></span> <span data-ttu-id="6a739-168">Típus **80** a porttartomány hello, és győződjön meg arról, hogy **engedélyezése** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="6a739-168">Type **80** in hello port range and make sure **Allow** is selected.</span></span> <span data-ttu-id="6a739-169">Ha elkészült, kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6a739-169">When you are done, click **OK**.</span></span>
   
    ![Képernyőfelvétel: hello gomb tooadd biztonsági szabály](./media/hero-role/port-80.png)

<span data-ttu-id="6a739-171">Az NSG-k, bejövő és kimenő szabályokat, további információ: [engedélyezése külső hozzáférés tooyour VM használatával hello Azure-portálon](nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="6a739-171">For more information about NSGs, inbound and outbound rules, see [Allow external access tooyour VM using hello Azure portal](nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

## <a name="connect-toohello-default-iis-website"></a><span data-ttu-id="6a739-172">Csatlakozás toohello alapértelmezett IIS-webhely</span><span class="sxs-lookup"><span data-stu-id="6a739-172">Connect toohello default IIS website</span></span>
1. <span data-ttu-id="6a739-173">Hello Azure-portálon, kattintson **virtuális gépek** , és válassza ki a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="6a739-173">In hello Azure portal, click **Virtual machines** and then select your VM.</span></span>
2. <span data-ttu-id="6a739-174">A hello **Essentials** panelen, a Másolás a **nyilvános IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="6a739-174">In hello **Essentials** blade, copy your **Public IP address**.</span></span>
   
    ![Ha toofind hello nyilvános IP-címet a virtuális gép ábrázoló képernyőfelvétel](./media/hero-role/ipaddress.png)
3. <span data-ttu-id="6a739-176">Nyisson meg egy böngészőt, és hello címsorába írja be a nyilvános IP-címet ehhez hasonló: http://<publicIPaddress> kattintson **Enter** toogo toothat cím.</span><span class="sxs-lookup"><span data-stu-id="6a739-176">Open a browser and in hello address bar, type in your public IP address like this: http://<publicIPaddress> and click **Enter** toogo toothat address.</span></span>
4. <span data-ttu-id="6a739-177">A böngésző meg kell nyitnia a hello alapértelmezett IIS weblap.</span><span class="sxs-lookup"><span data-stu-id="6a739-177">Your browser should open hello default IIS web page.</span></span> <span data-ttu-id="6a739-178">Ez a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="6a739-178">It looks something like this:</span></span>
   
    ![Egy böngészőben néz milyen hello alapértelmezett IIS lapot ábrázoló képernyőfelvétel](./media/hero-role/iis-default.png)

## <a name="next-steps"></a><span data-ttu-id="6a739-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6a739-180">Next steps</span></span>
* <span data-ttu-id="6a739-181">Is kísérletezhet [adatlemezt csatol](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooyour virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="6a739-181">You can also experiment with [attaching a data disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooyour virtual machine.</span></span> <span data-ttu-id="6a739-182">Az adatlemezek nagyobb tárterületet biztosítanak a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="6a739-182">Data disks provide more storage for your virtual machine.</span></span>

