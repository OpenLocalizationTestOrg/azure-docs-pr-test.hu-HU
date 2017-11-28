---
title: "egy Azure Linux virtuális gép mentése Oracle ASM aaaSet |} Microsoft Docs"
description: "Gyorsan karban lehessen Oracle ASM be és az Azure környezetben futna."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/19/2017
ms.author: rclaus
ms.openlocfilehash: d6a7046638e919876477d46943faabcb1872acac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-oracle-asm-on-an-azure-linux-virtual-machine"></a><span data-ttu-id="af144-103">Oracle címterület-kezelés beállítása az Azure Linux virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="af144-103">Set up Oracle ASM on an Azure Linux virtual machine</span></span>  

<span data-ttu-id="af144-104">Az Azure virtuális gépek adjon meg egy teljes mértékben konfigurálhatók és rugalmas számítási környezet.</span><span class="sxs-lookup"><span data-stu-id="af144-104">Azure virtual machines provide a fully configurable and flexible computing environment.</span></span> <span data-ttu-id="af144-105">Ez az oktatóanyag ismerteti alapszintű Azure-beli virtuális gép telepítési hello telepítése és konfigurálása az Oracle automatikus tárolási kezelési (ASM) együtt.</span><span class="sxs-lookup"><span data-stu-id="af144-105">This tutorial covers basic Azure virtual machine deployment combined with hello installation and configuration of Oracle Automated Storage Management (ASM).</span></span>  <span data-ttu-id="af144-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="af144-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="af144-107">Hozzon létre, és csatlakozzon az Oracle adatbázis VM tooan</span><span class="sxs-lookup"><span data-stu-id="af144-107">Create and connect tooan Oracle Database VM</span></span>
> * <span data-ttu-id="af144-108">Telepítse és konfigurálja az Oracle automatikus tárolók kezelése</span><span class="sxs-lookup"><span data-stu-id="af144-108">Install and configure Oracle Automated Storage Management</span></span>
> * <span data-ttu-id="af144-109">Telepítse és konfigurálja az Oracle rács infrastruktúra</span><span class="sxs-lookup"><span data-stu-id="af144-109">Install and configure Oracle Grid infrastructure</span></span>
> * <span data-ttu-id="af144-110">Oracle ASM telepítés inicializálása</span><span class="sxs-lookup"><span data-stu-id="af144-110">Initialize an Oracle ASM installation</span></span>
> * <span data-ttu-id="af144-111">A címterület-kezelés által felügyelt az Oracle-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="af144-111">Create an Oracle DB managed by ASM</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="af144-112">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="af144-112">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="af144-113">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="af144-113">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="af144-114">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="af144-114">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prepare-hello-environment"></a><span data-ttu-id="af144-115">Hello környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="af144-115">Prepare hello environment</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="af144-116">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="af144-116">Create a resource group</span></span>

<span data-ttu-id="af144-117">egy erőforráscsoport, toocreate hello használata [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="af144-117">toocreate a resource group, use hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="af144-118">Egy Azure erőforráscsoport egy olyan logikai tároló, amelyre erőforrások telepítése és kezelése.</span><span class="sxs-lookup"><span data-stu-id="af144-118">An Azure resource group is a logical container in which Azure resources are deployed and managed.</span></span> <span data-ttu-id="af144-119">Ebben a példában az erőforráscsoport neve *myResourceGroup* a hello *eastus* régióban.</span><span class="sxs-lookup"><span data-stu-id="af144-119">In this example, a resource group named *myResourceGroup* in hello *eastus* region.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-vm"></a><span data-ttu-id="af144-120">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="af144-120">Create a VM</span></span>

<span data-ttu-id="af144-121">a virtuális gépek toocreate hello Oracle adatbázis-lemezképen alapuló és toouse Oracle ASM konfigurálják, hello használata [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="af144-121">toocreate a virtual machine based on hello Oracle Database image and configure it toouse Oracle ASM, use hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="af144-122">hello alábbi példa létrehoz egy 50 GB négy csatolt adatlemezekkel rendelkező Standard_DS2_v2 méretű myVM nevű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="af144-122">hello following example creates a VM named myVM that is a Standard_DS2_v2 size with four attached data disks of 50 GB each.</span></span> <span data-ttu-id="af144-123">Ha még nem léteznek hello alapértelmezett kulcshelyen, SSH-kulcsok is létrehoz.</span><span class="sxs-lookup"><span data-stu-id="af144-123">If they do not already exist in hello default key location, it also creates SSH keys.</span></span>  <span data-ttu-id="af144-124">toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="af144-124">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

   ```azurecli-interactive
   az vm create --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50 50 50 50
   ```

<span data-ttu-id="af144-125">Hello virtuális gép létrehozása után az Azure CLI információkat a következő példa hasonló toohello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="af144-125">After you create hello VM, Azure CLI displays information similar toohello following example.</span></span> <span data-ttu-id="af144-126">Jegyezze fel a hello értékét `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="af144-126">Note hello value for `publicIpAddress`.</span></span> <span data-ttu-id="af144-127">A cím tooaccess hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="af144-127">You use this address tooaccess hello VM.</span></span>

   ```azurecli
   {
     "fqdns": "",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
     "location": "eastus",
     "macAddress": "00-0D-3A-36-2F-56",
     "powerState": "VM running",
     "privateIpAddress": "10.0.0.4",
     "publicIpAddress": "13.64.104.241",
     "resourceGroup": "myResourceGroup"
   }
   ```

### <a name="connect-toohello-vm"></a><span data-ttu-id="af144-128">Csatlakozás a virtuális gép toohello</span><span class="sxs-lookup"><span data-stu-id="af144-128">Connect toohello VM</span></span>

<span data-ttu-id="af144-129">az SSH-munkamenetet toocreate hello VM és további beállításokat, a következő parancs hello használata.</span><span class="sxs-lookup"><span data-stu-id="af144-129">toocreate an SSH session with hello VM and configure additional settings, use hello following command.</span></span> <span data-ttu-id="af144-130">Cserélje le hello IP-cím hello `publicIpAddress` értéket a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="af144-130">Replace hello IP address with hello `publicIpAddress` value for your VM.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="install-oracle-asm"></a><span data-ttu-id="af144-131">Oracle ASM telepítése</span><span class="sxs-lookup"><span data-stu-id="af144-131">Install Oracle ASM</span></span>

<span data-ttu-id="af144-132">tooinstall Oracle ASM, teljes hello a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="af144-132">tooinstall Oracle ASM, complete hello following steps.</span></span> 

<span data-ttu-id="af144-133">Oracle ASM telepítésével kapcsolatos további információkért lásd: [Oracle ASMLib letölti az Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html).</span><span class="sxs-lookup"><span data-stu-id="af144-133">For more information about installing Oracle ASM, see [Oracle ASMLib Downloads for Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html).</span></span>  

1. <span data-ttu-id="af144-134">A címterület-kezelési telepítésének sorrendje toocontinue legfelső szintű toologin lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="af144-134">You need toologin as root in order toocontinue with ASM installation:</span></span>

   ```bash
   sudo su -
   ```
   
2. <span data-ttu-id="af144-135">A parancsok további tooinstall Oracle ASM összetevőket:</span><span class="sxs-lookup"><span data-stu-id="af144-135">Run these additional commands tooinstall Oracle ASM components:</span></span>

   ```bash
    yum list | grep oracleasm 
    yum -y install kmod-oracleasm.x86_64 
    yum -y install oracleasm-support.x86_64 
    wget http://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    yum -y install oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    rm -f oracleasmlib-2.0.12-1.el6.x86_64.rpm
   ```

3. <span data-ttu-id="af144-136">Győződjön meg arról, hogy telepítve van-e az Oracle ASM:</span><span class="sxs-lookup"><span data-stu-id="af144-136">Verify that Oracle ASM is installed:</span></span>

   ```bash
   rpm -qa |grep oracleasm
   ```

    <span data-ttu-id="af144-137">hello a parancs kimenetében hello összetevők a következő:</span><span class="sxs-lookup"><span data-stu-id="af144-137">hello output of this command should list hello following components:</span></span>

    ```bash
   oracleasm-support-2.1.10-4.el6.x86_64
   kmod-oracleasm-2.0.8-15.el6_9.x86_64
   oracleasmlib-2.0.12-1.el6.x86_64
    ```

4. <span data-ttu-id="af144-138">A címterület-kezelés megfelelően igényel meghatározott felhasználókra és szerepkörökre a rendelés toofunction.</span><span class="sxs-lookup"><span data-stu-id="af144-138">ASM requires specific users and roles in order toofunction correctly.</span></span> <span data-ttu-id="af144-139">a következő parancsok hello hello működéséhez szükséges felhasználói fiókok és csoportok létrehozása:</span><span class="sxs-lookup"><span data-stu-id="af144-139">hello following commands create hello pre-requisite user accounts and groups:</span></span> 

   ```bash
    groupadd -g 54345 asmadmin 
    groupadd -g 54346 asmdba 
    groupadd -g 54347 asmoper 
    useradd -u 3000 -g oinstall -G dba,asmadmin,asmdba,asmoper grid 
    usermod -g oinstall -G dba,asmdba,asmadmin oracle
   ```

5. <span data-ttu-id="af144-140">Ellenőrizze a felhasználók és csoportok létrehozott megfelelően:</span><span class="sxs-lookup"><span data-stu-id="af144-140">Verify users and groups were created correctly:</span></span>

   ```bash
   id grid
   ```

    <span data-ttu-id="af144-141">hello a parancs kimenetében hello következő felhasználókat és csoportokat:</span><span class="sxs-lookup"><span data-stu-id="af144-141">hello output of this command should list hello following users and groups:</span></span>

    ```bash
    uid=3000(grid) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54345(asmadmin),54346(asmdba),54347(asmoper)
    ```
 
6. <span data-ttu-id="af144-142">Hozzon létre egy mappát felhasználói *rács* és hello tulajdonos módosítása:</span><span class="sxs-lookup"><span data-stu-id="af144-142">Create a folder for user *grid* and change hello owner:</span></span>

   ```bash
   mkdir /u01/app/grid 
   chown grid:oinstall /u01/app/grid
   ```

## <a name="set-up-oracle-asm"></a><span data-ttu-id="af144-143">Oracle címterület-kezelés beállítása</span><span class="sxs-lookup"><span data-stu-id="af144-143">Set up Oracle ASM</span></span>

<span data-ttu-id="af144-144">Ebben az oktatóanyagban hello alapértelmezett felhasználói van *rács* és hello alapértelmezett csoport *asmadmin*.</span><span class="sxs-lookup"><span data-stu-id="af144-144">For this tutorial, hello default user is *grid* and hello default group is *asmadmin*.</span></span> <span data-ttu-id="af144-145">Győződjön meg arról, hogy hello *oracle* felhasználói hello asmadmin csoport része.</span><span class="sxs-lookup"><span data-stu-id="af144-145">Ensure that hello *oracle* user is part of hello asmadmin group.</span></span> <span data-ttu-id="af144-146">az Oracle ASM telepítése, a következő lépéseket teljes hello mentése tooset:</span><span class="sxs-lookup"><span data-stu-id="af144-146">tooset up your Oracle ASM installation, complete hello following steps:</span></span>

1. <span data-ttu-id="af144-147">Hello Oracle címterület-kezelési könyvtár illesztőprogram beállítása magában foglalja a hello alapértelmezett felhasználói (grid) és az alapértelmezett csoportot (asmadmin) valamint a rendszerindító meghajtó toostart hello konfigurálása (válassza ki a y), és a rendszerindító lemezek tooscan (y kiválasztása).</span><span class="sxs-lookup"><span data-stu-id="af144-147">Setting up hello Oracle ASM library driver involves defining hello default user (grid) and default group (asmadmin) as well as configuring hello drive toostart on boot (choose y) and tooscan for disks on boot (choose y).</span></span> <span data-ttu-id="af144-148">Tooanswer hello kér a parancs a következő hello lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="af144-148">You need tooanswer hello prompts from hello following command:</span></span>

   ```bash
   /usr/sbin/oracleasm configure -i
   ```

   <span data-ttu-id="af144-149">a parancs kimenetének hello alábbihoz hasonló toohello a következő, a megválaszolt kérdések toobe leállítása.</span><span class="sxs-lookup"><span data-stu-id="af144-149">hello output of this command should look similar toohello following, stopping with prompts toobe answered.</span></span>

    ```bash
   Configuring hello Oracle ASM library driver.

   This will configure hello on-boot properties of hello Oracle ASM library
   driver. hello following questions will determine whether hello driver is
   loaded on boot and what permissions it will have. hello current values
   will be shown in brackets ('[]'). Hitting <ENTER> without typing an
   answer will keep that current value. Ctrl-C will abort.

   Default user tooown hello driver interface []: grid
   Default group tooown hello driver interface []: asmadmin
   Start Oracle ASM library driver on boot (y/n) [n]: y
   Scan for Oracle ASM disks on boot (y/n) [y]: y
   Writing Oracle ASM library driver configuration: done
   ```

2. <span data-ttu-id="af144-150">Hello lemez konfiguráció megtekintése:</span><span class="sxs-lookup"><span data-stu-id="af144-150">View hello disk configuration:</span></span>
   ```bash
   cat /proc/partitions
   ```

   <span data-ttu-id="af144-151">a parancs kimenetének hello alábbihoz hasonló toohello a rendelkezésre álló lemezek tőzsdei követően</span><span class="sxs-lookup"><span data-stu-id="af144-151">hello output of this command should look similar toohello following listing of available disks</span></span>

   ```bash
   8       16   14680064 sdb
   8       17   14678976 sdb1
   8        0   52428800 sda
   8        1     512000 sda1
   8        2   51915776 sda2
   8       48   52428800 sdd
   8       64   52428800 sde
   8       80   52428800 sdf
   8       32   52428800 sdc
   11       0       1152 sr0
   ```

3. <span data-ttu-id="af144-152">Lemez formázása */dev/sdc* hello futtassa a következő parancsot, és a fogadó hello kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="af144-152">Format disk */dev/sdc* by running hello following command and answering hello prompts with:</span></span>
   - <span data-ttu-id="af144-153">*n*Új partíció</span><span class="sxs-lookup"><span data-stu-id="af144-153">*n* for new partition</span></span>
   - <span data-ttu-id="af144-154">*p* elsődleges partíció</span><span class="sxs-lookup"><span data-stu-id="af144-154">*p* for primary partition</span></span>
   - <span data-ttu-id="af144-155">*1* tooselect hello első partíció</span><span class="sxs-lookup"><span data-stu-id="af144-155">*1* tooselect hello first partition</span></span>
   - <span data-ttu-id="af144-156">nyomja le az ENTER `enter` a hello alapértelmezett első palack</span><span class="sxs-lookup"><span data-stu-id="af144-156">press `enter` for hello default first cylinder</span></span>
   - <span data-ttu-id="af144-157">nyomja le az ENTER `enter` a hello alapértelmezett utolsó palack</span><span class="sxs-lookup"><span data-stu-id="af144-157">press `enter` for hello default last cylinder</span></span>
   - <span data-ttu-id="af144-158">nyomja le az ENTER *w* toowrite hello módosítások toohello partíciós tábla</span><span class="sxs-lookup"><span data-stu-id="af144-158">press *w* toowrite hello changes toohello partition table</span></span>  

   ```bash
   fdisk /dev/sdc
   ```
   
   <span data-ttu-id="af144-159">Használja a fenti hello választ, hello fdisk parancs kimenetét hello alábbihoz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="af144-159">Using hello answers provided above, hello output for hello fdisk command should look like hello following:</span></span>

   ```bash
   Device contains not a valid DOS partition table, or Sun, SGI or OSF disklabel
   Building a new DOS disklabel with disk identifier 0xf865c6ca.
   Changes will remain in memory only, until you decide toowrite them.
   After that, of course, hello previous content won't be recoverable.

   Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

   hello device presents a logical sector size that is smaller than
   hello physical sector size. Aligning tooa physical sector (or optimal
   I/O) size boundary is recommended, or performance may be impacted.

   WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
           switch off hello mode (command 'c') and change display units to
           sectors (command 'u').

   Command (m for help): n
   Command action
     e   extended
     p   primary partition (1-4)
   p
   Partition number (1-4): 1
   First cylinder (1-6527, default 1):
   Using default value 1
   Last cylinder, +cylinders or +size{K,M,G} (1-6527, default 6527):
   Using default value 6527

   Command (m for help): w
   hello partition table has been altered!

   Calling ioctl() toore-read partition table.
   Syncing disks.
   ```

4. <span data-ttu-id="af144-160">Ismétlődő hello fdisk parancsot megelőző `/dev/sdd`, `/dev/sde`, és `/dev/sdf`.</span><span class="sxs-lookup"><span data-stu-id="af144-160">Repeat hello preceding fdisk command for `/dev/sdd`, `/dev/sde`, and `/dev/sdf`.</span></span>

5. <span data-ttu-id="af144-161">Ellenőrizze a lemezkonfigurációt hello:</span><span class="sxs-lookup"><span data-stu-id="af144-161">Check hello disk configuration:</span></span>

   ```bash
   cat /proc/partitions
   ```

   <span data-ttu-id="af144-162">hello parancs kimenetében hello hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="af144-162">hello output of hello command should look like hello following:</span></span>

   ```bash
   major minor  #blocks  name

     8       16   14680064 sdb
     8       17   14678976 sdb1
     8       32   52428800 sdc
     8       33   52428096 sdc1
     8       48   52428800 sdd
     8       49   52428096 sdd1
     8       64   52428800 sde
     8       65   52428096 sde1
     8       80   52428800 sdf
     8       81   52428096 sdf1
     8        0   52428800 sda
     8        1     512000 sda1
     8        2   51915776 sda2
     11       0    1048575 sr0
   ```

6. <span data-ttu-id="af144-163">Hello Oracle címterület-kezelési szolgáltatás állapotának ellenőrzéséhez és hello Oracle címterület-kezelés szolgáltatás elindítása:</span><span class="sxs-lookup"><span data-stu-id="af144-163">Check hello Oracle ASM service status and start hello Oracle ASM service:</span></span>

   ```bash
   service oracleasm status 
   service oracleasm start
   ```

   <span data-ttu-id="af144-164">hello parancs kimenetében hello hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="af144-164">hello output of hello command should look like hello following:</span></span>
   
   ```bash
   Checking if ASM is loaded: no
   Checking if /dev/oracleasm is mounted: no
   Initializing hello Oracle ASMLib driver:                     [  OK  ]
   Scanning hello system for Oracle ASMLib disks:               [  OK  ]
   ```

7. <span data-ttu-id="af144-165">Oracle ASM lemezek létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="af144-165">Create Oracle ASM disks:</span></span>

   ```bash
   service oracleasm createdisk ASMSP /dev/sdc1 
   service oracleasm createdisk DATA /dev/sdd1 
   service oracleasm createdisk DATA1 /dev/sde1 
   service oracleasm createdisk FRA /dev/sdf1
   ```    

   <span data-ttu-id="af144-166">hello parancs kimenetében hello hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="af144-166">hello output of hello command should look like hello following:</span></span>

   ```bash
   Marking disk "ASMSP" as an ASM disk:                       [  OK  ]
   Marking disk "DATA" as an ASM disk:                        [  OK  ]
   Marking disk "DATA1" as an ASM disk:                       [  OK  ]
   Marking disk "FRA" as an ASM disk:                         [  OK  ]
   ```

8. <span data-ttu-id="af144-167">Oracle ASM lemezek listáját:</span><span class="sxs-lookup"><span data-stu-id="af144-167">List Oracle ASM disks:</span></span>

   ```bash
   service oracleasm listdisks
   ```   

   <span data-ttu-id="af144-168">hello hello parancs kimenetében ki a következő Oracle ASM lemezek hello:</span><span class="sxs-lookup"><span data-stu-id="af144-168">hello output of hello command should list off hello following Oracle ASM disks:</span></span>

   ```bash
    ASMSP
    DATA
    DATA1
    FRA
   ```

9. <span data-ttu-id="af144-169">Módosíthatja a hello gyökérszintű, oracle és rács felhasználók hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="af144-169">Change hello passwords for hello root, oracle, and grid users.</span></span> <span data-ttu-id="af144-170">**Jegyezze fel ezeket az új jelszót a** módon használja őket később hello telepítése során.</span><span class="sxs-lookup"><span data-stu-id="af144-170">**Make note of these new passwords** as you are using them later during hello installation.</span></span>

   ```bash
   passwd oracle 
   passwd grid 
   passwd root
   ```

10. <span data-ttu-id="af144-171">Hello mappa engedély módosítása:</span><span class="sxs-lookup"><span data-stu-id="af144-171">Change hello folder permission:</span></span>

   ```bash
   chmod -R 775 /opt 
   chown grid:oinstall /opt 
   chown oracle:oinstall /dev/sdc1 
   chown oracle:oinstall /dev/sdd1 
   chown oracle:oinstall /dev/sde1 
   chown oracle:oinstall /dev/sdf1 
   chmod 600 /dev/sdc1 
   chmod 600 /dev/sdd1 
   chmod 600 /dev/sde1 
   chmod 600 /dev/sdf1
   ```

## <a name="download-and-prepare-oracle-grid-infrastructure"></a><span data-ttu-id="af144-172">Töltse le és Oracle rács infrastruktúra előkészítése</span><span class="sxs-lookup"><span data-stu-id="af144-172">Download and prepare Oracle Grid Infrastructure</span></span>

<span data-ttu-id="af144-173">toodownload és hello Oracle rács infrastruktúra szoftver, a következő lépéseket teljes hello előkészítése:</span><span class="sxs-lookup"><span data-stu-id="af144-173">toodownload and prepare hello Oracle Grid Infrastructure software, complete hello following steps:</span></span>

1. <span data-ttu-id="af144-174">Oracle rács infrastruktúra letöltését hello [Oracle ASM letöltési oldala](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html).</span><span class="sxs-lookup"><span data-stu-id="af144-174">Download Oracle Grid Infrastructure from hello [Oracle ASM download page](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html).</span></span> 

   <span data-ttu-id="af144-175">Című hello letöltés alatt **Oracle adatbázis 12c kiadás 1 rács infrastruktúra (12.1.0.2.0) a Linux x86-64**, töltse le a két hello .zip fájlokat.</span><span class="sxs-lookup"><span data-stu-id="af144-175">Under hello download titled **Oracle Database 12c Release 1 Grid Infrastructure (12.1.0.2.0) for Linux x86-64**, download hello two .zip files.</span></span>

2. <span data-ttu-id="af144-176">Hello .zip fájlokat tooyour ügyfélszámítógép letöltése után biztonságos másolási protokoll (SCP) toocopy hello fájlok tooyour VM is használhatja:</span><span class="sxs-lookup"><span data-stu-id="af144-176">After you download hello .zip files tooyour client computer, you can use Secure Copy Protocol (SCP) toocopy hello files tooyour VM:</span></span>

   ```bash
   scp *.zip <publicIpAddress>:.
   ```

3. <span data-ttu-id="af144-177">SSH vissza az Oracle Azure-ban a rendelés toomove hello .zip fájlokat hello az / opt mappát.</span><span class="sxs-lookup"><span data-stu-id="af144-177">SSH back into your Oracle VM in Azure in order toomove hello .zip files into hello /opt folder.</span></span> <span data-ttu-id="af144-178">Majd módosíthatja hello fájlok hello tulajdonosa:</span><span class="sxs-lookup"><span data-stu-id="af144-178">Then, change hello owner of hello files:</span></span>

   ```bash
   ssh <publicIPAddress>
   sudo mv ./*.zip /opt
   cd /opt
   sudo chown grid:oinstall linuxamd64_12102_grid_1of2.zip
   sudo chown grid:oinstall linuxamd64_12102_grid_2of2.zip
   ```

4. <span data-ttu-id="af144-179">Hello fájlokat csomagolja ki.</span><span class="sxs-lookup"><span data-stu-id="af144-179">Unzip hello files.</span></span> <span data-ttu-id="af144-180">(Telepítés hello Linux csomagolja ki eszköz Ha még nincs telepítve.)</span><span class="sxs-lookup"><span data-stu-id="af144-180">(Install hello Linux unzip tool if it's not already installed.)</span></span>
   
   ```bash
   sudo yum install unzip
   sudo unzip linuxamd64_12102_grid_1of2.zip
   sudo unzip linuxamd64_12102_grid_2of2.zip
   ```

5. <span data-ttu-id="af144-181">Engedély módosítása:</span><span class="sxs-lookup"><span data-stu-id="af144-181">Change permission:</span></span>
   
   ```bash
   sudo chown -R grid:oinstall /opt/grid
   ```

6. <span data-ttu-id="af144-182">Frissítés konfigurált lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="af144-182">Update configured swap space.</span></span> <span data-ttu-id="af144-183">Az Oracle rács összetevők legalább 6,8 GB swap terület tooinstall rács kell.</span><span class="sxs-lookup"><span data-stu-id="af144-183">Oracle Grid components need at least 6.8 GB of swap space tooinstall Grid.</span></span> <span data-ttu-id="af144-184">hello alapértelmezett lapozófájl-kapacitás az Azure-ban Oracle Linux képek mérete csak 2048MB.</span><span class="sxs-lookup"><span data-stu-id="af144-184">hello default swap file size for Oracle Linux images in Azure is only 2048MB.</span></span> <span data-ttu-id="af144-185">Tooincrease kell `ResourceDisk.SwapSizeMB` a hello `/etc/waagent.conf` fájlt, és indítsa újra a hello WALinuxAgent szolgáltatást ahhoz, hogy a frissített hello beállítások tootake hatást.</span><span class="sxs-lookup"><span data-stu-id="af144-185">You need tooincrease `ResourceDisk.SwapSizeMB` in hello `/etc/waagent.conf` file and restart hello WALinuxAgent service in order for hello updated settings tootake effect.</span></span> <span data-ttu-id="af144-186">Mivel a csak olvasható fájlba, toochange fájl engedélyek tooenable írási hozzáférés szükséges.</span><span class="sxs-lookup"><span data-stu-id="af144-186">Because it is a read-only file, you need toochange file permissions tooenable write access.</span></span>

   ```bash
   sudo chmod 777 /etc/waagent.conf  
   vi /etc/waagent.conf
   ```
   
   <span data-ttu-id="af144-187">Keresse meg `ResourceDisk.SwapSizeMB` , és módosítsa a hello érték túl**8192**.</span><span class="sxs-lookup"><span data-stu-id="af144-187">Search for `ResourceDisk.SwapSizeMB` and change hello value too**8192**.</span></span> <span data-ttu-id="af144-188">Szüksége lesz a toopress `insert` tooenter beszúrási módban hello értékének típusa **8192** , és nyomja le az `esc` tooreturn toocommand mód.</span><span class="sxs-lookup"><span data-stu-id="af144-188">You will need toopress `insert` tooenter insert mode, type in hello value of **8192** and then press `esc` tooreturn toocommand mode.</span></span> <span data-ttu-id="af144-189">toowrite hello módosításokat, és kilép hello fájlt, írja be a `:wq` nyomja le az ENTER `enter`.</span><span class="sxs-lookup"><span data-stu-id="af144-189">toowrite hello changes and quit hello file, type `:wq` and press `enter`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="af144-190">Erősen ajánlott, hogy mindig használjon `WALinuxAgent` tooconfigure lapozófájl, hogy mindig létrejön a hello helyi elmúló (lemez ideiglenes) a legjobb teljesítmény érdekében.</span><span class="sxs-lookup"><span data-stu-id="af144-190">We highly recommend that you always use `WALinuxAgent` tooconfigure swap space so that it's always created on hello local ephemeral disk (temporary disk) for best performance.</span></span> <span data-ttu-id="af144-191">További információkért lásd: [hogyan tooadd egy felcserélés Linux Azure virtuális gépek fájlt](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="af144-191">For more information on, see [How tooadd a swap file in Linux Azure virtual machines](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines).</span></span>

## <a name="prepare-your-local-client-and-vm-toorun-x11"></a><span data-ttu-id="af144-192">A helyi ügyfél és a virtuális gép toorun x11 előkészítése</span><span class="sxs-lookup"><span data-stu-id="af144-192">Prepare your local client and VM toorun x11</span></span>
<span data-ttu-id="af144-193">Grafikus felület toocomplete hello telepítési és konfigurációs Oracle ASM konfigurálása szükséges.</span><span class="sxs-lookup"><span data-stu-id="af144-193">Configuring Oracle ASM requires a graphical interface toocomplete hello install and configuration.</span></span> <span data-ttu-id="af144-194">Használunk hello x11 protokoll toofacilitate ehhez a telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="af144-194">We are using hello x11 protocol toofacilitate this installation.</span></span> <span data-ttu-id="af144-195">Ha egy ügyfél rendszer (Mac vagy Linux), amely már rendelkezik X11 használ képességek engedélyezve és konfigurálva - kihagyhatja ezt a konfigurációt, és kizárólagos tooWindows gépek beállítása.</span><span class="sxs-lookup"><span data-stu-id="af144-195">If you are using a client system (Mac or Linux) that already has X11 capabilities enabled and configured - you can skip this configuration and setup exclusive tooWindows machines.</span></span> 

1. <span data-ttu-id="af144-196">[Töltse le a PuTTY](http://www.putty.org/) és [Xming letöltése](https://xming.en.softonic.com/) tooyour Windows rendszerű számítógépen.</span><span class="sxs-lookup"><span data-stu-id="af144-196">[Download PuTTY](http://www.putty.org/) and [download Xming](https://xming.en.softonic.com/) tooyour Windows computer.</span></span> <span data-ttu-id="af144-197">Mindkét alkalmazást, a folytatás előtt hello alapértelmezett értékekkel hello telepítésének toocomplete kell.</span><span class="sxs-lookup"><span data-stu-id="af144-197">You will need toocomplete hello installation of both of these applications with hello default values before proceeding.</span></span>

2. <span data-ttu-id="af144-198">PuTTY telepítése után nyisson meg egy parancssort, váltson át hello PuTTY mappát (például, C:\Program Files\PuTTY), és futtassa `puttygen.exe` a rendezés toogenerate egy kulcsot.</span><span class="sxs-lookup"><span data-stu-id="af144-198">After you install PuTTY, open a command prompt, change into hello PuTTY folder (for example, C:\Program Files\PuTTY), and run `puttygen.exe` in order toogenerate a key.</span></span>

3. <span data-ttu-id="af144-199">A PuTTY Megosztottelérésikulcs-készítő:</span><span class="sxs-lookup"><span data-stu-id="af144-199">In PuTTY Key Generator:</span></span>
   
   1. <span data-ttu-id="af144-200">Hozzon létre egy kulcsot hello kiválasztásával `Generate` gombra.</span><span class="sxs-lookup"><span data-stu-id="af144-200">Generate a key by selecting hello `Generate` button.</span></span>
   2. <span data-ttu-id="af144-201">Hello kulcs (Ctrl + C) hello tartalom másolása.</span><span class="sxs-lookup"><span data-stu-id="af144-201">Copy hello contents of hello key (Ctrl+C).</span></span>
   3. <span data-ttu-id="af144-202">Jelölje be hello `Save private key` gombra.</span><span class="sxs-lookup"><span data-stu-id="af144-202">Select hello `Save private key` button.</span></span>
   4. <span data-ttu-id="af144-203">Egy hozzáférési kóddal hello kulcs védelme hello figyelmeztetést figyelmen kívül, majd válassza ki `OK`.</span><span class="sxs-lookup"><span data-stu-id="af144-203">Ignore hello warning about securing hello key with a passphrase, and then select `OK`.</span></span>

   ![Képernyőkép a PuTTY Megosztottelérésikulcs-készítő](./media/oracle-asm/puttykeygen.png)

4. <span data-ttu-id="af144-205">A virtuális gépen, futtassa az alábbi parancsokat:</span><span class="sxs-lookup"><span data-stu-id="af144-205">In your VM, run these commands:</span></span>

   ```bash
   sudo su - grid
   mkdir .ssh 
   cd .ssh
   ```

5. <span data-ttu-id="af144-206">Hozzon létre egy fájlt `authorized_keys`.</span><span class="sxs-lookup"><span data-stu-id="af144-206">Create a file named `authorized_keys`.</span></span> <span data-ttu-id="af144-207">Hello kulcs tartalmát hello illessze be ezt a fájlt, és mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="af144-207">Paste hello contents of hello key in this file, and then save hello file.</span></span>

   > [!NOTE]
   > <span data-ttu-id="af144-208">hello kulcs tartalmaznia kell a hello karakterlánc `ssh-rsa`.</span><span class="sxs-lookup"><span data-stu-id="af144-208">hello key must contain hello string `ssh-rsa`.</span></span> <span data-ttu-id="af144-209">Hello hello kulcs tartalmát is, egy egyszerű szövegsor kell lennie.</span><span class="sxs-lookup"><span data-stu-id="af144-209">Also, hello contents of hello key must be a single line of text.</span></span>
   >  

6. <span data-ttu-id="af144-210">Indítsa el a PuTTY az ügyfélrendszeren.</span><span class="sxs-lookup"><span data-stu-id="af144-210">On your client system, start PuTTY.</span></span> <span data-ttu-id="af144-211">A hello **kategória** ablaktáblában lépjen túl**kapcsolat** > **SSH** > **Auth**. A hello **hitelesítéshez titkos kulcsfájl** mezőbe keresse meg a korábban létrehozott toohello kulcs.</span><span class="sxs-lookup"><span data-stu-id="af144-211">In hello **Category** pane, go too**Connection** > **SSH** > **Auth**. In hello **Private key file for authentication** box, browse toohello key that you generated earlier.</span></span>

   ![Képernyőfelvétel a hello SSH hitelesítés beállításai](./media/oracle-asm/setprivatekey.png)

7. <span data-ttu-id="af144-213">A hello **kategória** ablaktáblában lépjen túl**kapcsolat** > **SSH** > **X11**.</span><span class="sxs-lookup"><span data-stu-id="af144-213">In hello **Category** pane, go too**Connection** > **SSH** > **X11**.</span></span> <span data-ttu-id="af144-214">Jelölje be hello **engedélyezése X11 továbbítási** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="af144-214">Select hello **Enable X11 forwarding** check box.</span></span>

   ![Képernyőfelvétel a hello SSH X11 továbbítási beállítások](./media/oracle-asm/enablex11.png)

8. <span data-ttu-id="af144-216">A hello **kategória** ablaktáblában lépjen túl**munkamenet**.</span><span class="sxs-lookup"><span data-stu-id="af144-216">In hello **Category** pane, go too**Session**.</span></span> <span data-ttu-id="af144-217">Adja meg az Oracle ASM virtuális gép `<publicIPaddress>` hello állomás neve párbeszédpanelen töltse ki az új `Saved Session` nevet, majd kattintson a `Save`.</span><span class="sxs-lookup"><span data-stu-id="af144-217">Enter your Oracle ASM VM `<publicIPaddress>` in hello host name dialog box, fill in a new `Saved Session` name and then click on `Save`.</span></span>  <span data-ttu-id="af144-218">Ha menteni, kattintson a `open` tooconnect tooyour Oracle ASM virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="af144-218">Once saved, click on `open` tooconnect tooyour Oracle ASM virtual machine.</span></span>  <span data-ttu-id="af144-219">hello első csatlakozáskor akkor figyelmeztetést kap hello távoli rendszer nem gyorsítótárazza a beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="af144-219">hello first time you connect you are warned  hello remote system is not cached in your registry.</span></span> <span data-ttu-id="af144-220">Kattintson a `yes` tooadd, és továbbra is.</span><span class="sxs-lookup"><span data-stu-id="af144-220">Click on `yes` tooadd it and continue.</span></span>

   ![Képernyőfelvétel a hello PuTTY munkamenet-beállításokkal](./media/oracle-asm/puttysession.png)

## <a name="install-oracle-grid-infrastructure"></a><span data-ttu-id="af144-222">Oracle-rács infrastruktúra telepítése</span><span class="sxs-lookup"><span data-stu-id="af144-222">Install Oracle Grid Infrastructure</span></span>

<span data-ttu-id="af144-223">tooinstall Oracle rács infrastruktúra, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="af144-223">tooinstall Oracle Grid Infrastructure, complete hello following steps:</span></span>

1. <span data-ttu-id="af144-224">Jelentkezzen be a **rács**.</span><span class="sxs-lookup"><span data-stu-id="af144-224">Sign in as **grid**.</span></span> <span data-ttu-id="af144-225">(A jelszó megadása nélkül képes toosign kell lennie.)</span><span class="sxs-lookup"><span data-stu-id="af144-225">(You should be able toosign in without being prompted for a password.)</span></span> 

   > [!NOTE]
   > <span data-ttu-id="af144-226">Ha Windows fut, győződjön meg arról Xming elindította hello telepítés megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="af144-226">If you are running Windows, make sure you have started Xming before you begin hello installation.</span></span>

   ```bash
   cd /opt/grid
   ./runInstaller
   ```

   <span data-ttu-id="af144-227">Megnyílik a Oracle rács infrastruktúra 12c kiadás 1 telepítőjét.</span><span class="sxs-lookup"><span data-stu-id="af144-227">Oracle Grid Infrastructure 12c Release 1 Installer opens.</span></span> <span data-ttu-id="af144-228">(Hello telepítő toostart néhány percig is eltarthat.)</span><span class="sxs-lookup"><span data-stu-id="af144-228">(It might take a few minutes for hello  installer toostart.)</span></span>

2. <span data-ttu-id="af144-229">A hello **telepítési lehetőség kiválasztása** lapon, hogy melyik **telepítése és konfigurálása Oracle rács infrastruktúra egy önálló kiszolgáló**.</span><span class="sxs-lookup"><span data-stu-id="af144-229">On hello **Select Installation Option** page, select **Install and Configure Oracle Grid Infrastructure for a Standalone Server**.</span></span>

   ![Képernyőfelvétel a hello telepítő telepítési lehetőség kiválasztása lap](./media/oracle-asm/install01.png)

3. <span data-ttu-id="af144-231">A hello **termék nyelvek kiválasztása** oldalon **angol** vagy hello nyelv van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="af144-231">On hello **Select Product Languages** page, ensure **English** or hello language that you want is selected.</span></span>  <span data-ttu-id="af144-232">Kattintson a `next` gombra.</span><span class="sxs-lookup"><span data-stu-id="af144-232">Click `next`.</span></span>

4. <span data-ttu-id="af144-233">A hello **ASM lemez csoport létrehozása** lap:</span><span class="sxs-lookup"><span data-stu-id="af144-233">On hello **Create ASM Disk Group** page:</span></span>
   - <span data-ttu-id="af144-234">Adjon meg egy nevet hello lemez csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="af144-234">Enter a name for hello disk group.</span></span>
   - <span data-ttu-id="af144-235">A **redundancia**, jelölje be **külső**.</span><span class="sxs-lookup"><span data-stu-id="af144-235">Under **Redundancy**, select **External**.</span></span>
   - <span data-ttu-id="af144-236">A **foglalásiegység-méret**, jelölje be **4**.</span><span class="sxs-lookup"><span data-stu-id="af144-236">Under **Allocation Unit Size**, select **4**.</span></span>
   - <span data-ttu-id="af144-237">A **lemezek hozzáadása a**, jelölje be **ORCLASMSP**.</span><span class="sxs-lookup"><span data-stu-id="af144-237">Under **Add Disks**, select **ORCLASMSP**.</span></span>
   - <span data-ttu-id="af144-238">Kattintson a `next` gombra.</span><span class="sxs-lookup"><span data-stu-id="af144-238">Click `next`.</span></span>

5. <span data-ttu-id="af144-239">A hello **adja meg a címterület-kezelés jelszó** lapra, jelölje be hello **használja ugyanazt a jelszót a fiókokhoz** lehetőséget, és adjon meg egy jelszót.</span><span class="sxs-lookup"><span data-stu-id="af144-239">On hello **Specify ASM Password** page, select hello **Use same passwords for these accounts** option, and enter a password.</span></span>

   ![Hello telepítő ASM jelszót adjon meg oldalát bemutató képernyőkép](./media/oracle-asm/install04.png)

6. <span data-ttu-id="af144-241">A hello **felügyeleti beállítások megadása** lapon hello beállítás tooconfigure EM felhő vezérlő rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="af144-241">On hello **Specify Management Options** page, you have hello option tooconfigure EM Cloud Control.</span></span> <span data-ttu-id="af144-242">Ez a beállítás kihagyjuk - kattintson `next` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="af144-242">We are skipping this option - click `next` toocontinue.</span></span> 

7. <span data-ttu-id="af144-243">A hello **jogosultsággal rendelkező operációs rendszer csoportjának** lapján hello alapértelmezett beállításokat használja.</span><span class="sxs-lookup"><span data-stu-id="af144-243">On hello **Privileged Operating System Groups** page, use hello default settings.</span></span> <span data-ttu-id="af144-244">Kattintson a `next` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="af144-244">Click `next` toocontinue.</span></span>

8. <span data-ttu-id="af144-245">A hello **adja meg a telepítési hely** lapján hello alapértelmezett beállításokat használja.</span><span class="sxs-lookup"><span data-stu-id="af144-245">On hello **Specify Installation Location** page, use hello default settings.</span></span> <span data-ttu-id="af144-246">Kattintson a `next` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="af144-246">Click `next` toocontinue.</span></span>

9. <span data-ttu-id="af144-247">A hello **létrehozása** lapon, módosítsa a készlet Directory hello túl`/u01/app/grid/oraInventory`.</span><span class="sxs-lookup"><span data-stu-id="af144-247">On hello **Create Inventory** page, change hello Inventory Directory too`/u01/app/grid/oraInventory`.</span></span> <span data-ttu-id="af144-248">Kattintson a `next` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="af144-248">Click `next` toocontinue.</span></span>

   ![Képernyőfelvétel a hello telepítő létrehozása lap](./media/oracle-asm/install08.png)

10. <span data-ttu-id="af144-250">A hello **legfelső szintű parancsfájl végrehajtásának konfigurációs** lapra, jelölje be hello **automatikusan a parancsfájlokat futtasson** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="af144-250">On hello **Root script execution configuration** page, select hello **Automatically run configuration scripts** check box.</span></span> <span data-ttu-id="af144-251">Ezután válassza ki a hello **"Gyökér" felhasználó hitelesítő adatainak használata** lehetőséget, és adja meg a hello gyökér szintű felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="af144-251">Then, select hello **Use "root" user credential** option, and enter hello root user password.</span></span>

    ![Hello telepítő legfelső szintű parancsfájl végrehajtásának konfigurációs oldalát bemutató képernyőkép](./media/oracle-asm/install09.png)

11. <span data-ttu-id="af144-253">A hello **előfeltételek ellenőrzésének végre** lapon hello aktuális beállítása sikertelen lesz, és hibákat.</span><span class="sxs-lookup"><span data-stu-id="af144-253">On hello **Perform Prerequisite Checks** page, hello current setup will fail with errors.</span></span> <span data-ttu-id="af144-254">Ez az várt viselkedése.</span><span class="sxs-lookup"><span data-stu-id="af144-254">This is an expected behavior.</span></span> <span data-ttu-id="af144-255">Válassza a(z) `Fix & Check Again` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="af144-255">Select `Fix & Check Again`.</span></span>

12. <span data-ttu-id="af144-256">A hello **javítás parancsfájl** párbeszédpanel, kattintson a `OK`.</span><span class="sxs-lookup"><span data-stu-id="af144-256">In hello **Fixup Script** dialog box, click `OK`.</span></span>

13. <span data-ttu-id="af144-257">A hello **összegzés** lapon tekintse át a beállításokat, és kattintson a `Install`.</span><span class="sxs-lookup"><span data-stu-id="af144-257">On hello **Summary** page, review your selected settings, and then click `Install`.</span></span>

    ![Képernyőfelvétel a hello telepítő összegző lapja](./media/oracle-asm/install12.png)

14. <span data-ttu-id="af144-259">Egy figyelmeztető párbeszédpanel figyelmezteti konfigurációs parancsfájlokat kell toobe futtatnia szintű jogosultságokkal rendelkező felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="af144-259">A warning dialog box appears informing you configuration scripts need toobe run as a privileged user.</span></span> <span data-ttu-id="af144-260">Kattintson a `Yes` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="af144-260">Click `Yes` toocontinue.</span></span>

15. <span data-ttu-id="af144-261">A hello **Befejezés** kattintson `Close` toofinish hello telepítését.</span><span class="sxs-lookup"><span data-stu-id="af144-261">On hello **Finish** page, click `Close` toofinish hello installation.</span></span>

## <a name="set-up-your-oracle-asm-installation"></a><span data-ttu-id="af144-262">Állítsa be az Oracle ASM telepítése</span><span class="sxs-lookup"><span data-stu-id="af144-262">Set up your Oracle ASM installation</span></span>

<span data-ttu-id="af144-263">az Oracle ASM telepítése, a következő lépéseket teljes hello mentése tooset:</span><span class="sxs-lookup"><span data-stu-id="af144-263">tooset up your Oracle ASM installation, complete hello following steps:</span></span>

1. <span data-ttu-id="af144-264">Győződjön meg arról, hogy továbbra is felhasználóként van bejelentkezve **rács**, a X11 a munkamenet.</span><span class="sxs-lookup"><span data-stu-id="af144-264">Ensure you are still signed in as **grid**, from your X11 session.</span></span> <span data-ttu-id="af144-265">Előfordulhat, hogy toohit `enter` toorevive hello terminál.</span><span class="sxs-lookup"><span data-stu-id="af144-265">You might need toohit `enter` toorevive hello terminal.</span></span> <span data-ttu-id="af144-266">Majd indítsa el az Oracle automatikus tárolási felügyeleti konfiguráció Segéd hello:</span><span class="sxs-lookup"><span data-stu-id="af144-266">Then launch hello Oracle Automated Storage Management Configuration Assistant:</span></span>

   ```bash
   cd /u01/app/grid/product/12.1.0/grid/bin
   ./asmca
   ```

   <span data-ttu-id="af144-267">Oracle címterület-kezelési konfigurációs Segéd nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="af144-267">Oracle ASM Configuration Assistant opens.</span></span>

2. <span data-ttu-id="af144-268">A hello **a címterület-kezelés konfigurálása: lemez csoportok** párbeszédpanelen kattintson hello `Create` gombra, majd `Show Advanced Options`.</span><span class="sxs-lookup"><span data-stu-id="af144-268">In hello **Configure ASM: Disk Groups** dialog box, click hello `Create` button, and then click `Show Advanced Options`.</span></span>

3. <span data-ttu-id="af144-269">A hello **lemezcsoport létrehozása** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="af144-269">In hello **Create Disk Group** dialog box:</span></span>

   - <span data-ttu-id="af144-270">Adja meg a csoport Lemeznév hello **adatok**.</span><span class="sxs-lookup"><span data-stu-id="af144-270">Enter hello disk group name **DATA**.</span></span>
   - <span data-ttu-id="af144-271">A **tag lemez kiválasztása**, jelölje be **ORCL_DATA** és **ORCL_DATA1**.</span><span class="sxs-lookup"><span data-stu-id="af144-271">Under **Select Member Disks**, select **ORCL_DATA** and **ORCL_DATA1**.</span></span>
   - <span data-ttu-id="af144-272">A **foglalásiegység-méret**, jelölje be **4**.</span><span class="sxs-lookup"><span data-stu-id="af144-272">Under **Allocation Unit Size**, select **4**.</span></span>
   - <span data-ttu-id="af144-273">Kattintson a `ok` toocreate hello lemezcsoport.</span><span class="sxs-lookup"><span data-stu-id="af144-273">Click `ok` toocreate hello disk group.</span></span>
   - <span data-ttu-id="af144-274">Kattintson a `ok` tooclose hello ablak.</span><span class="sxs-lookup"><span data-stu-id="af144-274">Click `ok` tooclose hello confirmation window.</span></span>

   ![Képernyőfelvétel a hello lemez csoport létrehozása párbeszédpanel](./media/oracle-asm/asm02.png)

4. <span data-ttu-id="af144-276">A hello **a címterület-kezelés konfigurálása: lemez csoportok** párbeszédpanelen kattintson hello `Create` gombra, majd `Show Advanced Options`.</span><span class="sxs-lookup"><span data-stu-id="af144-276">In hello **Configure ASM: Disk Groups** dialog box, click hello `Create` button, and then click `Show Advanced Options`.</span></span>

5. <span data-ttu-id="af144-277">A hello **lemezcsoport létrehozása** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="af144-277">In hello **Create Disk Group** dialog box:</span></span>

   - <span data-ttu-id="af144-278">Adja meg a csoport Lemeznév hello **FRA**.</span><span class="sxs-lookup"><span data-stu-id="af144-278">Enter hello disk group name **FRA**.</span></span>
   - <span data-ttu-id="af144-279">A **redundancia**, jelölje be **(nincs) külső**.</span><span class="sxs-lookup"><span data-stu-id="af144-279">Under **Redundancy**, select **External (none)**.</span></span>
   - <span data-ttu-id="af144-280">A **tag lemez kiválasztása**, jelölje be **ORCL_FRA**.</span><span class="sxs-lookup"><span data-stu-id="af144-280">Under **Select Member Disks**, select **ORCL_FRA**.</span></span>
   - <span data-ttu-id="af144-281">A **foglalásiegység-méret**, jelölje be **4**.</span><span class="sxs-lookup"><span data-stu-id="af144-281">Under **Allocation Unit Size**, select **4**.</span></span>
   - <span data-ttu-id="af144-282">Kattintson a `ok` toocreate hello lemezcsoport.</span><span class="sxs-lookup"><span data-stu-id="af144-282">Click `ok` toocreate hello disk group.</span></span>
   - <span data-ttu-id="af144-283">Kattintson a `ok` tooclose hello ablak.</span><span class="sxs-lookup"><span data-stu-id="af144-283">Click `ok` tooclose hello confirmation window.</span></span>

   ![Képernyőfelvétel a hello lemez csoport létrehozása párbeszédpanel](./media/oracle-asm/asm04.png)

6. <span data-ttu-id="af144-285">Válassza ki **kilépési** tooclose címterület-kezelési konfigurációs Segéd.</span><span class="sxs-lookup"><span data-stu-id="af144-285">Select **Exit** tooclose ASM Configuration Assistant.</span></span>

   ![Képernyőfelvétel a hello konfigurálása ASM: lemez csoportok párbeszédpanel a Kilépés gombra](./media/oracle-asm/asm05.png)

## <a name="create-hello-database"></a><span data-ttu-id="af144-287">Hello adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="af144-287">Create hello database</span></span>

<span data-ttu-id="af144-288">hello Azure Piactéri lemezképhez hello Oracle adatbázis-szoftver már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="af144-288">hello Oracle database software is already installed on hello Azure Marketplace image.</span></span> <span data-ttu-id="af144-289">toocreate egy adatbázis teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="af144-289">toocreate a database, complete hello following steps:</span></span>

1. <span data-ttu-id="af144-290">Felhasználók toohello Oracle felügyelő váltson, és majd a naplózás hello figyelő inicializálása:</span><span class="sxs-lookup"><span data-stu-id="af144-290">Switch users toohello Oracle superuser, and then initialize hello listener for logging:</span></span>

   ```bash
   su - oracle
   cd /u01/app/oracle/product/12.1.0/dbhome_1/bin
   ./dbca
   ```
   <span data-ttu-id="af144-291">Adatbázis-konfigurációs Segéd nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="af144-291">Database Configuration Assistant opens.</span></span>

2. <span data-ttu-id="af144-292">A hello **adatbázis-művelet** kattintson `Create Database`.</span><span class="sxs-lookup"><span data-stu-id="af144-292">On hello **Database Operation** page, click `Create Database`.</span></span>

3. <span data-ttu-id="af144-293">A hello **létrehozási mód** lap:</span><span class="sxs-lookup"><span data-stu-id="af144-293">On hello **Creation Mode** page:</span></span>

   - <span data-ttu-id="af144-294">Adja meg a hello adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="af144-294">Enter a name for hello database.</span></span>
   - <span data-ttu-id="af144-295">A **tárolótípus**, győződjön meg arról **automatikus tárhely-kezelési (ASM)** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="af144-295">For **Storage Type**, ensure **Automatic Storage Management (ASM)** is selected.</span></span>
   - <span data-ttu-id="af144-296">A **adatbázis helye**, hello alapértelmezett ASM hely javasolt.</span><span class="sxs-lookup"><span data-stu-id="af144-296">For **Database Files Location**, use hello default ASM suggested location.</span></span>
   - <span data-ttu-id="af144-297">A **gyors helyreállítás terület**, hello alapértelmezett ASM hely javasolt.</span><span class="sxs-lookup"><span data-stu-id="af144-297">For **Fast Recovery Area**, use hello default ASM suggested location.</span></span>
   - <span data-ttu-id="af144-298">Adja meg egy **rendszergazdai jelszó** és **jelszó megerősítése**.</span><span class="sxs-lookup"><span data-stu-id="af144-298">type in an **Administrative Password** and **confirm password**.</span></span>
   - <span data-ttu-id="af144-299">Győződjön meg arról `create as container database` van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="af144-299">ensure `create as container database` is selected.</span></span>
   - <span data-ttu-id="af144-300">Írja be a `pluggable database name` érték.</span><span class="sxs-lookup"><span data-stu-id="af144-300">type in a `pluggable database name` value.</span></span>

4. <span data-ttu-id="af144-301">A hello **összegzés** lapon tekintse át a beállításokat, és kattintson a `Finish` toocreate hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="af144-301">On hello **Summary** page, review your selected settings, and then click `Finish` toocreate hello database.</span></span>

   ![Képernyőfelvétel a hello összegző lapja](./media/oracle-asm/createdb03.png)

5. <span data-ttu-id="af144-303">hello adatbázis létrehozását.</span><span class="sxs-lookup"><span data-stu-id="af144-303">hello Database has been created.</span></span> <span data-ttu-id="af144-304">A hello **Befejezés** oldalon lehetősége van a hello lehetőséget toounlock további fiókokat toouse ezt az adatbázist, és módosíthatja a hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="af144-304">On hello **Finish** page, you have hello option toounlock additional accounts toouse this database and change hello passwords.</span></span> <span data-ttu-id="af144-305">Ha a toodo tette, válassza ki a **jelszókezelés** – ellenkező esetben kattintson a `close`.</span><span class="sxs-lookup"><span data-stu-id="af144-305">If you wish toodo so, select **Password Management** - otherwise click on `close`.</span></span>

## <a name="delete-hello-vm"></a><span data-ttu-id="af144-306">Hello virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="af144-306">Delete hello VM</span></span>

<span data-ttu-id="af144-307">Sikeresen konfigurálta hello Oracle DB lemezkép hello Azure piactér Oracle automatikus tárolók kezelése.</span><span class="sxs-lookup"><span data-stu-id="af144-307">You have successfully configured Oracle Automated Storage Management on hello Oracle DB image from hello Azure Marketplace.</span></span>  <span data-ttu-id="af144-308">Ha már nincs szüksége a virtuális gép, használhatja a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="af144-308">When you no longer need this VM, you can use hello following command tooremove hello resource group, VM, and all related resources:</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="af144-309">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="af144-309">Next steps</span></span>

[<span data-ttu-id="af144-310">Oktatóanyag: Oracle DataGuard konfigurálása</span><span class="sxs-lookup"><span data-stu-id="af144-310">Tutorial: Configure Oracle DataGuard</span></span>](configure-oracle-dataguard.md)

[<span data-ttu-id="af144-311">Oktatóanyag: Oracle GoldenGate konfigurálása</span><span class="sxs-lookup"><span data-stu-id="af144-311">Tutorial: Configure Oracle GoldenGate</span></span>](Configure-oracle-golden-gate.md)

<span data-ttu-id="af144-312">Tekintse át [az Oracle DB tervezővel](oracle-design.md)</span><span class="sxs-lookup"><span data-stu-id="af144-312">Review [Architect an Oracle DB](oracle-design.md)</span></span>
