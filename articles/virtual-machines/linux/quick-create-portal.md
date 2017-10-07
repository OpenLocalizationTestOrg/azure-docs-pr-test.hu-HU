---
title: "gyors üzembe helyezés – aaaAzure hozzon létre Virtuálisgép-portál |} Microsoft Docs"
description: "Azure gyors üzembe helyezés – Virtuális gép létrehozása – Portal"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 984a400c976e349a59f873210d6e04bcdea39e1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-hello-azure-portal"></a>Hozzon létre egy Linux virtuális gép hello Azure-portálon

Az Azure virtuális gépek hello Azure-portálon keresztül is létrehozható. Ez a módszer egy böngészőalapú felhasználói felületet biztosít a virtuális gépek, valamint az összes kapcsolódó erőforrás létrehozásához és konfigurálásához. A gyors üzembe helyezés lépéseit egy virtuális gépet hoz létre, és egy webkiszolgáló hello VM telepíti.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

## <a name="create-ssh-key-pair"></a>SSH-kulcspár létrehozása

A gyors üzembe helyezési szüksége van egy SSH-kulcspár toocomplete. Ha már rendelkezik SSH-kulcspárral, kihagyhatja ezt a lépést.

A rendszerhéjakba futtassa ezt a parancsot, és kövesse hello képernyőn megjelenő utasításokat. hello parancs kimenete hello hello nyilvános kulcsfájl nevét tartalmazza. Hello tartalom hello nyilvános kulcsot tartalmazó fájlt toohello vágólapra másolása.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure 

Jelentkezzen be toohello http://portal.azure.com: az Azure portál.

## <a name="create-virtual-machine"></a>Virtuális gép létrehozása

1. Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.

2. Válassza a **Számítás**, majd az **Ubuntu Server 16.04 LTS** elemet. 

3. Adja meg a hello virtuális gép adatait. A **Hitelesítés típusa** résznél válassza az **SSH nyilvános kulcs** lehetőséget. A nyilvános SSH-kulcs beillesztéskor gondoskodunk tooremove bármely kezdő vagy záró szóközt. Amikor végzett, kattintson az **OK** gombra.

    ![Adja meg a virtuális Gépre vonatkozó alapvető adatok hello portál panel](./media/quick-create-portal/create-vm-portal-basic-blade.png)

4. Virtuális gép hello kiválasztásához. toosee további méretét, válasszon **összes** , vagy módosítsa a hello **lemez típusát támogatott** szűrő. 

    ![Képernyőkép a virtuális gépek méreteivel](./media/quick-create-portal/create-linux-vm-portal-sizes.png)  

5. A hello-beállítások panelen hello alapértelmezett tartani, és kattintson a **OK**.

6. Hello összefoglaló lapon kattintson **Ok** toostart hello virtuálisgép-telepítést.

7. virtuális gép hello Azure portál Irányítópultjára rögzített toohello lesz. Hello központi telepítés befejezése után automatikusan megnyílik hello VM összefoglaló panel.


## <a name="connect-toovirtual-machine"></a>Csatlakoztassa a gépet toovirtual

Az SSH-kapcsolat létrehozása hello virtuális géppel.

1. Kattintson a hello **Connect** hello virtuális gépek paneljét gombjára. hello csatlakozás gomb megjelenítése egy SSH-kapcsolati karakterlánc, amely használt tooconnect toohello virtuális géphez.

    ![Portál – 9](./media/quick-create-portal/portal-quick-start-9.png) 

2. Futtatási hello következő parancsot a toocreate egy SSH-munkamenetet. Cserélje le egy másolta az Azure-portálon hello hello hello kapcsolati karakterláncot.

```bash 
ssh azureuser@40.112.21.50
```

## <a name="install-nginx"></a>Az NGINX telepítése

Használjon hello alábbi parancsfájl tooupdate csomag források bash, és hello legújabb NGINX csomag telepítése. 

```bash 
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

Lépjen ki a hello SSH-munkamenetet, és adja vissza hello virtuális gép tulajdonságok hello Azure-portálon.


## <a name="open-port-80-for-web-traffic"></a>A 80-as port megnyitása a webes adatforgalom számára 

A hálózati biztonsági csoport (NSG) feladata a bejövő és kimenő forgalom védelme. Ha egy virtuális gép létrehozása az Azure-portálon hello, egy bejövő forgalomra vonatkozó szabály az SSH-kapcsolatokhoz 22-es portot jön létre. A virtuális gép egy webkiszolgáló üzemelteti, mert egy NSG-szabály a 80-as port számára létrehozott toobe kell.

1. Hello virtuális gépre, kattintson a hello hello neve **erőforráscsoport**.
2. Jelölje be hello **hálózati biztonsági csoport**. hello NSG használatával azonosíthatók hello **típus** oszlop. 
3. Kattintson a bal oldali menüben hello, a beállítások **bejövő biztonsági szabályok**.
4. Kattintson a **Hozzáadás** gombra.
5. A **Név** mezőbe írja be a **http** karakterláncot. Győződjön meg arról, hogy **porttartomány** too80 beállítása és **művelet** értéke túl**engedélyezése**. 
6. Kattintson az **OK** gombra.


## <a name="view-hello-nginx-welcome-page"></a>Hello NGINX üdvözlőlap megtekintése

Az NGINX telepített, és a port 80 nyissa meg a tooyour VM, hello webkiszolgáló most elérhető hello az interneten. Nyisson meg egy webböngészőt, és adja meg a virtuális gép hello hello nyilvános IP-címét. hello VM panel az Azure-portálon hello hello nyilvános IP-cím található.

![Alapértelmezett NGINX-webhely](./media/quick-create-cli/nginx.png) 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs szükség, törölje a hello erőforráscsoport, a virtuális gép és az összes kapcsolódó erőforrások. toodo Igen, jelölje ki hello erőforráscsoport hello virtuális gépek paneljét, majd kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, egy hálózati biztonsági csoport szabályát, valamint telepített egy webkiszolgálót. További információ az Azure virtuális gépeken, toolearn toohello oktatóanyag Linux virtuális gépek továbbra is.

> [!div class="nextstepaction"]
> [Azure-beli Linux rendszerű virtuális gépek – oktatóanyag](./tutorial-manage-vm.md)
