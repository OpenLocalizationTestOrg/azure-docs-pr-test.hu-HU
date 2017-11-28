---
title: "(A Windows PowerShell-parancsfájl) közzététele-WebApplicationWebSite |} Microsoft Docs"
description: "Ismerje meg, hogy egy webes projekt közzététele egy Azure-webhelyen. Ezt a parancsfájlt a szükséges erőforrásokat az Azure-előfizetése hoz létre, ha azok még nem léteznek."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 07d21b7ce6cd8aee1cff704d316e7a2ca8c00437
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a><span data-ttu-id="ed759-104">(A Windows PowerShell-parancsfájl) közzététele-WebApplicationWebSite</span><span class="sxs-lookup"><span data-stu-id="ed759-104">Publish-WebApplicationWebSite (Windows PowerShell script)</span></span>
## <a name="syntax"></a><span data-ttu-id="ed759-105">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="ed759-105">Syntax</span></span>
<span data-ttu-id="ed759-106">A webes projekt közzététele egy Azure-webhelyen.</span><span class="sxs-lookup"><span data-stu-id="ed759-106">Publishes a web project to an Azure website.</span></span> <span data-ttu-id="ed759-107">A parancsfájl a szükséges erőforrásokat az Azure-előfizetése hoz létre, ha azok még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="ed759-107">The script creates the required resources in your Azure subscription if they don't exist.</span></span>

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a><span data-ttu-id="ed759-108">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="ed759-108">Configuration</span></span>
<span data-ttu-id="ed759-109">A JSON-konfigurációs fájlt, amely leírja a központi telepítés részleteinek elérési útja.</span><span class="sxs-lookup"><span data-stu-id="ed759-109">The path to the JSON configuration file that describes the details of the deployment.</span></span>

| <span data-ttu-id="ed759-110">Paraméter</span><span class="sxs-lookup"><span data-stu-id="ed759-110">Parameter</span></span> | <span data-ttu-id="ed759-111">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ed759-111">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="ed759-112">Aliasok</span><span class="sxs-lookup"><span data-stu-id="ed759-112">Aliases</span></span> |<span data-ttu-id="ed759-113">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="ed759-113">none</span></span> |
| <span data-ttu-id="ed759-114">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="ed759-114">Required?</span></span> |<span data-ttu-id="ed759-115">Igaz</span><span class="sxs-lookup"><span data-stu-id="ed759-115">true</span></span> |
| <span data-ttu-id="ed759-116">Beosztás</span><span class="sxs-lookup"><span data-stu-id="ed759-116">Position</span></span> |<span data-ttu-id="ed759-117">nevű</span><span class="sxs-lookup"><span data-stu-id="ed759-117">named</span></span> |
| <span data-ttu-id="ed759-118">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ed759-118">Default value</span></span> |<span data-ttu-id="ed759-119">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="ed759-119">none</span></span> |
| <span data-ttu-id="ed759-120">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="ed759-120">Accept pipeline input?</span></span> |<span data-ttu-id="ed759-121">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-121">false</span></span> |
| <span data-ttu-id="ed759-122">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="ed759-122">Accept wildcard characters?</span></span> |<span data-ttu-id="ed759-123">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-123">false</span></span> |

## <a name="subscriptionname"></a><span data-ttu-id="ed759-124">SubscriptionName</span><span class="sxs-lookup"><span data-stu-id="ed759-124">SubscriptionName</span></span>
<span data-ttu-id="ed759-125">Az Azure-előfizetés, amelyet a webhely neve.</span><span class="sxs-lookup"><span data-stu-id="ed759-125">The name of the Azure subscription that you want to create the website in.</span></span>

| <span data-ttu-id="ed759-126">Paraméter</span><span class="sxs-lookup"><span data-stu-id="ed759-126">Parameter</span></span> | <span data-ttu-id="ed759-127">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ed759-127">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="ed759-128">Aliasok</span><span class="sxs-lookup"><span data-stu-id="ed759-128">Aliases</span></span> |<span data-ttu-id="ed759-129">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="ed759-129">none</span></span> |
| <span data-ttu-id="ed759-130">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="ed759-130">Required?</span></span> |<span data-ttu-id="ed759-131">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-131">false</span></span> |
| <span data-ttu-id="ed759-132">Beosztás</span><span class="sxs-lookup"><span data-stu-id="ed759-132">Position</span></span> |<span data-ttu-id="ed759-133">nevű</span><span class="sxs-lookup"><span data-stu-id="ed759-133">named</span></span> |
| <span data-ttu-id="ed759-134">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ed759-134">Default value</span></span> |<span data-ttu-id="ed759-135">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="ed759-135">none</span></span> |
| <span data-ttu-id="ed759-136">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="ed759-136">Accept pipeline input?</span></span> |<span data-ttu-id="ed759-137">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-137">false</span></span> |
| <span data-ttu-id="ed759-138">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="ed759-138">Accept wildcard characters?</span></span> |<span data-ttu-id="ed759-139">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-139">false</span></span> |

## <a name="webdeploypackage"></a><span data-ttu-id="ed759-140">WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="ed759-140">WebDeployPackage</span></span>
<span data-ttu-id="ed759-141">A webhelyen közzétenni a webes telepítési csomag elérési útja.</span><span class="sxs-lookup"><span data-stu-id="ed759-141">The path to the web deployment package to publish to the website.</span></span> <span data-ttu-id="ed759-142">Ezt a csomagot a Visual Studio webhely közzététele varázsló használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="ed759-142">You can create this package by using the Publish Web wizard in Visual Studio.</span></span> <span data-ttu-id="ed759-143">További információkért lásd: [Ismerkedés az Azure felhőalapú szolgáltatásairól és ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).</span><span class="sxs-lookup"><span data-stu-id="ed759-143">For more information, see [Get started with Azure Cloud Services and ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).</span></span>

| <span data-ttu-id="ed759-144">Paraméter</span><span class="sxs-lookup"><span data-stu-id="ed759-144">Parameter</span></span> | <span data-ttu-id="ed759-145">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ed759-145">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="ed759-146">Aliasok</span><span class="sxs-lookup"><span data-stu-id="ed759-146">Aliases</span></span> |<span data-ttu-id="ed759-147">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="ed759-147">none</span></span> |
| <span data-ttu-id="ed759-148">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="ed759-148">Required?</span></span> |<span data-ttu-id="ed759-149">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-149">false</span></span> |
| <span data-ttu-id="ed759-150">Beosztás</span><span class="sxs-lookup"><span data-stu-id="ed759-150">Position</span></span> |<span data-ttu-id="ed759-151">nevű</span><span class="sxs-lookup"><span data-stu-id="ed759-151">named</span></span> |
| <span data-ttu-id="ed759-152">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ed759-152">Default value</span></span> |<span data-ttu-id="ed759-153">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="ed759-153">none</span></span> |
| <span data-ttu-id="ed759-154">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="ed759-154">Accept pipeline input?</span></span> |<span data-ttu-id="ed759-155">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-155">false</span></span> |
| <span data-ttu-id="ed759-156">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="ed759-156">Accept wildcard characters?</span></span> |<span data-ttu-id="ed759-157">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-157">false</span></span> |

## <a name="databaseserverpassword"></a><span data-ttu-id="ed759-158">DatabaseServerPassword</span><span class="sxs-lookup"><span data-stu-id="ed759-158">DatabaseServerPassword</span></span>
<span data-ttu-id="ed759-159">A felhasználónevet és jelszót az SQL-adatbázis, az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ed759-159">The username and password for the SQL database in Azure.</span></span>

| <span data-ttu-id="ed759-160">Paraméter</span><span class="sxs-lookup"><span data-stu-id="ed759-160">Parameter</span></span> | <span data-ttu-id="ed759-161">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ed759-161">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="ed759-162">Aliasok</span><span class="sxs-lookup"><span data-stu-id="ed759-162">Aliases</span></span> |<span data-ttu-id="ed759-163">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="ed759-163">none</span></span> |
| <span data-ttu-id="ed759-164">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="ed759-164">Required?</span></span> |<span data-ttu-id="ed759-165">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-165">false</span></span> |
| <span data-ttu-id="ed759-166">Beosztás</span><span class="sxs-lookup"><span data-stu-id="ed759-166">Position</span></span> |<span data-ttu-id="ed759-167">nevű</span><span class="sxs-lookup"><span data-stu-id="ed759-167">named</span></span> |
| <span data-ttu-id="ed759-168">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ed759-168">Default value</span></span> |<span data-ttu-id="ed759-169">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="ed759-169">none</span></span> |
| <span data-ttu-id="ed759-170">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="ed759-170">Accept pipeline input?</span></span> |<span data-ttu-id="ed759-171">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-171">false</span></span> |
| <span data-ttu-id="ed759-172">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="ed759-172">Accept wildcard characters?</span></span> |<span data-ttu-id="ed759-173">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-173">false</span></span> |

## <a name="sendhostmessagestooutput"></a><span data-ttu-id="ed759-174">SendHostMessagesToOutput</span><span class="sxs-lookup"><span data-stu-id="ed759-174">SendHostMessagesToOutput</span></span>
<span data-ttu-id="ed759-175">Amennyiben az értéke igaz, a nyomtató érkező üzenetek a parancsfájl a kimeneti adatfolyamba.</span><span class="sxs-lookup"><span data-stu-id="ed759-175">If true, print messages from the script to the output stream.</span></span>

| <span data-ttu-id="ed759-176">Paraméter</span><span class="sxs-lookup"><span data-stu-id="ed759-176">Parameter</span></span> | <span data-ttu-id="ed759-177">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ed759-177">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="ed759-178">Aliasok</span><span class="sxs-lookup"><span data-stu-id="ed759-178">Aliases</span></span> |<span data-ttu-id="ed759-179">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="ed759-179">none</span></span> |
| <span data-ttu-id="ed759-180">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="ed759-180">Required?</span></span> |<span data-ttu-id="ed759-181">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-181">false</span></span> |
| <span data-ttu-id="ed759-182">Beosztás</span><span class="sxs-lookup"><span data-stu-id="ed759-182">Position</span></span> |<span data-ttu-id="ed759-183">nevű</span><span class="sxs-lookup"><span data-stu-id="ed759-183">named</span></span> |
| <span data-ttu-id="ed759-184">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ed759-184">Default value</span></span> |<span data-ttu-id="ed759-185">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-185">false</span></span> |
| <span data-ttu-id="ed759-186">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="ed759-186">Accept pipeline input?</span></span> |<span data-ttu-id="ed759-187">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-187">false</span></span> |
| <span data-ttu-id="ed759-188">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="ed759-188">Accept wildcard characters?</span></span> |<span data-ttu-id="ed759-189">hamis</span><span class="sxs-lookup"><span data-stu-id="ed759-189">false</span></span> |

## <a name="remarks"></a><span data-ttu-id="ed759-190">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ed759-190">Remarks</span></span>
<span data-ttu-id="ed759-191">A parancsfájl használata létrehozásához teljes leírását fejlesztési és tesztkörnyezetek, lásd: [Windows PowerShell parancsfájlok használata a közzététel a fejlesztési és tesztkörnyezetek](vs-azure-tools-publishing-using-powershell-scripts.md).</span><span class="sxs-lookup"><span data-stu-id="ed759-191">For a complete explanation of how to use the script to create Dev and Test environments, see [Using Windows PowerShell Scripts to Publish to Dev and Test Environments](vs-azure-tools-publishing-using-powershell-scripts.md).</span></span>

<span data-ttu-id="ed759-192">A JSON-konfigurációs fájl meghatározza, hogy mit telepítendő részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="ed759-192">The JSON configuration file specifies the details of what is to be deployed.</span></span> <span data-ttu-id="ed759-193">Ez magában foglalja a projekt, például a nevét, és a felhasználónév, a webhely létrehozásakor megadott adatokat.</span><span class="sxs-lookup"><span data-stu-id="ed759-193">It includes the information that you specified when you created the project, such as the name and username for the website.</span></span> <span data-ttu-id="ed759-194">Is az adatbázis rendelkezésre, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="ed759-194">It also includes the database to provision, if any.</span></span> <span data-ttu-id="ed759-195">A következő kód bemutatja egy példa JSON-konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="ed759-195">The following code shows an example JSON configuration file:</span></span>

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

<span data-ttu-id="ed759-196">Szerkesztheti a JSON-konfigurációs fájl módosítása, hogy telepítve van.</span><span class="sxs-lookup"><span data-stu-id="ed759-196">You can edit the JSON configuration file to change what is deployed.</span></span> <span data-ttu-id="ed759-197">A webhely szakaszban szükség, de az adatbázis szakaszban nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="ed759-197">A webSite section is required, but the database section is optional.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed759-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ed759-198">Next steps</span></span>
<span data-ttu-id="ed759-199">További információkért lásd: [Publish-WebApplicationVM (a Windows PowerShell-parancsfájl)](vs-azure-tools-publish-webapplicationvm.md)</span><span class="sxs-lookup"><span data-stu-id="ed759-199">For more information, see [Publish-WebApplicationVM (Windows PowerShell script)](vs-azure-tools-publish-webapplicationvm.md)</span></span>

