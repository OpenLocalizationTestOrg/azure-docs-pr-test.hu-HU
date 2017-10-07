---
title: "StorSimple virtuális tömb fájlkiszolgálóként mentése aaaSet |} Microsoft Docs"
description: "A StorSimple virtuális tömb telepítési harmadik oktatóanyag fájlkiszolgálóként arra utasítja tooset be egy virtuális eszközt."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: f609f6ff-0927-48bb-a68a-6d8985d2fe34
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 89cade37980f342695c0adee42c4ade0e1d53d2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---set-up-as-file-server-via-azure-portal"></a>Központi telepítése a StorSimple virtuális tömb - beállítva fel fájlkiszolgálóként Azure-portálon
![](./media/storsimple-virtual-array-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Bevezetés
Ez a cikk ismerteti, hogyan tooperform a kezdeti telepítés regisztrálása a StorSimple fájlkiszolgáló, az eszköz beállításának befejezése hello, és hozzon létre és tooSMB megosztások csatlakozzon. Ez az hello utolsó cikkében hello sorozat szükséges az üzembehelyezési oktatóanyagok toocompletely egy fájl vagy iSCSI-kiszolgáló telepítése a virtuális tömbjét.

hello beállítása és konfigurációja folyamat körülbelül 10 percig toocomplete vehet igénybe. Ez a cikk hello információk csak a StorSimple virtuális tömb hello toohello telepítését vonatkoznak. A StorSimple 8000 sorozat eszközeire hello központi telepítése, lásd: [a StorSimple 8000 series eszköz 2. frissítést futtató üzembe helyezése](storsimple-deployment-walkthrough-u2.md).

## <a name="setup-prerequisites"></a>Telepítési előfeltételek
Előtt konfigurálja, és állítsa be a StorSimple virtuális tömb, győződjön meg arról, hogy:

* Egy virtuális tömb és a csatlakoztatott tooit, a részletes hello ellátta [kiépíteni a StorSimple virtuális tömb a Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) vagy [kiépíteni a StorSimple virtuális tömb VMware-ben](storsimple-virtual-array-deploy2-provision-vmware.md).
* Hogy hello szolgáltatás regisztrációs kulcsának toomanage StorSimple virtuális tömbök létrehozott hello StorSimple Device Manager szolgáltatásból. További információkért lásd: [2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) a StorSimple virtuális tömb.
* Ha ez hello második vagy virtuális tömb, amely egy meglévő StorSimple Device Manager szolgáltatással regisztrál, rendelkeznie kell hello szolgáltatásadat-titkosítási kulcs. Ezt a kulcsot hozott létre, ha hello első eszköz regisztrálása sikeres volt a szolgáltatással. Ha elvesztette ezt a kulcsot, lásd: [Get hello szolgáltatásadat-titkosítási kulcs](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) a StorSimple virtuális tömbhöz.

## <a name="step-by-step-setup"></a>Részletes beállítása
Lépésenként tooset a következő hello használata, és a StorSimple virtuális tömb beállítása.

## <a name="step-1-complete-hello-local-web-ui-setup-and-register-your-device"></a>1. lépés: Hello helyi webes felhasználói felület telepítés befejeződik, és regisztrálja az eszközt
#### <a name="toocomplete-hello-setup-and-register-hello-device"></a>toocomplete hello beállítása és hello-eszköz regisztrálása
1. Nyisson meg egy böngészőablakot, és csatlakozzon a toohello helyi webes felhasználói felület. Típus:
   
   `https://<ip-address of network interface>`
   
   Hello kapcsolati URL-cím hello előző lépésben feljegyzett használja. Arról, hogy hello webhely biztonsági tanúsítványával kapcsolatos problémára hibaüzenet jelenik meg. Kattintson a **továbbra is toothis weblap**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image2.png)
2. Bejelentkezés toohello webes felhasználói felülete, mint a virtuális tömb **StorSimpleAdmin**. Adja meg a hello eszköz rendszergazdai jelszava, amely módosította a 3. lépés: Start hello virtuális tömb a [kiépíteni a StorSimple virtuális tömb a Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) vagy [kiépíteni a StorSimple virtuális tömb VMware-ben](storsimple-virtual-array-deploy2-provision-vmware.md).
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image3.png)
3. A következő lépés az toohello **Home** lap. Ez a lap ismerteti hello különböző beállításokat szükséges tooconfigure és regisztrálása hello virtuális tömb hello StorSimple Device Manager szolgáltatással. Hello **hálózati beállításai**, **webalkalmazás-proxy beállításainak**, és **időbeállítások** megadása nem kötelező. csak a szükséges beállítás hello **eszközbeállítások** és **Felhőbeállítások**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image4.png)
4. A hello **hálózati beállításai** lapon az **hálózati illesztőt**, DATA 0 automatikusan megtörténik az Ön. Minden hálózati illesztő beállítása alapértelmezett tooget IP-cím alapján automatikusan (DHCP). Emiatt egy IP-cím, alhálózati és átjáró automatikusan rendelt (az IPv4 és IPv6).
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image5.png)
   
   Ha egynél több hálózati adapter hello hello eszköz kiépítése során hozzáadott, konfigurálhatja őket itt. Vegye figyelembe, hogy beállíthat a hálózati adapter IPv4-alapú csak vagy mint IPv4 és IPv6. Csak IPv6-konfigurációk nem támogatottak.
5. DNS-kiszolgálók szükség, mert azok amikor az eszköz kísérli meg a felhő tárolási szolgáltatók vagy tooresolve toocommunicate az eszköz által használt nevét, ha a fájl konfigurálva. A hello **hálózati beállításai** hello lapján **DNS-kiszolgálók**:
   
   1. Egy elsődleges és másodlagos DNS-kiszolgáló automatikusan megtörténik. Ha úgy dönt, tooconfigure statikus IP-címeket, DNS-kiszolgálót is megadhat. A magas rendelkezésre állás érdekében azt javasoljuk, hogy egy elsődleges és másodlagos DNS-kiszolgáló konfigurálása.
   2. Kattintson a **alkalmaz** tooapply és hello hálózati beállításainak ellenőrzéséhez.
6. A hello **eszközbeállítások** lap:
   
   1. Rendeljen egy egyedi **neve** tooyour eszköz. Ez a név 1 – 15 karakterből állhat, és betűvel, számokat és kötőjeleket tartalmazhat.
   2. Hello kattintson **fájlkiszolgáló** ikon ![](./media/storsimple-virtual-array-deploy3-fs-setup/image6.png) a hello **típus** eszköz létrehozása. Egy fájlkiszolgáló toocreate megosztott mappák lehetővé teszi.
   3. Mivel az eszköz egy fájlkiszolgálón, szüksége lesz a toojoin hello eszköz tooa tartomány. Adjon meg egy **tartománynév**.
   4. Kattintson az **Alkalmaz** gombra.
7. Megjelenik egy párbeszédpanel. Hello megadott formátumban adja meg a tartományi hitelesítő adatokat. Kattintson a pipa ikonra hello. hello tartományi hitelesítő adatok ellenőrzése. Egy hibaüzenetet lát Ha hello hitelesítő adatok helytelenek.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image7.png)
8. Kattintson az **Alkalmaz** gombra. Ez alkalmazza, és hello eszköz beállításainak ellenőrzéséhez.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image8.png)
   
   > [!NOTE]
   > Győződjön meg arról, hogy a virtuális tömb saját szervezeti egységben (OU) az Active Directory és nem a csoportházirend-objektumok (GPO) alkalmazott tooit vagy örökölt. Csoportházirend alkalmazások, például víruskereső szoftver is telepíthető hello StorSimple virtuális tömb. További szoftverek telepítése nem támogatott, és toodata sérülés vezethet. 
   > 
   > 
9. A webes proxykiszolgáló (opcionális) konfigurálása. Bár a webproxy konfigurálása nem kötelező, vegye figyelembe, hogy ha olyan webproxyt használ, csak konfigurálhatja azt itt.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image9.png)
   
   A hello **webalkalmazás-proxy** lap:
   
   1. Adjon meg hello **webalkalmazás-proxy URL-címe** ebben a formátumban: *http://&lt;állomás IP-cím vagy teljes Tartománynevet&gt;: portszámát*. Vegye figyelembe, hogy HTTPS URL-címek nem támogatottak.
   2. Adja meg **hitelesítési** , **alapvető** vagy **nincs**.
   3. Ha használ hitelesítést, konfigurálnia kell tooprovide egy **felhasználónév** és **jelszó**.
   4. Kattintson az **Alkalmaz** gombra. A ellenőrizni és hello konfigurált webes proxy-beállítások alkalmazásához.
10. (Opcionális) időbeállításait hello az eszköz, például az időzóna és hello elsődleges és másodlagos NTP-kiszolgáló. NTP-kiszolgáló szükség, mert az eszköz kell próbálta szinkronizálni az időt, hogy azt a felhőszolgáltatók hitelesíthető.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/image10.png)
    
    A hello **időbeállítások** lap:
    
    1. Hello legördülő listából válassza ki a hello **időzóna** hello mely hello az eszköz üzembe helyezésének földrajzi helye alapján. az eszköz hello alapértelmezett időzónát a csendes-óceáni TÉLI. Az eszköz minden ütemezett művelethez ezt az időzónát használja.
    2. Adjon meg egy **elsődleges NTP-kiszolgáló** az eszközhöz, vagy fogadja el alapértékként hello time.windows.com. Győződjön meg arról, hogy a hálózat engedélyezi-e a datacenter toohello Internet az NTP-forgalom toopass.
    3. Megadhat egy **másodlagos NTP-kiszolgáló** az eszközhöz.
    4. Kattintson az **Alkalmaz** gombra. A ellenőrizni és hello beállított idő beállítások alkalmazásához.
11. Az eszköz hello felhő beállításainak konfigurálása. Ebben a lépésben hello helyi eszköz konfigurálásának befejezése, és majd hello eszköz regisztrálása a StorSimple Device Manager szolgáltatásban.
    
    1. Adja meg a hello **Szolgáltatásregisztrációs kulcs** portáltól [2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) a StorSimple virtuális tömb.
    2. Ha ez az első eszköz, a szolgáltatás regisztrálása, választhat a hello **szolgáltatásadat-titkosítási kulcs**. Másolja ki ezt a kulcsot, és mentse egy biztonságos helyre. Ezt a kulcsot nem kötelező megadni a hello szolgáltatás regisztrációs kulcs tooregister további eszközök hello StorSimple Device Manager szolgáltatást. 
       
       Ha ez nem hello első eszköz, amely ezzel a szolgáltatással regisztrál, szüksége lesz a tooprovide hello szolgáltatásadat-titkosítási kulcs. További információkért tekintse meg a tooget hello [szolgáltatásadat-titkosítási kulcs](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) a helyi webes felhasználói felülete.
    3. Kattintson a **regisztrálása**. Ez hello eszköz újraindul. Előfordulhat, hogy szüksége toowait 2-3 percet hello eszköz regisztrálása sikeres volt. Hello eszköz újraindítása után megnyílik toohello bejelentkezési oldalán.
       
       ![](./media/storsimple-virtual-array-deploy3-fs-setup/image13.png)
12. Visszatérési toohello Azure-portálon. Nyissa meg túl**összes erőforrás**, keressen rá a StorSimple eszköz Manager szolgáltatást.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/searchdevicemanagerservice1.png) 
13. A szűrt hello listán, válassza ki a StorSimple Device Manager szolgáltatást, majd lépjen túl**felügyeleti > eszközök**. A hello **eszközök** panelen ellenőrizze, hogy hello eszköz sikeresen csatlakozott toohello szolgáltatás és hello állapot **tooset kész mentése**.
    
    ![Fájlkiszolgáló konfigurálása](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png)

## <a name="step-2-configure-hello-device-as-file-server"></a>2. lépés: A fájlkiszolgáló hello eszközök konfigurálása
Hajtsa végre a következő lépéseket a hello hello [Azure-portálon](https://portal.azure.com/) toocomplete hello Eszközbeállítás szükséges.

#### <a name="tooconfigure-hello-device-as-file-server"></a>fájlkiszolgáló tooconfigure hello eszköz
1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást, és keresse meg a túl **felügyeleti > eszközök**. A hello **eszközök** panelen, jelölje be hello eszköz újonnan létrehozott. Ez az eszköz akkor jelenik meg **tooset kész mentése**.
   
   ![Fájlkiszolgáló konfigurálása](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png) 
2. Kattintson a hello eszközt, és arról, hogy hello eszköz készen toosetup szalagcím üzenet jelenik.
   
    ![Fájlkiszolgáló konfigurálása](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs3m.png)
3. Kattintson a **konfigurálása** hello parancssávon. Ezzel megnyílik hello **konfigurálása** panelen. A hello **konfigurálása** panelen a következő hello:
   
    1. a fájlkiszolgáló nevének hello automatikusan feltöltődik értékkel.
    
    2. Ellenőrizze, hogy túl van-e állítva hello felhőalapú tárolás titkosításának**engedélyezve**. Ez minden hello az elküldött adatokat toohello felhő titkosítja. 
    
    3. A 256 bites AES kulccsal hello a felhasználó által definiált titkosítási kulccsal. Adjon meg egy 32 karakter hosszúságú kulcs, és majd még egyszer hello kulcs tooconfirm azt. Rekord hello kulcs kulcskezelés alkalmazásban későbbi felhasználás céljából.
    
    4. Kattintson a **kötelező beállítások konfigurálása** toospecify tárfiók toobe az eszközhöz használt hitelesítő adatok. Kattintson a **új hozzáadása** nincs tárfiók hitelesítő adatainak konfigurálva vannak. **Győződjön meg arról, hogy hello tárfiókot használja blokk blobokat támogat. Nem támogatja a lapblobokat.** További információ a [blokkolja a blobokat és lapblobokat](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).
   
    ![Fájlkiszolgáló konfigurálása](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs6m.png) 
4. A hello **adja hozzá a tárfiók hitelesítő adatainak** panelen a következő hello: 

    1. A jelenlegi előfizetés válassza, ha hello tárfiók a hello ugyanahhoz az előfizetéshez hello szolgáltatást. Adja meg, hogy más hello tárolási fiók hello szolgáltatás előfizetésének kívül esik. 
    
    2. Hello legördülő listából válassza ki a meglévő tárfiókot. 
    
    3. hello hely adatok automatikusan kitöltődnek megadott hello alapján tárfiók. 
    
    4. Engedélyezi az SSL tooensure hello eszköz és hello felhő között a biztonságos hálózati kommunikációs csatornát.
    
    5. Kattintson a **Hozzáadás** tooadd a tárolási fiók hitelesítő adatot. 
   
        ![Fájlkiszolgáló konfigurálása](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs8m.png)

5. Hello tárolási fiók hitelesítő adatait sikeres létrehozását követően hello **konfigurálása** frissíti a panel toodisplay hello megadott tárfiók hitelesítő adatait. Kattintson a **Configure** (Konfigurálás) elemre.
   
   ![Fájlkiszolgáló konfigurálása](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs11m.png)
   
   Megjelenik egy, a fájl kiszolgáló létrehozása folyamatban van. Ha hello fájlkiszolgáló sikeresen létrejött, értesítést fog kapni.
   
   ![Fájlkiszolgáló konfigurálása](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs13m.png)
   
   hello Eszközállapot is megváltozik túl**Online**.
   
   ![Fájlkiszolgáló konfigurálása](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs14m.png)
   
   A megosztás tooadd lépne.

## <a name="step-3-add-a-share"></a>3. lépés: A Megosztás hozzáadása
Hajtsa végre a következő lépéseket a hello hello [Azure-portálon](https://portal.azure.com/) toocreate egy megosztást.

#### <a name="toocreate-a-share"></a>a megosztás toocreate
1. Válassza ki hello fájl server eszközt lépést megelőző hello konfigurált, és kattintson a **...**  (vagy kattintson a jobb gombbal). Hello helyi menü, válassza ki a **Hozzáadás megosztás**. Másik lehetőségként kattinthat **+ adja hozzá a megosztás** hello eszköz parancssávon.
   
   ![Megosztás hozzáadása](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs15m.png)
2. Adja meg a következő beállításokat hello:

    1. Egy egyedi nevet a megosztáshoz. hello neve 3 too127 karaktert tartalmazó karakterláncnak kell lennie.
    
    2. Egy nem kötelező **leírás** hello megosztás. hello leírás azonosításához hello fájlmegosztás-tulajdonosok.
    
    3. A **típus** hello megosztás. hello típus lehet **rétegzett** vagy **helyileg rögzített**, rétegzett alatt az alapértelmezett hello. Helyi garanciákat, kis késleltetést és magasabb teljesítményt igénylő munkaterhelésekhez, válassza ki a **helyileg rögzített** megosztani. Minden más adathoz válasszon egy **rétegzett** megosztani.
    A helyileg rögzített megosztás kiosztása, és biztosítja, hogy az elsődleges adatok hello hello megosztáson helyi toohello eszköz marad, és nem kerülnek toohello felhő. A rétegzett megosztás a hello ugyanakkor kiosztása. Egy rétegzett megosztás létrehozásakor 10 % hello terület hello helyi rétegen ki van építve, és 90 % hello terület hello felhőben lett kiépítve. Például ha 1 TB-os kötet létesített, 100 GB hello helyi terület kellene lennie, és 900 GB lesz felhasználva a következőben hello felhő amikor hello adat szintet. Ez viszont azt jelenti, hogy minden hello helyi tárhely fogyjon hello eszközön, ha nem használhatók egy rétegzett megosztást.
   
    4. A hello **értékre alapértelmezett teljes körű engedélyekkel** mezőbe hello engedélyek toohello felhasználói vagy fér hozzá a megosztás hello csoport hozzárendelése. Adja meg a hello felhasználó vagy felhasználói csoport hello hello nevét  *john@contoso.com*  formátumban. Javasoljuk, hogy használjon egy felhasználói csoport (helyett egy-egy felhasználóhoz) tooallow rendszergazdai jogosultságokkal tooaccess megosztást. Miután itt hello engedélyek hozzárendelt, használhatja a Fájlkezelőben toomodify ezeket az engedélyeket.
   
    5. Kattintson a **Hozzáadás** toocreate hello megosztást. 
    
        ![Megosztás hozzáadása](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs18m.png)
   
        Értesítést kap, hogy hello fájlmegosztás létrehozása folyamatban van.
   
        ![Megosztás hozzáadása](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs19m.png)
   
    A hello hello megosztás létrehozása után megadott beállításokat, hello **megosztások** panel tooreflect hello új megosztás frissíteni fogja. Alapértelmezés szerint figyelés és a biztonsági mentés engedélyezettek hello megosztás.
   
    ![Megosztás hozzáadása](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs22m.png)

## <a name="step-4-connect-toohello-share"></a>4. lépés: Csatlakozás toohello megosztás
Most kell tooconnect tooone vagy hello előző lépésben létrehozott további megosztások. Hajtsa végre ezeket a lépéseket a Windows Server-gazdagép csatlakozik tooyour StorSimple virtuális tömb.

#### <a name="tooconnect-toohello-share"></a>tooconnect toohello megosztás
1. Nyomja le az ![](./media/storsimple-virtual-array-deploy3-fs-setup/image22.png) + R. Hello Futtatás ablakba, adja meg a hello *&#92; &#92;&lt; a fájlkiszolgáló nevének&gt;*  hello elérési útjaként, cseréje *fájlkiszolgáló nevéhez* hello eszközzel nevet adott meg hozzárendelt tooyour fájlkiszolgálóhoz. Kattintson az **OK** gombra.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image23.png)
2. Ekkor megnyílik a Fájlkezelőben fel. Képes toosee hello megosztások, mappák létrehozott kell. Válassza ki, és kattintson duplán egy közös (mappa) tooview hello tartalmat.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image24.png)
3. Most hozzáadása toothese fájlmegosztásokat, és készítsen biztonsági mentést.

## <a name="next-steps"></a>Következő lépések
Ismerje meg, hogy a toouse hello helyi webes felhasználói felület túl[felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).

