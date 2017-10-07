---
title: "hello Azure-portálon a Linux virtuális gép teljes Tartományneve aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy teljesen minősített tartománynév vagy a teljes tartománynév, az erőforrás-kezelő alapú virtuális gép hello Azure-portálon."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2cd6c249-a737-4a0a-b5ba-e1c09e551b30
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1494a0cb1caa62069c72096a739aee111ac8b383
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-qualified-domain-name-in-hello-azure-portal-for-a-linux-vm"></a>Hozzon létre egy teljesen minősített tartománynevét hello Azure-portálon a Linux virtuális gép

Amikor egy virtuális gép (VM) hoz létre a hello [Azure-portálon](https://portal.azure.com), egy nyilvános IP-erőforrás hello virtuális gép automatikusan létrejön. Az IP cím tooremotely hozzáférés hello virtuális gép használja. Bár a hello portál nem hoz létre egy [teljesen minősített tartománynevét](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), vagy teljesen minősített Tartományneve, felveheti egy hello virtuális gép létrehozása után. Ez a cikk bemutatja a hello lépéseket toocreate egy DNS-nevét vagy teljes Tartománynevét.

## <a name="create-a-fqdn"></a>Hozzon létre egy teljesen minősített Tartományneve
Ez a cikk feltételezi, hogy már létrehozta a virtuális gépek. Ha szükséges, akkor [hozzon létre egy virtuális gép hello portálon](quick-create-portal.md) vagy [hello Azure parancssori Felülettel rendelkező](quick-create-cli.md). Miután a virtuális gép megfelelően működik, és kövesse az alábbi lépéseket:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Csatlakoztathatja távolról virtuális gép erre a DNS-nevével, mint toohello `ssh azureuser@mydns.westus.cloudapp.azure.com`.

## <a name="next-steps"></a>Következő lépések
Most, hogy a virtuális gép nyilvános IP-cím és DNS névvel rendelkezik, akkor általános alkalmazás-keretrendszerbeli vagy szolgáltatásokat tudjanak telepíteni nginx, a MongoDB, a Docker, például stb.

Akkor is olvashat további tudnivalókat [erőforrás-kezelő használatával](../../azure-resource-manager/resource-group-overview.md) kapcsolatos tippek felépítése az Azure-környezetekhez.

