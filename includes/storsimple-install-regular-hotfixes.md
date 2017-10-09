<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-regular-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="67703-101">rendszeres gyorsjavítások tooinstall keresztül a Windows PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="67703-101">tooinstall regular hotfixes via Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="67703-102">Csatlakozás toohello eszköz soros konzoljához.</span><span class="sxs-lookup"><span data-stu-id="67703-102">Connect toohello device serial console.</span></span> <span data-ttu-id="67703-103">További információkért lásd: [1. lépés: csatlakozás soros konzolon toohello](../articles/storsimple/storsimple-update-device.md#step1).</span><span class="sxs-lookup"><span data-stu-id="67703-103">For more information, see [Step 1: Connect toohello serial console](../articles/storsimple/storsimple-update-device.md#step1).</span></span>
2. <span data-ttu-id="67703-104">Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="67703-104">In hello serial console menu, select option 1, **Log in with full access**.</span></span> <span data-ttu-id="67703-105">Adja meg a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="67703-105">Type hello password.</span></span> <span data-ttu-id="67703-106">hello alapértelmezett jelszó **jelszó1**.</span><span class="sxs-lookup"><span data-stu-id="67703-106">hello default password is **Password1**.</span></span>
3. <span data-ttu-id="67703-107">Hello parancsot, írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="67703-107">At hello command prompt, type:</span></span>
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > <span data-ttu-id="67703-108">Ez a parancs csak tooregular gyorsjavítások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="67703-108">This command applies only tooregular hotfixes.</span></span> <span data-ttu-id="67703-109">A parancs futtatása csak egy tartományvezérlőn, de mindkét tartományvezérlők frissülni fog.</span><span class="sxs-lookup"><span data-stu-id="67703-109">You run this command on only one controller, but both controllers will be updated.</span></span>
    > <span data-ttu-id="67703-110">A vezérlő feladatátvétel Észreveheti hello frissítés során; azonban hello feladatátvételi nem érinti a rendszer rendelkezésre állás vagy a műveletet.</span><span class="sxs-lookup"><span data-stu-id="67703-110">You may notice a controller failover during hello update process; however, hello failover will not affect system availability or operation.</span></span>

4. <span data-ttu-id="67703-111">Amikor a rendszer kéri, adja meg a hello elérési toohello hálózati megosztott fájlokat tartalmazó mappa hello gyorsjavítás.</span><span class="sxs-lookup"><span data-stu-id="67703-111">When prompted, supply hello path toohello network shared folder that contains hello hotfix files.</span></span>
5. <span data-ttu-id="67703-112">Megerősítést kér bekéri.</span><span class="sxs-lookup"><span data-stu-id="67703-112">You will be prompted for confirmation.</span></span> <span data-ttu-id="67703-113">Típus **Y** hello gyorsjavítás telepítésének tooproceed.</span><span class="sxs-lookup"><span data-stu-id="67703-113">Type **Y** tooproceed with hello hotfix installation.</span></span>

