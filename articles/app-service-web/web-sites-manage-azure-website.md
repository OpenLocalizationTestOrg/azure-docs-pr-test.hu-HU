---
title: "Egy webalkalmazást az Azure App Service kezelése"
description: "Az Azure App Service webalkalmazás kezelésére szolgáló erőforrások hivatkozásait."
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
ms.openlocfilehash: 9e19618a1b24bbdf3163ddfc3423c5c932dcd7af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="manage-a-web-app-in-azure-app-service"></a>Egy webalkalmazást az Azure App Service kezelése
Ez a témakör a webes alkalmazás kezelésével kapcsolatos forrásokra mutató hivatkozásokat tartalmaz [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Felügyeleti tartalmaz minden olyan feladat, amely a webes alkalmazás zökkenőmentes tárolni. 

A webes alkalmazás életciklusa alatt különböző felügyeleti műveleteket kell végrehajtania, normál működés, a karbantartási és a frissítések kezdeti telepítéséből mozgatása.

Számos web app felügyeleti feladatot az Azure portálon hajtható végre.

## <a name="before-you-deploy-your-web-app-to-production"></a>A webalkalmazás üzemi központi telepítése előtt
### <a name="choose-a-tier"></a>Egy szint kiválasztása
Az Azure App Service öt rétegek akkor érhető el: ingyenes, a megosztott, Basic, Standard és Premium. A szolgáltatások és az egyes rétegekhez árképzési kapcsolatos információkért lásd: [díjszabása](https://azure.microsoft.com/pricing/details/app-service/). 

* [App Service-csomagok](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) lehetővé teszik, hogy több webalkalmazások alatt azonos tartozó csoport.
* Mindig is [rétegek kapcsoló](web-sites-scale.md) a webalkalmazás létrehozása után.

### <a name="configuration"></a>Konfiguráció
Használja a [Azure Portal](https://portal.azure.com/) különböző konfigurációs beállítások megadása. További információkért lásd: [webes alkalmazások konfigurálása az Azure App Service](web-sites-configure.md). Íme egy gyors Ellenőrzőlista:

* Válassza ki **futásidejű verziók** a .NET, PHP, Java vagy Python, ha szükséges.
* Engedélyezése **websocket elemek** Ha a webes alkalmazást használ a WebSocket protokoll. (Ez magában foglalja a használó alkalmazások [ASP.NET SignalR](http://www.asp.net/signalr) vagy [socket.io](web-sites-nodejs-chat-app-socketio.md).)
* A folyamatos webjobs fut? Ha igen, engedélyezze a **mindig a**.
* Állítsa be a **alapértelmezett dokumentum**, például a index.html.

A alapszintű konfigurációs beállítások mellett célszerű konfigurálja a következőket:

* **Secure Socket Layer (SSL)** titkosítás. SSL használata egy egyéni tartománynevet, SSL-tanúsítvány beszerzése és a webalkalmazás a használatára konfigurálnia kell. Lásd: [HTTPS engedélyezése az Azure App Service-ben webalkalmazáshoz](app-service-web-tutorial-custom-ssl.md).
* **Egyéni tartomány nevét.** A webalkalmazás automatikusan egy altartomány azurewebsites.net alatt van. Egy egyéni tartománynevet, például contoso.com lehet társítani. Lásd: [egyéni tartománynév beállítása az Azure App Service](app-service-web-tutorial-custom-domain.md).

Nyelvspecifikus konfiguráció:

* **A PHP**: [PHP konfigurálása az Azure App Service Web Apps](web-sites-php-configure.md).
* **Python**: [Python konfigurálása az Azure App Service Web Apps](web-sites-python-configure.md)

## <a name="while-your-web-app-is-running"></a>A webes alkalmazás futtatása közben
Miközben a webalkalmazás fut, győződjön meg arról, akkor érhető el, és hogy felhasználói forgalom megfelelő méretezés szeretné. Is szükség lehet hibák elhárítása.

### <a name="monitoring"></a>Figyelés
* Az Azure portálon keresztül is [metrikák hozzáadása](web-sites-monitor.md) például CPU-használat és ügyfél-kérelmek számát jelenti.
* [A webalkalmazás skálázása](web-sites-scale.md) forgalom adott válaszként. Attól függően, hogy a réteg a virtuális gépek számát és/vagy a Virtuálisgép-példány mérete méretezheti. A Standard és Premium rétegek is beállíthat automatikus skálázást, a webes alkalmazás automatikusan, méretezi a meghatározott ütemezés szerint vagy betölteni válaszként.  

### <a name="backups"></a>Biztonsági másolatok
* Állítsa be [automatikus biztonsági mentésekhez](web-sites-backup.md) webalkalmazás. További információ a biztonsági másolatok [Ez a videó](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).
* További tudnivalók a beállítások [adatbázis helyreállítás](../sql-database/sql-database-business-continuity.md) az Azure SQL-adatbázis.

### <a name="troubleshooting"></a>Hibaelhárítás
* Ha valamilyen hiba, akkor [hibaelhárítása a Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), a diagnosztikai naplók segítségével, és működés közbeni hibakeresés a felhőben. 
* Visual Studio kívül különböző módja van diagnosztikai naplók gyűjtésére. Lásd: [az Azure App Service web Apps diagnosztikai naplózás engedélyezése](web-sites-enable-diagnostic-log.md).
* Node.js-alkalmazások, lásd: [az Azure App Service a Node.js webalkalmazás hibakeresése](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Adatok visszaállítása
* [Visszaállítás](web-sites-restore.md) egy korábban mentett webalkalmazást.

## <a name="when-you-update-your-web-app"></a>A webalkalmazás frissítése
Ha nem engedélyezte az automatikus biztonsági mentésekhez, létrehozhat egy [manuális biztonsági mentés](web-sites-backup.md).

Érdemes lehet [előkészített központi telepítési](web-sites-staged-publishing.md). Ez a beállítás lehetővé teszi egy átmeneti telepítéshez egymás mellett futó közzétenni a frissítéseket a éles telepítés. 


<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website


