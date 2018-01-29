---
title: "Hozzon létre egy felhasználó által megadott útvonalat az útvonal hálózati forgalmat a hálózati virtuális készülék – az Azure parancssori felület használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy hálózati virtuális készüléken keresztül hálózati forgalom útválasztási útválasztást Azure alapértelmezett felülbírálásához felhasználói útvonalat."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/16/2017
ms.author: jdial
ms.openlocfilehash: adfcf53f9fca0efafb538edfd65b95313dcf1559
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-user-defined-route---azure-cli"></a>Felhasználó által definiált útvonal - létrehozása az Azure parancssori felület

Ebből az oktatóanyagból megtudhatja, hogyan hozhat létre útvonalakat felhasználói irányíthatja a forgalmat a kettő közötti [virtuális hálózati](virtual-networks-overview.md) alhálózat egy hálózati virtuális készüléken keresztül. A hálózati virtuális készülék valójában egy hálózati alkalmazás, például egy tűzfal futtató virtuális gép. Virtuális készülékek előre konfigurált hálózati központilag telepíthető egy Azure virtuális hálózatban kapcsolatos további tudnivalókért tekintse meg a [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

Alhálózatok virtuális hálózatban, létrehozásakor az Azure létrehoz alapértelmezett [rendszerútvonalak](virtual-networks-udr-overview.md#system-routes) , amelyek lehetővé teszik az erőforrások kommunikálnak egymással, hogy az összes alhálózat az alábbi ábrán látható módon:

![Alapértelmezett útvonalak](./media/create-user-defined-route/default-routes.png)

Ebben az oktatóanyagban létrehozhat egy virtuális hálózati nyilvános, magán és DMZ-alhálózatok, az alábbi képen látható módon. Általában webkiszolgálók nyilvános alhálózathoz telepíthető, és egy alkalmazás vagy az adatbázis-kiszolgáló telepíthető titkos alhálózathoz. A hálózati virtuális készülék DMZ alhálózaton meghatalmazottjaként járhatnak el egy virtuális gép létrehozása, és szükség esetén hozzon létre egy virtuális gép minden alhálózatban a hálózati virtuális készülék keresztül kommunikáló. A nyilvános és titkos alhálózatok közötti összes forgalom a készülék keresztül történik, az alábbi ábrán látható módon:

![Felhasználó által megadott útvonalak](./media/create-user-defined-route/user-defined-routes.png)

A cikkben a Resource Manager telepítési modell, amely a felhasználó által definiált útvonalak létrehozásakor használata javasolt üzembe helyezési modellel egy felhasználó által megadott útvonal létrehozásának lépéseit. Ha egy felhasználó által megadott útvonal (klasszikus) létrehozásához szüksége, tekintse meg [hozzon létre egy felhasználó által megadott útvonal (klasszikus)](virtual-network-create-udr-classic-cli.md). Ha nem ismeri az Azure üzembe helyezési modellel, lásd: [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Felhasználó által definiált útvonalak kapcsolatos további információkért lásd: [felhasználó által definiált útvonalak áttekintése](virtual-networks-udr-overview.md#user-defined).

## <a name="create-routes-and-network-virtual-appliance"></a>Útvonalak és hálózati virtuális készülék létrehozása

Az Azure parancssori felület parancsait megegyeznek, hogy a parancsok a Windows, Linux vagy macOS hajtható végre. Vannak azonban parancsfájlok különbségei operációs rendszer ismertetése. Hajtsa végre az alábbi lépéseket a parancsfájlok egy telepítéséhez és az Azure parancssori felület parancsai a rendszerhéjakba futtatásához szükséges. Választhatja [telepítése és konfigurálása az Azure parancssori felület](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json) a számítógépen, vagy kattintson a **próbálja ki** bármely, a parancsfájlok végrehajtása az Azure-felhő rendszerhéj-parancsfájlok gomb.
 
1. **Előfeltételek**: két vnettel rendelkező virtuális hálózat létrehozása a lépések végrehajtásával [hozzon létre egy virtuális hálózatot](#create-a-virtual-network).
2. Ha fut az Azure CLI-t a számítógépről, jelentkezzen be Azure-bA a `az login` parancsot. A felhő-rendszerhéj használata, ha automatikusan jelentkezett be.
3. A további lépéseket-ban használt néhány változók megadása:

    ```azurecli-interactive
    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    ```

4. Hozzon létre egy *DMZ* alhálózati az előfeltételként létrehozott virtuális hálózatban:

    ```azurecli-interactive
    az network vnet subnet create \
      --name DMZ \
      --address-prefix 10.0.2.0/24 \
      --vnet-name myVnet \
      --resource-group $rgName
    ```

5. A NVA virtuális gép létrehozásához. Statikus nyilvános és magánhálózati IP-címek kiosztása a CLI hoz létre hálózati kapcsolat. Statikus címeket a során a virtuális gép ne módosítsa. Az NVA a Linux vagy a Windows operációs rendszert futtató virtuális gép is lehet. A virtuális gép létrehozásához, másolja a parancsfájlt vagy operációs rendszerhez, és illessze be a parancssori felület. Ha egy Windows virtuális gép létrehozása illessze be a parancsfájlt egy szövegszerkesztőbe, módosítsa a *AdminPassword* változót, majd illessze be a módosított szöveg be a parancssori felület.

    **Linux**

    ```azurecli-interactive
    az vm create \
      --resource-group $rgName \
      --name myVm-Nva \
      --image UbuntuLTS \
      --private-ip-address 10.0.2.4 \
      --public-ip-address myPublicIp-myVm-Nva \
      --public-ip-address-allocation static \
      --subnet DMZ \
      --vnet-name myVnet \
      --admin-username azureuser \
      --generate-ssh-keys
    ```

    **Windows**

    ```azurecli-interactive
    AdminPassword=ChangeToYourPassword
    az vm create \
      --resource-group $rgName \
      --name myVm-Nva \
      --image win2016datacenter \
      --private-ip-address 10.0.2.4 \
      --public-ip-address myPublicIp-myVm-Nva \
      --public-ip-address-allocation static \
      --subnet DMZ \
      --vnet-name myVnet \
      --admin-username azureuser \
      --admin-password $AdminPassword      
    ```

6. Az NVA hálózati adapter IP-továbbítás engedélyezése. IP továbbító egy adott hálózati csatoló engedélyezésekor Azure nem a forrás vagy a cél IP-cím kereséséhez. Ha nem engedélyezi ezt a beállítást, nem kap, a hálózati adapter IP-cím szánt forgalom a rendszer eldobja az Azure-ban.

    ```azurecli-interactive
    az network nic update \
      --name myVm-NvaVMNic \
      --resource-group $rgName \
      --ip-forwarding true
    ```

7. Hozzon létre egy útvonaltábla a *nyilvános* alhálózat.

    ```azurecli-interactive
    az network route-table create \
      --name myRouteTable-Public \
      --resource-group $rgName
    ```
    
8. Alapértelmezés szerint az Azure útvonalak forgalmat a virtuális hálózaton belül minden alhálózatok között. Hozzon létre módosítása, hogy a titkos alhálózati a nyilvános alhálózat-forgalom áthalad az NVA ahelyett, hogy közvetlenül a személyes alhálózati az útválasztási Azure alapértelmezett útvonalat.

    ```azurecli-interactive    
    az network route-table route create \
      --name ToPrivateSubnet \
      --resource-group $rgName \
      --route-table-name myRouteTable-Public \
      --address-prefix 10.0.1.0/24 \
      --next-hop-type VirtualAppliance \
      --next-hop-ip-address 10.0.2.4
    ```

9. Társítsa a *myRouteTable nyilvános* útválasztási táblázatot, hogy a *nyilvános* alhálózat. Megfelelően az útvonalakat az alhálózat minden kimenő forgalom továbbításához az útvonaltáblát az Azure közötti társítás egy útválasztási táblázatot az alhálózathoz okoz. Egy útválasztási táblázatot lehet társítva a következőhöz: nulla vagy több alhálózatból, mivel alhálózat lehetnek nulla, vagy egy útválasztási táblázatot társítva.

    ```azurecli-interactive
    az network vnet subnet update \
      --name Public \
      --vnet-name myVnet \
      --resource-group $rgName \
      --route-table myRouteTable-Public
    ```

10. Az útvonaltábla létrehozása a *titkos* alhálózat.

    ```azurecli-interactive
    az network route-table create \
      --name myRouteTable-Private \
      --resource-group $rgName
    ```
      
11. Hozzon létre egy útvonalat az útvonal forgalmát a *titkos* alhálózatában ahhoz, hogy a *nyilvános* keresztül az NVA virtuális gépet.

    ```azurecli-interactive
    az network route-table route create \
      --name ToPublicSubnet \
      --resource-group $rgName \
      --route-table-name myRouteTable-Private \
      --address-prefix 10.0.0.0/24 \
      --next-hop-type VirtualAppliance \
      --next-hop-ip-address 10.0.2.4
    ```

12. Az útvonaltábla társítsa a *titkos* alhálózat.

    ```azurecli-interactive
    az network vnet subnet update \
      --name Private \
      --vnet-name myVnet \
      --resource-group $rgName \
      --route-table myRouteTable-Private
    ```
    
13. **Választható lehetőség:** a nyilvános és Magánfelhő alhálózatban lévő virtuális gép létrehozása, és ellenőrizze, hogy a virtuális gépek közötti kommunikáció áthalad a hálózati virtuális készülék a lépések végrehajtásával [útválasztásiellenőrzése](#validate-routing).
14. **Nem kötelező**: az erőforrásokat, amelyek ebben az oktatóanyagban létrehozhat törléséhez végrehajtásához a [törli az erőforrást](#delete-resources).

## <a name="validate-routing"></a>Útválasztás ellenőrzése

1. Ha még nem tette meg, hajtsa végre a [útvonalak és hálózati virtuális készülék létrehozása](#create-routes-and-network-virtual-appliance).
2. Kattintson a **kipróbálás** a csoportban, amely a következő, amely az Azure-felhő rendszerhéj megnyitása gombra. Ha a rendszer kéri, jelentkezzen be az Azure használatával a [Azure-fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Ha nincs Azure-fiókja, regisztráljon egy [ingyenes próbaverzióra](https://azure.microsoft.com/offers/ms-azr-0044p). Az Azure-felhő rendszerhéj a szabad rendszerhéjakba az Azure parancssori felületével előre telepítve. 

    Az alábbi parancsfájlok, hozzon létre két virtuális gép, egy-egy a *nyilvános* alhálózatot és egy-egy a *titkos* alhálózat. A parancsfájlok IP-továbbítás ahhoz, hogy az operációs rendszer NVA operációs rendszerében a hálózati adapter irányíthatja a forgalmat a hálózati kapcsolaton keresztül is engedélyezheti. Egy éles NVA általában megvizsgálja a forgalom útválasztási azt megelőzően, de ebben az oktatóanyagban az egyszerű NVA csak irányítja a forgalmat tanulmányozza az nélkül. 

    Kattintson a **másolási** gombra a **Linux** vagy **Windows** parancsfájlok, hajtsa végre, és illessze be a parancsfájl tartalmát egy szövegszerkesztőben. Módosítsa a jelszavát a *adminPassword* változót, majd illessze be a parancsfájl az Azure felhőalapú rendszerhéjat. Futtassa a parancsfájlt az 5. lépésében létrehozása után a virtuális hálózati berendezések kiválasztott operációs rendszer [útvonalak és hálózati virtuális készülék létrehozása](#create-routes-and-network-virtual-appliance). 

    **Linux**

    ```azurecli-interactive
    #!/bin/bash

    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    adminPassword=ChangeToYourPassword
    
    # Create a virtual machine in the Public subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Public \
      --image UbuntuLTS \
      --vnet-name myVnet \
      --subnet Public \
      --public-ip-address myPublicIp-Public \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Create a virtual machine in the Private subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Private \
      --image UbuntuLTS \
      --vnet-name myVnet \
      --subnet Private \
      --public-ip-address myPublicIp-Private \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Enable IP forwarding for the network interface in the NVA virtual machine's operating system.    
    az vm extension set \
      --resource-group $rgName \
      --vm-name myVm-Nva \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
    ```

    **Windows**

    ```azurecli-interactive

    #!/bin/bash
    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    adminPassword=ChangeToYourPassword
    
    # Create a virtual machine in the Public subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Public \
      --image win2016datacenter \
      --vnet-name myVnet \
      --subnet Public \
      --public-ip-address myPublicIp-Public \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Allow pings through the Windows Firewall.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Public \
      --resource-group $rgName \
      --settings '{"commandToExecute":"netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}'

    # Create a virtual machine in the Private subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Private \
      --image win2016datacenter \
      --vnet-name myVnet \
      --subnet Private \
      --public-ip-address myPublicIp-Private \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Allow pings through the Windows Firewall.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Private \
      --resource-group $rgName \
      --settings '{"commandToExecute":"netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}'

    # Enable IP forwarding for the network interface in the NVA virtual machine's operating system.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Nva \
      --resource-group $rgName \
      --settings '{"commandToExecute":"powershell.exe Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1"}'

    # Restart the NVA virtual machine.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Nva \
      --resource-group $rgName \
      --settings '{"commandToExecute":"powershell.exe Restart-Computer -ComputerName myVm-Nva -Force"}'
    ```

3. Ellenőrizze a nyilvános és Magánfelhő alhálózatban lévő virtuális gépek közötti kommunikáció. 

    - Nyissa meg egy [SSH](../virtual-machines/linux/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-vm) (Linux) vagy [távoli asztal](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-vm) a nyilvános IP-címe (Windows) kapcsolatot a *myVm nyilvános* virtuális gépet.
    - A parancssorból a *myVm nyilvános* virtuális gépet, adja meg `ping myVm-Private`. Mivel az NVA irányítja a forgalmat a nyilvánosság elől a titkos alhálózathoz választ kap.
    - Az a *myVm nyilvános* virtuális gépet, a nyilvános és titkos alhálózatokat a virtuális gépek közötti nyomkövetést futtatni. Adja meg a megfelelő parancsot, amely a következő, az operációs rendszertől függően telepítette, akkor a virtuális gépek használata a nyilvános és Magánfelhő alhálózatok:
        - **Windows**: egy parancssorból futtassa a `tracert myvm-private` parancsot.
        - **Ubuntu**: futtassa a `tracepath myvm-private` parancsot.
      Forgalom átmegy 10.0.2.4 (NVA) (a virtuális gép személyes alhálózaton) 10.0.1.4 elérése előtt. 
    - Az előző lépéseket csatlakozva a *myVm InPrivate-* virtuális gép és a pingelés a *myVm nyilvános* virtuális gépet. A nyomkövetési útvonal 10.0.0.4 (a virtuális gép nyilvános alhálózaton) elérése előtt 10.0.2.4 keresztül utazás kommunikációs jeleníti meg.
      
      > [!NOTE]
      > Az előző lépések lehetővé teszik annak ellenőrzéséhez, hogy az Azure magánhálózati IP-címek között. Ha előre, vagy a proxy, forgalom nyilvános IP-címek, egy hálózati virtuális készüléken keresztül:
      > - A készülék biztosítania kell a hálózati címfordítás vagy a proxy-képességet. Hálózati címfordítás, a készülék kell fordítani a forrás IP-cím a saját, és majd továbbítja a kérést a nyilvános IP-cím. Hogy a készülék hálózati cím lefordítani az forráscím, vagy a proxy használatát az ügynökökön, Azure fordítja le a hálózati virtuális készülék magánhálózati IP-cím egy nyilvános IP-cím. Azure nyilvános IP-címek magánhálózati IP-címeit használja a különböző módszerekkel kapcsolatos további információkért lásd: [kimenő kapcsolatok ismertetése](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
      > - Az útválasztási táblázatban előtag például egy további útvonal: 0.0.0.0/0, a következő ugrás típusa VirtualAppliance és a következő ugrás IP-10.0.2.4 (az előző példa parancsfájlban).
      >
    - **Opcionálisan**: Azure hálózati figyelőt a következő ugrás képességét segítségével ellenőrizheti azokat a következő Ugrás az Azure két virtuális gépek között. Hálózati figyelőt használatához először [hozzon létre egy Azure hálózati figyelőt példányt](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json) számára a használni kívánt régiót. Ebben az oktatóanyagban az Amerikai keleti terület szolgál. Miután engedélyezte a hálózati figyelőt példánya a régió, adja meg a nyilvános és Magánfelhő alhálózatban lévő virtuális gépek között a következő ugrás adatainak megtekintéséhez a következő parancsot:
     
        ```azurecli-interactive
        az network watcher show-next-hop --resource-group myResourceGroup --vm myVm-Public --source-ip 10.0.0.4 --dest-ip 10.0.1.4
        ```

       A kimeneti adja vissza *10.0.2.4* , a **nextHopIpAddress** és *VirtualAppliance* , a **nextHopType**.

> [!NOTE]
> Ebben az oktatóanyagban olvassanak, nyilvános IP-címek hozzá vannak rendelve a virtuális gépek használata a nyilvános és titkos alhálózatok, és minden hálózati port engedélyezve a hozzáférés az Azure mindkét virtuális gépekhez. Éles környezetben való használathoz a virtuális gépek létrehozásakor lehet, hogy nem nyilvános IP-címek kiosztása őket, és szűkítheti a hálózati forgalmat a magánhálózati alhálózathoz takarja el a hálózati virtuális készülék telepítésével, illetve a hálózati biztonsági csoportok hozzárendelése az alhálózatok hálózati adapter, vagy mindkettőt. Hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [hálózati biztonsági csoportok](virtual-networks-nsg.md).

## <a name="create-a-virtual-network"></a>Virtuális hálózat létrehozása

Ebben az oktatóanyagban egy meglévő virtuális hálózatot, két alhálózattal igényel. Kattintson a **kipróbálás** a mezőbe, amely gyorsan a virtuális hálózat létrehozása a következő gombra. Kattintson a **kipróbálás** gombra kattint, megnyílik a [Azure Cloud rendszerhéj](../cloud-shell/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bár a felhőalapú rendszerhéj a PowerShell vagy a Bash rendszerhéjat ebben a szakaszban fut, a virtuális hálózat létrehozása a rendszerhéjakba szolgál. A rendszerhéjakba az Azure parancssori felület telepítve van. Ha a felhő rendszerhéj kéri, jelentkezzen be az Azure használatával a [Azure-fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Ha nincs Azure-fiókja, regisztráljon egy [ingyenes próbaverzióra](https://azure.microsoft.com/offers/ms-azr-0044p). A jelen oktatóanyagban használt virtuális hálózat létrehozásához kattintson a **másolási** gombra kattint, a következő mezőbe, majd illessze be a parancsfájl az Azure-felhő rendszerhéj:

```azurecli-interactive
#!/bin/bash

#Set variables used in the script.
rgName="myResourceGroup"
location="eastus"

# Create a resource group.
az group create \
  --name $rgName \
  --location $location

# Create a virtual network with one subnet named Public.
az network vnet create \
  --name myVnet \
  --resource-group $rgName \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24

# Create an additional subnet named Private in the virtual network.
az network vnet subnet create \
  --name Private \
  --address-prefix 10.0.1.0/24 \
  --vnet-name myVnet \
  --resource-group $rgName
```

Virtuális hálózat létrehozása a portálon, a PowerShell vagy az Azure Resource Manager-sablonok használatával kapcsolatos további tudnivalókért lásd: [hozzon létre egy virtuális hálózatot](virtual-networks-create-vnet-arm-pportal.md).

## <a name="delete-resources"></a>Erőforrások törlése

Ez az oktatóanyag befejezése után előfordulhat, hogy törölni kívánja az erőforrásokat, amelyek hozott létre, így használati költségek. Erőforráscsoport törlésével, az erőforráscsoport összes erőforrást is törli. Adja meg egy parancssori munkamenetet a következő parancsot:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Következő lépések

- Hozzon létre egy [magas rendelkezésre állású hálózati virtuális készülék](/azure/architecture/reference-architectures/dmz/nva-ha?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Virtuális hálózati berendezések gyakran rendelkezik, több hálózati adapterrel és IP-címek hozzárendelve. Megtudhatja, hogyan [hálózati adapterek hozzáadása egy meglévő virtuális gép](virtual-network-network-interface-vm.md#vm-add-nic) és [IP-címek hozzáadása a meglévő hálózati illesztő](virtual-network-network-interface-addresses.md#add-ip-addresses). Bár az összes virtuálisgép-méretek lehet kapcsolódik legalább két hálózati adapterrel, minden virtuális gép méretét támogatja a hálózati adapterek maximális száma. Hány hálózati adapterek minden egyes virtuális gép mérete által támogatott, lásd: [Windows](../virtual-machines/windows/sizes.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json) és [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gépek méretét. 
- Kényszerített bújtatás forgalom helyszíni keresztül felhasználói útvonal létrehozása egy [pont-pont VPN-kapcsolat](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
