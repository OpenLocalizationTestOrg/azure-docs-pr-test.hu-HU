---
title: "Közzététel WebApplicationVM |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítheti egy webalkalmazást egy virtuális géphez. Ezt a parancsfájlt a szükséges erőforrásokat az Azure-előfizetése hoz létre, ha azok még nem léteznek."
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
ms.openlocfilehash: 2738fc1dff50a177a227ae2c7719bd9a192d82ad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a><span data-ttu-id="97769-104">(A Windows PowerShell-parancsfájl) közzététele-WebApplicationVM</span><span class="sxs-lookup"><span data-stu-id="97769-104">Publish-WebApplicationVM (Windows PowerShell script)</span></span>
<span data-ttu-id="97769-105">A webalkalmazások egy virtuális gépet telepít.</span><span class="sxs-lookup"><span data-stu-id="97769-105">Deploys a web application to a virtual machine.</span></span> <span data-ttu-id="97769-106">A parancsfájl a szükséges erőforrásokat az Azure-előfizetése hoz létre, ha azok még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="97769-106">The script creates the required resources in your Azure subscription if they don't exist.</span></span>

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

### <a name="configuration"></a><span data-ttu-id="97769-107">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="97769-107">Configuration</span></span>
<span data-ttu-id="97769-108">A JSON-konfigurációs fájlt, amely leírja a központi telepítés részleteinek elérési útja.</span><span class="sxs-lookup"><span data-stu-id="97769-108">The path to the JSON configuration file that describes the details of the deployment.</span></span>

| <span data-ttu-id="97769-109">Aliasok</span><span class="sxs-lookup"><span data-stu-id="97769-109">Aliases</span></span> | <span data-ttu-id="97769-110">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="97769-110">none</span></span> |
| --- | --- |
| <span data-ttu-id="97769-111">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="97769-111">Required?</span></span> |<span data-ttu-id="97769-112">Igaz</span><span class="sxs-lookup"><span data-stu-id="97769-112">true</span></span> |
| <span data-ttu-id="97769-113">Beosztás</span><span class="sxs-lookup"><span data-stu-id="97769-113">Position</span></span> |<span data-ttu-id="97769-114">nevű</span><span class="sxs-lookup"><span data-stu-id="97769-114">named</span></span> |
| <span data-ttu-id="97769-115">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="97769-115">Default value</span></span> |<span data-ttu-id="97769-116">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="97769-116">none</span></span> |
| <span data-ttu-id="97769-117">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="97769-117">Accept pipeline input?</span></span> |<span data-ttu-id="97769-118">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-118">false</span></span> |
| <span data-ttu-id="97769-119">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="97769-119">Accept wildcard characters?</span></span> |<span data-ttu-id="97769-120">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-120">false</span></span> |

### <a name="subscriptionname"></a><span data-ttu-id="97769-121">SubscriptionName</span><span class="sxs-lookup"><span data-stu-id="97769-121">SubscriptionName</span></span>
<span data-ttu-id="97769-122">Kívánja a virtuális gép létrehozása Azure-előfizetés neve.</span><span class="sxs-lookup"><span data-stu-id="97769-122">The name of the Azure subscription in which you want to create the virtual machine.</span></span>

| <span data-ttu-id="97769-123">Aliasok</span><span class="sxs-lookup"><span data-stu-id="97769-123">Aliases</span></span> | <span data-ttu-id="97769-124">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="97769-124">none</span></span> |
| --- | --- |
| <span data-ttu-id="97769-125">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="97769-125">Required?</span></span> |<span data-ttu-id="97769-126">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-126">false</span></span> |
| <span data-ttu-id="97769-127">Beosztás</span><span class="sxs-lookup"><span data-stu-id="97769-127">Position</span></span> |<span data-ttu-id="97769-128">nevű</span><span class="sxs-lookup"><span data-stu-id="97769-128">named</span></span> |
| <span data-ttu-id="97769-129">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="97769-129">Default value</span></span> |<span data-ttu-id="97769-130">Az előfizetés fájlban az első előfizetést használ</span><span class="sxs-lookup"><span data-stu-id="97769-130">Uses the first subscription in the subscription file</span></span> |
| <span data-ttu-id="97769-131">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="97769-131">Accept pipeline input?</span></span> |<span data-ttu-id="97769-132">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-132">false</span></span> |
| <span data-ttu-id="97769-133">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="97769-133">Accept wildcard characters?</span></span> |<span data-ttu-id="97769-134">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-134">false</span></span> |

### <a name="webdeploypackage"></a><span data-ttu-id="97769-135">WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="97769-135">WebDeployPackage</span></span>
<span data-ttu-id="97769-136">A webes telepítési csomag közzétételére a virtuális gép elérési útja.</span><span class="sxs-lookup"><span data-stu-id="97769-136">The path to the web deployment package to publish to the virtual machine.</span></span> <span data-ttu-id="97769-137">Ezt a csomagot a Visual Studio webhely közzététele varázsló használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="97769-137">You can create this package by using the Publish Web wizard in Visual Studio.</span></span> <span data-ttu-id="97769-138">Lásd: [Útmutató: webes telepítési csomag létrehozása a Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span><span class="sxs-lookup"><span data-stu-id="97769-138">See [How to: Create a Web Deployment Package in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span></span>

| <span data-ttu-id="97769-139">Aliasok</span><span class="sxs-lookup"><span data-stu-id="97769-139">Aliases</span></span> | <span data-ttu-id="97769-140">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="97769-140">none</span></span> |
| --- | --- |
| <span data-ttu-id="97769-141">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="97769-141">Required?</span></span> |<span data-ttu-id="97769-142">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-142">false</span></span> |
| <span data-ttu-id="97769-143">Beosztás</span><span class="sxs-lookup"><span data-stu-id="97769-143">Position</span></span> |<span data-ttu-id="97769-144">nevű</span><span class="sxs-lookup"><span data-stu-id="97769-144">named</span></span> |
| <span data-ttu-id="97769-145">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="97769-145">Default value</span></span> |<span data-ttu-id="97769-146">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="97769-146">none</span></span> |
| <span data-ttu-id="97769-147">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="97769-147">Accept pipeline input?</span></span> |<span data-ttu-id="97769-148">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-148">false</span></span> |
| <span data-ttu-id="97769-149">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="97769-149">Accept wildcard characters?</span></span> |<span data-ttu-id="97769-150">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-150">false</span></span> |

### <a name="allowuntrusted"></a><span data-ttu-id="97769-151">AllowUntrusted</span><span class="sxs-lookup"><span data-stu-id="97769-151">AllowUntrusted</span></span>
<span data-ttu-id="97769-152">Amennyiben az értéke igaz, nem egy megbízható legfelső szintű hitelesítésszolgáltató által aláírt tanúsítványok használatának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="97769-152">If true, allow the use of certificates that aren't signed by a trusted root authority.</span></span>

| <span data-ttu-id="97769-153">Aliasok</span><span class="sxs-lookup"><span data-stu-id="97769-153">Aliases</span></span> | <span data-ttu-id="97769-154">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="97769-154">none</span></span> |
| --- | --- |
| <span data-ttu-id="97769-155">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="97769-155">Required?</span></span> |<span data-ttu-id="97769-156">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-156">false</span></span> |
| <span data-ttu-id="97769-157">Beosztás</span><span class="sxs-lookup"><span data-stu-id="97769-157">Position</span></span> |<span data-ttu-id="97769-158">nevű</span><span class="sxs-lookup"><span data-stu-id="97769-158">named</span></span> |
| <span data-ttu-id="97769-159">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="97769-159">Default value</span></span> |<span data-ttu-id="97769-160">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-160">false</span></span> |
| <span data-ttu-id="97769-161">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="97769-161">Accept pipeline input?</span></span> |<span data-ttu-id="97769-162">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-162">false</span></span> |
| <span data-ttu-id="97769-163">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="97769-163">Accept wildcard characters?</span></span> |<span data-ttu-id="97769-164">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-164">false</span></span> |

### <a name="vmpassword"></a><span data-ttu-id="97769-165">VMPassword</span><span class="sxs-lookup"><span data-stu-id="97769-165">VMPassword</span></span>
<span data-ttu-id="97769-166">A virtuális gép fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="97769-166">The credentials for the virtual machine account.</span></span> <span data-ttu-id="97769-167">Példa: - VMPassword @{név = "rendszergazda"; Jelszó = a "password"}</span><span class="sxs-lookup"><span data-stu-id="97769-167">Example: -VMPassword @{Name = "admin"; Password = "password"}</span></span>

| <span data-ttu-id="97769-168">Aliasok</span><span class="sxs-lookup"><span data-stu-id="97769-168">Aliases</span></span> | <span data-ttu-id="97769-169">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="97769-169">none</span></span> |
| --- | --- |
| <span data-ttu-id="97769-170">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="97769-170">Required?</span></span> |<span data-ttu-id="97769-171">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-171">false</span></span> |
| <span data-ttu-id="97769-172">Beosztás</span><span class="sxs-lookup"><span data-stu-id="97769-172">Position</span></span> |<span data-ttu-id="97769-173">nevű</span><span class="sxs-lookup"><span data-stu-id="97769-173">named</span></span> |
| <span data-ttu-id="97769-174">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="97769-174">Default value</span></span> |<span data-ttu-id="97769-175">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="97769-175">none</span></span> |
| <span data-ttu-id="97769-176">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="97769-176">Accept pipeline input?</span></span> |<span data-ttu-id="97769-177">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-177">false</span></span> |
| <span data-ttu-id="97769-178">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="97769-178">Accept wildcard characters?</span></span> |<span data-ttu-id="97769-179">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-179">false</span></span> |

### <a name="databaseserverpassword"></a><span data-ttu-id="97769-180">DatabaseServerPassword</span><span class="sxs-lookup"><span data-stu-id="97769-180">DatabaseServerPassword</span></span>
<span data-ttu-id="97769-181">A hitelesítő adatokat az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="97769-181">The credentials for the SQL database in Azure.</span></span> <span data-ttu-id="97769-182">Példa: - DatabaseServerPassword @{név = "rendszergazda"; Jelszó = a "password"}</span><span class="sxs-lookup"><span data-stu-id="97769-182">Example: -DatabaseServerPassword @{Name = "admin"; Password = "password"}</span></span>

| <span data-ttu-id="97769-183">Aliasok</span><span class="sxs-lookup"><span data-stu-id="97769-183">Aliases</span></span> | <span data-ttu-id="97769-184">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="97769-184">none</span></span> |
| --- | --- |
| <span data-ttu-id="97769-185">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="97769-185">Required?</span></span> |<span data-ttu-id="97769-186">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-186">false</span></span> |
| <span data-ttu-id="97769-187">Beosztás</span><span class="sxs-lookup"><span data-stu-id="97769-187">Position</span></span> |<span data-ttu-id="97769-188">nevű</span><span class="sxs-lookup"><span data-stu-id="97769-188">named</span></span> |
| <span data-ttu-id="97769-189">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="97769-189">Default value</span></span> |<span data-ttu-id="97769-190">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="97769-190">none</span></span> |
| <span data-ttu-id="97769-191">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="97769-191">Accept pipeline input?</span></span> |<span data-ttu-id="97769-192">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-192">false</span></span> |
| <span data-ttu-id="97769-193">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="97769-193">Accept wildcard characters?</span></span> |<span data-ttu-id="97769-194">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-194">false</span></span> |

### <a name="sendhostmessagestooutput"></a><span data-ttu-id="97769-195">SendHostMessagesToOutput</span><span class="sxs-lookup"><span data-stu-id="97769-195">SendHostMessagesToOutput</span></span>
<span data-ttu-id="97769-196">Amennyiben az értéke igaz, a nyomtató érkező üzenetek a parancsfájl a kimeneti adatfolyamba.</span><span class="sxs-lookup"><span data-stu-id="97769-196">If true, print messages from the script to the output stream.</span></span>

| <span data-ttu-id="97769-197">Aliasok</span><span class="sxs-lookup"><span data-stu-id="97769-197">Aliases</span></span> | <span data-ttu-id="97769-198">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="97769-198">none</span></span> |
| --- | --- |
| <span data-ttu-id="97769-199">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="97769-199">Required?</span></span> |<span data-ttu-id="97769-200">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-200">false</span></span> |
| <span data-ttu-id="97769-201">Beosztás</span><span class="sxs-lookup"><span data-stu-id="97769-201">Position</span></span> |<span data-ttu-id="97769-202">nevű</span><span class="sxs-lookup"><span data-stu-id="97769-202">named</span></span> |
| <span data-ttu-id="97769-203">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="97769-203">Default value</span></span> |<span data-ttu-id="97769-204">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-204">false</span></span> |
| <span data-ttu-id="97769-205">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="97769-205">Accept pipeline input?</span></span> |<span data-ttu-id="97769-206">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-206">false</span></span> |
| <span data-ttu-id="97769-207">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="97769-207">Accept wildcard characters?</span></span> |<span data-ttu-id="97769-208">hamis</span><span class="sxs-lookup"><span data-stu-id="97769-208">false</span></span> |

## <a name="remarks"></a><span data-ttu-id="97769-209">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="97769-209">Remarks</span></span>
<span data-ttu-id="97769-210">A parancsfájl használata létrehozásához teljes leírását fejlesztési és tesztkörnyezetek, lásd: [Windows PowerShell parancsfájlok használata a közzététel a fejlesztési és tesztkörnyezetek](vs-azure-tools-publishing-using-powershell-scripts.md).</span><span class="sxs-lookup"><span data-stu-id="97769-210">For a complete explanation of how to use the script to create Dev and Test environments, see [Using Windows PowerShell Scripts to Publish to Dev and Test Environments](vs-azure-tools-publishing-using-powershell-scripts.md).</span></span>

<span data-ttu-id="97769-211">A JSON-konfigurációs fájl meghatározza, hogy mit telepítendő részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="97769-211">The JSON configuration file specifies the details of what is to be deployed.</span></span> <span data-ttu-id="97769-212">A projekt, például a nevét, a affinitáscsoport, a Virtuálismerevlemez-kép és a virtuális gép mérete létrehozásakor megadott információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="97769-212">It includes the information that you specified when you created the project, such as the name, affinity group, VHD image, and size of the virtual machine.</span></span> <span data-ttu-id="97769-213">Azt is magában foglalja a végpontokat a virtuális gépen, az adatbázisok kiépítéséhez, ha van ilyen, webes és üzembe helyezéshez megadott paraméterek.</span><span class="sxs-lookup"><span data-stu-id="97769-213">It also includes the endpoints on the virtual machine, the databases to provision, if any, and web deployment parameters.</span></span> <span data-ttu-id="97769-214">A következő kód bemutatja egy példa JSON-konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="97769-214">The following code shows an example JSON configuration file:</span></span>

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

<span data-ttu-id="97769-215">Szerkesztheti a JSON-konfigurációs fájl módosítása milyen ki van építve.</span><span class="sxs-lookup"><span data-stu-id="97769-215">You can edit the JSON configuration file to change what is provisioned.</span></span> <span data-ttu-id="97769-216">Szükség egy virtuális gép és egy felhőalapú szolgáltatás, de az adatbázis szakaszban nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="97769-216">A virtual machine and a cloud service are required, but the database section is optional.</span></span>

