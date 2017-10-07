---
title: aaaPublish-WebApplicationVM |} Microsoft Docs
description: "Megtudhatja, hogyan toodeploy egy webes alkalmazás tooa virtuális gépet. Ez a parancsfájl hello szükséges erőforrások az Azure-előfizetése hoz létre, ha azok még nem léteznek."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: de4cec95-f73f-44d9-babd-9f47f2633cdb
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: e4b52b620bebf44b87ddfc3b19c155bb65111814
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a><span data-ttu-id="d4353-104">(A Windows PowerShell-parancsfájl) közzététele-WebApplicationVM</span><span class="sxs-lookup"><span data-stu-id="d4353-104">Publish-WebApplicationVM (Windows PowerShell script)</span></span>
<span data-ttu-id="d4353-105">Telepít a webes alkalmazás tooa virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d4353-105">Deploys a web application tooa virtual machine.</span></span> <span data-ttu-id="d4353-106">hello parancsfájl hello szükséges erőforrások az Azure-előfizetése hoz létre, ha azok még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="d4353-106">hello script creates hello required resources in your Azure subscription if they don't exist.</span></span>

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a><span data-ttu-id="d4353-107">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="d4353-107">Configuration</span></span>
<span data-ttu-id="d4353-108">hello elérési toohello JSON-konfigurációs fájlt, amely hello hello központi telepítés részleteit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d4353-108">hello path toohello JSON configuration file that describes hello details of hello deployment.</span></span>

| <span data-ttu-id="d4353-109">Aliasok</span><span class="sxs-lookup"><span data-stu-id="d4353-109">Aliases</span></span> | <span data-ttu-id="d4353-110">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="d4353-110">none</span></span> |
| --- | --- |
| <span data-ttu-id="d4353-111">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="d4353-111">Required?</span></span> |<span data-ttu-id="d4353-112">Igaz</span><span class="sxs-lookup"><span data-stu-id="d4353-112">true</span></span> |
| <span data-ttu-id="d4353-113">Beosztás</span><span class="sxs-lookup"><span data-stu-id="d4353-113">Position</span></span> |<span data-ttu-id="d4353-114">nevű</span><span class="sxs-lookup"><span data-stu-id="d4353-114">named</span></span> |
| <span data-ttu-id="d4353-115">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="d4353-115">Default value</span></span> |<span data-ttu-id="d4353-116">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="d4353-116">none</span></span> |
| <span data-ttu-id="d4353-117">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="d4353-117">Accept pipeline input?</span></span> |<span data-ttu-id="d4353-118">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-118">false</span></span> |
| <span data-ttu-id="d4353-119">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="d4353-119">Accept wildcard characters?</span></span> |<span data-ttu-id="d4353-120">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-120">false</span></span> |

### <a name="subscriptionname"></a><span data-ttu-id="d4353-121">SubscriptionName</span><span class="sxs-lookup"><span data-stu-id="d4353-121">SubscriptionName</span></span>
<span data-ttu-id="d4353-122">hello neve hello toocreate hello virtuális géphez használni kívánt Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="d4353-122">hello name of hello Azure subscription in which you want toocreate hello virtual machine.</span></span>

| <span data-ttu-id="d4353-123">Aliasok</span><span class="sxs-lookup"><span data-stu-id="d4353-123">Aliases</span></span> | <span data-ttu-id="d4353-124">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="d4353-124">none</span></span> |
| --- | --- |
| <span data-ttu-id="d4353-125">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="d4353-125">Required?</span></span> |<span data-ttu-id="d4353-126">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-126">false</span></span> |
| <span data-ttu-id="d4353-127">Beosztás</span><span class="sxs-lookup"><span data-stu-id="d4353-127">Position</span></span> |<span data-ttu-id="d4353-128">nevű</span><span class="sxs-lookup"><span data-stu-id="d4353-128">named</span></span> |
| <span data-ttu-id="d4353-129">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="d4353-129">Default value</span></span> |<span data-ttu-id="d4353-130">Hello első előfizetés használ egy hello előfizetési fájl:</span><span class="sxs-lookup"><span data-stu-id="d4353-130">Uses hello first subscription in hello subscription file</span></span> |
| <span data-ttu-id="d4353-131">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="d4353-131">Accept pipeline input?</span></span> |<span data-ttu-id="d4353-132">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-132">false</span></span> |
| <span data-ttu-id="d4353-133">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="d4353-133">Accept wildcard characters?</span></span> |<span data-ttu-id="d4353-134">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-134">false</span></span> |

### <a name="webdeploypackage"></a><span data-ttu-id="d4353-135">WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="d4353-135">WebDeployPackage</span></span>
<span data-ttu-id="d4353-136">hello elérési toohello webes telepítési csomag toopublish toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d4353-136">hello path toohello web deployment package toopublish toohello virtual machine.</span></span> <span data-ttu-id="d4353-137">Ezt a csomagot a Visual Studio hello webhely közzététele varázsló használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="d4353-137">You can create this package by using hello Publish Web wizard in Visual Studio.</span></span> <span data-ttu-id="d4353-138">Lásd: [Útmutató: webes telepítési csomag létrehozása a Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span><span class="sxs-lookup"><span data-stu-id="d4353-138">See [How to: Create a Web Deployment Package in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span></span>

| <span data-ttu-id="d4353-139">Aliasok</span><span class="sxs-lookup"><span data-stu-id="d4353-139">Aliases</span></span> | <span data-ttu-id="d4353-140">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="d4353-140">none</span></span> |
| --- | --- |
| <span data-ttu-id="d4353-141">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="d4353-141">Required?</span></span> |<span data-ttu-id="d4353-142">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-142">false</span></span> |
| <span data-ttu-id="d4353-143">Beosztás</span><span class="sxs-lookup"><span data-stu-id="d4353-143">Position</span></span> |<span data-ttu-id="d4353-144">nevű</span><span class="sxs-lookup"><span data-stu-id="d4353-144">named</span></span> |
| <span data-ttu-id="d4353-145">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="d4353-145">Default value</span></span> |<span data-ttu-id="d4353-146">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="d4353-146">none</span></span> |
| <span data-ttu-id="d4353-147">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="d4353-147">Accept pipeline input?</span></span> |<span data-ttu-id="d4353-148">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-148">false</span></span> |
| <span data-ttu-id="d4353-149">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="d4353-149">Accept wildcard characters?</span></span> |<span data-ttu-id="d4353-150">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-150">false</span></span> |

### <a name="allowuntrusted"></a><span data-ttu-id="d4353-151">AllowUntrusted</span><span class="sxs-lookup"><span data-stu-id="d4353-151">AllowUntrusted</span></span>
<span data-ttu-id="d4353-152">Amennyiben az értéke igaz, a tanúsítványokat, amelyek nem megbízható legfelső szintű hatóság aláírásával hello használatának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d4353-152">If true, allow hello use of certificates that aren't signed by a trusted root authority.</span></span>

| <span data-ttu-id="d4353-153">Aliasok</span><span class="sxs-lookup"><span data-stu-id="d4353-153">Aliases</span></span> | <span data-ttu-id="d4353-154">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="d4353-154">none</span></span> |
| --- | --- |
| <span data-ttu-id="d4353-155">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="d4353-155">Required?</span></span> |<span data-ttu-id="d4353-156">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-156">false</span></span> |
| <span data-ttu-id="d4353-157">Beosztás</span><span class="sxs-lookup"><span data-stu-id="d4353-157">Position</span></span> |<span data-ttu-id="d4353-158">nevű</span><span class="sxs-lookup"><span data-stu-id="d4353-158">named</span></span> |
| <span data-ttu-id="d4353-159">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="d4353-159">Default value</span></span> |<span data-ttu-id="d4353-160">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-160">false</span></span> |
| <span data-ttu-id="d4353-161">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="d4353-161">Accept pipeline input?</span></span> |<span data-ttu-id="d4353-162">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-162">false</span></span> |
| <span data-ttu-id="d4353-163">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="d4353-163">Accept wildcard characters?</span></span> |<span data-ttu-id="d4353-164">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-164">false</span></span> |

### <a name="vmpassword"></a><span data-ttu-id="d4353-165">VMPassword</span><span class="sxs-lookup"><span data-stu-id="d4353-165">VMPassword</span></span>
<span data-ttu-id="d4353-166">hello hello virtuális gép fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="d4353-166">hello credentials for hello virtual machine account.</span></span> <span data-ttu-id="d4353-167">Példa: - VMPassword @{név = "rendszergazda"; Jelszó = a "password"}</span><span class="sxs-lookup"><span data-stu-id="d4353-167">Example: -VMPassword @{Name = "admin"; Password = "password"}</span></span>

| <span data-ttu-id="d4353-168">Aliasok</span><span class="sxs-lookup"><span data-stu-id="d4353-168">Aliases</span></span> | <span data-ttu-id="d4353-169">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="d4353-169">none</span></span> |
| --- | --- |
| <span data-ttu-id="d4353-170">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="d4353-170">Required?</span></span> |<span data-ttu-id="d4353-171">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-171">false</span></span> |
| <span data-ttu-id="d4353-172">Beosztás</span><span class="sxs-lookup"><span data-stu-id="d4353-172">Position</span></span> |<span data-ttu-id="d4353-173">nevű</span><span class="sxs-lookup"><span data-stu-id="d4353-173">named</span></span> |
| <span data-ttu-id="d4353-174">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="d4353-174">Default value</span></span> |<span data-ttu-id="d4353-175">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="d4353-175">none</span></span> |
| <span data-ttu-id="d4353-176">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="d4353-176">Accept pipeline input?</span></span> |<span data-ttu-id="d4353-177">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-177">false</span></span> |
| <span data-ttu-id="d4353-178">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="d4353-178">Accept wildcard characters?</span></span> |<span data-ttu-id="d4353-179">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-179">false</span></span> |

### <a name="databaseserverpassword"></a><span data-ttu-id="d4353-180">DatabaseServerPassword</span><span class="sxs-lookup"><span data-stu-id="d4353-180">DatabaseServerPassword</span></span>
<span data-ttu-id="d4353-181">az Azure SQL-adatbázis hello hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="d4353-181">hello credentials for hello SQL database in Azure.</span></span> <span data-ttu-id="d4353-182">Példa: - DatabaseServerPassword @{név = "rendszergazda"; Jelszó = a "password"}</span><span class="sxs-lookup"><span data-stu-id="d4353-182">Example: -DatabaseServerPassword @{Name = "admin"; Password = "password"}</span></span>

| <span data-ttu-id="d4353-183">Aliasok</span><span class="sxs-lookup"><span data-stu-id="d4353-183">Aliases</span></span> | <span data-ttu-id="d4353-184">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="d4353-184">none</span></span> |
| --- | --- |
| <span data-ttu-id="d4353-185">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="d4353-185">Required?</span></span> |<span data-ttu-id="d4353-186">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-186">false</span></span> |
| <span data-ttu-id="d4353-187">Beosztás</span><span class="sxs-lookup"><span data-stu-id="d4353-187">Position</span></span> |<span data-ttu-id="d4353-188">nevű</span><span class="sxs-lookup"><span data-stu-id="d4353-188">named</span></span> |
| <span data-ttu-id="d4353-189">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="d4353-189">Default value</span></span> |<span data-ttu-id="d4353-190">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="d4353-190">none</span></span> |
| <span data-ttu-id="d4353-191">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="d4353-191">Accept pipeline input?</span></span> |<span data-ttu-id="d4353-192">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-192">false</span></span> |
| <span data-ttu-id="d4353-193">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="d4353-193">Accept wildcard characters?</span></span> |<span data-ttu-id="d4353-194">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-194">false</span></span> |

### <a name="sendhostmessagestooutput"></a><span data-ttu-id="d4353-195">SendHostMessagesToOutput</span><span class="sxs-lookup"><span data-stu-id="d4353-195">SendHostMessagesToOutput</span></span>
<span data-ttu-id="d4353-196">Igaz értéke esetén a nyomtatási üzeneteit hello parancsfájl toohello kimeneti adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="d4353-196">If true, print messages from hello script toohello output stream.</span></span>

| <span data-ttu-id="d4353-197">Aliasok</span><span class="sxs-lookup"><span data-stu-id="d4353-197">Aliases</span></span> | <span data-ttu-id="d4353-198">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="d4353-198">none</span></span> |
| --- | --- |
| <span data-ttu-id="d4353-199">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="d4353-199">Required?</span></span> |<span data-ttu-id="d4353-200">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-200">false</span></span> |
| <span data-ttu-id="d4353-201">Beosztás</span><span class="sxs-lookup"><span data-stu-id="d4353-201">Position</span></span> |<span data-ttu-id="d4353-202">nevű</span><span class="sxs-lookup"><span data-stu-id="d4353-202">named</span></span> |
| <span data-ttu-id="d4353-203">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="d4353-203">Default value</span></span> |<span data-ttu-id="d4353-204">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-204">false</span></span> |
| <span data-ttu-id="d4353-205">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="d4353-205">Accept pipeline input?</span></span> |<span data-ttu-id="d4353-206">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-206">false</span></span> |
| <span data-ttu-id="d4353-207">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="d4353-207">Accept wildcard characters?</span></span> |<span data-ttu-id="d4353-208">hamis</span><span class="sxs-lookup"><span data-stu-id="d4353-208">false</span></span> |

## <a name="remarks"></a><span data-ttu-id="d4353-209">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d4353-209">Remarks</span></span>
<span data-ttu-id="d4353-210">Hogyan toouse hello parancsfájl toocreate fejlesztési és tesztkörnyezetek: teljes leírását [Windows PowerShell-parancsfájlok használatával tooPublish tooDev és a tesztkörnyezetek](vs-azure-tools-publishing-using-powershell-scripts.md).</span><span class="sxs-lookup"><span data-stu-id="d4353-210">For a complete explanation of how toouse hello script toocreate Dev and Test environments, see [Using Windows PowerShell Scripts tooPublish tooDev and Test Environments](vs-azure-tools-publishing-using-powershell-scripts.md).</span></span>

<span data-ttu-id="d4353-211">hello JSON-konfigurációs fájlt határozza meg, hogy mit telepített toobe hello részletei.</span><span class="sxs-lookup"><span data-stu-id="d4353-211">hello JSON configuration file specifies hello details of what is toobe deployed.</span></span> <span data-ttu-id="d4353-212">Ez magában foglalja a hello projekt, például hello nevét, az affinitáscsoport, a Virtuálismerevlemez-kép és a hello virtuális gép mérete létrehozásakor megadott hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="d4353-212">It includes hello information that you specified when you created hello project, such as hello name, affinity group, VHD image, and size of hello virtual machine.</span></span> <span data-ttu-id="d4353-213">Azt is magában foglalja a hello végpontok hello virtuális gépen, hello adatbázisok tooprovision, ha van ilyen, webes és üzembe helyezéshez megadott paraméterek.</span><span class="sxs-lookup"><span data-stu-id="d4353-213">It also includes hello endpoints on hello virtual machine, hello databases tooprovision, if any, and web deployment parameters.</span></span> <span data-ttu-id="d4353-214">a következő kód hello látható egy példa JSON-konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="d4353-214">hello following code shows an example JSON configuration file:</span></span>

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
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
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

<span data-ttu-id="d4353-215">Szerkesztheti a hello JSON konfigurációs fájl toochange mi lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="d4353-215">You can edit hello JSON configuration file toochange what is provisioned.</span></span> <span data-ttu-id="d4353-216">Szükség egy virtuális gép és egy felhőalapú szolgáltatás, de hello adatbázis szakasz nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="d4353-216">A virtual machine and a cloud service are required, but hello database section is optional.</span></span>

