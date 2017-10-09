## <a name="set-up-azure-powershell-for-azure-dns"></a>Az Azure DNS az Azure PowerShell beállítása

### <a name="before-you-begin"></a>Előkészületek

Győződjön meg arról, hogy rendelkezik-e elemek a konfigurációs megkezdése előtt a következő hello.

* Azure-előfizetés. Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).
* Tooinstall hello legújabb verziójára hello Azure Resource Manager PowerShell-parancsmagok van szüksége. További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs).

### <a name="sign-in-tooyour-azure-account"></a>Jelentkezzen be tooyour Azure-fiók

Nyissa meg a PowerShell-konzolt, és csatlakozzon a tooyour fiók. További információkért lásd: [PowerShell használata a Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).

```powershell
Login-AzureRmAccount
```

### <a name="select-hello-subscription"></a>Válassza ki a hello előfizetés
 
Hello előfizetések hello fiók ellenőrzése.

```powershell
Get-AzureRmSubscription
```

Válassza ki, amely az Azure-előfizetések toouse.

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet. Ezen a helyen az erőforráscsoport erőforrások lesz hello alapértelmezett helye. Azonban mivel minden DNS-erőforrás globális, nem pedig regionális, hello választás az erőforráscsoport helye nincs hatással van az Azure DNS szolgáltatásra.

Ezt a lépést kihagyhatja, ha egy meglévő erőforráscsoportot használ.

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a>Erőforrás-szolgáltató regisztrálása

hello Azure DNS-szolgáltatás hello Microsoft.Network erőforrás-szolgáltató kezeli. Az Azure-előfizetéssel kell regisztrált toouse az erőforrás-szolgáltató Azure DNS használata előtt. Ez a műveletet minden egyes előfizetés esetén csak egyszer kell elvégezni.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```