---
title: "Linux virtuális gép kiterjesztésű aaaAuthoring sablonok |} Microsoft Docs"
description: "További tudnivalók a bővítményekkel Azure Resource Manager sablonok készítése a Linux virtuális gépekhez"
services: virtual-machines-linux
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 322f8f0b-6697-4acb-b5f3-b3f58d28358b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: b797e442d8706956bbc06c5be611a2b0119055d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a><span data-ttu-id="6b212-103">Linux Virtuálisgép-bővítmények az Azure Resource Manager sablonok készítése</span><span class="sxs-lookup"><span data-stu-id="6b212-103">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

<span data-ttu-id="6b212-104">Az Azure parancssori felület futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="6b212-104">From Azure CLI, run hello following commnad:</span></span>

      Azure VM extension list

<span data-ttu-id="6b212-105">A parancs beolvasása hello közzétevő neve, a bővítmény neve és a következő verzióra:</span><span class="sxs-lookup"><span data-stu-id="6b212-105">This command returns hello publisher name, extension name and version as following:</span></span>

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

<span data-ttu-id="6b212-106">A három Tulajdonságok leképezése túl "publisher", "type" és "typeHandlerVersion" rendre a fenti sablon részlet hello.</span><span class="sxs-lookup"><span data-stu-id="6b212-106">These three properties map too"publisher", "type", and "typeHandlerVersion" respectively in hello above template snippet.</span></span>

> [!NOTE]
> <span data-ttu-id="6b212-107">Mindig ajánlott toouse hello legújabb bővítmény verzió tooget hello legtöbb frissített funkcióit.</span><span class="sxs-lookup"><span data-stu-id="6b212-107">It's always recommended toouse hello latest extension version tooget hello most updated functionality.</span></span>
> 
> 

## <a name="identifying-hello-schema-for-hello-extension-configuration-parameters"></a><span data-ttu-id="6b212-108">Hello séma hello bővítmény konfigurációs paraméterek azonosítása</span><span class="sxs-lookup"><span data-stu-id="6b212-108">Identifying hello schema for hello extension configuration parameters</span></span>
<span data-ttu-id="6b212-109">hello következő lépés egy bővítmény sablont készít az formátuma tooidentify hello szükséges konfigurációs paramétereket.</span><span class="sxs-lookup"><span data-stu-id="6b212-109">hello next step with authoring an extension template is tooidentify hello format for providing configuration parameters.</span></span> <span data-ttu-id="6b212-110">Mindegyik bővítményt a saját paraméterek támogat.</span><span class="sxs-lookup"><span data-stu-id="6b212-110">Each extension supports its own set of parameters.</span></span>

<span data-ttu-id="6b212-111">a Linux-bővítményeket, minta konfigurációi toolook kattintson hello dokumentációját lásd: [Linux eExtensions minták](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6b212-111">toolook at sample configurations for Linux extensions, click hello documentation for see [Linux eExtensions samples](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="6b212-112">Tekintse meg a következő tooget egy teljes mértékben teljes sablont a Virtuálisgép-bővítmények toohello.</span><span class="sxs-lookup"><span data-stu-id="6b212-112">Please refer toohello following tooget a fully complete template with VM Extensions.</span></span>

[<span data-ttu-id="6b212-113">A Linux virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="6b212-113">Custom script extension on a Linux VM</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

<span data-ttu-id="6b212-114">Hello sablont készít, hello Azure parancssori felület használatával telepítheti.</span><span class="sxs-lookup"><span data-stu-id="6b212-114">After authoring hello template, you can deploy it using hello Azure CLI.</span></span>

