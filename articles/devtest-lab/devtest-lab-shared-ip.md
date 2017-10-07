---
title: "aaaUnderstand megosztott IP-címek a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Ismerje meg, hogyan Azure DevTest Labs használja a megosztott IP címek toominimize hello nyilvános IP címek szükséges tooaccess a labor virtuális gépeken."
services: devtest-lab
documentationcenter: na
author: camsoper
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: casoper
ms.openlocfilehash: 8756410117a9d550d567d372174bf1ea96703c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a>Megosztott IP-címek az Azure DevTest Labs ismertetése

Az Azure DevTest Labs segítségével tesztlabor virtuális gépek megosztás hello azonos nyilvános IP-cím toominimize hello száma szükséges tooaccess nyilvános IP címek a egyedi labor virtuális gépeken.  Ez a cikk ismerteti, hogyan megosztott IP-címek munkahelyi és a kapcsolódó beállítási lehetőségekre.

## <a name="shared-ip-setting"></a>A megosztott IP-beállítása

Ha a labor létrehozása a virtuális hálózati alhálózat helyezkedik el.  Alapértelmezés szerint ez az alhálózat jön létre **engedélyezése megosztott nyilvános IP-cím** túl beállítása*Igen*.  Ez a konfiguráció egyetlen nyilvános IP-cím hello teljes alhálózathoz tartozó hoz létre.  Virtuális hálózatok és alhálózatok konfigurálásával kapcsolatos további információkért lásd: [virtuális hálózat konfigurálása az Azure DevTest Labs](devtest-lab-configure-vnet.md).

![Új labor alhálózat](media/devtest-lab-shared-ip/lab-subnet.png)

Meglévő labs, engedélyezheti a beállítás kiválasztásával **konfigurációs és házirendek > virtuális hálózatok**. Ezt követően válassza ki a virtuális hálózat hello listából, és válassza **engedélyezése megosztott nyilvános IP-cím** a kijelölt alhálózathoz. Letilthatja ezt a beállítást, a laborban is, ha nem szeretné, hogy a nyilvános IP-cím tooshare tesztlabor virtuális gépek között.

A virtuális gépek a labor alapértelmezett toousing létre egy megosztott IP-cím.  Hello virtuális gép létrehozásakor a beállítás figyelhető-e meg hello **speciális beállítások** részen **IP-címkonfigurációt**.

![Új virtuális gép](media/devtest-lab-shared-ip/new-vm.png)

- **Megosztott:** létrehozandó virtuális gépeinek **megosztott** bekerülnek egy erőforráscsoportban (RG). Egy egyetlen IP-cím hozzá van rendelve, az adott Entitáshoz és hello RG virtuális gépeinek fogja használni az IP-címet.
- **Nyilvános:** minden virtuális gép létrehozása a saját IP-címmel rendelkezik, és a saját erőforráscsoportban jön létre.
- **Saját:** minden virtuális Géphez létrejön egy privát IP-címet használja. Csak akkor tudja tooconnect toothis VM hello közvetlenül az interneten a távoli asztal.

Virtuális gép és a megosztott IP engedélyezve toohello alhálózati ad hozzá, amikor a DevTest Labs automatikusan hello VM tooa terheléselosztó hozzáadása, és hozzárendeli a hello nyilvános IP-cím, az TCP-portszámot továbbítási toohello hello virtuális gép RDP-portjához.  

## <a name="using-hello-shared-ip"></a>IP megosztott hello használata

- **Linux-felhasználók:** hello IP-címét vagy teljesen minősített tartománynevét, majd egy kettőspontot a virtuális gép SSH-toohello hello port követ. A hello alábbi rendszerképek közül, hogy hello RDP például cím a virtuális gép tooconnect toohello `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.

  ![Virtuális gép – példa](media/devtest-lab-shared-ip/vm-info.png)

- **Windows-felhasználók:** válassza hello **Connect** hello Azure portál toodownload gombra egy előre megadott RDP-fájlt, és hozzáférési hello virtuális gép.

## <a name="next-steps"></a>Következő lépések

* [Labor házirendeket definiálhat az Azure DevTest Labs szolgáltatásban](devtest-lab-set-lab-policy.md)
* [A Azure DevTest Labs szolgáltatásban virtuális hálózat konfigurálása](devtest-lab-configure-vnet.md)





