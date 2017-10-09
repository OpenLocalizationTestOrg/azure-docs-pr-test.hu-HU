## <a name="overview-of-azure-resource-manager-templates"></a><span data-ttu-id="550fe-101">Az Azure Resource Manager-sablonok áttekintése</span><span class="sxs-lookup"><span data-stu-id="550fe-101">Overview of Azure Resource Manager templates</span></span>
<span data-ttu-id="550fe-102">Az Azure Resource Manager-sablonok segítségével toodeclaratively Json nyelvi hello Azure infrastruktúra-szolgáltatási infrastruktúra megadása erőforrások közti függőségeket hello meghatározásával.</span><span class="sxs-lookup"><span data-stu-id="550fe-102">Azure Resource Manager templates allow you toodeclaratively specify hello Azure IaaS infrastructure in Json language by defining hello dependencies between resources.</span></span> <span data-ttu-id="550fe-103">A részletes áttekintés az Azure Resource Manager-sablonok tekintse meg az alábbi toohello cikk:</span><span class="sxs-lookup"><span data-stu-id="550fe-103">For a detailed overview of Azure Resource Manager Templates, please refer toohello article below:</span></span>

[<span data-ttu-id="550fe-104">Erőforráscsoport áttekintése</span><span class="sxs-lookup"><span data-stu-id="550fe-104">Resource Group Overview</span></span>](../articles/azure-resource-manager/resource-group-overview.md)

## <a name="sample-template-snippet-for-vm-extensions"></a><span data-ttu-id="550fe-105">Minta sablon részlet Virtuálisgép-bővítmények</span><span class="sxs-lookup"><span data-stu-id="550fe-105">Sample template snippet for VM extensions</span></span>
<span data-ttu-id="550fe-106">Virtuálisgép-bővítmények telepítése Azure Resource Manager-sablon része van szüksége, hogy toodeclaratively adja meg az hello bővítménykonfiguráció hello sablont.</span><span class="sxs-lookup"><span data-stu-id="550fe-106">Deploying VM extensions as part of an Azure Resource Manager template requires you toodeclaratively specify hello extension configuration in hello template.</span></span>
<span data-ttu-id="550fe-107">Itt formátuma hello hello bővítménykonfiguráció meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="550fe-107">Here is hello format for specifying hello extension configuration.</span></span>

      {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "MyExtension",
      "apiVersion": "2015-05-01-preview",
      "location": "[parameters('location')]",
      "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
      "properties":
      {
      "publisher": "Publisher Namespace",
      "type": "extension Name",
      "typeHandlerVersion": "extension version",
      "settings": {
      // Extension specific configuration goes in here.
      }
      }
      }

<span data-ttu-id="550fe-108">Ahogy látja, a fenti hello, hello bővítmény a sablonban két fő részből áll:</span><span class="sxs-lookup"><span data-stu-id="550fe-108">As you can see from hello above, hello extension template contains two main parts:</span></span>

1. <span data-ttu-id="550fe-109">Bővítmény nevét, kiadóját és verzió</span><span class="sxs-lookup"><span data-stu-id="550fe-109">Extension name, publisher and version</span></span>
2. <span data-ttu-id="550fe-110">Bővítmény konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="550fe-110">Extension Configuration.</span></span>

## <a name="identifying-hello-publisher-type-and-typehandlerversion-for-any-extension"></a><span data-ttu-id="550fe-111">Hello közzétevő, típusa és kiterjesztés typeHandlerVersion azonosítása</span><span class="sxs-lookup"><span data-stu-id="550fe-111">Identifying hello publisher, type, and typeHandlerVersion for any extension</span></span>
<span data-ttu-id="550fe-112">Az Azure Virtuálisgép-bővítmények a Microsoft által közzétett és megbízható 3. fél közzétevők és a egyes kiterjesztések egyedileg azonosítja a közzétevő, típusa és hello typeHandlerVersion.</span><span class="sxs-lookup"><span data-stu-id="550fe-112">Azure VM extensions are published by Microsoft and trusted 3rd party publishers and each extension is uniquely identified by its publisher,type and hello typeHandlerVersion.</span></span> <span data-ttu-id="550fe-113">A következőképpen lehet meghatározni:</span><span class="sxs-lookup"><span data-stu-id="550fe-113">These can be determined as following:</span></span>  

