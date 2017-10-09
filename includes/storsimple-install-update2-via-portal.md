<!--author=alkohli last changed: 02/06/17-->

#### <a name="tooinstall-an-update-from-hello-azure-portal"></a>tooinstall frissített hello Azure-portálon

1. Hello StorSimple szolgáltatás lapon jelölje be az eszközt. Keresse meg a túl**eszközök** > **karbantartási**.
2. Hello a hello lap alján, kattintson **frissítések keresése**. Egy feladat jön létre az elérhető frissítések tooscan. Értesítést kap hello feladat sikeresen befejeződik.
3. A hello **szoftverfrissítések** hello lap azonos lapon hello új szoftverfrissítések érhetők el. Javasoljuk, hogy hello kibocsátási megjegyzések tekintse át az eszközön egy frissítés alkalmazása előtt.
4. Hello a hello lap alján, kattintson **frissítések telepítése**, majd **OK**.
5. A hello **frissítések telepítése** párbeszédpanelen győződjön meg arról, hogy végrehajtotta-e hello ajánlásokat, majd válasszon **I fent követelmény hello ismertetése, és készen áll a tooupgrade az eszköz gyártója** hello jelölőnégyzetet, majd jelölt gombra.
   
    ![Megerősítő üzenet](./media/storsimple-install-update2-via-portal/InstallUpdate12_2M.png)
6. Megkezdődik az előfeltételek ellenőrzése. Ezekbe az ellenőrzésekbe a következők tartoznak:
   
   * **A tartományvezérlő állapotellenőrzést** , hogy mindkét hello eszközvezérlők tooverify kifogástalan és online.
   * **Hardver összetevő állapotellenőrzést** , hogy az összes hello hardverösszetevők a StorSimple eszköz tooverify kifogástalan.
   * **DATA 0 ellenőrzi** tooverify, hogy a DATA 0 engedélyezve van az eszközön. Ha ez az illesztőfelület nem engedélyezett, engedélyeznie kell, majd újra kell próbálkoznia.
   * **DATA 2 és a DATA 3 ellenőrzések** tooverify, hogy a DATA 2 és a DATA 3 hálózati adaptereket nem engedélyezettek. Ha ezek a kapcsolatok engedélyezve vannak, majd meg kell letiltja ezeket, majd próbálja tooupdate az eszközt. Ezt az ellenőrzést csak akkor végzi el a rendszer, ha egy GA szoftvert futtató eszközből frissít. A 0.1, 0.2 vagy 0.3 verziót futtató eszközöknek nincs szükségük erre az ellenőrzésre.
   * **Átjáró ellenőrzés** bármely eszközön futó verziója előzetes tooUpdate 1. Ez az ellenőrzés 1 frissítés előtti szoftvert futtató összes hello eszközön végbement, de nem sikerül, az átjáró is konfigurálva a hálózati illesztő DATA 0 kivételével hello eszközön.
     
     hello frissítés vonatkozik, ha az ellenőrzés sikeresen befejeződtek. Értesítést kap hello ellenőrzése folyamatban van.
     
     ![Ellenőrzés előtti értesítés](./media/storsimple-install-update2-via-portal/InstallUpdate12_3M.png)
     
     hello következő az hello ellenőrzése nem sikerült. Ellenőrizze, hogy mindkét hello eszközvezérlők kifogástalan és online-e. Szükség hello hardverösszetevők toocheck hello állapotát. Ebben a példában a Vezérlő 0 és a Vezérlő 1 összetevők igényelnek figyelmet. Ha saját maga által nem megoldja ezeket a problémákat, akkor esetleg toocontact Microsoft Support.
     
       ![Az ellenőrzések meghiúsultak](./media/storsimple-install-update2-via-portal/HCS_PreUpgradeChecksFailed-include.png)
7. Hello ellenőrzése sikeres elvégzése után létrejön egy frissítési feladatot. Értesítést kap hello frissítési feladat sikeresen létrehozva.
   
    ![Frissítési feladat létrehozása](./media/storsimple-install-update2-via-portal/InstallUpdate12_44M.png)
   
    hello frissítés alkalmazva lesz az eszközön.
    
8. toomonitor hello előrehaladását hello feladat frissítése, kattintson a **feladat megtekintése**. A hello **feladatok** lapon látható hello frissítése folyamatban van.
9. hello frissítés néhány óra toocomplete időbe telik. Válassza ki a hello feladat frissítése, és kattintson a **részletek** tooview hello bármikor hello feladat részletes adatait.
10. Hello feladat befejezése után nyissa meg a toohello **karbantartási** lapon, majd görgessen lefelé, túl**szoftverfrissítések**.

