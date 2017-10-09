---
title: "Azure Tárolószolgáltatás aaaDC/OS-ügynök készleteket |} Microsoft Docs"
description: "Egy Azure tároló szolgáltatás DC/OS-fürtről a hello nyilvános és titkos ügynök készletek működése"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: c7d3889db07cb9908e8b68b668bd8a14ef3c2552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dcos-agent-pools-for-azure-container-service"></a>Azure Tárolószolgáltatási DC/OS-ügynök készleteket
Azure Tárolószolgáltatási DC/OS fürtök ügynök csomópontok két rendelkezik, egy nyilvános készlet és egy személyes készletet tartalmaz. Egy alkalmazás telepíthető tooeither készletbe, a tárolószolgáltatás gépek közötti kisegítő érintő. hello gépek lehet kitett toohello internet (nyilvános) vagy belső (magánhálózati). Ez a cikk rövid áttekintést nyújt, ezért azonban nyilvános és titkos.


* **Személyes ügynökök**: titkos ügynök csomópontokat átszervezhető nem irányítható hálózathoz. Ez a hálózat hello admin zónából vagy hello nyilvános zóna peremhálózati útválasztó csak érhető el. Alapértelmezés szerint a DC/OS indít alkalmazások titkos ügynök csomópontján. 

* **Nyilvános ügynökök**: nyilvános ügynök csomópontok DC/OS-alkalmazások és szolgáltatások futtatása egy nyilvánosan elérhető a hálózaton keresztül. 

A DC/OS hálózati biztonsággal kapcsolatos további információkért lásd: hello [DC/OS-dokumentáció](https://dcos.io/docs/1.7/administration/securing-your-cluster/).

## <a name="deploy-agent-pools"></a>Ügynök-készletek központi telepítése

hello DC/OS-ügynök készletek az Azure Tárolószolgáltatás jönnek létre az alábbiak szerint:

* Hello **titkos készlet** hello számú meg kell adnia mikor ügynök csomópontot tartalmaz, [hello DC/OS-fürt üzembe](container-service-deployment.md). 

* Hello **nyilvános készlet** kezdetben a egy előre meghatározott számú ügynök csomópontot tartalmaz. Amikor hello DC/OS-fürt ki van építve a rendszer automatikusan hozzáadja a készlet.

hello titkos készlet és hello nyilvános készlet olyan Azure virtuálisgép-méretezési készlet. Telepítés után a készletek is átméretezhetők.

## <a name="use-agent-pools"></a>Ügynök-készletek használatára
Alapértelmezés szerint **Marathon** telepíti, minden új alkalmazás toohello *titkos* ügynök csomópontok. Hello alkalmazás toohello telepítése tooexplicitly rendelkezik *nyilvános* csomópontok hello alkalmazás hello létrehozása során. Jelölje be hello **nem kötelező** lapra, és írja be **slave_public** a hello **elfogadott erőforrás-szerepkörök** érték. Ez a folyamat dokumentált [Itt](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) és hello [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) dokumentációját.

## <a name="next-steps"></a>Következő lépések
* Tudjon meg többet az [a DC/OS-tárolók kezelése](container-service-mesos-marathon-ui.md).

* Ismerje meg, hogyan túl[nyissa meg a hello tűzfal](container-service-enable-public-access.md) Azure tooallow nyilvános hozzáférés tooyour DC/OS tárolók által biztosított.

