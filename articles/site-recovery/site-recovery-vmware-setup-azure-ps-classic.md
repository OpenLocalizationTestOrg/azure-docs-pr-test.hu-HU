---
title: " A Azure(Classic) folyamat kiszolgáló kezeléséhez |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset fel a feladatátvételi folyamat Server(Classic) az Azure."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: eadcc0236c77c9ebbbc885c4a7ee81098f1f4e72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-process-server-running-in-azure-classic"></a>Egy folyamat (klasszikus) Azure-t futtató kiszolgáló kezelése
> [!div class="op_single_selector"]
> * [Klasszikus Azure portál](./site-recovery-vmware-setup-azure-ps-classic.md)
> * [Resource Manager](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

A feladat-visszavétel során nagy késleltetésű hello Azure virtuális hálózat és a helyszíni hálózat között esetén ajánlott toodeploy Folyamatkiszolgáló az Azure-ban. Ez a cikk ismerteti, hogyan beállításához, konfigurálásához és hello folyamat kiszolgálók Azure-beli kezeléséhez.

> [!NOTE]
> Ez a cikk ha használt klasszikus telepítési modell hello hello virtuális gépek a feladatátvétel során használt toobe. Ha Ön használhat erőforrás-kezelő telepítési modell kövesse hello lépéseit hello [hogyan tooset akár & a feladat-visszavételi Folyamatkiszolgáló (erőforrás-kezelő) konfigurálása](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

## <a name="prerequisites"></a>Előfeltételek

[!INCLUDE [site-recovery-vmware-process-server-prereq](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a>Az Azure-on folyamat-kiszolgáló központi telepítése

1. Azure piactér, hozzon létre egy virtuális gép hello **Microsoft Azure Site helyreállítási folyamat kiszolgáló V2** </br>
    ![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)
2. Győződjön meg arról, hogy kiválassza hello üzembe helyezési modellel, **klasszikus** </br>
  ![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)
3. Hello hozzon létre virtuális gép varázsló > alapbeállítások, győződjön meg arról, átadja a hello virtuális gépek hello előfizetésben és helyen toowhere választja.</br>
  ![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)
4. Győződjön meg arról, hogy hello a virtuális gép csatlakozik toohello Azure Virtual Network toowhich hello átadja a virtuális géphez van csatlakoztatva.</br>
  ![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)
5. Hello Folyamatkiszolgáló virtuális gép üzembe helyezése után meg kell a toolog, és regisztrálhatja azt az hello konfigurációs kiszolgáló.

> [!NOTE]
> toobe képes toouse a Folyamatkiszolgálót a feladat-visszavétel tooregister kell azt hello helyszíni konfigurációs kiszolgáló.

## <a name="registering-hello-process-server-running-in-azure-tooa-configuration-server-running-on-premises"></a>Hello (Azure-beli) Folyamatkiszolgáló tooa konfigurációs kiszolgáló (a helyszínen fut) regisztrálása

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-hello-process-server-toolatest-version"></a>Hello Folyamatkiszolgáló toolatest verziójának frissítése.

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-hello-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a>Regisztrációjának megszüntetése hello Folyamatkiszolgáló (Azure-beli) a konfigurációs kiszolgálóról (a helyszínen fut)

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
