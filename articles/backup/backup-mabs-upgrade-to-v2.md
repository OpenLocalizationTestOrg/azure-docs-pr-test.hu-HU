---
title: Azure Backup Server v2 aaaInstall |} Microsoft Docs
description: "Az Azure Backup Server v2 lehetővé teszi a virtuális gép, fájlok és mappák, munkaterhelések és több védelméhez továbbfejlesztett biztonsági mentési lehetőségeket. Megtudhatja, hogyan tooinstall vagy frissítési tooAzure Backup Server v2."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: masaran;markgal
ms.openlocfilehash: 5b1699dadd3a173f1c0ef91a1a600bc5e12f20ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-azure-backup-server-v2"></a>Azure biztonságimásolat-kiszolgáló v2 telepítése

Az Azure Backup Server a virtuális gépek (VM) munkaterhelések, fájlok és mappák és több védi. Az Azure Backup Server v2 Azure Backup Server v1 épül, és lehetővé teszi az új szolgáltatások nem érhetők el az 1-es verzió. A szolgáltatások közötti v1 és v2, lásd: [Azure Backup Server védelmi mátrix](backup-mabs-protection-matrix.md). 

hello további szolgáltatásokat, a biztonsági mentés kiszolgáló v2 a biztonsági mentés kiszolgáló v1 frissítés. Biztonsági mentés kiszolgáló v1 viszont nem a biztonsági mentés kiszolgáló v2 telepítésének előfeltétele. Ha azt szeretné, hogy a biztonsági mentés kiszolgáló v1 tooBackup kiszolgáló v2 tooupgrade, telepítse a biztonsági mentés kiszolgáló v2 hello Backup Server védelmi kiszolgálón. A meglévő biztonsági mentés beállításait változatlanok maradnak.

Biztonsági mentés kiszolgáló v2 telepíthető Windows Server 2012 R2 vagy Windows Server 2016. új szolgáltatásainak tootake előnyeit, például a System Center 2016 adatok Protection Manager Modern Backup-tárhelyre, a biztonsági mentés kiszolgáló v2 telepítenie kell a Windows Server 2016. Biztonsági mentés kiszolgáló v2 tooor telepítés frissítése előtt olvassa el hello [telepítésének előfeltételei](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).

> [!NOTE]
> Az Azure Backup Server rendelkezik hello alap, a System Center Data Protection Manager ugyanazt a kódot. Tartalék kiszolgáló v1 egyenértékű tooData Protection Manager 2012 R2-ben, és biztonsági mentés kiszolgáló v2 egyenértékű tooData Protection Manager 2016. Ez a cikk alkalmanként hello Data Protection Manager dokumentációs hivatkozik.
>
>

## <a name="upgrade-backup-server-toov2"></a>Tartalék kiszolgáló toov2 frissítése
a biztonsági mentés kiszolgáló v1 tooBackup kiszolgáló v2 tooupgrade ellenőrizze, hogy a telepítés rendelkezik hello szükséges frissítések:

- [Hello védelmi ügynökök frissítésének](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) hello a védett kiszolgálók.
- Frissítse a Windows Server 2012 R2 tooWindows Server 2016.
- Azure biztonsági mentés kiszolgáló távoli felügyeleti frissítse az összes üzemi kiszolgálón.
- Győződjön meg arról, hogy a biztonsági mentések megfelelőek-e a toocontinue az üzemi kiszolgáló újraindítása nélkül.


### <a name="upgrade-steps-for-backup-server-v2"></a>Biztonsági mentés kiszolgáló 2-es verzió frissítési lépések

1. A letöltőközpontból, hello [hello frissítési telepítő letöltési](https://go.microsoft.com/fwlink/?LinkId=626082).

2. Miután kibontása hello beállítása varázsló, győződjön meg arról, hogy **hajtható végre a setup.exe** kiválasztva, majd jelölje ki **Befejezés**.

  ![A telepítő installer - futtassa a telepítő](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. A Microsoft Azure Backup Server varázslóban hello alatt **telepítése**, jelölje be **Microsoft Azure Backup Server**.

  ![A telepítő installer - válassza telepítése](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. A hello **üdvözlő** lapon olvassa el hello figyelmeztetéseket, és válassza ki **következő**.

  ![A telepítő installer - kezdőlap](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. hello beállítása varázslót, hogy a környezet verzióra frissítés előfeltétel-ellenőrzések toomake hajt végre. A hello **előfeltételek ellenőrzésének** lapon jelölje be **ellenőrizze**.

  ![A telepítő telepítő – Előfeltételek ellenőrzése lap](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. A környezet hello előfeltétel-ellenőrzések meg kell felelnie. Ha a környezet nem feleltek meg a hello során, hello szempontokat kell figyelembe vennie, és hárítsa el őket. Ezt követően válassza **ellenőrizze újra**. Amikor hello előfeltétel-ellenőrzéseket, válassza ki a **következő**.

  ![A telepítő installer - ellenőrizze újra gomb](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. A hello **SQL-beállítások** lapon, az SQL-telepítés hello a kívánt beállítást, majd válassza ki és **ellenőrzés és telepítés**.

  ![A telepítő installer - SQL-beállítások lap](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  hello ellenőrzések néhány percig is eltarthat. Ha hello ellenőrzése kész, jelölje be-e **következő**.

  ![A telepítő installer - SQL ellenőrizze és telepítése gombra](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and fix-settings.png)

8. A hello **telepítési beállítások** lapján bármilyen módosítások toohello a helyet, ahol a biztonsági mentés Server telepítve van, vagy toohello ideiglenes helyet. Válassza ki **következő**.

  ![A telepítő installer - telepítési beállítások lapon](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. toofinish hello beállítása varázsló, jelölje be **Befejezés**.

  ![A telepítő telepítő - a befejezési](./media/backup-mabs-upgrade-to-v2/run-setup.png)



## <a name="add-storage-for-modern-backup-storage"></a>Tároló hozzáadása a Modern Backup-tárhelyre

tooimprove biztonsági mentési tároló-hatékonyságot biztosít, v2 helykiszolgáló biztonsági mentése a kötetek támogatást. Biztonsági mentés kiszolgáló v1, például a biztonsági mentés kiszolgáló v2 lemezeit támogatja.

### <a name="add-volumes-and-disks"></a>Kötetek és a lemezek hozzáadása
Ha Windows Server 2016 tartalék kiszolgáló v2 futtatja, használhatja kötetek toostore biztonsági mentési adatokat. Kötet nyújtja a tárhely-megtakarítást és gyorsabb biztonsági mentéseket. Mivel kötetek új tooBackup kiszolgálóhoz, hozzá kell adnia őket. 

Egy kötet tooBackup kiszolgáló hozzáadásakor hello kötet egy rövid nevet adhat. Kattintson a hello **rövid név** oszlop hello kötet kívánt tooname. Később bármikor módosíthatja hello nevét, ha szükséges. Is használhatja a PowerShell tooadd vagy módosíthatja a kötetek rövid nevét.

a felügyeleti konzol hello kötet tooadd:

1. Hello Azure Backup Server felügyeleti konzolt, jelölje ki **felügyeleti** > **lemezegységet** > **Hozzáadás**.

    ![Nyissa meg hello lemezegységet hozzáadása varázsló](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

    Ekkor megnyílik a hello lemezegységet hozzáadása varázsló.

2. A hello **adja hozzá a lemezes tárolás** lap hello **rendelkezésre álló köteteken** mezőben, jelöljön ki egy kötetet, majd válassza ki **Hozzáadás**.
3. A hello **kijelölt kötetek** mezőbe, adjon meg egy rövid nevet hello kötet, és válassza ki **OK**.

      ![Adja hozzá a lemezes tárolás varázsló – a kötet hozzáadása](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  Ha azt szeretné, hogy a lemez tooadd, hello lemez örökölt tárolási rendelkező tooa védelmi csoporthoz kell tartoznia. Ezek a lemezek csak a védelmi csoportok használható. Ha a biztonsági kiszolgáló nem kapcsolódik az adatforrásokat, amelyek az örökölt védelmet, hello lemez nem szerepel a listában.

  Lemezek hozzáadásával kapcsolatos további információkért lásd: [hozzáadása a lemezek tooincrease örökölt tárolási](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage). A lemez nem adjon egy rövid nevet.


### <a name="assign-workloads-toovolumes"></a>Rendelje hozzá a munkaterhelések toovolumes

A kiszolgáló biztonsági mentése megadhatja, mely munkaterhelések toowhich kötetek vannak hozzárendelve. Például beállíthat költséges kötetek, amely támogatja a bemeneti/kimeneti műveletek második (IOPS) toostore csak igénylő munkaterhelések gyakori, nagy mennyiségű biztonsági másolatok száma túl magas száma. Példa: SQL Server, a tranzakciós naplók.

#### <a name="update-dpmdiskstorage"></a>Frissítés-DPMDiskStorage

hello tárolókészletben a kiszolgáló biztonsági mentési kötet tooupdate hello tulajdonságainak hello PowerShell parancsmag frissítés-DPMDiskStorage használható.

Szintaxis:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

Az összes végrehajtott módosításokat a PowerShell használatával hello felhasználói felület is megjelennek.


## <a name="protect-data-sources"></a>Adatforrások védelméhez
toobegin védelmet nyújtó adatforrások, hozzon létre egy védelmi csoportot. a következő lépéseket kiemelési módosításait és kiegészítéseit toohello új védelmi csoport varázsló hello.

a védelmi csoport toocreate:

1. Válassza ki a biztonsági mentés Server felügyeleti konzol hello, **védelmi**.

2. Hello eszközsávon kiválasztása **új**.

    Ekkor megnyílik a hello új védelmi csoport létrehozása varázsló.

  ![Új védelmi csoport létrehozása varázsló](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. A hello **üdvözlő** lapon jelölje be **következő**.
4. A hello **védelmi csoport típusának kiválasztása** lapon, válassza ki a kívánt toocreate, és válassza ki a védelmi csoport hello típusú **következő**.

  ![Válassza ki a védelmi csoport típusa lap](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. A hello **csoporttagok kiválasztása** lap hello **választható tagok** ablaktáblában hello tagokat a védelmi ügynökök találhatók. Ehhez a példához válassza ki a kötetet a D:\ és E:\ hozzá toohello **kijelölt tagok** ablaktáblán. Válassza ki **következő**.

  ![Csoport kijelölése tagok lap](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. A hello **adatvédelmi módszer kiválasztása** lapján adjon meg egy **védelmi csoport neve**, hello védelmi módszert, majd válassza ki és **következő**. Ha azt szeretné, hogy a rövid távú védelem, ki kell választania hello **lemez** metódus biztonsági mentését.

  ![Adatvédelmi módszer kiválasztása lap](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. A hello **rövid távú célok megadása** lapon, jelölje be hello részletei **megőrzési időtartam** és **szinkronizálási gyakoriság**. Ezt követően válassza **következő**. Másik lehetőségként toochange hello ütemezés amikor helyreállítási pontok tett, jelölje be a **módosítás**.

  ![Adja meg a rövid távú célok lapja](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. A hello **lemezterület-foglalás tekintse át** lapon ellenőrizze a kiválasztott hello adatforrások adatait, a méretének és hello terület toobe kiosztott értékeket, és hello cél tárolókötetet.

  ![Áttekintés lemezterület-foglalás lap](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  Tároló kötetek hello munkaterhelés kötet foglalási (beállítása a PowerShell használatával) alapulnak, és a rendelkezésre álló tár hello. Hello tárolókötetek hello legördülő menü más kötetek választásával módosíthatja. Ha megváltoztatja hello **célként megadott**, értékét hello **szabad tárhely** tooreflect értékek alapján dinamikusan változik **szabad terület** és **Terület underprovisioned**.

  Hello adatforrások nő, ahogy a tervezett, ha hello hello értéke **Underprovisioned terület** oszlopa **szabad tárhely** további tárhely szükséges mennyiségű hello tükrözi. Ez a tárolási igényei zökkenőmentes biztonsági mentés érték toohelp-csomag használata. Hello értéke nulla, ha nincsenek nem tárolási kapcsolatos esetleges problémák hello közeljövőben. Hello értéke egy számot a nullától eltérő, ha nincs elegendő tárterület lefoglalt (a védelmi házirend- és hello adatok mérete a védett tagok alapján).

  ![Szabad kapacitással rendelkező erőforrásokkal lemezes tárolás](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

   a védelmi csoport, teljes hello varázsló létrehozása toofinish.

## <a name="migrate-legacy-storage-toomodern-backup-storage"></a>Telepítse át a hagyományos tárolási tooModern Backup-tárhelyre
A frissítés befejezése után tooor telepítése Server biztonsági másolat v2 és frissítési hello operációs rendszer tooWindows Server 2016 frissítse a védelmi csoportok toouse Modern Backup-tárhelyre. Alapértelmezés szerint a védelmi csoportok nem változik. Toofunction akkor folytatható, mert kezdetben voltak beállítva. 

Védelmi csoportok toouse Modern Backup-tárhelyre frissítése nem kötelező megadni. tooupdate hello védelmi csoport összes adatforrásának hello használatával állítsa le a védelmet továbbra is az adatokat. Adja hozzá a hello adatok források tooa új védelmi csoportot.

1. A felügyeleti konzol hello, válassza ki a hello **védelmi** szolgáltatás. A hello **védelmi csoport tagjához** listában, majd válassza ki és kattintson a jobb gombbal a hello tag **tag védelmének kikapcsolása**.

  ![Tag védelmének kikapcsolása](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. A hello **távolítsa el a csoportból** párbeszédpanelen tekintse át a hello használt lemezterület és hello rendelkezésre álló szabad területet hello tárolókészlethez. hello alapértelmezett tooleave hello helyreállítási pontok hello lemezen, és lehetővé teszik a kapcsolódó adatmegőrzési / tooexpire. Kattintson az **OK** gombra.

  Ha tooimmediately használt visszatérési hello szabad terület toohello szabad tárolókészlet, jelölje be a hello **lemezen tárolt replika törlése** jelölőnégyzetet toodelete hello biztonsági mentési adatok (és a helyreállítási pontok) társított tag.

  ![Távolítsa el a csoport párbeszédpanel](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Hozzon létre egy védelmi csoportot, amely Modern biztonsági mentési tárolót használ. Védelem nélküli hello adatforrásokat tartalmaz.


## <a name="add-disks-tooincrease-legacy-storage"></a>Lemezek tooincrease örökölt tároló hozzáadása

Ha azt szeretné, hogy toouse örökölt storage Server biztonsági másolat, szükség lehet a tooadd lemezek tooincrease örökölt tároló. 

tooadd lemezterület:

1. Hello felügyeleti konzolt, jelölje ki **felügyeleti** > **lemezegységet** > **Hozzáadás**.

    ![Adja hozzá a lemezes tárolás párbeszédpanel](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. A hello **adja hozzá a lemezes tárolás** párbeszédablakban válassza **vegyen fel lemezeket**.

5. A rendelkezésre álló lemezek hello listában válassza ki a kívánt tooadd, jelölje be a hello lemezek **Hozzáadás**, majd válassza ki **OK**.

## <a name="update-hello-data-protection-manager-protection-agent"></a>Hello Data Protection Manager védelmi ügynök frissítése

Tartalék kiszolgáló hello System Center Data Protection Manager védelmi ügynök frissítések használ. Ha frissíti a védelmi ügynök, amely nem csatlakoztatott toohello hálózat, a Data Protection Manager felügyeleti konzol toocomplete a csatlakoztatott ügynökök frissítési hello nem használhat. Hello védelmi ügynök egy nem aktív tartomány környezetében kell frissítenie. Amíg hello ügyfélszámítógép csatlakoztatott toohello hálózati, hello Data Protection Manager felügyeleti konzol jeleníti meg, hogy hello védelmi ügynök frissítése függőben.

hello alábbi szakaszok azt ismertetik, hogyan tooupdate védelmi ügynököket a csatlakozó ügyfélszámítógépek és a nem csatlakozó ügyfélszámítógépekről.

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a>Frissítse a védelmi ügynököt a csatlakoztatott ügyfélszámítógépen

1. Válassza ki a biztonsági mentés Server felügyeleti konzol hello, **felügyeleti** > **ügynökök**.

2. Hello megjelenítési ablaktáblán jelölje ki a kívánt tooupdate hello védelmi ügynök hello ügyfélszámítógépeket.

  > [!NOTE]
  > Hello **Ügynökfrissítéssel** oszlop azt jelzi, ha a védelmi ügynök frissítése a védett számítógépekhez. A hello **műveletek** ablaktáblában hello **frissítés** művelet áll rendelkezésre, csak ha egy védett számítógép van kiválasztva, és frissítések érhetők el.
  >
  >

3. tooinstall frissíti a védelmi ügynököket a kiválasztott hello számítógépek hello **műveletek** ablaktáblán válassza előbb **frissítés**.

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a>Egy ügyfélszámítógépen, amely nem csatlakozik a védelmi ügynök frissítése

1. Válassza ki a biztonsági mentés Server felügyeleti konzol hello, **felügyeleti** > **ügynökök**.

2. Hello megjelenítési ablaktáblán jelölje ki a kívánt tooupdate hello védelmi ügynök hello ügyfélszámítógépeket.

  > [!NOTE]
   > Hello **Ügynökfrissítéssel** oszlop azt jelzi, ha a védelmi ügynök frissítése a védett számítógépekhez. A hello **műveletek** ablaktáblában hello **frissítés** művelet nem érhető el, ha egy védett számítógép van kiválasztva, kivéve, ha frissítések érhetők el.
  >
  >

3. tooinstall hello kijelölt számítógépekre, válassza ki a védelmi ügynökök frissítése **frissítés**.

4. Az ügyfélszámítógép, amely nem toohello hálózati csatlakoztatva, amíg a hello számítógép csatlakoztatott toohello hálózati, hello **ügynök állapota** az oszlopban látható a állapotának **frissítés függőben**.

  Miután egy ügyfélszámítógép csatlakoztatott toohello hálózati, hello **Ügynökfrissítéssel** hello ügyfélszámítógép az oszlopban látható a állapotának **Frissítéskísérleti**.
  
### <a name="move-legacy-protection-groups-from-old-version-and-sync-hello-new-version-with-azure"></a>Helyezze át a hagyományos védelmi csoportok régi verziója szinkronizálási hello új verziója az Azure-ral

Amennyiben az Azure Backup Server és az operációs rendszer hello is frissítve lett, áll készen tooprotect új adatforrásokból Modern Backup-tárhelyre. Azonban már védett adatforrások továbbra is toobe védett hello régebbi, mint az Azure Backup Server voltak, de minden új védelmi Modern biztonsági mentés a tárhelyet fogja használni.

Következő lépések vannak védelmi tooModern biztonsági mentési tároló örökölt üzemmódú toomigrate erőforrásait.

• Hello új kötet(ek) toohello DPM-tárolókészlet hozzáadása, és rendelje hozzá a rövid nevek és adatok forrás, ha szükséges.
• Minden adatforrás, amely örökölt üzemmódban, hello adatforrások és a "Védett adatok megőrzése" állítsa le a védelmet.  Ez lehetővé teszi a helyreállítás a régi helyreállítási pontok áttelepítés után.

• Hozzon létre egy új PG, és válassza ki, amelyek tárolása a új formátum használatával toobe hello adatforrásokat.
• A DPM rendszer lemásolni egy replika hello korábbi biztonsági mentési tárolóból hello Modern biztonságimásolat-tárolási kötet helyileg történő.
Megjegyzés: Ez fogja látni, a helyreállítás után a művelet feladat • minden új szinkronizálási és helyreállítási pontok majd tárolódnak a Modern Backup-tárhelyre.
• A régi helyreállítási pontok lesz kiürítve kimenő lejár, és végül hello lemezterület felszabadítása.
• Összes hello örökölt kötetek hello régi tárterületet, hello lemez a törölt távolíthatók el az Azure biztonsági mentési és hello rendszer.
• A hello Azure DPMDB biztonsági mentés készítése.

2. lépés:-fontos elemek > hello új kiszolgáló megnevezett azonos hello eredeti Azure Backup server toobe lesz szüksége. Hello hello új Azure biztonságimásolat-kiszolgáló neve nem módosítható, ha szeretné, hogy toouse régi tárolókészlet és a DPMDB tooretain helyreállítási pontok - rendelkeznie kell a dpmdb biztonsági másolat, el kell visszaállítani toobe

1) Leállítás hello eredeti Azure biztonságimásolat-kiszolgálóval, vagy ki hello vezetékes-EK.
2) Alaphelyzetbe állítja a hello számítógépfiókot az active Directoryban.
3) Server 2016 telepítése új gépen, akkor hello gép neve megegyezik a hello eredeti Azure Backup server neve.
4) Csatlakozás tartományhoz hello
5) Telepítse az Azure Backup server V2 (helyezze át a DPM Tárolókészletének lemezeit a régi kiszolgáló és az importálás)
6) 2. rész oldalától végrehajtott DPMDB hello visszaállítása
7) Hello tárolási csatolása hello eredeti tartalék kiszolgáló toohello új kiszolgálóról.
8) A DPMDB hello SQL visszaállítása
9) A rendszergazdai parancssorból az új kiszolgáló cd tooMicrosoft Azure Backup szolgáltatás telepítése a helyét és a bin mappa

Példa az útvonalra: C:\windows\system32 > cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\
tooAzure biztonsági másolat, futtassa a DPMSYNC-SYNC

10) Futtassa a DPMSYNC-SYNC Megjegyzés felvett új lemezek toohello DPM tárolókészlet helyett hello régiek, futtassa a DPMSYNC - Reallocatereplica

## <a name="new-powershell-cmdlets-in-v2"></a>A v2 új PowerShell-parancsmagok

Ha telepíti az Azure Backup Server v2, a két új parancsmagok érhetők el: 
* [Csatlakoztatási-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787159.aspx)
* [Dismount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan tooprepare a kiszolgáló vagy a munkaterhelések védelmének megkezdéséhez:
- [A kiszolgálói biztonsági mentési feladatok előkészítése](backup-azure-microsoft-azure-backup.md)
- [Használja a VMware Server biztonsági másolat Server tooback](backup-azure-backup-server-vmware.md)
- [SQL Server biztonsági másolat Server tooback használata](backup-azure-sql-mabs.md)
- [Modern biztonsági mentési tárhelyet használ a helykiszolgáló biztonsági mentése](backup-mabs-add-storage.md)

