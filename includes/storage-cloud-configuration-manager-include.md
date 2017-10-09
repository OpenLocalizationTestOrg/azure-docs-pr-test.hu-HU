<span data-ttu-id="0abf7-101">Hello [Microsoft Azure Configuration Manager könyvtár a .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) egy osztályt biztosít a konfigurációs fájlból származó kapcsolati karakterlánc elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="0abf7-101">hello [Microsoft Azure Configuration Manager Library for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) provides a class for parsing a connection string from a configuration file.</span></span> <span data-ttu-id="0abf7-102">Hello [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) osztály elemez, függetlenül attól, hogy hello ügyfél alkalmazás hello asztalon, egy mobileszközön, egy Azure virtuális gépen, vagy egy Azure-felhőszolgáltatásban fut-e a konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="0abf7-102">hello [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) class parses configuration settings regardless of whether hello client application is running on hello desktop, on a mobile device, in an Azure virtual machine, or in an Azure cloud service.</span></span>

<span data-ttu-id="0abf7-103">tooreference hello CloudConfigurationManager csomagra, adja hozzá a következő hello `using` irányelv:</span><span class="sxs-lookup"><span data-stu-id="0abf7-103">tooreference hello CloudConfigurationManager package, add hello following `using` directive:</span></span>

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
```

<span data-ttu-id="0abf7-104">Íme egy példa bemutatja, hogyan tooretrieve egy kapcsolati karakterlánc egy konfigurációs fájlból:</span><span class="sxs-lookup"><span data-stu-id="0abf7-104">Here's an example that shows how tooretrieve a connection string from a configuration file:</span></span>

```csharp
// Parse hello connection string and return a reference toohello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

<span data-ttu-id="0abf7-105">Hello Azure Configuration Manager használata nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="0abf7-105">Using hello Azure Configuration Manager is optional.</span></span> <span data-ttu-id="0abf7-106">Használhatja például a .NET-keretrendszer hello API [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="0abf7-106">You can also use an API like hello .NET Framework's [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) class.</span></span>

