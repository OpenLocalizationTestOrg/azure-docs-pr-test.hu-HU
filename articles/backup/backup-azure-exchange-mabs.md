---
title: "Készítsen biztonsági mentést egy Exchange-kiszolgálóhoz az Azure biztonsági mentés az Azure Backup Server |} Microsoft Docs"
description: "Útmutató: biztonsági mentése az Exchange-kiszolgáló Azure Backup szolgáltatás használatával az Azure Backup Server"
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
ms.assetid: e46557e8-2eaf-4ee0-99ea-00fbb8687dca
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 60b784fd00013c2b9504f8635c6b5c4c592563be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-azure-backup-server"></a><span data-ttu-id="870f9-103">Biztonsági mentése az Exchange-kiszolgáló Azure Backup szolgáltatás az Azure Backup Server</span><span class="sxs-lookup"><span data-stu-id="870f9-103">Back up an Exchange server to Azure Backup with Azure Backup Server</span></span>
<span data-ttu-id="870f9-104">Ez a cikk ismerteti, hogyan konfigurálása a Microsoft Azure Backup Server (MABS) biztonsági mentése egy Microsoft Exchange-kiszolgálóhoz az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="870f9-104">This article describes how to configure Microsoft Azure Backup Server (MABS) to back up a Microsoft Exchange server to Azure.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="870f9-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="870f9-105">Prerequisites</span></span>
<span data-ttu-id="870f9-106">A folytatás előtt győződjön meg arról, hogy Azure Backup Server [telepítve és előkészített](backup-azure-microsoft-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="870f9-106">Before you continue, make sure that Azure Backup Server is [installed and prepared](backup-azure-microsoft-azure-backup.md).</span></span>

## <a name="mabs-protection-agent"></a><span data-ttu-id="870f9-107">MABS védelmi ügynök</span><span class="sxs-lookup"><span data-stu-id="870f9-107">MABS protection agent</span></span>
<span data-ttu-id="870f9-108">Az Exchange kiszolgálón a MABS védelmi ügynök telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="870f9-108">To install the MABS protection agent on the Exchange server, follow these steps:</span></span>

1. <span data-ttu-id="870f9-109">Győződjön meg arról, hogy a tűzfalak konfigurációja megfelelő.</span><span class="sxs-lookup"><span data-stu-id="870f9-109">Make sure that the firewalls are correctly configured.</span></span> <span data-ttu-id="870f9-110">Lásd: [tűzfalkivétel konfigurálása az ügynök számára](https://technet.microsoft.com/library/Hh758204.aspx).</span><span class="sxs-lookup"><span data-stu-id="870f9-110">See [Configure firewall exceptions for the agent](https://technet.microsoft.com/library/Hh758204.aspx).</span></span>
2. <span data-ttu-id="870f9-111">Az Exchange-kiszolgálóhoz gombra kattintva a ügynök telepíthető **felügyeleti > ügynökök > telepítése** MABS felügyeleti konzolon.</span><span class="sxs-lookup"><span data-stu-id="870f9-111">Install the agent on the Exchange server by clicking **Management > Agents > Install** in MABS Administrator Console.</span></span> <span data-ttu-id="870f9-112">Lásd: [a MABS védelmi ügynök telepítése](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) a részletes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="870f9-112">See [Install the MABS protection agent](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) for detailed steps.</span></span>

## <a name="create-a-protection-group-for-the-exchange-server"></a><span data-ttu-id="870f9-113">Az Exchange-kiszolgáló védelmi csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="870f9-113">Create a protection group for the Exchange server</span></span>
1. <span data-ttu-id="870f9-114">A MABS felügyeleti konzolon kattintson a **védelmi**, és kattintson a **új** megnyitásához az eszközsávon a **új védelmi csoport létrehozása** varázsló.</span><span class="sxs-lookup"><span data-stu-id="870f9-114">In the MABS Administrator Console, click **Protection**, and then click **New** on the tool ribbon to open the **Create New Protection Group** wizard.</span></span>
2. <span data-ttu-id="870f9-115">Az a **üdvözlő** képernyőjén kattintson a varázsló **következő**.</span><span class="sxs-lookup"><span data-stu-id="870f9-115">On the **Welcome** screen of the wizard click **Next**.</span></span>
3. <span data-ttu-id="870f9-116">Az a **védelmi csoport típusának kiválasztása** képernyőn, jelölje be **kiszolgálók** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="870f9-116">On the **Select protection group type** screen, select **Servers** and click **Next**.</span></span>
4. <span data-ttu-id="870f9-117">Válassza ki az Exchange server-adatbázis védelmét, és kattintson a kívánt **következő**.</span><span class="sxs-lookup"><span data-stu-id="870f9-117">Select the Exchange server database that you want to protect and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="870f9-118">Ha az Exchange 2013 védelmét, ellenőrizze a [Exchange 2013 előfeltételei](https://technet.microsoft.com/library/dn751029.aspx).</span><span class="sxs-lookup"><span data-stu-id="870f9-118">If you are protecting Exchange 2013, check the [Exchange 2013 prerequisites](https://technet.microsoft.com/library/dn751029.aspx).</span></span>
   >
   >

    <span data-ttu-id="870f9-119">A következő példában az Exchange 2010 adatbázis van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="870f9-119">In the following example, the Exchange 2010 database is selected.</span></span>

    ![Csoporttagok kiválasztása](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. <span data-ttu-id="870f9-121">Az adatvédelmi módszer kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="870f9-121">Select the data protection method.</span></span>

    <span data-ttu-id="870f9-122">A védelmi csoport neve, majd válassza ki a következők mindegyikét:</span><span class="sxs-lookup"><span data-stu-id="870f9-122">Name the protection group, and then select both of the following options:</span></span>

   * <span data-ttu-id="870f9-123">Rövid távú lemezes védelmet szeretnék.</span><span class="sxs-lookup"><span data-stu-id="870f9-123">I want short-term protection using Disk.</span></span>
   * <span data-ttu-id="870f9-124">Online védelmet szeretnék.</span><span class="sxs-lookup"><span data-stu-id="870f9-124">I want online protection.</span></span>
6. <span data-ttu-id="870f9-125">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="870f9-125">Click **Next**.</span></span>
7. <span data-ttu-id="870f9-126">Válassza ki a **Eseutil futtatása az adatok sértetlenségének ellenőrzéséhez** lehetőséget, ha azt szeretné, hogy az Exchange Server-adatbázisok sértetlenségének ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="870f9-126">Select the **Run Eseutil to check data integrity** option if you want to check the integrity of the Exchange Server databases.</span></span>

    <span data-ttu-id="870f9-127">Miután kiválasztotta ezt a beállítást, biztonsági mentés konzisztencia-ellenőrzést fog futni a MABS az i/o-forgalmat futtatásával elkerülése érdekében a **eseutil** parancsot az Exchange-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="870f9-127">After you select this option, backup consistency checking will be run on MABS to avoid the I/O traffic that’s generated by running the **eseutil** command on the Exchange server.</span></span>

   > [!NOTE]
   > <span data-ttu-id="870f9-128">Használja ezt a beállítást, át kell másolnia az Ese.dll és az Eseutil.exe fájloknak a C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin könyvtárához a MAB.</span><span class="sxs-lookup"><span data-stu-id="870f9-128">To use this option, you must copy the Ese.dll and Eseutil.exe files to the C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin directory on the MAB server.</span></span> <span data-ttu-id="870f9-129">Ellenkező esetben aktiválódik, a következő hibával:</span><span class="sxs-lookup"><span data-stu-id="870f9-129">Otherwise, the following error is triggered:</span></span>  
   > <span data-ttu-id="870f9-130">![az Eseutil hiba](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span><span class="sxs-lookup"><span data-stu-id="870f9-130">![eseutil error](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span></span>
   >
   >
8. <span data-ttu-id="870f9-131">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="870f9-131">Click **Next**.</span></span>
9. <span data-ttu-id="870f9-132">Válassza ki az adatbázist a **másolásos biztonsági mentésre**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="870f9-132">Select the database for **Copy Backup**, and then click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="870f9-133">Ha nincs bejelölve a "Teljes biztonsági másolat" adatbázis másolatának legalább egy DAG, naplók nem lesznek csonkolva.</span><span class="sxs-lookup"><span data-stu-id="870f9-133">If you do not select “Full backup” for at least one DAG copy of a database, logs will not be truncated.</span></span>
   >
   >
10. <span data-ttu-id="870f9-134">Konfigurálja a célokat **rövid távú biztonsági mentés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="870f9-134">Configure the goals for **Short-Term backup**, and then click **Next**.</span></span>
11. <span data-ttu-id="870f9-135">Tekintse át a rendelkezésre álló lemezterület, majd a **következő**.</span><span class="sxs-lookup"><span data-stu-id="870f9-135">Review the available disk space, and then click **Next**.</span></span>
12. <span data-ttu-id="870f9-136">Válassza ki azt az időpontot, ahol a MAB kiszolgáló létrehoz a kezdeti replikálást, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="870f9-136">Select the time at which the MAB Server will create the initial replication, and then click **Next**.</span></span>
13. <span data-ttu-id="870f9-137">A konzisztencia-ellenőrzési beállítások kiválasztása, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="870f9-137">Select the consistency check options, and then click **Next**.</span></span>
14. <span data-ttu-id="870f9-138">Adja meg az adatbázis biztonsági mentése az Azure-ba, és kattintson a kívánt **következő**.</span><span class="sxs-lookup"><span data-stu-id="870f9-138">Choose the database that you want to back up to Azure, and then click **Next**.</span></span> <span data-ttu-id="870f9-139">Példa:</span><span class="sxs-lookup"><span data-stu-id="870f9-139">For example:</span></span>

    ![Online védelem adatainak megadása](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. <span data-ttu-id="870f9-141">Adja meg a ütemezését **Azure biztonsági mentés**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="870f9-141">Define the schedule for **Azure Backup**, and then click **Next**.</span></span> <span data-ttu-id="870f9-142">Példa:</span><span class="sxs-lookup"><span data-stu-id="870f9-142">For example:</span></span>

    ![Adja meg az online biztonsági mentés ütemezése](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="870f9-144">Jegyezze fel az Online helyreállítási pontok alapuló Expressz teljes helyreállítási pontokat.</span><span class="sxs-lookup"><span data-stu-id="870f9-144">Note Online recovery points are based on express full recovery points.</span></span> <span data-ttu-id="870f9-145">Az online helyreállítási pontot, ezért úgy kell ütemeznie után az idő megadott az expressz teljes helyreállítási pont.</span><span class="sxs-lookup"><span data-stu-id="870f9-145">Therefore, you must schedule the online recovery point after the time that’s specified for the express full recovery point.</span></span>
    >
    >
16. <span data-ttu-id="870f9-146">Konfigurálja az adatmegőrzési **Azure biztonsági mentés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="870f9-146">Configure the retention policy for **Azure Backup**, and then click **Next**.</span></span>
17. <span data-ttu-id="870f9-147">Válasszon egy online replikációs lehetőséget, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="870f9-147">Choose an online replication option and click **Next**.</span></span>

    <span data-ttu-id="870f9-148">Ha nagy adatbázis, a kezdeti biztonsági másolatot a hálózaton keresztül a létrehozandó hosszú ideig eltarthat.</span><span class="sxs-lookup"><span data-stu-id="870f9-148">If you have a large database, it could take a long time for the initial backup to be created over the network.</span></span> <span data-ttu-id="870f9-149">A probléma elkerülése érdekében, létrehozhat egy offline biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="870f9-149">To avoid this issue, you can create an offline backup.</span></span>  

    ![Online megőrzési szabály megadása](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. <span data-ttu-id="870f9-151">Hagyja jóvá a beállításokat, és kattintson a **csoport létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="870f9-151">Confirm the settings, and then click **Create Group**.</span></span>
19. <span data-ttu-id="870f9-152">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="870f9-152">Click **Close**.</span></span>

## <a name="recover-the-exchange-database"></a><span data-ttu-id="870f9-153">Az Exchange-adatbázis helyreállítása</span><span class="sxs-lookup"><span data-stu-id="870f9-153">Recover the Exchange database</span></span>
1. <span data-ttu-id="870f9-154">Exchange-adatbázis helyreállítása, kattintson a **helyreállítási** MABS felügyeleti konzolján.</span><span class="sxs-lookup"><span data-stu-id="870f9-154">To recover an Exchange database, click **Recovery** in the MABS Administrator Console.</span></span>
2. <span data-ttu-id="870f9-155">Keresse meg a helyreállítani kívánt Exchange-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="870f9-155">Locate the Exchange database that you want to recover.</span></span>
3. <span data-ttu-id="870f9-156">Válassza ki az online helyreállítási pontot a *helyreállításkor* legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="870f9-156">Select an online recovery point from the *recovery time* drop-down list.</span></span>
4. <span data-ttu-id="870f9-157">Kattintson a **helyreállítása** elindítani a **helyreállítási varázsló**.</span><span class="sxs-lookup"><span data-stu-id="870f9-157">Click **Recover** to start the **Recovery Wizard**.</span></span>

<span data-ttu-id="870f9-158">Az online helyreállítási pontok, öt helyreállítási típusa van:</span><span class="sxs-lookup"><span data-stu-id="870f9-158">For online recovery points, there are five recovery types:</span></span>

* <span data-ttu-id="870f9-159">**Helyreállítás az eredeti Exchange-kiszolgálón:** fogja visszaállítani az adatokat az eredeti Exchange-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="870f9-159">**Recover to original Exchange Server location:** The data will be recovered to the original Exchange server.</span></span>
* <span data-ttu-id="870f9-160">**Helyreállítás az Exchange-kiszolgáló egy másik adatbázisba:** fogja visszaállítani az adatokat egy másik egy másik Exchange server-adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="870f9-160">**Recover to another database on an Exchange Server:** The data will be recovered to another database on another Exchange server.</span></span>
* <span data-ttu-id="870f9-161">**Helyreállítás helyreállítási adatbázisba:** lesz az adatok helyreállítása egy Exchange helyreállítási adatbázis (Rekordadatbázis).</span><span class="sxs-lookup"><span data-stu-id="870f9-161">**Recover to a Recovery Database:** The data will be recovered to an Exchange Recovery Database (RDB).</span></span>
* <span data-ttu-id="870f9-162">**Másolás hálózati mappába:** fogja visszaállítani az adatokat egy hálózati mappába.</span><span class="sxs-lookup"><span data-stu-id="870f9-162">**Copy to a network folder:** The data will be recovered to a network folder.</span></span>
* <span data-ttu-id="870f9-163">**Másolás szalagra:** egy szalagtárat vagy egy önálló szalagos meghajtót csatlakoztatott, de MABS konfigurálva van, ha a helyreállítási pont egy szabad szalagra kerülnek-e.</span><span class="sxs-lookup"><span data-stu-id="870f9-163">**Copy to tape:** If you have a tape library or a stand-alone tape drive attached and configured on MABS, the recovery point will be copied to a free tape.</span></span>

    ![Válassza ki az online replikációs](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a><span data-ttu-id="870f9-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="870f9-165">Next steps</span></span>
* [<span data-ttu-id="870f9-166">Az Azure biztonsági mentési – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="870f9-166">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
