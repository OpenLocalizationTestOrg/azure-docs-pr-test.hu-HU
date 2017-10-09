Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.

[!INCLUDE [resource group intro text](resource-group.md)]

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westeurope* helyét.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

toosee hello elérhető helyről, futtassa a hello `az appservice list-locations` parancsot. Az erőforrásokat általában a közelében található régiókban hozhatja létre.
