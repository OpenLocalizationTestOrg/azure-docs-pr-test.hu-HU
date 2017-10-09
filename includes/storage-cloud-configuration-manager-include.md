Hello [Microsoft Azure Configuration Manager könyvtár a .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) egy osztályt biztosít a konfigurációs fájlból származó kapcsolati karakterlánc elemzéséhez. Hello [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) osztály elemez, függetlenül attól, hogy hello ügyfél alkalmazás hello asztalon, egy mobileszközön, egy Azure virtuális gépen, vagy egy Azure-felhőszolgáltatásban fut-e a konfigurációs beállításokat.

tooreference hello CloudConfigurationManager csomagra, adja hozzá a következő hello `using` irányelv:

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
```

Íme egy példa bemutatja, hogyan tooretrieve egy kapcsolati karakterlánc egy konfigurációs fájlból:

```csharp
// Parse hello connection string and return a reference toohello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

Hello Azure Configuration Manager használata nem kötelező megadni. Használhatja például a .NET-keretrendszer hello API [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) osztály.

