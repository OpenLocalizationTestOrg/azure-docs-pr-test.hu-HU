<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="08bf9-101">A StorSimple karbantartási mód gyorsjavítások Windows PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="08bf9-101">To install Maintenance mode hotfixes via Windows PowerShell for StorSimple</span></span>
> [!IMPORTANT]
> <span data-ttu-id="08bf9-102">A karbantartási módban kell először telepítse a gyorsjavítást, egy tartományvezérlő és a többi tartományvezérlőn.</span><span class="sxs-lookup"><span data-stu-id="08bf9-102">In Maintenance mode, you need to apply the hotfix first on one controller and then on the other controller.</span></span>
> 
> 

1. <span data-ttu-id="08bf9-103">Karbantartási módban helyezze az eszközt.</span><span class="sxs-lookup"><span data-stu-id="08bf9-103">Place the device into Maintenance mode.</span></span> <span data-ttu-id="08bf9-104">Lásd: [2. lépés: Adja meg a karbantartási mód](../articles/storsimple/storsimple-update-device.md#step2) adja meg a karbantartási mód útmutatást.</span><span class="sxs-lookup"><span data-stu-id="08bf9-104">See [Step 2: Enter Maintenance mode](../articles/storsimple/storsimple-update-device.md#step2) for instructions on how to enter Maintenance mode.</span></span>
2. <span data-ttu-id="08bf9-105">Telepítse a gyorsjavítást, írja be:</span><span class="sxs-lookup"><span data-stu-id="08bf9-105">To apply the hotfix, type:</span></span>
   
     `Start-HcsHotfix` 
3. <span data-ttu-id="08bf9-106">Amikor a rendszer kéri, adja meg a gyorsjavítás fájlokat tartalmazó hálózati megosztott mappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="08bf9-106">When prompted, supply the path to the network shared folder that contains the hotfix files.</span></span>
4. <span data-ttu-id="08bf9-107">Megerősítést kér bekéri.</span><span class="sxs-lookup"><span data-stu-id="08bf9-107">You will be prompted for confirmation.</span></span> <span data-ttu-id="08bf9-108">Típus **Y** a gyorsjavítás telepítése gombra.</span><span class="sxs-lookup"><span data-stu-id="08bf9-108">Type **Y** to proceed with the hotfix installation.</span></span>
5. <span data-ttu-id="08bf9-109">Egy tartományvezérlő a gyorsjavítás alkalmazása után jelentkezzen be a másik vezérlő.</span><span class="sxs-lookup"><span data-stu-id="08bf9-109">After you have applied the hotfix on one controller, log on to the other controller.</span></span> <span data-ttu-id="08bf9-110">Telepítse a gyorsjavítást, mint az előző tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="08bf9-110">Apply the hotfix as you did for the previous controller.</span></span>
6. <span data-ttu-id="08bf9-111">A gyorsjavítások alkalmazásakor kilép karbantartási módból.</span><span class="sxs-lookup"><span data-stu-id="08bf9-111">After the hotfixes are applied, exit Maintenance mode.</span></span> <span data-ttu-id="08bf9-112">Lásd: [4. lépés: Kilépés a karbantartási mód](../articles/storsimple/storsimple-update-device.md#step4) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="08bf9-112">See [Step 4: Exit Maintenance mode](../articles/storsimple/storsimple-update-device.md#step4) for instructions.</span></span>

