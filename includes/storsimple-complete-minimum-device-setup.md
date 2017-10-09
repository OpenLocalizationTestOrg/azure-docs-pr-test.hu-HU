<!--author=alkohli last changed: 9/17/15-->

#### <a name="toocomplete-hello-minimum-storsimple-device-setup"></a>toocomplete hello minimális StorSimple Eszközbeállítás
1. A hello **eszközök** lapon jelölje be hello eszköz elleni hello eszköz neve toogo toohello kívánt eszköz lapját hello nyílra. 
   
    ![Az Eszközök lap online eszközökkel](./media/storsimple-complete-minimum-device-setup/HCS_DevicesPageM-include.png) 
2. Gyors üzembe helyezési ikonra ![gyors üzembe helyezés ikonja](./media/storsimple-complete-minimum-device-setup/HCS_QuickStartIcon-include.png) tooaccess hello eszköz gyors üzembe helyezési oldala. Kattintson a **eszköz beállításának befejezése** toostart hello **eszköz konfigurálása** varázsló.
   
    ![Az eszköz gyors üzembe helyezési oldala](./media/storsimple-complete-minimum-device-setup/Device_Quick_Start_page_1M.png)
3. A hello **alapbeállítások** lapon, a következő hello:
   
   1. Adjon egy **rövid** nevet az eszköznek. hello alapértelmezett eszköznév olyan információkból például hello eszköz típusa és sorozatszáma. Egy rövid nevet a too64 karakterek toomanage fel az eszköz rendelhet hozzá.
   2. Set hello **időzóna** hello mely hello az eszköz üzembe helyezésének földrajzi helye alapján. Az eszköz minden ütemezett művelethez ezt az időzónát használja.
   3. A **DNS-beállítások** területen adjon meg egy címet a **Másodlagos DNS-kiszolgálóhoz**. Ha IPv6-címeket használ, hello mező fognak megjelenni hello hello Windows PowerShell felületén megadott IPv6-előtag alapján. 
      Ha hello másodlagos DNS-kiszolgáló nincs konfigurálva, akkor nem lesz engedélyezett toosave az eszközök konfigurációját.
   4. Az iSCSI-kompatibilis adapterek felületén engedélyezzen legalább egy hálózatot az iSCSI számára. Legalább egy hálózati adaptert kell toobe a felhőalapú és egyet toobe iSCSI-kompatibilis. A DATA 0 automatikusan engedélyezve van a felhőhöz.
      
      ![A StorSimple minimális eszközbeállításának alapbeállításai](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupBasicSettings1-include.png)
4. Kattintson a hello nyíl ikonra. ![StorSimple nyíl ikon](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)
5. A hello **hálózati illesztők** lapján adja meg a hello IP-címek a vezérlő 0 és a vezérlő 1 fix. Ha hello DATA 0 felület lett konfigurálva az IPv4, hello statikus IP-címek kell toobe hello megadott IPv4-formátumban. Ha IPv6-konfigurációhoz tartozó előtagot adott meg, statikus IP-címek hello a mezők automatikusan lesz kitöltve.

    > [!NOTE] 
    > - hello vezérlő rögzített IP-címeket kell toobe szabad IP-cím elérhető hello alhálózaton belüli hello eszköz IP-cím alapján.
    > - IP-címek hello vezérlő rögzített hello hello frissítések toohello eszköz karbantartásához használatosak, ezért rögzített IP-címei hello kell irányíthatók, és képes tooconnect toohello Internet.

    ![A StorSimple minimális eszközbeállításának hálózati adapterei](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

1. Kattintson a pipa ikonra hello ![StorSimple pipa ikon](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png).
   Ekkor visszakerül toohello eszköz **gyors üzembe helyezés** lap.
   
   > [!NOTE]
   > Módosíthatja az összes hello eszköz többi beállítását bármikor hello elérésével **konfigurálása** lap.
   > 
   > 

![Videó elérhető](./media/storsimple-complete-minimum-device-setup/Video_icon.png)**Videó elérhető**

a videó bemutatja, hogyan toocomplete hello minimális eszközbeállítások toowatch kattintson [Itt](https://azure.microsoft.com/documentation/videos/minimum-storsimple-device-setup/).

