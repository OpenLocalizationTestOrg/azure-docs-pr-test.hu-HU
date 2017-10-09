---
title: "a kiszolgáló MySQL az Azure-adatbázis aaaHow tooRestore |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toorestore egy kiszolgáló, a Azure-adatbázisban a MySQL használatára vonatkozó hello Azure-portálon."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4b990d5b37c5d4924de9571192b923e3c81094ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobackup-and-restore-a-server-in-azure-database-for-mysql-using-hello-azure-portal"></a><span data-ttu-id="151f1-103">Hogyan tooBackup és a kiszolgáló, a Azure-adatbázisban a MySQL használatára vonatkozó visszaállítási hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="151f1-103">How tooBackup and Restore a server in Azure Database for MySQL using hello Azure portal</span></span>

## <a name="backup-happens-automatically"></a><span data-ttu-id="151f1-104">Automatikusan megtörténik a biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="151f1-104">Backup happens Automatically</span></span>
<span data-ttu-id="151f1-105">Amikor MySQL az Azure-adatbázist használja, hello adatbázis-szolgáltatás automatikusan teszi hello szolgáltatás biztonsági másolatot, 5 percenként.</span><span class="sxs-lookup"><span data-stu-id="151f1-105">When using Azure Database for MySQL, hello database service automatically makes a backup of hello service every 5 minutes.</span></span> 

<span data-ttu-id="151f1-106">hello biztonsági mentések esetén érhetők el a 7 napja alapszintű rétegben, és 35 napon Standard csomag használata esetén.</span><span class="sxs-lookup"><span data-stu-id="151f1-106">hello backups are available for 7 days when using Basic Tier, and 35 days when using Standard Tier.</span></span> <span data-ttu-id="151f1-107">További információkért lásd: [MySQL szolgáltatási szinteket az Azure-adatbázis](concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="151f1-107">For more information, see [Azure Database for MySQL service tiers](concepts-service-tiers.md)</span></span>

<span data-ttu-id="151f1-108">Az automatikus biztonsági mentési szolgáltatás használatával állítsa vissza hello kiszolgáló és az adatbázisok egy új kiszolgáló tooan korábbi időpontban be.</span><span class="sxs-lookup"><span data-stu-id="151f1-108">Using this automatic backup feature you may restore hello server and all its databases into a new server tooan earlier point-in-time.</span></span>

## <a name="restore-in-hello-azure-portal"></a><span data-ttu-id="151f1-109">Állítsa vissza a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="151f1-109">Restore in hello Azure portal</span></span>
<span data-ttu-id="151f1-110">Azure MySQL-adatbázis lehetővé teszi toorestore hello server hátsó tooa pont idő és tooa hello server új példányát.</span><span class="sxs-lookup"><span data-stu-id="151f1-110">Azure Database for MySQL allows you toorestore hello server back tooa point in time and into tooa new copy of hello server.</span></span> <span data-ttu-id="151f1-111">Használhatja az új kiszolgáló toorecover adatait.</span><span class="sxs-lookup"><span data-stu-id="151f1-111">You can use this new server toorecover your data.</span></span> 

<span data-ttu-id="151f1-112">Például ha egy táblázat véletlenül dobva délben ma, akkor sikerült toohello idő előtt déltől visszaállítása, hiányzik a táblázat és adatainak áttelepítését hello kiszolgáló új példányt hello beolvasása.</span><span class="sxs-lookup"><span data-stu-id="151f1-112">For example, if a table was accidentally dropped at noon today, you could restore toohello time just before noon and retrieve hello missing table and data from that new copy of hello server.</span></span>

<span data-ttu-id="151f1-113">hello lépések visszaállítási hello minta kiszolgáló tooa pont időpont:</span><span class="sxs-lookup"><span data-stu-id="151f1-113">hello following steps restore hello sample server tooa point in time:</span></span>

1. <span data-ttu-id="151f1-114">Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="151f1-114">Sign into hello [Azure portal](https://portal.azure.com/)</span></span>

2. <span data-ttu-id="151f1-115">Keresse meg a MySQL-kiszolgálóhoz tartozó Azure-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="151f1-115">Locate your Azure Database for MySQL server.</span></span> <span data-ttu-id="151f1-116">Hello bal oldali ablaktáblában jelöljön ki **összes erőforrás**, majd válassza ki a kiszolgálót hello listából.</span><span class="sxs-lookup"><span data-stu-id="151f1-116">In hello left pane, select **All resources**, then select your server from hello list.</span></span>

3.  <span data-ttu-id="151f1-117">A hello hello server – áttekintés panel felső részén kattintson **visszaállítása** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="151f1-117">On hello top of hello server overview blade, click **Restore** on hello toolbar.</span></span> <span data-ttu-id="151f1-118">hello visszaállítás panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="151f1-118">hello Restore blade opens.</span></span>
<span data-ttu-id="151f1-119">![Visszaállítás gombra.](./media/howto-restore-server-portal/click-restore-button.png)</span><span class="sxs-lookup"><span data-stu-id="151f1-119">![click restore button](./media/howto-restore-server-portal/click-restore-button.png)</span></span>

4. <span data-ttu-id="151f1-120">Töltse ki hello visszaállítási űrlapot hello szükséges információkkal:</span><span class="sxs-lookup"><span data-stu-id="151f1-120">Fill out hello Restore form with hello required information:</span></span>

- <span data-ttu-id="151f1-121">**Visszaállítási pont (idő szerint UTC)**: hello Dátumválasztó és idő kiválasztása jelöljön ki egy időpontban toorestore számára.</span><span class="sxs-lookup"><span data-stu-id="151f1-121">**Restore point (UTC)**: Using hello Date picker and time picker, select a point-in-time toorestore to.</span></span> <span data-ttu-id="151f1-122">hello megadott ideje UTC formátumban, ezért az UTC valószínűleg kell tooconvert hello helyi idő.</span><span class="sxs-lookup"><span data-stu-id="151f1-122">hello time specified is in UTC format, so you likely need tooconvert hello local time into UTC.</span></span>
- <span data-ttu-id="151f1-123">**Toonew-kiszolgáló helyreállítása**: Adjon meg egy új kiszolgálót neve toorestore hello meglévő kiszolgálójáról.</span><span class="sxs-lookup"><span data-stu-id="151f1-123">**Restore toonew server**: Provide a new server name toorestore hello existing server into.</span></span>
- <span data-ttu-id="151f1-124">**Hely**: hello régió automatikusan hello forrás server régió tölti fel, és nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="151f1-124">**Location**: hello region choice automatically populates with hello source server region, and cannot be changed.</span></span>
- <span data-ttu-id="151f1-125">**IP-címek**: hello automatikusan árképzési szint választott tölti fel hello azonos árképzési hello forráskiszolgáló réteg, és itt nem módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="151f1-125">**Pricing tier**: hello pricing tier choice automatically populates with hello same pricing tier as hello source server, and cannot be changed here.</span></span> 
<span data-ttu-id="151f1-126">![PITR visszaállítása](./media/howto-restore-server-portal/pitr-restore.png)</span><span class="sxs-lookup"><span data-stu-id="151f1-126">![PITR Restore](./media/howto-restore-server-portal/pitr-restore.png)</span></span>

5. <span data-ttu-id="151f1-127">Kattintson a **OK** toorestore hello server toorestore tooa pont időben.</span><span class="sxs-lookup"><span data-stu-id="151f1-127">Click **OK** toorestore hello server toorestore tooa point in time.</span></span> 

6. <span data-ttu-id="151f1-128">Hello visszaállítás befejezése után keresse meg a hello tooverify hello adatbázisok volt visszaállítása az elvártaknak megfelelően létrehozott új kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="151f1-128">After hello restore finishes, locate hello new server that was created tooverify hello databases were restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="151f1-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="151f1-129">Next steps</span></span>
- [<span data-ttu-id="151f1-130">Adatkapcsolattárak MySQL az Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="151f1-130">Connection libraries for Azure Database for MySQL</span></span>](concepts-connection-libraries.md)