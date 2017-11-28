---
title: "Az adal-t használó Android alkalmazások közötti SSO engedélyezése |} Microsoft Docs"
description: "Hogyan szolgáltatásait is használni az ADAL SDK való egyszeri bejelentkezés engedélyezése az alkalmazások között. "
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 04/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 9c7e959530a836fe5ddf74708363a636c39b3cc6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a><span data-ttu-id="3e1b1-103">Az adal-t használó Android alkalmazások közötti SSO engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3e1b1-103">How to enable cross-app SSO on Android using ADAL</span></span>
<span data-ttu-id="3e1b1-104">Megadása, hogy a felhasználók csak egyszer adja meg a hitelesítő adataikat, és ezen hitelesítő adatok automatikusan rendelkezésére kell közötti használathoz egyszeri bejelentkezés (SSO) alkalmazások már várt ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-104">Providing Single Sign-On (SSO) so that users only need to enter their credentials once and have those credentials automatically work across applications is now expected by customers.</span></span> <span data-ttu-id="3e1b1-105">Nehézsége kis képernyőjű, gyakran alkalommal együtt egy további tényezőt (2FA) például telefonhívást vagy egy fogva kódot, a felhasználónév és jelszó megadása ehhez a termékhez egynél többször van gyors kapcsolatos, ha a felhasználó eredményez.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-105">The difficulty in entering their username and password on a small screen, often times combined with an additional factor (2FA) like a phone call or a texted code, results in quick dissatisfaction if a user has to do this more than one time for your product.</span></span>

<span data-ttu-id="3e1b1-106">Emellett ha egy identitás-platform, amely más alkalmazások is használhatnak, például a Microsoft Accounts vagy az Office365 munkahelyi fiókkal, elvárt, hogy a hitelesítő adatokat a felhasználható a szállító függetlenül az alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-106">In addition, if you apply an identity platform that other applications may use such as Microsoft Accounts or a work account from Office365, customers expect that those credentials to be available to use across all their applications no matter the vendor.</span></span>

<span data-ttu-id="3e1b1-107">A Microsoft Identity platform, a Microsoft Identity SDK-k, valamint a rögzített munkahelyi nem meg, és lehetőséget nyújt az ügyfelei egyszeri Bejelentkezéssel delight, vagy az alkalmazások saját suite belül vagy, például a broker funkció és a hitelesítő az alkalmazások, a teljes eszköz.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-107">The Microsoft Identity platform, along with our Microsoft Identity SDKs, does all this hard work for you and gives you the ability to delight your customers with SSO either within your own suite of applications or, as with our broker capability and Authenticator applications, across the entire device.</span></span>

<span data-ttu-id="3e1b1-108">Ez a forgatókönyv jelzi az SDK az alkalmazás számára az előnyök nyújtsanak az ügyfeleinek belül konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-108">This walkthrough will tell you how to configure our SDK within your application to provide this benefit to your customers.</span></span>

<span data-ttu-id="3e1b1-109">Ez a forgatókönyv a következőkre vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-109">This walkthrough applies to:</span></span>

* <span data-ttu-id="3e1b1-110">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3e1b1-110">Azure Active Directory</span></span>
* <span data-ttu-id="3e1b1-111">Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="3e1b1-111">Azure Active Directory B2C</span></span>
* <span data-ttu-id="3e1b1-112">Az Azure Active Directory B2B</span><span class="sxs-lookup"><span data-stu-id="3e1b1-112">Azure Active Directory B2B</span></span>
* <span data-ttu-id="3e1b1-113">Azure Active Directory feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="3e1b1-113">Azure Active Directory Conditional Access</span></span>

<span data-ttu-id="3e1b1-114">Az előző lépéseknél tudja hogyan [kiépíteni a régi portál alkalmazásokat az Azure Active Directory](active-directory-how-to-integrate.md) és az alkalmazásba integrálva a [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android).</span><span class="sxs-lookup"><span data-stu-id="3e1b1-114">The document preceding assumes you know how to [provision applications in the legacy portal for Azure Active Directory](active-directory-how-to-integrate.md) and integrated your application with the [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android).</span></span>

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a><span data-ttu-id="3e1b1-115">A Microsoft Identity Platform SSO jellemzői</span><span class="sxs-lookup"><span data-stu-id="3e1b1-115">SSO Concepts in the Microsoft Identity Platform</span></span>
### <a name="microsoft-identity-brokers"></a><span data-ttu-id="3e1b1-116">Microsoft – identitáskezelési brókerek</span><span class="sxs-lookup"><span data-stu-id="3e1b1-116">Microsoft Identity Brokers</span></span>
<span data-ttu-id="3e1b1-117">A Microsoft lehetővé teszi a minden mobileszköz platform, amely lehetővé teszi az adatközponthíd-képzés hitelesítő adatokat a különböző alkalmazások különböző szállítóktól származó, és lehetővé teszi, hogy honnan fiók hitelesítő adatainak érvényesítéséhez egy biztonságos helyen igénylő különleges továbbfejlesztett funkciók.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-117">Microsoft provides applications for every mobile platform that allow for the bridging of credentials across applications from different vendors and allows for special enhanced features that require a single secure place from where to validate credentials.</span></span> <span data-ttu-id="3e1b1-118">Ezek nevezzük **brókerek**.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-118">We call these **brokers**.</span></span> <span data-ttu-id="3e1b1-119">IOS és Android rendszeren ezek szolgáltatáson keresztül letölthető alkalmazás, hogy az ügyfelek telepítése függetlenül vagy lehet leküldeni az eszközre a vállalati, akár az összeset az eszköz az alkalmazott felügyelő által.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-119">On iOS and Android these are provided through downloadable applications that customers either install independently or can be pushed to the device by a company who manages some or all of the device for their employee.</span></span> <span data-ttu-id="3e1b1-120">Ezek a brókerek kezelése biztonsági csak az egyes alkalmazások vagy a rendszergazdák kívánják alapján a teljes eszköz támogatja.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-120">These brokers support managing security just for some applications or the entire device based on what IT Administrators desire.</span></span> <span data-ttu-id="3e1b1-121">Ez a funkció a Windows rendszerben egy fiók kiválasztásakor az operációs rendszer, a Webeshitelesítés-szervező műszaki ismert részét biztosítja.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-121">In Windows, this functionality is provided by an account chooser built in to the operating system, known technically as the Web Authentication Broker.</span></span>

<span data-ttu-id="3e1b1-122">További információt használjuk, a brókerek és hogyan az ügyfelek előfordulhat, hogy lássák a bejelentkezési folyamat a Microsoft Identity platform olvasni.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-122">For more information on how we use these brokers and how your customers might see them in their login flow for the Microsoft Identity platform read on.</span></span>

### <a name="patterns-for-logging-in-on-mobile-devices"></a><span data-ttu-id="3e1b1-123">Bejelentkezés mobileszközök minták</span><span class="sxs-lookup"><span data-stu-id="3e1b1-123">Patterns for logging in on mobile devices</span></span>
<span data-ttu-id="3e1b1-124">Eszközök felhasználó hitelesítő adatai hozzáférést a Microsoft Identity platform két alapvető mintázatokból kövesse:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-124">Access to credentials on devices follow two basic patterns for the Microsoft Identity platform:</span></span>

* <span data-ttu-id="3e1b1-125">Nem közvetített támogatott bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="3e1b1-125">Non-broker assisted logins</span></span>
* <span data-ttu-id="3e1b1-126">Támogatott bejelentkezések replikaszervező</span><span class="sxs-lookup"><span data-stu-id="3e1b1-126">Broker assisted logins</span></span>

#### <a name="non-broker-assisted-logins"></a><span data-ttu-id="3e1b1-127">Nem közvetített támogatott bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="3e1b1-127">Non-broker assisted logins</span></span>
<span data-ttu-id="3e1b1-128">Nem közvetített támogatott bejelentkezések olyan bejelentkezési élményt, amely az alkalmazás együtt, beágyazottan történik, és használja a helyi tárolót, az eszközön az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-128">Non-broker assisted logins are login experiences that happen inline with the application and use the local storage on the device for that application.</span></span> <span data-ttu-id="3e1b1-129">Ez a tároló lehet osztani alkalmazásra, de a hitelesítő adatok szorosan kötött az alkalmazás vagy csomag alkalmazásaikat, hogy a hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-129">This storage may be shared across applications but the credentials are tightly bound to the app or suite of apps using that credential.</span></span> <span data-ttu-id="3e1b1-130">Valószínűleg tapasztal Ez sok mobilalkalmazások a felhasználónév és jelszó belülről beírásakor.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-130">You've most likely experienced this in many mobile applications when you enter a username and password within the application itself.</span></span>

<span data-ttu-id="3e1b1-131">Ezek bejelentkezések rendelkezik a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-131">These logins have the following benefits:</span></span>

* <span data-ttu-id="3e1b1-132">Felhasználói élmény létezik-e teljes mértékben az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-132">User experience exists entirely within the application.</span></span>
* <span data-ttu-id="3e1b1-133">Hitelesítő adatok ugyanazt a tanúsítványt, biztosít egy egyszeri bejelentkezést az alkalmazások a csomag által aláírt alkalmazások között megosztható legyen.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-133">Credentials can be shared across applications that are signed by the same certificate, providing a single sign-on experience to your suite of applications.</span></span>
* <span data-ttu-id="3e1b1-134">Naplózás a felhasználói élmény körül vezérlő előtt és után bejelentkezhet az alkalmazásba valósul meg.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-134">Control around the experience of logging in is provided to the application before and after sign-in.</span></span>

<span data-ttu-id="3e1b1-135">Ezek bejelentkezések a következő hátrányokkal rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-135">These logins have the following drawbacks:</span></span>

* <span data-ttu-id="3e1b1-136">A felhasználó csak azokat az alkalmazás van-e beállítva a Microsoft Identities között a Microsoft Identity használó összes alkalmazások között nem számára az egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-136">User cannot experience single-sign on across all apps that use a Microsoft Identity, only across those Microsoft Identities that your application has configured.</span></span>
* <span data-ttu-id="3e1b1-137">Az alkalmazás az Intune-ban termékcsomagot használja, illetve a Speciális üzleti szolgáltatásokat, mint a feltételes hozzáférés nem használható.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-137">Your application cannot be used with more advanced business features such as Conditional Access or use the InTune suite of products.</span></span>
* <span data-ttu-id="3e1b1-138">Az alkalmazás nem támogatja a Tanúsítványalapú hitelesítés üzleti felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-138">Your application can't support certificate-based authentication for business users.</span></span>

<span data-ttu-id="3e1b1-139">Íme egy a Microsoft Identity SDK-k engedélyezése az egyszeri bejelentkezés az alkalmazások a megosztott tárolóhellyel rendelkező működése ábrázolása:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-139">Here is a representation of how the Microsoft Identity SDKs work with the shared storage of your applications to enable SSO:</span></span>

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a><span data-ttu-id="3e1b1-140">Támogatott bejelentkezések replikaszervező</span><span class="sxs-lookup"><span data-stu-id="3e1b1-140">Broker assisted logins</span></span>
<span data-ttu-id="3e1b1-141">Bejelentkezések Broker támogatású olyan bejelentkezési élmény a broker alkalmazáson belül és a tárolás és a biztonsági közvetítő segítségével fájlmegosztási hitelesítő adatokat, amelyek érvényesek a Microsoft Identity platform az eszközön lévő összes alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-141">Broker-assisted logins are login experiences that occur within the broker application and use the storage and security of the broker to share credentials across all applications on the device that apply the Microsoft Identity platform.</span></span> <span data-ttu-id="3e1b1-142">Ez azt jelenti, hogy az alkalmazások támaszkodnak az átvitelszervező felhasználók bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-142">This means that your applications rely on the broker to sign users in.</span></span> <span data-ttu-id="3e1b1-143">IOS és Android rendszeren a brókerek szolgáltatáson keresztül letölthető alkalmazás, hogy az ügyfelek telepítése függetlenül vagy lehet leküldeni az eszközre a felhasználó az eszközt felügyelő által.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-143">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed to the device by a company who manages the device for their user.</span></span> <span data-ttu-id="3e1b1-144">Ez az alkalmazástípus példa: a Microsoft Authenticator alkalmazás IOS rendszerű eszközökön.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-144">An example of this type of application is the Microsoft Authenticator application on iOS.</span></span> <span data-ttu-id="3e1b1-145">Ez a funkció a Windows egy fiók kiválasztásakor az operációs rendszer, a Webeshitelesítés-szervező műszaki ismert részét biztosítja.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-145">In Windows this functionality is provided by an account chooser built in to the operating system, known technically as the Web Authentication Broker.</span></span>
<span data-ttu-id="3e1b1-146">A felhasználói élmény platformonként változó, és egyes esetekben zavart okozhatnak a felhasználók számára nem esetén megfelelően.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-146">The experience varies by platform and can sometimes be disruptive to users if not managed correctly.</span></span> <span data-ttu-id="3e1b1-147">Ismeri valószínűleg legtöbb ebben a mintában Ha a Facebook alkalmazást telepítették, és Facebook csatlakozni egy másik alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-147">You're probably most familiar with this pattern if you have the Facebook application installed and use Facebook Connect from another application.</span></span> <span data-ttu-id="3e1b1-148">A Microsoft Identity platform ugyanilyen mintájú használja.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-148">The Microsoft Identity platform uses the same pattern.</span></span>

<span data-ttu-id="3e1b1-149">Ennek eredménye "átmenet" IOS animáció, ahol az alkalmazás a rendszer elküldi a háttérben, miközben a Microsoft Authenticator alkalmazás elérhető lesz, az az előtérben, válassza ki, melyik fiókot szeretné a bejelentkezéshez a felhasználónak.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-149">For iOS this leads to a "transition" animation where your application is sent to the background while the Microsoft Authenticator applications comes to the foreground for the user to select which account they would like to sign in with.</span></span>  

<span data-ttu-id="3e1b1-150">Android és Windows a fiók kiválasztásakor látható az alkalmazás, amely kevésbé zavaró, a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-150">For Android and Windows the account chooser is displayed on top of your application which is less disruptive to the user.</span></span>

#### <a name="how-the-broker-gets-invoked"></a><span data-ttu-id="3e1b1-151">Hogyan a broker meghívott beolvasása</span><span class="sxs-lookup"><span data-stu-id="3e1b1-151">How the broker gets invoked</span></span>
<span data-ttu-id="3e1b1-152">Kompatibilis ügynök telepítve van az eszközön, például a Microsoft Authenticator alkalmazást, ha a Microsoft Identity SDK-k automatikusan elvégzi a munkát meg broker hívásának, amikor a felhasználó azt jelzi, hogy kívánják-e jelentkezni a Microsoft bármely fiók használatával Identitásplatformmal.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-152">If a compatible broker is installed on the device, like the Microsoft Authenticator application, the Microsoft Identity SDKs will automatically do the work of invoking the broker for you when a user indicates they wish to log in using any account from the Microsoft Identity platform.</span></span> <span data-ttu-id="3e1b1-153">Ez a fiók lehet személyes Microsoft-Account, munkahelyi vagy iskolai fiókját, vagy egy fiókot, amely megadja és az Azure-ban B2C és B2B termékeink állomás.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-153">This account could be a personal Microsoft Account, a work or school account, or an account that you provide and host in Azure using our B2C and B2B products.</span></span> 
 
 #### <a name="how-we-ensure-the-application-is-valid"></a><span data-ttu-id="3e1b1-154">Hogyan gondoskodunk róla, hogy az alkalmazás érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-154">How we ensure the application is valid</span></span>
 
 <span data-ttu-id="3e1b1-155">Egy alkalmazás hívás a broker elengedhetetlen a átvitelszervező nyújtunk biztonsági identitásának érdekében bejelentkezések támogatott.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-155">The need to ensure the identity of an application call the broker is crucial to the security we provide in broker assisted logins.</span></span> <span data-ttu-id="3e1b1-156">IOS és Android nem kényszeríti az egyedi azonosítók, amelyek csak egy adott alkalmazáshoz tartozó érvényes, így rosszindulatú alkalmazások előfordulhat, hogy "hamis" jogos alkalmazás azonosítója és a jogkivonatok azt jelentette, hogy a jogos alkalmazás fogadni.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-156">Neither iOS nor Android enforces unique identifiers that are valid only for a given application, so malicious applications may "spoof" a legitimate application's identifier and receive the tokens meant for the legitimate application.</span></span> <span data-ttu-id="3e1b1-157">Gondoskodjon arról, hogy mindig a megfelelő alkalmazás futásidőben tájékoztatjuk, egy egyéni redirectURI megadására, amikor az alkalmazás regisztrálása a Microsoft developer kérjük.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-157">To ensure we are always communicating with the right application at runtime, we ask the developer to provide a custom redirectURI when registering their application with Microsoft.</span></span> <span data-ttu-id="3e1b1-158">**Az alábbiakban tárgyalt hogyan a fejlesztők kézműipari az átirányítási URI-t kell.**</span><span class="sxs-lookup"><span data-stu-id="3e1b1-158">**How developers should craft this redirect URI is discussed in detail below.**</span></span> <span data-ttu-id="3e1b1-159">Az egyéni redirectURI tartalmazza az alkalmazás tanúsítvány-ujjlenyomata, és a Google Play áruház biztosítja az alkalmazás egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-159">This custom redirectURI contains the certificate thumbprint of the application and is ensured to be unique to the application by the Google Play Store.</span></span> <span data-ttu-id="3e1b1-160">Amikor egy alkalmazás meghívja a broker, az átvitelszervező megkérdezi, hogy biztosítsa a tanúsítvány ujjlenyomatát a broker nevezett Android operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-160">When an application calls the broker, the broker asks the Android operating system to provide it with the certificate thumbprint that called the broker.</span></span> <span data-ttu-id="3e1b1-161">A broker biztosít a tanúsítvány ujjlenyomata a Microsoft az azonosítási rendszer hívásában.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-161">The broker provides this certificate thumbprint to Microsoft in the call to our identity system.</span></span> <span data-ttu-id="3e1b1-162">Ha az alkalmazás tanúsítvány-ujjlenyomata nem egyezik meg a tanúsítvány ujjlenyomatát a fejlesztő regisztráció során megadott számunkra, azt fogja megtagadja a hozzáférést a jogkivonatok az erőforráshoz, az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-162">If the certificate thumbprint of the application does not match the certificate thumbprint provided to us by the developer during registration, we will deny access to the tokens for the resource the application is requesting.</span></span> <span data-ttu-id="3e1b1-163">Ez az ellenőrzés biztosítja, hogy csak az alkalmazás által a fejlesztői jogkivonatokat kap.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-163">This check ensures that only the application registered by the developer receives tokens.</span></span>

<span data-ttu-id="3e1b1-164">**A fejlesztői lehetősége van, ha a Microsoft Identity SDK meghívja az átvitelszervező, vagy a nem közvetített támogatott folyamat használja.**</span><span class="sxs-lookup"><span data-stu-id="3e1b1-164">**The developer has the choice of if the Microsoft Identity SDK calls the broker or uses the non-broker assisted flow.**</span></span> <span data-ttu-id="3e1b1-165">Azonban ha a fejlesztői használatát választja, nem a broker támogatású folyamata megszakad a SSO használatának előnye, hogy a felhasználó már valószínűleg az eszköz-felhasználó hitelesítő adatait, és megakadályozza, hogy az alkalmazásokban történő használatát, az üzleti szolgáltatásokat biztosít a Microsoft a ügyfelek, például a feltételes hozzáférés, a Intune kezelési képességek és a tanúsítvány alapú hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-165">However if the developer chooses not to use the broker-assisted flow they lose the benefit of using SSO credentials that the user may have already added on the device and prevents their application from being used with business features Microsoft provides its customers such as Conditional Access, Intune Management capabilities, and certificate-based authentication.</span></span>

<span data-ttu-id="3e1b1-166">Ezek bejelentkezések rendelkezik a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-166">These logins have the following benefits:</span></span>

* <span data-ttu-id="3e1b1-167">Felhasználói SSO észlel, függetlenül attól, a szállító az alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-167">User experiences SSO across all their applications no matter the vendor.</span></span>
* <span data-ttu-id="3e1b1-168">Az alkalmazás használhatja a Speciális üzleti szolgáltatásokat, mint a feltételes hozzáférés, vagy az Intune-ban termékcsomagot használja.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-168">Your application can use more advanced business features such as Conditional Access or use the InTune suite of products.</span></span>
* <span data-ttu-id="3e1b1-169">Az alkalmazás tanúsítvány alapú hitelesítést is támogatja az üzleti felhasználók.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-169">Your application can support certificate-based authentication for business users.</span></span>
* <span data-ttu-id="3e1b1-170">A broker alkalmazás további biztonsági algoritmusokkal és a titkosítás sokkal biztonságosabb bejelentkezési élmény a kérelem és a felhasználói identitás ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-170">Much more secure sign-in experience as the identity of the application and the user are verified by the broker application with additional security algorithms and encryption.</span></span>

<span data-ttu-id="3e1b1-171">Ezek bejelentkezések a következő hátrányokkal rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-171">These logins have the following drawbacks:</span></span>

* <span data-ttu-id="3e1b1-172">IOS-ban a felhasználó van állapotra váltott ki az alkalmazás élmény közben a hitelesítő adatok közül választ.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-172">In iOS the user is transitioned out of your application's experience while credentials are chosen.</span></span>
* <span data-ttu-id="3e1b1-173">Képes kezelni az ügyfeleknek az alkalmazáson belül a bejelentkezési élmény megszűnését.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-173">Loss of the ability to manage the login experience for your customers within your application.</span></span>

<span data-ttu-id="3e1b1-174">Íme, hogy a Microsoft Identity SDK-k hogyan működnek együtt a broker alkalmazásokhoz, hogy egyszeri Bejelentkezéses ábrázolása:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-174">Here is a representation of how the Microsoft Identity SDKs work with the broker applications to enable SSO:</span></span>

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
|  ADAL SDK  | |  ADAL SDK  |   |  ADAL SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+

```

<span data-ttu-id="3e1b1-175">Volt képes a háttér-információkat, hogy jobban megismerheti, és alkalmazhatja a egyszeri bejelentkezés a Microsoft Identity platform és SDK-k segítségével az alkalmazáson belül képesnek kell lennie felvértezve.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-175">Armed with this background information you should be able to better understand and implement SSO within your application using the Microsoft Identity platform and SDKs.</span></span>

## <a name="enabling-cross-app-sso-using-adal"></a><span data-ttu-id="3e1b1-176">Adal-t használó alkalmazások közötti SSO engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3e1b1-176">Enabling cross-app SSO using ADAL</span></span>
<span data-ttu-id="3e1b1-177">Jelen példában használjuk a ADAL Android SDK használatával:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-177">Here we use the ADAL Android SDK to:</span></span>

* <span data-ttu-id="3e1b1-178">Nem közvetített támogatott egyszeri Bejelentkezést a csomag az alkalmazások bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="3e1b1-178">Turn on non-broker assisted SSO for your suite of apps</span></span>
* <span data-ttu-id="3e1b1-179">Kapcsolja be az SSO broker támogatású támogatása</span><span class="sxs-lookup"><span data-stu-id="3e1b1-179">Turn on support for broker-assisted SSO</span></span>

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a><span data-ttu-id="3e1b1-180">Nem közvetített egyszeri Bejelentkezést bekapcsolás támogatott az egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3e1b1-180">Turning on SSO for non-broker assisted SSO</span></span>
<span data-ttu-id="3e1b1-181">Nem közvetített támogatott egyszeri bejelentkezéshez különböző alkalmazások a Microsoft Identity SDK-k kezelése nagy részét SSO összetettsége meg.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-181">For non-broker assisted SSO across applications the Microsoft Identity SDKs manage much of the complexity of SSO for you.</span></span> <span data-ttu-id="3e1b1-182">Ez magában foglalja a megfelelő felhasználói keresése a gyorsítótárban, és karbantartása, hogy a lekérdezés bejelentkezett felhasználók listáját.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-182">This includes finding the right user in the cache and maintaining a list of logged in users for you to query.</span></span>

<span data-ttu-id="3e1b1-183">Egyszeri bejelentkezés engedélyezése a különböző alkalmazások saját végre kell hajtani a következőket:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-183">To enable SSO across applications you own you need to do the following:</span></span>

1. <span data-ttu-id="3e1b1-184">Győződjön meg arról az alkalmazások felhasználói a ugyanazon ügyfél-azonosító-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-184">Ensure all your applications user the same Client ID or Application ID.</span></span>
2. <span data-ttu-id="3e1b1-185">Győződjön meg arról, az alkalmazások SharedUserID ugyanazokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-185">Ensure all your applications have the same SharedUserID set.</span></span>
3. <span data-ttu-id="3e1b1-186">Győződjön meg arról, hogy ugyanazt a aláíró tanúsítványt a Google Play áruházból az alkalmazásokat használ, úgy, hogy a tároló megoszthatja.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-186">Ensure that all of your applications share the same signing certificate from the Google Play store so that you can share storage.</span></span>

#### <a name="step-1-using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a><span data-ttu-id="3e1b1-187">1. lépés: Az azonos ügyfél-Azonosítót használó / Application ID, az alkalmazások a csomagban található összes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="3e1b1-187">Step 1: Using the same Client ID / Application ID for all the applications in your suite of apps</span></span>
<span data-ttu-id="3e1b1-188">Ahhoz, hogy a Microsoft Identity platform tudni, hogy rendelkezik engedélyezett jogkivonatok megosztása az alkalmazások között az alkalmazások egyes kell, hogy az azonos ügyfél-azonosító-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-188">In order for the Microsoft Identity platform to know that it's allowed to share tokens across your applications, each of your applications will need to share the same Client ID or Application ID.</span></span> <span data-ttu-id="3e1b1-189">Ez az egyedi azonosítója, amely az Ön számára biztosított a portálon az első alkalmazás regisztrálásakor.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-189">This is the unique identifier that was provided to you when you registered your first application in the portal.</span></span>

<span data-ttu-id="3e1b1-190">Előfordulhat, hogy lehet szeretné megtudni, hogy hogyan határozható meg a Microsoft Identity szolgáltatás különböző alkalmazások Ha használja az ugyanazon alkalmazás azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-190">You may be wondering how you will identify different apps to the Microsoft Identity service if it uses the same Application ID.</span></span> <span data-ttu-id="3e1b1-191">A válasz a kérdésre a **átirányítási URI-azonosítók**.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-191">The answer is with the **Redirect URIs**.</span></span> <span data-ttu-id="3e1b1-192">Minden alkalmazás több átirányítási URI-azonosítók a bevezetési portálon regisztrált rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-192">Each application can have multiple Redirect URIs registered in the onboarding portal.</span></span> <span data-ttu-id="3e1b1-193">A csomagban található összes alkalmazás egy másik átirányítási URI-t fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-193">Each app in your suite will have a different redirect URI.</span></span> <span data-ttu-id="3e1b1-194">Ez megjelenésének például nem éri el:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-194">An example of how this looks is below:</span></span>

<span data-ttu-id="3e1b1-195">Az App1 átirányítási URI-ja:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`</span><span class="sxs-lookup"><span data-stu-id="3e1b1-195">App1 Redirect URI: `msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`</span></span>

<span data-ttu-id="3e1b1-196">App2 átirányítási URI-ja:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`</span><span class="sxs-lookup"><span data-stu-id="3e1b1-196">App2 Redirect URI: `msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`</span></span>

<span data-ttu-id="3e1b1-197">App3 átirányítási URI-ja:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`</span><span class="sxs-lookup"><span data-stu-id="3e1b1-197">App3 Redirect URI: `msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`</span></span>

<span data-ttu-id="3e1b1-198">....</span><span class="sxs-lookup"><span data-stu-id="3e1b1-198">....</span></span>

<span data-ttu-id="3e1b1-199">Ezek az azonos ügyfél-azonosító alapján beágyazott / alkalmazás azonosítója és keresni az átirányítási URI-t ismét nekünk a SDK-konfiguráció alapján.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-199">These are nested under the same client ID / application ID and looked up based on the redirect URI you return to us in your SDK configuration.</span></span>

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


<span data-ttu-id="3e1b1-200">*Vegye figyelembe, hogy az átirányítási URI-k formátuma ismertetése az alábbiakban olvasható. Segítségével bármely átirányítási URI-kivéve, ha kíván támogatni a broker ebben az esetben azok kell kinéznie hasonlóan a fentiek közül.*</span><span class="sxs-lookup"><span data-stu-id="3e1b1-200">*Note that the format of these Redirect URIs are explained below. You may use any Redirect URI unless you wish to support the broker, in which case they must look something like the above*</span></span>

#### <a name="step-2-configuring-shared-storage-in-android"></a><span data-ttu-id="3e1b1-201">2. lépés: Az Android megosztott tárolás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3e1b1-201">Step 2: Configuring shared storage in Android</span></span>
<span data-ttu-id="3e1b1-202">Beállítás a `SharedUserID` túlmutat a jelen dokumentum, de a Google Android rendszerhez dokumentáció a olvasásával ismernie kell a [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html).</span><span class="sxs-lookup"><span data-stu-id="3e1b1-202">Setting the `SharedUserID` is beyond the scope of this document but can be learned by reading the Google Android documentation on the [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html).</span></span> <span data-ttu-id="3e1b1-203">Fontos, hogy úgy dönt, hogy mi a sharedUserID hívja meg a rendszer, és azt használja az alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-203">What is important is that you decide what you want your sharedUserID will be called and use that across all your applications.</span></span>

<span data-ttu-id="3e1b1-204">Ha elvégezte a `SharedUserID` az összes alkalmazás készen áll SSO használata.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-204">Once you have the `SharedUserID` in all your applications you are ready to use SSO.</span></span>

> [!WARNING]
> <span data-ttu-id="3e1b1-205">Ha tárolási megosztani az alkalmazások bármely alkalmazás törölheti felhasználók, vagy rosszabb a jogkivonatok az alkalmazás minden részében.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-205">When you share storage across your applications any application can delete users, or worse delete all the tokens across your application.</span></span> <span data-ttu-id="3e1b1-206">Ez a különösen katasztrofális, ha a tokeneket a háttérben munkahelyi alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-206">This is particularly disastrous if you have applications that rely on the tokens to do background work.</span></span> <span data-ttu-id="3e1b1-207">Megosztása azt jelenti, hogy minden eltávolítása műveleteket a Microsoft Identity SDK-k segítségével nagyon gondosan kell.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-207">Sharing storage means that you must be very careful in any and all remove operations through the Microsoft Identity SDKs.</span></span>
> 
> 

<span data-ttu-id="3e1b1-208">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="3e1b1-208">That's it!</span></span> <span data-ttu-id="3e1b1-209">A Microsoft Identity SDK most fájlmegosztási hitelesítő adatokat az alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-209">The Microsoft Identity SDK will now share credentials across all your applications.</span></span> <span data-ttu-id="3e1b1-210">A Felhasználólista lesznek megosztva alkalmazáspéldányok között.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-210">The user list will also be shared across application instances.</span></span>

### <a name="turning-on-sso-for-broker-assisted-sso"></a><span data-ttu-id="3e1b1-211">Egyszeri bejelentkezés SSO Broker bekapcsolása esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-211">Turning on SSO for broker assisted SSO</span></span>
<span data-ttu-id="3e1b1-212">Az, hogy az alkalmazás számára bármilyen brokert, az eszközön telepített **alapértelmezés szerint ki van kapcsolva**.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-212">The ability for an application to use any broker that is installed on the device is **turned off by default**.</span></span> <span data-ttu-id="3e1b1-213">Ahhoz, hogy használhassa az alkalmazást a broker néhány további konfigurációs lehetőségek és néhány kódot hozzá az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-213">In order to use your application with the broker you must do some additional configuration and add some code to your application.</span></span>

<span data-ttu-id="3e1b1-214">A lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-214">The steps to follow are:</span></span>

1. <span data-ttu-id="3e1b1-215">Az alkalmazás kódjában hívása közben a MS SDK broker üzemmód engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3e1b1-215">Enable broker mode in your application code's call to the MS SDK</span></span>
2. <span data-ttu-id="3e1b1-216">Állítson be egy új átirányítási URI-t, és adja meg, hogy az alkalmazás és az alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="3e1b1-216">Establish a new redirect URI and provide that to both the app and your app registration</span></span>
3. <span data-ttu-id="3e1b1-217">Az Android-jegyzékfájlból a megfelelő engedélyek beállítása</span><span class="sxs-lookup"><span data-stu-id="3e1b1-217">Setting up the correct permissions in the Android manifest</span></span>

#### <a name="step-1-enable-broker-mode-in-your-application"></a><span data-ttu-id="3e1b1-218">1. lépés: Az alkalmazás broker üzemmód engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3e1b1-218">Step 1: Enable broker mode in your application</span></span>
<span data-ttu-id="3e1b1-219">Az alkalmazás használatához az átvitelszervező lehetősége van kapcsolva, a "beállítások" vagy a kezdeti beállítás a hitelesítési példány létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-219">The ability for your application to use the broker is turned on when you create the "settings" or initial setup of your Authentication instance.</span></span> <span data-ttu-id="3e1b1-220">Ehhez a ApplicationSettings beállítástípus a kódban:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-220">You do this by setting your ApplicationSettings type in your code:</span></span>

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a><span data-ttu-id="3e1b1-221">2. lépés: Egy új átirányítási URI-t és az URL-séma létrehozása</span><span class="sxs-lookup"><span data-stu-id="3e1b1-221">Step 2: Establish a new redirect URI with your URL Scheme</span></span>
<span data-ttu-id="3e1b1-222">Annak érdekében, hogy a Microsoft bármikor visszatérhet a hitelesítő adatok jogkivonatokat a megfelelő alkalmazás, meg kell győződnünk arról nevezzük vissza oly módon, hogy az Android operációs rendszer ellenőrizheti az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-222">In order to ensure that we always return the credential tokens to the correct application, we need to make sure we call back to your application in a way that the Android operating system can verify.</span></span> <span data-ttu-id="3e1b1-223">Android operációs rendszere a tanúsítvány kivonatát a Google Play áruházban.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-223">The Android operating system uses the hash of the certificate in the Google Play store.</span></span> <span data-ttu-id="3e1b1-224">Ez a nem egy engedélyezetlen alkalmazás megtévesztésre.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-224">This cannot be spoofed by a rogue application.</span></span> <span data-ttu-id="3e1b1-225">Ezért azt használja ki a győződjön meg arról, hogy a jogkivonatok visszatér a megfelelő alkalmazás broker az alkalmazás URI együtt.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-225">Therefore, we leverage this along with the URI of our broker application to ensure that the tokens are returned to the correct application.</span></span> <span data-ttu-id="3e1b1-226">Kérjük, az egyedi átirányítási URI-t mindkét létrehozásához az alkalmazásban, és egy átirányítási URI-t a fejlesztői portálon állítja be.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-226">We require you to establish this unique redirect URI both in your application and set as a Redirect URI in our developer portal.</span></span>

<span data-ttu-id="3e1b1-227">Az átirányítási URI-t a megfelelő formátumban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-227">Your redirect URI must be in the proper form of:</span></span>

`msauth://packagename/Base64UrlencodedSignature`

<span data-ttu-id="3e1b1-228">például: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*</span><span class="sxs-lookup"><span data-stu-id="3e1b1-228">ex: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*</span></span>

<span data-ttu-id="3e1b1-229">Az átirányítási URI-t kell adni az alkalmazás regisztrálása használatával a [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3e1b1-229">This Redirect URI needs to be specified in your app registration using the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="3e1b1-230">További információ az Azure AD alkalmazás-regisztrációval, lásd: [integrálása az Azure Active Directoryval](active-directory-how-to-integrate.md).</span><span class="sxs-lookup"><span data-stu-id="3e1b1-230">For more information on Azure AD app registration, see [Integrating with Azure Active Directory](active-directory-how-to-integrate.md).</span></span>

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a><span data-ttu-id="3e1b1-231">3. lépés: Az alkalmazás megfelelő engedélyek beállítása</span><span class="sxs-lookup"><span data-stu-id="3e1b1-231">Step 3: Set up the correct permissions in your application</span></span>
<span data-ttu-id="3e1b1-232">Az Android broker alkalmazás hitelesítő adatok kezelése a különböző alkalmazások az Android operációs rendszer fiókkezelő szolgáltatását használja.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-232">Our broker application in Android uses the Accounts Manager feature of the Android OS to manage credentials across applications.</span></span> <span data-ttu-id="3e1b1-233">Ahhoz, hogy a broker az Android a az alkalmazás jegyzékének AccountManager fiókok használatára engedélyekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-233">In order to use the broker in Android your app manifest must have permissions to use AccountManager accounts.</span></span> <span data-ttu-id="3e1b1-234">Erről részletes szól a [fiókkezelő a Google-dokumentáció Itt](http://developer.android.com/reference/android/accounts/AccountManager.html)</span><span class="sxs-lookup"><span data-stu-id="3e1b1-234">This is discussed in detail in the [Google documentation for Account Manager here](http://developer.android.com/reference/android/accounts/AccountManager.html)</span></span>

<span data-ttu-id="3e1b1-235">Ezek az engedélyek különösen a következőket is:</span><span class="sxs-lookup"><span data-stu-id="3e1b1-235">In particular, these permissions are:</span></span>

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a><span data-ttu-id="3e1b1-236">Egyszeri bejelentkezés konfigurálása!</span><span class="sxs-lookup"><span data-stu-id="3e1b1-236">You've configured SSO!</span></span>
<span data-ttu-id="3e1b1-237">Most már a Microsoft Identity SDK automatikusan is fájlmegosztási hitelesítő adatokat az alkalmazások között és a broker meghívni, ha az megtalálható az eszközükön.</span><span class="sxs-lookup"><span data-stu-id="3e1b1-237">Now the Microsoft Identity SDK will automatically both share credentials across your applications and invoke the broker if it's present on their device.</span></span>

