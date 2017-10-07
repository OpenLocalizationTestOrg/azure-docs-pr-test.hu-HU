---
title: "aaaCreate egy Azure virtuális hálózati társviszony - különböző üzembe helyezési modellel - ugyanahhoz az előfizetéshez |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate különböző Azure üzembe helyezési modellel, hello meglévő azonos virtuális hálózati társviszony-létesítés virtuális hálózatok közötti létrehozott Azure-előfizetés."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: jdial;narayan;annahar
ms.openlocfilehash: 365156d651c9042ed52baeb15bf629fcc5329af8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---different-deployment-models-same-subscription"></a>Hozzon létre egy virtuális hálózati társviszony - különböző üzembe helyezési modellel, ugyanahhoz az előfizetéshez 

Ebben az oktatóanyagban elsajátíthatja toocreate egy virtuális hálózati társviszony-létesítés különböző üzembe helyezési modellel létrehozott virtuális hálózatok között. Mindkét virtuális hálózat szerepel hello azonos előfizetéssel. Társviszony-létesítési két virtuális hálózatok lehetővé teszi, hogy erőforrások egymással a különböző virtuális hálózatokon toocommunicate hello azonos sávszélesség és a késés, mintha hello erőforrások szerepeltek hello ugyanazt a virtuális hálózatot. További információ [virtuális hálózati társviszony-létesítés](virtual-network-peering-overview.md). 

hello lépéseket toocreate virtuális hálózati társviszony-létesítés eltérőek, attól függően, hogy hello virtuális hálózatok vannak-e hello azonos vagy eltérő, előfizetések, és amely [Azure telepítési modell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuális hálózatok jönnek létre. keresztül. Megtudhatja, hogyan toocreate a virtuális hálózati társviszony más esetekben hello forgatókönyv a következő táblázat hello kattintva:

|Azure üzembehelyezési modell  | Azure-előfizetés  |
|--------- |---------|
|[Mindkét erőforrás-kezelő](virtual-network-create-peering.md) |Azonos|
|[Mindkét erőforrás-kezelő](create-peering-different-subscriptions.md) |Különböző|
|[Egy erőforrás-kezelő egy klasszikus](create-peering-different-deployment-models-subscriptions.md) |Különböző|

Virtuális hálózati társviszony-létesítés nem hozható létre, hello klasszikus üzembe helyezési modellben telepített virtuális hálózatok között. Virtuális hálózati társviszony-létesítés csak hozhatók létre, amely szerepel hello azonos virtuális hálózatok között Azure-régiót. Ha tooconnect virtuális hálózatokat egyaránt létrehozott hello klasszikus telepítési modell használatával, vagy, amely létezik a különböző Azure-régiók van szüksége, használhatja az Azure [VPN-átjáró](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuális hálózatok. 

Használhatja a hello [Azure-portálon](#portal), hello Azure [parancssori felület](#cli) (CLI), vagy Azure [PowerShell](#powershell) toocreate virtuális hálózati társviszony-létesítés. Kattintson bármelyik hello előző eszköz hivatkozások toogo, közvetlen toohello lépések létrehozása a virtuális hálózati társviszony-létesítés a kiválasztott eszköz segítségével.

## <a name="cli"></a>Hozzon létre a társviszony - portál

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com). hello fiókkal jelentkezik be az hello szükséges engedélyek toocreate rendelkeznie kell egy virtuális hálózati társviszony-létesítés. Lásd: hello [engedélyek](#permissions) jelen cikkben alább szakasza.
2. Kattintson a **+ új**, kattintson a **hálózati**, majd kattintson a **virtuális hálózati**.
3. A hello **virtuális hálózat létrehozása** panelen adja meg, vagy válassza ki a következő beállítások hello értékeit, majd kattintson **létrehozása**:
    - **Név**: *myVnet1*
    - **Címtér**: *10.0.0.0/16*
    - **Alhálózati név**: *alapértelmezett*
    - **Alhálózati címtartományt**: *10.0.0.0/24*
    - **Előfizetés**: Jelölje ki az előfizetését
    - **Erőforráscsoport**: válasszon **hozzon létre új** , és írja be *myResourceGroup*
    - **Hely**: *USA keleti régiója*
4. Kattintson a **+ új**. A hello **keresési hello piactér** mezőbe írja be *virtuális hálózati*. Kattintson a **virtuális hálózati** amikor megjelenik a hello keresési eredmények között. 
5. A hello **virtuális hálózati** panelen válassza **klasszikus** a hello **telepítési modell kiválasztása** gombra, majd **létrehozása**.
6. A hello **virtuális hálózat létrehozása** panelen adja meg, vagy válassza ki a következő beállítások hello értékeit, majd kattintson **létrehozása**:
    - **Név**: *myVnet2*
    - **Címtér**: *10.1.0.0/16*
    - **Alhálózati név**: *alapértelmezett*
    - **Alhálózati címtartományt**: *10.1.0.0/24*
    - **Előfizetés**: Jelölje ki az előfizetését
    - **Erőforráscsoport**: válasszon **meglévő** válassza *myResourceGroup*
    - **Hely**: *USA keleti régiója*
7. A hello **keresési erőforrások** mezőt hello portálon típus hello tetején *myResourceGroup*. Kattintson a **myResourceGroup** amikor megjelenik a hello keresési eredmények között. A panel jelenik meg hello **myresourcegroup** erőforráscsoportot. hello erőforráscsoport hello két létrehozott virtuális hálózatokat az előző lépéseket tartalmazza.
8. Kattintson a **myVNet1**.
9. A hello **myVnet1** panel, amelyen megjelenik, kattintson a **Társviszony** hello függőleges bal oldalán található hello panel hello beállítások listája.
10. A hello **myVnet1 - esetében** panel, amelyen jelent meg, kattintson a **+ Hozzáadás**
11. A hello **Hozzáadás társviszony-létesítés** panelen megjelenő, adja meg, vagy válassza ki az alábbi beállítások hello, majd kattintson **OK**:
     - **Név**: *myVnet1ToMyVnet2*
     - **Virtuális hálózat telepítési modell**: válasszon **klasszikus**. 
     - **Előfizetés**: Jelölje ki az előfizetését
     - **Virtuális hálózati**: kattintson a **virtuális hálózatot választ**, majd kattintson a **myVnet2**.
     - **Virtuális hálózati hozzáférés engedélyezése:** ügyeljen arra, hogy **engedélyezve** van kiválasztva.
    Ebben az oktatóanyagban nincs más beállítások használhatók. toolearn összes társviszony-létesítési beállításról, olvasni [kezelheti a virtuális hálózati társviszony](virtual-network-manage-peering.md#create-a-peering).
12. Kattintás után **OK** hello a korábbi lépésben hello **Hozzáadás társviszony-létesítés** panel bezárása után, és látni hello **myVnet1 - Társviszony** újra a panelt. Néhány másodpercen belül létrehozott társviszony hello hello panel jelenik meg. **Csatlakoztatott** hello szerepel **társviszony-LÉTESÍTÉS állapot** hello oszlopában **myVnet1ToMyVnet2** társviszony-létesítést, létre.

    hello társviszony-létesítés most létrejön. Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával. Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti. Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló. Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
13. **Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.
14. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések hello [törli az erőforrást](#delete-portal) című szakaszát.

## <a name="cli"></a>Társviszony - létrehozása az Azure parancssori felület

1. [Telepítés](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello Azure CLI 1.0 toocreate hello virtuális hálózat (klasszikus).
2. Nyisson meg egy parancsablakot, és jelentkezzen be hello segítségével tooAzure `azure login` parancsot.
3. Szolgáltatásfelügyelet módban hello CLI futtatásában hello `azure config mode asm` parancsot.
4. Adja meg a következő parancs toocreate hello virtuális hálózat (klasszikus) hello:
 
    ```azurecli
    azure network vnet create --vnet myVnet2 --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```

5. Hozzon létre egy erőforráscsoportot és egy virtuális hálózatot (Resource Manager). Hello CLI 1.0 vagy 2.0-s is használhatja ([telepítése](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)). Ebben az oktatóanyagban hello CLI 2.0 használt toocreate hello virtuális hálózat (erőforrás-kezelő) azért, mert 2.0 használt toocreate hello társviszony-létesítés kell lennie. Végrehajtás hello következő bash parancssori felület parancsfájlt a helyi gép hello CLI 2.0.4 vagy újabb verziója. Fut a beállítások a Windows-ügyfelén CLI parancsfájlok bash, lásd: [a Windows hello Azure CLI-t futtató](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Hello parancsfájl hello Azure Cloud rendszerhéj segítségével is futtathatja. hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül. Hello Azure CLI előtelepített és konfigurált toouse-fiókjához van. Kattintson a hello **kipróbálás** hello parancsfájl, amely a következő, a felhő rendszerhéjat, amelyre bejelentkezik, amellyel bejelentkezhet tooyour gombra az Azure-fiók. tooexecute hello parancsfájl, kattintson a hello **másolási** gombra és a beillesztési műveleteket, hello tartalmát a felhő rendszerhéjat, majd nyomja le az `Enter`.

    ```azurecli-interactive
    #!/bin/bash

    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus

    # Create hello virtual network (Resource Manager).
    az network vnet create \
      --name myVnet1 \
      --resource-group myResourceGroup \
      --location eastus \
      --address-prefix 10.0.0.0/16
    ```

6. Hozzon létre egy virtuális hálózati társviszony-létesítés hello két virtuális hálózatok hello különböző üzembe helyezési modellel létrehozott között. Másolás hello alábbi parancsfájl tooa szövegszerkesztőben a számítógépen. Cserélje le `<subscription id>` az előfizetés azonosítóját. Ha az előfizetés-azonosítója nem ismeri, adja meg a hello `az account show` parancsot. a következő hello **azonosító** hello kimeneti az előfizetés azonosítóját. Illessze be a hello módosító parancsfájlt tooyour CLI munkamenetben, és nyomja le az `Enter`.

    ```azurecli-interactive
    # Get hello id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group myResourceGroup \
      --name myVnet1 \
      --query id --out tsv)

    # Peer VNet1 tooVNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --remote-vnet-id /subscriptions/<subscription id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2 \
      --allow-vnet-access
    ```
7. Miután hello parancsprogram végrehajtása során, tekintse át a hello társviszony-létesítés hello virtuális hálózat (Resource Manager). Másolás hello következő parancsot, illessze be a CLI-munkamenetben, és nyomja le az `Enter`:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    hello kimeneti jelennek meg **csatlakoztatva** a hello **PeeringState** oszlop. 

    Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával. Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti. Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló. Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
8. **Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.
9. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések [törli az erőforrást](#delete-cli) ebben a cikkben.

## <a name="powershell"></a>Hozzon létre a társviszony - PowerShell

1. Hello hello PowerShell legújabb verziójának telepítéséhez [Azure](https://www.powershellgallery.com/packages/Azure) és [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulok. Ha új tooAzure PowerShell, lásd: [Azure PowerShell áttekintése](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Indítson el egy PowerShell-munkamenetet.
3. A PowerShellben, jelentkezzen be tooAzure hello megadásával `Add-AzureAccount` parancsot.
4. toocreate egy virtuális hálózat (klasszikus) a PowerShell-lel, létre kell hoznia egy új, vagy módosíthatja egy meglévő hálózati konfigurációs fájlban. Ismerje meg, hogyan túl[exportálása, frissítése és a hálózati konfigurációs fájlok importálása a](virtual-networks-using-network-configuration-file.md). hello fájlt tartalmaznia kell következő hello **VirtualNetworkSite** elem hello ebben az oktatóanyagban használt virtuális hálózathoz:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
      <AddressSpace>
        <AddressPrefix>10.1.0.0/16</AddressPrefix>
      </AddressSpace>
      <Subnets>
        <Subnet name="default">
          <AddressPrefix>10.1.0.0/24</AddressPrefix>
        </Subnet>
      </Subnets>
    </VirtualNetworkSite>
    ```

    > [!WARNING]
    > Megváltozott hálózati konfigurációs fájlok importálása okozhat módosítások tooexisting virtuális hálózatok (klasszikus), az előfizetésben. Győződjön meg arról, csak akkor hozzá hello korábbi virtuális hálózaton, és módosítsa vagy távolítsa el a meglévő virtuális hálózatok az előfizetésből nem. 
5. Jelentkezzen be tooAzure toocreate hello virtuális hálózat (erőforrás-kezelő) hello megadásával `login-azurermaccount` parancsot. hello fiókkal jelentkezik be az hello szükséges engedélyek toocreate rendelkeznie kell egy virtuális hálózati társviszony-létesítés. Lásd: hello [engedélyek](#permissions) jelen cikkben alább szakasza.
6. Hozzon létre egy erőforráscsoportot és egy virtuális hálózatot (Resource Manager). Hello parancsfájl másolása, illessze be a PowerShell és nyomja le az `Enter`.

    ```powershell
    # Create a resource group.
      New-AzureRmResourceGroup -Name myResourceGroup -Location eastus

    # Create hello virtual network (Resource Manager).
      $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus
    ```

7. Hozzon létre egy virtuális hálózati társviszony-létesítés hello két virtuális hálózatok hello különböző üzembe helyezési modellel létrehozott között. Másolás hello alábbi parancsfájl tooa szövegszerkesztőben a számítógépen. Cserélje le `<subscription id>` az előfizetés azonosítóját. Ha az előfizetés-azonosítója nem ismeri, adja meg a hello `Get-AzureRmSubscription` parancs tooview azt. a következő hello **azonosító** hello vissza a kimeneti az előfizetés-azonosító. tooexecute hello parancsfájl, a Másolás hello módosítva a parancsfájlt a szövegszerkesztőben, majd kattintson a jobb gombbal a PowerShell-munkamenetben, és nyomja le az `Enter`.

    ```powershell
    # Peer VNet1 tooVNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name myVnet1ToMyVnet2 `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId /subscriptions/<subscription Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2
    ```

8. Miután hello parancsprogram végrehajtása során, tekintse át a hello társviszony-létesítés hello virtuális hálózat (Resource Manager). Másolás hello következő parancsot, illessze be a PowerShell-munkamenethez, és nyomja le az `Enter`:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    hello kimeneti jelennek meg **csatlakoztatva** a hello **PeeringState** oszlop.

    Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával. Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti. Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló. Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

9. **Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.
10. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések [törli az erőforrást](#delete-powershell) ebben a cikkben.
 
## <a name="permissions"></a>Engedélyek

hello fiókjai virtuális hálózati társviszony-létesítés toocreate hello szükséges szerepkör- vagy hozzáférési engedélyekkel kell rendelkeznie. Például, ha két virtuális hálózatok myVnet1 és myVnet2 nevű volt társviszony, a fiókjához társítva kell lenni a következő minimális szerepkör vagy minden egyes virtuális hálózati engedélyeinek hello:
    
|Virtuális hálózat|Üzemi modell|Szerepkör|Engedélyek|
|---|---|---|---|
|myVnet1|Resource Manager|[Hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klasszikus|[Klasszikus hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|N/A|
|myVnet2|Resource Manager|[Hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klasszikus|[Klasszikus hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

További információ [beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) és konkrét engedélyeket túl[egyéni szerepkörök](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (csak Resource Manager).

## <a name="delete"></a>Erőforrások törlése
Ez az oktatóanyag befejezése után, érdemes lehet toodelete hello erőforrások hello oktatóanyag, így a használati költségek létrehozott. Erőforráscsoport törlésekor a hello erőforráscsoportban található összes erőforrást is törlődnek.

### <a name="delete-portal"></a>Azure-portálon

1. Hello portál keresési mezőbe, írja be a **myResourceGroup**. Hello keresési eredmények között kattintson **myResourceGroup**.
2. A hello **myResourceGroup** panelen kattintson hello **törlése** ikonra.
3. tooconfirm hello törlésre, hello **típus hello ERŐFORRÁSCSOPORT-név** adja meg a **myResourceGroup**, és kattintson a **törlése**.

### <a name="delete-cli"></a>Az Azure parancssori felület

1. Hello Azure CLI 2.0 toodelete hello virtuális hálózat (erőforrás-kezelő) használata a hello a következő parancsot:

    ```azurecli-interactive
    az group delete --name myResourceGroup --yes
    ```

2. A következő parancsok hello hello Azure CLI 1.0 toodelete hello virtuális hálózat (klasszikus) használata:

    ```azurecli
    azure config mode asm

    azure network vnet delete --vnet myVnet2 --quiet
    ```

### <a name="delete-powershell"></a>PowerShell

1. Adja meg a következő parancs toodelete hello virtuális hálózat (erőforrás-kezelő) hello:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroup -Force
    ```

2. toodelete hello virtuális hálózat (klasszikus) a PowerShell-lel, módosítania kell a hálózati konfigurációs fájlok. Ismerje meg, hogyan túl[exportálása, frissítése és a hálózati konfigurációs fájlok importálása a](virtual-networks-using-network-configuration-file.md). Távolítsa el a következő VirtualNetworkSite elem ebben az oktatóanyagban használt virtuális hálózat hello hello:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
      <AddressSpace>
        <AddressPrefix>10.1.0.0/16</AddressPrefix>
      </AddressSpace>
      <Subnets>
        <Subnet name="default">
          <AddressPrefix>10.1.0.0/24</AddressPrefix>
        </Subnet>
      </Subnets>
    </VirtualNetworkSite>
    ```

    > [!WARNING]
    > Megváltozott hálózati konfigurációs fájlok importálása okozhat módosítások tooexisting virtuális hálózatok (klasszikus), az előfizetésben. Győződjön meg arról, csak eltávolítása hello korábbi virtuális hálózaton, és nem módosítja, illetve bármely más meglévő virtuális hálózatot eltávolítása az előfizetésből. 

## <a name="next-steps"></a>Következő lépések

- Alaposan olvassa el a fontos [virtuális hálózati társviszony-létesítési korlátozások és viselkedéshez](virtual-network-manage-peering.md#requirements-and-constraints) társviszony-létesítés üzemi virtuális hálózat létrehozása előtt használja.
- További tudnivalók az összes [virtuális hálózati társviszony-létesítési beállítások](virtual-network-manage-peering.md#create-a-peering).
- Ismerje meg, hogyan túl[létrehoz egy központot, és a hálózati topológia irány](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) a virtuális hálózati társviszony-létesítés.
