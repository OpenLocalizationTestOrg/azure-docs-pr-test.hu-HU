## <a name="create-a-resource-group"></a><span data-ttu-id="d1a3c-101">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="d1a3c-101">Create a resource group</span></span>

<span data-ttu-id="d1a3c-102">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-102">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="d1a3c-103">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-103">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="d1a3c-104">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-104">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a><span data-ttu-id="d1a3c-105">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d1a3c-105">Create a virtual machine</span></span>

<span data-ttu-id="d1a3c-106">Hozzon létre egy virtuális gép hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-106">Create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="d1a3c-107">hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* és SSH-kulcsok létrehozása, ha még nem léteznek a kulcs alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-107">hello following example creates a VM named *myVM* and creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="d1a3c-108">toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-108">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="d1a3c-109">Hello virtuális gép létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-109">When hello VM has been created, hello Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="d1a3c-110">Jegyezze fel a hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-110">Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="d1a3c-111">Ez a cím használt tooaccess hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-111">This address is used tooaccess hello VM.</span></span>

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```



## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="d1a3c-112">A 80-as port megnyitása a webes adatforgalom számára</span><span class="sxs-lookup"><span data-stu-id="d1a3c-112">Open port 80 for web traffic</span></span> 

<span data-ttu-id="d1a3c-113">Alapértelmezés szerint csak az SSH-kapcsolatok engedélyezettek az Azure szolgáltatásba telepített Linux virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-113">By default, only SSH connections are allowed into Linux VMs deployed in Azure.</span></span> <span data-ttu-id="d1a3c-114">Ez a virtuális gép toobe webkiszolgáló állapotra vált, mert tooopen hello a 80-as portot kell internet.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-114">Because this VM is going toobe a web server, you need tooopen port 80 from hello internet.</span></span> <span data-ttu-id="d1a3c-115">Használjon hello [az vm-port megnyitása](/cli/azure/vm#open-port) parancs tooopen hello port szükséges.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-115">Use hello [az vm open-port](/cli/azure/vm#open-port) command tooopen hello desired port.</span></span>  
 
```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```
## <a name="ssh-into-your-vm"></a><span data-ttu-id="d1a3c-116">Bejelentkezés a virtuális gépre SSH-val</span><span class="sxs-lookup"><span data-stu-id="d1a3c-116">SSH into your VM</span></span>


<span data-ttu-id="d1a3c-117">Ha nem biztos a virtuális gép nyilvános IP-cím hello, futtassa a hello [az nyilvános ip-lista](/cli/azure/network/public-ip#list) parancs:</span><span class="sxs-lookup"><span data-stu-id="d1a3c-117">If you don't already know hello public IP address of your VM, run hello [az network public-ip list](/cli/azure/network/public-ip#list) command:</span></span>


```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

<span data-ttu-id="d1a3c-118">Használjon hello következő parancsot a toocreate egy hello virtuális gép SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-118">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="d1a3c-119">Helyettesítse be hello megfelelő nyilvános IP-címet a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-119">Substitute hello correct public IP address of your virtual machine.</span></span> <span data-ttu-id="d1a3c-120">Ebben a példában hello IP-cím van *40.68.254.142*.</span><span class="sxs-lookup"><span data-stu-id="d1a3c-120">In this example, hello IP address is *40.68.254.142*.</span></span>

```bash
ssh azureuser@40.68.254.142
```

