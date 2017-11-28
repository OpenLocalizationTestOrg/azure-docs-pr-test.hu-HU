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
# <a name="ports-beyond-1433-for-adonet-45"></a><span data-ttu-id="93978-104">Port túl az 1433-as ADO.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="93978-104">Ports beyond 1433 for ADO.NET 4.5</span></span>
<span data-ttu-id="93978-105">Ez a témakör ismerteti a hello Azure SQL adatbázis-kapcsolat viselkedését ADO.NET 4.5 vagy újabb verzióját használó ügyfelek számára.</span><span class="sxs-lookup"><span data-stu-id="93978-105">This topic describes hello Azure SQL Database connection behavior for clients that use ADO.NET 4.5 or a later version.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="93978-106">Kapcsolat architektúra kapcsolatos információkért lásd: [Azure SQL adatbázis-kapcsolat architektúra](sql-database-connectivity-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="93978-106">For information about connectivity architecture, see [Azure SQL Database connectivity architecture](sql-database-connectivity-architecture.md).</span></span>
>

## <a name="outside-vs-inside"></a><span data-ttu-id="93978-107">Kívül és belül</span><span class="sxs-lookup"><span data-stu-id="93978-107">Outside vs inside</span></span>
<span data-ttu-id="93978-108">A kapcsolatok tooAzure SQL-adatbázis, azt először kérje meg hogy fut-e az ügyfélprogram *kívül* vagy *belül* hello Azure felhőben határ.</span><span class="sxs-lookup"><span data-stu-id="93978-108">For connections tooAzure SQL Database, we must first ask whether your client program runs *outside* or *inside* hello Azure cloud boundary.</span></span> <span data-ttu-id="93978-109">hello alszakaszokat két gyakori helyzetek tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="93978-109">hello subsections discuss two common scenarios.</span></span>

#### <a name="outside-client-runs-on-your-desktop-computer"></a><span data-ttu-id="93978-110">*Külső:* ügyfél futtatja az asztali számítógépen</span><span class="sxs-lookup"><span data-stu-id="93978-110">*Outside:* Client runs on your desktop computer</span></span>
<span data-ttu-id="93978-111">Hello egyetlen port, amelyet az asztali számítógép, amelyen az SQL-adatbázis ügyfélalkalmazást nyitva kell lennie az 1433-as portot használja.</span><span class="sxs-lookup"><span data-stu-id="93978-111">Port 1433 is hello only port that must be open on your desktop computer that hosts your SQL Database client application.</span></span>

#### <a name="inside-client-runs-on-azure"></a><span data-ttu-id="93978-112">*Belső:* ügyfél futtatja az Azure-on</span><span class="sxs-lookup"><span data-stu-id="93978-112">*Inside:* Client runs on Azure</span></span>
<span data-ttu-id="93978-113">Amikor az ügyfél hello Azure felhőben határán belül fut, az úgynevezett is egy *közvetlen útvonal* toointeract hello SQL adatbázis-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="93978-113">When your client runs inside hello Azure cloud boundary, it uses what we can call a *direct route* toointeract with hello SQL Database server.</span></span> <span data-ttu-id="93978-114">A kapcsolat létrejötte után hello ügyfél és az adatbázis közötti további interakciókat tartalmaz, amely nincs köztes proxy.</span><span class="sxs-lookup"><span data-stu-id="93978-114">After a connection is established, further interactions between hello client and database involve no middleware proxy.</span></span>

<span data-ttu-id="93978-115">hello feladatütemezési a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="93978-115">hello sequence is as follows:</span></span>

1. <span data-ttu-id="93978-116">Az ADO.NET 4.5 (vagy újabb) indít el egy rövid interakcióba hello Azure-felhőbe, és dinamikusan azonosított portszámot kap.</span><span class="sxs-lookup"><span data-stu-id="93978-116">ADO.NET 4.5 (or later) initiates a brief interaction with hello Azure cloud, and receives a dynamically identified port number.</span></span>
   
   * <span data-ttu-id="93978-117">dinamikusan azonosított hello portszám 11000-11999 vagy 14000-14999 hello hatótávolságon belül van.</span><span class="sxs-lookup"><span data-stu-id="93978-117">hello dynamically identified port number is in hello range of 11000-11999 or 14000-14999.</span></span>
2. <span data-ttu-id="93978-118">Az ADO.NET majd kapcsolódik toohello SQL adatbázis-kiszolgálót közvetlenül, nincs köztes a kettő között.</span><span class="sxs-lookup"><span data-stu-id="93978-118">ADO.NET then connects toohello SQL Database server directly, with no middleware in between.</span></span>
3. <span data-ttu-id="93978-119">Lekérdezések közvetlenül toohello adatbázis, illetve az eredményeinek közvetlenül toohello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="93978-119">Queries are sent directly toohello database, and results are returned directly toohello client.</span></span>

<span data-ttu-id="93978-120">Győződjön meg arról, hogy SQL-adatbázis ADO.NET 4.5 ügyfél interakció 11000-11999 és az Azure ügyfélgépre 14000-14999 tartományok pedig megmarad hello port.</span><span class="sxs-lookup"><span data-stu-id="93978-120">Ensure that hello port ranges of 11000-11999 and 14000-14999 on your Azure client machine are left available for ADO.NET 4.5 client interactions with SQL Database.</span></span>

* <span data-ttu-id="93978-121">Portok hello közé különösen, bármilyen más kimenő blockers ingyenes kell lennie.</span><span class="sxs-lookup"><span data-stu-id="93978-121">In particular, ports in hello range must be free of any other outbound blockers.</span></span>
* <span data-ttu-id="93978-122">Az Azure virtuális gépen, hello **fokozott biztonságú Windows tűzfal** vezérlők hello portbeállításokat.</span><span class="sxs-lookup"><span data-stu-id="93978-122">On your Azure VM, hello **Windows Firewall with Advanced Security** controls hello port settings.</span></span>
  
  * <span data-ttu-id="93978-123">Használhatja a hello [tűzfal felhasználói felület](http://msdn.microsoft.com/library/cc646023.aspx) egy szabályt, amelynek meg hello tooadd **TCP** porttartomány hello szintaxissal együtt protokoll, például **11000-11999**.</span><span class="sxs-lookup"><span data-stu-id="93978-123">You can use hello [firewall's user interface](http://msdn.microsoft.com/library/cc646023.aspx) tooadd a rule for which you specify hello **TCP** protocol along with a port range with hello syntax like **11000-11999**.</span></span>

## <a name="version-clarifications"></a><span data-ttu-id="93978-124">Verzió pontosítások</span><span class="sxs-lookup"><span data-stu-id="93978-124">Version clarifications</span></span>
<span data-ttu-id="93978-125">Ez a szakasz hello monikerek tooproduct verziók hivatkozó értelmezi.</span><span class="sxs-lookup"><span data-stu-id="93978-125">This section clarifies hello monikers that refer tooproduct versions.</span></span> <span data-ttu-id="93978-126">Emellett néhány párosítása a verziók közötti termékeket sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="93978-126">It also lists some pairings of versions between products.</span></span>

#### <a name="adonet"></a><span data-ttu-id="93978-127">ADO.NET</span><span class="sxs-lookup"><span data-stu-id="93978-127">ADO.NET</span></span>
* <span data-ttu-id="93978-128">Az ADO.NET 4.0 hello TDS 7.3 protokollt, de nem 7.4 támogatja.</span><span class="sxs-lookup"><span data-stu-id="93978-128">ADO.NET 4.0 supports hello TDS 7.3 protocol, but not 7.4.</span></span>
* <span data-ttu-id="93978-129">Az ADO.NET 4.5-ös vagy újabb hello TDS 7.4 protokoll használatát támogatja.</span><span class="sxs-lookup"><span data-stu-id="93978-129">ADO.NET 4.5 and later supports hello TDS 7.4 protocol.</span></span>

## <a name="related-links"></a><span data-ttu-id="93978-130">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="93978-130">Related links</span></span>
* <span data-ttu-id="93978-131">2015. július 20 ADO.NET 4.6 lett szabadítva.</span><span class="sxs-lookup"><span data-stu-id="93978-131">ADO.NET 4.6 was released on July 20, 2015.</span></span> <span data-ttu-id="93978-132">Érhető el egy hello .NET csapat blogja közlemény [Itt](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).</span><span class="sxs-lookup"><span data-stu-id="93978-132">A blog announcement from hello .NET team is available [here](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).</span></span>
* <span data-ttu-id="93978-133">Az ADO.NET 4.5 2012 augusztus 15 lett szabadítva.</span><span class="sxs-lookup"><span data-stu-id="93978-133">ADO.NET 4.5 was released on August 15, 2012.</span></span> <span data-ttu-id="93978-134">Érhető el egy hello .NET csapat blogja közlemény [Itt](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).</span><span class="sxs-lookup"><span data-stu-id="93978-134">A blog announcement from hello .NET team is available [here](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).</span></span>
  
  * <span data-ttu-id="93978-135">Az ADO.NET 4.5.1 kapcsolatos blogbejegyzést érhető [Itt](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).</span><span class="sxs-lookup"><span data-stu-id="93978-135">A blog post about ADO.NET 4.5.1 is available [here](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).</span></span>
* [<span data-ttu-id="93978-136">TDS protokoll verzió lista</span><span class="sxs-lookup"><span data-stu-id="93978-136">TDS protocol version list</span></span>](http://www.freetds.org/userguide/tdshistory.htm)
* [<span data-ttu-id="93978-137">SQL adatbázis-fejlesztői áttekintés</span><span class="sxs-lookup"><span data-stu-id="93978-137">SQL Database Development Overview</span></span>](sql-database-develop-overview.md)
* [<span data-ttu-id="93978-138">Az Azure SQL Database-tűzfal</span><span class="sxs-lookup"><span data-stu-id="93978-138">Azure SQL Database firewall</span></span>](sql-database-firewall-configure.md)
* [<span data-ttu-id="93978-139">Útmutató: az SQL Database tűzfalbeállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="93978-139">How to: Configure firewall settings on SQL Database</span></span>](sql-database-configure-firewall-settings.md)

