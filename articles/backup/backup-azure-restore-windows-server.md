---
title: "aaaRestore adatok Azure tooa Windows Server vagy Windows-számítógép |} Microsoft Docs"
description: "Ismerje meg, hogyan toorestore az tárolt Azure tooa Windows Server vagy a Windows rendszerű számítógépen."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 742f4b9e-c0ab-4eeb-8e22-ee29b83c22c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/16/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 11335495e448986a74f1733406f87e04331641d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-files-tooa-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a><span data-ttu-id="0f5e6-103">Fájlok tooa Windows server vagy a Windows ügyfél gép Resource Manager üzembe helyezési modellben visszaállítása</span><span class="sxs-lookup"><span data-stu-id="0f5e6-103">Restore files tooa Windows server or Windows client machine using Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0f5e6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0f5e6-104">Azure portal</span></span>](backup-azure-restore-windows-server.md)
> * [<span data-ttu-id="0f5e6-105">Klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="0f5e6-105">Classic portal</span></span>](backup-azure-restore-windows-server-classic.md)
>
>

<span data-ttu-id="0f5e6-106">Ez a cikk azt ismerteti, hogyan toorestore adatokat a biztonsági mentési tárolóból.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-106">This article explains how toorestore data from a backup vault.</span></span> <span data-ttu-id="0f5e6-107">toorestore adatok hello adatok helyreállítása varázsló használható a Microsoft Azure Recovery Services (MARS) ügynök hello.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-107">toorestore data, you use hello Recover Data wizard in hello Microsoft Azure Recovery Services (MARS) agent.</span></span> <span data-ttu-id="0f5e6-108">Adatok helyreállításakor lehetősége:</span><span class="sxs-lookup"><span data-stu-id="0f5e6-108">When you restore data, it is possible to:</span></span>

* <span data-ttu-id="0f5e6-109">Ugyanaz a számítógép melyik hello biztonsági másolatból visszaállítási adatok toohello vették.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-109">Restore data toohello same machine from which hello backups were taken.</span></span>
* <span data-ttu-id="0f5e6-110">Állítsa vissza az adatok tooan másik gépen.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-110">Restore data tooan alternate machine.</span></span>

<span data-ttu-id="0f5e6-111">2017. január, a Microsoft, amely egy előzetes frissítés toohello MARS agent.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-111">In January 2017, Microsoft released a Preview update toohello MARS agent.</span></span> <span data-ttu-id="0f5e6-112">Hibajavításokat tartalmaz, valamint a frissítés lehetővé teszi, hogy azonnali visszaállítása, amely lehetővé teszi toomount írható helyreállítási pont pillanatkép helyreállítási kötetként.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-112">Along with bug fixes, this update enables Instant Restore, which allows you toomount a writeable recovery point snapshot as a recovery volume.</span></span> <span data-ttu-id="0f5e6-113">Megismerheti a hello helyreállítási kötet, és másolja fájlok tooa helyi számítógép ezáltal visszaállítása fájlok majd.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-113">You can then explore hello recovery volume and copy files tooa local computer thereby selectively restoring files.</span></span>

> [!NOTE]
> <span data-ttu-id="0f5e6-114">Hello [2017. január Azure Backup frissítését](https://support.microsoft.com/en-us/help/3216528?preview) szükség, ha azt szeretné, hogy toouse azonnali visszaállítása toorestore adatokat.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-114">hello [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) is required if you want toouse Instant Restore toorestore data.</span></span> <span data-ttu-id="0f5e6-115">Hello biztonsági mentési adatok is a tárolók az hello támogatási cikkben szereplő területi beállításokhoz kell védeni.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-115">Also hello backup data must be protected in vaults in locales listed in hello support article.</span></span> <span data-ttu-id="0f5e6-116">Tekintse át a hello [2017. január Azure Backup frissítését](https://support.microsoft.com/en-us/help/3216528?preview) hello területi beállításokat, amelyek támogatják a azonnali visszaállítása a legfrissebb listáját.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-116">Consult hello [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) for hello latest list of locales that support Instant Restore.</span></span> <span data-ttu-id="0f5e6-117">Azonnali visszaállítás **nem** elérhetők az összes területi beállításokat.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-117">Instant Restore is **not** currently available in all locales.</span></span>
>

<span data-ttu-id="0f5e6-118">Azonnali visszaállítási érhető el a Recovery Services-tárolók az Azure-portálon hello használata és a biztonsági mentési tárolók hello a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-118">Instant Restore is available for use in Recovery Services vaults in hello Azure portal and Backup vaults in hello classic portal.</span></span> <span data-ttu-id="0f5e6-119">Ha azt szeretné, hogy azonnali visszaállítása toouse, hello MARS frissítés letöltése, és kövesse a hello eljárások, amelyek említik azonnali visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-119">If you want toouse Instant Restore, download hello MARS update, and follow hello procedures that mention Instant Restore.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-toorecover-data-toohello-same-machine"></a><span data-ttu-id="0f5e6-120">Azonnali visszaállítása toorecover adatok toohello használja egyazon számítógépen</span><span class="sxs-lookup"><span data-stu-id="0f5e6-120">Use Instant Restore toorecover data toohello same machine</span></span>

<span data-ttu-id="0f5e6-121">Ha véletlenül törli a fájlt, és szeretné toorestore az egyazon számítógépen (mely hello a biztonsági mentés használatban van), a következő hello lépések toohello segít hello adatok helyreállítását.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-121">If you accidentally deleted a file and wish toorestore it toohello same machine (from which hello backup is taken), hello following steps will help you recover hello data.</span></span>

1. <span data-ttu-id="0f5e6-122">Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** az illesztési.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-122">Open hello **Microsoft Azure Backup** snap in.</span></span> <span data-ttu-id="0f5e6-123">Ha nem tudja, ahová a hello beépülő modul telepítve van, a keresés hello számítógép vagy kiszolgáló **a Microsoft Azure Backup szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-123">If you don't know where hello snap in was installed, search hello computer or server for **Microsoft Azure Backup**.</span></span>

    <span data-ttu-id="0f5e6-124">egy asztali alkalmazás hello hello keresési eredmények jelenjenek meg.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-124">hello desktop app should appear in hello search results.</span></span>

2. <span data-ttu-id="0f5e6-125">Kattintson a **adatok helyreállítása** toostart hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-125">Click **Recover Data** toostart hello wizard.</span></span>

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server/recover.png)

3. <span data-ttu-id="0f5e6-127">A hello **bevezetés** ablaktáblában toorestore hello adatok toohello ugyanazon a kiszolgálón vagy a számítógépen, válassza ki a **ehhez a kiszolgálóhoz (`<server name>`)** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-127">On hello **Getting Started** pane, toorestore hello data toohello same server or computer, select **This server (`<server name>`)** and click **Next**.</span></span>

    ![Válassza ki a kiszolgáló beállítás toorestore hello adatok toohello egyazon számítógépen](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. <span data-ttu-id="0f5e6-129">A hello **válassza ki a helyreállítási mód** ablaktáblán válassza **egyes fájlok és mappák** , majd **tovább**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-129">On hello **Select Recovery Mode** pane, choose **Individual files and folders** and then click **Next**.</span></span>

    ![Fájlok tallózása](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. <span data-ttu-id="0f5e6-131">A hello **mennyiség kiválasztása és a dátum** ablaktáblában hello fájlok és/vagy mappákról, amelyeket toorestore tartalmazó select hello kötetet.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-131">On hello **Select Volume and Date** pane, select hello volume that contains hello files and/or folders you want toorestore.</span></span>

    <span data-ttu-id="0f5e6-132">Hello naptárban válasszon ki egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-132">On hello calendar, select a recovery point.</span></span> <span data-ttu-id="0f5e6-133">Bármelyik helyreállítási pontra állíthatja vissza időben.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-133">You can restore from any recovery point in time.</span></span> <span data-ttu-id="0f5e6-134">A dátumok **félkövér** jelző hello legalább egy helyreállítási pont rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-134">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="0f5e6-135">Miután egy dátumot, ha több helyreállítási pont nem érhető el, hello adott helyreállítási pontot választhat hello **idő** legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-135">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Kötet és a dátum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. <span data-ttu-id="0f5e6-137">Miután kiválasztotta a hello helyreállítási pont toorestore, kattintson a **csatlakoztatási**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-137">Once you have chosen hello recovery point toorestore, click **Mount**.</span></span>

    <span data-ttu-id="0f5e6-138">Azure biztonsági mentés hello helyi helyreállítási pontot csatlakoztatja, és egy helyreállítási kötet használja.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-138">Azure Backup mounts hello local recovery point, and uses it as a recovery volume.</span></span>

7. <span data-ttu-id="0f5e6-139">A hello **tallózással keresse meg és a fájlok helyreállítása** ablaktáblában kattintson **Tallózás** tooopen Windows Explorer és hello fájlok és mappák.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-139">On hello **Browse and Recover Files** pane, click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span>

    ![Helyreállítási beállítások](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. <span data-ttu-id="0f5e6-141">A Windows Intézőben, hello fájlok másolása és/vagy mappák toorestore szeretné, és illessze be a tooany hely helyi toohello kiszolgálón vagy számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-141">In Windows Explorer, copy hello files and/or folders you want toorestore and paste them tooany location local toohello server or computer.</span></span> <span data-ttu-id="0f5e6-142">Nyissa meg vagy adatfolyam-fájlok hello közvetlenül hello helyreállítási kötet, és ellenőrizze a megfelelő verzió helyreállítás hello.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-142">You can open or stream hello files directly from hello recovery volume and verify hello correct versions are recovered.</span></span>

    ![Másolja és illessze be a fájlok és mappák csatlakoztatott kötet toolocal helyről](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. <span data-ttu-id="0f5e6-144">Ha elkészült a visszaállítási hello fájlok és/vagy mappák hello **tallózással keresse meg és a helyreállítási fájlokat** ablaktáblán kattintson a **leválasztási**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-144">When you are finished restoring hello files and/or folders, on hello **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="0f5e6-145">Kattintson a **Igen** tooconfirm, amelyet az toounmount hello kötet.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-145">Then click **Yes** tooconfirm that you want toounmount hello volume.</span></span>

    ![Válassza le a hello kötet, és erősítse meg](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="0f5e6-147">Ha leválasztási gombra nem kattint, hello helyreállítási kötet marad csatlakoztatott hat órán át hello óta, amikor csatlakoztatva lett.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-147">If you do not click Unmount, hello Recovery Volume will remain mounted for 6 hours from hello time when it was mounted.</span></span> <span data-ttu-id="0f5e6-148">Azonban hello csatlakoztatási ideje kiterjesztett legfeljebb 24 óra egy folyamatban lévő fájlmásolás esetén.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-148">However, hello mount time is extended upto a maximum of 24 hours in case of an ongoing file-copy.</span></span> <span data-ttu-id="0f5e6-149">Nincs biztonsági mentési műveletek fog futni, amíg hello kötet csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-149">No backup operations will run while hello volume is mounted.</span></span> <span data-ttu-id="0f5e6-150">Minden ütemezett biztonsági mentési művelet toorun hello időszakban, amikor hello kötet csatlakoztatva van, miután hello helyreállítási kötet nem fog futni.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-150">Any backup operation scheduled toorun during hello time when hello volume is mounted, will run after hello recovery volume is unmounted.</span></span>
    >


## <a name="use-instant-restore-toorestore-data-tooan-alternate-machine"></a><span data-ttu-id="0f5e6-151">Azonnali visszaállítása toorestore adatok tooan másik számítógépen</span><span class="sxs-lookup"><span data-stu-id="0f5e6-151">Use Instant Restore toorestore data tooan alternate machine</span></span>
<span data-ttu-id="0f5e6-152">A teljes kiszolgáló nem vesztek el, ha továbbra is helyreállítható adatok Azure biztonsági mentés tooa másik gépre.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-152">If your entire server is lost, you can still recover data from Azure Backup tooa different machine.</span></span> <span data-ttu-id="0f5e6-153">a lépéseket követve hello hello munkafolyamat mutatják be.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-153">hello following steps illustrate hello workflow.</span></span>


<span data-ttu-id="0f5e6-154">hello terminológia a következő lépéseket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="0f5e6-154">hello terminology used in these steps includes:</span></span>

* <span data-ttu-id="0f5e6-155">*Forrásgép* – hello eredeti számítógép melyik hello biztonsági mentésből készült, és amelyen jelenleg nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-155">*Source machine* – hello original machine from which hello backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="0f5e6-156">*Célszámítógép* – hello gép toowhich hello adatok helyreállítása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-156">*Target machine* – hello machine toowhich hello data is being recovered.</span></span>
* <span data-ttu-id="0f5e6-157">*Minta tároló* – hello Recovery Services-tároló toowhich hello *forrásgép* és *célgépen* van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-157">*Sample vault* – hello Recovery Services vault toowhich hello *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="0f5e6-158">Biztonsági mentések nem lehet visszaállított tooa hello operációs rendszer korábbi verzióját futtató célszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-158">Backups can't be restored tooa target machine running an earlier version of hello operating system.</span></span> <span data-ttu-id="0f5e6-159">Például egy készült biztonsági másolatok a Windows 7 számítógép lehet vissza a Windows 8-as vagy újabb verzióját, számítógép.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-159">For example, a backup taken from a Windows 7 computer can be restored on a Windows 8, or later, computer.</span></span> <span data-ttu-id="0f5e6-160">Egy készült biztonsági másolatok a Windows 8 számítógépről nem lehet visszaállított tooa Windows 7 számítógép.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-160">A backup taken from a Windows 8 computer cannot be restored tooa Windows 7 computer.</span></span>
>
>

1. <span data-ttu-id="0f5e6-161">Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** az illesztési hello *célgépen*.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-161">Open hello **Microsoft Azure Backup** snap in on hello *Target machine*.</span></span>

2. <span data-ttu-id="0f5e6-162">Győződjön meg arról hello *célgépen* és hello *forrásgép* azonos Recovery Services-tároló regisztrált toohello vannak.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-162">Ensure hello *Target machine* and hello *Source machine* are registered toohello same Recovery Services vault.</span></span>

3. <span data-ttu-id="0f5e6-163">Kattintson a **adatok helyreállítása** tooopen hello **adatok helyreállítása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-163">Click **Recover Data** tooopen hello **Recover Data wizard**.</span></span>

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server/recover.png)

4. <span data-ttu-id="0f5e6-165">A hello **bevezetés** ablaktáblán válassza előbb **egy másik kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="0f5e6-165">On hello **Getting Started** pane, select **Another server**</span></span>

    ![Egy másik kiszolgáló](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. <span data-ttu-id="0f5e6-167">Adja meg a hello tárolói hitelesítő adatok fájlját, amely megfelel a toohello *minta tároló*, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-167">Provide hello vault credential file that corresponds toohello *Sample vault*, and click **Next**.</span></span>

    <span data-ttu-id="0f5e6-168">Ha hello tárolói hitelesítő adatok fájlját érvénytelen (vagy lejárt), egy új tárolói hitelesítő adatok fájlját letöltését hello *minta tároló* a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-168">If hello vault credential file is invalid (or expired), download a new vault credential file from hello *Sample vault* in hello Azure portal.</span></span> <span data-ttu-id="0f5e6-169">Miután megadta a egy érvényes tároló hitelesítő adatait, megfelelő mentési tároló hello hello neve jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-169">Once you provide a valid vault credential, hello name of hello corresponding Backup Vault appears.</span></span>


6. <span data-ttu-id="0f5e6-170">A hello **biztonsági mentés kiszolgáló kiválasztása** ablaktáblában válassza hello *forrásgép* megjelenített gépek hello listából, és adja meg a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-170">On hello **Select Backup Server** pane, select hello *Source machine* from hello list of displayed machines and provide hello passphrase.</span></span> <span data-ttu-id="0f5e6-171">Ezután kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-171">Then click **Next**.</span></span>

    ![Számítógépek listája](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. <span data-ttu-id="0f5e6-173">A hello **válassza ki a helyreállítási mód** ablaktáblán válassza ki az **egyes fájlok és mappák** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-173">On hello **Select Recovery Mode** pane, select **Individual files and folders** and click **Next**.</span></span>

    ![Keresés](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. <span data-ttu-id="0f5e6-175">A hello **mennyiség kiválasztása és a dátum** ablaktáblában hello fájlok és/vagy mappákról, amelyeket toorestore tartalmazó select hello kötetet.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-175">On hello **Select Volume and Date** pane, select hello volume that contains hello files and/or folders you want toorestore.</span></span>

    <span data-ttu-id="0f5e6-176">Hello naptárban válasszon ki egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-176">On hello calendar, select a recovery point.</span></span> <span data-ttu-id="0f5e6-177">Bármelyik helyreállítási pontra állíthatja vissza időben.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-177">You can restore from any recovery point in time.</span></span> <span data-ttu-id="0f5e6-178">A dátumok **félkövér** jelző hello legalább egy helyreállítási pont rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-178">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="0f5e6-179">Miután egy dátumot, ha több helyreállítási pont nem érhető el, hello adott helyreállítási pontot választhat hello **idő** legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-179">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Keresési elemek](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. <span data-ttu-id="0f5e6-181">Kattintson a **csatlakoztatási** toolocally csatlakoztatási hello helyreállítási pont a helyreállítási kötetként a *célgépen*.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-181">Click **Mount** toolocally mount hello recovery point as a recovery volume on your *Target machine*.</span></span>

10. <span data-ttu-id="0f5e6-182">A hello **tallózással keresse meg és a fájlok helyreállítása** ablaktáblában kattintson **Tallózás** tooopen Windows Explorer és hello fájlok és mappák.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-182">On hello **Browse and Recover Files** pane, click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span>

    ![Titkosítás](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. <span data-ttu-id="0f5e6-184">A Windows Intézőben hello fájlok és/vagy mappák másolása hello helyreállítási kötet, és illessze be a tooyour *célgépen* helyét.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-184">In Windows Explorer, copy hello files and/or folders from hello recovery volume and paste them tooyour *Target machine* location.</span></span> <span data-ttu-id="0f5e6-185">Nyissa meg vagy adatfolyam-fájlok hello közvetlenül hello helyreállítási kötet, és ellenőrizze a megfelelő verzió helyreállítás hello.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-185">You can open or stream hello files directly from hello recovery volume and verify hello correct versions are recovered.</span></span>

    ![Titkosítás](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. <span data-ttu-id="0f5e6-187">Ha elkészült a visszaállítási hello fájlok és/vagy mappák hello **tallózással keresse meg és a helyreállítási fájlokat** ablaktáblán kattintson a **leválasztási**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-187">When you are finished restoring hello files and/or folders, on hello **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="0f5e6-188">Kattintson a **Igen** tooconfirm, amelyet az toounmount hello kötet.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-188">Then click **Yes** tooconfirm that you want toounmount hello volume.</span></span>

    ![Titkosítás](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="0f5e6-190">Ha leválasztási gombra nem kattint, hello helyreállítási kötet marad csatlakoztatott hat órán át hello óta, amikor csatlakoztatva lett.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-190">If you do not click Unmount, hello Recovery Volume will remain mounted for 6 hours from hello time when it was mounted.</span></span> <span data-ttu-id="0f5e6-191">Azonban hello csatlakoztatási ideje kiterjesztett legfeljebb 24 óra egy folyamatban lévő fájlmásolás esetén.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-191">However, hello mount time is extended upto a maximum of 24 hours in case of an ongoing file-copy.</span></span> <span data-ttu-id="0f5e6-192">Nincs biztonsági mentési műveletek fog futni, amíg hello kötet csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-192">No backup operations will run while hello volume is mounted.</span></span> <span data-ttu-id="0f5e6-193">Minden ütemezett biztonsági mentési művelet toorun hello időszakban, amikor hello kötet csatlakoztatva van, miután hello helyreállítási kötet nem fog futni.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-193">Any backup operation scheduled toorun during hello time when hello volume is mounted, will run after hello recovery volume is unmounted.</span></span>
    >

## <a name="troubleshooting"></a><span data-ttu-id="0f5e6-194">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="0f5e6-194">Troubleshooting</span></span>
<span data-ttu-id="0f5e6-195">Ha az Azure Backup szolgáltatás sikeresen csatlakoztatható hello helyreállítási kötet több percig való kattintás után is **csatlakoztatási** vagy sikertelen toomount hello helyreállítási kötet egy vagy több hiba, kövesse az alábbi helyreállítása általában toobegin hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-195">If Azure Backup does not successfully mount hello recovery volume even after several minutes of clicking **Mount** or fails toomount hello recovery volume with one or more errors, follow hello steps below toobegin recovering normally.</span></span>

1.  <span data-ttu-id="0f5e6-196">Szakítsa meg hello folyamatban lévő csatlakozási folyamat, abban az esetben, ha több percig futott.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-196">Cancel hello ongoing mount process in case it has been running for several minutes.</span></span>

2.  <span data-ttu-id="0f5e6-197">Győződjön meg arról, hogy hello hello Azure Backup szolgáltatás ügynökének legújabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-197">Ensure that you are on hello latest version of hello Azure Backup agent.</span></span> <span data-ttu-id="0f5e6-198">toofind hello verzió adatok Azure Backup szolgáltatás ügynökének, kattintson a **kapcsolatban a Microsoft Azure Recovery Services Agent** a hello **műveletek** ablaktábla a Microsoft Azure Backup szolgáltatás konzolon, és győződjön meg arról, hogy hello  **Verzió** értéke nagyobb, mint az említett hello verzió egyenlő tooor [Ez a cikk](https://go.microsoft.com/fwlink/?linkid=229525).</span><span class="sxs-lookup"><span data-stu-id="0f5e6-198">toofind out hello version information of Azure Backup agent, click on **About Microsoft Azure Recovery Services Agent** on hello **Actions** pane of Microsoft Azure Backup console and ensure that hello **Version** number is equal tooor higher than hello version mentioned in [this article](https://go.microsoft.com/fwlink/?linkid=229525).</span></span> <span data-ttu-id="0f5e6-199">Letöltheti a legújabb verziót hello [Itt](https://go.microsoft.com/fwLink/?LinkID=288905)</span><span class="sxs-lookup"><span data-stu-id="0f5e6-199">You can download hello latest version from [here](https://go.microsoft.com/fwLink/?LinkID=288905)</span></span>

3.  <span data-ttu-id="0f5e6-200">Nyissa meg túl**Eszközkezelő** -> **tárolóvezérlők** győződjön meg arról, keresse meg és **Microsoft iSCSI-kezdeményező**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-200">Go too**Device Manager** -> **Storage Controllers** and ensure that you can locate **Microsoft iSCSI Initiator**.</span></span> <span data-ttu-id="0f5e6-201">Ha azok kikereshetők, közvetlenül nyissa meg az alábbi 7 toostep.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-201">If you can locate it, directly go toostep 7 below.</span></span> 

4.  <span data-ttu-id="0f5e6-202">Ha a Microsoft iSCSI-kezdeményező szolgáltatás nem találja, ahogy azt korábban említettük, a 3. lépésben, ellenőrizze toosee, ha a bejegyzés alatt található **Eszközkezelő** -> **tárolóvezérlők** nevű  **Ismeretlen eszköz** hardver azonosítójú **ROOT\ISCSIPRT**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-202">If you cannot locate Microsoft iSCSI Initiator service as mentioned in step 3, check toosee if you can find an entry under **Device Manager** -> **Storage Controllers** called **Unknown Device** with Hardware ID **ROOT\ISCSIPRT**.</span></span>

5.  <span data-ttu-id="0f5e6-203">Kattintson a jobb gombbal **ismeretlen eszköz** válassza **illesztőprogram frissítése**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-203">Right click on **Unknown Device** and select **Update Driver Software**.</span></span>

6.  <span data-ttu-id="0f5e6-204">Hello illesztőprogram frissítése hello beállítással túl **automatikusan frissített illesztőprogram keresése**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-204">Update hello driver by selecting hello option too **Search automatically for updated driver software**.</span></span> <span data-ttu-id="0f5e6-205">Hello frissítés megvalósításának meg kell változtatni **ismeretlen eszköz** túl**Microsoft iSCSI-kezdeményező** alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-205">Completion of hello update should change **Unknown Device** too**Microsoft iSCSI Initiator** as shown below.</span></span> 

    ![Titkosítás](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  <span data-ttu-id="0f5e6-207">Nyissa meg túl**Feladatkezelő** -> **szolgáltatások (helyi)** -> **Microsoft iSCSI-kezdeményező szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-207">Go too**Task Manager** -> **Services (Local)** -> **Microsoft iSCSI Initiator Service**.</span></span> 

    ![Titkosítás](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  <span data-ttu-id="0f5e6-209">Indítsa újra a hello Microsoft iSCSI-kezdeményező szolgáltatás hello szolgáltatásban, ehhez kattintson a jobb gombbal kattintva **leállítása** és a jobb gombbal kattint rá újra, és kattintson a Tovább **Start**.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-209">Restart hello Microsoft iSCSI Initiator service by right-clicking on hello service, clicking on **Stop** and further right clicking again and clicking on **Start**.</span></span>

9.  <span data-ttu-id="0f5e6-210">Ismételje meg a helyreállítás azonnali visszaállítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-210">Retry recovering using Instant Restore.</span></span> 

<span data-ttu-id="0f5e6-211">Hello helyreállítási továbbra is sikertelen, ha indítsa újra a kiszolgálót vagy Windows-ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-211">If hello recovery still fails, reboot your server/client.</span></span> <span data-ttu-id="0f5e6-212">Ha a számítógép újraindítása nem kívánatos vagy hello helyreállítási továbbra sem sikerül hello kiszolgáló újraindítása után is, végezze el egy másik gép, és forduljon a Azure támogatja címen túl[Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) és a támogatási kérelem elküldése.</span><span class="sxs-lookup"><span data-stu-id="0f5e6-212">If a reboot is not desirable or hello recovery still fails even after rebooting hello server, try recovering from an Alternate Machine, and contact Azure Support by going too[Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) and submitting a support request.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f5e6-213">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0f5e6-213">Next steps</span></span>
* <span data-ttu-id="0f5e6-214">Most, hogy a fájlok és mappák már helyreállítva, akkor [kezelheti a biztonsági másolatok](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="0f5e6-214">Now that you've recovered your files and folders, you can [manage your backups](backup-azure-manage-windows-server.md).</span></span>
