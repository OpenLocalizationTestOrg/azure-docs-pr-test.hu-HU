## <a name="how-to-create-a-virtual-network-using-a-network-config-file-from-powershell"></a><span data-ttu-id="a1a65-101">PowerShell a hálózati konfigurációs fájl használatával egy virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1a65-101">How to create a virtual network using a network config file from PowerShell</span></span>
<span data-ttu-id="a1a65-102">Azure előfizetés az összes rendelkezésre álló virtuális hálózatok megadása egy XML-fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="a1a65-102">Azure uses an xml file to define all virtual networks available to a subscription.</span></span> <span data-ttu-id="a1a65-103">Letölteni a fájlt, módosítása vagy törlése a meglévő virtuális hálózatok szerkesztheti, majd hozzon létre új virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="a1a65-103">You can download this file, edit it to modify or delete existing virtual networks, and create new virtual networks.</span></span> <span data-ttu-id="a1a65-104">Ebből az oktatóanyagból megtudhatja, hogyan letölteni a fájlt, a hálózati konfiguráció (vagy netcfg) fájl, a továbbiakban, és új virtuális hálózat létrehozása szerkesztésére.</span><span class="sxs-lookup"><span data-stu-id="a1a65-104">In this tutorial, you learn how to download this file, referred to as network configuration (or netcfg) file, and edit it to create a new virtual network.</span></span> <span data-ttu-id="a1a65-105">A hálózati konfigurációs fájl kapcsolatos további tudnivalókért tekintse meg a [Azure-beli virtuális hálózat konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="a1a65-105">To learn more about the network configuration file, see the [Azure virtual network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>

<span data-ttu-id="a1a65-106">Virtuális hálózat létrehozása a PowerShell használatával netcfg fájllal, végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a1a65-106">To create a virtual network with a netcfg file using PowerShell, complete the following steps:</span></span>

1. <span data-ttu-id="a1a65-107">Ha még sosem használta az Azure PowerShell, hajtsa végre a a [telepítése és konfigurálása az Azure PowerShell](/powershell/azureps-cmdlets-docs) cikk, majd jelentkezzen be az Azure-ba, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="a1a65-107">If you have never used Azure PowerShell, complete the steps in the [How to Install and Configure Azure PowerShell](/powershell/azureps-cmdlets-docs) article, then sign in to Azure and select your subscription.</span></span>
2. <span data-ttu-id="a1a65-108">Az Azure PowerShell-konzolon, használja a **Get-AzureVnetConfig** parancsmag használatával töltheti le a hálózati konfigurációs fájlt a számítógépen egy könyvtár a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a1a65-108">From the Azure PowerShell console, use the **Get-AzureVnetConfig** cmdlet to download the network configuration file to a directory on your computer by running the following command:</span></span> 
   
   ```powershell
   Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
   ```
   
   <span data-ttu-id="a1a65-109">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="a1a65-109">Expected output:</span></span>
  
      ```
      XMLConfiguration                                                                                                     
      ----------------                                                                                                     
      <?xml version="1.0" encoding="utf-8"?>...
      ```

3. <span data-ttu-id="a1a65-110">Nyissa meg a bármely XML vagy szöveges szerkesztő alkalmazással 2. lépésben mentett fájlt, és keresse meg a  **<VirtualNetworkSites>**  elemet.</span><span class="sxs-lookup"><span data-stu-id="a1a65-110">Open the file you saved in step 2 using any XML or text editor application, and look for the **<VirtualNetworkSites>** element.</span></span> <span data-ttu-id="a1a65-111">Ha olyan hálózatra, már létrehozott, minden egyes hálózati jelenik meg a saját  **<VirtualNetworkSite>**  elemet.</span><span class="sxs-lookup"><span data-stu-id="a1a65-111">If you have any networks already created, each network is displayed as its own **<VirtualNetworkSite>** element.</span></span>
4. <span data-ttu-id="a1a65-112">Ebben a forgatókönyvben a virtuális hálózat létrehozásához adja hozzá a következő XML alatt csak a  **<VirtualNetworkSites>**  elem:</span><span class="sxs-lookup"><span data-stu-id="a1a65-112">To create the virtual network described in this scenario, add the following XML just under the **<VirtualNetworkSites>** element:</span></span>

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
   
5. <span data-ttu-id="a1a65-113">Mentse a hálózati konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="a1a65-113">Save the network configuration file.</span></span>
6. <span data-ttu-id="a1a65-114">Az Azure PowerShell-konzolon, használja a **Set-AzureVnetConfig** parancsmaggal töltse fel a hálózati konfigurációs fájlt a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a1a65-114">From the Azure PowerShell console, use the **Set-AzureVnetConfig** cmdlet to upload the network configuration file by running the following command:</span></span> 
   
   ```powershell
   Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
   ```
   
   <span data-ttu-id="a1a65-115">Kimeneti adott vissza:</span><span class="sxs-lookup"><span data-stu-id="a1a65-115">Returned output:</span></span>
   
      ```
      OperationDescription OperationId                          OperationStatus
      -------------------- -----------                          ---------------
      Set-AzureVNetConfig  <Id>                                 Succeeded 
      ```
   
   <span data-ttu-id="a1a65-116">Ha **OperationStatus** nem *sikeres* visszaadott kimenet, ellenőrizze az XML-fájl, a hibák és a Kész lépés 6 újra.</span><span class="sxs-lookup"><span data-stu-id="a1a65-116">If **OperationStatus** is not *Succeeded* in the returned output, check the xml file for errors and complete step 6 again.</span></span>

7. <span data-ttu-id="a1a65-117">Az Azure PowerShell-konzolon, használja a **Get-AzureVnetSite** parancsmag segítségével győződjön meg arról, hogy az új hálózati lett-e hozzáadva a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a1a65-117">From the Azure PowerShell console, use the **Get-AzureVnetSite** cmdlet to verify that the new network was added by running the following command:</span></span> 

   ```powershell
   Get-AzureVNetSite -VNetName TestVNet
   ```
   
   <span data-ttu-id="a1a65-118">A visszaadott (rövidített) parancs kimenete a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="a1a65-118">The returned (abbreviated) output includes the following text:</span></span>
  
      ```
      AddressSpacePrefixes : {192.168.0.0/16}
      Location             : Central US
      Name                 : TestVNet
      State                : Created
      Subnets              : {FrontEnd, BackEnd}
      OperationDescription : Get-AzureVNetSite
      OperationStatus      : Succeeded
      ```
