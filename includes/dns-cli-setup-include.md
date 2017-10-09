## <a name="set-up-azure-cli-for-azure-dns"></a><span data-ttu-id="a1b8e-101">Az Azure parancssori felület (CLI) beállítása az Azure DNS-hez</span><span class="sxs-lookup"><span data-stu-id="a1b8e-101">Set up Azure CLI for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="a1b8e-102">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="a1b8e-102">Before you begin</span></span>

<span data-ttu-id="a1b8e-103">Győződjön meg arról, hogy rendelkezik-e elemek a konfigurációs megkezdése előtt a következő hello.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-103">Verify that you have hello following items before beginning your configuration.</span></span>

* <span data-ttu-id="a1b8e-104">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-104">An Azure subscription.</span></span> <span data-ttu-id="a1b8e-105">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a1b8e-105">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="a1b8e-106">Hello hello Azure parancssori felület, elérhető a Windows, Linux vagy Mac legújabb verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="a1b8e-106">Install hello latest version of hello Azure CLI, available for Windows, Linux, or MAC.</span></span> <span data-ttu-id="a1b8e-107">További információt [telepítés hello Azure CLI](../articles/cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="a1b8e-107">More information is available at [Install hello Azure CLI](../articles/cli-install-nodejs.md).</span></span>

### <a name="sign-in-tooyour-azure-account"></a><span data-ttu-id="a1b8e-108">Jelentkezzen be tooyour Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="a1b8e-108">Sign in tooyour Azure account</span></span>

<span data-ttu-id="a1b8e-109">Nyisson meg egy konzolablakot, adja meg a saját hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-109">Open a console window and authenticate with your credentials.</span></span> <span data-ttu-id="a1b8e-110">További információkért lásd: [tooAzure a hello Azure CLI-e jelentkezni](../articles/xplat-cli-connect.md)</span><span class="sxs-lookup"><span data-stu-id="a1b8e-110">For more information, see [Log in tooAzure from hello Azure CLI](../articles/xplat-cli-connect.md)</span></span>

```azurecli
azure login
```

### <a name="switch-cli-mode"></a><span data-ttu-id="a1b8e-111">Kapcsolja át a parancssori felület működési módját</span><span class="sxs-lookup"><span data-stu-id="a1b8e-111">Switch CLI mode</span></span>

<span data-ttu-id="a1b8e-112">Az Azure DNS az Azure Resource Managert használja.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-112">Azure DNS uses Azure Resource Manager.</span></span> <span data-ttu-id="a1b8e-113">Váltson át CLI mód toouse Azure Resource Manager parancsok.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-113">Make sure you switch CLI mode toouse Azure Resource Manager commands.</span></span>

```azurecli
azure config mode arm
```

### <a name="select-hello-subscription"></a><span data-ttu-id="a1b8e-114">Válassza ki a hello előfizetés</span><span class="sxs-lookup"><span data-stu-id="a1b8e-114">Select hello subscription</span></span>

<span data-ttu-id="a1b8e-115">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-115">Check hello subscriptions for hello account.</span></span>

```azurecli
azure account list
```

<span data-ttu-id="a1b8e-116">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-116">Choose which of your Azure subscriptions toouse.</span></span>

```azurecli
azure account set "subscription name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="a1b8e-117">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="a1b8e-117">Create a resource group</span></span>

<span data-ttu-id="a1b8e-118">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-118">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="a1b8e-119">Ez az erőforráscsoport erőforrások hello alapértelmezett helye szolgál.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-119">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="a1b8e-120">Azonban mivel minden DNS-erőforrás globális, nem pedig regionális, hello választás az erőforráscsoport helye nincs hatással van az Azure DNS szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-120">However, because all DNS resources are global, not regional, hello choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="a1b8e-121">Ezt a lépést kihagyhatja, ha egy meglévő erőforráscsoportot használ.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-121">You can skip this step if you are using an existing resource group.</span></span>

```azurecli
azure group create -n myresourcegroup --location "West US"
```

### <a name="register-resource-provider"></a><span data-ttu-id="a1b8e-122">Erőforrás-szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="a1b8e-122">Register resource provider</span></span>

<span data-ttu-id="a1b8e-123">hello Azure DNS-szolgáltatás hello Microsoft.Network erőforrás-szolgáltató kezeli.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-123">hello Azure DNS service is managed by hello Microsoft.Network resource provider.</span></span> <span data-ttu-id="a1b8e-124">Az Azure-előfizetéssel kell regisztrált toouse az erőforrás-szolgáltató Azure DNS használata előtt.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-124">Your Azure subscription must be registered toouse this resource provider before you can use Azure DNS.</span></span> <span data-ttu-id="a1b8e-125">Ez a műveletet minden egyes előfizetés esetén csak egyszer kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="a1b8e-125">This is a one-time operation for each subscription.</span></span>

```azurecli
azure provider register --namespace Microsoft.Network
```

