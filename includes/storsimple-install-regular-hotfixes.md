<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="769bd-101">A StorSimple Windows PowerShell segítségével rendszeres gyorsjavítások telepítése</span><span class="sxs-lookup"><span data-stu-id="769bd-101">To install regular hotfixes via Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="769bd-102">Az eszköz soros konzoljához való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="769bd-102">Connect to the device serial console.</span></span> <span data-ttu-id="769bd-103">További információkért lásd: [1. lépés: csatlakozás soros konzolon](../articles/storsimple/storsimple-update-device.md#step1).</span><span class="sxs-lookup"><span data-stu-id="769bd-103">For more information, see [Step 1: Connect to the serial console](../articles/storsimple/storsimple-update-device.md#step1).</span></span>
2. <span data-ttu-id="769bd-104">A soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="769bd-104">In the serial console menu, select option 1, **Log in with full access**.</span></span> <span data-ttu-id="769bd-105">Írja be a jelszót.</span><span class="sxs-lookup"><span data-stu-id="769bd-105">Type the password.</span></span> <span data-ttu-id="769bd-106">Az alapértelmezett jelszó **jelszó1**.</span><span class="sxs-lookup"><span data-stu-id="769bd-106">The default password is **Password1**.</span></span>
3. <span data-ttu-id="769bd-107">A parancssorba írja be:</span><span class="sxs-lookup"><span data-stu-id="769bd-107">At the command prompt, type:</span></span>
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > <span data-ttu-id="769bd-108">Ez a parancs csak a rendszeres gyorsjavítások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="769bd-108">This command applies only to regular hotfixes.</span></span> <span data-ttu-id="769bd-109">A parancs futtatása csak egy tartományvezérlőn, de mindkét tartományvezérlők frissülni fog.</span><span class="sxs-lookup"><span data-stu-id="769bd-109">You run this command on only one controller, but both controllers will be updated.</span></span>
    > <span data-ttu-id="769bd-110">A vezérlő feladatátvétel Észreveheti a frissítés során; azonban a feladatátvétel nem érinti a rendszer rendelkezésre állás vagy a műveletet.</span><span class="sxs-lookup"><span data-stu-id="769bd-110">You may notice a controller failover during the update process; however, the failover will not affect system availability or operation.</span></span>

4. <span data-ttu-id="769bd-111">Amikor a rendszer kéri, adja meg a gyorsjavítás fájlokat tartalmazó hálózati megosztott mappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="769bd-111">When prompted, supply the path to the network shared folder that contains the hotfix files.</span></span>
5. <span data-ttu-id="769bd-112">Megerősítést kér bekéri.</span><span class="sxs-lookup"><span data-stu-id="769bd-112">You will be prompted for confirmation.</span></span> <span data-ttu-id="769bd-113">Típus **Y** a gyorsjavítás telepítése gombra.</span><span class="sxs-lookup"><span data-stu-id="769bd-113">Type **Y** to proceed with the hotfix installation.</span></span>

