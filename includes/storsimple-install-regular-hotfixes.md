<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-regular-hotfixes-via-windows-powershell-for-storsimple"></a>rendszeres gyorsjavítások tooinstall keresztül a Windows PowerShell-lel
1. Csatlakozás toohello eszköz soros konzoljához. További információkért lásd: [1. lépés: csatlakozás soros konzolon toohello](../articles/storsimple/storsimple-update-device.md#step1).
2. Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**. Adja meg a hello jelszót. hello alapértelmezett jelszó **jelszó1**.
3. Hello parancsot, írja be a parancssorba:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > Ez a parancs csak tooregular gyorsjavítások vonatkozik. A parancs futtatása csak egy tartományvezérlőn, de mindkét tartományvezérlők frissülni fog.
    > A vezérlő feladatátvétel Észreveheti hello frissítés során; azonban hello feladatátvételi nem érinti a rendszer rendelkezésre állás vagy a műveletet.

4. Amikor a rendszer kéri, adja meg a hello elérési toohello hálózati megosztott fájlokat tartalmazó mappa hello gyorsjavítás.
5. Megerősítést kér bekéri. Típus **Y** hello gyorsjavítás telepítésének tooproceed.

