---
title: "a virtuális hálózati aaaCreate |} Az Azure Resource Manager-sablon |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy virtuális hálózathoz, az Azure Resource Manager-sablon használatával."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 69530861-2f97-4a6e-b336-a7baf2690044
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b9c289433ff2a84bec19eac25fa28ab40d131c7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-an-azure-resource-manager-template"></a>Azure Resource Manager-sablonnal virtuális hálózat létrehozása

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Az Azure két üzemi modellel rendelkezik, az Azure Resource Managerrel és a klasszikussal. A Microsoft azt javasolja, hello Resource Manager üzembe helyezési modellben erőforrásoknak létrehozása. hello arról toolearn hello hello két modellek közötti különbséget olvasási [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md) cikk.
 
Ez a cikk azt ismerteti, hogyan toocreate egy Vnetet hello erőforrás-kezelő központi modellhez tartozó Azure Resource Manager-sablonnal. Hozzon létre egy Vnetet Resource Manageren keresztül más eszközök használatával vagy hello klasszikus telepítési modell használatával VNet létrehozása a következő lista hello egy másik lehetőség kiválasztásával is:

> [!div class="op_single_selector"]
- [Portál](virtual-networks-create-vnet-arm-pportal.md)
- [PowerShell](virtual-networks-create-vnet-arm-ps.md)
- [Parancssori felület](virtual-networks-create-vnet-arm-cli.md)
- [Sablon](virtual-networks-create-vnet-arm-template-click.md)
- [Portál (klasszikus)](virtual-networks-create-vnet-classic-pportal.md)
- [PowerShell (klasszikus)](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [Parancssori felület (klasszikus)](virtual-networks-create-vnet-classic-cli.md)

Megtudhatja, hogyan toodownload módosításához és a meglévő ARM-sablont a Githubból, és a GitHub, a PowerShell és a hello Azure CLI hello sablon üzembe helyezése.

Ha egyszerűen telepít hello ARM-sablon közvetlenül a Githubból, változtatások nélkül, hagyja ki túl[központi telepítése egy sablont a githubból](#deploy-the-arm-template-by-using-click-to-deploy).

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="download-and-understand-hello-azure-resource-manager-template"></a>Töltse le és hello Azure Resource Manager sablon ismertetése
Létrehozhat egy Vnetet két alhálózattal a Githubról meglévő sablon hello letöltése, hajtsa végre a módosításokat, előfordulhat, hogy szeretné, és újra felhasználhatja. toodo Igen, végezze el a hello a következő lépéseket:

1. Keresse meg a túl[hello mintasablon lapot](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Kattintson az **azuredeploy.json**, majd a **RAW** elemre.
3. Mentés hello fájl tooa egy helyi mappába a számítógépen.
4. Ha ismeri a sablonok, ugorjon a toostep 7.
5. Nyissa meg a hello előbb mentett fájlt, és nézze meg a hello tartalma **paraméterek** 5. sorban. Az ARM-sablonparaméterek az üzembe helyezés során kitölthető paraméterek helyőrzőiként működnek.
   
   | Paraméter | Leírás |
   | --- | --- |
   | **hely** |Azure-régió, ahol hello VNet létrejön |
   | **vnetName** |Hello nevét új virtuális hálózat |
   | **addressPrefix** |Címtartomány CIDR formátumban VNet hello |
   | **subnet1Name** |Hello nevét első virtuális hálózat |
   | **subnet1Prefix** |Hello első alhálózat CIDR-blokkja |
   | **subnet2Name** |Hello nevét a második virtuális hálózat |
   | **subnet2Prefix** |Hello második alhálózat CIDR-blokkja |
   
   > [!IMPORTANT]
   > A GitHubban fenntartott Azure Resource Manager-sablonok idővel módosulhatnak. Győződjön meg arról, hogy hello sablon ellenőrizze annak használata előtt.
   > 
   > 
6. Hello tartalom alapján ellenőrizze **erőforrások** , és figyelje meg a következő hello:
   
   * **type**. Hello sablon által létrehozott erőforrás típusát. Ebben az esetben a **Microsoft.Network/virtualNetworks**, ami egy VNetet jelöl.
   * **Név** Hello erőforrás nevét. Értesítés hello használata **[parameters('vnetName')]**, ami azt jelenti, hogy hello neve lesz a bemenetként megadott hello felhasználó vagy egy paraméterfájl üzembe helyezése során.
   * **properties**. Hello erőforrás tulajdonságainak listája. Ez a sablon hello cím és az alhálózati tulajdonságokat használja a VNet létrehozása során.
7. Lépjen vissza túl[hello mintasablon lapot](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. Kattintson az **azuredeploy-paremeters.json**, majd a **RAW** elemre.
9. Mentés hello fájl tooa egy helyi mappába a számítógépen.
10. Nyissa meg a hello előbb mentett fájlt, és hello értékeinek hello paraméterek szerkesztése. A következő értékek toodeploy hello hello forgatókönyvben bemutatott VNet alá hello használata:

    ```json
        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }
    ```

11. Hello fájl mentéséhez.


## <a name="deploy-hello-template-using-powershell"></a>PowerShell-lel hello sablon üzembe helyezése

Hajtsa végre a következő lépéseket a PowerShell használatával a letöltött toodeploy hello sablon hello:

1. Telepítse és konfigurálja az Azure PowerShell hello lépések végrehajtásával a hello [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) cikk.
2. Futtassa a következő parancs toocreate hello új erőforráscsoport:

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    hello parancs létrehoz egy erőforráscsoportot *TestRG* a hello *USA középső RÉGIÓJA* azure-régiót. További információ az erőforráscsoportokkal kapcsolatban: [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md) (Az Azure Resource Manager áttekintése).

    Várt kimenet:

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/[Id]/resourceGroups/TestRG

3. Futtassa a következő parancs toodeploy hello hello sablonnal és paraméterfájlokkal fájlokkal, fent letöltött és módosított új virtuális hálózat hello:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestVNetDeployment -ResourceGroupName TestRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

    Várt kimenet:
   
        DeploymentName    : TestVNetDeployment
        ResourceGroupName : TestRG
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            location         String                     Central US
                            vnetName         String                     TestVNet
                            addressPrefix    String                     192.168.0.0/16
                            subnet1Prefix    String                     192.168.1.0/24
                            subnet1Name      String                     FrontEnd
                            subnet2Prefix    String                     192.168.2.0/24
                            subnet2Name      String                     BackEnd
   
        Outputs           :
4. Futtatási hello következő parancsot a tooview hello tulajdonságainak hello új virtuális hálózat:

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

    Várt kimenet:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              },
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="deploy-hello-template-using-click-to-deploy"></a>Kattintson a központi telepítése segítségével hello sablon üzembe helyezése

Előre definiált Azure Resource Manager sablonok feltöltött tooa GitHub-tárházban tartja fenn a Microsoft és a közösségi Megnyitás toohello is felhasználhatja. Ezeket a sablonokat is telepíthető közvetlenül a github webhelyen, kívül, vagy letöltött, és módosított toofit igényeinek. toodeploy a sablont, amely hoz létre egy Vnetet két alhálózattal, teljes hello a következő lépéseket:

1. Egy böngészőből keresse túl[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Görgessen lefelé hello sablonok listáján, majd kattintson a **101-vnet-two-subnets**. Ellenőrizze a hello **README.md** fájlt a lent látható módon.

    ![A README.md fájl a GitHubban](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Kattintson a **tooAzure telepítése**. Szükség esetén adja meg az Azure bejelentkezési hitelesítő adatait. 
4. A hello **paraméterek** panelen adja meg a hello értékek toouse toocreate szeretné, hogy az új Vnetet, és kattintson a **OK**. a következő ábra azt mutatja be hello értékek hello forgatókönyvhöz hello:
   
    ![Az ARM-sablon paraméterei](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

5. Kattintson a **erőforráscsoport** , és válassza a erőforrás csoport tooadd hello VNet, vagy kattintson a **hozzon létre új** tooadd hello VNet tooa új erőforráscsoportot. hello következő ábrán látható hello erőforrás nevű új erőforráscsoport tartozó beállítások **TestRG**:

    ![Erőforráscsoport](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

6. Szükség esetén módosítsa a hello **előfizetés** és **hely** a VNet beállításait.
7. Ha nem szeretné, hogy toosee hello VNet csempe megjelenjen hello **Kezdőpulton**, tiltsa le a **PIN-kód tooStartboard**.
8. Kattintson a **jogi feltételeket**, olvassa el hello feltételeit, és kattintson **megvásárlása** tooagree. 
9. Kattintson a **létrehozása** toocreate hello virtuális hálózat.
   
    ![Üzembe helyezési csempe küldése a betekintő portálban](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

10. Hello központi telepítés befejezése után a hello Azure-portálon kattintson **további szolgáltatások**, típus *virtuális hálózatok* hello szűrő mezőbe, amely akkor jelenik meg, kattintson a virtuális hálózatok toosee hello virtuális hálózatok panelen. Hello paneljén kattintson *TestVNet*. A hello *TestVNet* panelen kattintson a **alhálózatok** toosee létrehozott hello alhálózatok, ahogy az alábbi képen hello:
    
     ![VNet létrehozása a betekintő portálon](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.png)

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan tooconnect:

- A virtuális gép (VM) tooa virtuális hálózatot, olvasási hello [Windows virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md) vagy [hozzon létre egy Linux virtuális Gépet](../virtual-machines/linux/quick-create-portal.md) cikkeket. Egy VNet és alhálózat létrehozása hello cikkek hello lépésekben, helyett választhatja egy meglévő VNet és alhálózat tooconnect egy virtuális Gépet.
- virtuális hálózat tooother virtuális hálózatok hello hello olvasásával [csatlakozás Vnetek](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) cikk.
- hello virtuális hálózati tooan a helyi hálózati helyek virtuális magánhálózati (VPN) vagy ExpressRoute-kapcsolatcsoportot. Megtudhatja, hogyan hello olvasásával [VNet tooan a helyi hálózat a telephelyek közötti VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) és [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](../expressroute/expressroute-howto-linkvnet-arm.md) cikkeket.
