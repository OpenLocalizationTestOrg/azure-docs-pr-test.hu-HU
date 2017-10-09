<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="5fe4b-101">tooinstall karbantartási mód gyorsjavítások keresztül a Windows PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="5fe4b-101">tooinstall Maintenance mode hotfixes via Windows PowerShell for StorSimple</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5fe4b-102">A karbantartási mód tooapply hello gyorsjavítást kell először egy tartományvezérlő, és majd a többi tartományvezérlő hello.</span><span class="sxs-lookup"><span data-stu-id="5fe4b-102">In Maintenance mode, you need tooapply hello hotfix first on one controller and then on hello other controller.</span></span>
> 
> 

1. <span data-ttu-id="5fe4b-103">Hely hello eszköz karbantartási módba.</span><span class="sxs-lookup"><span data-stu-id="5fe4b-103">Place hello device into Maintenance mode.</span></span> <span data-ttu-id="5fe4b-104">Lásd: [2. lépés: Adja meg a karbantartási mód](../articles/storsimple/storsimple-update-device.md#step2) útmutatást tooenter karbantartási módban.</span><span class="sxs-lookup"><span data-stu-id="5fe4b-104">See [Step 2: Enter Maintenance mode](../articles/storsimple/storsimple-update-device.md#step2) for instructions on how tooenter Maintenance mode.</span></span>
2. <span data-ttu-id="5fe4b-105">tooapply hello gyorsjavítás, típus:</span><span class="sxs-lookup"><span data-stu-id="5fe4b-105">tooapply hello hotfix, type:</span></span>
   
     `Start-HcsHotfix` 
3. <span data-ttu-id="5fe4b-106">Amikor a rendszer kéri, adja meg a hello elérési toohello hálózati megosztott fájlokat tartalmazó mappa hello gyorsjavítás.</span><span class="sxs-lookup"><span data-stu-id="5fe4b-106">When prompted, supply hello path toohello network shared folder that contains hello hotfix files.</span></span>
4. <span data-ttu-id="5fe4b-107">Megerősítést kér bekéri.</span><span class="sxs-lookup"><span data-stu-id="5fe4b-107">You will be prompted for confirmation.</span></span> <span data-ttu-id="5fe4b-108">Típus **Y** hello gyorsjavítás telepítésének tooproceed.</span><span class="sxs-lookup"><span data-stu-id="5fe4b-108">Type **Y** tooproceed with hello hotfix installation.</span></span>
5. <span data-ttu-id="5fe4b-109">Miután alkalmazott hello gyorsjavítás egy tartományvezérlőre, toohello bejelentkezés más vezérlő.</span><span class="sxs-lookup"><span data-stu-id="5fe4b-109">After you have applied hello hotfix on one controller, log on toohello other controller.</span></span> <span data-ttu-id="5fe4b-110">A gyorsjavítás hello hello előző vezérlő hasonló módon.</span><span class="sxs-lookup"><span data-stu-id="5fe4b-110">Apply hello hotfix as you did for hello previous controller.</span></span>
6. <span data-ttu-id="5fe4b-111">Hello gyorsjavítások alkalmazása, után kilép karbantartási módból.</span><span class="sxs-lookup"><span data-stu-id="5fe4b-111">After hello hotfixes are applied, exit Maintenance mode.</span></span> <span data-ttu-id="5fe4b-112">Lásd: [4. lépés: Kilépés a karbantartási mód](../articles/storsimple/storsimple-update-device.md#step4) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="5fe4b-112">See [Step 4: Exit Maintenance mode](../articles/storsimple/storsimple-update-device.md#step4) for instructions.</span></span>

