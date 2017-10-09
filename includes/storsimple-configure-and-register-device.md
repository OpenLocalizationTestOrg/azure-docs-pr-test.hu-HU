<!--author=alkohli last changed: 12/01/15-->


#### <a name="tooconfigure-and-register-hello-device"></a>hello eszköz tooconfigure és regisztrálása
1. Hozzáférés hello Windows PowerShell felületet a StorSimple eszköz soros konzoljához a. Lásd: [PuTTY használata tooconnect toohello eszköz soros konzoljához](#use-putty-to-connect-to-the-device-serial-console) utasításokat. **Lehet, hogy toofollow hello eljárás pontosan vagy nem fog tudni tooaccess hello konzol.**
2. A megnyitott munkamenetben hello nyomja le az adjon meg egy idő tooget egy parancssori ablakot. 
3. Rákérdezéses toochoose hello nyelvi, hogy milyen tooset az eszközhöz lesz. Adja meg a hello nyelv, és nyomja le az ENTER billentyűt. 
   
    ![StorSimple-eszköz konfigurálása és regisztrálása, 1](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice1-include.png)
4. Hello soros konzol menüben megjelenő válassza a 1. lehetőség toolog a teljes hozzáféréssel. 
   
    ![StorSimple-eszköz regisztrálása, 2](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice2-include.png)
   
     Hajtsa végre a lépéseket 5 – 12 tooconfigure hello minimálisan szükséges hálózati beállításokat az eszközhöz. **Ezeket a konfigurációs lépéseket kell toobe hello hello eszköz aktív vezérlőjén végre.** hello soros konzol menü hello vezérlő állapotát a szalagcím üdvözlőüzenetére jelzi. Ha korábban nem kapcsolódott a toohello aktív vezérlő, válassza le, és csatlakoztassa a toohello aktív vezérlő.
5. Hello parancssorba írja be a jelszavát. hello eszköz alapértelmezett jelszava: **jelszó1**.
6. Írja be a következő parancs hello:
   
     `Invoke-HcsSetupWizard` 
7. A telepítő varázsló jelenik meg toohelp hello hello eszköz hálózati beállításainak konfigurálása. Adja meg a következő információ hello hello: 
   
   * Hello DATA 0 hálózati adapterén IP-címe
   * Alhálózati maszk
   * Átjáró
   * Az elsődleges DNS-kiszolgáló IP-címe
   * Az elsődleges NTP-kiszolgáló IP-címe
     
     > [!NOTE]
     > Előfordulhat, hogy toowait hello alhálózati maszk és hello DNS beállítások toobe alkalmazása néhány percig. Ha a "hello eszköz nem áll készen." hibaüzenet, ellenőrizze hello fizikai hálózati kapcsolatot hello DATA 0 hálózati adapterén az aktív vezérlő.
     > 
     > 
8. (Nem kötelező) konfigurálja a webproxy-kiszolgálót. Bár a webproxy konfigurálása nem kötelező, **vegye figyelembe, hogy ha webproxyt használ, azt csak itt tudja beállítani**. További információ: túl[az eszköz webalkalmazás-proxy konfigurálása](../articles/storsimple/storsimple-configure-web-proxy.md). Ha probléma merül fel ezzel a lépéssel, tekintse meg az útmutató a tootroubleshooting [webproxy konfigurálásakor felmerülő hibák](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-during-the-optional-web-proxy-settings).

     > [!NOTE]
     > Minden alkalommal tooexit hello beállítása varázsló, is használja a Ctrl + C. A parancs kiadása előtt alkalmazott beállításokat megőrzi a rendszer.

1. Biztonsági okokból hello eszköz rendszergazdai jelszava hello első munkamenet végeztével lejár, és toochange kell azt a további munkamenetekhez. Amikor a rendszer erre kéri, adjon meg az eszközhöz egy rendszergazdai jelszót. Az eszköz rendszergazdai jelszavának 8–15 karakter hosszúságúnak kell lennie. hello jelszónak kisbetűk, nagybetűk, számok és speciális karakterek kombinációjából kell állnia.
2. hello StorSimple Snapshot Manager jelszavát is itt állíthatja be. Ezt a jelszót az eszköz StorSimple Snapshot Manager szolgáltatást futtató Windows-gazdagépen való hitelesítéséhez használhatja. Amikor a rendszer kéri, adjon meg egy 14 too15 jelszó. hello jelszóban közül hármat hello alábbi:, kisbetű, nagybetű, numerikus és speciális karaktereket. 
   
   ![StorSimple-eszköz regisztrálása, 4](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice4-include.png)
   
   Alaphelyzetbe állíthatja a StorSimple Snapshot Manager jelszavát hello hello StorSimple Manager szolgáltatási felületén. A részletes lépéseket lásd túl[hello StorSimple-jelszavak hello StorSimple Manager szolgáltatással módosítása](../articles/storsimple/storsimple-change-passwords.md).
   
   tootroubleshoot merül fel ezzel a lépéssel, tekintse meg a tootroubleshooting útmutatást [kapcsolatos hibák toopasswords](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords).
3. hello hello telepítővarázsló utolsó lépésként regisztrálja az eszközt a StorSimple Manager szolgáltatás hello. Ehhez szüksége lesz a 2. lépésben beszerzett hello Szolgáltatásregisztrációs kulcs. Hello regisztrációs kulcsot fogja tartalmazni, miután szükséges toowait 2-3 percet hello eszköz regisztrálva van-e.
   
   tootroubleshoot minden lehetséges eszközregisztrációs hibák, tekintse meg a túl[eszközök regisztrációja során hibák](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-during-device-registration). Részletes hibaelhárítási útmutatás túl[részletes hibaelhárítási példa](../articles/storsimple/storsimple-troubleshoot-deployment.md#step-by-step-storsimple-troubleshooting-example).
4. Hello eszköz regisztrálása után megjelenik egy szolgáltatásadat-titkosítási kulcs. Másolja ki ezt a kulcsot, és mentse egy biztonságos helyre.
   
   > [!WARNING]
   > Ezt a kulcsot kötelező megadni a hello szolgáltatás regisztrációs kulcs tooregister további eszközöket a StorSimple Manager szolgáltatás hello lesz. Tekintse meg a túl[StorSimple biztonsági](../articles/storsimple/storsimple-security.md) ezt a kulcsot további információt.
   > 
   > 
   
    ![StorSimple-eszköz regisztrálása, 6](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice6-include.png)
   
    toocopy hello szöveg hello soros konzol ablakából, egyszerűen jelölje ki a hello szöveget. Ezután meg kell tudni toopaste hello vágólapra vagy bármilyen szövegszerkesztővel azt. Ne használja a Ctrl + C toocopy hello szolgáltatásadat-titkosítási kulcs. A Ctrl + C miatt meg tooexit hello beállítása varázsló. Ennek eredményeképpen hello eszköz rendszergazdai jelszava és hello StorSimple Snapshot Manager jelszava nem változik, és hello eszköz visszaállítja toohello alapértelmezett jelszót.
5. Kilépés hello soros konzolon.
6. Térjen vissza a klasszikus Azure portálon toohello, és végezze el az alábbi lépésekkel hello:
   
   1. Kattintson duplán a StorSimple Manager szolgáltatás tooaccess hello **gyors üzembe helyezés** lap.
   2. Kattintson a **Csatlakoztatott eszközök megtekintése** lehetőségre.
   3. A hello **eszközök** lapon, győződjön meg arról, hogy hello eszköz sikeresen csatlakozott toohello szolgáltatás hello állapot megkeresésével. hello Eszközállapot kell **Online**. Ha hello eszköz állapota **Offline**, néhány perc alatt az online hello eszköz toocome várja.
   
   ![A StorSimple Eszközök lapja](./media/storsimple-configure-and-register-device/HCS_DevicesPageM-include.png) 
   
   > [!IMPORTANT]
   > Hello után eszköz online állapotban, csatlakoztassa, amely a hello lépés elején kihúzott hello hálózati kábeleket.
   > 
   > 

Miután hello eszköz regisztrálása sikeres volt, és mégsem kerül online állapotba, futtathatja hello `Test-HcsmConnection -Verbose` , amely hálózati kapcsolatot hello tooensure állapota kifogástalan. Hello Ez a parancsmag használatának részletes látogasson túl[Test-HcsmConnection parancsmag használatának ismertetésében](https://technet.microsoft.com/library/dn715782.aspx).

![Videó elérhető](./media/storsimple-configure-and-register-device/Video_icon.png)**Videó elérhető**

videó toowatch hogyan tooconfigure és regisztrálja az eszközt a StorSimple, a Windows PowerShell segítségével kattintson [Itt](https://azure.microsoft.com/documentation/videos/initialize-the-storsimple-appliance/).

