## <a name="incremental-and-complete-deployments"></a><span data-ttu-id="25f3c-101">Növekményes és teljes központi telepítések</span><span class="sxs-lookup"><span data-stu-id="25f3c-101">Incremental and complete deployments</span></span>
<span data-ttu-id="25f3c-102">Az erőforrások való telepítésekor, adja meg, hogy hello telepítési növekményes frissítés vagy a teljes frissítés.</span><span class="sxs-lookup"><span data-stu-id="25f3c-102">When deploying your resources, you specify that hello deployment is either an incremental update or a complete update.</span></span> <span data-ttu-id="25f3c-103">hello elsődleges e két mód közötti különbség miként kezeli az erőforrás-kezelő a meglévő erőforrásokat hello erőforráscsoportban, amelyek nincsenek hello sablonban:</span><span class="sxs-lookup"><span data-stu-id="25f3c-103">hello primary difference between these two modes is how Resource Manager handles existing resources in hello resource group that are not in hello template:</span></span>

* <span data-ttu-id="25f3c-104">Teljes módban, erőforrás-kezelő **törli** hello erőforráscsoportban léteznek, de nincs megadva hello sablon az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="25f3c-104">In complete mode, Resource Manager **deletes** resources that exist in hello resource group but are not specified in hello template.</span></span> 
* <span data-ttu-id="25f3c-105">Növekményes módban, erőforrás-kezelő **hagyja változatlanul** hello erőforráscsoportban léteznek, de nincs megadva hello sablon az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="25f3c-105">In incremental mode, Resource Manager **leaves unchanged** resources that exist in hello resource group but are not specified in hello template.</span></span>

<span data-ttu-id="25f3c-106">Mindkét módnál erőforrás-kezelő kísérel meg tooprovision hello sablonban megadott összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="25f3c-106">For both modes, Resource Manager attempts tooprovision all resources specified in hello template.</span></span> <span data-ttu-id="25f3c-107">Ha hello erőforrás már létezik egy hello erőforráscsoportban, és a beállítások nem változnak, hello műveletet okoz nincs változás.</span><span class="sxs-lookup"><span data-stu-id="25f3c-107">If hello resource already exists in hello resource group and its settings are unchanged, hello operation results in no change.</span></span> <span data-ttu-id="25f3c-108">Ha egy erőforrás hello beállításainak módosításához hello erőforrás ki van építve a új beállítások.</span><span class="sxs-lookup"><span data-stu-id="25f3c-108">If you change hello settings for a resource, hello resource is provisioned with those new settings.</span></span> <span data-ttu-id="25f3c-109">Ha úgy próbálja tooupdate hello helyet vagy egy meglévő erőforrás típusát, hello telepítése sikertelen, hiba történt.</span><span class="sxs-lookup"><span data-stu-id="25f3c-109">If you attempt tooupdate hello location or type of an existing resource, hello deployment fails with an error.</span></span> <span data-ttu-id="25f3c-110">Ehelyett egy új erőforrást hello hellyel rendelkező telepítéséhez, vagy írja be, hogy szüksége van.</span><span class="sxs-lookup"><span data-stu-id="25f3c-110">Instead, deploy a new resource with hello location or type that you need.</span></span>

<span data-ttu-id="25f3c-111">Alapértelmezés szerint az erőforrás-kezelő hello növekményes módot használ.</span><span class="sxs-lookup"><span data-stu-id="25f3c-111">By default, Resource Manager uses hello incremental mode.</span></span>

<span data-ttu-id="25f3c-112">tooillustrate hello különbségének növekményes és teljes üzemmódot, fontolja meg a következő forgatókönyv hello.</span><span class="sxs-lookup"><span data-stu-id="25f3c-112">tooillustrate hello difference between incremental and complete modes, consider hello following scenario.</span></span>

<span data-ttu-id="25f3c-113">**Meglévő erőforráscsoport** tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="25f3c-113">**Existing Resource Group** contains:</span></span>

* <span data-ttu-id="25f3c-114">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="25f3c-114">Resource A</span></span>
* <span data-ttu-id="25f3c-115">B erőforrás</span><span class="sxs-lookup"><span data-stu-id="25f3c-115">Resource B</span></span>
* <span data-ttu-id="25f3c-116">Erőforrás C</span><span class="sxs-lookup"><span data-stu-id="25f3c-116">Resource C</span></span>

<span data-ttu-id="25f3c-117">**Sablon** határozza meg:</span><span class="sxs-lookup"><span data-stu-id="25f3c-117">**Template** defines:</span></span>

* <span data-ttu-id="25f3c-118">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="25f3c-118">Resource A</span></span>
* <span data-ttu-id="25f3c-119">B erőforrás</span><span class="sxs-lookup"><span data-stu-id="25f3c-119">Resource B</span></span>
* <span data-ttu-id="25f3c-120">D erőforrás</span><span class="sxs-lookup"><span data-stu-id="25f3c-120">Resource D</span></span>

<span data-ttu-id="25f3c-121">Ha telepítve **növekményes** módban hello erőforráscsoport tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="25f3c-121">When deployed in **incremental** mode, hello resource group contains:</span></span>

* <span data-ttu-id="25f3c-122">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="25f3c-122">Resource A</span></span>
* <span data-ttu-id="25f3c-123">B erőforrás</span><span class="sxs-lookup"><span data-stu-id="25f3c-123">Resource B</span></span>
* <span data-ttu-id="25f3c-124">Erőforrás C</span><span class="sxs-lookup"><span data-stu-id="25f3c-124">Resource C</span></span>
* <span data-ttu-id="25f3c-125">D erőforrás</span><span class="sxs-lookup"><span data-stu-id="25f3c-125">Resource D</span></span>

<span data-ttu-id="25f3c-126">Ha telepítve **teljes** mód, erőforrás-C törlődik.</span><span class="sxs-lookup"><span data-stu-id="25f3c-126">When deployed in **complete** mode, Resource C is deleted.</span></span> <span data-ttu-id="25f3c-127">hello erőforráscsoport tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="25f3c-127">hello resource group contains:</span></span>

* <span data-ttu-id="25f3c-128">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="25f3c-128">Resource A</span></span>
* <span data-ttu-id="25f3c-129">B erőforrás</span><span class="sxs-lookup"><span data-stu-id="25f3c-129">Resource B</span></span>
* <span data-ttu-id="25f3c-130">D erőforrás</span><span class="sxs-lookup"><span data-stu-id="25f3c-130">Resource D</span></span>
