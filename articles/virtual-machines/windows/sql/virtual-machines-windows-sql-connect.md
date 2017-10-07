---
title: "SQL Server virtuális géphez (Resource Manager) aaaConnect tooa |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconnect tooSQL Server rendszert futtató Azure virtuális gépen. Ez a témakör hello klasszikus üzembe helyezési modellt használ. hello forgatókönyvek különböznek attól függően, hello hálózati konfigurációs és hello ügyfél hello helyét."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/14/2017
ms.author: jroth
ms.openlocfilehash: 7b127c14c37b9a72c19ed17f8b1dad61c7bc2d38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-sql-server-virtual-machine-on-azure-resource-manager"></a>Csatlakozás tooa SQL Server virtuális gépet az Azure (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-connect.md)
> * [Klasszikus](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Áttekintés

Ez a témakör ismerteti, hogyan tooconnect tooyour SQL Server példányt, egy Azure virtuális gépen futó. Bemutatja, néhány [általános kapcsolati forgatókönyvek](#connection-scenarios) , majd [egy Azure virtuális gép az SQL Server-kapcsolat beállításának lépései részletes](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Ha inkább a teljes útmutató a kiépítést és a kapcsolat kellene lennie, lásd: [Azure SQL Server virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Kapcsolat-forgatókönyvek

hello módon ügyfél csatlakozik egy virtuális gépen futó kiszolgáló eltér attól függően, hogy hello hely hello ügyfél és a hálózati konfiguráció hello tooSQL.

Ha egy SQL Server virtuális gép hello Azure-portálon, lehetősége van hello hello típusú megadásával **SQL-kapcsolat**.

![Nyilvános SQL kapcsolati lehetőséget kiépítése során](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

A hálózati kapcsolatot a lehetőségek a következők:

| Beállítás | Leírás |
|---|---|
| **Nyilvános** | Csatlakozás tooSQL kiszolgálón keresztül hello internet |
| **Magánfelhő** | Csatlakozzon a hello Server tooSQL azonos virtuális hálózatban |
| **Helyi** | Hello a helyi kiszolgáló tooSQL csatlakoztassa ugyanahhoz a virtuális géphez | 

hello alábbi szakaszok ismertetik a hello **nyilvános** és **titkos** beállítások részletesebben.

## <a name="connect-toosql-server-over-hello-internet"></a>Kiszolgáló tooSQL hello interneten keresztüli kapcsolódás

Ha tooconnect tooyour SQL Server adatbázismotor a hello Internet, jelölje be **nyilvános** a hello **SQL-kapcsolat** típus hello portálon kiépítése során. hello portál automatikusan hello lépéseket követve:

* Lehetővé teszi, hogy a TCP/IP-protokoll hello az SQL Server.
* Konfigurálja a tűzfal szabály tooopen hello SQL Server TCP port (alapértelmezés szerint 1433).
* Lehetővé teszi az SQL Server-hitelesítést, a nyilvános hozzáférés szükséges.
* VM tooall TCP-forgalmat az SQL Server port hello hello hello hálózati biztonsági csoport konfigurálása.

> [!IMPORTANT]
> az SQL Server Developer hello hello virtuálisgép-lemezképeket és Express kiadásait nem automatikusan hello TCP/IP protokoll engedélyezéséhez. A fejlesztői és Express kiadásait kell használnia az SQL Server Configuration Manager túl[manuálisan engedélyezi a hello TCP/IP-protokoll](#manualtcp) hello létrehozása után a virtuális gép.

Bármely, internet-hozzáféréssel rendelkező ügyfél kapcsolódhatnak toohello SQL Server-példány nyilvános IP-cím hello hello virtuális gép vagy a bármely DNS-címke hozzárendelt toothat IP-cím megadásával. Ha SQL Server port hello az 1433-as, nem kell toospecify azt hello kapcsolati karakterláncban. a következő kapcsolati karakterlánc hello tooa SQL virtuális gép csatlakozik egy DNS-címke a `sqlvmlabel.eastus.cloudapp.azure.com` SQL-hitelesítéssel (is használhatja hello nyilvános IP-cím).

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

Bár ez lehetővé teszi a kapcsolatot az ügyfelek keresztül hello internet, ez nem feltétlenül jelenti azt, hogy bárki kapcsolódhatnak-e az SQL Server tooyour. Külső ügyfelek toohello helyes felhasználónévvel és jelszóval rendelkezik. A fokozott biztonság érdekében elkerülheti hello jól ismert 1433-as port. Például ha konfigurálta az SQL Server toolisten 1500 porton és a meghatározott megfelelő tűzfal- és hálózati biztonsági csoportszabályok, kapcsolódhat hozzáfűzésével hello port száma toohello kiszolgáló neve. hello alábbi példa megváltoztatja hello előzőre egy egyéni portszám hozzáadásával **1500**, toohello kiszolgáló nevét:

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> Ha SQL Server virtuális gépen keresztül hello internet, minden kimenő adatok hello Azure datacenter nem tulajdonos toonormal [a kimenő adatátviteli díjszabás](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="connect-toosql-server-within-a-virtual-network"></a>Csatlakozás a virtuális hálózaton belül Server tooSQL

Ha úgy dönt, **titkos** a hello **SQL-kapcsolat** típus az Azure-portálon hello konfigurálása hello beállításai megegyeznek a legtöbb túl**nyilvános**. hello egy különbség, hogy nincs hálózati biztonsági csoport szabály tooallow forgalom hello SQL-kiszolgáló portja (alapértelmezés szerint 1433) kívül van.

> [!IMPORTANT]
> az SQL Server Developer hello hello virtuálisgép-lemezképeket és Express kiadásait nem automatikusan hello TCP/IP protokoll engedélyezéséhez. A fejlesztői és Express kiadásait kell használnia az SQL Server Configuration Manager túl[manuálisan engedélyezi a hello TCP/IP-protokoll](#manualtcp) hello létrehozása után a virtuális gép.

Magánhálózati kapcsolat gyakran használják a együtt [virtuális hálózati](../../../virtual-network/virtual-networks-overview.md), több olyan forgatókönyveket tesz lehetővé, amelyek. Az azonos virtuális hálózatban, még akkor is, ha ezen virtuális gépek szerepelnek a különböző erőforráscsoportokra hello virtuális gépek is elérheti. És egy [telephelyek közötti VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), létrehozhat egy hibrid architektúra, amely a virtuális gépek a helyszíni hálózatokkal és gépek.

Virtuális hálózatok az Azure virtuális gépek tooa tartomány is engedélyezheti, toojoin. Ez a hello csak úgy toouse Windows-hitelesítés tooSQL kiszolgáló. hello más kapcsolat esetekben szükséges SQL-hitelesítés a felhasználónevek és jelszavak.

Feltételezve, hogy a virtuális hálózaton konfigurálta a DNS, hello SQL Server rendszerű virtuális számítógép neve hello kapcsolati karakterlánc megadásával kapcsolható tooyour SQL Server-példány. a következő példa is hello feltételezi, hogy a Windows-hitelesítést is konfigurálva van, és hello felhasználó rendelkezik hozzáférési toohello SQL Server-példányt.

```
Server=mysqlvm;Integrated Security=true
```

## <a id="change"></a>SQL-kapcsolat beállításainak módosítása

Hello kapcsolat beállításokat az SQL Server virtuális gépen a hello Azure-portálon módosíthatja.

1. Hello Azure-portálon, válassza ki **virtuális gépek**.

2. Válassza ki az SQL Server rendszerű virtuális Géphez.

3. A **beállítások**, kattintson a **SQL Server-konfigurációs**.

4. Változás hello **SQL kapcsolati szint** tooyour szükséges beállítást. Is használhat a terület toochange hello SQL Server portja vagy hello SQL-hitelesítés beállításai.

   ![SQL-kapcsolat módosítása](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. Várjon néhány percet a frissítés toocomplete hello.

   ![SQL virtuális gép frissítési értesítés](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <a id="manualtcp"></a>Engedélyezze a TCP/IP protokollt a fejlesztői és az Express verziója

SQL Server-kapcsolat beállításainak módosításakor Azure nem automatikusan hello TCP/IP protokoll engedélyezéséhez az SQL Server Developer és Express kiadásait. az alábbi hello részben megtudhatja, hogyan toomanually engedélyezze a TCP/IP protokollt, hogy az IP-cím szerint távolról kapcsolódhatnak.

Csatlakoztasson toohello SQL Server-számítógépen a távoli asztalról.

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Következő lépésként engedélyezze a hello TCP/IP-protokoll **SQL Server Configuration Manager**.

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a>Csatlakozás SSMS segítségével

hello lépések megjelenítése toocreate egy nem kötelező DNS címke az Azure virtuális gép számára, és csatlakozzon az SQL Server Management Studio (SSMS).

[!INCLUDE [Connect tooSQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Következő lépések

üzembe helyezési útmutató toosee kapcsolat lépések együtt: [Azure SQL Server virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).

Egyéb témakörök kapcsolódó toorunning SQL Server Azure virtuális gépeken, a következő témakörben: [SQL Server Azure virtuális gépeken](virtual-machines-windows-sql-server-iaas-overview.md).
