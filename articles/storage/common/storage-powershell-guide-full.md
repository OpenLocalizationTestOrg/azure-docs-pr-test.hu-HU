---
title: az Azure Storage Azure PowerShell aaaUsing |} Microsoft Docs
description: "Megtudhatja, hogyan toouse hello Azure Storage toocreate Azure PowerShell-parancsmagok és a storage-fiókok; kezelése blobok, táblák, üzenetsorok és fájlok; használata konfigurálja és tárolási analitika lekérdezése, és a közös hozzáférésű jogosultságkód létrehozása."
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
ms.openlocfilehash: 17d638e741911ceafb9777d5c2fce7bfe533e50c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-powershell-with-azure-storage"></a>Using Azure PowerShell with Azure Storage (Az Azure PowerShell és az Azure Storage együttes használata)
## <a name="overview"></a>Áttekintés
Az Azure PowerShell egy modul, amely a parancsmagok toomanage Azure, a Windows PowerShell segítségével. Ez egy feladatalapú parancshéj és parancsnyelv, amely kifejezetten rendszerfelügyeleti célra készült. A PowerShell-lel könnyen szabályozhatja és hello felügyelete az Azure-szolgáltatások és alkalmazások automatizálása. Például használhatja hello parancsmagok tooperform hello hello keresztül végzett feladatok azonos [Azure-portálon](https://portal.azure.com).

Ez az útmutató azt fogja feltárja hogyan toouse hello [Azure tárolási parancsmagok](/powershell/module/azurerm.storage/#storage) tooperform számos fejlesztését és a felügyeleti feladatot az Azure Storage.

Ez az útmutató feltételezi, hogy rendelkezik tapasztalattal használatával [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) és [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx). hello az útmutató számos parancsfájlt toodemonstrate hello használata az Azure Storage PowerShell. Minden parancsprogram futtatása előtt a konfiguráció alapján hello parancsfájl-változókat frissítenie kell.

hello első rész a jelen útmutató az Azure Storage és a PowerShell kiadványok biztosít. Részletes információk és utasítások, indítsa el a hello [az Azure Storage Azure PowerShell használatára vonatkozó Előfeltételek](#prerequisites-for-using-azure-powershell-with-azure-storage).

## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Ismerkedés az Azure Storage és a PowerShell 5 perc
Ez a szakasz bemutatja, hogyan tooaccess Azure Storage PowerShell 5 perc múlva.

**Új tooAzure:** a Microsoft Azure-előfizetés és az adott előfizetéshez tartozó Microsoft-fiókkal. Az Azure megvásárlási lehetőségeinek információkért lásd: [ingyenes](https://azure.microsoft.com/pricing/free-trial/), [beszerzési lehetőségek](https://azure.microsoft.com/pricing/purchase-options/), és [ajánlatok](https://azure.microsoft.com/pricing/member-offers/) (az MSDN, a Microsoft Partner Network, és a BizSpark és egyéb Microsoft programok tagjai).

Lásd: [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Azure-előfizetések további információt.

**Miután létrehozta a Microsoft Azure-előfizetésre és -fiókra:**

1. Töltse le és telepítse legújabb hello [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).
2. Kezdő Windows PowerShell integrált parancsfájlkezelés környezet (ISE): A helyi számítógépen, váltson toohello **Start** menü. Típus **felügyeleti eszközök** toorun kattintson azt. A hello **felügyeleti eszközök** ablak, kattintson a jobb gombbal **Windows PowerShell ISE**, kattintson a **Futtatás rendszergazdaként**.
3. A **Windows PowerShell ISE**, kattintson a **fájl** > **új** toocreate egy új parancsfájlt.
4. Most lesz ad egy egyszerű parancsprogram, amely bemutatja az alapvető PowerShell parancsok tooaccess Azure Storage. hello parancsfájl először kéri az Azure-fiók hitelesítő adatait tooadd az Azure-fiók toohello helyi PowerShell környezetben. Ezután hello parancsfájl hello alapértelmezett Azure-előfizetéssel, és hozzon létre egy új tárfiókot az Azure-ban. A következő hello parancsfájl hozzon létre egy új tárolót az új tárfiókot, és töltse fel a meglévő lemezkép fájl (blob) toothat tároló. Hello parancsfájl megjeleníti az adott tároló összes BLOB, miután egy új célkönyvtáron létrehozza a helyi számítógépen, és hello képfájl letöltése.
5. A következő kódot a szakasz hello, válassza ki hello parancsfájl hello megjegyzések közötti **#begin** és **#end**. Nyomja le a CTRL + C toocopy azt toohello vágólapra.

    ```powershell
    # begin
    # Update with hello name of your subscription.
    $SubscriptionName = "YourSubscriptionName"
       
    # Give a name tooyour new storage account. It must be lowercase!
    $StorageAccountName = "yourstorageaccountname"
       
    # Choose "West US" as an example.
    $Location = "West US"
       
    # Give a name tooyour new container.
    $ContainerName = "imagecontainer"
       
    # Have an image file and a source directory in your local computer.
    $ImageToUpload = "C:\Images\HelloWorld.png"
       
    # A destination directory in your local computer.
    $DestinationFolder = "C:\DownloadImages"
       
    # Add your Azure account toohello local PowerShell environment.
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
       
    # Download blobs from hello container:
    # Get a reference tooa list of all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName
       
    # Create hello destination directory.
    New-Item -Path $DestinationFolder -ItemType Directory -Force  
       
    # Download blobs into hello local destination directory.
    $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
       
    # end
    ```

6. A **Windows PowerShell ISE**, nyomja le a CTRL + V toocopy hello parancsfájl. Kattintson a **fájl** > **mentése**. A hello **Mentés másként** párbeszédpanelen, a típusnév hello hello parancsfájl, például a "mystoragescript." Kattintson a **Save** (Mentés) gombra.
7. Most tooupdate hello parancsfájl-változókat a konfigurációs beállítások alapján van szüksége. Frissítenie kell a hello **$SubscriptionName** változó a saját előfizetésével. Tartsa hello más változók a parancsfájl hello, vagy igény szerint frissítse azokat.
   
   * **$SubscriptionName:** ezt a változót a saját előfizetés nevével kell frissíteni. Kövesse a következő három módon toolocate hello nevét az előfizetésben hello egyikét:
     
    a. A **Windows PowerShell ISE**, kattintson a **fájl** > **új** toocreate egy új parancsfájlt. Másolás hello alábbi parancsfájl-toohello új parancsfájlt, és kattintson **Debug** > **futtatása**. hello következő parancsfájlt a rendszer először kérje meg az Azure-fiók hitelesítő adatait tooadd az Azure-fiók toohello helyi PowerShell környezet és ezután meg lehet jeleníteni hello előfizetéseket, amelyek csatlakoztatott toohello helyi PowerShell-munkamenetben. Jegyezze fel az hello előfizetés hello neve, amelyet az toouse az oktatóanyag során:
     
    ```powershell
    Add-AzureAccount 
      Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName
    ```

    b. toolocate, és másolja az előfizetés neve hello [Azure-portálon](https://portal.azure.com), a központ menü a bal oldali hello hello, kattintson a **előfizetések**. Másolja a megjeleníteni kívánt toouse hello parancsfájlok futtatása az útmutató során előfizetés hello neve.
     
     ![Azure Portal](./media/storage-powershell-guide-full/Subscription_Previewportal.png)

    c. toolocate, és másolja az előfizetés neve hello [klasszikus Azure portál](https://manage.windowsazure.com/), görgessen lefelé, és kattintson a **beállítások** a bal oldalán található hello portal hello. Kattintson a **előfizetések** toosee az előfizetések listája. Másolja ki toouse az útmutatóban megadott hello parancsfájlok futtatása közben előfizetést hello nevét.
     
     ![klasszikus Azure portál](./media/storage-powershell-guide-full/Subscription_currentportal.png)

   * **$StorageAccountName:** hello megadott hello parancsfájl nevét használja, vagy adjon meg egy új nevet a tárfiók. **Fontos:** hello tárfiókja nevére hello Azure egyedinek kell lennie. Az kisbetűnek kell lennie, túl!
   * **$Location:** hello parancsfájlban használja a megadott "USA nyugati régiója" hello, vagy válasszon más Azure helyeken, például az USA keleti régiója, Észak-Európa, és így tovább.
   * **$ContainerName:** hello megadott hello parancsfájl nevét használja, vagy adjon meg egy új nevet a tároló.
   * **$ImageToUpload:** , mint a helyi számítógépen, adja meg egy elérési utat tooa kép: "C:\Images\HelloWorld.png".
   * **$DestinationFolder:** adjon meg egy elérési utat tooa helyi könyvtár toostore fájlokat töltött le az Azure Storage, például: "C:\DownloadImages".
8. Miután frissítette a parancsfájl-változókat hello hello "mystoragescript.ps1" fájlban, kattintson a **fájl** > **mentése**. Kattintson a **Debug** > **futtatása** vagy nyomja le az ENTER **F5** toorun hello parancsfájl.  

Hello parancsfájl futtatása után rendelkeznie kell egy helyi célmappát, amely tartalmazza a letöltött hello lemezképfájlt. a következő képernyőkép hello látható egy példa a kimenetre:

![Blobok letöltése](./media/storage-powershell-guide-full/Blobdownload.png)

> [!NOTE]
> hello "Bevezetés az Azure Storage és a PowerShell 5 percben" című szakaszt a megadott gyors bevezetés hogyan toouse az Azure Storage Azure PowerShell. Részletes információk és utasítások javasoljuk, a következő szakaszok tooread hello.
> 

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Az Azure Storage Azure PowerShell használatának előfeltételei
Szüksége van egy Azure előfizetésre és -fiókra toorun hello PowerShell-parancsmagok ebben az útmutatóban megadott fent leírt módon.

Az Azure PowerShell egy modul, amely a parancsmagok toomanage Azure, a Windows PowerShell segítségével. A telepítése és beállítása az Azure PowerShell információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview). Azt javasoljuk, hogy letöltése és telepítése vagy frissítése toohello legújabb Azure PowerShell modul az útmutató használata előtt.

Hello szabványos Windows PowerShell konzol vagy a Windows PowerShell integrált parancsfájlkezelési környezet (ISE) hello hello parancsmagokat futtathat. Például tooopen **Windows PowerShell ISE**nyissa meg toohello Start menüben, írja be a felügyeleti eszközök és toorun kattintson azt. Hello felügyeleti eszközök ablakban kattintson a jobb gombbal a Windows PowerShell ISE, kattintson a Futtatás rendszergazdaként.

## <a name="how-toomanage-storage-accounts-in-azure"></a>Hogyan toomanage tárfiókot az Azure-ban

Vessen egy pillantást a PowerShell-lel Azure storage-fiók kezelése

### <a name="how-tooset-a-default-azure-subscription"></a>Hogyan tooset egy alapértelmezett Azure-előfizetés
Azure Storage Azure PowerShell használatával, kell tooauthenticate az ügyfél-környezet Azure az Azure Active Directory-hitelesítés vagy Tanúsítványalapú hitelesítés használatával toomanage. Részletes információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) oktatóanyag. Ez az útmutató hello Azure Active Directory-hitelesítést használ.

1. A Windows PowerShell ISE írja be a következő parancs tooadd hello az Azure-fiók toohello helyi PowerShell környezetben:

    ```powershell
    Add-AzureAccount
    ```

2. Hello "tooMicrosoft Azure bejelentkezés", típus hello e-mail címét és a fiókhoz társított jelszót. Azure hitelesíti és menti a hello hitelesítő adatokat, és majd a hello ablak bezárása.

3. Ezután futtassa a következő parancs tooview hello Azure fiókok helyi PowerShell-környezetében, és ellenőrizze, hogy szerepel-e a fiók hello:
   
    ```powershell
    Get-AzureAccount
    ```
4. Ezután futtassa a következő parancsmag tooview hello hello előfizetéseket, amelyek csatlakoztatott toohello helyi PowerShell-munkamenetet, és ellenőrizze, hogy szerepel-e az előfizetés:

    ```powershell
    Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`
    ```
5. Azure-előfizetésre, az alapértelmezett tooset hello válassza-AzureSubscription parancsmag futtatásához:

    ```powershell
    $SubscriptionName = 'Your subscription Name'
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
    ```

6. Ellenőrizze a hello alapértelmezett előfizetés neve hello hello Get-AzureSubscription parancsmag futtatásával:

    ```powershell
    Get-AzureSubscription -Default
    ```

7. Futtassa az toosee összes hello elérhető PowerShell-parancsmagok az Azure Storage:
    
    ```powershell
    Get-Command -Module Azure -Noun *Storage*`
    ```

### <a name="how-toocreate-a-new-azure-storage-account"></a>Hogyan toocreate egy új Azure storage-fiók
az Azure storage toouse, szüksége lesz egy tárfiókot. Létrehozhat egy új Azure-tárfiókot, a számítógép tooconnect tooyour előfizetés konfigurálása után.

1. Futtassa a hello Get-AzureLocation parancsmag toofind összes hello elérhető adatközpont-helyeinek:

    ```powershell
    Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes
    ```

2. Ezt követően futtassa hello New-AzureStorageAccount parancsmag toocreate egy új tárfiókot. hello alábbi példa létrehoz egy új tárfiókot hello "USA nyugati régiója" adatközpontban.
   
    ```powershell
    $location = "West US"
    $StorageAccountName = "yourstorageaccount"
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location
    ```

> [!IMPORTANT]
> a tárfiók nevére hello Azure belül egyedieknek kell lenniük, és kisbetűnek kell lennie. Az elnevezési konvenciókat és korlátozások, lásd: [kapcsolatos Azure Storage-fiókok](../storage-create-storage-account.md) és [elnevezési és hivatkozó tárolók, Blobok és metaadatok](http://msdn.microsoft.com/library/azure/dd135715.aspx).
> 
> 

### <a name="how-tooset-a-default-azure-storage-account"></a>Hogyan tooset egy alapértelmezett Azure storage-fiók
Az előfizetés több tárfiókot is lehet. Válassza ki az egyiket, és állítsa be úgy, mint hello alapértelmezett tárfiókot az összes tárolási hello parancsai hello azonos PowerShell-munkamenetben. Ez lehetővé teszi toorun hello Azure PowerShell tárolási parancsok hello adattároló-környezet explicit megadása nélkül.

1. egy alapértelmezett tárfiókot az előfizetéshez tartozó tooset, hello Set-AzureSubscription parancsmag is futtathatja.

    ```powershell
    $SubscriptionName = "Your subscription name"
    $StorageAccountName = "yourstorageaccount"  
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

2. Ezután futtassa a Get-AzureSubscription hello parancsmag tooverify, hogy hello tárfiók kapcsolódik az alapértelmezett előfizetéses fiókba. Ez a parancs visszaadja a hello előfizetés tulajdonságai hello az aktuális előfizetésben többek között az aktuális tárfiók.

    ```powershell
    Get-AzureSubscription –Current
    ```

### <a name="how-toolist-all-azure-storage-accounts-in-a-subscription"></a>Hogyan toolist minden Azure tárfiókok egy előfizetésben található
Minden Azure-előfizetés mentése too100 tárfiókok rendelkezhet. Hello legfrissebb információk korlátozások: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md).

Futtassa a következő parancsmag toofind hello nevét és hello aktuális előfizetés tárfiókjai hello állapotának hello:

```powershell
Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus
```

### <a name="how-toocreate-an-azure-storage-context"></a>Hogyan toocreate egy Azure storage-környezetben
Az Azure storage-környezet egy objektum PowerShell tooencapsulate hello tároló hitelesítő adatait. Tárolási környezetet használ a következő parancsmag futtatása közben lehetővé teszi, hogy Ön tooauthenticate kérelmét hello tárfiók és a hozzáférési kulcs explicit megadása nélkül. Adattároló-környezet az sokféleképpen, például a tárolási fiók nevét és hívóbetűjét, a közös hozzáférésű jogosultságkód (SAS) jogkivonatot, a kapcsolati karakterlánc használatával hozhat létre vagy névtelen. További információkért lásd: [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).  

Adattároló-környezet használja a következő három módon toocreate hello egyikét:

* Futtassa a hello [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) parancsmag toofind kimenő hello elsődleges tárelérési kulcs az Azure-tárfiókot. Ezután hívja hello [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) parancsmag toocreate adattároló-környezet:

    ```powershell
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    ```

* Az Azure storage-tároló megosztott hozzáférési aláírást jogkivonat előállítása és toocreate adattároló-környezet használhatja:

    ```powershell
    $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken
    ```

    További információkért lásd: [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) és [használatával megosztott hozzáférési aláírásokkal (SAS)](../storage-dotnet-shared-access-signature-part-1.md).

* Bizonyos esetekben érdemes lehet toospecify hello Szolgáltatásvégpontok egy új adattároló-környezet létrehozásakor. Erre akkor lehet szükség, ha egy egyéni tartománynevet a tárfiók hello Blob szolgáltatásban regisztrált vagy a közös hozzáférésű jogosultságkód toouse kívánt tárolási erőforrások eléréséhez. Hello szolgáltatás végpontokat a kapcsolódási karakterláncban és az új adattároló-környezet toocreate alább látható:

    ```powershell
    $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
    $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString
    ```

További információt a tooconfigure egy tárolási kapcsolati karakterlánc, lásd: [kapcsolati karakterláncok konfigurálása](../storage-configure-connection-string.md).

Most, hogy a számítógépet, és megtanulta, hogyan toomanage előfizetések és a storage-fiókok az Azure PowerShell, lépjen a toohello hogyan toomanage Azure blobok következő szakasz toolearn és blob-pillanatképeket.

### <a name="how-tooretrieve-and-regenerate-azure-storage-keys"></a>Hogyan tooretrieve és újragenerálása az Azure storage-kulcsok
Egy Azure Storage-fiók két kulcsait tartalmaz. A következő parancsmag tooretrieve hello használhatja a kulcsokat.

```powershell
Get-AzureStorageKey -StorageAccountName "yourstorageaccount"
```

A következő parancsmag tooretrieve egy adott kulcs hello használata. Érvényes értékek: az elsődleges és másodlagos.  

```powershell
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary
    
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary
```

Ha azt szeretné, hogy tooregenerate a kulcs, használja a következő parancsmag hello. KeyType – az érvényes értékek: "Elsődleges" és "Másodlagos"

```powershell
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Primary"
    
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Secondary"
```

## <a name="how-toomanage-azure-blobs"></a>Hogyan toomanage Azure blobok
Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához. Ez a szakasz feltételezi, hogy Ön már ismeri a hello Azure Blob Storage szolgáltatással kapcsolatos fogalmak. Részletes információkért lásd: [az Blob storage .NET használatának első lépései](../blobs/storage-dotnet-how-to-use-blobs.md) és [Blob szolgáltatással kapcsolatos fogalmak](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-toocreate-a-container"></a>Hogyan toocreate tárolója
Az Azure storage összes blobjának egy tárolóban kell lennie. Egy személyes tárolót hello New-AzureStorageContainer parancsmaggal hozhat létre:

```powershell
$StorageContainerName = "yourcontainername"
New-AzureStorageContainer -Name $StorageContainerName -Permission Off
```

> [!NOTE]
> A névtelen olvasási hozzáférés három szintje van: **ki**, **Blob**, és **tároló**. tooprevent névtelen hozzáférés tooblobs, set hello engedély paraméter túl**ki**. Alapértelmezés szerint a hello új tároló privát, és csak a fiók tulajdonosának hello keresztül elérhető legyen. tooallow névtelen nyilvános olvasási hozzáférés tooblob erőforrásokat, de nem toocontainer metaadatok vagy toohello listája hello tárolóban lévő blobok, hello engedély paraméter értéke túl**Blob**. tooallow teljes nyilvános olvasási tooblob erőforrások eléréséhez, a tároló metaadatait, és a hello tárolóban lévő blobok hello listája, hello engedély paraméter értéke túl**tároló**. További információkért lásd: [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](../blobs/storage-manage-access-to-resources.md).
> 
> 

### <a name="how-tooupload-a-blob-into-a-container"></a>Hogyan tooupload blob egy tárolóba
Az Azure Blob Storage támogatja a blokkblobokat és a lapblobokat. További információkért lásd: [ismertetése Blokkblobokat, hozzáfűző blobokat és Lapblobokat](http://msdn.microsoft.com/library/azure/ee691964.aspx).

tooa tárolóban lévő blobok tooupload, hello használható [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) parancsmag. Alapértelmezés szerint ez a parancs hello helyi fájlok tooa blokkblob feltöltését. toospecify hello típus hello BLOB, hello - BlobType paraméterrel is használhatja.

hello alábbi példa fut hello [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) parancsmag tooget összes hello hello megadott mappában található fájlokat, és majd azokat továbbítja toohello a következő parancsmag használatával hello csővezeték-kezelőt. Hello [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) parancsmag feltölt hello helyi fájlok tooyour tároló:

```powershell
Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"
```

### <a name="how-toodownload-blobs-from-a-container"></a>Hogyan toodownload blobok egy tárolójából.
hello a következő példa bemutatja, hogyan toodownload blobok a tárolóból. hello példa először hoz létre a kapcsolat tooAzure tárolási hello tárfiók környezetét, ide tartozik az hello tárfiók neve és az elsődleges elérési kulcsot használ. Ezt követően hello példa kéri le egy blobhivatkozást, hello segítségével [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag. Ezután a hello példában hello [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) parancsmag toodownload blobok hello helyi cél mappába.

```powershell
#Define hello variables.
$ContainerName = "yourcontainername"
$DestinationFolder = "C:\DownloadImages"

#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#List all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Download blobs from a container.
New-Item -Path $DestinationFolder -ItemType Directory -Force
$blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx
```

### <a name="how-toocopy-blobs-from-one-storage-container-tooanother"></a>Hogyan toocopy egy tárolási tároló tooanother a blobok
Átmásolhatja BLOB storage-fiókok és régiók közötti aszinkron módon. hello következő példa bemutatja, hogyan toocopy blobok a egy tárolási tároló tooanother két különböző tárfiókokban. hello példa először beállítja a forrás és cél storage-fiókok változókat, és létrehoz egy adattároló-környezet az egyes fiókok számára. A következő hello példa átmásolja blobok hello forrás tároló toohello céltárolója hello segítségével [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) parancsmag. hello példa feltételezi, hogy hello forrás és cél tárfiók és tároló már létezik.

```powershell
#Define hello source storage account and context.
$SourceStorageAccountName = "yoursourcestorageaccount"
$SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
$SrcContainerName = "yoursrccontainername"
$SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

#Define hello destination storage account and context.
$DestStorageAccountName = "yourdeststorageaccount"
$DestStorageAccountKey = "Storage key for yourdeststorageaccount"
$DestContainerName = "destcontainername"
$DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

#Get a reference tooblobs in hello source container.
$blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

#Copy blobs from one container tooanother.
$blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext
```

Vegye figyelembe, hogy az ebben a példában egy aszinkron másolatot végez. Minden egyes példányra hello állapotának figyelheti hello futtatásával [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) parancsmag.

### <a name="how-toocopy-blobs-from-a-secondary-location"></a>Hogyan toocopy blobok egy másodlagos helyen
Blobok RA-GRS-engedélyezett fiók hello a másodlagos helyre másolhatja.

```powershell
#define secondary storage context using a connection string constructed from secondary endpoints.
$SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext
```

### <a name="how-toodelete-a-blob"></a>Hogyan toodelete blob
toodelete blob, először kérjen le egy blobhivatkozást, és hívhatja hello Remove-AzureStorageBlob parancsmag rajta. a következő példa hello egy adott tárolóban lévő összes hello blobok törlése. hello példa először beállítja a változókat a tárfiók, és létrehoz egy adattároló-környezet. A következő hello példa kéri le egy blobhivatkozást, hello segítségével [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag és futtatása hello [Remove-AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) parancsmag tooremove BLOB az Azure storage-tárolójából.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "containername"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference tooall hello blobs in hello container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Delete blobs in a specified container.
$blobs| Remove-AzureStorageBlob
```

## <a name="how-toomanage-azure-blob-snapshots"></a>Hogyan toomanage Azure blob-pillanatképek
Azure lehetővé teszi a pillanatkép létrehozása a blob. Pillanatkép egy blobot, amely egy adott időben csak olvasható verziója telepítve. Pillanatkép létrehozása, azt is kell olvasni, másolja, vagy törölni, de nem módosított. Pillanatképek adja meg egy módon tooback blob be időben a jelenleg megjelenített. További információkért lásd: [egy pillanatképet készíteni egy Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-toocreate-a-blob-snapshot"></a>Hogyan toocreate blob pillanatkép
egy BLOB, snaphot toocreate először kérjen le egy blobhivatkozást, és majd meghívják a hello `ICloudBlob.CreateSnapshot` metódust. hello alábbi példa először beállítja a változókat a tárfiók, és létrehoz egy adattároló-környezet. A következő hello példa kéri le egy blobhivatkozást, hello segítségével [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag és futtatása hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metódus toocreate pillanatképet.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "yourcontainername"
$BlobName = "yourblobname"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference tooa blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

#Create a snapshot of hello blob.
$snap = $blob.ICloudBlob.CreateSnapshot()
```

### <a name="how-toolist-a-blobs-snapshots"></a>Hogyan toolist blob pillanatképek
Egy BLOB kívánt annyi pillanatképeket hozhat létre. A blob tootrack tartozó hello pillanatképet listázhatja a jelenlegi pillanatképeket. hello alábbi példában egy előre meghatározott blob és hívások hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag toolist hello pillanatképek készítése, hogy a blob.  

```powershell
#Define hello blob name.
$BlobName = "yourblobname"

#List hello snapshots of a blob.
Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }
```

### <a name="how-toocopy-a-snapshot-of-a-blob"></a>Hogyan toocopy blob pillanatképe
A blob toorestore hello pillanatfelvétel pillanatkép másolhatja. Részletes információk és korlátozások: [egy pillanatképet készíteni egy Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx). hello alábbi példa először beállítja a változókat a tárfiók, és létrehoz egy adattároló-környezet. A következő hello példa hello tároló és a blob változók határozza meg. hello példa kéri le egy blobhivatkozást, hello segítségével [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag és futtatása hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metódus toocreate pillanatképet. Ezt követően hello példa fut hello [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) parancsmag toocopy hello pillanatképe a hello ICloudBlob objektummal hello forrás BLOB blob. Lehet, hogy tooupdate hello változók a konfiguráció alapján futó hello példa előtt. Vegye figyelembe, hogy a következő példa hello feltételezi, hogy hello forrás és cél tárolók, és hello forrás blob már létezik.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Define hello variables.
$SrcContainerName = "yoursourcecontainername"
$DestContainerName = "yourdestcontainername"
$SrcBlobName = "yourblobname"
$DestBlobName = "CopyBlobName"

#Get a reference tooa blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

#Create a snapshot of a blob.
$snap = $blob.ICloudBlob.CreateSnapshot()

#Copy hello snapshot tooanother container.
Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName
```

Most, hogy megtanulhatta, hogyan toomanage Azure-blobok és az Azure PowerShell pillanatképek blob, nyissa meg toohello következő szakasz toolearn hogyan toomanage táblák, várólisták és fájlokat.

## <a name="how-toomanage-azure-tables-and-table-entities"></a>Hogyan toomanage Azure táblázatok és a tábla az entitások
Az Azure Table storage szolgáltatás egy NoSQL-adattár, amelynek segítségével toostore és lekérdezés hatalmas strukturált, nem relációs adatokat. hello hello szolgáltatás fő összetevőit táblák, entitásokat és tulajdonságok. A tábla az entitások gyűjteményét. Egy entitás tulajdonságait. Minden entitás legfeljebb too252 tulajdonságait, amelyek minden név-érték párok tartalmazhat. Ez a szakasz feltételezi, hogy Ön már ismeri a hello Azure Table Storage szolgáltatással kapcsolatos fogalmak. Részletes információkért lásd: [ismertetése hello tábla szolgáltatás adatmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx) és [Ismerkedés az Azure Table storage használatának .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md).

A következő alszakaszokat hello megtudhatja, hogyan toomanage Azure Table storage szolgáltatást Azure PowerShell használatával. hello tárgyalt forgatókönyvekben szerepel a **létrehozása**, **törlése**, és **beolvasása** **táblák**, valamint **hozzáadása**, **lekérdezése**, és **tábla entitások törlése**.

### <a name="how-toocreate-a-table"></a>Hogyan toocreate tábla
Minden táblának kell lennie az Azure storage-fiók. hello következő példa bemutatja, hogyan toocreate Azure Storage-táblázathoz. hello példa először hoz létre a kapcsolat tooAzure tárolási hello tárfiók környezetét, beleértve hello a tárfiók neve vagy a hozzáférési kulcs használatával. Hello használja a következő [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) parancsmag toocreate Azure Storage-táblázathoz.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

#Create a new table.
$tabName = "yourtablename"
New-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-tooretrieve-a-table"></a>Hogyan tooretrieve tábla
Lekérdezi, és egy vagy az összes tábla tárfiókokban beolvasása. hello következő példa bemutatja, hogyan egy adott táblához használatával tooretrieve hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag.

```powershell
#Retrieve a table.
$tabName = "yourtablename"
Get-AzureStorageTable –Name $tabName –Context $Ctx
```

Meghívja a hello Get-AzureStorageTable parancsmag paraméterek nélkül, ha a tárfiók lekérdezi összes storage-táblákat.

### <a name="how-toodelete-a-table"></a>Hogyan toodelete tábla
Törölheti a tábla a tárfiókból hello segítségével [Remove-AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) parancsmag.  

```powershell
#Delete a table.
$tabName = "yourtablename"
Remove-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-toomanage-table-entities"></a>Hogyan toomanage tábla entitások
Jelenleg az Azure PowerShell nem biztosít parancsmagok toomanage táblaentitásokat közvetlenül. táblaentitásokat tooperform műveleteket, használhatja hello megadott hello osztályok [Azure Storage ügyféloldali kódtára a .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).

#### <a name="how-tooadd-table-entities"></a>Hogyan tooadd tábla entitások
egy entitás tooa tábla tooadd először létre kell hoznia egy objektum, amely meghatározza az entitás tulajdonságait. Egy entitás legfeljebb too255 tulajdonságokat, beleértve a 3 rendszertulajdonsággal tartalmazhat: **PartitionKey**, **RowKey**, és **időbélyeg**. Ön felelőssége lesz beszúrva, és hello értékek frissítése **PartitionKey** és **RowKey**. hello kiszolgáló kezeli hello értékének **időbélyeg**, amely nem módosítható. Együtt hello **PartitionKey** és **RowKey** egyedi módon azonosítja az egy táblázaton belüli összes entitás.

* **PartitionKey**: hello partíció hello entitás tárolt határozza meg.
* **RowKey**: hello entitás hello partíción belül egyedi azonosítására szolgál.

Be too252 egyéni tulajdonságait, hogy egy entitás adhatók meg. További információkért lásd: [ismertetése hello tábla szolgáltatás adatmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).

hello következő példa bemutatja, hogyan tooadd entitások tooa tábla. hello példa bemutatja, hogyan tooretrieve hello alkalmazott tábla, és vegyen fel több entitás. Először hoz létre a kapcsolat tooAzure tárolási hello tárfiók környezetét, beleértve hello a tárfiók neve vagy a hozzáférési kulcs használatával. Ezt követően lekérdezi a megadott tábla hello hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag. Ha hello tábla nem létezik, hello [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) parancsmag használt toocreate Azure Storage-táblázathoz. A következő hello példa minden entitás partíció- és sorkulcsa megadásával határozza meg Add-entitás tooadd entitások toohello tábla egyéni függvény. Add-entitás hello függvény hívások hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) parancsmagot a hello [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) osztály toocreate entitásobjektumra. Később, a hello példa meghívja a hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) ez entity objektum tooadd metódust azt tooa tábla.

```powershell
#Function Add-Entity: Adds an employee entity tooa table.
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

#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Retrieve hello table if it already exists.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

#Create a new table if it does not exist.
if ($table -eq $null)
{
   $table = New-AzureStorageTable –Name $TableName -Context $Ctx
}

#Add multiple entities tooa table.
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4
```

#### <a name="how-tooquery-table-entities"></a>Hogyan tooquery tábla entitások
egy tábla tooquery hello használata [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) osztály. hello alábbi példa feltételezi, hogy már futtatását hello hogyan megadott hello parancsfájl tooadd entitások című szakaszában talál. hello példa először hoz létre a kapcsolat tooAzure tárolási hello tárolási adataival, ide tartozik az hello tárfiók neve és a hozzáférési kulcsot. Ezt követően próbálja meg tooretrieve korábban létrehozott hello "alkalmazottak" tábla hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag. Hívása hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) parancsmagot a hello Microsoft.WindowsAzure.Storage.Table.TableQuery osztály új objektumot hoz létre lekérdezést. hello például egy "ID" oszlop, amelynek értéke 1 megadott egy karakterlánc-szűrővel rendelkező hello entitásokat keresi. Részletes információkért lásd: [lekérdezése táblákat és entitásokat](http://msdn.microsoft.com/library/azure/dd894031.aspx). Ez a lekérdezés futtatásakor hello szűrési feltételeknek megfelelő összes entitásokat ad vissza.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Get a reference tooa table.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx

#Create a table query.
$query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

#Define columns tooselect.
$list = New-Object System.Collections.Generic.List[string]
$list.Add("RowKey")
$list.Add("ID")
$list.Add("Name")

#Set query details.
$query.FilterString = "ID gt 0"
$query.SelectColumns = $list
$query.TakeCount = 20

#Execute hello query.
$entities = $table.CloudTable.ExecuteQuery($query)

#Display entity properties with hello table format.
$entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize
```

#### <a name="how-toodelete-table-entities"></a>Hogyan toodelete tábla entitások
A partíció- és sorfejlécek kulcsokkal entitás törölheti. hello alábbi példa feltételezi, hogy már futtatását hello hogyan megadott hello parancsfájl tooadd entitások című szakaszában talál. hello példa először hoz létre a kapcsolat tooAzure tárolási hello tárolási adataival, ide tartozik az hello tárfiók neve és a hozzáférési kulcsot. Ezt követően próbálja meg tooretrieve korábban létrehozott hello "alkalmazottak" tábla hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag. Ha hello tábla létezik, hello példa meghívja a hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) metódus tooretrieve entitás a partíció- és sorfejlécek értékek alapján. Így továbbítsa hello entitás toohello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) metódus toodelete.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve hello table.
$TableName = "Employees"
$table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

#If hello table exists, start deleting its entities.
if ($table -ne $null) 
{
    #Together hello PartitionKey and RowKey uniquely identify every  
    #entity within a table.
    $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
    $entity = $tableResult.Result
    if ($entity -ne $null)
    {
        #Delete hello entity.
        $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
}
```

## <a name="how-toomanage-azure-queues-and-queue-messages"></a>Hogyan toomanage Azure várólisták és üzenetek várólistára
Az Azure Queue storage egy olyan szolgáltatás, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLT használ, hitelesített hívásokon keresztül hello world üzenetek nagy számban tárolásához. Ez a szakasz feltételezi, hogy Ön már ismeri a hello Azure Queue Storage szolgáltatás fogalmakat. Részletes információkért lásd: [Ismerkedés az Azure Queue storage használatának .NET](../storage-dotnet-how-to-use-queues.md).

Ez a szakasz bemutatja, hogyan toomanage Azure Queue storage szolgáltatás Azure PowerShell használatával. hello tárgyalt forgatókönyvekben szerepel a **beszúrása** és **törlése** üzenetek, várólista, valamint **létrehozása**, **törlése**, és **sorok beolvasása**.

### <a name="how-toocreate-a-queue"></a>Hogyan toocreate várólista
hello alábbi példa először hoz létre a kapcsolat tooAzure tárolási hello tárfiók környezetét, beleértve hello a tárfiók neve vagy a hozzáférési kulcs használatával. Ezután meghívja [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) parancsmag toocreate "queuename" nevű várólista.

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$QueueName = "queuename"
$Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx
```

Azure Queue szolgáltatás elnevezési konvencióira vonatkozó információkért lásd: [elnevezési üzenetsorok és metaadatok](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-tooretrieve-a-queue"></a>Hogyan tooretrieve várólista
Lekérdezi, és egy konkrét várólistába helyezi vagy a tárfiókban lévő összes hello várólisták listájának beolvasása. hello következő példa bemutatja, hogy a megadott várólista használatával tooretrieve hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) parancsmag.

```powershell
#Retrieve a queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx
```

Ha meghívja a hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) parancsmag-paraméterek nélkül, akkor minden hello várólista listájának lekérése.

### <a name="how-toodelete-a-queue"></a>Hogyan toodelete várólista
toodelete várólista és köszönőüzenetei minden benne tárolt, hívás hello Remove-AzureStorageQueue parancsmag. hello a következő példa bemutatja, hogyan a megadott várólista használatával toodelete hello Remove-AzureStorageQueue parancsmag.

```powershell
#Delete a queue.
$QueueName = "yourqueuename"
Remove-AzureStorageQueue –Name $QueueName –Context $Ctx
```

#### <a name="how-tooinsert-a-message-into-a-queue"></a>Hogyan tooinsert egy várólistára üzenet
tooinsert egy létező üzenetsorba, az üzenet először létre kell hoznia egy új példányát hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) osztály. Ezután hívja hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) metódust. Egy CloudQueueMessage egy karakterláncból (UTF-8 formátumban), illetve egy bájttömböt hozhatók létre.

hello a következő példa bemutatja, hogyan tooadd üzenet-várólista tooa. hello példa először hoz létre a kapcsolat tooAzure tárolási hello tárfiók környezetét, beleértve hello a tárfiók neve vagy a hozzáférési kulcs használatával. Ezt követően lekérdezi a megadott várólista hello hello segítségével [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) parancsmag. Ha hello várólista létezik, hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) parancsmag használt toocreate hello példányának [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) osztály. Később, a hello példa meghívja a hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) ezen üzenet objektum tooadd metódust azt tooa várólista. Itt található kód, amely lekérdezi a várólista, és szúrja be a "MessageInfo" üdvözlőüzenetére:

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve hello queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

#If hello queue exists, add a new message.
if ($Queue -ne $null) {
   # Create a new message using a constructor of hello CloudQueueMessage class.
   $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

   # Add a new message toohello queue.
   $Queue.CloudQueue.AddMessage($QueueMessage)
}
```

#### <a name="how-toode-queue-at-hello-next-message"></a>Hogyan toode-várólista hello a következő üzenet
A kód két lépésben távolít el egy üzenetet az üzenetsorból. Hello hívás esetén [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) metódus, a hibaüzenet hello tovább a sorhoz. Az üzenet **GetMessage** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik. toofinish eltávolításakor üdvözlőüzenetére hello üzenetsorból, meg kell is hívni hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) metódust. A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód meghibásodásakor tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kérheti le hello ugyanazt az üzenetet, és próbálkozzon újra. A kód hívások **DeleteMessage** jobb gombbal az üdvözlő üzenet feldolgozása után.

```powershell
# Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

# Retrieve hello queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

$InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Get hello message object from hello queue.
$QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
# Delete hello message.
$Queue.CloudQueue.DeleteMessage($QueueMessage)
```

## <a name="how-toomanage-azure-file-shares-and-files"></a>Hogyan toomanage az Azure file közösen használja, és a fájlok
Az Azure File storage közös tárterületet hello szabványos SMB protokollt használó alkalmazások számára biztosít. Microsoft Azure virtuális gépek és felhőszolgáltatások megosztott csatlakoztatott megosztásokon keresztül alkalmazások összetevői között, és a helyszíni alkalmazások hozzáférhetnek a fájladatok olyan megosztáson található hello File storage API- vagy Azure PowerShell használatával.

További információ az Azure File storage: [Ismerkedés az Azure File storage on Windows](../storage-dotnet-how-to-use-files.md) és [File szolgáltatás REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-tooset-and-query-storage-analytics"></a>Hogyan tooset és lekérdezés tárolási analitika
Használhat [Azure Storage Analytics](../storage-analytics.md) toocollect metrikák az Azure storage-fiókok és a naplóadatok kérelmekkel kapcsolatos küldött tooyour tárfiók. Tárolási metrikák toomonitor hello állapotát egy tárfiók és tároló naplózási toodiagnose használja, és a problémák elhárítása a tárfiók. Használatával figyelését be tudja állítani az Azure portál vagy a Windows PowerShell hello, vagy programozott módon hello a storage ügyféloldali kódtára. Tárolási naplózási kiszolgálóoldali történik, és lehetővé teszi a sikeres és a sikertelen kérelmek a tárfiókban lévő toorecord részleteit. Ezek a naplók engedélyezése olvasási, írási és törlési műveleteket a táblák, üzenetsorok, blobok, valamint hello okok miatt a sikertelen kérelmek toosee részleteit.

Hogyan powershellel, tooenable és nézet Storage Metrics data: toolearn [hogyan tooenable Storage Metrics PowerShell-lel](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Hogyan powershellel, tooenable és lekérése tárolási naplózási adatait: toolearn [hogyan tooenable tároló-naplózás PowerShell-lel](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) és [keresése a naplózás tárolási naplóadatok](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
A Storage Metrics és tootroubleshoot tárolási problémák naplózási tároló részletes információkért lásd: [figyelés felismerése és hibaelhárítása a Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-toomanage-shared-access-signature-sas-and-stored-access-policy"></a>Hogyan toomanage közös hozzáférésű Jogosultságkód (SAS) és a hozzáférési házirendben tárolt
Közös hozzáférésű jogosultságkód hello biztonsági modellt az Azure Storage használó alkalmazások fontos részét képezik. Azok a megtekinthetik a korlátozott engedélyekkel tooyour tárolási fiók tooclients, amely nem rendelkezhet hello kulcsára. Alapértelmezés szerint csak hello tulajdonosa hello tárfiók elérhessék blobot, táblát és üzenetsort, hogy a fiókon belül. Ha a szolgáltatás vagy alkalmazás toomake ezen erőforrások elérhető tooother ügyfelek a hozzáférési kulcs megosztása nélkül, három lehetőség közül választhat:

* Állítsa be a tároló engedélyeit toopermit névtelen olvasási hozzáférés toohello tároló és a benne található blobokat. Ez nem engedélyezett táblák vagy olyan várólisták esetében.
* Használjon egy közös hozzáférésű jogosultságkódot, amely engedélyezi a korlátozott hozzáférésű jogok toocontainers, blobokat, üzenetsorokat és táblákat egy adott időtartam alatt.
* A tárolt hozzáférési házirend tooobtain egy közös hozzáférésű jogosultságkód szabályozhatják további szintet használja, egy tárolót vagy a benne található blobokat, egy sor vagy tábla. hello tárolt házirend lehetővé teszi toochange hello kezdési ideje, a lejárat időpontjának vagy az engedélyek az aláírás, vagy toorevoke azt követően, hogy ki van állítva.

A közös hozzáférésű jogosultságkód lehet két űrlap egyikében:

* **Ad hoc biztonsági Társítások**: egy alkalmi SAS, a hello kezdési ideje, a lejárat időpontjának létrehozásakor, és a hello SAS engedélyeinek összes adhatók meg hello SAS URI-t. Az ilyen típusú SAS lehet létrehozni egy tárolót, a blob, tábla, vagy várólista, és nem visszavonható.
* **SAS tárolt hozzáférési házirenddel**: A tárolt házirend van definiálva az erőforrás-tárolónak egy blob-tároló, táblázat vagy várólista - és használhatja azt toomanage megkötések legalább egy közös hozzáférésű jogosultságkódokat. Ha SAS-kód társítja a tárolt házirend, hello SAS hello korlátozásokat örököl - hello start idő, a lejárati idő és a tárolt hello hozzáférési házirend definiált - engedélyek. Ez a biztonsági Társítások típus visszavonható.

További információkért lásd: [használatával megosztott hozzáférési aláírásokkal (SAS)](../storage-dotnet-shared-access-signature-part-1.md) és [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](../blobs/storage-manage-access-to-resources.md).

A következő szakaszokban hello megtudhatja, hogyan toocreate egy megosztott hozzáférési aláírást token és tárolt hozzáférési házirend Azure-táblákban. Az Azure PowerShell hasonló parancsmagokat tárolók, blobok és várólisták is kínál. Ebben a szakaszban toorun hello parancsfájlok letöltése hello [Azure PowerShell verziója 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) vagy újabb.

### <a name="how-toocreate-a-policy-based-shared-access-signature-token"></a>Hogyan csoportházirend-alapú toocreate közös hozzáférésű Jogosultságkód jogkivonat
Hello New-AzureStorageTableStoredAccessPolicy parancsmag toocreate egy új tárolt házirend használja. Majd, meghívják a hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) parancsmag toocreate egy új csoportházirend-alapú megosztott aláírás jogkivonatot egy Azure Storage tábla.

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx
```

### <a name="how-toocreate-an-ad-hoc-non-revocable-shared-access-signature-token"></a>Hogyan toocreate egy ad hoc (nem visszavonható) közös hozzáférésű Jogosultságkód-jogkivonatot
Használjon hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) parancsmag toocreate új alkalmi (nem visszavonható) közös hozzáférésű Jogosultságkód tokent egy Azure Storage-tábla a(z):

```powershell
New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx
```
    
### <a name="how-toocreate-a-stored-access-policy"></a>Hogyan toocreate a tárolt házirend
Használja az Azure Storage tábla hello New-AzureStorageTableStoredAccessPolicy parancsmag toocreate egy új tárolt házirend:

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
```
    
### <a name="how-tooupdate-a-stored-access-policy"></a>Hogyan tooupdate a tárolt házirend
Egy Azure Storage tábla hello Set-AzureStorageTableStoredAccessPolicy parancsmag tooupdate egy meglévő tárolt hozzáférési házirendet használja:

```powershell
Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx
```

### <a name="how-toodelete-a-stored-access-policy"></a>Hogyan toodelete a tárolt házirend
Használja a Remove-AzureStorageTableStoredAccessPolicy hello parancsmag toodelete egy tárolt házirend egy Azure Storage táblán:

```powershell
Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx
```

## <a name="how-toouse-azure-storage-for-us-government-and-azure-china"></a>Hogyan toouse Azure Storage az Amerikai Egyesült Államok kormánya és Azure Kínában
Környezetet az Azure például egy Microsoft Azure-ban független telepítését [Azure Government az Amerikai Egyesült Államok kormánya](https://azure.microsoft.com/features/gov/), [globális Azure AzureCloud](https://portal.azure.com), és [AzureChinaCloud Azure Kínában a 21Vianet által működtetett](http://www.windowsazure.cn/). Új Azure-környezetek Egyesült kormányzati és Azure Kína telepítése.

Azure Storage a AzureChinaCloud toouse, kell toocreate AzureChinaCloud társított adattároló-környezet. Kövesse a lépéseket tooget használatba:

1. Futtassa a hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) parancsmag toosee hello érhető el az Azure-környezetek:
   
    ```powershell
    Get-AzureEnvironment
    ```

2. Egy Azure Kína fiók tooWindows PowerShell hozzáadása:
   
    ```powershell
    Add-AzureAccount –Environment AzureChinaCloud
    ```

3. Adattároló-környezet AzureChinaCloud fiók létrehozása:
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud
    ```

Azure Storage toouse rendelkező [USA Az Azure Government](https://azure.microsoft.com/features/gov/), érdemes egy új környezet, és ezután hozzon létre egy új adattároló-környezet ebben a környezetben:

1. Futtassa a hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) parancsmag toosee hello érhető el az Azure-környezetek:

    ```powershell
    Get-AzureEnvironment
    ```

2. Egy Azure Amerikai Egyesült államokbeli kormányzati fiók tooWindows PowerShell hozzáadása:
   
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
Az útmutatóban, hogy megismerte hogyan toomanage Azure Storage Azure PowerShell használatával. Íme, néhány kapcsolódó cikkek és a velük kapcsolatos további források.

* [Az Azure Storage dokumentációja](https://azure.microsoft.com/documentation/services/storage/)
* [Az Azure Storage PowerShell-parancsmagok](/powershell/module/azurerm.storage/#storage)
* [A Windows PowerShell-referencia](https://msdn.microsoft.com/library/ms714469.aspx)

[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How toomanage storage accounts in Azure]: #manageaccount
[How tooset a default Azure subscription]: #setdefsub
[How toocreate a new Azure storage account]: #createaccount
[How tooset a default Azure storage account]: #defaultaccount
[How toolist all Azure storage accounts in a subscription]: #listaccounts
[How toocreate an Azure storage context]: #createctx
[How toomanage Azure blobs and blob snapshots]: #manageblobs
[How toocreate a container]: #container
[How tooupload a blob into a container]: #uploadblob
[How toodownload blobs from a container]: #downblob
[How toocopy blobs from one storage container tooanother]: #copyblob
[How toodelete a blob]: #deleteblob
[How toomanage Azure blob snapshots]: #manageshots
[How toocreate a blob snapshot]: #createshot
[How toolist snapshots of a blob]: #listshot
[How toocopy a snapshot of a blob]: #copyshot
[How toomanage Azure tables and table entities]: #managetables
[How toocreate a table]: #createtable
[How tooretrieve a table]: #gettable
[How toodelete a table]: #remtable
[How toomanage table entities]: #mngentity
[How tooadd table entities]: #addentity
[How tooquery table entities]: #queryentity
[How toodelete table entities]: #deleteentity
[How toomanage Azure queues and queue messages]: #managequeues
[How toocreate a queue]: #createqueue
[How tooretrieve a queue]: #getqueue
[How toodelete a queue]: #remqueue
[How toomanage queue messages]: #mngqueuemsg
[How tooinsert a message into a queue]: #addqueuemsg
[How toode-queue at hello next message]: #dequeuemsg
[How toomanage Azure file shares and files]: #files
[How tooset and query storage analytics]: #stganalytics
[How toomanage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How toouse Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
