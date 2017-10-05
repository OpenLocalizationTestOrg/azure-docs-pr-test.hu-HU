---
title: "Az Azure Storage Azure PowerShell használatával |} Microsoft Docs"
description: "Az Azure Storage Azure PowerShell-parancsmagok használata létrehozásához és kezeléséhez a storage-fiókok; blobok, táblák, üzenetsorok és fájlok; használata konfigurálja és tárolási analitika lekérdezése, és a közös hozzáférésű jogosultságkód létrehozása."
services: storage
documentationcenter: na
author: robinsh
manager: timlt
ms.assetid: f4704f58-abc6-4f89-8b6d-1b1659746f5a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: 87a116111d085fe2913bf6f5f8751c3ff5f3c076
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="using-azure-powershell-with-azure-storage"></a>Using Azure PowerShell with Azure Storage (Az Azure PowerShell és az Azure Storage együttes használata)
## <a name="overview"></a>Áttekintés
Az Azure PowerShell modul, amely kezelése a Windows PowerShell segítségével Azure-parancsmagokat kínál. Ez egy feladatalapú parancshéj és parancsnyelv, amely kifejezetten rendszerfelügyeleti célra készült. A PowerShell-lel könnyen szabályozhatja és az Azure-szolgáltatások és alkalmazások automatizálják. Például használhatja a parancsmagokat a azonos feladatok végrehajtását, amelyek segítségével végezheti el a [Azure-portálon](https://portal.azure.com).

Az útmutató azt fogja felfedezés használata a [Azure tárolási parancsmagok](/powershell/module/azurerm.storage/#storage) különböző az Azure Storage fejlesztői és felügyeleti feladatok elvégzéséhez.

Ez az útmutató feltételezi, hogy rendelkezik tapasztalattal használatával [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) és [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx). Az útmutató számos bemutatásához az használatát, az Azure Storage PowerShell parancsfájlt. Minden parancsprogram futtatása előtt a konfiguráció alapján a parancsfájl-változókat frissítenie kell.

A jelen útmutató az első szakaszban Azure Storage és a PowerShell kiadványok biztosít. Részletes információk és utasítások, indítását a [az Azure Storage Azure PowerShell használatára vonatkozó Előfeltételek](#prerequisites-for-using-azure-powershell-with-azure-storage).

## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Ismerkedés az Azure Storage és a PowerShell 5 perc
Ez a szakasz bemutatja, hogyan számára az Azure Storage PowerShell 5 perc.

**Új Azure-bA:** a Microsoft Azure-előfizetés és az adott előfizetéshez tartozó Microsoft-fiókkal. Az Azure megvásárlási lehetőségeinek információkért lásd: [ingyenes](https://azure.microsoft.com/pricing/free-trial/), [beszerzési lehetőségek](https://azure.microsoft.com/pricing/purchase-options/), és [ajánlatok](https://azure.microsoft.com/pricing/member-offers/) (az MSDN, a Microsoft Partner Network, és a BizSpark és egyéb Microsoft programok tagjai).

Lásd: [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Azure-előfizetések további információt.

**Miután létrehozta a Microsoft Azure-előfizetésre és -fiókra:**

1. Töltse le és telepítse a legújabb [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).
2. Kezdő Windows PowerShell integrált parancsfájlkezelés környezet (ISE): A helyi számítógépen, válassza a **Start** menü. Típus **felügyeleti eszközök** kattintson futtatni. Az a **felügyeleti eszközök** ablak, kattintson a jobb gombbal **Windows PowerShell ISE**, kattintson a **Futtatás rendszergazdaként**.
3. A **Windows PowerShell ISE**, kattintson a **fájl** > **új** létrehozni egy új parancsfájlt.
4. Most lesz ad egy egyszerű parancsprogram, amely alapszintű PowerShell-parancsok Azure Storage eléréséhez. A parancsfájl először kérje meg az Azure fiók hitelesítő adatait az Azure hozzáadandó fiókot a helyi PowerShell környezetben. Ezt követően a parancsfájl az alapértelmezett Azure-előfizetés, és új tárfiók létrehozása az Azure-ban. Ezután a parancsfájl hozzon létre egy új tárolót az új tárfiókot, és töltse fel a meglévő képfájl (blob) a tárolóban. Miután a parancsfájl megjeleníti az adott tároló összes BLOB, a helyi számítógépen egy új célkönyvtáron létrehozza, és töltse le a lemezképet.
5. A következő kódot a szakaszban válassza ki a parancsfájl a megjegyzések között **#begin** és **#end**. Nyomja le a CTRL + C billentyűkombinációval másolja a vágólapra.

    ```powershell
    # begin
    # Update with the name of your subscription.
    $SubscriptionName = "YourSubscriptionName"
       
    # Give a name to your new storage account. It must be lowercase!
    $StorageAccountName = "yourstorageaccountname"
       
    # Choose "West US" as an example.
    $Location = "West US"
       
    # Give a name to your new container.
    $ContainerName = "imagecontainer"
       
    # Have an image file and a source directory in your local computer.
    $ImageToUpload = "C:\Images\HelloWorld.png"
       
    # A destination directory in your local computer.
    $DestinationFolder = "C:\DownloadImages"
       
    # Add your Azure account to the local PowerShell environment.
    Add-AzureAccount
       
    # Set a default Azure subscription.
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
       
    # Create a new storage account.
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location
       
    # Set a default storage account.
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
       
    # Create a new container.
    New-AzureStorageContainer -Name $ContainerName -Permission Off
       
    # Upload a blob into a container.
    Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload
       
    # List all blobs in a container.
    Get-AzureStorageBlob -Container $ContainerName
       
    # Download blobs from the container:
    # Get a reference to a list of all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName
       
    # Create the destination directory.
    New-Item -Path $DestinationFolder -ItemType Directory -Force  
       
    # Download blobs into the local destination directory.
    $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
       
    # end
    ```

6. A **Windows PowerShell ISE**, nyomja le a CTRL + V billentyűkombinációval másolja a parancsfájlt. Kattintson a **fájl** > **mentése**. Az a **Mentés másként** párbeszédpanelen írja be a nevét, a parancsfájl, például a "mystoragescript." Kattintson a **Save** (Mentés) gombra.
7. Most módosítania a parancsfájl-változókat a konfigurációs beállítások alapján. Frissítenie kell a **$SubscriptionName** változó a saját előfizetésével. Tartsa a többi változó a parancsfájlban megadott, vagy igény szerint frissítse azokat.
   
   * **$SubscriptionName:** ezt a változót a saját előfizetés nevével kell frissíteni. Kövesse a következő háromféleképpen keresse meg az előfizetés nevét:
     
    a. A **Windows PowerShell ISE**, kattintson a **fájl** > **új** létrehozni egy új parancsfájlt. Másolja a következő parancsfájlt az új parancsfájlt, és kattintson a **Debug** > **futtatása**. A következő parancsfájl először kérje meg az Azure fiók hitelesítő adatait az Azure hozzáadása a helyi PowerShell környezetre fiókot, és majd az összes olyan előfizetést, a helyi PowerShell-munkamenethez kapcsolódó megjelenítése. Jegyezze fel az oktatóanyag során használni kívánt előfizetést nevét:
     
    ```powershell
    Add-AzureAccount 
      Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName
    ```

    b. Keresse meg és másolja a előfizetés nevét a [Azure-portálon](https://portal.azure.com), kattintson a bal oldali menüben **előfizetések**. Másolja át az előfizetést, amelyet a jelen útmutató a parancsfájlok futtatása során használni kívánt nevét.
     
     ![Azure Portal](./media/storage-powershell-guide-full/Subscription_Previewportal.png)

    c. Keresse meg és másolja a előfizetés nevét a [klasszikus Azure portál](https://manage.windowsazure.com/), görgessen lefelé, és kattintson a **beállítások** a portál bal oldalán. Kattintson a **előfizetések** az előfizetések listájának megjelenítéséhez. Másolja át az előfizetést, amelyet a jelen útmutató a megadott parancsfájlok futtatása során használni kívánt nevét.
     
     ![klasszikus Azure portál](./media/storage-powershell-guide-full/Subscription_currentportal.png)

   * **$StorageAccountName:** használja a parancsfájl a megadott névvel, vagy adjon meg egy új nevet a tárfiók. **Fontos:** a tárfiók nevét az Azure-ban egyedinek kell lennie. Az kisbetűnek kell lennie, túl!
   * **$Location:** a parancsfájl a megadott "USA nyugati régiója" használja, vagy válasszon más Azure helyeken, például az USA keleti régiója, Észak-Európa, és így tovább.
   * **$ContainerName:** használja a parancsfájl a megadott névvel, vagy adjon meg egy új nevet a tároló.
   * **$ImageToUpload:** adja meg egy elérési utat kép a helyi számítógépen, például: "C:\Images\HelloWorld.png".
   * **$DestinationFolder:** meg elérési útját a helyi könyvtárat, többek között az Azure Storage-ból letöltött fájlok tárolására szolgáló: "C:\DownloadImages".
8. Miután frissítette a parancsfájl-változókat a "mystoragescript.ps1" fájlban, kattintson a **fájl** > **mentése**. Kattintson a **Debug** > **futtatása** vagy nyomja le az ENTER **F5** a parancsfájl futtatásához.  

A parancsfájl futtatása után rendelkeznie kell egy helyi célmappát, amely tartalmazza a letöltött lemezképfájlt. Az alábbi képernyőfelvételen látható egy példa a kimenetre:

![Blobok letöltése](./media/storage-powershell-guide-full/Blobdownload.png)

> [!NOTE]
> A "Bevezetés az Azure Storage és a PowerShell 5 percben" szakaszban megadott gyors Bevezetés az Azure Storage Azure PowerShell használatával. Részletes információk és utasítások javasoljuk, hogy olvassa el a következő szakaszokat.
> 

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Az Azure Storage Azure PowerShell használatának előfeltételei
Egy Azure-előfizetés és a fiók ebben az útmutatóban megadott PowerShell-parancsmagok futtatásához a fent leírt módon van szükség.

Az Azure PowerShell modul, amely kezelése a Windows PowerShell segítségével Azure-parancsmagokat kínál. A telepítése és beállítása az Azure PowerShell információkért lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview). Azt javasoljuk, hogy letöltése és telepítése vagy frissítése a legújabb Azure PowerShell modulra az útmutató használata előtt.

A parancsmagokat futtathatja a szabványos Windows PowerShell-konzol vagy a Windows PowerShell integrált parancsfájlkezelési környezet (ISE). Ahhoz például, hogy nyissa meg a **Windows PowerShell ISE**, válassza a Start menü, írja be a felügyeleti eszközök, és kattintson a futtatni. A felügyeleti eszközök ablakban kattintson a jobb gombbal a Windows PowerShell ISE, kattintson a Futtatás rendszergazdaként.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Az Azure storage-fiókok kezelése

Vessen egy pillantást a PowerShell-lel Azure storage-fiók kezelése

### <a name="how-to-set-a-default-azure-subscription"></a>Az alapértelmezett Azure-előfizetés beállítása
Kezeli az Azure Storage Azure PowerShell használatával, az ügyfél-környezet Azure az Azure Active Directory-hitelesítés vagy Tanúsítványalapú hitelesítés használatával hitelesíteni kell. Részletes információkért lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview) oktatóanyag. Ez az útmutató az Azure Active Directory-hitelesítést használ.

1. A Windows PowerShell ISE, írja be a következő parancs futtatásával adja hozzá az Azure fiók a helyi PowerShell környezetre:

    ```powershell
    Add-AzureAccount
    ```

2. A "Bejelentkezés a Microsoft Azure-beli" ablakban írja be az e-mail címet és a fiókhoz társított jelszót. Az Azure hitelesíti és menti a hitelesítő adatokat, majd bezárja az ablakot.

3. Ezután futtassa a következő parancsot az Azure-fiókra megtekintheti a helyi PowerShell-környezetében, és győződjön meg arról, hogy a fiók szerepel:
   
    ```powershell
    Get-AzureAccount
    ```
4. Ezután futtassa a következő parancsmagot a helyi PowerShell-munkamenethez kapcsolódó összes előfizetés megtekintéséhez, és ellenőrizze, hogy szerepel-e az előfizetés:

    ```powershell
    Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`
    ```
5. Az alapértelmezett Azure-előfizetés beállításához futtassa a Select-AzureSubscription parancsmagot:

    ```powershell
    $SubscriptionName = 'Your subscription Name'
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
    ```

6. Ellenőrizze a nevét, az alapértelmezett előfizetés a Get-AzureSubscription parancsmag futtatásával:

    ```powershell
    Get-AzureSubscription -Default
    ```

7. Az Azure Storage összes rendelkezésre álló PowerShell-parancsmagok megtekintéséhez futtassa:
    
    ```powershell
    Get-Command -Module Azure -Noun *Storage*`
    ```

### <a name="how-to-create-a-new-azure-storage-account"></a>Egy új Azure storage-fiók létrehozása
Az Azure storage használatához szüksége lesz egy tárfiókot. Egy új Azure-tárfiókot is létrehozhat, a számítógép csatlakozni az előfizetés konfigurálása után.

1. Futtassa a Get-AzureLocation parancsmagot a rendelkezésre álló adatközpont-helyeinek kereséséhez:

    ```powershell
    Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes
    ```

2. Ezután futtassa a New-AzureStorageAccount parancsmaggal hozzon létre egy új tárfiókot. Az alábbi példában az "USA nyugati régiója" adatközpontban hoz létre egy új tárfiókot.
   
    ```powershell
    $location = "West US"
    $StorageAccountName = "yourstorageaccount"
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location
    ```

> [!IMPORTANT]
> A tárfiók neve Azure belül egyedieknek kell lenniük, és kisbetűnek kell lennie. Az elnevezési konvenciókat és korlátozások, lásd: [kapcsolatos Azure Storage-fiókok](../storage-create-storage-account.md) és [elnevezési és hivatkozó tárolók, Blobok és metaadatok](http://msdn.microsoft.com/library/azure/dd135715.aspx).
> 
> 

### <a name="how-to-set-a-default-azure-storage-account"></a>Egy alapértelmezett Azure storage-fiók beállítása
Az előfizetés több tárfiókot is lehet. Válasszon egyet közülük, és állítsa be az alapértelmezett tárfiókot, a tárolási parancs összes ugyanabban a PowerShell-munkamenetben. Ez lehetővé teszi, hogy az Azure PowerShell tárolási parancsok az adattároló-környezet explicit megadása nélkül futtatja.

1. Állítsa be az előfizetés egy alapértelmezett tárfiókot, a Set-AzureSubscription parancsmag is futtathatja.

    ```powershell
    $SubscriptionName = "Your subscription name"
    $StorageAccountName = "yourstorageaccount"  
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

2. Ezután futtassa a Get-AzureSubscription parancsmagot, ellenőrizze, hogy a storage-fiók alapértelmezett előfizetés fiókhoz tartozó. Ez a parancs az előfizetés tulajdonságai a jelenlegi előfizetés, beleértve az aktuális tárfiók adja vissza.

    ```powershell
    Get-AzureSubscription –Current
    ```

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Hogyan listázhat előfizetés minden Azure storage-fiók
Minden Azure-előfizetés lehet akár 100 storage-fiókok. Korlátozások a legfrissebb információkért lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md).

Futtassa a nevét és a jelenlegi előfizetés tárfiókok állapotának megállapítása a következő parancsmagot:

```powershell
Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus
```

### <a name="how-to-create-an-azure-storage-context"></a>Egy Azure storage-környezet létrehozása
Az Azure storage-környezet egy objektum a PowerShell foglalják magukban a tároló hitelesítő adatait. Tárolási környezetet használ, bármely ezt követő parancsmag lehetővé teszi, hogy a kérés hitelesítéséhez a tárfiók és a hozzáférési kulcs explicit megadása nélkül. Adattároló-környezet az sokféleképpen, például a tárolási fiók nevét és hívóbetűjét, a közös hozzáférésű jogosultságkód (SAS) jogkivonatot, a kapcsolati karakterlánc használatával hozhat létre vagy névtelen. További információkért lásd: [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).  

Adattároló-környezet létrehozása a következő három módon egyikét használja:

* Futtassa a [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) parancsmag tudja meg a elsődleges hozzáférési kulcsot az Azure-tárfiókot. Ezután hívja a [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) parancsmaggal hozzon létre egy tárolási környezetben:

    ```powershell
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    ```

* Az Azure storage-tároló megosztott hozzáférési aláírást jogkivonat előállítása és adattároló-környezet létrehozására használható:

    ```powershell
    $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken
    ```

    További információkért lásd: [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) és [használatával megosztott hozzáférési aláírásokkal (SAS)](../storage-dotnet-shared-access-signature-part-1.md).

* Bizonyos esetekben érdemes lehet a szolgáltatás végpontokat határoz meg az új adattároló-környezet létrehozásakor. Erre akkor lehet szükség, ha egy egyéni tartománynevet a tárfiók a Blob szolgáltatásban regisztrált vagy a közös hozzáférésű jogosultságkód használandó tárolási erőforrások eléréséhez. Állítsa be a végpontok a kapcsolati karakterláncot, és segítségével hozzon létre egy új adattároló-környezet alább látható módon:

    ```powershell
    $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
    $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString
    ```

A tárolási kapcsolati karakterlánc konfigurálásával kapcsolatos további információkért lásd: [kapcsolati karakterláncok konfigurálása](../storage-configure-connection-string.md).

Most, hogy a számítógépet, és megtanulta, előfizetések és az Azure PowerShell storage-fiókok kezelése, ugorjon a következő szakaszban megtudhatja, hogyan kezelheti az Azure BLOB, és a blob-pillanatképek.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Hogyan kérhető le, és az Azure storage kulcsok újragenerálása
Egy Azure Storage-fiók két kulcsait tartalmaz. A következő parancsmag segítségével a kulcsok beolvasása.

```powershell
Get-AzureStorageKey -StorageAccountName "yourstorageaccount"
```

A következő parancsmag segítségével egy adott kulcs beolvasása. Érvényes értékek: az elsődleges és másodlagos.  

```powershell
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary
    
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary
```

Ha azt szeretné, újragenerálja a kulcsokat, használja a következő parancsmagot. KeyType – az érvényes értékek: "Elsődleges" és "Másodlagos"

```powershell
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Primary"
    
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Secondary"
```

## <a name="how-to-manage-azure-blobs"></a>Azure-blobokat kezelése
Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a világon HTTP vagy HTTPS PROTOKOLLON keresztül tárolásához. Ez a szakasz feltételezi, hogy Ön már ismeri az Azure Blob Storage szolgáltatással kapcsolatos fogalmak. Részletes információkért lásd: [az Blob storage .NET használatának első lépései](../blobs/storage-dotnet-how-to-use-blobs.md) és [Blob szolgáltatással kapcsolatos fogalmak](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-to-create-a-container"></a>Egy tároló létrehozása
Az Azure storage összes blobjának egy tárolóban kell lennie. A személyes tárolók a New-AzureStorageContainer parancsmaggal hozhat létre:

```powershell
$StorageContainerName = "yourcontainername"
New-AzureStorageContainer -Name $StorageContainerName -Permission Off
```

> [!NOTE]
> A névtelen olvasási hozzáférés három szintje van: **ki**, **Blob**, és **tároló**. Blobok névtelen hozzáférés érdekében a engedély paraméter értéke **ki**. Alapértelmezés szerint az új tároló privát, és csak a fiók tulajdonosa férhet. A névtelen felhasználók engedélyezése nyilvános olvasási hozzáférés a blob-erőforrásokhoz, de nem csomagtároló metaadatai vagy a tárolóban lévő blobok listájának értékre az engedély paraméter **Blob**. A blob-erőforrások, tároló metaadatait és a tárolóban lévő blobok listájának teljes nyilvános olvasási hozzáférés engedélyezéséhez a engedély paraméter értéke **tároló**. További információkért lásd: [tárolók és blobok névtelen olvasási hozzáférés kezelése](../blobs/storage-manage-access-to-resources.md).
> 
> 

### <a name="how-to-upload-a-blob-into-a-container"></a>Hogyan tölthetők fel blobok egy tárolóba
Az Azure Blob Storage támogatja a blokkblobokat és a lapblobokat. További információkért lásd: [ismertetése Blokkblobokat, hozzáfűző blobokat és Lapblobokat](http://msdn.microsoft.com/library/azure/ee691964.aspx).

A blobok feltöltése tárolót, használhatja a [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) parancsmag. Alapértelmezés szerint ez a parancs a helyi fájlok feltöltése blokkblobba. Adja meg a BLOB, - BlobType paraméterét használhatja.

A következő példában a [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) parancsmagot, hogy megkapja a megadott mappában lévő összes fájlt és majd továbbadja őket a következő parancsmag használatával a csővezeték-kezelőt. A [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) parancsmag feltölti a helyi fájlok a tároló:

```powershell
Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"
```

### <a name="how-to-download-blobs-from-a-container"></a>Blobok letöltése tárolója
A következő példa bemutatja, hogyan töltheti le blobok egy tárolóba. A példa első Azure Storage használata a tárfiók környezetét, amely tartalmazza a tárfiók nevét és az elsődleges elérési kulcsát kapcsolatot létesít. Majd, lekéri a példa egy blob hivatkozás segítségével a [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag. Ezután a példában a [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) parancsmag használatával töltheti le a blobok a helyi cél mappába.

```powershell
#Define the variables.
$ContainerName = "yourcontainername"
$DestinationFolder = "C:\DownloadImages"

#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#List all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Download blobs from a container.
New-Item -Path $DestinationFolder -ItemType Directory -Force
$blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx
```

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>A tároló egy másik BLOB másolása
Átmásolhatja BLOB storage-fiókok és régiók közötti aszinkron módon. A következő példa bemutatja, hogyan BLOB másolása egy tároló között két különböző tárfiókokban. A példa első beállítja a forrás és cél storage-fiókok változókat, és létrehoz egy adattároló-környezet az egyes fiókok számára. Ezután a példa másolja blobok a forrás-tárolójából. a cél tároló használata a [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) parancsmag. A példa feltételezi, hogy a forrás és cél tárfiók és tároló már létezik.

```powershell
#Define the source storage account and context.
$SourceStorageAccountName = "yoursourcestorageaccount"
$SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
$SrcContainerName = "yoursrccontainername"
$SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

#Define the destination storage account and context.
$DestStorageAccountName = "yourdeststorageaccount"
$DestStorageAccountKey = "Storage key for yourdeststorageaccount"
$DestContainerName = "destcontainername"
$DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

#Get a reference to blobs in the source container.
$blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

#Copy blobs from one container to another.
$blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext
```

Vegye figyelembe, hogy az ebben a példában egy aszinkron másolatot végez. Minden egyes példányra állapotának futtassa a figyelheti a [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) parancsmag.

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>Egy másodlagos helyet BLOB másolása
Blobok RA-GRS-engedélyezett fiók másodlagos helyre másolhatja.

```powershell
#define secondary storage context using a connection string constructed from secondary endpoints.
$SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext
```

### <a name="how-to-delete-a-blob"></a>Egy blob törlése
Blobok törléséhez először kérjen le egy blobhivatkozást, és majd hívja meg a Remove-AzureStorageBlob parancsmagot rajta. A következő példa egy adott tároló összes blobjának törli. A példa első beállítja a változókat a tárfiók, és létrehoz egy adattároló-környezet. A következő példában kéri le egy blob hivatkozás segítségével a [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag és futtatja a [Remove-AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) parancsmag segítségével távolítsa el a blobok az Azure storage-tárolójából.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "containername"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference to all the blobs in the container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Delete blobs in a specified container.
$blobs| Remove-AzureStorageBlob
```

## <a name="how-to-manage-azure-blob-snapshots"></a>Az Azure blob pillanatfelvételek kezelése
Azure lehetővé teszi a pillanatkép létrehozása a blob. Pillanatkép egy blobot, amely egy adott időben csak olvasható verziója telepítve. Pillanatkép létrehozása, azt is kell olvasni, másolja, vagy törölni, de nem módosított. A pillanatképek lehetőséget nyújtanak olyan biztonsági mentése blob, ahogyan megjelenik egy időben el. További információkért lásd: [egy pillanatképet készíteni egy Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-to-create-a-blob-snapshot"></a>Egy blob pillanatkép létrehozása
Hozzon létre egy BLOB snaphot, először kérjen le egy blobhivatkozást, és majd hívja a `ICloudBlob.CreateSnapshot` metódust. Az alábbi példa először beállítja a változókat a tárfiók, és létrehoz egy adattároló-környezet. A következő példában kéri le egy blob hivatkozás segítségével a [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmagot, és futtatja a [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) módszer segítségével készít pillanatképet.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "yourcontainername"
$BlobName = "yourblobname"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference to a blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

#Create a snapshot of the blob.
$snap = $blob.ICloudBlob.CreateSnapshot()
```

### <a name="how-to-list-a-blobs-snapshots"></a>Hogyan listázhat egy blob pillanatképek
Egy BLOB kívánt annyi pillanatképeket hozhat létre. A pillanatképek a blob nyomon követéséhez a jelenlegi pillanatképek társított felsorolása Az alábbi példában egy előre meghatározott blob és a hívások a [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) felsorolása található, hogy a blob-parancsmagot.  

```powershell
#Define the blob name.
$BlobName = "yourblobname"

#List the snapshots of a blob.
Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }
```

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>Pillanatkép készítése a blob másolása
Pillanatkép készítése a pillanatkép visszaállítása blob másolhatja. Részletes információk és korlátozások: [egy pillanatképet készíteni egy Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx). Az alábbi példa először beállítja a változókat a tárfiók, és létrehoz egy adattároló-környezet. A következő példában a tároló és a blob neve változók határozza meg. A példa lekéri a blob hivatkozás segítségével a [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmagot, és futtatja a [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) módszer segítségével készít pillanatképet. A példa fut majd, a [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) parancsmagot, hogy a pillanatkép a ICloudBlob objektum használatával a forrás BLOB BLOB másolása. Ne felejtse el frissíteni a változók a példa futtatása előtt a konfiguráció alapján. Vegye figyelembe, hogy az alábbi példa azt feltételezi, hogy a forrás és cél tárolókat, és a forrás blob már létezik.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Define the variables.
$SrcContainerName = "yoursourcecontainername"
$DestContainerName = "yourdestcontainername"
$SrcBlobName = "yourblobname"
$DestBlobName = "CopyBlobName"

#Get a reference to a blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

#Create a snapshot of a blob.
$snap = $blob.ICloudBlob.CreateSnapshot()

#Copy the snapshot to another container.
Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName
```

Most, hogy rendelkezik megtudta, hogyan kezelheti az Azure BLOB, és a blob-pillanatképeket az Azure PowerShell, ugorjon a következő szakaszban megtudhatja, hogyan kezelheti a táblák, üzenetsorok és fájlokat.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Azure-táblákban és táblaentitásokat kezelése
Az Azure Table storage szolgáltatás egy NoSQL-adattár, amely segítségével tárolja, és hatalmas strukturált, nem relációs adatok lekérdezése. A szolgáltatás fő összetevőit táblák, entitásokat és tulajdonságok. A tábla az entitások gyűjteményét. Egy entitás tulajdonságait. Minden entitás legfeljebb 252 tulajdonságot, amely minden név-érték pár is rendelkezik. Ez a szakasz feltételezi, hogy Ön már ismeri az Azure Table Storage szolgáltatással kapcsolatos fogalmak. Részletes információkért lásd: [ismertetése a Table szolgáltatás adatmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx) és [Ismerkedés az Azure Table storage használatának .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md).

A következő szakaszban akkor megtudhatja, hogyan kezelheti az Azure Table storage szolgáltatást Azure PowerShell használatával. Az ismertetett forgatókönyvek **létrehozása**, **törlése**, és **beolvasása** **táblák**, valamint **hozzáadása**, **lekérdezése**, és **tábla entitások törlése**.

### <a name="how-to-create-a-table"></a>Táblázat létrehozása
Minden táblának kell lennie az Azure storage-fiók. A következő példa bemutatja, hogyan hozzon létre egy táblát az Azure Storage. A példa első Azure Storage használata a tárfiók környezetét, amely tartalmazza a tárfiók nevét és a hozzáférési kulcsot kapcsolatot létesít. Ezután használja a [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) parancsmaggal hozzon létre egy táblát az Azure Storage.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

#Create a new table.
$tabName = "yourtablename"
New-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-to-retrieve-a-table"></a>Hogyan lehet lekérni egy tábla
Lekérdezi, és egy vagy az összes tábla tárfiókokban beolvasása. A következő példa bemutatja, hogyan lehet lekérni a megadott tábla használatával a [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag.

```powershell
#Retrieve a table.
$tabName = "yourtablename"
Get-AzureStorageTable –Name $tabName –Context $Ctx
```

Ha felhívja a Get-AzureStorageTable parancsmag paraméterek nélkül, egy tárfiók lekérdezi összes storage-táblákat.

### <a name="how-to-delete-a-table"></a>Tábla törlése
Törölheti a tábla a tárfiókból használatával a [Remove-AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) parancsmag.  

```powershell
#Delete a table.
$tabName = "yourtablename"
Remove-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-to-manage-table-entities"></a>Táblaentitásokat kezelése
Azure PowerShell jelenleg nem biztosít parancsmaggal táblaentitásokat közvetlenül. A táblaentitásokat műveletek végrehajtásához használhatja a megadott osztályok a [Azure Storage ügyféloldali kódtára a .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).

#### <a name="how-to-add-table-entities"></a>Táblaentitásokat hozzáadása
Entitás hozzáadása egy táblát, először létre kell hoznia egy objektum, amely meghatározza az entitás tulajdonságait. Egy entitás legfeljebb 255 tulajdonságokat, beleértve a 3 rendszertulajdonsággal állhat: **PartitionKey**, **RowKey**, és **időbélyeg**. Ön felelőssége lesz beszúrva, és az értékek frissítése **PartitionKey** és **RowKey**. A kiszolgáló kezeli értékének **időbélyeg**, amely nem módosítható. Együtt a **PartitionKey** és **RowKey** egyedi módon azonosítja az egy táblázaton belüli összes entitás.

* **PartitionKey**: meghatározza a partíció az entitás tárolt.
* **RowKey**: egyedileg azonosítja az entitást a partíción belül.

Egy entitás legfeljebb 252 egyéni tulajdonságok adhatók meg. További információkért lásd: [ismertetése a Table szolgáltatás adatmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).

A következő példa bemutatja, hogyan entitások hozzáadása a táblához. A példa bemutatja, hogyan kérhető le az alkalmazott tábla, és vegyen fel több entitás. Először az Azure Storage használata a tárfiók környezetét, amely tartalmazza a tárfiók nevét és a hozzáférési kulcsot kapcsolatot létesít. Ezt követően lekérdezi a megadott tábla használatával a [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag. Ha a tábla nem létezik, a [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) parancsmag létrehoz egy táblát az Azure Storage szolgál. A példában a következő egyéni függvény Add-entitás entitások hozzáadása a tábla minden egyes entitás partíció- és sorkulcsa megadásával határozza meg. Az Add-entitás függvényhívásokat a [New-Object](http://technet.microsoft.com/library/hh849885.aspx) parancsmagot a [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) osztály entitásobjektumra létrehozásához. Később, a példában meghívja a [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) metódust a forrásentitás-objektum hozzáadása a táblához.

```powershell
#Function Add-Entity: Adds an employee entity to a table.
function Add-Entity() {
    [CmdletBinding()]
    param(
       $table,
       [String]$partitionKey,
       [String]$rowKey,
       [String]$name,
       [Int]$id
    )

  $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
  $entity.Properties.Add("Name", $name)
  $entity.Properties.Add("ID", $id)

  $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
}

#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Retrieve the table if it already exists.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

#Create a new table if it does not exist.
if ($table -eq $null)
{
   $table = New-AzureStorageTable –Name $TableName -Context $Ctx
}

#Add multiple entities to a table.
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4
```

#### <a name="how-to-query-table-entities"></a>Táblaentitásokat lekérdezése
Ha egy táblából, használja a [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) osztály. Az alábbi példa azt feltételezi, hogy már futtatását a parancsfájl a megadott hozzáadása entitások című szakaszában talál. A példa első Azure Storage a tárolási adataival, amely tartalmazza a tárfiók nevét és a hozzáférési kulcsot kapcsolatot létesít. A következő megpróbálja beolvasni a korábban létrehozott "alkalmazottak" tábla használatával a [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag. Hívja a [New-Object](http://technet.microsoft.com/library/hh849885.aspx) parancsmagot a Microsoft.WindowsAzure.Storage.Table.TableQuery osztály az új objektumot hoz létre lekérdezést. A példa egy "ID" oszlop, amelynek értéke 1 megadott egy karakterlánc-szűrővel rendelkező entitások keresi. Részletes információkért lásd: [lekérdezése táblákat és entitásokat](http://msdn.microsoft.com/library/azure/dd894031.aspx). Ez a lekérdezés futtatásakor a szűrési feltételeknek megfelelő összes entitásokat ad vissza.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Get a reference to a table.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx

#Create a table query.
$query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

#Define columns to select.
$list = New-Object System.Collections.Generic.List[string]
$list.Add("RowKey")
$list.Add("ID")
$list.Add("Name")

#Set query details.
$query.FilterString = "ID gt 0"
$query.SelectColumns = $list
$query.TakeCount = 20

#Execute the query.
$entities = $table.CloudTable.ExecuteQuery($query)

#Display entity properties with the table format.
$entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize
```

#### <a name="how-to-delete-table-entities"></a>Tábla entitás törlése
A partíció- és sorfejlécek kulcsokkal entitás törölheti. Az alábbi példa azt feltételezi, hogy már futtatását a parancsfájl a megadott hozzáadása entitások című szakaszában talál. A példa első Azure Storage a tárolási adataival, amely tartalmazza a tárfiók nevét és a hozzáférési kulcsot kapcsolatot létesít. A következő megpróbálja beolvasni a korábban létrehozott "alkalmazottak" tábla használatával a [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag. Ha a tábla létezik, a példában meghívja a [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) metódusának segítéségével lekérheti az entitást a partíció- és sorfejlécek értékek alapján. Így az az entitás továbbítsa a [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) metódus törlése.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve the table.
$TableName = "Employees"
$table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

#If the table exists, start deleting its entities.
if ($table -ne $null) 
{
    #Together the PartitionKey and RowKey uniquely identify every  
    #entity within a table.
    $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
    $entity = $tableResult.Result
    if ($entity -ne $null)
    {
        #Delete the entity.
        $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
}
```

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>Az Azure várólisták és üzenetek kezelése
Az Azure Queue Storage szolgáltatás üzenetek nagy számban történő tárolására szolgál, amelyek HTTP- vagy HTTPS-kapcsolattal, hitelesített hívásokon keresztül a világon bárhonnan elérhetők. Ez a szakasz feltételezi, hogy Ön már ismeri az Azure Queue Storage szolgáltatás fogalmakat. Részletes információkért lásd: [Ismerkedés az Azure Queue storage használatának .NET](../storage-dotnet-how-to-use-queues.md).

Ez a szakasz bemutatja, hogyan kezelheti az Azure Queue storage szolgáltatás Azure PowerShell használatával. Az ismertetett forgatókönyvek **beszúrása** és **törlése** üzenetek várólistára, valamint **létrehozása**, **törlése**, és **sorok beolvasása**.

### <a name="how-to-create-a-queue"></a>A várólista létrehozása
Az alábbi példa első Azure Storage használata a tárfiók környezetét, amely tartalmazza a tárfiók nevét és a hozzáférési kulcsot kapcsolatot létesít. Ezután meghívja [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) parancsmag segítségével hozzon létre egy "queuename" nevű várólistát.

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$QueueName = "queuename"
$Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx
```

Azure Queue szolgáltatás elnevezési konvencióira vonatkozó információkért lásd: [elnevezési üzenetsorok és metaadatok](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Hogyan lehet lekérni egy várólistát
Lekérdezi, és egy konkrét várólistába helyezi vagy a tárfiókban lévő összes várólistán listájának beolvasása. A következő példa bemutatja, hogyan lehet lekérni a megadott várólista használatával a [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) parancsmag.

```powershell
#Retrieve a queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx
```

Ha meghívja a [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) parancsmag-paraméterek nélkül, akkor a várólisták listájának lekérése.

### <a name="how-to-delete-a-queue"></a>A várólista törlése
Egy üzenetsor és a benne tárolt összes üzenet törléséhez hívja meg a Remove-AzureStorageQueue parancsmagot. A következő példa bemutatja, hogyan használja a Remove-AzureStorageQueue parancsmag egy megadott várólista törlése.

```powershell
#Delete a queue.
$QueueName = "yourqueuename"
Remove-AzureStorageQueue –Name $QueueName –Context $Ctx
```

#### <a name="how-to-insert-a-message-into-a-queue"></a>Üzenet beszúrása egy várólistára
Üzenet beszúrása egy létező üzenetsorba, először létre kell hoznia egy új példányt a [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) osztály. Ezután hívja meg az [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) módszert. Egy CloudQueueMessage egy karakterláncból (UTF-8 formátumban), illetve egy bájttömböt hozhatók létre.

A következő példa bemutatja, hogyan üzenet hozzáadása egy üzenetsort. A példa első Azure Storage használata a tárfiók környezetét, amely tartalmazza a tárfiók nevét és a hozzáférési kulcsot kapcsolatot létesít. Ezt követően lekérdezi a megadott várólista használatával a [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) parancsmag. Ha a várólista létezik, a [New-Object](http://technet.microsoft.com/library/hh849885.aspx) parancsmag segítségével hozzon létre egy példányát a [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) osztály. Később, a példában meghívja a [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) az üzenet objektum egy várólista veheti fel a metódust. Itt található kód, amely lekérdezi a várólistát, és szúrja be a "MessageInfo" üzenet:

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve the queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

#If the queue exists, add a new message.
if ($Queue -ne $null) {
   # Create a new message using a constructor of the CloudQueueMessage class.
   $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

   # Add a new message to the queue.
   $Queue.CloudQueue.AddMessage($QueueMessage)
}
```

#### <a name="how-to-de-queue-at-the-next-message"></a>Útmutató a következő üzenet kivétele az üzenetsorból
A kód két lépésben távolít el egy üzenetet az üzenetsorból. A hívás esetén a [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) metódus, a következő üzenetet kap a sorhoz. A **GetMessage** módszerrel lekért üzenet láthatatlanná válik az adott üzenetsorban található üzeneteket olvasó többi kód számára. Szeretné távolítani az üzenetet az üzenetsorból, is hívja meg a [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) metódust. Az üzenetek kétlépéses eltávolítása lehetővé teszi, hogy ha a kód hardver- vagy szoftverhiba miatt nem tud feldolgozni egy üzenetet, a kód egy másik példánya megkaphassa ugyanazt az üzenetet, és újra megpróbálkozhasson a feldolgozásával. A kód a **DeleteMessage** módszert rögtön az üzenet feldolgozása után hívja meg.

```powershell
# Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

# Retrieve the queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

$InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Get the message object from the queue.
$QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
# Delete the message.
$Queue.CloudQueue.DeleteMessage($QueueMessage)
```

## <a name="how-to-manage-azure-file-shares-and-files"></a>Az Azure fájlmegosztások és fájlok kezelése
Az Azure File storage közös tárterületet biztosít számára a szabványos SMB protokollt használó alkalmazások. Microsoft Azure virtuális gépek és felhőszolgáltatások megosztott csatlakoztatott megosztásokon keresztül alkalmazások összetevői között, és a helyszíni alkalmazások hozzáférhetnek a fájladatok egy megosztáson található, a File storage API vagy az Azure PowerShell használatával.

További információ az Azure File storage: [Ismerkedés az Azure File storage on Windows](../storage-dotnet-how-to-use-files.md) és [File szolgáltatás REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-to-set-and-query-storage-analytics"></a>Beállítása és a lekérdezés tárolási analitika
Használhat [Azure Storage Analytics](../storage-analytics.md) gyűjtéséhez az Azure storage-fiókok és a tárfiók küldött kérelmek naplózni. Storage mérőszámainak segítségével egy tárfiókot, és a tárolási naplózási diagnosztizálása és elhárítása a tárfiók állapotának figyelésére. Az Azure portálon vagy a Windows PowerShell használatával, vagy programozott módon, a storage ügyféloldali kódtár figyelését be tudja állítani. Tárolási naplózási kiszolgálóoldali történik, és lehetővé teszi a tárfiókban lévő sikeres és a sikertelen kérelmek adatainak rögzítését. Ezek a naplók lehetővé teszik a Részletek területen az olvasási, írási és törlési műveleteket a táblák, üzenetsorok, blobok, valamint a sikertelen kérelmek okait.

Engedélyezése és megtekintése a PowerShell használatával Storage Metrics data kapcsolatban [PowerShell-lel Storage mérőszámainak engedélyezése](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Engedélyezése és a PowerShell használatával tároló-naplózás adatainak beolvasása további tudnivalókért lásd: [engedélyezése a tárolási-naplózás PowerShell-lel](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) és [keresése a naplózás tárolási naplóadatok](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
A Storage Metrics és a naplózás tárolási tárolási problémák elhárítása részletes információkért lásd: [figyelés felismerése és hibaelhárítása a Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>Közös hozzáférésű Jogosultságkód (SAS) és a tárolt házirend kezelése
Közös hozzáférésű jogosultságkód bármely alkalmazás Azure-tárolót a biztonsági modell fontos részét képezik. A tárfiókhoz korlátozott engedélyekkel biztosítani az ügyfelek nem rendelkezhet a fiókkulcs hasznosak. Alapértelmezés szerint csak a tárfiók tulajdonosa elérhessék blobot, táblát és üzenetsort, hogy a fiókon belül. Ha a szolgáltatás vagy alkalmazás ezek erőforrásai elérhetővé más ügyfelek számára a hozzáférési kulcs megosztása nélkül, három lehetőség közül választhat:

* Egy tároló engedélyeket a tároló és a blobok névtelen olvasási hozzáférés engedélyezéséhez. Ez nem engedélyezett táblák vagy olyan várólisták esetében.
* Használjon egy közös hozzáférésű jogosultságkódot, hogy korlátozott hozzáférési jogosultsága ahhoz, tárolók, blobok, üzenetsorok és táblák biztosít egy adott időintervallumban.
* A tárolt házirend segítségével szerezzen be egy közös hozzáférésű jogosultságkód egy tárolót vagy a benne található blobokat, egy sor vagy tábla szabályozhatják további szintet. A tárolt házirend lehetővé teszi a kezdő időpont, a lejárat időpontjának és az aláírás engedélyeinek módosítása, vagy visszavonni az azt követő ki van állítva.

A közös hozzáférésű jogosultságkód lehet két űrlap egyikében:

* **Ad hoc biztonsági Társítások**: egy ad hoc SAS-a kezdési ideje, a lejárat időpontjának létrehozásakor, és a SAS engedélyeinek összes adhatók meg a SAS URI-t. Az ilyen típusú SAS lehet létrehozni egy tárolót, a blob, tábla, vagy várólista, és nem visszavonható.
* **SAS tárolt hozzáférési házirenddel**: A tárolt házirend van definiálva az erőforrás-tárolónak egy blob-tároló, táblázat vagy várólista -, és legalább egy közös hozzáférésű jogosultságkód megkötéseit kezelésére használhatja. SAS-kód társítása a tárolt házirend, a SAS - a kezdési ideje, a lejárat időpontjának és a-vonatkozó engedélyeit a tárolt házirend korlátozásait örökölnek. Ez a biztonsági Társítások típus visszavonható.

További információkért lásd: [használatával megosztott hozzáférési aláírásokkal (SAS)](../storage-dotnet-shared-access-signature-part-1.md) és [tárolók és blobok névtelen olvasási hozzáférés kezelése](../blobs/storage-manage-access-to-resources.md).

A következő szakaszokban lévő, megtudhatja, hogyan Azure-táblákban egy megosztott hozzáférési aláírást token és tárolt hozzáférési házirend létrehozásához. Az Azure PowerShell hasonló parancsmagokat tárolók, blobok és várólisták is kínál. Ez a szakasz a parancsfájlok futtatásához töltse le a [Azure PowerShell verziója 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) vagy újabb.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>A csoportházirend-alapú közös hozzáférésű Jogosultságkód jogkivonat létrehozása
A New-AzureStorageTableStoredAccessPolicy parancsmag segítségével hozzon létre egy új tárolt házirend. Majd, meghívják a [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) parancsmaggal hozzon létre egy új csoportházirend-alapú megosztott aláírás jogkivonatot egy Azure Storage tábla.

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx
```

### <a name="how-to-create-an-ad-hoc-non-revocable-shared-access-signature-token"></a>Az ad hoc (nem visszavonható) közös hozzáférésű Jogosultságkód-token létrehozása
Használja a [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) parancsmag segítségével hozzon létre egy új alkalmi (nem visszavonható) közös hozzáférésű Jogosultságkód tokent egy Azure Storage-tábla a(z):

```powershell
New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx
```
    
### <a name="how-to-create-a-stored-access-policy"></a>A tárolt hozzáférési házirend létrehozása
A New-AzureStorageTableStoredAccessPolicy parancsmag segítségével hozzon létre egy új tárolt házirend egy Azure Storage táblához:

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
```
    
### <a name="how-to-update-a-stored-access-policy"></a>A tárolt házirend frissítése
A Set-AzureStorageTableStoredAccessPolicy parancsmag segítségével egy meglévő tárolt hozzáférési házirendet egy Azure Storage-táblázat frissítése:

```powershell
Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx
```

### <a name="how-to-delete-a-stored-access-policy"></a>A tárolt házirend törlése
A Remove-AzureStorageTableStoredAccessPolicy parancsmag segítségével törölheti a tárolt házirend egy Azure Storage táblán:

```powershell
Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx
```

## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Azure Storage használata az Amerikai Egyesült Államok kormánya és Azure Kínában
Környezetet az Azure például egy Microsoft Azure-ban független telepítését [Azure Government az Amerikai Egyesült Államok kormánya](https://azure.microsoft.com/features/gov/), [globális Azure AzureCloud](https://portal.azure.com), és [AzureChinaCloud Azure Kínában a 21Vianet által működtetett](http://www.windowsazure.cn/). Új Azure-környezetek Egyesült kormányzati és Azure Kína telepítése.

AzureChinaCloud az Azure Storage használatához szüksége AzureChinaCloud társított adattároló-környezet létrehozása. Kövesse az alábbi lépéseket az első lépésekhez:

1. Futtassa a [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) parancsmag használatával ellenőrizheti az elérhető az Azure-környezetek:
   
    ```powershell
    Get-AzureEnvironment
    ```

2. Egy Azure Kína fiók hozzáadása a Windows PowerShell:
   
    ```powershell
    Add-AzureAccount –Environment AzureChinaCloud
    ```

3. Adattároló-környezet AzureChinaCloud fiók létrehozása:
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud
    ```

Az Azure Storage használatához [USA Az Azure Government](https://azure.microsoft.com/features/gov/), érdemes egy új környezet, és ezután hozzon létre egy új adattároló-környezet ebben a környezetben:

1. Futtassa a [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) parancsmag használatával ellenőrizheti az elérhető az Azure-környezetek:

    ```powershell
    Get-AzureEnvironment
    ```

2. A Windows PowerShell Azure Amerikai Egyesült államokbeli kormányzati fiók hozzáadása:
   
    ```powershell
    Add-AzureAccount –Environment AzureUSGovernment
    ```

3. Adattároló-környezet AzureUSGovernment fiók létrehozása:
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment
    ```
     
További információkért lásd:

* [A Microsoft Azure Government – útmutató fejlesztőknek](../../azure-government/documentation-government-developer-guide.md).
* [Kínai szolgáltatás egy alkalmazás létrehozásakor különbségek áttekintése](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Következő lépések
Ez az útmutató az Azure PowerShell Azure Storage kezelése hogy megismerte. Íme, néhány kapcsolódó cikkek és a velük kapcsolatos további források.

* [Az Azure Storage dokumentációja](https://azure.microsoft.com/documentation/services/storage/)
* [Az Azure Storage PowerShell-parancsmagok](/powershell/module/azurerm.storage/#storage)
* [A Windows PowerShell-referencia](https://msdn.microsoft.com/library/ms714469.aspx)

[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
