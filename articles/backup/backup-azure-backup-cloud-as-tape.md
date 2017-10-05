---
title: "Cserélje le a szalag infrastruktúra az Azure Backup használatával |} Microsoft Docs"
description: "Ismerje meg, hogy Azure Backup szolgáltatás biztosítja a szalag-szerű szemantikáját, amely lehetővé teszi biztonsági mentése és visszaállítása az adatok Azure-ban"
services: backup
documentationcenter: 
author: trinadhk
manager: vijayts
editor: 
ms.assetid: 2e1bb67d-986c-4437-8056-3a63169b4214
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 1/10/2017
ms.author: saurse;trinadhk;markgal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0f3152daf5f91f7c9e540797bf09b21969d2d33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="move-your-long-term-storage-from-tape-to-the-azure-cloud"></a><span data-ttu-id="b4af0-103">A hosszú távú tárolás szalagon áthelyezése az Azure felhőben</span><span class="sxs-lookup"><span data-stu-id="b4af0-103">Move your long-term storage from tape to the Azure cloud</span></span>
<span data-ttu-id="b4af0-104">Azure biztonsági mentési és a System Center Data Protection Manager-ügyfél a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="b4af0-104">Azure Backup and System Center Data Protection Manager customers can:</span></span>

* <span data-ttu-id="b4af0-105">Az ütemezést, amely a szervezet igényeinek legjobban adatok biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="b4af0-105">Back up data in schedules which best suit the organizational needs.</span></span>
* <span data-ttu-id="b4af0-106">A biztonsági mentési adatok megőrzése mellett hosszabb ideig</span><span class="sxs-lookup"><span data-stu-id="b4af0-106">Retain the backup data for longer periods</span></span>
* <span data-ttu-id="b4af0-107">Ellenőrizze a hosszú távú megőrzési részét (szalag) és nem kell Azure.</span><span class="sxs-lookup"><span data-stu-id="b4af0-107">Make Azure a part of their long-term retention needs (instead of tape).</span></span>

<span data-ttu-id="b4af0-108">Ez a cikk azt ismerteti, hogyan ügyfelek biztonsági mentési és adatmegőrzési házirendek engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b4af0-108">This article explains how customers can enable backup and retention policies.</span></span> <span data-ttu-id="b4af0-109">Használja a szalagokat a hosszú-távú adatmegőrzés megoldására kell most rendelkeznek egy hatékony és életképes alternatív, ez a szolgáltatás rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="b4af0-109">Customers who use tapes to address their long-term-retention needs now have a powerful and viable alternative with the availability of this feature.</span></span> <span data-ttu-id="b4af0-110">A szolgáltatás engedélyezve van az Azure Backup szolgáltatás legújabb kiadása (elérhető [Itt](http://aka.ms/azurebackup_agent)).</span><span class="sxs-lookup"><span data-stu-id="b4af0-110">The feature is enabled in the latest release of the Azure Backup (which is available [here](http://aka.ms/azurebackup_agent)).</span></span> <span data-ttu-id="b4af0-111">A System Center DPM ügyfeleknek frissíteniük kell, legalább, a DPM 2012 R2 – UR5 az Azure Backup szolgáltatással a DPM használata előtt.</span><span class="sxs-lookup"><span data-stu-id="b4af0-111">System Center DPM customers must update to, at least, DPM 2012 R2 UR5 before using DPM with the Azure Backup service.</span></span>

## <a name="what-is-the-backup-schedule"></a><span data-ttu-id="b4af0-112">Mi az a biztonsági mentési ütemezés?</span><span class="sxs-lookup"><span data-stu-id="b4af0-112">What is the Backup Schedule?</span></span>
<span data-ttu-id="b4af0-113">A biztonsági mentési ütemezés a biztonsági mentés gyakoriságát jelöli.</span><span class="sxs-lookup"><span data-stu-id="b4af0-113">The backup schedule indicates the frequency of the backup operation.</span></span> <span data-ttu-id="b4af0-114">A beállítások a következő képernyőn például azt jelzi, hogy biztonsági mentést készít a napi 18: 00 és éjfélkor.</span><span class="sxs-lookup"><span data-stu-id="b4af0-114">For example, the settings in the following screen indicate that backups are taken daily at 6pm and at midnight.</span></span>

![Napi ütemezés](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

<span data-ttu-id="b4af0-116">Az ügyfelek is ütemezheti a heti biztonsági másolatot.</span><span class="sxs-lookup"><span data-stu-id="b4af0-116">Customers can also schedule a weekly backup.</span></span> <span data-ttu-id="b4af0-117">Például a következő képernyő beállításait jelzi, hogy biztonsági mentés készül minden másodlagos vasárnap & szerda 9:30 -kor és 13:00:00.</span><span class="sxs-lookup"><span data-stu-id="b4af0-117">For example, the settings in the following screen indicate that backups are taken every alternate Sunday & Wednesday at 9:30AM and 1:00AM.</span></span>

![Heti ütemezés](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-the-retention-policy"></a><span data-ttu-id="b4af0-119">Mi az az adatmegőrzési?</span><span class="sxs-lookup"><span data-stu-id="b4af0-119">What is the Retention Policy?</span></span>
<span data-ttu-id="b4af0-120">Az adatmegőrzési Megadja azt az időtartamot, amelyre a biztonsági mentést kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="b4af0-120">The retention policy specifies the duration for which the backup must be stored.</span></span> <span data-ttu-id="b4af0-121">Ahelyett, hogy csak adja meg az összes biztonsági mentési pont "egyszerű policy", az ügyfelek házirendeket határozhatnak meg, a különböző megőrzési alapján, ha a biztonsági mentés használatban van.</span><span class="sxs-lookup"><span data-stu-id="b4af0-121">Rather than just specifying a “flat policy” for all backup points, customers can specify different retention policies based on when the backup is taken.</span></span> <span data-ttu-id="b4af0-122">Például a biztonsági mentési pont végrehajtott naponta, ami működési helyreállítási pontjaként szolgál 90 napig őrzi meg.</span><span class="sxs-lookup"><span data-stu-id="b4af0-122">For example, the backup point taken daily, which serves as an operational recovery point, is preserved for 90 days.</span></span> <span data-ttu-id="b4af0-123">A naplózási célra minden negyedév végén végrehajtott biztonsági mentési pont hosszabb ideig megőrzi.</span><span class="sxs-lookup"><span data-stu-id="b4af0-123">The backup point taken at the end of each quarter for audit purposes is preserved for a longer duration.</span></span>

![Adatmegőrzési házirend](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

<span data-ttu-id="b4af0-125">"Adatmegőrzési pontokat" Ebben a házirendben megadott teljes száma érték a 90 (napi pontok) + 40 (minden egy 10 éve negyedév) = 130.</span><span class="sxs-lookup"><span data-stu-id="b4af0-125">The total number of “retention points” specified in this policy is 90 (daily points) + 40 (one each quarter for 10 years) = 130.</span></span>

## <a name="example--putting-both-together"></a><span data-ttu-id="b4af0-126">Példa – üzembe egyaránt</span><span class="sxs-lookup"><span data-stu-id="b4af0-126">Example – Putting both together</span></span>
![A minta képernyő](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. <span data-ttu-id="b4af0-128">**Napi adatmegőrzési**: naponta végrehajtott biztonsági másolatai hét napja.</span><span class="sxs-lookup"><span data-stu-id="b4af0-128">**Daily retention policy**: Backups taken daily are stored for seven days.</span></span>
2. <span data-ttu-id="b4af0-129">**Heti adatmegőrzési**: négy hétig minden nap, éjfélkor és 18: 00 Szombat készített biztonsági másolatok megmaradnak</span><span class="sxs-lookup"><span data-stu-id="b4af0-129">**Weekly retention policy**: Backups taken every day at midnight and 6PM Saturday are preserved for four weeks</span></span>
3. <span data-ttu-id="b4af0-130">**Havi adatmegőrzési**: éjfél és 18: 00 minden hónap utolsó szombaton készített biztonsági másolatok megmaradnak 12 hónapig</span><span class="sxs-lookup"><span data-stu-id="b4af0-130">**Monthly retention policy**: Backups taken at midnight and 6pm on the last Saturday of each month are preserved for 12 months</span></span>
4. <span data-ttu-id="b4af0-131">**Éves adatmegőrzési**: minden március utolsó vasárnap éjfélkor készített biztonsági másolatok tíz éve megmaradnak</span><span class="sxs-lookup"><span data-stu-id="b4af0-131">**Yearly retention policy**: Backups taken at midnight on the last Saturday of every March are preserved for 10 years</span></span>

<span data-ttu-id="b4af0-132">"Adatmegőrzési pontokat" teljes száma (a pont, amelyből az ügyfél az adatokat állíthatja vissza) a fenti ábrán számított az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b4af0-132">The total number of “retention points” (points from which a customer can restore data) in the preceding diagram is computed as follows:</span></span>

* <span data-ttu-id="b4af0-133">két hét nap = 14 naponta pont helyreállítási pontok</span><span class="sxs-lookup"><span data-stu-id="b4af0-133">two points per day for seven days = 14 recovery points</span></span>
* <span data-ttu-id="b4af0-134">két négy hét = 8 hetente pont helyreállítási pontok</span><span class="sxs-lookup"><span data-stu-id="b4af0-134">two points per week for four weeks = 8 recovery points</span></span>
* <span data-ttu-id="b4af0-135">két havonta a 12 hónappal = 24 pont helyreállítási pontok</span><span class="sxs-lookup"><span data-stu-id="b4af0-135">two points per month for 12 months = 24 recovery points</span></span>
* <span data-ttu-id="b4af0-136">egy pont évi 10 év = 10 helyreállítási pontok</span><span class="sxs-lookup"><span data-stu-id="b4af0-136">one point per year per 10 years = 10 recovery points</span></span>

<span data-ttu-id="b4af0-137">A helyreállítási pontok számát 56.</span><span class="sxs-lookup"><span data-stu-id="b4af0-137">The total number of recovery points is 56.</span></span>

> [!NOTE]
> <span data-ttu-id="b4af0-138">Azure biztonsági mentés nincs korlátozás a helyreállítási pontok száma.</span><span class="sxs-lookup"><span data-stu-id="b4af0-138">Azure backup doesn't have a restriction on number of recovery points.</span></span>
>
>

## <a name="advanced-configuration"></a><span data-ttu-id="b4af0-139">Speciális konfiguráció</span><span class="sxs-lookup"><span data-stu-id="b4af0-139">Advanced configuration</span></span>
<span data-ttu-id="b4af0-140">Kattintva **módosítás** az előző képernyőn a felhasználóknak kell további rugalmasságot adatmegőrzési ütemterv meghatározása.</span><span class="sxs-lookup"><span data-stu-id="b4af0-140">By clicking **Modify** in the preceding screen, customers have further flexibility in specifying retention schedules.</span></span>

![Módosítás](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a><span data-ttu-id="b4af0-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4af0-142">Next Steps</span></span>
<span data-ttu-id="b4af0-143">Azure biztonsági mentéssel kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="b4af0-143">For more information about Azure Backup, see:</span></span>

* [<span data-ttu-id="b4af0-144">Az Azure Backup bemutatása</span><span class="sxs-lookup"><span data-stu-id="b4af0-144">Introduction to Azure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="b4af0-145">Próbálja meg az Azure biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="b4af0-145">Try Azure Backup</span></span>](backup-try-azure-backup-in-10-mins.md)
