---
title: "az Azure PowerShell használatába aaaGet |} Microsoft Docs"
description: "Gyors bevezetés toohello toomanage kötegelt erőforrások használható Azure PowerShell-parancsmagokat."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: f9ad62c5-27bf-4e6b-a5bf-c5f5914e6199
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: powershell
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e4d12e9c1e52a5b2db2dd44346edda93b7ef92b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-resources-with-powershell-cmdlets"></a>Batch-erőforrások kezelése PowerShell-parancsmagokkal

Az Azure Batch PowerShell-parancsmagok hello, végrehajtása és a parancsfájl-hello számos ugyanazokhoz a feladatokhoz a kötegelt API-k, hello hajthat végre hello Azure-portálon, és hello Azure parancssori felület (CLI). Ez az egy gyors bevezetés toohello parancsmagokat használhatja toomanage a Batch-fiókok és a a kötegelt erőforrásokat, például a készletek, a feladatok és a feladatokat.

Kötegelt parancsmagok és részletes parancsmag szintaxisát teljes listáját lásd: hello [Azure Batch parancsmag-referencia](/powershell/module/azurerm.batch/#batch).

Ez a cikk az Azure PowerShell 3.0.0-s verziójának parancsmagjain alapul. Azt javasoljuk, hogy a frissíteni az Azure PowerShell gyakran tootake előnyeit szolgáltatás frissítéseket és fejlesztéseket.

## <a name="prerequisites"></a>Előfeltételek
Hajtsa végre a következő műveletek toouse Azure PowerShell toomanage hello a kötegelt erőforrásokhoz.

* [Telepítse és konfigurálja az Azure PowerShellt](/powershell/azure/overview)
* Futtassa a hello **Login-AzureRmAccount** parancsmag tooconnect tooyour előfizetés (hello Azure Batch parancsmagok szállítási hello Azure Resource Manager modul):
  
    `Login-AzureRmAccount`
* **Hello kötegelt szolgáltatójának névtere regisztrálása**. Ez a művelet csak kell végrehajtani toobe **előfizetésenként egyszer**.
  
    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Batch-fiókok és kulcsok felügyelete
### <a name="create-a-batch-account"></a>Batch-fiók létrehozása
A **New-AzureRmBatchAccount** parancs egy Batch-fiókot hoz létre a meghatározott erőforráscsoportban. Ha még nem rendelkezik egy erőforráscsoport, hozzon létre egyet hello futtatásával [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsmag. Adjon meg egy hello Azure régiói hello **hely** paraméter, például az "USA középső RÉGIÓJA". Példa:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Ezt követően hello erőforráscsoportban hello fióknak a nevét adja meg a Batch-fiók létrehozása <*fióknév*> és hello helyét, és az erőforráscsoport nevét. Hello Batch-fiók létrehozása eltarthat néhány alkalommal toocomplete. Példa:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [!NOTE]
> hello Batch-fiók nevének kell lennie az Azure-régió hello erőforráscsoport, egyedi toohello csak 3 és 24 karakter közötti lehet, és kisbetűket és számokat tartalmazhat csak.
> 
> 

### <a name="get-account-access-keys"></a>Fiók hívóbetűinek beszerzése
**Get-AzureRmBatchAccountKeys** Azure Batch-fiók társított hello hívóbetűk jeleníti meg. Például futtassa a következő tooget hello elsődleges és másodlagos létrehozott hello fiókjának kulcsok hello.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Új hívóbetű létrehozása
A **New-AzureRmBatchAccountKey** új elsődleges vagy másodlagos hívóbetűt hoz létre az Azure Batch-fiókhoz. Például toogenerate egy új elsődleges kulcsot a Batch-fiókhoz, típus:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [!NOTE]
> egy új másodlagos kulcs toogenerate "Másodlagos" megadása hello **KeyType** paraméter. Külön-külön kell tooregenerate hello elsődleges és másodlagos kulcsok.
> 
> 

### <a name="delete-a-batch-account"></a>Batch-fiók törlése
A **Remove-AzureRmBatchAccount** törli a Batch-fiókot. Példa:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Amikor a rendszer kéri, erősítse meg tooremove hello fiók. Fiók eltávolításának is igénybe vehet néhány alkalommal toocomplete.

## <a name="create-a-batchaccountcontext-object"></a>BatchAccountContext objektum létrehozása
Kötegelt PowerShell-parancsmagok használatával tooauthenticate hello, létrehozása és kezelése a Batch-készletek, feladatok, feladatok, és más erőforrások, először létre kell hoznia egy BatchAccountContext objektum toostore a fiók nevét és a kulcsok:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Át hello BatchAccountContext objektum parancsmagok be, hogy használjon hello **BatchContext** paraméter.

> [!NOTE]
> Alapértelmezés szerint hello fiók elsődleges kulcsot a hitelesítéshez használja, de hello kulcs toouse explicit módon kiválaszthatja a BatchAccountContext objektum módosításával **KeyInUse** tulajdonság: `$context.KeyInUse = "Secondary"`.
> 
> 

## <a name="create-and-modify-batch-resources"></a>Batch-erőforrások létrehozása és módosítása
Parancsmagokat használja, mint **New-AzureBatchPool**, **New-AzureBatchJob**, és **New-AzureBatchTask** toocreate erőforrások a Batch-fiók. Megfelelő **Get -** és **Set -** parancsmagok tooupdate hello tulajdonságainak meglévő erőforrásokat, és **Remove -** parancsmagok tooremove erőforrások a Batch-fiók.

Ha ezek a parancsmagok számos a hozzáadása toopassing egy BatchContext objektum, meg kell toocreate, vagy adja át az objektumokat, amelyek részletes erőforrás-beállítások, ahogy az alábbi példa hello. Lásd: hello részletes minden további példákat a parancsmag súgóját.

### <a name="create-a-batch-pool"></a>Batch-készlet létrehozása
Létrehozásakor vagy frissítésekor. a Batch-készlet, akkor válasszon ki hello felhőalapú szolgáltatás konfigurációja vagy a virtuálisgép-konfiguráció hello hello operációs rendszer hello a számítási csomópontok (lásd: [Batch funkcióinak áttekintése](batch-api-basics.md#pool)). Hello felhőalapú szolgáltatás konfigurációja adja meg, ha a számítási csomópontok telepíti egy hello [Azure vendég operációs rendszer feloldja a](../cloud-services/cloud-services-guestos-update-matrix.md#releases). Ha hello virtuálisgép-konfiguráció ad meg, vagy megadhatja hello egyik támogatott Linux vagy a Windows virtuális gép lemezképek szerepel az hello [Azure virtuális gépek piactér][vm_marketplace], vagy adjon meg egy egyéni előkészítette a kép.

Amikor futtatja **New-AzureBatchPool**, adjon át egy PSCloudServiceConfiguration vagy PSVirtualMachineConfiguration objektum hello operációsrendszer-beállításokat. Például hello következő parancsmag hoz létre egy új Batch-készlet mérete kis számítási csomópontjai hello felhőalapú szolgáltatás konfigurációja, telepíti a legújabb operációs rendszer verziójával hello termékcsalád 3 (Windows Server 2012). Itt hello **CloudServiceConfiguration** paraméterrel hello *$configuration* változó hello PSCloudServiceConfiguration objektumként. Hello **BatchContext** paraméter adja meg egy korábban definiált változó *$context* hello BatchAccountContext objektumként.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

az automatikus skálázás képlet hello új készlet számítási csomópontjai hello cél számát határozza meg. Ebben az esetben hello képlete egyszerűen **$TargetDedicated = 4**, érték 4, legfeljebb hello hello készlet számítási csomópontjainak számát.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Készletek, feladatok, tevékenységek és egyéb részletek lekérdezése
Használjon például parancsmagok **Get-AzureBatchPool**, **Get-AzureBatchJob**, és **Get-AzureBatchTask** tooquery entitások Batch-fiók alatt hozott létre.

### <a name="query-for-data"></a>Adatok lekérdezése
Tegyük fel, használjon **Get-AzureBatchPools** toofind az készletek. Ez a fiók alatt készletekben lekérdezi alapértelmezés szerint tárolja hello BatchAccountContext objektum már feltéve, hogy *$context*:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>OData-szűrő használata
Megadhat egy OData-szűrő használatával hello **szűrő** paraméter toofind csak hello kíváncsiak vagyunk objektumok. Megkeresheti például az összes olyan készletet, amelynek az azonosítója a „myPool” karakterlánccal kezdődik:

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Ez a módszer nem olyan rugalmas, mint a „Where-Object” használata a helyi folyamatokban. Azonban hello lekérdezés küldi el toohello Batch szolgáltatás közvetlenül, hogy az összes szűrése történik, az internetes sávszélességet mentése hello kiszolgálóoldali.

### <a name="use-hello-id-parameter"></a>Hello Id paraméter használata
Egy alternatív tooan OData szűrő toouse hello **azonosító** paraméter. az azonosító "myPool" adott készlet tooquery:

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

Hello **azonosító** paraméter csak teljes-id keresési, nem a helyettesítő karakterekkel vagy OData-stílusú szűrők támogatja.

### <a name="use-hello-maxcount-parameter"></a>Hello MaxCount paraméter használata
Alapértelmezés szerint mindegyik parancsmag maximum 1000 objektumot ad vissza. Ha eléri ezt a határt, pontosítsa a szűrő toobring vissza kevesebb objektumot, vagy beállítással, hello használatával legfeljebb **MaxCount** paraméter. Példa:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

tooremove hello felső határa, állítsa be **MaxCount** too0 vagy kisebb.

### <a name="use-hello-powershell-pipeline"></a>Hello PowerShell kimenetátirányításának használata
Kötegelt parancsmagok kihasználhatják a hello PowerShell adatcsatorna toosend adatok parancsmag között. Ugyanaz, mintha egy paraméter, de használata több entitás egyszerűbbé teszi érvényesíti hello azt.

Például keresse meg és jelenítse meg fiókjában az összes feladatot:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Indítsa újra a készlet összes számítási csomópontját:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Alkalmazáscsomagok kezelése
Alkalmazáscsomagok lehetőséget nyújtanak olyan egyszerűsített toodeploy alkalmazások toohello számítási csomópontjain a készletek. A kötegelt PowerShell-parancsmagok hello feltöltése és alkalmazáscsomagok kezelése a Batch-fiókhoz, és telepítse csomag verziók toocompute csomópontokat.

Alkalmazás **létrehozása**:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

Alkalmazáscsomag **hozzáadása**:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Set hello **alapértelmezett verzió** hello alkalmazáshoz:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

Alkalmazás csomagjainak **listázása**

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

Alkalmazáscsomag **törlése**

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

Alkalmazás **törlése**

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

> [!NOTE]
> Előtt törölnie kell minden olyan alkalmazás alkalmazáscsomag-verziók hello alkalmazás törlése. Egy "Ütközés" hibaüzenetet fog kapni, ha egy alkalmazás jelenleg alkalmazáscsomagok toodelete kísérli meg.
> 
> 

### <a name="deploy-an-application-package"></a>Alkalmazáscsomag üzembe helyezése
Készlet létrehozásakor megadhat egy vagy több alkalmazáscsomagot az üzembe helyezéshez. Olyan csomagot adjon meg a készlet létrehozás időpontjában, esetén telepített tooeach csomópont hello csomópont illesztések készlet szerint. A csomagok akkor is üzembe lesznek helyezve, ha a csomópontot újraindítják vagy alaphelyzetbe állítják.

Adja meg a hello `-ApplicationPackageReference` létrehozásakor egy alkalmazáskészlet toodeploy egy csomag toohello alkalmazáskészlet csomópontok, azok csatlakoztatását hello készlet lehetőséget. Először hozzon létre egy **PSApplicationPackageReference** objektumot, és állítsa be úgy a hello csomagazonosító és csomagnév Alkalmazásverzió kívánt toodeploy toohello készlet számítási csomópontok:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Most hello készlet létrehozásához, és adja meg hello csomag referenciaobjektum, mivel argumentum toohello hello `ApplicationPackageReferences` lehetőséget:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

További információt az alkalmazáscsomagok a [központi telepítése az alkalmazások toocompute csomópontokat a kötegelt alkalmazáscsomagok](batch-application-packages.md).

> [!IMPORTANT]
> Meg kell [egy Azure Storage-fiók csatolása](#linked-storage-account-autostorage) tooyour Batch-fiók toouse alkalmazáscsomagok.
> 
> 

### <a name="update-a-pools-application-packages"></a>Készlet alkalmazáscsomagjainak frissítése
tooupdate hello alkalmazások kijelölt tooan meglévő készlet, először létre kell hoznia egy PSApplicationPackageReference objektum szükséges hello tulajdonságokkal (csomagazonosító és csomagnév Alkalmazásverzió):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

A következő hello készlet beolvasása kötegben, törölje ki a meglévő csomagokat, az új csomag hivatkozás hozzáadása és hello Batch szolgáltatás hello új alkalmazáskészlet beállításainak frissítése:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

Most már frissítette hello készlet tulajdonságok hello Batch szolgáltatásban. tooactually hello új alkalmazás csomag toocompute csomópontok hello készletben, a rendszer azonban kell indítani, vagy újból lemezképet létrehozni azokat a csomópontokat. Az alábbi paranccsal újraindíthatja a készlet összes számítási csomópontját:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

> [!TIP]
> Telepíthet több alkalmazás csomagok toohello számítási csomópont a készletben. Ha azt szeretné, túl*hozzáadása* jelenleg telepített hello csomagok felülírása helyett egy alkalmazáscsomagot, hagyja el hello `$pool.ApplicationPackageReferences.Clear()` fenti sor.
> 
> 

## <a name="next-steps"></a>Következő lépések
* A parancsmag részletes szintaxisáért és példákért lásd: [Azure Batch-parancsmagok referenciája](/powershell/module/azurerm.batch/#batch).
* Alkalmazások és az alkalmazáscsomagok kötegben kapcsolatos további információkért lásd: [központi telepítése az alkalmazások toocompute csomópontokat a kötegelt alkalmazáscsomagok](batch-application-packages.md).

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/