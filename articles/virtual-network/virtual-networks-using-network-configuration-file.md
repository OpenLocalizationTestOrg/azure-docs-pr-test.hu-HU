---
title: "az Azure virtuális hálózat (klasszikus) - hálózati konfigurációs fájl aaaConfigure |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate, és módosítsa a virtuális hálózatok (klasszikus), módosítása, importálása és exportálása révén a hálózati konfigurációs fájlt."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 009108d4315b4b7146d3f1cf2436ee211d2d89ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a>A hálózati konfigurációs fájl segítségével virtuális hálózat (klasszikus) konfigurálása
> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello erőforrás-kezelő telepítési modellt használja.

Hozzon létre, és konfigurálja a virtuális hálózat (klasszikus) a hálózati konfigurációs fájllal hello Azure parancssori felület (CLI) 1.0-s vagy az Azure PowerShell használatával. Nem lehet létrehozni vagy módosítani a virtuális hálózaton hello Azure Resource Manager telepítési modell hálózati konfigurációs fájl használatával. Ön nem lehet nem hello Azure portál toocreate vagy módosítsa a virtuális hálózat (klasszikus) konfigurációs fájlt, azonban használhat hello Azure portál toocreate egy virtuális hálózat (klasszikus), a hálózati konfigurációs fájl használata nélkül.

Létrehozása és konfigurálása a virtuális hálózat (klasszikus) a hálózati konfigurációs fájllal szükséges exportálása, módosítása és hello fájl importálásakor.

## <a name="export"></a>A hálózati konfigurációs fájl exportálása

Használhatja a PowerShell vagy a hello Azure CLI tooexport hálózati konfigurációs fájlt. PowerShell exportálása az XML-fájl, hello Azure parancssori felület egy json-fájl exportálása során.

### <a name="powershell"></a>PowerShell
 
1. [Azure PowerShell telepítése, és jelentkezzen be tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Módosítsa a könyvtárat hello (és győződjön meg arról, hogy létezik-e) és a következő parancs megfelelő, majd futtassa hello parancs tooexport hello hálózati konfigurációs fájl nevét a hello:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure CLI 1.0

1. [Telepítse az Azure CLI 1.0 hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Végezze el a hátralévő lépéseket az Azure CLI 1.0 parancssorból hello.
2. Jelentkezzen be tooAzure hello megadásával `azure login` parancsot.
3. Nyissa meg a címterület-kezelési mód hello megadásával `azure config mode asm` parancsot.
4. Módosítsa a könyvtárat hello (és győződjön meg arról, hogy létezik-e) és a következő parancs megfelelő, majd futtassa hello parancs tooexport hello hálózati konfigurációs fájl nevét a hello:
    
    ```azurecli
    azure network export c:\azure\networkconfig.json
    ```

## <a name="create-or-modify-a-network-configuration-file"></a>Hozzon létre vagy módosítsa a hálózati konfigurációs fájlt.

A hálózati konfigurációs fájl XML-fájl (amikor a PowerShell használatával) vagy egy json-fájl (használata esetén hello Azure CLI). Hello fájl bármely szöveg vagy XML/json-szerkesztőben szerkesztheti. Hello [hálózati konfigurációs fájl séma beállításai](https://msdn.microsoft.com/library/azure/jj157100.aspx) cikk tartalmazza az összes beállítást. Hello beállítások további ismertetése [virtuális hálózatok és beállítások megtekintéséhez](virtual-network-manage-network.md#view-vnet). hello módosításokat toohello fájlt:

- Meg kell felelnie a hello séma vagy importáló hello hálózati konfigurációs fájl sikertelen lesz.
- Felülírja a meglévő hálózati beállításait az előfizetés, ezért rendkívül körültekintően járjon el módosításakor. Például hivatkozás hello példa hálózati konfigurációs fájlok kövesse. Tegyük fel például, hello eredeti fájlban szereplő két **VirtualNetworkSite** példányok, és megváltozott, hello példákban látható módon. Hello fájl importálásakor Azure törli a virtuális hálózat hello hello **VirtualNetworkSite** példány eltávolította a hello fájlban. Egyszerűsített ebben a forgatókönyvben azt feltételezi, hogy egyetlen erőforrás volt hello virtuális hálózatban, mintha, hello virtuális hálózat nem törölhető, és hello importálása sikertelen lesz.

> [!IMPORTANT]
> Azure úgy ítéli meg, olyan alhálózatot, amely rendelkezik egy telepített, tooit **használatban**. Ha egy alhálózat használatban van, nem lehet módosítani. Módosítása az alhálózati adatokat a hálózati konfigurációs fájlban, előtt helyezze át, amelyeket központilag telepített toohello alhálózati tooa másik alhálózatot, amely nem módosított. Lásd: [áthelyezése egy másik alhálózat virtuális gép vagy Szerepkörpéldány tooa](virtual-networks-move-vm-role-to-subnet.md) részleteiről.

### <a name="example-xml-for-use-with-powershell"></a>Példa XML PowerShell való használatra

hello következő hálózati konfigurációs szakasz létrehoz egy virtuális hálózatot nevű *myVirtualNetwork* a cím rendelkező *10.0.0.0/16* a hello *USA keleti régiója* Azure a régióban. hello virtuális hálózat nevű egy alhálózatot tartalmaz *mySubnet* rendelkező egy címelőtagot *10.0.0.0/24*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
  <VirtualNetworkConfiguration>
    <Dns />
    <VirtualNetworkSites>
      <VirtualNetworkSite name="myVirtualNetwork" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="mySubnet">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
  </VirtualNetworkConfiguration>
</NetworkConfiguration>
```

Exportált hello hálózati konfigurációs fájlt nem tartalma tartalmaz, az előző példában hello hello XML másolja, és illessze be egy új fájlt.

### <a name="example-json-for-use-with-hello-azure-cli-10"></a>Példa JSON hello Azure CLI 1.0 való használatra

hello következő hálózati konfigurációs szakasz létrehoz egy virtuális hálózatot nevű *myVirtualNetwork* a cím rendelkező *10.0.0.0/16* a hello *USA keleti régiója* Azure a régióban. hello virtuális hálózat nevű egy alhálózatot tartalmaz *mySubnet* rendelkező egy címelőtagot *10.0.0.0/24*.

```json
{
   "VirtualNetworkConfiguration" : {
      "Dns" : "",
      "VirtualNetworkSites" : [
         {
            "AddressSpace" : [ "10.0.0.0/16" ],
            "Location" : "East US",
            "Name" : "myVirtualNetwork",
            "Subnets" : [
               {
                  "AddressPrefix" : "10.0.0.0/24",
                  "Name" : "mySubnet"
               }
            ]
         }
      ]
   }
}
```

Exportált hello hálózati konfigurációs fájlt nem tartalma tartalmaz, az előző példában hello hello json másolja, és illessze be egy új fájlt.

## <a name="import"></a>A hálózati konfigurációs fájl importálása

Használhatja a PowerShell vagy a hello Azure CLI tooimport hálózati konfigurációs fájlt. PowerShell importálja az XML-fájlba, amíg hello Azure CLI importálja a json-fájl. Ha hello importálás sikertelen lesz, győződjön meg arról, hello fájl megfelel a hello [hálózati konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

### <a name="powershell"></a>PowerShell
 
1. [Azure PowerShell telepítése, és jelentkezzen be tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Hello directory és a fájlnév szükség szerint parancs a következő hello módosítsa, majd futtassa hello parancs tooimport hello hálózati konfigurációs fájlt:
 
    ```powershell
    Set-AzureVNetConfig  -ConfigurationPath c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure CLI 1.0

1. [Telepítse az Azure CLI 1.0 hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Végezze el a hátralévő lépéseket az Azure CLI 1.0 parancssorból hello.
2. Jelentkezzen be tooAzure hello megadásával `azure login` parancsot.
3. Nyissa meg a címterület-kezelési mód hello megadásával `azure config mode asm` parancsot.
4. Hello directory és a fájlnév szükség szerint parancs a következő hello módosítsa, majd futtassa hello parancs tooimport hello hálózati konfigurációs fájlt:

    ```azurecli
    azure network import c:\azure\networkconfig.json
    ```
