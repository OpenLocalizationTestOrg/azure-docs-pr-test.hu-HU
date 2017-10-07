---
title: "az SAP HANA Azure (nagy példányok) rendelkezésre állási és vészhelyreállítási helyreállítási aaaHigh |} Microsoft Docs"
description: "Magas rendelkezésre állás és az Azure (nagy példány) az SAP HANA vész-helyreállítási terv létrehozásához."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0c0967f54cf29bbb275eb7cda9d36608488add9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>SAP HANA (nagy példányok) magas rendelkezésre állási és vészhelyreállítási helyreállítási az Azure-on 

Magas rendelkezésre állású és vész-helyreállítási a kritikus fontosságú SAP HANA Azure (nagy példányok) kiszolgálón futó fontos szempontot. Annak fontos toowork az SAP, a rendszer integráló, vagy a Microsoft tooproperly rendszerfejlesztők és a megvalósítása hello jobb felső – rendelkezésre állási/vész-helyreállítási stratégiát. Akkor is fontos tooconsider hello helyreállításipont-célkitűzés és a helyreállítási idő célkitűzése, amelyek adott tooyour környezet.

## <a name="high-availability"></a>Magas rendelkezésre állás

A Microsoft SAP HANA magas rendelkezésre állású módszerek "hello mezőbe out" többek között támogatja:

- **Tárolóreplikálást:** hello tárolási rendszer képes tooreplicate összes adatok tooanother helye (belül, vagy különálló, hello ugyanabban az adatközpontban). SAP HANA működése független ezt a módszert.
- **HANA replikációs:** az összes adat replikálási hello SAP HANA tooa külön SAP HANA rendszerben. hello helyreállítási idő célkitűzése keresztül rendszeres időközönként adatreplikáció másodpercekre csökken. SAP HANA támogatja az aszinkron, szinkron memórián belüli és szinkron módban (csak SAP HANA javasolt belüli rendszerek hello ugyanabban az adatközpontban, vagy legalább 100 KM-egymástól). Csak magas rendelkezésre állású hello aktuális kialakításában HANA nagy példány bélyegzők HANA replikációs használható.
- **Gazdagép automatikus feladatátvételt:** egy helyi hiba-helyreállítási megoldás toouse, egy alternatív toosystem replikálásra. Hello főcsomópont nem érhető el, ha egy vagy több készenléti SAP HANA-csomópont a kibővített módban vannak konfigurálva, és az SAP HANA automatikusan átkerül tooanother csomópont.

A SAP HANA magas rendelkezésre állás további információkért lásd: hello SAP-adatait a következő:

- [SAP HANA magas rendelkezésre állású tanulmány](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [SAP HANA felügyeleti útmutató](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [Az SAP HANA replikációs SAP Academy videó](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [Támogatási Megjegyzés #1999880 – gyakori kérdések az SAP HANA replikációs SAP](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- [SAP támogatási Megjegyzés #2165547 – SAP HANA biztonsági mentési és visszaállítási SAP HANA rendszer replikációs környezeten belül](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [Támogatási Megjegyzés #1984882 – SAP HANA replikációs használatával SAP minimális vagy nulla állásidővel hardver Exchange-hez](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="disaster-recovery"></a>Vészhelyreállítás

A két Azure-régiók geopolitikai régióban SAP HANA Azure (nagy példányok) kínálják. Hello két nagy példány bélyegzők két különböző régiók közötti adatreplikálás vészhelyreállítás során a közvetlen kapcsolattal van. hello adatok replikálása hello tároló-infrastruktúra alapú. hello replikációs alapértelmezés szerint nem történik meg. Hello ügyfél konfigurációk hello vész-helyreállítási rendelt elkészült. Hello aktuális kialakításában HANA replikációs vész-helyreállítási nem használható.

Azonban tootake előny, hello vész-helyreállítási toostart toodesign hello hálózati kapcsolat toohello két különböző Azure-régiók kell. toodo Igen, szükséges a helyszíni Azure ExpressRoute körön kapcsolat a fő Azure-régió, és egy másik kapcsolat kapcsolat a helyszíni tooyour vész-helyreállítási régióban. Ez a mérték kiterjedő olyan helyzet, amelyben a a teljes Azure-régiót, beleértve a Microsoft vállalati peremhálózati útválasztó (MSEE) helye egy hibát tartalmaz.

Második mérőszámként minden Azure virtuális hálózatok, amelyek kapcsolódnak az Azure (nagy példányok) HANA tooSAP hello régiók tooboth az adott ExpressRoute-Kapcsolatcsoportok egyikével csatlakoztathatja. Ez a mérték megoldást egy esetet, ahol hello MSEE helyek csak az egyik, amely a helyszíni hely Azure-ral kerül nem működik.

hello következő ábrán látható hello optimális konfigurációs katasztrófa utáni helyreállítás:

![Optimális konfigurációs katasztrófa utáni helyreállítás](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)

hello hello vész-helyreállítási konfiguráció az optimális esetében toohave két ExpressRoute-Kapcsolatcsoportok a helyszíni toohello két különböző Azure-régiók. Egy kör tooregion #1 fut egy éles példánya kerül. hello második ExpressRoute-kapcsolatcsoportot tooregion #2, néhány nem éles HANA példányt futtatva kerül. (Ez akkor fontos, ha egy teljes Azure-régió, beleértve hello MSEE és nagy példány stamp visszatér hello rács.)

Második mérőszámként hello különböző virtuális hálózatok vannak csatlakoztatott toohello különböző ExpressRoute-Kapcsolatcsoportok, amelyek csatlakoztatott tooSAP HANA Azure (nagy példány). Később tárgyaljuk, megkerüléséhez hello helyre, ha az egy MSEE sikertelen vagy hello helyreállításipont-célkitűzés vész-helyreállítási csökkenthető.

a vész-helyreállítási telepítő hello következő követelményei a következők:

- Az Azure (nagy példányok) termékváltozatok hello ugyanaz a gyártási termékváltozatok méretezés, és azok hello vész-helyreállítási régióban az SAP HANA kell rendeznie. Ezek a példányok használt toorun teszt, a védőfal vagy a QA HANA példányok lehet.
- Kell rendeznie vész-helyreállítási profil az egyes az Azure (nagy példányok) termékváltozatok a SAP HANA, amelyet az toorecover hello vész-helyreállítási helyen, ha szükséges. Ez a művelet eredménye toohello lefoglalása a tároló kötetek, amelyek hello tárolóreplikálást a termelési régióban hello vész-helyreállítási régióba hello célját.

Megfelel a fenti követelmények hello, miután a felelősség toostart hello tárolóreplikálást áll. Hello tároló-infrastruktúrájában SAP Hana Azure (nagy példányok) használt hello tárolóreplikálást alapja storage-pillanatfelvételekkel. toostart hello vész-helyreállítási replikációs tooperform kell:

- A logikai egység már ismertetett rendszerindító pillanatképet.
- Pillanatkép készítése a HANA kapcsolatos kötetek, a fentebb leírt módon.

Ezeket a pillanatképeket végrehajtása után a program a vész-helyreállítási profil hello vész-helyreállítási régióban társított hello köteteken rendezés hello kötetek kezdeti replikáját.

Ezt követően a hello legutóbbi tárolási pillanatkép óránként szolgál, amely a fejlesztés hello tároló köteteken tooreplicate hello eltéréseit.

hello helyreállításipont-célkitűzést, amely érhető el, ez a konfiguráció 60 perc közötti too90 van. tooachieve jobb helyreállítási pont hello vész-helyreállítási helyzet, a Másolás hello HANA tranzakciónapló biztonsági mentései az SAP HANA az Azure (nagy példányok) toohello célkitűzést más Azure-régiót. tooachieve a helyreállításipont-célkitűzést, a következő hello:

1. Készítsen biztonsági másolatot hello HANA tranzakció jelentkezzen gyakran lehető túl/hana / / biztonsági másolatát.
2. Másolja a hello tranzakciónapló biztonsági mentései, amikor azok végzett tooan Azure virtuális gép (VM), amely olyan virtuális hálózatot, amely a csatlakozott toohello SAP HANA Azure (nagy példányok) kiszolgálón.
3. Ezt a virtuális Gépet másolja a biztonsági mentési tooa hello virtuális Gépet, amely a virtuális hálózat hello vész-helyreállítási régióban van.
4. Ne hello tranzakció napló-biztonságimásolatokat a virtuális gép hello régió.

Vészhelyzet esetén, miután hello vész-helyreállítási profil egy tényleges kiszolgálón van telepítve, másolja hello tranzakciónapló biztonsági mentései a hello VM toohello SAP HANA Azure (nagy példányokat), amely jelenleg hello elsődleges kiszolgálója hello vész-helyreállítási régióban, és állítania. A helyreállítás nem lehetséges, mert hello HANA hello vész-helyreállítási lemezeken állapota, amely a HANA pillanatfelvétel. Ez a tranzakciónapló biztonsági mentései további visszaállítását hello eltolási pontját.

## <a name="backup-and-restore"></a>Biztonsági mentés és visszaállítás

Hello egyik legfontosabb szempontja toooperating adatbázisok közül az egyik van annak biztosítása, hello adatbázis védelme a különböző katasztrofális esemény. Ezek az események természeti katasztrófa toosimple felhasználói hibák bármi más által is okozhat.

Egy adatbázis biztonsági mentéséhez, a hello képességét toorestore azt tooany pont időben (például egy, a kritikus fontosságú adatok törlése előtt), lehetővé teszi, hogy a visszaállítási tooa állapot, amely a lehető toohello megszokottól hello megszakítása előtt zárja be van.

Kétféle típusú biztonsági mentést kell végezni a legjobb eredmények elérése érdekében:

- Adatbázisok biztonsági mentése
- Tranzakciónapló biztonsági mentései

Továbbá toofull adatbázis a biztonsági másolatok az alkalmazás szintjén, akkor lehet alaposabb még által biztonsági másolatok tárolási pillanatképekkel. Napló biztonsági mentést végez fontos is hello adatbázis (és tooempty hello naplók már véglegesített tranzakciók) visszaállítása.

SAP HANA Azure (nagy példányok) két biztonsági mentési és helyreállítási lehetőségeket kínálja:

- Ehhez saját kezűleg (DIY). Miután tooensure elég szabad lemezterület kiszámításához, hajtsa végre a teljes adatbázis és a napló biztonsági mentések lemez biztonsági mentési módszerek (toothose lemezek) használatával. Az idő múlásával hello biztonsági mentések másolt tooan Azure storage-fiók (Miután beállította az Azure-alapú fájlkiszolgálón gyakorlatilag korlátlan tárolóval), vagy használhat egy Azure Backup-tárolóban, vagy a cold az Azure storage. Másik lehetőség is toouse egy külső data protection eszközt, például a Commvault, miután bekerültek toostore hello biztonsági mentések másolt tooa tárfiók. lehet, hogy hello DIY biztonsági mentési beállításának is szükséges, amelyet a toobe előírásainak és naplózási célokra hosszabb ideig tárolt adatokat.
- Hello biztonságimásolat-készítő és állítsa vissza az SAP HANA Azure (nagy példány) az alapul szolgáló infrastruktúra hello funkciókat biztosít. Ez a beállítás megfelel hello kell a biztonsági mentésekhez, és manuális biztonsági mentések majdnem elavult (kivéve, ha az adatok biztonsági szükséges megfelelőségi okokból) miatt. Ez a szakasz címek hello részeinek hello biztonsági mentési és működőképes állapotba hozni (nagy példányok) HANA ajánlott.

> [!NOTE]
> hello pillanatkép technológia hello alapul szolgáló infrastruktúrája HANA (nagy példányok) által használt függőség SAP HANA-pillanatképekkel rendelkezik. SAP HANA-pillanatképek nem működnek együtt a SAP HANA több-Bérlős adatbázis tárolók. Ez a biztonsági mentési módszer emiatt nem lehet használt toodeploy SAP HANA több-Bérlős adatbázis tárolók.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>(Nagyméretű példányok) Azure storage-pillanatfelvételekkel a SAP HANA használatával

hello tároló-infrastruktúrájának alapjául szolgáló SAP HANA Azure (nagy példányok) támogatja a kötetek tárolási pillanatképe hello fogalmát. Biztonsági mentési és visszaállítási egy adott kötet használata támogatott, a következő szempontok hello:

- Adatbázis biztonsági mentése helyett tárolási kötet pillanatfelvételeket készít a gyakran.
- hello tárolási pillanatkép kezdeményez egy SAP HANA-pillanatkép, mielőtt végrehajtja a hello tárolási pillanatkép. Az SAP HANA-pillanatkép hello tárolási pillanatkép helyreállítása után az hello telepítő pont a végleges napló visszaállítását.
- Hol hello tárolási pillanatkép sikeresen végre hello ponton hello SAP HANA pillanatkép törlését.
- Napló biztonsági mentése gyakran és hello napló biztonsági mentési köteten, vagy az Azure-ban tárolja.
- Ha hello adatbázis visszaállítására tooa egyes terjesztésipont-időben, a kérelem tooMicrosoft Azure támogatási szolgálatának (éles kimaradásáról) vagy SAP HANA az Azure szolgáltatásfelügyelet toorestore tooa bizonyos tárolási pillanatkép (például egy tervezett visszaállítás védőfal rendszer tooits eredeti állapot).
- hello SAP HANA pillanatkép hello tárolási pillanatfelvétel része egy eltolási pontot az alkalmazása a naplóalapú biztonsági mentések végrehajtása és hello tárolási pillanatkép készítése tárolja.
- A napló biztonsági mentése készít a toorestore hello adatbázis hátsó tooa egyes időpontra történő.

Adja meg hello biztonsági mentés\_nevét a következő kötetek hello pillanatképet készít:

- Hana/adatok
- Hana/napló
- Hana/log\_(csatlakoztatott tartalékként hana/naplóba) biztonsági mentése
- Hana/megosztott

### <a name="storage-snapshot-considerations"></a>Tárolási pillanatkép kapcsolatos szempontok

>[!NOTE]
>A pillanatképek tárolási _nem_ megadott díjmentesen, mert további tárhelyet kell kiosztani.

SAP Hana (nagy példányok) Azure storage-pillanatfelvételekkel hello adott idejéről a következők:

- Egy bizonyos tárolási pillanatkép (pontján hello mikor szükséges idő) igényel, csekély tárolási.
- Adatok a tartalmi változások és a fájlok módosulnak hello tárolási köteten SAP HANA-adatok tartalma hello hello pillanatkép kell toostore hello eredeti tartalom blokkolása.
- hello tárolási pillanatkép mérete növekszik. hello hosszabb hello készült pillanatkép, hello nagyobb hello tárolási pillanatkép válik.
- További módosítások toohello SAP HANA-adatbázis kötetén hello hello élettartama a tárolási pillanatfelvétel, hello nagyobb hello lemezterület-felhasználást hello tárolási pillanatkép válik.

Az Azure (nagy példányok) SAP HANA hello SAP HANA adatainak és naplókönyvtárainak kötet mérete rögzített tartalmaz. Pillanatképek készítése a köteteket végrehajtása eats a kötet térbe kerülnek, hogy a felelősség tooschedule storage-pillanatfelvételekkel (belül hello SAP HANA Azure [nagy példányok] folyamat).

hello alábbi szakaszok információt nyújtanak a ezeket a pillanatképeket, beleértve az általános ajánlásokat végrehajtásához.

- Bár a hello hardver kötetenként 255 pillanatképek képes elviselni, erősen ajánlott licencjelentéseket is e szám alá.
- Mielőtt elvégezné a storage-pillanatfelvételekkel, figyeléséhez és nyomon követésére szabad terület.
- Hello kevesebb a szabad lemezterület alapján storage-pillanatfelvételekkel. Előfordulhat, hogy mindig a pillanatképek számát toolower hello, vagy meg kell tooextend hello kötetek. (Sorba rendezheti további tárhely 1 TB méretű egységekbe.)
- Során tevékenységek, például SAP HANA rendszer áttelepítési eszközökről (R3load, vagy SAP HANA-adatbázisok visszaállítása biztonsági másolatból) adatok áthelyezése Határozottan javasoljuk, hogy nem elvégezhető a storage-pillanatfelvételekkel. (Ha a rendszer áttelepítés egy új SAP HANA-rendszeren kell végrehajtania, storage-pillanatfelvételekkel nem kellene végre toobe.)
- SAP HANA-táblák nagyobb átszervezések során storage-pillanatfelvételekkel kell elkerülhető, ha lehetséges.
- Tárolási pillanatképei egy SAP HANA Azure (nagy példányok) előfeltétel tooengaging hello vész-helyreállítási képességeit.

### <a name="setting-up-storage-snapshots"></a>Storage-pillanatfelvételekkel beállítása

1. Győződjön meg arról, hogy Perl hello Linux operációs rendszer hello HANA (nagy példányok) kiszolgálóra van telepítve.
2. Módosítsa/etc/ssh/ssh\_config tooadd hello sor _Mac számítógépekhez hmac-sha1_.
3. Hozzon létre egy SAP HANA-biztonsági mentés felhasználói fiókot hello fő csomóponton minden SAP HANA-példány (ha van ilyen) futtatja.
4. hello SAP HANA HDB ügyfél telepítenie kell minden olyan kiszolgálón SAP HANA (nagy példány).
5. Kiszolgálón hello első SAP HANA (nagy példányok) minden egyes régió egy nyilvános kulcsot kell létrehozni az alapul szolgáló tárolási infrastruktúra, amely szabályozza a pillanatkép létrehozása tooaccess hello.
6. Másolja az azure hello parancsfájlt\_hana\_/parancsfájlok toohello helyéről backup.pl **hdbsql** a hello SAP HANA-telepítés.
7. Másolás hello HANABackupDetails.txt fájlként/parancsfájlok toohello azonos módon hello Perl parancsfájl helyét.
8. Hello HANABackupDetails.txt fájl a megfelelő hello megfelelő felhasználói specifikációk módosításához.

### <a name="step-1-install-sap-hana-hdbclient"></a>1. lépés: Az SAP HANA HDBClient telepítése

hello Azure (nagy példányok) SAP HANA telepített Linux hello mappákat tartalmazza, és a parancsfájlok szükséges tooexecute SAP HANA storage-pillanatfelvételekkel biztonsági mentési és vész-helyreállítási célokra. Azonban célszerű a felelősség tooinstall SAP HANA HDBclient, SAP HANA telepítése. (A Microsoft telepíti a hello HDBclient sem SAP HANA.)

### <a name="step-2-change-etcsshsshconfig"></a>2. lépés: Módosítsa/etc/ssh/ssh\_config

Módosítsa/etc/ssh/ssh\_hozzáadásával config _Mac számítógépekhez hmac-sha1_ sor itt látható módon:
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

### <a name="step-3-create-a-public-key"></a>3. lépés:, Hozzon létre egy nyilvános kulcsot

A hello első SAP HANA minden Azure-régió, az Azure (nagy példányok) kiszolgálón hozza létre a használt nyilvános kulcsú toobe tooaccess hello tároló-infrastruktúra pillanatképeket hozhat létre. hello nyilvános kulcs ellenőrzi, hogy a jelszó nem szükséges toosign toohello Storage és jelszó hitelesítő adatok nem állnak fenn. A Linux hello SAP HANA (nagy példányok) kiszolgálón hajtható végre a következő parancs toogenerate hello nyilvános kulcs hello:
```
  ssh-keygen –t dsa –b 1024
```
Új helye _/root/.ssh/id hello\_dsa.pub. Ne adjon meg egy tényleges jelszót, vagy pedig fogja szükséges tooenter hello jelszavát minden egyes bejelentkezéskor. Ehelyett nyomja le az **Enter** kétszer tooremove hello adja meg a jelszót követelmény való bejelentkezéshez.

Ellenőrizze, hogy hello nyilvános kulcs javította mappák too/root/.ssh/ módosítása, illetve majd végrehajtása hello által várt toomake **ls** parancsot. Ha hello kulcs található, másolja azt hello a következő parancs futtatásával:

![Nyilvános kulcs a következő parancs futtatásával másolódik](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Ezen a ponton lépjen kapcsolatba az Azure szolgáltatásfelügyelet SAP HANA, és adja meg a hello kulcs. hello képviselőjével használ hello nyilvános kulcs tooregister legyen az alapul szolgáló tárolási infrastruktúra hello.

### <a name="step-4-create-an-sap-hana-user-account"></a>4. lépés: Egy SAP HANA-felhasználói fiók létrehozása

Hozzon létre egy SAP HANA-felhasználói fiókot SAP HANA Studio biztonsági mentés céljából. Ennek a fióknak rendelkeznie kell a következő jogosultságokkal hello: _biztonsági mentési rendszergazda_ és _katalógus olvasási_. Ebben a példában hello felhasználónév SCADMIN jön létre.

![A felhasználó létrehozása a HANA Studióban](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-hello-sap-hana-user-account"></a>5. lépés: Engedélyezze hello SAP HANA-felhasználói fiók

Engedélyezi a hello SAP HANA-felhasználói fiókot (toobe anélkül, hogy az engedélyezési, minden alkalommal, amikor hello parancsfájl futtatása hello parancsfájlok által használt). hello SAP HANA-parancs `hdbuserstore` lehetővé teszi, hogy egy SAP HANA felhasználói kulcs, legalább egy SAP HANA-csomópontok tárolt hello létrehozását. hello felhasználói kulcs is lehetővé teszi, hogy hello felhasználói tooaccess SAP HANA anélkül, hogy a folyamat, amelyet később scripting hello belül toomanage jelszavakat.

>[!IMPORTANT]
>Futtatási hello parancsot következő `_root_`. Ellenkező esetben hello parancsprogramot nem fog megfelelően működni.

Adja meg a hello `hdbuserstore` parancsot a következőképpen:

![Adja meg a hello hdbuserstore parancs](./media/hana-overview-high-availability-disaster-recovery/image4-hdbuserstore-command.png)

A következő példában, ahol hello felhasználói SCADMIN01 pedig hello állomásnév lhanad01, hello hello parancs a következő:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Kezelheti az összes parancsfájl-kezelési kibővített HANA példányok egyetlen kiszolgálóról. Ebben a példában hello SAP HANA-kulcs SCADMIN01 módosítani kell úgy, hogy a kapcsolódó toohello kulcs hello gazdagéphez tükrözi minden állomás számára. Ez azt jelenti, hogy hello SAP HANA biztonsági mentési fiók módosul hello példány számú hello HANA DB **lhanad**. hello kulcs rendszergazdai jogosultsággal kell rendelkeznie a hello gazdagépen hozzá van rendelve, és biztonsági mentési felhasználó hello kibővített kell hozzáférési jogok tooall SAP HANA-példányok.
```
hdbuserstore set SCADMIN01 lhanad:30015 SCADMIN <password>
hdbuserstore set SCADMIN02 lhanad:30115 SCADMIN <password>
hdbuserstore set SCADMIN03 lhanad:30215 SCADMIN <password>
```

### <a name="step-6-copy-items-from-hello-scripts-folder"></a>6. lépés: Hello/parancsfájlok mappából elemek másolása

Másolás hello alábbi elemek a hello/mappát (hello arany lemezképen hello telepítés része) toohello munkakönyvtár megadása a parancsfájlok **hdbsql**. Ez a könyvtár aktuális HANA telepítés esetén /hana/shared/D01/exe/linuxx86\_64/hdb.
```
azure\_hana\_backup.pl
testHANAConnection.pl
testStorageSnapshotConnection.pl
removeTestStorageSnapshot.pl
HANABackupCustomerDetails.txt
```
Ha kibővített futnak a következő elemek hello vagy OLAP másolja:
```
azure\_hana\_backup\_bw.pl
testHANAConnectionBW.pl
testStorageSnapshotConnectionBW.pl
removeTestStorageSnapshotBW.pl
HANABackupCustomerDetailsBW.txt
```
hello HANABackupCustomerDetails.txt fájl méretezett telepítéshez az alábbiak szerint módosítható. Hello storage-pillanatfelvételekkel futó hello parancsfájl hello vezérlő és a konfigurációs fájlt is. Hello kell kapott _tárolási biztonsági másolat neve_ és _tárolási IP-cím_ az SAP HANA az Azure szolgáltatásfelügyelet, ha a példányok üzembe helyezése. Nem módosítható hello feladatütemezési, rendezés, vagy térköz bármely hello változók vagy hello parancsfájl nem működik megfelelően.

Méretezett üzembe helyezés esetén hello konfigurációs fájl alábbihoz hasonlóan fog kinézni:
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Created by customer using hdbuserstore
HANA Backup Name: SCADMIND01
```
Kibővített konfiguráció esetén hello HANABackupCustomerDetailsBW.txt fájl alábbihoz hasonlóan fog kinézni:
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Node IP addresses, instance numbers, and HANA backup name
#provided by customer.  HANA backup name created using
#hdbuserstore utility.
Node 1 IP Address: 10.254.15.21
Node 1 HANA instance number: 01
Node 1 HANA Backup Name: SCADMIN01
Node 2 IP Address: 10.254.15.22
Node 2 HANA instance number: 02
Node 2 HANA Backup Name: SCADMIN02
Node 3 IP Address: 10.254.15.23
Node 3 HANA instance number: 03
Node 3 HANA Backup Name: SCADMIN03
Node 4 IP Address: 10.254.15.24
Node 4 HANA instance number: 04
Node 4 HANA Backup Name: SCADMIN04
Node 5 IP Address: 10.254.15.25
Node 5 HANA instance number: 05
Node 5 HANA Backup Name: SCADMIN05
Node 6 IP Address: 10.254.15.26
Node 6 HANA instance number: 06
Node 6 HANA Backup Name: SCADMIN06
Node 7 IP Address: 10.254.15.27
Node 7 HANA instance number: 07
Node 7 HANA Backup Name: SCADMIN07
Node 8 IP Address: 10.254.15.28
Node 8 HANA instance number: 08
Node 8 HANA Backup Name: SCADMIN08
```
>[!NOTE]
>Jelenleg csak az 1. csomóponton részletek hello tényleges HANA tárolási pillanatkép parancsfájl használnak. Azt javasoljuk, hogy minden HANA csomópontról hozzáférés tooor tesztelése úgy, hogy ha valaha is változik hello biztonsági mentési főcsomópont, már gondoskodott róla, hogy bármely másik csomópont végrehajthatók lesznek az 1. csomópont hello részletek módosításával.

toocheck hello megfelelő konfigurációk hello konfigurációs fájl vagy a megfelelő csatlakozási toohello HANA példányok, futtassa a következő parancsfájlok hello egyikét:
- (A SAP munkaterhelés független) méretezett konfigurációt:

 ```
testHANAConnection.pl
```
- Kibővített konfigurációt:

 ```
testHANAConnectionBW.pl
```

Győződjön meg arról, hogy hello fő HANA példány szükséges tooall HANA kiszolgálók. Nincsenek nem paraméterek toohello parancsprogram, de meg kell adnia a megfelelő HANABackupCustomerDetails hello / HANABackupCustomerDetailsBW hello parancsfájl toorun file ennek megfelelően. Csak hello rendszerhéj parancs hibakódok ad vissza, mert nincs lehetőség a hello parancsfájl tooerror-ellenőrzést minden példány. Még ha így hello parancsfájl megjegyzést néhány hasznos meg toodouble-ellenőrzéshez.

toorun hello parancsfájlt:
```
 ./testHANAConnection.pl
```
 Hello parancsprogram sikeresen állapotbeolvasás hello hello HANA példány, ha azt, hogy sikeres volt-e hello HANA kapcsolat üzenetet jelenít meg.

Pedig a második típus parancsfájl segítségével is toocheck hello fő HANA példány kiszolgáló képes toosign toohello tároló. Hello azure végrehajtása előtt\_hana\_biztonsági mentési (\_bw) .pl parancsfájl, végre kell hajtani a következő parancsfájl hello. Ha egy kötet pillanatképek nem tartalmaz, a rendszer lehetetlen toodetermine e hello egyszerűen üres, vagy nincs az ssh sikertelen tooobtain hello pillanatkép részleteit. Emiatt hello parancsfájl végrehajtása két lépésből áll:

- Azt ellenőrzi, hogy hello tárolási konzol érhető el.
- Pillanatképet hoz létre-teszt során, vagy helyőrző, minden olyan kötetre HANA példánya.

Emiatt hello HANA példány része, amelynek argumentuma. Ebben az esetben nem lehetséges tooprovide hibaellenőrzés hello tárolási kapcsolat, de hello parancsfájl hasznos mutatókat biztosít, ha hello végrehajtása meghiúsul.

hello parancsfájl futtatása:
```
 ./testStorageSnapshotConnection.pl <hana instance>
```
Vagy fut, mint:
```
./testStorageSnapshotConnectionBW.pl <hana instance>
```
hello parancsfájlt is, hogy-e a megfelelő telepített tooyour tárolási bérlő számára, amely hello logikaiegység-számok (LUN), amelyet Ön a tulajdonosa hello kiszolgálópéldányok használnak köré szerveződik, képes toosign üzenetet jelenít meg.

Hello első tárolási pillanatkép-alapú biztonsági mentések végrehajtása előtt futtassa a hello következő parancsfájlok toomake meg róla, hogy a hello konfigurálása helyes.

Miután ezek a parancsfájlok, törölheti hello pillanatképek hajtja végre:
```
./removeTestStorageSnapshot.pl <hana instance>
```
Vagy
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="step-7-perform-on-demand-snapshots"></a>7. lépés:, Hajtsa végre a tárolt pillanatképek

Hajtsa végre a pillanatképek igény (valamint ütemezett rendszeres pillanatképek használatával cron) itt leírt módon.

Méretezett konfigurációk esetén hajtsa végre a következő parancsfájl hello:
```
./azure_hana_backup.pl lhanad01 customer 20
```
Kibővített konfigurációk esetén hajtsa végre a következő parancsfájl hello:
```
./azure_hana_backup_bw.pl lhanad01 customer 20
```
hello kibővített parancsfájl létezik néhány további ellenőrzési toomake, hogy minden HANA-kiszolgáló elérhető, és példányainak HANA vissza megfelelő állapotot hello példány SAP HANA vagy tárolási pillanatképeinek folytatása előtt.

hello következő argumentumok szükségesek:

- hello HANA példányt igénylő biztonsági mentés.
- hello pillanatkép előtag hello tárolási pillanatkép.
- a pillanatképek toobe hello adott előtag esetében hello száma.

```
./azure_hana_backup.pl lhanad01 customer 20
```

hello parancsfájl végrehajtásának hello hello tárolási pillanatképet hoz ezen három különálló lépésből áll:

- HANA pillanatkép hajtható végre.
- Tárolási pillanatkép hajtható végre.
- Távolítsa el a hello HANA pillanatkép.

Hajtsa végre a hello parancsfájlt mappából hello HDB végrehajtható azt másolt meghívásával. Azt követően kötetek legalább hello biztonsági másolatot készít, de azt is biztonsági másolatot készít hello kötetneve hello explicit SAP HANA-példány nevét tartalmazó kötet.
```
hana_data_<hana instance>_prod_t020_vol
hana_log_<hana instance>_prod_t020_vol
hana_log_backup_<hana instance>_prod_t020_vol
hana_shared_<hana instance>_prod_t020_vol
```
hello megőrzési időszak szigorúan kezel, hello számú pillanatképek paraméterként be (például 20, korábbi műveletek szerint) hello parancsprogram végrehajtásakor. Ezért hello időre hello időszak pillanatképek hello hívásban hello parancsfájl végrehajtása és hello számának függvényében. Ha tárolják a pillanatképek hello száma meghaladja a hello hívás hello parancsfájl paraméterként nevű hello számát, ez a címke pillanatképe legrégebbi tárolási hello (ebben az előző esetben _egyéni_) törlődik, mielőtt új pillanatkép létrehozása végre. Ez azt jelenti, hello utolsó hello hívás paramétere használhatja a pillanatképek számát toocontrol hello hello számot ad hello számát.

>[!NOTE]
>Amint hello címkék módosításához hello számbavételi újraindul.

Tooinclude hello HANA példánynév által biztosított SAP HANA az Azure szolgáltatásfelügyelet argumentumként Ha több csomópontos környezetekben hoznak létre pillanatképek van szüksége. Egycsomópontos környezetben SAP HANA hello Azure (nagy példányok) egységen hello neve nem megfelelő, de továbbra is ajánlott hello HANA példány neve.

Emellett akkor is készítsen biztonsági másolatot a rendszerindító volumes\LUNs hello azonos parancsfájl. Készítsen biztonsági másolatot a rendszerindító kötet legalább egyszer HANA, első futtatásakor bár javasolt egy éjszakánként vagy heti biztonsági mentés ütemezését a cron rendszerindítású. Ahelyett, hogy vegye fel a SAP HANA-példány nevét, mint beszúrása _rendszerindító_ módon argumentum hello hello parancsprogramba az alábbiak szerint:
```
./azure_hana_backup boot customer 20
```
hello azonos adatmegőrzési jogot is toohello rendszerkötethez. Használja a pillanatképek igény leírtak csak, bizonyos esetekben a korábban, például SAP továbbfejlesztése (EHP) csomagot a frissítés során, vagy ha toocreate eltérő tárolási pillanatkép van szüksége.

Javasoljuk, ütemezett tooperform storage-pillanatfelvételekkel cron használatával, és azt javasoljuk, hogy használja-e hello ugyanazt a parancsfájlt a biztonsági mentések és a vész-helyreállítási igényeit (módosítása hello parancsfájl bemenetek toomatch hello különféle biztonsági mentési alkalommal kért). Ezek az összes ütemezett eltérően a cron attól függően, hogy a végrehajtási idő: óránkénti, 12 órás, napi vagy heti. hello cron-ütemezését, amelyek megfelelnek a korábban tárgyalt hello megőrzési címkézését a hosszú távú helyszínen biztonsági mentéshez tervezett toocreate storage-pillanatfelvételekkel. hello parancsfájl parancsok tooback készíteni összes éles kötetről, attól függően, hogy a kért gyakorisága (adatainak és naplókönyvtárainak fájlok biztonsági mentést óránként végzi, mivel hello rendszerindító kötet napi készül biztonsági másolat) tartalmazza.

cron parancsfájl a következő hello hello bejegyzések futtatni minden órában hello 10 perc, 12 óránként hello 10 percet, és naponta hello 10 perc. hello cron feladatok jönnek létre úgy, hogy csak egy SAP HANA-tároló pillanatkép történik bármely adott órán belül, hogy hello óránkénti és a napi biztonsági mentés nem halasztja hello azonos idő (12:10 óra). toohelp optimalizálhatja a pillanatkép létrehozása és a replikációs, az Azure szolgáltatásfelügyelet SAP HANA biztosít hello ajánlott meg toorun számára a biztonsági másolatok.

hello alapértelmezett cron /etc/crontab ütemezését a következőképpen történik:
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 14
```
A hello előző cron utasításokban hello HANA kötetek (nélkül a rendszerindító kötet) lekérése egy pillanatkép-hello címkével óránként. Ezeket a pillanatképeket a 66 megmaradnak. Emellett hello 12 órás címkével 14 pillanatképet megőrzi. Potenciálisan kapunk óránkénti pillanatképek három nap, valamint 12 órás pillanatképek egy másik négy napja, amely lehetővé teszi az egy teljes hét, a pillanatképek.

Cron belül ütemezés megkapni, mert csak egy parancsprogram kell végrehajtani adott bármikor, ha hello parancsfájlok kerülnek lépcsőzetes elrendezésben által néhány percig. Ha hosszú távú megőrzési napi biztonsági mentései használni szeretne, vagy napi pillanatkép tartják együtt (a hét minden megőrzési számát) 12 órás pillanatképet, vagy hello óránkénti pillanatkép lépcsőzetes tootake hely 10 percen belül. Csak egy napi pillanatkép hello éles kötet maradjanak.
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 7
10 0 * * * ./azure_hana_backup.pl lhanad01 daily 7
```
az itt felsorolt hello gyakoriságot példák csak. tooderive az optimális pillanatképek számát, a következő feltételek használata hello:

- A pont időponthoz kötött helyreállítás helyreállítási idő célkitűzése követelményeinek.
- Lemezterület-használat.
- Követelmények a helyreállítási cél és a helyreállítási idő célkitűzése a lehetséges vész-helyreállítási pontja.
- Végső végrehajtási HANA adatbázis teljes biztonsági mentések lemezek ellen. Amikor a lemezeket, szemben a teljes adatbázis biztonsági mentése vagy _backint_ csatoló, a hajtja végre, storage-pillanatfelvételekkel hello végrehajtása sikertelen lesz. Ha azt tervezi, hogy a storage-pillanatfelvételekkel fölött tooexecute adatbázis teljes biztonsági mentések, győződjön meg arról, hogy a pillanatképek tárolási hello végrehajtási le van tiltva, ebben az időszakban.

>[!IMPORTANT]
> hello storage-pillanatfelvételekkel SAP HANA biztonsági mentés használata érvényes csak akkor, ha hello pillanatképek SAP HANA-naplók biztonsági másolatainak együtt történik. Következő naplófájl-biztonsági mentések kell toobe képes toocover hello időszakok hello storage-pillanatfelvételekkel között. Ha beállította a 30 napos pont időponthoz kötött helyreállítás kötelezettségvállalás toousers, hello következőkre lesz szüksége:

- Képes tooaccess, amely a 30 napos tárolási pillanatképet.
- Összefüggő naplóalapú biztonsági mentések hello utolsó 30 nap alatt.

A napló biztonsági mentések hello tartományát, a pillanatkép létrehozása a hello naplómentési kötethez. Azonban lehet, hogy tooperform rendszeres naplóalapú biztonsági mentések, hogy:

- Hello összefüggő naplóalapú biztonsági mentések szükség tooperform időpontban helyreállítást végezni.
- Hello SAP HANA naplózási kötet megakadályozása kevés a hely.

Hello utolsó lépések egyike tooschedule SAP HANA biztonsági mentési naplók SAP HANA Studio. hello SAP HANA-napló biztonsági mentési célhelyre kifejezetten létrehozott hello hana/log\_hello csatlakoztatási pontját /hana/log/backups biztonsági mentések kötetet.

![Az SAP HANA Studio naplózza az SAP HANA-biztonsági mentés ütemezése](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Dönthet úgy, mint 15 percenként biztonsági mentéseket. Egyes felhasználók is biztonsági másolatokat napló percenként, bár nem javasolt folyamatos _keresztül_ 15 perc.

utolsó lépésként hello tooperform fájl alapú biztonsági mentés (után hello SAP HANA kezdeti telepítése) toocreate hello biztonságimásolat-katalógus léteznie kell egy biztonsági mentési tétel. Ellenkező esetben SAP HANA nem indítható el a megadott napló biztonsági másolatokat.

![A fájl alapú biztonsági mentési toocreate hozzon létre egy biztonsági mentési tételt](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)

### <a name="monitoring-hello-number-and-size-of-snapshots-on-hello-disk-volume"></a>Hello számát és méretét, a pillanatképek hello lemezköteten figyelése

Egy adott tárolási köteten figyelheti a pillanatképek és hello tároló fogyasztása a pillanatképek hello száma. Hello `ls` parancs hello pillanatkép könyvtár vagy fájl nem jeleníti meg. Azonban a Linux operációs rendszert futtató parancs hello `du` nem, a következő parancsok hello:

- `du –sh .snapshot`hello pillanatkép könyvtárban lévő összes pillanatképet összesen biztosít.
- `du –sh --max-depth=1`Felsorolja az összes hello .snapshot mappában, és minden pillanatkép hello mérete mentett pillanatképet.
- `du –hc`Itt hello teljes mérete használják az összes pillanatképet.

Ezen parancsok toomake meg arról, hogy venni és tárolt hello pillanatképek nem használ fel minden hello tároló hello köteteken használja.

### <a name="reducing-hello-number-of-snapshots-on-a-server"></a>A pillanatképek olyan kiszolgálón hello számának csökkentése

Ahogy korábban, csökkentheti a hello bizonyos címkék tárolt pillanatképek számát. hello utolsó két paraméterének hello parancs tooinitiate pillanatkép számos címke és hello kívánt pillanatképek tooretain.
```
./azure_hana_backup.pl lhanad01 customer 20
```
Hello előző példában hello pillanatkép címkéje _ügyfél_ és a megőrzött címke toobe pillanatképei hello száma _20_. Megfelelnek a toodisk lemezterület-felhasználást, érdemes lehet tárolt pillanatképek számát tooreduce hello. hello egyszerűen tooreduce hello számú pillanatképpel toorun hello parancsprogram hello utolsó paraméter beállítása too5:
```
./azure_hana_backup.pl lhanad01 customer 5
```
Miatt hello parancsfájl futtatását ezt a beállítást, a pillanatképek, beleértve a hello új pillanatkép, hello száma: _5_.

 >[!NOTE]
 > Ez a parancsfájl csökkenthető hello pillanatképek csak akkor, ha hello legutóbbi korábbi pillanatképből több mint egy órával korábbiak. hello parancsfájl nem törli a pillanatképeket, amelyek kevesebb, mint egy órával korábbiak.

Ezek a korlátozások kapcsolódó toohello választható vész-helyreállítási funkciói.

Ha már nem szeretné ezt az előtagot pillanatképei készlete toomaintain, Ön is végrehajthatja a hello parancsprogram _0_ , hello megőrzési number tooremove ezt az előtagot megfelelő összes pillanatképet. Azonban minden pillanatképet eltávolítása hatással lehet a vész-helyreállítási hello képességeit.

### <a name="recovering-toohello-most-recent-hana-snapshot"></a>Toohello legutóbbi HANA pillanatkép visszaállítása

Hello eseményben tapasztalja egy éles lefelé forgatókönyv, végezze el a tárolási pillanatkép hello folyamatán kezdeményezhető, a felhasználói, amelyhez az SAP HANA az Azure Service Management. Ilyen egy váratlan forgatókönyvet a sürgősség szerinti függetlenül attól, hogy előfordulhat, ha a adat törölve lett egy éles rendszer és hello csak módon tooretrieve hello adatok toorestore hello éles adatbázist.

A hello, ugyanakkor egy időpontban helyreállítási lehet alacsony sürgősség, nappal a tervezett. Megtervezheti ennek a helyreállításnak az SAP HANA az Azure szolgáltatásfelügyelet helyett egy magas prioritású probléma előléptetése. Például akkor lehet, hogy lehet tervezési tootry hello SAP szoftver frissítését úgy, hogy a fejlesztés új csomagot, és alkalmazza, majd kell toorevert biztonsági tooa pillanatkép, amely hello EHP frissítés előtt hello állapotát jeleníti meg.

Hello kérelem adja ki, meg kell toodo bizonyos előkészítés. SAP HANA Azure szolgáltatásfelügyeleti csoport ezután hello kérelem kezelésére, és adja meg a visszaállított hello kötetek. A későbbiekben is tooyou toorestore hello HANA adatbázis hello pillanatképek alapján. Itt hogyan tooprepare hello vonatkozó kérelem:

>[!NOTE]
>A felhasználói felület hello alábbi képernyőképek hello SAP HANA-kiadás módjától függően eltérhetnek.

1. Döntse el, melyik pillanatkép toorestore. Csak hello hana/adatmennyiség kellene állítani, hacsak másként nem kéri.

2. Hello HANA példány leállítása.

 ![Hello HANA példány leállítása](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Válassza le a hello az adatkötetek minden HANA-adatbázis csomóponton. hello hello pillanatkép visszaállítása sikertelen lesz, ha nincsenek fürtköteteiről hello az adatkötetek.

 ![Minden egyes HANA adatbázis csomóponton hello az adatkötetek leválasztása](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Nyissa meg az Azure támogatási kérelem tooinstruct hello visszaállítása egy adott pillanatkép.

 - Hello visszaállítás során: az Azure szolgáltatásfelügyelet SAP HANA kérheti tooattend egy konferencia hívás tooensure, amelyek nincsenek adatok lekérdezése megszakadt.

 - Hello visszaállítást követően: az Azure szolgáltatásfelügyelet SAP HANA értesíti, amikor hello tárolási pillanatkép visszaállítása megtörtént.

5. Hello visszaállítási folyamat befejezése után csatlakoztassa az összes kötetről.

 ![Minden adatkötetnél újracsatlakoztatni](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. Helyreállítási beállítások SAP HANA studióban, akkor válassza, ha nem automatikusan lépnek az újbóli tooHANA DB SAP HANA Studio alkalmazással. hello következő példa bemutatja, visszaállítás toohello utolsó HANA pillanatképet. Tárolási pillanatkép beágyazza egy HANA pillanatképet, és toohello legutóbbi tárolási pillanatkép visszaállítása, hello legutóbbi HANA pillanatkép kell lennie. (Tooolder storage-pillanatfelvételekkel állítja vissza, ha szüksége van-e toolocate hello HANA pillanatkép készítése a hello idő hello tárolási pillanatkép alapján.)

 ![Válassza ki a helyreállítás beállítások SAP HANA studióban](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Válassza ki **helyreállítás hello adatbázis tooa adott adatok biztonsági mentése és a tárolási pillanatkép**.

 ![hello "A helyreállítás típusának megadása" ablakban](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Válassza ki **adjon meg biztonsági másolat nélkül katalógus**.

 ![hello "Adja meg a hely biztonsági mentése" ablak](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. A hello **céltípus** listáról válassza ki **pillanatkép**.

 ![hello "Megadása hello biztonsági mentés tooRecover" ablak](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Kattintson a **Befejezés** toostart hello helyreállítási folyamatot.

 ![Kattintson a "Befejezés" toostart hello helyreállítási folyamat](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. hello HANA-adatbázisból helyreáll, és toohello HANA pillanatkép, amely megtalálható a hello tárolási pillanatkép helyre.

 ![HANA-adatbázis visszaállítása, és a helyreállított toohello HANA pillanatkép](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-toohello-most-recent-state"></a>Legutóbbi toohello-állapot helyreállítása

hello következő folyamat állítja vissza, amely megtalálható a hello tárolási pillanatkép hello HANA pillanatkép. Majd helyreállítja hello tranzakciós napló biztonsági mentések toohello legutóbbi hello adatbázis állapotának hello tárolási pillanatkép visszaállítása előtt.

>[!IMPORTANT]
>Mielőtt továbblépne, győződjön meg arról, hogy rendelkezik-e a tranzakciónapló biztonsági mentései teljes és összefüggő láncolata. A biztonsági mentése nélkül hello hello adatbázis jelenlegi állapota nem állítható vissza.

1. Végezze el az 1-6. lépéseket az előző eljárás a "Helyreállítás toohello legutóbbi HANA pillanatképe." hello

2. Válassza ki **hello adatbázis tooits legutóbbi állapot helyreállításához**.

 ![Válassza a "Hello adatbázis tooits legutóbbi állapot helyreállításához"](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. Adja meg a legutóbbi HANA naplómentésekre hello hello elhelyezkedését. hello helyre kell toocontain összes HANA tranzakciónapló biztonsági mentései a hello HANA pillanatkép toohello legutóbbi állapotból.

 ![Hello legutóbbi HANA naplóalapú biztonsági mentések hello helyének megadása](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. Válassza ki a biztonsági mentés mely toorecover hello adatbázisból alapjaként. A példánkban ez a hello HANA pillanatkép hello tárolási pillanatkép fájlban. (Csak egy pillanatképet, szerepel a következő képernyőkép hello.)

 ![Válassza ki a biztonsági mentés alapjául mely toorecover hello adatbázisból](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. Törölje a jelet hello **használata különbözeti biztonsági mentések** jelölőnégyzetet, ha hello HANA pillanatkép hello idő és a legutóbbi állapot hello közötti eltérések nem léteznek.

 ![Törölje a jelet hello "Használja a különbözeti biztonsági mentések" jelölőnégyzetet, ha nincs eltérések létezik](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. Hello összefoglaló képernyőn kattintson a **Befejezés** toostart hello visszaállítási eljáráshoz.

 ![Kattintson a "Befejezés" hello összefoglaló képernyőn](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-tooanother-point-in-time"></a>Időben helyreállított tooanother pont
toorecover tooa pont hello HANA pillanatkép (hello tárolási pillanatkép szerepel) és egy későbbi, mint hello HANA pillanatkép-időpontban helyreállítási közötti időben hello a következő:

1. Győződjön meg arról, hogy rendelkezik-e az összes tranzakciónapló biztonsági mentései a hello HANA pillanatkép toohello időtartamát toorecover.
2. Hello az eljárást a "Helyreállítás toohello legutóbbi állapot."
3. A hello eljárás hello a 2. lépés **helyreállítási típus megadása** ablakban válassza ki **helyreállítás hello adatbázis toohello következő időpontra történő**, adja meg a hello pontot időben, és fejezze be 3-6. lépéseket.

## <a name="monitoring-hello-execution-of-snapshots"></a>A pillanatképek figyelési hello végrehajtása

Storage-pillanatfelvételekkel toomonitor hello végrehajtásának van szüksége. hello parancsfájl, amely végrehajtja a tárolási pillanatkép kimeneti tooa fájlba ír, és toohello menti és hello Perl-parancsfájlokat ugyanazon a helyen. Minden egyes pillanatkép készült külön fájlt. az egyes fájlok hello kimeneti hello is egyértelműen mutatja, amely végrehajtja a hello pillanatkép parancsfájl különböző fázisait:

- Hello kötetek toocreate igénylő pillanatkép keresése
- Ezek a kötetek vett hello pillanatképek keresése
- Végső meglévő pillanatképek toomatch hello pillanatképek számát a megadott törlése
- HANA pillanatkép létrehozása
- Hello tárolási pillanatkép létrehozásához hello kötetek keresztül
- Hello HANA pillanatkép törlése
- Pillanatkép-túl legutóbbi hello átnevezése**.0**

hello legfontosabb hello parancsfájl része ezt:
```
**********************Creating HANA snapshot**********************
Creating hello HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting hello HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
Ettől a példától láthatja, hogyan hello parancsfájl rekordok hello hello HANA pillanatkép létrehozását. Ez a folyamat hello kibővített esetben hello főcsomópont indítható. hello főcsomópont hello pillanatképek egyes hello munkavégző csomópontokhoz hello szinkron létrehozását kezdeményezi. Majd hello tárolási pillanatkép készítésének időpontjában. Hello sikeres végrehajtása esetén hello storage-pillanatfelvételekkel hello HANA pillanatkép törlését.
