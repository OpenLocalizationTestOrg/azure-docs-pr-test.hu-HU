---
title: "virtuális gépek (klasszikus) - Azure CLI 1.0 aaaConfigure magánhálózati IP-címek |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure magánhálózati IP-címek használata virtuális gépek (klasszikus) hello Azure parancssori felület (CLI) 1.0-s."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17386acf-c708-4103-9b22-ff9bf04b778d
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 417a57181bcf5c2e6101bf3bdf63fc94ebc99df5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-hello-azure-cli-10"></a>Egy virtuális gép (klasszikus) Azure CLI 1.0 hello saját IP-címek konfigurálása

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben. Emellett [hello Resource Manager üzembe helyezési modellel statikus magánhálózati IP-cím kezelése](virtual-networks-static-private-ip-arm-cli.md).

minta hello Azure CLI-t az alábbi parancsok már létrehozott egy egyszerű környezetben várható. Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre leírt tesztkörnyezet hello [hozhat létre egy vnetet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a>Hogyan toospecify egy statikus magánhálózati IP-virtuális gép létrehozásakor
toocreate nevű új virtuális gép *DNS01* új felhőszolgáltatásban nevű *TestService* a fenti hello forgatókönyv alapján, kövesse az alábbi lépéseket:

1. Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.
2. Futtassa a hello **azure-szolgáltatás létrehozása** toocreate hello felhőszolgáltatás parancsot.
   
        azure service create TestService --location uscentral
   
    Várt kimenet:
   
        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
3. Futtassa a hello **azure virtuális gép létrehozása** parancs toocreate hello virtuális gép. Figyelje meg, hogy egy statikus magánhálózati IP-cím hello értéke. hello kimenet után látható hello lista hello paramétereket ismerteti.
   
        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd
   
    Várt kimenet:
   
        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting too"Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK
   
   * **-l (vagy --location)**. Azure-régió, ahol a virtuális gép hello létrejön. A mi esetünkben *centralus*.
   * **-n (vagy--virtuálisgép-név)**. Hello VM toobe létrehozott neve.
   * **-l (vagy--virtuális hálózat neve)**. Hello ahol létrejön a virtuális gép hello VNet neve. 
   * **-S (vagy--statikus ip-)**. Statikus magánhálózati IP-címet hello virtuális gép.
   * **TestService**. Ahol létrejön a virtuális gép hello hello felhőalapú szolgáltatás neve.
   * **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**. Kép toocreate hello virtuális gép használja.
   * **adminuser**. Hello Windows virtuális gép helyi rendszergazdája.
   * **AdminP@ssw0rd**. Hello Windows virtuális gép helyi rendszergazda jelszavát.

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a>Hogyan tooretrieve statikus magánhálózati IP-címe címadatok a virtuális gépek
tooview hello statikus magánhálózati IP-címe cím VM létre hello parancsfájl fenti, Futtatás a következő parancs az Azure parancssori felület hello hello adatait, és tekintse meg az hello értéke *hálózati StaticIP*:

    azure vm static-ip show DNS01

Várt kimenet:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a>Hogyan tooremove egy statikus magánhálózati IP-VM
tooremove hello statikus magánhálózati IP-cím toohello VM hello parancsfájlban felett, a következő Azure CLI-parancs futtatása hello hozzáadva:

    azure vm static-ip remove DNS01

Várt kimenet:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-tooadd-a-static-private-ip-tooan-existing-vm"></a>Hogyan tooadd statikus magánhálózati IP-tooan meglévő virtuális gép
a statikus magánhálózati IP cím toohello runt a fenti hello parancsfájl a következő parancs használatával létrehozott virtuális gép tooadd:

    azure vm static-ip set DNS01 192.168.1.101

Várt kimenet:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Következő lépések
* További tudnivalók [foglalt nyilvános IP-cím](virtual-networks-reserved-public-ip.md) címek.
* További tudnivalók [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) címek.
* Tekintse át a hello [fenntartott IP REST API-k](https://msdn.microsoft.com/library/azure/dn722420.aspx).

