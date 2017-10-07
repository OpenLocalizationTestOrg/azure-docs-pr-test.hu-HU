---
title: "Azure biztonsági mentés tooreplace aaaUse a szalag infrastruktúra |} Microsoft Docs"
description: "Ismerje meg, hogy Azure Backup szolgáltatás biztosítja a szalag-szerű szemantikáját, amely lehetővé teszi toobackup, és állítsa vissza az adatokat az Azure-ban"
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
ms.openlocfilehash: 4c5b095d95d39267c54b1eed9427bda09658bb94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-your-long-term-storage-from-tape-toohello-azure-cloud"></a><span data-ttu-id="abbc0-103">A hosszú távú tárolás áthelyezése szalag toohello Azure-felhőbe</span><span class="sxs-lookup"><span data-stu-id="abbc0-103">Move your long-term storage from tape toohello Azure cloud</span></span>
<span data-ttu-id="abbc0-104">Azure biztonsági mentési és a System Center Data Protection Manager-ügyfél a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="abbc0-104">Azure Backup and System Center Data Protection Manager customers can:</span></span>

* <span data-ttu-id="abbc0-105">Biztonsági mentést az ütemezést, amely a legjobban hello szervezet saját igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="abbc0-105">Back up data in schedules which best suit hello organizational needs.</span></span>
* <span data-ttu-id="abbc0-106">A hosszabb ideig tartó hello biztonsági mentési adatok megőrzése mellett</span><span class="sxs-lookup"><span data-stu-id="abbc0-106">Retain hello backup data for longer periods</span></span>
* <span data-ttu-id="abbc0-107">Ellenőrizze a hosszú távú megőrzési részét (szalag) és nem kell Azure.</span><span class="sxs-lookup"><span data-stu-id="abbc0-107">Make Azure a part of their long-term retention needs (instead of tape).</span></span>

<span data-ttu-id="abbc0-108">Ez a cikk azt ismerteti, hogyan ügyfelek biztonsági mentési és adatmegőrzési házirendek engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="abbc0-108">This article explains how customers can enable backup and retention policies.</span></span> <span data-ttu-id="abbc0-109">Szalagok tooaddress használó ügyfelek a hosszú-távú adatmegőrzés van szüksége, most már rendelkezik egy hatékony és életképes alternatív hello rendelkezésre állással, a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="abbc0-109">Customers who use tapes tooaddress their long-term-retention needs now have a powerful and viable alternative with hello availability of this feature.</span></span> <span data-ttu-id="abbc0-110">hello szolgáltatást hello Azure Backup szolgáltatás legújabb kiadása hello (elérhető [Itt](http://aka.ms/azurebackup_agent)).</span><span class="sxs-lookup"><span data-stu-id="abbc0-110">hello feature is enabled in hello latest release of hello Azure Backup (which is available [here](http://aka.ms/azurebackup_agent)).</span></span> <span data-ttu-id="abbc0-111">A System Center DPM ügyfeleknek frissíteniük kell, legalább, a DPM használata előtt a DPM 2012 R2 – UR5 hello Azure Backup szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="abbc0-111">System Center DPM customers must update to, at least, DPM 2012 R2 UR5 before using DPM with hello Azure Backup service.</span></span>

## <a name="what-is-hello-backup-schedule"></a><span data-ttu-id="abbc0-112">Mi az a biztonsági mentési ütemezés hello?</span><span class="sxs-lookup"><span data-stu-id="abbc0-112">What is hello Backup Schedule?</span></span>
<span data-ttu-id="abbc0-113">hello biztonsági mentés ütemezése hello hello biztonsági mentés gyakoriságát jelzi.</span><span class="sxs-lookup"><span data-stu-id="abbc0-113">hello backup schedule indicates hello frequency of hello backup operation.</span></span> <span data-ttu-id="abbc0-114">Például a következő képernyő hello hello beállításai azt jelzik, hogy biztonsági mentést készít a napi 18: 00 és éjfélkor.</span><span class="sxs-lookup"><span data-stu-id="abbc0-114">For example, hello settings in hello following screen indicate that backups are taken daily at 6pm and at midnight.</span></span>

![Napi ütemezés](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

<span data-ttu-id="abbc0-116">Az ügyfelek is ütemezheti a heti biztonsági másolatot.</span><span class="sxs-lookup"><span data-stu-id="abbc0-116">Customers can also schedule a weekly backup.</span></span> <span data-ttu-id="abbc0-117">Például hello beállítások a következő képernyő hello azt jelzik, hogy a biztonsági mentések kerül-e a minden másodlagos vasárnap & szerda 9:30 -kor és 13:00:00.</span><span class="sxs-lookup"><span data-stu-id="abbc0-117">For example, hello settings in hello following screen indicate that backups are taken every alternate Sunday & Wednesday at 9:30AM and 1:00AM.</span></span>

![Heti ütemezés](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-hello-retention-policy"></a><span data-ttu-id="abbc0-119">Mi az az adatmegőrzési hello?</span><span class="sxs-lookup"><span data-stu-id="abbc0-119">What is hello Retention Policy?</span></span>
<span data-ttu-id="abbc0-120">hello adatmegőrzési, amelynek meg kell tárolt hello biztonsági mentési hello időtartamát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="abbc0-120">hello retention policy specifies hello duration for which hello backup must be stored.</span></span> <span data-ttu-id="abbc0-121">Ahelyett, hogy csak adja meg az összes biztonsági mentési pont "egyszerű policy", az ügyfelek adhatja meg a különböző megőrzési házirend alapján hello biztonsági mentés időpontjában.</span><span class="sxs-lookup"><span data-stu-id="abbc0-121">Rather than just specifying a “flat policy” for all backup points, customers can specify different retention policies based on when hello backup is taken.</span></span> <span data-ttu-id="abbc0-122">Például hello biztonsági mentési pontok végrehajtott naponta, ami működési helyreállítási pontjaként szolgál 90 napig őrzi meg.</span><span class="sxs-lookup"><span data-stu-id="abbc0-122">For example, hello backup point taken daily, which serves as an operational recovery point, is preserved for 90 days.</span></span> <span data-ttu-id="abbc0-123">hello naplózási célra negyedévenként hello végén végrehajtott biztonsági mentési pontok hosszabb ideig megőrzi.</span><span class="sxs-lookup"><span data-stu-id="abbc0-123">hello backup point taken at hello end of each quarter for audit purposes is preserved for a longer duration.</span></span>

![Adatmegőrzési házirend](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

<span data-ttu-id="abbc0-125">hello "adatmegőrzési pontokat" Ebben a házirendben megadott száma összesen az 90 (napi pontok) + 40 (minden egy 10 éve negyedév) = 130.</span><span class="sxs-lookup"><span data-stu-id="abbc0-125">hello total number of “retention points” specified in this policy is 90 (daily points) + 40 (one each quarter for 10 years) = 130.</span></span>

## <a name="example--putting-both-together"></a><span data-ttu-id="abbc0-126">Példa – üzembe egyaránt</span><span class="sxs-lookup"><span data-stu-id="abbc0-126">Example – Putting both together</span></span>
![A minta képernyő](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. <span data-ttu-id="abbc0-128">**Napi adatmegőrzési**: naponta végrehajtott biztonsági másolatai hét napja.</span><span class="sxs-lookup"><span data-stu-id="abbc0-128">**Daily retention policy**: Backups taken daily are stored for seven days.</span></span>
2. <span data-ttu-id="abbc0-129">**Heti adatmegőrzési**: négy hétig minden nap, éjfélkor és 18: 00 Szombat készített biztonsági másolatok megmaradnak</span><span class="sxs-lookup"><span data-stu-id="abbc0-129">**Weekly retention policy**: Backups taken every day at midnight and 6PM Saturday are preserved for four weeks</span></span>
3. <span data-ttu-id="abbc0-130">**Havi adatmegőrzési**: éjfél és 18: 00 a hello minden hónap utolsó szombat készített biztonsági másolatok megmaradnak 12 hónapig</span><span class="sxs-lookup"><span data-stu-id="abbc0-130">**Monthly retention policy**: Backups taken at midnight and 6pm on hello last Saturday of each month are preserved for 12 months</span></span>
4. <span data-ttu-id="abbc0-131">**Éves adatmegőrzési**: készített biztonsági másolatokat hello éjfélkor utolsó szombat minden március 10 éve megmaradnak</span><span class="sxs-lookup"><span data-stu-id="abbc0-131">**Yearly retention policy**: Backups taken at midnight on hello last Saturday of every March are preserved for 10 years</span></span>

<span data-ttu-id="abbc0-132">hello "adatmegőrzési pontokat" teljes száma (a pont, amelyből az ügyfél adatok visszaállíthatók) hello a fenti ábrán van kiszámítása a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="abbc0-132">hello total number of “retention points” (points from which a customer can restore data) in hello preceding diagram is computed as follows:</span></span>

* <span data-ttu-id="abbc0-133">két hét nap = 14 naponta pont helyreállítási pontok</span><span class="sxs-lookup"><span data-stu-id="abbc0-133">two points per day for seven days = 14 recovery points</span></span>
* <span data-ttu-id="abbc0-134">két négy hét = 8 hetente pont helyreállítási pontok</span><span class="sxs-lookup"><span data-stu-id="abbc0-134">two points per week for four weeks = 8 recovery points</span></span>
* <span data-ttu-id="abbc0-135">két havonta a 12 hónappal = 24 pont helyreállítási pontok</span><span class="sxs-lookup"><span data-stu-id="abbc0-135">two points per month for 12 months = 24 recovery points</span></span>
* <span data-ttu-id="abbc0-136">egy pont évi 10 év = 10 helyreállítási pontok</span><span class="sxs-lookup"><span data-stu-id="abbc0-136">one point per year per 10 years = 10 recovery points</span></span>

<span data-ttu-id="abbc0-137">helyreállítási pontok száma hello 56.</span><span class="sxs-lookup"><span data-stu-id="abbc0-137">hello total number of recovery points is 56.</span></span>

> [!NOTE]
> <span data-ttu-id="abbc0-138">Azure biztonsági mentés nincs korlátozás a helyreállítási pontok száma.</span><span class="sxs-lookup"><span data-stu-id="abbc0-138">Azure backup doesn't have a restriction on number of recovery points.</span></span>
>
>

## <a name="advanced-configuration"></a><span data-ttu-id="abbc0-139">Speciális konfiguráció</span><span class="sxs-lookup"><span data-stu-id="abbc0-139">Advanced configuration</span></span>
<span data-ttu-id="abbc0-140">Kattintva **módosítás** hello megelőző képernyő, a felhasználóknak kell további rugalmasságot adatmegőrzési ütemterv meghatározása.</span><span class="sxs-lookup"><span data-stu-id="abbc0-140">By clicking **Modify** in hello preceding screen, customers have further flexibility in specifying retention schedules.</span></span>

![Módosítás](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a><span data-ttu-id="abbc0-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="abbc0-142">Next Steps</span></span>
<span data-ttu-id="abbc0-143">Azure biztonsági mentéssel kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="abbc0-143">For more information about Azure Backup, see:</span></span>

* [<span data-ttu-id="abbc0-144">Bevezetés tooAzure biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="abbc0-144">Introduction tooAzure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="abbc0-145">Próbálja meg az Azure biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="abbc0-145">Try Azure Backup</span></span>](backup-try-azure-backup-in-10-mins.md)
