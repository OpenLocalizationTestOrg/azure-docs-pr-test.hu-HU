<!--author=SharS last changed: 02/22/2016-->

### <a name="tooconfigure-and-register-hello-device"></a>hello eszköz tooconfigure és regisztrálása
1. Hozzáférés hello Windows PowerShell felületet a StorSimple eszköz soros konzoljához a. Lásd: [PuTTY használata tooconnect toohello eszköz soros konzoljához](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console) utasításokat. **Lehet, hogy toofollow hello eljárás pontosan vagy nem fog tudni tooaccess hello konzol.**
2. A megnyitott munkamenetben hello nyomja le az adjon meg egy idő tooget egy parancssori ablakot.
3. Rákérdezéses toochoose hello nyelvi, hogy milyen tooset az eszközhöz lesz. Adja meg a hello nyelv, és nyomja le az ENTER billentyűt.
   
    ![StorSimple-eszköz konfigurálása és regisztrálása, 1](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice1-gov-include.png)
4. Hello soros konzol menüben megjelenő válassza a 1. lehetőség toolog a teljes hozzáféréssel.
   
    ![StorSimple-eszköz regisztrálása, 2](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice2-gov-include.png)
5. Hajtsa végre a következő lépéseket tooconfigure hello minimálisan szükséges hálózati beállításokat az eszközhöz hello.
   
   > [!IMPORTANT]
   > Ezeket a konfigurációs lépéseket kell toobe hello hello eszköz aktív vezérlőjén végre. hello soros konzol menü hello vezérlő állapotát a szalagcím üdvözlőüzenetére jelzi. Ha vannak nem toohello aktív vezérlő, válassza le, és csatlakoztassa a toohello aktív vezérlő.
   > 
   > 
   
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
      > 
      > 
   4. Igény szerint állítsa be a proxy-webkiszolgáló.
      
      > [!IMPORTANT]
      > Bár a webproxy konfigurálása nem kötelező, vegye figyelembe, hogy ha olyan webproxyt használ, csak konfigurálhatja azt itt. További információ: túl[az eszköz webalkalmazás-proxy konfigurálása](../articles/storsimple/storsimple-configure-web-proxy.md).
      > 
      > 
6. Ctrl + C billentyűkombinációval tooexit hello beállítása varázsló.
7. Hello frissítések telepítése az alábbiak szerint:
   
   1. Használja a következő parancsmag tooset IP-címeket mindkét hello tartományvezérlőn hello:
      
      `Set-HcsNetInterface -InterfaceAlias Data0 -Controller0IPv4Address <Controller0 IP> -Controller1IPv4Address <Controller1 IP>`
   2. Hello parancssorban futtassa `Get-HcsUpdateAvailability`. Meg kell értesítés frissítések érhetők el.
   3. Futtassa az `Start-HcsUpdate` parancsot. Ez a parancs futtatható bármely csomópontján. Hello első tartományvezérlő alkalmazandó frissítések hello vezérlő hajt végre feladatátvételt és majd a alkalmazandó frissítések hello hello más vezérlő.
      
      Kísérheti hello hello frissítés futtatásával `Get-HcsUpdateStatus`.    
      
      hello következő minta kimenet látható hello frissítés folyamatban van.
      
      ````
      Controller0>Get-HcsUpdateStatus
      RunInprogress       : True
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ````
      
      a következő minta kimenet hello azt jelzi, hogy adott hello frissítése befejeződött.
      
      ```
      Controller1>Get-HcsUpdateStatus
      
      RunInprogress       : False
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ```
      
      Tooapply összes hello frissítéseket, beleértve a Windows-frissítések hello too11 órát igénybe vehet.
8. Futtassa a következő parancsmag toopoint hello eszköz toohello a Microsoft Azure Government portálon (mert alapértelmezés szerint mutat toohello nyilvános a klasszikus Azure portálon) hello. Ez mindkét tartományvezérlők újraindul. Azt javasoljuk, hogy használja-e két PuTTY munkamenet toosimultaneously csatlakozás tooboth vezérlőket, hogy a tartományvezérlők újraindításakor láthatóvá.
   
    `Set-CloudPlatform -AzureGovt_US`
   
   Egy megerősítő üzenet jelenik meg. Fogadja el az alapértelmezett hello (**Y**).
9. Futtassa a következő parancsmag tooresume telepítő hello:
   
    `Invoke-HcsSetupWizard`
   
    ![A telepítővarázsló folytatása](./media/storsimple-configure-and-register-device-gov-u2/HCS_ResumeSetup-gov-include.png)
   
   Amikor a telepítő folytatja, akkor hello varázsló hello Update 2 verziója lesz.
10. Fogadja el a hello hálózati beállításokat. Egy érvényesítési üzenet jelenik meg, ha elfogadja, hogy egyes beállítások.
11. Biztonsági okokból hello eszköz rendszergazdai jelszava hello első munkamenet végeztével lejár, és szüksége lesz a toochange informatikai most. Amikor a rendszer erre kéri, adjon meg az eszközhöz egy rendszergazdai jelszót. Az eszköz rendszergazdai jelszavának 8–15 karakter hosszúságúnak kell lennie. hello jelszó közül hármat tartalmaznia kell következő hello:, kisbetű, nagybetű, numerikus és speciális karaktereket.
    
    <br/>![StorSimple-eszköz regisztrálása, 5](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice5_gov-include.png)
12. hello hello telepítővarázsló utolsó lépésként regisztrálja az eszközt a StorSimple Manager szolgáltatás hello. Ezt akkor lesz kell hello beolvasott Szolgáltatásregisztrációs kulcs [2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key). Hello regisztrációs kulcsot fogja tartalmazni, miután szükséges toowait 2-3 percet hello eszköz regisztrálva van-e.
    
    > [!NOTE]
    > Minden alkalommal tooexit hello beállítása varázsló, is használja a Ctrl + C. Ha a megadott összes hello hálózati beállításokat (a Data 0, alhálózati maszk és átjáró IP-cím), a bejegyzéseket megőrzi a rendszer.
    > 
    > 
    
    ![A StorSimple regisztrációs folyamat](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegistrationProgress-gov-include.png)
13. Hello eszköz regisztrálása után megjelenik egy szolgáltatásadat-titkosítási kulcs. Másolja ki ezt a kulcsot, és mentse egy biztonságos helyre. **Ezt a kulcsot kötelező megadni a hello szolgáltatás regisztrációs kulcs tooregister további eszközöket a StorSimple Manager szolgáltatás hello lesz.** Tekintse meg a túl[StorSimple biztonsági](../articles/storsimple/storsimple-security.md) ezt a kulcsot további információt.
    
    ![StorSimple-eszköz regisztrálása, 7](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice7_gov-include.png)    
    
    > [!IMPORTANT]
    > toocopy hello szöveg hello soros konzol ablakából, egyszerűen jelölje ki a hello szöveget. Ezután meg kell tudni toopaste hello vágólapra vagy bármilyen szövegszerkesztővel azt.
    > 
    > Ne használja a Ctrl + C toocopy hello szolgáltatásadat-titkosítási kulcs. A Ctrl + C miatt meg tooexit hello beállítása varázsló. Ennek eredményeképpen hello eszköz rendszergazdai jelszava nem változik, és hello eszköz visszaállítja toohello alapértelmezett jelszót.
    > 
    > 
14. Kilépés hello soros konzolon.
15. Térjen vissza a toohello Azure Government portálon, és végezze el az alábbi lépésekkel hello:
    
    1. Kattintson duplán a StorSimple Manager szolgáltatás tooaccess hello **gyors üzembe helyezés** lap.
    2. Kattintson a **Csatlakoztatott eszközök megtekintése** lehetőségre.
    3. A hello **eszközök** lapon, győződjön meg arról, hogy hello eszköz sikeresen csatlakozott toohello szolgáltatás hello állapot megkeresésével. hello Eszközállapot kell **Online**.
       
        ![A StorSimple Eszközök lapja](./media/storsimple-configure-and-register-device-gov-u2/HCS_DeviceOnline-gov-include.png)
       
        Ha hello eszköz állapota **Offline**, néhány perc alatt az online hello eszköz toocome várja.
       
        Ha hello eszköz továbbra is kapcsolat nélküli üzemmódban néhány perc múlva, akkor meg kell, hogy a tűzfallal védett hálózat lett konfigurálva, a toomake [hálózati követelményei a StorSimple eszköz](../articles/storsimple/storsimple-system-requirements.md).
       
        Győződjön meg arról, hogy 9354-es port kimenő kommunikációra nyitva-e, ez használható hello service bus által a StorSimple Manager szolgáltatás-és eszköz közötti kommunikációt.

