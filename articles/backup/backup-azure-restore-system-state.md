---
title: "Az Azure Backup: Visszaállítási rendszerállapot tooa Windows Server |} Microsoft Docs"
description: "Lépés által lépés magyarázat a Windows Server rendszerállapot helyreállítása egy biztonsági másolatból, az Azure-ban."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/18/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: a45506507f53e2744350d3b6b2e52f1db415de4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-system-state-toowindows-server"></a><span data-ttu-id="a5df1-103">Rendszerállapot visszaállítása tooWindows kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="a5df1-103">Restore System State tooWindows Server</span></span>

<span data-ttu-id="a5df1-104">Ez a cikk azt ismerteti, hogyan toorestore Windows kiszolgáló rendszerállapotának készített biztonsági másolat az Azure Recovery Services tároló.</span><span class="sxs-lookup"><span data-stu-id="a5df1-104">This article explains how toorestore Windows Server System State backups from an Azure Recovery Services vault.</span></span> <span data-ttu-id="a5df1-105">Rendszerállapot toorestore, rendelkeznie kell a rendszerállapot (hello utasításait használatával létrehozott [rendszerállapot biztonsági mentése](backup-azure-system-state.md#back-up-windows-server-system-state-preview)), és győződjön meg arról, hogy telepítette a hello [legújabb verzióját a Microsoft Azure Recovery hello Szolgáltatások (MARS) ügynök](http://aka.ms/azurebackup_agent).</span><span class="sxs-lookup"><span data-stu-id="a5df1-105">toorestore System State, you must have a System State backup (created using hello instructions in [Back up System State](backup-azure-system-state.md#back-up-windows-server-system-state-preview)), and make sure you have installed hello [latest version of hello Microsoft Azure Recovery Services (MARS) agent](http://aka.ms/azurebackup_agent).</span></span> <span data-ttu-id="a5df1-106">Végezze el az Azure Recovery Services-tároló a Windows Server rendszerállapot-adatok két lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="a5df1-106">Recovering Windows Server System State data from an Azure Recovery Services vault is a two-step process:</span></span>

1. <span data-ttu-id="a5df1-107">Rendszerállapot visszaállítása az Azure Backup-fájlok formájában.</span><span class="sxs-lookup"><span data-stu-id="a5df1-107">Restore System State as files from Azure Backup.</span></span> <span data-ttu-id="a5df1-108">Rendszerállapot visszaállítása az Azure Backup-fájlok formájában, lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="a5df1-108">When restoring System State as files from Azure Backup, you can either:</span></span>
  * <span data-ttu-id="a5df1-109">Rendszerállapot visszaállítása toohello ahol hello biztonsági mentések vették, ugyanarra a kiszolgálóra vagy</span><span class="sxs-lookup"><span data-stu-id="a5df1-109">Restore System State toohello same server where hello backups were taken, or</span></span>
  * <span data-ttu-id="a5df1-110">Rendszerállapot visszaállítása fájl tooan másodlagos kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="a5df1-110">Restore System State file tooan alternate server.</span></span>

2. <span data-ttu-id="a5df1-111">Vissza hello rendszerállapot fájlok tooa Windows Server alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="a5df1-111">Apply hello restored System State files tooa Windows Server.</span></span>


## <a name="recover-system-state-files-toohello-same-server"></a><span data-ttu-id="a5df1-112">Rendszerállapot helyreállítása fájlok toohello ugyanarra a kiszolgálóra</span><span class="sxs-lookup"><span data-stu-id="a5df1-112">Recover System State files toohello same server</span></span>
<span data-ttu-id="a5df1-113">hello lépések azt ismertetik, hogyan tooroll biztonsági másolatot a Windows Server konfigurációs tooa korábbi állapotába.</span><span class="sxs-lookup"><span data-stu-id="a5df1-113">hello following steps explain how tooroll back your Windows Server configuration tooa previous state.</span></span> <span data-ttu-id="a5df1-114">Működés közbeni a kiszolgáló konfigurációs hátsó tooa ismert, stabil állapotot, rendkívül fontos lehet.</span><span class="sxs-lookup"><span data-stu-id="a5df1-114">Rolling your server configuration back tooa known, stable state, can be extremely valuable.</span></span> <span data-ttu-id="a5df1-115">hello követő lépéseket visszaállítási hello kiszolgáló rendszerállapotának a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="a5df1-115">hello following steps restore hello server's System State from a Recovery Services vault.</span></span> 

1. <span data-ttu-id="a5df1-116">Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="a5df1-116">Open hello **Microsoft Azure Backup** snap-in.</span></span> <span data-ttu-id="a5df1-117">Ha nem tudja, hol hello beépülő modul lett telepítve, a keresés hello számítógép vagy kiszolgáló **a Microsoft Azure Backup szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-117">If you don't know where hello snap-in was installed, search hello computer or server for **Microsoft Azure Backup**.</span></span>

    <span data-ttu-id="a5df1-118">egy asztali alkalmazás hello hello keresési eredmények jelenjenek meg.</span><span class="sxs-lookup"><span data-stu-id="a5df1-118">hello desktop app should appear in hello search results.</span></span>

2. <span data-ttu-id="a5df1-119">Kattintson a **adatok helyreállítása** toostart hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="a5df1-119">Click **Recover Data** toostart hello wizard.</span></span>

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server/recover.png)

3. <span data-ttu-id="a5df1-121">A hello **bevezetés** ablaktáblában toorestore hello adatok toohello ugyanazon a kiszolgálón vagy a számítógépen, válassza ki a **ehhez a kiszolgálóhoz (`<server name>`)** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-121">On hello **Getting Started** pane, toorestore hello data toohello same server or computer, select **This server (`<server name>`)** and click **Next**.</span></span>

    ![Válassza ki a kiszolgáló beállítás toorestore hello adatok toohello egyazon számítógépen](./media/backup-azure-restore-system-state/samemachine.png)

4. <span data-ttu-id="a5df1-123">A hello **válassza ki a helyreállítási mód** ablaktáblán válassza **rendszerállapot** , majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-123">On hello **Select Recovery Mode** pane, choose **System State** and then click **Next**.</span></span>

    ![Fájlok tallózása](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. <span data-ttu-id="a5df1-125">A hello naptárban **mennyiség kiválasztása és a dátum** ablaktáblán válasszon ki egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="a5df1-125">On hello calendar in **Select Volume and Date** pane, select a recovery point.</span></span> 

    <span data-ttu-id="a5df1-126">Bármelyik helyreállítási pontra állíthatja vissza időben.</span><span class="sxs-lookup"><span data-stu-id="a5df1-126">You can restore from any recovery point in time.</span></span> <span data-ttu-id="a5df1-127">A dátumok **félkövér** jelző hello legalább egy helyreállítási pont rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="a5df1-127">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="a5df1-128">Miután egy dátumot, ha több helyreállítási pont nem érhető el, hello adott helyreállítási pontot választhat hello **idő** legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="a5df1-128">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Kötet és a dátum](./media/backup-azure-restore-system-state/select-date.png)

6. <span data-ttu-id="a5df1-130">Miután kiválasztotta a hello helyreállítási pont toorestore, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-130">Once you have chosen hello recovery point toorestore, click **Next**.</span></span>

    <span data-ttu-id="a5df1-131">Azure biztonsági mentés hello helyi helyreállítási pontot csatlakoztatja, és egy helyreállítási kötet használja.</span><span class="sxs-lookup"><span data-stu-id="a5df1-131">Azure Backup mounts hello local recovery point, and uses it as a recovery volume.</span></span>

7. <span data-ttu-id="a5df1-132">A következő ablaktábla hello adja meg a hello hello céljának helyreállított fájlok rendszerállapot, majd kattintson **Tallózás** tooopen Windows Explorer és hello fájlok és mappák.</span><span class="sxs-lookup"><span data-stu-id="a5df1-132">On hello next pane, specify hello destination for hello recovered System State files and click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span> <span data-ttu-id="a5df1-133">lehetőségnél hello **példányt hoz létre, hogy mindkét változatához**, az egyes fájlok másolatokat készít a meglévő rendszerállapot fájl archiválhatja hello másolási hello teljes rendszerállapot-archívum létrehozása helyett.</span><span class="sxs-lookup"><span data-stu-id="a5df1-133">hello option, **Create copies so that you have both versions**, creates copies of individual files in an existing System State file archive instead of creating hello copy of hello entire System State archive.</span></span>

    ![Helyreállítási beállítások](./media/backup-azure-restore-system-state/recover-as-files.png)

8. <span data-ttu-id="a5df1-135">Ellenőrizze a hello helyreállítási hello adatait **megerősítő** kattintson **helyreállítása**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-135">Verify hello details of recovery on hello **Confirmation** pane and click **Recover**.</span></span>

   ![Kattintson a helyreállítás tooacknowledge hello helyreállítás művelet](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. <span data-ttu-id="a5df1-137">Másolás hello *WindowsImageBackup* könyvtárban lévő hello helyreállítási cél tooa nem kritikus kötet hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="a5df1-137">Copy hello *WindowsImageBackup* directory in hello Recovery destination tooa non-critical volume of hello server.</span></span> <span data-ttu-id="a5df1-138">Általában a Windows operációs rendszer mennyiségi hello hello kritikus kötet jelenti.</span><span class="sxs-lookup"><span data-stu-id="a5df1-138">Usually, hello Windows OS volume is hello critical volume.</span></span>

10. <span data-ttu-id="a5df1-139">Ha sikeres hello helyreállítási, lépésekkel hello hello területen [alkalmaz vissza rendszerállapot fájlok toohello Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), toocomplete hello rendszerállapot-helyreállítási folyamatot.</span><span class="sxs-lookup"><span data-stu-id="a5df1-139">Once hello recovery is successful, follow hello steps in hello section, [Apply restored System State files toohello Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), toocomplete hello System State recovery process.</span></span>

## <a name="recover-system-state-files-tooan-alternate-server"></a><span data-ttu-id="a5df1-140">Rendszerállapot helyreállítása fájlok tooan másodlagos kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="a5df1-140">Recover System State files tooan alternate server</span></span>

<span data-ttu-id="a5df1-141">Ha a Windows Server sérült vagy nem érhető el, és azt szeretné, hogy toorestore azt tooa stabil állapotot által helyreállítása hello Windows kiszolgáló rendszerállapotának, hello sérült kiszolgáló rendszerállapotának állíthatja vissza egy másik kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="a5df1-141">If your Windows Server is corrupted or inaccessible, and you want toorestore it tooa stable state by recovering hello Windows Server System State, you can restore hello corrupted server's System State from another server.</span></span> <span data-ttu-id="a5df1-142">Következő lépések toohello visszaállítási rendszerállapot külön kiszolgálón hello használja.</span><span class="sxs-lookup"><span data-stu-id="a5df1-142">Use hello following steps toohello restore System State on a separate server.</span></span>  

<span data-ttu-id="a5df1-143">hello terminológia a következő lépéseket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="a5df1-143">hello terminology used in these steps includes:</span></span>

- <span data-ttu-id="a5df1-144">*Forrásgép* – hello eredeti számítógép melyik hello biztonsági mentésből készült, és amelyen jelenleg nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="a5df1-144">*Source machine* – hello original machine from which hello backup was taken and which is currently unavailable.</span></span>
- <span data-ttu-id="a5df1-145">*Célszámítógép* – hello gép toowhich hello adatok helyreállítása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="a5df1-145">*Target machine* – hello machine toowhich hello data is being recovered.</span></span>
- <span data-ttu-id="a5df1-146">*Minta tároló* – hello Recovery Services-tároló toowhich hello *forrásgép* és *célgépen* van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="a5df1-146">*Sample vault* – hello Recovery Services vault toowhich hello *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="a5df1-147">Egyik gépről készített biztonsági másolatok hello operációs rendszer korábbi verzióját futtató visszaállított tooa gépet nem lehet.</span><span class="sxs-lookup"><span data-stu-id="a5df1-147">Backups taken from one machine cannot be restored tooa machine running an earlier version of hello operating system.</span></span> <span data-ttu-id="a5df1-148">Például egy Windows Server 2016 gép nem lehet biztonsági másolatokat vissza tooWindows Server 2012 R2-ben.</span><span class="sxs-lookup"><span data-stu-id="a5df1-148">For example, backups taken from a Windows Server 2016 machine can't be restored tooWindows Server 2012 R2.</span></span> <span data-ttu-id="a5df1-149">Hello inverz azonban lehetőség.</span><span class="sxs-lookup"><span data-stu-id="a5df1-149">However, hello inverse is possible.</span></span> <span data-ttu-id="a5df1-150">Használhatja a Windows Server 2012 R2 toorestore Windows Server 2016 készített biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="a5df1-150">You can use backups from Windows Server 2012 R2 toorestore Windows Server 2016.</span></span>
>

1. <span data-ttu-id="a5df1-151">Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** beépülő hello *célgépen*.</span><span class="sxs-lookup"><span data-stu-id="a5df1-151">Open hello **Microsoft Azure Backup** snap-in on hello *Target machine*.</span></span>
2. <span data-ttu-id="a5df1-152">Győződjön meg arról, hogy hello *célgépen* és hello *forrásgép* azonos Recovery Services-tároló regisztrált toohello vannak.</span><span class="sxs-lookup"><span data-stu-id="a5df1-152">Ensure that hello *Target machine* and hello *Source machine* are registered toohello same Recovery Services vault.</span></span>
3. <span data-ttu-id="a5df1-153">Kattintson a **adatok helyreállítása** tooinitiate hello munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="a5df1-153">Click **Recover Data** tooinitiate hello workflow.</span></span>

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server-classic/recover.png)

4. <span data-ttu-id="a5df1-155">Válassza ki **egy másik kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="a5df1-155">Select **Another server**</span></span>

    ![Egy másik kiszolgáló](./media/backup-azure-restore-system-state/anotherserver.png)

5. <span data-ttu-id="a5df1-157">Adja meg a hello tárolói hitelesítő adatok fájlját, amely megfelel a toohello *minta tároló*.</span><span class="sxs-lookup"><span data-stu-id="a5df1-157">Provide hello vault credential file that corresponds toohello *Sample vault*.</span></span> <span data-ttu-id="a5df1-158">Ha hello tárolói hitelesítő adatok fájlját érvénytelen (vagy lejárt), egy új tárolói hitelesítő adatok fájlját letöltését hello *minta tároló* a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="a5df1-158">If hello vault credential file is invalid (or expired), download a new vault credential file from hello *Sample vault* in hello Azure portal.</span></span> <span data-ttu-id="a5df1-159">Hello tárolói hitelesítő adatok fájlját valósul meg, miután hello Recovery Services-tároló hello tárolói hitelesítő adatok fájlját társított jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a5df1-159">Once hello vault credential file is provided, hello Recovery Services vault associated with hello vault credential file appears.</span></span>

6. <span data-ttu-id="a5df1-160">Hello biztonsági mentés kiszolgáló kiválasztása panelén válassza ki a hello *forrásgép* megjelenített gépek hello listája.</span><span class="sxs-lookup"><span data-stu-id="a5df1-160">On hello Select Backup Server pane, select hello *Source machine* from hello list of displayed machines.</span></span>

    ![Számítógépek listája](./media/backup-azure-restore-windows-server-classic/machinelist.png)

7. <span data-ttu-id="a5df1-162">Hello válassza ki a helyreállítási mód panelén válassza **rendszerállapot** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-162">On hello Select Recovery Mode pane, choose **System State** and click **Next**.</span></span> 

    ![Keresés](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. <span data-ttu-id="a5df1-164">A naptár hello a hello **mennyiség kiválasztása és a dátum** ablaktáblán válasszon ki egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="a5df1-164">On hello Calendar in hello **Select Volume and Date** pane, select a recovery point.</span></span> <span data-ttu-id="a5df1-165">Bármelyik helyreállítási pontra állíthatja vissza időben.</span><span class="sxs-lookup"><span data-stu-id="a5df1-165">You can restore from any recovery point in time.</span></span> <span data-ttu-id="a5df1-166">A dátumok **félkövér** jelző hello legalább egy helyreállítási pont rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="a5df1-166">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="a5df1-167">Miután egy dátumot, ha több helyreállítási pont nem érhető el, hello adott helyreállítási pontot választhat hello **idő** legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="a5df1-167">Once  you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span> 

    ![Keresési elemek](./media/backup-azure-restore-system-state/select-date.png)

9. <span data-ttu-id="a5df1-169">Miután kiválasztotta a hello helyreállítási pont toorestore, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-169">Once you have chosen hello recovery point toorestore, click **Next**.</span></span>

10. <span data-ttu-id="a5df1-170">A hello **válasszon rendszer állapota helyreállítási módban** ablaktáblán hello helyet, ahol rendszerállapot helyreállítása toobe fájlok adja meg, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-170">On hello **Select System State Recovery Mode** pane, specify hello destination where you want System State files toobe recovered, then click **Next**.</span></span>

    ![Titkosítás](./media/backup-azure-restore-system-state/recover-as-files.png)

    <span data-ttu-id="a5df1-172">lehetőségnél hello **példányt hoz létre, hogy mindkét változatához**, az egyes fájlok másolatokat készít a meglévő rendszerállapot fájl archiválhatja hello másolási hello teljes rendszerállapot-archívum létrehozása helyett.</span><span class="sxs-lookup"><span data-stu-id="a5df1-172">hello option, **Create copies so that you have both versions**, creates copies of individual files in an existing System State file archive instead of creating hello copy of hello entire System State archive.</span></span>

11. <span data-ttu-id="a5df1-173">Ellenőrizze a hello megerősítő panelen helyreállítási hello részleteit, majd kattintson **helyreállítása**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-173">Verify hello details of recovery on hello Confirmation pane, and click **Recover**.</span></span> 

    ![Kattintson a hello helyreállítás gomb tooconfirm hello helyreállítási folyamat](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. <span data-ttu-id="a5df1-175">Másolás hello *WindowsImageBackup* directory tooa nem kritikus kötet hello kiszolgáló (például D:\).</span><span class="sxs-lookup"><span data-stu-id="a5df1-175">Copy hello *WindowsImageBackup* directory tooa non-critical volume of hello server (for example D:\).</span></span> <span data-ttu-id="a5df1-176">Általában a Windows operációs rendszer mennyiségi hello a hello kritikus kötet.</span><span class="sxs-lookup"><span data-stu-id="a5df1-176">Usually hello Windows OS volume is hello critical volume.</span></span>

13. <span data-ttu-id="a5df1-177">toocomplete hello helyreállítási folyamat használata hello következő szakasz túl[vissza hello rendszerállapot fájlok a Windows Server alkalmazása](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).</span><span class="sxs-lookup"><span data-stu-id="a5df1-177">toocomplete hello recovery process, use hello following section too[apply hello restored System State files on a Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).</span></span>




## <a name="apply-restored-system-state-on-a-windows-server"></a><span data-ttu-id="a5df1-178">A visszaállított rendszerállapot alkalmazni a Windows Server</span><span class="sxs-lookup"><span data-stu-id="a5df1-178">Apply restored System State on a Windows Server</span></span>

<span data-ttu-id="a5df1-179">Miután helyreállított rendszerállapot-fájlként Azure Recovery Services Agent, használjon hello Windows Server biztonsági másolat segédprogram tooapply hello helyre rendszerállapot tooWindows kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="a5df1-179">Once you have recovered System State as files using Azure Recovery Services Agent, use hello Windows Server Backup utility tooapply hello recovered System State tooWindows Server.</span></span> <span data-ttu-id="a5df1-180">Windows Server biztonsági másolat segédprogram hello már hello kiszolgálón érhetők el.</span><span class="sxs-lookup"><span data-stu-id="a5df1-180">hello Windows Server Backup utility is already available on hello server.</span></span> <span data-ttu-id="a5df1-181">hello lépések azt ismertetik, hogyan tooapply hello helyre a rendszer állapotát.</span><span class="sxs-lookup"><span data-stu-id="a5df1-181">hello following steps explain how tooapply hello recovered System State.</span></span>

1. <span data-ttu-id="a5df1-182">Használjon hello következő parancsok tooreboot a kiszolgáló *a címtárszolgáltatás-javító módbeli*.</span><span class="sxs-lookup"><span data-stu-id="a5df1-182">Use hello following commands tooreboot your server in *Directory Services Repair Mode*.</span></span> <span data-ttu-id="a5df1-183">A rendszergazda jogú parancssorból:</span><span class="sxs-lookup"><span data-stu-id="a5df1-183">In an elevated command prompt:</span></span>

    ```
    PS C:\> Bcdedit /set safeboot dsrepair
    PS C:\> Shutdown /r /t 0
    ```

2. <span data-ttu-id="a5df1-184">Hello az újraindítást követően hello Windows Server biztonsági másolat beépülő modul megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="a5df1-184">After hello reboot, open hello Windows Server Backup snap-in.</span></span> <span data-ttu-id="a5df1-185">Ha nem tudja, hol hello beépülő modul lett telepítve, a keresés hello számítógép vagy kiszolgáló **Windows Server biztonsági másolat**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-185">If you don't know where hello snap-in was installed, search hello computer or server for **Windows Server Backup**.</span></span>

    <span data-ttu-id="a5df1-186">hello egy asztali alkalmazás hello keresési eredmények között megjelenik.</span><span class="sxs-lookup"><span data-stu-id="a5df1-186">hello desktop app appears in hello search results.</span></span>

3. <span data-ttu-id="a5df1-187">Válassza ki a beépülő modul hello **helyi biztonsági másolat**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-187">In hello snap-in, select **Local Backup**.</span></span>

    ![Válassza ki a helyi biztonsági másolat toorestore onnan](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

4. <span data-ttu-id="a5df1-189">A hello helyi biztonsági másolat konzoljának hello **műveletek panel**, kattintson a **helyreállítása** tooopen hello helyreállítási varázsló.</span><span class="sxs-lookup"><span data-stu-id="a5df1-189">On hello Local Backup console, in hello **Actions Pane**, click **Recover** tooopen hello Recovery Wizard.</span></span>

5. <span data-ttu-id="a5df1-190">Hello a beállítást választja, **más helyen tárolt biztonsági másolat**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-190">Select hello option, **A backup stored in another location**, and click **Next**.</span></span>

   ![Válassza ki a toorecover tooa másik kiszolgálóra](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

6. <span data-ttu-id="a5df1-192">Hello tárhely típusának megadása esetén válassza ki a **távoli megosztott mappa** Ha a rendszervédelmi biztonsági másolatot a helyreállított tooanother kiszolgáló volt.</span><span class="sxs-lookup"><span data-stu-id="a5df1-192">When specifying hello location type, select **Remote shared folder** if your System State backup was recovered tooanother server.</span></span> <span data-ttu-id="a5df1-193">Ha a rendszerállapot helyileg lett helyreállítva, akkor válasszon **helyi meghajtók**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-193">If your System State was recovered locally, then select **Local drives**.</span></span> 

    ![Válassza ki e a helyi kiszolgáló vagy egy másik toorecovery](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

7. <span data-ttu-id="a5df1-195">Adja meg a hello elérési toohello *WindowsImageBackup* könyvtár, vagy válassza ki a könyvtárat (például D:\WindowsImageBackup), Azure helyreállítással hello rendszerállapot fájlok helyreállítási helyre hello helyi meghajtót Ügynök, és kattintson a szolgáltatások **következő**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-195">Enter hello path toohello *WindowsImageBackup* directory, or choose hello local drive containing this directory (for example, D:\WindowsImageBackup), recovered as part of hello System State files recovery using Azure Recovery Services Agent and click **Next**.</span></span>

    ![toohello megosztott fájl elérési útja](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

8. <span data-ttu-id="a5df1-197">Jelölje be hello rendszerállapot verzió toorestore szeretné, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-197">Select hello System State version that you want toorestore, and click **Next**.</span></span>

9. <span data-ttu-id="a5df1-198">Hello helyreállítási típus kiválasztása ablaktáblában jelölje ki **rendszerállapot** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-198">In hello Select Recovery Type pane, select **System State** and click **Next**.</span></span>

10. <span data-ttu-id="a5df1-199">Rendszerállapot-helyreállítás hello hello helyét, válassza a **eredeti helyére**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="a5df1-199">For hello location of hello System State Recovery, select **Original Location**, and click **Next**.</span></span>

11. <span data-ttu-id="a5df1-200">Hello megerősítő részletes leírását, ellenőrizze a hello újraindítás beállításait, és kattintson **helyreállítása** tooapplly hello rendszerállapot fájlok visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="a5df1-200">Review hello confirmation details, verify hello reboot settings, and click **Recover** tooapplly hello restored System State files.</span></span>

    ![indítási hello rendszerállapot fájlok helyreállítása](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

## <a name="special-considerations-for-system-state-recovery-on-active-directory-server"></a><span data-ttu-id="a5df1-202">Különleges szempontok a rendszerállapot-helyreállítást az Active Directory-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="a5df1-202">Special considerations for System State recovery on Active Directory server</span></span>

<span data-ttu-id="a5df1-203">Rendszerállapot biztonsági mentésének Active Directory-adatokat.</span><span class="sxs-lookup"><span data-stu-id="a5df1-203">System State backup includes Active Directory data.</span></span> <span data-ttu-id="a5df1-204">Használja a hello követő lépéseket toorestore Active Directory tartományi szolgáltatások (AD DS) az aktuális állapot tooa korábbi állapotába.</span><span class="sxs-lookup"><span data-stu-id="a5df1-204">Use hello following steps toorestore Active Directory Domain Service (AD DS) from its current state tooa previous state.</span></span>

1. <span data-ttu-id="a5df1-205">Indítsa újra a hello tartományvezérlő a Címtárszolgáltatások helyreállító módban (DSRM).</span><span class="sxs-lookup"><span data-stu-id="a5df1-205">Restart hello domain controller in Directory Services Restore Mode (DSRM).</span></span>
2. <span data-ttu-id="a5df1-206">Hello lépésekkel [Itt](https://technet.microsoft.com/en-us/library/cc794755(v=ws.10).aspx) toouse Windows Server biztonsági másolat parancsmagjai toorecover Active Directory tartományi Szolgáltatásokban.</span><span class="sxs-lookup"><span data-stu-id="a5df1-206">Follow hello steps [here](https://technet.microsoft.com/en-us/library/cc794755(v=ws.10).aspx) toouse Windows Server Backup cmdlets toorecover AD DS.</span></span>


## <a name="troubleshoot-failed-system-state-restore"></a><span data-ttu-id="a5df1-207">Sikertelen rendszerállapot-visszaállítást hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="a5df1-207">Troubleshoot failed System State restore</span></span>

<span data-ttu-id="a5df1-208">Ha hello előző folyamat, amely során a rendszerállapot nem fejeződik, használja a Windows Server hello Windows helyreállítási környezet (Win RE) toorecover.</span><span class="sxs-lookup"><span data-stu-id="a5df1-208">If hello previous process of applying System State does not complete successfully, use hello Windows Recovery Environment (Win RE) toorecover your Windows Server.</span></span> <span data-ttu-id="a5df1-209">hello következő részben megtudhatja, hogyan toorecover Win újbóli használata.</span><span class="sxs-lookup"><span data-stu-id="a5df1-209">hello following steps explain how toorecover using Win RE.</span></span> <span data-ttu-id="a5df1-210">Használja ezt a beállítást csak akkor, ha a Windows Server normál esetben ne indítsa a rendszerállapot-visszaállítást követően.</span><span class="sxs-lookup"><span data-stu-id="a5df1-210">Use This option only if Windows Server does not boot normally after a System State restore.</span></span> <span data-ttu-id="a5df1-211">a folyamatot követve hello nem rendszerszintű adatot, legyen körültekintő töröl.</span><span class="sxs-lookup"><span data-stu-id="a5df1-211">hello following process erases non-system data, use caution.</span></span> 

1. <span data-ttu-id="a5df1-212">A Windows Server indul hello Windows helyreállítási környezet (Win RE).</span><span class="sxs-lookup"><span data-stu-id="a5df1-212">Boot your Windows Server into hello Windows Recovery Environment (Win RE).</span></span>

2. <span data-ttu-id="a5df1-213">Válassza ki a hibaelhárítás hello három rendelkezésre álló lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="a5df1-213">Select Troubleshoot from hello three available options.</span></span>

    ![a menü megnyitása](./media/backup-azure-restore-system-state/winre-1.png)

3. <span data-ttu-id="a5df1-215">A hello **speciális beállítások** képernyőn válassza ki **parancssor** és hello kiszolgálón rendszergazdai jogosultságú felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="a5df1-215">From hello **Advanced Options** screen, select **Command Prompt** and provide hello server administrator username and password.</span></span>

   ![a menü megnyitása](./media/backup-azure-restore-system-state/winre-2.png)

4. <span data-ttu-id="a5df1-217">Hello kiszolgálón rendszergazdai jogosultságú felhasználónevet és jelszót adjon meg.</span><span class="sxs-lookup"><span data-stu-id="a5df1-217">Provide hello server administrator username and password.</span></span>

    ![a menü megnyitása](./media/backup-azure-restore-system-state/winre-3.png)

5. <span data-ttu-id="a5df1-219">Amikor megnyitja a hello parancssort rendszergazdai módban, futtassa a következő parancs tooget hello rendszerállapot biztonságimásolat-verziók.</span><span class="sxs-lookup"><span data-stu-id="a5df1-219">When you open hello command prompt in administrator mode, run following command tooget hello System State backup versions.</span></span>

    ```
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```
    ![Rendszerállapot biztonságimásolat-verziók beolvasása](./media/backup-azure-restore-system-state/winre-4.png)

6. <span data-ttu-id="a5df1-221">Futtassa a következő parancs tooget hello összes kötet elérhető hello biztonsági mentése.</span><span class="sxs-lookup"><span data-stu-id="a5df1-221">Run hello following command tooget all volumes available in hello backup.</span></span>

    ```
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![Rendszerállapot biztonságimásolat-verziók beolvasása](./media/backup-azure-restore-system-state/winre-5.png)

7. <span data-ttu-id="a5df1-223">hello következő parancs helyreállítja hello rendszerállapot biztonsági mentését részét képező összes kötetet.</span><span class="sxs-lookup"><span data-stu-id="a5df1-223">hello following command recovers all volumes that are part of hello System State Backup.</span></span> <span data-ttu-id="a5df1-224">Vegye figyelembe, hogy a ezt a lépést csak hello kritikus kötetek hello rendszerállapot részét képező állítja helyre.</span><span class="sxs-lookup"><span data-stu-id="a5df1-224">Note that this step recovers only hello critical volumes that are part of hello System State.</span></span> <span data-ttu-id="a5df1-225">Nem rendszermeghajtó minden adat törlődik.</span><span class="sxs-lookup"><span data-stu-id="a5df1-225">All non-System data is erased.</span></span>

    ```
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```
     ![Rendszerállapot biztonságimásolat-verziók beolvasása](./media/backup-azure-restore-system-state/winre-6.png)



## <a name="next-steps"></a><span data-ttu-id="a5df1-227">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a5df1-227">Next steps</span></span>
* <span data-ttu-id="a5df1-228">Most, hogy a fájlok és mappák már helyreállítva, akkor [kezelheti a biztonsági másolatok](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="a5df1-228">Now that you've recovered your files and folders, you can [manage your backups](backup-azure-manage-windows-server.md).</span></span>
