<!--author=alkohli last changed: 01/12/17-->

#### <a name="toocomplete-hello-minimum-storsimple-device-setup"></a>toocomplete hello minimális StorSimple Eszközbeállítás

   > [!NOTE]
   > Hello eszköz neve nem módosítható, ha a minimális eszközbeállítások hello befejeződött.
   
1. A hello táblázatos felsorolása hello eszközök **eszközök** panelen jelölje ki, majd kattintson az eszköz. hello eszköz egy **tooset kész mentése** állapotát. Hello **eszköz konfigurálása** panel megnyílik.

     ![A StorSimple minimális eszközbeállításának hálózati adapterei](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig1.png)

2. A hello **eszköz konfigurálása** panel:
   
   1. Adjon egy **rövid** nevet az eszköznek. hello alapértelmezett eszköznév olyan információkból például hello eszköz típusa és sorozatszáma. Egy rövid nevet a too64 karakterek toomanage fel az eszköz rendelhet hozzá.
   2. Set hello **időzóna** hello mely hello az eszköz üzembe helyezésének földrajzi helye alapján. Az eszköz minden ütemezett művelethez ezt az időzónát használja.
   3. A hello **DATA 0 beállítások**:

       1. A DATA 0 hálózati adapter látható hello engedélyezett hálózati beállításai (IP, alhálózaton, átjáró) hello beállítása varázsló segítségével. A DATA 0 a felhőben és az iSCSI-n is automatikusan engedélyezve van.

       2. Adja meg a hello IP-címek a vezérlő 0 és a vezérlő 1 fix. **hello vezérlő rögzített IP-címeket kell toobe szabad IP-cím elérhető hello alhálózaton belüli hello eszköz IP-cím alapján.** Ha hello DATA 0 felület lett konfigurálva az IPv4, hello statikus IP-címek kell toobe hello megadott IPv4-formátumban. Ha IPv6-konfigurációhoz tartozó előtagot adott meg, statikus IP-címek hello feltöltése automatikusan ezekbe a mezőkbe.

            ![A StorSimple minimális eszközbeállításának hálózati adapterei](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig2.png)

            IP-címek hello vezérlő rögzített hello hello frissítések toohello eszköz karbantartásához használatosak. Emiatt rögzített IP-címei hello irányíthatók, és képes tooconnect toohello Internet kell lennie. Ellenőrizheti, hogy a fix vezérlő IP-címek k irányíthatóságát hello [Test-HcsmConnection] [ Test] parancsmag. a következő példa azt mutatja be a fix vezérlő IP irányított toohello Internet és férhetnek hozzá hello hello Microsoft Update-kiszolgálókról.

            ![Test-HcsmConnection irányítható IP-kkel](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig3.png)

1. Kattintson az **OK** gombra. hello eszközkonfiguráció kezdődik. Ha hello eszközkonfiguráció befejeződött, értesítés jelenik meg. eszköz állapotmódosítások túl hello**Online** a hello **eszközök** panelen.

    ![A StorSimple minimális eszközbeállításának hálózati adapterei](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig4.png)

<!--Link reference-->
[Test]: https://technet.microsoft.com/library/dn715782(v=wps.630).aspx
