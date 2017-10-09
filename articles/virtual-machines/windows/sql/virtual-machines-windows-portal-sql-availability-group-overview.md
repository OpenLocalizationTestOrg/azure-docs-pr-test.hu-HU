---
title: "Kiszolgáló rendelkezésre állási csoportok – Azure virtuális gépek – áttekintés aaaSQL |} Microsoft Docs"
description: "Ez a cikk Azure virtuális gépeken futó SQL Server rendelkezésre állási csoportok be."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/13/2017
ms.author: mikeray
ms.openlocfilehash: ecac8b8c5073021af2aa22a05490bb8c4c20ed17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>SQL Server Always On rendelkezésre állási csoportok az Azure virtuális gépeken bemutatása #

Ez a cikk be az SQL Server rendelkezésre állási csoportok Azure virtuális gépeken. 

Always On rendelkezésre állási csoportok Azure virtuális gépeken a rendelkezésre állási csoportok a helyszíni hasonló tooAlways. További információkért lásd: [Always On rendelkezésre állási csoportok (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx). 

hello ábra szemlélteti a teljes SQL Server rendelkezésre állási csoport az Azure virtuális gépek hello részeit.

![Rendelkezésre állási csoport](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

hello legfontosabb különbség az Azure virtuális gépek rendelkezésre állási csoport, amely az Azure virtuális gépek hello szükséges egy [terheléselosztó](../../../load-balancer/load-balancer-overview.md). hello terheléselosztó hello rendelkezésre állási csoport figyelőjének hello IP-címet tartalmazza. Ha egynél több rendelkezésre állási csoport minden egyes csoport szükséges egy figyelő. Egy terheléselosztó több figyelői képes támogatni.

Amikor készen áll a toobuild egy SQL Server rendelkezésre állási aroup Azure virtuális gépeken, tekintse meg a toothese oktatóanyagok.

## <a name="automatically-create-an-availability-group-from-a-template"></a>Rendelkezésre állási csoport automatikusan létrehozása sablonból

[Always On rendelkezésre állási csoport konfigurálásához Azure virtuális gép automatikusan - erőforrás-kezelő](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a>Hozzon létre manuálisan egy rendelkezésre állási csoport Azure-portálon

Is létrehozhat hello virtuális gépek saját kezűleg hello sablon nélkül. Első lépésként hello Előfeltételek befejeződését, majd hello rendelkezésre állási csoport létrehozására. Tekintse meg a következő témakörök hello: 

- [Előfeltételek az SQL Server Always On rendelkezésre állási csoportok konfigurálása Azure virtuális gépeken](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [Mindig a rendelkezésre állási csoport létrehozása tooimprove rendelkezésre állási és katasztrófa-helyreállítás](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Következő lépések

[SQL-kiszolgáló mindig konfigurálása az Azure virtuális gépeken különböző régiókban rendelkezésre állási csoporton](virtual-machines-windows-portal-sql-availability-group-dr.md).
