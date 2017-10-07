---
title: "PowerShell-lel több hálózati adapterrel rendelkező virtuális gép (klasszikus) aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és a PowerShell segítségével több hálózati adapterrel rendelkező virtuális gépek konfigurálása."
services: virtual-network, virtual-machines
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a1a3952c-2dcc-4977-bd7a-52d623c1fb07
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 8ef35bd4cfd7e6a527080f1cfc541275ca86f5e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics"></a>Több hálózati adapterrel rendelkező virtuális gép (klasszikus) létrehozása
Virtuális gépek (VM) létrehozása az Azure-ban, és csatlakoztassa a virtuális gépek több hálózati adapterek (NIC) tooeach. Több hálózati adapter áll számos hálózati virtuális készülékeket, például az alkalmazások biztosításán és WAN-optimalizálást megoldások követelményeit. Több hálózati adapterrel is közötti hálózati forgalom elkülönítést biztosítani.

![Több hálózati adapter a virtuális gép](./media/virtual-networks-multiple-nics/IC757773.png)

hello. ábra azt mutatja be a három hálózati adapterrel rendelkező virtuális gépet, tooa eltérő alhálózathoz csatlakoznak.

> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén használja a Resource Manager.

* Az Internet felé néző VIP (klasszikus üzembe helyezés) csak akkor támogatott a hello "alapértelmezett" hálózati adaptert. Nincs a csak egy VIP toohello IP-címe hello alapértelmezett hálózati adaptert.
* Ilyenkor (klasszikus üzembe helyezés) példány szint nyilvános IP (LPIP) címek nem támogatottak a több hálózati adapter virtuális gépeket.
* hello sorrendjének hello hálózati adaptert a virtuális gép véletlenszerű lesz, és különböző Azure-infrastruktúra frissítéseket is módosulhatnak hello belül. Azonban hello IP-címeket, és megfelelő ethernet MAC hello megmarad címek hello azonos. Tegyük fel például, **Eth1** ; 10.1.0.100 IP-cím és MAC-cím 00-0D-3A-B0-39-0D tartalmaz, az Azure infrastruktúra-frissítési és újraindítás után az módosítható túl**Eth2**, de hello az IP-cím és MAC párosítás lesz továbbra is hello azonos. Ha újraindításra az ügyfél által kezdeményezett, hello NIC rendelés marad hello azonos.
* hello minden egyes virtuális Gépen lévő hálózati adapter címe kell lennie az alhálózat, több hálózati adaptert egy virtuális gép egyes hozzárendelhetők legyenek címek hello ugyanazon az alhálózaton.
* Virtuálisgép-méretet hello határozza meg, hogy hello, létrehozhat egy virtuális hálózati Adapterrel. Hivatkozás hello [Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) virtuális gép méretének cikkek toodetermine hány hálózati ADAPTERT támogat minden egyes Virtuálisgép-méretet. 

## <a name="network-security-groups-nsgs"></a>Hálózati biztonsági csoportokkal (NSG-k)
Egy erőforrás-kezelő telepítése esetén a a hálózati adapterek, a virtuális gép is lehet társítani, a hálózati biztonsági csoport (NSG), például minden hálózati adaptert egy virtuális gépen, amelyen engedélyezve van több hálózati adapter. Ha egy hálózati adapter hozzá van rendelve egy címet, ahol hello alhálózat társítva van egy NSG-t egy alhálózaton belül, majd hello hello alhálózati NSG szabályait is érvényesek toothat hálózati adaptert. Továbbá tooassociating alhálózatokon az NSG-ket is társíthat egy hálózati adapter egy NSG.

Ha egy alhálózat társítva egy NSG-t, és egy hálózati adapter belül alhálózaton külön-külön társítva van egy NSG-t, hello társított NSG-szabályok érvényesek a **flow-rendelési** toohello forgalom iránya a hello átadta-e be vagy ki szerint a hálózati adapter hello:

* **Bejövő forgalom** először áthaladó amelyek célja az adott hálózati hello hello alhálózati hello alhálózati NSG-szabályok előtt hello NIC történő továbbításához, majd hello NIC NSG-szabályok kiváltó váltanak.
* **Kimenő forgalom** hello NIC kiváltó hello NIC NSG-szabályok előtt hello alhálózati áthaladó, majd kiváltó hello alhálózati NSG-szabályok az első kimeneti amelynek forrása hello az adott hálózati forgalmat.

További információ [hálózati biztonsági csoportok](virtual-networks-nsg.md) és társítások toosubnets, a virtuális gépek és a hálózati adapterek alkalmazásának módja alapján...

## <a name="how-tooconfigure-a-multi-nic-vm-in-a-classic-deployment"></a>Hogyan tooConfigure egy hálózati adapter több virtuális Gépre kiterjedő a klasszikus telepítési
az alábbi utasítások hello segítségével hozhat létre egy több hálózati adapter virtuális gép 3 hálózati adaptert tartalmazó: az alapértelmezett hálózati adapter és két további hálózati adapterek. hello konfigurációs lépéseket hozza létre a virtuális gépek toohello szolgáltatás konfigurációs fájl töredék alábbi megfelelően kell konfigurálni:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over hello remainder section …
    </VirtualNetworkSite>


Előfeltételek toorun hello PowerShell-parancsok hello példa próbálkozás előtt a következő hello van szüksége.

* Azure-előfizetés.
* A konfigurált virtuális hálózati. Lásd: [virtuális hálózat áttekintése](virtual-networks-overview.md) Vnetek további információt.
* hello Azure PowerShell legújabb verziójának letöltése, és telepítve. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

virtuális gép több hálózati adapter a következő lépéseket egy PowerShell-munkameneten belül minden parancs beírásával teljes hello és toocreate:

1. Válassza ki a Virtuálisgép-lemezkép Azure VM-lemezkép gyűjteményből. Vegye figyelembe, hogy a képek módosulnak, és régiónként elérhetők. hello hello az alábbi példában megadott lemezkép módosítása vagy előfordulhat, hogy nem a régióban kell, ezért mindenképpen toospecify hello lemezkép van szüksége.

    ```powershell
    $image = Get-AzureVMImage `
    -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"
    ```

2. Hozzon létre egy Virtuálisgép-konfiguráció.

    ```powershell
    $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
    -Image $image.ImageName –AvailabilitySetName "MyAVSet"
    ```

3. Hello alapértelmezett rendszergazdai bejelentkezés létrehozása.

    ```powershell
    Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
    -Password "<YourAdminPassword>"
    ```

4. Adja hozzá a további hálózati adapterek toohello Virtuálisgép-konfiguráció.

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
    -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
    Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
    -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm
    ```

5. Hello alhálózatot és IP-cím megadása hello alapértelmezett hálózati adaptert.

    ```powershell
    Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
    Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm
    ```

6. Hozzon létre hello VM a virtuális hálózat.

    ```powershell
    New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm
    ```

    > [!NOTE]
    > hello itt megadott virtuális hálózat már léteznie kell (ahogy azt hello Előfeltételek). az alábbi példa hello megadja nevű virtuális hálózat **MultiNIC-VNet**.
    >

## <a name="limitations"></a>Korlátozások
hello a következő korlátozások vonatkoznak, több hálózati adapter használata esetén:

* Több hálózati adapterrel rendelkező virtuális gépeket az Azure virtuális hálózatokról (Vnetekről) kell létrehozni. Nem-virtuális hálózat virtuális gépek több hálózati adapter nem állítható be.
* Rendelkezésre állási virtuális gépeinek kell toouse állítsa be, vagy több hálózati adapterrel, vagy egy egyetlen hálózati adaptert. Nem lehet több hálózati adapter virtuális gépek és egyetlen, a hálózati adapter virtuális gépek rendelkezésre állási csoportok belül. Ugyanazok a szabályok vonatkoznak a virtuális gépek felhőszolgáltatásban. Több hálózati adapter virtuális gépek esetén nem szükséges toohave hello azonos számú hálózati adaptert, mindaddig, amíg minden rendelkeznek legalább két.
* Virtuális gép egyetlen hálózati adapter és több hálózati adapter (és fordítva) nem állítható be, ha telepítve van, törlésével és ismételt létrehozása nélkül.

## <a name="secondary-nics-access-tooother-subnets"></a>Másodlagos hálózati adapterek hozzáférés tooother alhálózatok
Másodlagos hálózati adapter nem konfigurálható egy alapértelmezett átjáró alapértelmezés szerint miatt hello toowhich hello adatforgalmat másodlagos hálózati adapter lesz belül hello korlátozott toobe ugyanazon az alhálózaton. Hello tooenable másodlagos hálózati adapterek tootalk kívül a saját alhálózati szeretnék, azok bejegyzés hello útválasztási táblázat tooconfigure hello átjáró, az alább ismertetett tooadd kell.

> [!NOTE]
> A virtuális gépeket létrehozni, mielőtt a 2015. július lehet a hálózati adapterek összes beállított alapértelmezett átjárót. hello alapértelmezett átjáró másodlagos hálózati adaptert a virtuális gépeken újraindítása nem lesznek eltávolítva. Hello gyenge állomás útválasztási modellt használó, például a Linux operációs rendszerben internetkapcsolat érvénytelenné Ha hello bemenő és kimenő forgalom használjon másik hálózati adaptert.
> 

### <a name="configure-windows-vms"></a>Windows virtuális gépek
Tegyük fel, hogy két hálózati adapterrel rendelkező virtuális gép Windows az alábbiak szerint:

* Elsődleges hálózati adapter IP-cím: 192.168.1.4
* Másodlagos hálózati adapter IP-cím: 192.168.2.5

Ez a virtuális gép hello IPv4 útvonaltábla néz ki:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Láthatja, hogy hello alapértelmezett útvonalat (0.0.0.0) csak elérhető toohello elsődleges hálózati adaptert. Csak akkor tudja tooaccess erőforrások kívül másodlagos hello hello IP-alhálózatot a hálózati adapter, az alább látható módon:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

az alapértelmezett útvonal a tooadd hello másodlagos hálózati adapter, hajtsa végre hello alábbi lépéseket:

1. Egy parancssorból futtassa a tooidentify hello indexszámát hello hello parancsot másodlagos hálózati adapter:
   
        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================
2. Figyelje meg a második bejegyzés hello hello a táblában (a példában) 27 az indexet.
3. Hello parancssorból futtassa a hello **útvonal hozzáadása** parancsot a lent látható módon. Ebben a példában meg 192.168.2.1 hello alapértelmezett átjáróként hello a másodlagos hálózati adapter:
   
        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27
4. tootest kapcsolatot, lépjen vissza toohello parancssort, és próbálkozzon egy másik alhálózatból származó hello másodlagos hálózati adapter látható egész elemeként eh az alábbi példában tooping:
   
        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5
   
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
5. Ellenőrizheti az útvonal tábla toocheck hello újonnan hozzáadott útvonal, alább látható módon:
   
        C:\Users\Administrator>route print
   
        ...
   
        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Linux virtuális gépek
Linux virtuális gépekhez, mivel a hello alapértelmezett konfigurációját használja gyenge állomás útválasztási, azt javasoljuk, hogy hello másodlagos hálózati adapterek korlátozott tootraffic folyamatot csak belül hello ugyanazon az alhálózaton. Azonban bizonyos forgatókönyvek hello alhálózati kívüli kapcsolatot igényelnek, ha felhasználók lehetővé kell tennie a csoportházirend-alapú útválasztási tooensure érkező hello és kimenő forgalom által használt hello azonos hálózati adaptert.

## <a name="next-steps"></a>Következő lépések
* Telepítése [MultiNIC virtuális gépeket a 2-rétegbeli alkalmazás esetén a Resource Manager-telepítés a](virtual-network-deploy-multinic-arm-template.md).
* Telepítése [MultiNIC virtuális gépeket a 2-rétegbeli alkalmazás esetén a klasszikus üzembe helyezési a](virtual-network-deploy-multinic-classic-ps.md).

