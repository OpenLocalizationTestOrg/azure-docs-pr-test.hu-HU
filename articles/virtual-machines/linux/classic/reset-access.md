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
# <a name="how-tooreset-a-linux-vm-password-or-ssh-key-fix-hello-ssh-configuration-and-check-disk-consistency-using-hello-vmaccess-extension"></a>Hogyan tooreset Linux virtuális gép jelszó vagy SSH-kulcsban, javítsa ki a hello SSH-konfigurációt, és ellenőrizze a lemez konzisztencia hello VMAccess bővítmény használatával
Ha tooa Linux virtuális gépet az Azure elfelejtett jelszó, egy Secure Shell (SSH) helytelen kulcsot vagy hello SSH konfigurációs probléma miatt nem tud csatlakozni, használja a VMAccessForLinux bővítmény hello hello Azure CLI tooreset hello jelszó vagy SSH-kulcs, javítsa ki hello SSH-konfigurációt, és ellenőrizze a lemez konzisztencia. 

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

Hello Azure CLI-t, a hello használhatja **azure virtuálisgép-bővítmény készlet** parancsot a parancssori felület (Bash, terminál, parancssor) tooaccess parancsok. Futtatás **azure súgó virtuálisgép-bővítmény készlet** részletes bővítmény használatra.

Hello Azure CLI-t, az alábbi feladatok hello:

* [Hello jelszó alaphelyzetbe állítása](#pwresetcli)
* [Hello SSH-kulcs visszaállítása](#sshkeyresetcli)
* [Hello jelszó és hello SSH-kulcs visszaállítása](#resetbothcli)
* [Sudo új felhasználói fiók létrehozása](#createnewsudocli)
* [Hello SSH-konfigurációjának visszaállítása](#sshconfigresetcli)
* [Felhasználó törlése](#deletecli)
* [Hello VMAccess bővítmény hello állapotának megjelenítése](#statuscli)
* [Felvett lemezek egységességének ellenőrzése](#checkdisk)
* [A Linux virtuális gépén felvett lemezek javítása](#repairdisk)

## <a name="prerequisites"></a>Előfeltételek
Toodo hello alábbi lesz szüksége:

* Túl kell[hello Azure parancssori felület telepítése](../../../cli-install-nodejs.md) és [tooyour előfizetés csatlakozás](../../../xplat-cli-connect.md) toouse Azure-fiókjához tartozó erőforrásokat.
* Állítsa be a megfelelő módba hello hello klasszikus üzembe helyezési modellel hello következő hello parancs parancssorba beírásával:
    ``` 
        azure config mode asm
    ```
* Rendelkezik egy új jelszó vagy SSH-kulcsok, ha azt szeretné, hogy egy tooreset. Ezek nem szükséges, ha azt szeretné, hogy tooreset hello SSH-konfigurációt.

## <a name="pwresetcli"></a>Hello jelszó alaphelyzetbe állítása
1. A helyi számítógépen, ezek a sorok PrivateConf.json nevű fájl létrehozása. Cserélje le **sajátfelhasználónév** és  **myP@ssW0rd**  a saját felhasználói névvel és jelszóval és saját lejárati dátumának beállítása.

    ```   
        {
        "username":"myUserName",
        "password":"myP@ssW0rd",
        "expiration":"2020-01-01"
        }
    ```
        
2. Futtassa a parancsot, és a virtuális gép neve hello **myVM**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json
    ```

## <a name="sshkeyresetcli"></a>Hello SSH-kulcs visszaállítása
1. Ezek a tartalmak PrivateConf.json nevű fájl létrehozása. Cserélje le a hello **sajátfelhasználónév** és **mySSHKey** értékeket a saját adataival.

    ```   
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey"
        }
    ```
2. Futtassa a parancsot, és a virtuális gép neve hello **myVM**.
   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Hello jelszó és a hello SSH-kulcs visszaállítása
1. Ezek a tartalmak PrivateConf.json nevű fájl létrehozása. Cserélje le a hello **sajátfelhasználónév**, **mySSHKey** és  **myP@ssW0rd**  értékeket a saját adataival.

    ``` 
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey",
        "password":"myP@ssW0rd"
        }
    ```

2. Futtassa a parancsot, és a virtuális gép neve hello **myVM**.

    ```   
        azure vm extension set MyVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="createnewsudocli"></a>Sudo új felhasználói fiók létrehozása

Ha elfelejti a felhasználónevét, használhat egy új hello sudo hitelesítésszolgáltatóval VMAccess toocreate. Ebben az esetben hello meglévő felhasználónév és jelszó nem lehet módosítani.

egy új felhasználót sudo jelszó hozzáférés, hello parancsfájl használata a toocreate [jelszó-átállítási hello](#pwresetcli) , és adja meg a hello új felhasználónevet.

egy új felhasználót sudo SSH hozzáférés a kulcshoz, hello parancsfájl használata a toocreate [alaphelyzetbe állítása hello SSH-kulcs](#sshkeyresetcli) és adja meg a hello új felhasználónevet.

Is [hello jelszó és hello SSH-kulcs visszaállítása](#resetbothcli) toocreate jelszó és a SSH-kulcs hozzáférési egy új felhasználót.

## <a name="sshconfigresetcli"></a>Hello SSH-konfigurációjának visszaállítása
Hello SSH-konfigurációt nem várt állapotban van, ha hozzáférést toohello VM is elveszhetnek. Hello VMAccess bővítmény tooreset hello konfiguráció tooits alapértelmezett állapota is használhatja. toodo így egyszerűen tooset hello "reset_ssh" kulcs túl "igaz". hello bővítmény indítsa újra a hello SSH-kiszolgálót, nyissa meg a hello SSH-port a virtuális gépen, és alaphelyzetbe hello SSH konfigurációs toodefault értékeket. hello felhasználói fiókot (név, jelszó vagy SSH-kulcsok) nem lehet módosítani.

> [!NOTE]
> hello SSH konfigurációs fájl, amely lekérdezi alaphelyzetbe /etc/ssh/sshd_config helyezkedik el.
> 
> 

1. Hozzon létre egy fájlt PrivateConf.json ehhez a tartalomhoz.

    ```   
        {
        "reset_ssh":"True"
        }
    ```

2. Futtassa a parancsot, és a virtuális gép neve hello **myVM**. 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="deletecli"></a>Felhasználó törlése
Ha azt szeretné, hogy egy felhasználói fiókot, jelentkezzen be a virtuális gép toohello közvetlenül nélkül toodelete, használhatja ezt a parancsfájlt.

1. Hozzon létre egy fájlt PrivateConf.json ehhez a tartalomhoz, és hello felhasználói név tooremove a **removeUserName**. 

    ```   
        {
        "remove_user":"removeUserName"
        }
    ```

2. Futtassa a parancsot, és a virtuális gép neve hello **myVM**. 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="statuscli"></a>Hello VMAccess bővítmény hello állapotának megjelenítése
hello VMAccess bővítmény állapotának toodisplay hello futtassa ezt a parancsot.

```
        azure vm extension get
```

## <a name='checkdisk'></a>Felvett lemezek egységességének ellenőrzése
a Linux virtuális gép összes lemezen fsck toorun, szüksége lesz a toodo hello következő:

1. Hozzon létre egy fájlt PublicConf.json ehhez a tartalomhoz. Ellenőrizze a lemez e toocheck csatlakoztatva lemezek tooyour virtuális gépet, vagy nem olyan logikai érték szükséges. 

    ```   
        {   
        "check_disk": "true"
        }
    ```

2. Futtassa a parancs tooexecute, és a virtuális gép neve hello **myVM**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 
    ```

## <a name='repairdisk'></a>Lemezek javítása
toorepair lemezek csatlakoztatása nem vagy csatlakoztatási konfigurációs hibákat tartalmaz, a Linux virtuális gép hello VMAccess bővítmény tooreset hello csatlakoztatási konfigurációt használja. Hello nevét és a lemez **myDisk**.

1. Hozzon létre egy fájlt PublicConf.json ehhez a tartalomhoz. 

    ```   
        {
        "repair_disk":"true",
        "disk_name":"myDisk"
        }
    ```

2. Futtassa a parancs tooexecute, és a virtuális gép neve hello **myVM**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json
    ```

## <a name="next-steps"></a>Következő lépések
* Ha toouse Azure PowerShell-parancsmagok vagy Azure Resource Manager sablonok tooreset hello jelszó vagy SSH-kulcsot, hello SSH-konfigurációt, hárítsa és lemez konzisztenciájának ellenőrzése, lásd: hello [VMAccess bővítmény dokumentációnkat a Githubon](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 
* Is használhatja a hello [Azure-portálon](https://portal.azure.com) tooreset hello jelszó vagy SSH-kulcsot a Linux virtuális gépek telepített hello klasszikus üzembe helyezési modellben. A Linux virtuális gép hello Resource Manager üzembe helyezési modellben telepített hello portál do toothis jelenleg nem használható.
* Lásd: [virtuálisgép-bővítmények és szolgáltatásokkal kapcsolatban](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) több Virtuálisgép-bővítmények az Azure virtuális gépeken való használatáról.

