---
title: "Az Azure App Service Web Apps Java-alkalmazás hozzáadása"
description: "Ez az oktatóanyag bemutatja, hogyan lap vagy az Azure App Service Web Apps, amely már használatára van konfigurálva a Java-példány alkalmazás hozzáadása."
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
ms.openlocfilehash: c28e7c499ed02b759df580f4b14a971b6aec5b67
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-java-application-to-azure-app-service-web-apps"></a><span data-ttu-id="0ee86-103">Az Azure App Service Web Apps Java-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0ee86-103">Add a Java application to Azure App Service Web Apps</span></span>
<span data-ttu-id="0ee86-104">Amennyiben Ön inicializálta a Java-webalkalmazás a [Azure App Service] [ Azure App Service] dokumentált [Java-webalkalmazás létrehozása az Azure App Service](web-sites-java-get-started.md), feltöltheti az alkalmazást úgy, hogy az a WAR a **webapps** mappa.</span><span class="sxs-lookup"><span data-stu-id="0ee86-104">Once you have initialized your Java web app in [Azure App Service][Azure App Service] as documented at [Create a Java web app in Azure App Service](web-sites-java-get-started.md), you can upload your application by placing your WAR in the **webapps** folder.</span></span>

<span data-ttu-id="0ee86-105">A navigációs elérési útját a **webapps** mappa alapján hogyan lehet beállítani a Web Apps példány eltér.</span><span class="sxs-lookup"><span data-stu-id="0ee86-105">The navigation path to the **webapps** folder differs based on how you set up your Web Apps instance.</span></span>

* <span data-ttu-id="0ee86-106">Ha az Azure piactéren elérési útját használatával állítsa be a webalkalmazás a **webapps** mappa van a képernyőn **d:\home\site\wwwroot\bin\application\_server\webapps**, ahol **alkalmazás\_server** van érvényben a kiszolgáló nevét a Web Apps példányát.</span><span class="sxs-lookup"><span data-stu-id="0ee86-106">If you set up your web app by using the Azure Marketplace, the path to the **webapps** folder is in the form **d:\home\site\wwwroot\bin\application\_server\webapps**, where **application\_server** is the name of the application server in effect for your Web Apps instance.</span></span> 
* <span data-ttu-id="0ee86-107">Ha az Azure-alapú konfigurációs UI, az elérési útját használatával állítsa be a webalkalmazás a **webapps** mappa van a képernyőn **d:\home\site\wwwroot\webapps**.</span><span class="sxs-lookup"><span data-stu-id="0ee86-107">If you set up your web app by using the Azure configuration UI, the path to the **webapps** folder is in the form **d:\home\site\wwwroot\webapps**.</span></span> 

<span data-ttu-id="0ee86-108">Megjegyzés: használható verziókezelő töltse fel az alkalmazás vagy a weblapokat, beleértve a [folyamatos integrációt](app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="0ee86-108">Note that you can use source control to upload your application or web pages, including [continuous integration scenarios](app-service-continuous-deployment.md).</span></span> <span data-ttu-id="0ee86-109">FTP egyben a beállítás a az alkalmazás vagy a weblapok; feltöltése FTP keresztül az alkalmazások telepítésével kapcsolatos további információkért lásd: [telepítse az alkalmazást az Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="0ee86-109">FTP is also an option for uploading your application or web pages; for more information about deploying your applications over FTP, see [Deploy your app to Azure App Service].</span></span>

<span data-ttu-id="0ee86-110">Webalkalmazások Tomcat Megjegyzés: Miután a WAR-fájlt, a feltöltött a **webapps** mappa, a Tomcat alkalmazáskiszolgáló észleli, hogy hozzáadását, és automatikusan betölti azt.</span><span class="sxs-lookup"><span data-stu-id="0ee86-110">Note for Tomcat web apps: Once you've uploaded your WAR file to the **webapps** folder, the Tomcat application server will detect that you've added it and will automatically load it.</span></span> <span data-ttu-id="0ee86-111">Vegye figyelembe, hogy a gyökérkönyvtárba másolt fájlok (eltérő WAR-fájl), a kiszolgáló kell újra kell indítani azokat a fájlokat használja.</span><span class="sxs-lookup"><span data-stu-id="0ee86-111">Note that if you copy files (other than WAR files) to the ROOT directory, the application server will need to be restarted before those files are used.</span></span> <span data-ttu-id="0ee86-112">A Tomcat Java-webalkalmazások Azure-on futó autoload funkcionalitása alapján hozzáadott új WAR-fájlt, vagy új fájlok vagy könyvtárak hozzáadni a **webapps** mappa.</span><span class="sxs-lookup"><span data-stu-id="0ee86-112">The autoload functionality for the Tomcat Java web apps running on Azure is based on a new WAR file being added, or new files or directories added to the **webapps** folder.</span></span> 

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="0ee86-113">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0ee86-113">See Also</span></span>
<span data-ttu-id="0ee86-114">Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="0ee86-114">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

[<span data-ttu-id="0ee86-115">Application-insights-App-insights-Java-Get-Started</span><span class="sxs-lookup"><span data-stu-id="0ee86-115">application-insights-app-insights-java-get-started</span></span>](../application-insights/app-insights-java-get-started.md)

<!-- URL List -->

<span data-ttu-id="0ee86-116">[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="0ee86-116">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
<span data-ttu-id="0ee86-117">[telepítse az alkalmazást az Azure App Service]: ./web-sites-deploy.md</span><span class="sxs-lookup"><span data-stu-id="0ee86-117">[Deploy your app to Azure App Service]: ./web-sites-deploy.md</span></span>
