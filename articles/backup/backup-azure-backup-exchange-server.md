---
title: "Készítsen biztonsági mentést egy Exchange-kiszolgálóhoz, az Azure Backup és a System Center 2012 R2 DPM |} Microsoft Docs"
description: "Útmutató: biztonsági mentése az Exchange-kiszolgáló Azure Backup szolgáltatás használatával a System Center 2012 R2 DPM"
services: backup
documentationcenter: 
author: MaanasSaran
manager: NKolli1
editor: 
ms.assetid: 13f32256-888e-416e-a78b-40c2a26a5939
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: masaran;jimpark;delhan;trinadhk;markgal
ms.openlocfilehash: 2a0e416440e55cfde70cbd20d40c99fb29b4229c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-system-center-2012-r2-dpm"></a><span data-ttu-id="a9676-103">Exchange-kiszolgáló biztonsági mentése az Azure Backupba a System Center 2012 R2 DPM-mel</span><span class="sxs-lookup"><span data-stu-id="a9676-103">Back up an Exchange server to Azure Backup with System Center 2012 R2 DPM</span></span>
<span data-ttu-id="a9676-104">Ez a cikk ismerteti a System Center 2012 R2 Data Protection Manager (DPM) kiszolgáló biztonsági mentése egy Microsoft Exchange-kiszolgáló Azure Backup konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a9676-104">This article describes how to configure a System Center 2012 R2 Data Protection Manager (DPM) server to back up a Microsoft Exchange server to  Azure Backup.</span></span>  

## <a name="updates"></a><span data-ttu-id="a9676-105">Frissítések</span><span class="sxs-lookup"><span data-stu-id="a9676-105">Updates</span></span>
<span data-ttu-id="a9676-106">Sikerült regisztrálni a DPM-kiszolgáló Azure Backup szolgáltatásnál, telepítenie kell a legújabb kumulatív frissítéssel a System Center 2012 R2 DPM és az Azure Backup szolgáltatás ügynökének legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="a9676-106">To successfully register the DPM server with Azure Backup, you must install the latest update rollup for System Center 2012 R2 DPM and the latest version of the Azure Backup Agent.</span></span> <span data-ttu-id="a9676-107">A legújabb kumulatív frissítéssel az beszerzése a [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).</span><span class="sxs-lookup"><span data-stu-id="a9676-107">Get the latest update rollup from the [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).</span></span>

> [!NOTE]
> <span data-ttu-id="a9676-108">A cikkben szereplő példák az Azure Backup szolgáltatás ügynökének 2.0.8719.0 verziója van telepítve, és 6. kumulatív frissítés a System Center 2012 R2 DPM telepítve van.</span><span class="sxs-lookup"><span data-stu-id="a9676-108">For the examples in this article, version 2.0.8719.0 of the Azure Backup Agent is installed, and Update Rollup 6 is installed on System Center 2012 R2 DPM.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="a9676-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a9676-109">Prerequisites</span></span>
<span data-ttu-id="a9676-110">A folytatás előtt győződjön meg arról, hogy minden a [Előfeltételek](backup-azure-dpm-introduction.md#prerequisites) a Microsoft Azure Backup szolgáltatás használatával történő védelméhez munkaterhelések teljesült.</span><span class="sxs-lookup"><span data-stu-id="a9676-110">Before you continue, make sure that all the [prerequisites](backup-azure-dpm-introduction.md#prerequisites) for using Microsoft Azure Backup to protect workloads have been met.</span></span> <span data-ttu-id="a9676-111">Az Előfeltételek a következők:</span><span class="sxs-lookup"><span data-stu-id="a9676-111">These prerequisites include the following:</span></span>

* <span data-ttu-id="a9676-112">Az Azure webhelyen egy biztonsági mentési tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a9676-112">A backup vault on the Azure site has been created.</span></span>
* <span data-ttu-id="a9676-113">Ügynök és a tároló hitelesítő adatait a DPM-kiszolgáló lettek töltve.</span><span class="sxs-lookup"><span data-stu-id="a9676-113">Agent and vault credentials have been downloaded to the DPM server.</span></span>
* <span data-ttu-id="a9676-114">Az ügynök telepítve van a DPM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a9676-114">The agent is installed on the DPM server.</span></span>
* <span data-ttu-id="a9676-115">A tárolói hitelesítő adatokat a DPM-kiszolgáló regisztrálása használták.</span><span class="sxs-lookup"><span data-stu-id="a9676-115">The vault credentials were used to register the DPM server.</span></span>
* <span data-ttu-id="a9676-116">Exchange 2016 véd, frissítse a DPM 2012 R2 UR9 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="a9676-116">If you are protecting Exchange 2016, please upgrade to DPM 2012 R2 UR9 or later</span></span>

## <a name="dpm-protection-agent"></a><span data-ttu-id="a9676-117">A DPM védelmi ügynök</span><span class="sxs-lookup"><span data-stu-id="a9676-117">DPM protection agent</span></span>
<span data-ttu-id="a9676-118">Az Exchange kiszolgálón a DPM védelmi ügynök telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a9676-118">To install the DPM protection agent on the Exchange server, follow these steps:</span></span>

1. <span data-ttu-id="a9676-119">Győződjön meg arról, hogy a tűzfalak konfigurációja megfelelő.</span><span class="sxs-lookup"><span data-stu-id="a9676-119">Make sure that the firewalls are correctly configured.</span></span> <span data-ttu-id="a9676-120">Lásd: [tűzfalkivétel konfigurálása az ügynök számára](https://technet.microsoft.com/library/Hh758204.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9676-120">See [Configure firewall exceptions for the agent](https://technet.microsoft.com/library/Hh758204.aspx).</span></span>
2. <span data-ttu-id="a9676-121">Az Exchange-kiszolgálóhoz gombra kattintva a ügynök telepíthető **felügyeleti > ügynökök > telepítése** a DPM felügyeleti konzolján.</span><span class="sxs-lookup"><span data-stu-id="a9676-121">Install the agent on the Exchange server by clicking **Management > Agents > Install** in DPM Administrator Console.</span></span> <span data-ttu-id="a9676-122">Lásd: [a DPM védelmi ügynök telepítése](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) a részletes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a9676-122">See [Install the DPM protection agent](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) for detailed steps.</span></span>

## <a name="create-a-protection-group-for-the-exchange-server"></a><span data-ttu-id="a9676-123">Az Exchange-kiszolgáló védelmi csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="a9676-123">Create a protection group for the Exchange server</span></span>
1. <span data-ttu-id="a9676-124">A DPM felügyeleti konzolján kattintson **védelmi**, és kattintson a **új** megnyitásához az eszközsávon a **új védelmi csoport létrehozása** varázsló.</span><span class="sxs-lookup"><span data-stu-id="a9676-124">In the DPM Administrator Console, click **Protection**, and then click **New** on the tool ribbon to open the **Create New Protection Group** wizard.</span></span>
2. <span data-ttu-id="a9676-125">Az a **üdvözlő** képernyőjén kattintson a varázsló **következő**.</span><span class="sxs-lookup"><span data-stu-id="a9676-125">On the **Welcome** screen of the wizard click **Next**.</span></span>
3. <span data-ttu-id="a9676-126">Az a **védelmi csoport típusának kiválasztása** képernyőn, jelölje be **kiszolgálók** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="a9676-126">On the **Select protection group type** screen, select **Servers** and click **Next**.</span></span>
4. <span data-ttu-id="a9676-127">Válassza ki az Exchange server-adatbázis védelmét, és kattintson a kívánt **következő**.</span><span class="sxs-lookup"><span data-stu-id="a9676-127">Select the Exchange server database that you want to protect and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a9676-128">Ha az Exchange 2013 védelmét, ellenőrizze a [Exchange 2013 előfeltételei](https://technet.microsoft.com/library/dn751029.aspx).</span><span class="sxs-lookup"><span data-stu-id="a9676-128">If you are protecting Exchange 2013, check the [Exchange 2013 prerequisites](https://technet.microsoft.com/library/dn751029.aspx).</span></span>
   >
   >

    <span data-ttu-id="a9676-129">A következő példában az Exchange 2010 adatbázis van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="a9676-129">In the following example, the Exchange 2010 database is selected.</span></span>

    ![Csoporttagok kiválasztása](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. <span data-ttu-id="a9676-131">Az adatvédelmi módszer kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="a9676-131">Select the data protection method.</span></span>

    <span data-ttu-id="a9676-132">A védelmi csoport neve, majd válassza ki a következők mindegyikét:</span><span class="sxs-lookup"><span data-stu-id="a9676-132">Name the protection group, and then select both of the following options:</span></span>

   * <span data-ttu-id="a9676-133">Rövid távú lemezes védelmet szeretnék.</span><span class="sxs-lookup"><span data-stu-id="a9676-133">I want short-term protection using Disk.</span></span>
   * <span data-ttu-id="a9676-134">Online védelmet szeretnék.</span><span class="sxs-lookup"><span data-stu-id="a9676-134">I want online protection.</span></span>
6. <span data-ttu-id="a9676-135">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a9676-135">Click **Next**.</span></span>
7. <span data-ttu-id="a9676-136">Válassza ki a **Eseutil futtatása az adatok sértetlenségének ellenőrzéséhez** lehetőséget, ha azt szeretné, hogy az Exchange Server-adatbázisok sértetlenségének ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="a9676-136">Select the **Run Eseutil to check data integrity** option if you want to check the integrity of the Exchange Server databases.</span></span>

    <span data-ttu-id="a9676-137">Miután kiválasztotta ezt a beállítást, biztonsági mentés konzisztencia-ellenőrzést futni fog a DPM-kiszolgálón az i/o-forgalmat futtatásával elkerülése érdekében a **eseutil** parancsot az Exchange-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a9676-137">After you select this option, backup consistency checking will be run on the DPM server to avoid the I/O traffic that’s generated by running the **eseutil** command on the Exchange server.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a9676-138">Használja ezt a beállítást, át kell másolnia az Ese.dll és az Eseutil.exe fájloknak a C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin könyvtárba, a DPM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a9676-138">To use this option, you must copy the Ese.dll and Eseutil.exe files to the C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin directory on the DPM server.</span></span> <span data-ttu-id="a9676-139">Ellenkező esetben aktiválódik, a következő hibával:</span><span class="sxs-lookup"><span data-stu-id="a9676-139">Otherwise, the following error is triggered:</span></span>  
   > <span data-ttu-id="a9676-140">![az Eseutil hiba](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span><span class="sxs-lookup"><span data-stu-id="a9676-140">![eseutil error](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span></span>
   >
   >
8. <span data-ttu-id="a9676-141">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a9676-141">Click **Next**.</span></span>
9. <span data-ttu-id="a9676-142">Válassza ki az adatbázist a **másolásos biztonsági mentésre**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a9676-142">Select the database for **Copy Backup**, and then click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a9676-143">Ha nincs bejelölve a "Teljes biztonsági másolat" adatbázis másolatának legalább egy DAG, naplók nem lesznek csonkolva.</span><span class="sxs-lookup"><span data-stu-id="a9676-143">If you do not select “Full backup” for at least one DAG copy of a database, logs will not be truncated.</span></span>
   >
   >
10. <span data-ttu-id="a9676-144">Konfigurálja a célokat **rövid távú biztonsági mentés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a9676-144">Configure the goals for **Short-Term backup**, and then click **Next**.</span></span>
11. <span data-ttu-id="a9676-145">Tekintse át a rendelkezésre álló lemezterület, majd a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a9676-145">Review the available disk space, and then click **Next**.</span></span>
12. <span data-ttu-id="a9676-146">Válassza ki a időpont, amikor a DPM-kiszolgáló fog létrehozni a kezdeti replikálás, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a9676-146">Select the time at which the DPM server will create the initial replication, and then click **Next**.</span></span>
13. <span data-ttu-id="a9676-147">A konzisztencia-ellenőrzési beállítások kiválasztása, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="a9676-147">Select the consistency check options, and then click **Next**.</span></span>
14. <span data-ttu-id="a9676-148">Adja meg az adatbázis biztonsági mentése az Azure-ba, és kattintson a kívánt **következő**.</span><span class="sxs-lookup"><span data-stu-id="a9676-148">Choose the database that you want to back up to Azure, and then click **Next**.</span></span> <span data-ttu-id="a9676-149">Példa:</span><span class="sxs-lookup"><span data-stu-id="a9676-149">For example:</span></span>

    ![Online védelem adatainak megadása](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. <span data-ttu-id="a9676-151">Adja meg a ütemezését **Azure biztonsági mentés**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="a9676-151">Define the schedule for **Azure Backup**, and then click **Next**.</span></span> <span data-ttu-id="a9676-152">Példa:</span><span class="sxs-lookup"><span data-stu-id="a9676-152">For example:</span></span>

    ![Adja meg az online biztonsági mentés ütemezése](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="a9676-154">Jegyezze fel az Online helyreállítási pontok alapuló Expressz teljes helyreállítási pontokat.</span><span class="sxs-lookup"><span data-stu-id="a9676-154">Note Online recovery points are based on express full recovery points.</span></span> <span data-ttu-id="a9676-155">Az online helyreállítási pontot, ezért úgy kell ütemeznie után az idő megadott az expressz teljes helyreállítási pont.</span><span class="sxs-lookup"><span data-stu-id="a9676-155">Therefore, you must schedule the online recovery point after the time that’s specified for the express full recovery point.</span></span>
    >
    >
16. <span data-ttu-id="a9676-156">Konfigurálja az adatmegőrzési **Azure biztonsági mentés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a9676-156">Configure the retention policy for **Azure Backup**, and then click **Next**.</span></span>
17. <span data-ttu-id="a9676-157">Válasszon egy online replikációs lehetőséget, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a9676-157">Choose an online replication option and click **Next**.</span></span>

    <span data-ttu-id="a9676-158">Ha nagy adatbázis, a kezdeti biztonsági másolatot a hálózaton keresztül a létrehozandó hosszú ideig eltarthat.</span><span class="sxs-lookup"><span data-stu-id="a9676-158">If you have a large database, it could take a long time for the initial backup to be created over the network.</span></span> <span data-ttu-id="a9676-159">A probléma elkerülése érdekében, létrehozhat egy offline biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="a9676-159">To avoid this issue, you can create an offline backup.</span></span>  

    ![Online megőrzési szabály megadása](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. <span data-ttu-id="a9676-161">Hagyja jóvá a beállításokat, és kattintson a **csoport létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a9676-161">Confirm the settings, and then click **Create Group**.</span></span>
19. <span data-ttu-id="a9676-162">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a9676-162">Click **Close**.</span></span>

## <a name="recover-the-exchange-database"></a><span data-ttu-id="a9676-163">Az Exchange-adatbázis helyreállítása</span><span class="sxs-lookup"><span data-stu-id="a9676-163">Recover the Exchange database</span></span>
1. <span data-ttu-id="a9676-164">Exchange-adatbázis helyreállítása, kattintson a **helyreállítási** a DPM felügyeleti konzolon.</span><span class="sxs-lookup"><span data-stu-id="a9676-164">To recover an Exchange database, click **Recovery** in the DPM Administrator Console.</span></span>
2. <span data-ttu-id="a9676-165">Keresse meg a helyreállítani kívánt Exchange-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a9676-165">Locate the Exchange database that you want to recover.</span></span>
3. <span data-ttu-id="a9676-166">Válassza ki az online helyreállítási pontot a *helyreállításkor* legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="a9676-166">Select an online recovery point from the *recovery time* drop-down list.</span></span>
4. <span data-ttu-id="a9676-167">Kattintson a **helyreállítása** elindítani a **helyreállítási varázsló**.</span><span class="sxs-lookup"><span data-stu-id="a9676-167">Click **Recover** to start the **Recovery Wizard**.</span></span>

<span data-ttu-id="a9676-168">Az online helyreállítási pontok, öt helyreállítási típusa van:</span><span class="sxs-lookup"><span data-stu-id="a9676-168">For online recovery points, there are five recovery types:</span></span>

* <span data-ttu-id="a9676-169">**Helyreállítás az eredeti Exchange-kiszolgálón:** fogja visszaállítani az adatokat az eredeti Exchange-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="a9676-169">**Recover to original Exchange Server location:** The data will be recovered to the original Exchange server.</span></span>
* <span data-ttu-id="a9676-170">**Helyreállítás az Exchange-kiszolgáló egy másik adatbázisba:** fogja visszaállítani az adatokat egy másik egy másik Exchange server-adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="a9676-170">**Recover to another database on an Exchange Server:** The data will be recovered to another database on another Exchange server.</span></span>
* <span data-ttu-id="a9676-171">**Helyreállítás helyreállítási adatbázisba:** lesz az adatok helyreállítása egy Exchange helyreállítási adatbázis (Rekordadatbázis).</span><span class="sxs-lookup"><span data-stu-id="a9676-171">**Recover to a Recovery Database:** The data will be recovered to an Exchange Recovery Database (RDB).</span></span>
* <span data-ttu-id="a9676-172">**Másolás hálózati mappába:** fogja visszaállítani az adatokat egy hálózati mappába.</span><span class="sxs-lookup"><span data-stu-id="a9676-172">**Copy to a network folder:** The data will be recovered to a network folder.</span></span>
* <span data-ttu-id="a9676-173">**Másolás szalagra:** egy szalagtárat vagy egy önálló szalagos meghajtót csatlakoztatott és a DPM-kiszolgálón konfigurálva van, ha a helyreállítási pont egy szabad szalagra kerülnek-e.</span><span class="sxs-lookup"><span data-stu-id="a9676-173">**Copy to tape:** If you have a tape library or a stand-alone tape drive attached and configured on the DPM server, the recovery point will be copied to a free tape.</span></span>

    ![Válassza ki az online replikációs](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a><span data-ttu-id="a9676-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a9676-175">Next steps</span></span>
* [<span data-ttu-id="a9676-176">Az Azure biztonsági mentési – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="a9676-176">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
