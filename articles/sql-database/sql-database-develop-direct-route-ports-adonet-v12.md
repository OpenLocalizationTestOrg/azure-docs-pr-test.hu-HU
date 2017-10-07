---
title: "túl az 1433-as számú SQL-adatbázis aaaPorts |} Microsoft Docs"
description: "Az ADO.NET tooAzure SQL-adatbázis érkező ügyfélkapcsolatokat néha hello proxy megkerülése és hello adatbázis közvetlenül kommunikál. Ekkor válnak fontossá az 1433-astól különböző portok."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
ms.assetid: 3f17106a-92fd-4aa4-b6a9-1daa29421f64
ms.service: sql-database
ms.custom: develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: sstein
ms.openlocfilehash: a35ff2d827ae3fa29b3ea855dbb7ed78583c82eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ports-beyond-1433-for-adonet-45"></a>Port túl az 1433-as ADO.NET 4.5
Ez a témakör ismerteti a hello Azure SQL adatbázis-kapcsolat viselkedését ADO.NET 4.5 vagy újabb verzióját használó ügyfelek számára. 

> [!IMPORTANT]
> Kapcsolat architektúra kapcsolatos információkért lásd: [Azure SQL adatbázis-kapcsolat architektúra](sql-database-connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Kívül és belül
A kapcsolatok tooAzure SQL-adatbázis, azt először kérje meg hogy fut-e az ügyfélprogram *kívül* vagy *belül* hello Azure felhőben határ. hello alszakaszokat két gyakori helyzetek tárgyalja.

#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Külső:* ügyfél futtatja az asztali számítógépen
Hello egyetlen port, amelyet az asztali számítógép, amelyen az SQL-adatbázis ügyfélalkalmazást nyitva kell lennie az 1433-as portot használja.

#### <a name="inside-client-runs-on-azure"></a>*Belső:* ügyfél futtatja az Azure-on
Amikor az ügyfél hello Azure felhőben határán belül fut, az úgynevezett is egy *közvetlen útvonal* toointeract hello SQL adatbázis-kiszolgálóval. A kapcsolat létrejötte után hello ügyfél és az adatbázis közötti további interakciókat tartalmaz, amely nincs köztes proxy.

hello feladatütemezési a következőképpen történik:

1. Az ADO.NET 4.5 (vagy újabb) indít el egy rövid interakcióba hello Azure-felhőbe, és dinamikusan azonosított portszámot kap.
   
   * dinamikusan azonosított hello portszám 11000-11999 vagy 14000-14999 hello hatótávolságon belül van.
2. Az ADO.NET majd kapcsolódik toohello SQL adatbázis-kiszolgálót közvetlenül, nincs köztes a kettő között.
3. Lekérdezések közvetlenül toohello adatbázis, illetve az eredményeinek közvetlenül toohello ügyfél.

Győződjön meg arról, hogy SQL-adatbázis ADO.NET 4.5 ügyfél interakció 11000-11999 és az Azure ügyfélgépre 14000-14999 tartományok pedig megmarad hello port.

* Portok hello közé különösen, bármilyen más kimenő blockers ingyenes kell lennie.
* Az Azure virtuális gépen, hello **fokozott biztonságú Windows tűzfal** vezérlők hello portbeállításokat.
  
  * Használhatja a hello [tűzfal felhasználói felület](http://msdn.microsoft.com/library/cc646023.aspx) egy szabályt, amelynek meg hello tooadd **TCP** porttartomány hello szintaxissal együtt protokoll, például **11000-11999**.

## <a name="version-clarifications"></a>Verzió pontosítások
Ez a szakasz hello monikerek tooproduct verziók hivatkozó értelmezi. Emellett néhány párosítása a verziók közötti termékeket sorolja fel.

#### <a name="adonet"></a>ADO.NET
* Az ADO.NET 4.0 hello TDS 7.3 protokollt, de nem 7.4 támogatja.
* Az ADO.NET 4.5-ös vagy újabb hello TDS 7.4 protokoll használatát támogatja.

## <a name="related-links"></a>Kapcsolódó hivatkozások
* 2015. július 20 ADO.NET 4.6 lett szabadítva. Érhető el egy hello .NET csapat blogja közlemény [Itt](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).
* Az ADO.NET 4.5 2012 augusztus 15 lett szabadítva. Érhető el egy hello .NET csapat blogja közlemény [Itt](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
  
  * Az ADO.NET 4.5.1 kapcsolatos blogbejegyzést érhető [Itt](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).
* [TDS protokoll verzió lista](http://www.freetds.org/userguide/tdshistory.htm)
* [SQL adatbázis-fejlesztői áttekintés](sql-database-develop-overview.md)
* [Az Azure SQL Database-tűzfal](sql-database-firewall-configure.md)
* [Útmutató: az SQL Database tűzfalbeállításainak konfigurálása](sql-database-configure-firewall-settings.md)

