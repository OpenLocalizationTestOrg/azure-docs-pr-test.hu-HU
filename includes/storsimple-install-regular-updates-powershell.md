<!--author=SharS last changed: 11/18/16-->

#### <a name="tooinstall-regular-updates-via-windows-powershell-for-storsimple"></a>rendszeres frissítések tooinstall keresztül a Windows PowerShell-lel
1. Nyissa meg a hello eszköz soros konzoljához és select 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**. Adja meg a hello jelszót. hello alapértelmezett jelszó *jelszó1*. 
2. Hello parancsot, írja be a parancssorba:
   
     `Get-HcsUpdateAvailability`
   
    Ha frissítések érhetők el, és hogy hello frissítései zavaró vagy nem zavaró, értesítést fog kapni.
3. Hello parancsot, írja be a parancssorba:
   
     `Start-HcsUpdate`
   
    hello frissítési folyamat indul el.

> [!IMPORTANT]
> * Ez a parancs csak a tooregular frissítéseket alkalmazza. A parancs futtatása csak egy tartományvezérlőn, de mindkét tartományvezérlők frissülni fog. 
> * A vezérlő feladatátvétel Észreveheti hello frissítés során; azonban hello feladatátvételi nem érinti a rendszer rendelkezésre állás vagy a műveletet.
> 
> 

