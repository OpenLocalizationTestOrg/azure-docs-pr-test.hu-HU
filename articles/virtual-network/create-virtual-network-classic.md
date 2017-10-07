---
title: "egy Azure virtuális hálózat (klasszikus), több alhálózattal aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy virtuális hálózat (klasszikus), több alhálózattal az Azure-ban."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: cc7b9ad08d5c26dba09584762bedf614d2847032
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-classic-with-multiple-subnets"></a>Hozzon létre egy virtuális hálózat (klasszikus), több alhálózattal

> [!IMPORTANT]
> Két Azure tartozik [különböző üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) az erőforrások létrehozására és kezelésére vonatkozó: Resource Manager és klasszikus. Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, keresztül hello legtöbb új virtuális hálózat létrehozása [erőforrás-kezelő](virtual-networks-create-vnet-arm-pportal.md) üzembe helyezési modellben.

Ebből az oktatóanyagból megtudhatja, hogyan toocreate egy alapszintű Azure virtuális hálózat (klasszikus), amely rendelkezik külön nyilvános és titkos alhálózat. Azure-erőforrások, például a virtuális gépek és felhőszolgáltatások az alhálózat hozhat létre. Létrehozott virtuális hálózatok (klasszikus) erőforrásokhoz kommunikálhatnak egymással, illetve más hálózatokhoz csatlakozó tooa virtuális hálózatán lévő erőforrásokat.

További információk [virtuális hálózati](virtual-network-manage-network.md) és [alhálózati](virtual-network-manage-subnet.md) beállításait.

> [!WARNING]
> Virtuális hálózatok (klasszikus) Azure azonnal törli amikor egy [előfizetés le van tiltva](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit). Virtuális hálózatok (klasszikus) törlődnek, függetlenül attól, hogy erőforrások hello virtuális hálózat létezik. Ha később újra engedélyezi hello előfizetés, szereplő hello virtuális hálózati erőforrások kell újból létre kell hozni.

Hello segítségével létrehozhat egy virtuális hálózat (klasszikus) [Azure-portálon](#portal), hello [Azure parancssori felület (CLI) 1.0](#azure-cli), vagy [PowerShell](#powershell).

## <a name="portal"></a>Portál

1. Egy webböngészőben nyissa meg toohello [Azure-portálon](https://portal.azure.com). Jelentkezzen be a [Azure-fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Ha az Azure-fiók nem rendelkezik, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Kattintson a **+ új** hello portálon.
3. Adja meg *virtuális hálózati* a hello **keresési hello piactér** mezőt hello hello tetején **új** panel, amely akkor jelenik meg.  Kattintson a **virtuális hálózati** amikor megjelenik a hello keresési eredmények között.
4. Válassza ki **klasszikus** a hello **telepítési modell kiválasztása** hello párbeszédpanel **virtuális hálózati** panel, amely akkor jelenik meg, majd kattintson **létrehozása**. 
5. Adja meg a következő értékeket a hello hello **virtuális hálózat létrehozása (klasszikus)** panel megnyitásához, és kattintson **létrehozása**:

    |Beállítás|Érték|
    |---|---|
    |Név|myVNet|
    |Címtér|10.0.0.0/16|
    |Alhálózat neve|Nyilvános|
    |Alhálózati címtartomány|10.0.0.0/24|
    |Erőforráscsoport|Hagyja **hozzon létre új** kiválasztva, és írja be **myResourceGroup**.|
    |Egyes előfizetésekhez és helyekhez|Válassza ki az egyes előfizetésekhez és helyekhez.

    Ha új tooAzure, további információ [erőforráscsoportok](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [előfizetések](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), és [helyek](https://azure.microsoft.com/regions) (más néven tooas *régiók*).
4. Hello portálon csak egy alhálózat virtuális hálózat létrehozásakor is létrehozhat. Ebben az oktatóanyagban létrehoz egy második alhálózat hello virtuális hálózat létrehozása után. Később létrehozhat az internetről elérhető erőforrások a hello **nyilvános** alhálózat. Is létrehozhat, amelyek nem érhetők el az Internet hello hello az erőforrások **titkos** alhálózat. toocreate hello második alhálózat, adja meg **myVnet** a hello **keresési erőforrások** mezőt hello lap hello tetején. Kattintson a **myVnet** amikor megjelenik a hello keresési eredmények között.
5. Kattintson a **alhálózatok** (a hello **beállítások** szakasz) a hello **virtuális hálózat létrehozása (klasszikus)** megjelenő panelen.
6. Kattintson a **+ Hozzáadás** a hello **myVnet - alhálózatok** panel, amely akkor jelenik meg.
7. Adja meg **titkos** a **neve** a hello **alhálózat hozzáadása** panelen. Adja meg **10.0.1.0/24** a **-címtartományt**.  Kattintson az **OK** gombra.
8. A hello **myVnet - alhálózatok** panelen láthatja hello **nyilvános** és **titkos** létrehozott alhálózatok.
9. **Nem kötelező**: Ez az oktatóanyag befejezése után érdemes lehet toodelete hello erőforrások létrehozott, így a használati költségek:
    - Kattintson a **áttekintése** a hello **myVnet** panelen.
    - Kattintson a hello **törlése** hello ikonra **myVnet** panelen.
    - tooconfirm hello törlését, kattintson a **Igen** a hello **virtuális hálózati Delete** mezőbe.

## <a name="azure-cli"></a>Azure CLI

1. Megadhat [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), vagy a CLI hello hello Azure Cloud rendszerhéj belül használja. hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül. Hello Azure CLI előtelepített és konfigurált toouse-fiókjához van. Írja be a parancssori felület parancsait tooget súgóját `azure <command> --help`. 
2. CLI munkamenetben a következő paranccsal hello tooAzure jelentkezni. Ha **kipróbálás** alábbi hello mezőbe, a felhő rendszerhéj megnyitása. Azure-előfizetésre, tooyour bejelentkezhet a következő parancs hello megadása nélkül:

    ```azurecli-interactive
    azure login
    ```

3. tooensure hello CLI szolgáltatásfelügyelet módban van, adja meg a következő parancs hello:

    ```azurecli-interactive
    azure config mode asm
    ```

4. Virtuális hálózat létrehozása a saját alhálózattal:

    ```azurecli-interactive
    azure network vnet create --vnet myVnet --address-space 10.0.0.0 --cidr 16  --subnet-name Private --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --location "East US"
    ```

5. Hozzon létre egy nyilvános alhálózatot hello virtuális hálózaton belül:

    ```azurecli-interactive
    azure network vnet subnet create --name Public --vnet-name myVnet --address-prefix 10.0.1.0/24
    ```    
    
6. Tekintse át a virtuális hálózati hello és -alhálózatok:

    ```azurecli-interactive
    azure network vnet show --vnet myVnet
    ```

7. **Nem kötelező**: érdemes létrehozott Ez az oktatóanyag befejezésekor, hogy a használati költségek toodelete hello erőforrások:

    ```azurecli-interactive
    azure network vnet delete --vnet myVnet --quiet
    ```

> [!NOTE]
> Abban az esetben, ha nem adja meg egy erőforrás csoport toocreate egy (klasszikus) virtuális hálózati hello parancssori felület használatával, ha Azure hello virtuális hálózatot hoz létre a nevű erőforráscsoportban *alapértelmezett-hálózat*.

## <a name="powershell"></a>PowerShell

1. Hello hello PowerShell legújabb verziójának telepítéséhez [Azure](https://www.powershellgallery.com/packages/Azure) modul. Ha új tooAzure PowerShell, lásd: [Azure PowerShell áttekintése](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Indítson el egy PowerShell-munkamenetet.
3. A PowerShellben, jelentkezzen be tooAzure hello megadásával `Add-AzureAccount` parancsot.
4. Módosítsa a következő hello elérési út és fájlnév, mint a megfelelő, majd exportálja a meglévő hálózati konfigurációs fájlban:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
    ```

5. toocreate nyilvános és titkos alhálózatokat, virtuális hálózat használata bármilyen szöveg szerkesztő tooadd hello **VirtualNetworkSite** elem toohello hálózati konfigurációs fájlban.

    ```xml
    <VirtualNetworkSite name="myVnet" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Private">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Public">
            <AddressPrefix>10.0.1.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    ```

    Felülvizsgálati hello teljes [hálózati konfigurációs fájl séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).

6. Hello hálózati konfigurációs fájl importálása:

    ```powershell
    Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
    ```

    > [!WARNING]
    > Megváltozott hálózati konfigurációs fájlok importálása okozhat módosítások tooexisting virtuális hálózatok (klasszikus), az előfizetésben. Győződjön meg arról, csak akkor hozzá hello korábbi virtuális hálózaton, és módosítsa vagy távolítsa el a meglévő virtuális hálózatok az előfizetésből nem. 

7. Tekintse át a virtuális hálózati hello és -alhálózatok:

    ```powershell
    Get-AzureVNetSite -VNetName "myVnet"
    ```

8. **Nem kötelező**: érdemes toodelete hello erőforrások létrehozott Ez az oktatóanyag befejezésekor, hogy a használati költségek. toodelete hello virtuális hálózat, teljes lépések 4 – 6 újra, az idő eltávolításakor hello **VirtualNetworkSite** 5 lépésben hozzáadott elemet.
 
> [!NOTE]
> Abban az esetben, ha egy erőforrás csoport toocreate egy virtuális hálózat (klasszikus) nem adható meg a PowerShell használatával, ha Azure hello virtuális hálózatot hoz létre a nevű erőforráscsoportban *alapértelmezett-hálózat*.

---

## <a name="next-steps"></a>Következő lépések

- minden virtuális hálózati és alhálózati beállítások toolearn lásd: [virtuális hálózatok kezeléséhez](virtual-network-manage-network.md) és [kezelheti a virtuális hálózati alhálózat](virtual-network-manage-subnet.md). Lehetősége van különböző virtuális hálózatok és alhálózatok használatát mutatja be egy éles környezetben toomeet eltérő követelmények vonatkoznak.
- toofilter bejövő és kimenő forgalmat, létrehozása és alkalmazása [hálózati biztonsági csoportok](virtual-networks-nsg.md) toosubnets.
- Hozzon létre egy [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy egy [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gépet, és csatlakoztassa tooan már meglévő virtuális hálózatot.
- tooconnect két virtuális hálózatok a hello Azure ugyanazon a helyen, és hozzon létre egy [virtuális hálózati társviszony-létesítés](create-peering-different-deployment-models.md) hello virtuális hálózatok között. Is egyenrangú egy virtuális hálózat (erőforrás-kezelő) tooa virtuális hálózat (klasszikus), de nem hozható létre társviszony-létesítés virtuális hálózatok (klasszikus) között.
- Hello virtuális hálózat tooan a helyszíni hálózati használatával kapcsolódnak a [VPN-átjáró](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) körön.
