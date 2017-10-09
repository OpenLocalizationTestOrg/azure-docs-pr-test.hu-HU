---
title: "aaaLog tooa a klasszikus Azure virtuális gép |} Microsoft Docs"
description: "Az Azure portál toolog hello virtuális gépen használni tooa Windows hello klasszikus telepítési modellel létrehozott."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 3c1239ed-07dc-48b8-8b3d-dc8c5f0ab20e
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 2e32b7036c2538e73b46580e0f5f8f4979e8a685
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-on-tooa-windows-virtual-machine-using-hello-azure-portal"></a>Jelentkezzen be tooa Windows virtuális gép hello Azure-portál használatával
Hello Azure-portálon, használhatja az hello **Connect** egy távoli asztali munkamenet toostart gombra, és jelentkezzen be Windows virtuális gép tooa.

Linux virtuális gép tooconnect tooa szeretne? Lásd: [hogyan toolog a Linux rendszerű virtuális gép tooa](../../linux/mac-create-ssh-keys.md).

<!--
Deleting, but not 100% sure
Learn how too[perform these steps using new Azure portal](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Hogyan tooa VM használatával toolog hello erőforrás-kezelő információt objektummodell [Itt](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="connect-toohello-virtual-machine"></a>Csatlakoztassa toohello virtuális gépet
1. Bejelentkezés toohello Azure-portálon.
2. Kattintson a hello virtuális gép, amelyet az tooaccess. hello szerepel-e hello **összes erőforrás** ablaktáblán.

    ![Virtuális-machine-helyek](./media/connect-logon/azureportaldashboard.png)

3. Kattintson a **Connect** hello parancssávon elveire hello virtuális gép irányítópult.

    ![Csatlakozás hello virtuális gép ikonja](./media/connect-logon/virtualmachine_dashboard_connect.png)

<!-- Don't know if this still applies
     I think we can zap this.
> [!TIP]
> If hello **Connect** button isn't available, see hello troubleshooting tips at hello end of this article.
>
>
-->

## <a name="log-on-toohello-virtual-machine"></a>Jelentkezzen be a virtuális gép toohello
[!INCLUDE [virtual-machines-log-on-win-server](../../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Következő lépések
* Ha hello **Connect** gomb inaktív vagy egyéb problémák hello távoli asztali kapcsolattal, vagy próbálja hello-konfigurációjának visszaállítása. Kattintson a **távelérés alaphelyzetbe állítása** hello virtuális gép irányítópulton.

    ![Távelérés alaphelyzetbe állítása](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* A jelszó kapcsolatos problémák próbálkozzon a visszaállításával. Kattintson a **jelszó-átállítási** mentén hello bal szélétől virtuális gép irányítópult a **támogatási + hibaelhárítás**.

    ![Jelszó alaphelyzetbe állítása](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

Ha ezek a tippek nem működnek, vagy nem kapcsolódnak, mire van szüksége, tekintse meg [hibaelhárítása távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gépek](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ez a cikk útmutatást nyújt a gyakori problémák diagnosztizálásához és elhárításához.
