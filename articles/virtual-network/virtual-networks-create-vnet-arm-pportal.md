---
title: "egy Azure virtuális hálózatra, több alhálózattal aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate, több alhálózattal az Azure virtuális hálózat."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 0f56fa6ac24537d33b8e217f5b03f387826ab487
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-with-multiple-subnets"></a>Hozzon létre egy virtuális hálózatot, több alhálózattal

Ebből az oktatóanyagból megtudhatja, hogyan toocreate egy alapszintű Azure virtuális hálózat, amely rendelkezik külön nyilvános és titkos alhálózat. Az alhálózat Azure-erőforrások, például a virtuális gépek, az App Service-környezetek, a virtuálisgép-méretezési csoportok, a Azure HDInsight és a felhőszolgáltatások is létrehozhat. A virtuális hálózatokon, erőforrások kommunikálhatnak egymással, és az egyéb hálózatok csatlakoztatott tooa virtuális hálózatán lévő erőforrásokat.

hello következő szakaszok az alábbiakat menetéről, hogy toocreate virtuális hálózat hello segítségével [Azure-portálon](#portal), hello Azure parancssori felület ([Azure CLI](#azure-cli)), [Azure PowerShell ](#powershell), és egy [Azure Resource Manager sablon](#resource-manager-template). hello eredménye hello azonos, függetlenül attól, milyen eszköz toocreate hello virtuális hálózat használata. Kattintson az eszköz hivatkozásra toogo toothat szakasz hello oktatóanyag. További információk [virtuális hálózati](virtual-network-manage-network.md) és [alhálózati](virtual-network-manage-subnet.md) beállításait.

A cikkben lépéseket toocreate hello Resource Manager telepítési modell, amely hello üzembe helyezési modellel, azt javasoljuk, amikor új virtuális hálózatok létrehozása egy virtuális hálózatot. Ha egy virtuális hálózat (klasszikus) toocreate van szüksége, tekintse meg [hozzon létre egy virtuális hálózatot (klasszikus)](create-virtual-network-classic.md). Ha nem ismeri az Azure üzembe helyezési modellel, lásd: [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="portal"></a>Azure-portálon

1. Egy webböngészőben nyissa meg toohello [Azure-portálon](https://portal.azure.com). Jelentkezzen be a [Azure-fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Ha az Azure-fiók nem rendelkezik, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Hello portálon kattintson **+ új** > **hálózati** > **virtuális hálózati**.
3. A hello **virtuális hálózat létrehozása** panelen adja meg a következő értékek hello, és kattintson a **létrehozása**:

    |Beállítás|Érték|
    |---|---|
    |Név|myVNet|
    |Címtér|10.0.0.0/16|
    |Alhálózat neve|Nyilvános|
    |Alhálózati címtartomány|10.0.0.0/24|
    |Erőforráscsoport|Hagyja **hozzon létre új** kiválasztva, és írja be **myResourceGroup**.|
    |Egyes előfizetésekhez és helyekhez|Válassza ki az egyes előfizetésekhez és helyekhez.

    Ha új tooAzure, további információ [erőforráscsoportok](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [előfizetések](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), és [helyek](https://azure.microsoft.com/regions) (más néven tooas *régiók*).
4. Hello portálon csak egy alhálózat virtuális hálózat létrehozásakor is létrehozhat. Ebben az oktatóanyagban létrehoz egy második alhálózat hello virtuális hálózat létrehozása után. Később létrehozhat az internetről elérhető erőforrások a hello **nyilvános** alhálózat. Is létrehozhat, amelyek nem érhetők el az Internet hello hello az erőforrások **titkos** alhálózat. toocreate hello második alhálózat, a hello **keresési erőforrások** hello oldal hello tetején mezőbe írja be **myVnet**. Hello keresési eredmények között kattintson **myVnet**. Ha több virtuális hálózatokat és a hello ugyanezen a néven: az előfizetéshez, jelölje be minden egyes virtuális hálózati szereplő hello erőforráscsoportok. Győződjön meg arról, hogy hello kattintson **myVnet** keresési eredmények hello erőforráscsoport rendelkező **myResourceGroup**.
5. A hello **myVnet** panel alatt **beállítások**, kattintson a **alhálózatok**.
6. A hello **myVnet - alhálózatok** panelen kattintson a **+ alhálózati**.
7. A hello **alhálózat hozzáadása** panelen a **neve**, adja meg **titkos**. A **-címtartományt**, adja meg **10.0.1.0/24**.  Kattintson az **OK** gombra.
8. A hello **myVnet - alhálózatok** panelt, tekintse át hello alhálózatok. Megtekintheti a hello **nyilvános** és **titkos** létrehozott alhálózatokat.
9. **Választható lehetőség:** toodelete hello-erőforrások ebben az oktatóanyagban, teljes hello szükséges lépések [törli az erőforrást](#delete-portal) ebben a cikkben.

## <a name="azure-cli"></a>Azure CLI

Az Azure parancssori felület parancsait hello azonos, hogy hello parancsok Windows, Linux vagy macOS hajtható végre. Vannak azonban parancsfájlok különbségei operációs rendszer ismertetése. az alábbi lépésekkel hello hello parancsfájl a rendszerhéjakba hajtja végre. 

1. [Telepítse és konfigurálja az Azure parancssori felület hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy a legújabb verziójának hello hello Azure parancssori felület telepítve. Írja be a parancssori felület parancsait tooget súgóját `az <command> --help`. Ahelyett, hogy a parancssori felület telepítése hello és a szükséges előfeltételek hello Azure Cloud rendszerhéj is használhatja. hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül. hello felhő rendszerhéj hello Azure CLI előtelepített és konfigurált toouse-fiókjához tartozik. toouse hello felhő rendszerhéj, kattintson a hello felhő rendszerhéj (**> _**) gomb hello hello tetején [portal](https://portal.azure.com) vagy hello kattintson *kipróbálás* hello lépések gombjára. 
2. Ha hello CLI helyben fut, jelentkezzen be a hello tooAzure `az login` parancsot. Hello felhő rendszerhéj használata, ha már jelentkezett be.
3. Hello tekintse át a következő parancsfájl és észrevételeit. A böngészőben hello parancsfájl másolja, majd illessze be a CLI-munkamenethez:

    ```azurecli-interactive
    #!/bin/bash
    
    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus
    
    # Create a virtual network with one subnet named Public.
    az network vnet create \
      --name myVnet \
      --resource-group myResourceGroup \
      --subnet-name Public
    
    # Create an additional subnet named Private in hello virtual network.
    az network vnet subnet create \
      --name Private \
      --address-prefix 10.0.1.0/24 \
      --vnet-name myVnet \
      --resource-group myResourceGroup
    ```
    
4. Miután befejezte hello parancsfájlt futtat, tekintse át a hello alhálózatok hello virtuális hálózat. A következő parancs hello másolja, és illessze be a CLI-munkamenethez:

    ```azurecli
    az network vnet subnet list --resource-group myResourceGroup --vnet-name myVnet --output table
    ```

5. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések [törli az erőforrást](#delete-cli) ebben a cikkben.

## <a name="powershell"></a>PowerShell

1. Hello hello PowerShell legújabb verziójának telepítéséhez [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Ha új tooAzure PowerShell, lásd: [Azure PowerShell áttekintése](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. A PowerShell-munkamenetben jelentkezzen be a tooAzure a [Azure-fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) hello segítségével `login-azurermaccount` parancsot.

3. Hello tekintse át a következő parancsfájl és észrevételeit. A böngészőben hello parancsfájl másolja és illessze be a PowerShell-munkamenethez:

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name myResourceGroup `
      -Location eastus
    
    # Create hello public and private subnets.
    $Subnet1 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Public `
      -AddressPrefix 10.0.0.0/24
    $Subnet2 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Private `
      -AddressPrefix 10.0.1.0/24
    
    # Create a virtual network.
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Location eastus `
      -Name myVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet1,$Subnet2
    ```

4. tooreview hello alhálózatok hello virtuális hálózat, a következő parancs hello másolja, és illessze be a PowerShell-munkamenethez:

    ```powershell
    $Vnet.subnets | Format-Table Name, AddressPrefix
    ```

5. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések [törli az erőforrást](#delete-powershell) ebben a cikkben.

## <a name="resource-manager-template"></a>Resource Manager-sablon

Egy virtuális hálózatot az Azure Resource Manager-sablon segítségével telepíthet. További információ a sablonok, toolearn lásd [Mi az erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#template-deployment). tooaccess hello sablon és a paraméterek toolearn: hello [hozzon létre egy virtuális hálózatot, két alhálózattal](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) sablont. Hello sablon hello segítségével telepíthet [portal](#template-portal), [Azure CLI](#template-cli), vagy [PowerShell](#template-powershell).

**Választható lehetőség:** toodelete hello-erőforrások ebben az oktatóanyagban, bármely alszakaszaiban lépései teljes hello [törli az erőforrást](#delete) ebben a cikkben.

### <a name="template-portal"></a>Azure-portálon

1. A böngészőben nyissa meg a hello [sablonlap](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets).
2. Kattintson a hello **tooAzure telepítése** gombra. Ha még nem jelentkezett be tooAzure, jelentkezzen be a hello Azure portál bejelentkezési képernyő, amely akkor jelenik meg.
3. A toohello portál használatával írja alá a [Azure-fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Ha az Azure-fiók nem rendelkezik, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Adja meg a következő hello paraméterek értékeit hello:

    |Paraméter|Érték|
    |---|---|
    |Előfizetés|Jelölje ki az előfizetését|
    |Erőforráscsoport|myResourceGroup|
    |Hely|Válasszon ki egy helyet|
    |Vnet neve|myVNet|
    |Virtuális hálózat címelőtag|10.0.0.0/16|
    |Subnet1Prefix|10.0.0.0/24|
    |Subnet1Name|Nyilvános|
    |Subnet2Prefix|10.0.1.0/24|
    |Subnet2Name|Saját|

5. Fogadja el a toohello feltételeket és kikötéseket, és kattintson a **beszerzési** toodeploy hello virtuális hálózat.

### <a name="template-cli"></a>Az Azure parancssori felület

1. [Telepítse és konfigurálja az Azure parancssori felület hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ellenőrizze, hogy a legújabb verziójának hello hello Azure parancssori felület telepítve. Írja be a parancssori felület parancsait tooget súgóját `az <command> --help`. Ahelyett, hogy a parancssori felület telepítése hello és a szükséges előfeltételek hello Azure Cloud rendszerhéj is használhatja. hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül. hello felhő rendszerhéj hello Azure CLI előtelepített és konfigurált toouse-fiókjához tartozik. toouse hello felhő rendszerhéj, kattintson a felhő rendszerhéj hello **> _** hello hello tetején gomb [portal](https://portal.azure.com), vagy egyszerűen kattintson a hello **próbálja ki** hello lépések gombjára. 
2. Ha hello CLI helyben fut, jelentkezzen be a hello tooAzure `az login` parancsot. Hello felhő rendszerhéj használata, ha már jelentkezett be.
3. toocreate hello virtuális hálózat erőforráscsoport, a Másolás hello következő parancsot, és illessze be a CLI-munkamenethez:

    ```azurecli-interactive
    az group create --name myResourceGroup --location eastus
    ```
    
4. Az alábbi paraméterek beállítások hello hello sablon telepítheti:
    - **Alapértelmezett paraméterértékek**. Adja meg a következő parancs hello:
    
        ```azurecli-interactive
        az group deployment create --resource-group myResourceGroup --name VnetTutorial --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json`
        ```
    - **Egyéni paraméterértékek**. Töltse le és hello sablon módosítására hello sablon telepítése előtt. Is telepíthet hello sablon hello parancssori paraméterek használatával, vagy egy külön paraméterek fájllal hello sablon üzembe helyezése. toodownload hello sablon és a paraméterek fájlokat, kattintson a hello **keresse meg a Githubon** hello gombjára [hozzon létre egy virtuális hálózatot, két alhálózattal](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) mintasablon lapot. A github webhelyen, kattintson a hello **azuredeploy.parameters.json** vagy **azuredeploy.json** fájlt. Kattintson a hello **Raw** gomb toodisplay hello fájlt. A böngészőben hello fájl hello tartalom másolása. Mentse a hello tartalma tooa fájlt a számítógépen. Módosíthatja a hello paraméterértékek hello sablonban, vagy egy külön paraméterek fájllal hello sablon üzembe helyezése.  

    További részletek toolearn, hogyan toodeploy sablonok e módszerekkel, írja be `az group deployment create --help`.

### <a name="template-powershell"></a>PowerShell

1. Hello hello PowerShell legújabb verziójának telepítéséhez [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Ha új tooAzure PowerShell, lásd: [Azure PowerShell áttekintése](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. A PowerShell-munkamenetben, azzal toosign a [Azure-fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account), adja meg `login-azurermaccount`.
3. toocreate erőforráscsoport hello virtuális hálózat, adja meg a következő parancs hello:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
    ```
    
4. Az alábbi paraméterek beállítások hello hello sablon telepítheti:
    - **Alapértelmezett paraméterértékek**. Adja meg a következő parancs hello:
    
        ```powershell
        New-AzureRmResourceGroupDeployment -Name VnetTutorial -ResourceGroupName myResourceGroup -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json
        ```
        
    - **Egyéni paraméterértékek**. Töltse le és hello sablon módosítására telepítése előtt. Is telepíthet hello sablon hello parancssori paraméterek használatával, vagy egy külön paraméterek fájllal hello sablon üzembe helyezése. toodownload hello sablon és a paraméterek fájlokat, kattintson a hello **keresse meg a Githubon** hello gombjára [hozzon létre egy virtuális hálózatot, két alhálózattal](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) mintasablon lapot. A github webhelyen, kattintson a hello **azuredeploy.parameters.json** vagy **azuredeploy.json** fájlt. Kattintson a hello **Raw** gomb toodisplay hello fájlt. A böngészőben hello fájl hello tartalom másolása. Mentse a hello tartalma tooa fájlt a számítógépen. Módosíthatja a hello paraméterértékek hello sablonban, vagy egy külön paraméterek fájllal hello sablon üzembe helyezése.  

    További részletek toolearn, hogyan toodeploy sablonok e módszerekkel, írja be `Get-Help New-AzureRmResourceGroupDeployment`. 

## <a name="delete"></a>Erőforrások törlése

Ez az oktatóanyag befejezése után érdemes lehet toodelete hello erőforrások létrehozott, így használati költségek. Erőforráscsoport törlésekor a hello erőforráscsoportban található összes erőforrást is törlődnek.

### <a name="delete-portal"></a>Azure-portálon

1. Hello portál keresési mezőbe, írja be a **myResourceGroup**. Hello keresési eredmények között kattintson **myResourceGroup**.
2. A hello **myResourceGroup** panelen kattintson hello **törlése** ikonra.
3. tooconfirm hello törlésre, hello **típus hello ERŐFORRÁSCSOPORT-név** adja meg a **myResourceGroup**, és kattintson a **törlése**.

### <a name="delete-cli"></a>Az Azure parancssori felület

Adja meg egy parancssori munkamenetet hello a következő parancsot:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

Adja meg a következő parancs hello egy PowerShell-munkamenetben:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Következő lépések

- minden virtuális hálózati és alhálózati beállítások toolearn lásd: [virtuális hálózatok kezeléséhez](virtual-network-manage-network.md#view-vnet) és [kezelheti a virtuális hálózati alhálózat](virtual-network-manage-subnet.md#create-subnet). Lehetősége van különböző virtuális hálózatok és alhálózatok használatát mutatja be egy éles környezetben toomeet eltérő követelmények vonatkoznak.
- toofilter bejövő és kimenő forgalmat, létrehozása és alkalmazása [hálózati biztonsági csoportok](virtual-networks-nsg.md) toosubnets.
- Hozzon létre egy [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy egy [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gépet, és csatlakoztassa tooan már meglévő virtuális hálózatot.
- tooconnect két virtuális hálózatok a hello Azure ugyanazon a helyen, és hozzon létre egy [virtuális hálózati társviszony-létesítés](virtual-network-peering-overview.md) hello virtuális hálózatok között.
- Hello virtuális hálózat tooan a helyszíni hálózati használatával kapcsolódnak a [VPN-átjáró](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) körön.
