---
title: "aaaPlans és az Azure Scheduler számlázási"
description: "Csomagok és a számlázás az Azure Schedulerrel"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 13a2be8c-dc14-46cc-ab7d-5075bfd4d724
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 051b8eeb1ea19678b3cef4db3237ebf04c8b0e13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="plans-and-billing-in-azure-scheduler"></a>Csomagok és a számlázás az Azure Schedulerrel
## <a name="job-collection-plans"></a>Feladat gyűjtési terveket
Feladatgyűjtemények számlázható entitás hello Azure Scheduler. Feladatgyűjtemények feladatok száma tartalmazhat, és a három csomagokban – ingyenes, Standard és Premium –, az alábbiakban található származnak.

| **Feladat gyűjtési terv** | **Feladat gyűjteményenként feladatok maximális száma** | **Maximális ismétlődésére** | **Maximális Feladatgyűjteményei előfizetésenként** | **Korlátok** |
|:--- |:--- |:--- |:--- |:--- |
| **Ingyenes** |5 feladatok feladat gyűjteményenként |Óránként egyszer. Óránként többször nem hajtható végre feladatok |Egy előfizetés mentése too1 szabad feladatgyűjteményt engedélyezett |Nem használható [HTTP kimenő engedélyezési objektum](scheduler-outbound-authentication.md) |
| **Standard** |50 feladatok feladat gyűjteményenként |Percenkénti. Percenként egyszer többször nem hajtható végre feladatok |Egy előfizetés mentése too100 szabványos feladatgyűjteményei engedélyezett |Hozzáférés toofull a ütemező szolgáltatáskészlete |
| **P10 Premium** |50 feladatok feladat gyűjteményenként |Percenkénti. Percenként egyszer többször nem hajtható végre feladatok |Egy előfizetés mentése too10, 000 P10 prémium feladatgyűjteményei engedélyezett. <a href="mailto:wapteams@microsoft.com">Kapcsolatfelvétel</a> több. |Hozzáférés toofull a ütemező szolgáltatáskészlete |
| **P20 prémium** |1000 feladatok feladat gyűjteményenként |Percenkénti. Percenként egyszer többször nem hajtható végre feladatok |Egy előfizetés mentése too10, 000 P20 prémium feladatgyűjteményei engedélyezett. <a href="mailto:wapteams@microsoft.com">Kapcsolatfelvétel</a> több. |Hozzáférés toofull a ütemező szolgáltatáskészlete |

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Frissítések és a feladat gyűjtési terveket Downgrades
Előfordulhat, hogy frissítse vagy visszaminősítését egy feladat gyűjtési terv bármikor hello ingyenes, a Standard és Premium tervek között. Azonban ha alacsonyabb verziójúra változtatása tooa szabad feladatgyűjteményt, hello alacsonyabb szintre való visszalépést sikertelenek lehetnek hello a következő okok valamelyike miatt:

* Hello előfizetésben már tartalmaz szabad feladatgyűjteményt van
* A feladat, hello feladatgyűjteményben rendelkezik egy magasabb ismétlődési, mint a feladatok az ingyenes feladatgyűjtemények megengedett. hello tartalmaz szabad feladatgyűjteményt engedélyezett maximális ismétlődésére van óránként egyszer
* 5-nél több feladatok hello feladatgyűjteményben nincsenek
* A feladat hello feladatgyűjteményben rendelkezik HTTP vagy HTTPS PROTOKOLLT használó művelet egy [HTTP kimenő engedélyezési objektum](scheduler-outbound-authentication.md)

## <a name="billing-and-azure-plans"></a>Számlázási és az Azure-csomagok
Előfizetések nem szabad felszámított feladatgyűjteményei. Ha több mint 100 szabványos feladatgyűjteményei (10 szabványos, számlázási egységek), akkor célszerű egy jobb üzlet toohave összes sikertelen feladat-gyűjtemények hello prémium csomagban.

Ha egy szabványos feladatgyűjtemény és egy prémium szintű feladatgyűjtemény,-e számlázott egy standard számlázási egység *és* egy prémium szintű számlázási egységet. hello Feladatütemező szolgáltatás váltók hello tooeither standard vagy prémium; beállított aktív feladat gyűjtemények száma alapján Ennek a magyarázatát a következő két szakasz hello további.

## <a name="standard-billable-units"></a>Standard számlázható egység
Egy szabványos számlázható egység fel too10 szabványos feladatgyűjteményei tartalmazhatnak. Mivel a szabványos feladatgyűjtemény feladat gyűjteményenként too50 feladatot is rendelkeznek, egy standard számlázási egységet lehetővé teszi, hogy egy előfizetés toohave too500 feladatot – tooalmost 22 millió feladat végrehajtások havonta fel.

Ha szabványos feladatgyűjteményei 1 és 10 között, akkor 1 standard számlázási egység fogjuk számlázni. Ha szabványos feladatgyűjteményei 11 és 20 közötti, hogy 2 standard számlázási egység fogjuk számlázni. Ha 21 és 30 szabványos feladatgyűjteményei között, 3 standard számlázási egység fogjuk számlázni, és így tovább.

## <a name="p10-premium-billable-units"></a>P10 Prémium számlázható egységek
Egy P10 prémium számlázható egység fel too10, 000 P10 prémium feladatgyűjteményei tartalmazhatnak. Mivel egy P10 prémium feladatgyűjtemény feladat gyűjteményenként too50 feladatot is rendelkeznek, egy prémium szintű számlázási egységet lehetővé teszi, hogy egy előfizetés toohave too500, 000 feladatok be – tooalmost 22 milliárd feladat végrehajtások havonta fel.

Ha prémium szintű feladat gyűjtemények 1 és 10 000 között, akkor 1 P10 prémium számlázási egység fogjuk számlázni. Ha 10,001 és 20 000 prémium feladatgyűjteményei között, 2 P10 prémium számlázási egység fogjuk számlázni, és így tovább.

Ebből kifolyólag a gyűjteményeknél P10 prémium feladat hello ugyanezeket a funkciókat, hello szabványos feladatgyűjteményei de adjon meg ár szünet abban az esetben, ha az alkalmazás által igényelt feladatgyűjteményei számos.

## <a name="p20-premium-billable-units"></a>P20 Prémium számlázható egységek
P20 prémium számlázható egység fel too5, 000 P20 prémium feladatgyűjteményei tartalmazhatnak. Óta egy P20 prémium feladatgyűjtemény legfeljebb too1, 000 feladatok feladat gyűjteményenként tartalmazhat egy prémium szintű számlázási egységet lehetővé teszi, hogy egy előfizetés toohave be too5, 000 000 feladatok – tooalmost 220 milliárd feladat végrehajtások havonta fel.

P20 prémium feladatgyűjteményei hello P10 prémium feladatgyűjteményeket, ugyanazokat a képességeket biztosít, azonban további méretezhetőséget / feladatgyűjtemény és egy nagyobb feladatok teljes száma általános P10 prémium így Ön toohave-nál nagyobb számú feladatok is támogatja.

## <a name="billing-and-active-status"></a>Számlázási és aktív állapota
Feladatgyűjtemények mindig aktívak, kivéve, ha a teljes előfizetés toobilling problémák miatt egyes ideiglenes letiltott állapotba állapotba került. hello csak az, hogy a feladatgyűjtemény nem lesz számlázva módon tooensure tooeither állítsa toohello *szabad* terv vagy toodelete hello feladatgyűjtemény.

Bár előfordulhat, hogy letiltja a feladatokhoz minden esetben a feladatgyűjtemény egyetlen műveletben, nem változtatja meg hello feladatgyűjtemény hello számlázási állapota – hello feladatgyűjtemény fog *továbbra is* kell fizetni. Hasonlóképpen üres feladatgyűjteményei aktív számít, és lesz terhelve.

## <a name="pricing"></a>Díjszabás
A díjszabás részleteit, lásd: [Feladatütemező árképzési](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Lásd még:
 [A Scheduler ismertetése](scheduler-intro.md)

 [Az Azure Scheduler alapfogalmai, terminológiája és entitáshierarchiája](scheduler-concepts-terms.md)

 [Az ütemező hello Azure-portálon az első lépéseiben](scheduler-get-started-portal.md)

 [Az Azure Scheduler REST API-jának leírása](https://msdn.microsoft.com/library/mt629143)

 [Az Azure Scheduler PowerShell-parancsmagjainak leírása](scheduler-powershell-reference.md)

 [Azure Scheduler – magas fokú rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [Azure Scheduler – korlátozások, alapértékek és hibakódok](scheduler-limits-defaults-errors.md)

 [Kimenő hitelesítés az Azure Schedulerben](scheduler-outbound-authentication.md)

