---
title: "Azure-sablonok között összetett értéket átadni |} Microsoft Docs"
description: "Azt mutatja be megközelítés használatos összetett objektumok állapotadatok megosztása Azure Resource Manager-sablonok és a kapcsolt sablonok használata ajánlott."
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
ms.openlocfilehash: 23cc4321159a87b61c177b11381646af8bd9eb35
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="share-state-to-and-from-azure-resource-manager-templates"></a><span data-ttu-id="ba715-103">Fájlmegosztási állapot és az Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="ba715-103">Share state to and from Azure Resource Manager templates</span></span>
<span data-ttu-id="ba715-104">Ez a témakör gyakorlati tanácsok a kezelése és megosztása a sablonokon belüli állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ba715-104">This topic shows best practices for managing and sharing state within templates.</span></span> <span data-ttu-id="ba715-105">A paraméterek és változók ebben a témakörben szereplő példák az objektumtípus adhat meg a telepítési követelményeket kényelmesen rendszerezéséhez.</span><span class="sxs-lookup"><span data-stu-id="ba715-105">The parameters and variables shown in this topic are examples of the type of objects you can define to conveniently organize your deployment requirements.</span></span> <span data-ttu-id="ba715-106">Ezekben a példákban a saját objektumok, amelyek a környezetben jelentéssel bírnak valósíthat meg.</span><span class="sxs-lookup"><span data-stu-id="ba715-106">From these examples, you can implement your own objects with property values that make sense for your environment.</span></span>

<span data-ttu-id="ba715-107">Ez a témakör egy nagyobb tanulmány részét képezi.</span><span class="sxs-lookup"><span data-stu-id="ba715-107">This topic is part of a larger whitepaper.</span></span> <span data-ttu-id="ba715-108">Olvassa el a teljes dokumentum, le kell töltenie [globális osztály Resource Manager sablonok szempontok és eljárások bizonyítása](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).</span><span class="sxs-lookup"><span data-stu-id="ba715-108">To read the full paper, download [World Class Resource Manager Templates Considerations and Proven Practices](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).</span></span>

## <a name="provide-standard-configuration-settings"></a><span data-ttu-id="ba715-109">Adja meg a szabványos konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="ba715-109">Provide standard configuration settings</span></span>
<span data-ttu-id="ba715-110">Helyett a sablont, amely a teljes és számos változata kínál, a közös minta arra, hogy a kijelölt ismert konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="ba715-110">Rather than offer a template that provides total flexibility and countless variations, a common pattern is to provide a selection of known configurations.</span></span> <span data-ttu-id="ba715-111">Érvényben a felhasználók kiválaszthatják például védőfal kis, közepes és nagy szabványos pólóra méretét.</span><span class="sxs-lookup"><span data-stu-id="ba715-111">In effect, users can select standard t-shirt sizes such as sandbox, small, medium, and large.</span></span> <span data-ttu-id="ba715-112">Más pólóra méretű például a termék ajánlatokat, community edition vagy enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="ba715-112">Other examples of t-shirt sizes are product offerings, such as community edition or enterprise edition.</span></span> <span data-ttu-id="ba715-113">Más esetekben lehet munkaterhelés-specifikus konfigurációk egy technológia – például a térkép csökkentése vagy a nem sql.</span><span class="sxs-lookup"><span data-stu-id="ba715-113">In other cases, it may be workload-specific configurations of a technology – such as map reduce or no sql.</span></span>

<span data-ttu-id="ba715-114">Összetett objektumok, amelyek tartalmazzák a gyűjtött adatokat, más néven "tulajdonságcsomagok" változók létrehozása, és az adatokat az erőforrás deklarációja alapjául a sablonban.</span><span class="sxs-lookup"><span data-stu-id="ba715-114">With complex objects, you can create variables that contain collections of data, sometimes known as "property bags" and use that data to drive the resource declaration in your template.</span></span> <span data-ttu-id="ba715-115">Ezt a módszert biztosít az ügyfelek különböző méretű előre konfigurált megfelelő, ismert konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="ba715-115">This approach provides good, known configurations of varying sizes that are preconfigured for customers.</span></span> <span data-ttu-id="ba715-116">Ismert konfigurációk nélkül felhasználók sablon kell önállóan méretezése fürtön határozza meg, figyelembe a platform erőforrás-korlátozások és számításokat végezni az eredményül kapott particionálás storage-fiókok és egyéb erőforrások (mert a fürt méretét és az erőforrás azonosításához megkötések).</span><span class="sxs-lookup"><span data-stu-id="ba715-116">Without known configurations, users of the template must determine cluster sizing on their own, factor in platform resource constraints, and do math to identify the resulting partitioning of storage accounts and other resources (due to cluster size and resource constraints).</span></span> <span data-ttu-id="ba715-117">Azonkívül, hogy hatékonyabb működését az ügyfél, néhány ismert konfigurációk könnyebben is támogatja, és segít a következők támogatásával sűrűség magasabb szintű.</span><span class="sxs-lookup"><span data-stu-id="ba715-117">In addition to making a better experience for the customer, a few known configurations are easier to support and can help you deliver a higher level of density.</span></span>

<span data-ttu-id="ba715-118">A következő példa bemutatja, hogyan adja meg a gyűjtött adatokat a lapblobok az összetett objektumokat tartalmazó változókat.</span><span class="sxs-lookup"><span data-stu-id="ba715-118">The following example shows how to define variables that contain complex objects for representing collections of data.</span></span> <span data-ttu-id="ba715-119">A gyűjtemények megadása a virtuális gép méretét, a hálózati beállításokat, az operációs rendszeri beállítások és a rendelkezésre állási beállítások használt értékek.</span><span class="sxs-lookup"><span data-stu-id="ba715-119">The collections define values that are used for virtual machine size, network settings, operating system settings and availability settings.</span></span>

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

<span data-ttu-id="ba715-120">Figyelje meg, hogy a **tshirtSize** változó összefűzi egy paraméteren keresztül megadott pólóra méretét (**kis**, **Közepes**, **nagy** ) a szöveg **tshirtSize**.</span><span class="sxs-lookup"><span data-stu-id="ba715-120">Notice that the **tshirtSize** variable concatenates the t-shirt size you provided through a parameter (**Small**, **Medium**, **Large**) to the text **tshirtSize**.</span></span> <span data-ttu-id="ba715-121">Ez a változó segítségével pólóra méret társított összetett objektumot változója beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ba715-121">You use this variable to retrieve the associated complex object variable for that t-shirt size.</span></span>

<span data-ttu-id="ba715-122">Ezek a változók a sablonban később majd hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="ba715-122">You can then reference these variables later in the template.</span></span> <span data-ttu-id="ba715-123">Nevű változókat és a Tulajdonságok lehetőséget egyszerűbbé teszi a sablonok szintaxisát, és megkönnyíti, hogy tudni, hogy a környezetben.</span><span class="sxs-lookup"><span data-stu-id="ba715-123">The ability to reference named-variables and their properties simplifies the template syntax, and makes it easy to understand context.</span></span> <span data-ttu-id="ba715-124">A következő példa egy erőforrás értékeinek beállításához korábban megjelenített objektumok használatával történő telepítéséről határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ba715-124">The following example defines a resource to deploy by using the objects shown previously to set values.</span></span> <span data-ttu-id="ba715-125">A Virtuálisgép-méretet lekérésével értékét állítsa például `variables('tshirtSize').vmSize` során az értéket, a lemez méretét veszi át a `variables('tshirtSize').diskSize`.</span><span class="sxs-lookup"><span data-stu-id="ba715-125">For example, the VM size is set by retrieving the value for `variables('tshirtSize').vmSize` while the value for the disk size is retrieved from `variables('tshirtSize').diskSize`.</span></span> <span data-ttu-id="ba715-126">Ezenkívül csatolt sablon URI-JÁNAK értéke beállított `variables('tshirtSize').vmTemplate`.</span><span class="sxs-lookup"><span data-stu-id="ba715-126">In addition, the URI for a linked template is set with the value for `variables('tshirtSize').vmTemplate`.</span></span>

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

## <a name="pass-state-to-a-template"></a><span data-ttu-id="ba715-127">A sablonok hozzáférési állapota</span><span class="sxs-lookup"><span data-stu-id="ba715-127">Pass state to a template</span></span>
<span data-ttu-id="ba715-128">Megosztott állapot paramétereknek, hogy közvetlenül a telepítés során a sablonba.</span><span class="sxs-lookup"><span data-stu-id="ba715-128">You share state into a template through parameters that you provide directly during deployment.</span></span>

<span data-ttu-id="ba715-129">A következő táblázat a sablonok a gyakran használt paraméterek.</span><span class="sxs-lookup"><span data-stu-id="ba715-129">The following table lists commonly used parameters in templates.</span></span>

| <span data-ttu-id="ba715-130">Név</span><span class="sxs-lookup"><span data-stu-id="ba715-130">Name</span></span> | <span data-ttu-id="ba715-131">Érték</span><span class="sxs-lookup"><span data-stu-id="ba715-131">Value</span></span> | <span data-ttu-id="ba715-132">Leírás</span><span class="sxs-lookup"><span data-stu-id="ba715-132">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ba715-133">location</span><span class="sxs-lookup"><span data-stu-id="ba715-133">location</span></span> |<span data-ttu-id="ba715-134">Az Azure-régiók korlátozott listájából karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ba715-134">String from a constrained list of Azure regions</span></span> |<span data-ttu-id="ba715-135">A hely, ahol az erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="ba715-135">The location where the resources are deployed.</span></span> |
| <span data-ttu-id="ba715-136">storageAccountNamePrefix</span><span class="sxs-lookup"><span data-stu-id="ba715-136">storageAccountNamePrefix</span></span> |<span data-ttu-id="ba715-137">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ba715-137">String</span></span> |<span data-ttu-id="ba715-138">A Tárfiók, ahol a virtuális gép lemezeinek kerülnek egyedi DNS-neve</span><span class="sxs-lookup"><span data-stu-id="ba715-138">Unique DNS name for the Storage Account where the VM's disks are placed</span></span> |
| <span data-ttu-id="ba715-139">Tartománynév</span><span class="sxs-lookup"><span data-stu-id="ba715-139">domainName</span></span> |<span data-ttu-id="ba715-140">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ba715-140">String</span></span> |<span data-ttu-id="ba715-141">A nyilvánosan elérhető jumpbox VM formátumú tartománynevet: **{tartománynév}. { hely}.cloudapp.com** például: **mydomainname.westus.cloudapp.azure.com**</span><span class="sxs-lookup"><span data-stu-id="ba715-141">Domain name of the publicly accessible jumpbox VM in the format: **{domainName}.{location}.cloudapp.com** For example: **mydomainname.westus.cloudapp.azure.com**</span></span> |
| <span data-ttu-id="ba715-142">adminUsername</span><span class="sxs-lookup"><span data-stu-id="ba715-142">adminUsername</span></span> |<span data-ttu-id="ba715-143">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ba715-143">String</span></span> |<span data-ttu-id="ba715-144">A virtuális gépek felhasználónév</span><span class="sxs-lookup"><span data-stu-id="ba715-144">Username for the VMs</span></span> |
| <span data-ttu-id="ba715-145">adminPassword</span><span class="sxs-lookup"><span data-stu-id="ba715-145">adminPassword</span></span> |<span data-ttu-id="ba715-146">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ba715-146">String</span></span> |<span data-ttu-id="ba715-147">A virtuális gépek jelszava</span><span class="sxs-lookup"><span data-stu-id="ba715-147">Password for the VMs</span></span> |
| <span data-ttu-id="ba715-148">tshirtSize</span><span class="sxs-lookup"><span data-stu-id="ba715-148">tshirtSize</span></span> |<span data-ttu-id="ba715-149">A korlátozott listájából karakterlánc kínált pólóra méretek</span><span class="sxs-lookup"><span data-stu-id="ba715-149">String from a constrained list of offered t-shirt sizes</span></span> |<span data-ttu-id="ba715-150">A nevesített skálázási egység méretét kiépítését.</span><span class="sxs-lookup"><span data-stu-id="ba715-150">The named scale unit size to provision.</span></span> <span data-ttu-id="ba715-151">Például "Kicsi", "közepes", "Nagy"</span><span class="sxs-lookup"><span data-stu-id="ba715-151">For example, "Small", "Medium", "Large"</span></span> |
| <span data-ttu-id="ba715-152">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="ba715-152">virtualNetworkName</span></span> |<span data-ttu-id="ba715-153">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ba715-153">String</span></span> |<span data-ttu-id="ba715-154">A fogyasztó kívánja használni a virtuális hálózat neve.</span><span class="sxs-lookup"><span data-stu-id="ba715-154">Name of the virtual network that the consumer wants to use.</span></span> |
| <span data-ttu-id="ba715-155">enableJumpbox</span><span class="sxs-lookup"><span data-stu-id="ba715-155">enableJumpbox</span></span> |<span data-ttu-id="ba715-156">Korlátozott listájából (engedélyezett vagy letiltott) karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ba715-156">String from a constrained list (enabled/disabled)</span></span> |<span data-ttu-id="ba715-157">A paraméter, amely azonosítja, hogy a környezet egy jumpbox engedélyezi-e.</span><span class="sxs-lookup"><span data-stu-id="ba715-157">Parameter that identifies whether to enable a jumpbox for the environment.</span></span> <span data-ttu-id="ba715-158">Értékek: "engedélyezve", "Letiltva"</span><span class="sxs-lookup"><span data-stu-id="ba715-158">Values: "enabled", "disabled"</span></span> |

<span data-ttu-id="ba715-159">A **tshirtSize** az előző szakaszban használt paraméter típusúként van definiálva:</span><span class="sxs-lookup"><span data-stu-id="ba715-159">The **tshirtSize** parameter used in the previous section is defined as:</span></span>

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
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a><span data-ttu-id="ba715-160">Állapot átadása kapcsolt sablonok</span><span class="sxs-lookup"><span data-stu-id="ba715-160">Pass state to linked templates</span></span>
<span data-ttu-id="ba715-161">Csatolt sablonok való csatlakozáskor gyakran statikus vegyesen használ, és létrehozott változókat.</span><span class="sxs-lookup"><span data-stu-id="ba715-161">When connecting to linked templates, you often use a mix of static and generated variables.</span></span>

### <a name="static-variables"></a><span data-ttu-id="ba715-162">Statikus változó</span><span class="sxs-lookup"><span data-stu-id="ba715-162">Static variables</span></span>
<span data-ttu-id="ba715-163">Statikus változó gyakran használják arra, hogy alapértékek, például a használt URL-, egy sablon egész.</span><span class="sxs-lookup"><span data-stu-id="ba715-163">Static variables are often used to provide base values, such as URLs, that are used throughout a template.</span></span>

<span data-ttu-id="ba715-164">A következő sablon cikkből a `templateBaseUrl` a sablon legfelső szintű helyét adja meg a Githubon.</span><span class="sxs-lookup"><span data-stu-id="ba715-164">In the following template excerpt, `templateBaseUrl` specifies the root location for the template in GitHub.</span></span> <span data-ttu-id="ba715-165">A következő sorban összeállít egy új változót `sharedTemplateUrl` , amely összefűzi a megosztott erőforrások sablon ismert nevű alap URL-címet.</span><span class="sxs-lookup"><span data-stu-id="ba715-165">The next line builds a new variable `sharedTemplateUrl` that concatenates the base URL with the known name of the shared resources template.</span></span> <span data-ttu-id="ba715-166">A sor alatt egy összetett objektum változó fogja tárolni Póló mérete, ha az alap URL-címet a ismert konfiguráció sablon helyére összefűzendő és tárolja a `vmTemplate` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ba715-166">Below that line, a complex object variable is used to store a t-shirt size, where the base URL is concatenated to the known configuration template location and stored in the `vmTemplate` property.</span></span>

<span data-ttu-id="ba715-167">Ezt a módszert használja az az előnye, hogy a sablon helye megváltozik, ha csak módosítani szeretné az egyik helyen, amely továbbítja azokat a kapcsolt sablonok teljes statikus változó.</span><span class="sxs-lookup"><span data-stu-id="ba715-167">The benefit of this approach is that if the template location changes, you only need to change the static variable in one place, which passes it throughout the linked templates.</span></span>

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

### <a name="generated-variables"></a><span data-ttu-id="ba715-168">Létrehozott változók</span><span class="sxs-lookup"><span data-stu-id="ba715-168">Generated variables</span></span>
<span data-ttu-id="ba715-169">Statikus változó mellett a több változók generálása dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="ba715-169">In addition to static variables, several variables are generated dynamically.</span></span> <span data-ttu-id="ba715-170">Ez a szakasz azonosítja az egyes létrehozott változók a gyakori hibatípusokat.</span><span class="sxs-lookup"><span data-stu-id="ba715-170">This section identifies some of the common types of generated variables.</span></span>

#### <a name="tshirtsize"></a><span data-ttu-id="ba715-171">tshirtSize</span><span class="sxs-lookup"><span data-stu-id="ba715-171">tshirtSize</span></span>
<span data-ttu-id="ba715-172">A fenti példákban a létrehozott változó tisztában.</span><span class="sxs-lookup"><span data-stu-id="ba715-172">You are familiar with this generated variable from the examples above.</span></span>

#### <a name="networksettings"></a><span data-ttu-id="ba715-173">networkSettings</span><span class="sxs-lookup"><span data-stu-id="ba715-173">networkSettings</span></span>
<span data-ttu-id="ba715-174">Kapacitás, funkció, vagy végpont hatókörön belüli megoldássablonban a kapcsolt sablonok általában létre meglévő erőforrásokat a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="ba715-174">In a capacity, capability, or end-to-end scoped solution template, the linked templates typically create resources that exist on a network.</span></span> <span data-ttu-id="ba715-175">Egy egyszerű megközelítése, hogy egy összetett objektumot használja a hálózati beállítások tárolásához adja meg azokat a kapcsolt sablonok.</span><span class="sxs-lookup"><span data-stu-id="ba715-175">One straightforward approach is to use a complex object to store network settings and pass them to linked templates.</span></span>

<span data-ttu-id="ba715-176">Hálózati beállítások kommunikáció például alább látható.</span><span class="sxs-lookup"><span data-stu-id="ba715-176">An example of communicating network settings can be seen below.</span></span>

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

#### <a name="availabilitysettings"></a><span data-ttu-id="ba715-177">availabilitySettings</span><span class="sxs-lookup"><span data-stu-id="ba715-177">availabilitySettings</span></span>
<span data-ttu-id="ba715-178">A csatolt sablonok létrejött erőforrásokat gyakran kerülnek egy rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="ba715-178">Resources created in linked templates are often placed in an availability set.</span></span> <span data-ttu-id="ba715-179">A következő példában a rendelkezésre állási készlet neve meg van adva, de is a tartalék tartomány és a frissítési tartományok száma használatára.</span><span class="sxs-lookup"><span data-stu-id="ba715-179">In the following example, the availability set name is specified and also the fault domain and update domain count to use.</span></span>

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

<span data-ttu-id="ba715-180">Ha több rendelkezésre állási csoportok (például egy fő csomópontok) és egy másik adatok csomópontok előtagjaként is használhatja a nevét, adjon meg több rendelkezésre állási csoportok, vagy a modell létrehozásához egy adott pólóra méretű változót korábban bemutatott kövesse.</span><span class="sxs-lookup"><span data-stu-id="ba715-180">If you need multiple availability sets (for example, one for master nodes and another for data nodes), you can use a name as a prefix, specify multiple availability sets, or follow the model shown earlier for creating a variable for a specific t-shirt size.</span></span>

#### <a name="storagesettings"></a><span data-ttu-id="ba715-181">storageSettings</span><span class="sxs-lookup"><span data-stu-id="ba715-181">storageSettings</span></span>
<span data-ttu-id="ba715-182">Csatolt sablonok gyakran megosztott tárolási részleteit.</span><span class="sxs-lookup"><span data-stu-id="ba715-182">Storage details are often shared with linked templates.</span></span> <span data-ttu-id="ba715-183">Az alábbi példában egy *storageSettings* objektum tartalmazza a tárolási fiók és a tároló neve adatait.</span><span class="sxs-lookup"><span data-stu-id="ba715-183">In the example below, a *storageSettings* object provides details about the storage account and container names.</span></span>

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a><span data-ttu-id="ba715-184">osSettings</span><span class="sxs-lookup"><span data-stu-id="ba715-184">osSettings</span></span>
<span data-ttu-id="ba715-185">Csatolt sablonok szükség lehet operációs rendszer beállításait átadása különféle csomópontok különböző ismert konfigurációs típusok között.</span><span class="sxs-lookup"><span data-stu-id="ba715-185">With linked templates, you may need to pass operating system settings to various nodes types across different known configuration types.</span></span> <span data-ttu-id="ba715-186">Egy összetett objektum tárolására és megosztására operációsrendszer-információkat egyszerűen és is megkönnyíti több operációs rendszer választást is támogatnak a központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="ba715-186">A complex object is an easy way to store and share operating system information and also makes it easier to support multiple operating system choices for deployment.</span></span>

<span data-ttu-id="ba715-187">A következő példa bemutatja egy objektumot *osSettings*:</span><span class="sxs-lookup"><span data-stu-id="ba715-187">The following example shows an object for *osSettings*:</span></span>

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a><span data-ttu-id="ba715-188">machineSettings</span><span class="sxs-lookup"><span data-stu-id="ba715-188">machineSettings</span></span>
<span data-ttu-id="ba715-189">A létrehozott változó *machineSettings* core változók a virtuális gép létrehozása vegyesen tartalmazó összetett objektum.</span><span class="sxs-lookup"><span data-stu-id="ba715-189">A generated variable, *machineSettings* is a complex object containing a mix of core variables for creating a VM.</span></span> <span data-ttu-id="ba715-190">A változók közé tartoznak a rendszergazdai felhasználónevet és jelszót, egy virtuális gép nevét, és az operációs rendszer Képhivatkozás előtagot.</span><span class="sxs-lookup"><span data-stu-id="ba715-190">The variables include administrator user name and password, a prefix for the VM names, and an operating system image reference.</span></span>

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

<span data-ttu-id="ba715-191">Vegye figyelembe, hogy *osImageReference* értékeinek lekéri a *osSettings* a fő sablonban definiált változó.</span><span class="sxs-lookup"><span data-stu-id="ba715-191">Note that *osImageReference* retrieves the values from the *osSettings* variable defined in the main template.</span></span> <span data-ttu-id="ba715-192">Ez azt jelenti, hogy a virtuális gépek könnyen módosíthatja az operációs rendszer – teljes egészében vagy egy sablon fogyasztó beállítás alapján.</span><span class="sxs-lookup"><span data-stu-id="ba715-192">That means you can easily change the operating system for a VM—entirely or based on the preference of a template consumer.</span></span>

#### <a name="vmscripts"></a><span data-ttu-id="ba715-193">vmScripts</span><span class="sxs-lookup"><span data-stu-id="ba715-193">vmScripts</span></span>
<span data-ttu-id="ba715-194">A *vmScripts* objektum részletesen ismerteti a parancsfájlok letöltésére és végrehajtására a Virtuálisgép-példány, beleértve a külső és belső hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="ba715-194">The *vmScripts* object contains details about the scripts to download and execute on a VM instance, including outside and inside references.</span></span> <span data-ttu-id="ba715-195">Kívül hivatkozásokat tartalmaznak az infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="ba715-195">Outside references include the infrastructure.</span></span>
<span data-ttu-id="ba715-196">Belső hivatkozásokat tartalmaznak, a telepített szoftvereket és a konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="ba715-196">Inside references include the installed software installed and configuration.</span></span>

<span data-ttu-id="ba715-197">Használja a *scriptsToDownload* tulajdonság a parancsfájlok töltse le a virtuális gép listázásához.</span><span class="sxs-lookup"><span data-stu-id="ba715-197">You use the *scriptsToDownload* property to list the scripts to download to the VM.</span></span> <span data-ttu-id="ba715-198">Ez az objektum parancssori argumentumokat használni a különböző típusú műveleteket mutató hivatkozásokat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ba715-198">This object also contains references to command-line arguments for different types of actions.</span></span> <span data-ttu-id="ba715-199">Ilyen műveletek közé tartoznak az alapértelmezett telepítés minden egyes csomóponton, egy telepítés, amelyen az összes csomópont telepítése után és lehet, hogy a megadott sablonkonfigurációs vonatkozó további parancsfájlok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="ba715-199">These actions include executing the default installation for each individual node, an installation that runs after all nodes are deployed, and any additional scripts that may be specific to a given template.</span></span>

<span data-ttu-id="ba715-200">Ebben a példában a MongoDB, amelyhez szükséges egy soron képes biztosítani a magas rendelkezésre állású telepítésére használt sablon származik.</span><span class="sxs-lookup"><span data-stu-id="ba715-200">This example is from a template used to deploy MongoDB, which requires an arbiter to deliver high availability.</span></span> <span data-ttu-id="ba715-201">A *arbiterNodeInstallCommand* hozzá lett adva *vmScripts* a soron telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ba715-201">The *arbiterNodeInstallCommand* has been added to *vmScripts* to install the arbiter.</span></span>

<span data-ttu-id="ba715-202">A változók szakaszban, ahol megtalálja a változókat, amelyek meghatározzák az adott szöveget, futtassa a parancsfájlt a megfelelő értékekkel.</span><span class="sxs-lookup"><span data-stu-id="ba715-202">The variables section is where you find the variables that define the specific text to execute the script with the proper values.</span></span>

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a><span data-ttu-id="ba715-203">Egy sablonból visszatérési állapota</span><span class="sxs-lookup"><span data-stu-id="ba715-203">Return state from a template</span></span>
<span data-ttu-id="ba715-204">Nem csak is át adatokat sablonba, megoszthatja a hívó sablon vissza az adatokat.</span><span class="sxs-lookup"><span data-stu-id="ba715-204">Not only can you pass data into a template, you can also share data back to the calling template.</span></span> <span data-ttu-id="ba715-205">Az a **kimenete** szakasz csatolt sablon, megadhatja a kulcs/érték párok, amelyeket a Forrássablon lehet használni.</span><span class="sxs-lookup"><span data-stu-id="ba715-205">In the **outputs** section of a linked template, you can provide key/value pairs that can be consumed by the source template.</span></span>

<span data-ttu-id="ba715-206">A következő példa bemutatja, hogyan felelt meg a magánhálózati IP-cím, egy csatolt sablon hozott létre.</span><span class="sxs-lookup"><span data-stu-id="ba715-206">The following example shows how to pass the private IP address generated in a linked template.</span></span>

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

<span data-ttu-id="ba715-207">A fő sablonon belül az adatokat a következő szintaxissal:</span><span class="sxs-lookup"><span data-stu-id="ba715-207">Within the main template, you can use that data with the following syntax:</span></span>

    "[reference('master-node').outputs.masterip.value]"

<span data-ttu-id="ba715-208">A kifejezés a kimenetek szakasz vagy az erőforrások terület a fő sablon használható.</span><span class="sxs-lookup"><span data-stu-id="ba715-208">You can use this expression in either the outputs section or the resources section of the main template.</span></span> <span data-ttu-id="ba715-209">A kifejezés nem használható a változók szakaszban, mert a futásidejű állapot támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="ba715-209">You cannot use the expression in the variables section because it relies on the runtime state.</span></span> <span data-ttu-id="ba715-210">Ez az érték a fő sablonból visszaállításához használja:</span><span class="sxs-lookup"><span data-stu-id="ba715-210">To return this value from the main template, use:</span></span>

    "outputs": {
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }

<span data-ttu-id="ba715-211">A csatolt sablon kimenetek szakaszának segítségével a virtuális gép adatlemezek vissza egy példát, lásd: [hozzon létre egy virtuális gép több adatlemezek](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="ba715-211">For an example of using the outputs section of a linked template to return data disks for a virtual machine, see [Creating multiple data disks for a Virtual Machine](resource-group-create-multiple.md).</span></span>

## <a name="define-authentication-settings-for-virtual-machine"></a><span data-ttu-id="ba715-212">Virtuális gép hitelesítési beállításainak megadása</span><span class="sxs-lookup"><span data-stu-id="ba715-212">Define authentication settings for virtual machine</span></span>
<span data-ttu-id="ba715-213">A konfigurációs beállításokat a korábban bemutatott minta segítségével adja meg a hitelesítési beállításokat a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="ba715-213">You can use the same pattern shown previously for configuration settings to specify the authentication settings for a virtual machine.</span></span> <span data-ttu-id="ba715-214">Egy sikeres paramétert hoz létre a hitelesítés típusát.</span><span class="sxs-lookup"><span data-stu-id="ba715-214">You create a parameter for passing in the type of authentication.</span></span>

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

<span data-ttu-id="ba715-215">A különböző hitelesítési típusok változók hozzáadta, és ez a paraméter értéke alapján központi telepítéshez használt egy változó tárolásához típusát.</span><span class="sxs-lookup"><span data-stu-id="ba715-215">You add variables for the different authentication types, and a variable to store which type is used for this deployment based on the value of the parameter.</span></span>

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

<span data-ttu-id="ba715-216">A virtuális gép meghatározásakor beállíthatja a **osProfile** létrehozott változóhoz.</span><span class="sxs-lookup"><span data-stu-id="ba715-216">When defining the virtual machine, you set the **osProfile** to the variable you created.</span></span>

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a><span data-ttu-id="ba715-217">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ba715-217">Next steps</span></span>
* <span data-ttu-id="ba715-218">A sablon szakaszai további tudnivalókért lásd: [Azure Resource Manager sablonok készítése](resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="ba715-218">To learn about the sections of the template, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="ba715-219">A sablonon belül elérhető funkciókat, olvassa el [Azure Resource Manager Sablonfüggvényei](resource-group-template-functions.md)</span><span class="sxs-lookup"><span data-stu-id="ba715-219">To see the functions that are available within a template, see [Azure Resource Manager Template Functions](resource-group-template-functions.md)</span></span>
