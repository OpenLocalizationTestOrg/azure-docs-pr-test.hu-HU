<!--author=alkohli last changed: 06/22/17-->

#### <a name="toocreate-a-volume-container"></a><span data-ttu-id="36bbe-101">a kötettároló toocreate</span><span class="sxs-lookup"><span data-stu-id="36bbe-101">toocreate a volume container</span></span>
1. <span data-ttu-id="36bbe-102">Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="36bbe-102">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="36bbe-103">Válassza ki a hello táblázatos hello eszközök listája, és kattintson arra az eszközre.</span><span class="sxs-lookup"><span data-stu-id="36bbe-103">From hello tabular listing of hello devices, select and click a device.</span></span> 

    ![Kötettároló panel](./media/storsimple-8000-create-volume-container/createvolumecontainer1.png)

2. <span data-ttu-id="36bbe-105">Hello eszköz irányítópulton kattintson **+ Hozzáadás kötettároló**</span><span class="sxs-lookup"><span data-stu-id="36bbe-105">In hello device dashboard, click **+ Add volume container**</span></span>

    ![Kötettároló panel](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

3. <span data-ttu-id="36bbe-107">A hello **Hozzáadás kötettároló** panel:</span><span class="sxs-lookup"><span data-stu-id="36bbe-107">In hello **Add volume container** blade:</span></span>
   
   1. <span data-ttu-id="36bbe-108">hello eszköz automatikusan ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="36bbe-108">hello device is automatically selected.</span></span>
   2. <span data-ttu-id="36bbe-109">Adja meg a kötettároló **nevét**.</span><span class="sxs-lookup"><span data-stu-id="36bbe-109">Supply a **Name** for your volume container.</span></span> <span data-ttu-id="36bbe-110">hello nevének 3 too32 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="36bbe-110">hello name must be 3 too32 characters long.</span></span> <span data-ttu-id="36bbe-111">A kötettárolók nem nevezhetők át a létrehozásuk után.</span><span class="sxs-lookup"><span data-stu-id="36bbe-111">You cannot rename a volume container once it is created.</span></span>
   3. <span data-ttu-id="36bbe-112">Válassza ki **felhőalapú tárolás titkosításának engedélyezése** hello eszköz toohello felhőből küldi hello adatok tooenable titkosítását.</span><span class="sxs-lookup"><span data-stu-id="36bbe-112">Select **Enable Cloud Storage Encryption** tooenable encryption of hello data sent from hello device toohello cloud.</span></span>
   4. <span data-ttu-id="36bbe-113">Adja meg, és erősítse meg a **felhőalapú tárolás titkosítási kulcsát** , amely 8 too32 karakternél hosszabb.</span><span class="sxs-lookup"><span data-stu-id="36bbe-113">Provide and confirm a **Cloud Storage Encryption Key** that is 8 too32 characters long.</span></span> <span data-ttu-id="36bbe-114">Ez a kulcs hello eszköz tooaccess titkosított adatokat használják.</span><span class="sxs-lookup"><span data-stu-id="36bbe-114">This key is used by hello device tooaccess encrypted data.</span></span>
   5. <span data-ttu-id="36bbe-115">Válassza ki a **Tárfiók** tooassociate az ehhez a kötettárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="36bbe-115">Select a **Storage Account** tooassociate with this volume container.</span></span> <span data-ttu-id="36bbe-116">Választhat egy meglévő tárfiók vagy a hello alapértelmezett fiókot, amely hello időben, a szolgáltatás létrehozásakor jön létre.</span><span class="sxs-lookup"><span data-stu-id="36bbe-116">You can choose an existing storage account or hello default account that is generated at hello time of service creation.</span></span> <span data-ttu-id="36bbe-117">Is használhatja a hello **új hozzáadása** beállítás toospecify egy tárfiókot, amely nem kapcsolódó toothis szolgáltatási előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="36bbe-117">You can also use hello **Add new** option toospecify a storage account that is not linked toothis service subscription.</span></span>
   6. <span data-ttu-id="36bbe-118">Válassza ki **korlátlan** a hello **adja meg a sávszélesség** legördülő listában, ha tooconsume hello összes rendelkezésre álló sávszélességet.</span><span class="sxs-lookup"><span data-stu-id="36bbe-118">Select **Unlimited** in hello **Specify bandwidth** drop-down list if you wish tooconsume all hello available bandwidth.</span></span> <span data-ttu-id="36bbe-119">Túl is állítja ezt a beállítást**egyéni** tooemploy sávszélesség-vezérlés, és adjon meg 1 és 1000 MB/s közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="36bbe-119">You can also set this option too**Custom** tooemploy bandwidth controls, and specify a value between 1 Mbps and 1,000 Mbps.</span></span>
      <span data-ttu-id="36bbe-120">Ha a rendelkezésre álló sávszélesség használati információk, esetleg megadásával a ütemezés szerint képes tooallocate sávszélesség **sávszélességsablon kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="36bbe-120">If you have your bandwidth usage information available, you may be able tooallocate bandwidth based on a schedule by specifying **Select a bandwidth template**.</span></span> <span data-ttu-id="36bbe-121">Lépésenkénti útmutatót, nyissa meg túl[sávszélességsablon hozzáadása](../articles/storsimple/storsimple-8000-manage-bandwidth-templates.md#add-a-bandwidth-template).</span><span class="sxs-lookup"><span data-stu-id="36bbe-121">For a step-by-step procedure, go too[Add a bandwidth template](../articles/storsimple/storsimple-8000-manage-bandwidth-templates.md#add-a-bandwidth-template).</span></span>

      ![Kötettároló panel](./media/storsimple-8000-create-volume-container/createvolumecontainer6b.png)
   7. <span data-ttu-id="36bbe-123">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="36bbe-123">Click **Create**.</span></span>

        ![Kötettároló panel](./media/storsimple-8000-create-volume-container/createvolumecontainer6.png)
   
       <span data-ttu-id="36bbe-125">Értesítést kap hello kötettároló létrehozása sikerült.</span><span class="sxs-lookup"><span data-stu-id="36bbe-125">You are notified when hello volume container is successfully created.</span></span>

       ![Kötettároló létrehozási értesítése](./media/storsimple-8000-create-volume-container/createvolumecontainer8.png)

   <span data-ttu-id="36bbe-127">az újonnan létrehozott kötettároló hello, az eszköz kötettárolók hello listája szerepel.</span><span class="sxs-lookup"><span data-stu-id="36bbe-127">hello newly created volume container is listed in hello list of volume containers for your device.</span></span>

   ![Kötettároló hozzáadása panel](./media/storsimple-8000-create-volume-container/createvolumecontainer9.png)


