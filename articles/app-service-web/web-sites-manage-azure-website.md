---
title: "aaaManage egy webalkalmazást az Azure App Service-ben"
description: "Hivatkozások tooresources egy webalkalmazást az Azure App Service kezeléséhez."
services: app-service\web
documentationcenter: 
author: erikre
manager: erikre
editor: 
ms.assetid: d5e2887a-84f9-4747-a573-867635cb8b39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: rachelap
ms.openlocfilehash: daf69245e66068b0e97e3ae1c3fb5fce45605b91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-web-app-in-azure-app-service"></a>Egy webalkalmazást az Azure App Service kezelése
Ez a témakör a webes alkalmazás kezelésére szolgáló hivatkozások tooresources [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Felügyeleti tartalmazza az összes a webes alkalmazás zökkenőmentes mindig hello feladatokhoz. 

A webalkalmazás hello élettartama különböző felügyeleti műveleteket kell végrehajtania, a kezdeti telepítés toonormal műveletet, a karbantartási és a frissítések mozgatásakor.

Számos web app feladat hello Azure-portálon végezhető.

## <a name="before-you-deploy-your-web-app-tooproduction"></a>A webes alkalmazás tooproduction központi telepítése előtt
### <a name="choose-a-tier"></a>Egy szint kiválasztása
Az Azure App Service öt rétegek akkor érhető el: ingyenes, a megosztott, Basic, Standard és Premium. Hello szolgáltatásait, és az egyes rétegekhez díjszabási információkért lásd: [díjszabása](https://azure.microsoft.com/pricing/details/app-service/). 

* [App Service-csomagok](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) hello alatt több webalkalmazások csoportosítását teszik azonos tartozó.
* Mindig is [rétegek kapcsoló](web-sites-scale.md) a webalkalmazás létrehozása után.

### <a name="configuration"></a>Konfiguráció
Használjon hello [Azure Portal](https://portal.azure.com/) tooset különböző konfigurációs beállításait. További információkért lásd: [webes alkalmazások konfigurálása az Azure App Service](web-sites-configure.md). Íme egy gyors Ellenőrzőlista:

* Válassza ki **futásidejű verziók** a .NET, PHP, Java vagy Python, ha szükséges.
* Engedélyezése **websocket elemek** Ha a webalkalmazás hello WebSocket protokollt használja. (Ez magában foglalja a használó alkalmazások [ASP.NET SignalR](http://www.asp.net/signalr) vagy [socket.io](web-sites-nodejs-chat-app-socketio.md).)
* A folyamatos webjobs fut? Ha igen, engedélyezze a **mindig a**.
* Set hello **alapértelmezett dokumentum**, például a index.html.

Továbbá toothese alapszintű konfigurációs beállításokkal érdemes lehet tooconfigure hello következő:

* **Secure Socket Layer (SSL)** titkosítás. SSL toouse egy egyéni tartománynév, akkor be kell szereznie egy SSL tanúsítvány és a webes alkalmazás toouse konfigurálni azt. Lásd: [HTTPS engedélyezése az Azure App Service-ben webalkalmazáshoz](app-service-web-tutorial-custom-ssl.md).
* **Egyéni tartomány nevét.** A webalkalmazás automatikusan egy altartomány azurewebsites.net alatt van. Egy egyéni tartománynevet, például contoso.com lehet társítani. Lásd: [egyéni tartománynév beállítása az Azure App Service](app-service-web-tutorial-custom-domain.md).

Nyelvspecifikus konfiguráció:

* **A PHP**: [PHP konfigurálása az Azure App Service Web Apps](web-sites-php-configure.md).
* **Python**: [Python konfigurálása az Azure App Service Web Apps](web-sites-python-configure.md)

## <a name="while-your-web-app-is-running"></a>A webes alkalmazás futtatása közben
A webalkalmazás működik, amíg azt szeretné, akkor érhető el, és hogy toomeet felhasználói forgalom méretezés toomake. Tootroubleshoot hibákat is szükség lehet.

### <a name="monitoring"></a>Figyelés
* Hello Azure portál, keresztül is [metrikák hozzáadása](web-sites-monitor.md) például CPU-használat és ügyfél-kérelmek számát jelenti.
* [A webalkalmazás skálázása](web-sites-scale.md) a válasz tootraffic. Attól függően, hogy a réteg méretezheti hello virtuális gépek száma és/vagy a Virtuálisgép-példányok hello hello méretét. Hello Standard és prémium csomag szükséges is beállíthat automatikus skálázást, így a webes alkalmazás automatikusan méretezi a meghatározott ütemezés szerint vagy válasz tooload.  

### <a name="backups"></a>Biztonsági másolatok
* Állítsa be [automatikus biztonsági mentésekhez](web-sites-backup.md) webalkalmazás. További információ a biztonsági másolatok [Ez a videó](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).
* További tudnivalók a hello-beállítások [adatbázis helyreállítás](../sql-database/sql-database-business-continuity.md) az Azure SQL-adatbázis.

### <a name="troubleshooting"></a>Hibaelhárítás
* Ha valamilyen hiba, akkor [hibaelhárítása a Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), a diagnosztikai naplók segítségével, és élő hello felhőbeli hibakeresést. 
* Visual Studio kívül vannak különböző módokon toocollect diagnosztikai naplókat. Lásd: [az Azure App Service web Apps diagnosztikai naplózás engedélyezése](web-sites-enable-diagnostic-log.md).
* Node.js-alkalmazások, lásd: [hogyan toodebug egy Node.js webalkalmazás az Azure App Service](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Adatok visszaállítása
* [Visszaállítás](web-sites-restore.md) egy korábban mentett webalkalmazást.

## <a name="when-you-update-your-web-app"></a>A webalkalmazás frissítése
Ha nem engedélyezte az automatikus biztonsági mentésekhez, létrehozhat egy [manuális biztonsági mentés](web-sites-backup.md).

Érdemes lehet [előkészített központi telepítési](web-sites-staged-publishing.md). Ez a beállítás lehetővé teszi a frissítések tooa egymás mellett futó telepítési átmeneti közzététele a éles telepítés. 


<!-- Anchors. -->

[Before you deploy your site tooproduction]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website


