<!--author=alkohli last changed: 9/17/15-->

#### <a name="toocomplete-hello-minimum-storsimple-device-setup"></a>toocomplete hello minimális StorSimple Eszközbeállítás
1. Válassza ki hello eszközt, és kattintson a **gyors üzembe helyezés**. Kattintson a **eszköz beállításának befejezése** toostart hello eszköz konfigurálása varázsló.
2. Az eszköz konfigurálása varázsló hello **alapbeállítások** párbeszédpanel mezőbe hello a következő:
   
   1. Adjon egy **rövid** nevet az eszköznek. hello alapértelmezett eszköznév olyan információkból például hello eszköz típusa és sorozatszáma. Egy rövid nevet a too64 karakterek toomanage fel az eszköz rendelhet hozzá.
   2. Set hello **időzóna** hello mely hello az eszköz üzembe helyezésének földrajzi helye alapján. Az eszköz minden ütemezett művelethez ezt az időzónát használja.
   3. A **DNS-beállítások** területen adjon meg egy címet a **Másodlagos DNS-kiszolgálóhoz**. Ha IPv6-címeket használ, hello mező fognak megjelenni hello hello Windows PowerShell felületén megadott IPv6-előtag alapján. 
      Ha hello másodlagos DNS-kiszolgáló nincs konfigurálva, akkor nem lesz engedélyezett toosave az eszközök konfigurációját.
   4. Az iSCSI-kompatibilis adapterek felületén engedélyezzen legalább egy hálózatot az iSCSI számára. Legalább egy hálózati adaptert kell toobe a felhőalapú és egyet toobe iSCSI-kompatibilis. A DATA 0 automatikusan engedélyezve van a felhőhöz.
      
      ![A StorSimple minimális eszközbeállításának alapbeállításai](./media/storsimple-complete-minimum-device-setup-u1/HCS_MinDeviceSetupBasicSettings1-include.png)
3. Kattintson a hello nyíl ikonra. ![StorSimple nyíl ikon](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)
4. A hello **hálózati illesztők** párbeszédpanelen adja meg a hello IP-címek a vezérlő 0 és a vezérlő 1 fix. **hello vezérlő rögzített IP-címeket kell toobe szabad IP-cím elérhető hello alhálózaton belüli hello eszköz IP-cím alapján.** Ha hello DATA 0 felület lett konfigurálva az IPv4, hello statikus IP-címek kell toobe hello megadott IPv4-formátumban. Ha IPv6-konfigurációhoz tartozó előtagot adott meg, statikus IP-címek hello a mezők automatikusan lesz kitöltve.

    ![A StorSimple minimális eszközbeállításának hálózati adapterei](./media/storsimple-complete-minimum-device-setup-u1/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

    IP-címek hello vezérlő rögzített hello hello frissítések toohello eszköz karbantartásához használatosak, ezért rögzített IP-címei hello kell irányíthatók, és képes tooconnect toohello Internet. Ellenőrizheti, hogy a fix vezérlő IP-címek k irányíthatóságát hello [Test-HcsmConnection] [ Test] parancsmag. a következő példa azt mutatja be a fix vezérlő IP irányított toohello Internet és férhetnek hozzá hello hello Microsoft Update-kiszolgálókról. 

     ![Test-HcsmConnection irányítható IP-kkel](./media/storsimple-complete-minimum-device-setup-u1/Test-HcsmConnectionOutputRegisteredDevice.png)

1. Kattintson a pipa ikonra hello ![StorSimple pipa ikon](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png).
   Ekkor visszakerül toohello eszköz **gyors üzembe helyezés** lap.
   
   > [!NOTE]
   > Módosíthatja az összes hello eszköz többi beállítását bármikor hello elérésével **konfigurálása** lap.
   > 
   > 

<!--Link reference-->
[Test]: https://technet.microsoft.com/library/dn715782(v=wps.630).aspx