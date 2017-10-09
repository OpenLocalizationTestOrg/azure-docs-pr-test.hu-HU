---
title: "az Event Hubs használatával az Azure Key Vault aaaIntegrate naplók |} Microsoft Docs"
description: "Az oktatóanyag hello szükséges lépéseket toomake Key Vault biztosító elérhető tooa SIEM naplózza az Azure napló integrálása révén"
services: security
author: barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.topic: article
ms.date: 08/07/2017
ms.author: Barclayn
ms.custom: AzLog
ms.openlocfilehash: ada2fc846cc6bf09e12cc2c016815b27afef0d50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a><span data-ttu-id="94e5c-103">Azure napló integrációs Útmutató: az Event Hubs használatával folyamat az Azure Key Vault-események</span><span class="sxs-lookup"><span data-stu-id="94e5c-103">Azure Log Integration tutorial: Process Azure Key Vault events by using Event Hubs</span></span>

<span data-ttu-id="94e5c-104">Azure napló integrációs tooretrieve naplózott eseményeket használ, és tegye őket elérhetővé tooyour biztonsági információkat és az esemény (SIEM) rendszer.</span><span class="sxs-lookup"><span data-stu-id="94e5c-104">You can use Azure Log Integration tooretrieve logged events and make them available tooyour security information and event management (SIEM) system.</span></span> <span data-ttu-id="94e5c-105">Ez az oktatóanyag azt szemlélteti, hogyan Azure napló integrációs használt tooprocess naplók az Azure Event Hubs beszerzett lehet.</span><span class="sxs-lookup"><span data-stu-id="94e5c-105">This tutorial shows an example of how Azure Log Integration can be used tooprocess logs that are acquired through Azure Event Hubs.</span></span>
 
<span data-ttu-id="94e5c-106">Az oktatóanyag tooget szerezhessenek hogyan együtt által a következő Azure napló integráció és az Event Hubs munkahelyi hello példák azt ismertetik, és hogyan támogatja az egyes lépések a hello megoldás használata.</span><span class="sxs-lookup"><span data-stu-id="94e5c-106">Use this tutorial tooget acquainted with how Azure Log Integration and Event Hubs work together by following hello example steps and understanding how each step supports hello solution.</span></span> <span data-ttu-id="94e5c-107">Akkor is igénybe vehet néhány hogy megismerte itt toocreate saját lépéseket toosupport a vállalat egyedi követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="94e5c-107">Then you can take what you’ve learned here toocreate your own steps toosupport your company’s unique requirements.</span></span>

>[!WARNING]
<span data-ttu-id="94e5c-108">hello lépésekről és parancsokról ebben az oktatóanyagban nincsenek tervezett toobe másolni és beilleszteni.</span><span class="sxs-lookup"><span data-stu-id="94e5c-108">hello steps and commands in this tutorial are not intended toobe copied and pasted.</span></span> <span data-ttu-id="94e5c-109">Példák csak fontosságúak.</span><span class="sxs-lookup"><span data-stu-id="94e5c-109">They're examples only.</span></span> <span data-ttu-id="94e5c-110">Ne használja az "adott állapotban" hello PowerShell-parancsok élő környezetében.</span><span class="sxs-lookup"><span data-stu-id="94e5c-110">Do not use hello PowerShell commands “as is” in your live environment.</span></span> <span data-ttu-id="94e5c-111">Kell testre szabnia a saját egyedi környezete alapján.</span><span class="sxs-lookup"><span data-stu-id="94e5c-111">You must customize them based on your unique environment.</span></span>


<span data-ttu-id="94e5c-112">Ez az oktatóanyag bemutatja, hogyan hello folyamat véve az Azure Key Vault tevékenység naplózott tooan eseményközpont, és így JSON fájlok tooyour SIEM-rendszerben érhető el.</span><span class="sxs-lookup"><span data-stu-id="94e5c-112">This tutorial walks you through hello process of taking Azure Key Vault activity logged tooan event hub and making it available as JSON files tooyour SIEM system.</span></span> <span data-ttu-id="94e5c-113">Konfigurálhatja a SIEM rendszer tooprocess hello JSON-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="94e5c-113">You can then configure your SIEM system tooprocess hello JSON files.</span></span>

>[!NOTE]
><span data-ttu-id="94e5c-114">Ebben az oktatóanyagban hello lépéseket a legtöbb tartalmaz, amely kulcstárolót, storage-fiókok és az event hubs konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="94e5c-114">Most of hello steps in this tutorial involve configuring key vaults, storage accounts, and event hubs.</span></span> <span data-ttu-id="94e5c-115">hello adott Azure napló integrációs lépésekre hello Ez az oktatóanyag végén.</span><span class="sxs-lookup"><span data-stu-id="94e5c-115">hello specific Azure Log Integration steps are at hello end of this tutorial.</span></span> <span data-ttu-id="94e5c-116">Éles környezetben ne hajtsa végre ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="94e5c-116">Do not perform these steps in a production environment.</span></span> <span data-ttu-id="94e5c-117">Csak egy tesztkörnyezetet szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="94e5c-117">They are intended for a lab environment only.</span></span> <span data-ttu-id="94e5c-118">Az éles használat előtt testre kell szabnia hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="94e5c-118">You must customize hello steps before using them in production.</span></span>

<span data-ttu-id="94e5c-119">Megadva mentén hello módon segítséget nyújt az egyes lépések hello okaival tisztában.</span><span class="sxs-lookup"><span data-stu-id="94e5c-119">Information provided along hello way helps you understand hello reasons behind each step.</span></span> <span data-ttu-id="94e5c-120">Hivatkozások tooother cikkek nyújtanak további információkhoz juthat az egyes témakörök.</span><span class="sxs-lookup"><span data-stu-id="94e5c-120">Links tooother articles give you more detail on certain topics.</span></span>

<span data-ttu-id="94e5c-121">Ez az oktatóanyag említi hello szolgáltatások kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="94e5c-121">For more information about hello services that this tutorial mentions, see:</span></span> 

- [<span data-ttu-id="94e5c-122">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="94e5c-122">Azure Key Vault</span></span>](../key-vault/key-vault-whatis.md)
- [<span data-ttu-id="94e5c-123">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="94e5c-123">Azure Event Hubs</span></span>](../event-hubs/event-hubs-what-is-event-hubs.md)
- [<span data-ttu-id="94e5c-124">Azure Naplóelemzés integráció</span><span class="sxs-lookup"><span data-stu-id="94e5c-124">Azure Log Integration</span></span>](security-azure-log-integration-overview.md)


## <a name="initial-setup"></a><span data-ttu-id="94e5c-125">Kezdeti telepítés</span><span class="sxs-lookup"><span data-stu-id="94e5c-125">Initial setup</span></span>

<span data-ttu-id="94e5c-126">A cikkben ismertetett hello befejezése előtt hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="94e5c-126">Before you can complete hello steps in this article, you need hello following:</span></span>

1. <span data-ttu-id="94e5c-127">Azure-előfizetések és az adott előfizetéshez, rendszergazdai jogosultságokkal rendelkező fiókot.</span><span class="sxs-lookup"><span data-stu-id="94e5c-127">An Azure subscription and account on that subscription with administrator rights.</span></span> <span data-ttu-id="94e5c-128">Ha nem rendelkezik előfizetéssel, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="94e5c-128">If you don't have a subscription, you can create a [free account](https://azure.microsoft.com/free/).</span></span>
 
2. <span data-ttu-id="94e5c-129">A rendszer, amely hozzáférési toohello internet hello Azure napló integrációs telepítéséhez szükséges követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="94e5c-129">A system with access toohello internet that meets hello requirements for installing Azure Log Integration.</span></span> <span data-ttu-id="94e5c-130">hello rendszer lehet egy felhőalapú szolgáltatás, vagy helyben tárolt.</span><span class="sxs-lookup"><span data-stu-id="94e5c-130">hello system can be on a cloud service or hosted on-premises.</span></span>

3. <span data-ttu-id="94e5c-131">[Azure napló-integráció](https://www.microsoft.com/download/details.aspx?id=53324) telepítve.</span><span class="sxs-lookup"><span data-stu-id="94e5c-131">[Azure Log Integration](https://www.microsoft.com/download/details.aspx?id=53324) installed.</span></span> <span data-ttu-id="94e5c-132">tooinstall azt:</span><span class="sxs-lookup"><span data-stu-id="94e5c-132">tooinstall it:</span></span>

   <span data-ttu-id="94e5c-133">a.</span><span class="sxs-lookup"><span data-stu-id="94e5c-133">a.</span></span> <span data-ttu-id="94e5c-134">A távoli asztal tooconnect toohello rendszer a 2. lépésben említett használja.</span><span class="sxs-lookup"><span data-stu-id="94e5c-134">Use Remote Desktop tooconnect toohello system mentioned in step 2.</span></span>   
   <span data-ttu-id="94e5c-135">b.</span><span class="sxs-lookup"><span data-stu-id="94e5c-135">b.</span></span> <span data-ttu-id="94e5c-136">Hello Azure napló integrációs telepítő toohello rendszer másolja.</span><span class="sxs-lookup"><span data-stu-id="94e5c-136">Copy hello Azure Log Integration installer toohello system.</span></span> <span data-ttu-id="94e5c-137">Is [hello telepítési fájlok letöltési](https://www.microsoft.com/download/details.aspx?id=53324).</span><span class="sxs-lookup"><span data-stu-id="94e5c-137">You can [download hello installation files](https://www.microsoft.com/download/details.aspx?id=53324).</span></span>   
   <span data-ttu-id="94e5c-138">c.</span><span class="sxs-lookup"><span data-stu-id="94e5c-138">c.</span></span> <span data-ttu-id="94e5c-139">Hello a telepítőprogram indítása, és fogadja el a hello Microsoft szoftverlicenc-szerződést.</span><span class="sxs-lookup"><span data-stu-id="94e5c-139">Start hello installer and accept hello Microsoft Software License Terms.</span></span>   
   <span data-ttu-id="94e5c-140">d.</span><span class="sxs-lookup"><span data-stu-id="94e5c-140">d.</span></span> <span data-ttu-id="94e5c-141">Ha telemetriai tájékoztatást fogunk adni, hagyja bejelölve hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="94e5c-141">If you will provide telemetry information, leave hello check box selected.</span></span> <span data-ttu-id="94e5c-142">Ha nem ahelyett, hogy elküldése használati információk tooMicrosoft, törölje a hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="94e5c-142">If you'd rather not send usage information tooMicrosoft, clear hello check box.</span></span>
   
   <span data-ttu-id="94e5c-143">További információ az Azure napló integrációs és hogyan tooinstall, lásd: [Azure napló integráció az Azure diagnosztikai naplózás és a Windows-Eseménytovábbítást](security-azure-log-integration-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="94e5c-143">For more information about Azure Log Integration and how tooinstall it, see [Azure Log Integration with Azure Diagnostics logging and Windows Event Forwarding](security-azure-log-integration-get-started.md).</span></span>

4. <span data-ttu-id="94e5c-144">hello PowerShell letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="94e5c-144">hello latest PowerShell version.</span></span>
 
   <span data-ttu-id="94e5c-145">Ha a Windows Server 2016 telepítve van, akkor legalább rendelkezik PowerShell 5.0.</span><span class="sxs-lookup"><span data-stu-id="94e5c-145">If you have Windows Server 2016 installed, then you have at least PowerShell 5.0.</span></span> <span data-ttu-id="94e5c-146">A Windows Server bármely verziója használata, lehetséges, hogy egy korábbi telepített PowerShell.</span><span class="sxs-lookup"><span data-stu-id="94e5c-146">If you're using any other version of Windows Server, you might have an earlier version of PowerShell installed.</span></span> <span data-ttu-id="94e5c-147">Hello verzió beírásával ellenőrizheti ```get-host``` egy PowerShell-ablakban.</span><span class="sxs-lookup"><span data-stu-id="94e5c-147">You can check hello version by entering ```get-host``` in a PowerShell window.</span></span> <span data-ttu-id="94e5c-148">Ha nincs PowerShell 5.0 verziója, akkor [le is](https://www.microsoft.com/download/details.aspx?id=50395).</span><span class="sxs-lookup"><span data-stu-id="94e5c-148">If you don't have PowerShell 5.0 installed, you can [download it](https://www.microsoft.com/download/details.aspx?id=50395).</span></span>

   <span data-ttu-id="94e5c-149">Miután legalább PowerShell 5.0 tooinstall hello legújabb verzióra a Folytatás:</span><span class="sxs-lookup"><span data-stu-id="94e5c-149">After you have at least PowerShell 5.0, you can proceed tooinstall hello latest version:</span></span>
   
   <span data-ttu-id="94e5c-150">a.</span><span class="sxs-lookup"><span data-stu-id="94e5c-150">a.</span></span> <span data-ttu-id="94e5c-151">A PowerShell-ablakot, írja be a hello ```Install-Module Azure``` parancsot.</span><span class="sxs-lookup"><span data-stu-id="94e5c-151">In a PowerShell window, enter hello ```Install-Module Azure``` command.</span></span> <span data-ttu-id="94e5c-152">Hello telepítés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="94e5c-152">Complete hello installation steps.</span></span>    
   <span data-ttu-id="94e5c-153">b.</span><span class="sxs-lookup"><span data-stu-id="94e5c-153">b.</span></span> <span data-ttu-id="94e5c-154">Adja meg a hello ```Install-Module AzureRM``` parancsot.</span><span class="sxs-lookup"><span data-stu-id="94e5c-154">Enter hello ```Install-Module AzureRM``` command.</span></span> <span data-ttu-id="94e5c-155">Hello telepítés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="94e5c-155">Complete hello installation steps.</span></span>

   <span data-ttu-id="94e5c-156">További információkért lásd: [Azure PowerShell telepítése](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span><span class="sxs-lookup"><span data-stu-id="94e5c-156">For more information, see [Install Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span></span>


## <a name="create-supporting-infrastructure-elements"></a><span data-ttu-id="94e5c-157">Támogató infrastruktúra elemeinek létrehozása</span><span class="sxs-lookup"><span data-stu-id="94e5c-157">Create supporting infrastructure elements</span></span>

1. <span data-ttu-id="94e5c-158">Nyisson meg egy emelt szintű PowerShell ablakot, és nyissa meg túl**C:\Program Files\Microsoft Azure napló integrációs**.</span><span class="sxs-lookup"><span data-stu-id="94e5c-158">Open an elevated PowerShell window and go too**C:\Program Files\Microsoft Azure Log Integration**.</span></span>
2. <span data-ttu-id="94e5c-159">Importálja a hello AzLog parancsmagok LoadAzLogModule.ps1 hello parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="94e5c-159">Import hello AzLog cmdlets by running hello script LoadAzLogModule.ps1.</span></span> <span data-ttu-id="94e5c-160">Adja meg a hello `.\LoadAzLogModule.ps1` parancsot.</span><span class="sxs-lookup"><span data-stu-id="94e5c-160">Enter hello `.\LoadAzLogModule.ps1` command.</span></span> <span data-ttu-id="94e5c-161">(Értesítés hello ". \" parancsnak a.) Ennek nagyjából a következőképpen kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="94e5c-161">(Notice hello “.\” in that command.) You should see something like this:</span></span></br>

   ![A betöltött modulok listáján](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

3. <span data-ttu-id="94e5c-163">Adja meg a hello `Login-AzureRmAccount` parancsot.</span><span class="sxs-lookup"><span data-stu-id="94e5c-163">Enter hello `Login-AzureRmAccount` command.</span></span> <span data-ttu-id="94e5c-164">Hello bejelentkezési ablakban adja meg, amelyekkel a jelen oktatóanyag hello előfizetés hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="94e5c-164">In hello login window, enter hello credential information for hello subscription that you will use for this tutorial.</span></span>

   >[!NOTE]
   ><span data-ttu-id="94e5c-165">Ha hello először, akkor még a bejelentkezés tooAzure erről a gépről, akkor engedélyezése a Microsoft toocollect PowerShell használati adatok egy üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="94e5c-165">If this is hello first time that you're logging in tooAzure from this machine, you will see a message about allowing Microsoft toocollect PowerShell usage data.</span></span> <span data-ttu-id="94e5c-166">Azt javasoljuk, hogy engedélyezze az adatgyűjtés, mert használt tooimprove Azure PowerShell lesz.</span><span class="sxs-lookup"><span data-stu-id="94e5c-166">We recommend that you enable this data collection because it will be used tooimprove Azure PowerShell.</span></span>

4. <span data-ttu-id="94e5c-167">Sikeres hitelesítést követően jelentkezett be, és a következő képernyőkép hello hello adatok láthatók.</span><span class="sxs-lookup"><span data-stu-id="94e5c-167">After successful authentication, you're logged in and you see hello information in hello following screenshot.</span></span> <span data-ttu-id="94e5c-168">Jegyezze fel a hello előfizetési azonosító és az előfizetés nevét, mert lesz szüksége toocomplete később lépések.</span><span class="sxs-lookup"><span data-stu-id="94e5c-168">Take note of hello subscription ID and subscription name, because you'll need them toocomplete later steps.</span></span>

   ![PowerShell-ablakot](./media/security-azure-log-integration-keyvault-eventhub/login-azurermaccount.png)
5. <span data-ttu-id="94e5c-170">Változók létrehozása toostore értékeket, amelyeket a rendszer később.</span><span class="sxs-lookup"><span data-stu-id="94e5c-170">Create variables toostore values that will be used later.</span></span> <span data-ttu-id="94e5c-171">Adja meg az alábbi PowerShell hello mindegyikének.</span><span class="sxs-lookup"><span data-stu-id="94e5c-171">Enter each of hello following PowerShell lines.</span></span> <span data-ttu-id="94e5c-172">Szükség lehet tooadjust hello értékek toomatch a környezetben.</span><span class="sxs-lookup"><span data-stu-id="94e5c-172">You might need tooadjust hello values toomatch your environment.</span></span>
    - <span data-ttu-id="94e5c-173">```$subscriptionName = ‘Visual Studio Ultimate with MSDN’```(Az előfizetés neve eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="94e5c-173">```$subscriptionName = ‘Visual Studio Ultimate with MSDN’``` (Your subscription name might be different.</span></span> <span data-ttu-id="94e5c-174">Megnézheti hello kimeneti hello előző parancs részeként.)</span><span class="sxs-lookup"><span data-stu-id="94e5c-174">You can see it as part of hello output of hello previous command.)</span></span>
    - <span data-ttu-id="94e5c-175">```$location = 'West US'```(Ez a változó lehet használt toopass hello helyre, ahol erőforrásokat kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="94e5c-175">```$location = 'West US'``` (This variable will be used toopass hello location where resources should be created.</span></span> <span data-ttu-id="94e5c-176">A változó toobe bármely helyét módosíthatja a.)</span><span class="sxs-lookup"><span data-stu-id="94e5c-176">You can change this variable toobe any location of your choosing.)</span></span>
    - ```$random = Get-Random```
    - <span data-ttu-id="94e5c-177">``` $name = 'azlogtest' + $random```(hello neve bármi lehet, de ez csak kisbetűket és számokat kell tartalmaznia.)</span><span class="sxs-lookup"><span data-stu-id="94e5c-177">``` $name = 'azlogtest' + $random``` (hello name can be anything, but it should include only lowercase letters and numbers.)</span></span>
    - <span data-ttu-id="94e5c-178">``` $storageName = $name```(Ezt a változót használja a tárfiók nevének hello.)</span><span class="sxs-lookup"><span data-stu-id="94e5c-178">``` $storageName = $name``` (This variable will be used for hello storage account name.)</span></span>
    - <span data-ttu-id="94e5c-179">```$rgname = $name ```(Ez a változó használandó hello erőforráscsoport-név.)</span><span class="sxs-lookup"><span data-stu-id="94e5c-179">```$rgname = $name ``` (This variable will be used for hello resource group name.)</span></span>
    - <span data-ttu-id="94e5c-180">``` $eventHubNameSpaceName = $name```(Ez a hello hello event hub névtér nevét.)</span><span class="sxs-lookup"><span data-stu-id="94e5c-180">``` $eventHubNameSpaceName = $name``` (This is hello name of hello event hub namespace.)</span></span>
6. <span data-ttu-id="94e5c-181">Adja meg, hogy a működik hello előfizetés:</span><span class="sxs-lookup"><span data-stu-id="94e5c-181">Specify hello subscription that you will be working with:</span></span>
    
    ```Select-AzureRmSubscription -SubscriptionName $subscriptionName```
7. <span data-ttu-id="94e5c-182">Hozzon létre egy erőforráscsoportot:</span><span class="sxs-lookup"><span data-stu-id="94e5c-182">Create a resource group:</span></span>
    
    ```$rg = New-AzureRmResourceGroup -Name $rgname -Location $location```
    
   <span data-ttu-id="94e5c-183">Ha beírja `$rg` Ekkor megtekintheti az kimeneti hasonló toothis képernyőkép:</span><span class="sxs-lookup"><span data-stu-id="94e5c-183">If you enter `$rg` at this point, you should see output similar toothis screenshot:</span></span>

   ![Kimeneti erőforráscsoport létrehozása után](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
8. <span data-ttu-id="94e5c-185">Hozzon létre egy tárfiókot, amely állapotadatokat használt tookeep nyomon lesz:</span><span class="sxs-lookup"><span data-stu-id="94e5c-185">Create a storage account that will be used tookeep track of state information:</span></span>
    
    ```$storage = New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
9. <span data-ttu-id="94e5c-186">Hello event hub névtér létrehozása.</span><span class="sxs-lookup"><span data-stu-id="94e5c-186">Create hello event hub namespace.</span></span> <span data-ttu-id="94e5c-187">Ez azért szükség toocreate egy eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="94e5c-187">This is required toocreate an event hub.</span></span>
    
    ```$eventHubNameSpace = New-AzureRmEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
10. <span data-ttu-id="94e5c-188">Töltse le a hello insights szolgáltatónál használandó hello Szabályazonosító:</span><span class="sxs-lookup"><span data-stu-id="94e5c-188">Get hello rule ID that will be used with hello insights provider:</span></span>
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey' ```
11. <span data-ttu-id="94e5c-189">Az összes lehetséges az Azure-helyeinek lekéréséhez, és adja hozzá a hello nevek tooa változó, amely használható egy későbbi lépésben:</span><span class="sxs-lookup"><span data-stu-id="94e5c-189">Get all possible Azure locations and add hello names tooa variable that can be used in a later step:</span></span>
    
    <span data-ttu-id="94e5c-190">a.</span><span class="sxs-lookup"><span data-stu-id="94e5c-190">a.</span></span> ```$locationObjects = Get-AzureRMLocation```    
    <span data-ttu-id="94e5c-191">b.</span><span class="sxs-lookup"><span data-stu-id="94e5c-191">b.</span></span> ```$locations = @('global') + $locationobjects.location```
    
    <span data-ttu-id="94e5c-192">Ha beírja `$locations` ekkor megjelenik az hello helynevek hello Get-AzureRmLocation által visszaadott további adatok nélkül.</span><span class="sxs-lookup"><span data-stu-id="94e5c-192">If you enter `$locations` at this point, you see hello location names without hello additional information returned by Get-AzureRmLocation.</span></span>
12. <span data-ttu-id="94e5c-193">Azure Resource Manager napló profil létrehozása:</span><span class="sxs-lookup"><span data-stu-id="94e5c-193">Create an Azure Resource Manager log profile:</span></span> 
    
    ```Add-AzureRmLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    <span data-ttu-id="94e5c-194">Hello Azure naplóelemzés profil kapcsolatos további információkért lásd: [hello Azure tevékenységnapló áttekintése](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="94e5c-194">For more information about hello Azure log profile, see [Overview of hello Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).</span></span>

> [!NOTE]
> <span data-ttu-id="94e5c-195">Egy hibaüzenetet kaphat toocreate napló profil meg.</span><span class="sxs-lookup"><span data-stu-id="94e5c-195">You might get an error message when you try toocreate a log profile.</span></span> <span data-ttu-id="94e5c-196">Majd a Get-AzureRmLogProfile és eltávolítása-AzureRmLogProfile hello dokumentációjában tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="94e5c-196">You can then review hello documentation for Get-AzureRmLogProfile and Remove-AzureRmLogProfile.</span></span> <span data-ttu-id="94e5c-197">Ha a Get-AzureRmLogProfile futtatja, lásd: hello napló profillal kapcsolatos információk.</span><span class="sxs-lookup"><span data-stu-id="94e5c-197">If you run Get-AzureRmLogProfile, you see information about hello log profile.</span></span> <span data-ttu-id="94e5c-198">Hello meglévő napló profil törlése hello megadásával ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` parancsot.</span><span class="sxs-lookup"><span data-stu-id="94e5c-198">You can delete hello existing log profile by entering hello ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` command.</span></span>
>
>![Erőforrás-kezelő profil hiba](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a><span data-ttu-id="94e5c-200">Kulcstartó létrehozása</span><span class="sxs-lookup"><span data-stu-id="94e5c-200">Create a key vault</span></span>

1. <span data-ttu-id="94e5c-201">Hello kulcstároló létrehozása:</span><span class="sxs-lookup"><span data-stu-id="94e5c-201">Create hello key vault:</span></span>

   ```$kv = New-AzureRmKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location ```

2. <span data-ttu-id="94e5c-202">Hello kulcstároló naplózásának konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="94e5c-202">Configure logging for hello key vault:</span></span>

   ```Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true ```

## <a name="generate-log-activity"></a><span data-ttu-id="94e5c-203">Napló tevékenység létrehozása</span><span class="sxs-lookup"><span data-stu-id="94e5c-203">Generate log activity</span></span>

<span data-ttu-id="94e5c-204">Kérelmek tooKey tároló toogenerate naplózása küldött toobe kell.</span><span class="sxs-lookup"><span data-stu-id="94e5c-204">Requests need toobe sent tooKey Vault toogenerate log activity.</span></span> <span data-ttu-id="94e5c-205">Műveletek, például Kulcslétrehozási, titkok, tárolása, vagy titkos kulcsok beolvasása a Key Vault naplóbejegyzések hoz létre.</span><span class="sxs-lookup"><span data-stu-id="94e5c-205">Actions like key generation, storing secrets, or reading secrets from Key Vault will create log entries.</span></span>

1. <span data-ttu-id="94e5c-206">Hello aktuális tárolási kulcsok megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="94e5c-206">Display hello current storage keys:</span></span>
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
2. <span data-ttu-id="94e5c-207">Egy új készítése **key2**:</span><span class="sxs-lookup"><span data-stu-id="94e5c-207">Generate a new **key2**:</span></span>
    
   ```New-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
3. <span data-ttu-id="94e5c-208">Jelenjen meg többé hello kulcsok és, hogy látható **key2** egy másik értéket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="94e5c-208">Display hello keys again and see that **key2** holds a different value:</span></span>
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
4. <span data-ttu-id="94e5c-209">Állítsa be, és olvassa el a titkos toogenerate további naplóbejegyzések:</span><span class="sxs-lookup"><span data-stu-id="94e5c-209">Set and read a secret toogenerate additional log entries:</span></span>
    
   <span data-ttu-id="94e5c-210">a.</span><span class="sxs-lookup"><span data-stu-id="94e5c-210">a.</span></span> <span data-ttu-id="94e5c-211">```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b.</span><span class="sxs-lookup"><span data-stu-id="94e5c-211">```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b.</span></span> ```(Get-AzureKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Visszaadott titkos](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a><span data-ttu-id="94e5c-213">Azure Naplóelemzés-integráció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="94e5c-213">Configure Azure Log Integration</span></span>

<span data-ttu-id="94e5c-214">Most, hogy az összes hello belőlük bizonyos megkövetelt elemek toohave Key Vault naplózásának tooan eseményközpont van beállítva, tooconfigure Azure napló integrációs szüksége:</span><span class="sxs-lookup"><span data-stu-id="94e5c-214">Now that you have configured all hello required elements toohave Key Vault logging tooan event hub, you need tooconfigure Azure Log Integration:</span></span>

1. ```$storage = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename```
2. ```$eventHubKey = Get-AzureRmEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
3. ```$storagekeys = Get-AzureRmStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
4. ``` $storagekey = $storagekeys[0].Value```

<span data-ttu-id="94e5c-215">Futtassa az egyes eseményközpontban hello AzLog parancsot:</span><span class="sxs-lookup"><span data-stu-id="94e5c-215">Run hello AzLog command for each event hub:</span></span>

1. ```$eventhubs = Get-AzureRmEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
2. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

<span data-ttu-id="94e5c-216">Egy perc vagy így hello utolsó két parancsok futtatására megtekintheti az JSON-fájlok létrehozása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="94e5c-216">After a minute or so of running hello last two commands, you should see JSON files being generated.</span></span> <span data-ttu-id="94e5c-217">Hello directory figyelésével ellenőrizheti, hogy **C:\users\AzLog\EventHubJson**.</span><span class="sxs-lookup"><span data-stu-id="94e5c-217">You can confirm that by monitoring hello directory **C:\users\AzLog\EventHubJson**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="94e5c-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="94e5c-218">Next steps</span></span>

- [<span data-ttu-id="94e5c-219">Azure Naplóelemzés integrációs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="94e5c-219">Azure Log Integration FAQ</span></span>](security-azure-log-integration-faq.md)
- [<span data-ttu-id="94e5c-220">Ismerkedés az Azure napló integráció</span><span class="sxs-lookup"><span data-stu-id="94e5c-220">Get started with Azure Log Integration</span></span>](security-azure-log-integration-get-started.md)
- [<span data-ttu-id="94e5c-221">Az Azure-erőforrások naplók integrálja a SIEM-rendszerekről</span><span class="sxs-lookup"><span data-stu-id="94e5c-221">Integrate logs from Azure resources into your SIEM systems</span></span>](security-azure-log-integration-overview.md)
