---
title: "Adatbázis alkalmazások fejlesztése MySQL az Azure-adatbázis – áttekintés |} Microsoft Docs"
description: "Bemutatja, hogy a fejlesztő kell követnie, MySQL Azure adatbázishoz való kapcsolódáshoz alkalmazáskód írásakor kialakítási szempontok"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 350dd775e172120d806d1193877a34d94f4d3f6a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a><span data-ttu-id="fb300-103">Alkalmazások fejlesztése MySQL az Azure-adatbázis – áttekintés</span><span class="sxs-lookup"><span data-stu-id="fb300-103">Application development overview for Azure Database for MySQL</span></span> 
<span data-ttu-id="fb300-104">Ez a cikk ismerteti, hogy a fejlesztő kell követnie, MySQL Azure adatbázishoz való kapcsolódáshoz alkalmazáskód írásakor kialakítási szempontok</span><span class="sxs-lookup"><span data-stu-id="fb300-104">This article discusses design considerations that a developer should follow when writing application code to connect to Azure Database for MySQL</span></span> 

> [!TIP]
> <span data-ttu-id="fb300-105">Az oktatóanyag bemutatja, hogyan hozzon létre egy kiszolgálót, hozzon létre egy kiszolgáló-alapú tűzfal, kiszolgáló tulajdonságainak megtekintése, adatbázis létrehozása, csatlakozás és lekérdezés munkaterület és mysql.exe használatával, lásd: [tervezése az első Azure-beli MySQL database](tutorial-design-database-using-portal.md)</span><span class="sxs-lookup"><span data-stu-id="fb300-105">For a tutorial showing you how to create a server, create a server-based firewall, view server properties, create database, connect and query using workbench and mysql.exe, see [Design your first Azure MySQL database](tutorial-design-database-using-portal.md)</span></span>

## <a name="language-and-platform"></a><span data-ttu-id="fb300-106">Nyelv és platform</span><span class="sxs-lookup"><span data-stu-id="fb300-106">Language and platform</span></span>
<span data-ttu-id="fb300-107">Különböző programozási nyelvekhez és platformokhoz érhetők el kódminták.</span><span class="sxs-lookup"><span data-stu-id="fb300-107">There are code samples available for various programming languages and platforms.</span></span> <span data-ttu-id="fb300-108">A következő mintakódok hivatkozásait megtalálhatja: [MySQL Azure adatbázishoz való kapcsolódáshoz használt kapcsolódási függvénytárak](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="fb300-108">You can find links to the code samples at: [Connectivity libraries used to connect to Azure Database for MySQL](concepts-connection-libraries.md)</span></span>

## <a name="tools"></a><span data-ttu-id="fb300-109">Eszközök</span><span class="sxs-lookup"><span data-stu-id="fb300-109">Tools</span></span>
<span data-ttu-id="fb300-110">Azure MySQL-adatbázis MySQL közösségi verzióját használja, MySQL munkaterület vagy MySQL segédprogramok mysql.exe, mint például az általános kezelőeszközöket kompatibilis [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), stb.</span><span class="sxs-lookup"><span data-stu-id="fb300-110">Azure Database for MySQL uses the MySQL community version, compatible with MySQL common management tools such as Workbench or MySQL utilities such as mysql.exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), and others.</span></span> <span data-ttu-id="fb300-111">Az Azure portál, az Azure CLI és a REST API-k használatával is kommunikálni az adatbázis-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fb300-111">You can also use the Azure portal, Azure CLI, and REST APIs to interact with the database service.</span></span>

## <a name="resource-limitations"></a><span data-ttu-id="fb300-112">Erőforrás-korlátozások</span><span class="sxs-lookup"><span data-stu-id="fb300-112">Resource limitations</span></span>
<span data-ttu-id="fb300-113">Azure-beli MySQL adatbázis két különböző mechanizmusok használatával egy kiszolgáló számára elérhető erőforrások kezelése:</span><span class="sxs-lookup"><span data-stu-id="fb300-113">Azure MySQL Database manages the resources available to a server using two different mechanisms:</span></span> 
- <span data-ttu-id="fb300-114">Erőforrások irányítás</span><span class="sxs-lookup"><span data-stu-id="fb300-114">Resources Governance</span></span> 
- <span data-ttu-id="fb300-115">A korlátozások érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="fb300-115">Enforcement of Limits.</span></span>

## <a name="security"></a><span data-ttu-id="fb300-116">Biztonság</span><span class="sxs-lookup"><span data-stu-id="fb300-116">Security</span></span>
<span data-ttu-id="fb300-117">Azure-beli MySQL adatbázis korlátozó hozzáférési, a védelmet nyújtó adatokat, a konfigurálását a felhasználók és a szerepkör és a figyelési tevékenységek a MySQL-adatbázis forrásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="fb300-117">Azure MySQL Database provides resources for limiting access, protecting data, configuring users and role, and monitoring activities on a MySQL Database.</span></span>

## <a name="authentication"></a><span data-ttu-id="fb300-118">Authentication</span><span class="sxs-lookup"><span data-stu-id="fb300-118">Authentication</span></span>
<span data-ttu-id="fb300-119">Azure-beli MySQL adatbázis támogatja a kiszolgáló hitelesítése a felhasználók és bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="fb300-119">Azure MySQL Database supports server authentication of users and logins.</span></span>

## <a name="resiliency"></a><span data-ttu-id="fb300-120">Resiliency</span><span class="sxs-lookup"><span data-stu-id="fb300-120">Resiliency</span></span>
<span data-ttu-id="fb300-121">Amikor egy átmeneti hiba akkor fordul elő, amikor kapcsolódni próbált MySQL-adatbázis, a kódot kell próbálja meg újra a hívást.</span><span class="sxs-lookup"><span data-stu-id="fb300-121">When a transient error occurs while connecting to MySQL Database, your code should retry the call.</span></span> <span data-ttu-id="fb300-122">Javasoljuk az újrapróbálkozási logika használata készítsen biztonsági logika, így azt nem ne terhelje tovább az SQL-adatbázis az újrapróbálkozás egyszerre több ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="fb300-122">We recommend the retry logic use back off logic, so that it does not overwhelm the SQL Database with multiple clients retrying simultaneously.</span></span>

- <span data-ttu-id="fb300-123">Kódminták: mintakódok, mely újrapróbálkozási logika, lásd: a következő nyelvű-példák: [MySQL Azure adatbázishoz való kapcsolódáshoz használt kapcsolódási függvénytárak](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="fb300-123">Code samples: For code samples that illustrate retry logic, see samples for the language of your choice at: [Connectivity libraries used to connect to Azure Database for MySQL](concepts-connection-libraries.md)</span></span>

## <a name="managing-connections"></a><span data-ttu-id="fb300-124">Kapcsolatok kezelése</span><span class="sxs-lookup"><span data-stu-id="fb300-124">Managing Connections</span></span>
<span data-ttu-id="fb300-125">Adatbázis-kapcsolatok egy korlátozott erőforrás, a jobb teljesítmény érdekében a MySQL-adatbázis elérésekor kapcsolat ésszerű használata javasolt.</span><span class="sxs-lookup"><span data-stu-id="fb300-125">Database connections are a limited resource, so we recommend sensible use of connections when accessing your MySQL Database to achieve better performance.</span></span>
- <span data-ttu-id="fb300-126">Kapcsolatkészletezést vagy állandó kapcsolat használatával érik el az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="fb300-126">Access the database by using connection pooling or persistent connections.</span></span>
- <span data-ttu-id="fb300-127">Rövid kapcsolat élettartamát használatával érik el az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="fb300-127">Access the database by using short connection life span.</span></span> 
- <span data-ttu-id="fb300-128">Újrapróbálkozási logika helyén a kapcsolódási kísérlet az alkalmazás használatához van szüksége a hiba oka, hogy az egyidejű kapcsolatok száma elérte a megengedett.</span><span class="sxs-lookup"><span data-stu-id="fb300-128">Use retry logic in your application at the point of the connection attempt, to catch failures due to concurrent connections have reached the maximum allowed.</span></span> <span data-ttu-id="fb300-129">Az újrapróbálkozási logika be egy rövid ideig eltart, majd várjon véletlenszerű ideje előtt a további csatlakozási kísérletek.</span><span class="sxs-lookup"><span data-stu-id="fb300-129">In the retry logic, set a short delay, and then wait for a random time before the additional connection attempts.</span></span>