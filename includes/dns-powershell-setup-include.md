## <a name="set-up-azure-powershell-for-azure-dns"></a><span data-ttu-id="5606a-101">Az Azure DNS az Azure PowerShell beállítása</span><span class="sxs-lookup"><span data-stu-id="5606a-101">Set up Azure PowerShell for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="5606a-102">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="5606a-102">Before you begin</span></span>

<span data-ttu-id="5606a-103">A konfigurálás megkezdése előtt győződjön meg arról, hogy rendelkezik a következőkkel.</span><span class="sxs-lookup"><span data-stu-id="5606a-103">Verify that you have the following items before beginning your configuration.</span></span>

* <span data-ttu-id="5606a-104">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="5606a-104">An Azure subscription.</span></span> <span data-ttu-id="5606a-105">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5606a-105">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="5606a-106">Telepítse a legújabb verziót az Azure Resource Manager PowerShell-parancsmagokat kell.</span><span class="sxs-lookup"><span data-stu-id="5606a-106">You need to install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="5606a-107">További információt [az Azure PowerShell telepítésével és konfigurálásával](/powershell/azureps-cmdlets-docs) foglalkozó témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="5606a-107">For more information, see [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span>

### <a name="sign-in-to-your-azure-account"></a><span data-ttu-id="5606a-108">Jelentkezzen be az Azure-fiókjába</span><span class="sxs-lookup"><span data-stu-id="5606a-108">Sign in to your Azure account</span></span>

<span data-ttu-id="5606a-109">Nyissa meg a PowerShell konzolt, és csatlakozzon a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="5606a-109">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="5606a-110">További információkért lásd: [PowerShell használata a Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="5606a-110">For more information, see [Using PowerShell with Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="select-the-subscription"></a><span data-ttu-id="5606a-111">Válassza ki az előfizetést</span><span class="sxs-lookup"><span data-stu-id="5606a-111">Select the subscription</span></span>
 
<span data-ttu-id="5606a-112">Keresse meg a fiókot az előfizetésekben.</span><span class="sxs-lookup"><span data-stu-id="5606a-112">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="5606a-113">Válassza ki, hogy melyik Azure előfizetést fogja használni.</span><span class="sxs-lookup"><span data-stu-id="5606a-113">Choose which of your Azure subscriptions to use.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="5606a-114">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="5606a-114">Create a resource group</span></span>

<span data-ttu-id="5606a-115">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="5606a-115">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="5606a-116">Ez a hely lesz az erőforráscsoport erőforrásainak alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="5606a-116">This location is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="5606a-117">Mivel azonban minden DNS-erőforrás globális, nem pedig regionális, az erőforráscsoport kiválasztott helye nincs hatással az Azure DNS szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="5606a-117">However, because all DNS resources are global, not regional, the choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="5606a-118">Ezt a lépést kihagyhatja, ha egy meglévő erőforráscsoportot használ.</span><span class="sxs-lookup"><span data-stu-id="5606a-118">You can skip this step if you are using an existing resource group.</span></span>

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a><span data-ttu-id="5606a-119">Erőforrás-szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="5606a-119">Register resource provider</span></span>

<span data-ttu-id="5606a-120">Az Azure DNS szolgáltatást a Microsoft.Network erőforrás-szolgáltató kezeli.</span><span class="sxs-lookup"><span data-stu-id="5606a-120">The Azure DNS service is managed by the Microsoft.Network resource provider.</span></span> <span data-ttu-id="5606a-121">Az Azure DNS használatbavétele előtt az Azure-előfizetést regisztrálni kell ennek az erőforrás-szolgáltatónak a használatához.</span><span class="sxs-lookup"><span data-stu-id="5606a-121">Your Azure subscription must be registered to use this resource provider before you can use Azure DNS.</span></span> <span data-ttu-id="5606a-122">Ez a műveletet minden egyes előfizetés esetén csak egyszer kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="5606a-122">This is a one-time operation for each subscription.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```