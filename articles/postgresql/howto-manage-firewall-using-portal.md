---
title: "aaaCreate és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok hello Azure-portál használatával |} Microsoft Docs"
description: "Hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok hello Azure-portál használatával"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 6a41a077168657769e442401e9df9931aa809240
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-hello-azure-portal"></a><span data-ttu-id="b720f-103">Hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="b720f-103">Create and manage Azure Database for PostgreSQL firewall rules using hello Azure portal</span></span>
<span data-ttu-id="b720f-104">Kiszolgálószintű tűzfal-szabályokat engedélyezhetik a rendszergazdák tooaccess egy Azure-adatbázis PostgreSQL-kiszolgáló a megadott IP-cím vagy az IP-címek.</span><span class="sxs-lookup"><span data-stu-id="b720f-104">Server-level firewall rules enable administrators tooaccess an Azure Database for PostgreSQL Server from a specified IP address or range of IP addresses.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b720f-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b720f-105">Prerequisites</span></span>
<span data-ttu-id="b720f-106">Ez hogyan tooguide keresztül toostep, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="b720f-106">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="b720f-107">A kiszolgáló [PostgreSQL az Azure-adatbázis létrehozása](quickstart-create-server-database-portal.md)</span><span class="sxs-lookup"><span data-stu-id="b720f-107">A server [Create an Azure Database for PostgreSQL](quickstart-create-server-database-portal.md)</span></span>

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a><span data-ttu-id="b720f-108">Hozzon létre egy kiszolgálószintű tűzfalszabályt hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b720f-108">Create a server-level firewall rule in hello Azure portal</span></span>
1. <span data-ttu-id="b720f-109">Hello PostgreSQL server panelen, a beállítások elemcsoportban kattintson **kapcsolatbiztonsági** tooopen hello kapcsolatbiztonsági panel hello PostgreSQL az Azure-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b720f-109">On hello PostgreSQL server blade, under Settings heading, click **Connection Security** tooopen hello Connection Security blade for hello Azure Database for PostgreSQL.</span></span>

  ![Azure portál – kattintson a kapcsolat biztonságát](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="b720f-111">Kattintson a **hozzáadása a saját IP** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="b720f-111">Click **Add My IP** on hello toolbar.</span></span> <span data-ttu-id="b720f-112">Ekkor létrejön egy szabály automatikusan a számítógép, mint hello Azure rendszer által észlelt hello IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="b720f-112">This will create a rule automatically with hello IP address of your computer, as perceived by hello Azure system.</span></span>

  ![Azure portál – kattintson a saját IP-cím hozzáadása](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. <span data-ttu-id="b720f-114">Ellenőrizze az IP-címek hello konfiguráció mentése előtt.</span><span class="sxs-lookup"><span data-stu-id="b720f-114">Verify your IP address before saving hello configuration.</span></span> <span data-ttu-id="b720f-115">Bizonyos esetekben az Azure portál által megfigyelt hello IP-címe eltér használt hello IP-cím mikor fér hozzá hello a internetes és az Azure-kiszolgálók esetén.</span><span class="sxs-lookup"><span data-stu-id="b720f-115">In some situations, hello IP address observed by Azure portal differs from hello IP address used when accessing hello internet and Azure servers.</span></span> <span data-ttu-id="b720f-116">Ezért szükség lehet toochange hello kezdő IP- és a záró IP-toomake hello szabály függvény várt módon.</span><span class="sxs-lookup"><span data-stu-id="b720f-116">Therefore, you may need toochange hello Start IP and End IP toomake hello rule function as expected.</span></span>
<span data-ttu-id="b720f-117">Egy keresőmotor vagy más online eszköz toocheck a saját IP-cím (például a Bing keresési "Mi az a saját IP") használja.</span><span class="sxs-lookup"><span data-stu-id="b720f-117">Use a search engine or other online tool toocheck your own IP address (for example, Bing search "what is my IP").</span></span>

  ![Mi az a saját IP Bing keresése](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. <span data-ttu-id="b720f-119">Adjon hozzá további címtartományok.</span><span class="sxs-lookup"><span data-stu-id="b720f-119">Add additional address ranges.</span></span> <span data-ttu-id="b720f-120">Hello Azure adatbázis PostgreSQL tűzfal hello szabályait megadhatja a egyetlen IP-címet, vagy címtartományokat.</span><span class="sxs-lookup"><span data-stu-id="b720f-120">In hello rules for hello Azure Database for PostgreSQL firewall, you can specify a single IP address, or a range of addresses.</span></span> <span data-ttu-id="b720f-121">Ha azt szeretné, hogy toolimit hello szabály tooone egyetlen IP-címet, ugyanaz az kezdő IP- és a záró IP-cím hello mező típusa hello.</span><span class="sxs-lookup"><span data-stu-id="b720f-121">If you want toolimit hello rule tooone single IP address, type hello same address in hello field for Start IP and End IP.</span></span> <span data-ttu-id="b720f-122">Hello tűzfal megnyitása lehetővé teszi, hogy a rendszergazdák és felhasználók toologin tooany adatbázis hello PostgreSQL server toowhich rendelkeznek érvényes hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="b720f-122">Opening hello firewall enables administrators and users toologin tooany database on hello PostgreSQL server toowhich they have valid credentials.</span></span>

  ![<span data-ttu-id="b720f-123">Azure portál – tűzfalszabályok</span><span class="sxs-lookup"><span data-stu-id="b720f-123">Azure portal - firewall rules</span></span> ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. <span data-ttu-id="b720f-124">Kattintson a **mentése** a hello eszköztár toosave a kiszolgálószintű tűzfalszabály.</span><span class="sxs-lookup"><span data-stu-id="b720f-124">Click **Save** on hello toolbar toosave this server-level firewall rule.</span></span> <span data-ttu-id="b720f-125">Várja meg, hogy sikeres volt-e hello frissítés toohello tűzfalszabályok hello megerősítése.</span><span class="sxs-lookup"><span data-stu-id="b720f-125">Wait for hello confirmation that hello update toohello firewall rules was successful.</span></span>

  ![Azure portál – válassza a Mentés](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)


## <a name="manage-existing-server-level-firewall-rules-through-hello-azure-portal"></a><span data-ttu-id="b720f-127">Hello Azure-portálon keresztül meglévő kiszolgálószintű tűzfal-szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="b720f-127">Manage existing server-level firewall rules through hello Azure portal</span></span>
<span data-ttu-id="b720f-128">Ismételje meg a hello lépéseket toomanage hello tűzfalszabályokat.</span><span class="sxs-lookup"><span data-stu-id="b720f-128">Repeat hello steps toomanage hello firewall rules.</span></span>
* <span data-ttu-id="b720f-129">tooadd hello aktuális számítógépet, gombra hello túl + **hozzáadása a saját IP**.</span><span class="sxs-lookup"><span data-stu-id="b720f-129">tooadd hello current computer, click hello button too+ **Add My IP**.</span></span> <span data-ttu-id="b720f-130">Kattintson a **mentése** toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b720f-130">Click **Save** toosave hello changes.</span></span>
* <span data-ttu-id="b720f-131">a hello tooadd további IP-címeket, írja be a szabály nevét, a kezdő IP-címe és a záró IP-cím.</span><span class="sxs-lookup"><span data-stu-id="b720f-131">tooadd additional IP addresses, type in hello Rule Name, Start IP Address, and End IP Address.</span></span> <span data-ttu-id="b720f-132">Kattintson a **mentése** toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b720f-132">Click **Save** toosave hello changes.</span></span>
* <span data-ttu-id="b720f-133">egy meglévő szabály toomodify kattintson valamelyik hello mezők hello szabályban, és módosítsa.</span><span class="sxs-lookup"><span data-stu-id="b720f-133">toomodify an existing rule, click any of hello fields in hello rule and modify.</span></span> <span data-ttu-id="b720f-134">Kattintson a **mentése** toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b720f-134">Click **Save** toosave hello changes.</span></span>
* <span data-ttu-id="b720f-135">toodelete egy meglévő szabály hello három pont [...] kattintson, majd távolítsa el a hello szabály törlése.</span><span class="sxs-lookup"><span data-stu-id="b720f-135">toodelete an existing rule, click hello ellipsis […] and click Delete remove hello rule.</span></span> <span data-ttu-id="b720f-136">Kattintson a **mentése** toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b720f-136">Click **Save** toosave hello changes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b720f-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b720f-137">Next steps</span></span>
- <span data-ttu-id="b720f-138">Hasonlóképpen, akkor lehet parancsprogramot futtatni a túl[hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok Azure parancssori felület használatával](howto-manage-firewall-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="b720f-138">Similarly, you can script too[Create and manage Azure Database for PostgreSQL firewall rules using Azure CLI](howto-manage-firewall-using-cli.md)</span></span>
- <span data-ttu-id="b720f-139">Az Azure-adatbázis tooan PostgreSQL-kiszolgálóhoz csatlakozó útmutatásért lásd: [adatkapcsolattárak PostgreSQL Azure-adatbázis](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="b720f-139">For help in connecting tooan Azure Database for PostgreSQL server, see [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
