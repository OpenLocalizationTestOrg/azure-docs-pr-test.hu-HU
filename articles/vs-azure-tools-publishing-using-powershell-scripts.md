---
title: "aaaUsing Windows PowerShell-parancsfájlok tooPublish tooDev és a tesztkörnyezetek |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Windows PowerShell parancsfájl Visual Studio toopublish toodevelopment és tesztkörnyezetek."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5fff1301-5469-4d97-be88-c85c30f837c1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 491a058f96255576afa74f6156f20ae9559bb9f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-windows-powershell-scripts-toopublish-toodev-and-test-environments"></a><span data-ttu-id="70c83-103">A Windows PowerShell parancsfájlok toopublish toodev és tesztkörnyezetek</span><span class="sxs-lookup"><span data-stu-id="70c83-103">Using Windows PowerShell scripts toopublish toodev and test environments</span></span>
<span data-ttu-id="70c83-104">A Visual Studio egy webalkalmazás létrehozásakor egy Windows PowerShell-parancsfájlt, hogy akkor használható a webhely tooAzure a későbbi tooautomate hello közzétételét egy webalkalmazást az Azure App Service vagy egy virtuális gép is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="70c83-104">When you create a web application in Visual Studio, you can generate a Windows PowerShell script that you can use later tooautomate hello publishing of your website tooAzure as a Web App in Azure App Service or a virtual machine.</span></span> <span data-ttu-id="70c83-105">Szerkesztése és a Visual Studio-szerkesztőben toosuit hello hello Windows PowerShell-parancsfájl kiterjesztése a követelmények, vagy hello parancsfájl integrálható a meglévő build, a vizsgálati és a közzétételi parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="70c83-105">You can edit and extend hello Windows PowerShell script in hello Visual Studio editor toosuit your requirements, or integrate hello script with existing build, test, and publishing scripts.</span></span>

<span data-ttu-id="70c83-106">Az ezekhez a parancsfájlokhoz, megadhat testreszabott verziók (más néven a fejlesztési és tesztelési környezetben) a webhely ideiglenes használatra.</span><span class="sxs-lookup"><span data-stu-id="70c83-106">Using these scripts, you can provision customized versions (also known as dev and test environments) of your site for temporary use.</span></span> <span data-ttu-id="70c83-107">Például akkor lehet, hogy állítsa be a webhely adott verziójának Azure virtuális gép vagy egy webhely toorun a tárolóhely átmeneti hello egy tesztcsomag, programhiba Reprodukálja, teszteléséhez hibajavítás, próbaverzió változtatást vagy bemutató vagy bemutató egyéni környezet beállítása.</span><span class="sxs-lookup"><span data-stu-id="70c83-107">For example, you might set up a particular version of your website on an Azure virtual machine or on hello staging slot on a website toorun a test suite, reproduce a bug, test a bug fix, trial a proposed change, or set up a custom environment for a demo or presentation.</span></span> <span data-ttu-id="70c83-108">Olyan parancsfájlt, amely közzéteszi a projekt létrehozása után hozza létre újra az azonos környezetben újra hello parancsfájl futtatásával igény szerint, vagy a webes alkalmazás toocreate a saját build hello parancsfájl futtassa a teszteléshez egyéni környezet.</span><span class="sxs-lookup"><span data-stu-id="70c83-108">After you've created a script that publishes your project, you can recreate identical environments by re-running hello script as needed, or run hello script with your own build of your web application toocreate a custom environment for testing.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="70c83-109">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="70c83-109">What you need</span></span>
* <span data-ttu-id="70c83-110">Az Azure SDK 2.3-as vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="70c83-110">Azure SDK 2.3 or later.</span></span> <span data-ttu-id="70c83-111">Lásd: [Visual Studio letöltések](http://go.microsoft.com/fwlink/?LinkID=624384) további információt.</span><span class="sxs-lookup"><span data-stu-id="70c83-111">See [Visual Studio Downloads](http://go.microsoft.com/fwlink/?LinkID=624384) for more information.</span></span>

<span data-ttu-id="70c83-112">Webes projektek hello Azure SDK toogenerate hello parancsfájlok nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="70c83-112">You do not need hello Azure SDK toogenerate hello scripts for web projects.</span></span> <span data-ttu-id="70c83-113">Ez a szolgáltatás webes projektek, a felhőalapú szolgáltatások nem webes szerepkörök szolgál.</span><span class="sxs-lookup"><span data-stu-id="70c83-113">This feature is for web projects, not web roles in cloud services.</span></span>

* <span data-ttu-id="70c83-114">Az Azure PowerShell 0.7.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="70c83-114">Azure PowerShell 0.7.4 or later.</span></span> <span data-ttu-id="70c83-115">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) további információt.</span><span class="sxs-lookup"><span data-stu-id="70c83-115">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span>
* <span data-ttu-id="70c83-116">[A Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="70c83-116">[Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) or later.</span></span>

## <a name="additional-tools"></a><span data-ttu-id="70c83-117">További eszközök</span><span class="sxs-lookup"><span data-stu-id="70c83-117">Additional tools</span></span>
<span data-ttu-id="70c83-118">További eszközök és erőforrások használatához a PowerShell használatával a Visual Studio Azure fejlesztési érhetők el.</span><span class="sxs-lookup"><span data-stu-id="70c83-118">Additional tools and resources for working with PowerShell in Visual Studio for Azure development are available.</span></span> <span data-ttu-id="70c83-119">Lásd: [PowerShell Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).</span><span class="sxs-lookup"><span data-stu-id="70c83-119">See [PowerShell Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).</span></span>

## <a name="generating-hello-publish-scripts"></a><span data-ttu-id="70c83-120">Parancsfájlok hello generálása közzététele</span><span class="sxs-lookup"><span data-stu-id="70c83-120">Generating hello publish scripts</span></span>
<span data-ttu-id="70c83-121">Hello is létrehozhat egy virtuális gép, amelyen a webhely, amikor követve hozzon létre egy új projektet parancsfájlok közzététele [ezeket az utasításokat](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="70c83-121">You can generate hello publish scripts for a virtual machine that hosts your website when you create a new project by following [these instructions](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="70c83-122">Emellett [készítése az Azure App Service web Apps parancsfájlok közzététele](app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="70c83-122">You can also [generate publish scripts for web apps in Azure App Service](app-service-web/app-service-web-get-started-dotnet.md).</span></span>

## <a name="scripts-that-visual-studio-generates"></a><span data-ttu-id="70c83-123">Olyan parancsfájlok, amelyek a Visual Studio hoz létre</span><span class="sxs-lookup"><span data-stu-id="70c83-123">Scripts that Visual Studio generates</span></span>
<span data-ttu-id="70c83-124">A Visual Studio megoldás szintű mappa állít elő **PublishScripts** két Windows PowerShell-fájlok, a virtuális gép vagy a webhely és a modult tartalmaz, amelyek segítségével is hello funkciók közzététel parancsfájlt tartalmazó parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="70c83-124">Visual Studio generates a solution-level folder called **PublishScripts** that contains two Windows PowerShell files, a publish script for your virtual machine or website, and a module that contains functions that you can use in hello scripts.</span></span> <span data-ttu-id="70c83-125">A Visual Studio is létrehoz egy fájlt, amely meghatározza a hello projektet telepít hello részleteit hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="70c83-125">Visual Studio also generates a file in hello JSON format that specifies hello details of hello project you are deploying.</span></span>

### <a name="windows-powershell-publish-script"></a><span data-ttu-id="70c83-126">A Windows PowerShell parancsfájl közzététele</span><span class="sxs-lookup"><span data-stu-id="70c83-126">Windows PowerShell publish script</span></span>
<span data-ttu-id="70c83-127">hello közzététele a parancsfájl tartalmazza a megadott közzététele tooa webhelyére vagy a virtuális gép telepítésének lépései.</span><span class="sxs-lookup"><span data-stu-id="70c83-127">hello publish script contains specific publish steps for deploying tooa website or virtual machine.</span></span> <span data-ttu-id="70c83-128">A Visual Studio biztosít a Windows PowerShell fejlesztési színátmenetekhez szintaxist.</span><span class="sxs-lookup"><span data-stu-id="70c83-128">Visual Studio provides syntax coloring for Windows PowerShell development.</span></span> <span data-ttu-id="70c83-129">Súgó hello funkciók érhető el, és szabadon szerkeszthető hello parancsfájl toosuit hello funkciók a változó követelményeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="70c83-129">Help for hello functions is available, and you can freely edit hello functions in hello script toosuit your changing requirements.</span></span>

### <a name="windows-powershell-module"></a><span data-ttu-id="70c83-130">A Windows PowerShell-modul</span><span class="sxs-lookup"><span data-stu-id="70c83-130">Windows PowerShell module</span></span>
<span data-ttu-id="70c83-131">a Windows PowerShell modul, amely a Visual Studio hoz létre olyan függvényeket tartalmaz, hello hello közzététele parancsfájlt használ.</span><span class="sxs-lookup"><span data-stu-id="70c83-131">hello Windows PowerShell module that Visual Studio generates contains functions that hello publish script uses.</span></span> <span data-ttu-id="70c83-132">Azok az Azure PowerShell-funkciók és nem tervezett toobe módosítani.</span><span class="sxs-lookup"><span data-stu-id="70c83-132">These are Azure PowerShell functions and are not intended toobe modified.</span></span> <span data-ttu-id="70c83-133">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) további információt.</span><span class="sxs-lookup"><span data-stu-id="70c83-133">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span>

### <a name="json-configuration-file"></a><span data-ttu-id="70c83-134">JSON-konfigurációs fájlt</span><span class="sxs-lookup"><span data-stu-id="70c83-134">JSON configuration file</span></span>
<span data-ttu-id="70c83-135">hello JSON-fájl jön létre hello **konfigurációk** mappa, és pontosan mely erőforrások toodeploy tooAzure megadó konfigurációs adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="70c83-135">hello JSON file is created in hello **Configurations** folder and contains configuration data that specifies exactly which resources toodeploy tooAzure.</span></span> <span data-ttu-id="70c83-136">hello hello fájl neve, amely állít elő, a Visual Studio projekt-neve-WAWS-dev.json Ha létrehozott egy webhely vagy a név-VM-dev.json projekt Ha létrehozott egy virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="70c83-136">hello name of hello file that Visual Studio generates is project-name-WAWS-dev.json if you created a website, or project name-VM-dev.json if you created a virtual machine.</span></span> <span data-ttu-id="70c83-137">Íme egy példa egy JSON-konfigurációs fájlt, amely jön létre, amikor egy webhely létrehozása.</span><span class="sxs-lookup"><span data-stu-id="70c83-137">Here's an example of a JSON configuration file that's generated when you create a website.</span></span> <span data-ttu-id="70c83-138">Hello értékek többsége magától értetődő.</span><span class="sxs-lookup"><span data-stu-id="70c83-138">Most of hello values are self-explanatory.</span></span> <span data-ttu-id="70c83-139">webhely neve hello Azure, hozza létre, akkor előfordulhat, hogy a projekt neve nem egyezik.</span><span class="sxs-lookup"><span data-stu-id="70c83-139">hello website name is generated by Azure, so it might not match your project name.</span></span>

```json
{
    "environmentSettings": {
        "webSite": {
            "name": "WebApplication26632",
            "location": "West US"
        },
        "databases": [{
            "connectionStringName": "DefaultConnection",
            "databaseName": "WebApplication26632_db",
            "serverName": "YourDatabaseServerName",
            "user": "sqluser2",
            "password": "",
            "edition": "",
            "size": "",
            "collation": "",
            "location": "West US"
        }]
    }
}
```
<span data-ttu-id="70c83-140">A virtuális gép létrehozásakor hello JSON-konfigurációs fájl következőhöz hasonló toohello következő.</span><span class="sxs-lookup"><span data-stu-id="70c83-140">When you create a virtual machine, hello JSON configuration file looks similar toohello following.</span></span> <span data-ttu-id="70c83-141">Vegye figyelembe, hogy egy felhőalapú szolgáltatás hello virtuális gép elhelyezése jön létre.</span><span class="sxs-lookup"><span data-stu-id="70c83-141">Note that a cloud service is created as a container for hello virtual machine.</span></span> <span data-ttu-id="70c83-142">hello a virtuális gépben hello szokásos végpontok web Access HTTP és HTTPS használatával, valamint a Web Deploy, amely lehetővé teszi, hogy a helyi számítógépen, a távoli asztal és a Windows PowerShell toohello webhelyet közzéteheti végpontok.</span><span class="sxs-lookup"><span data-stu-id="70c83-142">hello virtual machine contains hello usual endpoints for web access through HTTP and HTTPS, as well as endpoints for Web Deploy, which lets you publish toohello website from your local machine, Remote Desktop, and Windows PowerShell.</span></span>

```json
{
    "environmentSettings": {
        "cloudService": {
            "name": "myusernamevm1",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myusernamevm1",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [{
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [{
            "connectionStringName": "",
            "databaseName": "",
            "serverName": "",
            "user": "",
            "password": ""
        }],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

<span data-ttu-id="70c83-143">Szerkesztheti hello JSON konfigurációs toochange mi történik, ha futtatja a hello parancsfájlok közzététele.</span><span class="sxs-lookup"><span data-stu-id="70c83-143">You can edit hello JSON configuration toochange what happens when you run hello publish scripts.</span></span> <span data-ttu-id="70c83-144">Hello `cloudService` és `virtualMachine` szakasz használata kötelező, de törölheti hello `databases` szakaszban, ha nincs szüksége.</span><span class="sxs-lookup"><span data-stu-id="70c83-144">hello `cloudService` and `virtualMachine` sections are required, but you can delete hello `databases` section if you don't need it.</span></span> <span data-ttu-id="70c83-145">hello tulajdonságokhoz üres hello alapértelmezett konfigurációs fájl, amely a Visual Studio létrehozza a rendszer kötelező megadni. értékek rendelkeznek hello alapértelmezett konfigurációs fájlban szükség.</span><span class="sxs-lookup"><span data-stu-id="70c83-145">hello properties that are empty in hello default configuration file that Visual Studio generates are optional; those that have values in hello default configuration file are required.</span></span>

<span data-ttu-id="70c83-146">Ha olyan webhelyet, ahol több telepítési környezetekben (más néven üzembe helyezési ponti) helyett egy egyetlen munkakörnyezeti helyet az Azure-ban, hello tárhely neve is megadhat a hello neve hello webhely hello JSON-konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="70c83-146">If you have a website that has multiple deployment environments (known as slots) instead of a single production site in Azure, you can include hello slot name in hello name of hello website in hello JSON configuration file.</span></span> <span data-ttu-id="70c83-147">Például, ha rendelkezik nevű webhellyel **saját webhely** és tárhely, nevű **tesztelése** majd hello URI saját webhely-test.cloudapp.net, de hello megfelelő nevet toouse hello konfigurációs fájlban mysite(test) .</span><span class="sxs-lookup"><span data-stu-id="70c83-147">For example, if you have a website that's named **mysite** and a slot for it named **test** then hello URI is mysite-test.cloudapp.net, but hello correct name toouse in hello configuration file is mysite(test).</span></span> <span data-ttu-id="70c83-148">Csak ehhez Ha hello webhely és a tárolóhelyek már létezik az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="70c83-148">You can only do this if hello website and slots already exist in your subscription.</span></span> <span data-ttu-id="70c83-149">Ha azok még nem léteznek, hello tárolóhely megadása nélkül hello parancsfájl futtatásával hello webhelyet hoz létre, majd hozzon létre a hello hello tárolóhely [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), ezt követően futtassa hello parancsfájl hello módosított webhely neve.</span><span class="sxs-lookup"><span data-stu-id="70c83-149">If they don't exist, create hello website by running hello script without specifying hello slot, then create hello slot in hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), and thereafter run hello script with hello modified website name.</span></span> <span data-ttu-id="70c83-150">A web Apps üzembe helyezési kapcsolatos további információkért lásd: [átmeneti környezet az Azure App Service web Apps beállítása](app-service-web/web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="70c83-150">For more information about deployment slots for web apps, see [Set up staging environments for web apps in Azure App Service](app-service-web/web-sites-staged-publishing.md).</span></span>

## <a name="how-toorun-hello-publish-scripts"></a><span data-ttu-id="70c83-151">Hogyan toorun hello közzététele parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="70c83-151">How toorun hello publish scripts</span></span>
<span data-ttu-id="70c83-152">Soha ne futtatása előtt egy Windows PowerShell-parancsfájlt, ha hello végrehajtási házirend tooenable parancsfájlok toorun először meg kell adnia.</span><span class="sxs-lookup"><span data-stu-id="70c83-152">If you have never run a Windows PowerShell script before, you must first set hello execution policy tooenable scripts toorun.</span></span> <span data-ttu-id="70c83-153">Ez az egy biztonsági funkció tooprevent felhasználók Windows PowerShell-parancsfájlok futtatását, ha sebezhető toomalware vagy a vírusok, például a parancsfájlok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="70c83-153">This is a security feature tooprevent users from running Windows PowerShell scripts if they're vulnerable toomalware or viruses that involve executing scripts.</span></span>

### <a name="run-hello-script"></a><span data-ttu-id="70c83-154">Hello parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="70c83-154">Run hello script</span></span>
1. <span data-ttu-id="70c83-155">A projekt hello Web Deploy csomagot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="70c83-155">Create hello Web Deploy package for your project.</span></span> <span data-ttu-id="70c83-156">A Web Deploy csomag egy tömörített (.zip fájl), amelyet toocopy tooyour webhelyére vagy a virtuális gép fájlokat tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="70c83-156">A Web Deploy package is a compressed archive (.zip file) that contain files that you want toocopy tooyour website or virtual machine.</span></span> <span data-ttu-id="70c83-157">A Visual Studio Web Deploy platformra alapuló csomagok bármely webalkalmazás hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="70c83-157">You can create Web Deploy packages in Visual Studio for any web application.</span></span>

![Webes hozható létre csomag telepítése](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

<span data-ttu-id="70c83-159">További információkért lásd: [Útmutató: webes telepítési csomag létrehozása a Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span><span class="sxs-lookup"><span data-stu-id="70c83-159">For more information, see [How to: Create a Web Deployment Package in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span></span> <span data-ttu-id="70c83-160">Automatizálható a Web Deploy csomag létrehozása hello hello szakaszban leírt módon **testreszabása és hello kiterjesztése közzététele parancsfájlok** a témakör későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="70c83-160">You can also automate hello creation of your Web Deploy package, as described in hello section **Customizing and extending hello publish scripts** later in this topic.</span></span>

1. <span data-ttu-id="70c83-161">A **Megoldáskezelőben**, nyissa meg a helyi menü hello hello parancsfájlt, és válassza **nyissa meg a PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="70c83-161">In **Solution Explorer**, open hello context menu for hello script, and then choose **Open with PowerShell ISE**.</span></span>
2. <span data-ttu-id="70c83-162">Ha hello első alkalommal ezen a számítógépen már futtatta a Windows PowerShell-parancsfájlok, nyisson meg egy parancssori ablakot rendszergazdai jogosultságokkal és típus hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="70c83-162">If this is hello first time you've run Windows PowerShell scripts on this computer, open a command prompt window with Administrator privileges and type hello following command:</span></span>

    ```powershell
    Set-ExecutionPolicy RemoteSigned
    ```

3. <span data-ttu-id="70c83-163">Jelentkezzen be tooAzure hello a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="70c83-163">Sign in tooAzure by using hello following command.</span></span>

    ```powershell
    Add-AzureAccount
    ```

    <span data-ttu-id="70c83-164">Amikor a rendszer kéri, adja meg a felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="70c83-164">When prompted, supply your username and password.</span></span>

    <span data-ttu-id="70c83-165">Vegye figyelembe, hogy ha automatizálni hello parancsfájl, ez a módszer az Azure hitelesítő adatokat adjanak nem fognak működni.</span><span class="sxs-lookup"><span data-stu-id="70c83-165">Note that when you automate hello script, this method of providing Azure credentials won't work.</span></span> <span data-ttu-id="70c83-166">Ehelyett használjon hello .publishsettings fájlt tooprovide hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="70c83-166">Instead, you should use hello .publishsettings file tooprovide credentials.</span></span> <span data-ttu-id="70c83-167">Egy alkalommal csak, paranccsal hello **Get-AzurePublishSettingsFile** toodownload hello fájlt az Azure-ból, és ezt követően használható **Import-AzurePublishSettingsFile** tooimport hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="70c83-167">One time only, you use hello command **Get-AzurePublishSettingsFile** toodownload hello file from Azure, and thereafter use **Import-AzurePublishSettingsFile** tooimport hello file.</span></span> <span data-ttu-id="70c83-168">Részletes útmutatásért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="70c83-168">For detailed instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

4. <span data-ttu-id="70c83-169">(Választható) Ha azt szeretné, hogy toocreate Azure erőforrások, például a hello virtuális gépet, az adatbázis és a webhely anélkül, hogy a webalkalmazás közzétételét hello használata **Publish-WebApplication.ps1** hello parancsot **-konfiguráció** argumentum kötelező értéke toohello JSON-konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="70c83-169">(Optional) If you want toocreate Azure resources such as hello virtual machine, database, and website without publishing your web application, use hello **Publish-WebApplication.ps1** command with hello **-Configuration** argument set toohello JSON configuration file.</span></span> <span data-ttu-id="70c83-170">Ez a parancssor a mely erőforrások toocreate hello JSON-konfigurációs fájl toodetermine használja.</span><span class="sxs-lookup"><span data-stu-id="70c83-170">This command line uses hello JSON configuration file toodetermine which resources toocreate.</span></span> <span data-ttu-id="70c83-171">A más parancssori argumentumok hello alapértelmezett beállításait használja, mert hoz létre hello erőforrásokat, de nem tegye közzé a webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="70c83-171">Because it uses hello default settings for other command-line arguments, it creates hello resources, but doesn't publish your web application.</span></span> <span data-ttu-id="70c83-172">hello – részletes kimenet lehetővé teszi az eseményekről további információt.</span><span class="sxs-lookup"><span data-stu-id="70c83-172">hello –Verbose option gives you more information about what's happening.</span></span>

    ```powershell
    Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json
    ```

5. <span data-ttu-id="70c83-173">Használjon hello **Publish-WebApplication.ps1** látható módon egy hello példák tooinvoke hello parancsfájl a következő parancsot, és tegye közzé a webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="70c83-173">Use hello **Publish-WebApplication.ps1** command as shown in one of hello following examples tooinvoke hello script and publish your web application.</span></span> <span data-ttu-id="70c83-174">Ha toooverride hello alapértelmezett beállítások hello bármely egyéb argumentumok hello előfizetés nevét, például közzététele a csomag neve, a virtuális gép hitelesítő adatait, vagy az adatbázis hitelesítő adatait, ezeket a paramétereket is megadhat.</span><span class="sxs-lookup"><span data-stu-id="70c83-174">If you need toooverride hello default settings for any of hello other arguments, such as hello subscription name, publish package name, virtual machine credentials, or database server credentials, you can specify those parameters.</span></span> <span data-ttu-id="70c83-175">Használjon hello **– részletes** toosee további információt a közzétételi folyamat hello hello folyamatban lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="70c83-175">Use hello **–Verbose** option toosee more information about hello progress of hello publishing process.</span></span>

    ```powershell
    Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
    –SubscriptionName Contoso `
    -WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
    -DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
    -Verbose
    ```

    <span data-ttu-id="70c83-176">Ha egy virtuális gépet hoz létre, a hello parancs következőhöz hasonló hello.</span><span class="sxs-lookup"><span data-stu-id="70c83-176">If you're creating a virtual machine, hello command looks like hello following.</span></span> <span data-ttu-id="70c83-177">Ez a példa azt is bemutatja, hogyan toospecify hello több adatbázis hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="70c83-177">This example also shows how toospecify hello credentials for multiple databases.</span></span> <span data-ttu-id="70c83-178">Ezek a parancsfájlok hozzon létre virtuális gépek hello hello SSL-tanúsítvány származik nem egy megbízható legfelső szintű hitelesítésszolgáltatóval.</span><span class="sxs-lookup"><span data-stu-id="70c83-178">For hello virtual machines that these scripts create, hello SSL certificate is not from a trusted root authority.</span></span> <span data-ttu-id="70c83-179">Ezért figyelembe kell toouse hello **– AllowUntrusted** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="70c83-179">Therefore, you need toouse hello **–AllowUntrusted** option.</span></span>

    ```powershell
    Publish-WebApplication.ps1 `
    -Configuration C:\Path\ADVM-VM-test.json `
    -SubscriptionName Contoso `
    -WebDeployPackage C:\Path\ADVM.zip `
    -AllowUntrusted `
    -VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
    -DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
    -Verbose
    ```

    <span data-ttu-id="70c83-180">hello parancsfájl adatbázisokat hozhat létre, de az adatbázis-kiszolgálók nem hoz létre.</span><span class="sxs-lookup"><span data-stu-id="70c83-180">hello script can create databases, but it doesn't create database servers.</span></span> <span data-ttu-id="70c83-181">Ha azt szeretné, hogy egy adatbázis-kiszolgáló toocreate, használhatja a hello **New-AzureSqlDatabaseServer** hello Azure modul függvényt.</span><span class="sxs-lookup"><span data-stu-id="70c83-181">If you want toocreate a database server, you can use hello **New-AzureSqlDatabaseServer** function in hello Azure module.</span></span>

## <a name="customizing-and-extending-hello-publish-scripts"></a><span data-ttu-id="70c83-182">Hello bővítik a parancsfájlok közzététele</span><span class="sxs-lookup"><span data-stu-id="70c83-182">Customizing and extending hello publish scripts</span></span>
<span data-ttu-id="70c83-183">Testre szabhatja a hello teszik közzé a parancsfájl és a JSON-konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="70c83-183">You can customize hello publish script and JSON configuration file.</span></span> <span data-ttu-id="70c83-184">Windows PowerShell-moduljának hello funkciók hello **AzureWebAppPublishModule.psm1** nincsenek tervezett toobe módosítani.</span><span class="sxs-lookup"><span data-stu-id="70c83-184">hello functions in hello Windows PowerShell module **AzureWebAppPublishModule.psm1** are not intended toobe modified.</span></span> <span data-ttu-id="70c83-185">Ha most szeretné, hogy egy másik adatbázishoz toospecify vagy néhány hello hello virtuális gép tulajdonságainak módosítása, szerkessze a hello JSON-konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="70c83-185">If you just want toospecify a different database or change some of hello properties of hello virtual machine, edit hello JSON configuration file.</span></span> <span data-ttu-id="70c83-186">Ha azt szeretné, hogy hello parancsfájl tooautomate összeállításakor és tesztelésekor a projekt tooextend hello funkcióit, a függvény kódcsonkok Megvalósíthat **Publish-WebApplication.ps1**.</span><span class="sxs-lookup"><span data-stu-id="70c83-186">If you want tooextend hello functionality of hello script tooautomate building and testing your project, you can implement function stubs in **Publish-WebApplication.ps1**.</span></span>

<span data-ttu-id="70c83-187">a projekt tooautomate túl MSBuild behívó kód hozzáadása`New-WebDeployPackage` a kód példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="70c83-187">tooautomate building your project, add code that calls MSBuild too`New-WebDeployPackage` as shown in this code example.</span></span> <span data-ttu-id="70c83-188">hello elérési toohello MSBuild parancs nem egyezik a telepített Visual Studio hello verziójától függően.</span><span class="sxs-lookup"><span data-stu-id="70c83-188">hello path toohello MSBuild command is different depending on hello version of Visual Studio you have installed.</span></span> <span data-ttu-id="70c83-189">tooget hello a helyes elérési utat, hello funkció használata **Get-MSBuildCmd**, ebben a példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="70c83-189">tooget hello correct path, you can use hello function **Get-MSBuildCmd**, as shown in this example.</span></span>

### <a name="tooautomate-building-your-project"></a><span data-ttu-id="70c83-190">a projekt tooautomate</span><span class="sxs-lookup"><span data-stu-id="70c83-190">tooautomate building your project</span></span>
1. <span data-ttu-id="70c83-191">Adja hozzá a hello `$ProjectFile` paraméter hello globális param szakaszban.</span><span class="sxs-lookup"><span data-stu-id="70c83-191">Add hello `$ProjectFile` parameter in hello global param section.</span></span>

    ```powershell
    [Parameter(Mandatory = $false)]
    [ValidateScript({Test-Path $_ -PathType Leaf})]
    [String]
    $ProjectFile,
    ```

2. <span data-ttu-id="70c83-192">Hello függvény másolni `Get-MSBuildCmd` a parancsfájl-fájlba.</span><span class="sxs-lookup"><span data-stu-id="70c83-192">Copy hello function `Get-MSBuildCmd` into your script file.</span></span>

    ```powershell
    function Get-MSBuildCmd
    {
            process
    {

                $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                    Sort-Object {[double]$_.PSChildName} -Descending |
                                    Select-Object -First 1 |
                                    Get-ItemProperty -Name MSBuildToolsPath |
                                    Select -ExpandProperty MSBuildToolsPath

                $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

            return Get-Item $path
        }
    }
    ```

3. <span data-ttu-id="70c83-193">Cserélje le `New-WebDeployPackage` hello az alábbi kód, és hello sor hozhat létre hello helyőrzőt cserélje le a `$msbuildCmd`.</span><span class="sxs-lookup"><span data-stu-id="70c83-193">Replace `New-WebDeployPackage` with hello following code and replace hello placeholders in hello line constructing `$msbuildCmd`.</span></span> <span data-ttu-id="70c83-194">Ez a kód a Visual Studio 2015 van.</span><span class="sxs-lookup"><span data-stu-id="70c83-194">This code is for Visual Studio 2015.</span></span> <span data-ttu-id="70c83-195">Ha a Visual Studio 2013 használ, módosítsa a hello **VisualStudioVersion** az alábbi tulajdonság túl`12.0`.</span><span class="sxs-lookup"><span data-stu-id="70c83-195">If you're using Visual Studio 2013, change hello **VisualStudioVersion** property below too`12.0`.</span></span>

    ```powershell
    function New-WebDeployPackage
    {
        #Write a function toobuild and package your web application
    ```

    <span data-ttu-id="70c83-196">toobuild a webalkalmazást, MsBuild.exe használja.</span><span class="sxs-lookup"><span data-stu-id="70c83-196">toobuild your web application, use MsBuild.exe.</span></span> <span data-ttu-id="70c83-197">Útmutatásért lásd: a MSBuild parancssori útmutatóban: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)</span><span class="sxs-lookup"><span data-stu-id="70c83-197">For help, see MSBuild Command-Line Reference at: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)</span></span>

    ```powershell
    Write-VerboseWithTime 'Build-WebDeployPackage: Start'

    $msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory

    Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
    ```

### <a name="start-execution-of-hello-build-command"></a><span data-ttu-id="70c83-198">Hello build parancs végrehajtásának indítása</span><span class="sxs-lookup"><span data-stu-id="70c83-198">Start execution of hello build command</span></span>

```powershell
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru

if ($job.ExitCode -ne 0) {
    throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain hello project name
$projectName = (Get-Item $ProjectFile).BaseName

#Construct hello path tooweb deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName

#Get hello full path for hello web deploy zip package. This is required for MSDeploy toowork
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir

Write-VerboseWithTime 'Build-WebDeployPackage: End'

return $WebDeployPackage
}
```

1. <span data-ttu-id="70c83-199">Hello hívás `New-WebDeployPackage` függvény előtt a sor: `$Config = Read-ConfigFile $Configuration` web Apps vagy `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="70c83-199">Call hello `New-WebDeployPackage` function before this line: `$Config = Read-ConfigFile $Configuration` for web apps or `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` for virtual machines.</span></span>

    ```powershell
    if($ProjectFile)
    {
    $WebDeployPackage = New-WebDeployPackage
    }
    ```

2. <span data-ttu-id="70c83-200">Testre szabott hello parancsfájl használata a sikeres hello parancssorból meghívása `$Project` argumentum, mint a következő példa parancssori hello.</span><span class="sxs-lookup"><span data-stu-id="70c83-200">Invoke hello customized script from command line using passing hello `$Project` argument, as in hello following example command line.</span></span>

    ```powershell
    .\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
    -ProjectFile ..\WebApplication5\WebApplication5.csproj `
    -VMPassword @{Name="VMUser";Password="Test.123"} `
    -AllowUntrusted `
    -Verbose
    ```

    <span data-ttu-id="70c83-201">az alkalmazás tesztelése tooautomate hozzáadása kód túl`Test-WebApplication`.</span><span class="sxs-lookup"><span data-stu-id="70c83-201">tooautomate testing of your application, add code too`Test-WebApplication`.</span></span> <span data-ttu-id="70c83-202">Lehet, hogy toouncomment hello sorainak **Publish-WebApplication.ps1** ahol ezek a funkciók nevezzük.</span><span class="sxs-lookup"><span data-stu-id="70c83-202">Be sure toouncomment hello lines in **Publish-WebApplication.ps1** where these functions are called.</span></span> <span data-ttu-id="70c83-203">Ha nem ad meg megvalósítást, manuálisan hozhat létre a projektet a Visual Studio, és hogy a futási hello tegye közzé parancsfájl toopublish tooAzure.</span><span class="sxs-lookup"><span data-stu-id="70c83-203">If you don't provide an implementation, you can manually build your project with Visual Studio, and then run hello publish script toopublish tooAzure.</span></span>

## <a name="publishing-function-summary"></a><span data-ttu-id="70c83-204">Közzétételi összesítő függvény</span><span class="sxs-lookup"><span data-stu-id="70c83-204">Publishing function summary</span></span>
<span data-ttu-id="70c83-205">hello Windows PowerShell parancssorába, használható függvények tooget súgóját hello paranccsal `Get-Help function-name`.</span><span class="sxs-lookup"><span data-stu-id="70c83-205">tooget help for functions you can use at hello Windows PowerShell command prompt, use hello command `Get-Help function-name`.</span></span> <span data-ttu-id="70c83-206">hello súgó paraméter Súgó és példákat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="70c83-206">hello help includes parameter help and examples.</span></span> <span data-ttu-id="70c83-207">azonos súgószöveg egyben hello parancsfájl forrásfájlok hello **AzureWebAppPublishModule.psm1** és **Publish-WebApplication.ps1**.</span><span class="sxs-lookup"><span data-stu-id="70c83-207">hello same help text is also in hello script source files, **AzureWebAppPublishModule.psm1** and **Publish-WebApplication.ps1**.</span></span> <span data-ttu-id="70c83-208">hello parancsfájl és súgó a Visual Studio nyelven honosítva vannak.</span><span class="sxs-lookup"><span data-stu-id="70c83-208">hello script and help are localized in your Visual Studio language.</span></span>

<span data-ttu-id="70c83-209">**AzureWebAppPublishModule**</span><span class="sxs-lookup"><span data-stu-id="70c83-209">**AzureWebAppPublishModule**</span></span>

| <span data-ttu-id="70c83-210">Függvény neve</span><span class="sxs-lookup"><span data-stu-id="70c83-210">Function name</span></span> | <span data-ttu-id="70c83-211">Leírás</span><span class="sxs-lookup"><span data-stu-id="70c83-211">Description</span></span> |
| --- | --- |
| <span data-ttu-id="70c83-212">Adja hozzá AzureSQLDatabase</span><span class="sxs-lookup"><span data-stu-id="70c83-212">Add-AzureSQLDatabase</span></span> |<span data-ttu-id="70c83-213">Létrehoz egy új Azure SQL-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="70c83-213">Creates a new Azure SQL database.</span></span> |
| <span data-ttu-id="70c83-214">Adja hozzá AzureSQLDatabases</span><span class="sxs-lookup"><span data-stu-id="70c83-214">Add-AzureSQLDatabases</span></span> |<span data-ttu-id="70c83-215">Hello értékek hello JSON-konfigurációs fájlt, amely a Visual Studio létrehozza az Azure SQL adatbázisokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="70c83-215">Creates Azure SQL databases from hello values in hello JSON configuration file that Visual Studio generates.</span></span> |
| <span data-ttu-id="70c83-216">Adja hozzá AzureVM</span><span class="sxs-lookup"><span data-stu-id="70c83-216">Add-AzureVM</span></span> |<span data-ttu-id="70c83-217">Létrehoz egy Azure virtuális gépet, és visszaadja a hello hello URL-CÍMÉT, központilag telepített virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="70c83-217">Creates a Azure virtual machine and returns hello URL of hello deployed VM.</span></span> <span data-ttu-id="70c83-218">hello függvény állít be hello Előfeltételek és hívások hello **New-AzureVM** (Azure modul) toocreate egy új virtuális gép működik.</span><span class="sxs-lookup"><span data-stu-id="70c83-218">hello function sets up hello prerequisites and then calls hello **New-AzureVM** function (Azure module) toocreate a new virtual machine.</span></span> |
| <span data-ttu-id="70c83-219">Adja hozzá AzureVMEndpoints</span><span class="sxs-lookup"><span data-stu-id="70c83-219">Add-AzureVMEndpoints</span></span> |<span data-ttu-id="70c83-220">Hozzáadja a bemeneti végpontok tooa új virtuális gép, és a virtuális gép hello hello új végpont beolvasása.</span><span class="sxs-lookup"><span data-stu-id="70c83-220">Adds new input endpoints tooa virtual machine and returns hello virtual machine with hello new endpoint.</span></span> |
| <span data-ttu-id="70c83-221">Adja hozzá AzureVMStorage</span><span class="sxs-lookup"><span data-stu-id="70c83-221">Add-AzureVMStorage</span></span> |<span data-ttu-id="70c83-222">Ebben az előfizetésben hello hoz létre egy új Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="70c83-222">Creates a new Azure storage account in hello current subscription.</span></span> <span data-ttu-id="70c83-223">hello fiók hello neve kezdődik "devtest" egyedi alfanumerikus karakterlánc követ.</span><span class="sxs-lookup"><span data-stu-id="70c83-223">hello name of hello account begins with "devtest" followed by a unique alphanumeric string.</span></span> <span data-ttu-id="70c83-224">hello függvény hello új tárfiók hello nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="70c83-224">hello function returns hello name of hello new storage account.</span></span> <span data-ttu-id="70c83-225">Adjon meg egy helyet vagy affinitáscsoport hello új tárfiók.</span><span class="sxs-lookup"><span data-stu-id="70c83-225">You must specify either a location or an affinity group for hello new storage account.</span></span> |
| <span data-ttu-id="70c83-226">Adja hozzá AzureWebsite</span><span class="sxs-lookup"><span data-stu-id="70c83-226">Add-AzureWebsite</span></span> |<span data-ttu-id="70c83-227">Hello megadott név és hely egy webhelyet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="70c83-227">Creates a website with hello specified name and location.</span></span> <span data-ttu-id="70c83-228">Ez a funkció meghívja a hello **New-AzureWebsite** hello Azure modul függvényt.</span><span class="sxs-lookup"><span data-stu-id="70c83-228">This function calls hello **New-AzureWebsite** function in hello Azure module.</span></span> <span data-ttu-id="70c83-229">Ha hello előfizetés már nem tartalmaz egy webhely hello megadott névvel, a függvény hello webhelyet hoz létre, és egy webhely objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="70c83-229">If hello subscription doesn't already include a website with hello specified name, this function creates hello website and returns a website object.</span></span> <span data-ttu-id="70c83-230">Ellenkező esetben az eredmény `$null`.</span><span class="sxs-lookup"><span data-stu-id="70c83-230">Otherwise, it returns `$null`.</span></span> |
| <span data-ttu-id="70c83-231">Backup-előfizetés</span><span class="sxs-lookup"><span data-stu-id="70c83-231">Backup-Subscription</span></span> |<span data-ttu-id="70c83-232">Menti az aktuális Azure-előfizetés a hello hello `$Script:originalSubscription` változó parancsfájl hatókörében. Ez a funkció menti hello jelenlegi Azure-előfizetés (módon nyert `Get-AzureSubscription -Current`) és a tárfiók, és hello előfizetés módosítja ezt a parancsfájlt (hello változó tárolja `$UserSpecifiedSubscription`) és a tárfiókot, a parancsfájl hatókörében.</span><span class="sxs-lookup"><span data-stu-id="70c83-232">Saves hello current Azure subscription in hello `$Script:originalSubscription` variable in script scope.This function saves hello current Azure subscription (as obtained by `Get-AzureSubscription -Current`) and its storage account, and hello subscription that is changed by this script (stored in hello variable `$UserSpecifiedSubscription`) and its storage account, in script scope.</span></span> <span data-ttu-id="70c83-233">Úgy, hogy elmenti hello értékek, a függvény használható, például a `Restore-Subscription`, toorestore hello eredeti aktuális előfizetés és a tárolási fiók toocurrent állapota Ha hello aktuális állapota megváltozott.</span><span class="sxs-lookup"><span data-stu-id="70c83-233">By saving hello values, you can use a function, such as `Restore-Subscription`, toorestore hello original current subscription and storage account toocurrent status if hello current status has changed.</span></span> |
| <span data-ttu-id="70c83-234">Keresés – AzureVM</span><span class="sxs-lookup"><span data-stu-id="70c83-234">Find-AzureVM</span></span> |<span data-ttu-id="70c83-235">Lekérdezi hello Azure virtuális géphez megadott.</span><span class="sxs-lookup"><span data-stu-id="70c83-235">Gets hello specified Azure virtual machine.</span></span> |
| <span data-ttu-id="70c83-236">Formátum-DevTestMessageWithTime</span><span class="sxs-lookup"><span data-stu-id="70c83-236">Format-DevTestMessageWithTime</span></span> |<span data-ttu-id="70c83-237">Dátum és idő tooa üdvözlőüzenetére lefoglalja.</span><span class="sxs-lookup"><span data-stu-id="70c83-237">Prepends hello date and time tooa message.</span></span> <span data-ttu-id="70c83-238">Ez a funkció a toohello hiba és részletes adatfolyamok üzenetek szolgál.</span><span class="sxs-lookup"><span data-stu-id="70c83-238">This function is designed for messages written toohello Error and Verbose streams.</span></span> |
| <span data-ttu-id="70c83-239">Get-AzureSQLDatabaseConnectionString</span><span class="sxs-lookup"><span data-stu-id="70c83-239">Get-AzureSQLDatabaseConnectionString</span></span> |<span data-ttu-id="70c83-240">Állítja össze a kapcsolati karakterlánc tooconnect tooan Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="70c83-240">Assembles a connection string tooconnect tooan Azure SQL database.</span></span> |
| <span data-ttu-id="70c83-241">Get-AzureVMStorage</span><span class="sxs-lookup"><span data-stu-id="70c83-241">Get-AzureVMStorage</span></span> |<span data-ttu-id="70c83-242">Beolvasása hello hello első tárfiók hello mintát neve "devtest*" (kis-és nagybetűket) hello megadott helyre, vagy a kapcsolat csoportban. Ha hello "devtest*" tárfiók nem egyezik a hello helye vagy affinitáscsoportja, hello függvény figyelmen kívül hagyja azt.</span><span class="sxs-lookup"><span data-stu-id="70c83-242">Returns hello name of hello first storage account with hello name pattern "devtest*" (case insensitive) in hello specified location or affinity group. If hello "devtest*" storage account doesn't match hello location or affinity group, hello function ignores it.</span></span> <span data-ttu-id="70c83-243">Meg kell adnia, vagy olyan helyre, vagy affinitáscsoport.</span><span class="sxs-lookup"><span data-stu-id="70c83-243">You must specify either a location or an affinity group.</span></span> |
| <span data-ttu-id="70c83-244">Get-MSDeployCmd</span><span class="sxs-lookup"><span data-stu-id="70c83-244">Get-MSDeployCmd</span></span> |<span data-ttu-id="70c83-245">A parancs toorun hello MsDeploy.exe eszköz adja vissza.</span><span class="sxs-lookup"><span data-stu-id="70c83-245">Returns a command toorun hello MsDeploy.exe tool.</span></span> |
| <span data-ttu-id="70c83-246">Új AzureVMEnvironment</span><span class="sxs-lookup"><span data-stu-id="70c83-246">New-AzureVMEnvironment</span></span> |<span data-ttu-id="70c83-247">Megállapítja, vagy létrehoz egy virtuális gépet hello az előfizetést, amely megfelel a hello értékek hello JSON-konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="70c83-247">Finds or creates a virtual machine in hello subscription that matches hello values in hello JSON configuration file.</span></span> |
| <span data-ttu-id="70c83-248">Közzététel WebPackage</span><span class="sxs-lookup"><span data-stu-id="70c83-248">Publish-WebPackage</span></span> |<span data-ttu-id="70c83-249">Felhasználási MsDeploy.exe és a webes közzétenni a csomagot. A zip-fájl toodeploy erőforrások tooa webhelyet.</span><span class="sxs-lookup"><span data-stu-id="70c83-249">Uses MsDeploy.exe and a web publish package .Zip file toodeploy resources tooa website.</span></span> <span data-ttu-id="70c83-250">Ez a függvény nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="70c83-250">This function doesn't generate any output.</span></span> <span data-ttu-id="70c83-251">Ha nem sikerül hello hívás tooMSDeploy.exe, hello függvény kivételt vált.</span><span class="sxs-lookup"><span data-stu-id="70c83-251">If hello call tooMSDeploy.exe fails, hello function throws an exception.</span></span> <span data-ttu-id="70c83-252">tooget részletesebb kimenet, használjon hello **-Verbose** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="70c83-252">tooget more detailed output, use hello **-Verbose** option.</span></span> |
| <span data-ttu-id="70c83-253">Közzététel WebPackageToVM</span><span class="sxs-lookup"><span data-stu-id="70c83-253">Publish-WebPackageToVM</span></span> |<span data-ttu-id="70c83-254">Hello paraméterértékek ellenőrzi, és ekkor meghívja a hello **Publish-WebPackage** függvény.</span><span class="sxs-lookup"><span data-stu-id="70c83-254">Verifies hello parameter values, and then calls hello **Publish-WebPackage** function.</span></span> |
| <span data-ttu-id="70c83-255">Olvasási-ConfigFile</span><span class="sxs-lookup"><span data-stu-id="70c83-255">Read-ConfigFile</span></span> |<span data-ttu-id="70c83-256">Hello JSON-konfigurációs fájl ellenőrzi, és egy kivonattáblát a kijelölt értékeket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="70c83-256">Validates hello JSON configuration file and returns a hash table of selected values.</span></span> |
| <span data-ttu-id="70c83-257">Visszaállítás-előfizetés</span><span class="sxs-lookup"><span data-stu-id="70c83-257">Restore-Subscription</span></span> |<span data-ttu-id="70c83-258">Alaphelyzetbe állítja a hello a jelenlegi előfizetés toohello eredeti előfizetés.</span><span class="sxs-lookup"><span data-stu-id="70c83-258">Resets hello current subscription toohello original subscription.</span></span> |
| <span data-ttu-id="70c83-259">Teszt-AzureModule</span><span class="sxs-lookup"><span data-stu-id="70c83-259">Test-AzureModule</span></span> |<span data-ttu-id="70c83-260">Beolvasása `$true` Ha telepített hello Azure modul verziószáma 0.7.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="70c83-260">Returns `$true` if hello installed Azure module version is 0.7.4 or later.</span></span> <span data-ttu-id="70c83-261">Beolvasása `$false` Ha hello modul nincs telepítve, vagy korábbi verziójú.</span><span class="sxs-lookup"><span data-stu-id="70c83-261">Returns `$false` if hello module isn't installed or is an earlier version.</span></span> <span data-ttu-id="70c83-262">Ez a funkció nem paraméterrel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="70c83-262">This function has no parameters.</span></span> |
| <span data-ttu-id="70c83-263">Teszt-AzureModuleVersion</span><span class="sxs-lookup"><span data-stu-id="70c83-263">Test-AzureModuleVersion</span></span> |<span data-ttu-id="70c83-264">Beolvasása `$true` Ha hello hello Azure modul verziószáma 0.7.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="70c83-264">Returns `$true` if hello version of hello Azure module is 0.7.4 or later.</span></span> <span data-ttu-id="70c83-265">Beolvasása `$false` Ha hello modul nincs telepítve, vagy korábbi verziójú.</span><span class="sxs-lookup"><span data-stu-id="70c83-265">Returns `$false` if hello module isn't installed or is an earlier version.</span></span> <span data-ttu-id="70c83-266">Ez a funkció nem paraméterrel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="70c83-266">This function has no parameters.</span></span> |
| <span data-ttu-id="70c83-267">Teszt-HttpsUrl</span><span class="sxs-lookup"><span data-stu-id="70c83-267">Test-HttpsUrl</span></span> |<span data-ttu-id="70c83-268">Hello bemeneti URL-cím tooa System.Uri objektumnak alakítja.</span><span class="sxs-lookup"><span data-stu-id="70c83-268">Converts hello input URL tooa System.Uri object.</span></span> <span data-ttu-id="70c83-269">Beolvasása `$True` Ha hello URL-cím abszolút pedig a rendszer https.</span><span class="sxs-lookup"><span data-stu-id="70c83-269">Returns `$True` if hello URL is absolute and its scheme is https.</span></span> <span data-ttu-id="70c83-270">Beolvasása `$false` Ha hello URL-cím relatív az sémája nem HTTPS, vagy a hello bemeneti karakterlánc nem lehet konvertált tooa URL-címet.</span><span class="sxs-lookup"><span data-stu-id="70c83-270">Returns `$false` if hello URL is relative, its scheme isn't HTTPS, or hello input string can't be converted tooa URL.</span></span> |
| <span data-ttu-id="70c83-271">Teszt-tag</span><span class="sxs-lookup"><span data-stu-id="70c83-271">Test-Member</span></span> |<span data-ttu-id="70c83-272">Beolvasása `$true` Ha egy tulajdonság vagy metódus hello objektum tagja.</span><span class="sxs-lookup"><span data-stu-id="70c83-272">Returns `$true` if a property or method is a member of hello object.</span></span> <span data-ttu-id="70c83-273">Ellenkező esetben adja vissza `$false`.</span><span class="sxs-lookup"><span data-stu-id="70c83-273">Otherwise, returns `$false`.</span></span> |
| <span data-ttu-id="70c83-274">Írási-ErrorWithTime</span><span class="sxs-lookup"><span data-stu-id="70c83-274">Write-ErrorWithTime</span></span> |<span data-ttu-id="70c83-275">Írja az aktuális idő hello előtagként hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="70c83-275">Writes an error message prefixed with hello current time.</span></span> <span data-ttu-id="70c83-276">Ez a funkció meghívja a hello **formátum-DevTestMessageWithTime** függvény tooprepend hello idő hello üzenet toohello hibafolyam írása előtt.</span><span class="sxs-lookup"><span data-stu-id="70c83-276">This function calls hello **Format-DevTestMessageWithTime** function tooprepend hello time before writing hello message toohello Error stream.</span></span> |
| <span data-ttu-id="70c83-277">Írási-HostWithTime</span><span class="sxs-lookup"><span data-stu-id="70c83-277">Write-HostWithTime</span></span> |<span data-ttu-id="70c83-278">Egy üzenet toohello állomás program ír (**Write-Host**) aktuális idő hello előtagként.</span><span class="sxs-lookup"><span data-stu-id="70c83-278">Writes a message toohello host program (**Write-Host**) prefixed with hello current time.</span></span> <span data-ttu-id="70c83-279">hello hatása toohello állomás program írásakor függ.</span><span class="sxs-lookup"><span data-stu-id="70c83-279">hello effect of writing toohello host program varies.</span></span> <span data-ttu-id="70c83-280">A legtöbb üzemeltető Windows PowerShell írási ezek az üzenetek toostandard kimeneti.</span><span class="sxs-lookup"><span data-stu-id="70c83-280">Most programs that host Windows PowerShell write these messages toostandard output.</span></span> |
| <span data-ttu-id="70c83-281">Írási-VerboseWithTime</span><span class="sxs-lookup"><span data-stu-id="70c83-281">Write-VerboseWithTime</span></span> |<span data-ttu-id="70c83-282">Írja az aktuális idő hello előtagként egy részletes üzenetet.</span><span class="sxs-lookup"><span data-stu-id="70c83-282">Writes a verbose message prefixed with hello current time.</span></span> <span data-ttu-id="70c83-283">Mert meghívja **Write-Verbose**, hello üzenet jelenik meg, hogy csak a hello hello parancsprogram futtatásakor **részletes** paraméter, vagy ha hello **VerbosePreference** beállítás ki van Állítsa be a túl**Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="70c83-283">Because it calls **Write-Verbose**, hello message displays only when hello script runs with hello **Verbose** parameter or when hello **VerbosePreference** preference is set too**Continue**.</span></span> |

<span data-ttu-id="70c83-284">**Közzététele: webalkalmazás**</span><span class="sxs-lookup"><span data-stu-id="70c83-284">**Publish-WebApplication**</span></span>

| <span data-ttu-id="70c83-285">Függvény neve</span><span class="sxs-lookup"><span data-stu-id="70c83-285">Function name</span></span> | <span data-ttu-id="70c83-286">Leírás</span><span class="sxs-lookup"><span data-stu-id="70c83-286">Description</span></span> |
| --- | --- |
| <span data-ttu-id="70c83-287">Új AzureWebApplicationEnvironment</span><span class="sxs-lookup"><span data-stu-id="70c83-287">New-AzureWebApplicationEnvironment</span></span> |<span data-ttu-id="70c83-288">Azure-erőforrások, például a webhelyek vagy virtuális gépet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="70c83-288">Creates Azure resources, such as a website or virtual machine.</span></span> |
| <span data-ttu-id="70c83-289">Új WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="70c83-289">New-WebDeployPackage</span></span> |<span data-ttu-id="70c83-290">Ez a funkció nincs megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="70c83-290">This function isn't implemented.</span></span> <span data-ttu-id="70c83-291">Adhat hozzá parancsok a függvény toobuild a projekthez.</span><span class="sxs-lookup"><span data-stu-id="70c83-291">You can add commands in this function toobuild your project.</span></span> |
| <span data-ttu-id="70c83-292">Közzététel AzureWebApplication</span><span class="sxs-lookup"><span data-stu-id="70c83-292">Publish-AzureWebApplication</span></span> |<span data-ttu-id="70c83-293">A webes alkalmazás tooAzure közzéteszi.</span><span class="sxs-lookup"><span data-stu-id="70c83-293">Publishes a web application tooAzure.</span></span> |
| <span data-ttu-id="70c83-294">Közzététele: webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="70c83-294">Publish-WebApplication</span></span> |<span data-ttu-id="70c83-295">Hoz létre, és a Web Apps, a virtuális gépek, a SQL-adatbázisok és a storage-fiókok a Visual Studio webes projektet telepít.</span><span class="sxs-lookup"><span data-stu-id="70c83-295">Creates and deploys Web Apps, virtual machines, SQL databases, and storage accounts for a Visual Studio web project.</span></span> |
| <span data-ttu-id="70c83-296">Teszt:-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="70c83-296">Test-WebApplication</span></span> |<span data-ttu-id="70c83-297">Ez a funkció nincs megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="70c83-297">This function isn't implemented.</span></span> <span data-ttu-id="70c83-298">Adhat hozzá parancsok a függvény tootest az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="70c83-298">You can add commands in this function tootest your application.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="70c83-299">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="70c83-299">Next steps</span></span>
<span data-ttu-id="70c83-300">PowerShell parancsfájl-kezelési beolvasásával kapcsolatos további [a Windows PowerShell parancsfájlok](https://technet.microsoft.com/library/bb978526.aspx) és egyéb Azure PowerShell-parancsfájlok, hello [Script Center](https://azure.microsoft.com/documentation/scripts/).</span><span class="sxs-lookup"><span data-stu-id="70c83-300">Learn more about PowerShell scripting by reading [Scripting with Windows PowerShell](https://technet.microsoft.com/library/bb978526.aspx) and see other Azure PowerShell scripts at hello [Script Center](https://azure.microsoft.com/documentation/scripts/).</span></span>
