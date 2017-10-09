---
title: az Azure Remoteappban QuickBooks aaaDeploy |} Microsoft Docs
description: Megtudhatja, hogyan tooshare QuickBooks az Azure RemoteApp.
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c5d00753-77c0-4f0d-a5df-9451b46a31d3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: c21b1ac311449be2281f9ce157828260e856f55d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Hogyan telepítheti az Azure Remoteappban QuickBooks?
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Hello információk tooshare QuickBooks követően az alkalmazások az Azure Remoteappban használja.

Azure RemoteApp QuickBooks 2015 vállalati felhőalapú vagy hibrid gyűjtemény megoszthatja. hello vállalati fájlt a QuickBooks adatbázis-kiszolgáló hello Azure RemoteApp-kiszolgáló számítógéptől eltérő operációs rendszert futtató virtuális gép kell lennie. Soha ne tárolja az hello vállalati fájlt az Azure RemoteApp-rendszerkép - adatvesztés várható, ha ezt teszi. Csak a QuickBooks vállalati szabványos Windows hálózati elérhetők QuickBooks-kiszolgálóval, külső megosztott üzemeltetési hello QuickBooks fájl támogatja.   

> [!IMPORTANT]
> hello QuickBooks adatbázis-kiszolgáló hello vállalati fájlt kell lennie egy külön virtuális gépen belül hello ugyanaz, mint hello Azure RemoteApp-gyűjtemény virtuális hálózat.  
> 
> 

## <a name="steps-toodeploy-quickbooks"></a>Lépéseket toodeploy QuickBooks
1. Hozzon létre egy Azure virtuális gép és a QuickBooks, QuickBooks adatbázis-kiszolgáló telepítése, és helyezze hello vállalati fájlt egy Azure virtuális gépen.  Győződjön meg arról, hogy tooproperly tűzfalszabályok konfigurálása.
2. QuickBooks telepíthető egy [egyéni lemezkép](remoteapp-imageoptions.md) , és hozzon létre egy [Azure RemoteApp-gyűjtemény](remoteapp-collections.md), vagy felhőalapú vagy hibrid, hello belül hol hello Virtuálisgép üzemeltetési hello QuickBooks adatbázis-kiszolgálón, ugyanazt a virtuális Hálózatot pontos Vállalati fájlok helyezkedik el. 
3. [Közzététel](remoteapp-publish.md) QuickBooks app toousers
4. Indítsa el a hello QuickBooks Azure RemoteApp-kiszolgálón futó ügyfél, lépjen a szabványos Windows hálózati toohello Virtuálisgép üzemeltetési hello QuickBooks adatbázis-kiszolgáló használatával hello vállalati fájl megnyitásával. 

## <a name="documentation-references"></a>Dokumentáció hivatkozások
* QuickBooks [által támogatott konfigurációk](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
* QuickBooks [üzembe helyezési lehetőségei](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Is megtekintheti a ignite-on bemutató [a Microsoft Azure RemoteApp felügyeleti alapjait és felügyeleti](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) -előre too1:02:45 tooget toohello QuickBooks része.

## <a name="deployment-architecture"></a>Üzembe helyezési architektúrája
![QuickBooks + az Azure RemoteApp központi telepítés](./media/remoteapp-quickbooks/ra-quickbooks.png)

