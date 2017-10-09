---
title: "a Java-alkalmazás tooAzure App Service Web Apps aaaAdd"
description: "Az oktatóanyag bemutatja, hogyan tooadd egy lap vagy az alkalmazás tooyour példányát az Azure App Service Web Apps már konfigurálva toouse Java."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9b46528b-e2d0-4f26-b8d7-af94bd8c31ef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 2feb464b2933921ad2887779a6b7589634e2e2f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-java-application-tooazure-app-service-web-apps"></a>A Java-alkalmazás tooAzure App Service Web Apps hozzáadása
Amennyiben Ön inicializálta a Java-webalkalmazás a [Azure App Service] [ Azure App Service] dokumentált [Java-webalkalmazás létrehozása az Azure App Service](web-sites-java-get-started.md), feltöltheti az alkalmazást úgy, hogy a WAR a hello **webapps** mappa.

navigációs elérési toohello hello **webapps** mappa alapján hogyan lehet beállítani a Web Apps példány eltér.

* Ha a webalkalmazás beállítása hello Azure piactér használatával, elérési út toohello hello **webapps** mappa hello formában van **d:\home\site\wwwroot\bin\application\_server\webapps**, ahol **alkalmazás\_server** hello név hello alkalmazáskiszolgáló érvényben a Web Apps példányát. 
* Ha a webalkalmazás beállítása hello Azure konfigurációs felhasználói felület használatával, elérési út toohello hello **webapps** mappa hello formában van **d:\home\site\wwwroot\webapps**. 

Vegye figyelembe, hogy forrás vezérlő tooupload használhatja, az alkalmazás vagy a weblapokat, beleértve a [folyamatos integrációt](app-service-continuous-deployment.md). FTP egyben a beállítás a az alkalmazás vagy a weblapok; feltöltése FTP keresztül az alkalmazások telepítésével kapcsolatos további információkért lásd: [telepítheti az alkalmazást tooAzure App Service].

Webalkalmazások Tomcat Megjegyzés: Miután a WAR-fájl toohello feltöltött **webapps** mappa, hello Tomcat alkalmazáskiszolgáló észleli, hogy hozzáadását, és automatikusan betölti azt. Vegye figyelembe, hogy ha toohello gyökérkönyvtár (eltérő WAR-fájlt) fájlokat másolja, hello alkalmazáskiszolgáló kell toobe indítani, hogy ezeket a fájlokat használja. hello autoload funkció hello Tomcat Java web Apps Azure-on futó alapul hozzáadott új WAR-fájlt, vagy új fájlok vagy mappák hozzáadása a toohello **webapps** mappa. 

<a name="see-also"></a>

## <a name="see-also"></a>Lásd még:
Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból].

[Application-insights-App-insights-Java-Get-Started](../application-insights/app-insights-java-get-started.md)

<!-- URL List -->

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[telepítheti az alkalmazást tooAzure App Service]: ./web-sites-deploy.md
