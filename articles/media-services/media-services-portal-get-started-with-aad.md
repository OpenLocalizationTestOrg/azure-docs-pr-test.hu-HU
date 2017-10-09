---
title: "aaaGet használatába az Azure AD-alapú hitelesítés hello Azure-portál használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure portál tooaccess Azure Active Directory (Azure AD) hitelesítési tooconsume hello Azure Media Services API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 497ad1806b3fd1262802adefb6e12b65ee9f2d61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-ad-authentication-by-using-hello-azure-portal"></a>Ismerkedés az Azure AD-alapú hitelesítés hello Azure-portál használatával

Ismerje meg, hogyan toouse hello Azure portál tooaccess Azure Active Directory (Azure AD) hitelesítési tooaccess hello Azure Media Services API.

## <a name="prerequisites"></a>Előfeltételek

- Egy Azure-fiók. Ha nincs fiókja, kezdje egy [Azure ingyenes próbát](https://azure.microsoft.com/pricing/free-trial/). 
- Egy Media Services-fiók. További információkért lásd: [Azure Media Services-fiók létrehozása hello Azure-portál használatával](media-services-portal-create-account.md).
- Ellenőrizze, hogy tekintse át a hello [eléréséhez Azure Media Services API-t az Azure AD hitelesítési áttekintés](media-services-use-aad-auth-to-access-ams-api.md). 

Az Azure AD-alapú hitelesítés használatakor az Azure Media Services hitelesítési két lehetőség közül választhat:

- **Felhasználó hitelesítése**. Egy Media Services-erőforrásokkal hello app toointeract használó felhasználó hitelesítéséhez. hello interaktív alkalmazás először kell hello felhasználók hitelesítő adatainak kéréséhez. Például egy engedéllyel rendelkező felhasználók toomonitor kódolási feladatok által használt felügyeleti konzol alkalmazás vagy élő adatfolyam. 
- **Szolgáltatás egyszerű hitelesítési**. A szolgáltatás hitelesítéséhez. Ezt a hitelesítési módszert gyakran használó alkalmazások olyan alkalmazások, amelyeket démon szolgáltatások, a középső rétegbeli szolgáltatások vagy az ütemezett feladatok futtatása: webalkalmazások, függvény alkalmazások, a logic apps, API-k vagy egy mikroszolgáltatási.

> [!IMPORTANT]
> Jelenleg a Media Services hello Azure Access Control service hitelesítési modellt támogatja. Azonban a 2018. június 1 elavulttá válik hozzáférés-vezérlés engedélyt. Azt javasoljuk, hogy az áttelepített toohello az Azure AD hitelesítési modell lehető legrövidebb időn belül.

## <a name="select-hello-authentication-method"></a>Hello hitelesítési módszer kiválasztása

1. A hello [Azure-portálon](https://portal.azure.com/), válassza ki a Media Services-fiókját.
2. Adja meg, hogyan tooconnect toohello Media Services API.

    ![Válassza ki a csatlakozási módszer lapja](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a>Felhasználóhitelesítés

tooconnect toohello Media Services API használatával hello felhasználói hitelesítést választja, hello ügyfélalkalmazás kell toorequest az Azure AD jogkivonatban hello rendelkezik a következő paramétereket:  

* Azure AD-bérlő végpont
* A Media Services erőforrás URI azonosítója
* A Media Services (natív) alkalmazás ügyfél-azonosító 
* Media Services (natív) alkalmazás átirányítási URI 
* Erőforrás URI azonosítója a többi Media Services szolgáltatásokhoz

Ezek a paraméterek a hello kaphat hello értékek **Media Services API és a felhasználói hitelesítés** lap. 

![Csatlakozás a felhasználói hitelesítés lap](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

Ha toohello Media Services API hello Media Services Microsoft .NET SDK használatával, a hello szükséges értékei elérhető tooyou hello SDK részeként. További információkért lásd: [használja az Azure AD hitelesítési tooaccess hello Azure Media Services API a .NET](media-services-dotnet-get-started-with-aad.md).

Ha ügyfél-hello Media Services .NET SDK nem használ, manuálisan kell létrehoznia az Azure AD-jogkivonatkérelem taglaltak hello paraméterek használatával. További információkért lásd: [hogyan toouse hello Azure AD Authentication Library tooget hello Azure AD-jogkivonat](../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="service-principal-authentication"></a>Egyszerű szolgáltatásnév hitelesítése

tooconnect toohello Media Services API használatával hello szolgáltatás egyszerű beállítás, a középső rétegbeli (webes API-t vagy webalkalmazás) kell toorequest az Azure AD jogkivonatban hello rendelkezik a következő paramétereket:  

* Azure AD-bérlő végpont
* A Media Services erőforrás URI azonosítója 
* Erőforrás URI azonosítója a többi Media Services szolgáltatásokhoz
* Az Azure AD alkalmazás értékek: hello **ügyfél-azonosító** és **ügyfélkulcs**

Ezek a paraméterek a hello kaphat hello értékek **tooMedia szolgáltatások API kapcsolattartásnak szolgáltatás egyszerű** lap. Használja az oldal toocreate egy új Azure AD-alkalmazást vagy tooselect egy meglévőt. Miután kiválasztotta a hello Azure AD-alkalmazáshoz, hello ügyfél-azonosítót (Alkalmazásazonosító), és hello ügyfél titkos (kulcs) értékek generálásához. 

![Csatlakozás szolgáltatás egyszerű lap](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

Ha hello **egyszerű** panel nyílik meg, a kiválasztott hello első Azure AD alkalmazás, amely megfelel a következő feltételek hello:

- Egy regisztrált Azure AD-alkalmazást.
- Hello fiók közreműködői vagy Owner Role-Based hozzáférés-vezérlés engedéllyel rendelkezik.

Miután hoz létre, vagy válasszon ki egy Azure AD-alkalmazást, hozzon létre és másolja át egy ügyfélkulcsot (kulcs), és hello ügyfél-azonosító (Alkalmazásazonosító). hello ügyfél titkos kulcs és az ügyfél-azonosító szükséges tooget hello hozzáférési jogkivonat ebben a forgatókönyvben is.

Ha nem rendelkezik engedélyekkel toocreate Azure AD alkalmazásaiban abban a tartományban, hello Azure AD alkalmazás vezérlők hello panelről nem láthatók, és megjelenik egy figyelmeztető üzenet.

Ha toohello Media Services API hello Media Services .NET SDK használatával, lásd: [használja az Azure AD hitelesítési tooaccess hello Azure Media Services API a .NET](media-services-dotnet-get-started-with-aad.md).

Ha nem használ hello Media Services .NET ügyfél SDK, manuálisan kell létrehoznia egy Azure AD-jogkivonatkérelem, azt a korábbiakban említettük hello paraméterek használatával. További információkért lásd: [hogyan toouse hello Azure AD Authentication Library tooget hello Azure AD-jogkivonat](../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="get-hello-client-id-and-client-secret"></a>Hello ügyfél azonosító és az ügyfél titkos kulcs beszerzése

Miután kiválasztott egy meglévő Azure AD-alkalmazáshoz vagy select hello beállítás toocreate egy új, a következő gombok hello jelennek meg:

![Engedélyek és kezelés alkalmazás gomb kezelése](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

tooopen hello Azure AD-alkalmazás paneljén kattintson **alkalmazás kezeléséhez**. A hello **alkalmazás kezeléséhez** panelen kaphat a hello alkalmazás ügyfél-Azonosítóját (Alkalmazásazonosító). a titkos ügyfélkulcsot (kulcs), jelölje be toogenerate **kulcsok**.

![Alkalmazás panel kulcsok beállítás kezelése](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-hello-application"></a>Engedélyek és hello alkalmazás kezelése

Miután kiválasztotta a hello Azure AD-alkalmazást, kezelheti a hello alkalmazás és az engedélyek. tooset mentése az Azure AD alkalmazás tooaccess más alkalmazásokat, kattintson a **kezeli az engedélyeket**. A felügyeleti feladatokat, például megváltoztatni a kulcsok és a válasz URL-címek vagy tooedit hello alkalmazás jegyzékfájlja, kattintson a **alkalmazás kezeléséhez**.

### <a name="edit-hello-apps-settings-or-manifest"></a>Hello alkalmazás beállításait vagy manifest

tooedit hello alkalmazás beállításai vagy jegyzékfájl, kattintson a **alkalmazás kezeléséhez**.

![Alkalmazások lap kezelése](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a>Következő lépések

Ismerkedés a [fájlok feltöltése a tooyour fiók](media-services-portal-upload-files.md).
