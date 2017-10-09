---
title: "egy Azure virtuális hálózati társviszony - aaaCreate különböző üzembe helyezési modellek - különböző előfizetések |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy virtuális hálózati társviszony-létesítés virtuális hálózatok közötti létre különböző Azure üzembe helyezési modellel, az Azure-előfizetések létező keresztül."
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
ms.openlocfilehash: 865bdabb5b87523ba943d7b5dcbdc2475b78bdb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---different-deployment-models-and-subscriptions"></a>Hozzon létre egy virtuális hálózati társviszony - különböző üzembe helyezési modellek és előfizetések

Ebben az oktatóanyagban elsajátíthatja toocreate egy virtuális hálózati társviszony-létesítés különböző üzembe helyezési modellel létrehozott virtuális hálózatok között. virtuális hálózatok hello különböző előfizetések szerepel. Társviszony-létesítési két virtuális hálózatok lehetővé teszi, hogy erőforrások egymással a különböző virtuális hálózatokon toocommunicate hello azonos sávszélesség és a késés, mintha hello erőforrások szerepeltek hello ugyanazt a virtuális hálózatot. További információ [virtuális hálózati társviszony-létesítés](virtual-network-peering-overview.md). 

hello lépéseket toocreate virtuális hálózati társviszony-létesítés eltérőek, attól függően, hogy hello virtuális hálózatok vannak-e hello azonos vagy eltérő, előfizetések, és amely [Azure telepítési modell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuális hálózatok jönnek létre. keresztül. Megtudhatja, hogyan toocreate a virtuális hálózati társviszony más esetekben hello forgatókönyv a következő táblázat hello kattintva:

|Azure üzembehelyezési modell  | Azure-előfizetés  |
|--------- |---------|
|[Mindkét erőforrás-kezelő](virtual-network-create-peering.md) |Azonos|
|[Mindkét erőforrás-kezelő](create-peering-different-subscriptions.md) |Különböző|
|[Egy erőforrás-kezelő egy klasszikus](create-peering-different-deployment-models.md) |Azonos|

Virtuális hálózati társviszony-létesítés nem hozható létre, hello klasszikus üzembe helyezési modellben telepített virtuális hálózatok között. Virtuális hálózati társviszony-létesítés csak hozhatók létre, amely szerepel hello azonos virtuális hálózatok között Azure-régiót. Ha egy virtuális hálózati társviszony-létesítés különböző előfizetésekhez, előfizetések lehet hello létező virtuális hálózatok közötti létrehozásának tartozó toohello azonos Azure Active Directory bérlői. Ha még nem rendelkezik egy Azure Active Directory-bérlőt, akkor gyorsan [hozzon létre egyet](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Ha tooconnect virtuális hálózatok mindkét létrehozott hello klasszikus üzembe helyezési modellel, keresztül, amely létezik a különböző Azure-régiók vagy, amely létezik az előfizetések társított toodifferent Azure Active Directory bérlők, használhatja az Azure [VPN-átjáró](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuális hálózatok. 

> [!WARNING]
> Virtuális hálózati társviszony-létesítés a virtuális hálózatok közötti létre különböző előfizetésekhez létező különböző Azure telepítési modellek létrehozása jelenleg előzetes verzió. Ebben a forgatókönyvben létrehozott virtuális hálózati társviszony nem rendelkezhet azonos szintű rendelkezésre állásának és megbízhatóságának létrehozása egy virtuális hálózati társviszony-létesítés forgatókönyvekben általános rendelkezésre állási kiadás hello. Ebben a forgatókönyvben létrehozott virtuális hálózati társviszony nem támogatottak, van, korlátozott képességeit, és előfordulhat, hogy nem érhető el az összes Azure-régiók. Az értesítésekhez hello legfrissebb a rendelkezésre állás és a szolgáltatás állapotának ellenőrzése hello [frissíti az Azure Virtual Network](https://azure.microsoft.com/updates/?product=virtual-network) lap.

Használhatja a hello [Azure-portálon](#portal), hello Azure [parancssori felület](#cli) (CLI), vagy Azure [PowerShell](#powershell) toocreate virtuális hálózati társviszony-létesítés. Kattintson bármelyik hello előző eszköz hivatkozások toogo, közvetlen toohello lépések létrehozása a virtuális hálózati társviszony-létesítés a kiválasztott eszköz segítségével.

## <a name="register"></a>Hello Preview regisztrálása

hello Preview tooregister, mindkét előfizetéshez hello virtuális hálózatokat tartalmazó követő lépéseket teljes hello toopeer szeretné. hello csak tooregister használható hello preview eszköze PowerShell.

1. Hello hello PowerShell legújabb verziójának telepítéséhez [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Ha új tooAzure PowerShell, lásd: [Azure PowerShell áttekintése](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Indítson el egy PowerShell-munkamenetet, és jelentkezzen be hello segítségével tooAzure `login-azurermaccount` parancsot.
3. Az előfizetés regisztrálása a hello preview hello a következő parancsok beírásával:

    ```powershell
    Register-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
    Ne hajtsa végre a cikk hello portál, az Azure parancssori felület vagy a PowerShell szakaszaiban hello lépéseit amíg hello **RegistrationState** követően a következő parancs hello megadása kimeneti **regisztrált** mindkét elő:

    ```powershell    
    Get-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    ```

## <a name="portal"></a>Hozzon létre a társviszony - Azure-portálon

Ez az oktatóanyag az egyes előfizetésekhez külön fiókot használja. Hello azonos fiókot használja minden lépést, hagyja ki a naplózás hello portálon kívül hello lépéseket és hello ugorja át egy másik felhasználói engedélyek toohello virtuális hálózatok hozzárendelése a tooboth előfizetések engedélyekkel rendelkező fiók használata esetén is használhatja. Hello lépések elvégzése előtt regisztrálnia kell az hello előzetes. tooregister, teljes hello szükséges lépések hello [hello Preview regisztrálása](#register) című szakaszát. Ne folytassa a hátralévő lépéseket, amíg mindkét előfizetéshez regisztrált hello Preview hello.
 
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) , "a" felhasználó. hello fiókkal jelentkezik be az hello szükséges engedélyek toocreate rendelkeznie kell egy virtuális hálózati társviszony-létesítés. Lásd: hello [engedélyek](#permissions) jelen cikkben alább szakasza.
2. Kattintson a **+ új**, kattintson a **hálózati**, majd kattintson a **virtuális hálózati**.
3. A hello **virtuális hálózat létrehozása** panelen adja meg, vagy válassza ki a következő beállítások hello értékeit, majd kattintson **létrehozása**:
    - **Név**: *myVnetA*
    - **Címtér**: *10.0.0.0/16*
    - **Alhálózati név**: *alapértelmezett*
    - **Alhálózati címtartományt**: *10.0.0.0/24*
    - **Előfizetés**: válassza ki az előfizetést azonosítójához.
    - **Erőforráscsoport**: válasszon **hozzon létre új** , és írja be *myResourceGroupA*
    - **Hely**: *USA keleti régiója*
4. A hello **keresési erőforrások** mezőt hello portálon típus hello tetején *myVnetA*. Kattintson a **myVnetA** amikor megjelenik a hello keresési eredmények között. A panel jelenik meg hello **myVnetA** virtuális hálózat.
5. A hello **myVnetA** panel, amelyen megjelenik, kattintson a **hozzáférés-vezérlés (IAM)** hello függőleges bal oldalán található hello panel hello beállítások listája.
6. A hello **myVnetA - hozzáférés-vezérlés (IAM)** panel, amelyen megjelenik, kattintson a **+ Hozzáadás**.
7. A hello **engedélyek hozzáadása** panelen megjelenő, válassza ki **hálózat közreműködő** a hello **szerepkör** mezőbe.
8. A hello **válasszon** mezőben válassza ki a "b" felhasználó, vagy írja be a "b" felhasználó tartozó e-mail cím toosearch azt. hello azoknak a felhasználóknak megjelenített származik hello azonos Azure Active Directory-bérlő hello virtuális hálózatként hoz létre hello társviszony-létesítés esetében. Amikor hello lista megjelenik, kattintson a "b" felhasználó.
9. Kattintson a **Save** (Mentés) gombra.
10. Jelentkezzen ki, "a" felhasználó hello portálon, majd jelentkezzen be "b" felhasználó.
11. Kattintson a **+ új**, típus *virtuális hálózati* a hello **keresési hello piactér** mezőbe, majd kattintson az **virtuális hálózati** hello keresési eredmények között .
12. A hello **virtuális hálózati** panelen megjelenő, válassza ki **klasszikus** a hello **telepítési modell kiválasztása** mezőbe, majd kattintson az **létrehozása**.
13.   Hello hozzon létre virtuális hálózat (klasszikus) mezőben, amely akkor jelenik meg adja meg a következő értékek hello:

    - **Név**: *myVnetB*
    - **Címtér**: *10.1.0.0/16*
    - **Alhálózati név**: *alapértelmezett*
    - **Alhálózati címtartományt**: *10.1.0.0/24*
    - **Előfizetés**: válassza ki az előfizetést a b kiszolgálóra.
    - **Erőforráscsoport**: válasszon **hozzon létre új** , és írja be *myResourceGroupB*
    - **Hely**: *USA keleti régiója*

14. A hello **keresési erőforrások** mezőt hello portálon típus hello tetején *myVnetB*. Kattintson a **myVnetB** amikor megjelenik a hello keresési eredmények között. A panel jelenik meg hello **myVnetB** virtuális hálózat.
15. A hello **myVnetB** panel, amelyen megjelenik, kattintson a **tulajdonságok** hello függőleges bal oldalán található hello panel hello beállítások listája. Másolás hello **erőforrás-azonosító**, amellyel egy későbbi lépésben. hello erőforrás-azonosító a következő példa hasonló toohello: /subscriptions/<Susbscription ID>/resourceGroups/myResoureGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
16. Végezze el myVnetB, írja be 5 – 9 lépéseket **"a" felhasználó** a 8. lépés.
17. Jelentkezzen ki, "b" felhasználó hello portálon, és jelentkezzen be "a" felhasználó.
18. A hello **keresési erőforrások** mezőt hello portálon típus hello tetején *myVnetA*. Kattintson a **myVnetA** amikor megjelenik a hello keresési eredmények között. A panel jelenik meg hello **myVnet** virtuális hálózat.
19. Kattintson a **myVnetA**.
20. A hello **myVnetA** panel, amelyen megjelenik, kattintson a **Társviszony** hello függőleges bal oldalán található hello panel hello beállítások listája.
21. A hello **myVnetA - esetében** panel, amelyen jelent meg, kattintson a **+ Hozzáadás**
22. A hello **Hozzáadás társviszony-létesítés** panelen megjelenő, adja meg, vagy válassza ki az alábbi beállítások hello, majd kattintson **OK**:
     - **Név**: *myVnetAToMyVnetB*
     - **Virtuális hálózat telepítési modell**: válasszon **klasszikus**.
     - **Tudható, hogy az erőforrás-azonosító**: ezt a jelölőnégyzetet.
     - **Erőforrás-azonosító**: Adja meg a myVnetB hello erőforrás-azonosítója a 15. lépéssel.
     - **Virtuális hálózati hozzáférés engedélyezése:** ügyeljen arra, hogy **engedélyezve** van kiválasztva.
    Ebben az oktatóanyagban nincs más beállítások használhatók. toolearn összes társviszony-létesítési beállításról, olvasni [kezelheti a virtuális hálózati társviszony](virtual-network-manage-peering.md#create-a-peering).
23. Kattintás után **OK** hello a korábbi lépésben hello **Hozzáadás társviszony-létesítés** panel bezárása után, és látni hello **myVnetA - Társviszony** újra a panelt. Néhány másodpercen belül létrehozott társviszony hello hello panel jelenik meg. **Csatlakoztatott** hello szerepel **társviszony-LÉTESÍTÉS állapot** hello oszlopában **myVnetAToMyVnetB** társviszony-létesítést, létre. hello társviszony-létesítés most létrejön. Nincs szükség toopeer hello virtuális hálózat (klasszikus) toohello virtuális hálózat (Resource Manager).

    Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával. Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti. Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló. Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

24. **Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.
25. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések hello [törli az erőforrást](#delete-portal) című szakaszát.

## <a name="cli"></a>Társviszony - létrehozása az Azure parancssori felület

Ez az oktatóanyag az egyes előfizetésekhez külön fiókot használja. Hello azonos fiókot használja minden lépést, hagyja ki hello lépéseket az Azure-ból a naplózás és eltávolítása hello parancssor által létrehozott felhasználói szerepkör-hozzárendelések tooboth előfizetések engedélyekkel rendelkező fiók használata esetén is használhatja. Cserélje le UserA@azure.com és UserB@azure.com összes parancsfájlok hello felhasználónevek a "a" felhasználó és a "b" felhasználó használata a következő hello. 

Hello lépések elvégzése előtt regisztrálnia kell az hello előzetes. tooregister, teljes hello szükséges lépések hello [hello Preview regisztrálása](#register) című szakaszát. Ne folytassa a hátralévő lépéseket, amíg mindkét előfizetéshez regisztrált hello Preview hello.

1. [Telepítés](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello Azure CLI 1.0 toocreate hello virtuális hálózat (klasszikus).
2. Nyisson meg egy parancssori munkamenetet, és jelentkezzen be tooAzure "b" felhasználó hello segítségével `azure login` parancsot.
3. Szolgáltatásfelügyelet módban hello CLI futtatásában hello `azure config mode asm` parancsot.
4. Adja meg a következő parancs toocreate hello virtuális hálózat (klasszikus) hello:
 
    ```azurecli
    azure network vnet create --vnet myVnetB --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```
5. hello hátralévő lépéseket el kell végezni a rendszerhéjakba használata az Azure parancssori felület 2.0.4 hello vagy újabb [telepített](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json), vagy a hello Azure Cloud rendszerhéj használatával. hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül. Hello Azure CLI előtelepített és konfigurált toouse-fiókjához van. Kattintson a hello **kipróbálás** hello gombjára a parancsfájlok, amely megnyit egy felhőalapú rendszerhéj, amelyre bejelentkezik, az Azure-fiók tooyour olvashat. Fut a beállítások bash parancssori felület parancsfájlok egy Windows-ügyfélen, a következő témakörben: [a Windows hello Azure CLI-t futtató](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 
6. Másolás hello alábbi parancsfájl tooa szövegszerkesztőben a számítógépen. Cserélje le `<SubscriptionB-Id>` az előfizetés-azonosítóval. Ha az előfizetés-azonosítója nem ismeri, adja meg a hello `az account show` parancsot. a következő hello **azonosító** hello kimeneti az előfizetés azonosítóját. Másolja hello módosító parancsfájlt, illessze be a parancssori felület 2.0 tooyour munkamenet, és nyomja le az `Enter`. 

    ```azurecli-interactive
    az role assignment create \
      --assignee UserA@azure.com \
      --role "Classic Network Contributor" \
      --scope /subscriptions/<SubscriptionB-Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

    Ha hello virtuális hálózat (klasszikus), a 4. lépésben létrehozott, Azure hello létrehoz hello virtuális hálózati *alapértelmezett-hálózat* erőforráscsoportot.
7. "A" felhasználó "b" felhasználó Azure és a bejelentkezés jelentkezzen a CLI 2.0 hello.
8. Hozzon létre egy erőforráscsoportot és egy virtuális hálózatot (Resource Manager). Másolás hello következő parancsfájl, beillesztheti tooyour CLI munkamenet és nyomja le az `Enter`. 

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout hello script.
    rgName="myResourceGroupA"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network A (Resource Manager).
    az network vnet create \
      --name myVnetA \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Get hello id for myVnetA.
    vNetAId=$(az network vnet show \
      --resource-group $rgName \
      --name myVnetA \
      --query id --out tsv)

    # Assign UserB permissions toomyVnetA.
    az role assignment create \
      --assignee UserB@azure.com \
      --role "Network Contributor" \
      --scope $vNetAId
    ```

9. Hozzon létre egy virtuális hálózati társviszony-létesítés hello két virtuális hálózatok hello különböző üzembe helyezési modellel létrehozott között. Másolás hello alábbi parancsfájl tooa szövegszerkesztőben a számítógépen. Cserélje le `<SubscriptionB-id>` az előfizetés azonosítóját. Ha az előfizetés-azonosítója nem ismeri, adja meg a hello `az account show` parancsot. a következő hello **azonosító** hello kimeneti az előfizetés azonosítóját. Azure létrehozott hello virtuális hálózat (klasszikus) az első lépésben létrehozott 4 erőforráscsoportban nevű *alapértelmezett-hálózat*. Illessze be a hello módosító parancsfájlt a CLI-munkamenetben, és nyomja le az `Enter`.

    ```azurecli-interactive
    # Peer VNet1 tooVNet2.
    az network vnet peering create \
      --name myVnetAToMyVnetB \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --remote-vnet-id  /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB \
      --allow-vnet-access
    ```

10. Miután hello parancsprogram végrehajtása során, tekintse át a hello társviszony-létesítés hello virtuális hálózat (Resource Manager). A következő parancsfájl hello másolja, és majd illessze be a CLI-munkamenetben:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --output table
    ```
    hello kimeneti jelennek meg **csatlakoztatva** a hello **PeeringState** oszlop.

    Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával. Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti. Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló. Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

11. **Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.
12. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések [törli az erőforrást](#delete-cli) ebben a cikkben.

## <a name="powershell"></a>Hozzon létre a társviszony - PowerShell

Ez az oktatóanyag az egyes előfizetésekhez külön fiókot használja. Hello azonos fiókot használja minden lépést, hagyja ki hello lépéseket az Azure-ból a naplózás és eltávolítása hello parancssor által létrehozott felhasználói szerepkör-hozzárendelések tooboth előfizetések engedélyekkel rendelkező fiók használata esetén is használhatja. Cserélje le UserA@azure.com és UserB@azure.com összes parancsfájlok hello felhasználónevek a "a" felhasználó és a "b" felhasználó használata a következő hello. 

Hello lépések elvégzése előtt regisztrálnia kell az hello előzetes. tooregister, teljes hello szükséges lépések hello [hello Preview regisztrálása](#register) című szakaszát. Ne folytassa a hátralévő lépéseket, amíg mindkét előfizetéshez regisztrált hello Preview hello.

1. Hello hello PowerShell legújabb verziójának telepítéséhez [Azure](https://www.powershellgallery.com/packages/Azure) és [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulok. Ha új tooAzure PowerShell, lásd: [Azure PowerShell áttekintése](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Indítson el egy PowerShell-munkamenetet.
3. A PowerShellben, jelentkezzen be a "b" felhasználó tooUserB tartozó előfizetés hello megadásával `Add-AzureAccount` parancsot.
4. toocreate egy virtuális hálózat (klasszikus) a PowerShell-lel, létre kell hoznia egy új, vagy módosíthatja egy meglévő hálózati konfigurációs fájlban. Ismerje meg, hogyan túl[exportálása, frissítése és a hálózati konfigurációs fájlok importálása a](virtual-networks-using-network-configuration-file.md). hello fájlt tartalmaznia kell következő hello **VirtualNetworkSite** elem hello ebben az oktatóanyagban használt virtuális hálózathoz:

    ```xml
    <VirtualNetworkSite name="myVnetB" Location="East US">
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

5. Jelentkezzen be a "b" felhasználó toouse erőforrás-kezelő parancsként tooUserB tartozó előfizetés hello megadása `login-azurermaccount` parancsot.
6. B. másolási hello alábbi parancsfájl-tooa szövegszerkesztőben a számítógépen, és cserélje hozzárendelése "a" felhasználó engedélyek toovirtual hálózati `<SubscriptionB-id>` hello azonosítójú előfizetés a b kiszolgálóra. Ha hello előfizetés-azonosítója nem ismeri, adja meg a hello `Get-AzureRmSubscription` parancs tooview azt. a következő hello **azonosító** hello vissza a kimeneti az előfizetés-azonosító. Azure létrehozott hello virtuális hálózat (klasszikus) az első lépésben létrehozott 4 erőforráscsoportban nevű *alapértelmezett-hálózat*. tooexecute hello parancsfájl, a Másolás hello módosítva a parancsfájl, illessze be tooPowerShell, és nyomja le az `Enter`.
    
    ```powershell 
    New-AzureRmRoleAssignment `
      -SignInName UserA@azure.com `
      -RoleDefinitionName "Classic Network Contributor" `
      -Scope /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

7. Jelentkezzen ki az Azure-bA biztosít, és jelentkezzen be "a" felhasználó tooUserA tartozó előfizetés hello megadásával `login-azurermaccount` parancsot. hello fiókkal jelentkezik be az hello szükséges engedélyek toocreate rendelkeznie kell egy virtuális hálózati társviszony-létesítés. Lásd: hello [engedélyek](#permissions) jelen cikkben alább szakasza.
8. Hozzon létre hello virtuális hálózatot (erőforrás-kezelő) parancsfájl, beillesztése tooPowerShell, majd nyomja le a következő hello másolásával `Enter`:

    ```powershell
    # Variables for common values
      $rgName='MyResourceGroupA'
      $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network A.
    $vnetA = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location
    ```

9. Rendelje hozzá a "b" felhasználó engedélyek toomyVnetA. Másolás hello alábbi parancsfájl-tooa szövegszerkesztőben a számítógépen, és cserélje le `<SubscriptionA-Id>` hello azonosítójú előfizetés azonosítójához. Ha hello előfizetés-azonosítója nem ismeri, adja meg a hello `Get-AzureRmSubscription` parancs tooview azt. a következő hello **azonosító** hello vissza a kimeneti az előfizetés-azonosító. Illessze be a hello hello parancsfájl módosított változatát a PowerShell, és nyomja le az `Enter` tooexecute azt.

    ```powershell
    New-AzureRmRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

10. Másolás hello alábbi parancsfájl-tooa szövegszerkesztőben a számítógépen, és cserélje `<SubscriptionB-id>` hello azonosítójú előfizetés b toopeer myVnetA toomyVNetB, másolja hello módosító parancsfájlt, illessze be tooPowerShell, és nyomja le az `Enter`.

    ```powershell
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vnetA `
      -RemoteVirtualNetworkId /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

11. Hello myVnetA társviszony-létesítési állapotának megtekintéséhez a következő parancsfájl illeszti be a PowerShell és az egyre erősebb hello másolásával `Enter`.

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName $rgName `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    hello állapota **csatlakoztatva**. Túl módosítja**csatlakoztatva** hello társviszony-létesítési toomyVnetA myVnetB a beállítása után.

    Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával. Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti. Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló. Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

12. **Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.
13. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések [törli az erőforrást](#delete-powershell) ebben a cikkben.

## <a name="permissions"></a>Engedélyek

hello fiókjai virtuális hálózati társviszony-létesítés toocreate hello szükséges szerepkör- vagy hozzáférési engedélyekkel kell rendelkeznie. Például, ha két virtuális hálózatok myVnetA és myVnetB nevű volt társviszony, a fiókjához társítva kell lenni a következő minimális szerepkör vagy minden egyes virtuális hálózati engedélyeinek hello:
    
|Virtuális hálózat|Üzemi modell|Szerepkör|Engedélyek|
|---|---|---|---|
|myVnetA|Resource Manager|[Hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klasszikus|[Klasszikus hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|N/A|
|myVnetB|Resource Manager|[Hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klasszikus|[Klasszikus hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

További információ [beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) és konkrét engedélyeket túl[egyéni szerepkörök](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (csak Resource Manager).

## <a name="delete"></a>Erőforrások törlése
Ez az oktatóanyag befejezése után, érdemes lehet toodelete hello erőforrások hello oktatóanyag, így a használati költségek létrehozott. Erőforráscsoport törlésekor a hello erőforráscsoportban található összes erőforrást is törlődnek.

### <a name="delete-portal"></a>Azure-portálon

1. Hello portál keresési mezőbe, írja be a **myResourceGroupA**. Hello keresési eredmények között kattintson **myResourceGroupA**.
2. A hello **myResourceGroupA** panelen kattintson a hello **törlése** ikon.
3. tooconfirm hello törlésre, hello **típus hello ERŐFORRÁSCSOPORT-név** adja meg a **myResourceGroupA**, és kattintson a **törlése**.
4. A hello **keresési erőforrások** mezőt hello portálon típus hello tetején *myVnetB*. Kattintson a **myVnetB** amikor megjelenik a hello keresési eredmények között. A panel jelenik meg hello **myVnetB** virtuális hálózat.
5. A hello **myVnetB** panelen kattintson a **törlése**.
6. tooconfirm hello törlését, kattintson a **Igen** a hello **virtuális hálózati Delete** mezőbe.

### <a name="delete-cli"></a>Az Azure parancssori felület

1. Jelentkezzen be tooAzure hello segítségével CLI 2.0 toodelete hello virtuális hálózat (erőforrás-kezelő) rendelkező hello a következő parancsot:

    ```azurecli-interactive
    az group delete --name myResourceGroupA --yes
    ```

2. Jelentkezzen be tooAzure hello használata Azure CLI 1.0 toodelete hello virtuális hálózat (klasszikus) a következő parancsok hello:

    ```azurecli
    azure config mode asm 

    azure network vnet delete --vnet myVnetB --quiet
    ```

### <a name="delete-powershell"></a>PowerShell

1. Hello PowerShell parancssorába adja meg a következő parancs toodelete hello virtuális hálózat (erőforrás-kezelő) hello:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupA -Force
    ```

2. toodelete hello virtuális hálózat (klasszikus) a PowerShell-lel, módosítania kell a hálózati konfigurációs fájlok. Ismerje meg, hogyan túl[exportálása, frissítése és a hálózati konfigurációs fájlok importálása a](virtual-networks-using-network-configuration-file.md). Távolítsa el a következő VirtualNetworkSite elem ebben az oktatóanyagban használt virtuális hálózat hello hello:

    ```xml
    <VirtualNetworkSite name="myVnetB" Location="East US">
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
