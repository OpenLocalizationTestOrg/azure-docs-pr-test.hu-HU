---
title: "Hogyan tooRestore egy kiszolgálóhoz az Azure-adatbázis a PostgreSQL |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toorestore PostgreSQL az Azure-adatbázis egy kiszolgáló hello Azure-portálon."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 07/20/2017
ms.openlocfilehash: bc7351f384607397806d837afd3e1d7a26575e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobackup-and-restore-a-server-in-azure-database-for-postgresql-using-hello-azure-portal"></a><span data-ttu-id="42f1a-103">Hogyan tooBackup és helyreállítását PostgreSQL az Azure-adatbázis egy kiszolgáló hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="42f1a-103">How tooBackup and Restore a server in Azure Database for PostgreSQL using hello Azure portal</span></span>

## <a name="backup-happens-automatically"></a><span data-ttu-id="42f1a-104">Automatikusan megtörténik a biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="42f1a-104">Backup happens Automatically</span></span>
<span data-ttu-id="42f1a-105">Ha Azure-adatbázis a PostgreSQL, hello adatbázis-szolgáltatás automatikusan teszi hello szolgáltatás biztonsági másolatot, 5 percenként.</span><span class="sxs-lookup"><span data-stu-id="42f1a-105">When using Azure Database for PostgreSQL, hello database service automatically makes a backup of hello service every 5 minutes.</span></span> 

<span data-ttu-id="42f1a-106">hello biztonsági mentések esetén érhetők el a 7 napja alapszintű rétegben, és 35 napon Standard csomag használata esetén.</span><span class="sxs-lookup"><span data-stu-id="42f1a-106">hello backups are available for 7 days when using Basic Tier, and 35 days when using Standard Tier.</span></span> <span data-ttu-id="42f1a-107">További információkért lásd: [PostgreSQL szolgáltatási szinteket az Azure-adatbázis](concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="42f1a-107">For more information, see [Azure Database for PostgreSQL service tiers](concepts-service-tiers.md)</span></span>

<span data-ttu-id="42f1a-108">Az automatikus biztonsági mentési szolgáltatás használatával állítsa vissza hello kiszolgáló és az adatbázisok egy új kiszolgáló tooan korábbi időpontban be.</span><span class="sxs-lookup"><span data-stu-id="42f1a-108">Using this automatic backup feature you may restore hello server and all its databases into a new server tooan earlier point-in-time.</span></span>

## <a name="restore-in-hello-azure-portal"></a><span data-ttu-id="42f1a-109">Állítsa vissza a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="42f1a-109">Restore in hello Azure portal</span></span>
<span data-ttu-id="42f1a-110">Azure PostgreSQL-adatbázishoz lehetővé teszi toorestore hello server hátsó tooa pont idő és tooa hello server új példányát.</span><span class="sxs-lookup"><span data-stu-id="42f1a-110">Azure Database for PostgreSQL allows you toorestore hello server back tooa point in time and into tooa new copy of hello server.</span></span> <span data-ttu-id="42f1a-111">Használhatja az új kiszolgáló toorecover adatait.</span><span class="sxs-lookup"><span data-stu-id="42f1a-111">You can use this new server toorecover your data.</span></span> 

<span data-ttu-id="42f1a-112">Például ha egy táblázat véletlenül dobva délben ma, akkor sikerült toohello idő előtt déltől visszaállítása, hiányzik a táblázat és adatainak áttelepítését hello kiszolgáló új példányt hello beolvasása.</span><span class="sxs-lookup"><span data-stu-id="42f1a-112">For example, if a table was accidentally dropped at noon today, you could restore toohello time just before noon and retrieve hello missing table and data from that new copy of hello server.</span></span>

<span data-ttu-id="42f1a-113">hello lépések visszaállítási hello minta kiszolgáló tooa pont időpont:</span><span class="sxs-lookup"><span data-stu-id="42f1a-113">hello following steps restore hello sample server tooa point in time:</span></span>
1. <span data-ttu-id="42f1a-114">Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="42f1a-114">Sign into hello [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="42f1a-115">Keresse meg az Azure-adatbázis PostgreSQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="42f1a-115">Locate your Azure Database for PostgreSQL server.</span></span> <span data-ttu-id="42f1a-116">Hello Azure-portálon, kattintson **összes erőforrás** hello bal oldali menüből és hello nevét, írja be például a **mypgserver-20170401**, a meglévő kiszolgáló toosearch.</span><span class="sxs-lookup"><span data-stu-id="42f1a-116">In hello Azure portal, click **All Resources** from hello left-hand menu and type in hello name, such as **mypgserver-20170401**, toosearch for your existing server.</span></span> <span data-ttu-id="42f1a-117">Kattintson a felsorolt hello keresési eredmény hello kiszolgálónevet.</span><span class="sxs-lookup"><span data-stu-id="42f1a-117">Click hello server name listed in hello search result.</span></span> <span data-ttu-id="42f1a-118">Hello **áttekintése** lapon, a kiszolgáló megnyílik, és további konfigurációs lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="42f1a-118">hello **Overview** page for your server opens and provides options for further configuration.</span></span>

   ![Azure portál – keresés toolocate a kiszolgálón](media/postgresql-howto-restore-server-portal/1-locate.png)

3. <span data-ttu-id="42f1a-120">A hello hello server – áttekintés panel felső részén kattintson **visszaállítása** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="42f1a-120">On hello top of hello server overview blade, click **Restore** on hello toolbar.</span></span> <span data-ttu-id="42f1a-121">hello visszaállítás panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="42f1a-121">hello Restore blade opens.</span></span>

   ![Azure-adatbázis PostgreSQL - áttekintése - Visszaállítás gomb](./media/postgresql-howto-restore-server-portal/2_server.png)

4. <span data-ttu-id="42f1a-123">Töltse ki hello visszaállítási űrlapot hello szükséges információkkal:</span><span class="sxs-lookup"><span data-stu-id="42f1a-123">Fill out hello Restore form with hello required information:</span></span>

   ![<span data-ttu-id="42f1a-124">PostgreSQL - visszaállítási információk az Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="42f1a-124">Azure Database for PostgreSQL - Restore information</span></span> ](./media/postgresql-howto-restore-server-portal/3_restore.png)
  - <span data-ttu-id="42f1a-125">**Visszaállítási pont**: egy pont időponthoz kötött, amely hello server módosítása előtt válasszon</span><span class="sxs-lookup"><span data-stu-id="42f1a-125">**Restore point**: Select a point-in-time that occurs before hello server was changed</span></span>
  - <span data-ttu-id="42f1a-126">**Célkiszolgáló**: Adjon meg egy új kiszolgálónevet azt szeretné, hogy toorestore</span><span class="sxs-lookup"><span data-stu-id="42f1a-126">**Target server**: Provide a new server name you want toorestore to</span></span>
  - <span data-ttu-id="42f1a-127">**Hely**: hello régió nem választhat, alapértelmezés szerint ugyanaz, mint a forráskiszolgálón hello</span><span class="sxs-lookup"><span data-stu-id="42f1a-127">**Location**: You cannot select hello region, by default it is same as hello source server</span></span>
  - <span data-ttu-id="42f1a-128">**A tarifacsomag**: Ez az érték nem módosítható, ha egy kiszolgáló visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="42f1a-128">**Pricing tier**: You cannot change this value when restoring a server.</span></span> <span data-ttu-id="42f1a-129">Ugyanaz, mint a forráskiszolgálón hello.</span><span class="sxs-lookup"><span data-stu-id="42f1a-129">It is same as hello source server.</span></span> 

5. <span data-ttu-id="42f1a-130">Kattintson a **OK** toorestore hello server toorestore tooa pont időben.</span><span class="sxs-lookup"><span data-stu-id="42f1a-130">Click **OK** toorestore hello server toorestore tooa point in time.</span></span> 

6. <span data-ttu-id="42f1a-131">Miután hello visszaállítás befejezését, keresse meg a hello új kiszolgálóra, amely adatok helyreállt a várt módon tooverify hello jön létre.</span><span class="sxs-lookup"><span data-stu-id="42f1a-131">Once hello restore finishes, locate hello new server that is created tooverify hello data was restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42f1a-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42f1a-132">Next steps</span></span>
- [<span data-ttu-id="42f1a-133">Adatkapcsolattárak PostgreSQL Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="42f1a-133">Connection libraries for Azure Database for PostgreSQL</span></span>](concepts-connection-libraries.md)
