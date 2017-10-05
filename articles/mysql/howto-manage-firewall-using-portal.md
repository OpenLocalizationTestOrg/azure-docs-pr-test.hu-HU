---
title: "Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályokat az Azure portál használatával |} Microsoft Docs"
description: "Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályokat az Azure portál használatával"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 33198e5a6e11df2db3a17fc96a0b3cd4b1a284e8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-the-azure-portal"></a><span data-ttu-id="e6346-103">Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályokat az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="e6346-103">Create and manage Azure Database for MySQL firewall rules using the Azure portal</span></span>
<span data-ttu-id="e6346-104">Kiszolgálószintű tűzfal-szabályok lehetővé teszik a rendszergazdák az Azure-adatbázisának eléréséhez MySQL-kiszolgáló a megadott IP-cím vagy az IP-címek.</span><span class="sxs-lookup"><span data-stu-id="e6346-104">Server-level firewall rules enable administrators to access an Azure Database for MySQL Server from a specified IP address or range of IP addresses.</span></span> 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a><span data-ttu-id="e6346-105">Kiszolgálószintű tűzfalszabály létrehozása az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="e6346-105">Create a server-level firewall rule in the Azure portal</span></span>

1. <span data-ttu-id="e6346-106">A MySQL server, a beállítások panelen elemcsoportban kattintson **kapcsolatbiztonsági** kapcsolatbiztonsági panel megnyitásához az Azure-adatbázis a MySQL.</span><span class="sxs-lookup"><span data-stu-id="e6346-106">On the MySQL server blade, under Settings heading, click **Connection Security** to open the Connection Security blade for the Azure Database for MySQL.</span></span>

   ![Azure portál – kattintson a kapcsolat biztonságát](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="e6346-108">Kattintson a **hozzáadása a saját IP** olyan szabály létrehozására a számítógép IP-címmel, mivel az Azure rendszer által érzékelt az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="e6346-108">Click **Add My IP** on the toolbar to create a rule with the IP address of your computer, as perceived by the Azure system.</span></span>

   ![Azure portál – kattintson a saját IP-cím hozzáadása](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. <span data-ttu-id="e6346-110">Az IP-cím ellenőrzése a konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="e6346-110">Verify your IP address before saving the configuration.</span></span> <span data-ttu-id="e6346-111">Bizonyos esetekben az IP-cím, az Azure portál által megfigyelt eltér az internetes és az Azure-kiszolgálók eléréséhez használt IP-címét.</span><span class="sxs-lookup"><span data-stu-id="e6346-111">In some situations, the IP address observed by Azure portal differs from the IP address used when accessing the internet and Azure servers.</span></span> <span data-ttu-id="e6346-112">Emiatt előfordulhat, hogy módosítani szeretné a kezdő IP- és a záró IP-, a várt módon szabály függvény végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="e6346-112">Therefore, you may need to change the Start IP and End IP to make the rule function as expected.</span></span>

   <span data-ttu-id="e6346-113">Egy keresőmotor vagy más online eszköz használatával ellenőrizze a saját IP-cím (például keresés "Mi az az IP-cím").</span><span class="sxs-lookup"><span data-stu-id="e6346-113">Use a search engine or other online tool to check your own IP address (for example, search "what is my IP address").</span></span>

   ![Mi az a saját IP Bing](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. <span data-ttu-id="e6346-115">Adjon hozzá további címtartományok.</span><span class="sxs-lookup"><span data-stu-id="e6346-115">Add additional address ranges.</span></span> <span data-ttu-id="e6346-116">Az Azure-adatbázishoz MySQL tűzfal szabályait megadhatja a egyetlen IP-címet, vagy címtartományokat.</span><span class="sxs-lookup"><span data-stu-id="e6346-116">In the rules for the Azure Database for MySQL firewall, you can specify a single IP address, or a range of addresses.</span></span> <span data-ttu-id="e6346-117">Ha szeretné korlátozni a szabály, amely egy egyetlen IP-címet, írja be ugyanazt a címet a mezőben a kezdő IP- és a záró IP.</span><span class="sxs-lookup"><span data-stu-id="e6346-117">If you want to limit the rule to one single IP address, type the same address in the field for Start IP and End IP.</span></span> <span data-ttu-id="e6346-118">Megnyitásáról a tűzfal lehetővé teszi a rendszergazdák és a felhasználók számára bármely adatbázis rendelkeznek érvényes hitelesítő adatokat a MySQL-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e6346-118">Opening the firewall enables administrators and users to access any database on the MySQL server to which they have valid credentials.</span></span>

   ![<span data-ttu-id="e6346-119">Azure portál – tűzfalszabályok</span><span class="sxs-lookup"><span data-stu-id="e6346-119">Azure portal - firewall rules</span></span> ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. <span data-ttu-id="e6346-120">Kattintson a **mentése** az eszköztáron menteni a kiszolgálószintű tűzfalszabály.</span><span class="sxs-lookup"><span data-stu-id="e6346-120">Click **Save** on the toolbar to save this server-level firewall rule.</span></span> <span data-ttu-id="e6346-121">Várja meg, hogy a tűzfalszabályok frissítése sikeres volt-e a jóváhagyást.</span><span class="sxs-lookup"><span data-stu-id="e6346-121">Wait for the confirmation that the update to the firewall rules was successful.</span></span>

   ![Azure portál – válassza a Mentés](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a><span data-ttu-id="e6346-123">Meglévő kiszolgálószintű tűzfalszabályok kezelése az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="e6346-123">Manage existing server-level firewall rules through the Azure portal</span></span>
<span data-ttu-id="e6346-124">Ismételje meg a tűzfal-szabályok kezelése.</span><span class="sxs-lookup"><span data-stu-id="e6346-124">Repeat the steps to manage the firewall rules.</span></span>
* <span data-ttu-id="e6346-125">Az aktuális számítógép hozzáadásához kattintson **+ saját IP-cím hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e6346-125">To add the current computer, click **+ Add My IP**.</span></span>
* <span data-ttu-id="e6346-126">További IP-cím hozzáadásához írja be a **SZABÁLYNÉV**, **KEZDŐ IP-**, és **záró IP-Címnél**.</span><span class="sxs-lookup"><span data-stu-id="e6346-126">To add additional IP addresses, type in the **RULE NAME**, **START IP**, and **END IP**.</span></span>
* <span data-ttu-id="e6346-127">Meglévő szabály módosításához kattintson a szabály valamelyik mezőjére, és adja meg a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="e6346-127">To modify an existing rule, click any of the fields in the rule and modify.</span></span>
* <span data-ttu-id="e6346-128">Meglévő szabály törléséhez kattintson a három pont [...], és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="e6346-128">To delete an existing rule, click the ellipsis […] and click **Delete**.</span></span>
* <span data-ttu-id="e6346-129">Kattintson a **Mentés** gombra a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="e6346-129">Click **Save** to save the changes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6346-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e6346-130">Next steps</span></span>
- <span data-ttu-id="e6346-131">A MySQL-kiszolgáló egy Azure-adatbázishoz szeretne csatlakozni, lásd: [adatkapcsolattárak MySQL az Azure-adatbázis](./concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="e6346-131">For help in connecting to an Azure Database for MySQL server, see [Connection libraries for Azure Database for MySQL](./concepts-connection-libraries.md)</span></span>
