---
title: "aaaAzure Key Vault naplózása |} Microsoft Docs"
description: "Használja az Azure Key Vault használatának első lépéseit az oktatóanyag toohelp naplózása."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 43f96a2b-3af8-4adc-9344-bc6041fface8
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: cabailey
ms.openlocfilehash: 38a173297948748bef45e3d857c06b50b3e21e74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-logging"></a><span data-ttu-id="2227d-103">Az Azure Key Vault naplózása</span><span class="sxs-lookup"><span data-stu-id="2227d-103">Azure Key Vault Logging</span></span>
<span data-ttu-id="2227d-104">Az Azure Key Vault a legtöbb régióban elérhető.</span><span class="sxs-lookup"><span data-stu-id="2227d-104">Azure Key Vault is available in most regions.</span></span> <span data-ttu-id="2227d-105">További információkért lásd: hello [Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="2227d-105">For more information, see hello [Key Vault pricing page](https://azure.microsoft.com/pricing/details/key-vault/).</span></span>

## <a name="introduction"></a><span data-ttu-id="2227d-106">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="2227d-106">Introduction</span></span>
<span data-ttu-id="2227d-107">Miután létrehozott egy vagy több kulcstároló naplófájljainak, érdemes toomonitor használt, és ki hogyan és mikor történjen a kulcs-tárolók.</span><span class="sxs-lookup"><span data-stu-id="2227d-107">After you have created one or more key vaults, you will likely want toomonitor how and when your key vaults are accessed, and by whom.</span></span> <span data-ttu-id="2227d-108">Ehhez engedélyezze a Key Vault naplózását, amely egy Ön által megadott Azure-tárfiókba menti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="2227d-108">You can do this by enabling logging for Key Vault, which saves information in an Azure storage account that you provide.</span></span> <span data-ttu-id="2227d-109">A megadott tárfiókhoz automatikusan létrehozunk egy **insights-logs-auditevent** nevű tárolót, amelyet több kulcstároló naplófájljainak tárolására is használhat.</span><span class="sxs-lookup"><span data-stu-id="2227d-109">A new container named **insights-logs-auditevent** is automatically created for your specified storage account, and you can use this same storage account for collecting logs for multiple key vaults.</span></span>

<span data-ttu-id="2227d-110">Elérheti a naplóinformációkat legfeljebb, hello kulcs követő 10 percen kulcstároló műveletei.</span><span class="sxs-lookup"><span data-stu-id="2227d-110">You can access your logging information at most, 10 minutes after hello key vault operation.</span></span> <span data-ttu-id="2227d-111">A legtöbb esetben azonban ez nem fog ennyi ideig tartani.</span><span class="sxs-lookup"><span data-stu-id="2227d-111">In most cases, it will be quicker than this.</span></span>  <span data-ttu-id="2227d-112">Hogy működik-e tooyou toomanage a tárfiók naplófájljait:</span><span class="sxs-lookup"><span data-stu-id="2227d-112">It's up tooyou toomanage your logs in your storage account:</span></span>

* <span data-ttu-id="2227d-113">A naplók az Azure szabványos hozzáférés-vezérlési módszerek toosecure használatára korlátozza, hogy ki férhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="2227d-113">Use standard Azure access control methods toosecure your logs by restricting who can access them.</span></span>
* <span data-ttu-id="2227d-114">Törölje a megjeleníteni nem kívánt tookeep a tárfiókban lévő naplókat.</span><span class="sxs-lookup"><span data-stu-id="2227d-114">Delete logs that you no longer want tookeep in your storage account.</span></span>

<span data-ttu-id="2227d-115">Az Azure Key Vault naplózása, toocreate használatának első lépéseit az oktatóanyag toohelp használja a tárfiók naplózását, és hello összegyűjtött naplóinformációk értelmezésével.</span><span class="sxs-lookup"><span data-stu-id="2227d-115">Use this tutorial toohelp you get started with Azure Key Vault logging, toocreate your storage account, enable logging, and interpret hello logging information collected.</span></span>  

> [!NOTE]
> <span data-ttu-id="2227d-116">Ez az oktatóanyag nem tartalmazza a hogyan toocreate tárolók, a kulcsok vagy titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="2227d-116">This tutorial does not include instructions for how toocreate key vaults, keys, or secrets.</span></span> <span data-ttu-id="2227d-117">Ezekről a [Get started with Azure Key Vault](key-vault-get-started.md) (Bevezetés az Azure Key Vault használatába) című cikkben találhat információt.</span><span class="sxs-lookup"><span data-stu-id="2227d-117">For this information, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span> <span data-ttu-id="2227d-118">A platformfüggetlen parancssori felületre vonatkozó utasításokat megtekintheti [ebben a megfelelő oktatóanyagban](key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="2227d-118">Or, for Cross-Platform Command-Line Interface instructions, see [this equivalent tutorial](key-vault-manage-with-cli2.md).</span></span>
>
> <span data-ttu-id="2227d-119">Jelenleg nem konfigurálhatja az Azure Key Vault hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2227d-119">Currently, you cannot configure Azure Key Vault in hello Azure portal.</span></span> <span data-ttu-id="2227d-120">Ehelyett kövesse ezeket az Azure PowerShell-utasításokat.</span><span class="sxs-lookup"><span data-stu-id="2227d-120">Instead, use these Azure PowerShell instructions.</span></span>
>
>

<span data-ttu-id="2227d-121">Áttekintést az Azure Key Vaultról a [What is Azure Key Vault?](key-vault-whatis.md) (Mi az az Azure Key Vault?) című cikkben találhat.</span><span class="sxs-lookup"><span data-stu-id="2227d-121">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2227d-122">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2227d-122">Prerequisites</span></span>
<span data-ttu-id="2227d-123">toocomplete ebben az oktatóanyagban rendelkeznie kell a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2227d-123">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="2227d-124">Egy meglévő kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="2227d-124">An existing key vault that you have been using.</span></span>  
* <span data-ttu-id="2227d-125">Az Azure PowerShell **legalább 1.0.1-es verziója**.</span><span class="sxs-lookup"><span data-stu-id="2227d-125">Azure PowerShell, **minimum version of 1.0.1**.</span></span> <span data-ttu-id="2227d-126">Azure PowerShell tooinstall és rendelje hozzá azt az Azure-előfizetéssel, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2227d-126">tooinstall Azure PowerShell and associate it with your Azure subscription, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="2227d-127">Ha már telepítette az Azure PowerShell, és nem tudja hello verzió hello Azure PowerShell-konzolon, írja be a `(Get-Module azure -ListAvailable).Version`.</span><span class="sxs-lookup"><span data-stu-id="2227d-127">If you have already installed Azure PowerShell and do not know hello version, from hello Azure PowerShell console, type `(Get-Module azure -ListAvailable).Version`.</span></span>  
* <span data-ttu-id="2227d-128">A Key Vault naplóihoz elegendő tárhely az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2227d-128">Sufficient storage on Azure for your Key Vault logs.</span></span>

## <span data-ttu-id="2227d-129"><a id="connect"></a>Csatlakozás tooyour előfizetések</span><span class="sxs-lookup"><span data-stu-id="2227d-129"><a id="connect"></a>Connect tooyour subscriptions</span></span>
<span data-ttu-id="2227d-130">Indítson el egy Azure PowerShell-munkamenetet, és jelentkezzen be Azure-fiók tooyour hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2227d-130">Start an Azure PowerShell session and sign in tooyour Azure account with hello following command:</span></span>  

    Login-AzureRmAccount

<span data-ttu-id="2227d-131">Hello előugró böngészőablakban adja meg a Azure-fiók felhasználói nevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="2227d-131">In hello pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="2227d-132">Az Azure PowerShell beolvassa hello előfizetéseket, amelyek társítva ezzel a fiókkal, és alapértelmezés szerint, használja az elsőt hello.</span><span class="sxs-lookup"><span data-stu-id="2227d-132">Azure PowerShell will get all hello subscriptions that are associated with this account and by default, uses hello first one.</span></span>

<span data-ttu-id="2227d-133">Ha több előfizetéssel rendelkezik, lehetséges, hogy toospecify, de a használt toocreate melyiket az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2227d-133">If you have multiple subscriptions, you might have toospecify a specific one that was used toocreate your Azure Key Vault.</span></span> <span data-ttu-id="2227d-134">Írja be a fiókhoz toosee hello előfizetések a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2227d-134">Type hello following toosee hello subscriptions for your account:</span></span>

    Get-AzureRmSubscription

<span data-ttu-id="2227d-135">Ezt követően toospecify hello előfizetés, amely a kulcstartót akkor lesz naplózás, típusa van társítva:</span><span class="sxs-lookup"><span data-stu-id="2227d-135">Then, toospecify hello subscription that's associated with your key vault you will be logging, type:</span></span>

    Set-AzureRmContext -SubscriptionId <subscription ID>

> [!NOTE]
> <span data-ttu-id="2227d-136">Ez egy nagyon fontos lépés, és különösen hasznosnak bizonyulhat, ha több előfizetés tartozik a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="2227d-136">This is an important step and especially helpful if you have multiple subscriptions associated with your account.</span></span> <span data-ttu-id="2227d-137">Egy hiba tooregister Microsoft.Insights is megjelenhet, ha ez a lépés kimarad.</span><span class="sxs-lookup"><span data-stu-id="2227d-137">You may receive an error tooregister Microsoft.Insights if this step is skipped.</span></span>
>   
>

<span data-ttu-id="2227d-138">Azure PowerShell konfigurálásával kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2227d-138">For more information about configuring Azure PowerShell, see  [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <span data-ttu-id="2227d-139"><a id="storage"></a>Új tárfiók létrehozása a naplóknak</span><span class="sxs-lookup"><span data-stu-id="2227d-139"><a id="storage"></a>Create a new storage account for your logs</span></span>
<span data-ttu-id="2227d-140">Bár a naplók használhat meglévő tárfiókot, mi létrehozunk egy új tárfiókot, amely dedikált tooKey Vault naplóinak lesz.</span><span class="sxs-lookup"><span data-stu-id="2227d-140">Although you can use an existing storage account for your logs, we'll create a new storage account that will be dedicated tooKey Vault logs.</span></span> <span data-ttu-id="2227d-141">Kényelmi kell toospecify ezt később, azt fogja tárolni hello részletek egy változóba nevű **sa**.</span><span class="sxs-lookup"><span data-stu-id="2227d-141">For convenience for when we have toospecify this later, we'll store hello details into a variable named **sa**.</span></span>

<span data-ttu-id="2227d-142">Az egyszerű, a is használjuk hello ugyanabban az erőforráscsoportban, mint a kulcstároló tartalmazó hello.</span><span class="sxs-lookup"><span data-stu-id="2227d-142">For additional ease of management, we'll also use hello same resource group as hello one that contains our key vault.</span></span> <span data-ttu-id="2227d-143">A hello [használatába bevezető oktatóanyagot](key-vault-get-started.md), ez az erőforráscsoport neve **ContosoResourceGroup** és toouse hello Kelet-Ázsia helyét is.</span><span class="sxs-lookup"><span data-stu-id="2227d-143">From hello [getting started tutorial](key-vault-get-started.md), this resource group is named **ContosoResourceGroup** and we'll continue toouse hello East Asia location.</span></span> <span data-ttu-id="2227d-144">Az alábbi értékeket helyettesítse a sajátjainak megfelelőkkel:</span><span class="sxs-lookup"><span data-stu-id="2227d-144">Substitute these values for your own, as applicable:</span></span>

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name contosokeyvaultlogs -Type Standard_LRS -Location 'East Asia'


> [!NOTE]
> <span data-ttu-id="2227d-145">Ha úgy dönt, hogy a meglévő tárfiók toouse, azt kell használnia hello ugyanahhoz az előfizetéshez, mint a kulcstartót és hello Resource Manager üzembe helyezési modellben, nem pedig hello klasszikus telepítési modellt kell használnia.</span><span class="sxs-lookup"><span data-stu-id="2227d-145">If you decide toouse an existing storage account, it must use hello same subscription as your key vault and it must use hello Resource Manager deployment model, rather than hello Classic deployment model.</span></span>
>
>

## <span data-ttu-id="2227d-146"><a id="identify"></a>Hello a naplók kulcstárolójának azonosítása</span><span class="sxs-lookup"><span data-stu-id="2227d-146"><a id="identify"></a>Identify hello key vault for your logs</span></span>
<span data-ttu-id="2227d-147">Az oktatóanyagban a kulcstároló neve lett **ContosoKeyVault**, így az is, amelyek neve és a tárolás a hello részletek nevű változó toouse **kv**:</span><span class="sxs-lookup"><span data-stu-id="2227d-147">In our getting started tutorial, our key vault name was **ContosoKeyVault**, so we'll continue toouse that name and store hello details into a variable named **kv**:</span></span>

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <span data-ttu-id="2227d-148"><a id="enable"></a>Naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2227d-148"><a id="enable"></a>Enable logging</span></span>
<span data-ttu-id="2227d-149">tooenable Key Vault naplózását, hello Set-AzureRmDiagnosticSetting parancsmaggal fogjuk használni, az új tárfiók és a key vault létrehozott hello változók együtt.</span><span class="sxs-lookup"><span data-stu-id="2227d-149">tooenable logging for Key Vault, we'll use hello Set-AzureRmDiagnosticSetting cmdlet, together with hello variables we created for our new storage account and our key vault.</span></span> <span data-ttu-id="2227d-150">Hello is be **-engedélyezve** túl jelzőt**$true** hello kategória tooAuditEvent (hello egyetlen kategóriája Key Vault naplózása), és:</span><span class="sxs-lookup"><span data-stu-id="2227d-150">We'll also set hello **-Enabled** flag too**$true** and set hello category tooAuditEvent (hello only category for Key Vault logging), :</span></span>

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

<span data-ttu-id="2227d-151">hello kimenet tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="2227d-151">hello output for this includes:</span></span>

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


<span data-ttu-id="2227d-152">Ez megerősíti, hogy naplózás engedélyezve van a kulcstartót információk tooyour storage-fiók mentése.</span><span class="sxs-lookup"><span data-stu-id="2227d-152">This confirms that logging is now enabled for your key vault, saving information tooyour storage account.</span></span>

<span data-ttu-id="2227d-153">Opcionálisan beállíthat egy megtartási házirendet a naplóihoz, így a régebbi naplófájlok automatikusan törlődni fognak.</span><span class="sxs-lookup"><span data-stu-id="2227d-153">Optionally you can also set retention policy for your logs such that older logs will be automatically deleted.</span></span> <span data-ttu-id="2227d-154">Például a megőrzési házirend használatával be **- RetentionEnabled** túl jelzőt**$true** és **- RetentionInDays** paraméter túl**90** , hogy a naplófájlok 90 napnál régebbi automatikusan törölve lesznek.</span><span class="sxs-lookup"><span data-stu-id="2227d-154">For example, set retention policy using **-RetentionEnabled** flag too**$true** and set **-RetentionInDays** parameter too**90** so that logs older than 90 days will be automatically deleted.</span></span>

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

<span data-ttu-id="2227d-155">Mi kerül naplózásra?</span><span class="sxs-lookup"><span data-stu-id="2227d-155">What's logged:</span></span>

* <span data-ttu-id="2227d-156">Minden hitelesített REST API-kérés naplózásra kerül, beleértve a hozzáférési engedélyekből, rendszerhibákból vagy hibás kérésekből adótó sikertelen kérelmeket is.</span><span class="sxs-lookup"><span data-stu-id="2227d-156">All authenticated REST API requests are logged, which includes failed requests as a result of access permissions, system errors or bad requests.</span></span>
* <span data-ttu-id="2227d-157">Műveletek hello kulcsot tároló magát, beleértve a létrehozás, törlés, beállítás kulcstároló hozzáférési szabályzatainak, és címkéket, mint a kulcstároló tulajdonságainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="2227d-157">Operations on hello key vault itself, which includes creation, deletion, setting key vault access policies, and updating key vault attributes such as tags.</span></span>
* <span data-ttu-id="2227d-158">Műveletek a kulcsok és titkos hello a key vaultban, amely tartalmazza a létrehozása, módosítása vagy törlése a kulcsok vagy titkos; Műveletek, például a bejelentkezési, győződjön meg arról, titkosítás, visszafejtése, burkolja és kulcsok kicsomagolásához, titkos kulcsok, listában kulcsok és titkos kulcsok és a verzióját.</span><span class="sxs-lookup"><span data-stu-id="2227d-158">Operations on keys and secrets in hello key vault, which includes creating, modifying, or deleting these keys or secrets; operations such as sign, verify, encrypt, decrypt, wrap and unwrap keys, get secrets, list keys and secrets and their versions.</span></span>
* <span data-ttu-id="2227d-159">A 401-es választ eredményező, nem hitelesített kérelmek.</span><span class="sxs-lookup"><span data-stu-id="2227d-159">Unauthenticated requests that result in a 401 response.</span></span> <span data-ttu-id="2227d-160">Ilyenek például azok a kérelmek, amelyek nem rendelkeznek tulajdonosi jogkivonattal, helytelen formátumúak vagy lejártak, vagy érvénytelen a jogkivonatuk.</span><span class="sxs-lookup"><span data-stu-id="2227d-160">For example, requests that do not have a bearer token, or are malformed or expired, or have an invalid token.</span></span>  

## <span data-ttu-id="2227d-161"><a id="access"></a>A naplók elérése</span><span class="sxs-lookup"><span data-stu-id="2227d-161"><a id="access"></a>Access your logs</span></span>
<span data-ttu-id="2227d-162">Kulcstároló naplófájljainak tárolása a hello **insights-logs-auditevent** hello storage-fiók a megadott tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="2227d-162">Key vault logs are stored in hello **insights-logs-auditevent** container in hello storage account you provided.</span></span> <span data-ttu-id="2227d-163">toolist valamennyi hello BLOB a tárolóban található írja be:</span><span class="sxs-lookup"><span data-stu-id="2227d-163">toolist all hello blobs in this container, type:</span></span>

<span data-ttu-id="2227d-164">Először hozzon létre egy változót hello tároló neve.</span><span class="sxs-lookup"><span data-stu-id="2227d-164">First, create a variable for hello container name.</span></span> <span data-ttu-id="2227d-165">Ez fogja használni a teljes hello lépésein végighaladva hello többi részétől.</span><span class="sxs-lookup"><span data-stu-id="2227d-165">This will be used throughout hello rest of hello walk through.</span></span>

    $container = 'insights-logs-auditevent'

<span data-ttu-id="2227d-166">toolist valamennyi hello BLOB a tárolóban található írja be:</span><span class="sxs-lookup"><span data-stu-id="2227d-166">toolist all hello blobs in this container, type:</span></span>

    Get-AzureStorageBlob -Container $container -Context $sa.Context
<span data-ttu-id="2227d-167">hello kimeneti valami hasonló toothis fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="2227d-167">hello output will look something similar toothis:</span></span>

<span data-ttu-id="2227d-168">**Tároló URI-ja: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**</span><span class="sxs-lookup"><span data-stu-id="2227d-168">**Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**</span></span>

<span data-ttu-id="2227d-169">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="2227d-169">**Name**</span></span>

- - -
<span data-ttu-id="2227d-170">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**</span><span class="sxs-lookup"><span data-stu-id="2227d-170">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**</span></span>

<span data-ttu-id="2227d-171">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**</span><span class="sxs-lookup"><span data-stu-id="2227d-171">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**</span></span>

<span data-ttu-id="2227d-172">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json****</span><span class="sxs-lookup"><span data-stu-id="2227d-172">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json****</span></span>

<span data-ttu-id="2227d-173">Mivel a kimenetből látható, hello blobok egy elnevezési konvenciója: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/fájlnév.JSON**</span><span class="sxs-lookup"><span data-stu-id="2227d-173">As you can see from this output, hello blobs follow a naming convention: **resourceId=<ARM resource ID>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json**</span></span>

<span data-ttu-id="2227d-174">hello dátum- és időértékek az UTC használata.</span><span class="sxs-lookup"><span data-stu-id="2227d-174">hello date and time values use UTC.</span></span>

<span data-ttu-id="2227d-175">Mivel a hello ugyanazt a tárfiókot több erőforrást használt toocollect naplókat, hello hello blob nevének teljes erőforrás-azonosító nem tooaccess nagyon hasznos vagy szükséges letöltési csak hello blobokat.</span><span class="sxs-lookup"><span data-stu-id="2227d-175">Because hello same storage account can be used toocollect logs for multiple resources, hello full resource ID in hello blob name is very useful tooaccess or download just hello blobs that you need.</span></span> <span data-ttu-id="2227d-176">De előtte még a nem, azt először érintünk hogyan toodownload összes hello blobokat.</span><span class="sxs-lookup"><span data-stu-id="2227d-176">But before we do that, we'll first cover how toodownload all hello blobs.</span></span>

<span data-ttu-id="2227d-177">Először hozzon létre egy mappát toodownload hello blobokat.</span><span class="sxs-lookup"><span data-stu-id="2227d-177">First, create a folder toodownload hello blobs.</span></span> <span data-ttu-id="2227d-178">Példa:</span><span class="sxs-lookup"><span data-stu-id="2227d-178">For example:</span></span>

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

<span data-ttu-id="2227d-179">Majd kérje le az összes blob listáját:</span><span class="sxs-lookup"><span data-stu-id="2227d-179">Then get a list of all blobs:</span></span>  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

<span data-ttu-id="2227d-180">Ebben a listában, a "Get-AzureStorageBlobContent" toodownload hello blobok átadhatja a célként megadott mappába:</span><span class="sxs-lookup"><span data-stu-id="2227d-180">Pipe this list through 'Get-AzureStorageBlobContent' toodownload hello blobs into our destination folder:</span></span>

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

<span data-ttu-id="2227d-181">A második parancs futtatásakor hello  **/**  hello blob nevének határolója hozzon létre egy teljes mapparendszert hello célmappát, és ez a struktúra lesz használt toodownload tároló hello blobok és -fájlok formájában.</span><span class="sxs-lookup"><span data-stu-id="2227d-181">When you run this second command, hello **/** delimiter in hello blob names create a full folder structure under hello destination folder, and this structure will be used toodownload and store hello blobs as files.</span></span>

<span data-ttu-id="2227d-182">tooselectively blobok letöltéséhez használjon helyettesítő karaktereket.</span><span class="sxs-lookup"><span data-stu-id="2227d-182">tooselectively download blobs, use wildcards.</span></span> <span data-ttu-id="2227d-183">Példa:</span><span class="sxs-lookup"><span data-stu-id="2227d-183">For example:</span></span>

* <span data-ttu-id="2227d-184">Ha több kulcstárolóval rendelkezik, és toodownload naplók csak egy kulcstartót, csak a contosokeyvault3 nevűhöz szeretne:</span><span class="sxs-lookup"><span data-stu-id="2227d-184">If you have multiple key vaults and want toodownload logs for just one key vault, named CONTOSOKEYVAULT3:</span></span>

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
* <span data-ttu-id="2227d-185">Ha több erőforráscsoportban, és szeretné toodownload naplók csak egy erőforráscsoport, `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:</span><span class="sxs-lookup"><span data-stu-id="2227d-185">If you have multiple resource groups and want toodownload logs for just one resource group, use `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:</span></span>

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
* <span data-ttu-id="2227d-186">Programmal toodownload összes hello napló 2016. január hello hónap, `-Blob '*/year=2016/m=01/*'`:</span><span class="sxs-lookup"><span data-stu-id="2227d-186">If you want toodownload all hello logs for hello month of January 2016, use `-Blob '*/year=2016/m=01/*'`:</span></span>

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

<span data-ttu-id="2227d-187">Most, hogy most már készen áll a hello megnézi toostart naplózza.</span><span class="sxs-lookup"><span data-stu-id="2227d-187">You're now ready toostart looking at what's in hello logs.</span></span> <span data-ttu-id="2227d-188">Mielőtt ezt a két paramétert a Get-azurermdiagnosticsetting parancshoz, hogy szükség lehet a tooknow azonban:</span><span class="sxs-lookup"><span data-stu-id="2227d-188">But before moving onto that, two more parameters for Get-AzureRmDiagnosticSetting that you might need tooknow:</span></span>

* <span data-ttu-id="2227d-189">a kulcstároló erőforrásához tartozó diagnosztikai beállítások tooquery hello állapota:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`</span><span class="sxs-lookup"><span data-stu-id="2227d-189">tooquery hello  status of diagnostic settings for your key vault resource: `Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`</span></span>
* <span data-ttu-id="2227d-190">a kulcstároló erőforrásához toodisable naplózás:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`</span><span class="sxs-lookup"><span data-stu-id="2227d-190">toodisable logging for your key vault resource: `Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`</span></span>

## <span data-ttu-id="2227d-191"><a id="interpret"></a>A Key Vault naplóinak értelmezése</span><span class="sxs-lookup"><span data-stu-id="2227d-191"><a id="interpret"></a>Interpret your Key Vault logs</span></span>
<span data-ttu-id="2227d-192">Az egyes blobok JSON-blobként, szöveges formában vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="2227d-192">Individual blobs are stored as text, formatted as a JSON blob.</span></span> <span data-ttu-id="2227d-193">Íme egy példa a `Get-AzureRmKeyVault -VaultName 'contosokeyvault'` parancsot futtató naplóbejegyzésre:</span><span class="sxs-lookup"><span data-stu-id="2227d-193">This is an example log entry from running `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:</span></span>

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


<span data-ttu-id="2227d-194">hello következő táblázatban hello mezők neveit és leírásait.</span><span class="sxs-lookup"><span data-stu-id="2227d-194">hello following table lists hello field names and descriptions.</span></span>

| <span data-ttu-id="2227d-195">Mező neve</span><span class="sxs-lookup"><span data-stu-id="2227d-195">Field name</span></span> | <span data-ttu-id="2227d-196">Leírás</span><span class="sxs-lookup"><span data-stu-id="2227d-196">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2227d-197">time</span><span class="sxs-lookup"><span data-stu-id="2227d-197">time</span></span> |<span data-ttu-id="2227d-198">Dátum és idő (UTC).</span><span class="sxs-lookup"><span data-stu-id="2227d-198">Date and time (UTC).</span></span> |
| <span data-ttu-id="2227d-199">resourceId</span><span class="sxs-lookup"><span data-stu-id="2227d-199">resourceId</span></span> |<span data-ttu-id="2227d-200">Az Azure Resource Manager szerinti erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="2227d-200">Azure Resource Manager Resource ID.</span></span> <span data-ttu-id="2227d-201">A Key Vault-naplók esetében mindig ez hello Key Vault erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="2227d-201">For Key Vault logs, this is always hello  Key Vault resource ID.</span></span> |
| <span data-ttu-id="2227d-202">operationName</span><span class="sxs-lookup"><span data-stu-id="2227d-202">operationName</span></span> |<span data-ttu-id="2227d-203">Hello művelet, a következő tábla hello ismertetett neve.</span><span class="sxs-lookup"><span data-stu-id="2227d-203">Name of hello operation, as documented in hello next table.</span></span> |
| <span data-ttu-id="2227d-204">operationVersion</span><span class="sxs-lookup"><span data-stu-id="2227d-204">operationVersion</span></span> |<span data-ttu-id="2227d-205">Ez a hello hello ügyfél által kért REST API-verzió.</span><span class="sxs-lookup"><span data-stu-id="2227d-205">This is hello REST API version requested by hello client.</span></span> |
| <span data-ttu-id="2227d-206">category</span><span class="sxs-lookup"><span data-stu-id="2227d-206">category</span></span> |<span data-ttu-id="2227d-207">A Key Vault-naplók AuditEvent érték hello egyetlen, érhető el.</span><span class="sxs-lookup"><span data-stu-id="2227d-207">For Key Vault logs, AuditEvent is hello single, available value.</span></span> |
| <span data-ttu-id="2227d-208">resultType</span><span class="sxs-lookup"><span data-stu-id="2227d-208">resultType</span></span> |<span data-ttu-id="2227d-209">A REST API-kérelem eredménye.</span><span class="sxs-lookup"><span data-stu-id="2227d-209">Result of REST API request.</span></span> |
| <span data-ttu-id="2227d-210">resultSignature</span><span class="sxs-lookup"><span data-stu-id="2227d-210">resultSignature</span></span> |<span data-ttu-id="2227d-211">A HTTP-állapot.</span><span class="sxs-lookup"><span data-stu-id="2227d-211">HTTP status.</span></span> |
| <span data-ttu-id="2227d-212">resultDescription</span><span class="sxs-lookup"><span data-stu-id="2227d-212">resultDescription</span></span> |<span data-ttu-id="2227d-213">Hello eredmény, ha elérhető további leírása.</span><span class="sxs-lookup"><span data-stu-id="2227d-213">Additional description about hello result, when available.</span></span> |
| <span data-ttu-id="2227d-214">durationMs</span><span class="sxs-lookup"><span data-stu-id="2227d-214">durationMs</span></span> |<span data-ttu-id="2227d-215">Idő, ezredmásodpercben megadva tooservice hello REST API-kérelem.</span><span class="sxs-lookup"><span data-stu-id="2227d-215">Time it took tooservice hello REST API request, in milliseconds.</span></span> <span data-ttu-id="2227d-216">Hello hálózati késés, ez nem vonatkozik, ezért mérni a hello ügyféloldalon hello idő nem egyeznek meg a megadott idő.</span><span class="sxs-lookup"><span data-stu-id="2227d-216">This does not include hello network latency, so hello time you measure on hello client side might not match this time.</span></span> |
| <span data-ttu-id="2227d-217">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="2227d-217">callerIpAddress</span></span> |<span data-ttu-id="2227d-218">Hello kérelmet leadó hello ügyfél IP-címe.</span><span class="sxs-lookup"><span data-stu-id="2227d-218">IP address of hello client who made hello request.</span></span> |
| <span data-ttu-id="2227d-219">correlationId</span><span class="sxs-lookup"><span data-stu-id="2227d-219">correlationId</span></span> |<span data-ttu-id="2227d-220">Egy nem kötelező GUID, amely az ügyfél hello teljen toocorrelate ügyféloldali naplók és a Szolgáltatásoldali (Key Vault) naplók.</span><span class="sxs-lookup"><span data-stu-id="2227d-220">An optional GUID that hello client can pass toocorrelate client-side logs with service-side (Key Vault) logs.</span></span> |
| <span data-ttu-id="2227d-221">identity</span><span class="sxs-lookup"><span data-stu-id="2227d-221">identity</span></span> |<span data-ttu-id="2227d-222">Hello tokenben hello REST API-kérelem meghozásakor szereplő identitás.</span><span class="sxs-lookup"><span data-stu-id="2227d-222">Identity from hello token that was presented when making hello REST API request.</span></span> <span data-ttu-id="2227d-223">Ez általában egy „felhasználó”, „egyszerű szolgáltatásnév” vagy „felhasználó + alkalmazás-azonosító”, az Azure PowerShell-parancsmagok által eredményezett kérelmekhez hasonlóan.</span><span class="sxs-lookup"><span data-stu-id="2227d-223">This is usually a "user", a "service principal" or a combination "user+appId" as in case of a request resulting from a Azure PowerShell cmdlet.</span></span> |
| <span data-ttu-id="2227d-224">properties</span><span class="sxs-lookup"><span data-stu-id="2227d-224">properties</span></span> |<span data-ttu-id="2227d-225">Ez a mező alapján hello művelettől (operationName) kapcsolatos különféle információk fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="2227d-225">This field will contain different information based on hello operation (operationName).</span></span> <span data-ttu-id="2227d-226">A legtöbb esetben ügyféladatokat (hello sztringjét hello ügyfél), tartalmaz pontos REST API-kérelem URI-azonosítója és a HTTP-állapotkód: hello.</span><span class="sxs-lookup"><span data-stu-id="2227d-226">In most cases, contains client information (hello useragent string passed by hello client), hello exact REST API request URI, and HTTP status code.</span></span> <span data-ttu-id="2227d-227">Továbbá amikor egy kérelem (például a KeyCreate vagy a VaultGet) eredményeként ad vissza egy objektumot is tartalmaz hello kulcs URI-JÁT ("id"), a tároló URI vagy a titkos kulcs URI.</span><span class="sxs-lookup"><span data-stu-id="2227d-227">In addition, when an object is returned as a result of a request (for example, KeyCreate or VaultGet) it will also contain hello Key URI (as "id"), Vault URI, or Secret URI.</span></span> |

<span data-ttu-id="2227d-228">Hello **operationName** mező értékei ObjectVerb formátumúak.</span><span class="sxs-lookup"><span data-stu-id="2227d-228">hello **operationName** field values are in ObjectVerb format.</span></span> <span data-ttu-id="2227d-229">Példa:</span><span class="sxs-lookup"><span data-stu-id="2227d-229">For example:</span></span>

* <span data-ttu-id="2227d-230">Minden kulcstárolón elvégzett művelet hello "tárolóban`<action>`" formátumú, például `VaultGet` és `VaultCreate`.</span><span class="sxs-lookup"><span data-stu-id="2227d-230">All key vault operations have hello 'Vault`<action>`' format, such as `VaultGet` and `VaultCreate`.</span></span>
* <span data-ttu-id="2227d-231">Minden kulcson elvégzett művelet hello "kulcs`<action>`" formátumú, például `KeySign` és `KeyList`.</span><span class="sxs-lookup"><span data-stu-id="2227d-231">All  key operations have hello 'Key`<action>`' format, such as `KeySign` and `KeyList`.</span></span>
* <span data-ttu-id="2227d-232">Minden titkos kulcson elvégzett művelet hello "Secret`<action>`" formátumú, például `SecretGet` és `SecretListVersions`.</span><span class="sxs-lookup"><span data-stu-id="2227d-232">All secret operations have hello 'Secret`<action>`' format, such as `SecretGet` and `SecretListVersions`.</span></span>

<span data-ttu-id="2227d-233">hello a következő táblázat felsorolja a hello operationname műveleteket és a megfelelő REST API-parancsot.</span><span class="sxs-lookup"><span data-stu-id="2227d-233">hello following table lists hello operationName and corresponding REST API command.</span></span>

| <span data-ttu-id="2227d-234">operationName</span><span class="sxs-lookup"><span data-stu-id="2227d-234">operationName</span></span> | <span data-ttu-id="2227d-235">REST API-parancs</span><span class="sxs-lookup"><span data-stu-id="2227d-235">REST API Command</span></span> |
| --- | --- |
| <span data-ttu-id="2227d-236">Authentication</span><span class="sxs-lookup"><span data-stu-id="2227d-236">Authentication</span></span> |<span data-ttu-id="2227d-237">Az Azure Active Directory végpontján keresztül</span><span class="sxs-lookup"><span data-stu-id="2227d-237">Via Azure Active Directory endpoint</span></span> |
| <span data-ttu-id="2227d-238">VaultGet</span><span class="sxs-lookup"><span data-stu-id="2227d-238">VaultGet</span></span> |[<span data-ttu-id="2227d-239">Kulcstároló adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="2227d-239">Get information about a key vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx) |
| <span data-ttu-id="2227d-240">VaultPut</span><span class="sxs-lookup"><span data-stu-id="2227d-240">VaultPut</span></span> |[<span data-ttu-id="2227d-241">Kulcstároló létrehozása vagy frissítése</span><span class="sxs-lookup"><span data-stu-id="2227d-241">Create or update a key vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx) |
| <span data-ttu-id="2227d-242">VaultDelete</span><span class="sxs-lookup"><span data-stu-id="2227d-242">VaultDelete</span></span> |[<span data-ttu-id="2227d-243">Kulcstároló törlése</span><span class="sxs-lookup"><span data-stu-id="2227d-243">Delete a key vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx) |
| <span data-ttu-id="2227d-244">VaultPatch</span><span class="sxs-lookup"><span data-stu-id="2227d-244">VaultPatch</span></span> |[<span data-ttu-id="2227d-245">Kulcstároló frissítése</span><span class="sxs-lookup"><span data-stu-id="2227d-245">Update a key vault</span></span>](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| <span data-ttu-id="2227d-246">VaultList</span><span class="sxs-lookup"><span data-stu-id="2227d-246">VaultList</span></span> |[<span data-ttu-id="2227d-247">Az erőforráscsoport összes kulcstárolójának listázása</span><span class="sxs-lookup"><span data-stu-id="2227d-247">List all key vaults in a resource group</span></span>](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx) |
| <span data-ttu-id="2227d-248">KeyCreate</span><span class="sxs-lookup"><span data-stu-id="2227d-248">KeyCreate</span></span> |[<span data-ttu-id="2227d-249">Kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="2227d-249">Create a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx) |
| <span data-ttu-id="2227d-250">KeyGet</span><span class="sxs-lookup"><span data-stu-id="2227d-250">KeyGet</span></span> |[<span data-ttu-id="2227d-251">Kulcs adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="2227d-251">Get information about a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx) |
| <span data-ttu-id="2227d-252">KeyImport</span><span class="sxs-lookup"><span data-stu-id="2227d-252">KeyImport</span></span> |[<span data-ttu-id="2227d-253">Kulcs importálása egy tárolóba</span><span class="sxs-lookup"><span data-stu-id="2227d-253">Import a key into a vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx) |
| <span data-ttu-id="2227d-254">KeyBackup</span><span class="sxs-lookup"><span data-stu-id="2227d-254">KeyBackup</span></span> |<span data-ttu-id="2227d-255">[Kulcs biztonsági mentése](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).</span><span class="sxs-lookup"><span data-stu-id="2227d-255">[Backup a key](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).</span></span> |
| <span data-ttu-id="2227d-256">KeyDelete</span><span class="sxs-lookup"><span data-stu-id="2227d-256">KeyDelete</span></span> |[<span data-ttu-id="2227d-257">Kulcs törlése</span><span class="sxs-lookup"><span data-stu-id="2227d-257">Delete a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx) |
| <span data-ttu-id="2227d-258">KeyRestore</span><span class="sxs-lookup"><span data-stu-id="2227d-258">KeyRestore</span></span> |[<span data-ttu-id="2227d-259">Kulcs helyreállítása</span><span class="sxs-lookup"><span data-stu-id="2227d-259">Restore a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx) |
| <span data-ttu-id="2227d-260">KeySign</span><span class="sxs-lookup"><span data-stu-id="2227d-260">KeySign</span></span> |[<span data-ttu-id="2227d-261">Aláírás kulccsal</span><span class="sxs-lookup"><span data-stu-id="2227d-261">Sign with a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx) |
| <span data-ttu-id="2227d-262">KeyVerify</span><span class="sxs-lookup"><span data-stu-id="2227d-262">KeyVerify</span></span> |[<span data-ttu-id="2227d-263">Ellenőrzés kulccsal</span><span class="sxs-lookup"><span data-stu-id="2227d-263">Verify with a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx) |
| <span data-ttu-id="2227d-264">KeyWrap</span><span class="sxs-lookup"><span data-stu-id="2227d-264">KeyWrap</span></span> |[<span data-ttu-id="2227d-265">Kulcs becsomagolása</span><span class="sxs-lookup"><span data-stu-id="2227d-265">Wrap a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx) |
| <span data-ttu-id="2227d-266">KeyUnwrap</span><span class="sxs-lookup"><span data-stu-id="2227d-266">KeyUnwrap</span></span> |[<span data-ttu-id="2227d-267">Kulcs kicsomagolása</span><span class="sxs-lookup"><span data-stu-id="2227d-267">Unwrap a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx) |
| <span data-ttu-id="2227d-268">KeyEncrypt</span><span class="sxs-lookup"><span data-stu-id="2227d-268">KeyEncrypt</span></span> |[<span data-ttu-id="2227d-269">Titkosítás kulccsal</span><span class="sxs-lookup"><span data-stu-id="2227d-269">Encrypt with a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx) |
| <span data-ttu-id="2227d-270">KeyDecrypt</span><span class="sxs-lookup"><span data-stu-id="2227d-270">KeyDecrypt</span></span> |[<span data-ttu-id="2227d-271">Visszafejtés kulccsal</span><span class="sxs-lookup"><span data-stu-id="2227d-271">Decrypt with a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx) |
| <span data-ttu-id="2227d-272">KeyUpdate</span><span class="sxs-lookup"><span data-stu-id="2227d-272">KeyUpdate</span></span> |[<span data-ttu-id="2227d-273">Kulcs frissítése</span><span class="sxs-lookup"><span data-stu-id="2227d-273">Update a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx) |
| <span data-ttu-id="2227d-274">KeyList</span><span class="sxs-lookup"><span data-stu-id="2227d-274">KeyList</span></span> |[<span data-ttu-id="2227d-275">Egy tároló kulcsainak hello listája</span><span class="sxs-lookup"><span data-stu-id="2227d-275">List hello keys in a vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx) |
| <span data-ttu-id="2227d-276">KeyListVersions</span><span class="sxs-lookup"><span data-stu-id="2227d-276">KeyListVersions</span></span> |[<span data-ttu-id="2227d-277">A kulcs verzióinak listázása hello</span><span class="sxs-lookup"><span data-stu-id="2227d-277">List hello versions of a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx) |
| <span data-ttu-id="2227d-278">SecretSet</span><span class="sxs-lookup"><span data-stu-id="2227d-278">SecretSet</span></span> |[<span data-ttu-id="2227d-279">Titkos kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="2227d-279">Create a secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx) |
| <span data-ttu-id="2227d-280">SecretGet</span><span class="sxs-lookup"><span data-stu-id="2227d-280">SecretGet</span></span> |[<span data-ttu-id="2227d-281">Titkos kulcs lekérése</span><span class="sxs-lookup"><span data-stu-id="2227d-281">Get secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx) |
| <span data-ttu-id="2227d-282">SecretUpdate</span><span class="sxs-lookup"><span data-stu-id="2227d-282">SecretUpdate</span></span> |[<span data-ttu-id="2227d-283">Titkos kulcs frissítése</span><span class="sxs-lookup"><span data-stu-id="2227d-283">Update a secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx) |
| <span data-ttu-id="2227d-284">SecretDelete</span><span class="sxs-lookup"><span data-stu-id="2227d-284">SecretDelete</span></span> |[<span data-ttu-id="2227d-285">Titkos kulcs törlése</span><span class="sxs-lookup"><span data-stu-id="2227d-285">Delete a secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx) |
| <span data-ttu-id="2227d-286">SecretList</span><span class="sxs-lookup"><span data-stu-id="2227d-286">SecretList</span></span> |[<span data-ttu-id="2227d-287">Egy tároló titkos kulcsainak listázása</span><span class="sxs-lookup"><span data-stu-id="2227d-287">List secrets in a vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx) |
| <span data-ttu-id="2227d-288">SecretListVersions</span><span class="sxs-lookup"><span data-stu-id="2227d-288">SecretListVersions</span></span> |[<span data-ttu-id="2227d-289">Titkos kulcs verzióinak listázása</span><span class="sxs-lookup"><span data-stu-id="2227d-289">List versions of a secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx) |

## <span data-ttu-id="2227d-290"><a id="loganalytics"></a>A Log Analytics használata</span><span class="sxs-lookup"><span data-stu-id="2227d-290"><a id="loganalytics"></a>Use Log Analytics</span></span>

<span data-ttu-id="2227d-291">A Naplóelemzési tooreview naplózza az Azure Key Vault AuditEvent hello Azure Key Vault megoldás is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2227d-291">You can use hello Azure Key Vault solution in Log Analytics tooreview Azure Key Vault AuditEvent logs.</span></span> <span data-ttu-id="2227d-292">További információt, beleértve a hogyan tooset, ez: [Naplóelemzési megoldás az Azure Key Vault](../log-analytics/log-analytics-azure-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="2227d-292">For more information, including how tooset this up, see [Azure Key Vault solution in Log Analytics](../log-analytics/log-analytics-azure-key-vault.md).</span></span> <span data-ttu-id="2227d-293">A cikk utasításokat is tartalmaz, ha toomigrate hello régi Key Vault-megoldásból által kínált hello Naplóelemzési előzetes, amelyen először átirányítva a naplók tooan Azure Storage-fiók, és ott Naplóelemzési tooread konfigurálva kell.</span><span class="sxs-lookup"><span data-stu-id="2227d-293">This article also contains instructions if you need toomigrate from hello old Key Vault solution that was offered during hello Log Analytics preview, where you first routed your logs tooan Azure Storage account and configured Log Analytics tooread from there.</span></span>

## <span data-ttu-id="2227d-294"><a id="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2227d-294"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="2227d-295">Az Azure Key Vault webalkalmazásban való használatáról a [Use Azure Key Vault from a Web Application](key-vault-use-from-web-application.md) (Az Azure Key Vault webalkalmazással való használata) című témakörben találhat útmutatást.</span><span class="sxs-lookup"><span data-stu-id="2227d-295">For a tutorial that uses Azure Key Vault in a web application, see [Use Azure Key Vault from a Web Application](key-vault-use-from-web-application.md).</span></span>

<span data-ttu-id="2227d-296">Programozási hivatkozások: [hello Azure Key Vault fejlesztői útmutatója](key-vault-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="2227d-296">For programming references, see [hello Azure Key Vault developer's guide](key-vault-developers-guide.md).</span></span>

<span data-ttu-id="2227d-297">Az Azure Key Vaultra vonatkozó Azure PowerShell 1.0-parancsmagok listáját az [Azure Key Vault Cmdlets](/powershell/module/azurerm.keyvault/#key_vault) (Az Azure Key Vault parancsmagjai) című témakörben találja.</span><span class="sxs-lookup"><span data-stu-id="2227d-297">For a list of Azure PowerShell 1.0 cmdlets for Azure Key Vault, see [Azure Key Vault Cmdlets](/powershell/module/azurerm.keyvault/#key_vault).</span></span>

<span data-ttu-id="2227d-298">Kulcs rotációjával és az Azure Key Vault naplózása napló oktatóanyag, lásd: [hogyan end tooend a Key Vault toosetup kulcs elforgatás és naplózási](key-vault-key-rotation-log-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="2227d-298">For a tutorial on key rotation and log auditing with Azure Key Vault, see [How toosetup Key Vault with end tooend key rotation and auditing](key-vault-key-rotation-log-monitoring.md).</span></span>
