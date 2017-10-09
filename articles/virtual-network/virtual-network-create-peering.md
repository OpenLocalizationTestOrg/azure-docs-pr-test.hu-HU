---
title: "egy Azure virtuális hálózati társviszony - aaaCreate Resource Manager - ugyanahhoz az előfizetéshez |} Microsoft Docs"
description: "Ismerje meg, hogyan hozták létre toocreate virtuális hálózati társviszony-létesítés virtuális hálózatok közötti erőforrás-kezelővel, amely szerepel hello azonos Azure-előfizetés."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 026bca75-2946-4c03-b4f6-9f3c5809c69a
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: jdial;narayan;annahar
ms.openlocfilehash: c2d24fdc8103c09c3bfb8e59be12e301d9e9a55a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---resource-manager-same-subscription"></a>Hozzon létre egy virtuális hálózati társviszony - erőforrás-kezelő ugyanahhoz az előfizetéshez

Ebben az oktatóanyagban elsajátíthatja toocreate egy virtuális hálózati társviszony-létesítés Resource Manager használatával létrehozott virtuális hálózatok között. Mindkét virtuális hálózat szerepel hello azonos előfizetéssel. Társviszony-létesítési két virtuális hálózatok lehetővé teszi, hogy erőforrások egymással a különböző virtuális hálózatokon toocommunicate hello azonos sávszélesség és a késés, mintha hello erőforrások szerepeltek hello ugyanazt a virtuális hálózatot. További információ [virtuális hálózati társviszony-létesítés](virtual-network-peering-overview.md). 

hello lépéseket toocreate virtuális hálózati társviszony-létesítés eltérőek, attól függően, hogy hello virtuális hálózatok vannak-e hello azonos vagy eltérő, előfizetések, és amely [Azure telepítési modell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuális hálózatok jönnek létre. keresztül. Megtudhatja, hogyan toocreate a virtuális hálózati társviszony más esetekben hello forgatókönyv a következő táblázat hello kattintva:

|Azure üzembehelyezési modell  | Azure-előfizetés  |
|--------- |---------|
|[Mindkét erőforrás-kezelő](create-peering-different-subscriptions.md) |Különböző|
|[Egy erőforrás-kezelő egy klasszikus](create-peering-different-deployment-models.md) |Azonos|
|[Egy erőforrás-kezelő egy klasszikus](create-peering-different-deployment-models-subscriptions.md) |Különböző|

Virtuális hálózati társviszony-létesítés nem hozható létre, hello klasszikus üzembe helyezési modellben telepített virtuális hálózatok között. Virtuális hálózati társviszony-létesítés csak hozhatók létre, amely szerepel hello azonos virtuális hálózatok között Azure-régiót. Ha tooconnect virtuális hálózatokat egyaránt létrehozott hello klasszikus telepítési modell használatával, vagy, amely létezik a különböző Azure-régiók van szüksége, használhatja az Azure [VPN-átjáró](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuális hálózatok. 

Használhatja a hello [Azure-portálon](#portal), hello Azure [parancssori felület](#cli) (CLI), Azure [PowerShell](#powershell), vagy egy [Azure Resource Manager sablon](#template) toocreate virtuális hálózati társviszony-létesítés. Kattintson bármelyik hello előző eszköz hivatkozások toogo, közvetlen toohello lépések létrehozása a virtuális hálózati társviszony-létesítés a kiválasztott eszköz segítségével.

## <a name="portal"></a>Hozzon létre a társviszony - Azure-portálon

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
4. Hajtsa végre a lépéseket 2-3 újra megadása az értékek a 3. lépésben a következő hello:
    - **Név**: *myVnet2*
    - **Címtér**: *10.1.0.0/16*
    - **Alhálózati név**: *alapértelmezett*
    - **Alhálózati címtartományt**: *10.1.0.0/24*
    - **Előfizetés**: Jelölje ki az előfizetését
    - **Erőforráscsoport**: válasszon **meglévő** válassza *myResourceGroup*
    - **Hely**: *USA keleti régiója*
5. A hello **keresési erőforrások** mezőt hello portálon típus hello tetején *myResourceGroup*. Kattintson a **myResourceGroup** amikor megjelenik a hello keresési eredmények között. A panel jelenik meg hello **myresourcegroup** erőforráscsoportot. hello erőforráscsoport hello két létrehozott virtuális hálózatokat az előző lépéseket tartalmazza.
6. Kattintson a **myVNet1**.
7. A hello **myVnet1** panel, amelyen megjelenik, kattintson a **Társviszony** hello függőleges bal oldalán található hello panel hello beállítások listája.
8. A hello **myVnet1 - esetében** panel, amelyen jelent meg, kattintson a **+ Hozzáadás**
9. A hello **Hozzáadás társviszony-létesítés** panelen megjelenő, adja meg, vagy válassza ki az alábbi beállítások hello, majd kattintson **OK**:
     - **Név**: *myVnet1ToMyVnet2*
     - **Virtuális hálózat telepítési modell**: válasszon **erőforrás-kezelő**. 
     - **Előfizetés**: Jelölje ki az előfizetését
     - **Virtuális hálózati**: kattintson a **virtuális hálózatot választ**, majd kattintson a **myVnet2**.
     - **Virtuális hálózati hozzáférés engedélyezése:** ügyeljen arra, hogy **engedélyezve** van kiválasztva.
    Ebben az oktatóanyagban nincs más beállítások használhatók. toolearn összes társviszony-létesítési beállításról, olvasni [kezelheti a virtuális hálózati társviszony](virtual-network-manage-peering.md#create-a-peering).
10. Kattintás után **OK** hello a korábbi lépésben hello **Hozzáadás társviszony-létesítés** panel bezárása után, és látni hello **myVnet1 - Társviszony** újra a panelt. Néhány másodpercen belül létrehozott társviszony hello hello panel jelenik meg. **Kezdeményezett** hello szerepel **társviszony-LÉTESÍTÉS állapot** hello oszlopában **myVnet1ToMyVnet2** társviszony-létesítés meg létrehozni. Már nincsenek társviszonyban Vnet1 tooVnet2, de most myVnet2 toomyVnet1 kell partnert. hello társviszony-létesítés léteznie kell mindkét irányban hello virtuális hálózatok toocommunicate tooenable erőforrások egymással.
11. Végezze el újra az myVnet2 5-10 lépéseket.  Név hello társviszony-létesítés *myVnet2ToMyVnet1*.
12. Néhány másodpercen belül kattintás után **OK** toocreate hello társviszony-létesítés MyVnet2, a hello **myVnet2ToMyVnet1** társviszony-létesítést az imént létrehozott szerepel **csatlakoztatva** a hello  **Társviszony-LÉTESÍTÉS állapot** oszlop.
13. Újra 5-7 lépéseinek elvégzését MyVnet1. Hello **társviszony-LÉTESÍTÉS állapot** a hello **myVnet1ToVNet2** társviszony-létesítés már is **csatlakoztatva**. hello társviszony-létesítés sikeresen létrejött, miután látta, **csatlakoztatva** a hello **társviszony-LÉTESÍTÉS állapot** mindkét virtuális hálózat hello társviszony-létesítés oszlopában.
14. **Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.
15. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések hello [törli az erőforrást](#delete-portal) című szakaszát.

Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával. Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti. Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló. Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="cli"></a>Társviszony - létrehozása az Azure parancssori felület

a következő parancsfájl hello:

- Szükséges hello Azure CLI 2.0.4 verzió vagy újabb. toofind hello verzió, futtassa a hello `az --version` parancsot. Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
- A rendszerhéjakba működik. Az Azure parancssori felület parancsfájlok futtatásához a Windows-ügyfelén beállítások, lásd: [a Windows hello Azure CLI-t futtató](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Hello CLI és függőségeinek telepítése, helyett hello Azure Cloud rendszerhéj is használhatja. hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül. Hello Azure CLI előtelepített és konfigurált toouse-fiókjához van. Kattintson a hello **kipróbálás** hello parancsfájl, amely a következő, a felhő rendszerhéjat, amelyre bejelentkezik, amellyel bejelentkezhet tooyour gombra az Azure-fiók. tooexecute hello parancsfájl, kattintson a hello **másolási** gombra, és hello tartalmának beillesztése a felhő rendszerhéjat.

1. Hozzon létre egy erőforráscsoportot és két virtuális hálózatot.

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout hello script.
    rgName="myResourceGroup"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network 1.
    az network vnet create \
      --name myVnet1 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Create virtual network 2.
    az network vnet create \
      --name myVnet2 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.1.0.0/16
    ```

2. Hozzon létre egy virtuális hálózati társviszony-létesítés hello virtuális hálózatok között.
 
    ```azurecli-interactive
    # Get hello id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet1 \
      --query id --out tsv)

    # Get hello id for VNet2.
    vnet2Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet2 \
      --query id \
      --out tsv)

    # Peer VNet1 tooVNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group $rgName \
      --vnet-name myVnet1 \
      --remote-vnet-id $vnet2Id \
      --allow-vnet-access

    # Peer VNet2 tooVNet1.
    az network vnet peering create \
      --name myVnet2ToMyVnet1 \
      --resource-group $rgName \
      --vnet-name myVnet2 \
      --remote-vnet-id $vnet1Id \
      --allow-vnet-access
    ```

3. Miután hello parancsprogram végrehajtása során, tekintse át a hello esetében minden virtuális hálózathoz. 

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    Újrafuttatása hello előző parancs, cseréje *myVnet1* rendelkező *myVnet2*. hello kimenetének mindkét parancsok látható **csatlakoztatva** a hello **PeeringState** oszlop.

     Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával. Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti. Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló. Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

4. **Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.
5. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések [törli az erőforrást](#delete-cli) ebben a cikkben.


## <a name="powershell"></a>Hozzon létre a társviszony - PowerShell

1. Hello hello PowerShell legújabb verziójának telepítéséhez [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Ha új tooAzure PowerShell, lásd: [Azure PowerShell áttekintése](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. toostart a PowerShell-munkamenetet, nyissa meg túl**Start**, adja meg **powershell**, és kattintson a **PowerShell**.
3. A PowerShellben, jelentkezzen be tooAzure hello megadásával `login-azurermaccount` parancsot. hello fiókkal jelentkezik be az hello szükséges engedélyek toocreate rendelkeznie kell egy virtuális hálózati társviszony-létesítés. Lásd: hello [engedélyek](#permissions) jelen cikkben alább szakasza.
4. Hozzon létre egy erőforráscsoportot és két virtuális hálózatot. tooexecute hello parancsfájl, másolása hello következő parancsfájlt, illessze be a PowerShell és nyomja le az `Enter` után utolsó sora hello hello képernyőn látható:

    ```powershell
    # Variables for common values used throughout hello script.
    $rgName='myResourceGroup'
    $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network 1.
    $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location

    # Create virtual network 2.
    $vnet2 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet2' `
      -AddressPrefix '10.1.0.0/16' `
      -Location $location
    ```

5. Hozzon létre egy virtuális hálózati társviszony-létesítés hello virtuális hálózatok között. Másolás hello következő parancsfájl-, illessze be a tooPowerShell, és nyomja le az `Enter` után utolsó sora hello hello képernyőn látható:
    ```powershell
    # Peer VNet1 tooVNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet1ToMyVnet2' `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId $vnet2.Id

    # Peer VNet2 tooVNet1.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet2ToMyVnet1' `
      -VirtualNetwork $vnet2 `
      -RemoteVirtualNetworkId $vnet1.Id
    ```
6. hello alhálózatok tooreview hello virtuális hálózat, másolása hello következő parancsot, illessze be a tooPowerShell, és nyomja le az `Enter`:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Újrafuttatása hello előző parancs, cseréje *myVnet1* rendelkező *myVnet2*. hello kimenetének mindkét parancsok látható **csatlakoztatva** a hello **PeeringState** oszlop.
7. **Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.
8. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések [törli az erőforrást](#delete-powershell) ebben a cikkben.

Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával. Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti. Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló. Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="template"></a>Hozzon létre a társviszony - Resource Manager-sablon

1. Hivatkozás [hozzon létre egy virtuális hálózati társviszony-létesítés](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) Resource Manager-sablon. Útmutatás a megadott hello sablonra telepítése hello sablon használatával hello Azure-portálon, a PowerShell vagy a hello Azure CLI el. Napló toowhichever eszközben úgy dönt, hogy toodeploy hello sablont egy olyan fiókkal, amely rendelkezik a szükséges engedélyek toocreate hello, virtuális hálózati társviszony-létesítés. Lásd: hello [engedélyek](#permissions) jelen cikkben alább szakasza.
2. **Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.
3. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések hello [törli az erőforrást](#delete) című szakaszát, használatával hello Azure-portálon, a PowerShell vagy a hello Azure parancssori felület.

## <a name="permissions"></a>Engedélyek

hello fiókjai virtuális hálózati társviszony-létesítés toocreate hello szükséges szerepkör- vagy hozzáférési engedélyekkel kell rendelkeznie. Például, ha két virtuális hálózatok VNet1 és VNet2 nevű volt társviszony, a fiókjához társítva kell lenni a következő minimális szerepkör vagy minden egyes virtuális hálózati engedélyeinek hello:
    
|Virtuális hálózat|Szerepkör|Engedélyek|
|---|---|---|
|VNet1|[Hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
|VNet2|[Hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|

További információ [beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) és konkrét engedélyeket túl[egyéni szerepkörök](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (csak Resource Manager).

## <a name="delete"></a>Erőforrások törlése
Ez az oktatóanyag befejezése után, érdemes lehet toodelete hello erőforrások hello oktatóanyag, így a használati költségek létrehozott. Erőforráscsoport törlésekor a hello erőforráscsoportban található összes erőforrást is törlődnek.

### <a name="delete-portal"></a>Azure-portálon

1. Hello portál keresési mezőbe, írja be a **myResourceGroup**. Hello keresési eredmények között kattintson **myResourceGroup**.
2. A hello **myResourceGroup** panelen kattintson hello **törlése** ikonra.
3. tooconfirm hello törlésre, hello **típus hello ERŐFORRÁSCSOPORT-név** adja meg a **myResourceGroup**, és kattintson a **törlése**.

### <a name="delete-cli"></a>Az Azure parancssori felület

Adja meg a következő parancs hello:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

Adja meg a következő parancs hello:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -force
```

## <a name="next-steps"></a>Következő lépések

- Alaposan olvassa el a fontos [virtuális hálózati társviszony-létesítési korlátozások és viselkedéshez](virtual-network-manage-peering.md#requirements-and-constraints) társviszony-létesítés üzemi virtuális hálózat létrehozása előtt használja.
- További tudnivalók az összes [virtuális hálózati társviszony-létesítési beállítások](virtual-network-manage-peering.md#create-a-peering).
- Ismerje meg, hogyan túl[létrehoz egy központot, és a hálózati topológia irány](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) a virtuális hálózati társviszony-létesítés.
