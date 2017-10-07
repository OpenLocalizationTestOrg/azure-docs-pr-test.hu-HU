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
# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a>A szerepkör telepítése a Windows virtuális gépén kísérletezhet
Miután az első virtuális gép (VM) be és fut, a továbblépés tooinstalling szoftvereinket és szolgáltatásainkat. Ebben az oktatóanyagban fogjuk a hello Windows Server virtuális gép tooinstall IIS toouse Kiszolgálókezelő. Ezután létrehozunk egy hálózati biztonsági csoport (NSG) hello Azure portál tooopen port 80 tooIIS forgalom használ. 

Ha még nem hozott létre az első virtuális gép, meg kell lépjen vissza túl[az első Windows rendszerű virtuális gép létrehozása az Azure-portálon hello](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Ez az oktatóanyag folytatása előtt.

## <a name="make-sure-hello-vm-is-running"></a>Győződjön meg arról, hogy hello virtuális gép fut.
1. Nyissa meg hello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben kattintson a **virtuális gépek**. Válassza ki a virtuális gép hello hello listából.
3. Ha hello állapota **leállítva (Deallocated)**, hello kattintson **Start** hello gombjára **Essentials** hello VM panel. Ha hello állapota **futtató**, továbbléphet a következő lépésben toohello.

## <a name="connect-toohello-virtual-machine-and-sign-in"></a>Csatlakoztassa toohello virtuális gépet, és jelentkezzen be
1. Hello központ menüben kattintson a **virtuális gépek**. Válassza ki a virtuális gép hello hello listából.
2. Virtuális gép hello hello paneljén kattintson **Connect**. Ez a hoz létre, és letölti a Remote Desktop Protocol fájlt (.rdp-fájl), mint a helyi tooconnect tooyour gépek. Érdemes lehet toosave hello fájl tooyour asztali egyszerűen hozzáférhetnek. **Nyissa meg** a fájl tooconnect tooyour virtuális gép.
   
    ![Képernyőfelvétel a hello Azure portál ábrázoló hogyan tooconnect tooyour méretű VM](./media/hero-role/connect.png)
3. Megjelenik egy figyelmeztetés, hogy hello RDP-fájl közzétevője ismeretlen. Ez nem jelent problémát. Hello távoli asztal ablakában kattintson **Connect** toocontinue.
   
    ![Képernyőkép az ismeretlen közzétevőre vonatkozó figyelmeztetésről](./media/hero-role/rdp-warn.png)
4. Hello Windows rendszerbiztonsági ablakban, a típus hello felhasználónév és jelszó létrehozásakor létrehozott helyi fiók hello hello virtuális gép. hello felhasználónév van megadva: *vmname*&#92; *felhasználónév*, majd kattintson a **OK**.
   
    ![Képernyőfelvétel a hello virtuális gép nevét, a felhasználónév és jelszó megadása](./media/hero-role/credentials.png)
5. Megjelenik egy figyelmeztetés hello tanúsítvány nem ellenőrizhető. Ez nem jelent problémát. Kattintson a **Igen** tooverify hello hello virtuális gép identitását, és a bejelentkezés befejezéséhez.
   
   ![Képernyőkép üzenetről hello VM hello identitásának ellenőrzése](./media/hero-role/cert-warning.png)

Ha fut, tootrouble tooconnect meg, tekintse meg [hibaelhárítása távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gépek](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="install-iis-on-your-vm"></a>Az IIS telepítése a virtuális gépre
Most, hogy a virtuális gép toohello van bejelentkezve, egy kiszolgálói szerepkör telepítjük, így több kísérletezhet.

1. Ha még nincs megnyitva, nyissa meg a **Kiszolgálókezelőt**. Kattintson a hello **Start** menüben, majd kattintson **Kiszolgálókezelő**.
2. A **Kiszolgálókezelő**, jelölje be **helyi kiszolgáló** hello bal oldali ablaktáblán. 
3. Hello menüben válasszon ki **kezelése** > **szerepkörök és szolgáltatások hozzáadása**.
4. Hello hozzáadása szerepkörök és szolgáltatások varázsló hello **telepítési típus** lapon, válassza ki **szerepköralapú vagy szolgáltatásalapú telepítés**, és kattintson a **következő**.
   
    ![Képernyőkép ábrázoló hello hozzáadása szerepkörök és szolgáltatások varázsló lapján a telepítési típus](./media/hero-role/role-wizard.png)
5. Válassza ki a virtuális gép hello hello kiszolgálókészletből, és kattintson a **következő**.
6. A hello **kiszolgálói szerepkörök** lapon jelölje be **webkiszolgáló (IIS)**.
   
    ![Képernyőkép ábrázoló hello hozzáadása szerepkörök és szolgáltatások varázsló lapon kiszolgálói szerepköre tekintetében](./media/hero-role/add-iis.png)
7. Hello előugró IIS szükséges szolgáltatások hozzáadása, ügyeljen arra, hogy **felügyeleti eszközök** van kijelölve, majd kattintson **szolgáltatások hozzáadása**. Előugró hello befejeződésekor kattintson **következő** hello varázslóban.
   
    ![Képernyőfelvétel: előugró tooconfirm hello IIS szerepkör hozzáadása](./media/hero-role/confirm-add-feature.png)
8. Hello szolgáltatások lapon kattintson a **következő**.
9. A hello **webkiszolgálói szerepkör (IIS)** kattintson **következő**. 
10. A hello **szerepkör-szolgáltatások** kattintson **következő**. 
11. A hello **megerősítő** kattintson **telepítése**. 
12. Ha hello telepítés befejeződött, kattintson a **Bezárás** hello varázsló.

## <a name="open-port-80"></a>A 80-as port megnyitása
Ahhoz, hogy a virtuális gép tooaccept érkező bejövő forgalmat a 80-as porton keresztül, egy bejövő forgalomra vonatkozó szabály toohello hálózati biztonsági csoport tooadd van szüksége. 

1. Nyissa meg hello [Azure-portálon](https://portal.azure.com).
2. A **virtuális gépek** válassza ki a létrehozott virtuális gép hello.
3. Hello virtuális gépek beállításaiban, válassza ki a **hálózati illesztőt** és majd hello az válassza ki a meglévő hálózati illesztő.
   
    ![Képernyőfelvétel a hello hello virtuális gép beállítása hálózati illesztők](./media/hero-role/network-interface.png)
4. A **Essentials** hello hálózati adapter, kattintson a hello **hálózati biztonsági csoport**.
   
    ![Képernyőfelvétel a hello Essentials szakaszban hello hálózati adapter](./media/hero-role/select-nsg.png)
5. A hello **Essentials** hello NSG paneljén rendelkeznie kell egy meglévő alapértelmezett bejövő szabály **alapértelmezett engedélyezése rdp** így toolog a virtuális gép toohello. Egy másik bejövő forgalomra vonatkozó szabály tooallow IIS forgalom adhat. Kattintson a **Bejövő biztonsági szabály** elemre.
   
    ![Képernyőfelvétel a hello NSG hello Essentials szakaszban](./media/hero-role/inbound.png)
6. A **Bejövő biztonsági szabályok** területen kattintson a **Hozzáadás** elemre.
   
    ![Képernyőfelvétel: hello gomb tooadd biztonsági szabály](./media/hero-role/add-rule.png)
7. A **Bejövő biztonsági szabályok** területen kattintson a **Hozzáadás** elemre. Típus **80** a porttartomány hello, és győződjön meg arról, hogy **engedélyezése** van kiválasztva. Ha elkészült, kattintson az **OK** gombra.
   
    ![Képernyőfelvétel: hello gomb tooadd biztonsági szabály](./media/hero-role/port-80.png)

Az NSG-k, bejövő és kimenő szabályokat, további információ: [engedélyezése külső hozzáférés tooyour VM használatával hello Azure-portálon](nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="connect-toohello-default-iis-website"></a>Csatlakozás toohello alapértelmezett IIS-webhely
1. Hello Azure-portálon, kattintson **virtuális gépek** , és válassza ki a virtuális Gépet.
2. A hello **Essentials** panelen, a Másolás a **nyilvános IP-cím**.
   
    ![Ha toofind hello nyilvános IP-címet a virtuális gép ábrázoló képernyőfelvétel](./media/hero-role/ipaddress.png)
3. Nyisson meg egy böngészőt, és hello címsorába írja be a nyilvános IP-címet ehhez hasonló: http://<publicIPaddress> kattintson **Enter** toogo toothat cím.
4. A böngésző meg kell nyitnia a hello alapértelmezett IIS weblap. Ez a következőhöz hasonló:
   
    ![Egy böngészőben néz milyen hello alapértelmezett IIS lapot ábrázoló képernyőfelvétel](./media/hero-role/iis-default.png)

## <a name="next-steps"></a>Következő lépések
* Is kísérletezhet [adatlemezt csatol](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooyour virtuális gépet. Az adatlemezek nagyobb tárterületet biztosítanak a virtuális gép számára.

