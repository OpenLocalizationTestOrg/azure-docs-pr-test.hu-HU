## <a name="setting-up-powershell-for-resource-manager-templates"></a>A Resource Manager sablonokhoz PowerShell beállítása
A Resource Manager használhatja az Azure PowerShell, szüksége lesz a jobb oldali-es számú toohave hello Windows PowerShell és az Azure PowerShell rendszerhez készült verziója.

### <a name="verify-powershell-versions"></a>Ellenőrizze a PowerShell-verzió
Ellenőrizze, hogy rendelkezik-e a Windows PowerShell 3.0 vagy 4.0-s verziója. toofind hello verziója a Windows PowerShell, a Windows PowerShell parancssorába írja be a parancsot.

    $PSVersionTable

A következő típusú információkat hello fog kapni:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Ellenőrizze, hogy hello értékét **PSVersion** 3.0 vagy 4.0-s verzióját. Ha nem, lásd: [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) vagy [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Az Azure-fiók és -előfizetés beállítása
Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) vagy regisztráljon egy [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).

Nyisson meg egy Azure PowerShell-parancssort, és jelentkezzen be a parancs tooAzure.

    Login-AzureRmAccount

Ha több Azure-előfizetéssel rendelkezik, az Azure-előfizetések ezzel a paranccsal jeleníthet meg.

    Get-AzureRmSubscription

A következő típusú információkat hello fog kapni:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Ezek a parancsok futtatásával hello Azure PowerShell parancssorban beállíthat hello jelenlegi Azure-előfizetés. Cserélje le mindent, ami hello idézőjelek között, beleértve a hello < és > karakter, helyes hello nevét.

    $subscr="<SubscriptionName from hello display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

További információ az Azure-előfizetések és a fiókok: [hogyan: csatlakozás tooyour előfizetés](/powershell/azureps-cmdlets-docs#step-3-connect).

