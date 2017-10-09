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
# <a name="add-a-java-application-tooazure-app-service-web-apps"></a><span data-ttu-id="fef24-103">A Java-alkalmazás tooAzure App Service Web Apps hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fef24-103">Add a Java application tooAzure App Service Web Apps</span></span>
<span data-ttu-id="fef24-104">Amennyiben Ön inicializálta a Java-webalkalmazás a [Azure App Service] [ Azure App Service] dokumentált [Java-webalkalmazás létrehozása az Azure App Service](web-sites-java-get-started.md), feltöltheti az alkalmazást úgy, hogy a WAR a hello **webapps** mappa.</span><span class="sxs-lookup"><span data-stu-id="fef24-104">Once you have initialized your Java web app in [Azure App Service][Azure App Service] as documented at [Create a Java web app in Azure App Service](web-sites-java-get-started.md), you can upload your application by placing your WAR in hello **webapps** folder.</span></span>

<span data-ttu-id="fef24-105">navigációs elérési toohello hello **webapps** mappa alapján hogyan lehet beállítani a Web Apps példány eltér.</span><span class="sxs-lookup"><span data-stu-id="fef24-105">hello navigation path toohello **webapps** folder differs based on how you set up your Web Apps instance.</span></span>

* <span data-ttu-id="fef24-106">Ha a webalkalmazás beállítása hello Azure piactér használatával, elérési út toohello hello **webapps** mappa hello formában van **d:\home\site\wwwroot\bin\application\_server\webapps**, ahol **alkalmazás\_server** hello név hello alkalmazáskiszolgáló érvényben a Web Apps példányát.</span><span class="sxs-lookup"><span data-stu-id="fef24-106">If you set up your web app by using hello Azure Marketplace, hello path toohello **webapps** folder is in hello form **d:\home\site\wwwroot\bin\application\_server\webapps**, where **application\_server** is hello name of hello application server in effect for your Web Apps instance.</span></span> 
* <span data-ttu-id="fef24-107">Ha a webalkalmazás beállítása hello Azure konfigurációs felhasználói felület használatával, elérési út toohello hello **webapps** mappa hello formában van **d:\home\site\wwwroot\webapps**.</span><span class="sxs-lookup"><span data-stu-id="fef24-107">If you set up your web app by using hello Azure configuration UI, hello path toohello **webapps** folder is in hello form **d:\home\site\wwwroot\webapps**.</span></span> 

<span data-ttu-id="fef24-108">Vegye figyelembe, hogy forrás vezérlő tooupload használhatja, az alkalmazás vagy a weblapokat, beleértve a [folyamatos integrációt](app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="fef24-108">Note that you can use source control tooupload your application or web pages, including [continuous integration scenarios](app-service-continuous-deployment.md).</span></span> <span data-ttu-id="fef24-109">FTP egyben a beállítás a az alkalmazás vagy a weblapok; feltöltése FTP keresztül az alkalmazások telepítésével kapcsolatos további információkért lásd: [telepítheti az alkalmazást tooAzure App Service].</span><span class="sxs-lookup"><span data-stu-id="fef24-109">FTP is also an option for uploading your application or web pages; for more information about deploying your applications over FTP, see [Deploy your app tooAzure App Service].</span></span>

<span data-ttu-id="fef24-110">Webalkalmazások Tomcat Megjegyzés: Miután a WAR-fájl toohello feltöltött **webapps** mappa, hello Tomcat alkalmazáskiszolgáló észleli, hogy hozzáadását, és automatikusan betölti azt.</span><span class="sxs-lookup"><span data-stu-id="fef24-110">Note for Tomcat web apps: Once you've uploaded your WAR file toohello **webapps** folder, hello Tomcat application server will detect that you've added it and will automatically load it.</span></span> <span data-ttu-id="fef24-111">Vegye figyelembe, hogy ha toohello gyökérkönyvtár (eltérő WAR-fájlt) fájlokat másolja, hello alkalmazáskiszolgáló kell toobe indítani, hogy ezeket a fájlokat használja.</span><span class="sxs-lookup"><span data-stu-id="fef24-111">Note that if you copy files (other than WAR files) toohello ROOT directory, hello application server will need toobe restarted before those files are used.</span></span> <span data-ttu-id="fef24-112">hello autoload funkció hello Tomcat Java web Apps Azure-on futó alapul hozzáadott új WAR-fájlt, vagy új fájlok vagy mappák hozzáadása a toohello **webapps** mappa.</span><span class="sxs-lookup"><span data-stu-id="fef24-112">hello autoload functionality for hello Tomcat Java web apps running on Azure is based on a new WAR file being added, or new files or directories added toohello **webapps** folder.</span></span> 

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="fef24-113">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="fef24-113">See Also</span></span>
<span data-ttu-id="fef24-114">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból].</span><span class="sxs-lookup"><span data-stu-id="fef24-114">For more information about using Azure with Java, see hello [Azure Java Developer Center].</span></span>

[<span data-ttu-id="fef24-115">Application-insights-App-insights-Java-Get-Started</span><span class="sxs-lookup"><span data-stu-id="fef24-115">application-insights-app-insights-java-get-started</span></span>](../application-insights/app-insights-java-get-started.md)

<!-- URL List -->

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[telepítheti az alkalmazást tooAzure App Service]: ./web-sites-deploy.md
