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
# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Linux Virtuálisgép-bővítmények az Azure Resource Manager sablonok készítése
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Az Azure parancssori felület futtassa a következő parancs hello:

      Azure VM extension list

A parancs beolvasása hello közzétevő neve, a bővítmény neve és a következő verzióra:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

A három Tulajdonságok leképezése túl "publisher", "type" és "typeHandlerVersion" rendre a fenti sablon részlet hello.

> [!NOTE]
> Mindig ajánlott toouse hello legújabb bővítmény verzió tooget hello legtöbb frissített funkcióit.
> 
> 

## <a name="identifying-hello-schema-for-hello-extension-configuration-parameters"></a>Hello séma hello bővítmény konfigurációs paraméterek azonosítása
hello következő lépés egy bővítmény sablont készít az formátuma tooidentify hello szükséges konfigurációs paramétereket. Mindegyik bővítményt a saját paraméterek támogat.

a Linux-bővítményeket, minta konfigurációi toolook kattintson hello dokumentációját lásd: [Linux eExtensions minták](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Tekintse meg a következő tooget egy teljes mértékben teljes sablont a Virtuálisgép-bővítmények toohello.

[A Linux virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Hello sablont készít, hello Azure parancssori felület használatával telepítheti.

