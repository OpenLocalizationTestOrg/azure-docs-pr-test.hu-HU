---
title: "aaaConfigure hello szerepkörök az Azure felhőalapú szolgáltatás, a Visual Studio |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset össze, és az Azure felhőszolgáltatások Visual Studio használatával szerepkörök konfigurálásához."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: d397ef87-64e5-401a-aad5-7f83f1022e16
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: d3c62eb57040ebe987787e73b17b468bb82122bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-cloud-service-roles-with-visual-studio"></a><span data-ttu-id="5fb29-103">A Visual Studio Azure cloud service szerepkörök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5fb29-103">Configure Azure cloud service roles with Visual Studio</span></span>
<span data-ttu-id="5fb29-104">Azure-felhőszolgáltatás rendelkezhet egy vagy több munkavégző vagy a webes szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="5fb29-104">An Azure cloud service can have one or more worker or web roles.</span></span> <span data-ttu-id="5fb29-105">Az egyes szerepkörökhöz kell, hogy a szerepkör beállítására toodefine és is konfigurálhatja, hogyan fut a szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="5fb29-105">For each role, you need toodefine how that role is set up and also configure how that role runs.</span></span> <span data-ttu-id="5fb29-106">További információ a felhőalapú szolgáltatások szerepkörök toolearn lásd: hello videó [bemutatása tooAzure Felhőszolgáltatások](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services).</span><span class="sxs-lookup"><span data-stu-id="5fb29-106">toolearn more about roles in cloud services, see hello video [Introduction tooAzure Cloud Services](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services).</span></span> 

<span data-ttu-id="5fb29-107">a felhőszolgáltatás hello adatokat következő fájlok hello tárolja:</span><span class="sxs-lookup"><span data-stu-id="5fb29-107">hello information for your cloud service is stored in hello following files:</span></span>

- <span data-ttu-id="5fb29-108">**ServiceDefinition.csdef** -hello szolgáltatásdefiníciós fájlt a milyen szerepkörre szükség, beleértve a felhőalapú szolgáltatás, a végpontok és a virtuálisgép-méret hello futásidejű beállításokat határoz meg.</span><span class="sxs-lookup"><span data-stu-id="5fb29-108">**ServiceDefinition.csdef** - hello service definition file defines hello runtime settings for your cloud service including what roles are required, endpoints, and virtual machine size.</span></span> <span data-ttu-id="5fb29-109">Nincs a hello adataihoz `ServiceDefinition.csdef` módosítható, ha a szerepkör fut..</span><span class="sxs-lookup"><span data-stu-id="5fb29-109">None of hello data stored in `ServiceDefinition.csdef` can be changed when your role is running.</span></span>
- <span data-ttu-id="5fb29-110">**ServiceConfiguration.cscfg** - hello szolgáltatás konfigurációs fájlja konfigurálja, hogy hány példánya egy futnak, és az egy szerepkörhöz definiált hello hello beállítások értékeit.</span><span class="sxs-lookup"><span data-stu-id="5fb29-110">**ServiceConfiguration.cscfg** - hello service configuration file configures how many instances of a role are run and hello values of hello settings defined for a role.</span></span> <span data-ttu-id="5fb29-111">a tárolt adatok hello `ServiceConfiguration.cscfg` módosíthatja a szerepkör futása közben.</span><span class="sxs-lookup"><span data-stu-id="5fb29-111">hello data stored in `ServiceConfiguration.cscfg` can be changed while your role is running.</span></span>

<span data-ttu-id="5fb29-112">egy szerepkör működésével vezérlőhöz hello-beállítások értékei különböző toostore, meghatározhatja, hogy több szolgáltatáskonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="5fb29-112">toostore different values for hello settings that control how a role runs, you can define multiple service configurations.</span></span> <span data-ttu-id="5fb29-113">Minden környezet különböző szolgáltatáskonfiguráció használható.</span><span class="sxs-lookup"><span data-stu-id="5fb29-113">You can use a different service configuration for each deployment environment.</span></span> <span data-ttu-id="5fb29-114">Például állítsa be a tárolási fiók kapcsolati karakterlánc toouse hello helyi az Azure storage emulator egy helyi szolgáltatás konfigurációjában, és hozzon létre egy másik szolgáltatás konfigurációs toouse az Azure storage hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="5fb29-114">For example, you can set your storage account connection string toouse hello local Azure storage emulator in a local service configuration and create another service configuration toouse Azure storage in hello cloud.</span></span>

<span data-ttu-id="5fb29-115">Azure-felhőszolgáltatás létrehozása a Visual Studióban, ha a két szolgáltatáskonfiguráció automatikusan jönnek létre, és a hozzáadott Azure-projekt tooyour:</span><span class="sxs-lookup"><span data-stu-id="5fb29-115">When you create an Azure cloud service in Visual Studio, two service configurations are automatically created and added tooyour Azure project:</span></span>

- `ServiceConfiguration.Cloud.cscfg`
- `ServiceConfiguration.Local.cscfg`

## <a name="configure-an-azure-cloud-service"></a><span data-ttu-id="5fb29-116">Azure-felhőszolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5fb29-116">Configure an Azure cloud service</span></span>
<span data-ttu-id="5fb29-117">A Visual Studio megoldáskezelőjében egy Azure felhőszolgáltatást konfigurálhatja, ahogy az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5fb29-117">You can configure an Azure cloud service from Solution Explorer in Visual Studio, as shown in hello following steps:</span></span>

1. <span data-ttu-id="5fb29-118">Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.</span><span class="sxs-lookup"><span data-stu-id="5fb29-118">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="5fb29-119">A **Megoldáskezelőben**, kattintson a jobb gombbal a projekt hello és, hello helyi menüből válassza ki a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-119">In **Solution Explorer**, right-click hello project, and, from hello context menu, select **Properties**.</span></span>
   
    ![Solution Explorer projekt helyi menü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-project-context-menu.png)

1. <span data-ttu-id="5fb29-121">Hello projekt Tulajdonságok lapján válassza hello **fejlesztési** fülre.</span><span class="sxs-lookup"><span data-stu-id="5fb29-121">In hello project's properties page, select hello **Development** tab.</span></span> 

    ![Projekt párbeszédpanel - fejlesztési lap](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-development-tab.png)

1. <span data-ttu-id="5fb29-123">A hello **szolgáltatáskonfiguráció** listán, válassza hello nevét, amelyet az tooedit hello szolgáltatás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="5fb29-123">In hello **Service Configuration** list, select hello name of hello service configuration that you want tooedit.</span></span> <span data-ttu-id="5fb29-124">(Ha toomake változik tooall hello szolgáltatáskonfiguráció ehhez a szerepkörhöz, jelölje be **összes konfiguráció**.)</span><span class="sxs-lookup"><span data-stu-id="5fb29-124">(If you want toomake changes tooall hello service configurations for this role, select **All Configurations**.)</span></span>
   
    > [!IMPORTANT]
    > <span data-ttu-id="5fb29-125">Ha úgy dönt, hogy egy adott szolgáltatás konfigurációs, néhány tulajdonságát le vannak tiltva, mivel azok csak az összes konfiguráció állítható be.</span><span class="sxs-lookup"><span data-stu-id="5fb29-125">If you choose a specific service configuration, some properties are disabled because they can only be set for all configurations.</span></span> <span data-ttu-id="5fb29-126">tooedit ezeket a tulajdonságokat, ki kell választania **összes konfiguráció**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-126">tooedit these properties, you must select **All Configurations**.</span></span>
    > 
    > 
   
    ![Azure-felhőszolgáltatás szolgáltatáskonfiguráció listája](./media/vs-azure-tools-configure-roles-for-cloud-service/cloud-service-service-configuration-property.png)

## <a name="change-hello-number-of-role-instances"></a><span data-ttu-id="5fb29-128">A szerepkörpéldányok számát hello módosítása</span><span class="sxs-lookup"><span data-stu-id="5fb29-128">Change hello number of role instances</span></span>
<span data-ttu-id="5fb29-129">a felhőalapú szolgáltatás tooimprove hello teljesítményétől, módosíthatja hello hány példánya egy szerepkört futtató, felhasználók vagy egy adott szerepkör várt hello betöltése hello száma alapján.</span><span class="sxs-lookup"><span data-stu-id="5fb29-129">tooimprove hello performance of your cloud service, you can change hello number of instances of a role that are running, based on hello number of users or hello load expected for a particular role.</span></span> <span data-ttu-id="5fb29-130">Egy különálló virtuális gép szerepkör minden egyes példányánál létrejön, amikor hello felhőalapú szolgáltatás fut az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5fb29-130">A separate virtual machine is created for each instance of a role when hello cloud service runs in Azure.</span></span> <span data-ttu-id="5fb29-131">Ez hatással van a hello számlázási hello telepítését a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5fb29-131">This affects hello billing for hello deployment of this cloud service.</span></span> <span data-ttu-id="5fb29-132">Számlázással kapcsolatos további információkért lásd: [a számlázási megérteni a Microsoft Azure](billing/billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="5fb29-132">For more information about billing, see [Understand your bill for Microsoft Azure](billing/billing-understand-your-bill.md).</span></span>

1. <span data-ttu-id="5fb29-133">Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.</span><span class="sxs-lookup"><span data-stu-id="5fb29-133">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="5fb29-134">A **Megoldáskezelőben**, bontsa ki a hello projekt csomópontjára.</span><span class="sxs-lookup"><span data-stu-id="5fb29-134">In **Solution Explorer**, expand hello project node.</span></span> <span data-ttu-id="5fb29-135">A hello **szerepkörök** csomópontot, kattintson a jobb gombbal hello szerepkör tooupdate, és, hello helyi menüből jelöljük ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-135">Under hello **Roles** node, right-click hello role you want tooupdate, and, from hello context menu, select **Properties**.</span></span>

    ![Solution Explorer Azure szerepkör helyi menü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="5fb29-137">Jelölje be hello **konfigurációs** fülre.</span><span class="sxs-lookup"><span data-stu-id="5fb29-137">Select hello **Configuration** tab.</span></span>

    ![Konfiguráció lap](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page.png)

1. <span data-ttu-id="5fb29-139">A hello **szolgáltatáskonfiguráció** listában, jelölje be hello szolgáltatás konfigurációja, amelyet az tooupdate.</span><span class="sxs-lookup"><span data-stu-id="5fb29-139">In hello **Service Configuration** list, select hello service configuration that you want tooupdate.</span></span>
   
    ![Konfigurációs szolgáltatás listája](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-select-configuration.png)

1. <span data-ttu-id="5fb29-141">A hello **száma példány** szöveg mezőbe írja be a kívánt toostart a szerepkör-példányok hello száma.</span><span class="sxs-lookup"><span data-stu-id="5fb29-141">In hello **Instance count** text box, enter hello number of instances that you want toostart for this role.</span></span> <span data-ttu-id="5fb29-142">Minden példány egy különálló virtuális gép futó, hello cloud service tooAzure közzétételekor.</span><span class="sxs-lookup"><span data-stu-id="5fb29-142">Each instance runs on a separate virtual machine when you publish hello cloud service tooAzure.</span></span>

    ![A példányok száma hello frissítése](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-instance-count.png)

1. <span data-ttu-id="5fb29-144">A Visual Studio hello eszköztárában kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-144">From hello Visual Studio, toolbar, select **Save**.</span></span>

## <a name="manage-connection-strings-for-storage-accounts"></a><span data-ttu-id="5fb29-145">Kapcsolati karakterláncok storage-fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="5fb29-145">Manage connection strings for storage accounts</span></span>
<span data-ttu-id="5fb29-146">Hozzáadhat, távolítsa el, vagy módosítsa a szolgáltatáskonfiguráció-kapcsolati karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="5fb29-146">You can add, remove, or modify connection strings for your service configurations.</span></span> <span data-ttu-id="5fb29-147">Például érdemes egy helyi kapcsolati karakterláncot egy helyi szolgáltatás konfigurációjához, amely értéke `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="5fb29-147">For example, you might want a local connection string for a local service configuration that has a value of `UseDevelopmentStorage=true`.</span></span> <span data-ttu-id="5fb29-148">Érdemes lehet tooconfigure egy felhőalapú szolgáltatás konfigurációja, amely egy tárfiókot használja az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5fb29-148">You might also want tooconfigure a cloud service configuration that uses a storage account in Azure.</span></span>

> [!WARNING]
> <span data-ttu-id="5fb29-149">Hello az Azure storage fiókhoz tartozó nyilvánoskulcs-adatokat egy tárolási fiók kapcsolati karakterláncot adja meg, ezeket az információkat tárolja helyileg hello szolgáltatás konfigurációs fájljában.</span><span class="sxs-lookup"><span data-stu-id="5fb29-149">When you enter hello Azure storage account key information for a storage account connection string, this information is stored locally in hello service configuration file.</span></span> <span data-ttu-id="5fb29-150">Azonban ez az információ nem tárolt titkosított szövegként.</span><span class="sxs-lookup"><span data-stu-id="5fb29-150">However, this information is currently not stored as encrypted text.</span></span>
> 
> 

<span data-ttu-id="5fb29-151">Minden szolgáltatás konfigurációját egy másik értéket használ, nem toouse eltérő kapcsolódási karakterláncokkal rendelkezik a felhőalapú szolgáltatás, vagy nem módosíthatja a kódot, a cloud service tooAzure közzétételekor.</span><span class="sxs-lookup"><span data-stu-id="5fb29-151">By using a different value for each service configuration, you do not have toouse different connection strings in your cloud service or modify your code when you publish your cloud service tooAzure.</span></span> <span data-ttu-id="5fb29-152">Használhatja ugyanazt a nevet a kódot és hello érték hello kapcsolati karakterlánc nem egyezik a felhőalapú szolgáltatás építésekor, vagy amikor közzétesszük kiválasztott hello szolgáltatás konfigurációja alapján hello.</span><span class="sxs-lookup"><span data-stu-id="5fb29-152">You can use hello same name for hello connection string in your code and hello value is different, based on hello service configuration that you select when you build your cloud service or when you publish it.</span></span>

1. <span data-ttu-id="5fb29-153">Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.</span><span class="sxs-lookup"><span data-stu-id="5fb29-153">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="5fb29-154">A **Megoldáskezelőben**, bontsa ki a hello projekt csomópontjára.</span><span class="sxs-lookup"><span data-stu-id="5fb29-154">In **Solution Explorer**, expand hello project node.</span></span> <span data-ttu-id="5fb29-155">A hello **szerepkörök** csomópontot, kattintson a jobb gombbal hello szerepkör tooupdate, és, hello helyi menüből jelöljük ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-155">Under hello **Roles** node, right-click hello role you want tooupdate, and, from hello context menu, select **Properties**.</span></span>

    ![Solution Explorer Azure szerepkör helyi menü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="5fb29-157">Jelölje be hello **beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="5fb29-157">Select hello **Settings** tab.</span></span>

    ![Beállítások lap](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. <span data-ttu-id="5fb29-159">A hello **szolgáltatáskonfiguráció** listában, jelölje be hello szolgáltatás konfigurációja, amelyet az tooupdate.</span><span class="sxs-lookup"><span data-stu-id="5fb29-159">In hello **Service Configuration** list, select hello service configuration that you want tooupdate.</span></span>

    ![Szolgáltatás konfigurációja](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. <span data-ttu-id="5fb29-161">egy kapcsolati karakterláncot tooadd kiválasztása **beállítás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-161">tooadd a connection string, select **Add Setting**.</span></span>

    ![Kapcsolati karakterlánc hozzáadása](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. <span data-ttu-id="5fb29-163">Új beállítás hello toohello lista hozzá lett adva, frissíti hello listában hello sort hello szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="5fb29-163">Once hello new setting has been added toohello list, update hello row in hello list with hello necessary information.</span></span>

    ![Új kapcsolati karakterlánc](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - <span data-ttu-id="5fb29-165">**Név** -adja meg, amelyet az toouse hello kapcsolati karakterlánc hello nevét.</span><span class="sxs-lookup"><span data-stu-id="5fb29-165">**Name** - Enter hello name that you want toouse for hello connection string.</span></span>
    - <span data-ttu-id="5fb29-166">**Típus** – Itt adhatja meg **kapcsolati karakterlánc** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="5fb29-166">**Type** - Select **Connection String** from hello drop-down list.</span></span>
    - <span data-ttu-id="5fb29-167">**Érték** -megadhat a hello kapcsolati karakterlánc közvetlenül a hello **érték** cellára, vagy válassza hello három ponttal (…) toowork a hello **tárolási kapcsolati karakterlánc létrehozása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5fb29-167">**Value** - You can either enter hello connection string directly into hello **Value** cell, or select hello ellipsis (...) toowork in hello **Create Storage Connection String** dialog.</span></span>  

1. <span data-ttu-id="5fb29-168">A hello **tárolási kapcsolati karakterlánc létrehozása** párbeszédpanelen válasszon egy beállítást **protokoll használatával kapcsolódó levelezőprogramokkal**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-168">In hello **Create Storage Connection String** dialog, select an option for **Connect using**.</span></span> <span data-ttu-id="5fb29-169">Kövesse a hello beállítástól hello utasításokat:</span><span class="sxs-lookup"><span data-stu-id="5fb29-169">Then follow hello instructions for hello option you select:</span></span>

    - <span data-ttu-id="5fb29-170">**A Microsoft Azure storage emulator** -ezt a beállítást, ha az hello fennmaradó beállításokat a következő párbeszédpanelen: hello le vannak tiltva csak tooAzure vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="5fb29-170">**Microsoft Azure storage emulator** - If you select this option, hello remaining settings on hello dialog are disabled as they apply only tooAzure.</span></span> <span data-ttu-id="5fb29-171">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="5fb29-171">Select **OK**.</span></span>
    - <span data-ttu-id="5fb29-172">**Az előfizetés** – Ha válassza ki a beállítást, használja tooeither hello legördülő listában válassza ki, és jelentkezzen be Microsoft-fiókba, vagy adja hozzá a Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="5fb29-172">**Your subscription** - If you select this option, use hello dropdown list tooeither select and sign into a Microsoft account, or add a Microsoft account.</span></span> <span data-ttu-id="5fb29-173">Válassza ki az Azure-előfizetés és a tárolási fiók.</span><span class="sxs-lookup"><span data-stu-id="5fb29-173">Select an Azure subscription and storage account.</span></span> <span data-ttu-id="5fb29-174">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="5fb29-174">Select **OK**.</span></span>
    - <span data-ttu-id="5fb29-175">**Manuálisan kell megadni a hitelesítő adatok** – hello tárfiók nevét adja meg, és vagy hello második vagy elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="5fb29-175">**Manually entered credentials** - Enter hello storage account name, and either hello primary or second key.</span></span> <span data-ttu-id="5fb29-176">Válassza ki a **kapcsolat** (HTTPS ajánlott a legtöbb esetben.) Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="5fb29-176">Select an option for **Connection** (HTTPS is recommended for most scenarios.) Select **OK**.</span></span>

1. <span data-ttu-id="5fb29-177">toodelete egy kapcsolati karakterláncot hello kapcsolati karakterláncot, majd válassza ki és **eltávolítása beállítás**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-177">toodelete a connection string, select hello connection string, and then select **Remove Setting**.</span></span>

1. <span data-ttu-id="5fb29-178">A Visual Studio hello eszköztárában kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-178">From hello Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-access-a-connection-string"></a><span data-ttu-id="5fb29-179">A kapcsolati karakterlánc programon keresztüli eléréséhez</span><span class="sxs-lookup"><span data-stu-id="5fb29-179">Programmatically access a connection string</span></span>

<span data-ttu-id="5fb29-180">hello következő lépések bemutatják, hogyan férhetnek hozzá a tooprogrammatically egy kapcsolati karakterláncot, amely C#.</span><span class="sxs-lookup"><span data-stu-id="5fb29-180">hello following steps show how tooprogrammatically access a connection string using C#.</span></span>

1. <span data-ttu-id="5fb29-181">Adja hozzá a következő hello toouse hello beállítás hová irányelvek tooa C#-fájl használatával:</span><span class="sxs-lookup"><span data-stu-id="5fb29-181">Add hello following using directives tooa C# file where you are going toouse hello setting:</span></span>

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. <span data-ttu-id="5fb29-182">hello alábbi kód bemutatja, hogyan például tooaccess egy kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="5fb29-182">hello following code illustrates an example of how tooaccess a connection string.</span></span> <span data-ttu-id="5fb29-183">Cserélje le a hello &lt;ConnectionStringName > helyőrzőt hello megfelelő értékre.</span><span class="sxs-lookup"><span data-stu-id="5fb29-183">Replace hello &lt;ConnectionStringName> placeholder with hello appropriate value.</span></span> 

    ```csharp
    // Setup hello connection tooAzure Storage
    var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("<ConnectionStringName>"));
    ```

## <a name="add-custom-settings-toouse-in-your-azure-cloud-service"></a><span data-ttu-id="5fb29-184">Egyéni beállítások toouse adja hozzá az Azure-felhőszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5fb29-184">Add custom settings toouse in your Azure cloud service</span></span>
<span data-ttu-id="5fb29-185">Hello szolgáltatás konfigurációs fájljában egyéni beállítások segítségével úgy adja hozzá egy nevet és értéket egy adott szolgáltatás-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="5fb29-185">Custom settings in hello service configuration file let you add a name and value for a string for a specific service configuration.</span></span> <span data-ttu-id="5fb29-186">Ez a beállítás tooconfigure hello értékének olvasásával a felhőszolgáltatásban található egy szolgáltatás és az érték toocontrol hello logika használatához a kódban hello toouse célszerű használni.</span><span class="sxs-lookup"><span data-stu-id="5fb29-186">You might choose toouse this setting tooconfigure a feature in your cloud service by reading hello value of hello setting and using this value toocontrol hello logic in your code.</span></span> <span data-ttu-id="5fb29-187">Ezeket a szolgáltatás konfigurációs értékeket módosíthatja, anélkül, hogy toorebuild a szolgáltatáscsomag vagy a felhőalapú szolgáltatás futása közben.</span><span class="sxs-lookup"><span data-stu-id="5fb29-187">You can change these service configuration values without having toorebuild your service package or when your cloud service is running.</span></span> <span data-ttu-id="5fb29-188">A kódot is keresése az értesítések, amikor a beállítások módosítása.</span><span class="sxs-lookup"><span data-stu-id="5fb29-188">Your code can check for notifications of when a setting changes.</span></span> <span data-ttu-id="5fb29-189">Lásd: [RoleEnvironment.Changing esemény](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).</span><span class="sxs-lookup"><span data-stu-id="5fb29-189">See [RoleEnvironment.Changing Event](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).</span></span>

<span data-ttu-id="5fb29-190">Is hozzáadhat, távolítsa el, vagy módosítsa a szolgáltatáskonfiguráció egyéni beállításait.</span><span class="sxs-lookup"><span data-stu-id="5fb29-190">You can add, remove, or modify custom settings for your service configurations.</span></span> <span data-ttu-id="5fb29-191">Ezek a különböző szolgáltatáskonfiguráció karakterláncok érdemes eltérő értékek tartoznak.</span><span class="sxs-lookup"><span data-stu-id="5fb29-191">You might want different values for these strings for different service configurations.</span></span>

<span data-ttu-id="5fb29-192">Minden szolgáltatás konfigurációját egy másik értéket használ, nem rendelkezik toouse különböző karakterláncokkal a felhőszolgáltatásban vagy módosítsa a kódot, a cloud service tooAzure közzétételekor.</span><span class="sxs-lookup"><span data-stu-id="5fb29-192">By using a different value for each service configuration, you do not have toouse different strings in your cloud service or modify your code when you publish your cloud service tooAzure.</span></span> <span data-ttu-id="5fb29-193">Használhatja ugyanazt a nevet a kódot és hello értékének hello karakterlánc nem egyezik a felhőalapú szolgáltatás építésekor, vagy amikor közzétesszük kiválasztott hello szolgáltatás konfigurációja alapján hello.</span><span class="sxs-lookup"><span data-stu-id="5fb29-193">You can use hello same name for hello string in your code and hello value is different, based on hello service configuration that you select when you build your cloud service or when you publish it.</span></span>

1. <span data-ttu-id="5fb29-194">Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.</span><span class="sxs-lookup"><span data-stu-id="5fb29-194">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="5fb29-195">A **Megoldáskezelőben**, bontsa ki a hello projekt csomópontjára.</span><span class="sxs-lookup"><span data-stu-id="5fb29-195">In **Solution Explorer**, expand hello project node.</span></span> <span data-ttu-id="5fb29-196">A hello **szerepkörök** csomópontot, kattintson a jobb gombbal hello szerepkör tooupdate, és, hello helyi menüből jelöljük ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-196">Under hello **Roles** node, right-click hello role you want tooupdate, and, from hello context menu, select **Properties**.</span></span>

    ![Solution Explorer Azure szerepkör helyi menü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="5fb29-198">Jelölje be hello **beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="5fb29-198">Select hello **Settings** tab.</span></span>

    ![Beállítások lap](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. <span data-ttu-id="5fb29-200">A hello **szolgáltatáskonfiguráció** listában, jelölje be hello szolgáltatás konfigurációja, amelyet az tooupdate.</span><span class="sxs-lookup"><span data-stu-id="5fb29-200">In hello **Service Configuration** list, select hello service configuration that you want tooupdate.</span></span>

    ![Konfigurációs szolgáltatás listája](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. <span data-ttu-id="5fb29-202">tooadd egy egyéni beállítás kiválasztása **beállítás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-202">tooadd a custom setting, select **Add Setting**.</span></span>

    ![Egyéni Alkalmazásbeállítás hozzáadása](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. <span data-ttu-id="5fb29-204">Új beállítás hello toohello lista hozzá lett adva, frissíti hello listában hello sort hello szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="5fb29-204">Once hello new setting has been added toohello list, update hello row in hello list with hello necessary information.</span></span>

    ![Új egyéni beállítása](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - <span data-ttu-id="5fb29-206">**Név** -hello beállítás hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="5fb29-206">**Name** - Enter hello name of hello setting.</span></span>
    - <span data-ttu-id="5fb29-207">**Típus** – Itt adhatja meg **karakterlánc** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="5fb29-207">**Type** - Select **String** from hello drop-down list.</span></span>
    - <span data-ttu-id="5fb29-208">**Érték** -hello hello beállítás értékét adja meg.</span><span class="sxs-lookup"><span data-stu-id="5fb29-208">**Value** - Enter hello value of hello setting.</span></span> <span data-ttu-id="5fb29-209">Megadhat a hello érték közvetlenül a hello **érték** cella, vagy válassza hello három ponttal (…) tooenter hello értéket hello **karakterlánc szerkesztése** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5fb29-209">You can either enter hello value directly into hello **Value** cell, or select hello ellipsis (...) tooenter hello value in hello **Edit String** dialog.</span></span>  

1. <span data-ttu-id="5fb29-210">toodelete egy egyéni beállítás hello beállítás, majd válassza ki és **eltávolítása beállítás**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-210">toodelete a custom setting, select hello setting, and then select **Remove Setting**.</span></span>

1. <span data-ttu-id="5fb29-211">A Visual Studio hello eszköztárában kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-211">From hello Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-access-a-custom-settings-value"></a><span data-ttu-id="5fb29-212">Programon keresztüli eléréséhez az egyéni beállítás értéke</span><span class="sxs-lookup"><span data-stu-id="5fb29-212">Programmatically access a custom setting's value</span></span>
 
<span data-ttu-id="5fb29-213">hello következő lépések bemutatják, hogyan férhetnek hozzá a tooprogrammatically egy C# segítségével egyéni beállítást.</span><span class="sxs-lookup"><span data-stu-id="5fb29-213">hello following steps show how tooprogrammatically access a custom setting using C#.</span></span>

1. <span data-ttu-id="5fb29-214">Adja hozzá a következő hello toouse hello beállítás hová irányelvek tooa C#-fájl használatával:</span><span class="sxs-lookup"><span data-stu-id="5fb29-214">Add hello following using directives tooa C# file where you are going toouse hello setting:</span></span>

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. <span data-ttu-id="5fb29-215">hello alábbi kód bemutatja, hogyan lehet például egy egyéni beállítás tooaccess.</span><span class="sxs-lookup"><span data-stu-id="5fb29-215">hello following code illustrates an example of how tooaccess a custom setting.</span></span> <span data-ttu-id="5fb29-216">Cserélje le a hello &lt;SettingName > helyőrzőt hello megfelelő értékre.</span><span class="sxs-lookup"><span data-stu-id="5fb29-216">Replace hello &lt;SettingName> placeholder with hello appropriate value.</span></span> 
    
    ```csharp
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("<SettingName>");
    ```

## <a name="manage-local-storage-for-each-role-instance"></a><span data-ttu-id="5fb29-217">Minden egyes szerepkörpéldányhoz helyi tárterület kezelése</span><span class="sxs-lookup"><span data-stu-id="5fb29-217">Manage local storage for each role instance</span></span>
<span data-ttu-id="5fb29-218">Egy szerepkör minden egyes példányánál helyi fájl rendszerhez szükséges tárhelyet adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="5fb29-218">You can add local file system storage for each instance of a role.</span></span> <span data-ttu-id="5fb29-219">hello található adatokhoz tároló nem érhető el más példánya hello mely hello vonatkozó adatokat tárolja, vagy más szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="5fb29-219">hello data stored in that storage is not accessible by other instances of hello role for which hello data is stored, or by other roles.</span></span>  

1. <span data-ttu-id="5fb29-220">Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.</span><span class="sxs-lookup"><span data-stu-id="5fb29-220">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="5fb29-221">A **Megoldáskezelőben**, bontsa ki a hello projekt csomópontjára.</span><span class="sxs-lookup"><span data-stu-id="5fb29-221">In **Solution Explorer**, expand hello project node.</span></span> <span data-ttu-id="5fb29-222">A hello **szerepkörök** csomópontot, kattintson a jobb gombbal hello szerepkör tooupdate, és, hello helyi menüből jelöljük ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-222">Under hello **Roles** node, right-click hello role you want tooupdate, and, from hello context menu, select **Properties**.</span></span>

    ![Solution Explorer Azure szerepkör helyi menü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="5fb29-224">Jelölje be hello **helyi tároló** fülre.</span><span class="sxs-lookup"><span data-stu-id="5fb29-224">Select hello **Local Storage** tab.</span></span>

    ![Helyi tárolás lap](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab.png)

1. <span data-ttu-id="5fb29-226">A hello **szolgáltatáskonfiguráció** listában, ügyeljen arra, hogy **összes konfiguráció** van jelölve, hogy hello helyi tárolási beállítások tooall szolgáltatáskonfiguráció alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="5fb29-226">In hello **Service Configuration** list, ensure that **All Configurations** is selected as hello local storage settings apply tooall service configurations.</span></span> <span data-ttu-id="5fb29-227">Semmilyen más érték eredményez az összes hello beviteli mezők hello lapon le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="5fb29-227">Any other value results in all hello input fields on hello page being disabled.</span></span> 

    ![Konfigurációs szolgáltatás listája](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-service-configuration.png)

1. <span data-ttu-id="5fb29-229">a helyi tároló bejegyzés tooadd válassza **hozzáadása helyi tároló**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-229">tooadd a local storage entry, select **Add Local Storage**.</span></span>

    ![Adja hozzá a helyi tároló](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-add-local-storage.png)

1. <span data-ttu-id="5fb29-231">Hello új helyi tároló bejegyzés toohello lista hozzáadva, frissíti hello listában hello sort hello szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="5fb29-231">Once hello new local storage entry has been added toohello list, update hello row in hello list with hello necessary information.</span></span>

    ![Új helyi tárterület-bejegyzés](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-new-local-storage.png)

    - <span data-ttu-id="5fb29-233">**Név** -adja meg, amelyet az toouse a hello új helyi tárolóhoz tartozó hello nevét.</span><span class="sxs-lookup"><span data-stu-id="5fb29-233">**Name** - Enter hello name that you want toouse for hello new local storage.</span></span>
    - <span data-ttu-id="5fb29-234">**Méret (MB)** -adja meg a hello méretét, amelyekre szüksége van a hello új helyi tárolóhoz tartozó MB-ban.</span><span class="sxs-lookup"><span data-stu-id="5fb29-234">**Size (MB)** - Enter hello size in MB that you need for hello new local storage.</span></span>
    - <span data-ttu-id="5fb29-235">**Tiszta a szerepkör újrahasznosítást** – Ez a beállítás tooremove hello adatok hello új helyi tároló válassza ki, amikor hello szerepkör hello virtuális gép újraindul.</span><span class="sxs-lookup"><span data-stu-id="5fb29-235">**Clean on role recycle** - Select this option tooremove hello data in hello new local storage when hello virtual machine for hello role is recycled.</span></span>

1. <span data-ttu-id="5fb29-236">toodelete egy helyi tárolóhoz bejegyzés hello bejegyzést, majd válassza ki és **helyi tárhely eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-236">toodelete a local storage entry, select hello entry, and then select **Remove Local Storage**.</span></span>

1. <span data-ttu-id="5fb29-237">A Visual Studio hello eszköztárában kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-237">From hello Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-accessing-local-storage"></a><span data-ttu-id="5fb29-238">Programozott módon a helyi tároló elérése</span><span class="sxs-lookup"><span data-stu-id="5fb29-238">Programmatically accessing local storage</span></span>

<span data-ttu-id="5fb29-239">Ez a szakasz bemutatja, miként férhetnek hozzá a tooprogrammatically helyi tárolóhely-C# teszt szövegfájlba írásával `MyLocalStorageTest.txt`.</span><span class="sxs-lookup"><span data-stu-id="5fb29-239">This section illustrates how tooprogrammatically access local storage using C# by writing a test text file `MyLocalStorageTest.txt`.</span></span>  

### <a name="write-a-text-file-toolocal-storage"></a><span data-ttu-id="5fb29-240">A szöveges fájl toolocal tárolási írása</span><span class="sxs-lookup"><span data-stu-id="5fb29-240">Write a text file toolocal storage</span></span>

<span data-ttu-id="5fb29-241">a következő kód hello azt szemlélteti, hogyan toowrite a szöveges fájl toolocal tároló.</span><span class="sxs-lookup"><span data-stu-id="5fb29-241">hello following code shows an example of how toowrite a text file toolocal storage.</span></span> <span data-ttu-id="5fb29-242">Cserélje le a hello &lt;LocalStorageName > helyőrzőt hello megfelelő értékre.</span><span class="sxs-lookup"><span data-stu-id="5fb29-242">Replace hello &lt;LocalStorageName> placeholder with hello appropriate value.</span></span> 

    ```csharp
    // Retrieve an object that points toohello local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("<LocalStorageName>");
    
    //Define hello file name and path
    string[] paths = { localResource.RootPath, "MyLocalStorageTest.txt" };
    String filePath = Path.Combine(paths);
    
    using (FileStream writeStream = File.Create(filePath))
    {
        Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
        writeStream.Write(textToWrite, 0, textToWrite.Length);
    }

    ```

### <a name="find-a-file-written-toolocal-storage"></a><span data-ttu-id="5fb29-243">Toolocal tárolási írt fájl keresése</span><span class="sxs-lookup"><span data-stu-id="5fb29-243">Find a file written toolocal storage</span></span>

<span data-ttu-id="5fb29-244">hello kóddal hello előző szakaszban létrehozott tooview hello fájl kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5fb29-244">tooview hello file created by hello code in hello previous section, follow these steps:</span></span>
    
1.  <span data-ttu-id="5fb29-245">A Windows tálca értesítési területén hello, kattintson a jobb gombbal a hello Azure ikon és, hello helyi menüből válassza ki a **Show Compute Emulator felhasználói felületén**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-245">In hello Windows notification area, right-click hello Azure icon, and, from hello context menu, select **Show Compute Emulator UI**.</span></span> 

    ![Az Azure compute emulator megjelenítése](./media/vs-azure-tools-configure-roles-for-cloud-service/show-compute-emulator.png)

1. <span data-ttu-id="5fb29-247">Válassza ki a hello webes szerepkör.</span><span class="sxs-lookup"><span data-stu-id="5fb29-247">Select hello web role.</span></span>

    ![Az Azure compute emulator](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator.png)

1. <span data-ttu-id="5fb29-249">A hello **Microsoft Azure Compute Emulator** menü **eszközök** > **nyissa meg a helyi tárolójába**.</span><span class="sxs-lookup"><span data-stu-id="5fb29-249">On hello **Microsoft Azure Compute Emulator** menu, select **Tools** > **Open local store**.</span></span>

    ![Nyissa meg a helyi tárolójába menüpont](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator-open-local-store-menu.png)

1. <span data-ttu-id="5fb29-251">Amikor megnyílik a hello Windows Explorer ablakot, írja be "MyLocalStorageTest.txt" hello be **keresési** szövegmezőbe, majd válassza ki **adjon meg** toostart hello keresési.</span><span class="sxs-lookup"><span data-stu-id="5fb29-251">When hello Windows Explorer window opens, enter `MyLocalStorageTest.txt`\` into hello **Search** text box, and select **Enter** toostart hello search.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="5fb29-252">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5fb29-252">Next steps</span></span>
<span data-ttu-id="5fb29-253">További tudnivalók a Visual Studio Azure projektek olvasásával [konfigurálása az Azure-projekt](vs-azure-tools-configuring-an-azure-project.md).</span><span class="sxs-lookup"><span data-stu-id="5fb29-253">Learn more about Azure projects in Visual Studio by reading [Configuring an Azure Project](vs-azure-tools-configuring-an-azure-project.md).</span></span> <span data-ttu-id="5fb29-254">További tudnivalók hello cloud service séma olvasásával [Sémareferenciája](https://msdn.microsoft.com/library/azure/dd179398).</span><span class="sxs-lookup"><span data-stu-id="5fb29-254">Learn more about hello cloud service schema by reading [Schema Reference](https://msdn.microsoft.com/library/azure/dd179398).</span></span>

