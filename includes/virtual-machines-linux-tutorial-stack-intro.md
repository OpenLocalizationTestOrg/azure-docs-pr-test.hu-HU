## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. 

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Virtuális gép létrehozása

Hozzon létre egy virtuális gép hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot. 

hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* és SSH-kulcsok létrehozása, ha még nem léteznek a kulcs alapértelmezett helye. toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Hello virtuális gép létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg. Jegyezze fel a hello `publicIpAddress`. Ez a cím használt tooaccess hello virtuális gép.

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



## <a name="open-port-80-for-web-traffic"></a>A 80-as port megnyitása a webes adatforgalom számára 

Alapértelmezés szerint csak az SSH-kapcsolatok engedélyezettek az Azure szolgáltatásba telepített Linux virtuális gépek. Ez a virtuális gép toobe webkiszolgáló állapotra vált, mert tooopen hello a 80-as portot kell internet. Használjon hello [az vm-port megnyitása](/cli/azure/vm#open-port) parancs tooopen hello port szükséges.  
 
```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```
## <a name="ssh-into-your-vm"></a>Bejelentkezés a virtuális gépre SSH-val


Ha nem biztos a virtuális gép nyilvános IP-cím hello, futtassa a hello [az nyilvános ip-lista](/cli/azure/network/public-ip#list) parancs:


```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

Használjon hello következő parancsot a toocreate egy hello virtuális gép SSH-munkamenetet. Helyettesítse be hello megfelelő nyilvános IP-címet a virtuális gép. Ebben a példában hello IP-cím van *40.68.254.142*.

```bash
ssh azureuser@40.68.254.142
```

