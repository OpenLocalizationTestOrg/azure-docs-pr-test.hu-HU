<!--author=alkohli last changed: 02/22/2016-->


### <a name="tooconfigure-and-register-hello-device"></a>hello eszköz tooconfigure és regisztrálása
1. Hozzáférés hello Windows PowerShell felületet a StorSimple eszköz soros konzoljához a. Lásd: [PuTTY használata tooconnect toohello eszköz soros konzoljához](#use-putty-to-connect-to-the-device-serial-console) utasításokat. **Lehet, hogy toofollow hello eljárás pontosan vagy nem fog tudni tooaccess hello konzol.**
2. A megnyitott munkamenetben hello nyomja le az adjon meg egy idő tooget egy parancssori ablakot. 
3. Rákérdezéses toochoose hello nyelvi, hogy milyen tooset az eszközhöz lesz. Adja meg a hello nyelv, és nyomja le az ENTER billentyűt. 
   
    ![StorSimple-eszköz konfigurálása és regisztrálása, 1](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice1-U1-include.png)
4. Hello soros konzol menüben megjelenő válassza a 1. lehetőség toolog a teljes hozzáféréssel. 
   
    ![StorSimple-eszköz regisztrálása, 2](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice2_U1-include.png)
   
     Hajtsa végre a lépéseket 5 – 12 tooconfigure hello minimálisan szükséges hálózati beállításokat az eszközhöz. **Ezeket a konfigurációs lépéseket kell toobe hello hello eszköz aktív vezérlőjén végre.** hello soros konzol menü hello vezérlő állapotát a szalagcím üdvözlőüzenetére jelzi. Ha korábban nem kapcsolódott a toohello aktív vezérlő, válassza le, és csatlakoztassa a toohello aktív vezérlő.
5. Hello parancssorba írja be a jelszavát. hello eszköz alapértelmezett jelszava: **jelszó1**.
6. A következő parancs típusa hello: `Invoke-HcsSetupWizard`. 
7. A telepítő varázsló jelenik meg toohelp hello hello eszköz hálózati beállításainak konfigurálása. Adja meg a következő információ hello hello: 
   
   * Hello DATA 0 hálózati adapterén IP-címe
   * Alhálózati maszk
   * Átjáró
   * Az elsődleges DNS-kiszolgáló IP-címe
     
        Vegye figyelembe, hogy hello rendszer hello folyamat minden lépése után érvényesíti a hálózati beállításokat.
     
     > [!NOTE]
     > Előfordulhat, hogy toowait hello alhálózati maszk és hello DNS beállítások toobe alkalmazása néhány percig. Ha a "Jelölőnégyzet hello hálózati kapcsolat tooData 0" hiba jelenik meg, ellenőrizze az aktív vezérlő hello DATA 0 hálózati adapterén hello fizikai hálózati kapcsolatot.
     > 
     > 
8. (Nem kötelező) konfigurálja a webproxy-kiszolgálót. Bár a webproxy konfigurálása nem kötelező, **vegye figyelembe, hogy ha webproxyt használ, azt csak itt tudja beállítani**. További információ: túl[az eszköz webalkalmazás-proxy konfigurálása](../articles/storsimple/storsimple-configure-web-proxy.md).
9. Állítson be egy elsődleges NTP-kiszolgálót az eszközhöz. Az NTP-kiszolgálókra azért van szükség, mert az eszköznek szinkronizálnia kell az időt ahhoz, hogy a felhőszolgáltatókkal hitelesítést végezhessen. Győződjön meg arról, hogy a hálózat engedélyezi-e a datacenter toohello Internet az NTP-forgalom toopass. Ha ez nem lehetséges, akkor adjon meg egy belső NTP-kiszolgálót. 
10. Biztonsági okokból hello eszköz rendszergazdai jelszava hello első munkamenet végeztével lejár, és szüksége lesz a toochange informatikai most. Amikor a rendszer erre kéri, adjon meg az eszközhöz egy rendszergazdai jelszót. Az eszköz rendszergazdai jelszavának 8–15 karakter hosszúságúnak kell lennie. hello jelszó közül hármat tartalmaznia kell következő hello:, kisbetű, nagybetű, numerikus és speciális karaktereket.
    
    <br/>![StorSimple-eszköz regisztrálása, 5](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice5_U1-include.png)
11. hello hello telepítővarázsló utolsó lépésként regisztrálja az eszközt a StorSimple Manager szolgáltatás hello. Ehhez szüksége lesz a 2. lépésben beszerzett hello Szolgáltatásregisztrációs kulcs. Hello regisztrációs kulcsot fogja tartalmazni, miután szükséges toowait 2-3 percet hello eszköz regisztrálva van-e.
    
    > [!NOTE]
    > Minden alkalommal tooexit hello beállítása varázsló, is használja a Ctrl + C. Ha a megadott összes hello hálózati beállításokat (a Data 0, alhálózati maszk és átjáró IP-cím), a bejegyzéseket megőrzi a rendszer.
    > 
    > 
    
    ![StorSimple-eszköz regisztrálása, 6](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice6_U1-include.png)
12. Hello eszköz regisztrálása után megjelenik egy szolgáltatásadat-titkosítási kulcs. Másolja ki ezt a kulcsot, és mentse egy biztonságos helyre. **Ezt a kulcsot kötelező megadni a hello szolgáltatás regisztrációs kulcs tooregister további eszközöket a StorSimple Manager szolgáltatás hello lesz.** Tekintse meg a túl[StorSimple biztonsági](../articles/storsimple/storsimple-security.md) ezt a kulcsot további információt.
    
    ![StorSimple-eszköz regisztrálása, 7](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice7_U1-include.png)    
    
    > [!NOTE]
    > toocopy hello szöveg hello soros konzol ablakából, egyszerűen jelölje ki a hello szöveget. Ezután meg kell tudni toopaste hello vágólapra vagy bármilyen szövegszerkesztővel azt. Ne használja a Ctrl + C toocopy hello szolgáltatásadat-titkosítási kulcs. A Ctrl + C miatt meg tooexit hello beállítása varázsló. Ennek eredményeképpen hello eszköz rendszergazdai jelszava nem változik, és hello eszköz visszaállítja toohello alapértelmezett jelszót.
    > 
    > 
13. Kilépés hello soros konzolon.
14. Térjen vissza a klasszikus Azure portálon toohello, és végezze el az alábbi lépésekkel hello:
    
    1. Kattintson duplán a StorSimple Manager szolgáltatás tooaccess hello **gyors üzembe helyezés** lap.
    2. Kattintson a **Csatlakoztatott eszközök megtekintése** lehetőségre.
    3. A hello **eszközök** lapon, győződjön meg arról, hogy hello eszköz sikeresen csatlakozott toohello szolgáltatás hello állapot megkeresésével. hello Eszközállapot kell **Online**.
       
        ![A StorSimple Eszközök lapja](./media/storsimple-configure-and-register-device-u1/HCS_DevicesPageM_U1-include.png) 
       
        Ha hello eszköz állapota **Offline**, néhány perc alatt az online hello eszköz toocome várja. 
       
        Ha hello eszköz továbbra is kapcsolat nélküli üzemmódban néhány perc múlva, akkor meg kell, hogy a tűzfallal védett hálózat lett konfigurálva, a toomake [hálózati követelményei a StorSimple eszköz](../articles/storsimple/storsimple-system-requirements.md). 
       
        Győződjön meg arról, hogy 9354-es port kimenő kommunikációra nyitva-e, ez használható hello service bus által a StorSimple Manager szolgáltatás-és eszköz közötti kommunikációt.

