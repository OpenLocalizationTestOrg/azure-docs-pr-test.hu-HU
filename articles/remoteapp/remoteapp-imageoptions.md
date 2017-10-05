---
title: "Azure RemoteApp-lemezkép létrehozása |} Microsoft Docs"
description: "Tudja meg, hogyan használható az Azure RemoteApp-lemezképek létrehozása"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: cb0f9424-6185-45a1-abe9-c23f1edf34f2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 4b8ba6f264f982e03930c5ad4ccdb2d80f2c8665
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-remoteapp-image"></a>Create an Azure RemoteApp image (Azure RemoteApp-rendszerkép létrehozása)
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Az Azure RemoteApp képek az alkalmazásokat, amelyek megosztása a a felhasználók tárolására használ. (Jelenleg igénybe vehet a lemezkép és közül a felhasználók hozzáférést amikor bejelentkeznek az Azure Remoteappba – a virtuális gépek létrehozására használható.) Egy Azure RemoteApp-gyűjtemény létrehozásához a választott alkalmazások, hogy felhőalapú vagy hibrid-e, akkor először hozzon létre kép az alkalmazások telepítése. Ezután hozzon létre egy gyűjteményt, amely használja ezt a lemezképet, és hozzárendelhet felhasználókat ehhez a gyűjteményhez alkalmazások közzétételére azoknak a felhasználóknak.

Több lehetőség közül választhat a létrehozására vagy a lemezképek használatával. A basic [követelmény](remoteapp-imagereqs.md) a képet, hogy futtassa a Windows Server 2012 R2, telepítve van a távoli asztali munkamenetgazda (RDSH) szerepkör. Hogyan le, ahol részek lesznek érdekes.

Amikor a képek rendelkezik a következő beállításokat:

* Lehet importálni és használni egy [lemezkép-alapú Azure virtuális géphez](remoteapp-image-on-azurevm.md). Ez az egyéni beállítások igénylő-üzleti alkalmazások helyes. Testre szabhatja a lemezképet, az alkalmazás működéséhez.
* Is [létrehozása és feltöltése az egyéni lemezkép](remoteapp-create-custom-image.md). Ez a helyes, ha már rendelkezik egy olyanra, amely a helyszíni távoli asztali szolgáltatások környezethez használja.
* Egyikét használhatja a [sablonrendszerképek](remoteapp-images.md) a RemoteApp-előfizetés tartalmazza. Ezeket a lemezképeket jönnek létre, és a RemoteApp csapatától tartja fenn, és néhány szokásos olyan alkalmazásokat tartalmaznak (például az Office suite), elérhetővé teheti a felhasználók számára. Vegye figyelembe, hogy csak az Office 365 Pro Plus kép üzemi környezetben is használható.

Függetlenül attól, hol szerezheti a lemezképet, vagy hogyan hoz létre, így megértheti érdemes a [alkalmazáskövetelmények](remoteapp-appreqs.md) annak érdekében, hogy az alkalmazás működik jól RemoteApp. Ezt követően a következő lépés, ha a egy [felhő](remoteapp-create-cloud-deployment.md) vagy [hibrid](remoteapp-create-hybrid-deployment.md) gyűjtemény.

