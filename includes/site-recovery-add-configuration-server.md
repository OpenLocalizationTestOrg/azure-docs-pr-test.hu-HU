1. Futtassa a hello az egységes telepítő telepítőfájlt.
2. A **előkészületek**, jelölje be **hello konfigurációs kiszolgáló és a folyamatkiszolgáló telepítése**.

    ![Előkészületek](./media/site-recovery-add-configuration-server/combined-wiz1.png)

3. A **harmadik féltől származó szoftverek licenc**, kattintson a **elfogadom** toodownload és MySQL telepítése.

    ![Külső gyártótól származó szoftverek](./media/site-recovery-add-configuration-server/combined-wiz2.png)
4. A **regisztrációs**, jelölje be a hello tárolójából letöltött hello regisztrációs kulcsot.

    ![Regisztráció](./media/site-recovery-add-configuration-server/combined-wiz3.png)
5. A **Internetbeállítások**, adja meg, hogyan hello konfigurációs kiszolgálón futó Provider az internetre csatlakozik a Site Recovery tooAzure hello hello Internet.

   a. Ha a jelenleg be van állítva a hello gépen hello proxy tooconnect, jelölje be **csatlakozzon a Site Recovery proxykiszolgálóval tooAzure**.

   b. Ha közvetlenül hello szolgáltató tooconnect, jelölje be **csatlakozzon közvetlenül a Site Recovery tooAzure proxykiszolgáló nélkül**.

   c. Ha a hello meglévő proxy hitelesítést igényel, vagy ha hello szolgáltatói kapcsolat toouse egyéni proxyt szeretne, válassza ki a **kapcsolódás egyéni proxybeállításokkal**.

     * Ha egyéni proxyt használ, akkor toospecify hello cím, port és a hitelesítő adatok.
     * Ha a proxy használata esetén kell már engedélyezte a hello URL-címeket a [Előfeltételek](#prerequisites).

     ![Tűzfal](./media/site-recovery-add-configuration-server/combined-wiz4.png)
6. A **szükséges előfeltételek ellenőrzése**, a telepítő elindítja a jelölőnégyzet toomake meg arról, hogy a telepítés futtatható-e. Ha figyelmeztetés jelenik meg a vonatkozó hello **globális időhöz sync ellenőrzése**, győződjön meg arról, hogy hello idő hello rendszerórája (**dátum és idő** beállítások) van hello ugyanaz, mint hello időzóna.

    ![Előfeltételek](./media/site-recovery-add-configuration-server/combined-wiz5.png)
7. A **MySQL konfigurációs**, toohello MySQL server-példány telepítve van a naplózás hitelesítő adatainak létrehozása.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz6.png)
8. A **környezet részletei**, adja meg, hogy tooreplicate VMware virtuális gépek fog. Ha, a telepítő ellenőrzi, hogy telepítve van-e a PowerCLI 6.0.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz7.png)

9. A **helyre telepítse**, jelölje be, ha szeretné, hogy tooinstall hello bináris fájljait és hello gyorsítótárban tárolja. hello meghajtó legalább 5 GB szabad lemezterülettel kell rendelkeznie, de azt javasoljuk, hogy a gyorsítótárazáshoz használt lemezen legalább 600 GB szabad lemezterület.

    ![Telepítés helye](./media/site-recovery-add-configuration-server/combined-wiz8.png)
10. A **Hálózatválasztás**, adja meg, melyik hello konfigurációs kiszolgáló küld hello figyelő (hálózati adapter és SSL-port), és kap replikációs adatokat. Port 9443 hello portot küldését és fogadását a replikációs forgalom használja, de módosíthatja a port száma toosuit a környezet követelményeinek. Továbbá toohello port 9443 azt is nyissa meg a web server tooorchestrate replikációs műveletek által használt 443-as porton. Ne használja a 443-as porton és-replikációs forgalom fogadására.

    ![Hálózat kiválasztása](./media/site-recovery-add-configuration-server/combined-wiz9.png)


11. A **összegzés**, tekintse át a hello információkat, majd kattintson **telepítése**. A telepítés után a rendszer létrehoz egy hozzáférési kódot. Erre szüksége lesz, amikor engedélyezi a replikálást, ezért másolja le, és tárolja biztonságos helyen.

    ![Összefoglalás](./media/site-recovery-add-configuration-server/combined-wiz10.png)

Regisztráció befejezése után hello kiszolgáló megjelenik a hello **beállítások** > **kiszolgálók** panel hello tárolóban lévő állapottal.
