---
title: "Linux virtuális gép lemezképét aaaCapture |} Microsoft Docs"
description: "Ismerje meg, hogyan toocapture a Linux-alapú Azure virtuális gépek (VM) lemezkép létre hello klasszikus üzembe helyezési modellben."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17d7ffee-a58e-4290-9de1-64c3cf1ddc05
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
ms.openlocfilehash: 33c4059d5bb919a86bdc3492abca540750f365ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocapture-a-classic-linux-virtual-machine-as-an-image"></a><span data-ttu-id="77232-103">Hogyan toocapture egy klasszikus Linuxos virtuális gép képként</span><span class="sxs-lookup"><span data-stu-id="77232-103">How toocapture a classic Linux virtual machine as an image</span></span>
> [!IMPORTANT]
> <span data-ttu-id="77232-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="77232-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="77232-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="77232-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="77232-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="77232-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="77232-107">Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="77232-107">Learn how too[perform these steps using hello Resource Manager model](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="77232-108">Ez a cikk bemutatja, hogyan toocapture a klasszikus Azure virtuális gép (VM) egy kép toocreate Linux futtató más virtuális gépekkel.</span><span class="sxs-lookup"><span data-stu-id="77232-108">This article shows you how toocapture a classic Azure virtual machine (VM) running Linux as an image toocreate other virtual machines.</span></span> <span data-ttu-id="77232-109">Ez a rendszerkép tartalmazza hello operációsrendszer-lemez, és adatlemezt csatolni toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="77232-109">This image includes hello OS disk and data disks attached toohello VM.</span></span> <span data-ttu-id="77232-110">Hálózati konfiguráció nem tartoznak bele, tooconfigure van szüksége, amely létrehozásakor hello más virtuális gép hello lemezképéről.</span><span class="sxs-lookup"><span data-stu-id="77232-110">It doesn't include networking configuration, so you need tooconfigure that when you create hello other VM from hello image.</span></span>

<span data-ttu-id="77232-111">Azure tárolók hello lemezkép **képek**, feltöltött képeket együtt.</span><span class="sxs-lookup"><span data-stu-id="77232-111">Azure stores hello image under **Images**, along with any images you've uploaded.</span></span> <span data-ttu-id="77232-112">Lemezképek kapcsolatos további információkért lásd: [kapcsolatos virtuálisgép-lemezképeket az Azure-ban][About Virtual Machine Images in Azure].</span><span class="sxs-lookup"><span data-stu-id="77232-112">For more information about images, see [About Virtual Machine Images in Azure][About Virtual Machine Images in Azure].</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="77232-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="77232-113">Before you begin</span></span>
<span data-ttu-id="77232-114">Ezek a lépések feltételezik, hogy már létrehozott egy Azure virtuális gép hello klasszikus telepítési modell és a beállított hello operációs rendszer, beleértve a bármely adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="77232-114">These steps assume that you've already created an Azure VM using hello Classic deployment model and configured hello operating system, including attaching any data disks.</span></span> <span data-ttu-id="77232-115">Ha egy virtuális gép toocreate van szüksége, olvassa el a [hogyan tooCreate Linux virtuális gépek][How tooCreate a Linux Virtual Machine].</span><span class="sxs-lookup"><span data-stu-id="77232-115">If you need toocreate a VM, read [How tooCreate a Linux Virtual Machine][How tooCreate a Linux Virtual Machine].</span></span>

## <a name="capture-hello-virtual-machine"></a><span data-ttu-id="77232-116">Hello virtuális gép rögzítése</span><span class="sxs-lookup"><span data-stu-id="77232-116">Capture hello virtual machine</span></span>
1. <span data-ttu-id="77232-117">[Csatlakozás a virtuális gép toohello](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) egy SSH-ügyfél az Ön által választott használatával.</span><span class="sxs-lookup"><span data-stu-id="77232-117">[Connect toohello VM](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) using an SSH client of your choice.</span></span>
2. <span data-ttu-id="77232-118">Hello SSH ablak írja be a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="77232-118">In hello SSH window, type hello following command.</span></span> <span data-ttu-id="77232-119">hello kimenetét `waagent` függően eltérőek lehetnek attól függően, hogy ezt a segédprogramot hello verziója:</span><span class="sxs-lookup"><span data-stu-id="77232-119">hello output from `waagent` may vary slightly depending on hello version of this utility:</span></span>

    ```bash
    sudo waagent -deprovision+user
    ```

    <span data-ttu-id="77232-120">hello előző parancs kísérletek tooclean hello rendszer és a megfelelő reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="77232-120">hello preceding command attempts tooclean hello system and make it suitable for reprovisioning.</span></span> <span data-ttu-id="77232-121">Ez a művelet a következő feladatok hello hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="77232-121">This operation performs hello following tasks:</span></span>

   * <span data-ttu-id="77232-122">SSH-állomások kulcsait eltávolítja (ha Provisioning.RegenerateSshHostKeyPair "y" hello konfigurációs fájlban)</span><span class="sxs-lookup"><span data-stu-id="77232-122">Removes SSH host keys (if Provisioning.RegenerateSshHostKeyPair is 'y' in hello configuration file)</span></span>
   * <span data-ttu-id="77232-123">Törli a /etc/resolv.conf névkiszolgáló-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="77232-123">Clears nameserver configuration in /etc/resolv.conf</span></span>
   * <span data-ttu-id="77232-124">Eltávolítja a hello `root` a felhasználó jelszava a/etc/árnyékmásolat (ha Provisioning.DeleteRootPassword "y" hello konfigurációs fájlban)</span><span class="sxs-lookup"><span data-stu-id="77232-124">Removes hello `root` user password from /etc/shadow (if Provisioning.DeleteRootPassword is 'y' in hello configuration file)</span></span>
   * <span data-ttu-id="77232-125">A gyorsítótárazott DHCP-ügyfélbérletek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="77232-125">Removes cached DHCP client leases</span></span>
   * <span data-ttu-id="77232-126">Alaphelyzetbe állítását gazdagép neve toolocalhost.localdomain</span><span class="sxs-lookup"><span data-stu-id="77232-126">Resets host name toolocalhost.localdomain</span></span>
   * <span data-ttu-id="77232-127">Törli a hello (/var/lib/waagent nyert) utolsó kiépített felhasználói fiókot **és az adatok**.</span><span class="sxs-lookup"><span data-stu-id="77232-127">Deletes hello last provisioned user account (obtained from /var/lib/waagent) **and associated data**.</span></span>

     > [!NOTE]
     > <span data-ttu-id="77232-128">Megszüntetés törli a fájlokat és adatokat túl "generalize" hello kép.</span><span class="sxs-lookup"><span data-stu-id="77232-128">Deprovisioning deletes files and data too"generalize" hello image.</span></span> <span data-ttu-id="77232-129">A parancs egy virtuális gépen, hogy szeretné-e új lemezkép sablonként toocapture csak fut.</span><span class="sxs-lookup"><span data-stu-id="77232-129">Only run this command on a VM that you intend toocapture as a new image template.</span></span> <span data-ttu-id="77232-130">Hello lemezkép minden a bizalmas adatok megszűnik, vagy alkalmas Újraterjesztési toothird felek nem garantálja.</span><span class="sxs-lookup"><span data-stu-id="77232-130">It does not guarantee that hello image is cleared of all sensitive information or is suitable for redistribution toothird parties.</span></span>

3. <span data-ttu-id="77232-131">Típus **y** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="77232-131">Type **y** toocontinue.</span></span> <span data-ttu-id="77232-132">Hozzáadhat hello `-force` paraméter tooavoid a megerősítési lépés.</span><span class="sxs-lookup"><span data-stu-id="77232-132">You can add hello `-force` parameter tooavoid this confirmation step.</span></span>
4. <span data-ttu-id="77232-133">Típus **kilépési** tooclose hello SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="77232-133">Type **Exit** tooclose hello SSH client.</span></span>

   > [!NOTE]
   > <span data-ttu-id="77232-134">hello fennmaradó lépések során feltételezzük, hogy már [hello Azure parancssori felület telepítve](../../../cli-install-nodejs.md) az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="77232-134">hello remaining steps assume you have already [installed hello Azure CLI](../../../cli-install-nodejs.md) on your client computer.</span></span> <span data-ttu-id="77232-135">A következő lépéseket az összes hello is elvégezhető a hello [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="77232-135">All hello following steps can also be done in hello [Azure portal](http://portal.azure.com).</span></span>

5. <span data-ttu-id="77232-136">Az ügyfélszámítógépen nyissa meg az Azure parancssori felület és a bejelentkezési tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="77232-136">From your client computer, open Azure CLI and login tooyour Azure subscription.</span></span> <span data-ttu-id="77232-137">További információkért olvassa el a [hello Azure CLI Azure-előfizetés tooan kapcsolódó](../../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="77232-137">For details, read [Connect tooan Azure subscription from hello Azure CLI](../../../xplat-cli-connect.md).</span></span>

   > [!NOTE]
   > <span data-ttu-id="77232-138">Hello Azure-portálon, a bejelentkezés toohello portálon.</span><span class="sxs-lookup"><span data-stu-id="77232-138">In hello Azure portal, log in toohello portal.</span></span>

6. <span data-ttu-id="77232-139">Ellenőrizze, hogy a szolgáltatás felügyeleti mód:</span><span class="sxs-lookup"><span data-stu-id="77232-139">Make sure you are in Service Management mode:</span></span>

    ```azurecli
    azure config mode asm
    ```

7. <span data-ttu-id="77232-140">Állítsa le a virtuális Gépet, amely már platformelőfizetés hello.</span><span class="sxs-lookup"><span data-stu-id="77232-140">Shut down hello VM that is already deprovisioned.</span></span> <span data-ttu-id="77232-141">hello alábbi példa leáll hello nevű virtuális gép `myVM`:</span><span class="sxs-lookup"><span data-stu-id="77232-141">hello following example shuts down hello VM named `myVM`:</span></span>

    ```azurecli
    azure vm shutdown myVM
    ```
   <span data-ttu-id="77232-142">Ha szükséges, az előfizetés használatával létrehozott összes hello virtuális gépek listáját megtekintheti`azure vm list`</span><span class="sxs-lookup"><span data-stu-id="77232-142">If needed, you can view a list all hello VMs created in your subscription by using `azure vm list`</span></span>

   > [!NOTE]
   > <span data-ttu-id="77232-143">Hello Azure portál használata, válassza ki a virtuális gép hello, és kattintson a **leállítása** hello VM leállítása.</span><span class="sxs-lookup"><span data-stu-id="77232-143">If you're using hello Azure portal, select hello VM and click **Stop** to shut down hello VM.</span></span>

8. <span data-ttu-id="77232-144">Hello virtuális gép leállítását követően hello lemezképének rögzítése.</span><span class="sxs-lookup"><span data-stu-id="77232-144">When hello VM is stopped, capture hello image.</span></span> <span data-ttu-id="77232-145">a következő példa rögzítésekre hello hello nevű virtuális gép `myVM` , és létrehoz egy általánosított nevű lemezkép `myNewVM`:</span><span class="sxs-lookup"><span data-stu-id="77232-145">hello following example captures hello VM named `myVM` and creates a generalized image named `myNewVM`:</span></span>

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    <span data-ttu-id="77232-146">Hello `-t` törli az eredeti virtuális gép hello alparancs.</span><span class="sxs-lookup"><span data-stu-id="77232-146">hello `-t` subcommand deletes hello original virtual machine.</span></span>

    > [!NOTE]
    > <span data-ttu-id="77232-147">Hello Azure-portálon, rögzítheti képként kiválasztásával **kép** hello hub menüből.</span><span class="sxs-lookup"><span data-stu-id="77232-147">In hello Azure portal, you can capture an image by selecting **Image** from hello hub menu.</span></span> <span data-ttu-id="77232-148">A következő hello lemezképének információit toosupply hello van szüksége: neve, a erőforráscsoport, a helye, a operációs rendszer típusa és a tárolási blob elérési útja.</span><span class="sxs-lookup"><span data-stu-id="77232-148">You need toosupply hello following information for hello image: name, resource group, location, operating system type, and storage blob path.</span></span>

9. <span data-ttu-id="77232-149">hello új lemezkép már csak olyan lemezképkészlet, amellyel lehet hello listában használt tooconfigure bármely új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="77232-149">hello new image is now available in hello list of images that can be used tooconfigure any new VM.</span></span> <span data-ttu-id="77232-150">Ez a hello paranccsal tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="77232-150">You can view it with hello command:</span></span>

   ```azurecli
   azure vm image list
   ```

   <span data-ttu-id="77232-151">A hello [Azure-portálon](http://portal.azure.com), hello jelenik meg hello **Virtuálisgép-rendszerképek (klasszikus)** toohello tartozó **számítási** szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="77232-151">On hello [Azure portal](http://portal.azure.com), hello new image appears in hello **VM images (classic)** that belongs toohello **Compute** services.</span></span> <span data-ttu-id="77232-152">Van-e hozzáférési **Virtuálisgép-rendszerképek (klasszikus)** kattintva _további szolgáltatások_ hello aljához hello Azure szolgáltatást a listában, majd a hello **számítási** szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="77232-152">You can access **VM images (classic)** by clicking _More services_ at hello bottom of hello Azure service list, and then looking in hello **Compute** services.</span></span>   

   ![Lemezkép-rögzítési sikeres](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a><span data-ttu-id="77232-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="77232-154">Next steps</span></span>
<span data-ttu-id="77232-155">hello kép készen toobe használt toocreate virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="77232-155">hello image is ready toobe used toocreate VMs.</span></span> <span data-ttu-id="77232-156">Hello Azure CLI paranccsal `azure vm create` és a létrehozott ellátási hello lemezkép nevét.</span><span class="sxs-lookup"><span data-stu-id="77232-156">You can use hello Azure CLI command `azure vm create` and supply hello image name you created.</span></span> <span data-ttu-id="77232-157">További információkért lásd: [Using hello Azure CLI klasszikus telepítési modellel rendelkező](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="77232-157">For more information, see [Using hello Azure CLI with Classic deployment model](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

<span data-ttu-id="77232-158">Másik megoldásként használhatja a hello [Azure-portálon](http://portal.azure.com) toocreate hello segítségével egyéni virtuális gép **kép** módszer, majd válassza a hello létrehozott lemezképet.</span><span class="sxs-lookup"><span data-stu-id="77232-158">Alternatively, use hello [Azure portal](http://portal.azure.com) toocreate a custom VM by using hello **Image** method and selecting hello image you created.</span></span> <span data-ttu-id="77232-159">További információkért lásd: [hogyan tooCreate egy egyéni VM][How tooCreate a Custom Virtual Machine].</span><span class="sxs-lookup"><span data-stu-id="77232-159">For more information, see [How tooCreate a Custom VM][How tooCreate a Custom Virtual Machine].</span></span>

<span data-ttu-id="77232-160">**További információ:** [Azure Linux ügynök felhasználói útmutatója](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="77232-160">**See also:** [Azure Linux Agent User Guide](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How tooCreate a Custom Virtual Machine]:create-custom.md
[How tooAttach a Data Disk tooa Virtual Machine]:attach-disk.md
[How tooCreate a Linux Virtual Machine]:create-custom.md
