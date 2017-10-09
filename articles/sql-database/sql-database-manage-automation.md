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
# <a name="managing-azure-sql-databases-using-azure-automation"></a><span data-ttu-id="98fb4-103">Azure SQL Database-adatbázisok Azure Automation kezelése</span><span class="sxs-lookup"><span data-stu-id="98fb4-103">Managing Azure SQL Databases using Azure Automation</span></span>
<span data-ttu-id="98fb4-104">Az útmutatóból megismerheti toohello Azure Automation szolgáltatás, és hogyan lehet az Azure SQL Database adatbázisok használt toosimplify kezelését.</span><span class="sxs-lookup"><span data-stu-id="98fb4-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of your Azure SQL databases.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="98fb4-105">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="98fb4-105">What is Azure Automation?</span></span>
<span data-ttu-id="98fb4-106">[Azure Automation szolgáltatásbeli](https://azure.microsoft.com/services/automation/) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén.</span><span class="sxs-lookup"><span data-stu-id="98fb4-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="98fb4-107">Azure Automation használ, hosszan futó, manuális, hibákhoz vezethet, és gyakran ismétlődő feladatokat lehet automatizált tooincrease megbízhatóságát, hatékonyságát és idő toovalue a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="98fb4-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="98fb4-108">Azure Automation szolgáltatásbeli biztosít a magas rendelkezésre állású és nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely méretezi a igényeinek toomeet a szervezet növekedésének megfelelően.</span><span class="sxs-lookup"><span data-stu-id="98fb4-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales toomeet your needs as your organization grows.</span></span> <span data-ttu-id="98fb4-109">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="98fb4-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="98fb4-110">Működési munkaterhelés csökkentése és az szabadítson fel informatikai / DevOps személyzet toofocus munka, amely azáltal, hogy a felhő felügyeleti feladatok toobe üzleti értéket hozzáadja az Azure Automation automatikusan futtatásához.</span><span class="sxs-lookup"><span data-stu-id="98fb4-110">Lower operational overhead and free up IT / DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a><span data-ttu-id="98fb4-111">Hogyan segíthet az Azure Automation Azure SQL-adatbázisok kezelése?</span><span class="sxs-lookup"><span data-stu-id="98fb4-111">How can Azure Automation help manage Azure SQL databases?</span></span>
<span data-ttu-id="98fb4-112">Az Azure SQL Database hello segítségével kezelhető az Azure Automationben [Azure SQL Database PowerShell parancsmagjait](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) elérhető az hello [Azure PowerShell eszközök](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="98fb4-112">Azure SQL Database can be managed in Azure Automation by using hello [Azure SQL Database PowerShell cmdlets](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) that are available in hello [Azure PowerShell tools](/powershell/azure/overview).</span></span> <span data-ttu-id="98fb4-113">Azure Automation a elérhető Azure SQL Database PowerShell parancsmagok hello kezdő verzióról rendelkezik, így lehet elvégezni, az SQL-adatbázis a felügyeleti feladatok hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="98fb4-113">Azure Automation has these Azure SQL Database PowerShell cmdlets available out of hello box, so that you can perform all of your SQL DB management tasks within hello service.</span></span> <span data-ttu-id="98fb4-114">Ezeket a parancsmagokat az Azure Automationben más Azure-szolgáltatások, az összetett feladatok tooautomate hello-parancsmagjaival is párosítható az Azure-szolgáltatások és a 3. fél rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="98fb4-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="98fb4-115">Azure Automation szolgáltatásbeli is rendelkezik hello képességét toocommunicate az SQL Server-kiszolgálók közvetlenül, PowerShell-lel SQL-parancsok beírásával.</span><span class="sxs-lookup"><span data-stu-id="98fb4-115">Azure Automation also has hello ability toocommunicate with SQL servers directly, by issuing SQL commands using PowerShell.</span></span>

<span data-ttu-id="98fb4-116">Hello [Azure Automation-runbook gyűjtemény](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) termék csoportját, és a Közösség runbookok tooget lépések az Azure SQL-adatbázisok, más Azure-szolgáltatások és a 3. fél rendszerek felügyeletének automatizálására különböző tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="98fb4-116">hello [Azure Automation runbook gallery](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) contains a variety of product team and community runbooks tooget started automating management of Azure SQL Databases, other Azure services, and 3rd party systems.</span></span> <span data-ttu-id="98fb4-117">Gyűjteményelem forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="98fb4-117">Gallery runbooks include:</span></span>

* [<span data-ttu-id="98fb4-118">SQL Server-adatbázis SQL-lekérdezések futtatásához</span><span class="sxs-lookup"><span data-stu-id="98fb4-118">Run SQL queries against a SQL Server database</span></span>](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
* [<span data-ttu-id="98fb4-119">Függőleges méretezni a (felfelé vagy lefelé) Azure SQL adatbázis ütemezés szerint</span><span class="sxs-lookup"><span data-stu-id="98fb4-119">Vertically scale (up or down) an Azure SQL Database on a schedule</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
* [<span data-ttu-id="98fb4-120">Egy SQL-tábla csonkolni, ha az adatbázis megközelíti a maximális méretét</span><span class="sxs-lookup"><span data-stu-id="98fb4-120">Truncate a SQL table if its database approaches its maximum size</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
* [<span data-ttu-id="98fb4-121">Azure SQL adatbázis táblák index, ha azok nagy mértékben töredezettek</span><span class="sxs-lookup"><span data-stu-id="98fb4-121">Index tables in an Azure SQL Database if they are highly fragmented</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a><span data-ttu-id="98fb4-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98fb4-122">Next steps</span></span>
<span data-ttu-id="98fb4-123">Most, hogy megismerte az Azure Automation, és hogyan lehet használt toomanage Azure SQL-adatbázisok hello alapjait, kövesse az alábbi hivatkozások toolearn Azure automatizálásával kapcsolatos további.</span><span class="sxs-lookup"><span data-stu-id="98fb4-123">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure SQL databases, follow these links toolearn more about Azure Automation.</span></span>

* [<span data-ttu-id="98fb4-124">Azure Automation – áttekintés</span><span class="sxs-lookup"><span data-stu-id="98fb4-124">Azure Automation Overview</span></span>](../automation/automation-intro.md)
* [<span data-ttu-id="98fb4-125">Az első runbookom</span><span class="sxs-lookup"><span data-stu-id="98fb4-125">My first runbook</span></span>](../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="98fb4-126">Azure Automation tanulási térkép</span><span class="sxs-lookup"><span data-stu-id="98fb4-126">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)
* [<span data-ttu-id="98fb4-127">Azure Automation szolgáltatásbeli: Az SQL Agent hello felhő</span><span class="sxs-lookup"><span data-stu-id="98fb4-127">Azure Automation: Your SQL Agent in hello Cloud</span></span>](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 

