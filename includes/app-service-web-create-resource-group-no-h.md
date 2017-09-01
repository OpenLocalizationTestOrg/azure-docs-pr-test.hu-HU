Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.

[!INCLUDE [resource group intro text](resource-group.md)]

A következő példában létrehozunk egy *myResourceGroup* nevű erőforráscsoportot a *westeurope* helyen.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Az erőforráscsoportot és az erőforrásokat általában a közelében található régiókban hozhatja létre. Az összes Azure Web Apps-t támogató hely megtekintéséhez futtassa az `az appservice list-locations` parancsot. 