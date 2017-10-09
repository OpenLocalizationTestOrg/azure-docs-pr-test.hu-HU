---
title: "aaaTroubleshoot RemoteApp felhőalapú gyűjtemények - létrehozási |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot RemoteApp felhőalapú gyűjtemény létrehozása sikertelen"
services: remoteapp
documentationcenter: 
author: vkbucha
manager: mbaldwin
ms.assetid: 95eb7797-7b5e-4781-ad23-f15dd85b213d
ms.service: remoteapp
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 9484ecbdb048ede8df725215b313e049cc7648f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Létrehozása RemoteApp felhőalapú gyűjtemények hibaelhárítása
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Ha problémába ütközik egy felhőalapú gyűjtemény létrehozása, tekintse meg a következő információk hello.

## <a name="your-image-is-invalid"></a>A kép érvénytelen.
Ha megjelenik egy üzenet, például "GoldImageInvalid", ha vár a Azure tooprovision a gyűjtemény, az azt jelenti, hogy a sablon lemezképe nem felel meg hello [kép követelmények definiált](remoteapp-imagereqs.md). Igen, nyissa meg olvasni azokat [követelmények](remoteapp-imagereqs.md), javítsa ki a lemezkép és toocreate próbálkozzon újra a gyűjteményben.

## <a name="common-errors-seen-in-hello-azure-management-portal"></a>Gyakori hibák látható a hello Azure felügyeleti portálon
    DNS server could not be reached
    ProvisioningTimeout

A felhőalapú gyűjtemények gyakran sikertelen, mert létrehozása során egyéni rendszerképet használ.  Ha a fenti hibák hello egyikét látja, és egy egyéni lemezkép toocreate hello gyűjteményt használ, ellenőrizze a dolgok következő hello:

* Győződjön meg arról, hello egyéni lemezképhez feltöltött lemezkép követelményeknek.
* Leggyakrabban hello gyakori probléma a hello lemezkép nem megfelelően a Sysprep segédprogrammal előkészített.  
* Győződjön meg arról hello kép rendszerindítást a Hyper-V belül, vagy próbáljon meg létrehozni egy infrastruktúra-szolgáltatási virtuális gép közvetlenül az Azure-előfizetése hello lemezképpel. Ha hello VM tooboot és nem kezdő nem sikerül, akkor ez általában jelzi hello egyéni lemezkép nem lett megfelelően előkészítve.  Ellenőrizze a hello egyéni lemezkép készült hello követően hogyan toocreate egy egyéni sablon rendszerképet a RemoteApp

Ha hello Microsoft rendszerképeket az előfizetéskor használunk, próbálja toocreate hello gyűjteményt újra. Ha nem szűnik hello majd forduljon a Microsoft támogatási szolgálatához.

    PlatformImageTrialModeOnly

Ha ezt a hibát látva Ez általában azt jelenti, hogy frissítették tooa fizetett fiók, de egy Microsoft által biztosított lemezképet, amely érvényes csak üzemmódban hello próba hello szolgáltatás toouse próbált. Ebben az esetben toocreate próbálkozzon újra a felhőalapú gyűjtemény, de lehet, hogy toospecify hello megfelelő lemezképet.

