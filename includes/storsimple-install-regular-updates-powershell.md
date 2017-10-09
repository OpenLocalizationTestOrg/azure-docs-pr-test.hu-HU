<!--author=SharS last changed: 11/18/16-->

#### <a name="tooinstall-regular-updates-via-windows-powershell-for-storsimple"></a><span data-ttu-id="b484a-101">rendszeres frissítések tooinstall keresztül a Windows PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="b484a-101">tooinstall regular updates via Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="b484a-102">Nyissa meg a hello eszköz soros konzoljához és select 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="b484a-102">Open hello device serial console and select option 1, **Log in with full access**.</span></span> <span data-ttu-id="b484a-103">Adja meg a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="b484a-103">Type hello password.</span></span> <span data-ttu-id="b484a-104">hello alapértelmezett jelszó *jelszó1*.</span><span class="sxs-lookup"><span data-stu-id="b484a-104">hello default password is *Password1*.</span></span> 
2. <span data-ttu-id="b484a-105">Hello parancsot, írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="b484a-105">At hello command prompt, type:</span></span>
   
     `Get-HcsUpdateAvailability`
   
    <span data-ttu-id="b484a-106">Ha frissítések érhetők el, és hogy hello frissítései zavaró vagy nem zavaró, értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="b484a-106">You will be notified if updates are available and whether hello updates are disruptive or non-disruptive.</span></span>
3. <span data-ttu-id="b484a-107">Hello parancsot, írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="b484a-107">At hello command prompt, type:</span></span>
   
     `Start-HcsUpdate`
   
    <span data-ttu-id="b484a-108">hello frissítési folyamat indul el.</span><span class="sxs-lookup"><span data-stu-id="b484a-108">hello update process will start.</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="b484a-109">Ez a parancs csak a tooregular frissítéseket alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="b484a-109">This command applies only tooregular updates.</span></span> <span data-ttu-id="b484a-110">A parancs futtatása csak egy tartományvezérlőn, de mindkét tartományvezérlők frissülni fog.</span><span class="sxs-lookup"><span data-stu-id="b484a-110">You run this command on only one controller, but both controllers will be updated.</span></span> 
> * <span data-ttu-id="b484a-111">A vezérlő feladatátvétel Észreveheti hello frissítés során; azonban hello feladatátvételi nem érinti a rendszer rendelkezésre állás vagy a műveletet.</span><span class="sxs-lookup"><span data-stu-id="b484a-111">You may notice a controller failover during hello update process; however, hello failover will not affect system availability or operation.</span></span>
> 
> 

