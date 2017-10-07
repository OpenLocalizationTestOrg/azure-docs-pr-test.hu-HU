---
title: "Azure RemoteApp-lemezkép az Azure virtuális gép alapján aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate kezdve az Azure virtuális gépként az Azure RemoteApp lemezkép."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: d41583ef-6cd8-4115-8dcb-b2cd5b3d301a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 2d432bcb15be68a2946d91b5f36f41d980726338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Egy Azure RemoteApp-rendszerképet az Azure virtuális gép létrehozása
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Azure RemoteApp-lemezképek (amely a gyűjteményben lévő megosztott hello alkalmazások tárolására) hozhat létre egy Azure virtuális gépről. Akkor is kiválaszthatják toouse hozzáadott toohello Azure virtuális gép kép gyűjtemény összes hello Azure RemoteApp kép vonatkozó követelményeknek megfelelő virtuálisgép-lemezkép - használhatja, hogy a Virtuálisgép-lemezkép kiindulási pontként a saját virtuális gép, ha azt szeretné. Hello "Windows Server távoli asztali munkamenetgazda" kép hello könyvtárban keresse.

Saját rendszerkép alapján egy Azure virtuális gép két lépéseket toocreate – hello lemezkép létrehozása és majd töltse fel a hello Azure virtuális könyvtár tooAzure RemoteApp.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Egy Azure virtuális Gépen alapuló egyéni lemezkép létrehozása
Ezen lépések toocreate egy Azure virtuális Gépen alapuló rendszerképet használja.

1. Hozzon létre egy Azure virtuális gépen. Használhat hello "Windows Server távoli asztali munkamenetgazda" vagy "Windows Server távoli asztali munkamenet állomást a Microsoft Office 365 ProPlus" kép hello hello Azure virtuális gép lemezképének gyűjteményből. Ez a rendszerkép összes hello Azure RemoteApp sablon kép követelménynek megfelelő.
   
    További információkért lásd: [Windows rendszerű virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Csatlakozás a virtuális gép toohello és telepíteni, amelyet a Remoteappen keresztül tooshare hello alkalmazások konfigurálása. Győződjön meg arról, hogy tooperform minden további, az alkalmazások által igényelt Windows konfigurációt.
   
    További információkért lásd: [hogyan tooLog a virtuális gép futó Windows Server tooa](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Hello Windows Server távoli asztali munkamenetgazda lemezképeit egyikét használja, ha egy befoglalt érvényesítési parancsfájlt, amely biztosítja a virtuális gép megfelel-e hello RemoteApp előtti-reqs. van toorun parancsfájl, kattintson duplán a **ValidateRemoteAppImage** hello asztalon. Győződjön meg arról, hogy a Folytatás toohello következő lépés előtt rögzítettek hello szkript által jelzett hibákat.
4. A SYSPREP generalize és hello lemezképének. Lásd: [hogyan tooCapture egy Windows rendszerű virtuális gép tooUse sablonként](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) utasításokat.

## <a name="import-hello-image-into-hello-azure-remoteapp-image-library"></a>Hello Azure RemoteApp Képtár hello kép importálása
Ezen lépések tooimport hello új lemezképet használja az Azure Remoteappba:

1. A hello **Sablonrendszerképek** lapon:
   
   * Ha nincs meglévő lemezképet, kattintson a **feltölteni, vagy importáljon egy sablon rendszerképet**.
   * Ha már legalább egy kép, kattintson a  **+**  tooadd új lemezképet.
2. Válassza ki **kép importálása a virtuális gépek** könyvtárban, és kattintson **következő**.
3. Hello következő lapon válassza ki az egyéni lemezképet hello listából, és győződjön meg arról, hogy követték-e a lemezkép létrehozásakor hello lépéseket. Kattintson a **Tovább** gombra.
4. Adjon meg egy nevet hello új RemoteApp lemezképet, majd mentse hello helyre hello pipa toostart hello az importálási folyamat kattintson.

> [!NOTE]
> Azure virtuális gépek tooany Azure RemoteApp által támogatott Azure-beli hely által támogatott Azure bárhonnan importálhatja a lemezképeket. Attól függően, hogy hello helyek hello importálási too25 percig is eltarthat.
> 
> 

Most már áll készen áll toocreate az új gyűjtemény vagy egy [felhő](remoteapp-create-cloud-deployment.md) gyűjtemény vagy [hibrid](remoteapp-create-hybrid-deployment.md), attól függően, az igényeknek megfelelően.

