---
title: "aaaCreate és freebsd rendszerű virtuális gép feltöltés kép |} Microsoft Docs"
description: "Toocreate ismerje meg, majd töltse fel a virtuális merevlemez (VHD), amely tartalmazza a hello freebsd rendszerű operációs rendszer toocreate Azure virtuális gép"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: kyliel
ms.openlocfilehash: f3bd155e496f1a2713d36bb66ea9824ed4c210ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-a-freebsd-vhd-tooazure"></a><span data-ttu-id="4e187-103">Hozzon létre, és töltse fel a freebsd rendszerű virtuális merevlemez tooAzure</span><span class="sxs-lookup"><span data-stu-id="4e187-103">Create and upload a FreeBSD VHD tooAzure</span></span>
<span data-ttu-id="4e187-104">Ez a cikk bemutatja, hogyan toocreate és feltöltése egy virtuális merevlemez (VHD) tartalmazó hello freebsd rendszerű operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="4e187-104">This article shows you how toocreate and upload a virtual hard disk (VHD) that contains hello FreeBSD operating system.</span></span> <span data-ttu-id="4e187-105">Miután a feltöltés, saját kép toocreate egy virtuális gép (VM) az Azure-ban, használhatja.</span><span class="sxs-lookup"><span data-stu-id="4e187-105">After you upload it, you can use it as your own image toocreate a virtual machine (VM) in Azure.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="4e187-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="4e187-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="4e187-107">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="4e187-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="4e187-108">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="4e187-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="4e187-109">További információ a hello Resource Manager modellt használó virtuális merevlemez feltöltése: [Itt](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4e187-109">For information about uploading a VHD using hello Resource Manager model, see [here](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4e187-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4e187-110">Prerequisites</span></span>
<span data-ttu-id="4e187-111">Ez a cikk feltételezi, hogy rendelkezik-e a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="4e187-111">This article assumes that you have hello following items:</span></span>

* <span data-ttu-id="4e187-112">**Azure-előfizetés**– Ha nincs fiókja, létrehozhat egyet, néhány perc alatt.</span><span class="sxs-lookup"><span data-stu-id="4e187-112">**An Azure subscription**--If you don't have an account, you can create one in just a couple of minutes.</span></span> <span data-ttu-id="4e187-113">Ha MSDN-előfizetéssel rendelkezik, tekintse meg [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="4e187-113">If you have an MSDN subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span> <span data-ttu-id="4e187-114">Ellenkező esetben megtudhatja, hogyan túl[hozzon létre egy próbafiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4e187-114">Otherwise, learn how too[create a free trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span>  
* <span data-ttu-id="4e187-115">**Az Azure PowerShell eszközök**– hello Azure PowerShell modul toouse az Azure-előfizetéshez konfigurált, és telepítve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4e187-115">**Azure PowerShell tools**--hello Azure PowerShell module must be installed and configured toouse your Azure subscription.</span></span> <span data-ttu-id="4e187-116">toodownload hello modult, tekintse meg [Azure letölti](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="4e187-116">toodownload hello module, see [Azure downloads](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="4e187-117">Egy oktatóanyag, amely ismerteti, hogyan tooinstall és konfigurálása hello modul érhető el itt.</span><span class="sxs-lookup"><span data-stu-id="4e187-117">A tutorial that describes how tooinstall and configure hello module is available here.</span></span> <span data-ttu-id="4e187-118">Használjon hello [Azure letölti](https://azure.microsoft.com/downloads/) parancsmag tooupload hello VHD-t.</span><span class="sxs-lookup"><span data-stu-id="4e187-118">Use hello [Azure Downloads](https://azure.microsoft.com/downloads/) cmdlet tooupload hello VHD.</span></span>
* <span data-ttu-id="4e187-119">**Freebsd rendszerű operációs rendszer van telepítve, a .vhd-fájllá**--egy támogatott freebsd rendszerű operációs rendszernek kell lennie a telepített tooa virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="4e187-119">**FreeBSD operating system installed in a .vhd file**--A supported   FreeBSD operating system must be installed tooa virtual hard disk.</span></span> <span data-ttu-id="4e187-120">Több különféle eszköz toocreate .vhd fájlok léteznek.</span><span class="sxs-lookup"><span data-stu-id="4e187-120">Multiple tools exist toocreate .vhd files.</span></span> <span data-ttu-id="4e187-121">Például egy virtualizálási megoldást használja például a Hyper-V toocreate hello .vhd fájl, és hello operációs rendszer telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4e187-121">For example, you can use a virtualization solution such as Hyper-V toocreate hello .vhd file and install hello operating system.</span></span> <span data-ttu-id="4e187-122">Leírja, hogyan tooinstall és használja a Hyper-V, tekintse meg a [telepítése Hyper-V virtuális gép létrehozása és](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e187-122">For instructions about how tooinstall and use Hyper-V, see [Install Hyper-V and create a virtual machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="4e187-123">hello újabb VHDX formátum nem támogatott az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="4e187-123">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="4e187-124">Hello tooVHD lemezformátum konvertálja a Hyper-V kezelőjével vagy parancsmag hello [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e187-124">You can convert hello disk tooVHD format by using Hyper-V Manager or hello cmdlet [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx).</span></span> <span data-ttu-id="4e187-125">Emellett nincs egy [kapcsolatos útmutató az MSDN-en toouse freebsd rendszerű Hyper-v](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e187-125">In addition, there is a [tutorial on MSDN about how toouse FreeBSD with Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).</span></span>
>
>

<span data-ttu-id="4e187-126">Ez a feladat hello öt lépéseket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="4e187-126">This task includes hello following five steps:</span></span>

## <a name="step-1-prepare-hello-image-for-upload"></a><span data-ttu-id="4e187-127">1. lépés: Hello lemezképének előkészítése feltöltése</span><span class="sxs-lookup"><span data-stu-id="4e187-127">Step 1: Prepare hello image for upload</span></span>
<span data-ttu-id="4e187-128">Hello freebsd rendszerű operációs rendszer telepítési hello virtuális gépen végezze el a következő eljárások hello:</span><span class="sxs-lookup"><span data-stu-id="4e187-128">On hello virtual machine where you installed hello FreeBSD operating system, complete hello following procedures:</span></span>

1. <span data-ttu-id="4e187-129">Engedélyezze a DHCP.</span><span class="sxs-lookup"><span data-stu-id="4e187-129">Enable DHCP.</span></span>

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart
2. <span data-ttu-id="4e187-130">SSH engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4e187-130">Enable SSH.</span></span>

    <span data-ttu-id="4e187-131">Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="4e187-131">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span> <span data-ttu-id="4e187-132">Alapértelmezés szerint engedélyezve van a freebsd rendszerű lemezről telepítése után.</span><span class="sxs-lookup"><span data-stu-id="4e187-132">By default it is enabled after installation from FreeBSD disc.</span></span> 
3. <span data-ttu-id="4e187-133">A soros konzol beállítása.</span><span class="sxs-lookup"><span data-stu-id="4e187-133">Set up a serial console.</span></span>

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf
4. <span data-ttu-id="4e187-134">Telepítse a sudo.</span><span class="sxs-lookup"><span data-stu-id="4e187-134">Install sudo.</span></span>

    <span data-ttu-id="4e187-135">hello root fiókjának le van tiltva, az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="4e187-135">hello root account is disabled in Azure.</span></span> <span data-ttu-id="4e187-136">Ez azt jelenti, hogy egy nem rendszerjogosultságú felhasználói toorun parancsokat emelt szintű jogosultságokkal a tooutilize sudo van szüksége.</span><span class="sxs-lookup"><span data-stu-id="4e187-136">This means you need tooutilize sudo from an unprivileged user toorun commands with elevated privileges.</span></span>

        # pkg install sudo
   
5. <span data-ttu-id="4e187-137">Előfeltételek az Azure-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="4e187-137">Prerequisites for Azure Agent.</span></span>

        # pkg install python27  
        # pkg install Py27-setuptools  
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git
6. <span data-ttu-id="4e187-138">Telepítse az Azure Agent ügynököt.</span><span class="sxs-lookup"><span data-stu-id="4e187-138">Install Azure Agent.</span></span>

    <span data-ttu-id="4e187-139">hello hello Azure ügynök legújabb kiadása mindig található [github](https://github.com/Azure/WALinuxAgent/releases).</span><span class="sxs-lookup"><span data-stu-id="4e187-139">hello latest release of hello Azure Agent can always be found on [github](https://github.com/Azure/WALinuxAgent/releases).</span></span> <span data-ttu-id="4e187-140">verzió 2.0.10 hello + hivatalosan támogatja a freebsd rendszerű 10 & 10.1 és hello verziója 2.1.4 + (beleértve a 2.2.x) hivatalosan támogatja a freebsd rendszerű 10.2 és a későbbi kibocsátásokban megtörténik.</span><span class="sxs-lookup"><span data-stu-id="4e187-140">hello version 2.0.10 + officially supports FreeBSD 10 & 10.1, and hello version 2.1.4 + (including 2.2.x) officially supports FreeBSD 10.2 and later releases.</span></span>

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    <span data-ttu-id="4e187-141">2.0 most használja 2.0.16 példa:</span><span class="sxs-lookup"><span data-stu-id="4e187-141">For 2.0, let's use 2.0.16 as an example:</span></span>

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    <span data-ttu-id="4e187-142">2.1 most használja 2.1.4 példa:</span><span class="sxs-lookup"><span data-stu-id="4e187-142">For 2.1, let's use 2.1.4 as an example:</span></span>

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

   > [!IMPORTANT]
   > <span data-ttu-id="4e187-143">Azure-ügynök telepítése után már fut egy jó ötlet tooverify:</span><span class="sxs-lookup"><span data-stu-id="4e187-143">After you install Azure Agent, it's a good idea tooverify that it's running:</span></span>
   >
   >

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # ps auxw | grep waagent
        root   639   0.0  0.5 104620 17520 u0- I    05:17    0:00.20 python /usr/local/sbin/waagent -daemon (python2.7)
        # cat /var/log/waagent.log
7. <span data-ttu-id="4e187-144">Hello rendszer kiosztásának megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="4e187-144">Deprovision hello system.</span></span>

    <span data-ttu-id="4e187-145">Deprovision hello rendszer tooclean, és ellenőrizze, megfelelő-e reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="4e187-145">Deprovision hello system tooclean it and make it suitable for reprovisioning.</span></span> <span data-ttu-id="4e187-146">hello következő parancs is törli hello utolsó kiosztott felhasználói fiókkal és a kapcsolódó hello adatokat:</span><span class="sxs-lookup"><span data-stu-id="4e187-146">hello following command also deletes hello last provisioned user account and hello associated data:</span></span>

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    <span data-ttu-id="4e187-147">Most, állítsa le a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="4e187-147">Now you can shut down your VM.</span></span>

## <a name="step-2-create-a-storage-account-in-azure"></a><span data-ttu-id="4e187-148">2. lépés: Az Azure storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="4e187-148">Step 2: Create a storage account in Azure</span></span>
<span data-ttu-id="4e187-149">A storage-fiókot az Azure tooupload .vhd-fájllá, azok használt toocreate egy virtuális gép kell.</span><span class="sxs-lookup"><span data-stu-id="4e187-149">You need a storage account in Azure tooupload a .vhd file so it can be used toocreate a virtual machine.</span></span> <span data-ttu-id="4e187-150">Hello Azure klasszikus portál toocreate tárfiókot is használhat.</span><span class="sxs-lookup"><span data-stu-id="4e187-150">You can use hello Azure classic portal toocreate a storage account.</span></span>

1. <span data-ttu-id="4e187-151">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="4e187-151">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="4e187-152">Hello parancssávon válassza **új**.</span><span class="sxs-lookup"><span data-stu-id="4e187-152">On hello command bar, select **New**.</span></span>
3. <span data-ttu-id="4e187-153">Válassza ki **adatszolgáltatások** > **tárolási** > **Gyorslétrehozás**.</span><span class="sxs-lookup"><span data-stu-id="4e187-153">Select **Data Services** > **Storage** > **Quick Create**.</span></span>

    ![Storage-fiók gyors létrehozása](./media/freebsd-create-upload-vhd/Storage-quick-create.png)
4. <span data-ttu-id="4e187-155">Töltse ki a hello mezőket az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="4e187-155">Fill out hello fields as follows:</span></span>

   * <span data-ttu-id="4e187-156">A hello **URL-cím** mezőbe írja be egy altartomány neve toouse hello tárolási fiók URL-címben.</span><span class="sxs-lookup"><span data-stu-id="4e187-156">In hello **URL** field, type a subdomain name toouse in hello storage account URL.</span></span> <span data-ttu-id="4e187-157">hello bejegyzés 3-24 számokat és kisbetűket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="4e187-157">hello entry can contain from 3-24 numbers and lowercase letters.</span></span> <span data-ttu-id="4e187-158">Ez a név lesz hello állomásnév hello URL-címen, amely használt tooaddress Azure Blob Storage tárolóban, az Azure Queue storage vagy a hello előfizetéshez tartozó Azure Table storage-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="4e187-158">This name becomes hello host name within hello URL that is used tooaddress Azure Blob storage, Azure Queue storage, or Azure Table storage resources for hello subscription.</span></span>
   * <span data-ttu-id="4e187-159">A hello **hely/Affinitáscsoport** legördülő menüben válassza ki a hello **helye vagy affinitáscsoportja** hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="4e187-159">In hello **Location/Affinity Group** drop-down menu, choose hello **location or affinity group** for hello storage account.</span></span> <span data-ttu-id="4e187-160">Az affinitáscsoportokban lehetővé teszi, hogy a felhőszolgáltatás és a tárolási be hello ugyanabban az adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="4e187-160">An affinity group lets you put your cloud services and storage in hello same data center.</span></span>
   * <span data-ttu-id="4e187-161">A hello **replikációs** mezőben, döntse el, hogy toouse **Georedundáns** replikációs hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="4e187-161">In hello **Replication** field, decide whether toouse **Geo-Redundant** replication for hello storage account.</span></span> <span data-ttu-id="4e187-162">A georeplikáció alapértelmezés szerint be van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="4e187-162">Geo-replication is turned on by default.</span></span> <span data-ttu-id="4e187-163">Ez a beállítás replikálja az adatokat tooa másodlagos helyen, semmilyen költség tooyou, hello elsődleges helyet, hogy a tároló átadja a feladatokat toothat helyre, ha súlyos hiba történik.</span><span class="sxs-lookup"><span data-stu-id="4e187-163">This option replicates your data tooa secondary location, at no cost tooyou, so that your storage fails over toothat location if a major failure occurs at hello primary location.</span></span> <span data-ttu-id="4e187-164">másodlagos hely hello automatikusan hozzá van rendelve, és nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="4e187-164">hello secondary location is assigned automatically and can't be changed.</span></span> <span data-ttu-id="4e187-165">Ha módosítania kell a felhőalapú tároló megfelelő toolegal követelmények vagy a szervezeti házirend hello helyét teljesebb körű vezérlése, kikapcsolhatja a georeplikáció.</span><span class="sxs-lookup"><span data-stu-id="4e187-165">If you need more control over hello location of your cloud-based storage due toolegal requirements or organizational policy, you can turn off geo-replication.</span></span> <span data-ttu-id="4e187-166">Azonban figyelembe, hogy később bekapcsolása georeplikáció, fizetnie kell egyszeri adatátviteli díjat tooreplicate a meglévő adatok toohello másodlagos helye.</span><span class="sxs-lookup"><span data-stu-id="4e187-166">However, be aware that if you later turn on geo-replication, you will be charged a one-time data transfer fee tooreplicate your existing data toohello secondary location.</span></span> <span data-ttu-id="4e187-167">Tárolási szolgáltatások nélkül georeplikáció kedvezményes áron érhető el.</span><span class="sxs-lookup"><span data-stu-id="4e187-167">Storage services without geo-replication are offered at a discount.</span></span> <span data-ttu-id="4e187-168">A tárfiókok georeplikáció kezelésével kapcsolatos további részletekért itt található: [Azure Storage replikációs](../../../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="4e187-168">More details about managing geo-replication of storage accounts can be found here: [Azure Storage replication](../../../storage/common/storage-redundancy.md).</span></span>

     ![Írja be a tárolási fiók adatait](./media/freebsd-create-upload-vhd/Storage-create-account.png)
5. <span data-ttu-id="4e187-170">Válassza ki **Storage-fiók létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="4e187-170">Select **Create Storage Account**.</span></span> <span data-ttu-id="4e187-171">hello fiók ekkor már megjelenik a **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="4e187-171">hello account now appears under **storage**.</span></span>

    ![A tárfiók sikeresen létrehozva](./media/freebsd-create-upload-vhd/Storagenewaccount.png)
6. <span data-ttu-id="4e187-173">Ezután hozzon létre egy a feltöltött .vhd fájlokat tároló.</span><span class="sxs-lookup"><span data-stu-id="4e187-173">Next, create a container for your uploaded .vhd files.</span></span> <span data-ttu-id="4e187-174">Hello tárfiók neve, majd válassza ki és **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="4e187-174">Select hello storage account name, and then select **Containers**.</span></span>

    ![Tárolási fiók részletei](./media/freebsd-create-upload-vhd/storageaccount_detail.png)
7. <span data-ttu-id="4e187-176">Válassza ki **létrehozni egy tárolót**.</span><span class="sxs-lookup"><span data-stu-id="4e187-176">Select **Create a Container**.</span></span>

    ![Tárolási fiók részletei](./media/freebsd-create-upload-vhd/storageaccount_container.png)
8. <span data-ttu-id="4e187-178">A hello **neve** mezőbe írja be a tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="4e187-178">In hello **Name** field, type a name for your container.</span></span> <span data-ttu-id="4e187-179">Ezt követően a hello **hozzáférés** legördülő menüben válassza kívánt hozzáférési házirendet milyen típusú.</span><span class="sxs-lookup"><span data-stu-id="4e187-179">Then, in hello **Access** drop-down menu, select what type of access policy you want.</span></span>

    ![Tárolónév](./media/freebsd-create-upload-vhd/storageaccount_containervalues.png)

   > [!NOTE]
   > <span data-ttu-id="4e187-181">Alapértelmezés szerint a hello tároló privát, és csak hello fiók tulajdonosának hozzáférhet.</span><span class="sxs-lookup"><span data-stu-id="4e187-181">By default, hello container is private and can only be accessed by hello account owner.</span></span> <span data-ttu-id="4e187-182">tooallow nyilvános olvasási hozzáférés toohello blobok hello tároló, de nem toohello a tároló tulajdonságainak és metaadatok, használja a hello **nyilvános Blob** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4e187-182">tooallow public read access toohello blobs in hello container, but not toohello container properties and metadata, use hello **Public Blob** option.</span></span> <span data-ttu-id="4e187-183">tooallow teljes nyilvános olvasási hozzáférés hello tároló és a blobokat, akkor hello **nyilvános tárolókban** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4e187-183">tooallow full public read access for hello container and blobs, use hello **Public Container** option.</span></span>
   >
   >

## <a name="step-3-prepare-hello-connection-tooazure"></a><span data-ttu-id="4e187-184">3. lépés: Hello kapcsolat tooAzure előkészítése</span><span class="sxs-lookup"><span data-stu-id="4e187-184">Step 3: Prepare hello connection tooAzure</span></span>
<span data-ttu-id="4e187-185">A .vhd fájlt tölthet, meg kell tooestablish biztonságos kapcsolatot a számítógép és az Azure-előfizetés között.</span><span class="sxs-lookup"><span data-stu-id="4e187-185">Before you can upload a .vhd file, you need tooestablish a secure connection between your computer and your Azure subscription.</span></span> <span data-ttu-id="4e187-186">Hello Azure Active Directory (Azure AD) módszert használja, vagy tanúsítvány metódus toodo hello azt.</span><span class="sxs-lookup"><span data-stu-id="4e187-186">You can use hello Azure Active Directory (Azure AD) method or hello certificate method toodo it.</span></span>

### <a name="use-hello-azure-ad-method-tooupload-a-vhd-file"></a><span data-ttu-id="4e187-187">Az Azure AD hello metódus tooupload .vhd fájl használata</span><span class="sxs-lookup"><span data-stu-id="4e187-187">Use hello Azure AD method tooupload a .vhd file</span></span>
1. <span data-ttu-id="4e187-188">Nyissa meg hello Azure PowerShell-konzolon.</span><span class="sxs-lookup"><span data-stu-id="4e187-188">Open hello Azure PowerShell console.</span></span>
2. <span data-ttu-id="4e187-189">Írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4e187-189">Type hello following command:</span></span>  
    `Add-AzureAccount`

    <span data-ttu-id="4e187-190">Ez a parancs megnyit egy bejelentkezési ablakot, ahol jelentkezhet be a munkahelyi vagy iskolai fiókjával.</span><span class="sxs-lookup"><span data-stu-id="4e187-190">This command opens a sign-in window where you can sign in with your work or school account.</span></span>

    ![PowerShell-ablakot](./media/freebsd-create-upload-vhd/add_azureaccount.png)
3. <span data-ttu-id="4e187-192">Azure hitelesíti, és menti a hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="4e187-192">Azure authenticates and saves hello credential information.</span></span> <span data-ttu-id="4e187-193">Majd lezárja hello ablak.</span><span class="sxs-lookup"><span data-stu-id="4e187-193">Then it closes hello window.</span></span>

### <a name="use-hello-certificate-method-tooupload-a-vhd-file"></a><span data-ttu-id="4e187-194">Hello tanúsítvány metódus tooupload .vhd fájl használata</span><span class="sxs-lookup"><span data-stu-id="4e187-194">Use hello certificate method tooupload a .vhd file</span></span>
1. <span data-ttu-id="4e187-195">Nyissa meg hello Azure PowerShell-konzolon.</span><span class="sxs-lookup"><span data-stu-id="4e187-195">Open hello Azure PowerShell console.</span></span>
2. <span data-ttu-id="4e187-196">Típus: `Get-AzurePublishSettingsFile`.</span><span class="sxs-lookup"><span data-stu-id="4e187-196">Type:  `Get-AzurePublishSettingsFile`.</span></span>
3. <span data-ttu-id="4e187-197">Egy böngészőablakban megnyitja, és hogy toodownload hello .publishsettings fájlt kér.</span><span class="sxs-lookup"><span data-stu-id="4e187-197">A browser window opens and prompts you toodownload hello .publishsettings file.</span></span> <span data-ttu-id="4e187-198">Ez a fájl tartalmazza az adatokat és az Azure-előfizetés tanúsítványát.</span><span class="sxs-lookup"><span data-stu-id="4e187-198">This file contains information and a certificate for your Azure subscription.</span></span>

    ![Böngésző letöltési oldala](./media/freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)
4. <span data-ttu-id="4e187-200">Hello .publishsettings fájlt mentse.</span><span class="sxs-lookup"><span data-stu-id="4e187-200">Save hello .publishsettings file.</span></span>
5. <span data-ttu-id="4e187-201">Típus: `Import-AzurePublishSettingsFile <PathToFile>`, ahol `<PathToFile>` hello teljes elérési útja toohello .publishsettings fájlt.</span><span class="sxs-lookup"><span data-stu-id="4e187-201">Type:  `Import-AzurePublishSettingsFile <PathToFile>`, where `<PathToFile>` is hello full path toohello .publishsettings file.</span></span>

   <span data-ttu-id="4e187-202">További információkért lásd: [Ismerkedés az Azure-parancsmagokkal](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e187-202">For more information, see [Get started with Azure cmdlets](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).</span></span>

   <span data-ttu-id="4e187-203">PowerShell konfigurálásával kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4e187-203">For more information about installing and configuring PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="step-4-upload-hello-vhd-file"></a><span data-ttu-id="4e187-204">4. lépés: Hello .vhd fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="4e187-204">Step 4: Upload hello .vhd file</span></span>
<span data-ttu-id="4e187-205">Hello .vhd fájlt tölt fel, ha azt bárhol elhelyezheti a Blob-tároló belül.</span><span class="sxs-lookup"><span data-stu-id="4e187-205">When you upload hello .vhd file, you can place it anywhere within your Blob storage.</span></span> <span data-ttu-id="4e187-206">Az alábbiakban néhány fogja használni, amikor hello-fájl feltöltése feltételek:</span><span class="sxs-lookup"><span data-stu-id="4e187-206">Following are some terms you will use when you upload hello file:</span></span>

* <span data-ttu-id="4e187-207">**BlobStorageURL** 2. lépésben létrehozott tárfiók hello hello URL-cím.</span><span class="sxs-lookup"><span data-stu-id="4e187-207">**BlobStorageURL** is hello URL for hello storage account that you created in Step 2.</span></span>
* <span data-ttu-id="4e187-208">**YourImagesFolder** a Blob storage tárolóban hello ahol van toostore a képeket.</span><span class="sxs-lookup"><span data-stu-id="4e187-208">**YourImagesFolder** is hello container within Blob storage where you want toostore your images.</span></span>
* <span data-ttu-id="4e187-209">**VHDName** hello címke sem, hogy megjelenik-hello Azure klasszikus portál tooidentify hello virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="4e187-209">**VHDName** is hello label that appears in hello Azure classic portal tooidentify hello virtual hard disk.</span></span>
* <span data-ttu-id="4e187-210">**PathToVHDFile** hello teljes elérési útját és nevét hello .vhd fájl.</span><span class="sxs-lookup"><span data-stu-id="4e187-210">**PathToVHDFile** is hello full path and name of hello .vhd file.</span></span>

<span data-ttu-id="4e187-211">Az előző lépésben hello használt hello Azure PowerShell ablakban írja be:</span><span class="sxs-lookup"><span data-stu-id="4e187-211">From hello Azure PowerShell window you used in hello previous step, type:</span></span>

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-hello-uploaded-vhd-file"></a><span data-ttu-id="4e187-212">5. lépés: Hello feltöltött .vhd fájl a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="4e187-212">Step 5: Create a VM with hello uploaded .vhd file</span></span>
<span data-ttu-id="4e187-213">Hello .vhd fájl feltöltése után az egyéni lemezképek, hogy az előfizetéshez, majd hozzon létre egy virtuális gépet egyéni lemezképpel kép toohello listájaként adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="4e187-213">After you upload hello .vhd file, you can add it as an image toohello list of custom images that are associated with your subscription and create a virtual machine with this custom image.</span></span>

1. <span data-ttu-id="4e187-214">Az előző lépésben hello használt hello Azure PowerShell ablakban írja be:</span><span class="sxs-lookup"><span data-stu-id="4e187-214">From hello Azure PowerShell window you used in hello previous step, type:</span></span>

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of hello VHD> -OS <Type of hello OS on hello VHD>

   > [!NOTE]
   > <span data-ttu-id="4e187-215">Linux használják hello operációs rendszer típusa.</span><span class="sxs-lookup"><span data-stu-id="4e187-215">Use Linux as hello OS type.</span></span> <span data-ttu-id="4e187-216">hello aktuális Azure PowerShell-verzió csak a "Linux" vagy "Windows" fogad paraméterként.</span><span class="sxs-lookup"><span data-stu-id="4e187-216">hello current Azure PowerShell version accepts only “Linux” or “Windows” as a parameter.</span></span>
   >
   >
2. <span data-ttu-id="4e187-217">Hello előző lépések végrehajtása után a hello új lemezkép szerepel-e, ha úgy dönt, hogy hello **képek** hello klasszikus Azure portálra a lap.</span><span class="sxs-lookup"><span data-stu-id="4e187-217">After you complete hello previous steps, hello new image is listed when you choose hello **Images** tab on hello Azure classic portal.</span></span>  

    ![Kép kiválasztása](./media/freebsd-create-upload-vhd/addfreebsdimage.png)
3. <span data-ttu-id="4e187-219">Hozzon létre egy virtuális gép hello gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="4e187-219">Create a virtual machine from hello gallery.</span></span> <span data-ttu-id="4e187-220">Az új lemezképet már elérhető a **saját lemezképek**.</span><span class="sxs-lookup"><span data-stu-id="4e187-220">This new image is now available under **My Images**.</span></span>
4. <span data-ttu-id="4e187-221">Hello új lemezkép kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="4e187-221">Select hello new image.</span></span> <span data-ttu-id="4e187-222">A következő halad át hello kér tooset be egy állomásnevet, a jelszót, az SSH-kulcs és a stb.</span><span class="sxs-lookup"><span data-stu-id="4e187-222">Next, go through hello prompts tooset up a host name, password, SSH key, and so on.</span></span>

    ![Kép: egyéni](./media/freebsd-create-upload-vhd/createfreebsdimageinazure.png)
5. <span data-ttu-id="4e187-224">Hello kiépítés befejezése után megjelenik az Azure-beli freebsd rendszerű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="4e187-224">After you complete hello provisioning, you'll see your FreeBSD VM running in Azure.</span></span>

    ![Az azure-ban freebsd rendszerű kép](./media/freebsd-create-upload-vhd/freebsdimageinazure.png)
