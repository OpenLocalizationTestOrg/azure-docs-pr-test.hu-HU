---
title: "SQL Server Azure virtuális gépeken – gyakori kérdések |} Microsoft Docs"
description: "Ez a cikk ismerteti az Azure virtuális gépeken futó SQL Server gyakran feltett kérdésekre adott válaszok."
services: virtual-machines-windows
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/23/2017
ms.author: v-shysun
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 447ece9653a4cf69153f9c85e222e3ea4e8bae16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="frequently-asked-questions-for-sql-server-on-azure-virtual-machines"></a>SQL Server használata az Azure Virtual Machines szolgáltatásban – gyakori kérdések
Ez a témakör ismerteti a leggyakoribb kérdésekre vonatkozó válaszokat [SQL Server Azure virtuális gépeken](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Gyakori kérdések

1. **Hogyan hozható létre egy Azure virtuális gépen futó SQL Server?**

    A legegyszerűbb megoldás az, hogy hozzon létre egy virtuális gépet, amely tartalmazza az SQL Server. Oktatóanyag regisztrál az Azure és az SQL virtuális gép létrehozása a portálon, lásd: [egy SQL Server rendszerű virtuális gép az Azure portálon](virtual-machines-windows-portal-sql-server-provision.md). Kiválaszthatja, hogy a virtuálisgép-lemezkép perc fizetési SQL Server licencelési használó, vagy egy olyanra, amely lehetővé teszi a saját SQL Server-licenc is használhat. Akkor is kézzel az SQL Server telepítése a virtuális gép, és újból felhasználja a helyszíni-licencre. Ha később saját licenc, rendelkeznie kell [Azure frissítési garancián keresztüli Licenchordozhatósági](https://azure.microsoft.com/pricing/license-mobility/). További információkért tekintse meg [az SQL Server Azure virtuális gépek díjszabási útmutatóját](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Mi az a különbség SQL virtuális gépek és az SQL Database szolgáltatásban?**

    Fogalmilag, SQL Server rendszert futtató Azure virtuális géphez eltér, hogy nem fut az SQL Server egy távoli adatközpontban. Ezzel szemben [SQL-adatbázis](../../../sql-database/sql-database-technical-overview.md) kínál az adatbázis-a-szolgáltatás. Az SQL Database nem rendelkezik hozzáféréssel az adatbázisok üzemeltetési gépekre. A teljes kapcsolatban lásd: [felhőalapú SQL Server-verzió: Azure SQL (PaaS) Database vagy az SQL Server Azure virtuális gépeken (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Hogyan telepíthet át a felhőbe a helyszíni SQL Server-adatbázist?**

    Először létre kell hoznia egy Azure virtuális gépet egy SQL Server-példányhoz. Telepítse át a helyszíni adatbázisokat példányhoz. Az adatok áttelepítési stratégiák, lásd: [egy SQL Server-adatbázis áttelepítése SQL Server egy Azure virtuális gép](virtual-machines-windows-migrate-sql.md).

1. **Telepíthető az SQL Server egy második példányát az azonos virtuális gépen? Módosíthatja az alapértelmezett példány telepített szolgáltatásokat?**

    Igen. Az SQL Server telepítési adathordozóról az egy mappában található a **C** meghajtó. Futtatás **Setup.exe** arról a helyről új SQL Server-példányok felvételéhez vagy módosíthatja a más telepítendő szolgáltatásokat az SQL Server a számítógépen. Vegye figyelembe, hogy bizonyos funkciók, például az automatikus biztonsági mentés, automatikus javítás és az Azure Key Vault-integráció csak az alapértelmezett példány működik.

1. **SQL Server alapértelmezett példányának eltávolítása**

    Igen. Azonban nincs szempontokat. Amint azt a korábbi választ, szolgáltatások, amelyek támaszkodjon a [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md) csak az alapértelmezett példányon működik. Ha az alapértelmezett példányt eltávolítja, a bővítmény továbbra is fennáll, amelyet meg kíván keresni az és Eseménynapló hibák léphetnek fel. Ezeket a hibákat a következő két forrásból származnak: **Microsoft SQL Server hitelesítőadat-kezelés** és **Microsoft SQL Server IaaS-ügynök**. A hibák lehetnek a következőhöz hasonló:
    
        A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. 
        
    Ha úgy dönt, az alapértelmezett példány eltávolítása, is eltávolítja a [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md) is.

1. **Hogyan lehet frissíteni az SQL Server egy Azure virtuális gép egy új verziójához/kiadásához számára?**

    Jelenleg nem egy Azure virtuális Gépen futó SQL Server helyszíni frissítését. Hozzon létre egy új Azure virtuális gép a kívánt SQL Server verziójához/kiadásához, majd utána áttelepíteni az adatbázisokat az új kiszolgálóra a szabványos [adatok áttelepítési technikák](virtual-machines-windows-migrate-sql.md).

1. **Hogyan telepíthetők a licencelt másolatával, az SQL Server egy Azure virtuális gépen?**

    Ehhez két módja van. Egyik létesíthet a [licencek támogató virtuálisgép-rendszerképek](virtual-machines-windows-sql-server-iaas-overview.md#BYOL). Lehetősége az SQL Server telepítési adathordozóról másolja a Windows Server virtuális géphez, és SQL Server telepítése a virtuális Gépen. Licencelés okok, rendelkeznie kell [Azure frissítési garancián keresztüli Licenchordozhatósági](https://azure.microsoft.com/pricing/license-mobility/). További információkért tekintse meg [az SQL Server Azure virtuális gépek díjszabási útmutatóját](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Módosíthatja a saját SQL Server licence használja, ha az egyik a használatalapú lévő képek hozták létre a virtuális gépek?**

    Nem. A fizetési-percalapú licencelés saját licenc használata nem válthat. Hozzon létre egy új Azure virtuális gép egyikével a [BYOL képek](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), majd telepítse át az adatbázisokat az új kiszolgálóra a szabványos [adatok áttelepítési technikák](virtual-machines-windows-migrate-sql.md).

1. **Azure virtuális gépeken támogatott SQL Server feladatátvevő fürt példány (FCI)?**

   Igen. Is [Windows feladatátvevő fürt létrehozása a Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) és a fürttároló közvetlen tárolóhelyek (S2D) használja. Másik megoldásként használhatja külső fürtszolgáltatás vagy a tárolási megoldások leírtak [az SQL Server Azure virtuális gépek magas rendelkezésre állási és vészhelyreállítási helyreállítási](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

1. **Rendelkeznie kell fizetnie licencre SQL Server egy Azure virtuális gépen, ha csak használatos készenléti vagy feladatátvételi?**

    Nincs egy SQL Server magas rendelkezésre ÁLLÁSÚ telepítés, passzív másodlagos másodpéldányként részt vevő, ha frissítési garancia és License Mobility használja, a licenc fizetnie [virtuális gép Licensing FAQ](http://azure.microsoft.com/pricing/licensing-faq/).

1. **Hogyan frissítést és szervizcsomagot alkalmazza a rendszer egy SQL Server virtuális gépen?**

    Virtuális gépek segítségével meghatározhatja a gazdaszámítógépet, beleértve az mikor és hogyan frissítések telepítését. Az operációs rendszer, hogy kézzel alkalmazhatja a windows-frissítések, vagy engedélyezheti a nevű szolgáltatás [automatikus javítás](virtual-machines-windows-sql-automated-patching.md). Az Automatikus javítás minden fontosként megjelölt frissítést telepít, így az e kategóriába eső SQL Server-frissítéseket is. Az SQL Server egyéb nem kötelező frissítéseit manuálisan kell telepíteni.

1. **Az nem látható a virtuális gép gyűjtemény (a + SQL Server 2012-ben például a Windows 2008 R2) konfigurációk beállítható?**

    Nem. A virtuális gép gyűjtemény lemezképek, amely tartalmazza az SQL Server ki kell választania, egy megadott lemezképet.

1. **Hogyan kell telepíteni a virtuális hálózat Azure SQL-adatfájl eszközök?**

     Töltse le és telepítse az SQL-adatfájl eszközeit [Microsoft SQL Server Data Tools - Visual Studio 2013 Business Intelligence](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Erőforrások

Az SQL Server Azure virtuális gépeken áttekintéséhez tekintse meg a videót [Azure virtuális gép a legjobb platform az SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016). A következő témakör is megkapható egy helyes bemutatása [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).

Egyéb erőforrások többek között:

* [SQL Server rendszerű virtuális gép létrehozása az Azure Portalon](virtual-machines-windows-portal-sql-server-provision.md)
* [Az Azure virtuális gép az SQL Server-adatbázis migrálása](virtual-machines-windows-migrate-sql.md)
* [Magas rendelkezésre álláshoz és Vészhelyreállításhoz az SQL Server Azure virtuális gépeken](virtual-machines-windows-sql-high-availability-dr.md)
* [Ajánlott eljárások az SQL Server teljesítményének Azure Virtual Machines szolgáltatásbeli növeléséhez](virtual-machines-windows-sql-performance.md)
* [Azure-beli virtuális gépeken futó SQL Server – alkalmazásminták és fejlesztési stratégiák](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md) 