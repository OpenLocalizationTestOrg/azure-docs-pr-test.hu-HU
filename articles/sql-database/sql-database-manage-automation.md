---
title: "Azure SQL Database-adatbázisok Azure Automation aaaManage |} Microsoft Docs"
description: "További információk a hello Azure Automation szolgáltatás hogyan lehet használt toomanage Azure SQL-adatbázisok méretekben."
services: sql-database, automation
documentationcenter: 
author: jodoglevy
manager: jhubbard
editor: monicar
ms.assetid: 77c262a1-9b93-456d-b3c7-b2f23bdfcd61
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: jhubbard
ms.openlocfilehash: d613cca32ba86e27b9c1b952c4e723ea7f07beb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a>Azure SQL Database-adatbázisok Azure Automation kezelése
Az útmutatóból megismerheti toohello Azure Automation szolgáltatás, és hogyan lehet az Azure SQL Database adatbázisok használt toosimplify kezelését.

## <a name="what-is-azure-automation"></a>Mi az Azure Automation?
[Azure Automation szolgáltatásbeli](https://azure.microsoft.com/services/automation/) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén. Azure Automation használ, hosszan futó, manuális, hibákhoz vezethet, és gyakran ismétlődő feladatokat lehet automatizált tooincrease megbízhatóságát, hatékonyságát és idő toovalue a szervezet számára.

Azure Automation szolgáltatásbeli biztosít a magas rendelkezésre állású és nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely méretezi a igényeinek toomeet a szervezet növekedésének megfelelően. Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.

Működési munkaterhelés csökkentése és az szabadítson fel informatikai / DevOps személyzet toofocus munka, amely azáltal, hogy a felhő felügyeleti feladatok toobe üzleti értéket hozzáadja az Azure Automation automatikusan futtatásához.

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Hogyan segíthet az Azure Automation Azure SQL-adatbázisok kezelése?
Az Azure SQL Database hello segítségével kezelhető az Azure Automationben [Azure SQL Database PowerShell parancsmagjait](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) elérhető az hello [Azure PowerShell eszközök](/powershell/azure/overview). Azure Automation a elérhető Azure SQL Database PowerShell parancsmagok hello kezdő verzióról rendelkezik, így lehet elvégezni, az SQL-adatbázis a felügyeleti feladatok hello szolgáltatást. Ezeket a parancsmagokat az Azure Automationben más Azure-szolgáltatások, az összetett feladatok tooautomate hello-parancsmagjaival is párosítható az Azure-szolgáltatások és a 3. fél rendszerek között.

Azure Automation szolgáltatásbeli is rendelkezik hello képességét toocommunicate az SQL Server-kiszolgálók közvetlenül, PowerShell-lel SQL-parancsok beírásával.

Hello [Azure Automation-runbook gyűjtemény](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) termék csoportját, és a Közösség runbookok tooget lépések az Azure SQL-adatbázisok, más Azure-szolgáltatások és a 3. fél rendszerek felügyeletének automatizálására különböző tartalmaz. Gyűjteményelem forgatókönyvek a következők:

* [SQL Server-adatbázis SQL-lekérdezések futtatásához](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
* [Függőleges méretezni a (felfelé vagy lefelé) Azure SQL adatbázis ütemezés szerint](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
* [Egy SQL-tábla csonkolni, ha az adatbázis megközelíti a maximális méretét](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
* [Azure SQL adatbázis táblák index, ha azok nagy mértékben töredezettek](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte az Azure Automation, és hogyan lehet használt toomanage Azure SQL-adatbázisok hello alapjait, kövesse az alábbi hivatkozások toolearn Azure automatizálásával kapcsolatos további.

* [Azure Automation – áttekintés](../automation/automation-intro.md)
* [Az első runbookom](../automation/automation-first-runbook-graphical.md)
* [Azure Automation tanulási térkép](https://azure.microsoft.com/documentation/learning-paths/automation/)
* [Azure Automation szolgáltatásbeli: Az SQL Agent hello felhő](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 

