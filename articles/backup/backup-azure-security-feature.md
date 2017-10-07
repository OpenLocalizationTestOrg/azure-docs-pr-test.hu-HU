---
title: "aaaSecurity szolgáltatások toohelp védeni az Azure Backup hibrid biztonsági mentések |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse biztonsági Azure biztonsági mentés toomake biztonsági mentések biztonságosabb szolgáltatásai"
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 47bc8423-0a08-4191-826d-3f52de0b4cb8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: pajosh
ms.openlocfilehash: 17a0f5e877f84af53c15062ec4a8df480383125e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-features-toohelp-protect-hybrid-backups-that-use-azure-backup"></a>Biztonsági funkciók toohelp védeni az Azure Backup hibrid biztonsági mentések
-Maillel kapcsolatos biztonsági problémák, például a kártevő szoftver, a nevű és a behatolás, növekednek. Lehet, hogy a biztonsági problémák költséges, pénzt és adatokat. ilyen támadások, az Azure biztonsági mentés most tooguard biztosít a biztonsági funkciók toohelp védelme hibrid biztonsági mentéseket. Ez a cikk ismerteti, hogyan tooenable és -felhasználási ezek szolgáltatásokat, az Azure Recovery Services Agent ügynök és az Azure Backup Server használatával. Ezek a funkciók a következők:

- **Megelőzési**. Egy további hitelesítési réteg kerül, amikor csak a kritikus például a jelszó módosítása a művelet. Ezen ellenőrzés, amely az ilyen műveleteket lehet tooensure csak érvényes Azure hitelesítő adataival rendelkező felhasználó végez.
- **Riasztási**. E-mailben értesítést küldött toohello előfizetés rendszergazdája, amikor egy kritikus művelet, például törlés, biztonsági mentési adatok történik. Az e-mailt biztosítja, hogy a felhasználó hello gyorsan tájékoztatva ilyen műveletek.
- **Helyreállítási**. Törölt biztonsági mentési adatok kiegészítő 14 napon hello hello törlési megőrzi. Ez biztosítja hello adatok helyreállíthatósága belül egy adott időszakban, így nincs adatvesztés nélküli, még akkor is, ha a támadás akkor fordul elő. Emellett minimális helyreállítási pontok nagyobb számú karbantartása tooguard sérült adatok alapján.

> [!NOTE]
> Biztonsági funkciókat is engedélyeznie kell használata infrastruktúra (IaaS) szolgáltatás VM biztonsági mentéséhez. Ezeket a szolgáltatásokat még nem állnak rendelkezésre az infrastruktúra-szolgáltatási virtuális gép biztonsági mentése, így azokat nem semmilyen hatással lesz. Biztonsági funkciók csak a rendszer használata esetén engedélyezni kell: <br/>
>  * **Az Azure Backup szolgáltatás ügynökének**. Minimális ügynökverzió 2.0.9052. Miután engedélyezte a ezeket a funkciókat, frissítenie kell a toothis ügynök verziója tooperform végrehajtott kulcsfontosságú műveleteket. <br/>
>  * **Az Azure Backup Server**. Azure Backup agent legalább 2.0.9052 Azure Backup Server 1. frissítés. <br/>
>  * **A System Center Data Protection Manager**. Azure Backup agent legalább 2.0.9052 a Data Protection Manager 2012 R2 UR12 vagy a Data Protection Manager 2016 UR2. <br/> 


> [!NOTE]
> Ezek a szolgáltatások csak Recovery Services-tároló érhetők el. Recovery Services-tárolók, újonnan létrehozott összes hello ezek a szolgáltatások alapértelmezés szerint engedélyezve van. Meglévő Recovery Services-tárolók a felhasználók ezeket a funkciókat engedélyezni hello a következő szakaszban említett hello lépések segítségével. Hello szolgáltatások engedélyezve vannak, után alkalmazza őket tooall hello Recovery Services agent számítógépek, Azure Backup Server-példányok és hello tárolóhoz rendelt Data Protection Manager-kiszolgálók. A beállítás engedélyezése egyszeri művelet, és nem tiltható le ezeket a funkciókat Miután engedélyezte őket.
>

## <a name="enable-security-features"></a>Biztonsági szolgáltatások engedélyezése
Recovery Services-tároló létrehozásakor, minden hello funkciókat is használhatja. Ha egy meglévő tárolóhoz dolgozik, biztonsági szolgáltatások engedélyezése a következő lépések végrehajtásával:

1. Jelentkezzen be toohello Azure portál Azure hitelesítő adataival.
2. Válassza ki **Tallózás**, és írja be **Recovery Services**.

    ![Képernyőfelvétel az Azure portál tallózási lehetőséget](./media/backup-azure-security-feature/browse-to-rs-vaults.png) <br/>

    helyreállítási szolgáltatások tárolók hello listája jelenik meg. Ebben a listában jelölje ki a tárolóban. Megnyitja a kijelölt tároló irányítópult hello.
3. A hello elemekből álló listák hello tárolóban, megjelenik a **beállítások**, kattintson a **tulajdonságok**.

    ![Képernyőfelvétel a Recovery Services-tároló beállítások](./media/backup-azure-security-feature/vault-list-properties.png)
4. A **biztonsági beállítások**, kattintson a **frissítés**.

    ![Képernyőkép a Recovery Services-tároló tulajdonságok](./media/backup-azure-security-feature/security-settings-update.png)

    hello frissítés a hivatkozás megnyitja hello **biztonsági beállítások** panel, amelyen a hello szolgáltatások összegzését tartalmazza, és lehetővé teheti, hogy azokat.
5. Hello legördülő listából **állított Azure multi-factor Authentication?**, jelölje be egy értéket tooconfirm, ha engedélyezte a [Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md). Ha engedélyezve van, az Azure-portálon toohello aláírása során egy másik eszközről (például egy mobiltelefon) tooauthenticate kell adnia.

   Ha a biztonsági mentés végrehajtott kulcsfontosságú műveleteket hajt végre, lehetősége van tooenter PIN-kód és hello Azure-portálon elérhető biztonsági. Azure multi-factor Authentication engedélyezése egy biztonsági réteget ad. Csak a feljogosított felhasználók érvényes Azure hitelesítő adatokkal, és egy második eszközről hitelesített, hello Azure-portálon érhetik el.
6. toosave biztonsági beállítást, **engedélyezése** kattintson **mentése**. Kiválaszthatja **engedélyezése** csak után kiválaszthat egy értéket a hello **állított Azure multi-factor Authentication?** lista hello előző lépésben.

    ![Képernyőkép a biztonsági beállítások](./media/backup-azure-security-feature/enable-security-settings-dpm-update.png)

## <a name="recover-deleted-backup-data"></a>A törölt biztonsági mentési adatok helyreállítása
Biztonsági mentés további 14 napig őrzi meg törölt biztonsági mentési adatokat, és nem töröl, ha azonnal hello **Stop biztonsági mentés, a biztonsági mentési adatok** művelet. toorestore ezeket az adatokat hello 14 napos időszak alatt, tegye meg hello lépések attól függően, hogy mi használ:

A **Azure Recovery Services Agent ügynököt** felhasználók:

1. Ha hello számítógépen, ahol a biztonsági mentések folyamatban volt továbbra is elérhető, akkor [adatok toohello helyreállítása ugyanaz a gép](backup-azure-restore-windows-server.md#use-instant-restore-to-recover-data-to-the-same-machine) az Azure Recovery Services toorecover összes hello régi helyreállítási pontokból.
2. Ez a számítógép nem érhető el, ha [helyreállítás tooan alternatív gép](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) toouse egy másik Azure Recovery Services számítógép tooget ezeket az adatokat.

A **Azure Backup Server** felhasználók:

1. Ha hello kiszolgáló, ahol a biztonsági mentések folyamatban volt továbbra is elérhető, törölt hello adatforrások védelmének újbóli beállítását, és hello használata **adatok helyreállítása** funkció toorecover összes hello régi helyreállítási pontokból.
2. Ez a kiszolgáló nem érhető el, ha [állíthat helyre adatokat egy másik Azure Backup Server](backup-azure-alternate-dpm-server.md) toouse egy másik Azure Backup Server-példány tooget ezeket az adatokat.

A **Data Protection Manager** felhasználók:

1. Ha hello kiszolgáló, ahol a biztonsági mentések folyamatban volt továbbra is elérhető, törölt hello adatforrások védelmének újbóli beállítását, és hello használata **adatok helyreállítása** funkció toorecover összes hello régi helyreállítási pontokból.
2. Ez a kiszolgáló nem érhető el, ha [külső DPM hozzáadása](backup-azure-alternate-dpm-server.md) toouse Data Protection Manager-kiszolgáló egy másik tooget ezeket az adatokat.

## <a name="prevent-attacks"></a>Támadások megelőzése érdekében
Ellenőrzi, hogy csak akkor érvényes, ha a felhasználók különböző műveleteket hajthat végre toomake váltak elérhetővé. Ezek közé tartozik egy további hitelesítési réteggel hozzáadása és karbantartása minimális megőrzési időtartamot helyreállítási célokra.

### <a name="authentication-tooperform-critical-operations"></a>Hitelesítési tooperform végrehajtott kulcsfontosságú műveleteket
Egy további hitelesítési végrehajtott kulcsfontosságú műveleteket az réteggel való hozzáadásának részeként áll felszólító tooenter PIN-kód biztonsági végrehajtásakor **védelem kikapcsolása az adatok** és **Módosítsa jelszavát** műveletek .

tooreceive a PIN-kód:

1. Bejelentkezés toohello Azure-portálon.
2. Keresse meg a túl**Recovery Services-tároló** > **beállítások** > **tulajdonságok**.
3. A **biztonsági PIN-kód**, kattintson a **Generate**. Ekkor megnyílik egy panel, amely tartalmazza a hello PIN-kód toobe hello Azure Recovery Services agent felhasználói felületen megadott.
    A PIN-kód érvényes csak 5 percig, és automatikusan lekérdezi létrehozott az időszak elteltével.

### <a name="maintain-a-minimum-retention-range"></a>A minimális megőrzési tartomány karbantartása
rendelkezésre álló tooensure, hogy számos mindig érvényes helyreállítási pont, a következő ellenőrzések hello lettek hozzáadva:

- A napi megőrzési, legalább **hét** napnyi megőrzési el kell végezni.
- A heti megőrzési, legalább **négy** hetes megőrzési el kell végezni.
- A havi megőrzési, legalább **három** hónapos megőrzéssel el kell végezni.
- Az éves megőrzési, legalább **egy** adatmegőrzési év el kell végezni.

## <a name="notifications-for-critical-operations"></a>A kritikus műveletek értesítések
Általában kritikus művelet végrehajtásakor hello előfizetés rendszergazdája küldött e-mailben értesítést hello műveletekre vonatkozó adatokkal. Az értesítések címzettjeit további e-mail hello Azure-portál használatával konfigurálható.

a cikkben említett hello funkciókat célzott támadások elleni védelmet mechanizmusok adja meg. Ennél is fontosabb, ha a támadás akkor fordul elő, ezeket a funkciókat nyújtanak hello képességét toorecover adatait.

## <a name="troubleshooting-errors"></a>Kapcsolatos hibák elhárítása
| Művelet | Hiba legutolsó részletes adatai | Megoldás: |
| --- | --- | --- |
| Házirend módosítása |hello biztonsági mentési házirend nem módosítható. Hiba: hello aktuális művelet tooan belső szolgáltatási hiba [0x29834] miatt nem sikerült. Hello művelet némi várakozás után próbálkozzon újra. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft támogatási szolgálatához. |**OK:**<br/>Ez a hiba elérhető lesz, ha engedélyezve vannak a biztonsági beállítások, tooreduce megőrzési időtartam alatt hello fent megadott minimális értékek meg, és nem támogatott verziójú (az első Megjegyzés: Ez a cikk a támogatott verziók vannak megadva). <br/>**Javasolt művelet:**<br/> Ebben az esetben célszerű a megőrzési időtartam hello minimális megőrzési megadott időszak (hét nap négy hétben-naponta, hetente, a havi vagy éves egy év három hétig) feletti tooproceed házirenddel kapcsolatos frissítések. Másik lehetőségként kedvelt módszer tooupdate biztonságimásolat-készítő ügynök, Azure biztonsági mentési kiszolgálón és/vagy a DPM UR tooleverage minden hello biztonsági frissítések. |
| Jelszó módosítása |Biztonsági megadott PIN-kód nem megfelelő. (AZONOSÍTÓ: 100130) Adja meg a hello megfelelő biztonsági PIN-kód toocomplete ezt a műveletet. |**OK:**<br/> Ez a hiba előre érvénytelen vagy lejárt biztonsági PIN-kódot kell megadni (például a jelszó módosítása) kritikus művelet végrehajtása közben. <br/>**Javasolt művelet:**<br/> toocomplete hello művelet, adjon meg érvényes biztonsági PIN-kódot. tooget hello PIN-kód, jelentkezzen be tooAzure portálon, és keresse meg a tooRecovery Services-tároló > Beállítások > Tulajdonságok > biztonsági PIN-kód készítése. A PIN-kód toochange jelszó használata. |
| Jelszó módosítása |A művelet sikertelen volt. AZONOSÍTÓ: 120002 |**OK:**<br/>Ez a hiba a biztonsági beállítások engedélyezve vannak, toochange jelszót és a nem támogatott verzióját (Ez a cikk első megjegyzés-ben megadott érvényes verzió) származik.<br/>**Javasolt művelet:**<br/> toochange jelszót, először frissítenie kell biztonságimásolat-készítő ügynök toominimum verziója minimális 2.0.9052, Azure Backup server toominimum update 1 és/vagy a DPM toominimum DPM 2012 R2 UR12 vagy a DPM 2016 UR2 (Letöltés a lenti hivatkozásokra kattintva), majd adjon meg érvényes biztonsági PIN-kódot. tooget hello PIN-kód, jelentkezzen be tooAzure portálon, és keresse meg a tooRecovery Services-tároló > Beállítások > Tulajdonságok > biztonsági PIN-kód készítése. A PIN-kód toochange jelszó használata. |

## <a name="next-steps"></a>Következő lépések
* [Ismerkedés az Azure Recovery Services-tároló](backup-azure-vms-first-look-arm.md) tooenable ezeket a szolgáltatásokat.
* [Töltse le az Azure Recovery Services agent legújabb hello](http://aka.ms/azurebackup_agent) toohelp Windows rendszerű számítógépek védelmét, és a biztonsági mentési adatok támadások elleni védelmet.
* [Letöltési hello legújabb Azure Backup Server](https://aka.ms/latest_azurebackupserver) toohelp munkaterhelések védelmét, és a biztonsági mentési adatok támadások elleni védelmet.
* [Töltse le a System Center 2012 R2 Data Protection Manager UR12](https://support.microsoft.com/help/3209592/update-rollup-12-for-system-center-2012-r2-data-protection-manager) vagy [töltse le a System Center 2016 Data Protection Manager UR2](https://support.microsoft.com/help/3209593/update-rollup-2-for-system-center-2016-data-protection-manager) toohelp munkaterhelések védelmét, és a biztonsági mentési adatok támadások elleni védelmet.
