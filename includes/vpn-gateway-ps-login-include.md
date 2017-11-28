<span data-ttu-id="625a9-101">Mielőtt hozzálát a művelethez, be kell jelentkeznie az Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="625a9-101">Before beginning this configuration, you must log in to your Azure account.</span></span> <span data-ttu-id="625a9-102">A parancsmag kéri az Azure-fiók bejelentkezési hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="625a9-102">The cmdlet prompts you for the login credentials for your Azure account.</span></span> <span data-ttu-id="625a9-103">A bejelentkezés után letölti a fiók beállításait, hogy elérhetők legyenek az Azure PowerShell számára.</span><span class="sxs-lookup"><span data-stu-id="625a9-103">After logging in, it downloads your account settings so they are available to Azure PowerShell.</span></span> <span data-ttu-id="625a9-104">További információ: [A Windows PowerShell használata a Resource Managerrel](../articles/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="625a9-104">For more information, see [Using Windows PowerShell with Resource Manager](../articles/powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="625a9-105">A bejelentkezéshez nyissa meg emelt szintű jogosultságokkal a PowerShell-konzolt, és csatlakozzon a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="625a9-105">To log in, open your PowerShell console with elevated privileges, and connect to your account.</span></span> <span data-ttu-id="625a9-106">A következő példa segít a kapcsolódásban:</span><span class="sxs-lookup"><span data-stu-id="625a9-106">Use the following example to help you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="625a9-107">Ha több Azure-előfizetéssel is rendelkezik, ellenőrizze a fiók előfizetéseit.</span><span class="sxs-lookup"><span data-stu-id="625a9-107">If you have multiple Azure subscriptions, check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="625a9-108">Válassza ki a használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="625a9-108">Specify the subscription that you want to use.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```