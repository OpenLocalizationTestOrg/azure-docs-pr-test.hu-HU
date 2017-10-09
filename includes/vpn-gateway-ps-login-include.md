<span data-ttu-id="00060-101">Ez a konfiguráció megkezdése előtt be kell jelentkeznie tooyour Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="00060-101">Before beginning this configuration, you must log in tooyour Azure account.</span></span> <span data-ttu-id="00060-102">hello parancsmag kéri hello bejelentkezési hitelesítő adatait az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="00060-102">hello cmdlet prompts you for hello login credentials for your Azure account.</span></span> <span data-ttu-id="00060-103">A bejelentkezés után az tölti le a fiók beállításait,-e elérhető tooAzure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="00060-103">After logging in, it downloads your account settings so they are available tooAzure PowerShell.</span></span> <span data-ttu-id="00060-104">További információ: [A Windows PowerShell használata a Resource Managerrel](../articles/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="00060-104">For more information, see [Using Windows PowerShell with Resource Manager](../articles/powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="00060-105">toolog, nyissa meg a PowerShell-konzolt emelt szintű jogosultságokkal, és csatlakozzon tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="00060-105">toolog in, open your PowerShell console with elevated privileges, and connect tooyour account.</span></span> <span data-ttu-id="00060-106">A következő példa toohelp csatlakozás hello használata:</span><span class="sxs-lookup"><span data-stu-id="00060-106">Use hello following example toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="00060-107">Ha több Azure-előfizetéssel rendelkezik, ellenőrizze a hello fiókhoz hello előfizetések.</span><span class="sxs-lookup"><span data-stu-id="00060-107">If you have multiple Azure subscriptions, check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="00060-108">Adja meg, hogy szeretné-e toouse hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="00060-108">Specify hello subscription that you want toouse.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```