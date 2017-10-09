---
title: "a Windows virtuális gépek az Azure-portálon hello FQDN aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy teljesen minősített tartománynév vagy a teljes tartománynév, az erőforrás-kezelő alapú virtuális gép hello Azure-portálon."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a2ae5887-76df-485e-ae19-0efd96df8600
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 67c817ec97073803e513bc41ebde67b75ced565e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-qualified-domain-name-in-hello-azure-portal-for-a-windows-vm"></a>A Windows virtuális gépek létrehozása egy teljesen minősített tartománynevét hello Azure-portálon

Amikor egy virtuális gép (VM) hoz létre a hello [Azure-portálon](https://portal.azure.com), egy nyilvános IP-erőforrás hello virtuális gép automatikusan létrejön. Az IP cím tooremotely hozzáférés hello virtuális gép használja. Bár a hello portál nem hoz létre egy [teljesen minősített tartománynevét](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), vagy teljes Tartománynevét, létrehozhat egy hello virtuális gép létrehozása után. Ez a cikk bemutatja a hello lépéseket toocreate egy DNS-nevét vagy teljes Tartománynevét.

## <a name="create-a-fqdn"></a>Hozzon létre egy teljesen minősített Tartományneve
Ez a cikk feltételezi, hogy már létrehozta a virtuális gépek. Ha szükséges, akkor [hozzon létre egy virtuális gép hello portálon](quick-create-portal.md) vagy [az Azure PowerShell](quick-create-powershell.md). Miután a virtuális gép megfelelően működik, és kövesse az alábbi lépéseket:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Csatlakoztathatja távolról toohello a DNS-neve, mint a távoli asztal protokoll (RDP) használó virtuális gépek.

## <a name="next-steps"></a>Következő lépések
Most, hogy a virtuális gép nyilvános IP-cím és DNS névvel rendelkezik, általános alkalmazás-keretrendszerbeli vagy szolgáltatásokat, például az IIS, SQL vagy SharePoint telepítheti.

Akkor is olvashat további tudnivalókat [erőforrás-kezelő használatával](../../azure-resource-manager/resource-group-overview.md) kapcsolatos tippek felépítése az Azure-környezetekhez.

