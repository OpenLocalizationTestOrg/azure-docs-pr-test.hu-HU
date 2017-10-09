


## <a name="using-vm-extensions"></a>Virtuálisgép-bővítmények használatával
Az Azure Virtuálisgép-bővítmények megvalósítása viselkedések vagy szolgáltatásokat vagy más programok Azure virtuális gépeken használata (például hello **WebDeployForVSDevTest** bővítmény lehetővé teszi, hogy a Visual Studio tooWeb telepítés megoldásokat az Azure virtuális gépen), vagy adjon meg hello lehetőségét, hogy a virtuális gép toosupport hello toointeract néhány más viselkedés (például használhatja hello VM eléréséhez kapcsolódó kiegészítő mezőket PowerShell hello Azure CLI és REST ügyfelek tooreset, de távelérési értékek módosítása az Azure virtuális gépen).

> [!IMPORTANT]
> Bővítmények hello funkcióihoz támogatják-e teljes listáját lásd: [Azure Virtuálisgép-bővítmények és a szolgáltatások](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Mivel minden egyes Virtuálisgép-bővítmény támogatja az egy adott funkcióhoz, pontosan mit, illetve használhat nem kiterjesztési függ hello bővítmény. -Módosítása előtt a virtuális gép mindenképpen elolvasta hello toouse kívánt Virtuálisgép-bővítmény hello dokumentációját. Egyes Virtuálisgép-bővítmények nem támogatott; mások rendelkezik tulajdonságokkal beállítható, hogy a virtuális gép megváltozzon jelentős.
> 
> 

hello leggyakoribb feladatokat is:

1. Rendelkezésre álló bővítmények keresése
2. A betöltött frissítései
3. Bővítmények hozzáadása
4. Bővítmények eltávolítása

## <a name="find-available-extensions"></a>Rendelkezésre álló bővítmények keresése
Megkeresheti a bővítményt, és részletes információk segítségével:

* PowerShell
* Az Azure platformfüggetlen parancssori felület (Azure CLI)
* Szolgáltatásfelügyeleti REST API

### <a name="azure-powershell"></a>Azure PowerShell
Néhány bővítmény rendelkezik, amelyek adott toothem, előfordulhat, hogy könnyebben konfigurációjuk powershellből; PowerShell-parancsmagok de hello következő parancsmagok működnek az összes Virtuálisgép-bővítmények.

A következő parancsmagok tooobtain információ a rendelkezésre álló bővítmények hello használhatja:

* A példányok webes szerepkörök vagy feldolgozói szerepköröket, használhatja a hello [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) parancsmag.
* A példányok a virtuális gépek, használhatja a hello [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) parancsmag.
  
   Például a hogyan kód példa azt mutatja meg a következő hello toolist hello információi **IaaSDiagnostics** bővítmény PowerShell használatával.
  
      PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
  
      Publisher                   : Microsoft.Azure.Diagnostics
      ExtensionName               : IaaSDiagnostics
      Version                     : 1.2
      Label                       : Microsoft Monitoring Agent Diagnostics
      Description                 : Microsoft Monitoring Agent Extension
      PublicConfigurationSchema   :
      PrivateConfigurationSchema  :
      IsInternalExtension         : False
      SampleConfig                :
      ReplicationCompleted        : True
      Eula                        :
      PrivacyUri                  :
      HomepageUri                 :
      IsJsonExtension             : True
      DisallowMajorVersionUpgrade : False
      SupportedOS                 :
      PublishedDate               :
      CompanyName                 :

### <a name="azure-command-line-interface-azure-cli"></a>Az Azure parancssori felület (Azure CLI)
Néhány bővítmény rendelkezik, amelyek adott toothem Azure parancssori felület parancsait (Docker Virtuálisgép-bővítmény hello csak egy példa a), amely könnyebben lehet, hogy a konfigurációs; de hello következő parancsokat az összes Virtuálisgép-bővítmények számára.

Használhatja a hello **azure virtuális gép Bővítménylista** tooobtain információ a rendelkezésre álló bővítmények parancsot, és a hello **–-json** toodisplay lehetőséget egy vagy több bővítmények az összes rendelkezésre álló információkat. Ha nem használja a bővítmények nevének, a hello parancsot az összes rendelkezésre álló bővítmények JSON leírását adja eredményül.

Például a következő példakód hello mutatja, hogyan toolist hello hello információi **IaaSDiagnostics** bővítmény használatával hello Azure CLI **azure virtuális gép Bővítménylista** parancs és felhasználási hello **–-json** tooreturn teljes körű információkat lehetőséget.

    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



### <a name="service-management-rest-apis"></a>Szolgáltatáskezelési REST API-k
A következő REST API-k tooobtain információ a rendelkezésre álló bővítmények hello használhatja:

* A példányok webes szerepkörök vagy feldolgozói szerepköröket, használhatja a hello [lista rendelkezésre álló bővítmények](https://msdn.microsoft.com/library/dn169559.aspx) műveletet. rendelkezésre álló bővítmények toolist hello verziói esetében használhat [lista bővítmény verziók](https://msdn.microsoft.com/library/dn495437.aspx).
* A példányok a virtuális gépek, használhatja a hello [erőforrás-kiterjesztések listájából](https://msdn.microsoft.com/library/dn495441.aspx) műveletet. rendelkezésre álló bővítmények toolist hello verziói esetében használhat [lista erőforrás-bővítmény verziók](https://msdn.microsoft.com/library/dn495440.aspx).

## <a name="add-update-or-disable-extensions"></a>Adja hozzá, frissítés, vagy bővítmények letiltása
Bővítmények példány létrehozásakor, illetve azok felveheti-példányt futtató tooa adhatók hozzá. Bővítmények frissítése, letiltott vagy eltávolított. Azure PowerShell-parancsmagok használatával vagy hello szolgáltatásfelügyelet REST API műveletek segítségével ezeket a műveleteket végezheti el. Paraméterek szükséges tooinstall és néhány bővítmény beállításához. Nyilvános és titkos paraméterek a bővítmények esetében támogatottak.

### <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell-parancsmagok használata hello legegyszerűbb módja tooadd és frissítés-bővítmények. Hello bővítmény parancsmagok használatakor hello bővítmény hello-beállítások legnagyobb részét történik meg. Esetenként, esetleg tooprogrammatically veheti fel. Ha toodo ez szükséges, meg kell adnia hello konfigurációs hello bővítmény.

Hogy egy-bővítményhez olyan nyilvános és titkos paraméterek konfigurálása a következő parancsmagok tooknow hello használhatja:

* A példányok webes szerepkörök vagy feldolgozói szerepköröket, használhatja a hello **Get-AzureServiceAvailableExtension** parancsmag.
* A példányok a virtuális gépek, használhatja a hello **Get-AzureVMAvailableExtension** parancsmag.

### <a name="service-management-rest-apis"></a>Szolgáltatáskezelési REST API-k
Hello REST API-k segítségével visszaállíthatja az elérhető bővítmények listája, a kapcsolatos tudnivalókért, hogyan hello bővítmény konfigurált toobe kapja meg. visszaadott hello információk megjelenítése előfordulhat, hogy egy séma nyilvános és titkos séma paraméterinformáció. Visszaadja a nyilvános paraméterértékek hello példányaira vonatkozó lekérdezésekben. Titkos paraméter értéke nem lehet megjeleníteni.

E kiterjesztése a nyilvános és titkos paraméterek olyan konfigurációt igényel, a REST API-k tooknow következő hello használhatja:

* A webes szerepkörök vagy feldolgozói szerepkörök példányai hello **PublicConfigurationSchema** és **PrivateConfigurationSchema** elemek hello hello válaszára a hello információkkal [listája Rendelkezésre álló bővítmények](https://msdn.microsoft.com/library/dn169559.aspx) műveletet.
* A virtuális gépek példányait hello **PublicConfigurationSchema** és **PrivateConfigurationSchema** elemek hello hello válaszára a hello információkkal [listája Erőforrás-bővítményt](https://msdn.microsoft.com/library/dn495441.aspx) műveletet.

> [!NOTE]
> Bővítmények JSON meghatározott konfigurációk is használható. Az ilyen típusú kiterjesztések használata esetén csak hello **SampleConfig** elem szolgál.
> 
> 

