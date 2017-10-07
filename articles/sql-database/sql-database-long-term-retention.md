---
title: "aaaStore Azure SQL Database készített biztonsági másolatot too10 év |} Microsoft Docs"
description: "Ismerje meg, hogyan támogatja az Azure SQL Database a tárolni készített biztonsági másolatot too10 év."
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 66fdb8b8-5903-4d3a-802e-af08d204566e
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/22/2016
ms.author: sashan
ms.openlocfilehash: 5825ebd4e3bd66b59b13aea603d377ef814a1df3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="store-azure-sql-database-backups-for-up-too10-years"></a><span data-ttu-id="89add-103">Az Azure SQL Database biztonsági másolatok a too10 év mentése</span><span class="sxs-lookup"><span data-stu-id="89add-103">Store Azure SQL Database backups for up too10 years</span></span>
<span data-ttu-id="89add-104">Számos alkalmazás rendelkezik szabályozó, megfelelőségi vagy egyéb üzleti célokat szolgál, amely a használatba tooretain adatbázis biztonsági másolatait túl hello Azure SQL Database által biztosított 7 – 35 nap [automatikus biztonsági mentésekhez](sql-database-automated-backups.md).</span><span class="sxs-lookup"><span data-stu-id="89add-104">Many applications have regulatory, compliance, or other business purposes that require you tooretain database backups beyond hello 7-35 days provided by Azure SQL Database [automatic backups](sql-database-automated-backups.md).</span></span> <span data-ttu-id="89add-105">Hello hosszú távú biztonsági másolatok megőrzésének funkció használatával tárolhatja az Azure Recovery Services-tároló a too10 év fel az SQL-adatbázis biztonsági másolatait.</span><span class="sxs-lookup"><span data-stu-id="89add-105">By using hello long-term backup retention feature, you can store your SQL database backups in an Azure Recovery Services vault for up too10 years.</span></span> <span data-ttu-id="89add-106">Too1, 000 adatbázisokat tároló egy másolatot is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="89add-106">You can store up too1,000 databases per vault.</span></span> <span data-ttu-id="89add-107">Majd igény szerint biztonsági másolat a hello tároló toorestore azt egy új adatbázist.</span><span class="sxs-lookup"><span data-stu-id="89add-107">You then can select any backup in hello vault toorestore it as a new database.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="89add-108">Hosszú távú biztonsági másolatok megőrzésének jelenleg jelenleg előzetes és érhető el a következő régiókban hello: Kelet-Ausztrália, Ausztrália délkeleti, Dél-Brazília, USA középső RÉGIÓJA, Kelet-Ázsia, USA keleti régiója, USA keleti régiója 2. régiója, közép-India, Dél-India, kelet-japán, Nyugat-japán, északi középső Régiójában, Észak Európa, USA déli középső RÉGIÓJA, Délkelet-Ázsiában, Nyugat-Európában és USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="89add-108">Long-term backup retention is currently in preview and available in hello following regions: Australia East, Australia Southeast, Brazil South, Central US, East Asia, East US, East US 2, India Central, India South, Japan East, Japan West, North Central US, North Europe, South Central US, Southeast Asia, West Europe, and West US.</span></span>
>

> [!NOTE]
> <span data-ttu-id="89add-109">Engedélyezheti / tároló too200 adatbázisok egy 24 órás időszakban.</span><span class="sxs-lookup"><span data-stu-id="89add-109">You can enable up too200 databases per vault during a 24-hour period.</span></span> <span data-ttu-id="89add-110">Azt javasoljuk, hogy minden kiszolgáló toominimize hello hatása ezt a határt egy külön tárolóba használjon.</span><span class="sxs-lookup"><span data-stu-id="89add-110">We recommend that you use a separate vault for each server toominimize hello impact of this limit.</span></span> 
> 

## <a name="how-sql-database-long-term-backup-retention-works"></a><span data-ttu-id="89add-111">SQL-adatbázis hosszú távú biztonsági másolatok megőrzésének működése</span><span class="sxs-lookup"><span data-stu-id="89add-111">How SQL Database long-term backup retention works</span></span>

<span data-ttu-id="89add-112">A hosszú távú biztonsági másolatok megőrzésének az Azure Recovery Services-tároló SQL adatbázis-kiszolgálót is társíthat.</span><span class="sxs-lookup"><span data-stu-id="89add-112">With long-term backup retention, you can associate a SQL database server with an Azure Recovery Services vault.</span></span> 

* <span data-ttu-id="89add-113">Hello tárolóban kell létrehoznia a hello azonos Azure-előfizetéshez létrehozott hello SQL server és a hello azonos földrajzi régióban és erőforrás-csoport.</span><span class="sxs-lookup"><span data-stu-id="89add-113">You must create hello vault in hello same Azure subscription that created hello SQL server and in hello same geographic region and resource group.</span></span> 
* <span data-ttu-id="89add-114">Konfigurálhat egy megőrzési házirend bármely adatbázis.</span><span class="sxs-lookup"><span data-stu-id="89add-114">You then configure a retention policy for any database.</span></span> <span data-ttu-id="89add-115">hello házirend okok hello heti adatbázis teljes biztonsági mentések toobe toohello Recovery Services-tároló másolta, és a (felfelé too10 év) hello megadott megőrzési ideig őrzi meg.</span><span class="sxs-lookup"><span data-stu-id="89add-115">hello policy causes hello weekly full database backups toobe copied toohello Recovery Services vault and retained for hello specified retention period (up too10 years).</span></span> 
* <span data-ttu-id="89add-116">Visszaállíthatja a hello adatbázis bármelyik a biztonsági mentések tooa új adatbázis bármely kiszolgálón hello előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="89add-116">You can then restore hello database from any of these backups tooa new database in any server in hello subscription.</span></span> <span data-ttu-id="89add-117">Az Azure storage létrehoz egy másolatot a meglévő biztonsági mentésekből, és hello példány nincs hatása van hello létező adatbázis felett.</span><span class="sxs-lookup"><span data-stu-id="89add-117">Azure storage creates a copy from existing backups, and hello copy has no performance impact on hello existing database.</span></span>

> [!TIP]
> <span data-ttu-id="89add-118">Tekintse meg a-tooguide, hogyan [és konfigurálása az Azure SQL Database hosszú távú biztonsági mentés megőrzési visszaállítás](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="89add-118">For a how-tooguide, see [Configure and restore from Azure SQL Database long-term backup retention](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="enable-long-term-backup-retention"></a><span data-ttu-id="89add-119">Hosszú távú biztonsági másolatok megőrzésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="89add-119">Enable long-term backup retention</span></span>

<span data-ttu-id="89add-120">tooconfigure hosszú távú biztonsági másolatok megőrzésének adatbázis esetén:</span><span class="sxs-lookup"><span data-stu-id="89add-120">tooconfigure long-term backup retention for a database:</span></span>

1. <span data-ttu-id="89add-121">Hozzon létre egy Azure Recovery Services-tároló hello azonos régióban, az előfizetés és az erőforrás tartozik, mint az SQL database-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="89add-121">Create an Azure Recovery Services vault in hello same region, subscription, and resource group as your SQL database server.</span></span> 
2. <span data-ttu-id="89add-122">Regisztrálja a hello server toohello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="89add-122">Register hello server toohello vault.</span></span>
3. <span data-ttu-id="89add-123">Hozzon létre egy Azure Recovery Services védelmi házirendet.</span><span class="sxs-lookup"><span data-stu-id="89add-123">Create an Azure Recovery Services protection policy.</span></span>
4. <span data-ttu-id="89add-124">Hello védelmi házirend toohello adatbázisok hosszú távú biztonsági másolatok megőrzésének igénylő alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="89add-124">Apply hello protection policy toohello databases that require long-term backup retention.</span></span>

<span data-ttu-id="89add-125">tooconfigure, kezeléséhez, és állítsa vissza egy adatbázist egy Azure Recovery Services-tároló az automatikus biztonsági másolatok hosszú távú biztonsági másolatok megőrzésének, hello alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="89add-125">tooconfigure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault, do either of hello following:</span></span>

* <span data-ttu-id="89add-126">Az Azure portál használatával hello: kattintson a **hosszú távú biztonsági másolatok megőrzésének**, válasszon ki egy adatbázist, és kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="89add-126">Using hello Azure portal: Click **Long-term backup retention**, select a database, and then click **Configure**.</span></span> 

   ![Válasszon egy adatbázist, a hosszú távú biztonsági másolatok megőrzésének](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

* <span data-ttu-id="89add-128">PowerShell használatával: Nyissa meg túl[és konfigurálása az Azure SQL Database hosszú távú biztonsági mentés megőrzési visszaállítás](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="89add-128">Using PowerShell: Go too[Configure and restore from Azure SQL Database long-term backup retention](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="restore-a-database-thats-stored-with-hello-long-term-backup-retention-feature"></a><span data-ttu-id="89add-129">Hello hosszú távú biztonsági másolatok megőrzésének szolgáltatásával tárolt adatbázis visszaállítása</span><span class="sxs-lookup"><span data-stu-id="89add-129">Restore a database that's stored with hello long-term backup retention feature</span></span>

<span data-ttu-id="89add-130">hosszú távú biztonsági másolatok megőrzésének másolatból toorecover:</span><span class="sxs-lookup"><span data-stu-id="89add-130">toorecover from a long-term backup retention backup:</span></span>

1. <span data-ttu-id="89add-131">Lista hello tároló hello biztonsági mentési tároló.</span><span class="sxs-lookup"><span data-stu-id="89add-131">List hello vault where hello backup is stored.</span></span>
2. <span data-ttu-id="89add-132">Lista hello-tárolóhoz csatlakoztatott tooyour logikai kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="89add-132">List hello container that is mapped tooyour logical server.</span></span>
3. <span data-ttu-id="89add-133">Lista hello adatforráshoz olyan csatlakoztatott tooyour adatbázis hello tárolóból.</span><span class="sxs-lookup"><span data-stu-id="89add-133">List hello data source within hello vault that is mapped tooyour database.</span></span>
4. <span data-ttu-id="89add-134">Lista hello helyreállítási pontok, amelyek a rendelkezésre álló toorestore.</span><span class="sxs-lookup"><span data-stu-id="89add-134">List hello recovery points that are available toorestore.</span></span>
5. <span data-ttu-id="89add-135">Hello adatbázis visszaállítása a célkiszolgálóról hello helyreállítási pont toohello az előfizetésen belül.</span><span class="sxs-lookup"><span data-stu-id="89add-135">Restore hello database from hello recovery point toohello target server within your subscription.</span></span>

<span data-ttu-id="89add-136">tooconfigure, kezeléséhez, és állítsa vissza egy adatbázist egy Azure Recovery Services-tároló az automatikus biztonsági másolatok hosszú távú biztonsági másolatok megőrzésének, hello alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="89add-136">tooconfigure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault, do either of hello following:</span></span>

* <span data-ttu-id="89add-137">Az Azure portál használatával hello: Nyissa meg túl[kezelése hosszú távú biztonsági másolatok megőrzésének használatával hello Azure-portálon](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="89add-137">Using hello Azure portal: Go too[Manage long-term backup retention using hello Azure portal](sql-database-long-term-backup-retention-configure.md).</span></span> 

* <span data-ttu-id="89add-138">PowerShell használatával: Nyissa meg túl[kezelése a PowerShell használatával hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="89add-138">Using PowerShell: Go too[Manage long-term backup retention using PowerShell](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="get-pricing-for-long-term-backup-retention"></a><span data-ttu-id="89add-139">A hosszú távú biztonsági másolatok megőrzésének Árképzés beolvasása</span><span class="sxs-lookup"><span data-stu-id="89add-139">Get pricing for long-term backup retention</span></span>

<span data-ttu-id="89add-140">Hosszú távú biztonsági másolatok megőrzésének SQL-adatbázis szerint toohello díjfizetéssel [díjszabás árképzési Azure biztonsági mentési szolgáltatások](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="89add-140">Long-term backup retention of a SQL database is charged according toohello [Azure backup services pricing rates](https://azure.microsoft.com/pricing/details/backup/).</span></span>

<span data-ttu-id="89add-141">Miután hello SQL adatbázis-kiszolgáló regisztrált toohello tárolóban, van szó, a teljes tárterület hello hello heti biztonsági mentései hello tárolóban tárolt által használt.</span><span class="sxs-lookup"><span data-stu-id="89add-141">After hello SQL database server is registered toohello vault, you are charged for hello total storage that's used by hello weekly backups stored in hello vault.</span></span>

## <a name="view-available-backups-that-are-stored-in-long-term-backup-retention"></a><span data-ttu-id="89add-142">Elérhető biztonsági másolatok megtekintése, hosszú távú biztonsági másolatok megőrzésének vannak tárolva</span><span class="sxs-lookup"><span data-stu-id="89add-142">View available backups that are stored in long-term backup retention</span></span>

<span data-ttu-id="89add-143">tooconfigure, kezeléséhez, és állítsa vissza egy adatbázist egy Azure Recovery Services-tároló az automatikus biztonsági másolatok hosszú távú biztonsági másolatok megőrzésének hello Azure-portál használatával, hello alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="89add-143">tooconfigure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault by using hello Azure portal, do either of hello following:</span></span>

* <span data-ttu-id="89add-144">Az Azure portál használatával hello: Nyissa meg túl[kezelése hosszú távú biztonsági másolatok megőrzésének használatával hello Azure-portálon](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="89add-144">Using hello Azure portal: Go too[Manage long-term backup retention using hello Azure portal](sql-database-long-term-backup-retention-configure.md).</span></span> 

* <span data-ttu-id="89add-145">PowerShell használatával: Nyissa meg túl[kezelése a PowerShell használatával hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="89add-145">Using PowerShell: Go too[Manage long-term backup retention using PowerShell](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="disable-long-term-retention"></a><span data-ttu-id="89add-146">Hosszú távú megőrzési letiltása</span><span class="sxs-lookup"><span data-stu-id="89add-146">Disable long-term retention</span></span>

<span data-ttu-id="89add-147">hello szolgáltatásban automatikusan kezeli a biztonsági mentések adatmegőrzési megadott hello alapján hello karbantartása.</span><span class="sxs-lookup"><span data-stu-id="89add-147">hello recovery service automatically handles hello cleanup of backups based on hello provided retention policy.</span></span> 

<span data-ttu-id="89add-148">egy adott adatbázis toohello tároló küldő toostop hello biztonsági mentések adatmegőrzési hello az adatbázishoz tartozó eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="89add-148">toostop sending hello backups for a specific database toohello vault, remove hello retention policy for that database.</span></span>
  
```
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName 'RG1' -ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
```

> [!NOTE]
> <span data-ttu-id="89add-149">hello biztonsági mentések, amelyek már hello tárolóban nem érinti.</span><span class="sxs-lookup"><span data-stu-id="89add-149">hello backups that are already in hello vault are unaffected.</span></span> <span data-ttu-id="89add-150">Ezek automatikusan törli hello szolgáltatásban a megőrzési időszak lejártával.</span><span class="sxs-lookup"><span data-stu-id="89add-150">They are automatically deleted by hello recovery service when their retention period expires.</span></span>

## <a name="long-term-backup-retention-faq"></a><span data-ttu-id="89add-151">Hosszú távú biztonsági másolatok megőrzésének – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="89add-151">Long-term backup retention FAQ</span></span>

<span data-ttu-id="89add-152">**Manuálisan törölhetek egyedi biztonsági másolatok hello tárolóban?**</span><span class="sxs-lookup"><span data-stu-id="89add-152">**Can I manually delete specific backups in hello vault?**</span></span>

<span data-ttu-id="89add-153">Jelenleg nem.</span><span class="sxs-lookup"><span data-stu-id="89add-153">Not currently.</span></span> <span data-ttu-id="89add-154">hello tárolóban automatikusan törli azokat a biztonsági mentések hello megőrzési időszak lejárta.</span><span class="sxs-lookup"><span data-stu-id="89add-154">hello vault automatically cleans up backups when hello retention period has expired.</span></span>

<span data-ttu-id="89add-155">**Regisztrálhatja a saját kiszolgáló toostore biztonsági mentések toomore mint külön-külön tárolóba?**</span><span class="sxs-lookup"><span data-stu-id="89add-155">**Can I register my server toostore backups toomore than one vault?**</span></span>

<span data-ttu-id="89add-156">Egyszerre nem, biztonsági mentések tooonly külön-külön tárolóba jelenleg tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="89add-156">No, you can currently store backups tooonly one vault at a time.</span></span>

<span data-ttu-id="89add-157">**Is kell a tároló és a kiszolgáló különböző előfizetésekhez?**</span><span class="sxs-lookup"><span data-stu-id="89add-157">**Can I have a vault and server in different subscriptions?**</span></span>

<span data-ttu-id="89add-158">Nem, jelenleg hello tároló és a kiszolgáló kell hello azonos előfizetésbe és erőforráscsoportba csoport.</span><span class="sxs-lookup"><span data-stu-id="89add-158">No, currently hello vault and server must be in hello same subscription and resource group.</span></span>

<span data-ttu-id="89add-159">**Használhatók egy olyan, amely eltér a kiszolgálója régió régióban létrehozott tárolóból?**</span><span class="sxs-lookup"><span data-stu-id="89add-159">**Can I use a vault that I created in a region that's different from my server’s region?**</span></span>

<span data-ttu-id="89add-160">Nem, hello tároló és a kiszolgáló kell hello ugyanabban a régióban toominimize idő másolása és forgalom díjak elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="89add-160">No, hello vault and server must be in hello same region toominimize copy time and avoid traffic charges.</span></span>

<span data-ttu-id="89add-161">**Hány adatbázisokat is tárolnak a külön-külön tárolóba?**</span><span class="sxs-lookup"><span data-stu-id="89add-161">**How many databases can I store in one vault?**</span></span>

<span data-ttu-id="89add-162">Jelenleg támogatott too1, tároló / 000 adatbázisok fel.</span><span class="sxs-lookup"><span data-stu-id="89add-162">Currently, we support up too1,000 databases per vault.</span></span> 

<span data-ttu-id="89add-163">**Hány tárolók hozhat létre előfizetésenként?**</span><span class="sxs-lookup"><span data-stu-id="89add-163">**How many vaults can I create per subscription?**</span></span>

<span data-ttu-id="89add-164">Előfizetésenként too25 tárolók másolatot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="89add-164">You can create up too25 vaults per subscription.</span></span>

<span data-ttu-id="89add-165">**Hány adatbázisokat is konfigurálja / nap / tároló?**</span><span class="sxs-lookup"><span data-stu-id="89add-165">**How many databases can I configure per day per vault?**</span></span>

<span data-ttu-id="89add-166">200 adatbázisok / nap / tároló állíthat be.</span><span class="sxs-lookup"><span data-stu-id="89add-166">You can set up 200 databases per day per vault.</span></span>

<span data-ttu-id="89add-167">**Hosszú távú biztonsági másolatok megőrzésének rugalmas készletek működik?**</span><span class="sxs-lookup"><span data-stu-id="89add-167">**Does long-term backup retention work with elastic pools?**</span></span>

<span data-ttu-id="89add-168">Igen.</span><span class="sxs-lookup"><span data-stu-id="89add-168">Yes.</span></span> <span data-ttu-id="89add-169">Minden adatbázis hello készletben hello adatmegőrzési konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="89add-169">Any database in hello pool can be configured with hello retention policy.</span></span>

<span data-ttu-id="89add-170">**Választhatja a hello idő, amellyel hello biztonsági másolat készítése során?**</span><span class="sxs-lookup"><span data-stu-id="89add-170">**Can I choose hello time at which hello backup is created?**</span></span>

<span data-ttu-id="89add-171">Nem, az SQL-adatbázis hello biztonsági mentés ütemezése toominimize hello teljesítményre gyakorolt hatás az adatbázisok szabályozza.</span><span class="sxs-lookup"><span data-stu-id="89add-171">No, SQL Database controls hello backup schedule toominimize hello performance impact on your databases.</span></span>

<span data-ttu-id="89add-172">**Átlátható adattitkosítás az adatbázis engedélyezve van. Használhatom az hello tárolóban?**</span><span class="sxs-lookup"><span data-stu-id="89add-172">**I have transparent data encryption enabled for my database. Can I use it with hello vault?**</span></span> 

<span data-ttu-id="89add-173">Igen, támogatja a transzparens adattitkosítás.</span><span class="sxs-lookup"><span data-stu-id="89add-173">Yes, transparent data encryption is supported.</span></span> <span data-ttu-id="89add-174">Visszaállíthatja hello adatbázis hello tárolóból, még akkor is, ha az eredeti adatbázis hello már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="89add-174">You can restore hello database from hello vault even if hello original database no longer exists.</span></span>

<span data-ttu-id="89add-175">**Mi történik hello biztonsági hello tárolóban, ha az előfizetés fel van függesztve?**</span><span class="sxs-lookup"><span data-stu-id="89add-175">**What happens with hello backups in hello vault if my subscription is suspended?**</span></span> 

<span data-ttu-id="89add-176">Ha az előfizetés fel van függesztve, a Microsoft hello meglévő adatbázisok és a biztonsági másolatok megőrzése mellett.</span><span class="sxs-lookup"><span data-stu-id="89add-176">If your subscription is suspended, we retain hello existing databases and backups.</span></span> <span data-ttu-id="89add-177">Új biztonsági másolatok csak másolt toohello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="89add-177">New backups are not copied toohello vault.</span></span> <span data-ttu-id="89add-178">Miután hello előfizetés újraaktiválásához hello szolgáltatás folytatása biztonsági mentések toohello tároló másolása</span><span class="sxs-lookup"><span data-stu-id="89add-178">After you reactivate hello subscription, hello service resumes copying backups toohello vault.</span></span> <span data-ttu-id="89add-179">A tároló másolt nincs előtt hello előfizetés fel lett függesztve hello biztonsági mentések használatával elérhető toohello visszaállítási műveletek válik.</span><span class="sxs-lookup"><span data-stu-id="89add-179">Your vault becomes accessible toohello restore operations by using hello backups that were copied there before hello subscription was suspended.</span></span> 

<span data-ttu-id="89add-180">**Kaphatok hozzáférés toohello SQL adatbázis biztonságimásolat-fájlok, letöltése és visszaállíthatják toohello SQL server is?**</span><span class="sxs-lookup"><span data-stu-id="89add-180">**Can I get access toohello SQL database backup files so I can download or restore them toohello SQL server?**</span></span>

<span data-ttu-id="89add-181">Nem, jelenleg nem.</span><span class="sxs-lookup"><span data-stu-id="89add-181">No, not currently.</span></span>

<span data-ttu-id="89add-182">**Ennyi az lehetséges toohave több ütemezi SQL adatmegőrzési belül (napi, heti, havi, évente).**</span><span class="sxs-lookup"><span data-stu-id="89add-182">**Is it possible toohave multiple schedules (daily, weekly, monthly, yearly) within a SQL retention policy.**</span></span>

<span data-ttu-id="89add-183">Nem, több ütemezések jelenleg csak virtuális gépek biztonsági mentéseinek érhető el.</span><span class="sxs-lookup"><span data-stu-id="89add-183">No, multiple schedules are currently available only for virtual machine backups.</span></span>

<span data-ttu-id="89add-184">**Mi történik, ha hosszú távú biztonsági másolatok megőrzésének olyan adatbázisban, amely egy aktív georeplikáció másodlagos adatbázis beállítani?**</span><span class="sxs-lookup"><span data-stu-id="89add-184">**What if we set up long-term backup retention on a database that is an active geo-replication secondary database?**</span></span>

<span data-ttu-id="89add-185">A replikák biztonsági mentések nem vesszük, mert jelenleg nincs beállítás, a hosszú távú biztonsági másolatok megőrzésének a másodlagos adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="89add-185">Because we don't take backups on replicas, there is currently no option for long-term backup retention on secondary databases.</span></span> <span data-ttu-id="89add-186">Azonban fontos felhasználók tooset fel egy aktív georeplikáció másodlagos adatbázison hosszú távú biztonsági másolatok megőrzésének ezen okok miatt:</span><span class="sxs-lookup"><span data-stu-id="89add-186">However, it is important for users tooset up long-term backup retention on an active geo-replication secondary database for these reasons:</span></span>
* <span data-ttu-id="89add-187">Ha feladatátvétel történik, és hello adatbázis elsődleges adatbázis válik, azt egy teljes biztonsági másolatok készítéséhez, amely feltöltött toovault.</span><span class="sxs-lookup"><span data-stu-id="89add-187">When a failover happens and hello database becomes a primary database, we take a full backup, which is uploaded toovault.</span></span>
* <span data-ttu-id="89add-188">Nincs semmilyen további költség toohello vevő egy másodlagos adatbázison hosszú távú biztonsági másolatok megőrzésének beállításához.</span><span class="sxs-lookup"><span data-stu-id="89add-188">There is no extra cost toohello customer for setting up long-term backup retention on a secondary database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="89add-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="89add-189">Next steps</span></span>
<span data-ttu-id="89add-190">Adatbázis biztonsági másolatait véletlen sérülése vagy a törlés adatok védelme, mert nagyon fontos részét képezik az üzletmenet folytonosságának és a vész-helyreállítási stratégiát fontosságúak.</span><span class="sxs-lookup"><span data-stu-id="89add-190">Because database backups protect data from accidental corruption or deletion, they're an essential part of any business continuity and disaster recovery strategy.</span></span> <span data-ttu-id="89add-191">toolearn készül más SQL-adatbázis üzleti folytonosságot biztosító megoldások hello című [üzleti folytonosság – áttekintés](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="89add-191">toolearn about hello other SQL Database business-continuity solutions, see [Business continuity overview](sql-database-business-continuity.md).</span></span>
