---
title: "aaaSQL Server Azure virtuális gépek – gyakori kérdések |} Microsoft Docs"
description: "Ez a cikk ismerteti a válaszok toofrequently Azure virtuális gépeken futó SQL Server kérdések."
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
ms.openlocfilehash: 70a8777bf765dcc69f433aa1fb59eb94929caab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-sql-server-on-azure-virtual-machines"></a>SQL Server használata az Azure Virtual Machines szolgáltatásban – gyakori kérdések
Ez a témakör válaszokat toosome a hello fut kapcsolatos általános kérdésekre [SQL Server Azure virtuális gépeken](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Gyakori kérdések

1. **Hogyan hozható létre egy Azure virtuális gépen futó SQL Server?**

    hello legegyszerűbb megoldás az toocreate egy virtuális gépet, amely tartalmazza az SQL Server. Egy regisztrál az Azure és hello portálról SQL virtuális gép létrehozása az oktatóanyaghoz, lásd: [egy SQL Server rendszerű virtuális gép az Azure portál hello](virtual-machines-windows-portal-sql-server-provision.md). Kiválaszthatja, hogy a virtuálisgép-lemezkép perc fizetési SQL Server licencelési használó, vagy egy olyanra, amely lehetővé teszi a toobring a saját SQL Server-licencét is használhat. Akkor is hello lehetőség manuálisan az SQL Server telepítése a virtuális gép, és újból felhasználja a helyszíni-licencre. Ha később saját licenc, rendelkeznie kell [Azure frissítési garancián keresztüli Licenchordozhatósági](https://azure.microsoft.com/pricing/license-mobility/). További információkért tekintse meg [az SQL Server Azure virtuális gépek díjszabási útmutatóját](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Mi az az SQL virtuális gépek és az SQL Database szolgáltatás hello hello különbségének?**

    Fogalmilag, SQL Server rendszert futtató Azure virtuális géphez eltér, hogy nem fut az SQL Server egy távoli adatközpontban. Ezzel szemben [SQL-adatbázis](../../../sql-database/sql-database-technical-overview.md) kínál az adatbázis-a-szolgáltatás. Az SQL Database Önnek nincs hozzáférési toohello az adatbázisokat üzemeltető gépek. A teljes kapcsolatban lásd: [felhőalapú SQL Server-verzió: Azure SQL (PaaS) Database vagy az SQL Server Azure virtuális gépeken (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Hogyan telepíthet át a helyszíni SQL Server adatbázis toohello felhő?**

    Először létre kell hoznia egy Azure virtuális gépet egy SQL Server-példányhoz. Telepítse át a helyszíni adatbázisok toothat példányát. Az adatok áttelepítési stratégiák, lásd: [egy SQL Server adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése](virtual-machines-windows-migrate-sql.md).

1. **Telepíthető az SQL Server másik példányának a hello azonos virtuális gép? Módosíthatja a telepített szolgáltatások hello alapértelmezett példány?**

    Igen. SQL Server telepítési adathordozóról hello hello mappájában található **C** meghajtó. Futtatás **Setup.exe** adott hely tooadd új SQL Server-példányok vagy toochange más telepített szolgáltatásokat az SQL Server hello gépen. Vegye figyelembe, hogy bizonyos funkciók, például az automatikus biztonsági mentés, automatikus javítás és az Azure Key Vault-integráció csak működése az hello alapértelmezett példány.

1. **Az SQL Server alapértelmezett példányát hello eltávolítása**

    Igen. Azonban nincs szempontokat. Hello előző választ, a hello használó szolgáltatások leírtaknak [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md) hello alapértelmezett példány csak működik. Ha eltávolítja a hello alapértelmezett példány, hello bővítmény toolook folytatja az és Eseménynapló hibák léphetnek fel. Ezek a hibák tartoznak, amely a következő két források hello: **Microsoft SQL Server hitelesítőadat-kezelés** és **Microsoft SQL Server IaaS-ügynök**. Hello hibák valamelyike hasonló toohello következő lehet:
    
        A network-related or instance-specific error occurred while establishing a connection tooSQL Server. hello server was not found or was not accessible. 
        
    Ha úgy dönt, toouninstall hello alapértelmezett példány, is eltávolítja a hello [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md) is.

1. **Hogyan lehet frissíteni a tooa új verziójához/kiadásához hello SQL Server egy Azure virtuális gép?**

    Jelenleg nem egy Azure virtuális Gépen futó SQL Server helyszíni frissítését. Hozzon létre egy új Azure virtuális gép hello szükséges SQL Server verziójához/kiadásához, majd utána áttelepíteni az adatbázisok toohello új kiszolgálóját a szabványos [adatok áttelepítési technikák](virtual-machines-windows-migrate-sql.md).

1. **Hogyan telepíthetők a licencelt másolatával, az SQL Server egy Azure virtuális gépen?**

    Nincsenek két módon toodo ez. Megadhat egyet hello [licencek támogató virtuálisgép-rendszerképek](virtual-machines-windows-sql-server-iaas-overview.md#BYOL). Egy másik lehetőség toocopy hello SQL Server telepítési adathordozóról tooa Windows Server virtuális gép, és telepítse az SQL Server a virtuális gép hello. Licencelés okok, rendelkeznie kell [Azure frissítési garancián keresztüli Licenchordozhatósági](https://azure.microsoft.com/pricing/license-mobility/). További információkért tekintse meg [az SQL Server Azure virtuális gépek díjszabási útmutatóját](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Módosítható a virtuális gép toouse saját SQL Server licence Ha valamelyik hello használatalapú gyűjtemény lemezképei hozták létre?**

    Nem. Nem válthat a fizetési-percalapú licencelési toousing saját licenc. Hozzon létre egy új Azure virtuális gép hello egyikével [BYOL képek](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), majd telepítse át az adatbázisok toohello új kiszolgálót a szabványos [adatok áttelepítési technikák](virtual-machines-windows-migrate-sql.md).

1. **Azure virtuális gépeken támogatott SQL Server feladatátvevő fürt példány (FCI)?**

   Igen. Is [Windows feladatátvevő fürt létrehozása a Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) és hello fürttároló közvetlen tárolóhelyek (S2D) használja. Másik megoldásként használhatja külső fürtszolgáltatás vagy a tárolási megoldások leírtak [az SQL Server Azure virtuális gépek magas rendelkezésre állási és vészhelyreállítási helyreállítási](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

1. **SQL Server egy Azure virtuális gépen toopay toolicense Ha csak használatos készenléti/feladatátvétel van?**

    Nem rendelkezik toopay toolicense egy SQL Server részt vesz egy magas rendelkezésre ÁLLÁSÚ telepítés passzív másodlagos másodpéldányként Ha frissítési garancia és License Mobility használja a [virtuális gép Licensing FAQ](http://azure.microsoft.com/pricing/licensing-faq/).

1. **Hogyan frissítést és szervizcsomagot alkalmazza a rendszer egy SQL Server virtuális gépen?**

    Virtuális gépek segítségével meghatározhatja hello gazdaszámítógépet, beleértve az mikor és hogyan frissítések telepítését. Hello operációs rendszer esetén manuálisan alkalmazhatja a windows frissítéseket, vagy engedélyezheti a nevű szolgáltatás [automatikus javítás](virtual-machines-windows-sql-automated-patching.md). Az Automatikus javítás minden fontosként megjelölt frissítést telepít, így az e kategóriába eső SQL Server-frissítéseket is. Egyéb opcionális frissítést tooSQL kiszolgálót manuálisan kell telepíteni.

1. **Ennyi az egész konfigurációk nem jelenik meg a virtuálisgép-katalógus hello (a + SQL Server 2012-ben például a Windows 2008 R2) mentése lehetséges tooset?**

    Nem. A virtuális gép gyűjtemény lemezképek, amely tartalmazza az SQL Server ki kell választania, hello által biztosított rendszerképek egyikét.

1. **Hogyan kell telepíteni a virtuális hálózat Azure SQL-adatfájl eszközök?**

     Töltse le és telepítse az SQL adatok eszközeit hello [Microsoft SQL Server Data Tools - Visual Studio 2013 Business Intelligence](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Erőforrások

Az SQL Server Azure virtuális gépeken áttekintéséhez tekintse meg a hello videót [Azure virtuális gép hello legjobb platform az SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016). Egy jó bemutatása hello témakörben is megkapható [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).

Egyéb erőforrások többek között:

* [Egy SQL Server rendszerű virtuális gép a hello Azure portál](virtual-machines-windows-portal-sql-server-provision.md)
* [Egy adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése](virtual-machines-windows-migrate-sql.md)
* [Magas rendelkezésre álláshoz és Vészhelyreállításhoz az SQL Server Azure virtuális gépeken](virtual-machines-windows-sql-high-availability-dr.md)
* [Ajánlott eljárások az SQL Server teljesítményének Azure Virtual Machines szolgáltatásbeli növeléséhez](virtual-machines-windows-sql-performance.md)
* [Azure-beli virtuális gépeken futó SQL Server – alkalmazásminták és fejlesztési stratégiák](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md) 