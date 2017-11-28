---
title: "Linux virtuális gép jelszavát aaaReset és SSH kulcsot hello CLI |} Microsoft Docs"
description: "Hogyan toouse hello VMAccess bővítmény hello Azure parancssori felület (CLI) tooreset Linux virtuális gép jelszó vagy SSH-kulcsban, javítsa ki a hello SSH-konfigurációt, és lemez konzisztenciájának ellenőrzése"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: d975eb70-5ff1-40d1-a634-8dd2646dcd17
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: cynthn
ms.openlocfilehash: 1650ad64fb982627ae9f90b1a8209bb56bac7004
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-a-linux-vm-password-or-ssh-key-fix-hello-ssh-configuration-and-check-disk-consistency-using-hello-vmaccess-extension"></a><span data-ttu-id="a0949-103">Hogyan tooreset Linux virtuális gép jelszó vagy SSH-kulcsban, javítsa ki a hello SSH-konfigurációt, és ellenőrizze a lemez konzisztencia hello VMAccess bővítmény használatával</span><span class="sxs-lookup"><span data-stu-id="a0949-103">How tooreset a Linux VM password or SSH key, fix hello SSH configuration, and check disk consistency using hello VMAccess extension</span></span>
<span data-ttu-id="a0949-104">Ha tooa Linux virtuális gépet az Azure elfelejtett jelszó, egy Secure Shell (SSH) helytelen kulcsot vagy hello SSH konfigurációs probléma miatt nem tud csatlakozni, használja a VMAccessForLinux bővítmény hello hello Azure CLI tooreset hello jelszó vagy SSH-kulcs, javítsa ki hello SSH-konfigurációt, és ellenőrizze a lemez konzisztencia.</span><span class="sxs-lookup"><span data-stu-id="a0949-104">If you can't connect tooa Linux virtual machine on Azure because of a forgotten password, an incorrect Secure Shell (SSH) key, or a problem with hello SSH configuration, use hello VMAccessForLinux extension with hello Azure CLI tooreset hello password or SSH key, fix hello SSH configuration, and check disk consistency.</span></span> 

> [!IMPORTANT] 
> <span data-ttu-id="a0949-105">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a0949-105">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="a0949-106">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="a0949-106">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="a0949-107">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="a0949-107">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="a0949-108">Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span><span class="sxs-lookup"><span data-stu-id="a0949-108">Learn how too[perform these steps using hello Resource Manager model](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span></span>

<span data-ttu-id="a0949-109">Hello Azure CLI-t, a hello használhatja **azure virtuálisgép-bővítmény készlet** parancsot a parancssori felület (Bash, terminál, parancssor) tooaccess parancsok.</span><span class="sxs-lookup"><span data-stu-id="a0949-109">With hello Azure CLI, you use hello **azure vm extension set** command from your command-line interface (Bash, Terminal, Command prompt) tooaccess commands.</span></span> <span data-ttu-id="a0949-110">Futtatás **azure súgó virtuálisgép-bővítmény készlet** részletes bővítmény használatra.</span><span class="sxs-lookup"><span data-stu-id="a0949-110">Run **azure help vm extension set** for detailed extension usage.</span></span>

<span data-ttu-id="a0949-111">Hello Azure CLI-t, az alábbi feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="a0949-111">With hello Azure CLI, you can do hello following tasks:</span></span>

* [<span data-ttu-id="a0949-112">Hello jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="a0949-112">Reset hello password</span></span>](#pwresetcli)
* [<span data-ttu-id="a0949-113">Hello SSH-kulcs visszaállítása</span><span class="sxs-lookup"><span data-stu-id="a0949-113">Reset hello SSH key</span></span>](#sshkeyresetcli)
* [<span data-ttu-id="a0949-114">Hello jelszó és hello SSH-kulcs visszaállítása</span><span class="sxs-lookup"><span data-stu-id="a0949-114">Reset hello password and hello SSH key</span></span>](#resetbothcli)
* [<span data-ttu-id="a0949-115">Sudo új felhasználói fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="a0949-115">Create a new sudo user account</span></span>](#createnewsudocli)
* [<span data-ttu-id="a0949-116">Hello SSH-konfigurációjának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="a0949-116">Reset hello SSH configuration</span></span>](#sshconfigresetcli)
* [<span data-ttu-id="a0949-117">Felhasználó törlése</span><span class="sxs-lookup"><span data-stu-id="a0949-117">Delete a user</span></span>](#deletecli)
* [<span data-ttu-id="a0949-118">Hello VMAccess bővítmény hello állapotának megjelenítése</span><span class="sxs-lookup"><span data-stu-id="a0949-118">Display hello status of hello VMAccess extension</span></span>](#statuscli)
* [<span data-ttu-id="a0949-119">Felvett lemezek egységességének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a0949-119">Check consistency of added disks</span></span>](#checkdisk)
* [<span data-ttu-id="a0949-120">A Linux virtuális gépén felvett lemezek javítása</span><span class="sxs-lookup"><span data-stu-id="a0949-120">Repair added disks on your Linux VM</span></span>](#repairdisk)

## <a name="prerequisites"></a><span data-ttu-id="a0949-121">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a0949-121">Prerequisites</span></span>
<span data-ttu-id="a0949-122">Toodo hello alábbi lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="a0949-122">You will need toodo hello following:</span></span>

* <span data-ttu-id="a0949-123">Túl kell[hello Azure parancssori felület telepítése](../../../cli-install-nodejs.md) és [tooyour előfizetés csatlakozás](../../../xplat-cli-connect.md) toouse Azure-fiókjához tartozó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="a0949-123">You will need too[install hello Azure CLI](../../../cli-install-nodejs.md) and [connect tooyour subscription](../../../xplat-cli-connect.md) toouse Azure resources associated with your account.</span></span>
* <span data-ttu-id="a0949-124">Állítsa be a megfelelő módba hello hello klasszikus üzembe helyezési modellel hello következő hello parancs parancssorba beírásával:</span><span class="sxs-lookup"><span data-stu-id="a0949-124">Set hello correct mode for hello classic deployment model by typing hello following at hello command prompt:</span></span>
    ``` 
        azure config mode asm
    ```
* <span data-ttu-id="a0949-125">Rendelkezik egy új jelszó vagy SSH-kulcsok, ha azt szeretné, hogy egy tooreset.</span><span class="sxs-lookup"><span data-stu-id="a0949-125">Have a new password or set of SSH keys, if you want tooreset either one.</span></span> <span data-ttu-id="a0949-126">Ezek nem szükséges, ha azt szeretné, hogy tooreset hello SSH-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="a0949-126">You don't need these if you want tooreset hello SSH configuration.</span></span>

## <span data-ttu-id="a0949-127"><a name="pwresetcli"></a>Hello jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="a0949-127"><a name="pwresetcli"></a>Reset hello password</span></span>
1. <span data-ttu-id="a0949-128">A helyi számítógépen, ezek a sorok PrivateConf.json nevű fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a0949-128">Create a file on your local computer named PrivateConf.json with these lines.</span></span> <span data-ttu-id="a0949-129">Cserélje le **sajátfelhasználónév** és  **myP@ssW0rd**  a saját felhasználói névvel és jelszóval és saját lejárati dátumának beállítása.</span><span class="sxs-lookup"><span data-stu-id="a0949-129">Replace **myUserName** and **myP@ssW0rd** with your own user name and password and set your own date for expiration.</span></span>

    ```   
        {
        "username":"myUserName",
        "password":"myP@ssW0rd",
        "expiration":"2020-01-01"
        }
    ```
        
2. <span data-ttu-id="a0949-130">Futtassa a parancsot, és a virtuális gép neve hello **myVM**.</span><span class="sxs-lookup"><span data-stu-id="a0949-130">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json
    ```

## <span data-ttu-id="a0949-131"><a name="sshkeyresetcli"></a>Hello SSH-kulcs visszaállítása</span><span class="sxs-lookup"><span data-stu-id="a0949-131"><a name="sshkeyresetcli"></a>Reset hello SSH key</span></span>
1. <span data-ttu-id="a0949-132">Ezek a tartalmak PrivateConf.json nevű fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a0949-132">Create a file named PrivateConf.json with these contents.</span></span> <span data-ttu-id="a0949-133">Cserélje le a hello **sajátfelhasználónév** és **mySSHKey** értékeket a saját adataival.</span><span class="sxs-lookup"><span data-stu-id="a0949-133">Replace hello **myUserName** and **mySSHKey** values with your own information.</span></span>

    ```   
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey"
        }
    ```
2. <span data-ttu-id="a0949-134">Futtassa a parancsot, és a virtuális gép neve hello **myVM**.</span><span class="sxs-lookup"><span data-stu-id="a0949-134">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span>
   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <span data-ttu-id="a0949-135"><a name="resetbothcli"></a>Hello jelszó és a hello SSH-kulcs visszaállítása</span><span class="sxs-lookup"><span data-stu-id="a0949-135"><a name="resetbothcli"></a>Reset both hello password and hello SSH key</span></span>
1. <span data-ttu-id="a0949-136">Ezek a tartalmak PrivateConf.json nevű fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a0949-136">Create a file named PrivateConf.json with these contents.</span></span> <span data-ttu-id="a0949-137">Cserélje le a hello **sajátfelhasználónév**, **mySSHKey** és  **myP@ssW0rd**  értékeket a saját adataival.</span><span class="sxs-lookup"><span data-stu-id="a0949-137">Replace hello **myUserName**, **mySSHKey** and **myP@ssW0rd** values with your own information.</span></span>

    ``` 
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey",
        "password":"myP@ssW0rd"
        }
    ```

2. <span data-ttu-id="a0949-138">Futtassa a parancsot, és a virtuális gép neve hello **myVM**.</span><span class="sxs-lookup"><span data-stu-id="a0949-138">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set MyVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <span data-ttu-id="a0949-139"><a name="createnewsudocli"></a>Sudo új felhasználói fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="a0949-139"><a name="createnewsudocli"></a>Create a new sudo user account</span></span>

<span data-ttu-id="a0949-140">Ha elfelejti a felhasználónevét, használhat egy új hello sudo hitelesítésszolgáltatóval VMAccess toocreate.</span><span class="sxs-lookup"><span data-stu-id="a0949-140">If you forget your user name, you can use VMAccess toocreate a new one with hello sudo authority.</span></span> <span data-ttu-id="a0949-141">Ebben az esetben hello meglévő felhasználónév és jelszó nem lehet módosítani.</span><span class="sxs-lookup"><span data-stu-id="a0949-141">In this case, hello existing user name and password will not be modified.</span></span>

<span data-ttu-id="a0949-142">egy új felhasználót sudo jelszó hozzáférés, hello parancsfájl használata a toocreate [jelszó-átállítási hello](#pwresetcli) , és adja meg a hello új felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="a0949-142">toocreate a new sudo user with password access, use hello script in [Reset hello password](#pwresetcli) and specify hello new user name.</span></span>

<span data-ttu-id="a0949-143">egy új felhasználót sudo SSH hozzáférés a kulcshoz, hello parancsfájl használata a toocreate [alaphelyzetbe állítása hello SSH-kulcs](#sshkeyresetcli) és adja meg a hello új felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="a0949-143">toocreate a new sudo user with SSH key access, use hello script in [Reset hello SSH key](#sshkeyresetcli) and specify hello new user name.</span></span>

<span data-ttu-id="a0949-144">Is [hello jelszó és hello SSH-kulcs visszaállítása](#resetbothcli) toocreate jelszó és a SSH-kulcs hozzáférési egy új felhasználót.</span><span class="sxs-lookup"><span data-stu-id="a0949-144">You can also use [Reset hello password and hello SSH key](#resetbothcli) toocreate a new user with both password and SSH key access.</span></span>

## <span data-ttu-id="a0949-145"><a name="sshconfigresetcli"></a>Hello SSH-konfigurációjának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="a0949-145"><a name="sshconfigresetcli"></a>Reset hello SSH configuration</span></span>
<span data-ttu-id="a0949-146">Hello SSH-konfigurációt nem várt állapotban van, ha hozzáférést toohello VM is elveszhetnek.</span><span class="sxs-lookup"><span data-stu-id="a0949-146">If hello SSH configuration is in an undesired state, you might also lose access toohello VM.</span></span> <span data-ttu-id="a0949-147">Hello VMAccess bővítmény tooreset hello konfiguráció tooits alapértelmezett állapota is használhatja.</span><span class="sxs-lookup"><span data-stu-id="a0949-147">You can use hello VMAccess extension tooreset hello configuration tooits default state.</span></span> <span data-ttu-id="a0949-148">toodo így egyszerűen tooset hello "reset_ssh" kulcs túl "igaz".</span><span class="sxs-lookup"><span data-stu-id="a0949-148">toodo so, you just need tooset hello “reset_ssh” key too“True”.</span></span> <span data-ttu-id="a0949-149">hello bővítmény indítsa újra a hello SSH-kiszolgálót, nyissa meg a hello SSH-port a virtuális gépen, és alaphelyzetbe hello SSH konfigurációs toodefault értékeket.</span><span class="sxs-lookup"><span data-stu-id="a0949-149">hello extension will restart hello SSH server, open hello SSH port on your VM, and reset hello SSH configuration toodefault values.</span></span> <span data-ttu-id="a0949-150">hello felhasználói fiókot (név, jelszó vagy SSH-kulcsok) nem lehet módosítani.</span><span class="sxs-lookup"><span data-stu-id="a0949-150">hello user account (name, password or SSH keys) will not be changed.</span></span>

> [!NOTE]
> <span data-ttu-id="a0949-151">hello SSH konfigurációs fájl, amely lekérdezi alaphelyzetbe /etc/ssh/sshd_config helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="a0949-151">hello SSH configuration file that gets reset is located at /etc/ssh/sshd_config.</span></span>
> 
> 

1. <span data-ttu-id="a0949-152">Hozzon létre egy fájlt PrivateConf.json ehhez a tartalomhoz.</span><span class="sxs-lookup"><span data-stu-id="a0949-152">Create a file named PrivateConf.json with this content.</span></span>

    ```   
        {
        "reset_ssh":"True"
        }
    ```

2. <span data-ttu-id="a0949-153">Futtassa a parancsot, és a virtuális gép neve hello **myVM**.</span><span class="sxs-lookup"><span data-stu-id="a0949-153">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span> 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <span data-ttu-id="a0949-154"><a name="deletecli"></a>Felhasználó törlése</span><span class="sxs-lookup"><span data-stu-id="a0949-154"><a name="deletecli"></a>Delete a user</span></span>
<span data-ttu-id="a0949-155">Ha azt szeretné, hogy egy felhasználói fiókot, jelentkezzen be a virtuális gép toohello közvetlenül nélkül toodelete, használhatja ezt a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="a0949-155">If you want toodelete a user account without logging into toohello VM directly, you can use this script.</span></span>

1. <span data-ttu-id="a0949-156">Hozzon létre egy fájlt PrivateConf.json ehhez a tartalomhoz, és hello felhasználói név tooremove a **removeUserName**.</span><span class="sxs-lookup"><span data-stu-id="a0949-156">Create a file named PrivateConf.json with this content, substituting hello user name tooremove for **removeUserName**.</span></span> 

    ```   
        {
        "remove_user":"removeUserName"
        }
    ```

2. <span data-ttu-id="a0949-157">Futtassa a parancsot, és a virtuális gép neve hello **myVM**.</span><span class="sxs-lookup"><span data-stu-id="a0949-157">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span> 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <span data-ttu-id="a0949-158"><a name="statuscli"></a>Hello VMAccess bővítmény hello állapotának megjelenítése</span><span class="sxs-lookup"><span data-stu-id="a0949-158"><a name="statuscli"></a>Display hello status of hello VMAccess extension</span></span>
<span data-ttu-id="a0949-159">hello VMAccess bővítmény állapotának toodisplay hello futtassa ezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="a0949-159">toodisplay hello status of hello VMAccess extension, run this command.</span></span>

```
        azure vm extension get
```

## <span data-ttu-id="a0949-160"><a name='checkdisk'></a>Felvett lemezek egységességének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a0949-160"><a name='checkdisk'></a>Check consistency of added disks</span></span>
<span data-ttu-id="a0949-161">a Linux virtuális gép összes lemezen fsck toorun, szüksége lesz a toodo hello következő:</span><span class="sxs-lookup"><span data-stu-id="a0949-161">toorun fsck on all disks in your Linux virtual machine, you will need toodo hello following:</span></span>

1. <span data-ttu-id="a0949-162">Hozzon létre egy fájlt PublicConf.json ehhez a tartalomhoz.</span><span class="sxs-lookup"><span data-stu-id="a0949-162">Create a file named PublicConf.json with this content.</span></span> <span data-ttu-id="a0949-163">Ellenőrizze a lemez e toocheck csatlakoztatva lemezek tooyour virtuális gépet, vagy nem olyan logikai érték szükséges.</span><span class="sxs-lookup"><span data-stu-id="a0949-163">Check Disk takes a boolean for whether toocheck disks attached tooyour virtual machine or not.</span></span> 

    ```   
        {   
        "check_disk": "true"
        }
    ```

2. <span data-ttu-id="a0949-164">Futtassa a parancs tooexecute, és a virtuális gép neve hello **myVM**.</span><span class="sxs-lookup"><span data-stu-id="a0949-164">Run this command tooexecute, substituting hello name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 
    ```

## <span data-ttu-id="a0949-165"><a name='repairdisk'></a>Lemezek javítása</span><span class="sxs-lookup"><span data-stu-id="a0949-165"><a name='repairdisk'></a>Repair disks</span></span>
<span data-ttu-id="a0949-166">toorepair lemezek csatlakoztatása nem vagy csatlakoztatási konfigurációs hibákat tartalmaz, a Linux virtuális gép hello VMAccess bővítmény tooreset hello csatlakoztatási konfigurációt használja.</span><span class="sxs-lookup"><span data-stu-id="a0949-166">toorepair disks that are not mounting or have mount configuration errors, use hello VMAccess extension tooreset hello mount configuration on your Linux virtual machine.</span></span> <span data-ttu-id="a0949-167">Hello nevét és a lemez **myDisk**.</span><span class="sxs-lookup"><span data-stu-id="a0949-167">Substituting hello name of your disk for **myDisk**.</span></span>

1. <span data-ttu-id="a0949-168">Hozzon létre egy fájlt PublicConf.json ehhez a tartalomhoz.</span><span class="sxs-lookup"><span data-stu-id="a0949-168">Create a file named PublicConf.json with this content.</span></span> 

    ```   
        {
        "repair_disk":"true",
        "disk_name":"myDisk"
        }
    ```

2. <span data-ttu-id="a0949-169">Futtassa a parancs tooexecute, és a virtuális gép neve hello **myVM**.</span><span class="sxs-lookup"><span data-stu-id="a0949-169">Run this command tooexecute, substituting hello name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json
    ```

## <a name="next-steps"></a><span data-ttu-id="a0949-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0949-170">Next steps</span></span>
* <span data-ttu-id="a0949-171">Ha toouse Azure PowerShell-parancsmagok vagy Azure Resource Manager sablonok tooreset hello jelszó vagy SSH-kulcsot, hello SSH-konfigurációt, hárítsa és lemez konzisztenciájának ellenőrzése, lásd: hello [VMAccess bővítmény dokumentációnkat a Githubon](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span><span class="sxs-lookup"><span data-stu-id="a0949-171">If you want toouse Azure PowerShell cmdlets or Azure Resource Manager templates tooreset hello password or SSH key, fix hello SSH configuration, and check disk consistency, see hello [VMAccess extension documentation on GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span></span> 
* <span data-ttu-id="a0949-172">Is használhatja a hello [Azure-portálon](https://portal.azure.com) tooreset hello jelszó vagy SSH-kulcsot a Linux virtuális gépek telepített hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="a0949-172">You can also use hello [Azure portal](https://portal.azure.com) tooreset hello password or SSH key of a Linux VM deployed in hello classic deployment model.</span></span> <span data-ttu-id="a0949-173">A Linux virtuális gép hello Resource Manager üzembe helyezési modellben telepített hello portál do toothis jelenleg nem használható.</span><span class="sxs-lookup"><span data-stu-id="a0949-173">You can't currently use hello portal do toothis for a Linux VM deployed in hello Resource Manager deployment model.</span></span>
* <span data-ttu-id="a0949-174">Lásd: [virtuálisgép-bővítmények és szolgáltatásokkal kapcsolatban](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) több Virtuálisgép-bővítmények az Azure virtuális gépeken való használatáról.</span><span class="sxs-lookup"><span data-stu-id="a0949-174">See [About virtual machine extensions and features](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more about using VM extensions for Azure virtual machines.</span></span>

