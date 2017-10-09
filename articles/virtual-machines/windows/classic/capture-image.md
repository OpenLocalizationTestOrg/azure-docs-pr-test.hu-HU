---
title: "az Azure Windows virtuális gép lemezképét aaaCapture |} Microsoft Docs"
description: "Készítsen lemezképet arról az hello klasszikus üzembe helyezési modellel létrehozott Azure Windows virtuális géphez."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: a5986eac-4cf3-40bd-9b79-7c811806b880
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: b9bbc437012aa44295f90941c9d72e39509df28f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-hello-classic-deployment-model"></a><span data-ttu-id="51259-103">Készítsen lemezképet arról az hello klasszikus üzembe helyezési modellel létrehozott Azure Windows virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="51259-103">Capture an image of an Azure Windows virtual machine created with hello classic deployment model.</span></span>
> [!IMPORTANT]
> <span data-ttu-id="51259-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="51259-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="51259-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="51259-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="51259-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="51259-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="51259-107">Erőforrás-kezelő modell információkért lásd: [egy általánosított virtuális Gépet az Azure-ban a felügyelt lemezképének](../capture-image-resource.md).</span><span class="sxs-lookup"><span data-stu-id="51259-107">For Resource Manager model information, see [Capture a managed image of a generalized VM in Azure](../capture-image-resource.md).</span></span>

<span data-ttu-id="51259-108">Ez a cikk bemutatja, hogyan toocapture Windows, mint egy kép toocreate használhassa más virtuális gépeket futtató Azure virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="51259-108">This article shows you how toocapture an Azure virtual machine running Windows so you can use it as an image toocreate other virtual machines.</span></span> <span data-ttu-id="51259-109">Ez a rendszerkép tartalmazza hello operációsrendszer-lemez, és olyan adatokat lemezek, amelyek csatlakoztatott toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="51259-109">This image includes hello operating system disk and any data disks that are attached toohello virtual machine.</span></span> <span data-ttu-id="51259-110">Hálózati beállítások, ezért fel egy hálózati konfigurációt tooset kell hello más hello lemezképet használó virtuális gépek létrehozásakor nem tartoznak bele.</span><span class="sxs-lookup"><span data-stu-id="51259-110">It doesn't include networking configurations, so you'll need tooset up network configurations when you create hello other virtual machines that use hello image.</span></span>

<span data-ttu-id="51259-111">Azure tárolók hello lemezkép **Virtuálisgép-rendszerképek (klasszikus)**, egy **számítási** szolgáltatás, amely szerepel a listában az összes megtekintésekor hello Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="51259-111">Azure stores hello image under **VM images (classic)**, a **Compute** service that is listed when you view all hello Azure services.</span></span> <span data-ttu-id="51259-112">Ez a hello feltöltött képeket tároló ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="51259-112">This is hello same place where any images you've uploaded are stored.</span></span> <span data-ttu-id="51259-113">Lemezképek kapcsolatos részletekért lásd: [kapcsolatos képek a virtuális gépek](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="51259-113">For details about images, see [About images for virtual machines](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="51259-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="51259-114">Before you begin</span></span>
<span data-ttu-id="51259-115">Ezek a lépések feltételezik, hogy korábban már létrehozott Azure virtuális gép és konfigurált hello operációs rendszer, beleértve a bármely adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="51259-115">These steps assume that you've already created an Azure virtual machine and configured hello operating system, including attaching any data disks.</span></span> <span data-ttu-id="51259-116">Ha még nem meg, tekintse meg a következő cikkekben talál létrehozására, illetve hello virtuális gép előkészítése hello:</span><span class="sxs-lookup"><span data-stu-id="51259-116">If you haven't done this yet, see hello following articles for information on creating and preparing hello virtual machine:</span></span>

* [<span data-ttu-id="51259-117">Virtuális gép létrehozása lemezkép</span><span class="sxs-lookup"><span data-stu-id="51259-117">Create a virtual machine from an image</span></span>](createportal.md)
* [<span data-ttu-id="51259-118">Hogyan tooattach az adatok lemezre tooa virtuális gép</span><span class="sxs-lookup"><span data-stu-id="51259-118">How tooattach a data disk tooa virtual machine</span></span>](attach-disk.md)
* <span data-ttu-id="51259-119">Ellenőrizze, hogy a Sysprep hello kiszolgálói szerepkörök támogatottak.</span><span class="sxs-lookup"><span data-stu-id="51259-119">Make sure hello server roles are supported with Sysprep.</span></span> <span data-ttu-id="51259-120">További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="51259-120">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span>

> [!WARNING]
> <span data-ttu-id="51259-121">Ez a folyamat törli a hello eredeti virtuális gép rögzítése után.</span><span class="sxs-lookup"><span data-stu-id="51259-121">This process deletes hello original virtual machine after it's captured.</span></span>
>
>

<span data-ttu-id="51259-122">Egy Azure virtuális gép lemezképének előzetes toocapturing ajánlott hello cél virtuális gép biztonsági másolatot.</span><span class="sxs-lookup"><span data-stu-id="51259-122">Prior toocapturing an image of an Azure virtual machine, it is recommended hello target virtual machine be backed up.</span></span> <span data-ttu-id="51259-123">Az Azure virtuális gépek Azure Backup segítségével készíthető.</span><span class="sxs-lookup"><span data-stu-id="51259-123">Azure virtual machines can be backed up using Azure Backup.</span></span> <span data-ttu-id="51259-124">További részletek: [Azure-beli virtuális gépek biztonsági mentése](../../../backup/backup-azure-vms.md).</span><span class="sxs-lookup"><span data-stu-id="51259-124">For details, see [Back up Azure virtual machines](../../../backup/backup-azure-vms.md).</span></span> <span data-ttu-id="51259-125">Hivatalos partnereinktől egyéb megoldások is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="51259-125">Other solutions are available from certified partners.</span></span> <span data-ttu-id="51259-126">jelenleg elérhető toofind keresési hello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="51259-126">toofind out what’s currently available, search hello Azure Marketplace.</span></span>

## <a name="capture-hello-virtual-machine"></a><span data-ttu-id="51259-127">Hello virtuális gép rögzítése</span><span class="sxs-lookup"><span data-stu-id="51259-127">Capture hello virtual machine</span></span>
1. <span data-ttu-id="51259-128">A hello [Azure-portálon](http://portal.azure.com), **Connect** toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="51259-128">In hello [Azure portal](http://portal.azure.com), **Connect** toohello virtual machine.</span></span> <span data-ttu-id="51259-129">Útmutatásért lásd: [hogyan tooa virtuális gép Windows Server rendszert futtató toosign][How toosign in tooa virtual machine running Windows Server].</span><span class="sxs-lookup"><span data-stu-id="51259-129">For instructions, see [How toosign in tooa virtual machine running Windows Server][How toosign in tooa virtual machine running Windows Server].</span></span>
2. <span data-ttu-id="51259-130">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="51259-130">Open a Command Prompt window as an administrator.</span></span>
3. <span data-ttu-id="51259-131">Hello könyvtárváltás túl`%windir%\system32\sysprep`, majd futtassa a sysprep.exe.</span><span class="sxs-lookup"><span data-stu-id="51259-131">Change hello directory too`%windir%\system32\sysprep`, and then run sysprep.exe.</span></span>
4. <span data-ttu-id="51259-132">Hello **rendszer-előkészítő eszköz** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="51259-132">hello **System Preparation Tool** dialog box appears.</span></span> <span data-ttu-id="51259-133">A következő hello:</span><span class="sxs-lookup"><span data-stu-id="51259-133">Do hello following:</span></span>

   * <span data-ttu-id="51259-134">A **Rendszerkarbantartási művelet**, jelölje be **adja meg a rendszer Out-of-Box élmény (OOBE)** , és győződjön meg arról, hogy **Generalize** be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="51259-134">In **System Cleanup Action**, select **Enter System Out-of-Box Experience (OOBE)** and make sure that **Generalize** is checked.</span></span> <span data-ttu-id="51259-135">A Sysprep használatával kapcsolatos további információkért lásd: [hogyan tooUse Sysprep: Bevezetés][How tooUse Sysprep: An Introduction].</span><span class="sxs-lookup"><span data-stu-id="51259-135">For more information about using Sysprep, see [How tooUse Sysprep: An Introduction][How tooUse Sysprep: An Introduction].</span></span>
   * <span data-ttu-id="51259-136">A **leállítási beállítások**, jelölje be **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="51259-136">In **Shutdown Options**, select **Shutdown**.</span></span>
   * <span data-ttu-id="51259-137">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="51259-137">Click **OK**.</span></span>

   ![Futtassa a Sysprep eszközt](./media/capture-image/SysprepGeneral.png)
5. <span data-ttu-id="51259-139">A Sysprep hello állapotot hello virtuális gép hello Azure-portálon módosítja túl hello virtuális gép leáll**leállítva**.</span><span class="sxs-lookup"><span data-stu-id="51259-139">Sysprep shuts down hello virtual machine, which changes hello status of hello virtual machine in hello Azure portal too**Stopped**.</span></span>
6. <span data-ttu-id="51259-140">Hello Azure-portálon, kattintson **virtuális gépek (klasszikus)** , és válassza ki a virtuális gép kívánt toocapture hello.</span><span class="sxs-lookup"><span data-stu-id="51259-140">In hello Azure portal, click **Virtual Machines (classic)** and select hello virtual machine you want toocapture.</span></span> <span data-ttu-id="51259-141">Hello **Virtuálisgép-rendszerképek (klasszikus)** csoport megtalálható-e **számítási** megtekintésekor **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="51259-141">hello **VM images (classic)** group is listed under **Compute** when you view **More services**.</span></span>

7. <span data-ttu-id="51259-142">A hello parancssávon kattintson **rögzítése**.</span><span class="sxs-lookup"><span data-stu-id="51259-142">On hello command bar, click **Capture**.</span></span>

   ![Virtuális gép rögzítése](./media/capture-image/CaptureVM.png)

   <span data-ttu-id="51259-144">Hello **virtuális gép rögzítése hello** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="51259-144">hello **Capture hello Virtual Machine** dialog box appears.</span></span>

8. <span data-ttu-id="51259-145">A **lemezképnév**, adjon meg egy nevet az új lemezképet hello.</span><span class="sxs-lookup"><span data-stu-id="51259-145">In **Image name**, type a name for hello new image.</span></span> <span data-ttu-id="51259-146">A **lemezképcímke**, írja be a címkék hello új lemezképet.</span><span class="sxs-lookup"><span data-stu-id="51259-146">In **Image label**, type a label for hello new image.</span></span>

9. <span data-ttu-id="51259-147">Kattintson a **hello virtuális gépen már futtattam a Sysprep**.</span><span class="sxs-lookup"><span data-stu-id="51259-147">Click **I've run Sysprep on hello virtual machine**.</span></span> <span data-ttu-id="51259-148">Ezt a jelölőnégyzetet a 3-5. lépéseket a Sysprep toohello műveletek hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="51259-148">This checkbox refers toohello actions with Sysprep in steps 3-5.</span></span> <span data-ttu-id="51259-149">A képfájl _kell_ futtassa a Sysprep a Windows Server hozzáadása előtt egyéni képek kép tooyour meghatározott általánosítva.</span><span class="sxs-lookup"><span data-stu-id="51259-149">An image _must_ be generalized by running Sysprep before you add a Windows Server image tooyour set of custom images.</span></span>

10. <span data-ttu-id="51259-150">Hello rögzítése után hello új lemezkép válik elérhetővé hello **piactér**, a hello **számítási**, **Virtuálisgép-rendszerképek (klasszikus)** tároló.</span><span class="sxs-lookup"><span data-stu-id="51259-150">Once hello capture completes, hello new image becomes available in hello **Marketplace**, in hello **Compute**, **VM images (classic)** container.</span></span>

    ![Lemezkép-rögzítési sikeres](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a><span data-ttu-id="51259-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51259-152">Next steps</span></span>
<span data-ttu-id="51259-153">hello kép készen toobe használt toocreate virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="51259-153">hello image is ready toobe used toocreate virtual machines.</span></span> <span data-ttu-id="51259-154">toodo, lesz hozzon létre egy virtuális gép hello kiválasztásával **további szolgáltatások** menüpont hello szolgáltatások menüt, majd hello alján **Virtuálisgép-rendszerképek (klasszikus)** a hello **számítási**csoport.</span><span class="sxs-lookup"><span data-stu-id="51259-154">toodo this, you'll create a virtual machine by selecting hello **More services** menu item at hello bottom of hello services menu, then **VM images (classic)** in hello **Compute** group.</span></span> <span data-ttu-id="51259-155">Útmutatásért lásd: [virtuális gép létrehozása lemezkép](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="51259-155">For instructions, see [Create a virtual machine from an image](createportal.md).</span></span>

[How toosign in tooa virtual machine running Windows Server]:connect-logon.md
[How tooUse Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/capture-image/SysprepGeneral.png
[hello virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of hello virtual machine]: ./media/capture-image/CaptureVM.png
[Enter hello image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use hello captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
