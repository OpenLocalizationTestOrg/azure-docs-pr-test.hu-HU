## <a name="set-up-azure-powershell-for-azure-dns"></a><span data-ttu-id="7c73c-101">Az Azure DNS az Azure PowerShell beállítása</span><span class="sxs-lookup"><span data-stu-id="7c73c-101">Set up Azure PowerShell for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="7c73c-102">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="7c73c-102">Before you begin</span></span>

<span data-ttu-id="7c73c-103">Győződjön meg arról, hogy rendelkezik-e elemek a konfigurációs megkezdése előtt a következő hello.</span><span class="sxs-lookup"><span data-stu-id="7c73c-103">Verify that you have hello following items before beginning your configuration.</span></span>

* <span data-ttu-id="7c73c-104">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="7c73c-104">An Azure subscription.</span></span> <span data-ttu-id="7c73c-105">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7c73c-105">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="7c73c-106">Tooinstall hello legújabb verziójára hello Azure Resource Manager PowerShell-parancsmagok van szüksége.</span><span class="sxs-lookup"><span data-stu-id="7c73c-106">You need tooinstall hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="7c73c-107">További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="7c73c-107">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span>

### <a name="sign-in-tooyour-azure-account"></a><span data-ttu-id="7c73c-108">Jelentkezzen be tooyour Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="7c73c-108">Sign in tooyour Azure account</span></span>

<span data-ttu-id="7c73c-109">Nyissa meg a PowerShell-konzolt, és csatlakozzon a tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="7c73c-109">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="7c73c-110">További információkért lásd: [PowerShell használata a Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="7c73c-110">For more information, see [Using PowerShell with Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="select-hello-subscription"></a><span data-ttu-id="7c73c-111">Válassza ki a hello előfizetés</span><span class="sxs-lookup"><span data-stu-id="7c73c-111">Select hello subscription</span></span>
 
<span data-ttu-id="7c73c-112">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="7c73c-112">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="7c73c-113">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="7c73c-113">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="7c73c-114">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="7c73c-114">Create a resource group</span></span>

<span data-ttu-id="7c73c-115">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="7c73c-115">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="7c73c-116">Ezen a helyen az erőforráscsoport erőforrások lesz hello alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="7c73c-116">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="7c73c-117">Azonban mivel minden DNS-erőforrás globális, nem pedig regionális, hello választás az erőforráscsoport helye nincs hatással van az Azure DNS szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="7c73c-117">However, because all DNS resources are global, not regional, hello choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="7c73c-118">Ezt a lépést kihagyhatja, ha egy meglévő erőforráscsoportot használ.</span><span class="sxs-lookup"><span data-stu-id="7c73c-118">You can skip this step if you are using an existing resource group.</span></span>

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a><span data-ttu-id="7c73c-119">Erőforrás-szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="7c73c-119">Register resource provider</span></span>

<span data-ttu-id="7c73c-120">hello Azure DNS-szolgáltatás hello Microsoft.Network erőforrás-szolgáltató kezeli.</span><span class="sxs-lookup"><span data-stu-id="7c73c-120">hello Azure DNS service is managed by hello Microsoft.Network resource provider.</span></span> <span data-ttu-id="7c73c-121">Az Azure-előfizetéssel kell regisztrált toouse az erőforrás-szolgáltató Azure DNS használata előtt.</span><span class="sxs-lookup"><span data-stu-id="7c73c-121">Your Azure subscription must be registered toouse this resource provider before you can use Azure DNS.</span></span> <span data-ttu-id="7c73c-122">Ez a műveletet minden egyes előfizetés esetén csak egyszer kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="7c73c-122">This is a one-time operation for each subscription.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```