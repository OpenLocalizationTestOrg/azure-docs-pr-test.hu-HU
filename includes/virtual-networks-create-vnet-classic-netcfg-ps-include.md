## <a name="how-toocreate-a-virtual-network-using-a-network-config-file-from-powershell"></a>Hogyan toocreate egy virtuális hálózatot használ a hálózati konfigurációs fájl PowerShell
Azure használ egy xml-fájl toodefine minden virtuális hálózatok elérhető tooa előfizetés. Letölteni a fájlt, toomodify szerkesztheti vagy törölje a meglévő virtuális hálózatot, majd hozzon létre új virtuális hálózatok. Ebben az oktatóanyagban elsajátíthatja, hogyan toodownload ezt a fájlt, említett tooas hálózati konfigurációját (vagy netcfg) fájl, és új virtuális hálózat toocreate szerkesztésére. toolearn hello hálózati konfigurációs fájl bővebben lásd: hello [Azure-beli virtuális hálózat konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).

a virtuális hálózati PowerShell, a következő lépéseket teljes hello segítségével netcfg fájlt tartalmazó toocreate:

1. Ha még sosem használta az Azure PowerShell, teljes hello hello szükséges lépések [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azureps-cmdlets-docs) cikk, majd jelentkezzen be tooAzure, és jelölje ki az előfizetését.
2. Hello Azure PowerShell-konzolon, használja a hello **Get-AzureVnetConfig** parancsmag toodownload hello hálózati konfigurációs fájl tooa könyvtár hello a következő parancs futtatásával a számítógépen: 
   
   ```powershell
   Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
   ```
   
   Várt kimenet:
  
      ```
      XMLConfiguration                                                                                                     
      ----------------                                                                                                     
      <?xml version="1.0" encoding="utf-8"?>...
      ```

3. Nyissa meg a bármely XML vagy szöveges szerkesztő alkalmazással 2. lépésben mentett hello fájlt, és keressen a hello  **<VirtualNetworkSites>**  elemet. Ha olyan hálózatra, már létrehozott, minden egyes hálózati jelenik meg a saját  **<VirtualNetworkSite>**  elemet.
4. toocreate hello virtuális hálózati ebben a forgatókönyvben leírtak hozzáadása XML alábbi csak, hello hello  **<VirtualNetworkSites>**  elem:

   ```xml
        <VirtualNetworkSite name="TestVNet" Location="East US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>
   ```
   
5. Mentse a hello hálózati konfigurációs fájlt.
6. Hello Azure PowerShell-konzolon, használja a hello **Set-AzureVnetConfig** parancsmag tooupload hello hálózati konfigurációs fájl hello a következő parancs futtatásával: 
   
   ```powershell
   Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
   ```
   
   Kimeneti adott vissza:
   
      ```
      OperationDescription OperationId                          OperationStatus
      -------------------- -----------                          ---------------
      Set-AzureVNetConfig  <Id>                                 Succeeded 
      ```
   
   Ha **OperationStatus** nem *sikeres* hello adott vissza a kimeneti, a hibák és a Kész lépés 6 hello XML-fájl ismételt ellenőrzéséhez.

7. Hello Azure PowerShell-konzolon, használja a hello **Get-AzureVnetSite** parancsmag tooverify, amely az új hálózati hello hozzá lett adva hello a következő parancs futtatásával: 

   ```powershell
   Get-AzureVNetSite -VNetName TestVNet
   ```
   
   a visszaadott hello (rövidített) kimenete a következő szöveg hello tartalmazza:
  
      ```
      AddressSpacePrefixes : {192.168.0.0/16}
      Location             : Central US
      Name                 : TestVNet
      State                : Created
      Subnets              : {FrontEnd, BackEnd}
      OperationDescription : Get-AzureVNetSite
      OperationStatus      : Succeeded
      ```
