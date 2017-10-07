---
title: "aaaWhat az hello Azure RemoteApp sablon rendszerképei? | Microsoft Docs"
description: "Tudnivalók az Azure Remoteapphez mellékelt hello sablonrendszerképek."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7f8442b2-81da-421e-a453-aa53ba2066b7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: ea012cec8dc581a8bd4a5a138ce302de19d5c6af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-in-hello-azure-remoteapp-template-images"></a>Mi az az hello Azure RemoteApp sablon rendszerképei?
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp-előfizetés három sablon rendszerképet tartalmaz:

* Windows Server 2012
* Microsoft Office 365 ProPlus (a használatához Office 365-előfizetés szükséges)
* Microsoft Office 2013 Professional Plus (csak próbaverzió)

> [!IMPORTANT]
> Az Azure RemoteApp előfizetés hozzáférést toohello szoftver hello képek hello kivétellel Office 365 ProPlus, amelyhez különálló előfizetés szükséges, és Office 2013, amely a termelésben nem használható. Ez azt jelenti, hogy megoszthatja lévő hello sablon rendszerképek hello programokat és alkalmazásokat a felhasználóival. Például létrehozhat egy gyűjteményt, amely hello Windows Server 2012 R2 rendszerképet használja, ha közzéteheti System Center Endpoint Protection a tooaccess felhasználókat a Remoteappen keresztül.
> 
> Tekintse meg a hello [RemoteApp licensing részletek](remoteapp-licensing.md) további információt. És [Office használata az Azure RemoteApp](remoteapp-o365.md) a hello Office licencelési információ.
> 
> 

Az egyes rendszerképek tartalmának részleteiért olvasson tovább.

## <a name="windows-server-2012-r2--hello-vanilla-image"></a>Windows Server 2012 R2 ("hello eredeti rendszerkép")
Ez a rendszerkép a Microsoft Windows Server 2012 R2 Datacenter operációs rendszeren alapul, és hello következő szerepköröket és szolgáltatásokat telepítette toomeet hello Azure RemoteApp sablon rendszerképek követelményeinek:

* .NET-keretrendszer 4.5, 3.5.1, 3.5
* Asztali élmény
* Szabadkézi beviteli és kézírás-felismerő szolgáltatások
* Multimédia alaprendszer
* Távoli asztali munkamenetgazda
* Windows PowerShell 4.0
* Windows PowerShell integrált parancsprogram-kezelési környezet (ISE)
* WoW64-támogatás

Ez a rendszerkép is rendelkezik a következő telepített alkalmazások hello:

* Adobe Flash Player
* Microsoft Silverlight
* Microsoft System Center 2012 Endpoint Protection
* Microsoft Windows Media Player

## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (a használatához előfizetés szükséges)
Az Office 365 van hello leginkább kért alkalmazás, ezért létrehoztunk egy "egyéni" rendszerképet, a toowork.

Ez a rendszerkép hello eredeti rendszerkép bővített verziója, és hello Microsoft Office 365 ProPlus következő összetevői telepítve van ezen kívül hello Windows Server 2012 R2 rendszerképnél ismertetett toohello összetevők:

* Hozzáférés
* Excel
* Lync
* OneNote
* Onedrive vállalati verzió (vegye figyelembe, hogy hello sync-ügynök nem támogatott az Azure RemoteApp használata)
* Outlook
* PowerPoint
* Word
* Microsoft Office Nyelvi ellenőrző eszközök

hello lemezképet is tartalmaz, a Visio Pro és a Project Pro alkalmazásokat.

És alkalmazások, valamint a következő hello:

* SQL Native Client
* ODBC-illesztő
* SQL Server Data Mining-ügyfél
* MasterDataServices-ügyfél
* Microsoft Publisher
* PowerQuery
* PowerMap

Az Office 365 ProPlus-alkalmazások összes funkciója csak az Office 365 ProPlus-előfizetéssel rendelkező felhasználók számára érhető el. Hello Office 365-előfizetésben tervek című témakörben olvashat [Office 365 service-csomagokról](http://technet.microsoft.com/library/office-365-plan-options.aspx). További kérdései vannak? Tekintse meg a hello [Office 365 + RemoteApp](remoteapp-o365.md) információkat. Emellett olvassa el a hello új cikket, [hogyan toouse az Office 365-előfizetéshez az Azure RemoteApp](remoteapp-officesubscription.md).

Vegye figyelembe, hogy szüksége toolicense Office 365 ProPlus, a Visio Pro és a Project Pro külön-külön – ezek mindegyike saját licenccel rendelkezik.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (csak próbaverzió)
Hello ingyenes próbaverziós időszak alatt hello szolgáltatás hello Office 2013 lemezképpel tesztelheti.

Ez a rendszerkép hello eredeti rendszerkép bővített verziója, és hello Microsoft Office 2013 Professional Plus következő összetevői telepítve van ezen kívül hello Windows Server 2012 R2 rendszerképnél ismertetett toohello összetevők:

* Hozzáférés
* Excel
* Lync
* OneNote
* Onedrive vállalati verzió (vegye figyelembe, hogy hello sync-ügynök nem támogatott az Azure RemoteApp használata)
* Outlook
* PowerPoint
* Project
* Visio
* Word
* Microsoft Office Nyelvi ellenőrző eszközök

> [!IMPORTANT]
> **Jogi információk:** Ez a rendszerkép nem tartalmaz Microsoft Office-licencet, és *nem használható termelési környezetben*. hello Office 2013 Professional Plus-rendszerkép csak próbaverziós felhasználásra számára készült. Ha azt szeretné, toouse Office-alkalmazások az Azure Remoteappban termelési környezetben, meg kell toouse hello Office 365 ProPlus-rendszerképet. Az Office licenckezelésével kapcsolatos további részletekért tekintse meg a [Using Office 365 with Azure RemoteApp](remoteapp-o365.md) (Az Office és az Azure RemoteApp használata) című témakört
> 
> 

