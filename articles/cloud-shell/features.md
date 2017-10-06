---
title: "aaaAzure felhő rendszerhéj (előzetes verzió) szolgáltatásainak |} Microsoft Docs"
description: "Azure Cloud rendszerhéj funkcióinak áttekintése"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 65482ca6caeac01dda18a6b12eabe943e3d68a96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="features-and-tools-for-azure-cloud-shell"></a>Funkciók és eszközök az Azure-felhőbe rendszerhéj
Azure Cloud rendszerhéj egy webböngésző-alapú rendszerhéj élmény toomanage és fejlesztése az Azure-erőforrások.

Felhő rendszerhéj kínál a böngésző által elérhető, előre konfigurált rendszerhéj élmény nélkül hello terhelését, telepítése, versioning, és a gép karbantartása, saját magának az Azure erőforrások kezeléséhez.

Felhő rendszerhéj kérelem alapú számítógépek kiépítését, és emiatt gép állapota nem minden munkamenetben megmarad. Felhő rendszerhéj interaktív munkamenet lett tervezve, mivel a parancskörnyezet automatikusan leáll, 20 perc rendszerhéj inaktivitás után.

## <a name="bash-in-cloud-shell"></a>A felhő rendszerhéj bash
### <a name="tools"></a>Eszközök
|Kategória   |Név   |
|---|---|
|Linux-rendszerhéj parancsértelmező|Bash<br> SH               |
|Azure-eszközök            |[Az Azure CLI 2.0](https://github.com/Azure/azure-cli) és [1.0](https://github.com/Azure/azure-xplat-cli)<br> [AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy)<br> [Batch Shipyard](https://github.com/Azure/batch-shipyard)     |
|A szerkesztő szövege           |VIM<br> nano<br> emacs       |
|A verziókövetési rendszerrel         |git                    |
|Buildet            |Ellenőrizze<br> maven<br> npm<br> a pip         |
|Tárolók             |[A docker parancssori felület](https://github.com/docker/cli)/[Docker gép](https://github.com/docker/machine)<br> [Kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [Vázlat](https://github.com/Azure/draft)<br> [DC/OS PARANCSSORI FELÜLET](https://github.com/dcos/dcos-cli)         |
|Adatbázisok              |MySQL-ügyfél<br> PostgreSql-ügyfél<br> [Az Sqlcmd segédprogram használatával](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [MSSQL-scripter](https://github.com/Microsoft/sql-xplat-cli) |
|Egyéb                  |iPython ügyfél<br> [Felhő Foundry parancssori felület](https://github.com/cloudfoundry/cli)<br> |

### <a name="language-support"></a>Nyelvi támogatás
|Nyelv   |Verzió   |
|---|---|
|.NET       |1.01       |
|Indítás         |1.7        |
|Java       |1.8        |
|Node.js    |6.9.4      |
|Python     |2.7 és 3.5-ös (alapértelmezett)|

## <a name="secure-automatic-authentication"></a>Biztonságos automatikus hitelesítés
Felhő rendszerhéj biztonságosan és automatikusan hitelesíti az Azure CLI 2.0 hello felhasználóifiók-hozzáférés.

## <a name="azure-files-persistence"></a>Az Azure fájlok adatmegőrzési
Ideiglenes gépek használatának kérelem alapú felhő rendszerhéj lefoglalt, mivel fájlok kívül a $Home és a gép állapota nem maradnak meg a munkamenetek között.
toopersist fájlok felhő rendszerhéj bemutatja, hogyan egy Azure fájl csatolása keresztül megosztott az első indítási,-munkamenetek között.
Egyszer befejezett felhő rendszerhéj automatikusan elvégzi a tárhely minden további munkamenetekhez.

[További tudnivalók az Azure file megosztások tooCloud rendszerhéj csatolni.](persisting-shell-storage.md)

## <a name="next-steps"></a>Következő lépések
[Felhő rendszerhéj gyors üzembe helyezés](quickstart.md) <br>
[További tudnivalók az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/) <br>