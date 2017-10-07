---
title: "aaaMigrate virtuális gépek tooResource Manager Azure parancssori felületével |} Microsoft Docs"
description: "Ez a cikk végigvezeti hello platform által támogatott áttelepítési erőforrások klasszikus tooAzure erőforrás-kezelő az Azure parancssori felület használatával"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d6f5a877-05b6-4127-a545-3f5bede4e479
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 0b11f4bb1b4658b3e88bf7629147ed953b678556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-iaas-resources-from-classic-tooazure-resource-manager-by-using-azure-cli"></a>IaaS-erőforrásokra át klasszikus tooAzure erőforrás-kezelő Azure parancssori felület használatával
Ezek a lépések bemutatják, hogyan toouse Azure parancssori felület (CLI) parancsok a szolgáltató (IaaS) erőforrásként hello klasszikus telepítési modell toohello Azure Resource Manager telepítési modellből toomigrate infrastruktúra. hello cikk szükséges hello [Azure CLI](../../cli-install-nodejs.md).

> [!NOTE]
> Az itt ismertetett összes hello művelet idempotent. Ha a probléma nem támogatott szolgáltatása vagy konfigurációs hiba, azt javasoljuk, hogy újra hello előkészítése, megszakítása vagy véglegesítése a műveletet. hello platform fogják megpróbálni hello művelet újrapróbálása.
> 
> 

<br>
Ez a folyamatábra tooidentify hello sorrendet, amelyben lépést kell végrehajtani az áttelepítési folyamat során toobe

![Képernyőkép a hello áttelepítés lépései](../windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-prepare-for-migration"></a>1. lépés: Felkészülés az áttelepítésre
Az alábbiakban néhány gyakorlati tanácsok, amely értékeli a klasszikus tooResource Manager áttelepítése IaaS-erőforrásokra, javasoljuk:

* Olvassa végig hello [listája nem támogatott konfigurációk vagy szolgáltatások](../windows/migration-classic-resource-manager-overview.md). Ha még nem támogatott konfigurációk vagy szolgáltatások használó virtuális gépek, azt javasoljuk, várja meg a hello szolgáltatást vagy a konfigurációs támogatás toobe jelentette be. Azt is megteheti távolítsa el a szolgáltatást, vagy a konfigurációs tooenable áttelepítést kilépni, ha az igényeknek.
* Ha olyan parancsfájlok, amelyek központi telepítése az infrastruktúra és az alkalmazások ma rendelkezik automatikus, próbálja toocreate egy hasonló vizsgálat beállítása az áttelepítés ezen parancsfájlok használatával. Másik lehetőségként állíthat be minta környezetek hello Azure-portál használatával.

> [!IMPORTANT]
> Alkalmazásátjárót jelenleg nem támogatottak az áttelepítést a klasszikus tooResource Manager. toomigrate Alkalmazásátjáró, klasszikus virtuális hálózat hello átjáró előkészítési művelet toomove hello hálózati futtatása előtt távolítsa el. Hello áttelepítés befejezése után újra hello átjáró az Azure Resource Manager. 
>
>Csatlakozás tooExpressRoute Kapcsolatcsoportok egy másik előfizetésben található ExpressRoute-átjárók nem telepíthetők át automatikusan. Ilyen esetekben hello ExpressRoute-átjáró eltávolítása, telepítse át a virtuális hálózati hello, és hozza létre újra a hello átjáró. Ellenőrizze a [áttelepítése ExpressRoute áramkörök és társított virtuális hálózatokat hello klasszikus toohello Resource Manager üzembe helyezési modellben](../../expressroute/expressroute-migration-classic-resource-manager.md) további információt.
> 
> 

## <a name="step-2-set-your-subscription-and-register-hello-provider"></a>2. lépés: Az előfizetés beállítása és hello szolgáltató regisztrálása
Áttelepítési forgatókönyvek esetén mindkét klasszikus környezet szüksége tooset és erőforrás-kezelő. [Az Azure parancssori felület telepítése](../../cli-install-nodejs.md) és [jelölje ki az előfizetését](../../xplat-cli-connect.md).

Bejelentkezési tooyour fiók.

    azure login

Válassza ki a hello Azure-előfizetés hello a következő parancs használatával.

    azure account set "<azure-subscription-name>"

> [!NOTE]
> Regisztráció az csak egyszer módosítható. lépés:, de igények toobe migrálás megkísérlése előtt egyszer történik. Regisztrálása nélkül látni fogja a hibaüzenet a következő hello 
> 
> *BadRequest: Az előfizetés nincs regisztrálva az áttelepítéshez.* 
> 
> 

Hello áttelepítési erőforrás-szolgáltató regisztrálása hello a következő parancs használatával. Vegye figyelembe, hogy néhány esetben ez a parancs végrehajtásának időkorlátja. Hello regisztrációs azonban sikeres lesz.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Kis türelmet hello regisztrációs toofinish öt percet. Hello jóváhagyási hello állapotának hello a következő parancs használatával ellenőrizheti. Győződjön meg arról, hogy van-e RegistrationState `Registered` folytatás előtt.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Most már a CLI toohello kapcsoló `asm` mód.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-hello-azure-region-of-your-current-deployment-or-vnet"></a>3. lépés: Ellenőrizze, hogy elegendő Azure Resource Manager virtuális gép magok a hello Azure-régió, a jelenlegi üzemelő példány vagy virtuális hálózaton
Ebben a lépésben szüksége lesz tooswitch túl`arm` mód. Ugyanezt a műveletet a következő parancs hello.

```
azure config mode arm
```

A következő parancssori felület parancs toocheck hello aktuális mennyisége az Azure Resource Manager rendelkezik magok hello is használhatja. toolearn kvótákat, az alapvető kapcsolatos további információkért lásd: [korlátozásai és hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Miután végzett ezzel a lépéssel ellenőrzése, váltson vissza túl`asm` mód.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>4. lépés: 1. lehetőség – a felhőszolgáltatásban található virtuális gépek áttelepítése
Hello listájának beszerzése felhőszolgáltatások hello a következő parancs használatával, majd mentse hello felhőalapú szolgáltatás, amelyet az toomigrate. Vegye figyelembe, hogy ha hello virtuális gépek hello a felhőszolgáltatásban található egy virtuális hálózatot, vagy webes vagy feldolgozói szerepkörök rendelkeznek, elérhetővé válik egy hibaüzenetet.

    azure service list

Hello hello részletes kimenet tooget hello telepítési parancsnév hello felhőszolgáltatás követően futtassa. A legtöbb esetben hello telepítési neve hello ugyanaz, mint a hello felhőszolgáltatás neve.

    azure service show <serviceName> -vv

Először ellenőrzi, hogy hello felhőalapú szolgáltatás a következő parancsok hello segítségével telepíthet át:

```shell
azure service deployment validate-migration <serviceName> <deploymentName> new "" "" ""
```

Készítse elő a hello virtuális gépek áttelepítésre hello felhőszolgáltatásban. A két beállítások toochoose rendelkezik.

Toomigrate hello virtuális gépek tooa platform által létrehozott virtuális hálózaton, használja a következő parancs hello.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Ha azt szeretné, hogy a meglévő virtuális hálózat hello Resource Manager üzembe helyezési modellel toomigrate tooan, használja a következő parancs hello.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> <subnetName> <vnetName>

Hello előkészítése után a művelet sikeres, nézze át a hello részletes kimenet tooget hello áttelepítési állapotának hello virtuális gépeket, és győződjön meg arról, hogy vannak-e a hello `Prepared` állapotát.

    azure vm show <vmName> -vv

A parancssori felületen vagy a hello Azure portál segítségével erőforrások előkészített hello hello konfigurációjának ellenőrzése. Ha nem az áttelepítéshez, és azt szeretné, hogy toogo hátsó toohello régi állapot, használja a következő parancs hello.

    azure service deployment abort-migration <serviceName> <deploymentName>

Ha hello előkészített konfiguráció megfelelőnek tűnik, előre, és véglegesíti hello erőforrások hello a következő parancs használatával.

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>4. lépés: 2. lehetőség – a virtuális hálózatban lévő virtuális gépek áttelepítése
Mentse hello virtuális hálózatot, hogy szeretné-e toomigrate. Vegye figyelembe, hogy ha hello virtuális hálózati webes vagy feldolgozói szerepköröket és a virtuális gépeken nem támogatott konfigurációjú, üzenetet fog kapni egy érvényesítési hiba.

A következő parancs hello segítségével könnyebben nyerhet hello előfizetés összes hello virtuális hálózatot.

    azure network vnet list

hello kimeneti következőhöz hasonlóan fog kinézni:

![Képernyőfelvétel a hello parancssori hello teljes virtuális hálózati név kiemelve.](../media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

A fenti példa hello, hello **virtualNetworkName** hello teljes név **"Csoport classicubuntu16 classicubuntu16"**.

Először ellenőrzi, hogy áttelepítheti a virtuális hálózat hello hello a következő parancs használatával:

```shell
azure network vnet validate-migration <virtualNetworkName>
```

Virtuális hálózati hello az Ön által választott Felkészülés az áttelepítésre hello a következő parancs használatával.

    azure network vnet prepare-migration <virtualNetworkName>

A virtuális gépek operációs parancssori felületen vagy a hello Azure-portál használatával hello hello konfigurációjának ellenőrzése. Ha nem az áttelepítéshez, és azt szeretné, hogy toogo hátsó toohello régi állapot, használja a következő parancs hello.

    azure network vnet abort-migration <virtualNetworkName>

Ha hello előkészített konfiguráció megfelelőnek tűnik, előre, és véglegesíti hello erőforrások hello a következő parancs használatával.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>5. lépés: A storage-fiókok áttelepítése
Ha elkészült hello virtuális gépeinek áttelepítését, ajánlott hello tárfiók az áttelepítést.

Hello tárfiók előkészítése az áttelepítésre hello a következő parancs használatával

    azure storage account prepare-migration <storageAccountName>

A tárfiók parancssori felületen vagy a hello Azure portál segítségével előkészített hello hello konfigurációjának ellenőrzése. Ha nem az áttelepítéshez, és azt szeretné, hogy toogo hátsó toohello régi állapot, használja a következő parancs hello.

    azure storage account abort-migration <storageAccountName>

Ha hello előkészített konfiguráció megfelelőnek tűnik, előre, és véglegesíti hello erőforrások hello a következő parancs használatával.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Következő lépések

* [IaaS-erőforrásokra, erőforrás-kezelő klasszikus tooAzure a platform által támogatott áttelepítésének áttekintése](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének tervezése](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [PowerShell toomigrate IaaS erőforrásainak klasszikus tooAzure erőforrás-kezelő használata](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének védelmével kapcsolatos közösségi eszközök](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [A leggyakoribb áttelepítési hibák áttekintése](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Felülvizsgálati hello legtöbb kapcsolatos gyakori kérdések a klasszikus tooAzure erőforrás-kezelő áttelepítése IaaS-erőforrásokra](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
