---
title: "egy virtuálisgép-lemezkép az Azure piactér hello létrehozása aaaTechnical előfeltételei |} Microsoft Docs"
description: "A létrehozása és telepítése a virtuális gép lemezképének toohello Azure piactér mások hello követelmények megértése érdekében toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 63c16966-0304-4b17-a715-368a0a5ccb2c
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: fcae4d9e052581e3c1dfe7962e6d50ec31040419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-hello-azure-marketplace"></a>A virtuálisgép-lemezkép az Azure piactér hello létrehozása műszaki előfeltételei
Hello folyamat megkezdése előtt alaposan és megértettem, hogy hol és miért minden egyes lépést. Amennyire csak lehetséges, meg kell készítse elő a vállalati adatok és egyéb adatokat, töltse le a szükséges eszközök, és/vagy technikai összetevő létrehozása hello ajánlat létrehozási folyamat megkezdése előtt. Ezek az elemek egyértelműen kiderül, hogy ez a cikk áttekintése kell lennie.  

## <a name="download-needed-tools--applications"></a>Töltse le a szükséges eszközök és alkalmazások
A következő elemek készen áll a hello folyamat megkezdése előtt hello kell rendelkezniük:

* Attól függően, hogy milyen operációs rendszert céloz meg, a telepítés hello [Azure PowerShell-parancsmagok](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) vagy [Linux parancssori felület eszköz](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) a hello [Azure letölti](https://azure.microsoft.com/downloads/) lap.
* Telepítse az Azure Storage Explorer a Codeplex webhelyen.
* Töltse le, és telepítse a hello hitelesítő vizsgálati eszköz az Azure hitelesített:
  * [http://go.microsoft.com/fwlink/?LinkId=526913](http://go.microsoft.com/fwlink/?LinkID=526913). Szüksége van egy Windows-alapú számítógép toorun hello hitelesítő eszközt. Ha nem rendelkezik egy Windows-alapú számítógép elérhető, futtathatja a Windows-alapú virtuális gépek használata az Azure-ban hello eszközt.

## <a name="platforms-supported"></a>Támogatott platformok
Virtuális gépek Azure-alapú Windows vagy Linux fejleszthet. Hello közzétételi folyamat – például létrehozhat egy Azure-kompatibilis virtuális merevlemez (VHD) – használja a különböző eszközök és lépések attól függően, hogy milyen operációs rendszert használ egyes elemei:  

* Ha Linux használ, tekintse meg a "Hozzon létre egy Azure-kompatibilis VHD-t (Linux-alapú)" című toohello hello [virtuális gép lemezképének közzétételi útmutató](marketplace-publishing-vm-image-creation.md).
* Ha Windows használ, tekintse meg a toohello "Hozzon létre egy Azure-kompatibilis VHD-t (Windows-alapú)" szakasza hello [virtuális gép lemezképének közzétételi útmutató](marketplace-publishing-vm-image-creation.md).

> [!NOTE]
> El kell érni tooa Windows-alapú gép:
> 
> * Hello hitelesítő érvényesítési eszköz futtatásához.
> * Hello megosztott virtuális merevlemez hozzáférési aláírás URL-címe hello VHD hitelesítő küldésének létrehozása.
> 
> 

## <a name="develop-your-vhd"></a>A virtuális merevlemez fejlesztése
Azure virtuális merevlemezek hello felhőalapú vagy helyszíni fejleszthet:

* Felhőalapú fejlesztési azt jelenti, hogy minden fejlesztési lépést távolról elvégzett rezidens Azure virtuális Merevlemezt.
* A helyi fejlesztési letöltése a virtuális merevlemez és a fájlrendszersérülések segítségével helyszíni infrastruktúra van szükség. Bár ez lehetséges, nem ajánlott. Vegye figyelembe, hogy Windows vagy SQL fejleszti a helyszíni meg toohave hello vonatkozó helyszíni licenckulcsot. Nem tartalmaznak, vagy az SQL Server telepítése a virtuális gép létrehozása után. Az ajánlat is hello Azure-portálon a jóváhagyott SQL kép kell alapozni. Ha úgy dönt, hogy a helyszíni toodevelop, néhány lépést működnek, mint ha volt fejlesztése hello felhőben kell elvégeznie. A releváns információt [hozzon létre egy helyszíni Virtuálisgép-lemezkép](marketplace-publishing-vm-image-creation-on-premise.md).

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
