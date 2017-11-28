---
title: "aaaManage Azure Backup-tárolók és a kiszolgálók Azure hello klasszikus üzembe helyezési modellel |} Microsoft Docs"
description: "Az oktatóanyag toolearn hogyan toomanage Azure Backup-tárolók és a kiszolgálók használata."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: f175eb12-0905-437f-91fd-eaee03ab6e81
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: markgal;
ms.openlocfilehash: 6c38b04f4a76604bfd639a9b2d58237ce44e2386
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-backup-vaults-and-servers-using-hello-classic-deployment-model"></a><span data-ttu-id="af26f-103">Azure mentési tárolókban és hello klasszikus üzembe helyezési modellel kiszolgálók kezelése</span><span class="sxs-lookup"><span data-stu-id="af26f-103">Manage Azure Backup vaults and servers using hello classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="af26f-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="af26f-104">Resource Manager</span></span>](backup-azure-manage-windows-server.md)
> * [<span data-ttu-id="af26f-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="af26f-105">Classic</span></span>](backup-azure-manage-windows-server-classic.md)
>
>

<span data-ttu-id="af26f-106">Ebben a cikkben találhat hello biztonságimásolat-felügyeleti feladatok hello a klasszikus Azure portálon keresztül elérhető és a Microsoft Azure Backup szolgáltatás ügynöke hello áttekintését.</span><span class="sxs-lookup"><span data-stu-id="af26f-106">In this article you'll find an overview of hello backup management tasks available through hello Azure classic portal and hello Microsoft Azure Backup agent.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="af26f-107">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="af26f-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="af26f-108">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="af26f-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="af26f-109">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="af26f-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="af26f-110">Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban.</span><span class="sxs-lookup"><span data-stu-id="af26f-110">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="af26f-111">További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="af26f-111">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="af26f-112">A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="af26f-112">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="af26f-113">2017. október 15. után PowerShell toocreate mentési tárolókban nem használható.</span><span class="sxs-lookup"><span data-stu-id="af26f-113">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="af26f-114">**2017. november 1-től**:</span><span class="sxs-lookup"><span data-stu-id="af26f-114">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="af26f-115">Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="af26f-115">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="af26f-116">Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="af26f-116">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="af26f-117">Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.</span><span class="sxs-lookup"><span data-stu-id="af26f-117">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="management-portal-tasks"></a><span data-ttu-id="af26f-118">Kezelési portál feladatok</span><span class="sxs-lookup"><span data-stu-id="af26f-118">Management portal tasks</span></span>
1. <span data-ttu-id="af26f-119">Jelentkezzen be toohello [kezelési portál](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="af26f-119">Sign in toohello [Management Portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="af26f-120">Kattintson a **Recovery Services**, majd kattintson a mentési tároló tooview hello gyors kezdés lapon hello nevére.</span><span class="sxs-lookup"><span data-stu-id="af26f-120">Click **Recovery Services**, then click hello name of backup vault tooview hello Quick Start page.</span></span>

    ![Helyreállítási szolgáltatások](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

<span data-ttu-id="af26f-122">Hello gyors kezdés lapon hello tetején hello lehetőségek kiválasztásával megtekintheti hello elérhető felügyeleti feladatait.</span><span class="sxs-lookup"><span data-stu-id="af26f-122">By selecting hello options at hello top of hello Quick Start page, you can see hello available management tasks.</span></span>

![Lapok kezelése](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a><span data-ttu-id="af26f-124">Irányítópult</span><span class="sxs-lookup"><span data-stu-id="af26f-124">Dashboard</span></span>
<span data-ttu-id="af26f-125">Válassza ki **irányítópult** toosee hello használatának áttekintése hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="af26f-125">Select **Dashboard** toosee hello usage overview for hello server.</span></span> <span data-ttu-id="af26f-126">Hello **használatának áttekintése** tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="af26f-126">hello **usage overview** includes:</span></span>

* <span data-ttu-id="af26f-127">a regisztrált toocloud Windows kiszolgálók hello száma</span><span class="sxs-lookup"><span data-stu-id="af26f-127">hello number of Windows Servers registered toocloud</span></span>
* <span data-ttu-id="af26f-128">az Azure virtuális gépek védett felhőben hello száma</span><span class="sxs-lookup"><span data-stu-id="af26f-128">hello number of Azure virtual machines protected in cloud</span></span>
* <span data-ttu-id="af26f-129">az Azure-ban felhasznált hello teljes tárhely</span><span class="sxs-lookup"><span data-stu-id="af26f-129">hello total storage consumed in Azure</span></span>
* <span data-ttu-id="af26f-130">hello legutóbbi feladatok állapota</span><span class="sxs-lookup"><span data-stu-id="af26f-130">hello status of recent jobs</span></span>

<span data-ttu-id="af26f-131">Hello irányítópult hello alján hello a következő feladatokat végezheti el:</span><span class="sxs-lookup"><span data-stu-id="af26f-131">At hello bottom of hello Dashboard you can perform hello following tasks:</span></span>

* <span data-ttu-id="af26f-132">**A tanúsítványnak a** – Ha egy tanúsítvány használt tooregister hello kiszolgáló volt, akkor a tooupdate hello tanúsítvány használatához.</span><span class="sxs-lookup"><span data-stu-id="af26f-132">**Manage certificate** - If a certificate was used tooregister hello server, then use this tooupdate hello certificate.</span></span> <span data-ttu-id="af26f-133">Ha a tárolói hitelesítő adatokat használ, ne használjon **szükséges tanúsítvány kezelése**.</span><span class="sxs-lookup"><span data-stu-id="af26f-133">If you are using vault credentials, do not use **Manage certificate**.</span></span>
* <span data-ttu-id="af26f-134">**Törlés** -törlések hello aktuális mentési tároló.</span><span class="sxs-lookup"><span data-stu-id="af26f-134">**Delete** - Deletes hello current backup vault.</span></span> <span data-ttu-id="af26f-135">A mentési tárolóban már nem használatos, ha törlése toofree tárolási helyet.</span><span class="sxs-lookup"><span data-stu-id="af26f-135">If a backup vault is no longer being used, you can delete it toofree up storage space.</span></span> <span data-ttu-id="af26f-136">**Törlés** csak akkor engedélyezett, miután az összes regisztrált kiszolgálókat, hogy törölték a hello tárolójából.</span><span class="sxs-lookup"><span data-stu-id="af26f-136">**Delete** is only enabled after all registered servers have been deleted from hello vault.</span></span>

![Biztonsági mentési irányítópult feladatok](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a><span data-ttu-id="af26f-138">Regisztrált cikkek</span><span class="sxs-lookup"><span data-stu-id="af26f-138">Registered items</span></span>
<span data-ttu-id="af26f-139">Válassza ki **regisztrált elemek** tooview hello neveket hello kiszolgálók regisztrálni toothis tárolóban.</span><span class="sxs-lookup"><span data-stu-id="af26f-139">Select **Registered Items** tooview hello names of hello servers that are registered toothis vault.</span></span>

![Regisztrált cikkek](./media/backup-azure-manage-windows-server-classic/registered-items.png)

<span data-ttu-id="af26f-141">Hello **típus** szűrő alapértelmezés szerint a virtuális gép tooAzure.</span><span class="sxs-lookup"><span data-stu-id="af26f-141">hello **Type** filter defaults tooAzure Virtual Machine.</span></span> <span data-ttu-id="af26f-142">tooview hello nevek regisztrált toothis tároló hello kiszolgálók kiválasztása **Windows server** hello a legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="af26f-142">tooview hello names of hello servers that are registered toothis vault, select **Windows server** from hello drop down menu.</span></span>

<span data-ttu-id="af26f-143">Itt hello a következő feladatokat végezheti el:</span><span class="sxs-lookup"><span data-stu-id="af26f-143">From here you can perform hello following tasks:</span></span>

* <span data-ttu-id="af26f-144">**Az Újraregisztrálás engedélyezése** – Ha ezt a beállítást is használhatja a kiszolgáló hello **regisztrációs varázsló** hello a helyszíni Microsoft Azure Backup agent tooregister hello Server hello mentési tárolóban még egyszer .</span><span class="sxs-lookup"><span data-stu-id="af26f-144">**Allow Re-registration** - When this option is selected for a server you can use hello **Registration Wizard** in hello on-premises Microsoft Azure Backup agent tooregister hello server with hello backup vault a second time.</span></span> <span data-ttu-id="af26f-145">Szükség lehet toore-nyilvántartás hello tanúsítványt, vagy ha egy kiszolgáló volt-e az újonnan létrehozott toobe tooan hiba miatt.</span><span class="sxs-lookup"><span data-stu-id="af26f-145">You might need toore-register due tooan error in hello certificate or if a server had toobe rebuilt.</span></span>
* <span data-ttu-id="af26f-146">**Törlés** -kiszolgáló törlése hello biztonsági mentési tárolóból.</span><span class="sxs-lookup"><span data-stu-id="af26f-146">**Delete** - Deletes a server from hello backup vault.</span></span> <span data-ttu-id="af26f-147">Hello kiszolgálóhoz társított hello tárolt adatok azonnal törli.</span><span class="sxs-lookup"><span data-stu-id="af26f-147">All of hello stored data associated with hello server is deleted immediately.</span></span>

    ![Regisztrált konfigurációelemekkel kapcsolatos feladatokhoz](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a><span data-ttu-id="af26f-149">Védett elemek</span><span class="sxs-lookup"><span data-stu-id="af26f-149">Protected items</span></span>
<span data-ttu-id="af26f-150">Válassza ki **védett elemek** tooview hello elemekre mentett hello kiszolgálókról.</span><span class="sxs-lookup"><span data-stu-id="af26f-150">Select **Protected Items** tooview hello items that have been backed up from hello servers.</span></span>

![Védett elemek](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a><span data-ttu-id="af26f-152">Konfigurálás</span><span class="sxs-lookup"><span data-stu-id="af26f-152">Configure</span></span>
<span data-ttu-id="af26f-153">A hello **konfigurálása** lapon hello megfelelő tárolási redundancia lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="af26f-153">From hello **Configure** tab you can select hello appropriate storage redundancy option.</span></span> <span data-ttu-id="af26f-154">hello idő tooselect hello tárolási redundancia legcélszerűbb egy tároló létrehozása után, és mielőtt a számítógépek regisztrált tooit.</span><span class="sxs-lookup"><span data-stu-id="af26f-154">hello best time tooselect hello storage redundancy option is right after creating a vault and before any machines are registered tooit.</span></span>

> [!WARNING]
> <span data-ttu-id="af26f-155">Egy elem regisztrált toohello tároló követően hello redundancia tárolómegoldást zárolva van, és nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="af26f-155">Once an item has been registered toohello vault, hello storage redundancy option is locked and cannot be modified.</span></span>
>
>

![Konfigurálás](./media/backup-azure-manage-windows-server-classic/configure.png)

<span data-ttu-id="af26f-157">További információ a jelen cikk tudnivalók [adattároló redundanciája, amely](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="af26f-157">See this article for more information about [storage redundancy](../storage/common/storage-redundancy.md).</span></span>

## <a name="microsoft-azure-backup-agent-tasks"></a><span data-ttu-id="af26f-158">A Microsoft Azure Backup szolgáltatás ügynöke feladatok</span><span class="sxs-lookup"><span data-stu-id="af26f-158">Microsoft Azure Backup agent tasks</span></span>
### <a name="console"></a><span data-ttu-id="af26f-159">Konzol</span><span class="sxs-lookup"><span data-stu-id="af26f-159">Console</span></span>
<span data-ttu-id="af26f-160">Nyissa meg hello **Microsoft Azure Backup szolgáltatás ügynökének** (a gép kereséssel megtalálja *a Microsoft Azure Backup szolgáltatás*).</span><span class="sxs-lookup"><span data-stu-id="af26f-160">Open hello **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

![Biztonságimásolat-készítő ügynökkel](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

<span data-ttu-id="af26f-162">A hello **műveletek** hello sarkában hello biztonságimásolat-készítő ügynök konzolon elérhető végezheti el a következő felügyeleti feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="af26f-162">From hello **Actions** available at hello right of hello backup agent console you can perform hello following management tasks:</span></span>

* <span data-ttu-id="af26f-163">Kiszolgáló regisztrálása</span><span class="sxs-lookup"><span data-stu-id="af26f-163">Register Server</span></span>
* <span data-ttu-id="af26f-164">Biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="af26f-164">Schedule Backup</span></span>
* <span data-ttu-id="af26f-165">Biztonsági másolat készítése</span><span class="sxs-lookup"><span data-stu-id="af26f-165">Back Up now</span></span>
* <span data-ttu-id="af26f-166">Tulajdonságainak módosítása</span><span class="sxs-lookup"><span data-stu-id="af26f-166">Change Properties</span></span>

![Ügynök konzol műveletek](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> <span data-ttu-id="af26f-168">túl**adatok helyreállítása**, lásd: [fájlok tooa Windows server vagy Windows-ügyfélszámítógép visszaállítása](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="af26f-168">too**Recover Data**, see [Restore files tooa Windows server or Windows client machine](backup-azure-restore-windows-server.md).</span></span>
>
>

### <a name="modify-an-existing-backup"></a><span data-ttu-id="af26f-169">Módosítsa a meglévő biztonsági mentésből</span><span class="sxs-lookup"><span data-stu-id="af26f-169">Modify an existing backup</span></span>
1. <span data-ttu-id="af26f-170">A Microsoft Azure Backup szolgáltatás ügynökének hello kattintson **biztonsági mentés ütemezése**.</span><span class="sxs-lookup"><span data-stu-id="af26f-170">In hello Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. <span data-ttu-id="af26f-172">A hello **ütemezett biztonsági mentés varázsló** hello hagyja **módosítása toobackup elemek vagy időpontok szerepelnek** jelölőnégyzetet és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="af26f-172">In hello **Schedule Backup Wizard** leave hello **Make changes toobackup items or times** option selected and click **Next**.</span></span>

    ![Ütemezett biztonsági mentés módosítása](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. <span data-ttu-id="af26f-174">Ha szeretné, hogy tooadd, vagy módosítsa az elemeket, hello **elemek kijelölése tooBackup** képernyőn kattintson **elemek hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="af26f-174">If you want tooadd or change items, on hello **Select Items tooBackup** screen click **Add Items**.</span></span>

    <span data-ttu-id="af26f-175">Azt is beállíthatja **kizárások beállításai** hello varázslóban ezen a lapon.</span><span class="sxs-lookup"><span data-stu-id="af26f-175">You can also set **Exclusion Settings** from this page in hello wizard.</span></span> <span data-ttu-id="af26f-176">Ha azt szeretné, hogy tooexclude fájlok vagy fájltípusok olvasási hozzáadására szolgáló eljárást hello [kizárások beállításai](#exclusion-settings).</span><span class="sxs-lookup"><span data-stu-id="af26f-176">If you want tooexclude files or file types read hello procedure for adding [exclusion settings](#exclusion-settings).</span></span>
4. <span data-ttu-id="af26f-177">Válassza ki a hello fájlok és mappák tooback szeretné, hogy fel, és kattintson a **gépházban**.</span><span class="sxs-lookup"><span data-stu-id="af26f-177">Select hello files and folders you want tooback up and click **Okay**.</span></span>

    ![Elemek hozzáadása](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. <span data-ttu-id="af26f-179">Adja meg a hello **biztonsági mentés ütemezése** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="af26f-179">Specify hello **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="af26f-180">(A 3-szor naponkénti maximum) napi vagy heti biztonsági mentései is ütemezheti.</span><span class="sxs-lookup"><span data-stu-id="af26f-180">You can schedule daily (at a maximum of 3 times per day) or weekly backups.</span></span>

    ![Adja meg a biztonsági mentés ütemezését](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > <span data-ttu-id="af26f-182">Adja meg a hello biztonsági mentés ütemezése esetén, tekintse meg a részletes [cikk](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="af26f-182">Specifying hello backup schedule is explained in detail in this [article](backup-azure-backup-cloud-as-tape.md).</span></span>
   >
   >
6. <span data-ttu-id="af26f-183">Jelölje be hello **adatmegőrzési** hello biztonsági másolatot, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="af26f-183">Select hello **Retention Policy** for hello backup copy and click **Next**.</span></span>

    ![Válassza ki az adatmegőrzési házirend](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. <span data-ttu-id="af26f-185">A hello **megerősítő** képernyőn hello információk áttekintése, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="af26f-185">On hello **Confirmation** screen review hello information and click **Finish**.</span></span>
8. <span data-ttu-id="af26f-186">Miután hello varázsló létrehozta a hello **biztonsági mentés ütemezése**, kattintson a **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="af26f-186">Once hello wizard finishes creating hello **backup schedule**, click **Close**.</span></span>

    <span data-ttu-id="af26f-187">Védelmi módosítása, után ellenőrizheti, hogy biztonsági mentések váltanak ki megfelelően fog toohello által **feladatok** lapra, és erősítse meg, hogy a módosítások megjelennek hello biztonsági mentési feladatok.</span><span class="sxs-lookup"><span data-stu-id="af26f-187">After modifying protection, you can confirm that backups are triggering correctly by going toohello **Jobs** tab and confirming that changes are reflected in hello backup jobs.</span></span>

### <a name="enable-network-throttling"></a><span data-ttu-id="af26f-188">Hálózati sávszélesség-szabályozás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="af26f-188">Enable Network Throttling</span></span>
<span data-ttu-id="af26f-189">hello Azure Backup szolgáltatás ügynökének a sávszélesség-szabályozási fülre, amely lehetővé teszi a hálózati sávszélesség használatának adatátvitel során toocontrol biztosít.</span><span class="sxs-lookup"><span data-stu-id="af26f-189">hello Azure Backup agent provides a Throttling tab which allows you toocontrol how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="af26f-190">Ez a vezérlő akkor lehet hasznos, ha tooback adatokat kell munkaidőben, de nem hello biztonsági mentési folyamat toointerfere a más internetes forgalmat.</span><span class="sxs-lookup"><span data-stu-id="af26f-190">This control can be helpful if you need tooback up data during work hours but do not want hello backup process toointerfere with other internet traffic.</span></span> <span data-ttu-id="af26f-191">Sávszélesség-szabályozás adatok átvitel mentése tooback vonatkozik, és állítsa vissza a tevékenységek.</span><span class="sxs-lookup"><span data-stu-id="af26f-191">Throttling of data transfer applies tooback up and restore activities.</span></span>  

<span data-ttu-id="af26f-192">sávszélesség-szabályozás tooenable:</span><span class="sxs-lookup"><span data-stu-id="af26f-192">tooenable throttling:</span></span>

1. <span data-ttu-id="af26f-193">A hello **Backup szolgáltatás ügynökének**, kattintson a **tulajdonságainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="af26f-193">In hello **Backup agent**, click **Change Properties**.</span></span>
2. <span data-ttu-id="af26f-194">Jelölje be hello **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="af26f-194">Select hello **Enable internet bandwidth usage throttling for backup operations** checkbox.</span></span>

    ![Hálózati sávszélesség-szabályozás](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. <span data-ttu-id="af26f-196">Miután engedélyezte a sávszélesség-szabályozás, adja meg a biztonsági mentési adatátvitel során sávszélesség engedélyezett hello **időpontokat a munkaidőhöz** és **munkaidőn kívüli**.</span><span class="sxs-lookup"><span data-stu-id="af26f-196">Once you have enabled throttling, specify hello allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="af26f-197">hello sávszélesség értékek kezdődjenek 512 kilobájt / másodperc (Kbps), és lépjen be too1023 megabájt / másodperc (Mbps).</span><span class="sxs-lookup"><span data-stu-id="af26f-197">hello bandwidth values begin at 512 kilobytes per second (Kbps) and can go up too1023 megabytes per second (Mbps).</span></span> <span data-ttu-id="af26f-198">Is kijelölése hello kezdő és a Befejezés **időpontokat a munkaidőhöz**, és hello hét mely napján minősülnek munkahelyi nap.</span><span class="sxs-lookup"><span data-stu-id="af26f-198">You can also designate hello start and finish for **Work hours**, and which days of hello week are considered Work days.</span></span> <span data-ttu-id="af26f-199">hello kívül időpontokat a munkaidőhöz kijelölt hello ideje figyelembe vett toobe munkaidőn kívüli órákra.</span><span class="sxs-lookup"><span data-stu-id="af26f-199">hello time outside of hello designated Work hours is considered toobe non-work hours.</span></span>
4. <span data-ttu-id="af26f-200">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="af26f-200">Click **OK**.</span></span>

## <a name="exclusion-settings"></a><span data-ttu-id="af26f-201">Kizárások beállításai</span><span class="sxs-lookup"><span data-stu-id="af26f-201">Exclusion settings</span></span>
1. <span data-ttu-id="af26f-202">Nyissa meg hello **Microsoft Azure Backup szolgáltatás ügynökének** (a gép kereséssel megtalálja *a Microsoft Azure Backup szolgáltatás*).</span><span class="sxs-lookup"><span data-stu-id="af26f-202">Open hello **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

    ![Nyissa meg a biztonságimásolat-készítő ügynökkel](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. <span data-ttu-id="af26f-204">A Microsoft Azure Backup szolgáltatás ügynökének hello kattintson **biztonsági mentés ütemezése**.</span><span class="sxs-lookup"><span data-stu-id="af26f-204">In hello Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. <span data-ttu-id="af26f-206">Az ütemezett biztonsági mentés varázsló hello hagyja hello **módosítása toobackup elemek vagy időpontok szerepelnek** jelölőnégyzetet és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="af26f-206">In hello Schedule Backup Wizard leave hello **Make changes toobackup items or times** option selected and click **Next**.</span></span>

    ![Ütemezésének módosítása](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. <span data-ttu-id="af26f-208">Kattintson a **kizárások beállításai**.</span><span class="sxs-lookup"><span data-stu-id="af26f-208">Click **Exclusions Settings**.</span></span>

    ![Válassza ki az elemek tooexclude](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. <span data-ttu-id="af26f-210">Kattintson a **adja hozzá a kizárási**.</span><span class="sxs-lookup"><span data-stu-id="af26f-210">Click **Add Exclusion**.</span></span>

    ![Kizárások hozzáadása](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. <span data-ttu-id="af26f-212">Adja meg hello helyét, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="af26f-212">Select hello location and then, click **OK**.</span></span>

    ![Válassza ki a kizárási helyét](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. <span data-ttu-id="af26f-214">Hello fájlkiterjesztés hozzáadása a hello **Fájltípus** mező.</span><span class="sxs-lookup"><span data-stu-id="af26f-214">Add hello file extension in hello **File Type** field.</span></span>

    ![Fájltípus kizárása](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    <span data-ttu-id="af26f-216">.Mp3 bővítmény felvétele</span><span class="sxs-lookup"><span data-stu-id="af26f-216">Adding an .mp3 extension</span></span>

    ![Példa a fájl típusa](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    <span data-ttu-id="af26f-218">tooadd egy másik kiterjesztést, kattintson a **Kizárás hozzáadása** , és adja meg egy másik típus fájlnévkiterjesztés (.jpeg-bővítmény hozzáadása).</span><span class="sxs-lookup"><span data-stu-id="af26f-218">tooadd another extension, click **Add Exclusion** and enter another file type extension (adding a .jpeg extension).</span></span>

    ![Egy másik példában a fájl típusa](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. <span data-ttu-id="af26f-220">Ha az összes hello bővítmény felvett, kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="af26f-220">When you've added all hello extensions, click **OK**.</span></span>
9. <span data-ttu-id="af26f-221">Ütemezett biztonsági mentés varázsló hello keresztül kattintva továbbra is **következő** amíg hello **visszaigazolási lapja**, kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="af26f-221">Continue through hello Schedule Backup Wizard by clicking **Next** until hello **Confirmation page**, then click **Finish**.</span></span>

    ![Kizárási megerősítése](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a><span data-ttu-id="af26f-223">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="af26f-223">Next steps</span></span>
* [<span data-ttu-id="af26f-224">Windows Server vagy a Windows ügyfél visszaállítása az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="af26f-224">Restore Windows Server or Windows Client from Azure</span></span>](backup-azure-restore-windows-server.md)
* <span data-ttu-id="af26f-225">További információ az Azure Backup szolgáltatásban toolearn lásd [Azure biztonsági mentés áttekintése](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="af26f-225">toolearn more about Azure Backup, see [Azure Backup Overview](backup-introduction-to-azure-backup.md)</span></span>
* <span data-ttu-id="af26f-226">A Microsoft hello [Azure biztonsági mentés fórum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span><span class="sxs-lookup"><span data-stu-id="af26f-226">Visit hello [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span></span>
