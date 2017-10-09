## <a name="set-up-azure-cli-for-azure-dns"></a>Az Azure parancssori felület (CLI) beállítása az Azure DNS-hez

### <a name="before-you-begin"></a>Előkészületek

Győződjön meg arról, hogy rendelkezik-e elemek a konfigurációs megkezdése előtt a következő hello.

* Azure-előfizetés. Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).
* Hello hello Azure parancssori felület, elérhető a Windows, Linux vagy Mac legújabb verziójának telepítése További információt [telepítés hello Azure CLI](../articles/cli-install-nodejs.md).

### <a name="sign-in-tooyour-azure-account"></a>Jelentkezzen be tooyour Azure-fiók

Nyisson meg egy konzolablakot, adja meg a saját hitelesítő adatait. További információkért lásd: [tooAzure a hello Azure CLI-e jelentkezni](../articles/xplat-cli-connect.md)

```azurecli
azure login
```

### <a name="switch-cli-mode"></a>Kapcsolja át a parancssori felület működési módját

Az Azure DNS az Azure Resource Managert használja. Váltson át CLI mód toouse Azure Resource Manager parancsok.

```azurecli
azure config mode arm
```

### <a name="select-hello-subscription"></a>Válassza ki a hello előfizetés

Hello előfizetések hello fiók ellenőrzése.

```azurecli
azure account list
```

Válassza ki, amely az Azure-előfizetések toouse.

```azurecli
azure account set "subscription name"
```

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet. Ez az erőforráscsoport erőforrások hello alapértelmezett helye szolgál. Azonban mivel minden DNS-erőforrás globális, nem pedig regionális, hello választás az erőforráscsoport helye nincs hatással van az Azure DNS szolgáltatásra.

Ezt a lépést kihagyhatja, ha egy meglévő erőforráscsoportot használ.

```azurecli
azure group create -n myresourcegroup --location "West US"
```

### <a name="register-resource-provider"></a>Erőforrás-szolgáltató regisztrálása

hello Azure DNS-szolgáltatás hello Microsoft.Network erőforrás-szolgáltató kezeli. Az Azure-előfizetéssel kell regisztrált toouse az erőforrás-szolgáltató Azure DNS használata előtt. Ez a műveletet minden egyes előfizetés esetén csak egyszer kell elvégezni.

```azurecli
azure provider register --namespace Microsoft.Network
```

