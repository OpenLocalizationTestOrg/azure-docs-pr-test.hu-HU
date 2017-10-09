---
title: "Azure RemoteApp lemezkép aaaCreate |} Microsoft Docs"
description: "További tudnivalók: hello lehetőségeit az Azure RemoteApp-lemezképek létrehozása"
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
ms.openlocfilehash: 54e63b6fa13addfcda96ce581910e1ac48d91e70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-remoteapp-image"></a>Create an Azure RemoteApp image (Azure RemoteApp-rendszerkép létrehozása)
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp képek toohold hello alkalmazásokat a felhasználóival megosztott használja. (Azt a képet, és felhasználása azt toocreate virtuális gépek - közül mi hello felhasználók érhető el, amikor bejelentkeznek az Azure Remoteappba.) toocreate egy Azure RemoteApp-gyűjteményt a választott az alkalmazások, hogy felhőalapú vagy hibrid,-e, először hozzon létre kép az alkalmazások telepítése. Ezután hozzon létre egy gyűjteményt, amely használja ezt a lemezképet, felhasználók toohello gyűjtemény hozzárendelése és alkalmazások toothose felhasználók közzététele.

Több lehetőség közül választhat a létrehozására vagy a lemezképek használatával. Alapszintű hello [követelmény](remoteapp-imagereqs.md) a képet, hogy futtassa a Windows Server 2012 R2, telepítve hello távoli asztali munkamenetgazda (RDSH) szerepkör. Hogyan le, ahol részek lesznek érdekes.

Hello tooimages ismét a következő lehetőségek közül választhat:

* Lehet importálni és használni egy [lemezkép-alapú Azure virtuális géphez](remoteapp-image-on-azurevm.md). Ez az egyéni beállítások igénylő-üzleti alkalmazások helyes. Testre szabhatja a hello kép toowork hello alkalmazás.
* Is [létrehozása és feltöltése az egyéni lemezkép](remoteapp-create-custom-image.md). Ez a helyes, ha már rendelkezik egy olyanra, amely a helyszíni távoli asztali szolgáltatások környezethez használja.
* Hello használhat [sablonrendszerképek](remoteapp-images.md) a RemoteApp-előfizetés tartalmazza. Ezeket a lemezképeket jönnek létre, és hello RemoteApp csapatától tartja fenn, és néhány szokásos alkalmazást az, hogy elérhető tooyour felhasználók (például az hello Office suite) tartalmaznak. Vegye figyelembe, hogy csak hello Office 365 Pro Plus lemezkép üzemi környezetben is használható.

Függetlenül attól, hol szerezheti a lemezképet, vagy hogyan hoz létre, érdemes tisztában lennie hello toomake [alkalmazáskövetelmények](remoteapp-appreqs.md) tooensure, amely az alkalmazás működik jól RemoteApp. Ezt követően hello következő lépésre toocreate egy [felhő](remoteapp-create-cloud-deployment.md) vagy [hibrid](remoteapp-create-hybrid-deployment.md) gyűjtemény.

