---
title: egy webhely ReportViewer aaaUse |} Microsoft Docs
description: "Ez a témakör ismerteti, hogyan toobuild hello Visual Studio ReportViewer vezérlő, amely a jelentés megjeleníti a Microsoft Azure webhely tárolja a Microsoft Azure virtuális gépeket."
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 78b76318-d9bf-48ef-9d9e-d1b7d8cf3042
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: asaxton
ms.openlocfilehash: 8e0729d6657f96c32a9ac7dffdff7745ff92b48d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>A ReportViewer használata Azure-ban üzemeltetett webhelyen
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

Hello Visual Studio ReportViewer vezérlő, amely a Microsoft Azure virtuális gépeket tárolt jelentés megjeleníti a Microsoft Azure webhely hozhat létre. hello ReportViewer vezérlőelem egy webalkalmazás hello ASP.NET webalkalmazás-sablonja segítségével készít.

> [!IMPORTANT]
> hello ASP.NET MVC webalkalmazás sablonok nem támogatják a hello ReportViewer vezérlő.

tooincorporate ReportViewer a Microsoft Azure-webhelyre, a következő feladatok toocomplete hello kell.

* **Adja hozzá** szerelvények toohello központi telepítési csomag
* **Konfigurálása** hitelesítés és engedélyezés
* **Közzététel** ASP.NET Web application tooAzure hello

## <a name="prerequisites"></a>Előfeltételek
Tekintse át az "általános javasolt és ajánlott eljárások" szakasz hello [SQL Server Business Intelligence Azure virtuális gépek](../classic/ps-sql-bi.md).

> [!NOTE]
> A Visual Studio, a Standard Edition vagy újabb mellékeltük a ReportViewer vezérlő. Ha hello Web Developer Express Edition használ, telepítenie kell a hello [MICROSOFT jelentés VIEWER 2012 FUTÁSIDEJŰ](https://www.microsoft.com/download/details.aspx?id=35747) toouse hello ReportViewer futtatókörnyezeti szolgáltatásai.
> 
> Helyi feldolgozási üzemmód ReportViewer nem támogatott a Microsoft Azure-ban.

## <a name="adding-assemblies-toohello-deployment-package"></a>Szerelvények toohello központi telepítési csomag hozzáadása
Ha az ASP.NET alkalmazás helyszíni, hello ReportViewer szerelvények hello globális szerelvény-gyorsítótárban (GAC) hello IIS-kiszolgáló közvetlenül a Visual Studio telepítése során általában telepített, és közvetlenül hello alkalmazás által elérhető. Azonban ha a gazdagép, az ASP.NET-alkalmazás hello felhőben, Microsoft Azure nem engedélyezi semmit toobe-bA telepítve hello GAC-ban, ezért meg kell győződnie arról hello ReportViewer szerelvények érhetők el helyileg az alkalmazáshoz. Ehhez a projekt hivatkozásainak toothem hozzáadásával, és konfigurálja őket toobe helyben másolt.

Távoli feldolgozás módban hello ReportViewer vezérlő használja a következő szerelvények hello:

* **Microsoft.ReportViewer.WebForms.dll**: hello ReportViewer kódot tartalmaz, amely szükséges a lapon ReportViewer toouse. Amikor egy ASP.NET lapra ReportViewer vezérlőelem dobja el a projekt, leírását, ezt a szerelvényt tooyour projekt megjelenik.
* **Microsoft.ReportViewer.Common.dll**: futás közben hello ReportViewer vezérlő által használt osztályok tartalmazza. Tooyour projekt automatikusan nincs hozzáadva.

### <a name="tooadd-a-reference-toomicrosoftreportviewercommon"></a>egy hivatkozási tooMicrosoft.ReportViewer.Common tooadd
* Kattintson a jobb gombbal a projekt **hivatkozások** csomópont, és válassza **hivatkozás hozzáadása**, hello szerelvény hello .NET lapon jelölje be, és kattintson a **OK**.

### <a name="toomake-hello-assemblies-locally-accessible-by-your-aspnet-application"></a>toomake hello szerelvények helyi elérhető-e az ASP.NET-alkalmazás
1. A hello **hivatkozások** mappát, kattintson a hello Microsoft.ReportViewer.Common szerelvény, hogy annak tulajdonságai hello tulajdonságai ablakban jelennek meg.
2. A hello tulajdonságok ablakban állítsa **másolása helyi** tooTrue.
3. A Microsoft.ReportViewer.WebForms ismételje meg az 1. és 2.

### <a name="tooget-reportviewer-language-pack"></a>tooget ReportViewer nyelvi csomagja
1. Hello megfelelő Microsoft jelentés Viewer 2012 futásidejű újraterjeszthető csomag telepítése a [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=317386).
2. Hello legördülő listából, és hello lapon jelölje be hello nyelvi lekérdezi az átirányított toohello megfelelő download center oldalára.
3. Kattintson a **letöltése** ReportViewerLP.exe toostart hello letöltését.
4. ReportViewerLP.exe letöltése után kattintson **futtatása** tooinstall azonnal, vagy kattintson a **mentése** toosave azt tooyour számítógép. Ha **mentése**, ne feledje hello mappa nevét, mely hello hello fájl mentési helyét.
5. Keresse meg a hello mappában, ahova hello fájlt mentette. Kattintson a jobb gombbal a ReportViewerLP.exe, kattintson a **Futtatás rendszergazdaként**, és kattintson a **Igen**.
6. Miután ReportViewerLP.exe, látni fogja hello c:\windows\assembly rendelkezik hello erőforrásfájlok **Microsoft.ReportViewer.Webforms.Resources** és **Microsoft.ReportViewer.Common.Resources** .

### <a name="tooconfigure-for-localized-reportviewer-control"></a>honosított ReportViewer vezérlő tooconfigure
1. Töltse le és hello Microsoft jelentés Viewer 2012 futásidejű újraterjeszthető csomag telepítése a következő hello fent megadott utasítások szerint.
2. Hozzon létre <language> hello projektet, és másolja hello mappájában kapcsolódó erőforrás szerelvény fájlok. hello erőforrás szerelvény fájlok toobe másolt vannak: **Microsoft.ReportViewer.Webforms.Resources.dll** és **Microsoft.ReportViewer.Common.Resources.dll**. Válassza ki a hello erőforrás fájlokhoz, és a hello tulajdonságok ablakban állítsa **tooOutput Directory másolása** túl "**mindig másolása**".
3. Állítsa be kulturális környezet & UICulture hello hello webes projektet. Hogyan tooset hello kulturális környezet és a felhasználói felületi kulturális környezet ASP.NET weblap: további információk [How to: Set hello kulturális környezet és a felhasználói felületi kulturális környezet ASP.NET-weblap globalizációs](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Hitelesítési és engedélyezési konfigurálása
hello ReportViewer kell toouse szükséges hitelesítő adatokat tooauthenticate hello a jelentéskészítő kiszolgálónál, és hello jelentéskészítő kiszolgáló tooaccess hello jelentések kívánt hello hitelesítő adatok engedéllyel kell rendelkeznie. A hitelesítés további információkért lásd: hello tanulmány [Reporting Services jelentés megjelenítő vezérlő és a Microsoft Azure virtuálisgép-alapú jelentés kiszolgálók](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-hello-aspnet-web-application-tooazure"></a>Az ASP.NET Web application tooAzure hello közzététele
Az ASP.NET Web application tooAzure közzétételi útmutatásért lásd: [hogyan: át, és tegye közzé a Visual Studióból egy webalkalmazás tooAzure](../../../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) és [Ismerkedés a webalkalmazásokkal és az ASP.NET](../../../app-service-web/app-service-web-get-started-dotnet.md).

> [!IMPORTANT]
> Ha hello Azure telepítési projekt hozzáadása vagy hozzáadása Azure Cloud Service-projekt parancs nem jelenik meg a Megoldáskezelőben hello helyi menü, szükség lehet hello projekt too.NET-keretrendszer 4-toochange hello célkeretrendszer.
> 
> a két hello parancsok lényegében adja meg ugyanazokat a funkció hello. Egy vagy hello más parancs fog megjelenni hello helyi menü attól függően, hogy a Microsoft Azure SDK hello verziójának telepítését.
> 
> 

## <a name="resources"></a>Erőforrások
[Microsoft-jelentések](http://go.microsoft.com/fwlink/?LinkId=205399)

[Az SQL Server Business Intelligence használata Azure-beli virtuális gépeken](../classic/ps-sql-bi.md)

[Használjon PowerShell tooCreate egy Azure virtuális gép, egy natív mód jelentéskészítő kiszolgáló](../classic/ps-sql-report.md)

