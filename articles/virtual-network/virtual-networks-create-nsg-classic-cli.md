---
title: "aaaHow toocreate NSG-ket a klasszikus mód használatával hello Azure parancssori Felülettel |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és NSG-k telepítése a klasszikus módban, hello Azure parancssori felület használatával"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 17d98950-5fbb-4653-bef6-d822ab37541e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: eb78861e10a0dd950bb2c3783ee957d1cce55016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-nsgs-classic-in-hello-azure-cli"></a>Hogyan toocreate NSG-k (klasszikus) hello Azure parancssori felület
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben. Emellett [NSG-k létrehozása hello Resource Manager üzembe helyezési modellel](virtual-networks-create-nsg-arm-cli.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

minta hello Azure CLI-t az alábbi parancsok már a fenti hello forgatókönyv alapján létre egy egyszerű környezetben várható. Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre tesztkörnyezetben hello által [létre virtuális hálózatok](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-toocreate-hello-nsg-for-hello-front-end-subnet"></a>Hogyan toocreate hello hello előtér alhálózat NSG
az NSG nevű toocreate nevű **NSG-előtérbeli** kövesse a fenti hello forgatókönyv alapján, hello alábbi lépéseket.

1. Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.
2. Futtassa a hello  **`azure config mode`**  tooswitch tooclassic üzemmód, alább látható módon.
   
        azure config mode asm
   
    Várt kimenet:
   
        info:    New mode is asm
3. Futtassa a hello  **`azure network nsg create`**  parancs toocreate egy NSG.
   
        azure network nsg create -l uswest -n NSG-FrontEnd
   
    Várt kimenet:
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up hello network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    Paraméterek:
   
   * **-l (vagy --location)**. Azure-régió, ahol hello új NSG létrejön. A mi esetünkben *westus*.
   * **-n (vagy --name)**. Hello nevét új NSG. A mi esetünkben *NSG-előtérbeli*.
4. Futtassa a hello  **`azure network nsg rule create`**  parancs toocreate egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 3389-es (RDP) a hello Internet.
   
        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    Várt kimenet:
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
   
    Paraméterek:
   
   * **-a (vagy--nsg-név)**. Mely hello szabály létrehozza hello NSG neve. A mi esetünkben *NSG-előtérbeli*.
   * **-n (vagy --name)**. Hello új szabály nevét. A mi esetünkben *rdp-szabály*.
   * **-c (vagy--művelet)**. Hozzáférési szint hello szabály (Megtagadás vagy engedélyezés).
   * **-p (vagy--protokoll)**. Protocol (Tcp, Udp vagy *) hello szabályhoz.
   * **-r (vagy--típus)**. Irányát (bejövő vagy kimenő) kapcsolat.
   * **-y (vagy--prioritás)**. Hello szabály prioritását.
   * **-f (vagy--forráscímelőtag)**. Forráscímelőtag CIDR vagy az alapértelmezett címkéket használ.
   * **-o (vagy--Forrásporttartomány)**. Forrásport, vagy porttartomány.
   * **-e (vagy--cél címelőtag)**. Célcím-előtag CIDR vagy az alapértelmezett címkéket használ.
   * **-u (vagy--Célporttartomány)**. Célport vagy porttartomány.
5. Futtassa a hello  **`azure network nsg rule create`**  parancs toocreate egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 80-as (HTTP) az hello Internet.
   
        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    Várt putput:
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK
6. Futtassa a hello  **`azure network nsg subnet add`**  parancs toolink hello NSG toohello előtérben lévő alhálózat alhálózat.
   
        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   
    Várt kimenet:
   
        info:    Executing command network nsg subnet add
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-toocreate-hello-nsg-for-hello-back-end-subnet"></a>Hogyan toocreate hello NSG hello vissza a záró alhálózat
az NSG nevű toocreate nevű *NSG-háttérrendszer* kövesse a fenti hello forgatókönyv alapján, hello alábbi lépéseket.

1. Futtassa a hello  **`azure network nsg create`**  parancs toocreate egy NSG.
   
        azure network nsg create -l uswest -n NSG-BackEnd
   
    Várt kimenet:
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up hello network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    Paraméterek:
   
   * **-l (vagy --location)**. Azure-régió, ahol hello új NSG létrejön. A mi esetünkben *westus*.
   * **-n (vagy --name)**. Hello nevét új NSG. A mi esetünkben *NSG-előtérbeli*.
2. Futtassa a hello  **`azure network nsg rule create`**  parancs toocreate egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 1433-as port (SQL) hello előtér alhálózatból.
   
        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    Várt kimenet:
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
3. Futtassa a hello  **`azure network nsg rule create`**  parancs toocreate egy szabályt, amely megtagadja a hozzáférést toohello Internet.
   
        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   
    Várt putput:
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK
4. Futtassa a hello  **`azure network nsg subnet add`**  toolink hello NSG toohello vissza end alhálózati parancsot.
   
        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
   
    Várt kimenet:
   
        info:    Executing command network nsg subnet add
        info:    Looking up hello network security group "NSG-BackEndX"
        info:    Looking up hello subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK

