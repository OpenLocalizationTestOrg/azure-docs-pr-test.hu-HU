---
title: "aaaMicrosoft Azure StorSimple virtuális tömb iSCSI telepítése |} Microsoft Docs"
description: "Leírja, hogyan tooperform a kezdeti telepítés regisztrálni a StorSimple iSCSI-kiszolgálót, és eszköz beállításának befejezése."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 4db116d1-978b-48e8-b572-a719a8425dbc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: b4ff6391cb2af69d4e83dcdac5e027f8498005b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array--set-up-as-an-iscsi-server-via-azure-portal"></a>Központi telepítése a StorSimple virtuális tömb – állítsa be Azure-portálon az iSCSI-kiszolgálóként

![az iSCSI-telepítés folyamata](./media/storsimple-virtual-array-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Áttekintés

A telepítési útmutató a Microsoft Azure StorSimple virtuális tömb toohello vonatkozik. Ez az oktatóanyag leírja, hogyan tooperform hello kezdeti beállítás, regisztrálja a StorSimple iSCSI-kiszolgálót, teljes hello Eszközbeállítás, és majd létrehozása, csatlakoztatása, inicializálása és formázása köteteket a StorSimple virtuális iSCSI-kiszolgálóként konfigurált tömbjét. 

hello leírt eljárások itt körülbelül 30 percet too1 óra toocomplete igénybe vehet. Ebben a cikkben közzétett hello információ tooStorSimple virtuális tömbök csak vonatkozik.

## <a name="setup-prerequisites"></a>Telepítési előfeltételek

Előtt konfigurálja, és állítsa be a StorSimple virtuális tömb, győződjön meg arról, hogy:

* Egy virtuális tömb kiosztása és tooit csatlakoztatva a [központi telepítése a StorSimple virtuális tömb - Provision a Hyper-V egy virtuális tömb](storsimple-ova-deploy2-provision-hyperv.md) vagy [központi telepítése a StorSimple virtuális tömb - Provision egy virtuális tömb VMware-ben ](storsimple-virtual-array-deploy2-provision-vmware.md).
* Hello szolgáltatás regisztrációs kulcsának hello létrehozott toomanage a StorSimple virtuális tömbök StorSimple Device Manager szolgáltatás a rendelkezik. További információkért lásd: **2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése** a [központi telepítése a StorSimple virtuális tömb - hello portal előkészítése](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
* Ha ez hello második vagy virtuális tömb, amely egy meglévő StorSimple Device Manager szolgáltatással regisztrál, rendelkeznie kell hello szolgáltatásadat-titkosítási kulcs. Ezt a kulcsot hozott létre, ha hello első eszköz regisztrálása sikeres volt a szolgáltatással. Ha elvesztette ezt a kulcsot, lásd: **Get hello szolgáltatásadat-titkosítási kulcs** a [használata hello webes felhasználói felületén tooadminister a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Részletes beállítása

Lépésenként tooset a következő hello használata, és a StorSimple virtuális tömb beállítása:

* [1. lépés: Hello helyi webes felhasználói felület telepítés befejeződik, és regisztrálja az eszközt](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
* [2. lépés: A szükséges eszköz beállításának befejezése hello](#step-2-complete-the-required-device-setup)
* [3. lépés: Kötet hozzáadása](#step-3-add-a-volume)
* [4. lépés: Csatlakoztassa, inicializálja és formázza a kötetet](#step-4-mount-initialize-and-format-a-volume)

## <a name="step-1-complete-hello-local-web-ui-setup-and-register-your-device"></a>1. lépés: Hello helyi webes felhasználói felület telepítés befejeződik, és regisztrálja az eszközt

#### <a name="toocomplete-hello-setup-and-register-hello-device"></a>toocomplete hello beállítása és hello-eszköz regisztrálása

1. Nyisson meg egy böngészőablakot. tooconnect toohello webes felhasználói felület típusa:
   
    `https://<ip-address of network interface>`
   
    Hello kapcsolati URL-cím hello előző lépésben feljegyzett használja. Értesítés, hogy nincs-e hello webhely biztonsági tanúsítványával kapcsolatos problémára hiba jelenik meg. Kattintson a **Folytatás toothis weblap**.
   
    ![biztonsági tanúsítvány hiba](./media/storsimple-virtual-array-deploy3-iscsi-setup/image3.png)
2. Jelentkezzen be toohello webes felhasználói felülete, mint a virtuális eszköz **StorSimpleAdmin**. Adja meg a hello eszköz rendszergazdai jelszava, amely módosította a 3. lépés: Start hello virtuális eszköz [központi telepítése a StorSimple virtuális tömb - a Hyper-V virtuális eszköz kiépítése](storsimple-virtual-array-deploy2-provision-hyperv.md) vagy [központi telepítése a StorSimple virtuális tömb - A VMware virtuális eszköz kiépítése](storsimple-virtual-array-deploy2-provision-vmware.md).
   
    ![Bejelentkezési oldal](./media/storsimple-virtual-array-deploy3-iscsi-setup/image4.png)
3. Megnyílik toohello **Home** lap. Ez a lap ismerteti hello különböző beállításokat szükséges tooconfigure és regisztrálása hello virtuális eszköz hello StorSimple Device Manager szolgáltatásban. Vegye figyelembe, hogy hello **hálózati beállításai**, **webalkalmazás-proxy beállításainak**, és **időbeállítások** megadása nem kötelező. csak a szükséges beállítás hello **eszközbeállítások** és **Felhőbeállítások**.
   
    ![Kezdőlap](./media/storsimple-virtual-array-deploy3-iscsi-setup/image5.png)
4. A hello **hálózati beállításai** lapon az **hálózati illesztőt**, DATA 0 automatikusan megtörténik az Ön. Mindegyik hálózati interfész állítja be alapértelmezett tooget IP-cím automatikusan (DHCP). Ezért egy IP-cím, alhálózati és az átjáró a rendszer automatikusan hozzárendeli (az IPv4 és IPv6).
   
    Tervezésekor toodeploy az eszköz (tooprovision blokktárolást) iSCSI-kiszolgálóként, azt javasoljuk, hogy tiltsa le a hello **IP-cím automatikus beszerzése** lehetőséget, majd a statikus IP-címek konfigurálásához.
   
    ![Hálózati beállítások lap](./media/storsimple-virtual-array-deploy3-iscsi-setup/image6.png)
   
    Ha egynél több hálózati adapter hello hello eszköz kiépítése során hozzáadott, konfigurálhatja őket itt. Vegye figyelembe, hogy beállíthat a hálózati adapter IPv4-alapú csak vagy mint IPv4 és IPv6. Csak IPv6-konfigurációk nem támogatottak.
5. DNS-kiszolgálók szükség, mert azok használata, amikor az eszköz a felhő tárolási szolgáltatók vagy tooresolve toocommunicate megkísérli az eszköz neve Ha erre van konfigurálva, mint a fájlkiszolgáló. A hello **hálózati beállításai** hello lapján **DNS-kiszolgálók**:
   
   1. Egy elsődleges és másodlagos DNS-kiszolgáló automatikusan megtörténik. Ha úgy dönt, tooconfigure statikus IP-címeket, DNS-kiszolgálót is megadhat. A magas rendelkezésre állás érdekében azt javasoljuk, hogy egy elsődleges és másodlagos DNS-kiszolgáló konfigurálása.
   2. Kattintson az **Alkalmaz** gombra. Ez alkalmazza, és hello hálózati beállításainak ellenőrzéséhez.
6. A hello **eszközbeállítások** lap:
   
   1. Rendeljen egy egyedi **neve** tooyour eszköz. Ez a név 1 – 15 karakterből állhat, és betűvel, számokat és kötőjeleket tartalmazhat.
   2. Hello kattintson **iSCSI server** ikon ![iSCSI kiszolgáló ikonja](./media/storsimple-virtual-array-deploy3-iscsi-setup/image7.png) a hello **típus** eszköz létrehozása. Az iSCSI-kiszolgáló lehetővé teszi a tooprovision blokktárolókhoz csatlakoznának.
   3. Adja meg, ha az eszköz toobe tartományhoz. Ha az eszköz egy iSCSI-kiszolgálót, majd a hello tartományhoz való csatlakozás nem kötelező megadni. Ha úgy dönt, toonot illesztési a iSCSI tooa tartományában, kattintson a **alkalmaz**, hello beállítások toobe alkalmazott várja meg, és folytassa a következő lépés toohello.
      
       Ha azt szeretné, hogy toojoin hello eszköz tooa tartomány. Adjon meg egy **tartománynév**, és kattintson a **alkalmaz**.
      
      > [!NOTE]
      > Ha az iSCSI server tooa tartományhoz való csatlakozás ügyeljen arra, hogy a virtuális tömb saját szervezeti egységet (OU) a Microsoft Azure Active Directory és a nem a csoportházirend-objektumok (GPO) olyan alkalmazott tooit.
      > 
      > 
   4. Megjelenik egy párbeszédpanel. Hello megadott formátumban adja meg a tartományi hitelesítő adatokat. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-virtual-array-deploy3-iscsi-setup/image15.png). hello tartományi hitelesítő adatokat fogja ellenőrizni. Egy hibaüzenet jelenik meg, ha hello hitelesítő adatok helytelenek.
      
       ![hitelesítő adatok](./media/storsimple-virtual-array-deploy3-iscsi-setup/image8.png)
   5. Kattintson az **Alkalmaz** gombra. Ez alkalmazza, és hello eszköz beállításainak ellenőrzéséhez.
7. A webes proxykiszolgáló (opcionális) konfigurálása. Bár a webproxy konfigurálása nem kötelező, vegye figyelembe, hogy ha olyan webproxyt használ, csak konfigurálhatja azt itt.
   
    ![webalkalmazás-proxy konfigurálása](./media/storsimple-virtual-array-deploy3-iscsi-setup/image9.png)
   
    A hello **webalkalmazás-proxy** lap:
   
   1. Adjon meg hello **webalkalmazás-proxy URL-címe** ebben a formátumban: *http://host-IP cím* vagy *FDQN:Port szám*. Vegye figyelembe, hogy HTTPS URL-címek nem támogatottak.
   2. Adja meg **hitelesítési** , **alapvető** vagy **nincs**.
   3. Ha hitelesítést használ, akkor tooprovide is egy **felhasználónév** és **jelszó**.
   4. Kattintson az **Alkalmaz** gombra. A ellenőrizni és hello konfigurált webes proxy-beállítások alkalmazásához.
8. (Opcionális) időbeállításait hello az eszköz, például az időzóna és hello elsődleges és másodlagos NTP-kiszolgáló. NTP-kiszolgáló szükség, mert az eszköz kell próbálta szinkronizálni az időt, hogy azt a felhőszolgáltatók hitelesíthető.
   
    ![Idő beállítása](./media/storsimple-virtual-array-deploy3-iscsi-setup/image10.png)
   
    A hello **időbeállítások** lap:
   
   1. Hello legördülő listából válassza ki a hello **időzóna** hello mely hello az eszköz üzembe helyezésének földrajzi helye alapján. az eszköz hello alapértelmezett időzónát a csendes-óceáni TÉLI. Az eszköz minden ütemezett művelethez ezt az időzónát használja.
   2. Adjon meg egy **elsődleges NTP-kiszolgáló** az eszközhöz, vagy fogadja el alapértékként hello time.windows.com. Győződjön meg arról, hogy a hálózat engedélyezi-e a datacenter toohello Internet az NTP-forgalom toopass.
   3. Megadhat egy **másodlagos NTP-kiszolgáló** az eszközhöz.
   4. Kattintson az **Alkalmaz** gombra. A ellenőrizni és hello beállított idő beállítások alkalmazásához.
9. Az eszköz hello felhő beállításainak konfigurálása. Ebben a lépésben hello helyi eszköz konfigurálásának befejezése, és majd hello eszköz regisztrálása a StorSimple Device Manager szolgáltatásban.
   
   1. Adja meg a hello **Szolgáltatásregisztrációs kulcs** portáltól **2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése** a [központi telepítése a StorSimple virtuális tömb - portál hello előkészítése](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
   2. Ha ez nem hello első eszköz, amely ezzel a szolgáltatással regisztrál, szüksége lesz a tooprovide hello **szolgáltatásadat-titkosítási kulcs**. Ezt a kulcsot nem kötelező megadni a hello szolgáltatás regisztrációs kulcs tooregister további eszközök hello StorSimple Device Manager szolgáltatást. További információkért tekintse meg túl[Get hello szolgáltatásadat-titkosítási kulcs](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) a helyi webes felhasználói felülete.
   3. Kattintson a **regisztrálása**. Ez hello eszköz újraindul. Előfordulhat, hogy szüksége toowait 2-3 percet hello eszköz regisztrálása sikeres volt. Hello eszköz újraindítása után megnyílik toohello bejelentkezési oldalán.
      
      ![Eszköz regisztrálása](./media/storsimple-virtual-array-deploy3-iscsi-setup/image11.png)
10. Visszatérési toohello Azure-portálon.
11. Keresse meg a toohello **eszközök** panel a szolgáltatás. Ha nagy mennyiségű erőforrást, kattintson a **összes erőforrás**, kattintson a szolgáltatás neve (keresse meg azt, ha szükséges), majd **eszközök**.
12. A hello **eszközök** panelen ellenőrizze, hogy hello eszköz sikeresen csatlakozott toohello szolgáltatás hello állapot megkeresésével. hello Eszközállapot kell **tooset kész mentése**.
    
    ![Eszköz regisztrálása](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png)

## <a name="step-2-configure-hello-device-as-iscsi-server"></a>2. lépés: Hello eszköz konfigurálása iSCSI-kiszolgálóként

Hajtsa végre a következő lépéseket az Azure portál toocomplete szükséges hello Eszközbeállítás hello hello.

#### <a name="tooconfigure-hello-device-as-iscsi-server"></a>tooconfigure hello eszköz iSCSI-kiszolgálóként

1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást, és keresse meg a túl**felügyeleti > eszközök**. A hello **eszközök** panelen, jelölje be hello eszköz újonnan létrehozott. Ez az eszköz akkor jelenik meg **tooset kész mentése**.
   
    ![Eszközök konfigurálása iSCSI-kiszolgálóként](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png) 
2. Kattintson a hello eszközt, és arról, hogy hello eszköz készen toosetup szalagcím üzenet jelenik.
   
    ![Eszközök konfigurálása iSCSI-kiszolgálóként](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis2m.png)  
3. Kattintson a **konfigurálása** hello eszköz parancssávon. Ezzel megnyílik hello **konfigurálása** panelen. A hello **konfigurálása** panelen a következő hello:
   
   * hello iSCSI kiszolgálónév automatikusan feltöltődik értékkel.
   * Ellenőrizze, hogy túl van-e állítva hello felhőalapú tárolás titkosításának**engedélyezve**. Ez biztosítja, hogy hello eszköz toohello felhőből küldött hello adatok titkosítva van.
   * Adjon meg egy 32 karakterből álló titkosítási kulcs, és jegyezze fel a jövőben kulcskezelés alkalmazásban.
   * Válassza ki a tárolási fiók toobe az eszközhöz használt. Ebben az előfizetésben, válassza ki egy meglévő tárfiókot használ, vagy kattintson **Hozzáadás** toochoose az-fiók egy másik előfizetést.
     
     ![Eszközök konfigurálása iSCSI-kiszolgálóként](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis4m.png)
4. Kattintson a **konfigurálása** hello iSCSI kiszolgáló toocomplete beállítását.
   
    ![Eszközök konfigurálása iSCSI-kiszolgálóként](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis5m.png) 
5. Értesítést fog kapni, hogy hello iSCSI-kiszolgáló létrehozása folyamatban van. Hello iSCSI kiszolgáló sikeres létrehozása után hello **eszközök** panel frissül, és megfelelő Eszközállapot hello **Online**.
   
    ![Eszközök konfigurálása iSCSI-kiszolgálóként](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis9m.png)

## <a name="step-3-add-a-volume"></a>3. lépés: Kötet hozzáadása

1. A hello **eszközök** panelen, jelölje be hello eszköz iSCSI-kiszolgálóként konfigurált. Kattintson a **...**  (másik lehetőségként kattintson a jobb gombbal a sorhoz) hello helyi menüből válassza ki a **kötet hozzáadása**. Is **+ kötet hozzáadása** hello parancs segítségével. Ezzel megnyílik hello **kötet hozzáadása** panelen.
   
    ![Kötet hozzáadása](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis10m.png)
2. A hello **kötet hozzáadása** panelen a következő hello:
   
   * A hello **kötetneve** mezőben adjon meg egy egyedi nevet a kötethez. hello neve 3 too127 karaktert tartalmazó karakterláncnak kell lennie.
   * A hello **típus** legördülő listában, adja meg, hogy toocreate egy **rétegzett** vagy **helyileg rögzített** kötet. Helyi garanciákat, kis késleltetést és magasabb teljesítményt igénylő munkaterheléseknél válasszon **helyileg rögzített** **kötet**. Minden más adathoz válasszon **rétegzett** **kötet**.
   * A hello **kapacitás** mezőben adja meg a hello hello kötet méretét. A rétegzett kötetek 500 GB és 5 TB között kell lennie, és egy helyileg rögzített kötet 50 GB-os és 500 GB között kell lennie.
     
     Egy helyileg rögzített kötet kiosztása, és biztosítja, hogy hello elsődleges adatok hello kötetén hello eszközön marad, és nem kerülnek toohello felhő.
     
     A rétegzett kötetek a hello ugyanakkor kiosztása. Ha rétegzett kötetet hoz létre, körülbelül 10 %-a hello terület hello helyi rétegen van kiépítve és 90 % hello terület hello felhőben regisztráltak-e. Például ha 1 TB-os kötet létesített, 100 GB hello helyi terület kellene lennie, és 900 GB lesz felhasználva a következőben hello felhő amikor hello adat szintet. Ez viszont azt jelenti, hogy elfogyott az összes hello helyi hello eszköz futtatásakor nem használhatók a rétegzett megosztás (mert hello 10 % csak akkor érhető el).
     
     ![Kötet hozzáadása](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis12.png)
   * Kattintson a **csatlakozó állomások**, válassza ki a hozzáférési vezérlési rekordot (ACR) megfelelő toohello iSCSI-kezdeményező szeretné, hogy tooconnect toothis kötet, és kattintson **válasszon**. <br><br> 
3. új csatlakoztatott gazdagépek esetén tooadd kattintson **új hozzáadása**, adjon meg egy nevet a hello állomás és az iSCSI minősített nevét (IQN), és kattintson **Hozzáadás**. Ha nem rendelkezik hello IQN, nyissa meg túl[hello függelék: megfigyelendő lekérése a Windows Server-állomás IQN-Nevének](#appendix-a-get-the-iqn-of-a-windows-server-host).
   
      ![Kötet hozzáadása](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis15m.png)
4. Ha végzett a kötet konfigurálása, kattintson a **OK**. A kötet jön létre a megadott hello beállításait, és megjelenik egy értesítés. Alapértelmezés szerint figyelése és a biztonsági mentés engedélyezve lesz hello kötet.
   
     ![Kötet hozzáadása](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis18m.png)
5. amely a kötet hello tooconfirm lett sikeresen létrehozva, lépjen toohello **kötetek** panelen. Meg kell jelenniük a felsorolt hello kötetet.
   
   ![Kötet hozzáadása](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis20m.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>4. lépés: Csatlakoztassa, inicializálja és formázza a kötetet

Hajtsa végre a hello következő lépések toomount, inicializálja és formázza a StorSimple-köteteket egy Windows Server-állomáson.

#### <a name="toomount-initialize-and-format-a-volume"></a>toomount, inicializálja és formázza a kötetet

1. Nyissa meg hello **iSCSI-kezdeményező** app hello megfelelő kiszolgálón.
2. A hello **iSCSI-kezdeményező tulajdonságai** ablakban a hello **felderítési** lapra, majd **kapu felderítése**.
   
    ![Fedezze fel a portálra](./media/storsimple-virtual-array-deploy3-iscsi-setup/image22.png)
3. A hello **tárolókapu felderítése** párbeszédpanelen adja meg az iSCSI-kompatibilis hálózati adaptert hello IP-címét, és kattintson **OK**.
   
    ![IP-cím](./media/storsimple-virtual-array-deploy3-iscsi-setup/image23.png)
4. A hello **iSCSI-kezdeményező tulajdonságai** ablakban a hello **célok** lapján keresse meg a hello **Felderített tárolók**. (Minden olyan kötetre lesz felderített target.) hello eszköz állapotúnak kell lennie **inaktív**.
   
    ![Felderített tárolók](./media/storsimple-virtual-array-deploy3-iscsi-setup/image24.png)
5. Jelöljön ki egy cél eszközt, és kattintson a **Connect**. Hello eszköz csatlakoztatása után hello kell állapotmódosítási túl**csatlakoztatva**. (Hello Microsoft iSCSI-kezdeményező használatával kapcsolatos további információkért lásd: [telepítése és konfigurálása a Microsoft iSCSI-kezdeményező][1].
   
    ![Válassza ki a céleszközt](./media/storsimple-virtual-array-deploy3-iscsi-setup/image25.png)
6. A Windows-gazdagépen, nyomja meg a hello Windows billentyű + X, és kattintson **futtatása**.
7. A hello **futtatása** párbeszédpanelen írja be **Diskmgmt.msc**. Kattintson a **OK**, és hello **Lemezkezelés** párbeszédpanel jelenik meg. hello jobb oldali ablaktáblán jelennek meg hello kötetek a gazdagépen.
8. A hello **Lemezkezelés** ablakban hello csatlakoztatott kötetek fog megjelenni a hello a következő ábrán látható módon. Kattintson a jobb gombbal a hello felderített kötetre (kattintson a hello lemez nevére), és kattintson a **Online**.
   
    ![Lemezkezelés](./media/storsimple-virtual-array-deploy3-iscsi-setup/image26.png)
9. Kattintson a jobb gombbal, és válassza ki **lemez inicializálása**.
   
    ![1 lemez inicializálása](./media/storsimple-virtual-array-deploy3-iscsi-setup/image27.png)
10. A hello párbeszédpanelen jelölje ki a hello lemez(ek) tooinitialize, és kattintson **OK**.
    
    ![2 lemez inicializálása](./media/storsimple-virtual-array-deploy3-iscsi-setup/image28.png)
11. hello új egyszerű kötet varázsló elindul. Válassza ki a lemez méretét, és kattintson a **következő**.
    
    ![Új kötet varázsló 1](./media/storsimple-virtual-array-deploy3-iscsi-setup/image29.png)
12. Meghajtó betűjele toohello kötet hozzárendelése, majd a **következő**.
    
    ![Új kötet varázsló 2. régiója](./media/storsimple-virtual-array-deploy3-iscsi-setup/image30.png)
13. Adja meg a hello paraméterek tooformat hello kötet. **Windows Server rendszeren csak az NTFS fájlrendszer esetén támogatott.** Hello lemezfoglalási egység mérete too64K beállítása. Adjon meg egy címkét a kötet. A név toobe azonos toohello kötet név a StorSimple virtuális tömb a megadott ajánlott. Kattintson a **Tovább** gombra.
    
    ![Új kötet varázsló 3](./media/storsimple-virtual-array-deploy3-iscsi-setup/image31.png)
14. Ellenőrizze a kötethez tartozó hello értékeket, és kattintson a **Befejezés**.
    
    ![Új kötet varázsló 4](./media/storsimple-virtual-array-deploy3-iscsi-setup/image32.png)
    
    hello kötetek frissítésként jelenik meg **Online** a hello **Lemezkezelés** lap.
    
    ![a kötetek online](./media/storsimple-virtual-array-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogy a toouse hello helyi webes felhasználói felület túl[felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-hello-iqn-of-a-windows-server-host"></a>A függelék: Get hello egy Windows Server-állomás IQN-Nevének

Hajtsa végre a hello a következő lépéseket tooget hello iSCSI minősített nevét (IQN) egy Windows Server 2012 rendszert futtató Windows-állomás.

#### <a name="tooget-hello-iqn-of-a-windows-host"></a>a Windows-állomás IQN-Nevének tooget hello

1. Indítsa el a hello Microsoft iSCSI-kezdeményezőt a Windows-gazdagépen.
2. A hello **iSCSI-kezdeményező tulajdonságai** ablakban a hello **konfigurációs** lapra, válassza ki, másolja hello karakterlánc hello **kezdeményező neve** mező.
   
    ![iSCSI-kezdeményező tulajdonságai](./media/storsimple-virtual-array-deploy3-iscsi-setup/image34.png)
3. Mentse ezt a karakterláncot.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



