---
title: "(a Windows PowerShell-parancsfájl) aaaPublish-WebApplicationWebSite |} Microsoft Docs"
description: "Ismerje meg, hogyan toopublish egy webes projekt tooan Azure-webhelyen. Ez a parancsfájl hello szükséges erőforrások az Azure-előfizetése hoz létre, ha azok még nem léteznek."
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
ms.openlocfilehash: d46904e30e3c2e040e57888fa31543e8e366527f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a><span data-ttu-id="af272-104">(A Windows PowerShell-parancsfájl) közzététele-WebApplicationWebSite</span><span class="sxs-lookup"><span data-stu-id="af272-104">Publish-WebApplicationWebSite (Windows PowerShell script)</span></span>
## <a name="syntax"></a><span data-ttu-id="af272-105">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="af272-105">Syntax</span></span>
<span data-ttu-id="af272-106">Közzéteszi a webes projekt tooan Azure-webhelyen.</span><span class="sxs-lookup"><span data-stu-id="af272-106">Publishes a web project tooan Azure website.</span></span> <span data-ttu-id="af272-107">hello parancsfájl hello szükséges erőforrások az Azure-előfizetése hoz létre, ha azok még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="af272-107">hello script creates hello required resources in your Azure subscription if they don't exist.</span></span>

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a><span data-ttu-id="af272-108">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="af272-108">Configuration</span></span>
<span data-ttu-id="af272-109">hello elérési toohello JSON-konfigurációs fájlt, amely hello hello központi telepítés részleteit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="af272-109">hello path toohello JSON configuration file that describes hello details of hello deployment.</span></span>

| <span data-ttu-id="af272-110">Paraméter</span><span class="sxs-lookup"><span data-stu-id="af272-110">Parameter</span></span> | <span data-ttu-id="af272-111">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="af272-111">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="af272-112">Aliasok</span><span class="sxs-lookup"><span data-stu-id="af272-112">Aliases</span></span> |<span data-ttu-id="af272-113">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="af272-113">none</span></span> |
| <span data-ttu-id="af272-114">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="af272-114">Required?</span></span> |<span data-ttu-id="af272-115">Igaz</span><span class="sxs-lookup"><span data-stu-id="af272-115">true</span></span> |
| <span data-ttu-id="af272-116">Beosztás</span><span class="sxs-lookup"><span data-stu-id="af272-116">Position</span></span> |<span data-ttu-id="af272-117">nevű</span><span class="sxs-lookup"><span data-stu-id="af272-117">named</span></span> |
| <span data-ttu-id="af272-118">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="af272-118">Default value</span></span> |<span data-ttu-id="af272-119">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="af272-119">none</span></span> |
| <span data-ttu-id="af272-120">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="af272-120">Accept pipeline input?</span></span> |<span data-ttu-id="af272-121">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-121">false</span></span> |
| <span data-ttu-id="af272-122">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="af272-122">Accept wildcard characters?</span></span> |<span data-ttu-id="af272-123">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-123">false</span></span> |

## <a name="subscriptionname"></a><span data-ttu-id="af272-124">SubscriptionName</span><span class="sxs-lookup"><span data-stu-id="af272-124">SubscriptionName</span></span>
<span data-ttu-id="af272-125">hello hello Azure-előfizetést, amelyet szeretne toocreate hello webhely neve.</span><span class="sxs-lookup"><span data-stu-id="af272-125">hello name of hello Azure subscription that you want toocreate hello website in.</span></span>

| <span data-ttu-id="af272-126">Paraméter</span><span class="sxs-lookup"><span data-stu-id="af272-126">Parameter</span></span> | <span data-ttu-id="af272-127">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="af272-127">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="af272-128">Aliasok</span><span class="sxs-lookup"><span data-stu-id="af272-128">Aliases</span></span> |<span data-ttu-id="af272-129">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="af272-129">none</span></span> |
| <span data-ttu-id="af272-130">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="af272-130">Required?</span></span> |<span data-ttu-id="af272-131">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-131">false</span></span> |
| <span data-ttu-id="af272-132">Beosztás</span><span class="sxs-lookup"><span data-stu-id="af272-132">Position</span></span> |<span data-ttu-id="af272-133">nevű</span><span class="sxs-lookup"><span data-stu-id="af272-133">named</span></span> |
| <span data-ttu-id="af272-134">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="af272-134">Default value</span></span> |<span data-ttu-id="af272-135">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="af272-135">none</span></span> |
| <span data-ttu-id="af272-136">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="af272-136">Accept pipeline input?</span></span> |<span data-ttu-id="af272-137">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-137">false</span></span> |
| <span data-ttu-id="af272-138">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="af272-138">Accept wildcard characters?</span></span> |<span data-ttu-id="af272-139">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-139">false</span></span> |

## <a name="webdeploypackage"></a><span data-ttu-id="af272-140">WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="af272-140">WebDeployPackage</span></span>
<span data-ttu-id="af272-141">hello elérési toohello webes telepítési csomag toopublish toohello webhelyet.</span><span class="sxs-lookup"><span data-stu-id="af272-141">hello path toohello web deployment package toopublish toohello website.</span></span> <span data-ttu-id="af272-142">Ezt a csomagot a Visual Studio hello webhely közzététele varázsló használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="af272-142">You can create this package by using hello Publish Web wizard in Visual Studio.</span></span> <span data-ttu-id="af272-143">További információkért lásd: [Ismerkedés az Azure felhőalapú szolgáltatásairól és ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).</span><span class="sxs-lookup"><span data-stu-id="af272-143">For more information, see [Get started with Azure Cloud Services and ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).</span></span>

| <span data-ttu-id="af272-144">Paraméter</span><span class="sxs-lookup"><span data-stu-id="af272-144">Parameter</span></span> | <span data-ttu-id="af272-145">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="af272-145">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="af272-146">Aliasok</span><span class="sxs-lookup"><span data-stu-id="af272-146">Aliases</span></span> |<span data-ttu-id="af272-147">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="af272-147">none</span></span> |
| <span data-ttu-id="af272-148">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="af272-148">Required?</span></span> |<span data-ttu-id="af272-149">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-149">false</span></span> |
| <span data-ttu-id="af272-150">Beosztás</span><span class="sxs-lookup"><span data-stu-id="af272-150">Position</span></span> |<span data-ttu-id="af272-151">nevű</span><span class="sxs-lookup"><span data-stu-id="af272-151">named</span></span> |
| <span data-ttu-id="af272-152">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="af272-152">Default value</span></span> |<span data-ttu-id="af272-153">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="af272-153">none</span></span> |
| <span data-ttu-id="af272-154">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="af272-154">Accept pipeline input?</span></span> |<span data-ttu-id="af272-155">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-155">false</span></span> |
| <span data-ttu-id="af272-156">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="af272-156">Accept wildcard characters?</span></span> |<span data-ttu-id="af272-157">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-157">false</span></span> |

## <a name="databaseserverpassword"></a><span data-ttu-id="af272-158">DatabaseServerPassword</span><span class="sxs-lookup"><span data-stu-id="af272-158">DatabaseServerPassword</span></span>
<span data-ttu-id="af272-159">hello felhasználónév és jelszó hello SQL-adatbázis az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="af272-159">hello username and password for hello SQL database in Azure.</span></span>

| <span data-ttu-id="af272-160">Paraméter</span><span class="sxs-lookup"><span data-stu-id="af272-160">Parameter</span></span> | <span data-ttu-id="af272-161">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="af272-161">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="af272-162">Aliasok</span><span class="sxs-lookup"><span data-stu-id="af272-162">Aliases</span></span> |<span data-ttu-id="af272-163">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="af272-163">none</span></span> |
| <span data-ttu-id="af272-164">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="af272-164">Required?</span></span> |<span data-ttu-id="af272-165">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-165">false</span></span> |
| <span data-ttu-id="af272-166">Beosztás</span><span class="sxs-lookup"><span data-stu-id="af272-166">Position</span></span> |<span data-ttu-id="af272-167">nevű</span><span class="sxs-lookup"><span data-stu-id="af272-167">named</span></span> |
| <span data-ttu-id="af272-168">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="af272-168">Default value</span></span> |<span data-ttu-id="af272-169">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="af272-169">none</span></span> |
| <span data-ttu-id="af272-170">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="af272-170">Accept pipeline input?</span></span> |<span data-ttu-id="af272-171">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-171">false</span></span> |
| <span data-ttu-id="af272-172">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="af272-172">Accept wildcard characters?</span></span> |<span data-ttu-id="af272-173">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-173">false</span></span> |

## <a name="sendhostmessagestooutput"></a><span data-ttu-id="af272-174">SendHostMessagesToOutput</span><span class="sxs-lookup"><span data-stu-id="af272-174">SendHostMessagesToOutput</span></span>
<span data-ttu-id="af272-175">Igaz értéke esetén a nyomtatási üzeneteit hello parancsfájl toohello kimeneti adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="af272-175">If true, print messages from hello script toohello output stream.</span></span>

| <span data-ttu-id="af272-176">Paraméter</span><span class="sxs-lookup"><span data-stu-id="af272-176">Parameter</span></span> | <span data-ttu-id="af272-177">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="af272-177">Default value</span></span> |
| --- | --- |
| <span data-ttu-id="af272-178">Aliasok</span><span class="sxs-lookup"><span data-stu-id="af272-178">Aliases</span></span> |<span data-ttu-id="af272-179">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="af272-179">none</span></span> |
| <span data-ttu-id="af272-180">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="af272-180">Required?</span></span> |<span data-ttu-id="af272-181">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-181">false</span></span> |
| <span data-ttu-id="af272-182">Beosztás</span><span class="sxs-lookup"><span data-stu-id="af272-182">Position</span></span> |<span data-ttu-id="af272-183">nevű</span><span class="sxs-lookup"><span data-stu-id="af272-183">named</span></span> |
| <span data-ttu-id="af272-184">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="af272-184">Default value</span></span> |<span data-ttu-id="af272-185">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-185">false</span></span> |
| <span data-ttu-id="af272-186">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="af272-186">Accept pipeline input?</span></span> |<span data-ttu-id="af272-187">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-187">false</span></span> |
| <span data-ttu-id="af272-188">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="af272-188">Accept wildcard characters?</span></span> |<span data-ttu-id="af272-189">hamis</span><span class="sxs-lookup"><span data-stu-id="af272-189">false</span></span> |

## <a name="remarks"></a><span data-ttu-id="af272-190">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="af272-190">Remarks</span></span>
<span data-ttu-id="af272-191">Hogyan toouse hello parancsfájl toocreate fejlesztési és tesztkörnyezetek: teljes leírását [Windows PowerShell-parancsfájlok használatával tooPublish tooDev és a tesztkörnyezetek](vs-azure-tools-publishing-using-powershell-scripts.md).</span><span class="sxs-lookup"><span data-stu-id="af272-191">For a complete explanation of how toouse hello script toocreate Dev and Test environments, see [Using Windows PowerShell Scripts tooPublish tooDev and Test Environments](vs-azure-tools-publishing-using-powershell-scripts.md).</span></span>

<span data-ttu-id="af272-192">hello JSON-konfigurációs fájlt határozza meg, hogy mit telepített toobe hello részletei.</span><span class="sxs-lookup"><span data-stu-id="af272-192">hello JSON configuration file specifies hello details of what is toobe deployed.</span></span> <span data-ttu-id="af272-193">Ez magában foglalja a hello projekt, például hello nevét és a felhasználónév hello webhely létrehozásakor megadott hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="af272-193">It includes hello information that you specified when you created hello project, such as hello name and username for hello website.</span></span> <span data-ttu-id="af272-194">Is hello adatbázis tooprovision, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="af272-194">It also includes hello database tooprovision, if any.</span></span> <span data-ttu-id="af272-195">a következő kód hello látható egy példa JSON-konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="af272-195">hello following code shows an example JSON configuration file:</span></span>

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

<span data-ttu-id="af272-196">Szerkesztheti a hello JSON konfigurációs fájl toochange telepítve van.</span><span class="sxs-lookup"><span data-stu-id="af272-196">You can edit hello JSON configuration file toochange what is deployed.</span></span> <span data-ttu-id="af272-197">A webhely szakaszban szükség, de hello adatbázis szakasz nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="af272-197">A webSite section is required, but hello database section is optional.</span></span>

## <a name="next-steps"></a><span data-ttu-id="af272-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="af272-198">Next steps</span></span>
<span data-ttu-id="af272-199">További információkért lásd: [Publish-WebApplicationVM (a Windows PowerShell-parancsfájl)](vs-azure-tools-publish-webapplicationvm.md)</span><span class="sxs-lookup"><span data-stu-id="af272-199">For more information, see [Publish-WebApplicationVM (Windows PowerShell script)](vs-azure-tools-publish-webapplicationvm.md)</span></span>

