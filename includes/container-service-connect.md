# <a name="make-a-remote-connection-tooa-kubernetes-dcos-or-docker-swarm-cluster"></a><span data-ttu-id="19826-101">Ellenőrizze a távoli kapcsolat tooa Kubernetes, DC/OS- vagy Docker Swarm-fürt</span><span class="sxs-lookup"><span data-stu-id="19826-101">Make a remote connection tooa Kubernetes, DC/OS, or Docker Swarm cluster</span></span>
<span data-ttu-id="19826-102">Miután létrehozta az Azure Tárolószolgáltatás-fürtöt, tooconnect toohello fürt toodeploy kell, és munkaterhelések kezelése.</span><span class="sxs-lookup"><span data-stu-id="19826-102">After creating an Azure Container Service cluster, you need tooconnect toohello cluster toodeploy and manage workloads.</span></span> <span data-ttu-id="19826-103">Ez a cikk ismerteti, hogyan tooconnect toohello fő hello virtuális fürt távoli számítógépről.</span><span class="sxs-lookup"><span data-stu-id="19826-103">This article describes how tooconnect toohello master VM of hello cluster from a remote computer.</span></span> 

<span data-ttu-id="19826-104">hello Kubernetes, a DC/OS és a Docker Swarm-fürtök adja meg a HTTP-végpontokról helyileg.</span><span class="sxs-lookup"><span data-stu-id="19826-104">hello Kubernetes, DC/OS, and Docker Swarm clusters provide HTTP endpoints locally.</span></span> <span data-ttu-id="19826-105">A Kubernetes, ehhez a végponthoz biztonságosan tesz elérhetővé a hello internet, és hozzá tud férni hello futtatásával `kubectl` parancssori eszközt a bármely internetkapcsolattal rendelkező gép.</span><span class="sxs-lookup"><span data-stu-id="19826-105">For Kubernetes, this endpoint is securely exposed on hello internet, and you can access it by running hello `kubectl` command-line tool from any internet-connected machine.</span></span> 

<span data-ttu-id="19826-106">A DC/OS- és Docker Swarm azt javasoljuk, hogy a helyi számítógép toohello fürt felügyeleti rendszerről hozzon létre egy secure shell (SSH) alagút.</span><span class="sxs-lookup"><span data-stu-id="19826-106">For DC/OS and Docker Swarm, we recommend that you create a secure shell (SSH) tunnel from your local computer toohello cluster management system.</span></span> <span data-ttu-id="19826-107">Hello alagút létrejötte után parancsok, amelyek hello HTTP-végpontokról és nézet hello orchestrator webes felülete (ha elérhető) a helyi rendszerről futtathatja.</span><span class="sxs-lookup"><span data-stu-id="19826-107">After hello tunnel is established, you can run commands which use hello HTTP endpoints and view hello orchestrator's web interface (if available) from your local system.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="19826-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="19826-108">Prerequisites</span></span>

* <span data-ttu-id="19826-109">Egy, az [Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md)-ben üzembe helyezett Kubernetes-, DC/OS- vagy Docker Swarm-fürt.</span><span class="sxs-lookup"><span data-stu-id="19826-109">A Kubernetes, DC/OS, or Docker Swarm cluster [deployed in Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md).</span></span>
* <span data-ttu-id="19826-110">SSH-RSA titkos kulcs fájlja, megfelelő toohello nyilvános kulcs hozzáadott toohello fürt üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="19826-110">SSH RSA private key file, corresponding toohello public key added toohello cluster during deployment.</span></span> <span data-ttu-id="19826-111">A parancsok használata, hogy hello titkos SSH-kulcs van `$HOME/.ssh/id_rsa` a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="19826-111">These commands assume that hello private SSH key is in `$HOME/.ssh/id_rsa` on your computer.</span></span> <span data-ttu-id="19826-112">További információkat a [macOS és Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) rendszerre vagy a [Windows](../articles/virtual-machines/linux/ssh-from-windows.md) rendszerre vonatkozó útmutatókban találhat.</span><span class="sxs-lookup"><span data-stu-id="19826-112">See these instructions for [macOS and Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) or [Windows](../articles/virtual-machines/linux/ssh-from-windows.md) for more information.</span></span> <span data-ttu-id="19826-113">Hello SSH-kapcsolat nem működik, ha esetleg túl [az SSH-kulcsok alaphelyzetbe](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).</span><span class="sxs-lookup"><span data-stu-id="19826-113">If hello SSH connection isn't working, you may need too [reset your SSH keys](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).</span></span>

## <a name="connect-tooa-kubernetes-cluster"></a><span data-ttu-id="19826-114">Csatlakoztassa tooa Kubernetes fürtöt</span><span class="sxs-lookup"><span data-stu-id="19826-114">Connect tooa Kubernetes cluster</span></span>

<span data-ttu-id="19826-115">Kövesse az alábbi lépéseket tooinstall és konfigurálása `kubectl` a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="19826-115">Follow these steps tooinstall and configure `kubectl` on your computer.</span></span>

> [!NOTE] 
> <span data-ttu-id="19826-116">A Linux vagy macOS, szükség lehet a toorun hello parancsok be ez a szakasz `sudo`.</span><span class="sxs-lookup"><span data-stu-id="19826-116">On Linux or macOS, you might need toorun hello commands in this section using `sudo`.</span></span>
> 

### <a name="install-kubectl"></a><span data-ttu-id="19826-117">A kubectl telepítése</span><span class="sxs-lookup"><span data-stu-id="19826-117">Install kubectl</span></span>
<span data-ttu-id="19826-118">Egyirányú tooinstall az eszköz toouse hello `az acs kubernetes install-cli` Azure CLI 2.0 parancsot.</span><span class="sxs-lookup"><span data-stu-id="19826-118">One way tooinstall this tool is toouse hello `az acs kubernetes install-cli` Azure CLI 2.0 command.</span></span> <span data-ttu-id="19826-119">Ez a parancs, győződjön meg arról, hogy toorun meg [telepített](/cli/azure/install-az-cli2) hello Azure CLI legújabb 2.0 és tooan Azure-fiókjával bejelentkezve (`az login`).</span><span class="sxs-lookup"><span data-stu-id="19826-119">toorun this command, make sure that you [installed](/cli/azure/install-az-cli2) hello latest Azure CLI 2.0 and logged in tooan Azure account (`az login`).</span></span>

```azurecli
# Linux or macOS
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

<span data-ttu-id="19826-120">Alternatív megoldásként letöltheti a hello legújabb `kubectl` ügyfél közvetlenül a hello [Kubernetes feloldja a lap](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).</span><span class="sxs-lookup"><span data-stu-id="19826-120">Alternatively, you can download hello latest `kubectl` client directly from hello [Kubernetes releases page](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).</span></span> <span data-ttu-id="19826-121">További információ: [A kubectl telepítése és beállítása](https://kubernetes.io/docs/tasks/kubectl/install/).</span><span class="sxs-lookup"><span data-stu-id="19826-121">For more information, see [Installing and Setting up kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span></span>

### <a name="download-cluster-credentials"></a><span data-ttu-id="19826-122">A fürt hitelesítő adatainak letöltése</span><span class="sxs-lookup"><span data-stu-id="19826-122">Download cluster credentials</span></span>
<span data-ttu-id="19826-123">Ha már `kubectl` telepítve, toocopy hello fürt hitelesítő adatok tooyour gép kell.</span><span class="sxs-lookup"><span data-stu-id="19826-123">Once you have `kubectl` installed, you need toocopy hello cluster credentials tooyour machine.</span></span> <span data-ttu-id="19826-124">Egyirányú toodo get hello hitelesítő adatokat kell a hello `az acs kubernetes get-credentials` parancsot.</span><span class="sxs-lookup"><span data-stu-id="19826-124">One way toodo get hello credentials is with hello `az acs kubernetes get-credentials` command.</span></span> <span data-ttu-id="19826-125">Hozzáférési hello hello erőforráscsoport és hello nevét hello tároló szolgáltatás-erőforrást:</span><span class="sxs-lookup"><span data-stu-id="19826-125">Pass hello name of hello resource group and hello name of hello container service resource:</span></span>

```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

<span data-ttu-id="19826-126">Ez a parancs letölti hello fürt hitelesítő adatok túl`$HOME/.kube/config`, ahol `kubectl` található toobe vár.</span><span class="sxs-lookup"><span data-stu-id="19826-126">This command downloads hello cluster credentials too`$HOME/.kube/config`, where `kubectl` expects it toobe located.</span></span>

<span data-ttu-id="19826-127">Másik megoldásként használhatja `scp` toosecurely hello fájl másolása a `$HOME/.kube/config` hello fő VM tooyour helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="19826-127">Alternatively, you can use `scp` toosecurely copy hello file from `$HOME/.kube/config` on hello master VM tooyour local machine.</span></span> <span data-ttu-id="19826-128">Példa:</span><span class="sxs-lookup"><span data-stu-id="19826-128">For example:</span></span>

```bash
mkdir $HOME/.kube
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

<span data-ttu-id="19826-129">Ha a Windows, a Windows, hello PuTTy biztonságos fájl másolása ügyfél vagy egy hasonló eszköz Ubuntu Bash is használhatja.</span><span class="sxs-lookup"><span data-stu-id="19826-129">If you are on Windows, you can use Bash on Ubuntu on Windows, hello PuTTy secure file copy client, or a similar tool.</span></span>

### <a name="use-kubectl"></a><span data-ttu-id="19826-130">Kubectl használata</span><span class="sxs-lookup"><span data-stu-id="19826-130">Use kubectl</span></span>

<span data-ttu-id="19826-131">Ha már `kubectl` hello kapcsolat konfigurálva, tesztelje a fürtben található csomópontok hello listázása:</span><span class="sxs-lookup"><span data-stu-id="19826-131">Once you have `kubectl` configured, test hello connection by listing hello nodes in your cluster:</span></span>

```bash
kubectl get nodes
```

<span data-ttu-id="19826-132">Egyéb `kubectl` parancsokat is kipróbálhat.</span><span class="sxs-lookup"><span data-stu-id="19826-132">You can try other `kubectl` commands.</span></span> <span data-ttu-id="19826-133">Például hello Kubernetes irányítópulton megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="19826-133">For example, you can view hello Kubernetes Dashboard.</span></span> <span data-ttu-id="19826-134">Először futtassa a proxy toohello Kubernetes API-kiszolgálóhoz:</span><span class="sxs-lookup"><span data-stu-id="19826-134">First, run a proxy toohello Kubernetes API server:</span></span>

```bash
kubectl proxy
```

<span data-ttu-id="19826-135">hello Kubernetes felhasználói felület érhető el most: `http://localhost:8001/ui`.</span><span class="sxs-lookup"><span data-stu-id="19826-135">hello Kubernetes UI is now available at: `http://localhost:8001/ui`.</span></span>

<span data-ttu-id="19826-136">További információkért lásd: hello [Kubernetes gyors üzembe helyezési](http://kubernetes.io/docs/user-guide/quick-start/).</span><span class="sxs-lookup"><span data-stu-id="19826-136">For more information, see hello [Kubernetes quick start](http://kubernetes.io/docs/user-guide/quick-start/).</span></span>

## <a name="connect-tooa-dcos-or-swarm-cluster"></a><span data-ttu-id="19826-137">Csatlakoztassa tooa DC/OS és Swarm fürtöt</span><span class="sxs-lookup"><span data-stu-id="19826-137">Connect tooa DC/OS or Swarm cluster</span></span>

<span data-ttu-id="19826-138">toouse hello DC/OS- és Docker Swarm-fürtöket Azure tárolószolgáltatáson telepített kövesse ezeket utasításokat toocreate egy SSH-alagút a helyi Linux, macOS vagy a Windows rendszert.</span><span class="sxs-lookup"><span data-stu-id="19826-138">toouse hello DC/OS and Docker Swarm clusters deployed by Azure Container Service, follow these instructions toocreate a SSH tunnel from your local Linux, macOS, or Windows system.</span></span> 

> [!NOTE]
> <span data-ttu-id="19826-139">A jelen útmutatások elsősorban a TCP-forgalom SSH-n keresztüli bújtatására vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="19826-139">These instructions focus on tunneling TCP traffic over SSH.</span></span> <span data-ttu-id="19826-140">A hello belső fürt felügyeleti rendszerek használatával is elkezdheti interaktív SSH-munkamenetet, de nem ajánlott ennek.</span><span class="sxs-lookup"><span data-stu-id="19826-140">You can also start an interactive SSH session with one of hello internal cluster management systems, but we don't recommend this.</span></span> <span data-ttu-id="19826-141">A közvetlenül a belső rendszeren végzett munka növeli a konfiguráció véletlen módosításának kockázatát.</span><span class="sxs-lookup"><span data-stu-id="19826-141">Working directly on an internal system risks inadvertent configuration changes.</span></span>  
> 

### <a name="create-an-ssh-tunnel-on-linux-or-macos"></a><span data-ttu-id="19826-142">SSH-alagút létrehozása Linux vagy macOS rendszeren</span><span class="sxs-lookup"><span data-stu-id="19826-142">Create an SSH tunnel on Linux or macOS</span></span>
<span data-ttu-id="19826-143">Amikor az SSH-alagút létrehozása Linux vagy macOS hello elsőként toolocate hello nyilvános DNS-neve hello terhelésű főkiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="19826-143">hello first thing that you do when you create an SSH tunnel on Linux or macOS is toolocate hello public DNS name of hello load-balanced masters.</span></span> <span data-ttu-id="19826-144">Kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="19826-144">Follow these steps:</span></span>


1. <span data-ttu-id="19826-145">A hello [Azure-portálon](https://portal.azure.com), keresse meg a tárolószolgáltatási fürt tartalmazó toohello erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="19826-145">In hello [Azure portal](https://portal.azure.com), browse toohello resource group containing your container service cluster.</span></span> <span data-ttu-id="19826-146">Bontsa ki a hello erőforráscsoportot, így az egyes erőforrások jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="19826-146">Expand hello resource group so that each resource is displayed.</span></span> 

2. <span data-ttu-id="19826-147">Kattintson a hello **tárolószolgáltatás** erőforrás, majd kattintson **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="19826-147">Click hello **Container service** resource, and click **Overview**.</span></span> <span data-ttu-id="19826-148">Hello **fő FQDN** hello a fürt jelenik meg az **Essentials**.</span><span class="sxs-lookup"><span data-stu-id="19826-148">hello **Master FQDN** of hello cluster appears under **Essentials**.</span></span> <span data-ttu-id="19826-149">Mentse ezt a nevet a későbbi felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="19826-149">Save this name for later use.</span></span> 

    ![Nyilvános DNS-név](./media/container-service-connect/pubdns.png)

    <span data-ttu-id="19826-151">Alternatív megoldásként futtassa hello `az acs show` a tárolószolgáltatás parancsot.</span><span class="sxs-lookup"><span data-stu-id="19826-151">Alternatively, run hello `az acs show` command on your container service.</span></span> <span data-ttu-id="19826-152">Hello keressen **fő profil: fqdn** hello parancs kimenetében tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="19826-152">Look for hello **Master Profile:fqdn** property in hello command output.</span></span>

3. <span data-ttu-id="19826-153">Most nyisson meg egy kezelőfelületet, és futtassa a hello `ssh` hello a következő értékek megadásával parancsot:</span><span class="sxs-lookup"><span data-stu-id="19826-153">Now open a shell and run hello `ssh` command by specifying hello following values:</span></span> 

    <span data-ttu-id="19826-154">**LOCAL_PORT** hello TCP port hello alagút tooconnect való hello szolgáltatás oldalán.</span><span class="sxs-lookup"><span data-stu-id="19826-154">**LOCAL_PORT** is hello TCP port on hello service side of hello tunnel tooconnect to.</span></span> <span data-ttu-id="19826-155">Swarm esetén ez too2375 beállítása.</span><span class="sxs-lookup"><span data-stu-id="19826-155">For Swarm, set this too2375.</span></span> <span data-ttu-id="19826-156">DC/os állítsa be a too80.</span><span class="sxs-lookup"><span data-stu-id="19826-156">For DC/OS, set this too80.</span></span> 
    <span data-ttu-id="19826-157">**REMOTE_PORT** hello port, amelyet az tooexpose hello végpont.</span><span class="sxs-lookup"><span data-stu-id="19826-157">**REMOTE_PORT** is hello port of hello endpoint that you want tooexpose.</span></span> <span data-ttu-id="19826-158">Swarm esetén használja a 2375-ös portot.</span><span class="sxs-lookup"><span data-stu-id="19826-158">For Swarm, use port 2375.</span></span> <span data-ttu-id="19826-159">DC/OS esetén használja a 80-as portot.</span><span class="sxs-lookup"><span data-stu-id="19826-159">For DC/OS, use port 80.</span></span>  
    <span data-ttu-id="19826-160">**FELHASZNÁLÓNÉV** hello fürt telepítésekor megadott felhasználónév hello.</span><span class="sxs-lookup"><span data-stu-id="19826-160">**USERNAME** is hello user name that was provided when you deployed hello cluster.</span></span>  
    <span data-ttu-id="19826-161">**DNSPREFIX** hello hello fürt telepítésekor megadott DNS-előtag van.</span><span class="sxs-lookup"><span data-stu-id="19826-161">**DNSPREFIX** is hello DNS prefix that you provided when you deployed hello cluster.</span></span>  
    <span data-ttu-id="19826-162">**A régióban** hello régió, ahol az erőforráscsoport megtalálható.</span><span class="sxs-lookup"><span data-stu-id="19826-162">**REGION** is hello region in which your resource group is located.</span></span>  
    <span data-ttu-id="19826-163">**PATH_TO_PRIVATE_KEY** [OPCIONÁLIS] hello elérési toohello titkos kulcsot, amely megfelel a toohello hello fürt létrehozásakor megadott nyilvános kulcs.</span><span class="sxs-lookup"><span data-stu-id="19826-163">**PATH_TO_PRIVATE_KEY** [OPTIONAL] is hello path toohello private key that corresponds toohello public key you provided when you created hello cluster.</span></span> <span data-ttu-id="19826-164">Ez a beállítás használata hello `-i` jelzőt.</span><span class="sxs-lookup"><span data-stu-id="19826-164">Use this option with hello `-i` flag.</span></span>

    ```bash
    ssh -fNL LOCAL_PORT:localhost:REMOTE_PORT -p 2200 [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com
    ```
  
  > [!NOTE]
  > <span data-ttu-id="19826-165">hello SSH-kapcsolati port 2200, és nem a szokásos 22-es portot hello.</span><span class="sxs-lookup"><span data-stu-id="19826-165">hello SSH connection port is 2200 and not hello standard port 22.</span></span> <span data-ttu-id="19826-166">Egynél több fő virtuális gép a fürtben Ez a hello kapcsolati port toohello első főkiszolgálójának virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="19826-166">In a cluster with more than one master VM, this is hello connection port toohello first master VM.</span></span>
  > 

  <span data-ttu-id="19826-167">hello parancs kimenete nélkül adja vissza.</span><span class="sxs-lookup"><span data-stu-id="19826-167">hello command returns without output.</span></span>

<span data-ttu-id="19826-168">Példák hello a DC/OS és Swarm a következő részekben hello.</span><span class="sxs-lookup"><span data-stu-id="19826-168">See hello examples for DC/OS and Swarm in hello following sections.</span></span>    

### <a name="dcos-tunnel"></a><span data-ttu-id="19826-169">DC/OS-alagút</span><span class="sxs-lookup"><span data-stu-id="19826-169">DC/OS tunnel</span></span>
<span data-ttu-id="19826-170">tooopen alagutat DC/OS végpontok hello hasonló parancsot:</span><span class="sxs-lookup"><span data-stu-id="19826-170">tooopen a tunnel for DC/OS endpoints, run a command like hello following:</span></span>

```bash
sudo ssh -fNL 80:localhost:80 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com 
```

> [!NOTE]
> <span data-ttu-id="19826-171">Győződjön meg arról, hogy nincs másik olyan helyi folyamat, amely lekötné a 80-as portot.</span><span class="sxs-lookup"><span data-stu-id="19826-171">Ensure that you do not have another local process that binds port 80.</span></span> <span data-ttu-id="19826-172">Ha szükséges, beállíthat más, a 80-as porttól eltérő helyi portokat is (például a 8080-as portot).</span><span class="sxs-lookup"><span data-stu-id="19826-172">If necessary, you can specify a local port other than port 80, such as port 8080.</span></span> <span data-ttu-id="19826-173">Ha azonban ezt a portot használja, előfordulhat, hogy egyes webes felhasználói felületek hivatkozásai nem működnek.</span><span class="sxs-lookup"><span data-stu-id="19826-173">However, some web UI links might not work when you use this port.</span></span>
>

<span data-ttu-id="19826-174">Most a helyi rendszerről hello következő URL-címek (feltéve, hogy a helyi 80-as porton) keresztül hello DC/OS végpontok érhető el:</span><span class="sxs-lookup"><span data-stu-id="19826-174">You can now access hello DC/OS endpoints from your local system through hello following URLs (assuming local port 80):</span></span>

* <span data-ttu-id="19826-175">DC/OS:`http://localhost:80/`</span><span class="sxs-lookup"><span data-stu-id="19826-175">DC/OS: `http://localhost:80/`</span></span>
* <span data-ttu-id="19826-176">Marathon:`http://localhost:80/marathon`</span><span class="sxs-lookup"><span data-stu-id="19826-176">Marathon: `http://localhost:80/marathon`</span></span>
* <span data-ttu-id="19826-177">Mesos:`http://localhost:80/mesos`</span><span class="sxs-lookup"><span data-stu-id="19826-177">Mesos: `http://localhost:80/mesos`</span></span>

<span data-ttu-id="19826-178">Ehhez hasonlóan hello rest API-k minden alkalmazáshoz ezen az alagúton keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="19826-178">Similarly, you can reach hello rest APIs for each application through this tunnel.</span></span>

### <a name="swarm-tunnel"></a><span data-ttu-id="19826-179">Swarm-alagút</span><span class="sxs-lookup"><span data-stu-id="19826-179">Swarm tunnel</span></span>
<span data-ttu-id="19826-180">egy alagút toohello Swarm végponthoz tooopen hello hasonló parancs futtatása:</span><span class="sxs-lookup"><span data-stu-id="19826-180">tooopen a tunnel toohello Swarm endpoint, run a command like hello following:</span></span>

```bash
ssh -fNL 2375:localhost:2375 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com
```
> [!NOTE]
> <span data-ttu-id="19826-181">Győződjön meg arról, hogy nincs másik olyan helyi folyamat, amely lekötné a 2375-ös portot.</span><span class="sxs-lookup"><span data-stu-id="19826-181">Ensure that you do not have another local process that binds port 2375.</span></span> <span data-ttu-id="19826-182">Például ha hello Docker démon helyileg futtat, van beállítva alapértelmezett toouse port 2375.</span><span class="sxs-lookup"><span data-stu-id="19826-182">For example, if you are running hello Docker daemon locally, it's set by default toouse port 2375.</span></span> <span data-ttu-id="19826-183">Ha szükséges, beállíthat más, a 2375-ös porttól eltérő helyi portokat is.</span><span class="sxs-lookup"><span data-stu-id="19826-183">If necessary, you can specify a local port other than port 2375.</span></span>
>

<span data-ttu-id="19826-184">Most hello Docker Swarm-fürt a helyi rendszeren hello Docker parancssori felületet (Docker CLI) használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="19826-184">Now you can access hello Docker Swarm cluster using hello Docker command-line interface (Docker CLI) on your local system.</span></span> <span data-ttu-id="19826-185">A telepítési utasításokért lásd [a Docker telepítését](https://docs.docker.com/engine/installation/) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="19826-185">For installation instructions, see [Install Docker](https://docs.docker.com/engine/installation/).</span></span>

<span data-ttu-id="19826-186">Állítsa be a DOCKER_HOST környezeti változó toohello hello alagúthoz helyi portot.</span><span class="sxs-lookup"><span data-stu-id="19826-186">Set your DOCKER_HOST environment variable toohello local port you configured for hello tunnel.</span></span> 

```bash
export DOCKER_HOST=:2375
```

<span data-ttu-id="19826-187">Futtassa a Docker parancsokat alagút toohello Docker Swarm-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="19826-187">Run Docker commands that tunnel toohello Docker Swarm cluster.</span></span> <span data-ttu-id="19826-188">Példa:</span><span class="sxs-lookup"><span data-stu-id="19826-188">For example:</span></span>

```bash
docker info
```

### <a name="create-an-ssh-tunnel-on-windows"></a><span data-ttu-id="19826-189">SSH-alagút létrehozása Windows rendszeren</span><span class="sxs-lookup"><span data-stu-id="19826-189">Create an SSH tunnel on Windows</span></span>
<span data-ttu-id="19826-190">Windows-rendszeren az SSH-alagutak többféleképpen is létrehozhatók.</span><span class="sxs-lookup"><span data-stu-id="19826-190">There are multiple options for creating SSH tunnels on Windows.</span></span> <span data-ttu-id="19826-191">Ha Bash Ubuntu a Windows vagy egy hasonló eszköz futtat, kövesse hello SSH tunneling megjelenő utasításokat az ebben a cikkben macOS és Linux rendszerekhez.</span><span class="sxs-lookup"><span data-stu-id="19826-191">If you are running Bash on Ubuntu on Windows or a similar tool, you can follow hello SSH tunneling instructions shown earlier in this article for macOS and Linux.</span></span> <span data-ttu-id="19826-192">Másik megoldásként a Windows, ez a szakasz ismerteti, hogyan toouse PuTTY toocreate hello alagutat.</span><span class="sxs-lookup"><span data-stu-id="19826-192">As an alternative on Windows, this section describes how toouse PuTTY toocreate hello tunnel.</span></span>

1. <span data-ttu-id="19826-193">[Töltse le a PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) tooyour Windows rendszert.</span><span class="sxs-lookup"><span data-stu-id="19826-193">[Download PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) tooyour Windows system.</span></span>

2. <span data-ttu-id="19826-194">Hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="19826-194">Run hello application.</span></span>

3. <span data-ttu-id="19826-195">Adjon meg egy állomásnevet, amely hello fürt rendszergazda felhasználóneve és hello hello fürt első főkiszolgálójának hello nyilvános DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="19826-195">Enter a host name that is comprised of hello cluster admin user name and hello public DNS name of hello first master in hello cluster.</span></span> <span data-ttu-id="19826-196">Hello **állomásnév** túl hasonlít`azureuser@PublicDNSName`.</span><span class="sxs-lookup"><span data-stu-id="19826-196">hello **Host Name** looks similar too`azureuser@PublicDNSName`.</span></span> <span data-ttu-id="19826-197">Adja meg a 2200 hello **Port**.</span><span class="sxs-lookup"><span data-stu-id="19826-197">Enter 2200 for hello **Port**.</span></span>

    ![A PuTTY-konfigurálásának 1. lépése](./media/container-service-connect/putty1.png)

4. <span data-ttu-id="19826-199">Válassza az **SSH > Auth** (SSH > Hitelesítés) parancsot. Adja hozzá az elérési út tooyour titkos kulcsfájlt (.ppk formátum) a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="19826-199">Select **SSH > Auth**. Add a path tooyour private key file (.ppk format) for authentication.</span></span> <span data-ttu-id="19826-200">Használhatja például egy eszközt [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) ez fájlként toogenerate hello SSH-kulcs használt toocreate hello fürt.</span><span class="sxs-lookup"><span data-stu-id="19826-200">You can use a tool such as [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) toogenerate this file from hello SSH key used toocreate hello cluster.</span></span>

    ![A PuTTY-konfigurálásának 2. lépése](./media/container-service-connect/putty2.png)

5. <span data-ttu-id="19826-202">Válassza ki **SSH > alagutak** , és konfigurálja a hello következő továbbított portokat:</span><span class="sxs-lookup"><span data-stu-id="19826-202">Select **SSH > Tunnels** and configure hello following forwarded ports:</span></span>

    * <span data-ttu-id="19826-203">**Source port** (Forrásport): DC/OS esetén használja a 80-as, Swarm estén a 2375-ös portot.</span><span class="sxs-lookup"><span data-stu-id="19826-203">**Source Port:** Use 80 for DC/OS or 2375 for Swarm.</span></span>
    * <span data-ttu-id="19826-204">**Destination** (Cél): DC/OS esetén használja a localhost:80, Swarm esetén a localhost:2375 portot.</span><span class="sxs-lookup"><span data-stu-id="19826-204">**Destination:** Use localhost:80 for DC/OS or localhost:2375 for Swarm.</span></span>

    <span data-ttu-id="19826-205">a következő példa hello DC/OS van konfigurálva, de Docker Swarm esetén hasonlóan fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="19826-205">hello following example is configured for DC/OS, but will look similar for Docker Swarm.</span></span>

    > [!NOTE]
    > <span data-ttu-id="19826-206">Amikor ezt az alagutat létrehozza, a 80-as port nem lehet használatban.</span><span class="sxs-lookup"><span data-stu-id="19826-206">Port 80 must not be in use when you create this tunnel.</span></span>
    > 

    ![A PuTTY-konfigurálásának 3. lépése](./media/container-service-connect/putty3.png)

6. <span data-ttu-id="19826-208">Amikor végzett, kattintson a **munkamenet > Mentés** toosave hello kapcsolat konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="19826-208">When you're finished, click **Session > Save** toosave hello connection configuration.</span></span>

7. <span data-ttu-id="19826-209">tooconnect toohello PuTTY munkamenet, kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="19826-209">tooconnect toohello PuTTY session, click **Open**.</span></span> <span data-ttu-id="19826-210">Sikeres csatlakozás esetén hello portkonfigurációjának hello PuTTY eseménynaplójában tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="19826-210">When you connect, you can see hello port configuration in hello PuTTY event log.</span></span>

    ![A PuTTY eseménynaplója](./media/container-service-connect/putty4.png)

<span data-ttu-id="19826-212">Miután konfigurálta a hello alagutat DC/os, van-e hozzáférési hello kapcsolódó a végpontokat:</span><span class="sxs-lookup"><span data-stu-id="19826-212">After you've configured hello tunnel for DC/OS, you can access hello related endpoints at:</span></span>

* <span data-ttu-id="19826-213">DC/OS:`http://localhost/`</span><span class="sxs-lookup"><span data-stu-id="19826-213">DC/OS: `http://localhost/`</span></span>
* <span data-ttu-id="19826-214">Marathon:`http://localhost/marathon`</span><span class="sxs-lookup"><span data-stu-id="19826-214">Marathon: `http://localhost/marathon`</span></span>
* <span data-ttu-id="19826-215">Mesos:`http://localhost/mesos`</span><span class="sxs-lookup"><span data-stu-id="19826-215">Mesos: `http://localhost/mesos`</span></span>

<span data-ttu-id="19826-216">Miután konfigurálta a hello alagutat a Docker Swarmra, nyissa meg a Windows-beállítások tooconfigure nevű rendszer környezeti változó `DOCKER_HOST` értékkel rendelkező `:2375`.</span><span class="sxs-lookup"><span data-stu-id="19826-216">After you've configured hello tunnel for Docker Swarm, open your Windows settings tooconfigure a system environment variable named `DOCKER_HOST` with a value of `:2375`.</span></span> <span data-ttu-id="19826-217">Ezt követően érheti el hello Swarm-fürt hello Docker parancssori felületén keresztül.</span><span class="sxs-lookup"><span data-stu-id="19826-217">Then, you can access hello Swarm cluster through hello Docker CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="19826-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19826-218">Next steps</span></span>
<span data-ttu-id="19826-219">Tárolók telepítése és felügyelete a fürtben:</span><span class="sxs-lookup"><span data-stu-id="19826-219">Deploy and manage containers in your cluster:</span></span>

* [<span data-ttu-id="19826-220">Az Azure Container Service és a Kubernetes használata</span><span class="sxs-lookup"><span data-stu-id="19826-220">Work with Azure Container Service and Kubernetes</span></span>](../articles/container-service/kubernetes/container-service-kubernetes-ui.md)
* [<span data-ttu-id="19826-221">Az Azure Container Service és a DC/OS használata</span><span class="sxs-lookup"><span data-stu-id="19826-221">Work with Azure Container Service and DC/OS</span></span>](../articles/container-service//dcos-swarm/container-service-mesos-marathon-rest.md)
* [<span data-ttu-id="19826-222">Hello Azure Tárolószolgáltatás és a Docker Swarm használata</span><span class="sxs-lookup"><span data-stu-id="19826-222">Work with hello Azure Container Service and Docker Swarm</span></span>](../articles//container-service/dcos-swarm/container-service-docker-swarm.md)

