### <a name="troubleshoot-azure-diagnostics"></a><span data-ttu-id="d9e1d-101">Az Azure Diagnostics hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="d9e1d-101">Troubleshoot Azure Diagnostics</span></span>

<span data-ttu-id="d9e1d-102">Ha hibaüzenet jelenik meg a következő hello, hello Microsoft.insights erőforrás-szolgáltató nincs regisztrálva:</span><span class="sxs-lookup"><span data-stu-id="d9e1d-102">If you receive hello following error message, hello Microsoft.insights resource provider is not registered:</span></span>

`Failed tooupdate diagnostics for 'resource'. {"code":"Forbidden","message":"Please register hello subscription 'subscription id' with Microsoft.Insights."}`

<span data-ttu-id="d9e1d-103">tooregister hello erőforrás-szolgáltató, hajtsa végre a következő lépéseket az Azure-portálon hello hello:</span><span class="sxs-lookup"><span data-stu-id="d9e1d-103">tooregister hello resource provider, perform hello following steps in hello Azure portal:</span></span>

1.  <span data-ttu-id="d9e1d-104">Hello bal oldali hello navigációs ablaktábláján kattintson *előfizetések*</span><span class="sxs-lookup"><span data-stu-id="d9e1d-104">In hello navigation pane on hello left, click *Subscriptions*</span></span>
2.  <span data-ttu-id="d9e1d-105">Válassza ki a hello hibaüzenet azonosított hello előfizetést</span><span class="sxs-lookup"><span data-stu-id="d9e1d-105">Select hello subscription identified in hello error message</span></span>
3.  <span data-ttu-id="d9e1d-106">Kattintson az *Erőforrás-szolgáltatók* elemre</span><span class="sxs-lookup"><span data-stu-id="d9e1d-106">Click *Resource Providers*</span></span>
4.  <span data-ttu-id="d9e1d-107">Hello található *Microsoft.insights* szolgáltató</span><span class="sxs-lookup"><span data-stu-id="d9e1d-107">Find hello *Microsoft.insights* provider</span></span>
5.  <span data-ttu-id="d9e1d-108">Kattintson a hello *regisztrálása* hivatkozás</span><span class="sxs-lookup"><span data-stu-id="d9e1d-108">Click hello *Register* link</span></span>

![Regisztrálja a microsoft.insights erőforrás-szolgáltatót](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

<span data-ttu-id="d9e1d-110">Egyszer hello *Microsoft.insights* erőforrás-szolgáltató regisztrálva van, próbálja meg újra konfigurálni diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="d9e1d-110">Once hello *Microsoft.insights* resource provider is registered, retry configuring diagnostics.</span></span>


<span data-ttu-id="d9e1d-111">A PowerShellben Ha jelenik meg hibaüzenet, a következő hello kell tooupdate PowerShell verziójának:</span><span class="sxs-lookup"><span data-stu-id="d9e1d-111">In PowerShell, if you receive hello following error message, you need tooupdate your version of PowerShell:</span></span>

`Set-AzureRmDiagnosticSetting : A parameter cannot be found that matches parameter name 'WorkspaceId'.`

<span data-ttu-id="d9e1d-112">Frissítse a PowerShell toohello November 2016 (v2.3.0), vagy később, a hello utasítások használatát hello feloldása [Ismerkedés az Azure PowerShell-parancsmagok](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) cikk.</span><span class="sxs-lookup"><span data-stu-id="d9e1d-112">Update your version of PowerShell toohello November 2016 (v2.3.0), or later, release using hello instructions in hello [Get started with Azure PowerShell cmdlets](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) article.</span></span>
