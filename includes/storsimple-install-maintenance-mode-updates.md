<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>tooinstall karbantartási mód frissítések keresztül a Windows PowerShell-lel
1. Ha még nem tette meg, hozzáférhet a hello eszköz soros konzoljához és select 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**. 
2. Adja meg a hello jelszót. hello alapértelmezett jelszó **jelszó1**.
3. Hello parancsot, írja be a parancssorba:
   
     `Get-HcsUpdateAvailability` 
4. Ha frissítések érhetők el, és hogy hello frissítései zavaró vagy nem zavaró, értesítést fog kapni. tooapply zavaró frissítések kell tooput hello eszköz karbantartási módba. Lásd: [2. lépés: Adja meg a karbantartási mód](../articles/storsimple/storsimple-update-device.md#step2) utasításokat.
5. Amikor az eszköz a karbantartási mód hello parancsot, írja be a parancssorba:`Start-HcsUpdate`
6. Megerősítést kér bekéri. Miután meggyőződött róla, hogy hello frissítéseket, azok jelenleg elérő hello tartományvezérlőn telepíti. Hello frissítések telepítése után hello controller újraindításához. 
7. Frissítések hello állapotának figyelése. Típus:
   
    `Get-HcsUpdateStatus`
   
    Ha hello `RunInProgress` van `True`, hello frissítése még folyamatban van. Ha `RunInProgress` van `False`, azt jelzi, hogy hello frissítése befejeződött.  
8. Hello frissítés telepítve van az aktuális hello történik, és azt újraindult, toohello csatlakozzon a többi tartományvezérlő, és hajtsa végre az 1 – 6. lépéseket.
9. Miután mindkét tartományvezérlők frissítése, lépjen ki a karbantartási módban. Lásd: [4. lépés: Kilépés a karbantartási mód](../articles/storsimple/storsimple-update-device.md#step4) utasításokat.

