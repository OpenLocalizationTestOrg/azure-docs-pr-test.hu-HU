---
title: "aaaCreate és kezelheti az Azure-adatbázis MySQL tűzfalszabályok hello Azure-portál használatával |} Microsoft Docs"
description: "Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályok hello Azure-portál használatával"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 30dbdde4299df564fbf6581419e908186fe3b823
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-hello-azure-portal"></a><span data-ttu-id="e214e-103">Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályok hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="e214e-103">Create and manage Azure Database for MySQL firewall rules using hello Azure portal</span></span>
<span data-ttu-id="e214e-104">Kiszolgálószintű tűzfal-szabályokat engedélyezhetik a rendszergazdák tooaccess egy Azure-adatbázis MySQL-kiszolgáló a megadott IP-cím vagy az IP-címek.</span><span class="sxs-lookup"><span data-stu-id="e214e-104">Server-level firewall rules enable administrators tooaccess an Azure Database for MySQL Server from a specified IP address or range of IP addresses.</span></span> 

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a><span data-ttu-id="e214e-105">Hozzon létre egy kiszolgálószintű tűzfalszabályt hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e214e-105">Create a server-level firewall rule in hello Azure portal</span></span>

1. <span data-ttu-id="e214e-106">Hello MySQL server panelen, a beállítások elemcsoportban kattintson **kapcsolatbiztonsági** tooopen hello kapcsolatbiztonsági panel hello MySQL az Azure-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e214e-106">On hello MySQL server blade, under Settings heading, click **Connection Security** tooopen hello Connection Security blade for hello Azure Database for MySQL.</span></span>

   ![Azure portál – kattintson a kapcsolat biztonságát](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="e214e-108">Kattintson a **hozzáadása a saját IP** a hello eszköztár toocreate egy szabály a számítógép, mivel hello Azure rendszer által érzékelt hello IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="e214e-108">Click **Add My IP** on hello toolbar toocreate a rule with hello IP address of your computer, as perceived by hello Azure system.</span></span>

   ![Azure portál – kattintson a saját IP-cím hozzáadása](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. <span data-ttu-id="e214e-110">Ellenőrizze az IP-címek hello konfiguráció mentése előtt.</span><span class="sxs-lookup"><span data-stu-id="e214e-110">Verify your IP address before saving hello configuration.</span></span> <span data-ttu-id="e214e-111">Bizonyos esetekben az Azure portál által megfigyelt hello IP-címe eltér használt hello IP-cím mikor fér hozzá hello a internetes és az Azure-kiszolgálók esetén.</span><span class="sxs-lookup"><span data-stu-id="e214e-111">In some situations, hello IP address observed by Azure portal differs from hello IP address used when accessing hello internet and Azure servers.</span></span> <span data-ttu-id="e214e-112">Ezért szükség lehet toochange hello kezdő IP- és a záró IP-toomake hello szabály függvény várt módon.</span><span class="sxs-lookup"><span data-stu-id="e214e-112">Therefore, you may need toochange hello Start IP and End IP toomake hello rule function as expected.</span></span>

   <span data-ttu-id="e214e-113">Használjon egy keresőmotor vagy más online eszköz toocheck a saját IP-címet (például keresés "Mi az az IP-cím").</span><span class="sxs-lookup"><span data-stu-id="e214e-113">Use a search engine or other online tool toocheck your own IP address (for example, search "what is my IP address").</span></span>

   ![Mi az a saját IP Bing](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. <span data-ttu-id="e214e-115">Adjon hozzá további címtartományok.</span><span class="sxs-lookup"><span data-stu-id="e214e-115">Add additional address ranges.</span></span> <span data-ttu-id="e214e-116">Hello Azure adatbázis MySQL tűzfal hello szabályait megadhatja a egyetlen IP-címet, vagy címtartományokat.</span><span class="sxs-lookup"><span data-stu-id="e214e-116">In hello rules for hello Azure Database for MySQL firewall, you can specify a single IP address, or a range of addresses.</span></span> <span data-ttu-id="e214e-117">Ha azt szeretné, hogy toolimit hello szabály tooone egyetlen IP-címet, ugyanaz az kezdő IP- és a záró IP-cím hello mező típusa hello.</span><span class="sxs-lookup"><span data-stu-id="e214e-117">If you want toolimit hello rule tooone single IP address, type hello same address in hello field for Start IP and End IP.</span></span> <span data-ttu-id="e214e-118">Hello tűzfal megnyitása lehetővé teszi a rendszergazdák és felhasználók tooaccess bármely adatbázis hello MySQL server toowhich rendelkeznek érvényes hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="e214e-118">Opening hello firewall enables administrators and users tooaccess any database on hello MySQL server toowhich they have valid credentials.</span></span>

   ![<span data-ttu-id="e214e-119">Azure portál – tűzfalszabályok</span><span class="sxs-lookup"><span data-stu-id="e214e-119">Azure portal - firewall rules</span></span> ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. <span data-ttu-id="e214e-120">Kattintson a **mentése** a hello eszköztár toosave a kiszolgálószintű tűzfalszabály.</span><span class="sxs-lookup"><span data-stu-id="e214e-120">Click **Save** on hello toolbar toosave this server-level firewall rule.</span></span> <span data-ttu-id="e214e-121">Várja meg, hogy sikeres volt-e hello frissítés toohello tűzfalszabályok hello megerősítése.</span><span class="sxs-lookup"><span data-stu-id="e214e-121">Wait for hello confirmation that hello update toohello firewall rules was successful.</span></span>

   ![Azure portál – válassza a Mentés](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-through-hello-azure-portal"></a><span data-ttu-id="e214e-123">Hello Azure-portálon keresztül meglévő kiszolgálószintű tűzfal-szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="e214e-123">Manage existing server-level firewall rules through hello Azure portal</span></span>
<span data-ttu-id="e214e-124">Ismételje meg a hello lépéseket toomanage hello tűzfalszabályokat.</span><span class="sxs-lookup"><span data-stu-id="e214e-124">Repeat hello steps toomanage hello firewall rules.</span></span>
* <span data-ttu-id="e214e-125">tooadd hello aktuális számítógépet, kattintson a **+ saját IP-cím hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e214e-125">tooadd hello current computer, click **+ Add My IP**.</span></span>
* <span data-ttu-id="e214e-126">tooadd további IP-címeket, írja be a hello **SZABÁLYNÉV**, **KEZDŐ IP-**, és **záró IP-Címnél**.</span><span class="sxs-lookup"><span data-stu-id="e214e-126">tooadd additional IP addresses, type in hello **RULE NAME**, **START IP**, and **END IP**.</span></span>
* <span data-ttu-id="e214e-127">egy meglévő szabály toomodify kattintson valamelyik hello mezők hello szabályban, és módosítsa.</span><span class="sxs-lookup"><span data-stu-id="e214e-127">toomodify an existing rule, click any of hello fields in hello rule and modify.</span></span>
* <span data-ttu-id="e214e-128">egy meglévő toodelete szabály, hello három pont [...] kattintson, majd **törlése**.</span><span class="sxs-lookup"><span data-stu-id="e214e-128">toodelete an existing rule, click hello ellipsis […] and click **Delete**.</span></span>
* <span data-ttu-id="e214e-129">Kattintson a **mentése** toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="e214e-129">Click **Save** toosave hello changes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e214e-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e214e-130">Next steps</span></span>
- <span data-ttu-id="e214e-131">A csatlakozás az Azure-adatbázis tooan MySQL-kiszolgáló, lásd: [adatkapcsolattárak MySQL az Azure-adatbázis](./concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="e214e-131">For help in connecting tooan Azure Database for MySQL server, see [Connection libraries for Azure Database for MySQL](./concepts-connection-libraries.md)</span></span>
