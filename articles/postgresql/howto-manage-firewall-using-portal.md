---
title: "Hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok az Azure portál használatával |} Microsoft Docs"
description: "Hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok az Azure portál használatával"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 20ac1392949a6f604e68d984cb50273b61051037
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-the-azure-portal"></a><span data-ttu-id="fc309-103">Hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="fc309-103">Create and manage Azure Database for PostgreSQL firewall rules using the Azure portal</span></span>
<span data-ttu-id="fc309-104">Kiszolgálószintű tűzfal-szabályok lehetővé teszik a rendszergazdák az Azure-adatbázisának eléréséhez PostgreSQL-kiszolgáló a megadott IP-cím vagy az IP-címek.</span><span class="sxs-lookup"><span data-stu-id="fc309-104">Server-level firewall rules enable administrators to access an Azure Database for PostgreSQL Server from a specified IP address or range of IP addresses.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="fc309-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fc309-105">Prerequisites</span></span>
<span data-ttu-id="fc309-106">Ez az útmutató Útmutató lépéseit, az alábbiak szükségesek:</span><span class="sxs-lookup"><span data-stu-id="fc309-106">To step through this how-to guide, you need:</span></span>
- <span data-ttu-id="fc309-107">A kiszolgáló [PostgreSQL az Azure-adatbázis létrehozása](quickstart-create-server-database-portal.md)</span><span class="sxs-lookup"><span data-stu-id="fc309-107">A server [Create an Azure Database for PostgreSQL](quickstart-create-server-database-portal.md)</span></span>

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a><span data-ttu-id="fc309-108">Kiszolgálószintű tűzfalszabály létrehozása az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="fc309-108">Create a server-level firewall rule in the Azure portal</span></span>
1. <span data-ttu-id="fc309-109">A PostgreSQL server, a beállítások panelen elemcsoportban kattintson **kapcsolatbiztonsági** megnyitandó a kapcsolat biztonsági panel az Azure-adatbázis PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="fc309-109">On the PostgreSQL server blade, under Settings heading, click **Connection Security** to open the Connection Security blade for the Azure Database for PostgreSQL.</span></span>

  ![Azure portál – kattintson a kapcsolat biztonságát](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="fc309-111">Kattintson a **hozzáadása a saját IP** az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="fc309-111">Click **Add My IP** on the toolbar.</span></span> <span data-ttu-id="fc309-112">Ekkor létrejön egy szabály automatikusan a számítógép IP-címmel, az Azure rendszer által érzékelt.</span><span class="sxs-lookup"><span data-stu-id="fc309-112">This will create a rule automatically with the IP address of your computer, as perceived by the Azure system.</span></span>

  ![Azure portál – kattintson a saját IP-cím hozzáadása](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. <span data-ttu-id="fc309-114">Az IP-cím ellenőrzése a konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="fc309-114">Verify your IP address before saving the configuration.</span></span> <span data-ttu-id="fc309-115">Bizonyos esetekben az IP-cím, az Azure portál által megfigyelt eltér az internetes és az Azure-kiszolgálók eléréséhez használt IP-címét.</span><span class="sxs-lookup"><span data-stu-id="fc309-115">In some situations, the IP address observed by Azure portal differs from the IP address used when accessing the internet and Azure servers.</span></span> <span data-ttu-id="fc309-116">Emiatt előfordulhat, hogy módosítani szeretné a kezdő IP- és a záró IP-, a várt módon szabály függvény végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="fc309-116">Therefore, you may need to change the Start IP and End IP to make the rule function as expected.</span></span>
<span data-ttu-id="fc309-117">Egy keresőmotor vagy más online eszköz használatával ellenőrizze a saját IP-cím (például a Bing keresési "Mi az a saját IP").</span><span class="sxs-lookup"><span data-stu-id="fc309-117">Use a search engine or other online tool to check your own IP address (for example, Bing search "what is my IP").</span></span>

  ![Mi az a saját IP Bing keresése](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. <span data-ttu-id="fc309-119">Adjon hozzá további címtartományok.</span><span class="sxs-lookup"><span data-stu-id="fc309-119">Add additional address ranges.</span></span> <span data-ttu-id="fc309-120">Az Azure-adatbázishoz PostgreSQL tűzfal szabályait megadhatja a egyetlen IP-címet, vagy címtartományokat.</span><span class="sxs-lookup"><span data-stu-id="fc309-120">In the rules for the Azure Database for PostgreSQL firewall, you can specify a single IP address, or a range of addresses.</span></span> <span data-ttu-id="fc309-121">Ha szeretné korlátozni a szabály, amely egy egyetlen IP-címet, írja be ugyanazt a címet a mezőben a kezdő IP- és a záró IP.</span><span class="sxs-lookup"><span data-stu-id="fc309-121">If you want to limit the rule to one single IP address, type the same address in the field for Start IP and End IP.</span></span> <span data-ttu-id="fc309-122">Megnyitásáról a tűzfal lehetővé teszi a rendszergazdák és felhasználók bejelentkezési bármely adatbázisra, amelyhez PostgreSQL-kiszolgálón érvényes hitelesítő adatokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fc309-122">Opening the firewall enables administrators and users to login to any database on the PostgreSQL server to which they have valid credentials.</span></span>

  ![<span data-ttu-id="fc309-123">Azure portál – tűzfalszabályok</span><span class="sxs-lookup"><span data-stu-id="fc309-123">Azure portal - firewall rules</span></span> ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. <span data-ttu-id="fc309-124">Kattintson a **mentése** az eszköztáron menteni a kiszolgálószintű tűzfalszabály.</span><span class="sxs-lookup"><span data-stu-id="fc309-124">Click **Save** on the toolbar to save this server-level firewall rule.</span></span> <span data-ttu-id="fc309-125">Várja meg, hogy a tűzfalszabályok frissítése sikeres volt-e a jóváhagyást.</span><span class="sxs-lookup"><span data-stu-id="fc309-125">Wait for the confirmation that the update to the firewall rules was successful.</span></span>

  ![Azure portál – válassza a Mentés](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)


## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a><span data-ttu-id="fc309-127">Meglévő kiszolgálószintű tűzfalszabályok kezelése az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="fc309-127">Manage existing server-level firewall rules through the Azure portal</span></span>
<span data-ttu-id="fc309-128">Ismételje meg a tűzfal-szabályok kezelése.</span><span class="sxs-lookup"><span data-stu-id="fc309-128">Repeat the steps to manage the firewall rules.</span></span>
* <span data-ttu-id="fc309-129">Az aktuális számítógép hozzáadásához kattintson a gombra kattintva + **hozzáadása a saját IP**.</span><span class="sxs-lookup"><span data-stu-id="fc309-129">To add the current computer, click the button to + **Add My IP**.</span></span> <span data-ttu-id="fc309-130">Kattintson a **Mentés** gombra a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="fc309-130">Click **Save** to save the changes.</span></span>
* <span data-ttu-id="fc309-131">További IP-címek hozzáadásához adja meg a Szabály neve, a Kezdő IP-cím és a Záró IP-cím értékét.</span><span class="sxs-lookup"><span data-stu-id="fc309-131">To add additional IP addresses, type in the Rule Name, Start IP Address, and End IP Address.</span></span> <span data-ttu-id="fc309-132">Kattintson a **Mentés** gombra a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="fc309-132">Click **Save** to save the changes.</span></span>
* <span data-ttu-id="fc309-133">Meglévő szabály módosításához kattintson a szabály valamelyik mezőjére, és adja meg a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="fc309-133">To modify an existing rule, click any of the fields in the rule and modify.</span></span> <span data-ttu-id="fc309-134">Kattintson a **Mentés** gombra a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="fc309-134">Click **Save** to save the changes.</span></span>
* <span data-ttu-id="fc309-135">Meglévő szabály törléséhez kattintson a három pont [...], majd kattintson a törlés távolítsa el a szabályt.</span><span class="sxs-lookup"><span data-stu-id="fc309-135">To delete an existing rule, click the ellipsis […] and click Delete remove the rule.</span></span> <span data-ttu-id="fc309-136">Kattintson a **Mentés** gombra a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="fc309-136">Click **Save** to save the changes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fc309-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fc309-137">Next steps</span></span>
- <span data-ttu-id="fc309-138">Ehhez hasonlóan is, a parancsfájl [hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok Azure parancssori felület használatával](howto-manage-firewall-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="fc309-138">Similarly, you can script to [Create and manage Azure Database for PostgreSQL firewall rules using Azure CLI](howto-manage-firewall-using-cli.md)</span></span>
- <span data-ttu-id="fc309-139">Egy PostgreSQL-kiszolgáló Azure-adatbázishoz szeretne csatlakozni a témakörben talál segítséget [adatkapcsolattárak PostgreSQL Azure-adatbázis](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="fc309-139">For help in connecting to an Azure Database for PostgreSQL server, see [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
