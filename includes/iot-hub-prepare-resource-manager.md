## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a><span data-ttu-id="12f56-101">Felkészülés az Azure Resource Manager-kérelmek hitelesítéséhez szükséges</span><span class="sxs-lookup"><span data-stu-id="12f56-101">Prepare to authenticate Azure Resource Manager requests</span></span>
<span data-ttu-id="12f56-102">A műveleteket hajt végre eszközzel kell hitelesítenie a [Azure Resource Manager] [ lnk-authenticate-arm] az Azure Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="12f56-102">You must authenticate all the operations that you perform on resources using the [Azure Resource Manager][lnk-authenticate-arm] with Azure Active Directory (AD).</span></span> <span data-ttu-id="12f56-103">A legegyszerűbben úgy konfigurálhatja ezt a PowerShell vagy Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="12f56-103">The easiest way to configure this is to use PowerShell or Azure CLI.</span></span>

<span data-ttu-id="12f56-104">Telepítse a [Azure PowerShell-parancsmagok] [ lnk-powershell-install] a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="12f56-104">Install the [Azure PowerShell cmdlets][lnk-powershell-install] before you continue.</span></span>

<span data-ttu-id="12f56-105">A következő lépések bemutatják, hogyan állíthat be egy PowerShell használatával AD-alkalmazást a jelszó-hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="12f56-105">The following steps show how to set up password authentication for an AD application using PowerShell.</span></span> <span data-ttu-id="12f56-106">Ezek a parancsok a szabványos PowerShell-munkamenetben futtathatja.</span><span class="sxs-lookup"><span data-stu-id="12f56-106">You can run these commands in a standard PowerShell session.</span></span>

1. <span data-ttu-id="12f56-107">Jelentkezzen be az Azure-előfizetéshez a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="12f56-107">Log in to your Azure subscription using the following command:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="12f56-108">Ha több Azure-előfizetéssel rendelkezik, a jelentkezik be az Azure ad hozzáférést az összes Azure-előfizetést a hitelesítő adatok társított.</span><span class="sxs-lookup"><span data-stu-id="12f56-108">If you have multiple Azure subscriptions, signing in to Azure grants you access to all the Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="12f56-109">Használja a következő parancsot a rendelkezésre álló használata Azure-előfizetések listázásához:</span><span class="sxs-lookup"><span data-stu-id="12f56-109">Use the following command to list the Azure subscriptions available for you to use:</span></span>

    ```powershell
    Get-AzureRMSubscription
    ```

    <span data-ttu-id="12f56-110">A következő parancs segítségével válassza ki, hogy az IoT hub kezelésére szolgáló parancsok futtatásához használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="12f56-110">Use the following command to select subscription that you want to use to run the commands to manage your IoT hub.</span></span> <span data-ttu-id="12f56-111">Az előfizetés neve vagy azonosítója is használhatja, ha az előző parancs kimenetében:</span><span class="sxs-lookup"><span data-stu-id="12f56-111">You can use either the subscription name or ID from the output of the previous command:</span></span>

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. <span data-ttu-id="12f56-112">Jegyezze fel a **TenantId** és **SubscriptionId**.</span><span class="sxs-lookup"><span data-stu-id="12f56-112">Make a note of your **TenantId** and **SubscriptionId**.</span></span> <span data-ttu-id="12f56-113">Ezeket később szüksége.</span><span class="sxs-lookup"><span data-stu-id="12f56-113">You need them later.</span></span>
3. <span data-ttu-id="12f56-114">Hozzon létre egy új Azure Active Directory-alkalmazás helyett a helyi tartozó felhasználók számára, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="12f56-114">Create a new Azure Active Directory application using the following command, replacing the place holders:</span></span>
   
   * <span data-ttu-id="12f56-115">**{Név}:** egy megjelenítési nevet az alkalmazásnak, többek között **MySampleApp**</span><span class="sxs-lookup"><span data-stu-id="12f56-115">**{Display name}:** a display name for your application such as **MySampleApp**</span></span>
   * <span data-ttu-id="12f56-116">**{Kezdőlap URL-cím}:** a kezdőlap, például az alkalmazás URL-CÍMÉT **http://mysampleapp/home**.</span><span class="sxs-lookup"><span data-stu-id="12f56-116">**{Home page URL}:** the URL of the home page of your app such as **http://mysampleapp/home**.</span></span> <span data-ttu-id="12f56-117">Az URL-cím nem kell egy valós alkalmazás mutasson.</span><span class="sxs-lookup"><span data-stu-id="12f56-117">This URL does not need to point to a real application.</span></span>
   * <span data-ttu-id="12f56-118">**{Alkalmazásazonosító}:** egyedi azonosítója, mint **http://mysampleapp**.</span><span class="sxs-lookup"><span data-stu-id="12f56-118">**{Application identifier}:** A unique identifier such as **http://mysampleapp**.</span></span> <span data-ttu-id="12f56-119">Az URL-cím nem kell egy valós alkalmazás mutasson.</span><span class="sxs-lookup"><span data-stu-id="12f56-119">This URL does not need to point to a real application.</span></span>
   * <span data-ttu-id="12f56-120">**{Jelszó}:** az alkalmazás hitelesítéséhez használt jelszót.</span><span class="sxs-lookup"><span data-stu-id="12f56-120">**{Password}:** A password that you use to authenticate with your app.</span></span>
     
     ```powershell
     New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
     ```
4. <span data-ttu-id="12f56-121">Jegyezze fel a **ApplicationId** létrehozott alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="12f56-121">Make a note of the **ApplicationId** of the application you created.</span></span> <span data-ttu-id="12f56-122">Később szüksége.</span><span class="sxs-lookup"><span data-stu-id="12f56-122">You need this later.</span></span>
5. <span data-ttu-id="12f56-123">A következő parancsot, hogy új szolgáltatásnevet létrehozni **{MyApplicationId}** rendelkező a **ApplicationId** az előző lépésben:</span><span class="sxs-lookup"><span data-stu-id="12f56-123">Create a new service principal using the following command, replacing **{MyApplicationId}** with the **ApplicationId** from the previous step:</span></span>
   
    ```powershell
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. <span data-ttu-id="12f56-124">Állítsa be a következő parancsot, hogy a szerepkör-hozzárendelés **{MyApplicationId}** rendelkező a **ApplicationId**.</span><span class="sxs-lookup"><span data-stu-id="12f56-124">Set up a role assignment using the following command, replacing **{MyApplicationId}** with your **ApplicationId**.</span></span>
   
    ```powershell
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

<span data-ttu-id="12f56-125">Most már befejezte az Azure AD-alkalmazás, amely lehetővé teszi, hogy a hitelesítésre, az egyéni C#-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="12f56-125">You have now finished creating the Azure AD application that enables you to authenticate from your custom C# application.</span></span> <span data-ttu-id="12f56-126">Az oktatóanyag későbbi részében szüksége a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="12f56-126">You need the following values later in this tutorial:</span></span>

* <span data-ttu-id="12f56-127">A TenantId</span><span class="sxs-lookup"><span data-stu-id="12f56-127">TenantId</span></span>
* <span data-ttu-id="12f56-128">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="12f56-128">SubscriptionId</span></span>
* <span data-ttu-id="12f56-129">ApplicationId</span><span class="sxs-lookup"><span data-stu-id="12f56-129">ApplicationId</span></span>
* <span data-ttu-id="12f56-130">Jelszó</span><span class="sxs-lookup"><span data-stu-id="12f56-130">Password</span></span>

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
