---
title: "hello Azure CLI 1.0 használatával több IP-címmel aaaVM |} Microsoft Docs"
description: "Ismerje meg, hogyan tooassign több IP-címek tooa virtuális gép használt hello Azure CLI 1.0 |} Erőforrás-kezelő."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
ms.openlocfilehash: 83ad48e67309fb21d5aca967d4f2c01afdc0b5cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-azure-cli-10"></a>Több IP-cím használata az Azure CLI 1.0 toovirtual gépek hozzárendelése

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Ez a cikk azt ismerteti, hogyan toocreate keresztül hello Azure Resource Manager deployment model használatával egy virtuális gép (VM) hello Azure CLI 1.0. Több IP-cím hello klasszikus telepítési modell használatával létrehozott tooresources nem lehet hozzárendelni. További információk az Azure üzembe helyezési modellel, olvassa el a hello toolearn [üzembe helyezési modellel megértéséhez](../resource-manager-deployment-model.md) cikk.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Több IP-címekkel rendelkező virtuális gép létrehozása

Hajthatja végre ezt a feladatot az Azure CLI 1.0 (Ez a cikk) hello vagy hello [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md). hello következő lépések azt ismertetik, hogyan toocreate például VM több IP-címek, hello forgatókönyvben leírtak szerint. Módosítsa a változó nevét és IP cím típusát a végrehajtásához szükség szerint.

1. Telepítse és konfigurálja a következő hello által az Azure CLI 1.0 lépések hello a hello [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a következő cikket, és jelentkezzen be az Azure-fiókjával hello `azure-login` parancsot.

2. Hozzon létre egy erőforráscsoportot:
    
    ```azurecli
    RgName=myResourceGroup
    Location=westcentralus
    azure group create --name $RgName --location $Location
    ```
3. Virtuális hálózat létrehozása:

    ```azurecli
    azure network vnet create --resource-group $RgName --location $Location --name myVNet \
    --address-prefixes 10.0.0.0/16
    ```
4. A hello virtuális hálózati alhálózat létrehozása:

    ```azurecli
    azure network vnet subnet create --name mySubnet --resource-group $RgName --vnet-name myVNet \
    --address-prefix 10.0.0.0/24
    ```
    
5. Hozzon létre egy tárfiókot a virtuális gép hello. Hello futtatása előtt a következő parancsban cserélje le *mystorageaccount* egyedi névvel. hello nevének egyedinek kell lennie a Azure között:

    ```azurecli
    az storage account create --resource-group $RgName --location $Location --name mystorageaccount \
    --kind Storage --sku Standard_LRS
    ```

6. Hozzon létre hello IP-konfigurációk, egy hálózati Adaptert, és rendelje a hello IP konfigurációk toohello hálózati adaptert. Hozzáadása, eltávolítása vagy szükség szerint hello konfigurációjának módosításához. a következő konfigurációk hello hello forgatókönyvet ismerteti:

    **IPConfig-1**

    Adja meg a hello további parancsoknál, amelyek toocreate:

    - A nyilvános IP-cím erőforrás egy statikus nyilvános IP-cím
    - Egy hálózati Adaptert, hello nyilvános IP-cím és a statikus magánhálózati IP-cím tooit hozzárendelése.
    
    Cserélje le *mypublicdns* hello Azure-beli hely egyedi névvel.

      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP1 \
      --domain-name-label mypublicdns --allocation-method Static
        
      azure network nic create --resource-group $RgName --location $Location --name myNic1 \
      --private-ip-address 10.0.0.4 --subnet-name mySubnet --subnet-vnet-name myVNet \
      --subnet-name mySubnet --public-ip-name myPublicIP1
      ```

      > [!NOTE]
      > Nyilvános IP-címek névleges díjat kell. tudnivalók az IP-cím díjszabást toolearn olvasási hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap. Nincs előfizetés használható nyilvános IP-címek számának korlátozása toohello. További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.

    **IPConfig-2**

     Adja meg a következő parancsok toocreate hello, egy új nyilvános IP-cím erőforrás és egy új IP-konfiguráció egy statikus nyilvános IP-címet és egy statikus magánhálózati IP-cím:
    
      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP2
      --domain-name-label mypublicdns2 --allocation-method Static

      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --name IPConfig-2
      --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
      ```

    **IPConfig-3**

    Adja meg a következő parancsok toocreate hozzá statikus magánhálózati IP-cím és a nyilvános IP-cím az IP-konfigurációt hello:

      ```azurecli
      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --private-ip-address 10.0.0.6 \
      --name IPConfig-3
      ```

    >[!NOTE] 
    >Ez a cikk összes IP-konfigurációk tooa rendeli hozzá, ha egyetlen hálózati adapter, több IP-konfigurációk tooany egy virtuális hálózati adapter is hozzárendelhetők. toolearn hogyan toocreate virtuális gép több hálózati adaptert, és olvasási hello létrehozása a cikk több hálózati adapterrel rendelkező virtuális gépeknél.

7. Linux rendszerű virtuális gép létrehozása 

    ```azurecli
    az vm create --resource-group $RgName --name myVM1 --location $Location --nics myNic1 \
    --image UbuntuLTS --ssh-key-value ~/.ssh/id_rsa.pub --admin-username azureuser
    ```

    toochange hello mérete tooStandard DS2 v2 virtuális gép, például egyszerűen adja hozzá a következő tulajdonság hello `--vm-size Standard_DS3_v2` toohello `azure vm create` parancsot a 6.

8. Adja meg a következő parancs tooview hello NIC hello és hello társított IP-konfigurációk:

    ```azurecli
    azure network nic show --resource-group $RgName --name myNic1
    ```
9. Hozzáadás hello privát IP-címek toohello virtuális gép operációs rendszer hello lépések végrehajtásával, az operációs rendszernek a hello [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát.

## <a name="add"></a>Adja hozzá az IP-címek tooa méretű VM

Hozzáadhat további magán- és IP címeket tooan meglévő hálózati hello következő lépések végrehajtásával. hello példák épül hello [forgatókönyv](#Scenario) a cikkben.

1. Nyissa meg az Azure parancssori felület és a teljes hello hátralévő lépéseket ebben a szakaszban egy CLI-munkameneten belül. Ha még nem rendelkezik Azure CLI telepítése és konfigurálása, teljes hello hello szükséges lépések [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a következő cikket, és jelentkezzen be az Azure-fiókjával.

2. Végezze el a következő szakaszok a követelmények alapján hello egyikében hello lépéseket:

    - **Adja hozzá a magánhálózati IP-cím**
    
        egy privát IP-cím tooa NIC tooadd, létre kell hoznia hello parancsot az alábbi IP-konfigurációt. hello statikus cím hello alhálózat nem használt címnek kell lennie.

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 \
        --private-ip-address 10.0.0.7 --name IPConfig-4
        ```
        Tetszőleges számú konfigurációk összes kívánt beállítást, egyedi konfigurációs nevek és privát IP-címek használata (a statikus IP-címekkel rendelkező konfigurációk) létrehozása.

    - **A nyilvános IP-cím hozzáadása**
    
        A nyilvános IP-cím való társítás tooeither által hozzáadott új IP-konfiguráció vagy a meglévő IP-konfigurációt. Hajtsa végre hello egyik hello szakaszokban szereplő, az összes kívánt beállítást.

        > [!NOTE]
        > Nyilvános IP-címek névleges díjat kell. tudnivalók az IP-cím díjszabást toolearn olvasási hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap. Nincs előfizetés használható nyilvános IP-címek számának korlátozása toohello. További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.
        >

        **Hello erőforrás tooa új IP-konfiguráció hozzárendelése**
    
        Egy nyilvános IP-címet ad hozzá egy új IP-konfiguráció, amikor is hozzá kell magánhálózati IP-cím, mert az összes IP-konfiguráció magánhálózati IP-címnek kell rendelkeznie. Egy meglévő nyilvános IP-cím erőforrás hozzáadása, vagy hozzon létre egy újat. toocreate egy új, adja meg a következő parancs hello:

        ```azurecli
        azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 \
        --domain-name-label mypublicdns3
        ```

        egy új IP-beállítását egy statikus magánhálózati IP-cím és a kapcsolódó hello toocreate *myPublicIP3* nyilvános IP-cím erőforrás címet, adja meg a következő parancs hello:

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 \
        --private-ip-address 10.0.0.8 --public-ip-name myPublicIP3
        ```

        **Hello erőforrás tooan meglévő IP-konfiguráció hozzárendelése**

        A nyilvános IP-cím erőforrás csak lehet társított tooan IP-konfigurációja, amely még nincs társítva. Segítségével meghatározhatja, hogy rendelkezik-e IP-konfigurációt tartozó nyilvános IP-cím hello a következő parancs beírásával:

        ```azurecli
        azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
        ```

        Egy sor hasonló toohello egy adott vissza a kimeneti hello az IPConfig-3 következő keresése:

        ```         
        Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet
        ```
          
        Hello óta **nyilvános IP-cím** oszlopában *IpConfig-3* van üres, nem nyilvános IP-cím erőforrás jelenleg társított tooit. Adja hozzá egy meglévő nyilvános IP cím erőforrás tooIpConfig-3, vagy adja meg a következő parancs toocreate egy hello:

        ```azurecli
        azure network public-ip create --resource-group  myResourceGroup --location westcentralus \
        --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
        ```

        Adja meg a következő parancs tooassociate hello nyilvános IP-cím erőforrás toohello meglévő IP-konfiguráció hello *IPConfig-3*:
        ```azurecli
        azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 \
        --public-ip-name myPublicIP3
        ```

3. Hello magánhálózati IP-címek megtekintése és hello nyilvános IP cím erőforrások hozzárendelt toohello megadásával NIC hello a következő parancsot:

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

      hello visszaadott kimeneti hasonló toohello következő:
      ```
      Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        
      default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
      IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
      IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet  myPublicIP3
      ```
4. Hello toohello NIC toohello virtuális gép operációs rendszer hello hello utasításait követve hozzáadott magánhálózati IP-címek hozzáadása [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát. Hello nyilvános IP-címek toohello operációs rendszere nem adja hozzá.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
