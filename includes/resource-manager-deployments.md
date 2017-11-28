## <a name="incremental-and-complete-deployments"></a><span data-ttu-id="eba5a-101">Növekményes és teljes központi telepítések</span><span class="sxs-lookup"><span data-stu-id="eba5a-101">Incremental and complete deployments</span></span>
<span data-ttu-id="eba5a-102">Az erőforrások telepítésekor Megadja, hogy a központi telepítés egy növekményes frissítés vagy egy teljes frissítés.</span><span class="sxs-lookup"><span data-stu-id="eba5a-102">When deploying your resources, you specify that the deployment is either an incremental update or a complete update.</span></span> <span data-ttu-id="eba5a-103">E két mód között az elsődleges különbség az, hogyan kezeli az erőforrás-kezelő a meglévő erőforrások az erőforráscsoportban, amelyek nincsenek a sablonban:</span><span class="sxs-lookup"><span data-stu-id="eba5a-103">The primary difference between these two modes is how Resource Manager handles existing resources in the resource group that are not in the template:</span></span>

* <span data-ttu-id="eba5a-104">Teljes módban, erőforrás-kezelő **törli** erőforrásokat, az erőforráscsoport szerepel, de nincs megadva a sablonban.</span><span class="sxs-lookup"><span data-stu-id="eba5a-104">In complete mode, Resource Manager **deletes** resources that exist in the resource group but are not specified in the template.</span></span> 
* <span data-ttu-id="eba5a-105">Növekményes módban, erőforrás-kezelő **hagyja változatlanul** erőforrásokat, az erőforráscsoport szerepel, de nincs megadva a sablonban.</span><span class="sxs-lookup"><span data-stu-id="eba5a-105">In incremental mode, Resource Manager **leaves unchanged** resources that exist in the resource group but are not specified in the template.</span></span>

<span data-ttu-id="eba5a-106">Mindkét módnál az erőforrás-kezelő megpróbálja a sablonban megadott összes erőforrások biztosításához.</span><span class="sxs-lookup"><span data-stu-id="eba5a-106">For both modes, Resource Manager attempts to provision all resources specified in the template.</span></span> <span data-ttu-id="eba5a-107">Ha az erőforrás már létezik az erőforráscsoporthoz tartozik, és a beállítások nem változnak, a művelet nincs változás eredményezi.</span><span class="sxs-lookup"><span data-stu-id="eba5a-107">If the resource already exists in the resource group and its settings are unchanged, the operation results in no change.</span></span> <span data-ttu-id="eba5a-108">Ha egy erőforrás beállításait módosítja, az erőforrás ki van építve a új beállítások.</span><span class="sxs-lookup"><span data-stu-id="eba5a-108">If you change the settings for a resource, the resource is provisioned with those new settings.</span></span> <span data-ttu-id="eba5a-109">Ha úgy próbálja frissíteni a helyet vagy a meglévő erőforrás típusa, a telepítés sikertelen, a hibaüzenet.</span><span class="sxs-lookup"><span data-stu-id="eba5a-109">If you attempt to update the location or type of an existing resource, the deployment fails with an error.</span></span> <span data-ttu-id="eba5a-110">Ehelyett egy új erőforrást a hely telepítése, vagy adjon meg, hogy van szüksége.</span><span class="sxs-lookup"><span data-stu-id="eba5a-110">Instead, deploy a new resource with the location or type that you need.</span></span>

<span data-ttu-id="eba5a-111">Alapértelmezés szerint az erőforrás-kezelő a növekményes módot használ.</span><span class="sxs-lookup"><span data-stu-id="eba5a-111">By default, Resource Manager uses the incremental mode.</span></span>

<span data-ttu-id="eba5a-112">Növekményes és teljes mód közötti különbséget mutatja be, a következő eset.</span><span class="sxs-lookup"><span data-stu-id="eba5a-112">To illustrate the difference between incremental and complete modes, consider the following scenario.</span></span>

<span data-ttu-id="eba5a-113">**Meglévő erőforráscsoport** tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="eba5a-113">**Existing Resource Group** contains:</span></span>

* <span data-ttu-id="eba5a-114">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="eba5a-114">Resource A</span></span>
* <span data-ttu-id="eba5a-115">B erőforrás</span><span class="sxs-lookup"><span data-stu-id="eba5a-115">Resource B</span></span>
* <span data-ttu-id="eba5a-116">Erőforrás C</span><span class="sxs-lookup"><span data-stu-id="eba5a-116">Resource C</span></span>

<span data-ttu-id="eba5a-117">**Sablon** határozza meg:</span><span class="sxs-lookup"><span data-stu-id="eba5a-117">**Template** defines:</span></span>

* <span data-ttu-id="eba5a-118">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="eba5a-118">Resource A</span></span>
* <span data-ttu-id="eba5a-119">B erőforrás</span><span class="sxs-lookup"><span data-stu-id="eba5a-119">Resource B</span></span>
* <span data-ttu-id="eba5a-120">D erőforrás</span><span class="sxs-lookup"><span data-stu-id="eba5a-120">Resource D</span></span>

<span data-ttu-id="eba5a-121">Ha telepítve **növekményes** módot, az erőforráscsoport tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="eba5a-121">When deployed in **incremental** mode, the resource group contains:</span></span>

* <span data-ttu-id="eba5a-122">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="eba5a-122">Resource A</span></span>
* <span data-ttu-id="eba5a-123">B erőforrás</span><span class="sxs-lookup"><span data-stu-id="eba5a-123">Resource B</span></span>
* <span data-ttu-id="eba5a-124">Erőforrás C</span><span class="sxs-lookup"><span data-stu-id="eba5a-124">Resource C</span></span>
* <span data-ttu-id="eba5a-125">D erőforrás</span><span class="sxs-lookup"><span data-stu-id="eba5a-125">Resource D</span></span>

<span data-ttu-id="eba5a-126">Ha telepítve **teljes** mód, erőforrás-C törlődik.</span><span class="sxs-lookup"><span data-stu-id="eba5a-126">When deployed in **complete** mode, Resource C is deleted.</span></span> <span data-ttu-id="eba5a-127">Az erőforráscsoport tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="eba5a-127">The resource group contains:</span></span>

* <span data-ttu-id="eba5a-128">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="eba5a-128">Resource A</span></span>
* <span data-ttu-id="eba5a-129">B erőforrás</span><span class="sxs-lookup"><span data-stu-id="eba5a-129">Resource B</span></span>
* <span data-ttu-id="eba5a-130">D erőforrás</span><span class="sxs-lookup"><span data-stu-id="eba5a-130">Resource D</span></span>
