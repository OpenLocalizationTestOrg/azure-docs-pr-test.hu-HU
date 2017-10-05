---
title: "Hozzon létre és freebsd rendszerű Virtuálisgép-lemezkép feltöltése |} Microsoft Docs"
description: "Megismerheti, hogyan kell létrehozni, majd töltse fel a virtuális merevlemez (VHD), amely tartalmazza a freebsd rendszerű operációs rendszer egy Azure virtuális gép létrehozása"
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
ms.openlocfilehash: 918f454784a9676297077c2e94c3e49ab2872d2f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a><span data-ttu-id="5dc52-103">Hozzon létre és freebsd rendszerű virtuális merevlemez feltöltése az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="5dc52-103">Create and upload a FreeBSD VHD to Azure</span></span>
<span data-ttu-id="5dc52-104">Ez a cikk bemutatja, hogyan hozhat létre, és töltse fel a virtuális merevlemez (VHD), amely tartalmazza a freebsd rendszerű operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="5dc52-104">This article shows you how to create and upload a virtual hard disk (VHD) that contains the FreeBSD operating system.</span></span> <span data-ttu-id="5dc52-105">Miután a feltöltés, segítségével azt saját képként (VM) virtuális gép létrehozása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5dc52-105">After you upload it, you can use it as your own image to create a virtual machine (VM) in Azure.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="5dc52-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5dc52-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5dc52-107">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="5dc52-107">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="5dc52-108">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="5dc52-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="5dc52-109">További információ a Resource Manager modellt használó virtuális merevlemez feltöltése: [Itt](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5dc52-109">For information about uploading a VHD using the Resource Manager model, see [here](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5dc52-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5dc52-110">Prerequisites</span></span>
<span data-ttu-id="5dc52-111">Ez a cikk feltételezi, hogy rendelkezik-e a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="5dc52-111">This article assumes that you have the following items:</span></span>

* <span data-ttu-id="5dc52-112">**Azure-előfizetés**– Ha nincs fiókja, létrehozhat egyet, néhány perc alatt.</span><span class="sxs-lookup"><span data-stu-id="5dc52-112">**An Azure subscription**--If you don't have an account, you can create one in just a couple of minutes.</span></span> <span data-ttu-id="5dc52-113">Ha MSDN-előfizetéssel rendelkezik, tekintse meg [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="5dc52-113">If you have an MSDN subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span> <span data-ttu-id="5dc52-114">Ellenkező esetben megtudhatja, hogyan [hozzon létre egy próbafiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5dc52-114">Otherwise, learn how to [create a free trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span>  
* <span data-ttu-id="5dc52-115">**Az Azure PowerShell eszközök**--az Azure PowerShell modul telepítve legyen, és az Azure-előfizetés használatára konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="5dc52-115">**Azure PowerShell tools**--The Azure PowerShell module must be installed and configured to use your Azure subscription.</span></span> <span data-ttu-id="5dc52-116">A modul letöltése: [Azure letölti](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="5dc52-116">To download the module, see [Azure downloads](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="5dc52-117">Itt érhető el egy oktatóanyag, amely ismerteti, hogyan telepítse és konfigurálja a modult.</span><span class="sxs-lookup"><span data-stu-id="5dc52-117">A tutorial that describes how to install and configure the module is available here.</span></span> <span data-ttu-id="5dc52-118">Használja a [Azure letölti](https://azure.microsoft.com/downloads/) parancsmag a virtuális merevlemez feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="5dc52-118">Use the [Azure Downloads](https://azure.microsoft.com/downloads/) cmdlet to upload the VHD.</span></span>
* <span data-ttu-id="5dc52-119">**Freebsd rendszerű operációs rendszer van telepítve, a .vhd-fájllá**--egy támogatott freebsd rendszerű operációs rendszer telepítenie kell egy virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="5dc52-119">**FreeBSD operating system installed in a .vhd file**--A supported   FreeBSD operating system must be installed to a virtual hard disk.</span></span> <span data-ttu-id="5dc52-120">Több különféle eszköz található .vhd fájlok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5dc52-120">Multiple tools exist to create .vhd files.</span></span> <span data-ttu-id="5dc52-121">Például a Hyper-V hálózatvirtualizálási megoldás segítségével például a .vhd fájlt, és az operációs rendszer telepítése.</span><span class="sxs-lookup"><span data-stu-id="5dc52-121">For example, you can use a virtualization solution such as Hyper-V to create the .vhd file and install the operating system.</span></span> <span data-ttu-id="5dc52-122">Leírja, hogyan kell telepíteni és használni a Hyper-V, lásd: [telepítése Hyper-V virtuális gép létrehozása és](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dc52-122">For instructions about how to install and use Hyper-V, see [Install Hyper-V and create a virtual machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="5dc52-123">Az újabb VHDX formátum nem támogatott az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5dc52-123">The newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="5dc52-124">VHD-formátumban a lemezen a Hyper-V kezelője vagy a parancsmag használatával konvertálhatja [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dc52-124">You can convert the disk to VHD format by using Hyper-V Manager or the cmdlet [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx).</span></span> <span data-ttu-id="5dc52-125">Emellett nincs egy [freebsd rendszerű Hyper-v használatával kapcsolatos útmutató az MSDN-en](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dc52-125">In addition, there is a [tutorial on MSDN about how to use FreeBSD with Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).</span></span>
>
>

<span data-ttu-id="5dc52-126">Ez a feladat a következő öt szükséges lépéseket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="5dc52-126">This task includes the following five steps:</span></span>

## <a name="step-1-prepare-the-image-for-upload"></a><span data-ttu-id="5dc52-127">1. lépés: A lemezkép előkészítése feltöltése</span><span class="sxs-lookup"><span data-stu-id="5dc52-127">Step 1: Prepare the image for upload</span></span>
<span data-ttu-id="5dc52-128">A virtuális gépen, amelyre telepítette a freebsd rendszerű operációs rendszer az alábbi eljárások végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="5dc52-128">On the virtual machine where you installed the FreeBSD operating system, complete the following procedures:</span></span>

1. <span data-ttu-id="5dc52-129">Engedélyezze a DHCP.</span><span class="sxs-lookup"><span data-stu-id="5dc52-129">Enable DHCP.</span></span>

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart
2. <span data-ttu-id="5dc52-130">SSH engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5dc52-130">Enable SSH.</span></span>

    <span data-ttu-id="5dc52-131">Győződjön meg arról, hogy az SSH-kiszolgálót telepítse és konfigurálja a rendszerindítás elindításához.</span><span class="sxs-lookup"><span data-stu-id="5dc52-131">Ensure that the SSH server is installed and configured to start at boot time.</span></span> <span data-ttu-id="5dc52-132">Alapértelmezés szerint engedélyezve van a freebsd rendszerű lemezről telepítése után.</span><span class="sxs-lookup"><span data-stu-id="5dc52-132">By default it is enabled after installation from FreeBSD disc.</span></span> 
3. <span data-ttu-id="5dc52-133">A soros konzol beállítása.</span><span class="sxs-lookup"><span data-stu-id="5dc52-133">Set up a serial console.</span></span>

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf
4. <span data-ttu-id="5dc52-134">Telepítse a sudo.</span><span class="sxs-lookup"><span data-stu-id="5dc52-134">Install sudo.</span></span>

    <span data-ttu-id="5dc52-135">A legfelső szintű fiók le van tiltva, az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5dc52-135">The root account is disabled in Azure.</span></span> <span data-ttu-id="5dc52-136">Ez azt jelenti, hogy a magas jogosultságszinttel nem rendelkező parancsok futtatásához emelt szintű jogosultságokkal rendelkező felhasználó sudo használatára kell.</span><span class="sxs-lookup"><span data-stu-id="5dc52-136">This means you need to utilize sudo from an unprivileged user to run commands with elevated privileges.</span></span>

        # pkg install sudo
   
5. <span data-ttu-id="5dc52-137">Előfeltételek az Azure-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="5dc52-137">Prerequisites for Azure Agent.</span></span>

        # pkg install python27  
        # pkg install Py27-setuptools  
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git
6. <span data-ttu-id="5dc52-138">Telepítse az Azure Agent ügynököt.</span><span class="sxs-lookup"><span data-stu-id="5dc52-138">Install Azure Agent.</span></span>

    <span data-ttu-id="5dc52-139">Az Azure Agent ügynököt a legújabb kiadásának mindig található [github](https://github.com/Azure/WALinuxAgent/releases).</span><span class="sxs-lookup"><span data-stu-id="5dc52-139">The latest release of the Azure Agent can always be found on [github](https://github.com/Azure/WALinuxAgent/releases).</span></span> <span data-ttu-id="5dc52-140">A verzió 2.0.10 hivatalosan freebsd rendszerű 10 támogatja & 10.1, és a verzió 2.1.4 + (beleértve a 2.2.x) hivatalosan támogatja freebsd rendszerű 10.2 és a későbbi kibocsátásokban megtörténik.</span><span class="sxs-lookup"><span data-stu-id="5dc52-140">The version 2.0.10 + officially supports FreeBSD 10 & 10.1, and the version 2.1.4 + (including 2.2.x) officially supports FreeBSD 10.2 and later releases.</span></span>

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    <span data-ttu-id="5dc52-141">2.0 most használja 2.0.16 példa:</span><span class="sxs-lookup"><span data-stu-id="5dc52-141">For 2.0, let's use 2.0.16 as an example:</span></span>

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    <span data-ttu-id="5dc52-142">2.1 most használja 2.1.4 példa:</span><span class="sxs-lookup"><span data-stu-id="5dc52-142">For 2.1, let's use 2.1.4 as an example:</span></span>

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

   > [!IMPORTANT]
   > <span data-ttu-id="5dc52-143">Azure-ügynök telepítése után célszerű ellenőrizni, hogy fut-e:</span><span class="sxs-lookup"><span data-stu-id="5dc52-143">After you install Azure Agent, it's a good idea to verify that it's running:</span></span>
   >
   >

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # ps auxw | grep waagent
        root   639   0.0  0.5 104620 17520 u0- I    05:17    0:00.20 python /usr/local/sbin/waagent -daemon (python2.7)
        # cat /var/log/waagent.log
7. <span data-ttu-id="5dc52-144">A rendszer kiosztásának megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="5dc52-144">Deprovision the system.</span></span>

    <span data-ttu-id="5dc52-145">A rendszer megtisztítsa tőle, és lehetővé teszi a megfelelő reprovisioning kiosztásának megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="5dc52-145">Deprovision the system to clean it and make it suitable for reprovisioning.</span></span> <span data-ttu-id="5dc52-146">A következő parancs is törli, az utolsó kiépített felhasználói fiókot és a kapcsolódó adatokat:</span><span class="sxs-lookup"><span data-stu-id="5dc52-146">The following command also deletes the last provisioned user account and the associated data:</span></span>

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    <span data-ttu-id="5dc52-147">Most, állítsa le a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="5dc52-147">Now you can shut down your VM.</span></span>

## <a name="step-2-create-a-storage-account-in-azure"></a><span data-ttu-id="5dc52-148">2. lépés: Az Azure storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="5dc52-148">Step 2: Create a storage account in Azure</span></span>
<span data-ttu-id="5dc52-149">A .vhd fájl feltöltéséhez, így a virtuális gépek létrehozásához használhatók az Azure-ban storage-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="5dc52-149">You need a storage account in Azure to upload a .vhd file so it can be used to create a virtual machine.</span></span> <span data-ttu-id="5dc52-150">A klasszikus Azure portál segítségével hozzon létre egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="5dc52-150">You can use the Azure classic portal to create a storage account.</span></span>

1. <span data-ttu-id="5dc52-151">Jelentkezzen be a [klasszikus Azure portálra](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="5dc52-151">Sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="5dc52-152">A parancssávon válassza **új**.</span><span class="sxs-lookup"><span data-stu-id="5dc52-152">On the command bar, select **New**.</span></span>
3. <span data-ttu-id="5dc52-153">Válassza ki **adatszolgáltatások** > **tárolási** > **Gyorslétrehozás**.</span><span class="sxs-lookup"><span data-stu-id="5dc52-153">Select **Data Services** > **Storage** > **Quick Create**.</span></span>

    ![Storage-fiók gyors létrehozása](./media/freebsd-create-upload-vhd/Storage-quick-create.png)
4. <span data-ttu-id="5dc52-155">Töltse ki a mezőket az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5dc52-155">Fill out the fields as follows:</span></span>

   * <span data-ttu-id="5dc52-156">Az a **URL-cím** mezőbe írja be egy altartomány nevet a tárolási fiók URL-címét.</span><span class="sxs-lookup"><span data-stu-id="5dc52-156">In the **URL** field, type a subdomain name to use in the storage account URL.</span></span> <span data-ttu-id="5dc52-157">A bejegyzés a 3-24 számokat és kisbetűket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="5dc52-157">The entry can contain from 3-24 numbers and lowercase letters.</span></span> <span data-ttu-id="5dc52-158">Ez a név lesz az állomásnév, az Azure Blob Storage tárolóban, Azure Queue storage vagy az előfizetéshez tartozó Azure Table storage-erőforrások megoldására használt URL-cím belül.</span><span class="sxs-lookup"><span data-stu-id="5dc52-158">This name becomes the host name within the URL that is used to address Azure Blob storage, Azure Queue storage, or Azure Table storage resources for the subscription.</span></span>
   * <span data-ttu-id="5dc52-159">Az a **hely/Affinitáscsoport** legördülő menüben válassza ki a **helye vagy affinitáscsoportja** a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="5dc52-159">In the **Location/Affinity Group** drop-down menu, choose the **location or affinity group** for the storage account.</span></span> <span data-ttu-id="5dc52-160">Az affinitáscsoportokban lehetővé teszi a felhőalapú szolgáltatások és a tárolási be ugyanabban az adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="5dc52-160">An affinity group lets you put your cloud services and storage in the same data center.</span></span>
   * <span data-ttu-id="5dc52-161">Az a **replikációs** mezőbe döntsön a **Georedundáns** a tárfiók replikációs.</span><span class="sxs-lookup"><span data-stu-id="5dc52-161">In the **Replication** field, decide whether to use **Geo-Redundant** replication for the storage account.</span></span> <span data-ttu-id="5dc52-162">A georeplikáció alapértelmezés szerint be van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="5dc52-162">Geo-replication is turned on by default.</span></span> <span data-ttu-id="5dc52-163">Ez a beállítás, hogy a tároló átadja a feladatokat, hogy a hely az elsődleges helyen jelentős hiba esetén replikálja a másodlagos helyre, hogy az adatok.</span><span class="sxs-lookup"><span data-stu-id="5dc52-163">This option replicates your data to a secondary location, at no cost to you, so that your storage fails over to that location if a major failure occurs at the primary location.</span></span> <span data-ttu-id="5dc52-164">A másodlagos hely automatikusan hozzá van rendelve, és nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="5dc52-164">The secondary location is assigned automatically and can't be changed.</span></span> <span data-ttu-id="5dc52-165">Ha hatékonyabban szeretné vezérelni jogi követelmények vagy a szervezeti házirend miatt a felhőalapú tároló helyét, kikapcsolhatja a georeplikáció.</span><span class="sxs-lookup"><span data-stu-id="5dc52-165">If you need more control over the location of your cloud-based storage due to legal requirements or organizational policy, you can turn off geo-replication.</span></span> <span data-ttu-id="5dc52-166">Azonban vegye figyelembe, hogy később bekapcsolása georeplikáció, fizetnie kell replikálni a másodlagos hely a meglévő adatokat egyszeri adatok adatátviteli díjat.</span><span class="sxs-lookup"><span data-stu-id="5dc52-166">However, be aware that if you later turn on geo-replication, you will be charged a one-time data transfer fee to replicate your existing data to the secondary location.</span></span> <span data-ttu-id="5dc52-167">Tárolási szolgáltatások nélkül georeplikáció kedvezményes áron érhető el.</span><span class="sxs-lookup"><span data-stu-id="5dc52-167">Storage services without geo-replication are offered at a discount.</span></span> <span data-ttu-id="5dc52-168">A tárfiókok georeplikáció kezelésével kapcsolatos további részletekért itt található: [Azure Storage replikációs](../../../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="5dc52-168">More details about managing geo-replication of storage accounts can be found here: [Azure Storage replication](../../../storage/common/storage-redundancy.md).</span></span>

     ![Írja be a tárolási fiók adatait](./media/freebsd-create-upload-vhd/Storage-create-account.png)
5. <span data-ttu-id="5dc52-170">Válassza ki **Storage-fiók létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5dc52-170">Select **Create Storage Account**.</span></span> <span data-ttu-id="5dc52-171">A fiók ekkor már megjelenik a **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="5dc52-171">The account now appears under **storage**.</span></span>

    ![A tárfiók sikeresen létrehozva](./media/freebsd-create-upload-vhd/Storagenewaccount.png)
6. <span data-ttu-id="5dc52-173">Ezután hozzon létre egy a feltöltött .vhd fájlokat tároló.</span><span class="sxs-lookup"><span data-stu-id="5dc52-173">Next, create a container for your uploaded .vhd files.</span></span> <span data-ttu-id="5dc52-174">A tárfiók nevét, majd válassza ki és **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="5dc52-174">Select the storage account name, and then select **Containers**.</span></span>

    ![Tárolási fiók részletei](./media/freebsd-create-upload-vhd/storageaccount_detail.png)
7. <span data-ttu-id="5dc52-176">Válassza ki **létrehozni egy tárolót**.</span><span class="sxs-lookup"><span data-stu-id="5dc52-176">Select **Create a Container**.</span></span>

    ![Tárolási fiók részletei](./media/freebsd-create-upload-vhd/storageaccount_container.png)
8. <span data-ttu-id="5dc52-178">Az a **neve** mezőbe írja be a tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="5dc52-178">In the **Name** field, type a name for your container.</span></span> <span data-ttu-id="5dc52-179">Ezt követően a a **hozzáférés** legördülő menüben válassza kívánt hozzáférési házirendet milyen típusú.</span><span class="sxs-lookup"><span data-stu-id="5dc52-179">Then, in the **Access** drop-down menu, select what type of access policy you want.</span></span>

    ![Tárolónév](./media/freebsd-create-upload-vhd/storageaccount_containervalues.png)

   > [!NOTE]
   > <span data-ttu-id="5dc52-181">Alapértelmezés szerint a tároló privát, és csak a fiók tulajdonosának hozzáférhet.</span><span class="sxs-lookup"><span data-stu-id="5dc52-181">By default, the container is private and can only be accessed by the account owner.</span></span> <span data-ttu-id="5dc52-182">A tárolóban lévő blobok, de nem a tároló tulajdonságainak és a metaadatok nyilvános olvasási hozzáférés engedélyezéséhez használja a **nyilvános Blob** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5dc52-182">To allow public read access to the blobs in the container, but not to the container properties and metadata, use the **Public Blob** option.</span></span> <span data-ttu-id="5dc52-183">A tároló és a blobok teljes nyilvános olvasási hozzáférés engedélyezéséhez használja a **nyilvános tárolókban** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5dc52-183">To allow full public read access for the container and blobs, use the **Public Container** option.</span></span>
   >
   >

## <a name="step-3-prepare-the-connection-to-azure"></a><span data-ttu-id="5dc52-184">3. lépés: Felkészülés a kapcsolat az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="5dc52-184">Step 3: Prepare the connection to Azure</span></span>
<span data-ttu-id="5dc52-185">A .vhd fájlt tölthet, mielőtt kell a számítógép és az Azure-előfizetés között biztonságos kapcsolatot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5dc52-185">Before you can upload a .vhd file, you need to establish a secure connection between your computer and your Azure subscription.</span></span> <span data-ttu-id="5dc52-186">Ehhez használhatja az Azure Active Directory (Azure AD) vagy a tanúsítvány metódust.</span><span class="sxs-lookup"><span data-stu-id="5dc52-186">You can use the Azure Active Directory (Azure AD) method or the certificate method to do it.</span></span>

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a><span data-ttu-id="5dc52-187">Az Azure AD a módszerhez a .vhd fájl feltöltéséhez</span><span class="sxs-lookup"><span data-stu-id="5dc52-187">Use the Azure AD method to upload a .vhd file</span></span>
1. <span data-ttu-id="5dc52-188">Nyissa meg az Azure PowerShell-konzolt.</span><span class="sxs-lookup"><span data-stu-id="5dc52-188">Open the Azure PowerShell console.</span></span>
2. <span data-ttu-id="5dc52-189">Írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="5dc52-189">Type the following command:</span></span>  
    `Add-AzureAccount`

    <span data-ttu-id="5dc52-190">Ez a parancs megnyit egy bejelentkezési ablakot, ahol jelentkezhet be a munkahelyi vagy iskolai fiókjával.</span><span class="sxs-lookup"><span data-stu-id="5dc52-190">This command opens a sign-in window where you can sign in with your work or school account.</span></span>

    ![PowerShell-ablakot](./media/freebsd-create-upload-vhd/add_azureaccount.png)
3. <span data-ttu-id="5dc52-192">Azure hitelesíti, és menti a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="5dc52-192">Azure authenticates and saves the credential information.</span></span> <span data-ttu-id="5dc52-193">Majd bezárja az ablakot.</span><span class="sxs-lookup"><span data-stu-id="5dc52-193">Then it closes the window.</span></span>

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a><span data-ttu-id="5dc52-194">A tanúsítvány a módszerhez a .vhd fájl feltöltéséhez</span><span class="sxs-lookup"><span data-stu-id="5dc52-194">Use the certificate method to upload a .vhd file</span></span>
1. <span data-ttu-id="5dc52-195">Nyissa meg az Azure PowerShell-konzolt.</span><span class="sxs-lookup"><span data-stu-id="5dc52-195">Open the Azure PowerShell console.</span></span>
2. <span data-ttu-id="5dc52-196">Típus: `Get-AzurePublishSettingsFile`.</span><span class="sxs-lookup"><span data-stu-id="5dc52-196">Type:  `Get-AzurePublishSettingsFile`.</span></span>
3. <span data-ttu-id="5dc52-197">Egy böngészőablakban megnyitja, és megkéri, hogy töltse le a .publishsettings fájlt.</span><span class="sxs-lookup"><span data-stu-id="5dc52-197">A browser window opens and prompts you to download the .publishsettings file.</span></span> <span data-ttu-id="5dc52-198">Ez a fájl tartalmazza az adatokat és az Azure-előfizetés tanúsítványát.</span><span class="sxs-lookup"><span data-stu-id="5dc52-198">This file contains information and a certificate for your Azure subscription.</span></span>

    ![Böngésző letöltési oldala](./media/freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)
4. <span data-ttu-id="5dc52-200">Mentse a .publishsettings fájlt.</span><span class="sxs-lookup"><span data-stu-id="5dc52-200">Save the .publishsettings file.</span></span>
5. <span data-ttu-id="5dc52-201">Típus: `Import-AzurePublishSettingsFile <PathToFile>`, ahol `<PathToFile>` .publishsettings fájl teljes elérési útja.</span><span class="sxs-lookup"><span data-stu-id="5dc52-201">Type:  `Import-AzurePublishSettingsFile <PathToFile>`, where `<PathToFile>` is the full path to the .publishsettings file.</span></span>

   <span data-ttu-id="5dc52-202">További információkért lásd: [Ismerkedés az Azure-parancsmagokkal](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dc52-202">For more information, see [Get started with Azure cmdlets](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).</span></span>

   <span data-ttu-id="5dc52-203">PowerShell konfigurálásával kapcsolatos további információkért lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5dc52-203">For more information about installing and configuring PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="step-4-upload-the-vhd-file"></a><span data-ttu-id="5dc52-204">4. lépés: A .vhd fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="5dc52-204">Step 4: Upload the .vhd file</span></span>
<span data-ttu-id="5dc52-205">A .vhd fájlt tölt fel, ha azt bárhol elhelyezheti a Blob-tároló belül.</span><span class="sxs-lookup"><span data-stu-id="5dc52-205">When you upload the .vhd file, you can place it anywhere within your Blob storage.</span></span> <span data-ttu-id="5dc52-206">Az alábbiakban néhány fogja használni, amikor a fájl feltöltése feltételek:</span><span class="sxs-lookup"><span data-stu-id="5dc52-206">Following are some terms you will use when you upload the file:</span></span>

* <span data-ttu-id="5dc52-207">**BlobStorageURL** az URL-cím, a 2. lépésben létrehozott tárfiók.</span><span class="sxs-lookup"><span data-stu-id="5dc52-207">**BlobStorageURL** is the URL for the storage account that you created in Step 2.</span></span>
* <span data-ttu-id="5dc52-208">**YourImagesFolder** a Blob storage tárolóban van, hol szeretné tárolni a képeket.</span><span class="sxs-lookup"><span data-stu-id="5dc52-208">**YourImagesFolder** is the container within Blob storage where you want to store your images.</span></span>
* <span data-ttu-id="5dc52-209">**VHDName** a címke sem, hogy a virtuális merevlemez azonosítására a klasszikus Azure portálon megjelenik.</span><span class="sxs-lookup"><span data-stu-id="5dc52-209">**VHDName** is the label that appears in the Azure classic portal to identify the virtual hard disk.</span></span>
* <span data-ttu-id="5dc52-210">**PathToVHDFile** a teljes elérési útja és neve a .vhd fájlt.</span><span class="sxs-lookup"><span data-stu-id="5dc52-210">**PathToVHDFile** is the full path and name of the .vhd file.</span></span>

<span data-ttu-id="5dc52-211">Az előző lépésben használt Azure PowerShell-ablakot írja be:</span><span class="sxs-lookup"><span data-stu-id="5dc52-211">From the Azure PowerShell window you used in the previous step, type:</span></span>

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a><span data-ttu-id="5dc52-212">5. lépés: Virtuális gép létrehozása a feltöltött .vhd fájl</span><span class="sxs-lookup"><span data-stu-id="5dc52-212">Step 5: Create a VM with the uploaded .vhd file</span></span>
<span data-ttu-id="5dc52-213">A .vhd fájl feltöltése után hozzáadhatja képként az egyéni lemezképek, hogy az előfizetéshez, majd hozzon létre egy virtuális gépet egyéni lemezképpel listájához.</span><span class="sxs-lookup"><span data-stu-id="5dc52-213">After you upload the .vhd file, you can add it as an image to the list of custom images that are associated with your subscription and create a virtual machine with this custom image.</span></span>

1. <span data-ttu-id="5dc52-214">Az előző lépésben használt Azure PowerShell-ablakot írja be:</span><span class="sxs-lookup"><span data-stu-id="5dc52-214">From the Azure PowerShell window you used in the previous step, type:</span></span>

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

   > [!NOTE]
   > <span data-ttu-id="5dc52-215">Linux operációsrendszer-típus használható.</span><span class="sxs-lookup"><span data-stu-id="5dc52-215">Use Linux as the OS type.</span></span> <span data-ttu-id="5dc52-216">A jelenlegi Azure PowerShell-verzió csak a "Linux" vagy "Windows" fogad paraméterként.</span><span class="sxs-lookup"><span data-stu-id="5dc52-216">The current Azure PowerShell version accepts only “Linux” or “Windows” as a parameter.</span></span>
   >
   >
2. <span data-ttu-id="5dc52-217">Miután elvégezte az előző lépést, az új lemezkép szerepel-e, ha úgy dönt, a **képek** lapját, amelyen a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="5dc52-217">After you complete the previous steps, the new image is listed when you choose the **Images** tab on the Azure classic portal.</span></span>  

    ![Kép kiválasztása](./media/freebsd-create-upload-vhd/addfreebsdimage.png)
3. <span data-ttu-id="5dc52-219">Virtuális gép létrehozása a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="5dc52-219">Create a virtual machine from the gallery.</span></span> <span data-ttu-id="5dc52-220">Az új lemezképet már elérhető a **saját lemezképek**.</span><span class="sxs-lookup"><span data-stu-id="5dc52-220">This new image is now available under **My Images**.</span></span>
4. <span data-ttu-id="5dc52-221">Válassza ki az új lemezképet.</span><span class="sxs-lookup"><span data-stu-id="5dc52-221">Select the new image.</span></span> <span data-ttu-id="5dc52-222">A következő halad át a megjelenő utasításokat állítson be egy állomásnevet, a jelszót, az SSH-kulcs és a stb.</span><span class="sxs-lookup"><span data-stu-id="5dc52-222">Next, go through the prompts to set up a host name, password, SSH key, and so on.</span></span>

    ![Kép: egyéni](./media/freebsd-create-upload-vhd/createfreebsdimageinazure.png)
5. <span data-ttu-id="5dc52-224">Miután elvégezte a kiépítés, látni fogja az Azure-beli freebsd rendszerű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5dc52-224">After you complete the provisioning, you'll see your FreeBSD VM running in Azure.</span></span>

    ![Az azure-ban freebsd rendszerű kép](./media/freebsd-create-upload-vhd/freebsdimageinazure.png)
