## <a name="how-toocreate-a-virtual-network-using-a-network-config-file-from-powershell"></a><span data-ttu-id="3c35e-101">Hogyan toocreate egy virtuális hálózatot használ a hálózati konfigurációs fájl PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c35e-101">How toocreate a virtual network using a network config file from PowerShell</span></span>
<span data-ttu-id="3c35e-102">Azure használ egy xml-fájl toodefine minden virtuális hálózatok elérhető tooa előfizetés.</span><span class="sxs-lookup"><span data-stu-id="3c35e-102">Azure uses an xml file toodefine all virtual networks available tooa subscription.</span></span> <span data-ttu-id="3c35e-103">Letölteni a fájlt, toomodify szerkesztheti vagy törölje a meglévő virtuális hálózatot, majd hozzon létre új virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="3c35e-103">You can download this file, edit it toomodify or delete existing virtual networks, and create new virtual networks.</span></span> <span data-ttu-id="3c35e-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toodownload ezt a fájlt, említett tooas hálózati konfigurációját (vagy netcfg) fájl, és új virtuális hálózat toocreate szerkesztésére.</span><span class="sxs-lookup"><span data-stu-id="3c35e-104">In this tutorial, you learn how toodownload this file, referred tooas network configuration (or netcfg) file, and edit it toocreate a new virtual network.</span></span> <span data-ttu-id="3c35e-105">toolearn hello hálózati konfigurációs fájl bővebben lásd: hello [Azure-beli virtuális hálózat konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c35e-105">toolearn more about hello network configuration file, see hello [Azure virtual network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>

<span data-ttu-id="3c35e-106">a virtuális hálózati PowerShell, a következő lépéseket teljes hello segítségével netcfg fájlt tartalmazó toocreate:</span><span class="sxs-lookup"><span data-stu-id="3c35e-106">toocreate a virtual network with a netcfg file using PowerShell, complete hello following steps:</span></span>

1. <span data-ttu-id="3c35e-107">Ha még sosem használta az Azure PowerShell, teljes hello hello szükséges lépések [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azureps-cmdlets-docs) cikk, majd jelentkezzen be tooAzure, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="3c35e-107">If you have never used Azure PowerShell, complete hello steps in hello [How tooInstall and Configure Azure PowerShell](/powershell/azureps-cmdlets-docs) article, then sign in tooAzure and select your subscription.</span></span>
2. <span data-ttu-id="3c35e-108">Hello Azure PowerShell-konzolon, használja a hello **Get-AzureVnetConfig** parancsmag toodownload hello hálózati konfigurációs fájl tooa könyvtár hello a következő parancs futtatásával a számítógépen:</span><span class="sxs-lookup"><span data-stu-id="3c35e-108">From hello Azure PowerShell console, use hello **Get-AzureVnetConfig** cmdlet toodownload hello network configuration file tooa directory on your computer by running hello following command:</span></span> 
   
   ```powershell
   Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
   ```
   
   <span data-ttu-id="3c35e-109">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="3c35e-109">Expected output:</span></span>
  
      ```
      XMLConfiguration                                                                                                     
      ----------------                                                                                                     
      <?xml version="1.0" encoding="utf-8"?>...
      ```

3. <span data-ttu-id="3c35e-110">Nyissa meg a bármely XML vagy szöveges szerkesztő alkalmazással 2. lépésben mentett hello fájlt, és keressen a hello  **<VirtualNetworkSites>**  elemet.</span><span class="sxs-lookup"><span data-stu-id="3c35e-110">Open hello file you saved in step 2 using any XML or text editor application, and look for hello **<VirtualNetworkSites>** element.</span></span> <span data-ttu-id="3c35e-111">Ha olyan hálózatra, már létrehozott, minden egyes hálózati jelenik meg a saját  **<VirtualNetworkSite>**  elemet.</span><span class="sxs-lookup"><span data-stu-id="3c35e-111">If you have any networks already created, each network is displayed as its own **<VirtualNetworkSite>** element.</span></span>
4. <span data-ttu-id="3c35e-112">toocreate hello virtuális hálózati ebben a forgatókönyvben leírtak hozzáadása XML alábbi csak, hello hello  **<VirtualNetworkSites>**  elem:</span><span class="sxs-lookup"><span data-stu-id="3c35e-112">toocreate hello virtual network described in this scenario, add hello following XML just under hello **<VirtualNetworkSites>** element:</span></span>

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
   
5. <span data-ttu-id="3c35e-113">Mentse a hello hálózati konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="3c35e-113">Save hello network configuration file.</span></span>
6. <span data-ttu-id="3c35e-114">Hello Azure PowerShell-konzolon, használja a hello **Set-AzureVnetConfig** parancsmag tooupload hello hálózati konfigurációs fájl hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="3c35e-114">From hello Azure PowerShell console, use hello **Set-AzureVnetConfig** cmdlet tooupload hello network configuration file by running hello following command:</span></span> 
   
   ```powershell
   Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
   ```
   
   <span data-ttu-id="3c35e-115">Kimeneti adott vissza:</span><span class="sxs-lookup"><span data-stu-id="3c35e-115">Returned output:</span></span>
   
      ```
      OperationDescription OperationId                          OperationStatus
      -------------------- -----------                          ---------------
      Set-AzureVNetConfig  <Id>                                 Succeeded 
      ```
   
   <span data-ttu-id="3c35e-116">Ha **OperationStatus** nem *sikeres* hello adott vissza a kimeneti, a hibák és a Kész lépés 6 hello XML-fájl ismételt ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="3c35e-116">If **OperationStatus** is not *Succeeded* in hello returned output, check hello xml file for errors and complete step 6 again.</span></span>

7. <span data-ttu-id="3c35e-117">Hello Azure PowerShell-konzolon, használja a hello **Get-AzureVnetSite** parancsmag tooverify, amely az új hálózati hello hozzá lett adva hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="3c35e-117">From hello Azure PowerShell console, use hello **Get-AzureVnetSite** cmdlet tooverify that hello new network was added by running hello following command:</span></span> 

   ```powershell
   Get-AzureVNetSite -VNetName TestVNet
   ```
   
   <span data-ttu-id="3c35e-118">a visszaadott hello (rövidített) kimenete a következő szöveg hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="3c35e-118">hello returned (abbreviated) output includes hello following text:</span></span>
  
      ```
      AddressSpacePrefixes : {192.168.0.0/16}
      Location             : Central US
      Name                 : TestVNet
      State                : Created
      Subnets              : {FrontEnd, BackEnd}
      OperationDescription : Get-AzureVNetSite
      OperationStatus      : Succeeded
      ```
