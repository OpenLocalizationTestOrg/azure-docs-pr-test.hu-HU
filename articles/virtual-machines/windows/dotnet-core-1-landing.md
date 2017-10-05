---
title: "A Windows Azure virtuális gép DotNet Core oktatóanyag 1 |} Microsoft Docs"
description: "Azure virtuális gép DotNet fő oktatóanyag"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 14d5f250-1f76-49d4-898f-07b58fd39e7c
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfb3a27d20e8cdcff8dff75e4dfb2685e2781d45
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="automating-application-deployments-to-windows-virtual-machines"></a>Alkalmazások központi telepítésének Windows virtuális gépek automatizálása

A négyrészes sorozat részletezi, telepítése és konfigurálása az Azure-erőforrások, az alkalmazások Azure Resource Manager-sablonok használatával. A sorozat mintasablon telepítve van, és a központi telepítési sablont vizsgálni. Az adatsorozat célja az Azure-erőforrások közötti kapcsolat felkészíteni, és adja meg a beavatkozás nélküli a teljesen integrált Azure Resource Manager-sablonok telepítésével kapcsolatos. Ez a dokumentum azt feltételezi, hogy egy alapszintű Tudásbázis az Azure Resource Manager, az oktatóanyag elindítása előtt ismerkedjen meg Azure Resource Manager alapvető fogalmait.

## <a name="music-store-application"></a>Zene áruházbeli alkalmazás
A sorozat felhasznált minta a .net Core alkalmazás élmény vásárlás zene Áruházbeli szimulálva. Ez az alkalmazás telepíthető legyen Windows vagy Linux rendszerű virtuális rendszer, a minta központi telepítések egyaránt létrejött. Az alkalmazás tartalmazza a webalkalmazás és az SQL-adatbázis. A cikkek a sorozat olvasása, mielőtt telepítené az alkalmazást, a telepítés gombra kattintva ezen a lapon található. Amikor teljesen telepítve van, az alkalmazás / Azure architektúra néz az alábbi ábrán. 

A zene tároló Resource Manager-sablon, itt található [zene tároló Windows sablon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)

![Zene Áruházbeli alkalmazás](./media/dotnet-core-1-landing/music-store.png)

A következő négy cikkekben megvizsgálják mindhárom összetevőt tartalmazzák, beleértve a társítás sablon JSON-JÁT.

* [**Alkalmazásarchitektúra** ](dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – alkalmazás-összetevők, például webes helyek és adatbázisokat kell Azure számítógép erőforrások, például virtuális gépek és az Azure SQL-adatbázisok üzemeltetni. Ez a dokumentum végigvezeti számítási szükség leképezési, Azure-erőforrások, és ezeket az erőforrásokat az Azure Resource Manager-sablonok telepítése. 
* [**Hozzáférés és biztonság** ](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – az Azure-ban üzemeltetéséhez, esetén figyelembe kell venni hogyan érhető el az alkalmazást, és különböző alkalmazás-összetevők hozzáférés egymással. Ez a dokumentum részletesen biztosítása és az alkalmazás internet-hozzáférést és alkalmazás-összetevők közötti hozzáférés biztonságossá tétele.
* [**Rendelkezésre állás és méretezhetőség** ](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – rendelkezésre állásának és méretezésének hivatkoznak marad legelvesebb leállás alatt futó alkalmazások lehetőséget, és számítási erőforrásokat alkalmazás igény szerinti méretezési képességét. Ez a dokumentum adatokat egy elosztott terhelésű telepítéséhez szükséges összetevők és magas rendelkezésre állású alkalmazás.
* [**Alkalmazás központi telepítésének** ](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – Ha alakzatot Azure virtuális gépek, a módszer, amellyel a bináris alkalmazásfájlokat számítógépen telepítve vannak a virtuális alkalmazások központi telepítése figyelembe kell venni. Ez a dokumentum részletesen automatizálása alkalmazás telepítése Azure virtuális gép egyéni parancsfájl-kiterjesztés használatával.

Az Azure Resource Manager-sablonok fejlesztésekor célja automatikus központi telepítése az Azure infrastruktúra, és a telepítése és konfigurálása az Azure infrastruktúra folyamatban futó alkalmazások. Ezek a cikkek keresztül működő mutatja a felhasználói felület.

## <a name="deploy-the-music-store-application"></a>Zene áruházból származó alkalmazás központi telepítése
Zene áruházból származó alkalmazás segítségével erre a gombra kattintva is telepíthető.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-windows%2Fazuredeploy.json" target="_blank"> <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Az Azure Resource Manager sablonhoz szükséges a következő paraméterértékeket.

| Paraméter neve | Leírás |
| --- | --- |
| ADMINUSERNAME |A virtuális gép és az Azure SQL Database rendszergazda felhasználóneve. |
| ADMINPASSWORD |Az Azure virtuális gépen, SQL-adatbázis használt jelszót. |
| NUMBEROFINSTANCES |A létrehozandó virtuális gépek száma. Ezek a virtuális gépek mindegyikének a zeneáruház webalkalmazás üzemeltetéséhez, és az összes forgalom terhelésű ezek között. |
| PUBLICIPADDRESSDNSNAME |A nyilvános IP-cím társított globálisan egyedi DNS-neve |

A sablon telepítésének befejezése után keresse meg azt a nyilvános IP-cím, bármely internetes böngésző használatával. A .net Core zene webhely számára jelenik meg.

## <a name="next-steps"></a>Következő lépések
<hr>

[1. lépés – az Azure Resource Manager-sablonok Alkalmazásarchitektúra](dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[2. lépés - hozzáférés és biztonság az Azure Resource Manager-sablonok](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[3. lépés – rendelkezésre állásának és méretezésének az Azure Resource Manager-sablonok](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[4. lépés - alkalmazás központi telepítése Azure Resource Manager-sablonok](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

