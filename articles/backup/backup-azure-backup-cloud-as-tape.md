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
# <a name="move-your-long-term-storage-from-tape-toohello-azure-cloud"></a>A hosszú távú tárolás áthelyezése szalag toohello Azure-felhőbe
Azure biztonsági mentési és a System Center Data Protection Manager-ügyfél a következőket teheti:

* Biztonsági mentést az ütemezést, amely a legjobban hello szervezet saját igényeinek megfelelően.
* A hosszabb ideig tartó hello biztonsági mentési adatok megőrzése mellett
* Ellenőrizze a hosszú távú megőrzési részét (szalag) és nem kell Azure.

Ez a cikk azt ismerteti, hogyan ügyfelek biztonsági mentési és adatmegőrzési házirendek engedélyezéséhez. Szalagok tooaddress használó ügyfelek a hosszú-távú adatmegőrzés van szüksége, most már rendelkezik egy hatékony és életképes alternatív hello rendelkezésre állással, a szolgáltatás. hello szolgáltatást hello Azure Backup szolgáltatás legújabb kiadása hello (elérhető [Itt](http://aka.ms/azurebackup_agent)). A System Center DPM ügyfeleknek frissíteniük kell, legalább, a DPM használata előtt a DPM 2012 R2 – UR5 hello Azure Backup szolgáltatás.

## <a name="what-is-hello-backup-schedule"></a>Mi az a biztonsági mentési ütemezés hello?
hello biztonsági mentés ütemezése hello hello biztonsági mentés gyakoriságát jelzi. Például a következő képernyő hello hello beállításai azt jelzik, hogy biztonsági mentést készít a napi 18: 00 és éjfélkor.

![Napi ütemezés](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

Az ügyfelek is ütemezheti a heti biztonsági másolatot. Például hello beállítások a következő képernyő hello azt jelzik, hogy a biztonsági mentések kerül-e a minden másodlagos vasárnap & szerda 9:30 -kor és 13:00:00.

![Heti ütemezés](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-hello-retention-policy"></a>Mi az az adatmegőrzési hello?
hello adatmegőrzési, amelynek meg kell tárolt hello biztonsági mentési hello időtartamát határozza meg. Ahelyett, hogy csak adja meg az összes biztonsági mentési pont "egyszerű policy", az ügyfelek adhatja meg a különböző megőrzési házirend alapján hello biztonsági mentés időpontjában. Például hello biztonsági mentési pontok végrehajtott naponta, ami működési helyreállítási pontjaként szolgál 90 napig őrzi meg. hello naplózási célra negyedévenként hello végén végrehajtott biztonsági mentési pontok hosszabb ideig megőrzi.

![Adatmegőrzési házirend](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

hello "adatmegőrzési pontokat" Ebben a házirendben megadott száma összesen az 90 (napi pontok) + 40 (minden egy 10 éve negyedév) = 130.

## <a name="example--putting-both-together"></a>Példa – üzembe egyaránt
![A minta képernyő](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. **Napi adatmegőrzési**: naponta végrehajtott biztonsági másolatai hét napja.
2. **Heti adatmegőrzési**: négy hétig minden nap, éjfélkor és 18: 00 Szombat készített biztonsági másolatok megmaradnak
3. **Havi adatmegőrzési**: éjfél és 18: 00 a hello minden hónap utolsó szombat készített biztonsági másolatok megmaradnak 12 hónapig
4. **Éves adatmegőrzési**: készített biztonsági másolatokat hello éjfélkor utolsó szombat minden március 10 éve megmaradnak

hello "adatmegőrzési pontokat" teljes száma (a pont, amelyből az ügyfél adatok visszaállíthatók) hello a fenti ábrán van kiszámítása a következőképpen:

* két hét nap = 14 naponta pont helyreállítási pontok
* két négy hét = 8 hetente pont helyreállítási pontok
* két havonta a 12 hónappal = 24 pont helyreállítási pontok
* egy pont évi 10 év = 10 helyreállítási pontok

helyreállítási pontok száma hello 56.

> [!NOTE]
> Azure biztonsági mentés nincs korlátozás a helyreállítási pontok száma.
>
>

## <a name="advanced-configuration"></a>Speciális konfiguráció
Kattintva **módosítás** hello megelőző képernyő, a felhasználóknak kell további rugalmasságot adatmegőrzési ütemterv meghatározása.

![Módosítás](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a>Következő lépések
Azure biztonsági mentéssel kapcsolatos további információkért lásd:

* [Bevezetés tooAzure biztonsági mentése](backup-introduction-to-azure-backup.md)
* [Próbálja meg az Azure biztonsági mentés](backup-try-azure-backup-in-10-mins.md)
