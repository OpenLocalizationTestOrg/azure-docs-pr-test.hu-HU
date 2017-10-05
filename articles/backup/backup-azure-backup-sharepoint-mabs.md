---
title: "Azure Backup-kiszolgáló SharePoint-farm mentésére az Azure-bA |} Microsoft Docs"
description: "Azure Backup Server használatával készítsen biztonsági másolatot, és a SharePoint-adatok helyreállítása. A cikkben az adatokat a SharePoint-farm konfigurálásához, hogy a kívánt adatokat tárolhatja az Azure-ban. Védett SharePoint-adatok visszaállíthatja a lemezről, vagy az Azure-ból."
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
ms.openlocfilehash: 3ed000affd326eb1bd7c99773ec021ad6e03cc3b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-a-sharepoint-farm-to-azure"></a><span data-ttu-id="34e52-105">SharePoint-farm biztonsági mentése az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="34e52-105">Back up a SharePoint farm to Azure</span></span>
<span data-ttu-id="34e52-106">Akkor készítsen biztonsági másolatot egy SharePoint-farm Microsoft Azure hasonlóan, amely a biztonsági mentést más adatforrások a Microsoft Azure Backup Server (MABS) használatával.</span><span class="sxs-lookup"><span data-stu-id="34e52-106">You back up a SharePoint farm to Microsoft Azure by using Microsoft Azure Backup Server (MABS) in much the same way that you back up other data sources.</span></span> <span data-ttu-id="34e52-107">Azure biztonsági mentés naponta létrehozásához a biztonsági mentés ütemezése rugalmasságot biztosít, heti, havi vagy éves biztonsági mentést mutat, és lehetővé teszi az adatmegőrzési házirend-beállítások a különféle biztonsági mentési pontok.</span><span class="sxs-lookup"><span data-stu-id="34e52-107">Azure Backup provides flexibility in the backup schedule to create daily, weekly, monthly, or yearly backup points and gives you retention policy options for various backup points.</span></span> <span data-ttu-id="34e52-108">Helyi lemez másolat gyors helyreállítási idő célkitűzés (RTO) tárolására és gazdaságos, hosszú távú megőrzési Azure másolat tárolására is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="34e52-108">It also provides the capability to store local disk copies for quick recovery-time objectives (RTO) and to store copies to Azure for economical, long-term retention.</span></span>

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a><span data-ttu-id="34e52-109">SharePoint támogatott verziója, és a kapcsolódó védelmi forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="34e52-109">SharePoint supported versions and related protection scenarios</span></span>
<span data-ttu-id="34e52-110">Azure biztonsági mentés a DPM a következő szituációkat ismerteti:</span><span class="sxs-lookup"><span data-stu-id="34e52-110">Azure Backup for DPM supports the following scenarios:</span></span>

| <span data-ttu-id="34e52-111">Számítási feladat</span><span class="sxs-lookup"><span data-stu-id="34e52-111">Workload</span></span> | <span data-ttu-id="34e52-112">Verzió</span><span class="sxs-lookup"><span data-stu-id="34e52-112">Version</span></span> | <span data-ttu-id="34e52-113">A SharePoint központi telepítés</span><span class="sxs-lookup"><span data-stu-id="34e52-113">SharePoint deployment</span></span> | <span data-ttu-id="34e52-114">Védelem és helyreállítás</span><span class="sxs-lookup"><span data-stu-id="34e52-114">Protection and recovery</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="34e52-115">SharePoint</span><span class="sxs-lookup"><span data-stu-id="34e52-115">SharePoint</span></span> |<span data-ttu-id="34e52-116">A SharePoint 2013, SharePoint 2007, 2010 SharePoint SharePoint 3.0</span><span class="sxs-lookup"><span data-stu-id="34e52-116">SharePoint 2013, SharePoint 2010, SharePoint 2007, SharePoint 3.0</span></span> |<span data-ttu-id="34e52-117">A fizikai kiszolgálóként vagy Hyper-V vagy VMware virtuális gépeket telepített SharePoint</span><span class="sxs-lookup"><span data-stu-id="34e52-117">SharePoint deployed as a physical server or Hyper-V/VMware virtual machine</span></span> <br> -------------- <br> <span data-ttu-id="34e52-118">Az SQL AlwaysOn</span><span class="sxs-lookup"><span data-stu-id="34e52-118">SQL AlwaysOn</span></span> | <span data-ttu-id="34e52-119">Helyreállítási beállítások SharePoint-Farm védelme: a helyreállítási farm, adatbázis és a lemezes helyreállítási pontok a fájl vagy listaelem.</span><span class="sxs-lookup"><span data-stu-id="34e52-119">Protect SharePoint Farm recovery options: Recovery farm, database, and file or list item from disk recovery points.</span></span>  <span data-ttu-id="34e52-120">Adatbázis és a farm helyreállítási Azure helyreállítási pontokból.</span><span class="sxs-lookup"><span data-stu-id="34e52-120">Farm and database recovery from Azure recovery points.</span></span> |

## <a name="before-you-start"></a><span data-ttu-id="34e52-121">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="34e52-121">Before you start</span></span>
<span data-ttu-id="34e52-122">Néhány dolgot győződjön meg arról, előtt készítsen biztonsági másolatot az Azure-bA SharePoint-farm.</span><span class="sxs-lookup"><span data-stu-id="34e52-122">There are a few things you need to confirm before you back up a SharePoint farm to Azure.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="34e52-123">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="34e52-123">Prerequisites</span></span>
<span data-ttu-id="34e52-124">Mielőtt továbblépne, győződjön meg arról, hogy rendelkezik [telepítve, és az Azure Backup Server előkészített](backup-azure-microsoft-azure-backup.md) munkaterhelések védelme érdekében.</span><span class="sxs-lookup"><span data-stu-id="34e52-124">Before you proceed, make sure that you have [installed and prepared the Azure Backup Server](backup-azure-microsoft-azure-backup.md) to protect workloads.</span></span>

### <a name="protection-agent"></a><span data-ttu-id="34e52-125">Védelmi ügynök</span><span class="sxs-lookup"><span data-stu-id="34e52-125">Protection agent</span></span>
<span data-ttu-id="34e52-126">A védelmi ügynököt telepíteni kell a SharePoint, az SQL Server rendszert futtató kiszolgálók és más a SharePoint-farm részét képező kiszolgálók futtató kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="34e52-126">The Protection agent must be installed on the server that's running SharePoint, the servers that are running SQL Server, and all other servers that are part of the SharePoint farm.</span></span> <span data-ttu-id="34e52-127">A védelmi ügynök beállítása kapcsolatos további információkért lásd: [telepítő védelmi ügynök](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).</span><span class="sxs-lookup"><span data-stu-id="34e52-127">For more information about how to set up the protection agent, see [Setup Protection Agent](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).</span></span>  <span data-ttu-id="34e52-128">Az egyetlen kivétel ez alól, hogy az ügynököt telepít csak egy egyetlen (WFE) előtér-webkiszolgálóján.</span><span class="sxs-lookup"><span data-stu-id="34e52-128">The one exception is that you install the agent only on a single web front end (WFE) server.</span></span> <span data-ttu-id="34e52-129">DPM-nek az ügynök egy ELŐTÉR-webkiszolgálón csak a védelem belépési pontként szolgál.</span><span class="sxs-lookup"><span data-stu-id="34e52-129">DPM needs the agent on one WFE server only to serve as the entry point for protection.</span></span>

### <a name="sharepoint-farm"></a><span data-ttu-id="34e52-130">SharePoint-farm</span><span class="sxs-lookup"><span data-stu-id="34e52-130">SharePoint farm</span></span>
<span data-ttu-id="34e52-131">A farm minden 10 millió eleme, a legalább 2 GB lemezterületet a köteten, ahol a MABS mappában kell lennie.</span><span class="sxs-lookup"><span data-stu-id="34e52-131">For every 10 million items in the farm, there must be at least 2 GB of space on the volume where the MABS folder is located.</span></span> <span data-ttu-id="34e52-132">Ez a terület szükség a katalógus előállításához.</span><span class="sxs-lookup"><span data-stu-id="34e52-132">This space is required for catalog generation.</span></span> <span data-ttu-id="34e52-133">Katalógus-előállítás MABS elemeket (webhelycsoportok, webhelyek, listák, dokumentumtárak, mappák, egyes dokumentumok és listaelemek) helyreállítása, létrehoz az egyes tartalom-adatbázisokban tárolt URL-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="34e52-133">For MABS to recover specific items (site collections, sites, lists, document libraries, folders, individual documents, and list items), catalog generation creates a list of the URLs that are contained within each content database.</span></span> <span data-ttu-id="34e52-134">A helyreállítható elemek ablaktáblán lévő URL-címek listáját megtekintheti a **helyreállítási** MABS felügyeleti konzol feladatterületén.</span><span class="sxs-lookup"><span data-stu-id="34e52-134">You can view the list of URLs in the recoverable item pane in the **Recovery** task area of MABS Administrator Console.</span></span>

### <a name="sql-server"></a><span data-ttu-id="34e52-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="34e52-135">SQL Server</span></span>
<span data-ttu-id="34e52-136">MABS rendszerfiókként fut.</span><span class="sxs-lookup"><span data-stu-id="34e52-136">MABS runs as a LocalSystem account.</span></span> <span data-ttu-id="34e52-137">SQL Server-adatbázisok biztonsági mentéséhez MABS kell, hogy a fiók rendszergazdai jogosultságokkal az SQL Servert futtató kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="34e52-137">To back up SQL Server databases, MABS needs sysadmin privileges on that account for the server that's running SQL Server.</span></span> <span data-ttu-id="34e52-138">NT AUTHORITY\SYSTEM beállítása *sysadmin* a kiszolgálón, amelyen SQL Server előtt készítsen biztonsági másolatot.</span><span class="sxs-lookup"><span data-stu-id="34e52-138">Set NT AUTHORITY\SYSTEM to *sysadmin* on the server that's running SQL Server before you back it up.</span></span>

<span data-ttu-id="34e52-139">Ha a SharePoint-farm SQL Server-aliasokkal használt konfigurált SQL Server-adatbázisok, telepítse az SQL Server ügyfél összetevőit MABS által védendő előtér-webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="34e52-139">If the SharePoint farm has SQL Server databases that are configured with SQL Server aliases, install the SQL Server client components on the front-end Web server that MABS will protect.</span></span>

### <a name="sharepoint-server"></a><span data-ttu-id="34e52-140">SharePoint Server</span><span class="sxs-lookup"><span data-stu-id="34e52-140">SharePoint Server</span></span>
<span data-ttu-id="34e52-141">Amíg teljesítmény például a SharePoint-farm méret számos tényezőtől függ, általános útmutatásként egy MABS is 25 TB SharePoint-farm védelméhez.</span><span class="sxs-lookup"><span data-stu-id="34e52-141">While performance depends on many factors such as size of SharePoint farm, as general guidance one MABS can protect a 25 TB SharePoint farm.</span></span>

### <a name="whats-not-supported"></a><span data-ttu-id="34e52-142">Nem támogatott műveletek</span><span class="sxs-lookup"><span data-stu-id="34e52-142">What's not supported</span></span>
* <span data-ttu-id="34e52-143">Egy SharePoint-farm védelme MABS nem nyújt védelmet a keresési indexek vagy szolgáltatás adatbázisai.</span><span class="sxs-lookup"><span data-stu-id="34e52-143">MABS that protects a SharePoint farm does not protect search indexes or application service databases.</span></span> <span data-ttu-id="34e52-144">Ezeknek az adatbázisoknak a védelmét külön-külön konfigurálni kell.</span><span class="sxs-lookup"><span data-stu-id="34e52-144">You will need to configure the protection of these databases separately.</span></span>
* <span data-ttu-id="34e52-145">MABS ad üzemeltetett SharePoint SQL Server-adatbázisok biztonsági mentése (SOFS) kibővíthető fájlkiszolgáló-megosztásokat.</span><span class="sxs-lookup"><span data-stu-id="34e52-145">MABS does not provide backup of SharePoint SQL Server databases that are hosted on scale-out file server (SOFS) shares.</span></span>

## <a name="configure-sharepoint-protection"></a><span data-ttu-id="34e52-146">SharePoint-védelem beállítása</span><span class="sxs-lookup"><span data-stu-id="34e52-146">Configure SharePoint protection</span></span>
<span data-ttu-id="34e52-147">MABS védelme a SharePoint használata előtt konfigurálnia kell a SharePoint VSS-író szolgáltatás (WSS-író szolgáltatás) használatával **ConfigureSharePoint.exe**.</span><span class="sxs-lookup"><span data-stu-id="34e52-147">Before you can use MABS to protect SharePoint, you must configure the SharePoint VSS Writer service (WSS Writer service) by using **ConfigureSharePoint.exe**.</span></span>

<span data-ttu-id="34e52-148">Található **ConfigureSharePoint.exe** előtér-webkiszolgálón [MABS telepítési elérési út] \bin mappában.</span><span class="sxs-lookup"><span data-stu-id="34e52-148">You can find **ConfigureSharePoint.exe** in the [MABS Installation Path]\bin folder on the front-end web server.</span></span> <span data-ttu-id="34e52-149">Ezt az eszközt biztosít a SharePoint-farm hitelesítő adataival, a védelmi ügynök.</span><span class="sxs-lookup"><span data-stu-id="34e52-149">This tool provides the protection agent with the credentials for the SharePoint farm.</span></span> <span data-ttu-id="34e52-150">Futtatja egy WFE-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="34e52-150">You run it on a single WFE server.</span></span> <span data-ttu-id="34e52-151">Ha több ELŐTÉR-webkiszolgáló, csak egyet válasszon ki egy védelmi csoport konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="34e52-151">If you have multiple WFE servers, select just one when you configure a protection group.</span></span>

### <a name="to-configure-the-sharepoint-vss-writer-service"></a><span data-ttu-id="34e52-152">A SharePoint VSS-író szolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="34e52-152">To configure the SharePoint VSS Writer service</span></span>
1. <span data-ttu-id="34e52-153">Az ELŐTÉR-webkiszolgálón, a parancssorban navigáljon a [MABS telepítés helye] \bin\\</span><span class="sxs-lookup"><span data-stu-id="34e52-153">On the WFE server, at a command prompt, go to [MABS installation location]\bin\\</span></span>
2. <span data-ttu-id="34e52-154">Adja meg a ConfigureSharePoint - EnableSharePointProtection.</span><span class="sxs-lookup"><span data-stu-id="34e52-154">Enter ConfigureSharePoint -EnableSharePointProtection.</span></span>
3. <span data-ttu-id="34e52-155">Adja meg a farm rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="34e52-155">Enter the farm administrator credentials.</span></span> <span data-ttu-id="34e52-156">Ez a fiók a WFE-kiszolgálón a helyi Rendszergazdák csoport tagjának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="34e52-156">This account should be a member of the local Administrator group on the WFE server.</span></span> <span data-ttu-id="34e52-157">Ha a farm rendszergazdája nem helyi rendszergazda adja meg a következő engedélyeket az ELŐTÉR-webkiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="34e52-157">If the farm administrator isn’t a local admin grant the following permissions on the WFE server:</span></span>
   * <span data-ttu-id="34e52-158">A WSS_Admin_WPG csoportnak teljes hozzáférést a DPM mappát (% Program Files%\Microsoft Azure Backup\DPM).</span><span class="sxs-lookup"><span data-stu-id="34e52-158">Grant the WSS_Admin_WPG group full control to the DPM folder (%Program Files%\Microsoft Azure Backup\DPM).</span></span>
   * <span data-ttu-id="34e52-159">A WSS_Admin_WPG csoportnak olvasási engedélyt a DPM beállításkulcsot (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).</span><span class="sxs-lookup"><span data-stu-id="34e52-159">Grant the WSS_Admin_WPG group read access to the DPM Registry key (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).</span></span>

> [!NOTE]
> <span data-ttu-id="34e52-160">Futtassa újra a ConfigureSharePoint.exe, amikor megváltozik a SharePoint-farm rendszergazdai hitelesítő adatait a lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="34e52-160">You’ll need to rerun ConfigureSharePoint.exe whenever there’s a change in the SharePoint farm administrator credentials.</span></span>
>
>

## <a name="back-up-a-sharepoint-farm-by-using-mabs"></a><span data-ttu-id="34e52-161">Készítsen biztonsági másolatot egy SharePoint-farm MABS</span><span class="sxs-lookup"><span data-stu-id="34e52-161">Back up a SharePoint farm by using MABS</span></span>
<span data-ttu-id="34e52-162">Miután konfigurálta a MABS és a SharePoint-farm, amint azt korábban, a SharePoint védi őket MABS.</span><span class="sxs-lookup"><span data-stu-id="34e52-162">After you have configured MABS and the SharePoint farm as explained previously, SharePoint can be protected by MABS.</span></span>

### <a name="to-protect-a-sharepoint-farm"></a><span data-ttu-id="34e52-163">Egy SharePoint-farm védelme</span><span class="sxs-lookup"><span data-stu-id="34e52-163">To protect a SharePoint farm</span></span>
1. <span data-ttu-id="34e52-164">A a **védelmi** lapon kattintson a MABS felügyeleti konzol **új**.</span><span class="sxs-lookup"><span data-stu-id="34e52-164">From the **Protection** tab of the MABS Administrator Console, click **New**.</span></span>
    <span data-ttu-id="34e52-165">![Új védelem lap](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)</span><span class="sxs-lookup"><span data-stu-id="34e52-165">![New Protection Tab](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)</span></span>
2. <span data-ttu-id="34e52-166">Az a **védelmi csoport típusának kiválasztása** oldalán a **új védelmi csoport létrehozása** varázsló, jelölje be **kiszolgálók**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="34e52-166">On the **Select Protection Group Type** page of the **Create New Protection Group** wizard, select **Servers**, and then click **Next**.</span></span>

    ![Válassza ki a védelmi csoport típusa](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. <span data-ttu-id="34e52-168">Az a **csoporttagok kiválasztása** jelölje be a SharePoint-kiszolgáló védelmére, és kattintson a jobb **következő**.</span><span class="sxs-lookup"><span data-stu-id="34e52-168">On the **Select Group Members** screen, select the check box for the SharePoint server you want to protect and click **Next**.</span></span>

    ![Csoporttagok kiválasztása](./media/backup-azure-backup-sharepoint/select-group-members2.png)

   > [!NOTE]
   > <span data-ttu-id="34e52-170">A védelmi ügynök telepítve tekintse meg a kiszolgáló, a varázsló.</span><span class="sxs-lookup"><span data-stu-id="34e52-170">With the protection agent installed, you can see the server in the wizard.</span></span> <span data-ttu-id="34e52-171">MABS látható struktúrája.</span><span class="sxs-lookup"><span data-stu-id="34e52-171">MABS also shows its structure.</span></span> <span data-ttu-id="34e52-172">ConfigureSharePoint.exe futtatta, mert a MABS kommunikál a SharePoint VSS-író szolgáltatás és a megfelelő SQL Server-adatbázisok, és felismeri a SharePoint-farm struktúráját, a kapcsolódó tartalom-adatbázisok és a megfelelő elemeket.</span><span class="sxs-lookup"><span data-stu-id="34e52-172">Because you ran ConfigureSharePoint.exe, MABS communicates with the SharePoint VSS Writer service and its corresponding SQL Server databases and recognizes the SharePoint farm structure, the associated content databases, and any corresponding items.</span></span>
   >
   >
4. <span data-ttu-id="34e52-173">Az a **adatvédelmi módszer kiválasztása** lapján adja meg a **védelmi csoport**, és válassza ki a kívánt *védelmi módszert*.</span><span class="sxs-lookup"><span data-stu-id="34e52-173">On the **Select Data Protection Method** page, enter the name of the **Protection Group**, and select your preferred *protection methods*.</span></span> <span data-ttu-id="34e52-174">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="34e52-174">Click **Next**.</span></span>

    ![Adatvédelmi módszer kiválasztása](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)

   > [!NOTE]
   > <span data-ttu-id="34e52-176">A lemez adatvédelmi módszer segítségével rövid helyreállítási idő céljai.</span><span class="sxs-lookup"><span data-stu-id="34e52-176">The disk protection method helps to meet short recovery-time objectives.</span></span>
   >
   >
5. <span data-ttu-id="34e52-177">Az a **rövid távú célok megadása** lapon, válassza ki a kívánt **megőrzési időtartam** , és azonosíthatja, ha azt szeretné, hogy a biztonsági mentések.</span><span class="sxs-lookup"><span data-stu-id="34e52-177">On the **Specify Short-Term Goals** page, select your preferred **Retention range** and identify when you want backups to occur.</span></span>

    ![Rövid távú célok megadása](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)

   > [!NOTE]
   > <span data-ttu-id="34e52-179">Általában szükség, amely öt napnál régebbi adatok, mert jelenleg kijelölt lemezen öt napnyi megőrzési időtartamot, és biztosítani, hogy a biztonsági mentés nem éles órában, ehhez a példához történik.</span><span class="sxs-lookup"><span data-stu-id="34e52-179">Because recovery is most often required for data that's less than five days old, we selected a retention range of five days on disk and ensured that the backup happens during non-production hours, for this example.</span></span>
   >
   >
6. <span data-ttu-id="34e52-180">Tekintse át a tárolókészletben lefoglalt lemezterület a védelmi csoport, és majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="34e52-180">Review the storage pool disk space allocated for the protection group, and click then **Next**.</span></span>
7. <span data-ttu-id="34e52-181">Az összes védelmi csoportra MABS foglal le a hely a lemezen tárolja, és a replikák kezelése.</span><span class="sxs-lookup"><span data-stu-id="34e52-181">For every protection group, MABS allocates disk space to store and manage replicas.</span></span> <span data-ttu-id="34e52-182">Ezen a ponton MABS létre kell hoznia a kijelölt adatok másolatát.</span><span class="sxs-lookup"><span data-stu-id="34e52-182">At this point, MABS must create a copy of the selected data.</span></span> <span data-ttu-id="34e52-183">Válassza ki, hogyan és mikor szeretne létrehozni a replikát, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="34e52-183">Select how and when you want the replica created, and then click **Next**.</span></span>

    ![A replika-létrehozási módszer kiválasztása](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)

   > [!NOTE]
   > <span data-ttu-id="34e52-185">Győződjön meg arról, hogy a hálózati forgalom nem történik, válassza ki a megfelelő üzemi a munkaidőn kívül.</span><span class="sxs-lookup"><span data-stu-id="34e52-185">To make sure that network traffic is not effected, select a time outside production hours.</span></span>
   >
   >
8. <span data-ttu-id="34e52-186">MABS érdekében végezzen konzisztencia-ellenőrzést a replika az adatok integritásának biztosítja.</span><span class="sxs-lookup"><span data-stu-id="34e52-186">MABS ensures data integrity by performing consistency checks on the replica.</span></span> <span data-ttu-id="34e52-187">Kétféleképpen érhető el.</span><span class="sxs-lookup"><span data-stu-id="34e52-187">There are two available options.</span></span> <span data-ttu-id="34e52-188">Végezzen konzisztencia-ellenőrzést a kívánt ütemezést adhat meg, vagy a DPM is végezzen konzisztencia-ellenőrzést a replikán automatikusan, amikor inkonzisztenssé válik.</span><span class="sxs-lookup"><span data-stu-id="34e52-188">You can define a schedule to run consistency checks, or DPM can run consistency checks automatically on the replica whenever it becomes inconsistent.</span></span> <span data-ttu-id="34e52-189">Válassza ki a kívánt beállítást, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="34e52-189">Select your preferred option, and then click **Next**.</span></span>

    ![Konzisztencia-ellenőrzés](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. <span data-ttu-id="34e52-191">Az a **Online védelem adatainak megadása** lapon, válassza ki a SharePoint-farm védelmét, és kattintson a kívánt **következő**.</span><span class="sxs-lookup"><span data-stu-id="34e52-191">On the **Specify Online Protection Data** page, select the SharePoint farm that you want to protect, and then click **Next**.</span></span>

    ![A DPM SharePoint Protection1](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. <span data-ttu-id="34e52-193">Az a **Online biztonsági mentési ütemezés megadása** lapon válassza ki a kívánt ütemezést, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="34e52-193">On the **Specify Online Backup Schedule** page, select your preferred schedule, and then click **Next**.</span></span>

    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="34e52-195">MABS biztosít két napi biztonsági mentések maximum Azure majd elérhető legújabb lemez biztonsági mentési pont számára.</span><span class="sxs-lookup"><span data-stu-id="34e52-195">MABS provides a maximum of two daily backups to Azure from the then available latest disk backup point.</span></span> <span data-ttu-id="34e52-196">Azure biztonsági mentés WAN sávszélességet, amely alkalmas a biztonsági másolatok maximális és kevesen használatával is szabályozhatja [Azure biztonsági mentési hálózati sávszélesség-szabályozás](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).</span><span class="sxs-lookup"><span data-stu-id="34e52-196">Azure Backup can also control the amount of WAN bandwidth that can be used for backups in peak and off-peak hours by using [Azure Backup Network Throttling](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).</span></span>
    >
    >
11. <span data-ttu-id="34e52-197">Attól függően, hogy a kiválasztott, a biztonsági mentés ütemezése a **Online adatmegőrzési szabály megadása** lapon, válassza ki az adatmegőrzési napi, heti, havi vagy éves biztonsági mentési pontok.</span><span class="sxs-lookup"><span data-stu-id="34e52-197">Depending on the backup schedule that you selected, on the **Specify Online Retention Policy** page, select the retention policy for daily, weekly, monthly, and yearly backup points.</span></span>

    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)

    > [!NOTE]
    > <span data-ttu-id="34e52-199">MABS egy szerzett-édesapja-fia megőrzési séma, amelyben a különböző adatmegőrzési választható ki a másik biztonsági mentési pontok használja.</span><span class="sxs-lookup"><span data-stu-id="34e52-199">MABS uses a grandfather-father-son retention scheme in which a different retention policy can be chosen for different backup points.</span></span>
    >
    >
12. <span data-ttu-id="34e52-200">Hasonló lemezre, kezdeti hivatkozás pont replikájának kell létrehozni az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="34e52-200">Similar to disk, an initial reference point replica needs to be created in Azure.</span></span> <span data-ttu-id="34e52-201">Válassza ki a kívánt beállítást, hozzon létre egy kezdeti biztonsági másolatot az Azure-ba, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="34e52-201">Select your preferred option to create an initial backup copy to Azure, and then click **Next**.</span></span>

    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. <span data-ttu-id="34e52-203">Tekintse át a beállításokat a a **összegzés** lapon, majd **csoport létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="34e52-203">Review your selected settings on the **Summary** page, and then click **Create Group**.</span></span> <span data-ttu-id="34e52-204">Egy sikeres üzenet jelenik meg a védelmi csoport létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="34e52-204">You will see a success message after the protection group has been created.</span></span>

    ![Összefoglalás](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-mabs"></a><span data-ttu-id="34e52-206">Egy SharePoint-elem visszaállítása a lemezről MABS használatával</span><span class="sxs-lookup"><span data-stu-id="34e52-206">Restore a SharePoint item from disk by using MABS</span></span>
<span data-ttu-id="34e52-207">A következő példában a *helyreállítás SharePoint-elem* véletlenül törölve lett, és helyre szeretné állítani.</span><span class="sxs-lookup"><span data-stu-id="34e52-207">In the following example, the *Recovering SharePoint item* has been accidentally deleted and needs to be recovered.</span></span>
<span data-ttu-id="34e52-208">![MABS SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)</span><span class="sxs-lookup"><span data-stu-id="34e52-208">![MABS SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)</span></span>

1. <span data-ttu-id="34e52-209">Nyissa meg a **DPM felügyeleti konzol**.</span><span class="sxs-lookup"><span data-stu-id="34e52-209">Open the **DPM Administrator Console**.</span></span> <span data-ttu-id="34e52-210">Minden DPM által védett SharePoint-farmok megjelennek-e a **védelmi** fülre.</span><span class="sxs-lookup"><span data-stu-id="34e52-210">All SharePoint farms that are protected by DPM are shown in the **Protection** tab.</span></span>

    ![MABS SharePoint Protection3](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. <span data-ttu-id="34e52-212">Első lépéseként állítsa helyre az elemet, válassza ki a **helyreállítási** fülre.</span><span class="sxs-lookup"><span data-stu-id="34e52-212">To begin to recover the item, select the **Recovery** tab.</span></span>

    ![MABS SharePoint Protection5](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. <span data-ttu-id="34e52-214">A SharePoint kereshet *helyreállítás SharePoint-elem* kereséssel egy helyettesítő karakteres alapú belül egy helyreállítási pontot a tartományon.</span><span class="sxs-lookup"><span data-stu-id="34e52-214">You can search SharePoint for *Recovering SharePoint item* by using a wildcard-based search within a recovery point range.</span></span>

    ![MABS SharePoint Protection6](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. <span data-ttu-id="34e52-216">Válassza ki a megfelelő helyreállítási pont a keresési eredmények, kattintson a jobb gombbal az elemet, és válassza **helyreállítása**.</span><span class="sxs-lookup"><span data-stu-id="34e52-216">Select the appropriate recovery point from the search results, right-click the item, and then select **Recover**.</span></span>
5. <span data-ttu-id="34e52-217">Tallózzon a különböző helyreállítási pontokat is, és válasszon egy adatbázist vagy elem helyreállítása.</span><span class="sxs-lookup"><span data-stu-id="34e52-217">You can also browse through various recovery points and select a database or item to recover.</span></span> <span data-ttu-id="34e52-218">Válassza ki **dátum > helyreállításkor**, és válassza ki a megfelelő **adatbázis > SharePoint-farm > helyreállítási pont > elem**.</span><span class="sxs-lookup"><span data-stu-id="34e52-218">Select **Date > Recovery time**, and then select the correct **Database > SharePoint farm > Recovery point > Item**.</span></span>

    ![MABS SharePoint Protection7](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. <span data-ttu-id="34e52-220">Kattintson a jobb gombbal az elemet, majd válassza ki **helyreállítása** megnyitásához a **helyreállítási varázsló**.</span><span class="sxs-lookup"><span data-stu-id="34e52-220">Right-click the item, and then select **Recover** to open the **Recovery Wizard**.</span></span> <span data-ttu-id="34e52-221">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="34e52-221">Click **Next**.</span></span>

    ![Helyreállítási beállítások áttekintése](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. <span data-ttu-id="34e52-223">Válassza ki a végrehajtani, és kattintson a kívánt helyreállítási **következő**.</span><span class="sxs-lookup"><span data-stu-id="34e52-223">Select the type of recovery that you want to perform, and then click **Next**.</span></span>

    ![Helyreállítási típus](./media/backup-azure-backup-sharepoint/select-recovery-type.png)

   > [!NOTE]
   > <span data-ttu-id="34e52-225">A kijelölt **visszaállítás az eredeti** példában helyreállítja a cikk az eredeti SharePoint-webhelyre.</span><span class="sxs-lookup"><span data-stu-id="34e52-225">The selection of **Recover to original** in the example recovers the item to the original SharePoint site.</span></span>
   >
   >
8. <span data-ttu-id="34e52-226">Válassza ki a **helyreállítási folyamat** használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="34e52-226">Select the **Recovery Process** that you want to use.</span></span>

   * <span data-ttu-id="34e52-227">Válassza ki **helyreállítás helyreállítási farm nélkül** Ha a SharePoint-farm nem változott, és ugyanaz, mint a helyreállítási pont, amely visszaállítása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="34e52-227">Select **Recover without using a recovery farm** if the SharePoint farm has not changed and is the same as the recovery point that is being restored.</span></span>
   * <span data-ttu-id="34e52-228">Válassza ki **helyreállításához a helyreállítási farm** Ha a SharePoint-farm megváltozott a helyreállítási pont létrehozása óta.</span><span class="sxs-lookup"><span data-stu-id="34e52-228">Select **Recover using a recovery farm** if the SharePoint farm has changed since the recovery point was created.</span></span>

     ![A helyreállítási folyamat](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. <span data-ttu-id="34e52-230">Állítsa helyre az adatbázist ideiglenesen egy SQL Server átmeneti helyét, és átmeneti fájlmegosztás MABS és a SharePoint-elem helyreállítása futtató kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="34e52-230">Provide a staging SQL Server instance location to recover the database temporarily, and provide a staging file share on MABS and the server that's running SharePoint to recover the item.</span></span>

    ![Átmeneti Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)

    <span data-ttu-id="34e52-232">MABS csatolja a tartalom-adatbázist, amelyen a SharePoint-elem az ideiglenes SQL Server-példányhoz.</span><span class="sxs-lookup"><span data-stu-id="34e52-232">MABS attaches the content database that is hosting the SharePoint item to the temporary SQL Server instance.</span></span> <span data-ttu-id="34e52-233">A tartalom-adatbázist azt állítja helyre az elemet, és az átmeneti tárolási helye a MABS helyezi.</span><span class="sxs-lookup"><span data-stu-id="34e52-233">From the content database, it recovers the item and puts it on the staging file location on MABS.</span></span> <span data-ttu-id="34e52-234">A helyreállított elem, amely az ideiglenes helyet most az ideiglenes helyet azon a SharePoint-farmon exportálni kell.</span><span class="sxs-lookup"><span data-stu-id="34e52-234">The recovered item that's on the staging location now needs to be exported to the staging location on the SharePoint farm.</span></span>

    ![Átmeneti Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. <span data-ttu-id="34e52-236">Válassza ki **helyreállítási beállítások megadása**, és a SharePoint-farm biztonsági beállítások alkalmazása, vagy a helyreállítási pont biztonsági beállításainak alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="34e52-236">Select **Specify recovery options**, and apply security settings to the SharePoint farm or apply the security settings of the recovery point.</span></span> <span data-ttu-id="34e52-237">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="34e52-237">Click **Next**.</span></span>

    ![Helyreállítási beállítások](./media/backup-azure-backup-sharepoint/recovery-options.png)

    > [!NOTE]
    > <span data-ttu-id="34e52-239">Ha szeretné, a hálózati sávszélesség használatának szabályozását.</span><span class="sxs-lookup"><span data-stu-id="34e52-239">You can choose to throttle the network bandwidth usage.</span></span> <span data-ttu-id="34e52-240">Ez minimalizálja az üzemi kiszolgálón történő éles órában.</span><span class="sxs-lookup"><span data-stu-id="34e52-240">This minimizes impact to the production server during production hours.</span></span>
    >
    >
11. <span data-ttu-id="34e52-241">Ellenőrizze az összefoglaló információkat, és kattintson a **helyreállítása** fájl helyreállítási megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="34e52-241">Review the summary information, and then click **Recover** to begin recovery of the file.</span></span>

    ![A helyreállítási összefoglaló](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. <span data-ttu-id="34e52-243">Most válassza ki a **figyelés** lapra a **MABS felügyeleti konzol** megtekintéséhez a **állapot** a helyreállítási.</span><span class="sxs-lookup"><span data-stu-id="34e52-243">Now select the **Monitoring** tab in the **MABS Administrator Console** to view the **Status** of the recovery.</span></span>

    ![Helyreállítási állapota](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)

    > [!NOTE]
    > <span data-ttu-id="34e52-245">A fájl már vissza.</span><span class="sxs-lookup"><span data-stu-id="34e52-245">The file is now restored.</span></span> <span data-ttu-id="34e52-246">A SharePoint-webhelyre a visszaállított fájl is frissítheti.</span><span class="sxs-lookup"><span data-stu-id="34e52-246">You can refresh the SharePoint site to check the restored file.</span></span>
    >
    >

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a><span data-ttu-id="34e52-247">Egy SharePoint-adatbázis visszaállítása az Azure-ból a DPM használatával</span><span class="sxs-lookup"><span data-stu-id="34e52-247">Restore a SharePoint database from Azure by using DPM</span></span>
1. <span data-ttu-id="34e52-248">Egy SharePoint tartalom-adatbázis helyreállításához tallózzon a különböző helyreállítási pontokat (ahogy korábban), és válassza ki a visszaállítani kívánt helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="34e52-248">To recover a SharePoint content database, browse through various recovery points (as shown previously), and select the recovery point that you want to restore.</span></span>

    ![MABS SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. <span data-ttu-id="34e52-250">Kattintson duplán a SharePoint helyreállítási pont megjelenítése a elérhető SharePoint-katalógus adatait.</span><span class="sxs-lookup"><span data-stu-id="34e52-250">Double-click the SharePoint recovery point to show the available SharePoint catalog information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="34e52-251">A SharePoint-farm hosszú távú megőrzési az Azure-ban a védett, mert a MABS nincs katalógus információkkal (metaadatokkal) érhető el.</span><span class="sxs-lookup"><span data-stu-id="34e52-251">Because the SharePoint farm is protected for long-term retention in Azure, no catalog information (metadata) is available on MABS.</span></span> <span data-ttu-id="34e52-252">Ennek eredményeképpen időpontban a SharePoint tartalom-adatbázist kell a helyre kell állítani, amikor kell a SharePoint-farm katalógus újra.</span><span class="sxs-lookup"><span data-stu-id="34e52-252">As a result, whenever a point-in-time SharePoint content database needs to be recovered, you need to catalog the SharePoint farm again.</span></span>
   >
   >
3. <span data-ttu-id="34e52-253">Kattintson a **újrakatalogizáláshoz**.</span><span class="sxs-lookup"><span data-stu-id="34e52-253">Click **Re-catalog**.</span></span>

    ![MABS SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    <span data-ttu-id="34e52-255">A **felhő Újrakatalogizálni** állapotkezelő ablak nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="34e52-255">The **Cloud Recatalog** status window opens.</span></span>

    ![MABS SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    <span data-ttu-id="34e52-257">Katalogizálni befejezése után a állapota *sikeres*.</span><span class="sxs-lookup"><span data-stu-id="34e52-257">After cataloging is finished, the status changes to *Success*.</span></span> <span data-ttu-id="34e52-258">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="34e52-258">Click **Close**.</span></span>

    ![MABS SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. <span data-ttu-id="34e52-260">Kattintson a SharePoint-objektum a MABS látható **helyreállítási** lapot a tartalom-adatbázist-struktúrában.</span><span class="sxs-lookup"><span data-stu-id="34e52-260">Click the SharePoint object shown in the MABS **Recovery** tab to get the content database structure.</span></span> <span data-ttu-id="34e52-261">Kattintson a jobb gombbal az elemet, és kattintson **helyreállítása**.</span><span class="sxs-lookup"><span data-stu-id="34e52-261">Right-click the item, and then click **Recover**.</span></span>

    ![MABS SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. <span data-ttu-id="34e52-263">Ezen a ponton, kövesse a [helyreállítási lépések az ebben a cikkben](#restore-a-sharepoint-item-from-disk-using-dpm) a lemezről egy SharePoint tartalom-adatbázis helyreállításához.</span><span class="sxs-lookup"><span data-stu-id="34e52-263">At this point, follow the [recovery steps earlier in this article](#restore-a-sharepoint-item-from-disk-using-dpm) to recover a SharePoint content database from disk.</span></span>

## <a name="faqs"></a><span data-ttu-id="34e52-264">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="34e52-264">FAQs</span></span>
<span data-ttu-id="34e52-265">K: helyreállíthatók a SharePoint-elem az eredeti helyre, ha SharePoint (a védelem a lemezen) az SQL AlwaysOn használatára van konfigurálva?</span><span class="sxs-lookup"><span data-stu-id="34e52-265">Q: Can I recover a SharePoint item to the original location if SharePoint is configured by using SQL AlwaysOn (with protection on disk)?</span></span><br>
<span data-ttu-id="34e52-266">V: Igen, az elem állíthatók helyre az eredeti SharePoint-webhelyre.</span><span class="sxs-lookup"><span data-stu-id="34e52-266">A: Yes, the item can be recovered to the original SharePoint site.</span></span>

<span data-ttu-id="34e52-267">K: helyreállíthatók a SharePoint-adatbázist az eredeti helyre, ha a SharePoint SQL AlwaysOn használatára van konfigurálva?</span><span class="sxs-lookup"><span data-stu-id="34e52-267">Q: Can I recover a SharePoint database to the original location if SharePoint is configured by using SQL AlwaysOn?</span></span><br>
<span data-ttu-id="34e52-268">V:, mert a SharePoint-adatbázisok vannak konfigurálva az SQL AlwaysOn, ezeket nem lehet módosítani kivéve, ha a rendelkezésre állási csoport eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="34e52-268">A: Because SharePoint databases are configured in SQL AlwaysOn, they cannot be modified unless the availability group is removed.</span></span> <span data-ttu-id="34e52-269">Ennek eredményeképpen az MABS nem tudja visszaállítani az adatbázis az eredeti helyre.</span><span class="sxs-lookup"><span data-stu-id="34e52-269">As a result, MABS cannot restore a database to the original location.</span></span> <span data-ttu-id="34e52-270">Helyreállíthatja az SQL Server-adatbázis egy másik SQL Server-példányhoz.</span><span class="sxs-lookup"><span data-stu-id="34e52-270">You can recover a SQL Server database to another SQL Server instance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34e52-271">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="34e52-271">Next steps</span></span>
* <span data-ttu-id="34e52-272">További tudnivalók a SharePoint védelme MABS – lásd [videó sorozat - a SharePoint védelme a DPM](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)</span><span class="sxs-lookup"><span data-stu-id="34e52-272">Learn more about MABS Protection of SharePoint - see [Video Series - DPM Protection of SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)</span></span>
