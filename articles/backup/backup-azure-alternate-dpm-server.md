---
title: "aaaRecover adatainak áttelepítését egy Azure biztonsági mentés |} Microsoft Docs"
description: "Korábban védett hello adatok helyreállítása tooa Recovery Services-tároló bármely Azure Backup Server regisztrált toothat tárolóból."
services: backup
documentationcenter: 
author: nkolli1
manager: shreeshd
editor: 
ms.assetid: a55f8c6b-3627-42e1-9d25-ed3e4ab17b1f
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: adigan;giridham;trinadhk;markgal
ms.openlocfilehash: 74847880e646c3c4f198afe318f1db30363d137a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="recover-data-from-azure-backup-server"></a>Adatok helyreállítása az Azure Backup Serverről
Azure Backup Server toorecover hello adatokat készített biztonsági mentést a Recovery Services-tároló tooa használhatja. hello folyamat ennek így hello Azure Backup Server kezelőkonzolján integrálva van, és hasonló toohello helyreállítási munkafolyamat más Azure Backup szolgáltatás-összetevők.

> [!NOTE]
> Ez a cikk esetén [System Center Data Protection Manager 2012 R2 vagy újabb verziójával UR7] (https://support.microsoft.com/en-us/kb/3065246) alkalmazható hello együtt [legújabb Azure Backup szolgáltatás ügynökének](http://aka.ms/azurebackup_agent).
>
>

toorecover adatainak áttelepítését egy Azure biztonsági mentés:

1. A hello **helyreállítási** hello Azure Backup Server kezelőkonzolján, lapján kattintson **"Külső DPM hozzáadása"** (hello a bal felső részén üdvözlő képernyőt).   
    ![Külső DPM hozzáadása](./media/backup-azure-alternate-dpm-server/add-external-dpm.png)
2. Új letöltési **hitelesítő adatokat tároló** hello társított hello tárolóból **Azure Backup Server** hello adatok helyreállítása folyamatban, ahol hello Azure Backup Server választhat hello Azure biztonsági mentés kiszolgálók listája Recovery Services-tároló hello regisztrált, és adja meg a hello **titkosítási jelszó** hello kiszolgálóhoz, amelynek adatok helyreállítása folyamatban.

    ![Külső DPM hitelesítő adatait](./media/backup-azure-alternate-dpm-server/external-dpm-credentials.png)

   > [!NOTE]
   > Csak az Azure biztonsági mentés kiszolgálók társított hello azonos regisztrációja tárolóval helyreállíthatók az adatok egymás.
   >
   >

    Amint hello külső Azure Backup Server sikeresen hozzáadva, navigálhat hello adatok hello külső kiszolgáló és hello helyi Azure Backup Server hello **helyreállítási** fülre.
3. Tallózás hello érhető el az üzemi kiszolgálók listája által védett külső Azure Backup Server hello, és válassza ki a megfelelő adatforrást hello.

    ![Keresse meg a külső DPM-kiszolgáló](./media/backup-azure-alternate-dpm-server/browse-external-dpm.png)
4. Válassza ki **hónapban és évben hello** a hello **helyreállítási pontok** legördülő listán válassza hello szükséges **helyreállítási dátum** amikor hello helyreállítási pont lett hello létrehozott, majd válassza a **Helyreállításkor**.

    Hello alsó ablaktáblában, amely tallózható és tooany hely helyreállítása meg fájlok és mappák listáját.

    ![Külső DPM-kiszolgáló helyreállítási pontok](./media/backup-azure-alternate-dpm-server/external-dpm-recoverypoint.png)
5. Kattintson jobb gombbal a hello megfelelő elemet, majd kattintson a **helyreállítása**.

    ![Külső DPM helyreállítás](./media/backup-azure-alternate-dpm-server/recover.png)
6. Felülvizsgálati hello **helyreállítása kijelölés**. Ellenőrizze a hello adatok és hello biztonsági másolat helyreállítás alatt álló időt, valamint hello biztonsági másolat létrehozásához használt hello forrás. Ha hello kiválasztása helytelen, kattintson a **Mégse** toonavigate hátsó toorecovery lapon tooselect megfelelő helyreállítási pont. Ha hello kijelölés helyes-e, kattintson a **következő**.

    ![Külső DPM helyreállítási összefoglaló](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-summary.png)
7. Válassza ki **helyreállítása másik helyre tooan**. **Tallózás** hello helyreállítási toohello megfelelő helyére.

    ![Külső DPM helyreállítás másik helyre](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-alternate-location.png)
8. Kapcsolódó túl hello választógomb**készítsen másolatot**, **kihagyása**, vagy **felülírása**.

   * **Készítsen másolatot** -hello fájl másolatát hozza létre, ha egy.
   * **Kihagyás** - Ha névütközés, nem tudja elhárítani hello fájlt, így hello eredeti fájlt.
   * **Írja felül** - Ha névütközés, felülírja a meglévő másolatát hello hello fájl.

     Válassza ki a megfelelő lehetőséget annak hello túl**biztonság visszaállítása**. Ahol hello adatok helyre hello célszámítógép biztonsági beállításainak hello vagy hello biztonsági beállítások megfelelő tooproduct hello idő hello helyreállítási pont létrehozása is alkalmazhatja.

     Azonosítsa e egy **értesítési** zajlik, miután hello helyreállítási sikeresen befejeződött.

     ![Külső DPM helyreállítási értesítések](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-notifications.png)
9. Hello **összegzés** képernyő, amennyiben a kiválasztott hello-beállítások listája. Miután rákattintott **"Helyreállítása"**, hello adatok az helyreállított toohello megfelelő helyszíni hely.

    ![Külső DPM-helyreállítási beállítások összegzése](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-options-summary.png)

   > [!NOTE]
   > hello helyreállítási feladat lesz figyelhető hello **figyelés** hello Azure Backup Server lapján.
   >
   >

    ![Helyreállítási figyelő](./media/backup-azure-alternate-dpm-server/monitoring-recovery.png)
10. Kattinthat **egyértelmű a külső DPM** a hello **helyreállítási** hello DPM kiszolgáló tooremove hello nézet hello külső DPM-kiszolgáló lapján.

    ![Külső DPM törlése](./media/backup-azure-alternate-dpm-server/clear-external-dpm.png)

## <a name="troubleshooting-error-messages"></a>Hibaüzenetek hibaelhárítása
| Nem. | Hibaüzenet | Hibaelhárítási lépések |
|:---:|:--- |:--- |
| 1. |A kiszolgáló nincs regisztrált toohello tárolónál, amelyet hello tároló hitelesítő adatait. |**OK:** Ez a hiba jelenik meg, ha nem tartozik hello tárolói hitelesítő adatok fájlját kijelölt Recovery Services-tároló toohello társított Azure Backup Server mely hello a helyreállítási kísérlet. <br> **Megoldás:** letöltési hello tárolói hitelesítő adatok fájlját az hello Recovery Services tároló toowhich hello Azure Backup-kiszolgáló regisztrálva van. |
| 2. |Hello helyreállítható adatok nem érhető el, vagy hello kijelölt kiszolgálón található DPM-kiszolgáló. |**OK:** nincsenek nincs más Azure biztonsági mentés kiszolgálók regisztrált toohello Recovery Services-tároló, vagy hello kiszolgálók még nem feltöltött hello metaadatok, vagy hello kiválasztott kiszolgáló nem egy Azure Backup Server (más néven a Windows Server vagy a Windows ügyfél). <br> **Megoldás:** Ha egyéb Azure biztonsági mentés kiszolgálók regisztrált toohello Recovery Services-tároló, győződjön meg arról, hogy hello legújabb Azure Backup szolgáltatás ügynöke telepítve van. <br>Ha más Azure biztonsági mentés kiszolgálók regisztrált toohello Recovery Services-tároló, várja meg a nap telepítési toostart hello helyreállítási folyamat után. ütemezett feladat hello fel kell töltenie az összes védett hello biztonsági mentések toocloud hello metaadatait. hello adatokat kell rendelkezésre a helyreállításhoz. |
| 3. |Egyetlen más DPM-kiszolgáló regisztrált toothis tárolóban. |**OK:** nincsenek egyéb Azure biztonsági mentés kiszolgálók, amelyek mely hello a helyreállítási kísérletek regisztrált toohello tárolóban.<br>**Megoldás:** Ha egyéb Azure biztonsági mentés kiszolgálók regisztrált toohello Recovery Services-tároló, győződjön meg arról, hogy hello legújabb Azure Backup szolgáltatás ügynöke telepítve van.<br>Ha más Azure biztonsági mentés kiszolgálók regisztrált toohello Recovery Services-tároló, várja meg a nap telepítési toostart hello helyreállítási folyamat után. hello ütemezett feladat feltölti az összes védett biztonsági mentések toocloud hello metaadatait. hello adatokat kell rendelkezésre a helyreállításhoz. |
| 4. |megadott hello titkosítási jelszó nem egyezik a következő kiszolgáló hello tartozó jelszóval:**<server name>** |**OK:** hello folyamatban történő hello adatait, amelyik a helyreállítandó hello Azure Backup Server-adatok titkosítására használt hello titkosítási jelszó nem egyezik meg a megadott hello titkosítási jelszó. hello ügynök az nem toodecrypt hello adatai. Ezért hello helyreállítás sikertelen lesz.<br>**Megoldás:** hello pontos azonos titkosítási jelszó hello Azure Backup Server adatai helyreállítás alatt álló társított adja meg. |

## <a name="frequently-asked-questions"></a>Gyakori kérdések

### <a name="why-cant-i-add-an-external-dpm-server-after-installing-ur7-and-latest-azure-backup-agent"></a>Miért nem külső DPM-kiszolgáló hozzáadása UR7 és a legújabb Azure Backup szolgáltatás ügynökének telepítése után?

Hello adatforrásokat, amelyek a DPM-kiszolgálók védett toohello felhő (használatával Update Rollup 7-nél korábbi kumulatív frissítés), meg kell várnia, hello UR7 és a legújabb Azure Backup szolgáltatás ügynöke, toostart telepítése után legalább egy napot **vegye fel a külső DPM-kiszolgáló** . hello egy napos időszak az az hello DPM védelmi csoportok tooAzure szükséges tooupload hello metaadatait. Védelmi csoport metaadatai feltöltődtek hello először egy éjszakai feladattal keresztül.

### <a name="what-is-hello-minimum-version-of-hello-microsoft-azure-recovery-services-agent-needed"></a>Mi az a hello hello Microsoft Azure Recovery Services Agent ügynököt szükséges minimális verzióját?

hello Microsoft Azure Recovery Services Agent ügynököt, vagy az Azure Backup szolgáltatás ügynöke, szükséges tooenable minimális verziója hello Ez a szolgáltatás 2.0.8719.0.  tooview hello ügynök verziója: Nyissa meg a Vezérlőpultot  **>**  összes elemek  **>**  programok és szolgáltatások  **>**  A Microsoft Azure Recovery Services Agent ügynököt. Ha hello verziószáma kisebb, mint 2.0.8719.0, töltse le és telepítse a hello [legújabb Azure Backup szolgáltatás ügynökének](https://go.microsoft.com/fwLink/?LinkID=288905).

![Külső DPM törlése](./media/backup-azure-alternate-dpm-server/external-dpm-azurebackupagentversion.png)

## <a name="next-steps"></a>Következő lépések:
• [Azure biztonsági mentési – gyakori kérdések](backup-azure-backup-faq.md)
