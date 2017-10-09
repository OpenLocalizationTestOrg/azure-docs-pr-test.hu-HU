<!--author=SharS last changed: 9/17/15-->

#### <a name="tooconnect-through-hello-serial-console"></a><span data-ttu-id="37f49-101">tooconnect hello soros konzolon keresztül</span><span class="sxs-lookup"><span data-stu-id="37f49-101">tooconnect through hello serial console</span></span>
1. <span data-ttu-id="37f49-102">Csatlakoztassa a soros kábelt toohello eszköz (közvetlenül vagy egy USB – soros adapteren keresztül).</span><span class="sxs-lookup"><span data-stu-id="37f49-102">Connect your serial cable toohello device (directly or through a USB-serial adapter).</span></span>
2. <span data-ttu-id="37f49-103">Nyissa meg hello **Vezérlőpult**, majd nyissa meg a hello **Eszközkezelő**.</span><span class="sxs-lookup"><span data-stu-id="37f49-103">Open hello **Control Panel**, and then open hello **Device Manager**.</span></span>
3. <span data-ttu-id="37f49-104">Azonosítsa hello COM-port a hello a következő ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="37f49-104">Identify hello COM port as shown in hello following illustration.</span></span>
   
     ![Csatlakozás soros konzolon keresztül](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. <span data-ttu-id="37f49-106">Indítsa el a PuTTY alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="37f49-106">Start PuTTY.</span></span> 
5. <span data-ttu-id="37f49-107">Hello jobb oldali ablaktáblában módosíthatja a hello **kapcsolattípus** túl**soros**.</span><span class="sxs-lookup"><span data-stu-id="37f49-107">In hello right pane, change hello **Connection type** too**Serial**.</span></span>
6. <span data-ttu-id="37f49-108">Hello jobb oldali ablaktáblában írja be a hello megfelelő COM-portot.</span><span class="sxs-lookup"><span data-stu-id="37f49-108">In hello right pane, type hello appropriate COM port.</span></span> <span data-ttu-id="37f49-109">Győződjön meg arról, hogy hello soros konfiguráció paraméterei az alábbiak szerint vannak-e beállítva:</span><span class="sxs-lookup"><span data-stu-id="37f49-109">Make sure that hello serial configuration parameters are set as follows:</span></span>
   
   * <span data-ttu-id="37f49-110">Sebesség: 115 200</span><span class="sxs-lookup"><span data-stu-id="37f49-110">Speed: 115,200</span></span>
   * <span data-ttu-id="37f49-111">Adatbitek: 8</span><span class="sxs-lookup"><span data-stu-id="37f49-111">Data bits: 8</span></span>
   * <span data-ttu-id="37f49-112">Stopbitek: 1</span><span class="sxs-lookup"><span data-stu-id="37f49-112">Stop bits: 1</span></span>
   * <span data-ttu-id="37f49-113">Paritás: Nincs</span><span class="sxs-lookup"><span data-stu-id="37f49-113">Parity: None</span></span>
   * <span data-ttu-id="37f49-114">Folyamatvezérlés: Nincs.</span><span class="sxs-lookup"><span data-stu-id="37f49-114">Flow control: None</span></span>
     
     <span data-ttu-id="37f49-115">Ezek a beállítások hello a következő ábrán láthatók.</span><span class="sxs-lookup"><span data-stu-id="37f49-115">These settings are shown in hello following illustration.</span></span>
     
     ![PuTTY-beállítások](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > <span data-ttu-id="37f49-117">Ha hello alapértelmezett folyamatvezérlési beállítás nem működik, próbálja meg hello adatfolyam vezérlés tooXON/XOFF értékre.</span><span class="sxs-lookup"><span data-stu-id="37f49-117">If hello default flow control setting does not work, try setting hello flow control tooXON/XOFF.</span></span>
     > 
     > 
7. <span data-ttu-id="37f49-118">Kattintson a **nyitott** toostart soros munkamenet.</span><span class="sxs-lookup"><span data-stu-id="37f49-118">Click **Open** toostart a serial session.</span></span>

