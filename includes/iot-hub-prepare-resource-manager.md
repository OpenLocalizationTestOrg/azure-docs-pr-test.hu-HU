## <a name="prepare-tooauthenticate-azure-resource-manager-requests"></a><span data-ttu-id="e18da-101">Azure Resource Manager-kérelmek tooauthenticate előkészítése</span><span class="sxs-lookup"><span data-stu-id="e18da-101">Prepare tooauthenticate Azure Resource Manager requests</span></span>
<span data-ttu-id="e18da-102">Összes hello művelet hajt végre hello eszközzel kell hitelesítenie [Azure Resource Manager] [ lnk-authenticate-arm] az Azure Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="e18da-102">You must authenticate all hello operations that you perform on resources using hello [Azure Resource Manager][lnk-authenticate-arm] with Azure Active Directory (AD).</span></span> <span data-ttu-id="e18da-103">Ennek legegyszerűbb módja tooconfigure hello toouse PowerShell vagy az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="e18da-103">hello easiest way tooconfigure this is toouse PowerShell or Azure CLI.</span></span>

<span data-ttu-id="e18da-104">Telepítse a hello [Azure PowerShell-parancsmagok] [ lnk-powershell-install] a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="e18da-104">Install hello [Azure PowerShell cmdlets][lnk-powershell-install] before you continue.</span></span>

<span data-ttu-id="e18da-105">a következő lépéseket megjelenítése hogyan hello tooset PowerShell használatával AD alkalmazások jelszó-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="e18da-105">hello following steps show how tooset up password authentication for an AD application using PowerShell.</span></span> <span data-ttu-id="e18da-106">Ezek a parancsok a szabványos PowerShell-munkamenetben futtathatja.</span><span class="sxs-lookup"><span data-stu-id="e18da-106">You can run these commands in a standard PowerShell session.</span></span>

1. <span data-ttu-id="e18da-107">Jelentkezzen be tooyour Azure-előfizetés a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="e18da-107">Log in tooyour Azure subscription using hello following command:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="e18da-108">Ha több Azure-előfizetéssel rendelkezik, birtokában tooall hozzáférhet tooAzure bejelentkezés hello Azure-előfizetéssel társított hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="e18da-108">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="e18da-109">A következő parancs toolist hello Azure-előfizetések elérhető az Ön toouse hello használata:</span><span class="sxs-lookup"><span data-stu-id="e18da-109">Use hello following command toolist hello Azure subscriptions available for you toouse:</span></span>

    ```powershell
    Get-AzureRMSubscription
    ```

    <span data-ttu-id="e18da-110">A következő parancs tooselect előfizetést, amelyet toouse toorun hello parancsok toomanage az IoT hub hello használata.</span><span class="sxs-lookup"><span data-stu-id="e18da-110">Use hello following command tooselect subscription that you want toouse toorun hello commands toomanage your IoT hub.</span></span> <span data-ttu-id="e18da-111">Hello előfizetés név vagy azonosító hello kimenetből hello előző parancs használható:</span><span class="sxs-lookup"><span data-stu-id="e18da-111">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. <span data-ttu-id="e18da-112">Jegyezze fel a **TenantId** és **SubscriptionId**.</span><span class="sxs-lookup"><span data-stu-id="e18da-112">Make a note of your **TenantId** and **SubscriptionId**.</span></span> <span data-ttu-id="e18da-113">Ezeket később szüksége.</span><span class="sxs-lookup"><span data-stu-id="e18da-113">You need them later.</span></span>
3. <span data-ttu-id="e18da-114">Hozzon létre egy új Azure Active Directory-alkalmazást a következő parancsot, cseréje hello hely tulajdonosainak hello:</span><span class="sxs-lookup"><span data-stu-id="e18da-114">Create a new Azure Active Directory application using hello following command, replacing hello place holders:</span></span>
   
   * <span data-ttu-id="e18da-115">**{Név}:** egy megjelenítési nevet az alkalmazásnak, többek között **MySampleApp**</span><span class="sxs-lookup"><span data-stu-id="e18da-115">**{Display name}:** a display name for your application such as **MySampleApp**</span></span>
   * <span data-ttu-id="e18da-116">**{Kezdőlap URL-cím}:** hello kezdőlap, az alkalmazás URL-címe például hello **http://mysampleapp/home**.</span><span class="sxs-lookup"><span data-stu-id="e18da-116">**{Home page URL}:** hello URL of hello home page of your app such as **http://mysampleapp/home**.</span></span> <span data-ttu-id="e18da-117">Az URL-cím nem kell toopoint tooa valós alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e18da-117">This URL does not need toopoint tooa real application.</span></span>
   * <span data-ttu-id="e18da-118">**{Alkalmazásazonosító}:** egyedi azonosítója, mint **http://mysampleapp**.</span><span class="sxs-lookup"><span data-stu-id="e18da-118">**{Application identifier}:** A unique identifier such as **http://mysampleapp**.</span></span> <span data-ttu-id="e18da-119">Az URL-cím nem kell toopoint tooa valós alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e18da-119">This URL does not need toopoint tooa real application.</span></span>
   * <span data-ttu-id="e18da-120">**{Jelszó}:** tooauthenticate használhatja az alkalmazást a jelszót.</span><span class="sxs-lookup"><span data-stu-id="e18da-120">**{Password}:** A password that you use tooauthenticate with your app.</span></span>
     
     ```powershell
     New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
     ```
4. <span data-ttu-id="e18da-121">Jegyezze fel a hello **ApplicationId** létrehozott hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e18da-121">Make a note of hello **ApplicationId** of hello application you created.</span></span> <span data-ttu-id="e18da-122">Később szüksége.</span><span class="sxs-lookup"><span data-stu-id="e18da-122">You need this later.</span></span>
5. <span data-ttu-id="e18da-123">Hozzon létre egy új egyszerű szolgáltatás használata a következő parancsot, hogy hello **{MyApplicationId}** a hello **ApplicationId** hello előző lépésben:</span><span class="sxs-lookup"><span data-stu-id="e18da-123">Create a new service principal using hello following command, replacing **{MyApplicationId}** with hello **ApplicationId** from hello previous step:</span></span>
   
    ```powershell
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. <span data-ttu-id="e18da-124">Állítsa be a következő parancsot, hogy hello segítségével szerepkör-hozzárendelés **{MyApplicationId}** rendelkező a **ApplicationId**.</span><span class="sxs-lookup"><span data-stu-id="e18da-124">Set up a role assignment using hello following command, replacing **{MyApplicationId}** with your **ApplicationId**.</span></span>
   
    ```powershell
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

<span data-ttu-id="e18da-125">Most már befejezte a hello Azure AD-alkalmazást, amely lehetővé teszi tooauthenticate az egyéni C#-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e18da-125">You have now finished creating hello Azure AD application that enables you tooauthenticate from your custom C# application.</span></span> <span data-ttu-id="e18da-126">Hello követő értékek az oktatóanyag későbbi részében szüksége:</span><span class="sxs-lookup"><span data-stu-id="e18da-126">You need hello following values later in this tutorial:</span></span>

* <span data-ttu-id="e18da-127">A TenantId</span><span class="sxs-lookup"><span data-stu-id="e18da-127">TenantId</span></span>
* <span data-ttu-id="e18da-128">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="e18da-128">SubscriptionId</span></span>
* <span data-ttu-id="e18da-129">ApplicationId</span><span class="sxs-lookup"><span data-stu-id="e18da-129">ApplicationId</span></span>
* <span data-ttu-id="e18da-130">Jelszó</span><span class="sxs-lookup"><span data-stu-id="e18da-130">Password</span></span>

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
