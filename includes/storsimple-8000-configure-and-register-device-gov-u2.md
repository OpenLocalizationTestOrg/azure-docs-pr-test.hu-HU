<!--author=SharS last changed: 06/22/2016-->

### <a name="tooconfigure-and-register-hello-device"></a>hello eszköz tooconfigure és regisztrálása
1. Hozzáférés hello Windows PowerShell felületet a StorSimple eszköz soros konzoljához a. Lásd: [PuTTY használata tooconnect toohello eszköz soros konzoljához](../articles/storsimple/storsimple-8000-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console) utasításokat. **Lehet, hogy toofollow hello eljárás pontosan vagy nem fog tudni tooaccess hello konzol.**
2. Hello megnyitott munkamenetben, nyomja le a **Enter** egy idő tooget egy parancssori ablakot.
3. Rákérdezéses toochoose hello nyelvi, hogy milyen tooset az eszközhöz lesz. Adja meg a hello nyelv, és nyomja le az **Enter**.
   
    ![StorSimple-eszköz konfigurálása és regisztrálása, 1](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice1-gov-include.png)
4. Hello soros konzol menüben megjelenő válassza a 1. lehetőség toolog a teljes hozzáféréssel.
   
    ![StorSimple-eszköz regisztrálása, 2](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice2-gov-include.png)
5. Hajtsa végre a következő lépéseket tooconfigure hello minimálisan szükséges hálózati beállításokat az eszközhöz hello.
   
   > [!IMPORTANT]
   > Ezeket a konfigurációs lépéseket kell toobe hello hello eszköz aktív vezérlőjén végre. hello soros konzol menü hello vezérlő állapotát a szalagcím üdvözlőüzenetére jelzi. Ha vannak nem toohello aktív vezérlő, válassza le, és csatlakoztassa a toohello aktív vezérlő.
   
   1. Hello parancssorba írja be a jelszavát. hello eszköz alapértelmezett jelszava: **jelszó1**.
   2. Írja be a következő parancs hello:
      
        `Invoke-HcsSetupWizard`
   3. A telepítő varázsló jelenik meg toohelp hello hello eszköz hálózati beállításainak konfigurálása. Adja meg a következő információ hello:
      
      * DATA 0 hálózati adapterén IP-címe
      * Alhálózati maszk
      * Átjáró
      * Az elsődleges DNS-kiszolgáló IP-címe
      * Az elsődleges NTP-kiszolgáló IP-címe
      
      > [!NOTE]
      > Előfordulhat, hogy toowait hello alhálózati maszk és a DNS-beállítások toobe alkalmazása néhány percig.
    
   4. Igény szerint állítsa be a proxy-webkiszolgáló.
      
      > [!IMPORTANT]
      > Bár a webproxy konfigurálása nem kötelező, vegye figyelembe, hogy ha olyan webproxyt használ, csak konfigurálhatja azt itt. További információ: túl[az eszköz webalkalmazás-proxy konfigurálása](../articles/storsimple/storsimple-configure-web-proxy.md).
     
6. Ctrl + C billentyűkombinációval tooexit hello beállítása varázsló.
8. Futtassa a következő parancsmag toopoint hello eszköz toohello a Microsoft Azure Government portálon (mert alapértelmezés szerint mutat toohello nyilvános a klasszikus Azure portálon) hello. Ez mindkét tartományvezérlők újraindul. Azt javasoljuk, hogy használja-e két PuTTY munkamenet toosimultaneously csatlakozás tooboth vezérlőket, hogy a tartományvezérlők újraindításakor láthatóvá.
   
    `Set-CloudPlatform -AzureGovt_US`
   
   Egy megerősítő üzenet jelenik meg. Fogadja el az alapértelmezett hello (**Y**).
9. Futtassa a következő parancsmag tooresume telepítő hello:
   
    `Invoke-HcsSetupWizard`
   
    ![A telepítővarázsló folytatása](./media/storsimple-configure-and-register-device-gov-u2/HCS_ResumeSetup-gov-include.png)
   
10. Fogadja el a hello hálózati beállításokat. Egy érvényesítési üzenet jelenik meg, ha elfogadja, hogy egyes beállítások.
11. Biztonsági okokból hello eszköz rendszergazdai jelszava hello első munkamenet végeztével lejár, és szüksége lesz a toochange informatikai most. Amikor a rendszer erre kéri, adjon meg az eszközhöz egy rendszergazdai jelszót. Az eszköz rendszergazdai jelszavának 8–15 karakter hosszúságúnak kell lennie. hello jelszó közül hármat tartalmaznia kell következő hello:, kisbetű, nagybetű, numerikus és speciális karaktereket.
    
    <br/>![StorSimple-eszköz regisztrálása, 5](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice5_gov-include.png)
12. hello hello telepítővarázsló utolsó lépésként regisztrálja az eszközt hello StorSimple Device Manager szolgáltatást. Ezt akkor lesz kell hello beolvasott Szolgáltatásregisztrációs kulcs [2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése](../articles/storsimple/storsimple-8000-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key). Hello regisztrációs kulcsot fogja tartalmazni, miután szükséges toowait 2-3 percet hello eszköz regisztrálva van-e.
    
    > [!NOTE]
    > Minden alkalommal tooexit hello beállítása varázsló, is használja a Ctrl + C. Ha a megadott összes hello hálózati beállításokat (a Data 0, alhálózati maszk és átjáró IP-cím), a bejegyzéseket megőrzi a rendszer.
    
    ![A StorSimple regisztrációs folyamat](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegistrationProgress-gov-include.png)
13. Hello eszköz regisztrálása után megjelenik egy szolgáltatásadat-titkosítási kulcs. Másolja ki ezt a kulcsot, és mentse egy biztonságos helyre. **Ezt a kulcsot nem kötelező megadni a hello szolgáltatás regisztrációs kulcs tooregister további eszközök hello StorSimple Device Manager szolgáltatást.** Tekintse meg a túl[StorSimple biztonsági](../articles/storsimple/storsimple-8000-security.md) ezt a kulcsot további információt.
    
    ![StorSimple-eszköz regisztrálása, 7](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice7_gov-include.png)
    > [!IMPORTANT]
    > toocopy hello szöveg hello soros konzol ablakából, egyszerűen jelölje ki a hello szöveget. Ezután meg kell tudni toopaste hello vágólapra vagy bármilyen szövegszerkesztővel azt.
    > 
    > Ne használjon **Ctrl + C** toocopy hello szolgáltatásadat-titkosítási kulcs. Használatával **Ctrl + C** miatt a tooexit hello beállítása varázsló. Ennek eredményeképpen hello eszköz rendszergazdai jelszava nem változik, és hello eszköz visszaállítja toohello alapértelmezett jelszót.
    
14. Kilépés hello soros konzolon.
15. Térjen vissza a toohello Azure Government portálon, és végezze el az alábbi lépésekkel hello:
    
    1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást.
    2. Kattintson az **Eszközök** elemre. Az eszközök hello listáról, hogy-e ddeploying hello eszköz azonosítására. Győződjön meg arról, hogy hello eszköz sikeresen csatlakozott toohello szolgáltatás hello állapot megkeresésével. hello Eszközállapot kell **Online**.
            
        Ha hello eszköz állapota **Offline**, néhány perc alatt az online hello eszköz toocome várja.
       
        Ha hello eszköz továbbra is kapcsolat nélküli üzemmódban néhány perc múlva, akkor meg kell, hogy a tűzfallal védett hálózat lett konfigurálva, a toomake [hálózati követelményei a StorSimple eszköz](../articles/storsimple/storsimple-8000-system-requirements.md).
       
        Győződjön meg arról, hogy 9354-es port kimenő kommunikációra nyitva-e, ez használható hello service bus által a StorSimple Device Manager szolgáltatás-és eszköz közötti kommunikációt.

