<!--author=alkohli last changed: 01/18/2017-->


#### <a name="tooconfigure-and-register-hello-device"></a>hello eszköz tooconfigure és regisztrálása

1. Hozzáférés hello Windows PowerShell felületet a StorSimple eszköz soros konzoljához a. Lásd: [PuTTY használata tooconnect toohello eszköz soros konzoljához](#use-putty-to-connect-to-the-device-serial-console) utasításokat. **Lehet, hogy toofollow hello eljárás pontosan vagy nem fog tudni tooaccess hello konzol.**

2. Hello megnyitott munkamenetben, nyomja le a **Enter** egy idő tooget egy parancssori ablakot.

3. Rákérdezéses toochoose hello nyelvi, hogy milyen tooset az eszközhöz lesz. Adja meg a hello nyelv, és nyomja le az **Enter**.

4. Hello soros konzol menüben megjelenő, válassza az 1 túl**jelentkezzen be a teljes körű hozzáférési**.
     Hajtsa végre a lépéseket 5 – 12 tooconfigure hello minimálisan szükséges hálózati beállításokat az eszközhöz. **Ezeket a konfigurációs lépéseket kell toobe hello hello eszköz aktív vezérlőjén végre.** hello soros konzol menü hello vezérlő állapotát a szalagcím üdvözlőüzenetére jelzi. Ha korábban nem kapcsolódott a toohello aktív vezérlő, válassza le, és csatlakoztassa a toohello aktív vezérlő.

5. Hello parancssorba írja be a jelszavát. hello eszköz alapértelmezett jelszava: **jelszó1**.

6. A következő parancs típusa hello: `Invoke-HcsSetupWizard`.

7. A telepítő varázsló jelenik meg toohelp hello hello eszköz hálózati beállításainak konfigurálása. Adja meg a következő információ hello hello:
   
   * Hello DATA 0 hálózati adapterén IP-címe
   * Alhálózati maszk
   * Átjáró
   * Az elsődleges DNS-kiszolgáló IP-címe

   Az alábbiakban egy példa látható a kimenetre.

    ```
        ---------------------------------------------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: 8100-SHX0991003G44MT
        Software Version: 6.3.9600.17759
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Active
        ---------------------------------------------------------------

        Your device needs toobe registered with hello Microsoft Azure StorSimple Manager service. Please run 'Invoke-HcsSetupWizard' tooset up your device.

        Controller0>Invoke-HcsSetupWizard

        Which IP address family would you like tooconfigure on interface Data0?
        [4] IPv4 [6] IPv6 [B] Both (Default is "4"): 4

        Data0 IPv4 address:10.111.111.00
        Data0 IPv4 subnet: 255.255.252.0
        Data0 IPv4 gateway: 10.111.111.11

        IPv4 primary DNS server [10.222.118.154]:10.222.222.111
    ```

    <br>
    A minta kimenet megelőző hello láthatja, hogy hello rendszer hello folyamat minden lépése után érvényesíti a hálózati beállításokat.

     > [!NOTE]
     > Előfordulhat, hogy toowait hello alhálózati maszk és hello DNS beállítások toobe alkalmazása néhány percig. Ha a "Jelölőnégyzet hello hálózati kapcsolat tooData 0" hiba jelenik meg, ellenőrizze az aktív vezérlő hello DATA 0 hálózati adapterén hello fizikai hálózati kapcsolatot.

8. (Nem kötelező) konfigurálja a webproxy-kiszolgálót. Bár a webproxy konfigurálása nem kötelező, **vegye figyelembe, hogy ha webproxyt használ, azt csak itt tudja beállítani**. További információ: túl[az eszköz webalkalmazás-proxy konfigurálása](../articles/storsimple/storsimple-8000-configure-web-proxy.md).
9. Állítson be egy elsődleges NTP-kiszolgálót az eszközhöz. Az NTP-kiszolgálókra azért van szükség, mert az eszköznek szinkronizálnia kell az időt ahhoz, hogy a felhőszolgáltatókkal hitelesítést végezhessen. Győződjön meg arról, hogy a hálózat engedélyezi-e a datacenter toohello Internet az NTP-forgalom toopass. Ha ez nem lehetséges, akkor adjon meg egy belső NTP-kiszolgálót.

    Az alábbiakban egy példa látható a kimenetre.

    ```
        Would you like tooconfigure a web proxy?
        [Y] Yes [N] No (Default is "N"):N

        Primary NTP server [time.windows.com]:time.windows.com

    ```

10. Biztonsági okokból hello eszköz rendszergazdai jelszava hello első munkamenet végeztével lejár, és szüksége lesz a toochange informatikai most. Amikor a rendszer erre kéri, adjon meg az eszközhöz egy rendszergazdai jelszót. Az eszköz rendszergazdai jelszavának 8–15 karakter hosszúságúnak kell lennie. hello jelszó közül hármat tartalmaznia kell következő hello:, kisbetű, nagybetű, numerikus és speciális karaktereket.

    ```
        hello device administrator password must be between 8 and 15 characters. hello password must contain a combination of uppercase letters, lowercase letters, numbers and special characters.
        Administrator Password:********
        Confirm Administrator Password:********
    ```
11. hello hello telepítővarázsló utolsó lépésként regisztrálja az eszközt hello StorSimple Device Manager szolgáltatást. Ehhez szüksége lesz a 2. lépésben beszerzett hello Szolgáltatásregisztrációs kulcs. Hello regisztrációs kulcsot fogja tartalmazni, miután szükséges toowait 2-3 percet hello eszköz regisztrálva van-e.
    
    > [!NOTE]
    > Minden alkalommal tooexit hello beállítása varázsló, is használja a Ctrl + C. Ha a megadott összes hello hálózati beállításokat (a Data 0, alhálózati maszk és átjáró IP-cím), a bejegyzéseket megőrzi a rendszer.
    
    Az alábbiakban egy példa látható a kimenetre.

    ```
        hello service registration key is available in hello StorSimple Manager service.
        Enter service registration key:**************************************
        Device registration is in progress. Please wait.

    ```

12. Hello eszköz regisztrálása után megjelenik egy szolgáltatásadat-titkosítási kulcs. Másolja ki ezt a kulcsot, és mentse egy biztonságos helyre. **Ezt a kulcsot kötelező megadni a hello szolgáltatás regisztrációs kulcs tooregister további eszközök hello StorSimple Device Manager szolgáltatás lesz.** Tekintse meg a túl[StorSimple biztonsági](../articles/storsimple/storsimple-security.md) ezt a kulcsot további információt.
    
    ![StorSimple-eszköz regisztrálása, 7](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup1.png)
    
    > [!NOTE]
    > toocopy hello szöveg hello soros konzol ablakából, egyszerűen jelölje ki a hello szöveget. Ezután meg kell tudni toopaste hello vágólapra vagy bármilyen szövegszerkesztővel azt. Ne használja a Ctrl + C toocopy hello szolgáltatásadat-titkosítási kulcs. A Ctrl + C miatt meg tooexit hello beállítása varázsló. Ennek eredményeképpen hello eszköz rendszergazdai jelszava nem változik, és hello eszköz visszaállítja toohello alapértelmezett jelszót.
    
13. Kilépés hello soros konzolon.
14. Térjen vissza a toohello Azure-portálon, és végezze el az alábbi lépésekkel hello:
    
    1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást.
    2. Kattintson az **Eszközök** elemre.
    3. Hello táblázatos eszközök listája ellenőrizze, hogy hello eszköz sikeresen csatlakozott toohello szolgáltatás hello állapot megkeresésével. hello Eszközállapot kell **tooset kész mentése**.
       
        ![A StorSimple Eszközök lapja](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup2.png)
       
        Szükség lehet toowait néhány percig, amíg hello eszköz állapota toochange túl**tooset kész mentése**.
       
        Ha hello eszköz nem jelenik meg ezen a listán, akkor meg kell, hogy a tűzfallal védett hálózat lett konfigurálva, a toomake [hálózati követelményei a StorSimple eszköz](../articles/storsimple/storsimple-8000-system-requirements.md). Győződjön meg arról, hogy 9354-es port kimenő kommunikációra nyitva-e, ez használható hello service bus által a StorSimple Device Manager szolgáltatás és eszköz közötti kommunikációt.

