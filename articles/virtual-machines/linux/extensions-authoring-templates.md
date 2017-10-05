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
# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Linux Virtuálisgép-bővítmények az Azure Resource Manager sablonok készítése
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Az Azure parancssori felület futtassa a következő parancs:

      Azure VM extension list

Ez a parancs visszaadja a közzétevő neve, a bővítmény neve és a következő verzióra:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Ezeket a tulajdonságokat a "publisher", "type" és "typeHandlerVersion", illetve a fenti sablon kódrészletet a hozzárendelése.

> [!NOTE]
> A bővítmény legújabb verzióját használja a legnaprakészebb működtetéséhez mindig javasoljuk.
> 
> 

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>A séma kiterjesztése konfigurációs paraméterek azonosítása
A következő lépés egy bővítmény sablont készít az, hogy azonosítsa a konfigurációs paraméterek biztosítása formátumát. Mindegyik bővítményt a saját paraméterek támogat.

Nézze meg a minta konfigurációk Linux-bővítmények, kattintson a dokumentációját lásd: [Linux eExtensions minták](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Tekintse meg a következő egy teljes mértékben teljes sablont a Virtuálisgép-bővítmények.

[A Linux virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

A sablont készít, az Azure parancssori felület használatával telepítheti.

