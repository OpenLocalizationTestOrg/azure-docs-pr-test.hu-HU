---
title: "Azure mentési tárolókban, és a kiszolgálók Azure-ban a klasszikus üzembe helyezési modellel kezelése |} Microsoft Docs"
description: "Ez az oktatóanyag segítségével megtudhatja, hogyan Azure mentési tárolókban és kiszolgálók kezelésére."
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
ms.openlocfilehash: 91451b2cdc42ed05ef7c1ba9c66ad5b4b45dd788
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-backup-vaults-and-servers-using-the-classic-deployment-model"></a><span data-ttu-id="da070-103">Azure Backup-tárolók és -kiszolgálók konfigurálása a klasszikus üzemi modellel</span><span class="sxs-lookup"><span data-stu-id="da070-103">Manage Azure Backup vaults and servers using the classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="da070-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="da070-104">Resource Manager</span></span>](backup-azure-manage-windows-server.md)
> * [<span data-ttu-id="da070-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="da070-105">Classic</span></span>](backup-azure-manage-windows-server-classic.md)
>
>

<span data-ttu-id="da070-106">Ebben a cikkben a biztonságimásolat-felügyeleti feladatokat a klasszikus Azure portál és a Microsoft Azure Backup szolgáltatás ügynöke egy áttekintést talál.</span><span class="sxs-lookup"><span data-stu-id="da070-106">In this article you'll find an overview of the backup management tasks available through the Azure classic portal and the Microsoft Azure Backup agent.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="da070-107">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="da070-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="da070-108">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="da070-108">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="da070-109">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="da070-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="da070-110">A biztonsági mentési tárolókról mostantól lehetőség van Recovery Services-tárolókra váltani.</span><span class="sxs-lookup"><span data-stu-id="da070-110">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="da070-111">A részletekről bővebben az [Váltás biztonsági mentési tárolóról Recovery Services-tárolóra](backup-azure-upgrade-backup-to-recovery-services.md) című cikkben olvashat.</span><span class="sxs-lookup"><span data-stu-id="da070-111">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="da070-112">A Microsoft azt javasolja, hogy a biztonsági mentési tárolóról váltson Recovery Services-tárolóra.</span><span class="sxs-lookup"><span data-stu-id="da070-112">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="da070-113">2017. október 15-től a PowerShell nem használható Backup-tárolók létrehozására.</span><span class="sxs-lookup"><span data-stu-id="da070-113">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="da070-114">**2017. november 1-től**:</span><span class="sxs-lookup"><span data-stu-id="da070-114">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="da070-115">Minden fennmaradó Backup-tároló automatikusan Recovery Services-tárolóra frissül.</span><span class="sxs-lookup"><span data-stu-id="da070-115">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="da070-116">A klasszikus portálon nem lehet majd hozzáférni a biztonsági másolati adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="da070-116">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="da070-117">Helyette az Azure Portal segítségével férhet hozzá a Recovery Services-tárolókban található biztonsági mentési adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="da070-117">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="management-portal-tasks"></a><span data-ttu-id="da070-118">Kezelési portál feladatok</span><span class="sxs-lookup"><span data-stu-id="da070-118">Management portal tasks</span></span>
1. <span data-ttu-id="da070-119">Jelentkezzen be a [kezelési portál](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="da070-119">Sign in to the [Management Portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="da070-120">Kattintson a **Recovery Services**, majd kattintson a gyors kezdés lapon megtekintheti a mentési tároló nevére.</span><span class="sxs-lookup"><span data-stu-id="da070-120">Click **Recovery Services**, then click the name of backup vault to view the Quick Start page.</span></span>

    ![Helyreállítási szolgáltatások](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

<span data-ttu-id="da070-122">Az első lépések lap tetején beállítások kiválasztását, megtekintheti az elérhető felügyeleti feladatait.</span><span class="sxs-lookup"><span data-stu-id="da070-122">By selecting the options at the top of the Quick Start page, you can see the available management tasks.</span></span>

![Lapok kezelése](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a><span data-ttu-id="da070-124">Irányítópult</span><span class="sxs-lookup"><span data-stu-id="da070-124">Dashboard</span></span>
<span data-ttu-id="da070-125">Válassza ki **irányítópult** a használati áttekintésben a kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="da070-125">Select **Dashboard** to see the usage overview for the server.</span></span> <span data-ttu-id="da070-126">A **használatának áttekintése** tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="da070-126">The **usage overview** includes:</span></span>

* <span data-ttu-id="da070-127">A felhőbe regisztrált Windows-kiszolgálók száma</span><span class="sxs-lookup"><span data-stu-id="da070-127">The number of Windows Servers registered to cloud</span></span>
* <span data-ttu-id="da070-128">A védett felhőben található Azure virtuális gépek száma</span><span class="sxs-lookup"><span data-stu-id="da070-128">The number of Azure virtual machines protected in cloud</span></span>
* <span data-ttu-id="da070-129">A teljes Azure-ban felhasznált tárterület</span><span class="sxs-lookup"><span data-stu-id="da070-129">The total storage consumed in Azure</span></span>
* <span data-ttu-id="da070-130">A legutóbbi feladatok állapota</span><span class="sxs-lookup"><span data-stu-id="da070-130">The status of recent jobs</span></span>

<span data-ttu-id="da070-131">Az irányítópult alján a következő feladatokat végezheti el:</span><span class="sxs-lookup"><span data-stu-id="da070-131">At the bottom of the Dashboard you can perform the following tasks:</span></span>

* <span data-ttu-id="da070-132">**A tanúsítványnak a** – Ha a tanúsítvány használatával regisztrálja a kiszolgálót, majd használja ezt a tanúsítványt frissíteni.</span><span class="sxs-lookup"><span data-stu-id="da070-132">**Manage certificate** - If a certificate was used to register the server, then use this to update the certificate.</span></span> <span data-ttu-id="da070-133">Ha a tárolói hitelesítő adatokat használ, ne használjon **szükséges tanúsítvány kezelése**.</span><span class="sxs-lookup"><span data-stu-id="da070-133">If you are using vault credentials, do not use **Manage certificate**.</span></span>
* <span data-ttu-id="da070-134">**Törlés** -törli a jelenlegi biztonsági mentési tárolóba.</span><span class="sxs-lookup"><span data-stu-id="da070-134">**Delete** - Deletes the current backup vault.</span></span> <span data-ttu-id="da070-135">Ha a mentési tárolóban már használatban van, úgy, hogy szabadítson fel helyet a tárolási törölheti.</span><span class="sxs-lookup"><span data-stu-id="da070-135">If a backup vault is no longer being used, you can delete it to free up storage space.</span></span> <span data-ttu-id="da070-136">**Törlés** csak akkor engedélyezett, miután az összes regisztrált kiszolgálókat, hogy törölték a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="da070-136">**Delete** is only enabled after all registered servers have been deleted from the vault.</span></span>

![Biztonsági mentési irányítópult feladatok](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a><span data-ttu-id="da070-138">Regisztrált cikkek</span><span class="sxs-lookup"><span data-stu-id="da070-138">Registered items</span></span>
<span data-ttu-id="da070-139">Válassza ki **regisztrált elemek** ehhez a tárolóhoz regisztrált kiszolgáló nevének megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="da070-139">Select **Registered Items** to view the names of the servers that are registered to this vault.</span></span>

![Regisztrált cikkek](./media/backup-azure-manage-windows-server-classic/registered-items.png)

<span data-ttu-id="da070-141">A **típus** szűrés alapértelmezett Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="da070-141">The **Type** filter defaults to Azure Virtual Machine.</span></span> <span data-ttu-id="da070-142">Válassza ki, ha ehhez a tárolóhoz regisztrált kiszolgálók nevei, **Windows server** a legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="da070-142">To view the names of the servers that are registered to this vault, select **Windows server** from the drop down menu.</span></span>

<span data-ttu-id="da070-143">Itt a következő feladatokat végezheti el:</span><span class="sxs-lookup"><span data-stu-id="da070-143">From here you can perform the following tasks:</span></span>

* <span data-ttu-id="da070-144">**Az Újraregisztrálás engedélyezése** – Ha ezt a beállítást a kiszolgáló is használhatja a **regisztrációs varázsló** a helyszíni Microsoft Azure Backup-ügynök regisztrálni a kiszolgálót a biztonsági mentési tároló még egyszer.</span><span class="sxs-lookup"><span data-stu-id="da070-144">**Allow Re-registration** - When this option is selected for a server you can use the **Registration Wizard** in the on-premises Microsoft Azure Backup agent to register the server with the backup vault a second time.</span></span> <span data-ttu-id="da070-145">Akkor lehet, hogy újra kell regisztrálnia a tanúsítványt a bekövetkezett hiba miatt, vagy ha egy kiszolgáló építhető újra kellett.</span><span class="sxs-lookup"><span data-stu-id="da070-145">You might need to re-register due to an error in the certificate or if a server had to be rebuilt.</span></span>
* <span data-ttu-id="da070-146">**Törlés** -kiszolgáló törlése a biztonsági mentési tárolóból.</span><span class="sxs-lookup"><span data-stu-id="da070-146">**Delete** - Deletes a server from the backup vault.</span></span> <span data-ttu-id="da070-147">A tárolt adatok a kiszolgálóhoz tartozó összes azonnal törli.</span><span class="sxs-lookup"><span data-stu-id="da070-147">All of the stored data associated with the server is deleted immediately.</span></span>

    ![Regisztrált konfigurációelemekkel kapcsolatos feladatokhoz](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a><span data-ttu-id="da070-149">Védett elemek</span><span class="sxs-lookup"><span data-stu-id="da070-149">Protected items</span></span>
<span data-ttu-id="da070-150">Válassza ki **védett elemek** mentett a kiszolgálók elemek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="da070-150">Select **Protected Items** to view the items that have been backed up from the servers.</span></span>

![Védett elemek](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a><span data-ttu-id="da070-152">Konfigurálás</span><span class="sxs-lookup"><span data-stu-id="da070-152">Configure</span></span>
<span data-ttu-id="da070-153">Az a **konfigurálása** lapon kiválaszthatja a megfelelő tárolási redundancia beállítást.</span><span class="sxs-lookup"><span data-stu-id="da070-153">From the **Configure** tab you can select the appropriate storage redundancy option.</span></span> <span data-ttu-id="da070-154">Válassza ki a tárolási redundancia beállítást érdemes megfelelő, egy tároló létrehozása után, és mielőtt a számítógépek van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="da070-154">The best time to select the storage redundancy option is right after creating a vault and before any machines are registered to it.</span></span>

> [!WARNING]
> <span data-ttu-id="da070-155">Amint egy elem van regisztrálva a tárolóba, a redundancia tárolómegoldást zárolva van, ezért nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="da070-155">Once an item has been registered to the vault, the storage redundancy option is locked and cannot be modified.</span></span>
>
>

![Konfigurálás](./media/backup-azure-manage-windows-server-classic/configure.png)

<span data-ttu-id="da070-157">További információ a jelen cikk tudnivalók [adattároló redundanciája, amely](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="da070-157">See this article for more information about [storage redundancy](../storage/common/storage-redundancy.md).</span></span>

## <a name="microsoft-azure-backup-agent-tasks"></a><span data-ttu-id="da070-158">A Microsoft Azure Backup szolgáltatás ügynöke feladatok</span><span class="sxs-lookup"><span data-stu-id="da070-158">Microsoft Azure Backup agent tasks</span></span>
### <a name="console"></a><span data-ttu-id="da070-159">Konzol</span><span class="sxs-lookup"><span data-stu-id="da070-159">Console</span></span>
<span data-ttu-id="da070-160">Nyissa meg a **Microsoft Azure Backup szolgáltatás ügynökének** (a gép kereséssel megtalálja *a Microsoft Azure Backup szolgáltatás*).</span><span class="sxs-lookup"><span data-stu-id="da070-160">Open the **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

![Biztonságimásolat-készítő ügynökkel](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

<span data-ttu-id="da070-162">Az a **műveletek** érhető el a biztonságimásolat-készítő ügynök konzol jobb végezheti el az alábbi felügyeleti feladatokat:</span><span class="sxs-lookup"><span data-stu-id="da070-162">From the **Actions** available at the right of the backup agent console you can perform the following management tasks:</span></span>

* <span data-ttu-id="da070-163">Kiszolgáló regisztrálása</span><span class="sxs-lookup"><span data-stu-id="da070-163">Register Server</span></span>
* <span data-ttu-id="da070-164">Biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="da070-164">Schedule Backup</span></span>
* <span data-ttu-id="da070-165">Biztonsági másolat készítése</span><span class="sxs-lookup"><span data-stu-id="da070-165">Back Up now</span></span>
* <span data-ttu-id="da070-166">Tulajdonságainak módosítása</span><span class="sxs-lookup"><span data-stu-id="da070-166">Change Properties</span></span>

![Ügynök konzol műveletek](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> <span data-ttu-id="da070-168">A **adatok helyreállítása**, lásd: [fájlokat állíthatja vissza a Windows server vagy a Windows-ügyfélszámítógép](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="da070-168">To **Recover Data**, see [Restore files to a Windows server or Windows client machine](backup-azure-restore-windows-server.md).</span></span>
>
>

### <a name="modify-an-existing-backup"></a><span data-ttu-id="da070-169">Módosítsa a meglévő biztonsági mentésből</span><span class="sxs-lookup"><span data-stu-id="da070-169">Modify an existing backup</span></span>
1. <span data-ttu-id="da070-170">Kattintson a Microsoft Azure Backup szolgáltatás ügynökének **biztonsági mentés ütemezése**.</span><span class="sxs-lookup"><span data-stu-id="da070-170">In the Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. <span data-ttu-id="da070-172">Az a **ütemezett biztonsági mentés varázsló** hagyja a **biztonsági másolati elemei vagy időpontok szerepelnek módosítása** jelölőnégyzetet és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="da070-172">In the **Schedule Backup Wizard** leave the **Make changes to backup items or times** option selected and click **Next**.</span></span>

    ![Ütemezett biztonsági mentés módosítása](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. <span data-ttu-id="da070-174">Ha szeretné-e hozzáadása vagy módosítása elemek, a a **elemek kijelölése biztonsági mentéshez** képernyőn kattintson **elemek hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="da070-174">If you want to add or change items, on the **Select Items to Backup** screen click **Add Items**.</span></span>

    <span data-ttu-id="da070-175">Azt is beállíthatja **kizárások beállításai** ezen a lapon, a varázslóban.</span><span class="sxs-lookup"><span data-stu-id="da070-175">You can also set **Exclusion Settings** from this page in the wizard.</span></span> <span data-ttu-id="da070-176">Ha ki szeretné zárni a fájlok vagy fájltípusok olvassa el, hogyan adhat hozzá [kizárások beállításai](#exclusion-settings).</span><span class="sxs-lookup"><span data-stu-id="da070-176">If you want to exclude files or file types read the procedure for adding [exclusion settings](#exclusion-settings).</span></span>
4. <span data-ttu-id="da070-177">Válassza ki a fájlok és mappák biztonsági mentése, és kattintson a kívánt **gépházban**.</span><span class="sxs-lookup"><span data-stu-id="da070-177">Select the files and folders you want to back up and click **Okay**.</span></span>

    ![Elemek hozzáadása](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. <span data-ttu-id="da070-179">Adja meg a **biztonsági mentés ütemezése** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="da070-179">Specify the **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="da070-180">(A 3-szor naponkénti maximum) napi vagy heti biztonsági mentései is ütemezheti.</span><span class="sxs-lookup"><span data-stu-id="da070-180">You can schedule daily (at a maximum of 3 times per day) or weekly backups.</span></span>

    ![Adja meg a biztonsági mentés ütemezését](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > <span data-ttu-id="da070-182">Adja meg a biztonsági mentés ütemezése esetén, tekintse meg a részletes [cikk](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="da070-182">Specifying the backup schedule is explained in detail in this [article](backup-azure-backup-cloud-as-tape.md).</span></span>
   >
   >
6. <span data-ttu-id="da070-183">Válassza ki a **adatmegőrzési** a biztonsági másolatot, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="da070-183">Select the **Retention Policy** for the backup copy and click **Next**.</span></span>

    ![Válassza ki az adatmegőrzési házirend](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. <span data-ttu-id="da070-185">Az a **megerősítő** képernyőn tekintse át az adatokat, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="da070-185">On the **Confirmation** screen review the information and click **Finish**.</span></span>
8. <span data-ttu-id="da070-186">Miután a varázsló befejezi a **biztonsági mentés ütemezése**, kattintson a **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="da070-186">Once the wizard finishes creating the **backup schedule**, click **Close**.</span></span>

    <span data-ttu-id="da070-187">Védelmi módosítása, után ellenőrizheti, hogy biztonsági mentést a megfelelő váltanak a **feladatok** lapra, és erősítse meg, hogy a módosítások megjelennek a biztonsági mentési feladatok.</span><span class="sxs-lookup"><span data-stu-id="da070-187">After modifying protection, you can confirm that backups are triggering correctly by going to the **Jobs** tab and confirming that changes are reflected in the backup jobs.</span></span>

### <a name="enable-network-throttling"></a><span data-ttu-id="da070-188">Hálózati sávszélesség-szabályozás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="da070-188">Enable Network Throttling</span></span>
<span data-ttu-id="da070-189">Az Azure Backup szolgáltatás ügynökének biztosítja a sávszélesség-szabályozási fülre, amely lehetővé teszi, hogy vezérlését a hálózati sávszélesség használatának adatátvitel során.</span><span class="sxs-lookup"><span data-stu-id="da070-189">The Azure Backup agent provides a Throttling tab which allows you to control how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="da070-190">Ez a vezérlő akkor lehet hasznos, ha biztonsági kell során az adatokat munkaidő, de nem szeretné a biztonsági mentési folyamat zavarja a más internetes forgalmat.</span><span class="sxs-lookup"><span data-stu-id="da070-190">This control can be helpful if you need to back up data during work hours but do not want the backup process to interfere with other internet traffic.</span></span> <span data-ttu-id="da070-191">Átviteli biztonsági mentése és visszaállítása a tevékenységek adatok szabályozás vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="da070-191">Throttling of data transfer applies to back up and restore activities.</span></span>  

<span data-ttu-id="da070-192">Elemre a szabályozás engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="da070-192">To enable throttling:</span></span>

1. <span data-ttu-id="da070-193">Az a **Backup szolgáltatás ügynökének**, kattintson a **tulajdonságainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="da070-193">In the **Backup agent**, click **Change Properties**.</span></span>
2. <span data-ttu-id="da070-194">Válassza ki a **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="da070-194">Select the **Enable internet bandwidth usage throttling for backup operations** checkbox.</span></span>

    ![Hálózati sávszélesség-szabályozás](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. <span data-ttu-id="da070-196">Miután engedélyezte a sávszélesség-szabályozás, adja meg a megengedett sávszélesség vonatkozó biztonsági mentési adatátvitel során **időpontokat a munkaidőhöz** és **munkaidőn kívüli**.</span><span class="sxs-lookup"><span data-stu-id="da070-196">Once you have enabled throttling, specify the allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="da070-197">A sávszélesség értékek kezdődjenek 512 kilobájt / másodperc (Kbps), és folytathatja a legfeljebb 1023 megabájt / másodperc (Mbps).</span><span class="sxs-lookup"><span data-stu-id="da070-197">The bandwidth values begin at 512 kilobytes per second (Kbps) and can go up to 1023 megabytes per second (Mbps).</span></span> <span data-ttu-id="da070-198">Is kijelölni a kezdő és a Befejezés **időpontokat a munkaidőhöz**, és a hét melyik napjain minősülnek munkahelyi nap.</span><span class="sxs-lookup"><span data-stu-id="da070-198">You can also designate the start and finish for **Work hours**, and which days of the week are considered Work days.</span></span> <span data-ttu-id="da070-199">A megadott munkaidőn kívül idő tekinthető munkaidőn kívüli időszakra.</span><span class="sxs-lookup"><span data-stu-id="da070-199">The time outside of the designated Work hours is considered to be non-work hours.</span></span>
4. <span data-ttu-id="da070-200">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="da070-200">Click **OK**.</span></span>

## <a name="exclusion-settings"></a><span data-ttu-id="da070-201">Kizárások beállításai</span><span class="sxs-lookup"><span data-stu-id="da070-201">Exclusion settings</span></span>
1. <span data-ttu-id="da070-202">Nyissa meg a **Microsoft Azure Backup szolgáltatás ügynökének** (a gép kereséssel megtalálja *a Microsoft Azure Backup szolgáltatás*).</span><span class="sxs-lookup"><span data-stu-id="da070-202">Open the **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

    ![Nyissa meg a biztonságimásolat-készítő ügynökkel](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. <span data-ttu-id="da070-204">Kattintson a Microsoft Azure Backup szolgáltatás ügynökének **biztonsági mentés ütemezése**.</span><span class="sxs-lookup"><span data-stu-id="da070-204">In the Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. <span data-ttu-id="da070-206">A biztonsági mentés ütemezése varázsló hagyja a a **biztonsági másolati elemei vagy időpontok szerepelnek módosítása** jelölőnégyzetet és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="da070-206">In the Schedule Backup Wizard leave the **Make changes to backup items or times** option selected and click **Next**.</span></span>

    ![Ütemezésének módosítása](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. <span data-ttu-id="da070-208">Kattintson a **kizárások beállításai**.</span><span class="sxs-lookup"><span data-stu-id="da070-208">Click **Exclusions Settings**.</span></span>

    ![Jelölje ki az elemet kizárása](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. <span data-ttu-id="da070-210">Kattintson a **adja hozzá a kizárási**.</span><span class="sxs-lookup"><span data-stu-id="da070-210">Click **Add Exclusion**.</span></span>

    ![Kizárások hozzáadása](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. <span data-ttu-id="da070-212">Válassza ki azt a helyet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="da070-212">Select the location and then, click **OK**.</span></span>

    ![Válassza ki a kizárási helyét](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. <span data-ttu-id="da070-214">Adja hozzá a fájl kiterjesztése a **Fájltípus** mező.</span><span class="sxs-lookup"><span data-stu-id="da070-214">Add the file extension in the **File Type** field.</span></span>

    ![Fájltípus kizárása](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    <span data-ttu-id="da070-216">.Mp3 bővítmény felvétele</span><span class="sxs-lookup"><span data-stu-id="da070-216">Adding an .mp3 extension</span></span>

    ![Példa a fájl típusa](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    <span data-ttu-id="da070-218">Egy másik bővítmény hozzáadásához kattintson **Kizárás hozzáadása** , és adja meg egy másik típus fájlnévkiterjesztés (.jpeg-bővítmény hozzáadása).</span><span class="sxs-lookup"><span data-stu-id="da070-218">To add another extension, click **Add Exclusion** and enter another file type extension (adding a .jpeg extension).</span></span>

    ![Egy másik példában a fájl típusa](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. <span data-ttu-id="da070-220">Ha a bővítményeket adott hozzá, kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="da070-220">When you've added all the extensions, click **OK**.</span></span>
9. <span data-ttu-id="da070-221">A biztonsági mentés ütemezése varázsló segítségével kattintva folytatni **következő** mindaddig, amíg a **visszaigazolási lapja**, kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="da070-221">Continue through the Schedule Backup Wizard by clicking **Next** until the **Confirmation page**, then click **Finish**.</span></span>

    ![Kizárási megerősítése](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a><span data-ttu-id="da070-223">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="da070-223">Next steps</span></span>
* [<span data-ttu-id="da070-224">Windows Server vagy a Windows ügyfél visszaállítása az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="da070-224">Restore Windows Server or Windows Client from Azure</span></span>](backup-azure-restore-windows-server.md)
* <span data-ttu-id="da070-225">Azure biztonsági mentés kapcsolatos további információkért lásd: [Azure biztonsági mentés áttekintése](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="da070-225">To learn more about Azure Backup, see [Azure Backup Overview](backup-introduction-to-azure-backup.md)</span></span>
* <span data-ttu-id="da070-226">Látogasson el a [Azure biztonsági mentési fórum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span><span class="sxs-lookup"><span data-stu-id="da070-226">Visit the [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span></span>
