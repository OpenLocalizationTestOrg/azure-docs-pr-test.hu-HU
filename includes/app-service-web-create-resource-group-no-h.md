Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.

[!INCLUDE [resource group intro text](resource-group.md)]

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westeurope* helyét.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Általában létrehozhat az erőforrás csoport és hello erőforrások környéken régióban. toosee minden támogatott helyek az Azure Web Apps, futtassa a hello `az appservice list-locations` parancsot. 
