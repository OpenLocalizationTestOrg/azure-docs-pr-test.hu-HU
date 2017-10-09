---
title: "az Exchange server tooAzure Azure Backup Server biztonsági másolatának mentése aaaBack |} Microsoft Docs"
description: "Megtudhatja, hogyan mentése az Exchange server tooAzure tooback biztonsági mentése Azure Backup Server használatával"
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
ms.openlocfilehash: db874161151fc57c5b79c41531e18d577f567f66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-an-exchange-server-tooazure-backup-with-azure-backup-server"></a><span data-ttu-id="0c47d-103">Készítsen biztonsági másolatot az Exchange server tooAzure biztonsági mentéshez az Azure Backup Server</span><span class="sxs-lookup"><span data-stu-id="0c47d-103">Back up an Exchange server tooAzure Backup with Azure Backup Server</span></span>
<span data-ttu-id="0c47d-104">Ez a cikk ismerteti, hogyan tooconfigure Microsoft Azure Backup Server (MABS) tooback be egy Microsoft Exchange server tooAzure.</span><span class="sxs-lookup"><span data-stu-id="0c47d-104">This article describes how tooconfigure Microsoft Azure Backup Server (MABS) tooback up a Microsoft Exchange server tooAzure.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="0c47d-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0c47d-105">Prerequisites</span></span>
<span data-ttu-id="0c47d-106">A folytatás előtt győződjön meg arról, hogy Azure Backup Server [telepítve és előkészített](backup-azure-microsoft-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="0c47d-106">Before you continue, make sure that Azure Backup Server is [installed and prepared](backup-azure-microsoft-azure-backup.md).</span></span>

## <a name="mabs-protection-agent"></a><span data-ttu-id="0c47d-107">MABS védelmi ügynök</span><span class="sxs-lookup"><span data-stu-id="0c47d-107">MABS protection agent</span></span>
<span data-ttu-id="0c47d-108">tooinstall hello MABS védelmi ügynök hello Exchange-kiszolgálón, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0c47d-108">tooinstall hello MABS protection agent on hello Exchange server, follow these steps:</span></span>

1. <span data-ttu-id="0c47d-109">Győződjön meg arról, hogy hello tűzfalak helyesen van-e konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="0c47d-109">Make sure that hello firewalls are correctly configured.</span></span> <span data-ttu-id="0c47d-110">Lásd: [hello ügynök tűzfalkivétel konfigurálása](https://technet.microsoft.com/library/Hh758204.aspx).</span><span class="sxs-lookup"><span data-stu-id="0c47d-110">See [Configure firewall exceptions for hello agent](https://technet.microsoft.com/library/Hh758204.aspx).</span></span>
2. <span data-ttu-id="0c47d-111">Hello ügynök telepíthető hello Exchange-kiszolgálóhoz gombra kattintva **felügyeleti > ügynökökkel > telepítése** MABS felügyeleti konzolon.</span><span class="sxs-lookup"><span data-stu-id="0c47d-111">Install hello agent on hello Exchange server by clicking **Management > Agents > Install** in MABS Administrator Console.</span></span> <span data-ttu-id="0c47d-112">Lásd: [hello MABS védelmi ügynök telepítéséhez](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) a részletes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="0c47d-112">See [Install hello MABS protection agent](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) for detailed steps.</span></span>

## <a name="create-a-protection-group-for-hello-exchange-server"></a><span data-ttu-id="0c47d-113">Hello Exchange-kiszolgáló védelmi csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="0c47d-113">Create a protection group for hello Exchange server</span></span>
1. <span data-ttu-id="0c47d-114">Hello MABS felügyeleti konzolt, kattintson **védelmi**, és kattintson a **új** a hello eszköz menüszalag tooopen hello **új védelmi csoport létrehozása** varázsló.</span><span class="sxs-lookup"><span data-stu-id="0c47d-114">In hello MABS Administrator Console, click **Protection**, and then click **New** on hello tool ribbon tooopen hello **Create New Protection Group** wizard.</span></span>
2. <span data-ttu-id="0c47d-115">A hello **üdvözlő** hello varázslóban kattintson a képernyő **következő**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-115">On hello **Welcome** screen of hello wizard click **Next**.</span></span>
3. <span data-ttu-id="0c47d-116">A hello **védelmi csoport típusának kiválasztása** képernyőn, jelölje be **kiszolgálók** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-116">On hello **Select protection group type** screen, select **Servers** and click **Next**.</span></span>
4. <span data-ttu-id="0c47d-117">Jelölje be hello Exchange server-adatbázis tooprotect szeretné, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-117">Select hello Exchange server database that you want tooprotect and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0c47d-118">Ha az Exchange 2013 védelmét, ellenőrizze a hello [Exchange 2013 előfeltételei](https://technet.microsoft.com/library/dn751029.aspx).</span><span class="sxs-lookup"><span data-stu-id="0c47d-118">If you are protecting Exchange 2013, check hello [Exchange 2013 prerequisites](https://technet.microsoft.com/library/dn751029.aspx).</span></span>
   >
   >

    <span data-ttu-id="0c47d-119">A következő példa hello hello Exchange 2010 adatbázis van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="0c47d-119">In hello following example, hello Exchange 2010 database is selected.</span></span>

    ![Csoporttagok kiválasztása](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. <span data-ttu-id="0c47d-121">Hello adatvédelmi módszer kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="0c47d-121">Select hello data protection method.</span></span>

    <span data-ttu-id="0c47d-122">Hello védelmi csoport neve, majd válassza ki mindkét alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="0c47d-122">Name hello protection group, and then select both of hello following options:</span></span>

   * <span data-ttu-id="0c47d-123">Rövid távú lemezes védelmet szeretnék.</span><span class="sxs-lookup"><span data-stu-id="0c47d-123">I want short-term protection using Disk.</span></span>
   * <span data-ttu-id="0c47d-124">Online védelmet szeretnék.</span><span class="sxs-lookup"><span data-stu-id="0c47d-124">I want online protection.</span></span>
6. <span data-ttu-id="0c47d-125">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="0c47d-125">Click **Next**.</span></span>
7. <span data-ttu-id="0c47d-126">Jelölje be hello **futtassa az Eseutil toocheck adatintegritást** lehetőséget, ha azt szeretné, hogy az Exchange Server-adatbázisok hello toocheck hello integritását.</span><span class="sxs-lookup"><span data-stu-id="0c47d-126">Select hello **Run Eseutil toocheck data integrity** option if you want toocheck hello integrity of hello Exchange Server databases.</span></span>

    <span data-ttu-id="0c47d-127">Miután kiválasztotta ezt a beállítást, biztonsági mentés konzisztencia-ellenőrzést fog futni a MABS tooavoid hello i/o-forgalmat hello futtatásával **eseutil** parancs hello Exchange-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="0c47d-127">After you select this option, backup consistency checking will be run on MABS tooavoid hello I/O traffic that’s generated by running hello **eseutil** command on hello Exchange server.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0c47d-128">toouse ezt a beállítást, hello Ese.dll és az Eseutil.exe fájlok toohello C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin könyvtár hello MAB kiszolgálón kell átmásolnia.</span><span class="sxs-lookup"><span data-stu-id="0c47d-128">toouse this option, you must copy hello Ese.dll and Eseutil.exe files toohello C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin directory on hello MAB server.</span></span> <span data-ttu-id="0c47d-129">Ellenkező esetben a következő hiba hello akkor váltódik ki:</span><span class="sxs-lookup"><span data-stu-id="0c47d-129">Otherwise, hello following error is triggered:</span></span>  
   > <span data-ttu-id="0c47d-130">![az Eseutil hiba](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span><span class="sxs-lookup"><span data-stu-id="0c47d-130">![eseutil error](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span></span>
   >
   >
8. <span data-ttu-id="0c47d-131">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="0c47d-131">Click **Next**.</span></span>
9. <span data-ttu-id="0c47d-132">A SELECT hello adatbázis **másolásos biztonsági mentésre**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-132">Select hello database for **Copy Backup**, and then click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0c47d-133">Ha nincs bejelölve a "Teljes biztonsági másolat" adatbázis másolatának legalább egy DAG, naplók nem lesznek csonkolva.</span><span class="sxs-lookup"><span data-stu-id="0c47d-133">If you do not select “Full backup” for at least one DAG copy of a database, logs will not be truncated.</span></span>
   >
   >
10. <span data-ttu-id="0c47d-134">Hello célokat konfigurálása **rövid távú biztonsági mentés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-134">Configure hello goals for **Short-Term backup**, and then click **Next**.</span></span>
11. <span data-ttu-id="0c47d-135">Tekintse át a hello rendelkezésre álló szabad lemezterület, majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-135">Review hello available disk space, and then click **Next**.</span></span>
12. <span data-ttu-id="0c47d-136">Válassza ki a hello idő, mely hello MAB kiszolgáló létrehoz hello kezdeti replikálást, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-136">Select hello time at which hello MAB Server will create hello initial replication, and then click **Next**.</span></span>
13. <span data-ttu-id="0c47d-137">Hello konzisztencia-ellenőrzési beállítások kiválasztása, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-137">Select hello consistency check options, and then click **Next**.</span></span>
14. <span data-ttu-id="0c47d-138">Adja meg, hogy szeretné, hogy tooback tooAzure fel, és kattintson hello adatbázis **következő**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-138">Choose hello database that you want tooback up tooAzure, and then click **Next**.</span></span> <span data-ttu-id="0c47d-139">Példa:</span><span class="sxs-lookup"><span data-stu-id="0c47d-139">For example:</span></span>

    ![Online védelem adatainak megadása](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. <span data-ttu-id="0c47d-141">Hello ütemezésének megadása **Azure biztonsági mentés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-141">Define hello schedule for **Azure Backup**, and then click **Next**.</span></span> <span data-ttu-id="0c47d-142">Példa:</span><span class="sxs-lookup"><span data-stu-id="0c47d-142">For example:</span></span>

    ![Adja meg az online biztonsági mentés ütemezése](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="0c47d-144">Jegyezze fel az Online helyreállítási pontok alapuló Expressz teljes helyreállítási pontokat.</span><span class="sxs-lookup"><span data-stu-id="0c47d-144">Note Online recovery points are based on express full recovery points.</span></span> <span data-ttu-id="0c47d-145">Ezért úgy kell ütemeznie hello online helyreállítási pont hello később megadott hello az expressz teljes helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="0c47d-145">Therefore, you must schedule hello online recovery point after hello time that’s specified for hello express full recovery point.</span></span>
    >
    >
16. <span data-ttu-id="0c47d-146">Hello megőrzési házirend konfigurálásában az **Azure biztonsági mentés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-146">Configure hello retention policy for **Azure Backup**, and then click **Next**.</span></span>
17. <span data-ttu-id="0c47d-147">Válasszon egy online replikációs lehetőséget, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-147">Choose an online replication option and click **Next**.</span></span>

    <span data-ttu-id="0c47d-148">Ha nagy adatbázis, a kezdeti biztonsági mentési toobe hello hello hálózaton keresztül létrehozott hosszú ideig eltarthat.</span><span class="sxs-lookup"><span data-stu-id="0c47d-148">If you have a large database, it could take a long time for hello initial backup toobe created over hello network.</span></span> <span data-ttu-id="0c47d-149">tooavoid probléma hozhat létre offline biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="0c47d-149">tooavoid this issue, you can create an offline backup.</span></span>  

    ![Online megőrzési szabály megadása](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. <span data-ttu-id="0c47d-151">Erősítse meg hello beállításait, és kattintson a **csoport létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-151">Confirm hello settings, and then click **Create Group**.</span></span>
19. <span data-ttu-id="0c47d-152">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0c47d-152">Click **Close**.</span></span>

## <a name="recover-hello-exchange-database"></a><span data-ttu-id="0c47d-153">Hello Exchange-adatbázis helyreállítása</span><span class="sxs-lookup"><span data-stu-id="0c47d-153">Recover hello Exchange database</span></span>
1. <span data-ttu-id="0c47d-154">Kattintson az Exchange-adatbázis toorecover **helyreállítási** hello MABS felügyeleti konzol a.</span><span class="sxs-lookup"><span data-stu-id="0c47d-154">toorecover an Exchange database, click **Recovery** in hello MABS Administrator Console.</span></span>
2. <span data-ttu-id="0c47d-155">Keresse meg, hogy szeretné-e toorecover hello Exchange-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="0c47d-155">Locate hello Exchange database that you want toorecover.</span></span>
3. <span data-ttu-id="0c47d-156">Az online helyreállítási pontot válasszon hello *helyreállításkor* legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="0c47d-156">Select an online recovery point from hello *recovery time* drop-down list.</span></span>
4. <span data-ttu-id="0c47d-157">Kattintson a **helyreállítása** toostart hello **helyreállítási varázsló**.</span><span class="sxs-lookup"><span data-stu-id="0c47d-157">Click **Recover** toostart hello **Recovery Wizard**.</span></span>

<span data-ttu-id="0c47d-158">Az online helyreállítási pontok, öt helyreállítási típusa van:</span><span class="sxs-lookup"><span data-stu-id="0c47d-158">For online recovery points, there are five recovery types:</span></span>

* <span data-ttu-id="0c47d-159">**Helyreállítás toooriginal Exchange-kiszolgálón:** hello adatokat kell helyreállított toohello eredeti Exchange-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="0c47d-159">**Recover toooriginal Exchange Server location:** hello data will be recovered toohello original Exchange server.</span></span>
* <span data-ttu-id="0c47d-160">**Exchange Server tooanother adatbázis helyreállítása:** hello adatokat kell helyreállított tooanother egy másik Exchange server-adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="0c47d-160">**Recover tooanother database on an Exchange Server:** hello data will be recovered tooanother database on another Exchange server.</span></span>
* <span data-ttu-id="0c47d-161">**Helyreállítás helyreállítási adatbázisba tooa:** hello adatokat fogja a helyreállított tooan Exchange helyreállítási adatbázis (Rekordadatbázis).</span><span class="sxs-lookup"><span data-stu-id="0c47d-161">**Recover tooa Recovery Database:** hello data will be recovered tooan Exchange Recovery Database (RDB).</span></span>
* <span data-ttu-id="0c47d-162">**Tooa hálózati mappa másolása:** hello adatokat fogja a helyreállított tooa hálózati mappába.</span><span class="sxs-lookup"><span data-stu-id="0c47d-162">**Copy tooa network folder:** hello data will be recovered tooa network folder.</span></span>
* <span data-ttu-id="0c47d-163">**Másolja a tootape:** Ha rendelkezik egy szalagtárat, vagy egy önálló szalagos meghajtót csatlakoztatott és a beállított MABS, hello helyreállítási pont lesz tooa szabad szalagra másolni.</span><span class="sxs-lookup"><span data-stu-id="0c47d-163">**Copy tootape:** If you have a tape library or a stand-alone tape drive attached and configured on MABS, hello recovery point will be copied tooa free tape.</span></span>

    ![Válassza ki az online replikációs](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a><span data-ttu-id="0c47d-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0c47d-165">Next steps</span></span>
* [<span data-ttu-id="0c47d-166">Az Azure biztonsági mentési – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="0c47d-166">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
