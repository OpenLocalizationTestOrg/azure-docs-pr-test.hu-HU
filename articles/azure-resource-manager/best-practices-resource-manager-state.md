---
title: "aaaPass összetett értékek tartománya: Azure-sablonok |} Microsoft Docs"
description: "Azt mutatja be ajánlott megközelítés használatos összetett objektumok tooshare állapot adatait az Azure Resource Manager-sablonok és a csatolt sablonok használatával."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fd2f5e2d-d56d-4e01-a57d-34f3eaead4a9
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: tomfitz
ms.openlocfilehash: 72df1dee351446cea6ce15269e6db288b1f1db79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="share-state-tooand-from-azure-resource-manager-templates"></a><span data-ttu-id="2eeb4-103">Megosztás állapot tooand Azure Resource Manager-sablonok alapján</span><span class="sxs-lookup"><span data-stu-id="2eeb4-103">Share state tooand from Azure Resource Manager templates</span></span>
<span data-ttu-id="2eeb4-104">Ez a témakör gyakorlati tanácsok a kezelése és megosztása a sablonokon belüli állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-104">This topic shows best practices for managing and sharing state within templates.</span></span> <span data-ttu-id="2eeb4-105">hello paraméterek és változók ebben a témakörben szereplő példák hello típusú objektumok definiálhat tooconveniently rendezheti a telepítési követelményeket.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-105">hello parameters and variables shown in this topic are examples of hello type of objects you can define tooconveniently organize your deployment requirements.</span></span> <span data-ttu-id="2eeb4-106">Ezekben a példákban a saját objektumok, amelyek a környezetben jelentéssel bírnak valósíthat meg.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-106">From these examples, you can implement your own objects with property values that make sense for your environment.</span></span>

<span data-ttu-id="2eeb4-107">Ez a témakör egy nagyobb tanulmány részét képezi.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-107">This topic is part of a larger whitepaper.</span></span> <span data-ttu-id="2eeb4-108">Töltse le a teljes papír tooread hello [globális osztály Resource Manager sablonok szempontok és eljárások bizonyítása](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).</span><span class="sxs-lookup"><span data-stu-id="2eeb4-108">tooread hello full paper, download [World Class Resource Manager Templates Considerations and Proven Practices](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).</span></span>

## <a name="provide-standard-configuration-settings"></a><span data-ttu-id="2eeb4-109">Adja meg a szabványos konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="2eeb4-109">Provide standard configuration settings</span></span>
<span data-ttu-id="2eeb4-110">Helyett a sablont, amely a teljes és számos változata kínál, egy közös mintát a kijelölt ismert konfigurációk tooprovide.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-110">Rather than offer a template that provides total flexibility and countless variations, a common pattern is tooprovide a selection of known configurations.</span></span> <span data-ttu-id="2eeb4-111">Érvényben a felhasználók kiválaszthatják például védőfal kis, közepes és nagy szabványos pólóra méretét.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-111">In effect, users can select standard t-shirt sizes such as sandbox, small, medium, and large.</span></span> <span data-ttu-id="2eeb4-112">Más pólóra méretű például a termék ajánlatokat, community edition vagy enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-112">Other examples of t-shirt sizes are product offerings, such as community edition or enterprise edition.</span></span> <span data-ttu-id="2eeb4-113">Más esetekben lehet munkaterhelés-specifikus konfigurációk egy technológia – például a térkép csökkentése vagy a nem sql.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-113">In other cases, it may be workload-specific configurations of a technology – such as map reduce or no sql.</span></span>

<span data-ttu-id="2eeb4-114">Összetett objektumok, amelyek tartalmazzák a gyűjtött adatokat, más néven "tulajdonságcsomagok" változók létrehozása és a sablonban lévő adatok toodrive hello erőforrás nyilatkozatot használva.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-114">With complex objects, you can create variables that contain collections of data, sometimes known as "property bags" and use that data toodrive hello resource declaration in your template.</span></span> <span data-ttu-id="2eeb4-115">Ezt a módszert biztosít az ügyfelek különböző méretű előre konfigurált megfelelő, ismert konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-115">This approach provides good, known configurations of varying sizes that are preconfigured for customers.</span></span> <span data-ttu-id="2eeb4-116">Ismert konfigurációk nélkül hello sablon felhasználók kell fürt méretezése a platform erőforrás korlátozó saját, tényező határozza meg, és tegye matematikai tooidentify hello eredő storage-fiókok és egyéb erőforrások particionálás (toocluster mérete miatt és erőforrás-korlátozások).</span><span class="sxs-lookup"><span data-stu-id="2eeb4-116">Without known configurations, users of hello template must determine cluster sizing on their own, factor in platform resource constraints, and do math tooidentify hello resulting partitioning of storage accounts and other resources (due toocluster size and resource constraints).</span></span> <span data-ttu-id="2eeb4-117">Továbbá toomaking hello ügyfél hatékonyabb működését, néhány ismert konfigurációk egyszerűbb toosupport és sűrűség magasabb szintű fájlmegosztásba nyújt segítséget.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-117">In addition toomaking a better experience for hello customer, a few known configurations are easier toosupport and can help you deliver a higher level of density.</span></span>

<span data-ttu-id="2eeb4-118">a következő példa azt mutatja meg hogyan hello toodefine változókat, amelyek tartalmazzák a gyűjtött adatokat jelképező összetett objektumra.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-118">hello following example shows how toodefine variables that contain complex objects for representing collections of data.</span></span> <span data-ttu-id="2eeb4-119">hello gyűjtemények megadása a virtuális gép méretét, a hálózati beállításokat, az operációs rendszeri beállítások és a rendelkezésre állási beállítások használt értékek.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-119">hello collections define values that are used for virtual machine size, network settings, operating system settings and availability settings.</span></span>

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

<span data-ttu-id="2eeb4-120">Figyelje meg, hogy hello **tshirtSize** változó összefűzi egy paraméteren keresztül megadott hello Póló mérete (**kis**, **Közepes**, **nagy**) toohello szöveg **tshirtSize**.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-120">Notice that hello **tshirtSize** variable concatenates hello t-shirt size you provided through a parameter (**Small**, **Medium**, **Large**) toohello text **tshirtSize**.</span></span> <span data-ttu-id="2eeb4-121">A változó tooretrieve hello társított összetett objektumot változó használata pólóra méret.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-121">You use this variable tooretrieve hello associated complex object variable for that t-shirt size.</span></span>

<span data-ttu-id="2eeb4-122">Ezek a változók később hello sablonban majd hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-122">You can then reference these variables later in hello template.</span></span> <span data-ttu-id="2eeb4-123">hello képességét tooreference nevű-változók és azok tulajdonságaival egyszerűbbé teszi a hello sablonok szintaxisát, és teszi, hogy könnyen toounderstand környezetben.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-123">hello ability tooreference named-variables and their properties simplifies hello template syntax, and makes it easy toounderstand context.</span></span> <span data-ttu-id="2eeb4-124">a következő példa hello egy erőforrás toodeploy hello objektumok előzőleg bemutatott tooset értékek használatával határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-124">hello following example defines a resource toodeploy by using hello objects shown previously tooset values.</span></span> <span data-ttu-id="2eeb4-125">Hello Virtuálisgép-méretet lekérésével hello értékét állítsa például `variables('tshirtSize').vmSize` hello érték során hello lemez méretét veszi át a `variables('tshirtSize').diskSize`.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-125">For example, hello VM size is set by retrieving hello value for `variables('tshirtSize').vmSize` while hello value for hello disk size is retrieved from `variables('tshirtSize').diskSize`.</span></span> <span data-ttu-id="2eeb4-126">Emellett a csatolt sablon hello értékkel van beállítva a hello URI `variables('tshirtSize').vmTemplate`.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-126">In addition, hello URI for a linked template is set with hello value for `variables('tshirtSize').vmTemplate`.</span></span>

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-tooa-template"></a><span data-ttu-id="2eeb4-127">Hozzáférési állapot tooa sablon</span><span class="sxs-lookup"><span data-stu-id="2eeb4-127">Pass state tooa template</span></span>
<span data-ttu-id="2eeb4-128">Megosztott állapot paramétereknek, hogy közvetlenül a telepítés során a sablonba.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-128">You share state into a template through parameters that you provide directly during deployment.</span></span>

<span data-ttu-id="2eeb4-129">a következő tábla listák általánosan használt paraméterek a sablonok hello.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-129">hello following table lists commonly used parameters in templates.</span></span>

| <span data-ttu-id="2eeb4-130">Név</span><span class="sxs-lookup"><span data-stu-id="2eeb4-130">Name</span></span> | <span data-ttu-id="2eeb4-131">Érték</span><span class="sxs-lookup"><span data-stu-id="2eeb4-131">Value</span></span> | <span data-ttu-id="2eeb4-132">Leírás</span><span class="sxs-lookup"><span data-stu-id="2eeb4-132">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2eeb4-133">location</span><span class="sxs-lookup"><span data-stu-id="2eeb4-133">location</span></span> |<span data-ttu-id="2eeb4-134">Az Azure-régiók korlátozott listájából karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2eeb4-134">String from a constrained list of Azure regions</span></span> |<span data-ttu-id="2eeb4-135">hello hely, ahol hello erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-135">hello location where hello resources are deployed.</span></span> |
| <span data-ttu-id="2eeb4-136">storageAccountNamePrefix</span><span class="sxs-lookup"><span data-stu-id="2eeb4-136">storageAccountNamePrefix</span></span> |<span data-ttu-id="2eeb4-137">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2eeb4-137">String</span></span> |<span data-ttu-id="2eeb4-138">Hello Tárfiókot, ahol hello virtuális gép lemezeinek kerülnek egyedi DNS-neve</span><span class="sxs-lookup"><span data-stu-id="2eeb4-138">Unique DNS name for hello Storage Account where hello VM's disks are placed</span></span> |
| <span data-ttu-id="2eeb4-139">Tartománynév</span><span class="sxs-lookup"><span data-stu-id="2eeb4-139">domainName</span></span> |<span data-ttu-id="2eeb4-140">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2eeb4-140">String</span></span> |<span data-ttu-id="2eeb4-141">Hello nyilvánosan elérhető jumpbox VM hello formátumban tartománynevet: **{tartománynév}. { hely}.cloudapp.com** például: **mydomainname.westus.cloudapp.azure.com**</span><span class="sxs-lookup"><span data-stu-id="2eeb4-141">Domain name of hello publicly accessible jumpbox VM in hello format: **{domainName}.{location}.cloudapp.com** For example: **mydomainname.westus.cloudapp.azure.com**</span></span> |
| <span data-ttu-id="2eeb4-142">adminUsername</span><span class="sxs-lookup"><span data-stu-id="2eeb4-142">adminUsername</span></span> |<span data-ttu-id="2eeb4-143">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2eeb4-143">String</span></span> |<span data-ttu-id="2eeb4-144">Virtuális gépek hello tartozó felhasználónév</span><span class="sxs-lookup"><span data-stu-id="2eeb4-144">Username for hello VMs</span></span> |
| <span data-ttu-id="2eeb4-145">adminPassword</span><span class="sxs-lookup"><span data-stu-id="2eeb4-145">adminPassword</span></span> |<span data-ttu-id="2eeb4-146">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2eeb4-146">String</span></span> |<span data-ttu-id="2eeb4-147">Virtuális gépek hello jelszavát</span><span class="sxs-lookup"><span data-stu-id="2eeb4-147">Password for hello VMs</span></span> |
| <span data-ttu-id="2eeb4-148">tshirtSize</span><span class="sxs-lookup"><span data-stu-id="2eeb4-148">tshirtSize</span></span> |<span data-ttu-id="2eeb4-149">A korlátozott listájából karakterlánc kínált pólóra méretek</span><span class="sxs-lookup"><span data-stu-id="2eeb4-149">String from a constrained list of offered t-shirt sizes</span></span> |<span data-ttu-id="2eeb4-150">hello nevű méretezési egység méretének tooprovision.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-150">hello named scale unit size tooprovision.</span></span> <span data-ttu-id="2eeb4-151">Például "Kicsi", "közepes", "Nagy"</span><span class="sxs-lookup"><span data-stu-id="2eeb4-151">For example, "Small", "Medium", "Large"</span></span> |
| <span data-ttu-id="2eeb4-152">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="2eeb4-152">virtualNetworkName</span></span> |<span data-ttu-id="2eeb4-153">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2eeb4-153">String</span></span> |<span data-ttu-id="2eeb4-154">Hello fogyasztói hello virtuális hálózat neve toouse szeretne.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-154">Name of hello virtual network that hello consumer wants toouse.</span></span> |
| <span data-ttu-id="2eeb4-155">enableJumpbox</span><span class="sxs-lookup"><span data-stu-id="2eeb4-155">enableJumpbox</span></span> |<span data-ttu-id="2eeb4-156">Korlátozott listájából (engedélyezett vagy letiltott) karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2eeb4-156">String from a constrained list (enabled/disabled)</span></span> |<span data-ttu-id="2eeb4-157">Paraméter, amely azonosítja az e tooenable egy jumpbox hello környezethez.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-157">Parameter that identifies whether tooenable a jumpbox for hello environment.</span></span> <span data-ttu-id="2eeb4-158">Értékek: "engedélyezve", "Letiltva"</span><span class="sxs-lookup"><span data-stu-id="2eeb4-158">Values: "enabled", "disabled"</span></span> |

<span data-ttu-id="2eeb4-159">Hello **tshirtSize** hello előző szakaszban használt paraméter típusúként van definiálva:</span><span class="sxs-lookup"><span data-stu-id="2eeb4-159">hello **tshirtSize** parameter used in hello previous section is defined as:</span></span>

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of hello MongoDB deployment"
        }
      }
    }


## <a name="pass-state-toolinked-templates"></a><span data-ttu-id="2eeb4-160">Továbbítsa állapot toolinked sablonok</span><span class="sxs-lookup"><span data-stu-id="2eeb4-160">Pass state toolinked templates</span></span>
<span data-ttu-id="2eeb4-161">Ha toolinked sablonok, gyakran statikus vegyesen használ, és létrehozott változókat.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-161">When connecting toolinked templates, you often use a mix of static and generated variables.</span></span>

### <a name="static-variables"></a><span data-ttu-id="2eeb4-162">Statikus változó</span><span class="sxs-lookup"><span data-stu-id="2eeb4-162">Static variables</span></span>
<span data-ttu-id="2eeb4-163">Statikus változók értékei gyakran használt tooprovide alap, például a használt URL-, egy sablon egész.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-163">Static variables are often used tooprovide base values, such as URLs, that are used throughout a template.</span></span>

<span data-ttu-id="2eeb4-164">A következő sablon cikkből hello `templateBaseUrl` hello gyökerét hello sablon meghatározza a Githubon.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-164">In hello following template excerpt, `templateBaseUrl` specifies hello root location for hello template in GitHub.</span></span> <span data-ttu-id="2eeb4-165">hello következő sor összeállít egy új változót `sharedTemplateUrl` , amely összefűzi hello alap URL-cím elé hello ismert hello megosztott erőforrások sablon nevét.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-165">hello next line builds a new variable `sharedTemplateUrl` that concatenates hello base URL with hello known name of hello shared resources template.</span></span> <span data-ttu-id="2eeb4-166">Adott sor alatt egy összetett objektum változó akkor használt toostore pólóra méretétől, melynél hello alap URL-cím összefűzött toohello ismert konfiguráció sablon helyére, és a rendszer a hello `vmTemplate` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-166">Below that line, a complex object variable is used toostore a t-shirt size, where hello base URL is concatenated toohello known configuration template location and stored in hello `vmTemplate` property.</span></span>

<span data-ttu-id="2eeb4-167">hello ezt a módszert előnye, hogy ha hello sablon helye megváltozik, csak kell toochange hello statikus változó az egyik helyen, amely továbbítja azokat a teljes kapcsolódó hello sablonok.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-167">hello benefit of this approach is that if hello template location changes, you only need toochange hello static variable in one place, which passes it throughout hello linked templates.</span></span>

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a><span data-ttu-id="2eeb4-168">Létrehozott változók</span><span class="sxs-lookup"><span data-stu-id="2eeb4-168">Generated variables</span></span>
<span data-ttu-id="2eeb4-169">Továbbá toostatic változók több változók generálása dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-169">In addition toostatic variables, several variables are generated dynamically.</span></span> <span data-ttu-id="2eeb4-170">Ez a szakasz azonosítja a legelterjedtebb fajtáit hello közös létrehozott változókat.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-170">This section identifies some of hello common types of generated variables.</span></span>

#### <a name="tshirtsize"></a><span data-ttu-id="2eeb4-171">tshirtSize</span><span class="sxs-lookup"><span data-stu-id="2eeb4-171">tshirtSize</span></span>
<span data-ttu-id="2eeb4-172">A fenti hello példákban a létrehozott változó tisztában.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-172">You are familiar with this generated variable from hello examples above.</span></span>

#### <a name="networksettings"></a><span data-ttu-id="2eeb4-173">networkSettings</span><span class="sxs-lookup"><span data-stu-id="2eeb4-173">networkSettings</span></span>
<span data-ttu-id="2eeb4-174">A kapacitás, a funkció vagy a végpont hatókörön belüli megoldássablonban, hello kapcsolt sablonok általában létrehozása meglévő erőforrások hálózaton.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-174">In a capacity, capability, or end-to-end scoped solution template, hello linked templates typically create resources that exist on a network.</span></span> <span data-ttu-id="2eeb4-175">Egy egyszerű megközelítése toouse egy összetett objektum toostore hálózati beállításokat, és adja meg azokat toolinked sablonok.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-175">One straightforward approach is toouse a complex object toostore network settings and pass them toolinked templates.</span></span>

<span data-ttu-id="2eeb4-176">Hálózati beállítások kommunikáció például alább látható.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-176">An example of communicating network settings can be seen below.</span></span>

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a><span data-ttu-id="2eeb4-177">availabilitySettings</span><span class="sxs-lookup"><span data-stu-id="2eeb4-177">availabilitySettings</span></span>
<span data-ttu-id="2eeb4-178">A csatolt sablonok létrejött erőforrásokat gyakran kerülnek egy rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-178">Resources created in linked templates are often placed in an availability set.</span></span> <span data-ttu-id="2eeb4-179">A következő példa hello hello rendelkezésre állási készlet neve van megadva, és is hello tartalék tartományt, és tartományi száma toouse frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-179">In hello following example, hello availability set name is specified and also hello fault domain and update domain count toouse.</span></span>

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

<span data-ttu-id="2eeb4-180">Ha több rendelkezésre állási csoportok (például egy fő csomópontok) és egy másik adatok csomópontok előtagjaként is használhatja a nevét, adjon meg több rendelkezésre állási csoportok, vagy hello modell létrehozásához egy adott pólóra méretű változót korábban bemutatott kövesse.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-180">If you need multiple availability sets (for example, one for master nodes and another for data nodes), you can use a name as a prefix, specify multiple availability sets, or follow hello model shown earlier for creating a variable for a specific t-shirt size.</span></span>

#### <a name="storagesettings"></a><span data-ttu-id="2eeb4-181">storageSettings</span><span class="sxs-lookup"><span data-stu-id="2eeb4-181">storageSettings</span></span>
<span data-ttu-id="2eeb4-182">Csatolt sablonok gyakran megosztott tárolási részleteit.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-182">Storage details are often shared with linked templates.</span></span> <span data-ttu-id="2eeb4-183">Az alábbi hello példában egy *storageSettings* objektum ismerteti hello tárolási fiók és a tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-183">In hello example below, a *storageSettings* object provides details about hello storage account and container names.</span></span>

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a><span data-ttu-id="2eeb4-184">osSettings</span><span class="sxs-lookup"><span data-stu-id="2eeb4-184">osSettings</span></span>
<span data-ttu-id="2eeb4-185">A csatolt sablonokkal szükség lehet az toopass operációs rendszer beállításait toovarious csomópontok típusú különböző ismert konfigurációs típusok között.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-185">With linked templates, you may need toopass operating system settings toovarious nodes types across different known configuration types.</span></span> <span data-ttu-id="2eeb4-186">Egy összetett objektumot egy egyszerűen toostore és megosztási operációsrendszer-információkat, és megkönnyíti a könnyebb toosupport több operációs rendszer lehetőség központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-186">A complex object is an easy way toostore and share operating system information and also makes it easier toosupport multiple operating system choices for deployment.</span></span>

<span data-ttu-id="2eeb4-187">hello alábbi példa bemutatja egy objektumot *osSettings*:</span><span class="sxs-lookup"><span data-stu-id="2eeb4-187">hello following example shows an object for *osSettings*:</span></span>

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a><span data-ttu-id="2eeb4-188">machineSettings</span><span class="sxs-lookup"><span data-stu-id="2eeb4-188">machineSettings</span></span>
<span data-ttu-id="2eeb4-189">A létrehozott változó *machineSettings* core változók a virtuális gép létrehozása vegyesen tartalmazó összetett objektum.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-189">A generated variable, *machineSettings* is a complex object containing a mix of core variables for creating a VM.</span></span> <span data-ttu-id="2eeb4-190">hello változók közé tartoznak a rendszergazdai felhasználónevet és jelszót, hello virtuális gép nevét egy előtagot és egy operációs rendszer lemezképének mutató hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-190">hello variables include administrator user name and password, a prefix for hello VM names, and an operating system image reference.</span></span>

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

<span data-ttu-id="2eeb4-191">Vegye figyelembe, hogy *osImageReference* lekéri hello hello értékeinek *osSettings* hello fő sablonban definiált változó.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-191">Note that *osImageReference* retrieves hello values from hello *osSettings* variable defined in hello main template.</span></span> <span data-ttu-id="2eeb4-192">Ez azt jelenti, hogy könnyen módosíthatja a virtuális gépek hello operációs rendszer – teljes egészében vagy sablon felhasználóinak hello beállítás alapján.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-192">That means you can easily change hello operating system for a VM—entirely or based on hello preference of a template consumer.</span></span>

#### <a name="vmscripts"></a><span data-ttu-id="2eeb4-193">vmScripts</span><span class="sxs-lookup"><span data-stu-id="2eeb4-193">vmScripts</span></span>
<span data-ttu-id="2eeb4-194">Hello *vmScripts* objektumra a hello parancsfájlok toodownload kapcsolatos részleteket tartalmaz, és egy Virtuálisgép-példány, beleértve a külső és belső hivatkozások hajt végre.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-194">hello *vmScripts* object contains details about hello scripts toodownload and execute on a VM instance, including outside and inside references.</span></span> <span data-ttu-id="2eeb4-195">Kívülről való hivatkozások hello infrastruktúra is.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-195">Outside references include hello infrastructure.</span></span>
<span data-ttu-id="2eeb4-196">Belső hivatkozások hello telepített szoftvereket és a konfigurációs tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-196">Inside references include hello installed software installed and configuration.</span></span>

<span data-ttu-id="2eeb4-197">Hello használata *scriptsToDownload* tulajdonság toolist hello parancsfájlok toodownload toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-197">You use hello *scriptsToDownload* property toolist hello scripts toodownload toohello VM.</span></span> <span data-ttu-id="2eeb4-198">Ez az objektum is tartalmaz hivatkozásokat toocommand-sor argumentumok különféle műveletek.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-198">This object also contains references toocommand-line arguments for different types of actions.</span></span> <span data-ttu-id="2eeb4-199">Ilyen műveletek közé tartoznak, hello alapértelmezett telepítés minden egyes csomóponton, egy telepítés, amely minden csomópont telepítése után fut és esetlegesen a sablonban megadott adott tooa további parancsfájlok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-199">These actions include executing hello default installation for each individual node, an installation that runs after all nodes are deployed, and any additional scripts that may be specific tooa given template.</span></span>

<span data-ttu-id="2eeb4-200">Ebben a példában a MongoDB, amely egy soron toodeliver magas rendelkezésre állást igényel használt sablon toodeploy származik.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-200">This example is from a template used toodeploy MongoDB, which requires an arbiter toodeliver high availability.</span></span> <span data-ttu-id="2eeb4-201">Hello *arbiterNodeInstallCommand* túl hozzá lett adva*vmScripts* tooinstall hello soron.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-201">hello *arbiterNodeInstallCommand* has been added too*vmScripts* tooinstall hello arbiter.</span></span>

<span data-ttu-id="2eeb4-202">hello változók szakaszban, ahol található hello változókat, amelyek meghatározzák a hello adott szöveg tooexecute hello parancsfájl hello megfelelő értékekkel.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-202">hello variables section is where you find hello variables that define hello specific text tooexecute hello script with hello proper values.</span></span>

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a><span data-ttu-id="2eeb4-203">Egy sablonból visszatérési állapota</span><span class="sxs-lookup"><span data-stu-id="2eeb4-203">Return state from a template</span></span>
<span data-ttu-id="2eeb4-204">Nem csak egy sablonba át adatokat, is megosztás hátsó toohello hívó sablon.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-204">Not only can you pass data into a template, you can also share data back toohello calling template.</span></span> <span data-ttu-id="2eeb4-205">A hello **kimenete** szakasz csatolt sablon, megadhatja a kulcs/érték párok, amelyeket a hello Forrássablon lehet használni.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-205">In hello **outputs** section of a linked template, you can provide key/value pairs that can be consumed by hello source template.</span></span>

<span data-ttu-id="2eeb4-206">hello következő példa bemutatja, hogyan toopass hello magánhálózati IP-cím egy csatolt sablon hozott létre.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-206">hello following example shows how toopass hello private IP address generated in a linked template.</span></span>

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

<span data-ttu-id="2eeb4-207">Hello fő sablont, belül használható hello szintaxisa a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="2eeb4-207">Within hello main template, you can use that data with hello following syntax:</span></span>

    "[reference('master-node').outputs.masterip.value]"

<span data-ttu-id="2eeb4-208">Ebben a kifejezésben hello kimenetek szakasz vagy hello források szakaszában hello fő sablont is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-208">You can use this expression in either hello outputs section or hello resources section of hello main template.</span></span> <span data-ttu-id="2eeb4-209">Hello kifejezés nem használható hello változók szakaszban, mert hello futásidejű állapot támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-209">You cannot use hello expression in hello variables section because it relies on hello runtime state.</span></span> <span data-ttu-id="2eeb4-210">tooreturn hello fő sablont, használja ezt az értéket:</span><span class="sxs-lookup"><span data-stu-id="2eeb4-210">tooreturn this value from hello main template, use:</span></span>

    "outputs": {
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }

<span data-ttu-id="2eeb4-211">Hello használatának példája adja kimenetként, egy virtuális géphez csatolt sablon tooreturn adatlemezek szakaszában, a következő témakörben: [hozzon létre egy virtuális gép több adatlemezek](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="2eeb4-211">For an example of using hello outputs section of a linked template tooreturn data disks for a virtual machine, see [Creating multiple data disks for a Virtual Machine](resource-group-create-multiple.md).</span></span>

## <a name="define-authentication-settings-for-virtual-machine"></a><span data-ttu-id="2eeb4-212">Virtuális gép hitelesítési beállításainak megadása</span><span class="sxs-lookup"><span data-stu-id="2eeb4-212">Define authentication settings for virtual machine</span></span>
<span data-ttu-id="2eeb4-213">Hello használható konfigurációs beállítások toospecify hello hitelesítési beállításait a virtuális gépek korábban bemutatott minta.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-213">You can use hello same pattern shown previously for configuration settings toospecify hello authentication settings for a virtual machine.</span></span> <span data-ttu-id="2eeb4-214">Hello típusú hitelesítés sikeres paramétere hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-214">You create a parameter for passing in hello type of authentication.</span></span>

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

<span data-ttu-id="2eeb4-215">Hozzáadhat hello különböző hitelesítési típust és egy változó toostore, mely a hello paraméter értékének hello alapján központi telepítéshez használt változókat.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-215">You add variables for hello different authentication types, and a variable toostore which type is used for this deployment based on hello value of hello parameter.</span></span>

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

<span data-ttu-id="2eeb4-216">Hello virtuális gép definiálásakor meg hello **osProfile** létrehozott toohello változó.</span><span class="sxs-lookup"><span data-stu-id="2eeb4-216">When defining hello virtual machine, you set hello **osProfile** toohello variable you created.</span></span>

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a><span data-ttu-id="2eeb4-217">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2eeb4-217">Next steps</span></span>
* <span data-ttu-id="2eeb4-218">hello sablon hello szakaszai toolearn lásd [Azure Resource Manager sablonok készítése](resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="2eeb4-218">toolearn about hello sections of hello template, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="2eeb4-219">a sablonon belül elérhető toosee hello funkciók lásd [Azure Resource Manager Sablonfüggvényei](resource-group-template-functions.md)</span><span class="sxs-lookup"><span data-stu-id="2eeb4-219">toosee hello functions that are available within a template, see [Azure Resource Manager Template Functions](resource-group-template-functions.md)</span></span>
