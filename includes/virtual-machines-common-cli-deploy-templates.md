
* [<span data-ttu-id="05104-101">Virtuális gép gyors létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="05104-101">Quick-create a virtual machine in Azure</span></span>](#quick-create-a-vm-in-azure)
* [<span data-ttu-id="05104-102">Virtuális gép központi telepítése sablonból az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="05104-102">Deploy a virtual machine in Azure from a template</span></span>](#deploy-a-vm-in-azure-from-a-template)
* [<span data-ttu-id="05104-103">Virtuális gép létrehozása egyéni rendszerképből</span><span class="sxs-lookup"><span data-stu-id="05104-103">Create a virtual machine from a custom image</span></span>](#create-a-custom-vm-image)
* [<span data-ttu-id="05104-104">Virtuális hálózatot és terheléselosztót használó virtuális gép központi telepítése</span><span class="sxs-lookup"><span data-stu-id="05104-104">Deploy a virtual machine that uses a virtual network and a load balancer</span></span>](#deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer)
* [<span data-ttu-id="05104-105">Erőforráscsoport eltávolítása</span><span class="sxs-lookup"><span data-stu-id="05104-105">Remove a resource group</span></span>](#remove-a-resource-group)
* [<span data-ttu-id="05104-106">Egy erőforrás-csoport központi telepítésének hello naplóban megjelenítése</span><span class="sxs-lookup"><span data-stu-id="05104-106">Show hello log for a resource group deployment</span></span>](#show-the-log-for-a-resource-group-deployment)
* [<span data-ttu-id="05104-107">Virtuális gépre vonatkozó információk megjelenítése</span><span class="sxs-lookup"><span data-stu-id="05104-107">Display information about a virtual machine</span></span>](#display-information-about-a-virtual-machine)
* [<span data-ttu-id="05104-108">Csatlakoztassa tooa Linux-alapú virtuális gépet</span><span class="sxs-lookup"><span data-stu-id="05104-108">Connect tooa Linux-based virtual machine</span></span>](#log-on-to-a-linux-based-virtual-machine)
* [<span data-ttu-id="05104-109">Virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="05104-109">Stop a virtual machine</span></span>](#stop-a-virtual-machine)
* [<span data-ttu-id="05104-110">Virtuális gép elindítása</span><span class="sxs-lookup"><span data-stu-id="05104-110">Start a virtual machine</span></span>](#start-a-virtual-machine)
* [<span data-ttu-id="05104-111">Adatlemez csatolása</span><span class="sxs-lookup"><span data-stu-id="05104-111">Attach a data disk</span></span>](#attach-a-data-disk)

## <a name="getting-ready"></a><span data-ttu-id="05104-112">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="05104-112">Getting ready</span></span>
<span data-ttu-id="05104-113">Azure erőforráscsoport-sablonok hello Azure CLI használata előtt kell toohave hello Azure CLI verzió és az Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="05104-113">Before you can use hello Azure CLI with Azure resource groups, you need toohave hello right Azure CLI version and an Azure account.</span></span> <span data-ttu-id="05104-114">Ha még nem rendelkezik Azure CLI hello [telepítse](../articles/cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="05104-114">If you don't have hello Azure CLI, [install it](../articles/cli-install-nodejs.md).</span></span>

### <a name="update-your-azure-cli-version-too090-or-later"></a><span data-ttu-id="05104-115">Az Azure CLI verzió too0.9.0 frissítése vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="05104-115">Update your Azure CLI version too0.9.0 or later</span></span>
<span data-ttu-id="05104-116">Típus `azure --version` fel kell-e már telepített 0.9.0-s verziója toosee vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="05104-116">Type `azure --version` toosee whether you have already installed version 0.9.0 or later.</span></span>

```azurecli
azure --version
0.9.0 (node: 0.10.25)
```

<span data-ttu-id="05104-117">Ha a verziószáma nem 0.9.0-s vagy újabb rendszert kell tooupdate azt egyikével hello natív telepítők vagy **npm** beírásával `npm update -g azure-cli`.</span><span class="sxs-lookup"><span data-stu-id="05104-117">If your version is not 0.9.0 or later, you need tooupdate it by using one of hello native installers or through **npm** by typing `npm update -g azure-cli`.</span></span>

<span data-ttu-id="05104-118">Is futtathatja, az Azure parancssori felület egy Docker-tároló által hello alábbi [Docker kép](https://registry.hub.docker.com/u/microsoft/azure-cli/).</span><span class="sxs-lookup"><span data-stu-id="05104-118">You can also run Azure CLI as a Docker container by using hello following [Docker image](https://registry.hub.docker.com/u/microsoft/azure-cli/).</span></span> <span data-ttu-id="05104-119">Docker gazdagépről futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="05104-119">From a Docker host, run hello following command:</span></span>

```bash
docker run -it microsoft/azure-cli
```

### <a name="set-your-azure-account-and-subscription"></a><span data-ttu-id="05104-120">Az Azure-fiók és -előfizetés beállítása</span><span class="sxs-lookup"><span data-stu-id="05104-120">Set your Azure account and subscription</span></span>
<span data-ttu-id="05104-121">Ha még nincs Azure-előfizetése, de van MSDN-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="05104-121">If you don't already have an Azure subscription but you do have an MSDN subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span> <span data-ttu-id="05104-122">Regisztrálhat [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/) is.</span><span class="sxs-lookup"><span data-stu-id="05104-122">Or you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

<span data-ttu-id="05104-123">Most [interaktív jelentkezzen be Azure-fiók tooyour](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login) beírásával `azure login` és a következő hello kér egy interaktív bejelentkezési élmény tooyour Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="05104-123">Now [log in tooyour Azure account interactively](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login) by typing `azure login` and following hello prompts for an interactive login experience tooyour Azure account.</span></span> 

> [!NOTE]
> <span data-ttu-id="05104-124">Ha rendelkezik munkahelyi vagy iskolai azonosítója, és nem rendelkezik kéttényezős hitelesítést, akkor tudja **is** használja `azure login -u` együtt hello munkahelyi vagy iskolai azonosító toolog a *nélkül* egy interaktív munkamenet.</span><span class="sxs-lookup"><span data-stu-id="05104-124">If you have a work or school ID and you know you do not have two-factor authentication enabled, you can **also** use `azure login -u` along with hello work or school ID toolog in *without* an interactive session.</span></span> <span data-ttu-id="05104-125">Ha nem rendelkezik munkahelyi vagy iskolai azonosítója, akkor [munkahelyi vagy iskolai azonosító létrehozása a személyes Microsoft-fiókhoz tartozó](../articles/virtual-machines/windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toolog hello a megszokott módon.</span><span class="sxs-lookup"><span data-stu-id="05104-125">If you don't have a work or school ID, you can [create a work or school id from your personal Microsoft account](../articles/virtual-machines/windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toolog in hello same way.</span></span>
>
>

<span data-ttu-id="05104-126">A fiókjához tartozhat több előfizetés is.</span><span class="sxs-lookup"><span data-stu-id="05104-126">Your account may have more than one subscription.</span></span> <span data-ttu-id="05104-127">Az előfizetések listájának megjelenítéséhez írja be az `azure account list` karakterláncot. Az eredmény a következőhöz hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="05104-127">You can list your subscriptions by typing `azure account list`, which might look something like this:</span></span>

```azurecli
azure account list
info:    Executing command account list
data:    Name                              Id                                    Tenant Id                            Current
data:    --------------------------------  ------------------------------------  ------------------------------------  -------
data:    Contoso Admin                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  true
data:    Fabrikam dev                      xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Fabrikam test                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Contoso production                xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
```

<span data-ttu-id="05104-128">Hello jelenlegi Azure-előfizetés hello következő állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="05104-128">You can set hello current Azure subscription by typing hello following.</span></span> <span data-ttu-id="05104-129">Hello nevét vagy hello Előfizetésazonosító toomanage kívánt hello erőforrásokat tartalmazó használja.</span><span class="sxs-lookup"><span data-stu-id="05104-129">Use hello subscription name or hello ID that has hello resources you want toomanage.</span></span>

```azurecli
azure account set <subscription name or ID> true
```

### <a name="switch-toohello-azure-cli-resource-group-mode"></a><span data-ttu-id="05104-130">Váltás toohello Azure CLI erőforrás mód</span><span class="sxs-lookup"><span data-stu-id="05104-130">Switch toohello Azure CLI resource group mode</span></span>
<span data-ttu-id="05104-131">Alapértelmezés szerint az Azure parancssori felület hello hello szolgáltatás felügyeleti üzemmódban indul (**asm** módot).</span><span class="sxs-lookup"><span data-stu-id="05104-131">By default, hello Azure CLI starts in hello service management mode (**asm** mode).</span></span> <span data-ttu-id="05104-132">Írja be a következő tooswitch tooresource mód hello.</span><span class="sxs-lookup"><span data-stu-id="05104-132">Type hello following tooswitch tooresource group mode.</span></span>

```azurecli
azure config mode arm
```

## <a name="understanding-azure-resource-templates-and-resource-groups"></a><span data-ttu-id="05104-133">Az Azure-erőforrássablonok és -erőforráscsoportok ismertetése</span><span class="sxs-lookup"><span data-stu-id="05104-133">Understanding Azure resource templates and resource groups</span></span>
<span data-ttu-id="05104-134">A legtöbb alkalmazás különböző erőforrástípusokból épül fel (például egy vagy több virtuális gép és tárfiók, egy SQL-adatbázis, egy virtuális hálózat vagy egy tartalomkézbesítési hálózat).</span><span class="sxs-lookup"><span data-stu-id="05104-134">Most applications are built from a combination of different resource types (such as one or more VMs and storage accounts, a SQL database, a virtual network, or a content delivery network).</span></span> <span data-ttu-id="05104-135">alapértelmezett Azure service management API hello és az hello a klasszikus Azure portálon ezeket az elemeket a szolgáltatások által módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="05104-135">hello default Azure service management API and hello Azure classic portal represented these items by using a service-by-service approach.</span></span> <span data-ttu-id="05104-136">Ez a megközelítés toodeploy meg és hello egyéni szolgáltatások kezelése (vagy találhatók, így más eszközök), nem pedig a központi telepítés egyetlen logikai egységbe.</span><span class="sxs-lookup"><span data-stu-id="05104-136">This approach requires you toodeploy and manage hello individual services individually (or find other tools that do so), and not as a single logical unit of deployment.</span></span>

<span data-ttu-id="05104-137">*Az Azure Resource Manager-sablonok*, azonban Ön toodeploy lehetővé teszik, és a különböző erőforrások kezelését egy logikai telepítési egységként deklaratív módon.</span><span class="sxs-lookup"><span data-stu-id="05104-137">*Azure Resource Manager templates*, however, make it possible for you toodeploy and manage these different resources as one logical deployment unit in a declarative fashion.</span></span> <span data-ttu-id="05104-138">Helyett imperatively szólítja fel Azure milyen toodeploy egy parancs egymás után, egy JSON-fájl – összes hello erőforrások és a társított konfigurációs és üzembe helyezéshez megadott paraméterek – a teljes telepítését írja le, és mondja el ezeket az erőforrásokat, mint az Azure toodeploy a csoport.</span><span class="sxs-lookup"><span data-stu-id="05104-138">Instead of imperatively telling Azure what toodeploy one command after another, you describe your entire deployment in a JSON file -- all of hello resources and associated configuration and deployment parameters -- and tell Azure toodeploy those resources as one group.</span></span>

<span data-ttu-id="05104-139">Hello kezelhetők teljes életciklusát hello csoport erőforrások az Azure parancssori felület erőforrás felügyeleti parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="05104-139">You can then manage hello overall life cycle of hello group's resources by using Azure CLI resource management commands to:</span></span>

* <span data-ttu-id="05104-140">Állítsa le, elindításához, vagy törölje az összes hello erőforrások hello csoporton belüli egyszerre.</span><span class="sxs-lookup"><span data-stu-id="05104-140">Stop, start, or delete all of hello resources within hello group at once.</span></span>
* <span data-ttu-id="05104-141">Szerepköralapú hozzáférés-vezérlés (RBAC) szabályok toolock le a biztonsági engedélyek alkalmazza őket.</span><span class="sxs-lookup"><span data-stu-id="05104-141">Apply Role-Based Access Control (RBAC) rules toolock down security permissions on them.</span></span>
* <span data-ttu-id="05104-142">Auditálási műveletek.</span><span class="sxs-lookup"><span data-stu-id="05104-142">Audit operations.</span></span>
* <span data-ttu-id="05104-143">Erőforrások címkézése további metaadatokkal a jobb nyomon követés érdekében.</span><span class="sxs-lookup"><span data-stu-id="05104-143">Tag resources with additional metadata for better tracking.</span></span>

<span data-ttu-id="05104-144">Sok Azure erőforráscsoport-sablonok és mit tehet meg a hello többet tudhat [Azure Resource Manager áttekintése](../articles/azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="05104-144">You can learn lots more about Azure resource groups and what they can do for you in hello [Azure Resource Manager overview](../articles/azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="05104-145">A sablonok készítésével kapcsolatos további információk: [Azure Resource Manager-sablonok készítése](../articles/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="05104-145">If you're interested in authoring templates, see [Authoring Azure Resource Manager templates](../articles/resource-group-authoring-templates.md).</span></span>

## <span data-ttu-id="05104-146"><a id="quick-create-a-vm-in-azure"></a>Feladat: Virtuális gép gyors létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="05104-146"><a id="quick-create-a-vm-in-azure"></a>Task: Quick-create a VM in Azure</span></span>
<span data-ttu-id="05104-147">Néha ismernie kell, képet és van szüksége a virtuális gépek erről a képről most nem túlságosan érdeklik hello infrastruktúra – lehet, hogy rendelkezik tootest valami egy tiszta virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="05104-147">Sometimes you know what image you need, and you need a VM from that image right now and you don't care too much about hello infrastructure -- maybe you have tootest something on a clean VM.</span></span> <span data-ttu-id="05104-148">Amely megadása, ha azt szeretné, hogy toouse hello `azure vm quick-create` parancsot, és adja át hello argumentum szükséges toocreate, a virtuális gépek és az infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="05104-148">That's when you want toouse hello `azure vm quick-create` command, and pass hello arguments necessary toocreate a VM and its infrastructure.</span></span>

<span data-ttu-id="05104-149">Először is hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="05104-149">First, create your resource group.</span></span>

```azurecli
azure group create coreos-quick westus
info:    Executing command group create
+ Getting resource group coreos-quick
+ Creating resource group coreos-quick
info:    Created resource group coreos-quick
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick
data:    Name:                coreos-quick
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

<span data-ttu-id="05104-150">Ezután szüksége lesz egy rendszerképre.</span><span class="sxs-lookup"><span data-stu-id="05104-150">Second, you'll need an image.</span></span> <span data-ttu-id="05104-151">hello Azure parancssori felület, a lemezkép egy toofind lásd [Navigating és a PowerShell és az Azure parancssori felület hello Azure virtuálisgép-rendszerképek kiválasztásáról](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="05104-151">toofind an image with hello Azure CLI, see [Navigating and selecting Azure virtual machine images with PowerShell and hello Azure CLI](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="05104-152">Ebben a cikkben azonban az alábbiakban látható egy rövid lista a népszerű rendszerképekről.</span><span class="sxs-lookup"><span data-stu-id="05104-152">But for this article, here's a short list of popular images.</span></span> <span data-ttu-id="05104-153">A gyors létrehozáshoz a CoreOS Stable rendszerképét fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="05104-153">We'll use CoreOS's Stable image for this quick-create.</span></span>

> [!NOTE]
> <span data-ttu-id="05104-154">A ComputeImageVersion egyszerűen is megadhat "legújabb" módon paraméter mindkét hello sablon nyelven, és az Azure CLI hello hello.</span><span class="sxs-lookup"><span data-stu-id="05104-154">For ComputeImageVersion, you can also simply supply 'latest' as hello parameter in both hello template language and in hello Azure CLI.</span></span> <span data-ttu-id="05104-155">Ez lehetővé teszi a tooalways hello legújabb és javított verzióját használja hello kép toomodify nélkül, a parancsfájlok vagy sablonok.</span><span class="sxs-lookup"><span data-stu-id="05104-155">This will allow you tooalways use hello latest and patched version of hello image without having toomodify your scripts or templates.</span></span> <span data-ttu-id="05104-156">Ez az alábbiakban látható.</span><span class="sxs-lookup"><span data-stu-id="05104-156">This is shown below.</span></span>
>
>

| <span data-ttu-id="05104-157">Közzétevő neve</span><span class="sxs-lookup"><span data-stu-id="05104-157">PublisherName</span></span> | <span data-ttu-id="05104-158">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="05104-158">Offer</span></span> | <span data-ttu-id="05104-159">SKU</span><span class="sxs-lookup"><span data-stu-id="05104-159">Sku</span></span> | <span data-ttu-id="05104-160">Verzió</span><span class="sxs-lookup"><span data-stu-id="05104-160">Version</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="05104-161">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="05104-161">OpenLogic</span></span> |<span data-ttu-id="05104-162">CentOS</span><span class="sxs-lookup"><span data-stu-id="05104-162">CentOS</span></span> |<span data-ttu-id="05104-163">7</span><span class="sxs-lookup"><span data-stu-id="05104-163">7</span></span> |<span data-ttu-id="05104-164">7.0.201503</span><span class="sxs-lookup"><span data-stu-id="05104-164">7.0.201503</span></span> |
| <span data-ttu-id="05104-165">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="05104-165">OpenLogic</span></span> |<span data-ttu-id="05104-166">CentOS</span><span class="sxs-lookup"><span data-stu-id="05104-166">CentOS</span></span> |<span data-ttu-id="05104-167">7.1</span><span class="sxs-lookup"><span data-stu-id="05104-167">7.1</span></span> |<span data-ttu-id="05104-168">7.1.201504</span><span class="sxs-lookup"><span data-stu-id="05104-168">7.1.201504</span></span> |
| <span data-ttu-id="05104-169">CoreOS</span><span class="sxs-lookup"><span data-stu-id="05104-169">CoreOS</span></span> |<span data-ttu-id="05104-170">CoreOS</span><span class="sxs-lookup"><span data-stu-id="05104-170">CoreOS</span></span> |<span data-ttu-id="05104-171">Beta</span><span class="sxs-lookup"><span data-stu-id="05104-171">Beta</span></span> |<span data-ttu-id="05104-172">647.0.0</span><span class="sxs-lookup"><span data-stu-id="05104-172">647.0.0</span></span> |
| <span data-ttu-id="05104-173">CoreOS</span><span class="sxs-lookup"><span data-stu-id="05104-173">CoreOS</span></span> |<span data-ttu-id="05104-174">CoreOS</span><span class="sxs-lookup"><span data-stu-id="05104-174">CoreOS</span></span> |<span data-ttu-id="05104-175">Stable</span><span class="sxs-lookup"><span data-stu-id="05104-175">Stable</span></span> |<span data-ttu-id="05104-176">633.1.0</span><span class="sxs-lookup"><span data-stu-id="05104-176">633.1.0</span></span> |
| <span data-ttu-id="05104-177">MicrosoftDynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="05104-177">MicrosoftDynamicsNAV</span></span> |<span data-ttu-id="05104-178">DynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="05104-178">DynamicsNAV</span></span> |<span data-ttu-id="05104-179">2015</span><span class="sxs-lookup"><span data-stu-id="05104-179">2015</span></span> |<span data-ttu-id="05104-180">8.0.40459</span><span class="sxs-lookup"><span data-stu-id="05104-180">8.0.40459</span></span> |
| <span data-ttu-id="05104-181">MicrosoftSharePoint</span><span class="sxs-lookup"><span data-stu-id="05104-181">MicrosoftSharePoint</span></span> |<span data-ttu-id="05104-182">MicrosoftSharePointServer</span><span class="sxs-lookup"><span data-stu-id="05104-182">MicrosoftSharePointServer</span></span> |<span data-ttu-id="05104-183">2013</span><span class="sxs-lookup"><span data-stu-id="05104-183">2013</span></span> |<span data-ttu-id="05104-184">1.0.0</span><span class="sxs-lookup"><span data-stu-id="05104-184">1.0.0</span></span> |
| <span data-ttu-id="05104-185">msopentech</span><span class="sxs-lookup"><span data-stu-id="05104-185">msopentech</span></span> |<span data-ttu-id="05104-186">Oracle-Database-12c-Weblogic-Server-12c</span><span class="sxs-lookup"><span data-stu-id="05104-186">Oracle-Database-12c-Weblogic-Server-12c</span></span> |<span data-ttu-id="05104-187">Standard</span><span class="sxs-lookup"><span data-stu-id="05104-187">Standard</span></span> |<span data-ttu-id="05104-188">1.0.0</span><span class="sxs-lookup"><span data-stu-id="05104-188">1.0.0</span></span> |
| <span data-ttu-id="05104-189">msopentech</span><span class="sxs-lookup"><span data-stu-id="05104-189">msopentech</span></span> |<span data-ttu-id="05104-190">Oracle-Database-12c-Weblogic-Server-12c</span><span class="sxs-lookup"><span data-stu-id="05104-190">Oracle-Database-12c-Weblogic-Server-12c</span></span> |<span data-ttu-id="05104-191">Enterprise</span><span class="sxs-lookup"><span data-stu-id="05104-191">Enterprise</span></span> |<span data-ttu-id="05104-192">1.0.0</span><span class="sxs-lookup"><span data-stu-id="05104-192">1.0.0</span></span> |
| <span data-ttu-id="05104-193">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="05104-193">MicrosoftSQLServer</span></span> |<span data-ttu-id="05104-194">SQL2014-WS2012R2</span><span class="sxs-lookup"><span data-stu-id="05104-194">SQL2014-WS2012R2</span></span> |<span data-ttu-id="05104-195">Enterprise-Optimized-for-DW</span><span class="sxs-lookup"><span data-stu-id="05104-195">Enterprise-Optimized-for-DW</span></span> |<span data-ttu-id="05104-196">12.0.2430</span><span class="sxs-lookup"><span data-stu-id="05104-196">12.0.2430</span></span> |
| <span data-ttu-id="05104-197">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="05104-197">MicrosoftSQLServer</span></span> |<span data-ttu-id="05104-198">SQL2014-WS2012R2</span><span class="sxs-lookup"><span data-stu-id="05104-198">SQL2014-WS2012R2</span></span> |<span data-ttu-id="05104-199">Enterprise-Optimized-for-OLTP</span><span class="sxs-lookup"><span data-stu-id="05104-199">Enterprise-Optimized-for-OLTP</span></span> |<span data-ttu-id="05104-200">12.0.2430</span><span class="sxs-lookup"><span data-stu-id="05104-200">12.0.2430</span></span> |
| <span data-ttu-id="05104-201">Canonical</span><span class="sxs-lookup"><span data-stu-id="05104-201">Canonical</span></span> |<span data-ttu-id="05104-202">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="05104-202">UbuntuServer</span></span> |<span data-ttu-id="05104-203">12.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="05104-203">12.04.5-LTS</span></span> |<span data-ttu-id="05104-204">12.04.201504230</span><span class="sxs-lookup"><span data-stu-id="05104-204">12.04.201504230</span></span> |
| <span data-ttu-id="05104-205">Canonical</span><span class="sxs-lookup"><span data-stu-id="05104-205">Canonical</span></span> |<span data-ttu-id="05104-206">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="05104-206">UbuntuServer</span></span> |<span data-ttu-id="05104-207">14.04.2-LTS</span><span class="sxs-lookup"><span data-stu-id="05104-207">14.04.2-LTS</span></span> |<span data-ttu-id="05104-208">14.04.201503090</span><span class="sxs-lookup"><span data-stu-id="05104-208">14.04.201503090</span></span> |
| <span data-ttu-id="05104-209">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="05104-209">MicrosoftWindowsServer</span></span> |<span data-ttu-id="05104-210">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="05104-210">WindowsServer</span></span> |<span data-ttu-id="05104-211">2012-Datacenter</span><span class="sxs-lookup"><span data-stu-id="05104-211">2012-Datacenter</span></span> |<span data-ttu-id="05104-212">3.0.201503</span><span class="sxs-lookup"><span data-stu-id="05104-212">3.0.201503</span></span> |
| <span data-ttu-id="05104-213">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="05104-213">MicrosoftWindowsServer</span></span> |<span data-ttu-id="05104-214">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="05104-214">WindowsServer</span></span> |<span data-ttu-id="05104-215">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="05104-215">2012-R2-Datacenter</span></span> |<span data-ttu-id="05104-216">4.0.201503</span><span class="sxs-lookup"><span data-stu-id="05104-216">4.0.201503</span></span> |
| <span data-ttu-id="05104-217">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="05104-217">MicrosoftWindowsServer</span></span> |<span data-ttu-id="05104-218">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="05104-218">WindowsServer</span></span> |<span data-ttu-id="05104-219">Windows-Server-Technical-Preview</span><span class="sxs-lookup"><span data-stu-id="05104-219">Windows-Server-Technical-Preview</span></span> |<span data-ttu-id="05104-220">5.0.201504</span><span class="sxs-lookup"><span data-stu-id="05104-220">5.0.201504</span></span> |
| <span data-ttu-id="05104-221">MicrosoftWindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="05104-221">MicrosoftWindowsServerEssentials</span></span> |<span data-ttu-id="05104-222">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="05104-222">WindowsServerEssentials</span></span> |<span data-ttu-id="05104-223">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="05104-223">WindowsServerEssentials</span></span> |<span data-ttu-id="05104-224">1.0.141204</span><span class="sxs-lookup"><span data-stu-id="05104-224">1.0.141204</span></span> |
| <span data-ttu-id="05104-225">MicrosoftWindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="05104-225">MicrosoftWindowsServerHPCPack</span></span> |<span data-ttu-id="05104-226">WindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="05104-226">WindowsServerHPCPack</span></span> |<span data-ttu-id="05104-227">2012R2</span><span class="sxs-lookup"><span data-stu-id="05104-227">2012R2</span></span> |<span data-ttu-id="05104-228">4.3.4665</span><span class="sxs-lookup"><span data-stu-id="05104-228">4.3.4665</span></span> |

<span data-ttu-id="05104-229">Csak a virtuális gép létrehozása az hello megadásával `azure vm quick-create` parancsot, és készen áll a hello megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="05104-229">Just create your VM by entering hello `azure vm quick-create` command and being ready for hello prompts.</span></span> <span data-ttu-id="05104-230">A következőhöz hasonló eredményt kell kapnia:</span><span class="sxs-lookup"><span data-stu-id="05104-230">It should look something like this:</span></span>

```azurecli
azure vm quick-create
info:    Executing command vm quick-create
Resource group name: coreos-quick
Virtual machine name: coreos
Location name: westus
Operating system Type [Windows, Linux]: linux
ImageURN (format: "publisherName:offer:skus:version"): coreos:coreos:stable:latest
User name: ops
Password: *********
Confirm password: *********
+ Looking up hello VM "coreos"
info:    Using hello VM Size "Standard_A1"
info:    hello [OS, Data] Disk or image configuration requires storage account
+ Retrieving storage accounts
info:    Could not find any storage accounts in hello region "westus", trying toocreate new one
+ Creating storage account "cli9fd3fce49e9a9b3d14302" in "westus"
+ Looking up hello storage account cli9fd3fce49e9a9b3d14302
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
info:    An nic with given name "coreo-westu-1430261891570-nic" not found, creating a new one
+ Looking up hello virtual network "coreo-westu-1430261891570-vnet"
info:    Preparing toocreate new virtual network and subnet
/ Creating a new virtual network "coreo-westu-1430261891570-vnet" [address prefix: "10.0.0.0/16"] with subnet "coreo-westu-1430261891570-sne+" [address prefix: "10.0.1.0/24"]
+ Looking up hello virtual network "coreo-westu-1430261891570-vnet"
+ Looking up hello subnet "coreo-westu-1430261891570-snet" under hello virtual network "coreo-westu-1430261891570-vnet"
info:    Found public ip parameters, trying toosetup PublicIP profile
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
info:    PublicIP with given name "coreo-westu-1430261891570-pip" not found, creating a new one
+ Creating public ip "coreo-westu-1430261891570-pip"
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
+ Creating NIC "coreo-westu-1430261891570-nic"
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
+ Creating VM "coreos"
+ Looking up hello VM "coreos"
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Compute/virtualMachines/coreos
data:    ProvisioningState               :Succeeded
data:    Name                            :coreos
data:    Location                        :westus
data:    FQDN                            :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :coreos
data:        Offer                       :coreos
data:        Sku                         :stable
data:        Version                     :633.1.0
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :cli9fd3fce49e9a9b3d-os-1430261892283
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli9fd3fce49e9a9b3d14302.blob.core.windows.net/vhds/cli9fd3fce49e9a9b3d-os-1430261892283.vhd
data:
data:    OS Profile:
data:      Computer Name                 :coreos
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :false
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Network/networkInterfaces/coreo-westu-1430261891570-nic
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-30-72-E3
data:          Provisioning State        :Succeeded
data:          Name                      :coreo-westu-1430261891570-nic
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.1.4
data:            Public IP address       :104.40.24.124
data:            FQDN                    :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
info:    vm quick-create command OK
```

<span data-ttu-id="05104-231">És már kész is az új virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="05104-231">And away you go with your new VM.</span></span>

## <span data-ttu-id="05104-232"><a id="deploy-a-vm-in-azure-from-a-template"></a>Feladat: Virtuális gép központi telepítése sablonból az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="05104-232"><a id="deploy-a-vm-in-azure-from-a-template"></a>Task: Deploy a VM in Azure from a template</span></span>
<span data-ttu-id="05104-233">Használja a hello utasításokat a szakaszok toodeploy egy új Azure virtuális Gépet a sablon hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="05104-233">Use hello instructions in these sections toodeploy a new Azure VM by using a template with hello Azure CLI.</span></span> <span data-ttu-id="05104-234">Ez a sablon egyetlen virtuális gép létrehoz egy új virtuális hálózatot egyetlen alhálózattal, és eltérően a `azure vm quick-create`, lehetővé teszi, hogy Ön toodescribe mit pontosan, és ismételje meg a hiba nélkül betöltődtek.</span><span class="sxs-lookup"><span data-stu-id="05104-234">This template creates a single virtual machine in a new virtual network with a single subnet, and unlike `azure vm quick-create`, enables you toodescribe what you want precisely and repeat it without errors.</span></span> <span data-ttu-id="05104-235">Itt látható, amit a sablon létrehoz:</span><span class="sxs-lookup"><span data-stu-id="05104-235">Here's what this template creates:</span></span>

![](./media/virtual-machines-common-cli-deploy-templates/new-vm.png)

### <a name="step-1-examine-hello-json-file-for-hello-template-parameters"></a><span data-ttu-id="05104-236">1. lépés:, Vizsgálja meg hello JSON-fájl hello Sablonparaméterek</span><span class="sxs-lookup"><span data-stu-id="05104-236">Step 1: Examine hello JSON file for hello template parameters</span></span>
<span data-ttu-id="05104-237">Az alábbiakban hello hello hello sablon JSON-fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="05104-237">Here are hello contents of hello JSON file for hello template.</span></span> <span data-ttu-id="05104-238">(hello sablon is található [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json).)</span><span class="sxs-lookup"><span data-stu-id="05104-238">(hello template is also located in [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json).)</span></span>

<span data-ttu-id="05104-239">Sablonok rugalmasak, így előfordulhat, hogy a kiválasztott toogive nagy mennyiségű paraméterek vagy toooffer csak hozzon létre egy sablont, amely több rögzített néhány kiválasztott hello designer.</span><span class="sxs-lookup"><span data-stu-id="05104-239">Templates are flexible, so hello designer may have chosen toogive you lots of parameters or chosen toooffer only a few by creating a template that is more fixed.</span></span> <span data-ttu-id="05104-240">A toocollect hello rendelésinformációkat toopass hello sablon paraméterként van szüksége, nyissa meg a hello sablonfájl (Ez a témakör tartalmaz egy sablon beágyazott alább), és vizsgálja meg a hello **paraméterek** értékeket.</span><span class="sxs-lookup"><span data-stu-id="05104-240">In order toocollect hello information you need toopass hello template as parameters, open hello template file (this topic has a template inline, below) and examine hello **parameters** values.</span></span>

<span data-ttu-id="05104-241">Ebben az esetben az alábbi hello sablon kéri:</span><span class="sxs-lookup"><span data-stu-id="05104-241">In this case, hello template below will ask for:</span></span>

* <span data-ttu-id="05104-242">Egyedi tárfióknév.</span><span class="sxs-lookup"><span data-stu-id="05104-242">A unique storage account name.</span></span>
* <span data-ttu-id="05104-243">Egy virtuális gép hello rendszergazda felhasználó felhasználóneve.</span><span class="sxs-lookup"><span data-stu-id="05104-243">An admin user name for hello VM.</span></span>
* <span data-ttu-id="05104-244">Jelszó.</span><span class="sxs-lookup"><span data-stu-id="05104-244">A password.</span></span>
* <span data-ttu-id="05104-245">Hello world toouse kívül tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="05104-245">A domain name for hello outside world toouse.</span></span>
* <span data-ttu-id="05104-246">Ubuntu Server-verziószám – de a rendszer csak egyet fogad el egy listáról.</span><span class="sxs-lookup"><span data-stu-id="05104-246">An Ubuntu Server version number -- but it will accept only one of a list.</span></span>

<span data-ttu-id="05104-247">További információk a [felhasználónév- és jelszókövetelményekről](../articles/virtual-machines/linux/faq.md#what-are-the-username-requirements-when-creating-a-vm).</span><span class="sxs-lookup"><span data-stu-id="05104-247">See more about [username and password requirements](../articles/virtual-machines/linux/faq.md#what-are-the-username-requirements-when-creating-a-vm).</span></span>

<span data-ttu-id="05104-248">Ha eldöntötte, ezeket az értékeket, Ön készen toocreate egy csoportot, és ez a sablon üzembe helyezés az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="05104-248">Once you decide on these values, you're ready toocreate a group for and deploy this template into your Azure subscription.</span></span>

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "newStorageAccountName": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for hello storage account where hello virtual machine's disks will be placed."
    }
    },
    "adminUsername": {
    "type": "string",
    "metadata": {
        "description": "User name for hello virtual machine."
    }
    },
    "adminPassword": {
    "type": "securestring",
    "metadata": {
        "description": "Password for hello virtual machine."
    }
    },
    "dnsNameForPublicIP": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for hello public IP used tooaccess hello virtual machine."
    }
    },
    "ubuntuOSVersion": {
    "type": "string",
    "defaultValue": "14.04.2-LTS",
    "allowedValues": [
        "12.04.5-LTS",
        "14.04.2-LTS",
        "15.04"
    ],
    "metadata": {
        "description": "hello Ubuntu version for hello VM. This will pick a fully patched image of this given Ubuntu version. Allowed values: 12.04.5-LTS, 14.04.2-LTS, 15.04."
    }
    }
},
"variables": {
    "location": "West US",
    "imagePublisher": "Canonical",
    "imageOffer": "UbuntuServer",
    "OSDiskName": "osdiskforlinuxsimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyUbuntuVM",
    "vmSize": "Standard_D1",
    "virtualNetworkName": "MyVNET",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
},
"resources": [
    {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('newStorageAccountName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[variables('location')]",
    "properties": {
        "accountType": "[variables('storageAccountType')]"
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[variables('publicIPAddressName')]",
    "location": "[variables('location')]",
    "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
        "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
        }
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
    "location": "[variables('location')]",
    "properties": {
        "addressSpace": {
        "addressPrefixes": [
            "[variables('addressPrefix')]"
        ]
        },
        "subnets": [
        {
            "name": "[variables('subnetName')]",
            "properties": {
            "addressPrefix": "[variables('subnetPrefix')]"
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('nicName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
    ],
    "properties": {
        "ipConfigurations": [
        {
            "name": "ipconfig1",
            "properties": {
            "privateIPAllocationMethod": "Dynamic",
            "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
            },
            "subnet": {
                "id": "[variables('subnetRef')]"
            }
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', parameters('newStorageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
        "computername": "[variables('vmName')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
        "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('ubuntuOSVersion')]",
            "version": "latest"
        },
        "osDisk": {
            "name": "osdisk",
            "vhd": {
            "uri": "[concat('http://',parameters('newStorageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
        }
        },
        "networkProfile": {
        "networkInterfaces": [
            {
            "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
        ]
        }
    }
    }
]
}
```

### <a name="step-2-create-hello-virtual-machine-by-using-hello-template"></a><span data-ttu-id="05104-249">2. lépés: Hello virtuális gép létrehozása hello sablon használatával</span><span class="sxs-lookup"><span data-stu-id="05104-249">Step 2: Create hello virtual machine by using hello template</span></span>
<span data-ttu-id="05104-250">Ha már készen áll a paraméterértékek, hozzon létre egy erőforráscsoportot a sablon üzembe helyezésére, és majd a hello sablon üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="05104-250">Once you have your parameter values ready, you must create a resource group for your template deployment and then deploy hello template.</span></span>

<span data-ttu-id="05104-251">toocreate hello erőforráscsoportot, a típus `azure group create <group name> <location>` hello nevű hello csoportot, és szeretné toodeploy hello adatközpont helye.</span><span class="sxs-lookup"><span data-stu-id="05104-251">toocreate hello resource group, type `azure group create <group name> <location>` with hello name of hello group you want and hello datacenter location into which you want toodeploy.</span></span> <span data-ttu-id="05104-252">Ez gyorsan megtörténik:</span><span class="sxs-lookup"><span data-stu-id="05104-252">This happens quickly:</span></span>

```azurecli
azure group create myResourceGroup westus
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

<span data-ttu-id="05104-253">Most toocreate hello telepítés, a hívás `azure group deployment create` , és adja át:</span><span class="sxs-lookup"><span data-stu-id="05104-253">Now toocreate hello deployment, call `azure group deployment create` and pass:</span></span>

* <span data-ttu-id="05104-254">hello sablonfájl (Ha mentett JSON-sablon tooa helyi fájl fent hello).</span><span class="sxs-lookup"><span data-stu-id="05104-254">hello template file (if you saved hello above JSON template tooa local file).</span></span>
* <span data-ttu-id="05104-255">A sablon URI (Ha azt szeretné, hogy a hello fájlt a Githubon vagy valamilyen más webcímet toopoint).</span><span class="sxs-lookup"><span data-stu-id="05104-255">A template URI (if you want toopoint at hello file in GitHub or some other web address).</span></span>
* <span data-ttu-id="05104-256">szeretné toodeploy hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="05104-256">hello resource group into which you want toodeploy.</span></span>
* <span data-ttu-id="05104-257">A központi telepítés nevét (nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="05104-257">An optional deployment name.</span></span>

<span data-ttu-id="05104-258">Rákérdezéses toosupply hello értékeket a paraméterek hello "paraméterek" részében hello JSON-fájl lesz.</span><span class="sxs-lookup"><span data-stu-id="05104-258">You will be prompted toosupply hello values of parameters in hello "parameters" section of hello JSON file.</span></span> <span data-ttu-id="05104-259">Minden hello paraméterértékek megadását, megkezdődik a telepítés.</span><span class="sxs-lookup"><span data-stu-id="05104-259">When you have specified all hello parameter values, your deployment will begin.</span></span>

<span data-ttu-id="05104-260">Például:</span><span class="sxs-lookup"><span data-stu-id="05104-260">Here is an example:</span></span>

```azurecli
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json myResourceGroup firstDeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
newStorageAccountName: storageaccount
adminUsername: ops
adminPassword: password
dnsNameForPublicIP: newdomainname
```

<span data-ttu-id="05104-261">A következő típusú információkat hello fog kapni:</span><span class="sxs-lookup"><span data-stu-id="05104-261">You will receive hello following type of information:</span></span>

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "firstDeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment toocomplete
data:    DeploymentName     : firstDeployment
data:    ResourceGroupName  : myResourceGroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T07:53:55.1828878Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  -------------
data:    newStorageAccountName  String        storageaccount
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameForPublicIP     String        newdomainname
data:    ubuntuOSVersion        String        14.10
info:    group deployment create command OK
```


## <span data-ttu-id="05104-262"><a id="create-a-custom-vm-image"></a>Feladat: Egyéni virtuális gép rendszerképének létrehozása</span><span class="sxs-lookup"><span data-stu-id="05104-262"><a id="create-a-custom-vm-image"></a>Task: Create a custom VM image</span></span>
<span data-ttu-id="05104-263">Most láthatta, a fenti sablonok hello alapvető használatát, így most is használunk hasonló utasításokat toocreate egyéni virtuális gép egy adott .vhd fájl az Azure-ban sablonnal keresztül hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="05104-263">You've seen hello basic usage of templates above, so now we can use similar instructions toocreate a custom VM from a specific .vhd file in Azure by using a template via hello Azure CLI.</span></span> <span data-ttu-id="05104-264">hello különbség itt az, hogy ez a sablon a megadott virtuális merevlemezről (VHD) egyetlen virtuális gép létrehoz.</span><span class="sxs-lookup"><span data-stu-id="05104-264">hello difference here is that this template creates a single virtual machine from a specified virtual hard disk (VHD).</span></span>

### <a name="step-1-examine-hello-json-file-for-hello-template"></a><span data-ttu-id="05104-265">1. lépés:, Vizsgálja meg hello hello sablon JSON-fájl</span><span class="sxs-lookup"><span data-stu-id="05104-265">Step 1: Examine hello JSON file for hello template</span></span>
<span data-ttu-id="05104-266">Az alábbiakban hello JSON-fájl hello sablonból, amely ebben a szakaszban példaként hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="05104-266">Here are hello contents of hello JSON file for hello template that this section uses as an example.</span></span> <span data-ttu-id="05104-267">(hello sablon is található [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).)</span><span class="sxs-lookup"><span data-stu-id="05104-267">(hello template is also located in [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).)</span></span>

<span data-ttu-id="05104-268">Ebben az esetben szüksége lesz toofind hello értékeket a tooenter hello paraméterek, amely nem rendelkezik alapértelmezett értékekkel.</span><span class="sxs-lookup"><span data-stu-id="05104-268">Again, you will need toofind hello values you want tooenter for hello parameters that do not have default values.</span></span> <span data-ttu-id="05104-269">Hello futtatásakor `azure group deployment create` parancs hello Azure parancssori felület felszólítja, tooenter ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="05104-269">When you run hello `azure group deployment create` command, hello Azure CLI will prompt you tooenter those values.</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters" : {
        "userImageStorageAccountName": {
            "type": "string",
            "defaultValue" : "userImageStorageAccountName"
        },
        "userImageStorageContainerName" : {
            "type" : "string",
            "defaultValue" : "userImageStorageContainerName"
        },
        "userImageVhdName" : {
            "type" : "string",
            "defaultValue" : "userImageVhdName"
        },
        "dnsNameForPublicIP" : {
            "type" : "string",
            "defaultValue": "uniqueDnsNameForPublicIP"
        },
        "adminUserName": {
            "type": "string"
        },
        "adminPassword": {
            "type": "securestring"
        },
        "osType" : {
            "type" : "string",
            "allowedValues" : [
                "windows",
                "linux"
            ]
        },
        "subscriptionId":{
            "type" : "string"
        },
        "location": {
            "type": "String",
            "defaultValue" : "West US"
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A2"
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue" : "myPublicIP"
        },
        "vmName": {
            "type": "string",
            "defaultValue" : "myVM"
        },
        "virtualNetworkName":{
            "type" : "string",
            "defaultValue" : "myVNET"
        },
        "nicName":{
            "type" : "string",
            "defaultValue":"myNIC"
        }
    },
    "variables": {
        "addressPrefix":"10.0.0.0/16",
        "subnet1Name": "Subnet-1",
        "subnet2Name": "Subnet-2",
        "subnet1Prefix" : "10.0.0.0/24",
        "subnet2Prefix" : "10.0.1.0/24",
        "publicIPAddressType" : "Dynamic",
        "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
        "subnet1Ref" : "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
        "userImageName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('userImageVhdName'))]",
        "osDiskVhdName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
    },
    "resources": [
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[parameters('publicIPAddressName')]",
        "location": "[parameters('location')]",
        "properties": {
            "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
            }
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/virtualNetworks",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "properties": {
        "addressSpace": {
            "addressPrefixes": [
            "[variables('addressPrefix')]"
            ]
        },
        "subnets": [
            {
            "name": "[variables('subnet1Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet1Prefix')]"
            }
            },
            {
            "name": "[variables('subnet2Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet2Prefix')]"
            }
            }
        ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[parameters('nicName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
            "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
        ],
        "properties": {
            "ipConfigurations": [
            {
                "name": "ipconfig1",
                "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                        "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                    },
                    "subnet": {
                        "id": "[variables('subnet1Ref')]"
                    }
                }
            }
            ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[parameters('vmName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
        ],
        "properties": {
            "hardwareProfile": {
                "vmSize": "[parameters('vmSize')]"
            },
            "osProfile": {
                "computername": "[parameters('vmName')]",
                "adminUsername": "[parameters('adminUsername')]",
                "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
                "osDisk" : {
                    "name" : "[concat(parameters('vmName'),'-osDisk')]",
                    "osType" : "[parameters('osType')]",
                    "caching" : "ReadWrite",
                    "image" : {
                        "uri" : "[variables('userImageName')]"
                    },
                    "vhd" : {
                        "uri" : "[variables('osDiskVhdName')]"
                    }
                }
            },
            "networkProfile": {
                "networkInterfaces" : [
                {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                }
                ]
            }
        }
    }
    ]
}
```

### <a name="step-2-obtain-hello-vhd"></a><span data-ttu-id="05104-270">2. lépés: Virtuális merevlemez hello beszerzése</span><span class="sxs-lookup"><span data-stu-id="05104-270">Step 2: Obtain hello VHD</span></span>
<span data-ttu-id="05104-271">Ehhez természetesen szükség van egy .vhd fájlra.</span><span class="sxs-lookup"><span data-stu-id="05104-271">Obviously, you'll need a .vhd for this.</span></span> <span data-ttu-id="05104-272">Ha már rendelkezik eggyel az Azure-ban, használhatja azt, vagy feltölthet egyet.</span><span class="sxs-lookup"><span data-stu-id="05104-272">You can use one you already have in Azure, or you can upload one.</span></span>

<span data-ttu-id="05104-273">Windows-alapú virtuális gép esetén lásd: [létrehozása és feltöltése a Windows Server VHD tooAzure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="05104-273">For a Windows-based virtual machine, see [Create and upload a Windows Server VHD tooAzure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="05104-274">Linux-alapú virtuális gép esetén lásd: [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="05104-274">For a Linux-based virtual machine, see [Creating and uploading a virtual hard disk that contains hello Linux operating system](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

### <a name="step-3-create-hello-virtual-machine-by-using-hello-template"></a><span data-ttu-id="05104-275">3. lépés: Hello virtuális gép létrehozása hello sablon használatával</span><span class="sxs-lookup"><span data-stu-id="05104-275">Step 3: Create hello virtual machine by using hello template</span></span>
<span data-ttu-id="05104-276">Most már készen áll a toocreate hello .vhd alapuló új virtuális gép most.</span><span class="sxs-lookup"><span data-stu-id="05104-276">Now you're ready toocreate a new virtual machine based on hello .vhd.</span></span> <span data-ttu-id="05104-277">Hozzon létre egy csoport toodeploy, `azure group create <location>`:</span><span class="sxs-lookup"><span data-stu-id="05104-277">Create a group toodeploy into, by using `azure group create <location>`:</span></span>

```azurecli
azure group create myResourceGroupUser eastus
info:    Executing command group create
+ Getting resource group myResourceGroupUser
+ Creating resource group myResourceGroupUser
info:    Created resource group myResourceGroupUser
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupUser
data:    Name:                myResourceGroupUser
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

<span data-ttu-id="05104-278">Majd hello segítségével a hello központi telepítés létrehozásához `--template-uri` beállítás toocall közvetlenül hello sablonban (vagy használhatja a hello `--template-file` beállítás toouse helyileg mentett fájl).</span><span class="sxs-lookup"><span data-stu-id="05104-278">Then create hello deployment by using hello `--template-uri` option toocall in hello template directly (or you can use hello `--template-file` option toouse a file that you have saved locally).</span></span> <span data-ttu-id="05104-279">Jegyezze fel, mert hello szolgáltatássablonra megadott alapértelmezett értéke, meg kell adni csak néhány dolgot.</span><span class="sxs-lookup"><span data-stu-id="05104-279">Note that because hello template has defaults specified, you are prompted for only a few things.</span></span> <span data-ttu-id="05104-280">Ha hello sablon különböző helyeken telepít, előfordulhat, hogy néhány elnevezési ütközések hello alapértelmezett értékükön (különösen hello DNS-név létrehozása) történik-e.</span><span class="sxs-lookup"><span data-stu-id="05104-280">If you deploy hello template in different places, you may find that some naming collisions occur with hello default values (particularly hello DNS name you create).</span></span>

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json \
> myResourceGroup \
> customVhdDeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
adminUserName: ops
adminPassword: password
osType: linux
subscriptionId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

<span data-ttu-id="05104-281">Kimeneti alábbihoz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="05104-281">Output looks something like hello following:</span></span>

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "customVhdDeployment"
+ Registering providers
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment toocomplete
error:   Deployment provisioning state was not successful
data:    DeploymentName     : customVhdDeployment
data:    ResourceGroupName  : myResourceGroupUser
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T14:55:48.0963829Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                           Type          Value
data:    -----------------------------  ------------  ------------------------------------
data:    userImageStorageAccountName    String        userImageStorageAccountName
data:    userImageStorageContainerName  String        userImageStorageContainerName
data:    userImageVhdName               String        userImageVhdName
data:    dnsNameForPublicIP             String        uniqueDnsNameForPublicIP
data:    adminUserName                  String        ops
data:    adminPassword                  SecureString  undefined
data:    osType                         String        linux
data:    subscriptionId                 String        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
data:    location                       String        West US
data:    vmSize                         String        Standard_A2
data:    publicIPAddressName            String        myPublicIP
data:    vmName                         String        myVM
data:    virtualNetworkName             String        myVNET
data:    nicName                        String        myNIC
info:    group deployment create command OK
```

## <span data-ttu-id="05104-282"><a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Feladat: Virtuális hálózatot és külső terheléselosztót használó több virtuális gépes alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="05104-282"><a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Task: Deploy a multi-VM application that uses a virtual network and an external load balancer</span></span>
<span data-ttu-id="05104-283">Ez a sablon lehetővé teszi a toocreate két virtuális gép egy terheléselosztó alatt, és konfiguráljon egy terheléselosztási szabályt a 80-as Port.</span><span class="sxs-lookup"><span data-stu-id="05104-283">This template allows you toocreate two virtual machines under a load balancer and configure a load-balancing rule on Port 80.</span></span> <span data-ttu-id="05104-284">Emellett ez a sablon üzembe helyez egy tárfiókot, egy virtuális hálózatot, és nyilvános IP-címet, egy rendelkezésre állási csoportot és hálózati adaptereket is.</span><span class="sxs-lookup"><span data-stu-id="05104-284">This template also deploys a storage account, virtual network, public IP address, availability set, and network interfaces.</span></span>

![](./media/virtual-machines-common-cli-deploy-templates/multivmextlb.png)

<span data-ttu-id="05104-285">Kövesse ezeket a lépéseket toodeploy egy virtuális Gépre kiterjedő alkalmazás, amely egy virtuális hálózatot és egy terheléselosztót használ hello GitHub sablon tárházban Azure PowerShell-parancsok segítségével a Resource Manager-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="05104-285">Follow these steps toodeploy a multi-VM application that uses a virtual network and a load balancer by using a Resource Manager template in hello GitHub template repository via Azure PowerShell commands.</span></span>

### <a name="step-1-examine-hello-json-file-for-hello-template"></a><span data-ttu-id="05104-286">1. lépés:, Vizsgálja meg hello hello sablon JSON-fájl</span><span class="sxs-lookup"><span data-stu-id="05104-286">Step 1: Examine hello JSON file for hello template</span></span>
<span data-ttu-id="05104-287">Az alábbiakban hello hello hello sablon JSON-fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="05104-287">Here are hello contents of hello JSON file for hello template.</span></span> <span data-ttu-id="05104-288">Ha azt szeretné, hogy hello legújabb verziója, akkor található [hello GitHub-tárházban sablonok](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="05104-288">If you want hello most recent version, it's located [at hello GitHub repository for templates](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json).</span></span> <span data-ttu-id="05104-289">Ez a témakör használ hello `--template-uri` kapcsoló toocall hello sablon, de használhatja hello `--template-file` toopass egy helyi verzióra váltani.</span><span class="sxs-lookup"><span data-stu-id="05104-289">This topic uses hello `--template-uri` switch toocall in hello template, but you can also use hello `--template-file` switch toopass a local version.</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of resources"
            }
        },
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "Name of storage account"
            }
        },
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "Admin user name"
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Admin password"
            }
        },
        "dnsNameforLBIP": {
            "type": "string",
            "metadata": {
                "description": "DNS for load balancer IP"
            }
        },
        "backendPort": {
            "type": "int",
            "defaultValue": 3389,
            "metadata": {
                "description": "Back-end port"
            }
        },
        "vmNamePrefix": {
            "type": "string",
            "defaultValue": "myVM",
            "metadata": {
                "description": "Prefix toouse for VM names"
            }
        },
        "vmSourceImageName": {
            "type": "string",
            "defaultValue": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201412.01-en.us-127GB.vhd"
        },
        "lbName": {
            "type": "string",
            "defaultValue": "myLB",
            "metadata": {
                "description": "Load balancer name"
            }
        },
        "nicNamePrefix": {
            "type": "string",
            "defaultValue": "nic",
            "metadata": {
                "description": "Network interface name prefix"
            }
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue": "myPublicIP",
            "metadata": {
                "description": "Public IP name"
            }
        },
        "vnetName": {
            "type": "string",
            "defaultValue": "myVNET",
            "metadata": {
                "description": "Virtual network name"
            }
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A1",
            "metadata": {
                "description": "Size of hello VM"
            }
        }
    },
    "variables": {
        "storageAccountType": "Standard_LRS",
        "vmStorageAccountContainerName": "vhds",
        "availabilitySetName": "myAvSet",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "Subnet-1",
        "subnetPrefix": "10.0.0.0/24",
        "publicIPAddressType": "Dynamic",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
        "numberOfInstances": 2,
        "nicId1": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 0))]",
        "nicId2": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 1))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LBFE')]",
        "backEndIPConfigID1": "[concat(variables('nicId1'),'/ipConfigurations/ipconfig1')]",
        "backEndIPConfigID2": "[concat(variables('nicId2'),'/ipConfigurations/ipconfig1')]",
        "sourceImageName": "[concat('/', subscription().subscriptionId,'/services/images/',parameters('vmSourceImageName'))]",
        "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/LBBE')]",
        "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": { }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('dnsNameforLBIP')]"
                }
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('vnetName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnetName')]",
                        "properties": {
                            "addressPrefix": "[variables('subnetPrefix')]"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/networkInterfaces",
            "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
            "location": "[parameters('location')]",
            "copy": {
                "name": "nicLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "subnet": {
                                "id": "[variables('subnetRef')]"
                            }
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/backendAddressPools/LBBE')]"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/inboundNatRule/RDP-VM', copyindex())]"
                            }
                        ]


                    }
                ]

            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "name": "[parameters('lbName')]",
            "type": "Microsoft.Network/loadBalancers",
            "location": "[parameters('location')]",
            "dependsOn": [
                "nicLoop",
                "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
            ],
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "LBFE",
                        "properties": {
                            "publicIPAddress": {
                                "id": "[variables('publicIPAddressID')]"
                            }
                        }
                    }
                ],
                "backendAddressPools": [
                    {
                        "name": "LBBE"

                    }
                ],
                "inboundNatRules": [
                    {
                        "name": "RDP-VM1",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50001,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    },
                    {
                        "name": "RDP-VM2",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50002,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "LBRule",
                        "properties": {
                            "frontendIPConfiguration": {
                                "id": "[variables('frontEndIPConfigID')]"
                            },
                            "backendAddressPool": {
                                "id": "[variables('lbPoolID')]"
                            },
                            "protocol": "tcp",
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 5,
                            "probe": {
                                "id": "[variables('lbProbeID')]"
                            }
                        }
                    }
                ],
                "probes": [
                    {
                        "name": "tcpProbe",
                        "properties": {
                            "protocol": "tcp",
                            "port": 80,
                            "intervalInSeconds": "5",
                            "numberOfProbes": "2"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
            "copy": {
                "name": "virtualMachineLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
                "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
                "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
            ],
            "properties": {
                "availabilitySet": {
                    "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
                },
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computername": "[concat(parameters('vmNamePrefix'), copyIndex())]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "sourceImage": {
                        "id": "[variables('sourceImageName')]"
                    },
                    "destinationVhdsContainer": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/')]"
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="step-2-create-hello-deployment-by-using-hello-template"></a><span data-ttu-id="05104-290">2. lépés: Hello központi telepítés létrehozásához hello sablon használatával</span><span class="sxs-lookup"><span data-stu-id="05104-290">Step 2: Create hello deployment by using hello template</span></span>
<span data-ttu-id="05104-291">Hozzon létre egy erőforráscsoportot hello sablon használatával `azure group create <location>`.</span><span class="sxs-lookup"><span data-stu-id="05104-291">Create a resource group for hello template by using `azure group create <location>`.</span></span> <span data-ttu-id="05104-292">Ezután hozzon létre egy erőforráscsoport szolgáltatássablonjaikat használatával `azure group deployment create` hello erőforráscsoport sikeres, majd átadja a központi telepítés nevét, és a hello kérdések megválaszolásával hello sablont, amely nem volt alapértelmezett értékeket a paraméterek.</span><span class="sxs-lookup"><span data-stu-id="05104-292">Then, create a deployment into that resource group by using `azure group deployment create` and passing hello resource group, passing a deployment name, and answering hello prompts for parameters in hello template that did not have default values.</span></span>

```azurecli
azure group create lbgroup westus
info:    Executing command group create
+ Getting resource group lbgroup
+ Creating resource group lbgroup
info:    Created resource group lbgroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/lbgroup
data:    Name:                lbgroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

<span data-ttu-id="05104-293">Most már használja a hello `azure group deployment create` parancs és hello `--template-uri` beállítás toodeploy hello sablont.</span><span class="sxs-lookup"><span data-stu-id="05104-293">Now use hello `azure group deployment create` command and hello `--template-uri` option toodeploy hello template.</span></span> <span data-ttu-id="05104-294">Készítse elő a paraméterértékeket, hogy megadja őket, amikor a rendszer kéri, ahogyan az az alábbiakban látható.</span><span class="sxs-lookup"><span data-stu-id="05104-294">Be ready with your parameter values when it prompts you, as shown below.</span></span>

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json \
> lbgroup \
> newdeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
location: westus
newStorageAccountName: storagename
adminUsername: ops
adminPassword: password
dnsNameforLBIP: lbdomainname
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "newdeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.compute
info:    Registering provider microsoft.network
+ Waiting for deployment toocomplete
data:    DeploymentName     : newdeployment
data:    ResourceGroupName  : lbgroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T20:58:40.1678876Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  ----------------------
data:    location               String        westus
data:    newStorageAccountName  String        storagename
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameforLBIP         String        lbdomainname
data:    backendPort            Int           3389
data:    vmNamePrefix           String        myVM
data:    imagePublisher         String        MicrosoftWindowsServer
data:    imageOffer             String        WindowsServer
data:    imageSKU               String        2012-R2-Datacenter
data:    lbName                 String        myLB
data:    nicNamePrefix          String        nic
data:    publicIPAddressName    String        myPublicIP
data:    vnetName               String        myVNET
data:    vmSize                 String        Standard_A1
info:    group deployment create command OK
```

<span data-ttu-id="05104-295">Ez a sablon egy Windows Server rendszerképet telepít, de könnyen helyettesíthető lenne bármilyen Linux rendszerképpel is.</span><span class="sxs-lookup"><span data-stu-id="05104-295">Note that this template deploys a Windows Server image; however, it could easily be replaced by any Linux image.</span></span> <span data-ttu-id="05104-296">Szeretné, hogy több swarm felügyelőkkel rendelkező toocreate egy Docker-fürtöt?</span><span class="sxs-lookup"><span data-stu-id="05104-296">Want toocreate a Docker cluster with multiple swarm managers?</span></span> <span data-ttu-id="05104-297">[Megteheti](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).</span><span class="sxs-lookup"><span data-stu-id="05104-297">[You can do it](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).</span></span>

## <span data-ttu-id="05104-298"><a id="remove-a-resource-group"></a>Feladat: Erőforráscsoport eltávolítása</span><span class="sxs-lookup"><span data-stu-id="05104-298"><a id="remove-a-resource-group"></a>Task: Remove a resource group</span></span>
<span data-ttu-id="05104-299">Ne feledje, hogy tooa erőforráscsoport központilag telepítheti, de ha elkészült, az egyik, törölheti azt használatával `azure group delete <group name>`.</span><span class="sxs-lookup"><span data-stu-id="05104-299">Remember that you can redeploy tooa resource group, but if you are done with one, you can delete it by using `azure group delete <group name>`.</span></span>

```azurecli
azure group delete myResourceGroup
info:    Executing command group delete
Delete resource group myResourceGroup? [y/n] y
+ Deleting resource group myResourceGroup
info:    group delete command OK
```

## <span data-ttu-id="05104-300"><a id="show-the-log-for-a-resource-group-deployment"></a>Feladat: Hello keresse meg a központi telepítés erőforrás csoport megjelenítése</span><span class="sxs-lookup"><span data-stu-id="05104-300"><a id="show-the-log-for-a-resource-group-deployment"></a>Task: Show hello log for a resource group deployment</span></span>
<span data-ttu-id="05104-301">Ez azonos a sablonok létrehozásakor és használatakor.</span><span class="sxs-lookup"><span data-stu-id="05104-301">This one is common while you're creating or using templates.</span></span> <span data-ttu-id="05104-302">hello hívás toodisplay hello telepítési naplói a csoport `azure group log show <groupname>`, amely jeleníti meg, amely akkor hasznos, ha ismertetése, hogy valami történt –, vagy nem elég a bit.</span><span class="sxs-lookup"><span data-stu-id="05104-302">hello call toodisplay hello deployment logs for a group is `azure group log show <groupname>`, which displays quite a bit of information that's useful for understanding why something happened -- or didn't.</span></span> <span data-ttu-id="05104-303">(Az üzemelő példányok hibaelhárításával, illetve általánosságban a hibákkal kapcsolatban az [Azure-telepítések gyakori hibáinak elhárítása az Azure Resource Managerrel](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md) témájú cikkben talál további információt.)</span><span class="sxs-lookup"><span data-stu-id="05104-303">(For more information on troubleshooting your deployments, as well as other information about issues, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md).)</span></span>

<span data-ttu-id="05104-304">tootarget vonatkozó hibákat, például használhatja a hasonló eszközök **jq** tooquery dolgot egy kicsit nagyobb pontosan, mely az egyes hibák például szüksége toocorrect.</span><span class="sxs-lookup"><span data-stu-id="05104-304">tootarget specific failures, for example, you might use tools like **jq** tooquery things a bit more precisely, such as which individual failures you need toocorrect.</span></span> <span data-ttu-id="05104-305">hello alábbi példában **jq** a központi telepítés keresse meg a tooparse **lbgroup**, akik szeretnének hibák.</span><span class="sxs-lookup"><span data-stu-id="05104-305">hello following example uses **jq** tooparse a deployment log for **lbgroup**, looking for failures.</span></span>

```azurecli
azure group log show lbgroup -l --json | jq '.[] | select(.status.value == "Failed") | .properties'
```
<span data-ttu-id="05104-306">A problémát gyorsan felismerheti, elháríthatja, majd megpróbálkozhat a kérdéses művelet újbóli végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="05104-306">You can discover very quickly what went wrong, fix, and retry.</span></span> <span data-ttu-id="05104-307">A következő esetben hello, hello sablon kellett lett létrehozása két virtuális gépek hello zárolást létre hello .vhd egy időben.</span><span class="sxs-lookup"><span data-stu-id="05104-307">In hello following case, hello template had been creating two VMs at hello same time, which created a lock on hello .vhd.</span></span> <span data-ttu-id="05104-308">(Hello sablon módosítása azt, után hello sikeres telepítés gyorsan.)</span><span class="sxs-lookup"><span data-stu-id="05104-308">(After we modified hello template, hello deployment succeeded quickly.)</span></span>

```json
{
    "statusCode": "Conflict",
    "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"hello resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"AcquireDiskLeaseFailed\",\"message\":\"Failed tooacquire lease while creating disk 'osdisk' using blob with URI http://storage.blob.core.windows.net/vhds/osdisk.vhd.\"}]}}"
}
```

## <span data-ttu-id="05104-309"><a id="display-information-about-a-virtual-machine"></a>Feladat: Virtuális gépre vonatkozó információk megjelenítése</span><span class="sxs-lookup"><span data-stu-id="05104-309"><a id="display-information-about-a-virtual-machine"></a>Task: Display information about a virtual machine</span></span>
<span data-ttu-id="05104-310">Megtekintheti adott virtuális gépek kapcsolatos információkat az erőforráscsoportban hello segítségével `azure vm show <groupname> <vmname>` parancsot.</span><span class="sxs-lookup"><span data-stu-id="05104-310">You can see information about specific VMs in your resource group by using hello `azure vm show <groupname> <vmname>` command.</span></span> <span data-ttu-id="05104-311">Ha egynél több virtuális gép a csoportban található, először meg kell egy csoport a virtuális gépek toolist hello segítségével `azure vm list <groupname>`.</span><span class="sxs-lookup"><span data-stu-id="05104-311">If you have more than one VM in your group, you might first need toolist hello VMs in a group by using `azure vm list <groupname>`.</span></span>

```azurecli
azure vm list zoo
info:    Executing command vm list
+ Getting virtual machines
data:    Name   ProvisioningState  Location  Size
data:    -----  -----------------  --------  -----------
data:    myVM0  Succeeded          westus    Standard_A1
data:    myVM1  Failed             westus    Standard_A1
```

<span data-ttu-id="05104-312">Ezután keresheti meg a myVM1-et:</span><span class="sxs-lookup"><span data-stu-id="05104-312">And then, looking up myVM1:</span></span>

```azurecli
azure vm show zoo myVM1
info:    Executing command vm show
+ Looking up hello VM "myVM1"
+ Looking up hello NIC "nic1"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Failed
data:    Name                            :myVM1
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :MicrosoftWindowsServer
data:        Offer                       :WindowsServer
data:        Sku                         :2012-R2-Datacenter
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Windows
data:        Name                        :osdisk
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :http://zoostorageralph.blob.core.windows.net/vhds/osdisk.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Windows Configuration:
data:        Provision VM Agent          :true
data:        Enable automatic updates    :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Network/networkInterfaces/nic1
data:          Primary                   :false
data:          Provisioning State        :Succeeded
data:          Name                      :nic1
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.0.5
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/availabilitySets/MYAVSET
info:    vm show command OK
```

> [!NOTE]
> <span data-ttu-id="05104-313">Ha szeretné, hogy tooprogrammatically tároló, és kezelheti a konzol parancsok hello kimenetét, érdemes lehet például egy JSON-elemző eszköz toouse  **[jq](https://github.com/stedolan/jq)**  vagy  **[jsawk](https://github.com/micha/jsawk)** , vagy nyelvi szalagtár szerepel, amely jó hello feladathoz.</span><span class="sxs-lookup"><span data-stu-id="05104-313">If you want tooprogrammatically store and manipulate hello output of your console commands, you may want toouse a JSON parsing tool such as **[jq](https://github.com/stedolan/jq)** or **[jsawk](https://github.com/micha/jsawk)**, or language libraries that are good for hello task.</span></span>
>
>

## <span data-ttu-id="05104-314"><a id="log-on-to-a-linux-based-virtual-machine"></a>Feladat: Bejelentkezés tooa Linux-alapú virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="05104-314"><a id="log-on-to-a-linux-based-virtual-machine"></a>Task: Log on tooa Linux-based virtual machine</span></span>
<span data-ttu-id="05104-315">Linux-gépek általában csatlakoztatott toothrough SSH.</span><span class="sxs-lookup"><span data-stu-id="05104-315">Typically Linux machines are connected toothrough SSH.</span></span> <span data-ttu-id="05104-316">További információkért lásd: [hogyan toouse Azure Linux SSH](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="05104-316">For more information, see [How toouse SSH with Linux on Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <span data-ttu-id="05104-317"><a id="stop-a-virtual-machine"></a>Feladat: Virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="05104-317"><a id="stop-a-virtual-machine"></a>Task: Stop a VM</span></span>
<span data-ttu-id="05104-318">Futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="05104-318">Run this command:</span></span>

```azurecli
azure vm stop <group name> <virtual machine name>
```

> [!IMPORTANT]
> <span data-ttu-id="05104-319">Ez a paraméter tookeep hello virtuális IP-cím (VIP) hello vnet használja abban az esetben, ha az utolsó virtuális gép a vnetben hello.</span><span class="sxs-lookup"><span data-stu-id="05104-319">Use this parameter tookeep hello virtual IP (VIP) of hello vnet in case it's hello last VM in that vnet.</span></span> <br><br> <span data-ttu-id="05104-320">Ha hello `StayProvisioned` paraméter, akkor fogjuk továbbra is számlázni hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="05104-320">If you use hello `StayProvisioned` parameter, you'll still be billed for hello VM.</span></span>
>
>

## <span data-ttu-id="05104-321"><a id="start-a-virtual-machine"></a>Feladat: Virtuális gép indítása</span><span class="sxs-lookup"><span data-stu-id="05104-321"><a id="start-a-virtual-machine"></a>Task: Start a VM</span></span>
<span data-ttu-id="05104-322">Futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="05104-322">Run this command:</span></span>

```azurecli
azure vm start <group name> <virtual machine name>
```

## <span data-ttu-id="05104-323"><a id="attach-a-data-disk"></a>Feladat: Adatlemez csatolása</span><span class="sxs-lookup"><span data-stu-id="05104-323"><a id="attach-a-data-disk"></a>Task: Attach a data disk</span></span>
<span data-ttu-id="05104-324">Meg kell toodecide e tooattach új lemez vagy egy tartalmazó adatokat.</span><span class="sxs-lookup"><span data-stu-id="05104-324">You'll also need toodecide whether tooattach a new disk or one that contains data.</span></span> <span data-ttu-id="05104-325">Egy új lemez hello parancs hello .vhd fájlt hoz létre, és csatolja a hello ugyanezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="05104-325">For a new disk, hello command creates hello .vhd file and attaches it in hello same command.</span></span>

<span data-ttu-id="05104-326">egy új lemezt tooattach futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="05104-326">tooattach a new disk, run this command:</span></span>

```azurecli
    azure vm disk attach-new <resource-group> <vm-name> <size-in-gb>
```

<span data-ttu-id="05104-327">egy meglévő adatlemez tooattach futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="05104-327">tooattach an existing data disk, run this command:</span></span>

```azurecli
azure vm disk attach <resource-group> <vm-name> [vhd-url]
```

<span data-ttu-id="05104-328">Majd kell toomount hello lemez, a szokásos módon a Linux.</span><span class="sxs-lookup"><span data-stu-id="05104-328">Then you'll need toomount hello disk, as you normally would in Linux.</span></span>

## <a name="next-steps"></a><span data-ttu-id="05104-329">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="05104-329">Next steps</span></span>
<span data-ttu-id="05104-330">Amennyiben további példák a hello az Azure parancssori felület használatának **arm** mód, lásd: [Using hello Azure parancssori felület Mac, Linux és a Windows Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="05104-330">For far more examples of Azure CLI usage with hello **arm** mode, see [Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md).</span></span> <span data-ttu-id="05104-331">További információ az Azure-erőforrások és a fogalmakat toolearn lásd [Azure Resource Manager áttekintése](../articles/azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="05104-331">toolearn more about Azure resources and their concepts, see [Azure Resource Manager overview](../articles/azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="05104-332">További felhasználható sablonokat az [Azure-gyorssablonok](https://azure.microsoft.com/documentation/templates/) és a [Sablonokat használó alkalmazás-keretrendszerek](../articles/virtual-machines/linux/app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) között talál.</span><span class="sxs-lookup"><span data-stu-id="05104-332">For more templates you can use, see [Azure Quickstart templates](https://azure.microsoft.com/documentation/templates/) and [Application frameworks using templates](../articles/virtual-machines/linux/app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
