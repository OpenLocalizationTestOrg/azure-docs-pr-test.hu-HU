---
title: "aaaReplicate VMware virtuális gépek vagy fizikai kiszolgálók tooanother hely (klasszikus Azure portálon) |} Microsoft Docs"
description: "Ez a cikk tooreplicate VMware virtuális gépek vagy windowsos/Linuxos fizikai kiszolgálók tooa a másodlagos hely használja az Azure Site Recovery szolgáltatással."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: jwhit
editor: 
ms.assetid: b2cba944-d3b4-473c-8d97-9945c7eabf63
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: nisoneji
ms.openlocfilehash: 5789ca07f0aa15cf194615fd33103dac930d7b7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-tooa-secondary-site-in-hello-classic-azure-portal"></a>Helyszíni VMware virtuális gépek vagy fizikai kiszolgálók tooa másodlagos hely a klasszikus Azure portálon hello replikálása

## <a name="overview"></a>Áttekintés
Az Azure Site Recovery InMage Scout biztosít a valós idejű, a helyszíni VMware helyek közötti replikáció. Azure Site Recovery szolgáltatás előfizetések InMage Scout tartalmazza. 

## <a name="prerequisites"></a>Előfeltételek
**Azure-fiók**: szüksége lesz egy [Microsoft Azure](https://azure.microsoft.com/) fiók. Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is. [További információk](https://azure.microsoft.com/pricing/details/site-recovery/) a Site Recovery díjszabásáról.

## <a name="step-1-create-a-vault"></a>1. lépés: A tároló létrehozása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Új kattintson > felügyeleti > biztonsági mentés és helyreállítás (OMS). Másik lehetőségként kattinthat a Tallózás > Recovery Services-tároló > Hozzáadás.
3. A **neve** adjon meg egy rövid nevet tooidentify hello tárolóban. Ha egynél több előfizetéssel rendelkezik, válasszon egyet ezek közül.
4. A **erőforráscsoport** hozzon létre egy új erőforráscsoportot, vagy válasszon egy meglévőt. Adja meg egy Azure-régiót toocomplete kötelező mezőket.
5. A **hely**, válassza ki a hello hello tároló földrajzi régióját. támogatott toocheck régiókban, lásd: [Azure Site Recovery Díjszabásáról](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Ha azt szeretné, tooquickly hozzáférés hello tároló hello irányítópult a PIN-kód toodashboard kattintson, és majd kattintson a Létrehozás gombra.
7. hello új tároló megjelenik hello irányítópult > összes erőforrást, és a hello fő Recovery Services-tárolók panelen.

## <a name="step-2-configure-hello-vault-and-download-inmage-scout-components"></a>2. lépés: Hello tároló konfigurálása és InMage Scout összetevők letöltése
1. Hello Recovery Services-tárolók panelen válassza ki a tároló, és kattintson a beállítások gombra.
2. A **beállítások** > **bevezetés** kattintson **Site Recovery** > 1. lépés: **infrastruktúra előkészítése** > **védelmi cél**.
3. A **védelmi cél** válasszon toorecovery helyet, és válassza az Igen, amelyen a VMware vSphere Hipervizorra. Ezután kattintson az OK gombra.
4. A **Scout telepítő**, kattintson a letöltés toodownload InMage Scout 8.0.1 GA szoftver és a regisztrációs kulcsot. hello telepítőfájljainak összes hello szükséges összetevők: hello letöltött .zip-fájlban.

## <a name="step-3-install-component-updates"></a>3. lépés: Az összetevő-frissítések telepítése
További információ a hello legújabb [frissítések](#updates). Hello frissítési fájlok kiszolgálók sorrend hello lesz telepít:

1. Ha van ilyen a RX kiszolgáló
2. Konfigurációs kiszolgálók
3. Folyamat kiszolgálók
4. Fő célkiszolgálóra
5. vContinuum-kiszolgáló
6. A forráskiszolgálón (a Windows és Linux-kiszolgáló)

Hello frissítések telepítése az alábbiak szerint:

1. Töltse le a hello [frissítése](https://aka.ms/asr-scout-update5) .zip fájl. A .zip fájl tartalmazza a következő fájlok hello:

   * RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.GZ
   * CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
   * UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe
   * UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
   * vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe
   * EE update4 bits RHEL5, OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
2. Bontsa ki a hello .zip fájlt.<br>
3. **A hello RX server**: másolás **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** toohello a RX kiszolgáló, és bontsa ki. Hello a kibontott mappát, futtassa **/telepítése**.<br>
4. **Hello konfigurációs kiszolgáló/folyamat kiszolgáló**: másolás **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** toohello konfigurációs kiszolgáló és a folyamatkiszolgáló. Kattintson duplán a toorun azt.<br>
5. **A Windows-fő célkiszolgáló hello**: tooupdate hello unified agent, a Másolás **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** toohello fő célkiszolgáló. Kattintson rá duplán toorun azt. Vegye figyelembe, hogy a unified agent hello esetén is alkalmazható toohello forráskiszolgáló forrás Update4 vége nem frissül. Telepítenie kell azt forráskiszolgálón hello és, ahogy azt korábban említettük, a listában szereplő később.<br>
6. **Hello vContinuum-kiszolgáló**: másolás **vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe** toohello vContinuum-kiszolgáló.  Győződjön meg arról, hogy bezárt hello vContinuum varázsló. A fájl toorun hello kattintson rá duplán.<br>
7. **A Linux-fő célkiszolgáló hello**: tooupdate hello unified agent, a Másolás **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** toohello fő célkiszolgáló, és bontsa ki. Hello a kibontott mappát, futtassa **/telepítése**.<br>
8. **Hello Windows forráskiszolgálóhoz**: Ha soruce már update4, nem kell forrás tooinstall frissítés 5-ügynököt. Ha kisebb, mint update4, alkalmazza a hello frissítés 5-ügynök.
tooupdate hello unified agent, a Másolás **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** toohello forráskiszolgáló. Kattintson rá duplán toorun azt. <br>
9. **Hello Linux forráskiszolgálóhoz**: tooupdate hello egyesített ügynök, másolja a megfelelő verziója Linux toohello EE fájlkiszolgáló, és bontsa ki. Hello a kibontott mappát, futtassa **/telepítése**.  Példa: RHEL 6,7 64 bites kiszolgálót, másolja át **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** toohello kiszolgáló, és bontsa ki. Hello a kibontott mappát, futtassa **/telepítése**.

## <a name="step-4-set-up-replication"></a>4. lépés: A replikáció beállítása
1. Hello forrás- és VMware közötti replikáció helyek beállítása.
2. Útmutatásért hello termék hello InMage Scout dokumentáció, és letöltődik használata. Másik lehetőségként hello dokumentációjában az alábbiak szerint végezheti el:

   * [Kibocsátási megjegyzések](https://aka.ms/asr-scout-release-notes)
   * [Kompatibilitási mátrix](https://aka.ms/asr-scout-cm)
   * [Felhasználói útmutató](https://aka.ms/asr-scout-user-guide)
   * [Útmutató a RX felhasználó](https://aka.ms/asr-scout-rx-user-guide)
   * [A gyors telepítési útmutató](https://aka.ms/asr-scout-quick-install-guide)

## <a name="updates"></a>Frissítések
### <a name="azure-site-recovery-scout-801-update-5"></a>Az Azure Site Recovery Scout 8.0.1 5. frissítése
5. Scout frissítése az összesítő frissítés. Minden hello megjelent update4 és a következő új hibajavításokat és fejlesztések: 1. frissítés van.
Az ASR Scout update4 tooupdate5 hozzáadott javítások olyan adott tooMaster cél- és vContinuum összetevők. Minden a forrás-kiszolgálók, a fő célkiszolgálón, a konfigurációs kiszolgáló, a Folyamatkiszolgáló és a RX esetén már ASR Scout update4 majd meg kell tooapply frissíteni 5 csak a fő célkiszolgálón. 

**Új eszközplatform-támogatás**
* SUSE Linux Enterprise Server 11 szervizcsomag 4(SP4)

> [!NOTE]
> SLES 11 SP4 64 bites **InMage_UA_8.0.1.0_SLES11-SP4-64_GA_13Apr2017_release.tar.gz** ALAPCSOMAG Scout GA együtt van csomagolva **InMage_Scout_Standard_8.0.1 GA.zip**. Töltse le a Scout GA csomag portálról a [1. lépés](#step-1-create-a-vault).
>

**Hibajavításokat és fejlesztések**

* Windows-fürt támogatása megbízhatóságának növelése
    * Rögzített-bizonyos néhány hello P2V MSCS fürtlemezekhez válik a helyreállítás után RAW
    * Fixed-P2V MSCS fürt helyreállítás toodisk rendelés eltérése miatt sikertelen
    * A művelet sikertelen, és a lemez méretének eltérése lemezek hozzáadása a Fixed-MSCS-fürtökben
    * Fixed-forrás MSCS fürt RDM LUN-ok leképezése készültség-ellenőrzés sikertelen, a méret ellenőrzése
    * Fixed-egyetlen csomópont fürt védelme tooSCSI eltérés probléma miatt nem működik 
    * Fixed-ismételt védelemmel hello P2V Windows fürtkiszolgáló sikertelen lesz, ha a cél fürtlemezek szerepelnek. 
    
* Feladat-visszavétel védelem alatt Ha nincs a kijelölt fő Célkiszolgáló hello azonos ESXi-kiszolgálón, az hello védett forrásgép (során előre védelemmel), akkor vContinuum szerzi be hello helytelen MT helyreállítási feladat-visszavétel során, és ezt követően a helyreállítási művelet sikertelen lesz.

> [!NOTE]
> 
> * P2V fürt javítások fent vannak alkalmazható tooonly adott fizikai MSCS-fürtökben az automatikus rendszer-Helyreállítás Scout update5 frissen védett. tooavail hello fürt javítások hello az már védett P2V az MSCS-fürtökben régebbi frissítésekkel, toofollow hello frissítési lépések hello szakasz 12 ismertetett van szüksége, frissítés a P2V MSCS-fürt tooScout Update5 védett [ASR Scout kiadás Megjegyzések](https://aka.ms/asr-scout-release-notes).
> 
> * Állítsa be a fizikai MSCS-fürt védelmét meglévő céllemezek, csak ha helyreállításkor hello az újbóli védelem, ugyanazokat a lemezek aktívak az egyes csomópontok mint az eredetileg védett hello fürt hello is felhasználhatja. Ha nem, majd manuális lépések amit 12 részben [ASR Scout kibocsátási megjegyzések](https://aka.ms/asr-scout-release-notes) hello cél ügyféloldali lemezek toohello megfelelő datastore elérési toore-használata túl áthelyezni őket, az újbóli védelem alatt. Ha Ön lássa el újból védelemmel az MSCS-fürtökben hello P2V módban frissítési lépések nélkül majd hoz létre az új lemez hello célként megadott ESXi-kiszolgálón. Toomanually delete hello régi lemezei hello datastore van szüksége.
> 
> * Amikor forrás SLES11 vagy bármely service pack kiszolgálóval SLES11 szabályosan újraindítása után, majd egy manuálisan kell megjelölni a hello **legfelső szintű** lemez replikációs pár szinkronizálja újra, akkor nem jelenik meg értesítés CX felhasználói felületén. Ha ezt elmulasztja "jel hello legfelső szintű lemez újraszinkronizálási, az adatok integritását (DI) kapcsolatos problémák jelenhetnek meg.
> 

### <a name="azure-site-recovery-scout-801-update-4"></a>Az Azure Site Recovery Scout 8.0.1 Update 4
Scout Update 4 az összesítő frissítés. Minden hello megjelent update3 és a következő új hibajavításokat és fejlesztések: 1. frissítés van.

**Új eszközplatform-támogatás**

* Már támogatja a vCenter vagy vSphere 6.0, 6.1 és 6.2
* Már támogatja a következő Linux operációs rendszerek
  * Red Hat Enterprise Linux (RHEL) 7.0, 7.1-es és 7.2
  * 7.0, 7.1-es és 7.2 centOS
  * Red Hat Enterprise Linux (RHEL) 6.8
  * CentOS 6.8

> [!NOTE]
> RHEL/CentOS 7 64 bites **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** ALAPCSOMAG Scout GA együtt van csomagolva **InMage_Scout_Standard_8.0.1 GA.zip**. Töltse le a Scout GA csomag portálról a [1. lépés](#step-1-create-a-vault).
>
>

**Hibajavításokat és fejlesztések**

* Továbbfejlesztett leállítási kezelése előtt a következő Linux operációs rendszer és a klónok tooprevent nemkívánatos nteljes problémákat.
  * Red Hat Enterprise Linux (RHEL) 6.x
  * Oracle Linux (OL) 6.x
* Linux a mappa teljes hozzáférési engedélyek szükségesek a unified agent telepítési könyvtára jelenleg korlátozott csak toohello helyi felhasználói.
* A Windows közös elosztott konzisztencia könyvjelző elküldése a fokozottan során probléma csatornainicializálásnak elosztott alkalmazások, például az SQL és megosztási pont fürtök betöltve.
* A hozzáadott napló kapcsolódó javítás CX alap telepítő.
* VMware vCLI 6.0 letöltési hivatkozás tooWindows fő célkiszolgáló alap telepítő kerül.
* További ellenőrzéseket és naplókat a további hálózati konfigurációkat módosítások hozzá feladatátvételi és vész-Helyreállítási csukja során.
* Bizonyos megőrzési információ nincs jelentett toohello CX.  
* Fizikai fürt kötet újra méretezés művelet vContinuum varázsló lépéseinek forrás kötet zsugorítás történt, amikor nem működik.
* Védelem "Lemezaláírását sikertelen toofind hello" hiba miatt sikertelen volt a fürt PRDM lemez fürtlemez esetén.
* cxps server összeomlása átviteli out tartományon kívüli kivétel miatt.
* Kiszolgáló neve és IP-oszlopok is méretezhető vContinuum varázsló leküldéses telepítés lapján.
* Fejlesztések a RX API
  * Öt legújabb elérhető közös konzisztencia pontokat biztosít (csak a garantált címkék).
  * Itt a kapacitás és szabad lemezterület részletei összes hello védett eszközök.
  * Ez a Scout illesztőprogram állapot forráskiszolgáló.

> [!NOTE]
> * **InMage_Scout_Standard_8.0.1_GA.zip** ALAPCSOMAG most már frissítve van CX alap telepítő **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** és a Windows a fő célkiszolgálón alap installer **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Minden új telepítésre használja új CX és a Windows a fő célkiszolgálón GA bits.
> * 4. frissítés közvetlenül alkalmazható 8.0.1 GA
> * hello konfigurációs kiszolgáló és a RX frissítések után hello rendszer alkalmazza azokat a még nem állítható.
>
>

### <a name="azure-site-recovery-scout-801-update-3"></a>Az Azure Site Recovery Scout 8.0.1 Update 3
3 frissítésekor a hello következő hibajavításokat és fejlesztései:

* hello konfigurációs kiszolgáló és a RX hello proxy mögött fontosságúak tooregister toohello Site Recovery-tárolóban az sikertelen.
* hello helyreállítási időkorlát (RPO) hello órák száma nem teljesül az hello állapotjelentése nem frissülnek.
* hello konfigurációs kiszolgáló nem szinkronizálja a RX, amikor hello ESX hardveres jellemzőit vagy hálózati adatok bármely UTF-8 karaktereket tartalmaz.
* Windows Server 2008 R2 rendszerű tartományvezérlők tooboot sikertelen helyreállítás után.
* Kapcsolat nélküli szinkronizálás nem a várt módon működik.
* A virtuális gép (VM) feladatátvétel után replikációs érpáras törlése hosszú ideig lekérdezi Beragadt hello CX felhasználói felület, felhasználók nem és hello feladat-visszavétel befejezése folytatási művelete.
* A teljes pillanatkép hello konzisztencia feladat által végzett műveletek optimalizált toohelp csökkenti az alkalmazás bontja a kapcsolatot, például az SQL-ügyfelek számára.
* hello konzisztencia eszköz (VACP.exe) hello teljesítménye javult szükséges pillanatképeinek Windows hello memóriahasználat csökkentése révén.
* hello leküldéses telepítési szolgáltatás összeomlik, ha hello jelszó hosszabb 16 karakternél.
* vContinuum nincs ellenőrzése és új vCenter hitelesítő adatokat kér, ha hello hitelesítő adatok módosításakor.
* Linux hello fő gyorsítótár-kezelő (cachemgr) értéke nem fájlok letöltését hello folyamatkiszolgáló, így replikációs pár sávszélesség-szabályozás.
* Hello fizikai feladatátvevő fürtöt (MSCS) lemez sorrendje nem hello azonos hello minden csomóponton, ha nincs beállítva replikációs hello fürtkötetek részénél.
  <br/>Vegye figyelembe, hogy hello fürthöz kell látható el újra védelemmel toobe tootake előnyeit, ez a javítás.  
* SMTP-funkciók nem a várt módon működik Scout 7.1 tooScout 8.0.1 a RX frissítése után.
* További statisztikák hozzáadott hello napló hello visszaállítási művelet tootrack hello ideig tartott toocomplete azt.
* Már támogatja a Linux operációs rendszereken hello forráskiszolgálón:
  * Red Hat Enterprise Linux (RHEL) 6 frissítés 7
  * CentOS 6 7 frissítése
* hello CX és a RX felhasználói felülete most megjelenítése hello értesítési hello pár, amelyek bitkép üzemmódba.
* hello következő biztonsági javításokat bővült a RX:

| **A probléma leírása** | **Eljárások végrehajtása** |
| --- | --- |
| Engedélyezési megkerülése paraméter illetéktelen keresztül |Korlátozott hozzáférés toonon megfelelő felhasználók. |
| Webhelyközi kérések hamisításának megakadályozása |Megvalósított hello lap-jogkivonat fogalom, amely minden oldalon véletlenszerűen generálja. <br/>Ez jelenik meg: <li> Csak egy egyszeri bejelentkezési példány hello az ugyanahhoz a felhasználóhoz.</li><li>Oldal frissítési nem működik – toohello irányítópult átirányítja.</li> |
| Rosszindulatú fájl feltöltése |Korlátozott fájlok toocertain bővítmények. A bővítmények engedélyezett: 7z, aiff, ASP avi, bmp, csv, doc, docx, fla, flv, gif, gz, gzip, jpeg, jpg, napló mid, mov, mp3, mp4, Microsoft termékkód, mpeg, mpg, ods, odt, pdf, png, ppt, pptx, pxd, qt, ram, rar, erőforrás-kezelő, rmi, rmvb, rtf, sdc, sitd, swf, sxc, sxw, bont, tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml, és zip. |
| Állandó többhelyes parancsfájlok |Hozzáadja a bemeneti érvényesítést. |

> [!NOTE]
> * Összes helyreállítás esetben összegző frissítésről. Frissítés 3 rendelkezik Update 1 és 2. frissítés összes hello javítását. Frissítés 3 közvetlenül alkalmazható 8.0.1 GA
> * hello konfigurációs kiszolgáló és a RX frissítések után hello rendszer alkalmazza azokat a még nem állítható.
>
>

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Az Azure Site Recovery Scout 8.0.1 frissítés 2 (a 03 frissítés Dec 15)
2. frissítés javításairól a következők:

* **Konfigurációs kiszolgáló**: javítást alkalmaztunk a hibát, amely miatt nem sikerült hello 31 napos ingyenes mérési szolgáltatás működését várt hello konfigurációs kiszolgáló sikeresen regisztrálva lett a Site Recovery szolgáltatásban.
* **Az Unified agent**: egy a, amelyek nem telepítésének hello fő célkiszolgálón a 8.0-s verzió too8.0.1 frissítés hello frissítés Update 1 a hiba javítása.

### <a name="azure-site-recovery-scout-801-update-1"></a>Az Azure Site Recovery Scout 8.0.1 Update 1
1. frissítés tartalmazza a hello következő hibajavításokat tartalmaz, és új funkciók:

* 31 napos ingyenes védelmet egy kiszolgáló-példány. Ez lehetővé teszi, hogy tootest funkcionalitással kapcsolatban, vagy egy-a-koncepció igazolása beállítása.
  * Összes művelet hello kiszolgálón, beleértve a feladatátvételi és a feladat-visszavétel szabadon hello az első 31 napig, hello idő a kiszolgáló először által védett hely helyreállítási Scout kezdve.
  * A hello 32 nap-től, egyes védett kiszolgálókon kell fizetnie Azure Site Recovery védelmét tooa felhasználói tulajdonú hely hello szabványos példány sebessége.
  * Bármikor jelenleg felszámított védett kiszolgálók számának hello érhető hello irányítópult oldalán hello Azure Site Recovery-tárolóban.
* Támogatja a vSphere parancssori felület (vCLI) 5.5 Update 2.
* Megjelent a Linux operációs rendszerek hello forráskiszolgálón támogatása:
  * RHEL 6 6 frissítése
  * RHEL 5 11 frissítése
  * A centOS 6 frissítés 6
  * CentOS 5 frissítés 11
* Hibajavítások tooaddress hello a következő problémákat:
  * Tároló regisztrálása meghiúsul az hello konfigurációs kiszolgáló vagy a RX kiszolgáló.
  * Fürtkötetek nem megfelelően jelenik meg a fürtözött virtuális gépek vannak látható el újra védelemmel visszatértekor azokat.
  * Feladat-visszavétel sikertelen lesz, amikor a fő célkiszolgáló hello hello helyszíni üzemi virtuális gépektől eltérő ESXi-kiszolgálón található.
  * Konfigurációs fájl engedélyei módosulnak too8.0.1, amely hatással van a védelem és a műveletek frissítésekor.
  * hello újraszinkronizálás küszöbérték a vártnak, amely vezet tooinconsistent replikáció működését nem érvényesíti.
  * hello helyreállítási Időkorlátra vonatkozó beállításait nem jelennek meg helyesen hello konfigurációs kiszolgáló felületén. tömörítetlen hello értéke helytelenül jeleníti meg a tömörített hello értékét.
  * hello eltávolítási művelet nem a várt módon hello vContinuum varázsló törli, és replikációs hello konfigurációs kiszolgáló felületén nem törlődik.
  * Hello vContinuum varázslóban hello lemez automatikusan ki nem jelölt kattintva **részletek** hello lemez nézetben MSCS virtuális gépek védelme során.
  * Hello fizikai-virtuális (P2V) forgatókönyvben során HP szükséges szolgáltatások, például CIMnotify és CqMgHost, nem a virtuális gép helyreállítása áthelyezett toomanual. Az eredmény további rendszerindítás.
  * Linux virtuális gép védelme sikertelen lesz, ha több mint 26 lemezek a fő célkiszolgáló hello.

## <a name="next-steps"></a>Következő lépések
A hello rendelkező kérdéseit [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).
