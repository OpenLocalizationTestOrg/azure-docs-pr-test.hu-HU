---
title: "biztonsági mentés Server aaaAzure rendszerállapot védelme, és visszaállítja a toobare nélküli |} Microsoft Docs"
description: "Azure Backup Server tooback használja a rendszer állapotát, és az operációs rendszer nélküli helyreállítás (BMR) védelmet nyújt."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
keywords: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.targetplatform: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: markgal,masaran
ms.openlocfilehash: d34c8bbdc7cc24c905f81ceaf199698c1ee923db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-system-state-and-restore-toobare-metal-with-azure-backup-server"></a>Rendszerállapot biztonsági mentését, és az Azure Backup Server toobare nélküli helyreállítása

Az Azure Backup Server készít biztonsági másolatot a rendszer állapotát, és operációs rendszer nélküli helyreállítás (BMR) védelmet nyújt.

*   **Rendszerállapot biztonsági mentését**: operációs rendszer fájljait, készít biztonsági másolatot, akkor helyreállíthatja a számítógép indításakor, de a rendszerfájlok és hello beállításjegyzék elvesznek. A rendszerállapot biztonsági mentése tartalmazza:
    * Tartományi tag: rendszerindítási fájlokat, a COM + osztályregisztrációs adatbázis, a beállításjegyzék
    * Tartományvezérlő: Windows Server Active Directory (NTDS), a rendszerindító fájlok, a COM + osztályregisztrációs adatbázis, a beállításjegyzék, a system volume (SYSVOL)
    * Fürtözött szolgáltatásokat futtató számítógép: Fürtkiszolgáló metaadatai
    * Tanúsítvány-szolgáltatásokat futtató számítógép: tanúsítvány-adatok
* **Operációs rendszer nélküli biztonsági mentés**: biztonsági mentést készít operációs rendszer fájljait és minden adat a kritikus köteteken (kivéve a felhasználói adatok). BMR biztonsági mentés definícióját, foglalja magában a rendszerállapot biztonsági mentését. Ha a számítógép nem indul el, és hogy toorecover minden védelmet nyújt.

hello a következő táblázat összefoglalja, milyen akkor is biztonsági mentése és helyreállítása. A BMR és rendszerállapot védhető app verzióival kapcsolatos részletes információkért lásd: [biztonsági mentése Azure Backup Server funkciója?](backup-mabs-protection-matrix.md).

|Biztonsági mentés|Probléma|Az Azure Backup Server biztonsági másolat helyreállítása|Rendszerállapot helyreállítása|OPERÁCIÓS RENDSZER NÉLKÜLI HELYREÁLLÍTÁS|
|----------|---------|---------------------------|------------------------------------|-------|
|**Fájladatok**<br /><br />Rendszeres biztonsági mentését<br /><br />BMR vagy rendszerállapot biztonsági mentése|Elveszett adatok|I|N|N|
|**Fájladatok**<br /><br />Az Azure Backup Server az adatok biztonsági mentése fájlba<br /><br />BMR vagy rendszerállapot biztonsági mentése|Elveszett vagy sérült operációs rendszer|N|I|I|
|**Fájladatok**<br /><br />Az Azure Backup Server az adatok biztonsági mentése fájlba<br /><br />BMR vagy rendszerállapot biztonsági mentése|Elveszett kiszolgáló (az adatkötetek nem sérültek)|N|N|I|
|**Fájladatok**<br /><br />Az Azure Backup Server az adatok biztonsági mentése fájlba<br /><br />BMR vagy rendszerállapot biztonsági mentése|Elveszett kiszolgáló (az adatkötetek elvesztek)|I|Nem|Igen (operációs rendszer nélküli Helyreállítás, biztonsági másolat adatainak normál helyreállítása követi)|
|**A SharePoint-adatok**:<br /><br />Az Azure Backup Server az adatok biztonsági mentése farm<br /><br />BMR vagy rendszerállapot biztonsági mentése|Elveszett hely, listák, listaelemek és dokumentumok|I|N|N|
|**A SharePoint-adatok**:<br /><br />Az Azure Backup Server az adatok biztonsági mentése farm<br /><br />BMR vagy rendszerállapot biztonsági mentése|Elveszett vagy sérült operációs rendszer|N|I|I|
|**A SharePoint-adatok**:<br /><br />Az Azure Backup Server az adatok biztonsági mentése farm<br /><br />BMR vagy rendszerállapot biztonsági mentése|Vészhelyreállítás|N|N|N|
|Windows Server 2012 R2 Hyper-V<br /><br />Azure Backup Server biztonsági mentés Hyper-V gazdagép vagy Vendég<br /><br />BMR vagy rendszerállapot biztonsági mentését gazdagép|Elveszett VM|I|N|N|
|Hyper-V<br /><br />Azure Backup Server biztonsági mentés Hyper-V gazdagép vagy Vendég<br /><br />BMR vagy rendszerállapot biztonsági mentését gazdagép|Elveszett vagy sérült operációs rendszer|N|I|I|
|Hyper-V<br /><br />Azure Backup Server biztonsági mentés Hyper-V gazdagép vagy Vendég<br /><br />BMR vagy rendszerállapot biztonsági mentését gazdagép|Elveszett Hyper-V állomás (a VM nem sérültek)|N|N|I|
|Hyper-V<br /><br />Azure Backup Server biztonsági mentés Hyper-V gazdagép vagy Vendég<br /><br />BMR vagy rendszerállapot biztonsági mentését gazdagép|Elveszett Hyper-V állomás (virtuális gépek elveszett)|N|N|I<br /><br />Operációs rendszer nélküli Helyreállítás, Azure Backup Server normál helyreállítása követi|
|SQL Server és az Exchange<br /><br />Az Azure Backup Server alkalmazás biztonsági mentése<br /><br />BMR vagy rendszerállapot biztonsági mentése|Az alkalmazásadatok elveszett|I|N|N|
|SQL Server és az Exchange<br /><br />Az Azure Backup Server alkalmazás biztonsági mentése<br /><br />BMR vagy rendszerállapot biztonsági mentése|Elveszett vagy sérült operációs rendszer|N|Y|I|
|SQL Server és az Exchange<br /><br />Az Azure Backup Server alkalmazás biztonsági mentése<br /><br />BMR vagy rendszerállapot biztonsági mentése|Elveszett kiszolgáló (adatbázis/tranzakciós naplók nem sérültek)|N|N|I|
|SQL Server és az Exchange<br /><br />Az Azure Backup Server alkalmazás biztonsági mentése<br /><br />BMR vagy rendszerállapot biztonsági mentése|Elveszett kiszolgáló (adatbázis-tranzakciókhoz naplók elveszett)|N|N|I<br /><br />Operációs rendszer nélküli helyreállítás Azure Backup Server normál helyreállítása követi|

## <a name="how-system-state-backup-works"></a>Hogyan működik a rendszerállapot biztonsági mentése

Amikor a rendszerállapot biztonsági mentése fut, tartalék kiszolgáló kommunikál a Windows Server biztonsági másolat toorequest hello kiszolgáló rendszerállapotának biztonsági másolata. Alapértelmezés szerint a biztonsági mentés kiszolgáló és a Windows Server biztonsági másolat hello legnagyobb elérhető szabad területtel rendelkező meghajtóra hello használja. A meghajtó információ hello PSDataSourceConfig.xml fájl kerül. Ez az Windows Server biztonsági másolat a biztonsági mentésekhez használó hello meghajtóra.

Testre szabhatja a helykiszolgáló biztonsági mentése a hello rendszerállapot biztonsági mentését használó hello meghajtóra. Hello védett kiszolgálón lépjen a Data Protection Manager\MABS\Datasources tooC:\Program Files\Microsoft. Nyissa meg szerkesztésre hello PSDataSourceConfig.xml fájlt. Változás hello \<FilesToProtect\> hello meghajtó betűjeléhez tartozó értéket. Mentse és zárja be hello fájlt. Ha a védelmi csoport beállítása hello számítógép tooprotect hello állapotát, futtasson konzisztencia-ellenőrzést. Ha riasztás jön létre, jelölje be **a védelmi csoport módosítása** hello riasztást, és majd a teljes hello varázslóban. Ezután futtassa a konzisztencia-ellenőrzést.

Ne feledje, hogy ha hello védelmi kiszolgáló egy fürt része, lehetséges, hogy a fürt meghajtó hello meghajtóként hello legtöbb szabad területtel rendelkező lesz-e kiválasztva. Ha a meghajtó tulajdonosa kapcsolt tooanother csomópont, és a rendszerállapot biztonsági mentése fut, hello meghajtó nem érhető el, és hello biztonsági mentés sikertelen. Ebben az esetben módosítsa a PSDataSourceConfig.xml toopoint tooa helyi meghajtóról.

Ezt követően a Windows Server biztonsági másolat egy hello gyökérmappájában hello visszaállítási mappát a WindowsImageBackup nevű mappát hoz létre. Mivel a Windows Server biztonsági másolat hozza létre a hello biztonsági mentés, minden hello adat ebben a mappában kerül. Hello biztonsági mentés végeztével hello fájl átvitt toohello tartalék kiszolgáló számítógép. Vegye figyelembe a következő információ hello:

* Ez a mappa és annak tartalma nem törlődnek hello biztonsági mentési vagy átviteli befejezésekor. hello legjobb módja toothink erre, hogy hello terület a rendszer fenntartja hello legközelebb a biztonsági mentés befejeződött.
* hello mappában jön létre minden alkalommal, amikor a biztonsági másolat legyen. hello dátumával és időpontjával stamp tükrözze hello idején az utolsó rendszerállapot biztonsági mentését.

## <a name="bmr-backup"></a>BMR biztonsági mentéssel

Operációs rendszer nélküli Helyreállítás (beleértve a rendszerállapot biztonsági mentése), a biztonsági mentési feladat hello menti közvetlenül tooa megosztás hello tartalék kiszolgáló számítógép. Tooa mappa mentése nem történik hello védett kiszolgálón.

Tartalék kiszolgáló meghívja a Windows Server biztonsági másolat, és közösen használja az adott BMR biztonsági mentés hello replikakötet ki. Ebben az esetben azt nem adja meg a Windows Server biztonsági másolat toouse hello meghajtó hello legtöbb szabad területtel rendelkező. Ehelyett létrehozott hello megosztást használ hello feladat.

Hello biztonsági mentés végeztével hello fájl átvitt toohello tartalék kiszolgáló számítógép. Naplók C:\Windows\Logs\WindowsServerBackup vannak tárolva.

## <a name="prerequisites-and-limitations"></a>Előfeltételek és korlátozások

-   A BMR nem támogatott a Windows Server 2003 rendszerű számítógépeken, vagy egy ügyfél operációs rendszert futtató számítógépekre.

-   Operációs rendszer nélküli Helyreállítás nem védhető és a rendszer állapotát hello azonos számítógépen különböző védelmi csoportokban.

-   A biztonsági mentés számítógépet nem tudja védeni magát az operációs rendszer nélküli Helyreállítás.

-   Rövid távú védelem tootape (lemezről szalagra vagy D2T) a BMR esetében nem támogatott. Hosszú távú tárolási tootape (lemez lemez-az-szalagra vagy D2D2T) támogatott.

-   A BMR-védelemre vált a Windows Server biztonsági másolat hello védett számítógépen telepítenie kell.

-   A BMR-védelemre vált, eltérően rendszerállapot-védelemről, a biztonsági mentés kiszolgáló nem rendelkezik semmilyen lemezterületről hello védett számítógépen. Windows Server biztonsági másolat közvetlenül küld a biztonsági mentések toohello biztonsági mentés számítógépén. hello biztonsági mentési átviteli feladat nem jelenik meg a helykiszolgáló biztonsági mentése hello **feladatok** nézet.

-   Tartalék kiszolgáló 30 GB helyet hello replikaköteten a BMR foglalja le. Hello ezt módosíthatja **lemezfoglalás** lapon hello védelmi csoport módosítása varázslót vagy hello Get-DatasourceDiskAllocation és a Set-DatasourceDiskAllocation PowerShell-parancsmagok használatával. Hello helyreállítási pont kötetén a BMR-védelem igényel körülbelül 6 GB egy 5 napos megőrzési.
    * Megjegyzés: hello replika kötet mérete tooless 15 GB-nál nem csökkenthető.
    * Helykiszolgáló biztonsági mentése nem számítja ki a hello hello BMR-adatforrás méretét. Az összes feltételezi 30 GB. Módosítsa a BMR biztonsági mentések adott környezetben várható méretének hello hello értéket. BMR biztonsági mentés hello méretével nagyjából kiszámítható, hogy a felhasznált lemezterületet hello a kritikus köteteken. Kritikus kötetek = rendszerindító kötet + rendszerkötet + a rendszerállapot-adatok, például az Active Directory tartalmazó kötet.

-   Ha módosítja a rendszerállapot-védelem tooBMR védelemről, az BMR-védelemre hello kevesebb területre van szükség. *helyreállításipont-kötet*. Azonban hello hello köteten felszabaduló hely nem felszabadul. Manuálisan zsugorítását hello kötet méretét a hello **lemezfoglalás módosítása** oldalán hello védelmi csoport módosítása varázslót vagy hello Get-DatasourceDiskAllocation és a Set-DatasourceDiskAllocation PowerShell-parancsmagok használatával.

    Rendszerállapot-védelem tooBMR védelemre vált, ha a BMR-védelemre hello a több helyet igényel *replikakötet*. hello kötet automatikusan ki van bővítve. Ha azt szeretné, hogy toochange hello alapértelmezett helyfoglalásokat, használja a hello Modify-DiskAllocation PowerShell-parancsmagot.

-   A BMR védelmét toosystem rendszerállapot-védelemre vált, ha helyre van szüksége további hello helyreállítási pont kötetén. Helykiszolgáló biztonsági mentése megpróbálja tooautomatically növekedése hello kötet. Nincs elegendő hely a hello tárolókészletben, ha hiba történik.

    Ha a BMR védelmét toosystem rendszerállapot-védelemre vált, hello védett számítógép területre van szükség. Ennek az az oka a rendszerállapot-védelemre hello replika toohello helyi számítógép először írja, és toohello biztonsági mentés kiszolgáló számítógép továbbítja.

## <a name="before-you-begin"></a>Előkészületek

1.  **Az Azure Backup Server telepítése**. Győződjön meg arról, hogy a biztonsági mentés kiszolgáló megfelelően van-e telepítve. További információkért lásd:
    * [Azure Backup Server rendszerkövetelményei](http://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites)
    * [Tartalék kiszolgáló védelmi mátrix](backup-mabs-protection-matrix.md)

2.  **Tárolás beállítása**. A lemezen, szalagon, és az Azure hello felhőben tárolhatja a biztonsági mentési adatokat. További információkért lásd: [adatok a tárterület előkészítése](https://docs.microsoft.com/system-center/dpm/plan-long-and-short-term-data-storage).

3.  **Hello védelmi ügynök beállítása**. Hello védelmi ügynök telepíthető hello kívánt számítógép tooback fel. További információkért lásd: [telepítés hello DPM védelmi ügynök](http://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent).

## <a name="back-up-system-state-and-bare-metal"></a>Készítsen biztonsági másolatot a rendszerállapot és az operációs rendszer nélküli
A védelmi csoport beállítása [telepíteni a védelmi csoportok](http://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups). Vegye figyelembe, hogy nem láthatók el védelemmel a BMR és a rendszer állapotát hello azonos számítógépre a különböző csoporthoz. Emellett operációs rendszer nélküli Helyreállítás kiválasztásakor rendszerállapot automatikusan engedélyezve van.


1.  tooopen hello új védelmi csoport létrehozása varázsló hello Backup Server felügyeleti konzol, válassza ki a **védelmi** > **műveletek** > **védelmi létrehozása Csoport**.

2.  A hello **védelmi csoport típusának kiválasztása** lapon, hogy melyik **kiszolgálók**, majd válassza ki **következő**.

3.  A hello **csoporttagok kiválasztása** lapon bontsa ki a hello számítógép, és válassza ki vagy **BMR** vagy **rendszerállapot**.

    Ne feledje, hogy nem láthatók el védelemmel hello BMR és a rendszer állapotát a különböző csoporthoz ugyanazon a számítógépen. Emellett operációs rendszer nélküli Helyreállítás kiválasztásakor rendszerállapot automatikusan engedélyezve van. További információkért lásd: [telepíteni a védelmi csoportok](http://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups).

4.  A hello **adatvédelmi módszer kiválasztása** lapon, válassza ki, hogy toohandle rövid és hosszú távú biztonsági mentés. Rövid távú biztonsági mentés mindig toodisk először, hello emellett szeretne biztonsági másolatot készíteni hello lemez toohello Azure felhőalapú Azure biztonsági mentési (rövid távú vagy hosszú távú) használatával. Egy alternatív toolong távú biztonsági mentési toohello felhőben tooset hosszú távú biztonsági mentési tooa önálló eszköz vagy a szalaggal szalagtárhoz társított csatlakozott tooBackup kiszolgáló fel.

5.  A hello **rövid távú célok kiválasztása** lapon, válassza ki, hogy tooback tooshort távú tárolás lemezen fel:
    1. A **megőrzési időtartam**, válassza ki, hogy mennyi ideig tookeep hello a lemezen lévő adatokat. 
    2. A **szinkronizálási gyakoriság**, válassza ki, milyen gyakran toorun egy növekményes biztonsági mentési toodisk. Ha nem szeretné, egy biztonsági mentési időszakban tooset, ellenőrizheti a hello **csak helyreállítási pont létrehozása előtt** lehetőséget. Helykiszolgáló biztonsági mentése elindul egy kifejezett, teljes biztonsági mentést, egyes helyreállítási pontok ütemezése előtt.

6.  Ha a szalagon lévő adatok toostore kívánt hosszú távú tárolás, a hello **rövid távú célok megadása** lapon, válassza ki, hogy mennyi ideig tookeep szalagon tárolt adatokat (1 – 99 év). 
    1. A **a biztonsági mentés gyakorisága**, válassza ki milyen gyakran fusson a biztonsági mentési tootape. hello gyakoriság hello megőrzési tartomány választott alapul:
        * Ha hello megőrzési tartomány 1 – 99 év, napi, heti, Kétheti, havi, negyedéves, féléves vagy éves biztonsági mentést toooccur választhatja meg.
        * Ha hello megőrzési tartomány 1 – 11 hónap, kiválaszthatja a napi, heti, Kétheti vagy havi biztonsági mentés toooccur.
        * Ha hello megőrzési tartomány 1 – 4 hét, napi vagy heti biztonsági mentések toooccur választhatja meg.

    2. A hello **szalag és szalagtár részleteinek kiválasztása** lapra, jelölje be hello szalag és szalagtár toouse, és hogy adatokat kell tömöríteni és titkosított.

7.  A hello **tekintse át a lemezfoglalás** lapján tekintse át a hello tárolókészletben lemezterületet lefoglalt hello védelmi csoportra vonatkozóan.

    1. **Teljes méret adatok** hello tooback akarja hello adatok mérete.
    2. **Szabad terület toobe Azure Backup-kiszolgálón regisztráltak** hello lemezterület a biztonsági mentés Server figyelmeztetéssel hello védelmi csoportra vonatkozóan. Helykiszolgáló biztonsági mentése úgy dönt, hogy épp ezért tökéletes választás a biztonsági mentési kötet hello hello beállításai alapján. Azonban módosíthatja a biztonsági mentési kötet lehetőségei hello **foglalás részletei lemez**. 
    3. Olyan munkaterhelések esetén hello legördülő menüben válasszon ki előnyben részesített hello tároló. A módosítások hello értékeinek módosítása **tárhelyet** és **szabad tárhely** a hello **rendelkezésre álló lemezterület** ablaktáblán. Underprovisioned-e hely, hogy a biztonsági mentés Server javasol toohello kötet, tooensure zökkenőmentes biztonsági mentések hozzáadása tárolási hello mennyiségét.

8.  A hello **replika-létrehozási módszer kiválasztása** lapon, válassza ki, hogy toohandle hello kezdeti teljes adatreplikálás. Ha úgy dönt, tooreplicate hello hálózaton keresztül, ajánlott csúcsidőn kívüli időpontot választani. Nagy mennyiségű adat vagy kisebb, mint optimális hálózati állapotok esetén érdemes lehet adatreplikálás hello offline cserélhető adathordozó használatával.

9. A hello **válasszon konzisztencia-ellenőrzési beállítások** lapon, válassza ki, hogy tooautomate konzisztencia-ellenőrzést. Választható toorun a jelölőnégyzet csak akkor, ha a replikaadatok válik inkonzisztenssé, vagy ütemezés szerint. Ha nem szeretné tooconfigure automatikus konzisztencia-ellenőrzését, bármikor futtathatja egy manuális ellenőrzést. egy manuális ellenőrzést, a hello toorun **védelmi** hello Backup Server felügyeleti konzol területén kattintson a jobb gombbal a hello védelmi csoportot, és adja **konzisztencia-ellenőrzés**.

10. Ha választott mentése toohello felhő tooback hello Azure Backup használatával **Online védelem adatainak megadása** lapon, győződjön meg arról, hogy azt szeretné, hogy másolatot tooAzure tooback hello munkaterhelések választotta.

11. A hello **Online biztonsági mentési ütemezés megadása** lapon, jelölje be a növekményes biztonsági mentések tooAzure történik. Ütemezett biztonsági mentések toorun minden nap, hét, hónap és év, és válassza ki a hello dátumával és időpontjával, amelynél futtatni kell. Biztonsági mentés akkor fordulhat elő, napi tootwice fel. Minden alkalommal, amikor a biztonsági mentés futtatása adatok helyreállítási pont készült Azure-ban hello hello tartalék kiszolgáló lemezen tárolt biztonsági mentési adatok hello példányát.

12. A hello **Online adatmegőrzési szabály megadása** lapon, válassza ki, hogyan hello helyreállítási pontok hello napi, heti, havi vagy éves biztonsági mentést készített megmaradnak az Azure-ban.

13. A hello **Online replikációs válasszon** lapon, válassza ki, hogyan történik a hello teljes adatok kezdeti replikálása. Hello hálózaton keresztül replikálja, vagy hajtsa végre a kapcsolat nélküli (offline összehangolása) biztonsági mentés. Offline biztonsági másolat hello Azure Import szolgáltatással használja. További információkért lásd: [az Azure Backup Offline biztonsági másolat munkafolyamat](backup-azure-backup-import-export.md).

14. A hello **összegzés** lapján tekintse át a beállításokat. Miután kiválasztotta a **csoport létrehozása**, akkor fordul elő, hello adatok kezdeti replikálása. Ha adatreplikáció befejezése, hello **állapot** lapon hello védelmi csoport állapota **OK**. Biztonsági mentés majd történik / hello védelmi csoport beállításait.

## <a name="recover-system-state-or-bmr"></a>Rendszerállapotra vagy operációs rendszer nélküli helyreállítás
BMR vagy állapot tooa hálózati helyének állíthatja helyre. Ha készített biztonsági másolatot az operációs rendszer nélküli Helyreállítás, a Windows helyreállítási környezet (WinRE) toostart használni a rendszert, és csatlakoztassa toohello hálózati. Ezután használja a Windows Server biztonsági másolat toorecover hello hálózati helyről. Ha készített biztonsági másolatot a rendszer állapotát, használja a Windows Server biztonsági másolat toorecover hello hálózati helyről.

### <a name="restore-bmr"></a>BMR-visszaállítás
Futtassa a helyreállítási kiszolgálón hello biztonsági mentés:

1.  A hello **helyreállítási** ablaktáblában található hello számítógép toorecover szeretne, és válassza **operációs rendszer nélküli helyreállítás**.

2.  Rendelkezésre álló helyreállítási pontok félkövérrel szedve jelennek meg hello naptárban. Hello dátum és idő, amelyet az toouse hello helyreállítási pont kiválasztása.

3.  A hello **helyreállítási típus kiválasztása** lapon, hogy melyik **tooa hálózati mappa másolása.**

4.  A hello **célhely megadása** lapon jelölje be, ahol szeretné toocopy hello adatokat. Ne feledje, hogy hello kijelölt helyre kell toohave elegendő hely. Azt javasoljuk, hogy hozzon létre egy új mappát.

5.  A hello **helyreállítási beállítások megadása** lapra, jelölje be hello biztonsági beállítások tooapply. Ezután válassza ki, hogy toouse tárolóhálózat (SAN)-alapú hardver-pillanatfelvételeket a gyorsabb helyreállítás érdekében. (Ez a lehetőség érhető csak ha a SAN, ez a funkció érhető el, és képes toocreate hello és a Klónozás toomake vágási az írható. In Addition, hello védett számítógép és a biztonsági mentés kiszolgáló számítógép lehet csatlakoztatott toohello ugyanazon a hálózaton.)

6.  Értesítési beállítások megadása. A hello **megerősítő** lapon jelölje be **helyreállítása**.

Állítsa be hello megosztás helye:

1.  Hello visszaállítási helyre nyissa meg toohello mappa, amely hello biztonsági mentés.

2.  Ossza meg, amely WindowsImageBackup egy szinttel, hogy hello hello megosztott mappa gyökeréhez hello WindowsImageBackup mappa hello mappát. Ha nem így tesz, a visszaállítás nem találja hello biztonsági mentés. tooconnect Windows helyreállítási környezet (WinRE) segítségével, meg kell egy megosztásra, így hozzáférhet a WinRE hello megfelelő IP-címét és hitelesítő adatokat.

Hello rendszer visszaállítása:

1.  Kezdő hello számítógép, amelyen toorestore hello kép hello rendszer hello Windows DVD használatával állítja vissza.

2.  A hello első lapon ellenőrizze a nyelvi és területi beállításokat. A hello **telepítése** lapon jelölje be **javítsa ki a számítógép**.

3.  A hello **rendszer-helyreállítási beállítások** lapon jelölje be **visszaállíthatja a számítógép korábban létrehozott rendszerkép használatával**.

4.  A hello **válassza ki a rendszerkép biztonsági mentése** lapon, hogy melyik **lemezkép kiválasztása** > **speciális** > **keresse meg a rendszer kép hello hálózaton**. Ha megjelenik egy figyelmeztetés, válassza ki a **Igen**. Nyissa meg toohello megosztás elérési útja, hello hitelesítő adatait adja meg, majd válassza ki hello helyreállítási pontot. Ez a művelet megkeresi az adott biztonsági mentések helyreállítási pontban elérhető. Válassza ki a megjeleníteni kívánt toouse hello helyreállítási pontot.

5.  A hello **válassza ki, hogyan toorestore hello biztonsági mentési** lapon jelölje be **lemezek formázása és újraparticionálása**. Hello következő lapon ellenőrizze a beállításokat. 

6.  toobegin hello visszaállítási, jelölje be **Befejezés**. Újraindításra szükség.

### <a name="restore-system-state"></a>Rendszerállapot visszaállítása

Helyreállítás futtatása a biztonsági kiszolgálón:

1.  A hello **helyreállítási** ablaktáblában található hello számítógép toorecover szeretne, és válassza **operációs rendszer nélküli helyreállítás**.

2.  Rendelkezésre álló helyreállítási pontok félkövérrel szedve jelennek meg hello naptárban. Hello dátum és idő, amelyet az toouse hello helyreállítási pont kiválasztása.

3.  A hello **helyreállítási típus kiválasztása** lapon, hogy melyik **tooa hálózati mappa másolása**.

4.  A hello **célhely megadása** lapon jelölje be ahová toocopy hello adatokat. Ne feledje, hogy hello kijelölt helyre kell elegendő hely. Azt javasoljuk, hogy hozzon létre egy új mappát.

5.  A hello **helyreállítási beállítások megadása** lapra, jelölje be hello biztonsági beállítások tooapply. Ezután válassza ki, hogy toouse SAN-alapú hardveres pillanatfelvételeket a gyorsabb helyreállítás érdekében. (Ez a lehetőség érhető csak akkor, ha rendelkezik ezzel a funkcióval TÁROLÓHÁLÓZATTAL, és képes toocreate hello és az egy klónozott toomake osztani az írható. In Addition, hello védett számítógép és a biztonsági mentés Server-kiszolgálónak kell lennie a csatlakoztatott toohello ugyanazon a hálózaton.)

6.  Értesítési beállítások megadása. A hello **megerősítő** lapon jelölje be **helyreállítása**.

Futtassa a Windows Server biztonsági másolat:

1.  Válassza ki **műveletek** > **helyreállítása** > **ehhez a kiszolgálóhoz** > **következő**.

2.  Válassza ki **egy másik kiszolgáló**, jelölje be hello **tárhely típusának megadása** lapon, majd válassza ki **távoli megosztott mappa**. Adja meg a hello elérési toohello tartalmazó mappa hello helyreállítási pontot.

3.  A hello **helyreállítási típus kiválasztása** lapon, hogy melyik **rendszerállapot**. 

4. A hello **helyének megadása a rendszerállapot-helyreállítás** lapon, hogy melyik **eredeti helyére**.

5.  A hello **megerősítő** lapon jelölje be **helyreállítása**. Hello visszaállítás után indítsa újra a hello kiszolgálót.

6.  Is futtathatja hello rendszerállapot-visszaállítást a parancssorba. toodo, a Windows Server biztonsági másolat start hello számítógépen toorecover szeretné. tooget hello verzió azonosítóját, a parancssorba írja be:```wbadmin get versions -backuptarget \<servername\sharename\>```

    Használja a hello verzió azonosítója toostart hello rendszerállapot-visszaállítást. Hello parancssorba írja be:```wbadmin start systemstaterecovery -version:<versionidentified> -backuptarget:<servername\sharename>```

    Győződjön meg arról, hogy szeretné-e toostart hello helyreállítási. Hello folyamat hello parancssori ablakban látható. Egy visszaállítási napló jön létre. Hello visszaállítás után indítsa újra a hello kiszolgálót.

