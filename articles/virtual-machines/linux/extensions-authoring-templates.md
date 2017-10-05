---
title: "Linux virtuális gép kiterjesztésű sablonok készítése |} Microsoft Docs"
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
ms.openlocfilehash: 8b017306474670bf8dde1440128e16ec35146f24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a><span data-ttu-id="0f392-103">Linux Virtuálisgép-bővítmények az Azure Resource Manager sablonok készítése</span><span class="sxs-lookup"><span data-stu-id="0f392-103">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

<span data-ttu-id="0f392-104">Az Azure parancssori felület futtassa a következő parancs:</span><span class="sxs-lookup"><span data-stu-id="0f392-104">From Azure CLI, run the following commnad:</span></span>

      Azure VM extension list

<span data-ttu-id="0f392-105">Ez a parancs visszaadja a közzétevő neve, a bővítmény neve és a következő verzióra:</span><span class="sxs-lookup"><span data-stu-id="0f392-105">This command returns the publisher name, extension name and version as following:</span></span>

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

<span data-ttu-id="0f392-106">Ezeket a tulajdonságokat a "publisher", "type" és "typeHandlerVersion", illetve a fenti sablon kódrészletet a hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="0f392-106">These three properties map to "publisher", "type", and "typeHandlerVersion" respectively in the above template snippet.</span></span>

> [!NOTE]
> <span data-ttu-id="0f392-107">A bővítmény legújabb verzióját használja a legnaprakészebb működtetéséhez mindig javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="0f392-107">It's always recommended to use the latest extension version to get the most updated functionality.</span></span>
> 
> 

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a><span data-ttu-id="0f392-108">A séma kiterjesztése konfigurációs paraméterek azonosítása</span><span class="sxs-lookup"><span data-stu-id="0f392-108">Identifying the schema for the extension configuration parameters</span></span>
<span data-ttu-id="0f392-109">A következő lépés egy bővítmény sablont készít az, hogy azonosítsa a konfigurációs paraméterek biztosítása formátumát.</span><span class="sxs-lookup"><span data-stu-id="0f392-109">The next step with authoring an extension template is to identify the format for providing configuration parameters.</span></span> <span data-ttu-id="0f392-110">Mindegyik bővítményt a saját paraméterek támogat.</span><span class="sxs-lookup"><span data-stu-id="0f392-110">Each extension supports its own set of parameters.</span></span>

<span data-ttu-id="0f392-111">Nézze meg a minta konfigurációk Linux-bővítmények, kattintson a dokumentációját lásd: [Linux eExtensions minták](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0f392-111">To look at sample configurations for Linux extensions, click the documentation for see [Linux eExtensions samples](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="0f392-112">Tekintse meg a következő egy teljes mértékben teljes sablont a Virtuálisgép-bővítmények.</span><span class="sxs-lookup"><span data-stu-id="0f392-112">Please refer to the following to get a fully complete template with VM Extensions.</span></span>

[<span data-ttu-id="0f392-113">A Linux virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="0f392-113">Custom script extension on a Linux VM</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

<span data-ttu-id="0f392-114">A sablont készít, az Azure parancssori felület használatával telepítheti.</span><span class="sxs-lookup"><span data-stu-id="0f392-114">After authoring the template, you can deploy it using the Azure CLI.</span></span>

