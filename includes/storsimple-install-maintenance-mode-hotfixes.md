<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>tooinstall karbantartási mód gyorsjavítások keresztül a Windows PowerShell-lel
> [!IMPORTANT]
> A karbantartási mód tooapply hello gyorsjavítást kell először egy tartományvezérlő, és majd a többi tartományvezérlő hello.
> 
> 

1. Hely hello eszköz karbantartási módba. Lásd: [2. lépés: Adja meg a karbantartási mód](../articles/storsimple/storsimple-update-device.md#step2) útmutatást tooenter karbantartási módban.
2. tooapply hello gyorsjavítás, típus:
   
     `Start-HcsHotfix` 
3. Amikor a rendszer kéri, adja meg a hello elérési toohello hálózati megosztott fájlokat tartalmazó mappa hello gyorsjavítás.
4. Megerősítést kér bekéri. Típus **Y** hello gyorsjavítás telepítésének tooproceed.
5. Miután alkalmazott hello gyorsjavítás egy tartományvezérlőre, toohello bejelentkezés más vezérlő. A gyorsjavítás hello hello előző vezérlő hasonló módon.
6. Hello gyorsjavítások alkalmazása, után kilép karbantartási módból. Lásd: [4. lépés: Kilépés a karbantartási mód](../articles/storsimple/storsimple-update-device.md#step4) utasításokat.

