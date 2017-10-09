---
title: "az Azure Windows virtuális gép lemezképét aaaCapture |} Microsoft Docs"
description: "Készítsen lemezképet arról az hello klasszikus üzembe helyezési modellel létrehozott Azure Windows virtuális géphez."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: a5986eac-4cf3-40bd-9b79-7c811806b880
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: b9bbc437012aa44295f90941c9d72e39509df28f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-hello-classic-deployment-model"></a>Készítsen lemezképet arról az hello klasszikus üzembe helyezési modellel létrehozott Azure Windows virtuális géphez.
> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Erőforrás-kezelő modell információkért lásd: [egy általánosított virtuális Gépet az Azure-ban a felügyelt lemezképének](../capture-image-resource.md).

Ez a cikk bemutatja, hogyan toocapture Windows, mint egy kép toocreate használhassa más virtuális gépeket futtató Azure virtuális géphez. Ez a rendszerkép tartalmazza hello operációsrendszer-lemez, és olyan adatokat lemezek, amelyek csatlakoztatott toohello virtuális gép. Hálózati beállítások, ezért fel egy hálózati konfigurációt tooset kell hello más hello lemezképet használó virtuális gépek létrehozásakor nem tartoznak bele.

Azure tárolók hello lemezkép **Virtuálisgép-rendszerképek (klasszikus)**, egy **számítási** szolgáltatás, amely szerepel a listában az összes megtekintésekor hello Azure-szolgáltatásokhoz. Ez a hello feltöltött képeket tároló ugyanazon a helyen. Lemezképek kapcsolatos részletekért lásd: [kapcsolatos képek a virtuális gépek](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).

## <a name="before-you-begin"></a>Előkészületek
Ezek a lépések feltételezik, hogy korábban már létrehozott Azure virtuális gép és konfigurált hello operációs rendszer, beleértve a bármely adatlemezt csatolni. Ha még nem meg, tekintse meg a következő cikkekben talál létrehozására, illetve hello virtuális gép előkészítése hello:

* [Virtuális gép létrehozása lemezkép](createportal.md)
* [Hogyan tooattach az adatok lemezre tooa virtuális gép](attach-disk.md)
* Ellenőrizze, hogy a Sysprep hello kiszolgálói szerepkörök támogatottak. További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [!WARNING]
> Ez a folyamat törli a hello eredeti virtuális gép rögzítése után.
>
>

Egy Azure virtuális gép lemezképének előzetes toocapturing ajánlott hello cél virtuális gép biztonsági másolatot. Az Azure virtuális gépek Azure Backup segítségével készíthető. További részletek: [Azure-beli virtuális gépek biztonsági mentése](../../../backup/backup-azure-vms.md). Hivatalos partnereinktől egyéb megoldások is elérhetők. jelenleg elérhető toofind keresési hello Azure piactéren.

## <a name="capture-hello-virtual-machine"></a>Hello virtuális gép rögzítése
1. A hello [Azure-portálon](http://portal.azure.com), **Connect** toohello virtuális gépet. Útmutatásért lásd: [hogyan tooa virtuális gép Windows Server rendszert futtató toosign][How toosign in tooa virtual machine running Windows Server].
2. Nyisson meg egy parancssori ablakot rendszergazdaként.
3. Hello könyvtárváltás túl`%windir%\system32\sysprep`, majd futtassa a sysprep.exe.
4. Hello **rendszer-előkészítő eszköz** párbeszédpanel jelenik meg. A következő hello:

   * A **Rendszerkarbantartási művelet**, jelölje be **adja meg a rendszer Out-of-Box élmény (OOBE)** , és győződjön meg arról, hogy **Generalize** be van jelölve. A Sysprep használatával kapcsolatos további információkért lásd: [hogyan tooUse Sysprep: Bevezetés][How tooUse Sysprep: An Introduction].
   * A **leállítási beállítások**, jelölje be **leállítási**.
   * Kattintson az **OK** gombra.

   ![Futtassa a Sysprep eszközt](./media/capture-image/SysprepGeneral.png)
5. A Sysprep hello állapotot hello virtuális gép hello Azure-portálon módosítja túl hello virtuális gép leáll**leállítva**.
6. Hello Azure-portálon, kattintson **virtuális gépek (klasszikus)** , és válassza ki a virtuális gép kívánt toocapture hello. Hello **Virtuálisgép-rendszerképek (klasszikus)** csoport megtalálható-e **számítási** megtekintésekor **további szolgáltatások**.

7. A hello parancssávon kattintson **rögzítése**.

   ![Virtuális gép rögzítése](./media/capture-image/CaptureVM.png)

   Hello **virtuális gép rögzítése hello** párbeszédpanel jelenik meg.

8. A **lemezképnév**, adjon meg egy nevet az új lemezképet hello. A **lemezképcímke**, írja be a címkék hello új lemezképet.

9. Kattintson a **hello virtuális gépen már futtattam a Sysprep**. Ezt a jelölőnégyzetet a 3-5. lépéseket a Sysprep toohello műveletek hivatkozik. A képfájl _kell_ futtassa a Sysprep a Windows Server hozzáadása előtt egyéni képek kép tooyour meghatározott általánosítva.

10. Hello rögzítése után hello új lemezkép válik elérhetővé hello **piactér**, a hello **számítási**, **Virtuálisgép-rendszerképek (klasszikus)** tároló.

    ![Lemezkép-rögzítési sikeres](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Következő lépések
hello kép készen toobe használt toocreate virtuális gépek. toodo, lesz hozzon létre egy virtuális gép hello kiválasztásával **további szolgáltatások** menüpont hello szolgáltatások menüt, majd hello alján **Virtuálisgép-rendszerképek (klasszikus)** a hello **számítási**csoport. Útmutatásért lásd: [virtuális gép létrehozása lemezkép](createportal.md).

[How toosign in tooa virtual machine running Windows Server]:connect-logon.md
[How tooUse Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/capture-image/SysprepGeneral.png
[hello virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of hello virtual machine]: ./media/capture-image/CaptureVM.png
[Enter hello image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use hello captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
