
## <a name="start-your-powershell-session"></a>Indítsa el a PowerShell-munkamenetet
Először meg kell toohave hello legújabb [Azure PowerShell](http://msdn.microsoft.com/library/mt619274.aspx) telepíteni és futtatni. Részletes információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Ez a témakör használatban példák hello [Azure Resource Manager üzembe helyezési modellben](../articles/azure-resource-manager/resource-group-overview.md), így a példák hello [Azure Resource Manager parancsmagjainak](http://msdn.microsoft.com/library/azure/mt125356.aspx). 
> 
> 

Futtassa a hello [ **Add-AzureRmAccount** ](http://msdn.microsoft.com/library/mt619267.aspx) parancsmagot – ekkor megjelenik egy bejelentkezési képernyő tooenter a hitelesítő adatok. Használja az azonos hello toosign használhatja az Azure-portálon toohello a hitelesítő adatokat.

    Add-AzureRmAccount

Ha több előfizetéssel használja-e a hello [ **Set-AzureRmContext** ](http://msdn.microsoft.com/library/mt619263.aspx) parancsmag tooselect melyik előfizetéssel használja a PowerShell-munkamenethez. milyen előfizetésre hello munkamenet használja, aktuális PowerShell toosee futtatása [ **Get-AzureRmContext**](http://msdn.microsoft.com/library/mt619265.aspx). toosee az előfizetések futtatása [ **Get-AzureRmSubscription**](http://msdn.microsoft.com/library/mt619284.aspx).

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

