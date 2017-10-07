---
title: "Windows virtuális gép DotNet Core oktatóanyag 1 aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 8df69c496f44acb02d8afc45695349ec1f558f99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automating-application-deployments-toowindows-virtual-machines"></a>Alkalmazás központi telepítésének tooWindows virtuális gépek automatizálása

A négyrészes sorozat részletezi, telepítése és konfigurálása az Azure-erőforrások, az alkalmazások Azure Resource Manager-sablonok használatával. A sorozat mintasablon telepítve van, és hello megvizsgálta a központi telepítési sablont. az adatsorozat hello célját tooeducate hello kapcsolat Azure-erőforrások között, és tooprovide átadja a teljesen integrált Azure Resource Manager-sablonok telepítésével kapcsolatos. Ez a dokumentum azt feltételezi, hogy egy alapszintű Tudásbázis az Azure Resource Manager, az oktatóanyag elindítása előtt ismerkedjen meg Azure Resource Manager alapvető fogalmait.

## <a name="music-store-application"></a>Zene áruházbeli alkalmazás
hello az adatsorozathoz használt minta a .net Core alkalmazás élmény vásárlás zene Áruházbeli szimulálva. Ez az alkalmazás telepített tooeither egy Linux vagy a Windows virtuális rendszer minta központi telepítések készített is lehet. hello alkalmazás tartalmazza a webalkalmazás és az SQL-adatbázis. A sorozat hello cikkek olvasása, mielőtt a hello telepítési gombjára kattintva ezen a lapon található hello alkalmazás központi telepítése. Teljes telepítésekor hello alkalmazás / Azure architektúra tűnik a következő diagram hello. 

hello zene tároló Resource Manager-sablon, itt található [zene tároló Windows sablon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)

![Zene Áruházbeli alkalmazás](./media/dotnet-core-1-landing/music-store.png)

A sablon JSON elbírálása van a következő négy cikkek hello mindhárom összetevőt tartalmazzák, beleértve a hello társítani.

* [**Alkalmazásarchitektúra** ](dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – például alkalmazás-összetevők webalkalmazás-helyek és adatbázisok Azure számítógép erőforrások, például a virtuális gépek és az Azure SQL-adatbázisokban tárolt toobe kell. Ez a dokumentum végigvezeti a számítási szükség leképezési, tooAzure erőforrások és az Azure Resource Manager sablonnal ezen erőforrásokat üzembe helyezi. 
* [**Hozzáférés és biztonság** ](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – az Azure-ban üzemeltetéséhez, esetén szükséges tooconsider hogyan hello alkalmazás érhető el, és hogyan különböző alkalmazás-összetevők elérni egymást. Ez a dokumentum részletesen biztosítása és internet access tooan alkalmazás és az alkalmazás-összetevők közötti hozzáférés biztonságossá tételéhez.
* [**Rendelkezésre állás és méretezhetőség** ](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – rendelkezésre állásának és méretezésének tekintse meg a toohello alkalmazások képességét toostay infrastruktúra leállások során fut, és a hello képességét tooscale számítási erőforrások toomeet alkalmazás igény szerint. Ez a dokumentum részletek hello összetevők toodeploy egy elosztott terhelésű és magas rendelkezésre állású alkalmazás szükséges.
* [**Alkalmazás központi telepítésének** ](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – Ha alkalmazások alakzatot Azure virtuális gépek, mely hello által bináris alkalmazásfájlokat telepített virtuális gép hello hello metódus figyelembe kell venni. Ez a dokumentum részletesen automatizálása alkalmazás telepítése Azure virtuális gép egyéni parancsfájl-kiterjesztés használatával.

hello Azure Resource Manager-sablonok fejlesztésekor célja az Azure infrastruktúra, és hello telepítési tooautomate hello központi telepítését és konfigurációját az Azure infrastruktúra folyamatban futó alkalmazások. Ezek a cikkek keresztül működő mutatja a felhasználói felület.

## <a name="deploy-hello-music-store-application"></a>Hello zene áruházbeli alkalmazás központi telepítése
hello zene áruházból származó alkalmazás segítségével erre a gombra kattintva is telepíthető.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-windows%2Fazuredeploy.json" target="_blank"> <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

hello Azure Resource Manager-sablon a következő paraméterértékeket hello igényel.

| Paraméter neve | Leírás |
| --- | --- |
| ADMINUSERNAME |Hello virtuális gép és az Azure SQL Database hello rendszergazda felhasználóneve. |
| ADMINPASSWORD |Hello Azure virtuális gép és az SQL-adatbázis használt jelszót. |
| NUMBEROFINSTANCES |létrehozott virtuális gépek toobe hello száma. A virtuális gépek állomás hello zeneáruház webes alkalmazás egyes, és az összes forgalom ezek között elosztott terhelésű. |
| PUBLICIPADDRESSDNSNAME |Hello nyilvános IP-cím társított globálisan egyedi DNS-nevét. |

Hello sablon telepítésének befejezése után keresse meg a nyilvános IP-címet használ az internetes böngészők toohello. hello .net Core zene webhely számára jelenik meg.

## <a name="next-steps"></a>Következő lépések
<hr>

[1. lépés – az Azure Resource Manager-sablonok Alkalmazásarchitektúra](dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[2. lépés - hozzáférés és biztonság az Azure Resource Manager-sablonok](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[3. lépés – rendelkezésre állásának és méretezésének az Azure Resource Manager-sablonok](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[4. lépés - alkalmazás központi telepítése Azure Resource Manager-sablonok](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

