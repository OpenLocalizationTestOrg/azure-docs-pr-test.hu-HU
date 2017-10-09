---
title: "gyors üzembe helyezés – aaaAzure Windows virtuális gép portál létrehozása |} Microsoft Docs"
description: "Azure gyors üzembe helyezés – Windows virtuális gép létrehozása a Portalon"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5646ad51244db6d214c0121d1f7cc45c59f9a78b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-hello-azure-portal"></a>Hozzon létre egy Windows rendszerű virtuális gép hello Azure-portálon

Az Azure virtuális gépek hello Azure-portálon keresztül is létrehozható. Ez a módszer egy böngészőalapú felhasználói felületet biztosít a virtuális gépek, valamint az összes kapcsolódó erőforrás létrehozásához és konfigurálásához. A gyors üzembe helyezés lépéseit egy virtuális gépet hoz létre, és egy webkiszolgáló hello VM telepíti.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Jelentkezzen be toohello http://portal.azure.com: az Azure portál.

## <a name="create-virtual-machine"></a>Virtuális gép létrehozása

1. Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.

2. Válassza a **Számítás**, majd a **Windows Server 2016 Datacenter** elemet. 

3. Adja meg a hello virtuális gép adatait. hello felhasználónevét és jelszavát, az itt megadott használt toolog toohello virtuális gépen. Amikor végzett, kattintson az **OK** gombra.

    ![Adja meg a virtuális Gépre vonatkozó alapvető adatok hello portál panel](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)  

4. Virtuális gép hello kiválasztásához. toosee további méretét, válasszon **összes** , vagy módosítsa a hello **lemez típusát támogatott** szűrő. 

    ![Képernyőkép a virtuális gépek méreteivel](./media/quick-create-portal/create-windows-vm-portal-sizes.png)  

5. A hello-beállítások panelen hello alapértelmezett tartani, és kattintson a **OK**.

6. Hello összefoglaló lapon kattintson **Ok** toostart hello virtuálisgép-telepítést.

7. virtuális gép hello Azure portál Irányítópultjára rögzített toohello lesz. Hello központi telepítés befejezése után automatikusan megnyílik hello VM összefoglaló panel.


## <a name="connect-toovirtual-machine"></a>Csatlakoztassa a gépet toovirtual

Távoli asztali kapcsolat toohello virtuális gép létrehozása.

1. Kattintson a hello **Connect** hello virtuális gép Tulajdonságok gombra. A rendszer létrehoz és letölt egy Remote Desktop Protocol-fájlt (.rdp-fájlt).

    ![Portál – 9](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png) 

2. virtuális gép, nyissa meg hello tooconnect tooyour letöltött RDP-fájlt. Ha a rendszer kéri, akkor kattintson a **Csatlakozás** gombra. A Mac, ez például RDP-ügyfelet kell [távoli asztali ügyfél](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) hello Mac App Store áruházból származó.

3. Adja meg a hello felhasználónevet és jelszót hello virtuális gép létrehozásakor megadott, majd kattintson a **Ok**.

4. Hello bejelentkezési folyamat során a tanúsítványra vonatkozó figyelmeztetést is megjelenhet. Kattintson a **Igen** vagy **Folytatás** tooproceed hello kapcsolattal.


## <a name="install-iis-using-powershell"></a>Az IIS telepítése a PowerShell-lel

Hello virtuális gépen indítson el egy PowerShell-munkamenetet, és futtassa a következő parancs tooinstall IIS hello.

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Lépjen ki a hello RDP-munkamenetet, és adja vissza hello virtuális gép tulajdonságok hello Azure-portálon.

## <a name="open-port-80-for-web-traffic"></a>A 80-as port megnyitása a webes adatforgalom számára 

A hálózati biztonsági csoport (NSG) feladata a bejövő és kimenő forgalom védelme. Ha egy virtuális gép létrehozása az Azure-portálon hello, egy bejövő forgalomra vonatkozó szabály a 3389-es porton az RDP-kapcsolatok jön létre. A virtuális gép egy webkiszolgáló üzemelteti, mert egy NSG-szabály a 80-as port számára létrehozott toobe kell.

1. Hello virtuális gépre, kattintson a hello hello neve **erőforráscsoport**.
2. Jelölje be hello **hálózati biztonsági csoport**. hello NSG használatával azonosíthatók hello **típus** oszlop. 
3. Kattintson a bal oldali menüben hello, a beállítások **bejövő biztonsági szabályok**.
4. Kattintson a **Hozzáadás** gombra.
5. A **Név** mezőbe írja be a **http** karakterláncot. Győződjön meg arról, hogy **porttartomány** too80 beállítása és **művelet** értéke túl**engedélyezése**. 
6. Kattintson az **OK** gombra.


## <a name="view-hello-iis-welcome-page"></a>Nézet hello IIS-kezdőlap

Az IIS-kiszolgálón telepített, és a port 80 nyissa meg a virtuális gép tooyour, hello webkiszolgáló most már elérhető az hello internet. Nyisson meg egy webböngészőt, és adja meg a virtuális gép hello hello nyilvános IP-címét. hello VM panel az Azure-portálon hello hello nyilvános IP-cím található.

![Alapértelmezett IIS-webhely](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs szükség, törölje a hello erőforráscsoport, a virtuális gép és az összes kapcsolódó erőforrások. toodo Igen, jelölje ki hello erőforráscsoport hello virtuális gépek paneljét, majd kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, egy hálózati biztonsági csoport szabályát, valamint telepített egy webkiszolgálót. További információ az Azure virtuális gépeken, toolearn toohello oktatóanyag Windows virtuális gépek továbbra is.

> [!div class="nextstepaction"]
> [Windowsos virtuális gépek az Azure-ban – oktatóanyagok](./tutorial-manage-vm.md)
