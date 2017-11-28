---
title: "aaaDatabase alkalmazások fejlesztése MySQL az Azure-adatbázis – áttekintés |} Microsoft Docs"
description: "Bemutatja, hogy a fejlesztő kell követnie, MySQL alkalmazás kód tooconnect tooAzure adatbázis írásakor kialakítási szempontok"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: f08df605eba21b4ba4b43565c0a7ded95779a171
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a><span data-ttu-id="618ff-103">Alkalmazások fejlesztése MySQL az Azure-adatbázis – áttekintés</span><span class="sxs-lookup"><span data-stu-id="618ff-103">Application development overview for Azure Database for MySQL</span></span> 
<span data-ttu-id="618ff-104">Ez a cikk ismerteti, hogy a fejlesztő kell követnie, MySQL alkalmazás kód tooconnect tooAzure adatbázis írásakor kialakítási szempontok</span><span class="sxs-lookup"><span data-stu-id="618ff-104">This article discusses design considerations that a developer should follow when writing application code tooconnect tooAzure Database for MySQL</span></span> 

> [!TIP]
> <span data-ttu-id="618ff-105">Egy oktatóanyag megjelenítő, hogyan toocreate egy kiszolgálót, hozzon létre egy kiszolgáló-alapú tűzfal kiszolgáló tulajdonságainak megtekintése, létrehozásakor adatbázis, kapcsolódás és lekérdezés munkaterület és mysql.exe használatával, lásd: [tervezése az első Azure-beli MySQL database](tutorial-design-database-using-portal.md)</span><span class="sxs-lookup"><span data-stu-id="618ff-105">For a tutorial showing you how toocreate a server, create a server-based firewall, view server properties, create database, connect and query using workbench and mysql.exe, see [Design your first Azure MySQL database](tutorial-design-database-using-portal.md)</span></span>

## <a name="language-and-platform"></a><span data-ttu-id="618ff-106">Nyelv és platform</span><span class="sxs-lookup"><span data-stu-id="618ff-106">Language and platform</span></span>
<span data-ttu-id="618ff-107">Különböző programozási nyelvekhez és platformokhoz érhetők el kódminták.</span><span class="sxs-lookup"><span data-stu-id="618ff-107">There are code samples available for various programming languages and platforms.</span></span> <span data-ttu-id="618ff-108">Hivatkozásait megtalálhatja toohello kód minták: [kapcsolat szalagtárak használt tooconnect tooAzure adatbázis MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="618ff-108">You can find links toohello code samples at: [Connectivity libraries used tooconnect tooAzure Database for MySQL](concepts-connection-libraries.md)</span></span>

## <a name="tools"></a><span data-ttu-id="618ff-109">Eszközök</span><span class="sxs-lookup"><span data-stu-id="618ff-109">Tools</span></span>
<span data-ttu-id="618ff-110">Azure MySQL-adatbázis hello MySQL közösségi telepített verzióját használja, MySQL munkaterület vagy MySQL segédprogramok mysql.exe, mint például az általános kezelőeszközöket kompatibilis [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), stb.</span><span class="sxs-lookup"><span data-stu-id="618ff-110">Azure Database for MySQL uses hello MySQL community version, compatible with MySQL common management tools such as Workbench or MySQL utilities such as mysql.exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), and others.</span></span> <span data-ttu-id="618ff-111">Hello Azure-portálon, az Azure CLI és a REST API-k toointeract hello adatbázis szolgáltatással is használja.</span><span class="sxs-lookup"><span data-stu-id="618ff-111">You can also use hello Azure portal, Azure CLI, and REST APIs toointeract with hello database service.</span></span>

## <a name="resource-limitations"></a><span data-ttu-id="618ff-112">Erőforrás-korlátozások</span><span class="sxs-lookup"><span data-stu-id="618ff-112">Resource limitations</span></span>
<span data-ttu-id="618ff-113">Azure-beli MySQL adatbázis hello erőforrások elérhető tooa kiszolgálói kétféle különböző módon kezelő:</span><span class="sxs-lookup"><span data-stu-id="618ff-113">Azure MySQL Database manages hello resources available tooa server using two different mechanisms:</span></span> 
- <span data-ttu-id="618ff-114">Erőforrások irányítás</span><span class="sxs-lookup"><span data-stu-id="618ff-114">Resources Governance</span></span> 
- <span data-ttu-id="618ff-115">A korlátozások érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="618ff-115">Enforcement of Limits.</span></span>

## <a name="security"></a><span data-ttu-id="618ff-116">Biztonság</span><span class="sxs-lookup"><span data-stu-id="618ff-116">Security</span></span>
<span data-ttu-id="618ff-117">Azure-beli MySQL adatbázis korlátozó hozzáférési, a védelmet nyújtó adatokat, a konfigurálását a felhasználók és a szerepkör és a figyelési tevékenységek a MySQL-adatbázis forrásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="618ff-117">Azure MySQL Database provides resources for limiting access, protecting data, configuring users and role, and monitoring activities on a MySQL Database.</span></span>

## <a name="authentication"></a><span data-ttu-id="618ff-118">Authentication</span><span class="sxs-lookup"><span data-stu-id="618ff-118">Authentication</span></span>
<span data-ttu-id="618ff-119">Azure-beli MySQL adatbázis támogatja a kiszolgáló hitelesítése a felhasználók és bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="618ff-119">Azure MySQL Database supports server authentication of users and logins.</span></span>

## <a name="resiliency"></a><span data-ttu-id="618ff-120">Resiliency</span><span class="sxs-lookup"><span data-stu-id="618ff-120">Resiliency</span></span>
<span data-ttu-id="618ff-121">Amikor egy átmeneti hiba akkor fordul elő, tooMySQL adatbázis kapcsolódás közben, a kódot újra kell hello hívás.</span><span class="sxs-lookup"><span data-stu-id="618ff-121">When a transient error occurs while connecting tooMySQL Database, your code should retry hello call.</span></span> <span data-ttu-id="618ff-122">Hello újrapróbálkozási logika használata leállás logikát, azt javasoljuk, így azt nem ne terhelje tovább hello SQL-adatbázis az újrapróbálkozás egyszerre több ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="618ff-122">We recommend hello retry logic use back off logic, so that it does not overwhelm hello SQL Database with multiple clients retrying simultaneously.</span></span>

- <span data-ttu-id="618ff-123">Kódminták: mintakódok, mely újrapróbálkozási logika, lásd: hello nyelvű-példák: [kapcsolat szalagtárak használt tooconnect tooAzure adatbázis MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="618ff-123">Code samples: For code samples that illustrate retry logic, see samples for hello language of your choice at: [Connectivity libraries used tooconnect tooAzure Database for MySQL](concepts-connection-libraries.md)</span></span>

## <a name="managing-connections"></a><span data-ttu-id="618ff-124">Kapcsolatok kezelése</span><span class="sxs-lookup"><span data-stu-id="618ff-124">Managing Connections</span></span>
<span data-ttu-id="618ff-125">Adatbázis-kapcsolatok egy korlátozott erőforrás, ezért azt javasoljuk kapcsolat ésszerű használata a MySQL-adatbázis elérésekor tooachieve jobb teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="618ff-125">Database connections are a limited resource, so we recommend sensible use of connections when accessing your MySQL Database tooachieve better performance.</span></span>
- <span data-ttu-id="618ff-126">Hello adatbázist kapcsolatkészletezést vagy állandó kapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="618ff-126">Access hello database by using connection pooling or persistent connections.</span></span>
- <span data-ttu-id="618ff-127">Hello adatbázist élettartamát rövid kapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="618ff-127">Access hello database by using short connection life span.</span></span> 
- <span data-ttu-id="618ff-128">Használja újrapróbálkozási logika hello csatlakozási kísérlet, toocatch hibák miatt tooconcurrent kapcsolatok hello ponton az alkalmazás elérte a megengedett maximális hello.</span><span class="sxs-lookup"><span data-stu-id="618ff-128">Use retry logic in your application at hello point of hello connection attempt, toocatch failures due tooconcurrent connections have reached hello maximum allowed.</span></span> <span data-ttu-id="618ff-129">A hello újrapróbálkozási logika, rövid késleltetés beállítása és majd várjon, amíg egy véletlenszerű idő előtt hello további csatlakozási kísérletek.</span><span class="sxs-lookup"><span data-stu-id="618ff-129">In hello retry logic, set a short delay, and then wait for a random time before hello additional connection attempts.</span></span>
