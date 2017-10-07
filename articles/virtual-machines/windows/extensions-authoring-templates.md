---
title: "Windows virtuális gép kiterjesztésű sablonok aaaAuthoring |} Microsoft Docs"
description: "További tudnivalók a bővítményekkel Azure Resource Manager-sablonok készítése Windows virtuális gépek"
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 418dd1f7-ded8-45ab-9a5a-a59d245e2555
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: c5156cb3859f7ff86bebda942150d268e57d6486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a><span data-ttu-id="bbb69-103">Windows Virtuálisgép-bővítmények az Azure Resource Manager sablonok készítése</span><span class="sxs-lookup"><span data-stu-id="bbb69-103">Authoring Azure Resource Manager templates with Windows VM extensions</span></span>
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

<span data-ttu-id="bbb69-104">Azure PowerShell futtassa a következő Azure PowerShell-parancsmag hello:</span><span class="sxs-lookup"><span data-stu-id="bbb69-104">From Azure PowerShell, run hello following Azure PowerShell cmdlet:</span></span>

      Get-AzureVMAvailableExtension


<span data-ttu-id="bbb69-105">Ez a parancsmag adja vissza hello közzétevő neve, a bővítmény neve és verziója az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="bbb69-105">This cmdlet returns hello publisher name, extension name, and version as follows:</span></span>

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

<span data-ttu-id="bbb69-106">A három Tulajdonságok leképezése túl "publisher", "type" és "typeHandlerVersion" rendre a fenti sablon részlet hello.</span><span class="sxs-lookup"><span data-stu-id="bbb69-106">These three properties map too"publisher", "type", and "typeHandlerVersion" respectively in hello above template snippet.</span></span>

> [!NOTE]
> <span data-ttu-id="bbb69-107">Mindig ajánlott toouse hello legújabb bővítmény verzió tooget hello legtöbb frissített funkcióit.</span><span class="sxs-lookup"><span data-stu-id="bbb69-107">It's always recommended toouse hello latest extension version tooget hello most updated functionality.</span></span>
> 
> 

## <a name="identifying-hello-schema-for-hello-extension-configuration-parameters"></a><span data-ttu-id="bbb69-108">Hello séma hello bővítmény konfigurációs paraméterek azonosítása</span><span class="sxs-lookup"><span data-stu-id="bbb69-108">Identifying hello schema for hello extension configuration parameters</span></span>
<span data-ttu-id="bbb69-109">hello következő lépés egy bővítmény sablont készít az formátuma tooidentify hello szükséges konfigurációs paramétereket.</span><span class="sxs-lookup"><span data-stu-id="bbb69-109">hello next step with authoring an extension template is tooidentify hello format for providing configuration parameters.</span></span> <span data-ttu-id="bbb69-110">Mindegyik bővítményt a saját paraméterek támogat.</span><span class="sxs-lookup"><span data-stu-id="bbb69-110">Each extension supports its own set of parameters.</span></span>

<span data-ttu-id="bbb69-111">a minta konfigurációi Windows-bővítmények, toolook lásd: [Windows bővítmények minták](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bbb69-111">toolook at sample configurations for Windows extensions, see [Windows extensions samples](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="bbb69-112">Tekintse meg a következő tooget egy teljes mértékben teljes sablont a Virtuálisgép-bővítmények toohello.</span><span class="sxs-lookup"><span data-stu-id="bbb69-112">Please refer toohello following tooget a fully complete template with VM extensions.</span></span>

[<span data-ttu-id="bbb69-113">Virtuális gép Windows egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="bbb69-113">Custom Script Extension on a Windows VM</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

<span data-ttu-id="bbb69-114">Hello sablont készít, az Azure PowerShell használatával telepítheti.</span><span class="sxs-lookup"><span data-stu-id="bbb69-114">After authoring hello template, you can deploy it using Azure PowerShell.</span></span>

