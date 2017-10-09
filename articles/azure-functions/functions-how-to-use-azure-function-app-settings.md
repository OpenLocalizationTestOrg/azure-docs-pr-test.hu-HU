---
title: "Azure-függvény Alkalmazásbeállítások aaaConfigure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure működni Alkalmazásbeállítások."
services: 
documentationcenter: .net
author: rachelappel
manager: erikre
editor: 
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 04/23/2017
ms.author: glenga
ms.openlocfilehash: 539e203ac449061ef3ceae5e93df3bdbb326e43b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-a-function-app-in-hello-azure-portal"></a>Hogyan toomanage egy függvény alkalmazás hello Azure-portálon 

Az Azure Functions egy függvény app kontextust biztosít a hello végrehajtása az egyéni függvényei. Függvény app viselkedések tooall funkciók egy adott funkció alkalmazás által üzemeltetett alkalmazni. Ez a témakör ismerteti, hogyan tooconfigure és hello Azure-portálon függvény alkalmazások kezelése.

toobegin, nyissa meg toohello [Azure-portálon](http://portal.azure.com) , jelentkezzen be Azure-fiók tooyour. Hello hello portál hello felső keresősávban írja be a függvény alkalmazás hello nevét, és hello listából válassza ki. Miután kiválasztotta a függvény alkalmazás, a következő lap hello jelenik meg:

![A hello Azure-portálon függvény alkalmazás – áttekintés](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

## <a name="manage-app-service-settings"></a>Függvény app beállítások lap

![Függvény app áttekintése a hello Azure-portálon.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

Hello **beállítások** lap, ahol frissítheti a függvény alkalmazás által használt hello funkciók futásidejű verzióját. Akkor is hello állomás használt kulcsok toorestrict HTTP hozzáférés tooall funkciók hello függvény alkalmazás által üzemeltetett kezelhetik.

Funkciók fogyasztás futtató és az App Service csomagokban üzemeltető támogatja. További információkért lásd: [válasszon hello megfelelő service-csomag az Azure Functions](functions-scale.md). Hello fogyasztás tervben jobb kiszámítható funkciók lehetővé teszi a platform-használatát korlátozása úgy, hogy a napi használat kvóta GB-másodperc. Hello napi memóriahasználati kvóta elérésekor hello függvény alkalmazás leállt. Egy függvény app hello költségeik kvóta elérése miatt leállt újból engedélyezhető a hello naponta költségeik kvóta hello létrehozó azonos összefüggésben. Lásd: hello [árképzést ismertető oldalra Azure Functions](http://azure.microsoft.com/pricing/details/functions/) számlázással kapcsolatos részletekért.   

## <a name="platform-features-tab"></a>Platform szolgáltatások lap

![Függvény alkalmazás platform szolgáltatások lapon.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

Függvény alkalmazások futnak, és tartható fenn, hello Azure App Service platformon. A függvény alkalmazások, a hozzáférés toomost hello szolgáltatást az Azure alapvető webszolgáltatási platform is rendelkeznek. Hello **Platform funkciói** lap, ahol a hozzáférést, a függvény alkalmazásokban használható App Service platform hello számos szolgáltatása hello. 

> [!NOTE]
> Nem minden App Service-szolgáltatások érhetők el a hello fogyasztás üzemeltetési terv egy függvény alkalmazás futtatásakor.

a következő hello az App Service-szolgáltatások hello Azure-portálon, amelyek hasznosak a funkciók összpontosít hello a témakör hátralévő része:

+ [App Service-szerkesztő](#editor)
+ [Alkalmazásbeállítások](#settings) 
+ [Console](#console)
+ [Speciális eszközöket (a Kudu)](#kudu)
+ [Telepítési lehetőségek](#deployment)
+ [CORS](#cors)
+ [Hitelesítés](#auth)
+ [API-definíció](#swagger)

További információ az App Service-beállítások segítségével toowork lásd: [Azure App Service beállításainak konfigurálása](../app-service-web/web-sites-configure.md).

### <a name="editor"></a>App Service-szerkesztő

| | |
|-|-|
| ![Függvény alkalmazás az App Service-szerkesztőt.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | App Service-szerkesztő hello egy speciális-portal szerkesztő toomodify JSON konfigurációs fájlokat és a kód fájlok egyaránt használható. Ez a beállítás egy külön böngészőlapon alapvető szerkesztővel indít. Lehetővé teszi a hello Git tárház, futtatása és hibakeresési kód toointegrate, és funkció beállításainak módosításához. A szerkesztő hello alapértelmezett függvény alkalmazás paneljének összehasonlítja a funkciók bővített fejlesztési környezetet biztosít.    |

![hello App Service-szerkesztő](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <a name="settings"></a>Alkalmazásbeállítások

| | |
|-|-|
| ![Alkalmazás függvény Alkalmazásbeállítások.](./media/functions-how-to-use-azure-function-app-settings/function-app-application-settings.png) | App Service hello **Alkalmazásbeállítások** panel, ahol konfigurálhatja és kezelheti a keretrendszer-verziók, távoli hibakeresés, Alkalmazásbeállítások és kapcsolati karakterláncok. A függvény alkalmazás integrációjához át az egyéb Azure és a harmadik féltől származó szolgáltatással, módosíthatja itt ezeket a beállításokat. |

![Az Alkalmazásbeállítások konfigurálása](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-settings.png)

### <a name="console"></a>Konzol

| | |
|-|-|
| ![Függvény app konzol hello Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | hello-portal konzol az ideális fejlesztői eszközét esetén szeretné toointeract függvény alkalmazása hello parancssorból. Általános jellegű parancsok közé tartozik a címtár és a fájl létrehozásának és navigációs, valamint parancsfájlok végrehajtása. |

![Függvény app konzol](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <a name="kudu"></a>Speciális eszközöket (a Kudu)

| | |
|-|-|
| ![Függvény alkalmazás Kudu hello Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | hello speciális eszközök az App Service (más néven a Kudu) hozzáférést biztosítson tooadvanced felügyeleti funkciókat az függvény alkalmazás. Kudu kezelheti az Rendszerinformáció, Alkalmazásbeállítások, környezeti változókat, helyhez kiterjesztések, HTTP-fejlécek és kiszolgáló-változók. Is **Kudu** toohello SCM végpont függvény alkalmazás, például tallózással`https://<myfunctionapp>.scm.azurewebsites.net/` |

![A Kudu konfigurálása](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><a name="deployment">Telepítési lehetőségek

| | |
|-|-|
| ![Függvény alkalmazás központi telepítési beállítások hello Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | Funkciók lehetővé teszi, hogy a helyi számítógépen a funkciókódot fejleszthet. A helyi függvény app projektet tooAzure majd feltöltheti. Ezenkívül tootraditional FTP feltöltés funkciók lehetővé teszi a népszerű folyamatos integrációt megoldások, például a Githubon, VSTS, Dropbox, Bitbucket és mások segítségével függvény alkalmazás telepítése. További információkért lásd: [folyamatos üzembe helyezés az Azure Functions](functions-continuous-deployment.md). FTP- vagy helyi Git segítségével manuálisan tooupload is kell [az üzembe helyezési hitelesítő adatok konfigurálása](functions-continuous-deployment.md#credentials). |


### <a name="cors"></a>A CORS

| | |
|-|-|
| ![Függvény alkalmazás CORS hello Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | tooprevent rosszindulatú kód végrehajtása a szolgáltatások, az App Service-blokkok tooyour függvény alkalmazások meghívja a külső forrásból. Funkciók támogatja az eltérő eredetű erőforrások megosztása (CORS) toolet adhat meg egy "engedélyezett" az engedélyezett eredeteket, amelyből a funkciók fogadhat távoli kérelmek.  |

![Függvény alkalmazás konfigurálása CORS](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <a name="auth"></a>Hitelesítés

| | |
|-|-|
| ![Függvény app hitelesítéshez hello Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | Egy HTTP-eseményindítóval használatakor a funkciók megkövetelheti hívások toofirst hitelesíthető. App Service támogatja az Azure Active Directory-hitelesítés és be kell jelentkezniük a közösségi szolgáltatók, köztük a Facebook, a Microsoft és a Twitter. További részletek az adott hitelesítési szolgáltatókat konfigurál: [Azure App Service hitelesítés áttekintése](../app-service/app-service-authentication-overview.md). |

![Egy függvény alkalmazások hitelesítésének konfigurálása](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <a name="swagger"></a>API-definíció

| | |
|-|-|
| ![Függvény app API swagger-definíciójában hello Azure-portálon](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | Funkciókat támogatja a Swagger tooallow ügyfelek toomore könnyen használni a HTTP-eseményindítókkal aktivált függvényeket. A Swagger API-definíciók létrehozásával kapcsolatos további információkért látogasson el a [Ismerkedés az API Apps, az ASP.NET és az Azure-ban Swagger](../app-service-api/app-service-api-dotnet-get-started.md). Funkciók proxyk toodefine egyetlen API felülete több függvények is használható. További információkért lásd: [használata az Azure Functions proxyk](functions-proxies.md). |

![Függvény App API konfigurálása](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a>Következő lépések

+ [Az Azure App Service-beállítások konfigurálása](../app-service-web/web-sites-configure.md)
+ [Azure Functions – folyamatos üzembe helyezés](functions-continuous-deployment.md)



