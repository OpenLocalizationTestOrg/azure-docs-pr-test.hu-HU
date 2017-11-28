---
title: "az Exchange server tooAzure System Center 2012 R2 DPM biztonsági másolatának mentése aaaBack |} Microsoft Docs"
description: "Megtudhatja, hogyan mentése az Exchange server tooAzure tooback biztonsági mentését, a System Center 2012 R2 DPM használatával"
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
ms.openlocfilehash: fa99296d095c180333474b6d419ebc5ec727547a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-an-exchange-server-tooazure-backup-with-system-center-2012-r2-dpm"></a><span data-ttu-id="e4e3c-103">Készítsen biztonsági másolatot az Exchange server tooAzure a System Center 2012 R2 DPM biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="e4e3c-103">Back up an Exchange server tooAzure Backup with System Center 2012 R2 DPM</span></span>
<span data-ttu-id="e4e3c-104">Ez a cikk ismerteti, hogyan tooconfigure be a Microsoft Exchange-kiszolgálót a System Center 2012 R2 Data Protection Manager (DPM) kiszolgáló tooback túl Azure biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-104">This article describes how tooconfigure a System Center 2012 R2 Data Protection Manager (DPM) server tooback up a Microsoft Exchange server too Azure Backup.</span></span>  

## <a name="updates"></a><span data-ttu-id="e4e3c-105">Frissítések</span><span class="sxs-lookup"><span data-stu-id="e4e3c-105">Updates</span></span>
<span data-ttu-id="e4e3c-106">toosuccessfully register hello DPM-kiszolgáló Azure Backup szolgáltatásban, telepítenie kell a hello legújabb kumulatív frissítés a System Center 2012 R2 DPM és a hello hello Azure Backup szolgáltatás ügynökének legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-106">toosuccessfully register hello DPM server with Azure Backup, you must install hello latest update rollup for System Center 2012 R2 DPM and hello latest version of hello Azure Backup Agent.</span></span> <span data-ttu-id="e4e3c-107">Hello legújabb kumulatív beszerezni hello [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).</span><span class="sxs-lookup"><span data-stu-id="e4e3c-107">Get hello latest update rollup from hello [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).</span></span>

> [!NOTE]
> <span data-ttu-id="e4e3c-108">A cikkben szereplő példák hello hello Azure Backup szolgáltatás ügynökének 2.0.8719.0 verziója van telepítve, és 6. kumulatív frissítés a System Center 2012 R2 DPM telepítve van.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-108">For hello examples in this article, version 2.0.8719.0 of hello Azure Backup Agent is installed, and Update Rollup 6 is installed on System Center 2012 R2 DPM.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="e4e3c-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e4e3c-109">Prerequisites</span></span>
<span data-ttu-id="e4e3c-110">A folytatás előtt győződjön meg arról, hogy az összes hello [Előfeltételek](backup-azure-dpm-introduction.md#prerequisites) a Microsoft Azure Backup segítségével tooprotect munkaterhelések teljesült.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-110">Before you continue, make sure that all hello [prerequisites](backup-azure-dpm-introduction.md#prerequisites) for using Microsoft Azure Backup tooprotect workloads have been met.</span></span> <span data-ttu-id="e4e3c-111">Az Előfeltételek hello alábbiakat foglalja magába:</span><span class="sxs-lookup"><span data-stu-id="e4e3c-111">These prerequisites include hello following:</span></span>

* <span data-ttu-id="e4e3c-112">Egy hello Azure hely a biztonsági mentési tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-112">A backup vault on hello Azure site has been created.</span></span>
* <span data-ttu-id="e4e3c-113">Ügynök és a tároló hitelesítő adatok már letöltött toohello DPM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-113">Agent and vault credentials have been downloaded toohello DPM server.</span></span>
* <span data-ttu-id="e4e3c-114">hello ügynök hello DPM-kiszolgálóra van telepítve.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-114">hello agent is installed on hello DPM server.</span></span>
* <span data-ttu-id="e4e3c-115">hello tárolói hitelesítő adatok lettek használt tooregister hello DPM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-115">hello vault credentials were used tooregister hello DPM server.</span></span>
* <span data-ttu-id="e4e3c-116">Exchange 2016 véd, akkor frissítse a 2012 R2 UR9 tooDPM vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="e4e3c-116">If you are protecting Exchange 2016, please upgrade tooDPM 2012 R2 UR9 or later</span></span>

## <a name="dpm-protection-agent"></a><span data-ttu-id="e4e3c-117">A DPM védelmi ügynök</span><span class="sxs-lookup"><span data-stu-id="e4e3c-117">DPM protection agent</span></span>
<span data-ttu-id="e4e3c-118">tooinstall hello DPM védelmi ügynök hello Exchange-kiszolgálón, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e4e3c-118">tooinstall hello DPM protection agent on hello Exchange server, follow these steps:</span></span>

1. <span data-ttu-id="e4e3c-119">Győződjön meg arról, hogy hello tűzfalak helyesen van-e konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-119">Make sure that hello firewalls are correctly configured.</span></span> <span data-ttu-id="e4e3c-120">Lásd: [hello ügynök tűzfalkivétel konfigurálása](https://technet.microsoft.com/library/Hh758204.aspx).</span><span class="sxs-lookup"><span data-stu-id="e4e3c-120">See [Configure firewall exceptions for hello agent](https://technet.microsoft.com/library/Hh758204.aspx).</span></span>
2. <span data-ttu-id="e4e3c-121">Hello ügynök telepíthető hello Exchange-kiszolgálóhoz gombra kattintva **felügyeleti > ügynökökkel > telepítése** a DPM felügyeleti konzolján.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-121">Install hello agent on hello Exchange server by clicking **Management > Agents > Install** in DPM Administrator Console.</span></span> <span data-ttu-id="e4e3c-122">Lásd: [hello DPM védelmi ügynök telepítéséhez](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) a részletes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-122">See [Install hello DPM protection agent](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) for detailed steps.</span></span>

## <a name="create-a-protection-group-for-hello-exchange-server"></a><span data-ttu-id="e4e3c-123">Hello Exchange-kiszolgáló védelmi csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4e3c-123">Create a protection group for hello Exchange server</span></span>
1. <span data-ttu-id="e4e3c-124">Hello DPM felügyeleti konzolt, kattintson **védelmi**, és kattintson a **új** a hello eszköz menüszalag tooopen hello **új védelmi csoport létrehozása** varázsló.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-124">In hello DPM Administrator Console, click **Protection**, and then click **New** on hello tool ribbon tooopen hello **Create New Protection Group** wizard.</span></span>
2. <span data-ttu-id="e4e3c-125">A hello **üdvözlő** hello varázslóban kattintson a képernyő **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-125">On hello **Welcome** screen of hello wizard click **Next**.</span></span>
3. <span data-ttu-id="e4e3c-126">A hello **védelmi csoport típusának kiválasztása** képernyőn, jelölje be **kiszolgálók** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-126">On hello **Select protection group type** screen, select **Servers** and click **Next**.</span></span>
4. <span data-ttu-id="e4e3c-127">Jelölje be hello Exchange server-adatbázis tooprotect szeretné, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-127">Select hello Exchange server database that you want tooprotect and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e4e3c-128">Ha az Exchange 2013 védelmét, ellenőrizze a hello [Exchange 2013 előfeltételei](https://technet.microsoft.com/library/dn751029.aspx).</span><span class="sxs-lookup"><span data-stu-id="e4e3c-128">If you are protecting Exchange 2013, check hello [Exchange 2013 prerequisites](https://technet.microsoft.com/library/dn751029.aspx).</span></span>
   >
   >

    <span data-ttu-id="e4e3c-129">A következő példa hello hello Exchange 2010 adatbázis van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-129">In hello following example, hello Exchange 2010 database is selected.</span></span>

    ![Csoporttagok kiválasztása](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. <span data-ttu-id="e4e3c-131">Hello adatvédelmi módszer kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-131">Select hello data protection method.</span></span>

    <span data-ttu-id="e4e3c-132">Hello védelmi csoport neve, majd válassza ki mindkét alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="e4e3c-132">Name hello protection group, and then select both of hello following options:</span></span>

   * <span data-ttu-id="e4e3c-133">Rövid távú lemezes védelmet szeretnék.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-133">I want short-term protection using Disk.</span></span>
   * <span data-ttu-id="e4e3c-134">Online védelmet szeretnék.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-134">I want online protection.</span></span>
6. <span data-ttu-id="e4e3c-135">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-135">Click **Next**.</span></span>
7. <span data-ttu-id="e4e3c-136">Jelölje be hello **futtassa az Eseutil toocheck adatintegritást** lehetőséget, ha azt szeretné, hogy az Exchange Server-adatbázisok hello toocheck hello integritását.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-136">Select hello **Run Eseutil toocheck data integrity** option if you want toocheck hello integrity of hello Exchange Server databases.</span></span>

    <span data-ttu-id="e4e3c-137">Miután kiválasztotta ezt a beállítást, biztonsági mentés konzisztencia-ellenőrzést fog futni a hello DPM server tooavoid hello i/o-forgalmat hello futtatásával **eseutil** parancs hello Exchange-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-137">After you select this option, backup consistency checking will be run on hello DPM server tooavoid hello I/O traffic that’s generated by running hello **eseutil** command on hello Exchange server.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e4e3c-138">toouse ezt a beállítást, át kell másolnia hello Ese.dll és az Eseutil.exe fájlok toohello C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin directory hello DPM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-138">toouse this option, you must copy hello Ese.dll and Eseutil.exe files toohello C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin directory on hello DPM server.</span></span> <span data-ttu-id="e4e3c-139">Ellenkező esetben a következő hiba hello akkor váltódik ki:</span><span class="sxs-lookup"><span data-stu-id="e4e3c-139">Otherwise, hello following error is triggered:</span></span>  
   > <span data-ttu-id="e4e3c-140">![az Eseutil hiba](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span><span class="sxs-lookup"><span data-stu-id="e4e3c-140">![eseutil error](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span></span>
   >
   >
8. <span data-ttu-id="e4e3c-141">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-141">Click **Next**.</span></span>
9. <span data-ttu-id="e4e3c-142">A SELECT hello adatbázis **másolásos biztonsági mentésre**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-142">Select hello database for **Copy Backup**, and then click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e4e3c-143">Ha nincs bejelölve a "Teljes biztonsági másolat" adatbázis másolatának legalább egy DAG, naplók nem lesznek csonkolva.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-143">If you do not select “Full backup” for at least one DAG copy of a database, logs will not be truncated.</span></span>
   >
   >
10. <span data-ttu-id="e4e3c-144">Hello célokat konfigurálása **rövid távú biztonsági mentés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-144">Configure hello goals for **Short-Term backup**, and then click **Next**.</span></span>
11. <span data-ttu-id="e4e3c-145">Tekintse át a hello rendelkezésre álló szabad lemezterület, majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-145">Review hello available disk space, and then click **Next**.</span></span>
12. <span data-ttu-id="e4e3c-146">Válassza ki a hello idő, mely hello DPM kiszolgáló létrehoz hello kezdeti replikálást, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-146">Select hello time at which hello DPM server will create hello initial replication, and then click **Next**.</span></span>
13. <span data-ttu-id="e4e3c-147">Hello konzisztencia-ellenőrzési beállítások kiválasztása, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-147">Select hello consistency check options, and then click **Next**.</span></span>
14. <span data-ttu-id="e4e3c-148">Adja meg, hogy szeretné, hogy tooback tooAzure fel, és kattintson hello adatbázis **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-148">Choose hello database that you want tooback up tooAzure, and then click **Next**.</span></span> <span data-ttu-id="e4e3c-149">Példa:</span><span class="sxs-lookup"><span data-stu-id="e4e3c-149">For example:</span></span>

    ![Online védelem adatainak megadása](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. <span data-ttu-id="e4e3c-151">Hello ütemezésének megadása **Azure biztonsági mentés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-151">Define hello schedule for **Azure Backup**, and then click **Next**.</span></span> <span data-ttu-id="e4e3c-152">Példa:</span><span class="sxs-lookup"><span data-stu-id="e4e3c-152">For example:</span></span>

    ![Adja meg az online biztonsági mentés ütemezése](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="e4e3c-154">Jegyezze fel az Online helyreállítási pontok alapuló Expressz teljes helyreállítási pontokat.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-154">Note Online recovery points are based on express full recovery points.</span></span> <span data-ttu-id="e4e3c-155">Ezért úgy kell ütemeznie hello online helyreállítási pont hello később megadott hello az expressz teljes helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-155">Therefore, you must schedule hello online recovery point after hello time that’s specified for hello express full recovery point.</span></span>
    >
    >
16. <span data-ttu-id="e4e3c-156">Hello megőrzési házirend konfigurálásában az **Azure biztonsági mentés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-156">Configure hello retention policy for **Azure Backup**, and then click **Next**.</span></span>
17. <span data-ttu-id="e4e3c-157">Válasszon egy online replikációs lehetőséget, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-157">Choose an online replication option and click **Next**.</span></span>

    <span data-ttu-id="e4e3c-158">Ha nagy adatbázis, a kezdeti biztonsági mentési toobe hello hello hálózaton keresztül létrehozott hosszú ideig eltarthat.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-158">If you have a large database, it could take a long time for hello initial backup toobe created over hello network.</span></span> <span data-ttu-id="e4e3c-159">tooavoid probléma hozhat létre offline biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-159">tooavoid this issue, you can create an offline backup.</span></span>  

    ![Online megőrzési szabály megadása](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. <span data-ttu-id="e4e3c-161">Erősítse meg hello beállításait, és kattintson a **csoport létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-161">Confirm hello settings, and then click **Create Group**.</span></span>
19. <span data-ttu-id="e4e3c-162">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-162">Click **Close**.</span></span>

## <a name="recover-hello-exchange-database"></a><span data-ttu-id="e4e3c-163">Hello Exchange-adatbázis helyreállítása</span><span class="sxs-lookup"><span data-stu-id="e4e3c-163">Recover hello Exchange database</span></span>
1. <span data-ttu-id="e4e3c-164">Kattintson az Exchange-adatbázis toorecover **helyreállítási** a DPM felügyeleti konzol hello.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-164">toorecover an Exchange database, click **Recovery** in hello DPM Administrator Console.</span></span>
2. <span data-ttu-id="e4e3c-165">Keresse meg, hogy szeretné-e toorecover hello Exchange-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-165">Locate hello Exchange database that you want toorecover.</span></span>
3. <span data-ttu-id="e4e3c-166">Az online helyreállítási pontot válasszon hello *helyreállításkor* legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-166">Select an online recovery point from hello *recovery time* drop-down list.</span></span>
4. <span data-ttu-id="e4e3c-167">Kattintson a **helyreállítása** toostart hello **helyreállítási varázsló**.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-167">Click **Recover** toostart hello **Recovery Wizard**.</span></span>

<span data-ttu-id="e4e3c-168">Az online helyreállítási pontok, öt helyreállítási típusa van:</span><span class="sxs-lookup"><span data-stu-id="e4e3c-168">For online recovery points, there are five recovery types:</span></span>

* <span data-ttu-id="e4e3c-169">**Helyreállítás toooriginal Exchange-kiszolgálón:** hello adatokat kell helyreállított toohello eredeti Exchange-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-169">**Recover toooriginal Exchange Server location:** hello data will be recovered toohello original Exchange server.</span></span>
* <span data-ttu-id="e4e3c-170">**Exchange Server tooanother adatbázis helyreállítása:** hello adatokat kell helyreállított tooanother egy másik Exchange server-adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-170">**Recover tooanother database on an Exchange Server:** hello data will be recovered tooanother database on another Exchange server.</span></span>
* <span data-ttu-id="e4e3c-171">**Helyreállítás helyreállítási adatbázisba tooa:** hello adatokat fogja a helyreállított tooan Exchange helyreállítási adatbázis (Rekordadatbázis).</span><span class="sxs-lookup"><span data-stu-id="e4e3c-171">**Recover tooa Recovery Database:** hello data will be recovered tooan Exchange Recovery Database (RDB).</span></span>
* <span data-ttu-id="e4e3c-172">**Tooa hálózati mappa másolása:** hello adatokat fogja a helyreállított tooa hálózati mappába.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-172">**Copy tooa network folder:** hello data will be recovered tooa network folder.</span></span>
* <span data-ttu-id="e4e3c-173">**Másolja a tootape:** Ha rendelkezik egy szalagtárat, vagy egy önálló szalagos meghajtót csatlakoztatott és a beállított hello DPM-kiszolgáló, hello helyreállítási pont lesz tooa szabad szalagra másolni.</span><span class="sxs-lookup"><span data-stu-id="e4e3c-173">**Copy tootape:** If you have a tape library or a stand-alone tape drive attached and configured on hello DPM server, hello recovery point will be copied tooa free tape.</span></span>

    ![Válassza ki az online replikációs](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a><span data-ttu-id="e4e3c-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4e3c-175">Next steps</span></span>
* [<span data-ttu-id="e4e3c-176">Az Azure biztonsági mentési – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="e4e3c-176">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
