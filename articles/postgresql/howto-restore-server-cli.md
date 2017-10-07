---
title: "Hogyan tooback össze, és visszaállíthatja egy kiszolgálóhoz az Azure-adatbázis PostgreSQL |} Microsoft Docs"
description: "Ismerje meg, hogyan tooback mentése és visszaállítása egy kiszolgálóhoz az Azure-adatbázis a PostgreSQL használatával hello Azure parancssori felület."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 0b9ed25e3e3a88dd5c7ffe2ae7c27f8eef9be710
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooback-up-and-restore-a-server-in-azure-database-for-postgresql-by-using-hello-azure-cli"></a><span data-ttu-id="f8fe6-103">Hogyan tooback mentése és visszaállítása egy kiszolgálóhoz az Azure-adatbázis a PostgreSQL használatával hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="f8fe6-103">How tooback up and restore a server in Azure Database for PostgreSQL by using hello Azure CLI</span></span>

<span data-ttu-id="f8fe6-104">Azure-adatbázist használ a kiszolgáló adatbázis tooan PostgreSQL toorestore too35 7 napról kiterjedő korábbi dátumot.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-104">Use Azure Database for PostgreSQL toorestore a server database tooan earlier date that spans from 7 too35 days.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8fe6-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f8fe6-105">Prerequisites</span></span>
<span data-ttu-id="f8fe6-106">toocomplete e-tooguide, hogyan lehet szüksége:</span><span class="sxs-lookup"><span data-stu-id="f8fe6-106">toocomplete this how-tooguide, you need:</span></span>
- <span data-ttu-id="f8fe6-107">Egy [PostgreSQL-kiszolgáló és adatbázis Azure-adatbázis](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="f8fe6-107">An [Azure Database for PostgreSQL server and database](quickstart-create-server-database-azure-cli.md)</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> <span data-ttu-id="f8fe6-108">Telepítésekor, és hello Azure CLI helyileg használja, ez hogyan tooguide használatához Azure CLI 2.0-s vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-108">If you install and use hello Azure CLI locally, this how-tooguide requires that you use Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="f8fe6-109">Adja meg a tooconfirm hello verziója, a parancssorba hello Azure CLI `az --version`.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-109">tooconfirm hello version, at hello Azure CLI command prompt, enter `az --version`.</span></span> <span data-ttu-id="f8fe6-110">tooinstall vagy frissítés, lásd: [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f8fe6-110">tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="back-up-happens-automatically"></a><span data-ttu-id="f8fe6-111">Készítsen biztonsági másolatot automatikusan történik,</span><span class="sxs-lookup"><span data-stu-id="f8fe6-111">Back up happens automatically</span></span>
<span data-ttu-id="f8fe6-112">Amikor PostgreSQL Azure-adatbázist használ, hello adatbázis-szolgáltatás automatikusan teszi hello szolgáltatás biztonsági másolatot, 5 percenként.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-112">When you use Azure Database for PostgreSQL, hello database service automatically makes a backup of hello service every 5 minutes.</span></span> 

<span data-ttu-id="f8fe6-113">Az alapszintű csomag hello biztonsági mentések érhetők el a 7 napja.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-113">For Basic Tier, hello backups are available for 7 days.</span></span> <span data-ttu-id="f8fe6-114">Standard szint hello biztonsági mentések érhetők el 35 napon.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-114">For Standard Tier, hello backups are available for 35 days.</span></span> <span data-ttu-id="f8fe6-115">További információkért lásd: [Azure-adatbázis a tarifacsomagok PostgreSQL](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="f8fe6-115">For more information, see [Azure Database for PostgreSQL pricing tiers](concepts-service-tiers.md).</span></span>

<span data-ttu-id="f8fe6-116">Az automatikus biztonsági mentési szolgáltatással hello kiszolgáló és az adatbázisok tooan visszaállítása korábbi dátumot, vagy mikor.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-116">With this automatic backup feature, you can restore hello server and its databases tooan earlier date, or point in time.</span></span>

## <a name="restore-a-database-tooa-previous-point-in-time-by-using-hello-azure-cli"></a><span data-ttu-id="f8fe6-117">Állítsa vissza egy adatbázis tooa korábbi időpontra időben hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="f8fe6-117">Restore a database tooa previous point in time by using hello Azure CLI</span></span>
<span data-ttu-id="f8fe6-118">Az Azure Database PostgreSQL toorestore hello server tooa előző pont időben.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-118">Use Azure Database for PostgreSQL toorestore hello server tooa previous point in time.</span></span> <span data-ttu-id="f8fe6-119">hello visszaállított adatok másolt tooa új kiszolgálóra, és hello meglévő kiszolgáló marad, mert a.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-119">hello restored data is copied tooa new server, and hello existing server is left as is.</span></span> <span data-ttu-id="f8fe6-120">Például egy táblázat véletlenül megszakadása ma órakor, toohello idő előtt déltől helyreállíthatja.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-120">For example, if a table is accidentally dropped at noon today, you can restore toohello time just before noon.</span></span> <span data-ttu-id="f8fe6-121">Ezután le hello hiányzik a táblázat, és vissza hello példányát hello kiszolgáló adatait.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-121">Then, you can retrieve hello missing table and data from hello restored copy of hello server.</span></span> 

<span data-ttu-id="f8fe6-122">toorestore hello kiszolgáló, az Azure parancssori felület használata hello [az postgres kiszolgálójának visszaállítását](/cli/azure/postgres/server#restore) parancsot.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-122">toorestore hello server, use hello Azure CLI [az postgres server restore](/cli/azure/postgres/server#restore) command.</span></span>

### <a name="run-hello-restore-command"></a><span data-ttu-id="f8fe6-123">Hello visszaállítási parancs futtatása</span><span class="sxs-lookup"><span data-stu-id="f8fe6-123">Run hello restore command</span></span>

<span data-ttu-id="f8fe6-124">toorestore hello server parancssorba hello Azure CLI adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="f8fe6-124">toorestore hello server, at hello Azure CLI command prompt, enter hello following command:</span></span>

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

<span data-ttu-id="f8fe6-125">Hello `az postgres server restore` parancshoz szükséges a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="f8fe6-125">hello `az postgres server restore` command requires hello following parameters:</span></span>
| <span data-ttu-id="f8fe6-126">Beállítás</span><span class="sxs-lookup"><span data-stu-id="f8fe6-126">Setting</span></span> | <span data-ttu-id="f8fe6-127">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="f8fe6-127">Suggested value</span></span> | <span data-ttu-id="f8fe6-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="f8fe6-128">Description</span></span>  |
| --- | --- | --- |
| <span data-ttu-id="f8fe6-129">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="f8fe6-129">resource-group</span></span> |  <span data-ttu-id="f8fe6-130">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f8fe6-130">myResourceGroup</span></span> |  <span data-ttu-id="f8fe6-131">Az erőforráscsoport, ahol a forráskiszolgáló hello van.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-131">The resource group where hello source server exists.</span></span>  |
| <span data-ttu-id="f8fe6-132">név</span><span class="sxs-lookup"><span data-stu-id="f8fe6-132">name</span></span> | <span data-ttu-id="f8fe6-133">mypgserver visszaállítása</span><span class="sxs-lookup"><span data-stu-id="f8fe6-133">mypgserver-restored</span></span> | <span data-ttu-id="f8fe6-134">új kiszolgáló hello hello visszaállítási parancs által létrehozott hello neve.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-134">hello name of hello new server that is created by hello restore command.</span></span> |
| <span data-ttu-id="f8fe6-135">visszaállítás--időpontban</span><span class="sxs-lookup"><span data-stu-id="f8fe6-135">restore-point-in-time</span></span> | <span data-ttu-id="f8fe6-136">2017-04-13T13:59:00Z</span><span class="sxs-lookup"><span data-stu-id="f8fe6-136">2017-04-13T13:59:00Z</span></span> | <span data-ttu-id="f8fe6-137">Jelölje ki a pont idő toorestore számára.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-137">Select a point in time toorestore to.</span></span> <span data-ttu-id="f8fe6-138">A dátum és idő hello forráskiszolgáló készítsen biztonsági másolatot a megőrzési időn belül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-138">This date and time must be within hello source server's back up retention period.</span></span> <span data-ttu-id="f8fe6-139">Használjon hello ISO8601 dátum és idő formátumban.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-139">Use hello ISO8601 date and time format.</span></span> <span data-ttu-id="f8fe6-140">Például használhatja a saját helyi időzóna, például a `2017-04-13T05:59:00-08:00`.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-140">For example, you can use your own local time zone, such as `2017-04-13T05:59:00-08:00`.</span></span> <span data-ttu-id="f8fe6-141">Is használhatja a UTC Zulu formátumban, például hello `2017-04-13T13:59:00Z`.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-141">You can also use hello UTC Zulu format, for example, `2017-04-13T13:59:00Z`.</span></span> |
| <span data-ttu-id="f8fe6-142">forrás-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="f8fe6-142">source-server</span></span> | <span data-ttu-id="f8fe6-143">mypgserver-20170401</span><span class="sxs-lookup"><span data-stu-id="f8fe6-143">mypgserver-20170401</span></span> | <span data-ttu-id="f8fe6-144">hello nevét vagy hello forrás server toorestore az azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-144">hello name or ID of hello source server toorestore from.</span></span> |

<span data-ttu-id="f8fe6-145">Amikor egy kiszolgáló tooan helyreállítja idő korábbi pont, egy új kiszolgáló akkor jön létre.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-145">When you restore a server tooan earlier point in time, a new server is created.</span></span> <span data-ttu-id="f8fe6-146">hello eredeti kiszolgáló és az adatbázisok hello van megadva időpont: másolt toohello új kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-146">hello original server and its databases from hello specified point in time are copied toohello new server.</span></span>

<span data-ttu-id="f8fe6-147">hello helyét és árképzési szint értékek a visszaállított hello-kiszolgálók hello azonos hello eredeti kiszolgálóként.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-147">hello location and pricing tier values for hello restored server remain hello same as hello original server.</span></span> 

<span data-ttu-id="f8fe6-148">Hello `az postgres server restore` parancs szinkron.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-148">hello `az postgres server restore` command is synchronous.</span></span> <span data-ttu-id="f8fe6-149">Miután visszaállította hello kiszolgáló, azt újra használhatná toorepeat hello folyamat egy másik pont időben.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-149">After hello server is restored, you can use it again toorepeat hello process for a different point in time.</span></span> 

<span data-ttu-id="f8fe6-150">Hello után állítsa vissza a folyamat befejeződik, keresse meg az új kiszolgáló hello, és győződjön meg arról, hogy hello adatok visszaállítása, az elvártaknak megfelelően.</span><span class="sxs-lookup"><span data-stu-id="f8fe6-150">After hello restore process finishes, locate hello new server and verify that hello data is restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8fe6-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f8fe6-151">Next steps</span></span>
[<span data-ttu-id="f8fe6-152">Adatkapcsolattárak PostgreSQL Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="f8fe6-152">Connection libraries for Azure Database for PostgreSQL</span></span>](concepts-connection-libraries.md)
