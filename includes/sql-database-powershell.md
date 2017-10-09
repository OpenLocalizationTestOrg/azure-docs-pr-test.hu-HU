
## <a name="start-your-powershell-session"></a>Indítsa el a PowerShell-munkamenetet
Először meg kell rendelkeznie hello legújabb Azure PowerShell telepítése és futtatása. Részletes információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> SQL-adatbázis számos új funkciója csak akkor támogatott, ha hello használata [Azure Resource Manager üzembe helyezési modellben](../articles/azure-resource-manager/resource-group-overview.md), így a példák hello [Azure SQL Database PowerShell parancsmagjait](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx) az erőforrás-kezelő . hello szolgáltatást (klasszikus) felügyeleti telepítési modell [Azure SQL Database szolgáltatás felügyeleti parancsmagok](https://msdn.microsoft.com/library/azure/dn546723\(v=azure.300\).aspx) használata támogatott a visszamenőleges kompatibilitás érdekében mégis hello Resource Manager parancsmagjainak használata javasolt.
> 
> 

Futtassa a hello [ **Add-AzureRmAccount** ](https://msdn.microsoft.com/library/azure/mt619267\(v=azure.300\).aspx) parancsmagot, és megjelenik egy bejelentkezési képernyő tooenter a hitelesítő adatait. Használja az azonos hello toosign használhatja az Azure-portálon toohello a hitelesítő adatokat.

```PowerShell
Add-AzureRmAccount
```

Ha több előfizetéssel rendelkezik, akkor hello [ **Set-AzureRmContext** ](https://msdn.microsoft.com/library/azure/mt619263\(v=azure.300\).aspx) parancsmag tooselect melyik előfizetéssel használja a PowerShell-munkamenethez. milyen előfizetésre hello munkamenet használja, aktuális PowerShell toosee futtatása [ **Get-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619265\(v=azure.300\).aspx). toosee az előfizetések futtatása [ **Get-AzureRmSubscription**](https://msdn.microsoft.com/library/azure/mt619284\(v=azure.300\).aspx).

```PowerShell
Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```
