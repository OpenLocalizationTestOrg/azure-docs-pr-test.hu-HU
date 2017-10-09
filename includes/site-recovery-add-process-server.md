1. Indítsa el az Azure Site Recovery UnifiedSetup.exe hello
2. A **megkezdése előtt**, jelölje be **további folyamat kiszolgálók tooscale ki a központi telepítés hozzáadása**.

  ![Folyamatkiszolgáló hozzáadása](./media/site-recovery-add-process-server/ps-page-1.png)

3. A **konfigurációs kiszolgáló adatait**, adja meg a konfigurációs kiszolgáló hello hello IP-címét, és hello jelszót.

  ![2. folyamatkiszolgáló hozzáadása](./media/site-recovery-add-process-server/ps-page-2.png)
4. A **Internetbeállítások**, adja meg, hogyan hello konfigurációs kiszolgálón futó szolgáltató az internetre csatlakozik a Site Recovery tooAzure hello hello Internet.

  ![3. folyamatkiszolgáló hozzáadása](./media/site-recovery-add-process-server/ps-page-3.png)

   * Ha a jelenleg be van állítva a hello gépen hello proxy tooconnect, jelölje be **csatlakozás meglévő proxybeállításokkal**.
   * Ha közvetlenül hello szolgáltató tooconnect, jelölje be **kapcsolódás proxy nélkül közvetlenül**.
   * Ha a hello meglévő proxy hitelesítést igényel, vagy ha hello szolgáltatói kapcsolat toouse egyéni proxyt szeretne, válassza ki a **kapcsolódás egyéni proxybeállításokkal**.

     * Ha egyéni proxyt használ, akkor toospecify hello cím, port és a hitelesítő adatok.
     * Ha proxyt használ, érdemes már engedélyezte a toohello szolgáltatás URL-címek.

5. A **szükséges előfeltételek ellenőrzése**, a telepítő elindítja a jelölőnégyzet toomake meg arról, hogy a telepítés futtatható-e. Ha figyelmeztetés jelenik meg a vonatkozó hello **globális időhöz sync ellenőrzése**, győződjön meg arról, hogy hello idő hello rendszerórája (**dátum és idő** beállítások) van hello ugyanaz, mint hello időzóna.

     ![4. folyamatkiszolgáló hozzáadása](./media/site-recovery-add-process-server/ps-page-4.png)

6. A **környezet részletei**, adja meg, hogy tooreplicate VMware virtuális gépek fog. Ha igen, a telepítő ellenőrzi, hogy a PowerCLI 6.0 telepítve van-e.

     ![5. folyamatkiszolgáló hozzáadása](./media/site-recovery-add-process-server/ps-page-5.png)

7. A **helyre telepítse**, jelölje be, ha szeretné, hogy tooinstall hello bináris fájljait és hello gyorsítótárban tárolja. hello meghajtó legalább 5 GB szabad lemezterülettel kell rendelkeznie, de azt javasoljuk, hogy a gyorsítótárazáshoz használt lemezen legalább 600 GB szabad lemezterület.
     ![5. folyamatkiszolgáló hozzáadása](./media/site-recovery-add-process-server/ps-page-6.png)

8. A **Hálózatválasztás**, adja meg, melyik hello konfigurációs kiszolgáló küld hello figyelő (hálózati adapter és SSL-port), és kap replikációs adatokat. Port 9443 hello portot küldését és fogadását a replikációs forgalom használja, de módosíthatja a port száma toosuit a környezet követelményeinek. Továbbá toohello port 9443 azt is nyissa meg a web server tooorchestrate replikációs műveletek által használt 443-as porton. A replikációs forgalom küldésére és fogadására ne használja a 443-as portot.

     ![6. folyamatkiszolgáló hozzáadása](./media/site-recovery-add-process-server/ps-page-7.png)
9. A **összegzés**, tekintse át a hello információkat, majd kattintson **telepítése**. A telepítés után a rendszer létrehoz egy hozzáférési kódot. Erre szüksége lesz, amikor engedélyezi a replikálást, ezért másolja le, és tárolja biztonságos helyen.

     ![7. folyamatkiszolgáló hozzáadása](./media/site-recovery-add-process-server/ps-page-8.png)
