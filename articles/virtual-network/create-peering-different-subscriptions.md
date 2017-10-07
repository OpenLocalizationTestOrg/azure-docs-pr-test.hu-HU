---
title: "aaaCreate egy Azure virtuális hálózati társviszony - Resource Manager - különböző előfizetések |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy virtuális hálózati társviszony-létesítés virtuális hálózatok közötti létrehozott erőforrás-kezelővel, amely másik Azure-előfizetések szerepel."
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
ms.openlocfilehash: c7983a86031e061c1155144e5c493ee9578fa583
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---resource-manager-different-subscriptions"></a>Hozzon létre egy virtuális hálózati társviszony - erőforrás-kezelő különböző előfizetésekhez 

Ebben az oktatóanyagban elsajátíthatja toocreate egy virtuális hálózati társviszony-létesítés Resource Manager használatával létrehozott virtuális hálózatok között. virtuális hálózatok hello különböző előfizetések szerepel. Társviszony-létesítési két virtuális hálózatok lehetővé teszi, hogy erőforrások egymással a különböző virtuális hálózatokon toocommunicate hello azonos sávszélesség és a késés, mintha hello erőforrások szerepeltek hello ugyanazt a virtuális hálózatot. További információ [virtuális hálózati társviszony-létesítés](virtual-network-peering-overview.md). 

hello lépéseket toocreate virtuális hálózati társviszony-létesítés eltérőek, attól függően, hogy hello virtuális hálózatok vannak-e hello azonos vagy eltérő, előfizetések, és amely [Azure telepítési modell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuális hálózatok jönnek létre. keresztül. Megtudhatja, hogyan toocreate a virtuális hálózati társviszony más esetekben hello forgatókönyv a következő táblázat hello kattintva:

|Azure üzembehelyezési modell  | Azure-előfizetés  |
|--------- |---------|
|[Mindkét erőforrás-kezelő](virtual-network-create-peering.md) |Azonos|
|[Egy erőforrás-kezelő egy klasszikus](create-peering-different-deployment-models.md) |Azonos|
|[Egy erőforrás-kezelő egy klasszikus](create-peering-different-deployment-models-subscriptions.md) |Különböző|

Virtuális hálózati társviszony-létesítés nem hozható létre, hello klasszikus üzembe helyezési modellben telepített virtuális hálózatok között. Virtuális hálózati társviszony-létesítés csak hozhatók létre, amely szerepel hello azonos virtuális hálózatok között Azure-régiót. Ha egy virtuális hálózati társviszony-létesítés különböző előfizetésekhez, előfizetések lehet hello létező virtuális hálózatok közötti létrehozásának tartozó toohello azonos Azure Active Directory bérlői. Ha még nem rendelkezik egy Azure Active Directory-bérlőt, akkor gyorsan [hozzon létre egyet](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Ha tooconnect virtuális hálózatok mindkét létrehozott hello klasszikus üzembe helyezési modellel, keresztül, amely létezik a különböző Azure-régiók vagy, amely létezik az előfizetések társított toodifferent Azure Active Directory bérlők, használhatja az Azure [VPN-átjáró](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuális hálózatok. 

Használhatja a hello [Azure-portálon](#portal), hello Azure [parancssori felület](#cli) (CLI), vagy Azure [PowerShell](#powershell) toocreate virtuális hálózati társviszony-létesítés. Kattintson bármelyik hello előző eszköz hivatkozások toogo, közvetlen toohello lépések létrehozása a virtuális hálózati társviszony-létesítés a kiválasztott eszköz segítségével.

## <a name="portal"></a>Hozzon létre a társviszony - Azure-portálon

Ez az oktatóanyag az egyes előfizetésekhez külön fiókot használja. Hello azonos fiókot használja minden lépést, hagyja ki a naplózás hello portálon kívül hello lépéseket és hello ugorja át egy másik felhasználói engedélyek toohello virtuális hálózatok hozzárendelése a tooboth előfizetések engedélyekkel rendelkező fiók használata esetén is használhatja.

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
8. A hello **válasszon** mezőben jelölje be a "b" felhasználó, vagy írja be a "b" felhasználó tartozó e-mail cím toosearch azt. hello azoknak a felhasználóknak megjelenített származik hello azonos Azure Active Directory-bérlő hello virtuális hálózatként hoz létre hello társviszony-létesítés esetében.
9. Kattintson a **Save** (Mentés) gombra.
10. A hello **myVnetA - hozzáférés-vezérlés (IAM)** panelen kattintson a **tulajdonságok** hello függőleges bal oldalán található hello panel hello beállítások listája. Másolás hello **erőforrás-azonosító**, amellyel egy későbbi lépésben. hello erőforrás-azonosító a következő példa hasonló toohello: /subscriptions/<Subscription Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/virtualNetworks/myVnetA.
11. Jelentkezzen ki, "a" felhasználó hello portálon, majd jelentkezzen be "b" felhasználó.
12. 2-3, megadásával vagy kiválasztásával hello értékek a 3. lépésben a következő lépéseket:

    - **Név**: *myVnetB*
    - **Címtér**: *10.1.0.0/16*
    - **Alhálózati név**: *alapértelmezett*
    - **Alhálózati címtartományt**: *10.1.0.0/24*
    - **Előfizetés**: válassza ki az előfizetést a b kiszolgálóra.
    - **Erőforráscsoport**: válasszon **hozzon létre új** , és írja be *myResourceGroupB*
    - **Hely**: *USA keleti régiója*

13. A hello **keresési erőforrások** mezőt hello portálon típus hello tetején *myVnetB*. Kattintson a **myVnetB** amikor megjelenik a hello keresési eredmények között. A panel jelenik meg hello **myVnetB** virtuális hálózat.
14. A hello **myVnetB** panel, amelyen megjelenik, kattintson a **tulajdonságok** hello függőleges bal oldalán található hello panel hello beállítások listája. Másolás hello **erőforrás-azonosító**, amellyel egy későbbi lépésben. hello erőforrás-azonosító a következő példa hasonló toohello: /subscriptions/<Susbscription ID>/resourceGroups/myResoureGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB.
15. Kattintson a **hozzáférés-vezérlés (IAM)** a hello **myVnetB** panel megnyitásához, és ezután lépéseket 5-10 myVnetB, írja be a **"a" felhasználó** a 8. lépés.
16. Jelentkezzen ki, "b" felhasználó hello portálon, és jelentkezzen be "a" felhasználó.
17. A hello **keresési erőforrások** mezőt hello portálon típus hello tetején *myVnetA*. Kattintson a **myVnetA** amikor megjelenik a hello keresési eredmények között. A panel jelenik meg hello **myVnet** virtuális hálózat.
18. Kattintson a **myVnetA**.
19. A hello **myVnetA** panel, amelyen megjelenik, kattintson a **Társviszony** hello függőleges bal oldalán található hello panel hello beállítások listája.
20. A hello **myVnetA - esetében** panel, amelyen jelent meg, kattintson a **+ Hozzáadás**
21. A hello **Hozzáadás társviszony-létesítés** panelen megjelenő, adja meg, vagy válassza ki az alábbi beállítások hello, majd kattintson **OK**:
     - **Név**: *myVnetAToMyVnetB*
     - **Virtuális hálózat telepítési modell**: válasszon **erőforrás-kezelő**.
     - **Tudható, hogy az erőforrás-azonosító**: ezt a jelölőnégyzetet.
     - **Erőforrás-azonosító**: 14 lépésben adja meg a hello erőforrás-azonosító.
     - **Virtuális hálózati hozzáférés engedélyezése:** ügyeljen arra, hogy **engedélyezve** van kiválasztva.
    Ebben az oktatóanyagban nincs más beállítások használhatók. toolearn összes társviszony-létesítési beállításról, olvasni [kezelheti a virtuális hálózati társviszony](virtual-network-manage-peering.md#create-a-peering).
22. Kattintás után **OK** hello a korábbi lépésben hello **Hozzáadás társviszony-létesítés** panel bezárása után, és látni hello **myVnetA - Társviszony** újra a panelt. Néhány másodpercen belül létrehozott társviszony hello hello panel jelenik meg. **Kezdeményezett** hello szerepel **társviszony-LÉTESÍTÉS állapot** hello oszlopában **myVnetAToMyVnetB** társviszony-létesítés meg létrehozni. Már nincsenek társviszonyban myVnetA toomyVnetB, de most myVnetB toomyVnetA kell partnert. hello társviszony-létesítés léteznie kell mindkét irányban hello virtuális hálózatok toocommunicate tooenable erőforrások egymással.
23. Jelentkezzen ki hello portált mint "a" felhasználó, és jelentkezzen be, mint a "b" felhasználó.
24. Végezze el újra az myVnetB 17-21 lépéseket. 21. lépésben, hello társviszony-létesítés neve *myVnetBToMyVnetA*, jelölje be *myVnetA* a **virtuális hálózati**, és adjon meg azonosító hello hello 10 lépés **erőforrás-azonosító** mezőbe.
25. Néhány másodpercen belül kattintás után **OK** toocreate hello társviszony-létesítés myVnetB, a hello **myVnetBToMyVnetA** társviszony-létesítést az imént létrehozott szerepel **csatlakoztatva** a hello  **Társviszony-LÉTESÍTÉS állapot** oszlop.
26. Jelentkezzen ki, "b" felhasználó hello portálon, és jelentkezzen be "a" felhasználó.
27. Végezze el újra a 17-19 lépéseket. Hello **társviszony-LÉTESÍTÉS állapot** a hello **myVnetAToVNetB** társviszony-létesítés már is **csatlakoztatva**. hello társviszony-létesítés sikeresen létrejött, miután látta, **csatlakoztatva** a hello **társviszony-LÉTESÍTÉS állapot** mindkét virtuális hálózat hello társviszony-létesítés oszlopában. Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával. Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti. Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló. Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
28. **Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.
29. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések hello [törli az erőforrást](#delete-portal) című szakaszát.

## <a name="cli"></a>Társviszony - létrehozása az Azure parancssori felület

Ez az oktatóanyag az egyes előfizetésekhez külön fiókot használja. Hello azonos fiókot használja minden lépést, hagyja ki hello lépéseket az Azure-ból a naplózás és eltávolítása hello parancssor által létrehozott felhasználói szerepkör-hozzárendelések tooboth előfizetések engedélyekkel rendelkező fiók használata esetén is használhatja. Cserélje le UserA@azure.com és UserB@azure.com összes parancsfájlok hello felhasználónevek a "a" felhasználó és a "b" felhasználó használata a következő hello.

a következő parancsfájl hello:

- Szükséges hello Azure CLI 2.0.4 verzió vagy újabb. toofind hello verzió, futtassa `az --version`. Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
- A rendszerhéjakba működik. Az Azure parancssori felület parancsfájlok futtatásához a Windows-ügyfelén beállítások, lásd: [a Windows hello Azure CLI-t futtató](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Hello CLI és függőségeinek telepítése, helyett hello Azure Cloud rendszerhéj is használhatja. hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül. Hello Azure CLI előtelepített és konfigurált toouse-fiókjához van. Kattintson a hello **kipróbálás** hello parancsfájlt, amely a következő, hívja meg a felhő rendszerhéjat, amely bejelentkezhet tooyour az Azure-fiók gombjára. 

1. Nyisson meg egy parancssori munkamenetet, és jelentkezzen be tooAzure "a" hello segítségével felhasználó `azure login` parancsot. hello fiókkal jelentkezik be az hello szükséges engedélyek toocreate rendelkeznie kell egy virtuális hálózati társviszony-létesítés. Lásd: hello [engedélyek](#permissions) jelen cikkben alább szakasza.
2. Parancsfájl tooa szövegszerkesztőben a számítógépen a következő hello másolja, hogy lecseréli `<SubscriptionA-Id>` a hello azonosítója a SubscriptionA, majd másolási hello parancsfájl módosított, illessze be a parancssori munkamenetet, és nyomja le az `Enter`. Ha az előfizetés-azonosítója nem ismeri, adja meg a hello "az fiók megjelenítése" parancsot. a következő hello **azonosító** hello kimeneti az előfizetés azonosítóját.

    ```azurecli-interactive
    # Create a resource group.
    az group create \
      --name myResourceGroupA \
      --location eastus

    # Create virtual network A.
    az network vnet create \
      --name myVnetA \
      --resource-group myResourceGroupA \
      --location eastus \
      --address-prefix 10.0.0.0/16

    # Assign UserB permissions toovirtual network A.
    az role assignment create \
      --assignee UserB@azure.com \
      --role "Network Contributor" \
      --scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```
    
     "b" felhasználó hello engedély hozzárendelése nélkül is működik. Társviszony-létesítés hozhatók létre még akkor is, ha a felhasználók külön-külön emelje a saját virtuális hálózataikhoz társviszony-létesítési kérelmek mindaddig, amíg hello kérelmek egyezés. MyVNetB kiemelt felhasználó hozzáadása a helyi virtuális hálózat hello hálózati közreműködőként teszi egyszerűbbé toodo hello beállítása.
3. Jelentkezzen az Azure-ból "a" hello segítségével felhasználó `az logout` parancsot, majd jelentkezzen be, "b" felhasználó tooAzure. hello fiókkal jelentkezik be az hello szükséges engedélyek toocreate rendelkeznie kell egy virtuális hálózati társviszony-létesítés. Lásd: hello [engedélyek](#permissions) jelen cikkben alább szakasza.
4. Hozzon létre myVnetB. Hello parancsfájl tartalom másolása. lépés 2 tooa szövegszerkesztőben a számítógépen. Cserélje le `<SubscriptionA-Id>` az azonosítója a SubscriptionB hello. 10.0.0.0/16 too10.1.0.0/16, tooB, az összes módosítás és az összes Bs tooA módosítása. Másolja hello módosító parancsfájlt, majd illessze be a tooyour parancssori munkamenetet, és nyomja le az `Enter`. 
5. Jelentkezzen ki az Azure-bA biztosít, és jelentkezzen be, "a" felhasználó tooAzure.
6. Társviszony-létesítés myVnetA toomyVnetB a virtuális hálózat létrehozása. Másolja a parancsfájl tartalmát tooa szövegszerkesztőben a számítógépen a következő hello. Cserélje le `<SubscriptionB-Id>` az azonosítója a SubscriptionB hello. tooexecute hello parancsfájl, másolja hello módosító parancsfájlt, illessze be a parancssori felület munkamenet, és nyomja le az ENTER billentyűt.
 
    ```azurecli-interactive
        # Get hello id for myVnetA.
        vnetAId=$(az network vnet show \
          --resource-group myResourceGroupA \
          --name myVnetA \
          --query id --out tsv)
    
        # Peer myVNetA toomyVNetB.
        az network vnet peering create \
          --name myVnetAToMyVnetB \
          --resource-group myResourceGroupA \
          --vnet-name myVnetA \
          --remote-vnet-id /subscriptions/<SubscriptionB-Id>/resourceGroups/myResourceGroupB/providers/Microsoft.Network/VirtualNetworks/myVnetB \
          --allow-vnet-access
    ```

7. Hello myVnetA társviszony-létesítési állapotának megtekintése.

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroupA \
      --vnet-name myVnetA \
      --output table
    ```

    hello állapota **kezdeményezett**. Túl módosítja**csatlakoztatva** hello társviszony-létesítési toomyVnetA myVnetB a létrehozásuk után.

8. Jelentkezzen ki "a" felhasználó az Azure-ból, és jelentkezzen be, "b" felhasználó tooAzure.
9. Hozzon létre hello myVnetB toomyVnetA a társviszony-létesítés. Hello parancsfájl tartalom másolása lépés 6 tooa szövegszerkesztőben a számítógépen. Cserélje le `<SubscriptionB-Id>` hello azonosítójú SubscriptionA és tooB és minden Bs tooA, minden módosítása. Hello módosításokat hajtott végre, ha a Másolás hello parancsfájl módosított, illessze be a parancssori munkamenetet, és nyomja le az `Enter`.
10. Hello myVnetB társviszony-létesítési állapotának megtekintése. Hello parancsfájl tartalom másolása lépés 7 tooa szövegszerkesztőben a számítógépen. Egy tooB hello erőforráscsoport és a virtuális hálózat nevének módosítása, hello parancsfájl másolása, illessze be a hello módosító parancsfájlt tooyour CLI munkamenetben, és nyomja le az `Enter`. hello társviszony-létesítési állapota **csatlakoztatva**. túl hello myVnetA változások társviszony-létesítési állapot**csatlakoztatva** hello társviszony-létesítés myVnetB toomyVnetA a létrehozása után. Bejelentkezhet "a" felhasználó vissza tooAzure és a Kész lépés 7 újra tooverify hello társviszony-létesítési állapotát myVnetA. 

    > [!NOTE]
    > hello társviszony-létesítés nincs létrehozva, amíg a hello társviszony-létesítési állapot **csatlakoztatva** mindkét virtuális hálózat számára.

11. **Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.
12. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések [törli az erőforrást](#delete-cli) ebben a cikkben.

Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával. Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti. Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló. Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
 
## <a name="powershell"></a>Hozzon létre a társviszony - PowerShell

Ez az oktatóanyag az egyes előfizetésekhez külön fiókot használja. Hello azonos fiókot használja minden lépést, hagyja ki hello lépéseket az Azure-ból a naplózás és eltávolítása hello parancssor által létrehozott felhasználói szerepkör-hozzárendelések tooboth előfizetések engedélyekkel rendelkező fiók használata esetén is használhatja. Cserélje le UserA@azure.com és UserB@azure.com összes parancsfájlok hello felhasználónevek a "a" felhasználó és a "b" felhasználó használata a következő hello.

1. Hello hello PowerShell legújabb verziójának telepítéséhez [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Ha új tooAzure PowerShell, lásd: [Azure PowerShell áttekintése](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Indítson el egy PowerShell-munkamenetet.
3. A PowerShellben, jelentkezzen be tooAzure "a" felhasználó hello megadásával `login-azurermaccount` parancsot. hello fiókkal jelentkezik be az hello szükséges engedélyek toocreate rendelkeznie kell egy virtuális hálózati társviszony-létesítés. Lásd: hello [engedélyek](#permissions) jelen cikkben alább szakasza.
4. Hozzon létre egy erőforráscsoport és a virtuális hálózat A. másolási hello parancsfájl tooa szöveg a következő szerkesztő a számítógépen. Cserélje le `<SubscriptionA-Id>` az azonosítója a SubscriptionA hello. Ha az előfizetés-azonosítója nem ismeri, adja meg a hello `Get-AzureRmSubscription` parancs tooview azt. a következő hello **azonosító** hello vissza a kimeneti az előfizetés-azonosító. tooexecute hello parancsfájl, a Másolás hello módosítva a parancsfájl, illessze be tooPowerShell, és nyomja le az `Enter`.

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name MyResourceGroupA `
      -Location eastus

    # Create virtual network A.
    $vNetA = New-AzureRmVirtualNetwork `
      -ResourceGroupName MyResourceGroupA `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus

    # Assign UserB permissions toomyVnetA.
    New-AzureRmRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

    "b" felhasználó hello engedély hozzárendelése nélkül is működik. Társviszony-létesítés hozhatók létre még akkor is, ha a felhasználók külön-külön emelje a saját virtuális hálózataikhoz társviszony-létesítési kérelmek mindaddig, amíg hello kérelmek egyezés. Hello kiemelt felhasználó hozzáadása más felhasználóként hello helyi virtuális hálózatot a virtuális hálózat teszi egyszerűbbé toodo hello beállítása.
5. Jelentkezzen ki "a" felhasználó az Azure-ból, és jelentkezzen be a "b" felhasználó. hello fiókkal jelentkezik be az hello szükséges engedélyek toocreate rendelkeznie kell egy virtuális hálózati társviszony-létesítés. Lásd: hello [engedélyek](#permissions) jelen cikkben alább szakasza.
6. Hello parancsfájl tartalom másolása lépés 4 tooa szövegszerkesztőben a számítógépen. Cserélje le `<SubscriptionA-Id>` hello azonosítójú előfizetés b módosítás 10.0.0.0/16 too10.1.0.0/16. Módosítsa az összes, tooB és minden Bs tooA. tooexecute hello parancsfájl, másolása hello módosító parancsfájlt, illessze be a PowerShell és nyomja le az `Enter`.
7. Jelentkezzen ki a "b" felhasználó az Azure-ból, és jelentkezzen be a "a" felhasználó.
8. Hozzon létre hello myVnetA toomyVnetB a társviszony-létesítés. Másolás hello alábbi parancsfájl tooa szövegszerkesztőben a számítógépen. Cserélje le `<SubscriptionB-Id>` előfizetés B. tooexecute hello parancsfájl hello azonosítójú másolja hello módosító parancsfájlt, illessze be a tooPowerShell, és nyomja le az `Enter`.
 
    ```powershell
    # Peer myVnetA toomyVnetB.
    $vNetA=Get-AzureRmVirtualNetwork -Name myVnetA -ResourceGroupName myResourceGroupA
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vNetA `
      -RemoteVirtualNetworkId "/subscriptions/<SubscriptionB-Id>/resourceGroups/myResourceGroupB/providers/Microsoft.Network/virtualNetworks/myVnetB"
    ```

9. Hello myVnetA társviszony-létesítési állapotának megtekintése.

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroupA `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    hello állapota **kezdeményezett**. Túl módosítja**csatlakoztatva** hello társviszony-létesítési toomyVnetA myVnetB a beállítása után.

10. Jelentkezzen ki "a" felhasználó az Azure-ból, és jelentkezzen be a "b" felhasználó.
11. Hozzon létre hello myVnetB toomyVnetA a társviszony-létesítés. Hello parancsfájl tartalom másolása lépés 8 tooa szövegszerkesztőben a számítógépen. Cserélje le `<SubscriptionB-Id>` hello azonosítójú előfizetés A, és módosítsa az összes, az A tooB és az összes B tooA. tooexecute hello parancsfájl, a Másolás hello módosítva a parancsfájl, illessze be tooPowerShell, és nyomja le az `Enter`.
12. Hello myVnetB társviszony-létesítési állapotának megtekintése. Hello parancsfájl tartalom másolása lépés 9 tooa szövegszerkesztőben a számítógépen. Egy tooB hello erőforráscsoport és a virtuális hálózat nevének módosítása tooexecute hello parancsfájl, PowerShell parancsfájl módosított hello beillesztése, és nyomja le az `Enter`. hello állapota **csatlakoztatva**. a társviszony-létesítési állapot hello **myVnetA** túl változik**csatlakoztatva** hello társviszony-létesítés a létrehozása után **myVnetB** túl**myVnetA**. Bejelentkezhet "a" felhasználó vissza tooAzure és a Kész lépés 9 újra tooverify hello társviszony-létesítési állapotát myVnetA. 

    > [!NOTE]
    > hello társviszony-létesítés nincs létrehozva, amíg a hello társviszony-létesítési állapot **csatlakoztatva** mindkét virtuális hálózat számára.

    Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával. Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti. Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló. Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

13. **Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.
14. **Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések [törli az erőforrást](#delete-powershell) ebben a cikkben.

## <a name="permissions"></a>Engedélyek

hello fiókjai virtuális hálózati társviszony-létesítés toocreate hello szükséges szerepkör- vagy hozzáférési engedélyekkel kell rendelkeznie. Ha Ön volt társviszony-létesítés két virtuális hálózat neve például **myVnetA** és **myVnetB**, a fiókot hozzá kell rendelni a következő minimális szerepkör vagy minden egyes virtuális hálózati engedélyeinek hello:
    
|Virtuális hálózat|Szerepkör|Engedélyek|
|---|---|---|
|myVnetA|[Hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
|myVnetB|[Hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|

További információ [beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) és konkrét engedélyeket túl[egyéni szerepkörök](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (csak Resource Manager).

## <a name="delete"></a>Erőforrások törlése
Ez az oktatóanyag befejezése után, érdemes lehet toodelete hello erőforrások hello oktatóanyag, így a használati költségek létrehozott. Erőforráscsoport törlésekor a hello erőforráscsoportban található összes erőforrást is törlődnek.

### <a name="delete-portal"></a>Azure-portálon

1. Jelentkezzen be toohello Azure portal "a" felhasználó.
2. Hello portál keresési mezőbe, írja be a **myResourceGroupA**. Hello keresési eredmények között kattintson **myResourceGroupA**.
3. A hello **myResourceGroupA** panelen kattintson a hello **törlése** ikon.
4. tooconfirm hello törlésre, hello **típus hello ERŐFORRÁSCSOPORT-név** adja meg a **myResourceGroupA**, és kattintson a **törlése**.
5. Jelentkezzen ki hello portált mint "a" felhasználó, és jelentkezzen be, mint a "b" felhasználó.
6. Végezze el myResourceGroupB 2 – 4 lépéseket.

### <a name="delete-cli"></a>Az Azure parancssori felület

1. Jelentkezzen be tooAzure, "a" felhasználó, és hajtsa végre a következő parancs hello:

    ```azurecli-interactive
    az group delete --name myResourceGroupA --yes
    ```
2. Jelentkezzen ki az Azure-bA "a" felhasználó, és jelentkezzen be, mint a "b" felhasználó.
3. A következő parancs hello hajtható végre:

    ```azurecli-interactive
    az group delete --name myResourceGroupB --yes
    ```

### <a name="delete-powershell"></a>PowerShell

1. Jelentkezzen be tooAzure, "a" felhasználó, és hajtsa végre a következő parancs hello:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupA -force
    ```

2. Jelentkezzen ki az Azure-bA "a" felhasználó, és jelentkezzen be, mint a "b" felhasználó.
3. A következő parancs hello hajtható végre:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupB -force
    ```

## <a name="next-steps"></a>Következő lépések

- Alaposan olvassa el a fontos [virtuális hálózati társviszony-létesítési korlátozások és viselkedéshez](virtual-network-manage-peering.md#requirements-and-constraints) társviszony-létesítés üzemi virtuális hálózat létrehozása előtt használja.
- További tudnivalók az összes [virtuális hálózati társviszony-létesítési beállítások](virtual-network-manage-peering.md#create-a-peering).
- Ismerje meg, hogyan túl[létrehoz egy központot, és a hálózati topológia irány](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) a virtuális hálózati társviszony-létesítés.
