<!--author=alkohli last changed: 06/22/17-->

#### <a name="to-create-a-volume-container"></a><span data-ttu-id="4c240-101">Kötettároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c240-101">To create a volume container</span></span>
1. <span data-ttu-id="4c240-102">A StorSimple-eszközkezelő szolgáltatásban kattintson az **Eszközök** elemre.</span><span class="sxs-lookup"><span data-stu-id="4c240-102">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="4c240-103">Az eszközök táblázatos listájából válassza ki az eszközt, majd kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="4c240-103">From the tabular listing of the devices, select and click a device.</span></span> 

    ![Kötettároló panel](./media/storsimple-8000-create-volume-container/createvolumecontainer1.png)

2. <span data-ttu-id="4c240-105">Az eszköz irányítópultján kattintson a **+ Kötettároló hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c240-105">In the device dashboard, click **+ Add volume container**</span></span>

    ![Kötettároló panel](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

3. <span data-ttu-id="4c240-107">A **Kötettároló hozzáadása** panelen:</span><span class="sxs-lookup"><span data-stu-id="4c240-107">In the **Add volume container** blade:</span></span>
   
   1. <span data-ttu-id="4c240-108">Az eszköz automatikusan ki lesz választva.</span><span class="sxs-lookup"><span data-stu-id="4c240-108">The device is automatically selected.</span></span>
   2. <span data-ttu-id="4c240-109">Adja meg a kötettároló **nevét**.</span><span class="sxs-lookup"><span data-stu-id="4c240-109">Supply a **Name** for your volume container.</span></span> <span data-ttu-id="4c240-110">A név 3–32 karakter hosszúságú lehet.</span><span class="sxs-lookup"><span data-stu-id="4c240-110">The name must be 3 to 32 characters long.</span></span> <span data-ttu-id="4c240-111">A kötettárolók nem nevezhetők át a létrehozásuk után.</span><span class="sxs-lookup"><span data-stu-id="4c240-111">You cannot rename a volume container once it is created.</span></span>
   3. <span data-ttu-id="4c240-112">A **Felhőalapú tárolás titkosításának engedélyezése** lehetőséggel engedélyezheti az eszközről a felhőbe küldött adatok titkosítását.</span><span class="sxs-lookup"><span data-stu-id="4c240-112">Select **Enable Cloud Storage Encryption** to enable encryption of the data sent from the device to the cloud.</span></span>
   4. <span data-ttu-id="4c240-113">Adja meg a **Felhőalapú tárolás titkosítási kulcsát**, amely 8–32 karakter hosszúságú lehet.</span><span class="sxs-lookup"><span data-stu-id="4c240-113">Provide and confirm a **Cloud Storage Encryption Key** that is 8 to 32 characters long.</span></span> <span data-ttu-id="4c240-114">Ezt a kulcsot az eszköz a titkosított adatok hozzáféréséhez használja.</span><span class="sxs-lookup"><span data-stu-id="4c240-114">This key is used by the device to access encrypted data.</span></span>
   5. <span data-ttu-id="4c240-115">Válasszon egy **tárfiókot**, amelyet ehhez a kötettárolóhoz kíván társítani.</span><span class="sxs-lookup"><span data-stu-id="4c240-115">Select a **Storage Account** to associate with this volume container.</span></span> <span data-ttu-id="4c240-116">Választhat egy meglévő tárfiókot vagy kiválaszthatja az alapértelmezett fiókot is, amely a szolgáltatás létrehozásakor jön létre.</span><span class="sxs-lookup"><span data-stu-id="4c240-116">You can choose an existing storage account or the default account that is generated at the time of service creation.</span></span> <span data-ttu-id="4c240-117">Használhatja az **Új hozzáadása** lehetőséget is. Ezzel megadhat egy olyan tárfiókot, amely nincs ehhez a szolgáltatás-előfizetéshez kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="4c240-117">You can also use the **Add new** option to specify a storage account that is not linked to this service subscription.</span></span>
   6. <span data-ttu-id="4c240-118">A **Sávszélesség megadása** legördülő listában válassza a **Korlátlan** lehetőséget, ha az összes rendelkezésre álló sávszélességet fel kívánja használni.</span><span class="sxs-lookup"><span data-stu-id="4c240-118">Select **Unlimited** in the **Specify bandwidth** drop-down list if you wish to consume all the available bandwidth.</span></span> <span data-ttu-id="4c240-119">Ehhez a beállításhoz megadhatja az **Egyéni** értéket is, ha sávszélesség-korlátozást szeretne alkalmazni. 1 MB/s és 1000 Mb/s közötti értéket adhat meg.</span><span class="sxs-lookup"><span data-stu-id="4c240-119">You can also set this option to **Custom** to employ bandwidth controls, and specify a value between 1 Mbps and 1,000 Mbps.</span></span>
      <span data-ttu-id="4c240-120">Ha hozzáfér a sávszélesség-használati adatokhoz, lehetősége van a sávszélesség ütemezett lefoglalására a **Sávszélességsablon kiválasztása** beállítás megadásával.</span><span class="sxs-lookup"><span data-stu-id="4c240-120">If you have your bandwidth usage information available, you may be able to allocate bandwidth based on a schedule by specifying **Select a bandwidth template**.</span></span> <span data-ttu-id="4c240-121">Az eljárás pontos lépéseit a [Sávszélességsablon hozzáadása](../articles/storsimple/storsimple-8000-manage-bandwidth-templates.md#add-a-bandwidth-template) szakaszban találja.</span><span class="sxs-lookup"><span data-stu-id="4c240-121">For a step-by-step procedure, go to [Add a bandwidth template](../articles/storsimple/storsimple-8000-manage-bandwidth-templates.md#add-a-bandwidth-template).</span></span>

      ![Kötettároló panel](./media/storsimple-8000-create-volume-container/createvolumecontainer6b.png)
   7. <span data-ttu-id="4c240-123">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c240-123">Click **Create**.</span></span>

        ![Kötettároló panel](./media/storsimple-8000-create-volume-container/createvolumecontainer6.png)
   
       <span data-ttu-id="4c240-125">A kötettároló sikeres létrehozásáról értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="4c240-125">You are notified when the volume container is successfully created.</span></span>

       ![Kötettároló létrehozási értesítése](./media/storsimple-8000-create-volume-container/createvolumecontainer8.png)

   <span data-ttu-id="4c240-127">Az újonnan létrehozott kötettároló megjelenik az eszköz kötettárolóinak listájában.</span><span class="sxs-lookup"><span data-stu-id="4c240-127">The newly created volume container is listed in the list of volume containers for your device.</span></span>

   ![Kötettároló hozzáadása panel](./media/storsimple-8000-create-volume-container/createvolumecontainer9.png)


