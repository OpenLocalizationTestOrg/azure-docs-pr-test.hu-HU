# <a name="scale-agent-nodes-in-a-container-service-cluster"></a><span data-ttu-id="e61dd-101">Ügynökcsomópontok méretezése a Container Service-fürtökben</span><span class="sxs-lookup"><span data-stu-id="e61dd-101">Scale agent nodes in a Container Service cluster</span></span>
<span data-ttu-id="e61dd-102">Miután [Azure Tárolószolgáltatás-fürt telepítése](../articles/container-service/dcos-swarm/container-service-deployment.md), előfordulhat, hogy az ügynök csomópontok toochange hello számát.</span><span class="sxs-lookup"><span data-stu-id="e61dd-102">After [deploying an Azure Container Service cluster](../articles/container-service/dcos-swarm/container-service-deployment.md), you might need toochange hello number of agent nodes.</span></span> <span data-ttu-id="e61dd-103">Például ha több ügynökre van szüksége további tárolóalkalmazások vagy -példányok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="e61dd-103">For example, you might need more agents so you can run more container applications or instances.</span></span> 

<span data-ttu-id="e61dd-104">DC/OS, Docker Swarm vagy Kubernetes fürt csomópontjai ügynök hello számának módosításához hello Azure-portál használatával, vagy Azure CLI 2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="e61dd-104">You can change hello number of agent nodes in a DC/OS, Docker Swarm, or Kubernetes cluster by using hello Azure portal or hello Azure CLI 2.0.</span></span> 

## <a name="scale-with-hello-azure-portal"></a><span data-ttu-id="e61dd-105">Méretezést hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e61dd-105">Scale with hello Azure portal</span></span>

1. <span data-ttu-id="e61dd-106">A hello [Azure-portálon](https://portal.azure.com), tallózással keresse meg **tárolószolgáltatásainak**, majd kattintson a megjeleníteni kívánt toomodify hello tárolószolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e61dd-106">In hello [Azure portal](https://portal.azure.com), browse for **Container services**, and then click hello container service that you want toomodify.</span></span>
2. <span data-ttu-id="e61dd-107">A hello **tárolószolgáltatás** panelen kattintson a **ügynökök**.</span><span class="sxs-lookup"><span data-stu-id="e61dd-107">In hello **Container service** blade, click **Agents**.</span></span>
3. <span data-ttu-id="e61dd-108">A **virtuális gépek száma**, adja meg a szükséges hello ügynökök csomópontok száma.</span><span class="sxs-lookup"><span data-stu-id="e61dd-108">In **VM Count**, enter hello desired number of agents nodes.</span></span>

    ![Egy készlet hello portálon méretezése](./media/container-service-scale/container-service-scale-portal.png)

4. <span data-ttu-id="e61dd-110">toosave hello konfigurációs, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="e61dd-110">toosave hello configuration, click **Save**.</span></span>

## <a name="scale-with-hello-azure-cli-20"></a><span data-ttu-id="e61dd-111">Az Azure CLI 2.0 hello méretezése</span><span class="sxs-lookup"><span data-stu-id="e61dd-111">Scale with hello Azure CLI 2.0</span></span>

<span data-ttu-id="e61dd-112">Győződjön meg arról, hogy Ön [telepített](/cli/azure/install-az-cli2) hello Azure CLI legújabb 2.0 és tooan bejelentkezve azure-fiók (`az login`).</span><span class="sxs-lookup"><span data-stu-id="e61dd-112">Make sure that you [installed](/cli/azure/install-az-cli2) hello latest Azure CLI 2.0 and logged in tooan azure account (`az login`).</span></span>

### <a name="see-hello-current-agent-count"></a><span data-ttu-id="e61dd-113">Lásd: hello aktuális ügynökök száma</span><span class="sxs-lookup"><span data-stu-id="e61dd-113">See hello current agent count</span></span>
<span data-ttu-id="e61dd-114">jelenleg ügynökök száma toosee hello hello fürtben futtatni hello `az acs show` parancsot.</span><span class="sxs-lookup"><span data-stu-id="e61dd-114">toosee hello number of agents currently in hello cluster, run hello `az acs show` command.</span></span> <span data-ttu-id="e61dd-115">Ez azt jelenti, hello fürtkonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="e61dd-115">This shows hello cluster configuration.</span></span> <span data-ttu-id="e61dd-116">Például a parancs azt mutatja be hello nevű hello tároló szolgáltatás konfigurációját a következő hello `containerservice-myACSName` hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="e61dd-116">For example, hello following command shows hello configuration of hello container service named `containerservice-myACSName` in hello resource group `myResourceGroup`:</span></span>

```azurecli
az acs show -g myResourceGroup -n containerservice-myACSName
```

<span data-ttu-id="e61dd-117">hello parancs hello ügynökök számát adja vissza hello `Count` értékét `AgentPoolProfiles`.</span><span class="sxs-lookup"><span data-stu-id="e61dd-117">hello command returns hello number of agents in hello `Count` value under `AgentPoolProfiles`.</span></span>

### <a name="use-hello-az-acs-scale-command"></a><span data-ttu-id="e61dd-118">Használja hello az acs méretezési parancs</span><span class="sxs-lookup"><span data-stu-id="e61dd-118">Use hello az acs scale command</span></span>
<span data-ttu-id="e61dd-119">ügynök csomópontok, futtassa a hello toochange hello száma `az acs scale` parancsot, és a szállítási hello **erőforráscsoport**, **Tárolónév-szolgáltatás**, és a szükséges hello **új ügynökök száma**.</span><span class="sxs-lookup"><span data-stu-id="e61dd-119">toochange hello number of agent nodes, run hello `az acs scale` command and supply hello **resource group**, **container service name**, and hello desired **new agent count**.</span></span> <span data-ttu-id="e61dd-120">Alacsonyabb illetve magasabb érték megadásával vertikálisan le- illetve felskálázhatja a fürtöt.</span><span class="sxs-lookup"><span data-stu-id="e61dd-120">By using a smaller or higher number, you can scale down or up, respectively.</span></span>

<span data-ttu-id="e61dd-121">Például toochange hello ügynökök száma hello előző fürt too10, típus hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="e61dd-121">For example, toochange hello number of agents in hello previous cluster too10, type hello following command:</span></span>

```azurecli
az acs scale -g myResourceGroup -n containerservice-myACSName --new-agent-count 10
```

<span data-ttu-id="e61dd-122">hello Azure CLI 2.0 adja vissza JSON karakterláncként hello új konfigurációjának hello tárolószolgáltatás, beleértve a hello új ügynökök száma.</span><span class="sxs-lookup"><span data-stu-id="e61dd-122">hello Azure CLI 2.0 returns a JSON string representing hello new configuration of hello container service, including hello new agent count.</span></span>

<span data-ttu-id="e61dd-123">További parancsbeállításokért futtassa az `az acs scale --help` parancsot.</span><span class="sxs-lookup"><span data-stu-id="e61dd-123">For more command options, run `az acs scale --help`.</span></span>

## <a name="scaling-considerations"></a><span data-ttu-id="e61dd-124">Méretezési szempontok</span><span class="sxs-lookup"><span data-stu-id="e61dd-124">Scaling considerations</span></span>

* <span data-ttu-id="e61dd-125">hello ügynök csomópontok száma 1 és 100, a határokat is beleértve között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e61dd-125">hello number of agent nodes must be between 1 and 100, inclusive.</span></span> 

* <span data-ttu-id="e61dd-126">A magok kvótájának hello ügynök fürtben található csomópontok számát korlátozhatja.</span><span class="sxs-lookup"><span data-stu-id="e61dd-126">Your cores quota can limit hello number of agent nodes in a cluster.</span></span>

* <span data-ttu-id="e61dd-127">Ügynök csomópont skálázási műveletek esetében alkalmazott tooan Azure virtuális gépek méretezési hello ügynök készletet tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="e61dd-127">Agent node scaling operations are applied tooan Azure virtual machine scale set that contains hello agent pool.</span></span> <span data-ttu-id="e61dd-128">A DC/OS fürtben csak ügynök csomópontok hello titkos készletben méretezve, ebben a cikkben szereplő hello műveletei által.</span><span class="sxs-lookup"><span data-stu-id="e61dd-128">In a DC/OS cluster, only agent nodes in hello private pool are scaled by hello operations shown in this article.</span></span>

* <span data-ttu-id="e61dd-129">Attól függően, hogy a fürt telepítése hello orchestrator külön-külön méretezheti a tároló hello fürtben futó példányok hello száma.</span><span class="sxs-lookup"><span data-stu-id="e61dd-129">Depending on hello orchestrator you deploy in your cluster, you can separately scale hello number of instances of a container running on hello cluster.</span></span> <span data-ttu-id="e61dd-130">Például a DC/OS fürtben, használja a hello [Marathon felhasználói felület](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) toochange hello több példányban egy tároló alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e61dd-130">For example, in a DC/OS cluster, use hello [Marathon UI](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) toochange hello number of instances of a container application.</span></span>

* <span data-ttu-id="e61dd-131">Az ügynökcsomópontok automatikus méretezése a Container Service-fürtökben jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="e61dd-131">Currently, autoscaling of agent nodes in a container service cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e61dd-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e61dd-132">Next steps</span></span>
* <span data-ttu-id="e61dd-133">Tekintse meg az Azure CLI 2.0-parancsok az Azure Container Service szolgáltatásban való használatát bemutató [további példákat](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e61dd-133">See [more examples](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) of using Azure CLI 2.0 commands with Azure Container Service.</span></span>
* <span data-ttu-id="e61dd-134">Ismerkedjen meg a [DC/OS-ügynökkészletekkel](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) az Azure Container Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="e61dd-134">Learn more about [DC/OS agent pools](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) in Azure Container Service.</span></span>

