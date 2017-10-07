---
title: "aaaDeployment – gyakori kérdések az Azure web Apps |} Microsoft Docs"
description: "Az Azure App Service Web Apps szolgáltatás hello telepítésére vonatkozó kérdések válaszok toofrequently beolvasása."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 566e1d7028e678f9679200f436118d27dfb07079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-faqs-for-web-apps-in-azure"></a>Központi telepítés – gyakori kérdések az Azure Web Apps

Ez a cikk rendelkezik kérdések (GYIK) kapcsolatos telepítési problémák esetére hello a válaszok toofrequently [az Azure App Service Web Apps szolgáltatásának](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-am-just-getting-started-with-app-service-web-apps-how-do-i-publish-my-code"></a>I vagyok csak Ismerkedés az App Service web Apps alkalmazások. Hogyan tehetők közzé a kódot?

Az alábbiak közzétételéhez a webes alkalmazás kódja:

*   Telepítse a Visual Studio használatával. Ha hello Visual Studio megoldás, kattintson a jobb gombbal a hello webalkalmazás projekthez, és válassza **közzététel**.
*   Az FTP-ügyfél központi telepítését. Hello Azure-portálon, letöltési hello hello webalkalmazás profil közzé, amelyet toodeploy a kódot. Ezt követően töltse fel a hello fájlok too\site\wwwroot hello segítségével azonos közzététele profil FTP hitelesítő adatokat.

További információkért lásd: [telepítheti az alkalmazást tooApp szolgáltatás](web-sites-deploy.md).

## <a name="i-see-an-error-message-when-i-try-toodeploy-from-visual-studio-how-do-i-resolve-this"></a>Egy hibaüzenet jelenik meg a Visual Studio eszközből toodeploy látható. Hogyan lehet elhárítani ezt?

Ha hello a következő üzenet jelenik meg, előfordulhat, hogy használni hello SDK régebbi verziója: "hiba történt az erőforrás"YourResourceName"erőforráscsoport"YourResourceGroup"központi telepítése során: MissingRegistrationForLocation: hello előfizetés nincs regisztrálva hello erőforrás írja be a "összetevők" hello helyen "USA középső RÉGIÓJA". Regisztrálja újra a szolgáltató rendelés toohave hozzáférés toothis helyen." 

tooresolve ezt a hibát, a frissítési toohello [SDK legújabb](https://azure.microsoft.com/downloads/). Ha ez az üzenet megjelenik, és meg kell hello SDK legújabb, a támogatási kérelem elküldéséhez.

## <a name="how-do-i-deploy-an-aspnet-application-from-visual-studio-tooapp-service"></a>Hogyan telepítsék központilag az ASP.NET alkalmazás, a Visual Studio tooApp szolgáltatás?
<a id="deployasp"></a>

hello oktatóanyag [az első ASP.NET-webalkalmazás létrehozása az Azure-ban öt perc múlva](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-get-started/) bemutatja, hogyan toodeploy az ASP.NET webalkalmazás-alkalmazás tooa webalkalmazást az App Service szolgáltatásban a Visual Studio 2015 használatával.

## <a name="what-are-hello-different-types-of-deployment-credentials"></a>Mik azok a különböző típusú hello üzembe helyezési hitelesítő adatokat?

App Service kétféle típusú hitelesítő adatok helyi Git-telepítés és az FTP/S központi telepítés támogatja. További információ tooconfigure üzembe helyezési hitelesítő adatokat, lásd: [telepítési hitelesítő adatok beállítása az App Service](app-service-deployment-credentials.md).

## <a name="what-is-hello-file-or-directory-structure-of-my-app-service-web-app"></a>Mi az az App Service webalkalmazás fájl vagy könyvtár struktúra hello?

App Service-alkalmazás hello fájlstruktúrájával kapcsolatos információkért lásd: [struktúra fájlt az Azure-ban](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure).

## <a name="how-do-i-resolve-ftp-error-550---there-is-not-enough-space-on-hello-disk-when-i-try-tooftp-my-files"></a>Hogyan hárítható "FTP hiba 550 - ott nincs elég hely a lemezen hello" Amikor megpróbálok tooFTP a fájlokat?

Ez az üzenet jelenik meg, akkor valószínű, hogy fut a lemezkvóta hello service-csomagot a webalkalmazás. Szükség lehet tooscale tooa magasabb szolgáltatásszint alapján a lemezterület-szükséglet fel. Tervek és erőforrás-korlátozások árazással kapcsolatos további információkért lásd: [App Service díjszabás](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="how-do-i-set-up-continuous-deployment-for-my-app-service-web-app"></a>Hogyan állíthatom be a folyamatos üzembe helyezést az App Service-webalkalmazás?

Beállíthat több erőforrást, beleértve a Visual Studio Team Services, OneDrive, GitHub, Bitbucket, Dropbox és egyéb Git adattárak folyamatos üzembe helyezést. A beállítások hello portálon érhetők el. [Folyamatos üzembe helyezés tooApp szolgáltatás](app-service-continuous-deployment.md) egy hasznos oktatóanyag, amely azt ismerteti, hogyan tooset folyamatos központi telepítése szükséges.

## <a name="how-do-i-troubleshoot-issues-with-continuous-deployment-from-github-and-bitbucket"></a>Hogyan hibaelhárítása a folyamatos üzembe helyezés a Githubról, majd Bitbucket problémái?

Folyamatos üzembe helyezés, GitHub vagy Bitbucket problémákat vizsgál, lásd: [vizsgálja a folyamatos üzembe helyezés](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment).

## <a name="i-cant-ftp-toomy-site-and-publish-my-code-how-do-i-resolve-this"></a>Nem lehet FTP-hely toomy és közzététele a kódot. Hogyan lehet elhárítani ezt?

tooresolve FTP problémák:

1. Győződjön meg arról, hogy felvételekor hello megfelelő gazdagép nevét és hitelesítő adatait. Különböző típusú hitelesítő adatokkal kapcsolatos részletes információkat, és hogyan toouse, [üzembe helyezési hitelesítő adatok](https://github.com/projectkudu/kudu/wiki/Deployment-credentials).
2. Győződjön meg arról, hogy hello FTP portot nem blokkolja tűzfal. hello portnak ezeket a beállításokat kell rendelkeznie:
    * FTP-vezérlőkapcsolati port: 21
    * FTP-adatok csatlakozási portja: 989, 10001-10300

## <a name="how-do-i-publish-my-code-tooapp-service"></a>Hogyan tehetők közzé a kód tooApp szolgáltatás?

hello Azure gyors üzembe helyezési hello telepítési verem és a választott módszerrel használatával az alkalmazást telepítené kialakított toohelp. toouse hello gyors üzembe helyezés, a hello Azure-portálon lépjen túl**beállítások** > **alkalmazástelepítés**.

## <a name="why-does-my-app-sometimes-restart-after-deployment-tooapp-service"></a>Miért nem saját alkalmazás néha újraindítása után telepítési tooApp szolgáltatás?

toolearn hello körülmények között, amely alatt az alkalmazás központi telepítése eredményezhet, újraindítás kapcsolatban lásd: [futásidejű problémák és telepítési](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues#deployments-and-web-app-restarts"). Mivel hello a cikk ismerteti, telepíti az App Service fájlok toohello wwwroot mappát. Soha ne közvetlenül újraindul az alkalmazás.

## <a name="how-do-i-integrate-visual-studio-team-services-code-with-app-service"></a>Hogyan integrálja a Visual Studio Team Services kódot az App Service?

A Visual Studio Team Services használata a folyamatos üzembe helyezés két lehetőség közül választhat:

*   Használjon egy Git-projektet. Csatlakozás az App Service adott tárház hello központi telepítési beállítások használatával.
*   A Team Foundation verzió vezérlő (TFVC) projekt használja. Az App Service hello build ügynök segítségével telepítheti.

Folyamatos kód telepítésének mindkét ezeket a beállításokat attól függ, meglévő fejlesztői munkafolyamatok és a bejelentkezési eljárásokat. További információkért lásd: ezek a cikkek: 

*   [Valósítja meg az alkalmazás tooan Azure webhelyén, a folyamatos üzembe helyezés](https://www.visualstudio.com/docs/release/examples/azure/azure-web-apps-from-build-and-release-hubs)
*   [Azt is telepíthető tooa webes alkalmazás a Visual Studio Team Services-fiók beállítása](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)

## <a name="how-do-i-use-ftp-or-ftps-toodeploy-my-app-tooapp-service"></a>Miként használható az FTP- vagy ftps-t toodeploy my app tooApp szolgáltatás?

A webes alkalmazás tooApp szolgáltatás, lásd: FTP vagy ftps-t toodeploy használatáról [az alkalmazás tooApp szolgáltatás telepítését az FTP/S](app-service-deploy-ftp.md).
