---
title: aaaUse Azure Backup server tooback be egy SharePoint-farm tooAzure |} Microsoft Docs
description: "Azure Backup Server tooback használja, és a SharePoint-adatok helyreállítása. A cikkben hello információk tooconfigure a SharePoint-farm, hogy a kívánt adatokat tárolhatja az Azure-ban. Védett SharePoint-adatok visszaállíthatja a lemezről, vagy az Azure-ból."
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
ms.assetid: 34ba87a4-91f1-4054-a4a1-272af1e15496
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 350c1ac0f3518f400062f3b586bbe9710a79915a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-sharepoint-farm-tooazure"></a><span data-ttu-id="a27a5-105">Készítsen biztonsági másolatot egy SharePoint-farm tooAzure</span><span class="sxs-lookup"><span data-stu-id="a27a5-105">Back up a SharePoint farm tooAzure</span></span>
<span data-ttu-id="a27a5-106">Biztonsági másolatot egy SharePoint-farm tooMicrosoft Azure mennyi hello a Microsoft Azure Backup Server (MABS) használatával biztonsági másolatot készíteni más adatforrások ugyanúgy.</span><span class="sxs-lookup"><span data-stu-id="a27a5-106">You back up a SharePoint farm tooMicrosoft Azure by using Microsoft Azure Backup Server (MABS) in much hello same way that you back up other data sources.</span></span> <span data-ttu-id="a27a5-107">Azure biztonsági mentés napi, heti, havi vagy éves biztonsági mentési pontok hello biztonsági mentés ütemezése toocreate rugalmasságot biztosít, és lehetővé teszi az adatmegőrzési házirend-beállítások a különféle biztonsági mentési pontok.</span><span class="sxs-lookup"><span data-stu-id="a27a5-107">Azure Backup provides flexibility in hello backup schedule toocreate daily, weekly, monthly, or yearly backup points and gives you retention policy options for various backup points.</span></span> <span data-ttu-id="a27a5-108">Gyors helyreállítási idő célkitűzés (RTO) hello funkció toostore helyi lemez másolatot is biztosít, és toostore másolja a gazdaságos, hosszú távú megőrzési tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a27a5-108">It also provides hello capability toostore local disk copies for quick recovery-time objectives (RTO) and toostore copies tooAzure for economical, long-term retention.</span></span>

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a><span data-ttu-id="a27a5-109">SharePoint támogatott verziója, és a kapcsolódó védelmi forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="a27a5-109">SharePoint supported versions and related protection scenarios</span></span>
<span data-ttu-id="a27a5-110">Azure biztonsági mentés a DPM támogatja a következő forgatókönyvek hello:</span><span class="sxs-lookup"><span data-stu-id="a27a5-110">Azure Backup for DPM supports hello following scenarios:</span></span>

| <span data-ttu-id="a27a5-111">Számítási feladat</span><span class="sxs-lookup"><span data-stu-id="a27a5-111">Workload</span></span> | <span data-ttu-id="a27a5-112">Verzió</span><span class="sxs-lookup"><span data-stu-id="a27a5-112">Version</span></span> | <span data-ttu-id="a27a5-113">A SharePoint központi telepítés</span><span class="sxs-lookup"><span data-stu-id="a27a5-113">SharePoint deployment</span></span> | <span data-ttu-id="a27a5-114">Védelem és helyreállítás</span><span class="sxs-lookup"><span data-stu-id="a27a5-114">Protection and recovery</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="a27a5-115">SharePoint</span><span class="sxs-lookup"><span data-stu-id="a27a5-115">SharePoint</span></span> |<span data-ttu-id="a27a5-116">A SharePoint 2013, SharePoint 2007, 2010 SharePoint SharePoint 3.0</span><span class="sxs-lookup"><span data-stu-id="a27a5-116">SharePoint 2013, SharePoint 2010, SharePoint 2007, SharePoint 3.0</span></span> |<span data-ttu-id="a27a5-117">A fizikai kiszolgálóként vagy Hyper-V vagy VMware virtuális gépeket telepített SharePoint</span><span class="sxs-lookup"><span data-stu-id="a27a5-117">SharePoint deployed as a physical server or Hyper-V/VMware virtual machine</span></span> <br> -------------- <br> <span data-ttu-id="a27a5-118">Az SQL AlwaysOn</span><span class="sxs-lookup"><span data-stu-id="a27a5-118">SQL AlwaysOn</span></span> | <span data-ttu-id="a27a5-119">Helyreállítási beállítások SharePoint-Farm védelme: a helyreállítási farm, adatbázis és a lemezes helyreállítási pontok a fájl vagy listaelem.</span><span class="sxs-lookup"><span data-stu-id="a27a5-119">Protect SharePoint Farm recovery options: Recovery farm, database, and file or list item from disk recovery points.</span></span>  <span data-ttu-id="a27a5-120">Adatbázis és a farm helyreállítási Azure helyreállítási pontokból.</span><span class="sxs-lookup"><span data-stu-id="a27a5-120">Farm and database recovery from Azure recovery points.</span></span> |

## <a name="before-you-start"></a><span data-ttu-id="a27a5-121">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="a27a5-121">Before you start</span></span>
<span data-ttu-id="a27a5-122">Néhány dolgot előtt készítsen biztonsági másolatot egy SharePoint-farm tooAzure tooconfirm kell.</span><span class="sxs-lookup"><span data-stu-id="a27a5-122">There are a few things you need tooconfirm before you back up a SharePoint farm tooAzure.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="a27a5-123">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a27a5-123">Prerequisites</span></span>
<span data-ttu-id="a27a5-124">Mielőtt továbblépne, győződjön meg arról, hogy rendelkezik [telepítve, és előkészített hello Azure Backup Server](backup-azure-microsoft-azure-backup.md) tooprotect munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="a27a5-124">Before you proceed, make sure that you have [installed and prepared hello Azure Backup Server](backup-azure-microsoft-azure-backup.md) tooprotect workloads.</span></span>

### <a name="protection-agent"></a><span data-ttu-id="a27a5-125">Védelmi ügynök</span><span class="sxs-lookup"><span data-stu-id="a27a5-125">Protection agent</span></span>
<span data-ttu-id="a27a5-126">hello védelmi ügynököt telepíteni kell a SharePoint, az SQL Server rendszert futtató kiszolgálók hello és egyéb hello SharePoint-farm részét képező kiszolgálók futtató kiszolgálón hello.</span><span class="sxs-lookup"><span data-stu-id="a27a5-126">hello Protection agent must be installed on hello server that's running SharePoint, hello servers that are running SQL Server, and all other servers that are part of hello SharePoint farm.</span></span> <span data-ttu-id="a27a5-127">További információ tooset be hello védelmi ügynököt, lásd: [telepítő védelmi ügynök](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).</span><span class="sxs-lookup"><span data-stu-id="a27a5-127">For more information about how tooset up hello protection agent, see [Setup Protection Agent](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).</span></span>  <span data-ttu-id="a27a5-128">hello egyetlen kivétel ez alól, hogy hello ügynököt telepít csak egy egyetlen (WFE) előtér-webkiszolgálóján.</span><span class="sxs-lookup"><span data-stu-id="a27a5-128">hello one exception is that you install hello agent only on a single web front end (WFE) server.</span></span> <span data-ttu-id="a27a5-129">DPM-nek egy előtér-Webkiszolgálón kiszolgáló csak tooserve hello ügynök védelemre hello belépési pontként.</span><span class="sxs-lookup"><span data-stu-id="a27a5-129">DPM needs hello agent on one WFE server only tooserve as hello entry point for protection.</span></span>

### <a name="sharepoint-farm"></a><span data-ttu-id="a27a5-130">SharePoint-farm</span><span class="sxs-lookup"><span data-stu-id="a27a5-130">SharePoint farm</span></span>
<span data-ttu-id="a27a5-131">A hello farm minden 10 millió eleme legalább 2 GB lemezterület hello köteten, ahol hello MABS mappa kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a27a5-131">For every 10 million items in hello farm, there must be at least 2 GB of space on hello volume where hello MABS folder is located.</span></span> <span data-ttu-id="a27a5-132">Ez a terület szükség a katalógus előállításához.</span><span class="sxs-lookup"><span data-stu-id="a27a5-132">This space is required for catalog generation.</span></span> <span data-ttu-id="a27a5-133">MABS toorecover elemeket (webhelycsoportok, webhelyek, listák, dokumentumtárak, mappák, egyes dokumentumok és listaelemek), a katalógus-előállítás hoz létre minden egyes tartalom-adatbázist belüli hello URL-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="a27a5-133">For MABS toorecover specific items (site collections, sites, lists, document libraries, folders, individual documents, and list items), catalog generation creates a list of hello URLs that are contained within each content database.</span></span> <span data-ttu-id="a27a5-134">Megtekintheti az URL-listák hello hello helyreállítható elem paneljén hello **helyreállítási** MABS felügyeleti konzol feladatterületén.</span><span class="sxs-lookup"><span data-stu-id="a27a5-134">You can view hello list of URLs in hello recoverable item pane in hello **Recovery** task area of MABS Administrator Console.</span></span>

### <a name="sql-server"></a><span data-ttu-id="a27a5-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a27a5-135">SQL Server</span></span>
<span data-ttu-id="a27a5-136">MABS rendszerfiókként fut.</span><span class="sxs-lookup"><span data-stu-id="a27a5-136">MABS runs as a LocalSystem account.</span></span> <span data-ttu-id="a27a5-137">SQL Server-adatbázisok tooback, MABS hello kiszolgáló SQL Server rendszert futtató fiók rendszergazdai jogosultságokkal szüksége van.</span><span class="sxs-lookup"><span data-stu-id="a27a5-137">tooback up SQL Server databases, MABS needs sysadmin privileges on that account for hello server that's running SQL Server.</span></span> <span data-ttu-id="a27a5-138">Állítsa be a NT AUTHORITY\SYSTEM túl*sysadmin* hello kiszolgálón, amelyen SQL Server előtt készítsen biztonsági másolatot.</span><span class="sxs-lookup"><span data-stu-id="a27a5-138">Set NT AUTHORITY\SYSTEM too*sysadmin* on hello server that's running SQL Server before you back it up.</span></span>

<span data-ttu-id="a27a5-139">Ha hello SharePoint-farm SQL Server-aliasokkal használt konfigurált SQL Server-adatbázisok, telepíthető hello előtér-webkiszolgálón, amely védeni fogja a MABS hello SQL Server ügyféloldali összetevőit.</span><span class="sxs-lookup"><span data-stu-id="a27a5-139">If hello SharePoint farm has SQL Server databases that are configured with SQL Server aliases, install hello SQL Server client components on hello front-end Web server that MABS will protect.</span></span>

### <a name="sharepoint-server"></a><span data-ttu-id="a27a5-140">SharePoint Server</span><span class="sxs-lookup"><span data-stu-id="a27a5-140">SharePoint Server</span></span>
<span data-ttu-id="a27a5-141">Amíg teljesítmény például a SharePoint-farm méret számos tényezőtől függ, általános útmutatásként egy MABS is 25 TB SharePoint-farm védelméhez.</span><span class="sxs-lookup"><span data-stu-id="a27a5-141">While performance depends on many factors such as size of SharePoint farm, as general guidance one MABS can protect a 25 TB SharePoint farm.</span></span>

### <a name="whats-not-supported"></a><span data-ttu-id="a27a5-142">Nem támogatott műveletek</span><span class="sxs-lookup"><span data-stu-id="a27a5-142">What's not supported</span></span>
* <span data-ttu-id="a27a5-143">Egy SharePoint-farm védelme MABS nem nyújt védelmet a keresési indexek vagy szolgáltatás adatbázisai.</span><span class="sxs-lookup"><span data-stu-id="a27a5-143">MABS that protects a SharePoint farm does not protect search indexes or application service databases.</span></span> <span data-ttu-id="a27a5-144">Ezek az adatbázisok védelméről tooconfigure hello külön-külön kell.</span><span class="sxs-lookup"><span data-stu-id="a27a5-144">You will need tooconfigure hello protection of these databases separately.</span></span>
* <span data-ttu-id="a27a5-145">MABS ad üzemeltetett SharePoint SQL Server-adatbázisok biztonsági mentése (SOFS) kibővíthető fájlkiszolgáló-megosztásokat.</span><span class="sxs-lookup"><span data-stu-id="a27a5-145">MABS does not provide backup of SharePoint SQL Server databases that are hosted on scale-out file server (SOFS) shares.</span></span>

## <a name="configure-sharepoint-protection"></a><span data-ttu-id="a27a5-146">SharePoint-védelem beállítása</span><span class="sxs-lookup"><span data-stu-id="a27a5-146">Configure SharePoint protection</span></span>
<span data-ttu-id="a27a5-147">MABS tooprotect SharePoint használata előtt konfigurálnia kell hello SharePoint VSS-író szolgáltatást (WSS-író szolgáltatás) használatával **ConfigureSharePoint.exe**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-147">Before you can use MABS tooprotect SharePoint, you must configure hello SharePoint VSS Writer service (WSS Writer service) by using **ConfigureSharePoint.exe**.</span></span>

<span data-ttu-id="a27a5-148">Található **ConfigureSharePoint.exe** hello [MABS telepítési elérési út] \bin mappában hello előtér-webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a27a5-148">You can find **ConfigureSharePoint.exe** in hello [MABS Installation Path]\bin folder on hello front-end web server.</span></span> <span data-ttu-id="a27a5-149">Ez az eszköz biztosít hello SharePoint-farm hello védelmi ügynök hello hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="a27a5-149">This tool provides hello protection agent with hello credentials for hello SharePoint farm.</span></span> <span data-ttu-id="a27a5-150">Futtatja egy WFE-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a27a5-150">You run it on a single WFE server.</span></span> <span data-ttu-id="a27a5-151">Ha több ELŐTÉR-webkiszolgáló, csak egyet válasszon ki egy védelmi csoport konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="a27a5-151">If you have multiple WFE servers, select just one when you configure a protection group.</span></span>

### <a name="tooconfigure-hello-sharepoint-vss-writer-service"></a><span data-ttu-id="a27a5-152">tooconfigure hello SharePoint VSS-író szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a27a5-152">tooconfigure hello SharePoint VSS Writer service</span></span>
1. <span data-ttu-id="a27a5-153">Hello ELŐTÉR-webkiszolgálón, a parancssorban lépjen túl [MABS telepítés helye] \bin\\</span><span class="sxs-lookup"><span data-stu-id="a27a5-153">On hello WFE server, at a command prompt, go too[MABS installation location]\bin\\</span></span>
2. <span data-ttu-id="a27a5-154">Adja meg a ConfigureSharePoint - EnableSharePointProtection.</span><span class="sxs-lookup"><span data-stu-id="a27a5-154">Enter ConfigureSharePoint -EnableSharePointProtection.</span></span>
3. <span data-ttu-id="a27a5-155">Adja meg a hello farm rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="a27a5-155">Enter hello farm administrator credentials.</span></span> <span data-ttu-id="a27a5-156">Ez a fiók tagja a helyi rendszergazdák csoportjának hello hello ELŐTÉR-webkiszolgálón kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a27a5-156">This account should be a member of hello local Administrator group on hello WFE server.</span></span> <span data-ttu-id="a27a5-157">Ha hello farm rendszergazdája nem egy helyi rendszergazda grant hello alábbi engedélyek hello ELŐTÉR-webkiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="a27a5-157">If hello farm administrator isn’t a local admin grant hello following permissions on hello WFE server:</span></span>
   * <span data-ttu-id="a27a5-158">Hello WSS_Admin_WPG csoportnak teljes hozzáférés biztosítása toohello DPM mappájához (% Program Files%\Microsoft Azure Backup\DPM).</span><span class="sxs-lookup"><span data-stu-id="a27a5-158">Grant hello WSS_Admin_WPG group full control toohello DPM folder (%Program Files%\Microsoft Azure Backup\DPM).</span></span>
   * <span data-ttu-id="a27a5-159">Adja meg a hello WSS_Admin_WPG csoportnak olvasási hozzáférés toohello DPM beállításkulcsot (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).</span><span class="sxs-lookup"><span data-stu-id="a27a5-159">Grant hello WSS_Admin_WPG group read access toohello DPM Registry key (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).</span></span>

> [!NOTE]
> <span data-ttu-id="a27a5-160">Toorerun ConfigureSharePoint.exe lesz szüksége, amikor megváltozik a hello SharePoint-farm rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="a27a5-160">You’ll need toorerun ConfigureSharePoint.exe whenever there’s a change in hello SharePoint farm administrator credentials.</span></span>
>
>

## <a name="back-up-a-sharepoint-farm-by-using-mabs"></a><span data-ttu-id="a27a5-161">Készítsen biztonsági másolatot egy SharePoint-farm MABS</span><span class="sxs-lookup"><span data-stu-id="a27a5-161">Back up a SharePoint farm by using MABS</span></span>
<span data-ttu-id="a27a5-162">Miután konfigurálta a MABS és hello korábban leírtak szerint SharePoint-farm, SharePoint védi őket MABS.</span><span class="sxs-lookup"><span data-stu-id="a27a5-162">After you have configured MABS and hello SharePoint farm as explained previously, SharePoint can be protected by MABS.</span></span>

### <a name="tooprotect-a-sharepoint-farm"></a><span data-ttu-id="a27a5-163">egy SharePoint-farm tooprotect</span><span class="sxs-lookup"><span data-stu-id="a27a5-163">tooprotect a SharePoint farm</span></span>
1. <span data-ttu-id="a27a5-164">A hello **védelmi** hello MABS felügyeleti konzol lapján kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-164">From hello **Protection** tab of hello MABS Administrator Console, click **New**.</span></span>
    <span data-ttu-id="a27a5-165">![Új védelem lap](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)</span><span class="sxs-lookup"><span data-stu-id="a27a5-165">![New Protection Tab](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)</span></span>
2. <span data-ttu-id="a27a5-166">A hello **védelmi csoport típusának kiválasztása** hello oldalán **új védelmi csoport létrehozása** varázsló, jelölje be **kiszolgálók**, és kattintson a **következő** .</span><span class="sxs-lookup"><span data-stu-id="a27a5-166">On hello **Select Protection Group Type** page of hello **Create New Protection Group** wizard, select **Servers**, and then click **Next**.</span></span>

    ![Válassza ki a védelmi csoport típusa](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. <span data-ttu-id="a27a5-168">A hello **csoporttagok kiválasztása** képernyő, jelölje be hello jelölőnégyzetet hello SharePoint server tooprotect ki, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-168">On hello **Select Group Members** screen, select hello check box for hello SharePoint server you want tooprotect and click **Next**.</span></span>

    ![Csoporttagok kiválasztása](./media/backup-azure-backup-sharepoint/select-group-members2.png)

   > [!NOTE]
   > <span data-ttu-id="a27a5-170">Hello védelmi ügynök telepítve lásd: hello server hello varázslóban.</span><span class="sxs-lookup"><span data-stu-id="a27a5-170">With hello protection agent installed, you can see hello server in hello wizard.</span></span> <span data-ttu-id="a27a5-171">MABS látható struktúrája.</span><span class="sxs-lookup"><span data-stu-id="a27a5-171">MABS also shows its structure.</span></span> <span data-ttu-id="a27a5-172">Mivel ConfigureSharePoint.exe futtatta, MABS hello SharePoint VSS-író szolgáltatás és a megfelelő SQL Server-adatbázisok kommunikál, és felismeri a SharePoint-farm struktúráját hello, hello tartozó megfelelő elemeit, valamint a tartalom-adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="a27a5-172">Because you ran ConfigureSharePoint.exe, MABS communicates with hello SharePoint VSS Writer service and its corresponding SQL Server databases and recognizes hello SharePoint farm structure, hello associated content databases, and any corresponding items.</span></span>
   >
   >
4. <span data-ttu-id="a27a5-173">A hello **adatvédelmi módszer kiválasztása** lapján adjon meg hello hello **védelmi csoport**, és válassza ki a kívánt *védelmi módszert*.</span><span class="sxs-lookup"><span data-stu-id="a27a5-173">On hello **Select Data Protection Method** page, enter hello name of hello **Protection Group**, and select your preferred *protection methods*.</span></span> <span data-ttu-id="a27a5-174">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a27a5-174">Click **Next**.</span></span>

    ![Adatvédelmi módszer kiválasztása](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)

   > [!NOTE]
   > <span data-ttu-id="a27a5-176">hello lemez védelmi módszert toomeet rövid helyreállítási idő célok segítségével.</span><span class="sxs-lookup"><span data-stu-id="a27a5-176">hello disk protection method helps toomeet short recovery-time objectives.</span></span>
   >
   >
5. <span data-ttu-id="a27a5-177">A hello **rövid távú célok megadása** lapon, válassza ki a kívánt **megőrzési időtartam** , és azonosíthatja, ha azt szeretné, hogy a biztonsági mentések toooccur.</span><span class="sxs-lookup"><span data-stu-id="a27a5-177">On hello **Specify Short-Term Goals** page, select your preferred **Retention range** and identify when you want backups toooccur.</span></span>

    ![Rövid távú célok megadása](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)

   > [!NOTE]
   > <span data-ttu-id="a27a5-179">Általában szükség, amely öt napnál régebbi adatok, mert jelenleg kijelölt lemezen öt napnyi megőrzési időtartamot, és biztosítani a biztonsági mentését végző hello történik, nem éles órában, ehhez a példához.</span><span class="sxs-lookup"><span data-stu-id="a27a5-179">Because recovery is most often required for data that's less than five days old, we selected a retention range of five days on disk and ensured that hello backup happens during non-production hours, for this example.</span></span>
   >
   >
6. <span data-ttu-id="a27a5-180">Tekintse át a hello tárolási tárolókészletben lefoglalt lemezterületet hello védelmi csoporthoz, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-180">Review hello storage pool disk space allocated for hello protection group, and click then **Next**.</span></span>
7. <span data-ttu-id="a27a5-181">Az összes védelmi csoportra MABS szabad terület toostore foglal le, és a replikák kezelése.</span><span class="sxs-lookup"><span data-stu-id="a27a5-181">For every protection group, MABS allocates disk space toostore and manage replicas.</span></span> <span data-ttu-id="a27a5-182">Ezen a ponton MABS hello kiválasztott adatok másolatának kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="a27a5-182">At this point, MABS must create a copy of hello selected data.</span></span> <span data-ttu-id="a27a5-183">Válassza ki, hogyan és mikor szeretne létrehozni hello replika, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-183">Select how and when you want hello replica created, and then click **Next**.</span></span>

    ![A replika-létrehozási módszer kiválasztása](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)

   > [!NOTE]
   > <span data-ttu-id="a27a5-185">toomake meg arról, hogy a hálózati forgalom nem történik meg, válassza ki a megfelelő üzemi a munkaidőn kívül.</span><span class="sxs-lookup"><span data-stu-id="a27a5-185">toomake sure that network traffic is not effected, select a time outside production hours.</span></span>
   >
   >
8. <span data-ttu-id="a27a5-186">MABS érdekében végezzen konzisztencia-ellenőrzést a replika hello az adatok integritásának biztosítja.</span><span class="sxs-lookup"><span data-stu-id="a27a5-186">MABS ensures data integrity by performing consistency checks on hello replica.</span></span> <span data-ttu-id="a27a5-187">Kétféleképpen érhető el.</span><span class="sxs-lookup"><span data-stu-id="a27a5-187">There are two available options.</span></span> <span data-ttu-id="a27a5-188">Megadhat egy ütemezést toorun konzisztencia-ellenőrzéseket, vagy a DPM is végezzen konzisztencia-ellenőrzést automatikusan hello replikán, amikor inkonzisztenssé válik.</span><span class="sxs-lookup"><span data-stu-id="a27a5-188">You can define a schedule toorun consistency checks, or DPM can run consistency checks automatically on hello replica whenever it becomes inconsistent.</span></span> <span data-ttu-id="a27a5-189">Válassza ki a kívánt beállítást, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-189">Select your preferred option, and then click **Next**.</span></span>

    ![Konzisztencia-ellenőrzés](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. <span data-ttu-id="a27a5-191">A hello **Online védelem adatainak megadása** lapon, válassza ki, hogy szeretné, hogy tooprotect, és kattintson a hello SharePoint-farm **következő**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-191">On hello **Specify Online Protection Data** page, select hello SharePoint farm that you want tooprotect, and then click **Next**.</span></span>

    ![A DPM SharePoint Protection1](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. <span data-ttu-id="a27a5-193">A hello **Online biztonsági mentési ütemezés megadása** lapon válassza ki a kívánt ütemezést, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-193">On hello **Specify Online Backup Schedule** page, select your preferred schedule, and then click **Next**.</span></span>

    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="a27a5-195">MABS két napi biztonsági mentések tooAzure a hello majd elérhető legfrissebb lemez biztonsági mentési pont legfeljebb biztosít.</span><span class="sxs-lookup"><span data-stu-id="a27a5-195">MABS provides a maximum of two daily backups tooAzure from hello then available latest disk backup point.</span></span> <span data-ttu-id="a27a5-196">Azure biztonsági mentés is meghatározhatja, alkalmas a biztonsági másolatok maximális és kevesen a WAN sávszélesség mennyiségét hello [Azure biztonsági mentési hálózati sávszélesség-szabályozás](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).</span><span class="sxs-lookup"><span data-stu-id="a27a5-196">Azure Backup can also control hello amount of WAN bandwidth that can be used for backups in peak and off-peak hours by using [Azure Backup Network Throttling](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).</span></span>
    >
    >
11. <span data-ttu-id="a27a5-197">Attól függően, hogy a biztonsági mentés ütemezése hello kiválasztott, hello **Online adatmegőrzési szabály megadása** lapon, jelölje be hello adatmegőrzési napi, heti, havi vagy éves biztonsági mentési pontok.</span><span class="sxs-lookup"><span data-stu-id="a27a5-197">Depending on hello backup schedule that you selected, on hello **Specify Online Retention Policy** page, select hello retention policy for daily, weekly, monthly, and yearly backup points.</span></span>

    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)

    > [!NOTE]
    > <span data-ttu-id="a27a5-199">MABS egy szerzett-édesapja-fia megőrzési séma, amelyben a különböző adatmegőrzési választható ki a másik biztonsági mentési pontok használja.</span><span class="sxs-lookup"><span data-stu-id="a27a5-199">MABS uses a grandfather-father-son retention scheme in which a different retention policy can be chosen for different backup points.</span></span>
    >
    >
12. <span data-ttu-id="a27a5-200">Hasonló toodisk kezdeti hivatkozás pont replikájának kell az Azure-ban létrehozott toobe.</span><span class="sxs-lookup"><span data-stu-id="a27a5-200">Similar toodisk, an initial reference point replica needs toobe created in Azure.</span></span> <span data-ttu-id="a27a5-201">Válassza ki a kívánt beállítást toocreate egy kezdeti biztonsági másolatot tooAzure, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-201">Select your preferred option toocreate an initial backup copy tooAzure, and then click **Next**.</span></span>

    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. <span data-ttu-id="a27a5-203">Tekintse át a beállításokat a hello **összegzés** lapon, majd **csoport létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-203">Review your selected settings on hello **Summary** page, and then click **Create Group**.</span></span> <span data-ttu-id="a27a5-204">Látni fogja a sikerről szóló üzenetet hello védelmi csoport létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="a27a5-204">You will see a success message after hello protection group has been created.</span></span>

    ![Összefoglalás](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-mabs"></a><span data-ttu-id="a27a5-206">Egy SharePoint-elem visszaállítása a lemezről MABS használatával</span><span class="sxs-lookup"><span data-stu-id="a27a5-206">Restore a SharePoint item from disk by using MABS</span></span>
<span data-ttu-id="a27a5-207">A következő példa hello, hello *helyreállítás SharePoint-elem* véletlenül törölve lett, és a helyreállított toobe kell.</span><span class="sxs-lookup"><span data-stu-id="a27a5-207">In hello following example, hello *Recovering SharePoint item* has been accidentally deleted and needs toobe recovered.</span></span>
<span data-ttu-id="a27a5-208">![MABS SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)</span><span class="sxs-lookup"><span data-stu-id="a27a5-208">![MABS SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)</span></span>

1. <span data-ttu-id="a27a5-209">Nyissa meg hello **DPM felügyeleti konzol**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-209">Open hello **DPM Administrator Console**.</span></span> <span data-ttu-id="a27a5-210">Minden DPM által védett SharePoint-farmok megjelennek-e hello **védelmi** fülre.</span><span class="sxs-lookup"><span data-stu-id="a27a5-210">All SharePoint farms that are protected by DPM are shown in hello **Protection** tab.</span></span>

    ![MABS SharePoint Protection3](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. <span data-ttu-id="a27a5-212">toobegin toorecover hello elem, jelölje be hello **helyreállítási** fülre.</span><span class="sxs-lookup"><span data-stu-id="a27a5-212">toobegin toorecover hello item, select hello **Recovery** tab.</span></span>

    ![MABS SharePoint Protection5](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. <span data-ttu-id="a27a5-214">A SharePoint kereshet *helyreállítás SharePoint-elem* kereséssel egy helyettesítő karakteres alapú belül egy helyreállítási pontot a tartományon.</span><span class="sxs-lookup"><span data-stu-id="a27a5-214">You can search SharePoint for *Recovering SharePoint item* by using a wildcard-based search within a recovery point range.</span></span>

    ![MABS SharePoint Protection6](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. <span data-ttu-id="a27a5-216">Válassza ki a megfelelő helyreállítási pont hello hello a keresési eredmények, kattintson a jobb gombbal a hello elemet, és válassza **helyreállítása**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-216">Select hello appropriate recovery point from hello search results, right-click hello item, and then select **Recover**.</span></span>
5. <span data-ttu-id="a27a5-217">Tallózzon a különböző helyreállítási pontok is, majd válassza ki az adatbázis vagy elem toorecover.</span><span class="sxs-lookup"><span data-stu-id="a27a5-217">You can also browse through various recovery points and select a database or item toorecover.</span></span> <span data-ttu-id="a27a5-218">Válassza ki **dátum > helyreállításkor**, majd válassza ki a megfelelő hello **adatbázis > SharePoint-farm > helyreállítási pont > elem**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-218">Select **Date > Recovery time**, and then select hello correct **Database > SharePoint farm > Recovery point > Item**.</span></span>

    ![MABS SharePoint Protection7](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. <span data-ttu-id="a27a5-220">Kattintson a jobb gombbal a hello elemet, majd válassza ki **helyreállítása** tooopen hello **helyreállítási varázsló**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-220">Right-click hello item, and then select **Recover** tooopen hello **Recovery Wizard**.</span></span> <span data-ttu-id="a27a5-221">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a27a5-221">Click **Next**.</span></span>

    ![Helyreállítási beállítások áttekintése](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. <span data-ttu-id="a27a5-223">Válassza ki, hogy szeretné, hogy tooperform, és kattintson a helyreállítási hello típusú **következő**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-223">Select hello type of recovery that you want tooperform, and then click **Next**.</span></span>

    ![Helyreállítási típus](./media/backup-azure-backup-sharepoint/select-recovery-type.png)

   > [!NOTE]
   > <span data-ttu-id="a27a5-225">a kijelölés hello **toooriginal helyreállítása** hello példa hello elem toohello eredeti SharePoint hely helyreállítására szolgál..</span><span class="sxs-lookup"><span data-stu-id="a27a5-225">hello selection of **Recover toooriginal** in hello example recovers hello item toohello original SharePoint site.</span></span>
   >
   >
8. <span data-ttu-id="a27a5-226">Jelölje be hello **helyreállítási folyamat** , amelyet az toouse.</span><span class="sxs-lookup"><span data-stu-id="a27a5-226">Select hello **Recovery Process** that you want toouse.</span></span>

   * <span data-ttu-id="a27a5-227">Válassza ki **helyreállítás helyreállítási farm nélkül** Ha hello SharePoint-farm nem változott, és megegyezik az hello helyreállítási pont, amely hello visszaállítása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="a27a5-227">Select **Recover without using a recovery farm** if hello SharePoint farm has not changed and is hello same as hello recovery point that is being restored.</span></span>
   * <span data-ttu-id="a27a5-228">Válassza ki **helyreállításához a helyreállítási farm** Ha hello SharePoint-farm megváltozott a hello helyreállítási pont létrehozása óta.</span><span class="sxs-lookup"><span data-stu-id="a27a5-228">Select **Recover using a recovery farm** if hello SharePoint farm has changed since hello recovery point was created.</span></span>

     ![A helyreállítási folyamat](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. <span data-ttu-id="a27a5-230">Adjon meg egy átmeneti SQL Server példány hely toorecover hello adatbázis átmenetileg, és adjon meg egy átmeneti fájlmegosztást MABS és hello futtató kiszolgáló esetében SharePoint toorecover hello elem a.</span><span class="sxs-lookup"><span data-stu-id="a27a5-230">Provide a staging SQL Server instance location toorecover hello database temporarily, and provide a staging file share on MABS and hello server that's running SharePoint toorecover hello item.</span></span>

    ![Átmeneti Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)

    <span data-ttu-id="a27a5-232">MABS hello tartalom-adatbázist, amelyen az hello SharePoint elem toohello ideiglenes SQL Server-példányhoz csatolja.</span><span class="sxs-lookup"><span data-stu-id="a27a5-232">MABS attaches hello content database that is hosting hello SharePoint item toohello temporary SQL Server instance.</span></span> <span data-ttu-id="a27a5-233">Hello tartalom-adatbázist akkor hello elem helyreállítása és átmeneti tárolási helye a MABS hello helyezi.</span><span class="sxs-lookup"><span data-stu-id="a27a5-233">From hello content database, it recovers hello item and puts it on hello staging file location on MABS.</span></span> <span data-ttu-id="a27a5-234">hello elem, amely a hello most átmeneti helyre kell exportált toobe toohello átmeneti helyre hello SharePoint-farm helyreállítása.</span><span class="sxs-lookup"><span data-stu-id="a27a5-234">hello recovered item that's on hello staging location now needs toobe exported toohello staging location on hello SharePoint farm.</span></span>

    ![Átmeneti Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. <span data-ttu-id="a27a5-236">Válassza ki **helyreállítási beállítások megadása**, és alkalmazza a biztonsági beállítások toohello SharePoint-farm vagy hello hello helyreállítási pont biztonsági beállításainak alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="a27a5-236">Select **Specify recovery options**, and apply security settings toohello SharePoint farm or apply hello security settings of hello recovery point.</span></span> <span data-ttu-id="a27a5-237">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a27a5-237">Click **Next**.</span></span>

    ![Helyreállítási beállítások](./media/backup-azure-backup-sharepoint/recovery-options.png)

    > [!NOTE]
    > <span data-ttu-id="a27a5-239">Kiválaszthatja a toothrottle hello sávszélesség-használat.</span><span class="sxs-lookup"><span data-stu-id="a27a5-239">You can choose toothrottle hello network bandwidth usage.</span></span> <span data-ttu-id="a27a5-240">Ez minimalizálja a hatás toohello az üzemi kiszolgáló éles órában.</span><span class="sxs-lookup"><span data-stu-id="a27a5-240">This minimizes impact toohello production server during production hours.</span></span>
    >
    >
11. <span data-ttu-id="a27a5-241">Tekintse át hello összefoglaló információit, és kattintson a **helyreállítása** toobegin helyreállítási hello fájl.</span><span class="sxs-lookup"><span data-stu-id="a27a5-241">Review hello summary information, and then click **Recover** toobegin recovery of hello file.</span></span>

    ![A helyreállítási összefoglaló](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. <span data-ttu-id="a27a5-243">Immár hello **figyelés** hello lapján **MABS felügyeleti konzol** tooview hello **állapot** hello helyreállítási.</span><span class="sxs-lookup"><span data-stu-id="a27a5-243">Now select hello **Monitoring** tab in hello **MABS Administrator Console** tooview hello **Status** of hello recovery.</span></span>

    ![Helyreállítási állapota](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)

    > [!NOTE]
    > <span data-ttu-id="a27a5-245">hello fájl most helyreáll.</span><span class="sxs-lookup"><span data-stu-id="a27a5-245">hello file is now restored.</span></span> <span data-ttu-id="a27a5-246">Hello SharePoint webhely toocheck hello visszaállított fájl is frissítheti.</span><span class="sxs-lookup"><span data-stu-id="a27a5-246">You can refresh hello SharePoint site toocheck hello restored file.</span></span>
    >
    >

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a><span data-ttu-id="a27a5-247">Egy SharePoint-adatbázis visszaállítása az Azure-ból a DPM használatával</span><span class="sxs-lookup"><span data-stu-id="a27a5-247">Restore a SharePoint database from Azure by using DPM</span></span>
1. <span data-ttu-id="a27a5-248">toorecover egy SharePoint tartalom-adatbázist, tallózzon a különböző helyreállítási pontokat (ahogy korábban), és válassza ki, hogy szeretné-e toorestore hello helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="a27a5-248">toorecover a SharePoint content database, browse through various recovery points (as shown previously), and select hello recovery point that you want toorestore.</span></span>

    ![MABS SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. <span data-ttu-id="a27a5-250">Kattintson duplán a hello SharePoint helyreállítási pont tooshow hello elérhető SharePoint katalógus adatait.</span><span class="sxs-lookup"><span data-stu-id="a27a5-250">Double-click hello SharePoint recovery point tooshow hello available SharePoint catalog information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a27a5-251">Hello SharePoint-farm hosszú távú megőrzési az Azure-ban a védett, mert a MABS nincs katalógus információkkal (metaadatokkal) érhető el.</span><span class="sxs-lookup"><span data-stu-id="a27a5-251">Because hello SharePoint farm is protected for long-term retention in Azure, no catalog information (metadata) is available on MABS.</span></span> <span data-ttu-id="a27a5-252">Ennek eredményeképpen időpontban a SharePoint tartalom-adatbázis helyreállítása toobe van szüksége, amikor szüksége toocatalog hello SharePoint-farm újra.</span><span class="sxs-lookup"><span data-stu-id="a27a5-252">As a result, whenever a point-in-time SharePoint content database needs toobe recovered, you need toocatalog hello SharePoint farm again.</span></span>
   >
   >
3. <span data-ttu-id="a27a5-253">Kattintson a **újrakatalogizáláshoz**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-253">Click **Re-catalog**.</span></span>

    ![MABS SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    <span data-ttu-id="a27a5-255">Hello **felhő Újrakatalogizálni** állapotkezelő ablak nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="a27a5-255">hello **Cloud Recatalog** status window opens.</span></span>

    ![MABS SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    <span data-ttu-id="a27a5-257">Katalogizálni befejezése után hello állapota túl*sikeres*.</span><span class="sxs-lookup"><span data-stu-id="a27a5-257">After cataloging is finished, hello status changes too*Success*.</span></span> <span data-ttu-id="a27a5-258">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a27a5-258">Click **Close**.</span></span>

    ![MABS SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. <span data-ttu-id="a27a5-260">Hello SharePoint objektum hello MABS látható **helyreállítási** tooget hello tartalom-adatbázist struktúra fülre.</span><span class="sxs-lookup"><span data-stu-id="a27a5-260">Click hello SharePoint object shown in hello MABS **Recovery** tab tooget hello content database structure.</span></span> <span data-ttu-id="a27a5-261">Kattintson a jobb gombbal a hello elemet, és kattintson **helyreállítása**.</span><span class="sxs-lookup"><span data-stu-id="a27a5-261">Right-click hello item, and then click **Recover**.</span></span>

    ![MABS SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. <span data-ttu-id="a27a5-263">Ezen a ponton, kövesse az hello [helyreállítási lépések az ebben a cikkben](#restore-a-sharepoint-item-from-disk-using-dpm) toorecover a lemezről egy SharePoint tartalmi adatbázist.</span><span class="sxs-lookup"><span data-stu-id="a27a5-263">At this point, follow hello [recovery steps earlier in this article](#restore-a-sharepoint-item-from-disk-using-dpm) toorecover a SharePoint content database from disk.</span></span>

## <a name="faqs"></a><span data-ttu-id="a27a5-264">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="a27a5-264">FAQs</span></span>
<span data-ttu-id="a27a5-265">K: helyreállíthatók egy SharePoint elem toohello eredeti helyre, ha SharePoint (a védelem a lemezen) az SQL AlwaysOn használatára van konfigurálva?</span><span class="sxs-lookup"><span data-stu-id="a27a5-265">Q: Can I recover a SharePoint item toohello original location if SharePoint is configured by using SQL AlwaysOn (with protection on disk)?</span></span><br>
<span data-ttu-id="a27a5-266">A: Igen hello elem lehet helyreállított toohello eredeti SharePoint-webhelyen.</span><span class="sxs-lookup"><span data-stu-id="a27a5-266">A: Yes, hello item can be recovered toohello original SharePoint site.</span></span>

<span data-ttu-id="a27a5-267">K: helyreállíthatók egy SharePoint adatbázis toohello eredeti helyre, ha a SharePoint SQL AlwaysOn használatára van konfigurálva?</span><span class="sxs-lookup"><span data-stu-id="a27a5-267">Q: Can I recover a SharePoint database toohello original location if SharePoint is configured by using SQL AlwaysOn?</span></span><br>
<span data-ttu-id="a27a5-268">V:, mert a SharePoint-adatbázisok az SQL AlwaysOn vannak konfigurálva, akkor csak módosíthatók hello rendelkezésre állási csoport eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="a27a5-268">A: Because SharePoint databases are configured in SQL AlwaysOn, they cannot be modified unless hello availability group is removed.</span></span> <span data-ttu-id="a27a5-269">Ennek eredményeképpen MABS nem állítható vissza egy adatbázis toohello eredeti helyre.</span><span class="sxs-lookup"><span data-stu-id="a27a5-269">As a result, MABS cannot restore a database toohello original location.</span></span> <span data-ttu-id="a27a5-270">Helyreállíthatja az SQL Server adatbázis tooanother SQL Server-példányt.</span><span class="sxs-lookup"><span data-stu-id="a27a5-270">You can recover a SQL Server database tooanother SQL Server instance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a27a5-271">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a27a5-271">Next steps</span></span>
* <span data-ttu-id="a27a5-272">További tudnivalók a SharePoint védelme MABS – lásd [videó sorozat - a SharePoint védelme a DPM](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)</span><span class="sxs-lookup"><span data-stu-id="a27a5-272">Learn more about MABS Protection of SharePoint - see [Video Series - DPM Protection of SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)</span></span>
