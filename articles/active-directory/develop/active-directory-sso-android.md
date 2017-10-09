---
title: "aaaHow tooenable az adal-t használó Android alkalmazások közötti SSO |} Microsoft Docs"
description: "Hogyan toouse hello funkcióit hello az alkalmazások között az egyszeri bejelentkezés ADAL SDK tooenable. "
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
ms.openlocfilehash: 3867e15030e5516464e4dbd92ba35894430daf00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-cross-app-sso-on-android-using-adal"></a><span data-ttu-id="90b39-103">Hogyan tooenable alkalmazások közötti SSO-ADAL használatával Android rendszeren</span><span class="sxs-lookup"><span data-stu-id="90b39-103">How tooenable cross-app SSO on Android using ADAL</span></span>
<span data-ttu-id="90b39-104">Egyszeri bejelentkezés (SSO) biztosít, így a felhasználók csak kell tooenter hitelesítő adataikkal egyszer, és ezen hitelesítő adatok automatikusan munkahelyi rendelkezik a különböző alkalmazások most várt ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="90b39-104">Providing Single Sign-On (SSO) so that users only need tooenter their credentials once and have those credentials automatically work across applications is now expected by customers.</span></span> <span data-ttu-id="90b39-105">hello nehéz kis képernyőjű, gyakran alkalommal együtt egy további tényezőt (2FA) például telefonhívást vagy egy fogva kódot, a felhasználónév és jelszó megadása eredményezi, ha egy felhasználó gyors kapcsolatos van toodo ennek a termékhez egynél többször.</span><span class="sxs-lookup"><span data-stu-id="90b39-105">hello difficulty in entering their username and password on a small screen, often times combined with an additional factor (2FA) like a phone call or a texted code, results in quick dissatisfaction if a user has toodo this more than one time for your product.</span></span>

<span data-ttu-id="90b39-106">Ezenkívül ha alkalmaz egy identitás-platform, amely más alkalmazások is használhatnak, például a Microsoft Accounts vagy az Office365 munkahelyi fiókkal, az ügyfelek várt, hogy ezen hitelesítő adatok toobe elérhető toouse az alkalmazások között nem számít, hello szállító.</span><span class="sxs-lookup"><span data-stu-id="90b39-106">In addition, if you apply an identity platform that other applications may use such as Microsoft Accounts or a work account from Office365, customers expect that those credentials toobe available toouse across all their applications no matter hello vendor.</span></span>

<span data-ttu-id="90b39-107">hello Microsoft Identity platform, a Microsoft Identity SDK-k, valamint elvégzi a rögzített munka, és képes toodelight egyszeri Bejelentkezést, vagy az alkalmazások vagy, mint a saját suite belül az ügyfelek hello biztosítja a broker funkció és a hitelesítő az alkalmazások, az eszköz teljes hello.</span><span class="sxs-lookup"><span data-stu-id="90b39-107">hello Microsoft Identity platform, along with our Microsoft Identity SDKs, does all this hard work for you and gives you hello ability toodelight your customers with SSO either within your own suite of applications or, as with our broker capability and Authenticator applications, across hello entire device.</span></span>

<span data-ttu-id="90b39-108">Ez a forgatókönyv megtudhatja, hogyan tooconfigure az SDK az alkalmazás tooprovide belül a juttatás tooyour ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="90b39-108">This walkthrough will tell you how tooconfigure our SDK within your application tooprovide this benefit tooyour customers.</span></span>

<span data-ttu-id="90b39-109">Ez a forgatókönyv a következőkre vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="90b39-109">This walkthrough applies to:</span></span>

* <span data-ttu-id="90b39-110">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="90b39-110">Azure Active Directory</span></span>
* <span data-ttu-id="90b39-111">Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="90b39-111">Azure Active Directory B2C</span></span>
* <span data-ttu-id="90b39-112">Az Azure Active Directory B2B</span><span class="sxs-lookup"><span data-stu-id="90b39-112">Azure Active Directory B2B</span></span>
* <span data-ttu-id="90b39-113">Azure Active Directory feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="90b39-113">Azure Active Directory Conditional Access</span></span>

<span data-ttu-id="90b39-114">hello előző lépéseknél túl hogyan tudja[alkalmazások hello örökölt portálon az Azure Active Directory kiépítése](active-directory-how-to-integrate.md) és az alkalmazás integrálható hello [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android) .</span><span class="sxs-lookup"><span data-stu-id="90b39-114">hello document preceding assumes you know how too[provision applications in hello legacy portal for Azure Active Directory](active-directory-how-to-integrate.md) and integrated your application with hello [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android).</span></span>

## <a name="sso-concepts-in-hello-microsoft-identity-platform"></a><span data-ttu-id="90b39-115">A Microsoft Identity Platform hello SSO fogalmak</span><span class="sxs-lookup"><span data-stu-id="90b39-115">SSO Concepts in hello Microsoft Identity Platform</span></span>
### <a name="microsoft-identity-brokers"></a><span data-ttu-id="90b39-116">Microsoft – identitáskezelési brókerek</span><span class="sxs-lookup"><span data-stu-id="90b39-116">Microsoft Identity Brokers</span></span>
<span data-ttu-id="90b39-117">A Microsoft lehetővé teszi a minden mobileszköz platform, amelyek lehetővé teszik a különböző alkalmazások különböző szállítóktól származó hello hídképzést, a hitelesítő adatok, és lehetővé teszi, hogy honnan egy biztonságos helyen igénylő különleges továbbfejlesztett funkciók toovalidate hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="90b39-117">Microsoft provides applications for every mobile platform that allow for hello bridging of credentials across applications from different vendors and allows for special enhanced features that require a single secure place from where toovalidate credentials.</span></span> <span data-ttu-id="90b39-118">Ezek nevezzük **brókerek**.</span><span class="sxs-lookup"><span data-stu-id="90b39-118">We call these **brokers**.</span></span> <span data-ttu-id="90b39-119">IOS és Android rendszeren ezek szolgáltatáson keresztül letölthető alkalmazás, hogy az ügyfelek telepítése függetlenül vagy egy vállalati, akár az összeset hello eszköz az alkalmazott felügyelő a toohello eszköz továbbíthatja.</span><span class="sxs-lookup"><span data-stu-id="90b39-119">On iOS and Android these are provided through downloadable applications that customers either install independently or can be pushed toohello device by a company who manages some or all of hello device for their employee.</span></span> <span data-ttu-id="90b39-120">Ezek a brókerek kezelése biztonsági csak az egyes alkalmazások vagy hello: az eszköz teljes alapján a rendszergazdák törlését támogatja.</span><span class="sxs-lookup"><span data-stu-id="90b39-120">These brokers support managing security just for some applications or hello entire device based on what IT Administrators desire.</span></span> <span data-ttu-id="90b39-121">A Windows rendszerben a funkcióit egy toohello operációs rendszerben, technikailag néven hello Webeshitelesítés-szervező beépített fiók kiválasztásakor.</span><span class="sxs-lookup"><span data-stu-id="90b39-121">In Windows, this functionality is provided by an account chooser built in toohello operating system, known technically as hello Web Authentication Broker.</span></span>

<span data-ttu-id="90b39-122">További információt használjuk, a brókerek, és hogyan az ügyfelek előfordulhat, hogy lássák a bejelentkezési folyamat hello Microsoft Identity platform olvasni.</span><span class="sxs-lookup"><span data-stu-id="90b39-122">For more information on how we use these brokers and how your customers might see them in their login flow for hello Microsoft Identity platform read on.</span></span>

### <a name="patterns-for-logging-in-on-mobile-devices"></a><span data-ttu-id="90b39-123">Bejelentkezés mobileszközök minták</span><span class="sxs-lookup"><span data-stu-id="90b39-123">Patterns for logging in on mobile devices</span></span>
<span data-ttu-id="90b39-124">Hozzáférés toocredentials eszközökön két alapvető mintázatokból hello Microsoft Identity platform kövesse:</span><span class="sxs-lookup"><span data-stu-id="90b39-124">Access toocredentials on devices follow two basic patterns for hello Microsoft Identity platform:</span></span>

* <span data-ttu-id="90b39-125">Nem közvetített támogatott bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="90b39-125">Non-broker assisted logins</span></span>
* <span data-ttu-id="90b39-126">Támogatott bejelentkezések replikaszervező</span><span class="sxs-lookup"><span data-stu-id="90b39-126">Broker assisted logins</span></span>

#### <a name="non-broker-assisted-logins"></a><span data-ttu-id="90b39-127">Nem közvetített támogatott bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="90b39-127">Non-broker assisted logins</span></span>
<span data-ttu-id="90b39-128">Nem közvetített támogatott bejelentkezések hello alkalmazás együtt, beágyazottan történik, és hello helyi tárhelyet használó hello eszközön az alkalmazás bejelentkezési élmény.</span><span class="sxs-lookup"><span data-stu-id="90b39-128">Non-broker assisted logins are login experiences that happen inline with hello application and use hello local storage on hello device for that application.</span></span> <span data-ttu-id="90b39-129">Ez a tároló lehet osztani alkalmazásra, de hello hitelesítő adatok szorosan kötött toohello alkalmazás vagy csomag alkalmazásaikat, hogy a hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="90b39-129">This storage may be shared across applications but hello credentials are tightly bound toohello app or suite of apps using that credential.</span></span> <span data-ttu-id="90b39-130">Valószínűleg tapasztal Ez sok mobilalkalmazások a felhasználónév és jelszó maga hello alkalmazásban beírásakor.</span><span class="sxs-lookup"><span data-stu-id="90b39-130">You've most likely experienced this in many mobile applications when you enter a username and password within hello application itself.</span></span>

<span data-ttu-id="90b39-131">Ezek bejelentkezések a következő előnyöket hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="90b39-131">These logins have hello following benefits:</span></span>

* <span data-ttu-id="90b39-132">Felhasználói élmény létezik-e teljes mértékben hello alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="90b39-132">User experience exists entirely within hello application.</span></span>
* <span data-ttu-id="90b39-133">Hitelesítő adatok meg lehet osztani összes hello által aláírt alkalmazások ugyanazt a tanúsítványt, egy egyszeri bejelentkezéses élményt tooyour suite alkalmazások biztosítása.</span><span class="sxs-lookup"><span data-stu-id="90b39-133">Credentials can be shared across applications that are signed by hello same certificate, providing a single sign-on experience tooyour suite of applications.</span></span>
* <span data-ttu-id="90b39-134">Bejelentkezés hello élménye körül vezérlő toohello alkalmazás előtt és után bejelentkezhet valósul meg.</span><span class="sxs-lookup"><span data-stu-id="90b39-134">Control around hello experience of logging in is provided toohello application before and after sign-in.</span></span>

<span data-ttu-id="90b39-135">Ezek bejelentkezések a következő hátrányokkal hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="90b39-135">These logins have hello following drawbacks:</span></span>

* <span data-ttu-id="90b39-136">A felhasználó csak azokat az alkalmazás van-e beállítva a Microsoft Identities között a Microsoft Identity használó összes alkalmazások között nem számára az egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="90b39-136">User cannot experience single-sign on across all apps that use a Microsoft Identity, only across those Microsoft Identities that your application has configured.</span></span>
* <span data-ttu-id="90b39-137">Az alkalmazás nem használható például a feltételes hozzáférés, vagy használjon hello InTune termékcsomagot speciális üzleti szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="90b39-137">Your application cannot be used with more advanced business features such as Conditional Access or use hello InTune suite of products.</span></span>
* <span data-ttu-id="90b39-138">Az alkalmazás nem támogatja a Tanúsítványalapú hitelesítés üzleti felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="90b39-138">Your application can't support certificate-based authentication for business users.</span></span>

<span data-ttu-id="90b39-139">Íme egy hello Microsoft Identity SDK-k működése megosztott hello tárolására az alkalmazások tooenable SSO ábrázolása:</span><span class="sxs-lookup"><span data-stu-id="90b39-139">Here is a representation of how hello Microsoft Identity SDKs work with hello shared storage of your applications tooenable SSO:</span></span>

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

#### <a name="broker-assisted-logins"></a><span data-ttu-id="90b39-140">Támogatott bejelentkezések replikaszervező</span><span class="sxs-lookup"><span data-stu-id="90b39-140">Broker assisted logins</span></span>
<span data-ttu-id="90b39-141">Bejelentkezések Broker támogatású bejelentkezési élmény hello broker alkalmazáson belül és hello tárolási és biztonsági hello broker tooshare hitelesítő adatok használata minden alkalmazásra hello eszközön, amelyek érvényesek a Microsoft Identity platform hello.</span><span class="sxs-lookup"><span data-stu-id="90b39-141">Broker-assisted logins are login experiences that occur within hello broker application and use hello storage and security of hello broker tooshare credentials across all applications on hello device that apply hello Microsoft Identity platform.</span></span> <span data-ttu-id="90b39-142">Ez azt jelenti, hogy az alkalmazások támaszkodnak hello broker toosign felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="90b39-142">This means that your applications rely on hello broker toosign users in.</span></span> <span data-ttu-id="90b39-143">IOS és Android rendszeren a brókerek szolgáltatáson keresztül letölthető alkalmazás, hogy az ügyfelek telepítése függetlenül vagy olyan cég, amely kezeli a felhasználó hello az eszközön a toohello eszköz továbbíthatja.</span><span class="sxs-lookup"><span data-stu-id="90b39-143">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed toohello device by a company who manages hello device for their user.</span></span> <span data-ttu-id="90b39-144">Ez az alkalmazástípus példa: hello Microsoft Authenticator alkalmazás IOS rendszerű eszközökön.</span><span class="sxs-lookup"><span data-stu-id="90b39-144">An example of this type of application is hello Microsoft Authenticator application on iOS.</span></span> <span data-ttu-id="90b39-145">A Windows e funkcióit egy toohello operációs rendszerben, technikailag néven hello Webeshitelesítés-szervező beépített fiók kiválasztásakor.</span><span class="sxs-lookup"><span data-stu-id="90b39-145">In Windows this functionality is provided by an account chooser built in toohello operating system, known technically as hello Web Authentication Broker.</span></span>
<span data-ttu-id="90b39-146">hello élmény platformonként változó, és előfordulhat zavaró toousers nem esetén megfelelően.</span><span class="sxs-lookup"><span data-stu-id="90b39-146">hello experience varies by platform and can sometimes be disruptive toousers if not managed correctly.</span></span> <span data-ttu-id="90b39-147">Ismeri valószínűleg legtöbb ebben a mintában Ha hello Facebook-alkalmazás telepítve, és Facebook csatlakozni egy másik alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="90b39-147">You're probably most familiar with this pattern if you have hello Facebook application installed and use Facebook Connect from another application.</span></span> <span data-ttu-id="90b39-148">hello Microsoft – identitáskezelési platform által használt hello ugyanilyen mintájú.</span><span class="sxs-lookup"><span data-stu-id="90b39-148">hello Microsoft Identity platform uses hello same pattern.</span></span>

<span data-ttu-id="90b39-149">Az IOS-es ennek eredménye tooa "átmenet" animáció ahol az alkalmazás küldése közben hello Microsoft Authenticator alkalmazások toohello háttér elérhető lesz a hello felhasználói tooselect toohello előtér melyik fiókot szeretné toosign be.</span><span class="sxs-lookup"><span data-stu-id="90b39-149">For iOS this leads tooa "transition" animation where your application is sent toohello background while hello Microsoft Authenticator applications comes toohello foreground for hello user tooselect which account they would like toosign in with.</span></span>  

<span data-ttu-id="90b39-150">Android és Windows hello fiók kiválasztásakor az alkalmazás, amely kevésbé zavaró toohello felhasználói felett jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="90b39-150">For Android and Windows hello account chooser is displayed on top of your application which is less disruptive toohello user.</span></span>

#### <a name="how-hello-broker-gets-invoked"></a><span data-ttu-id="90b39-151">Hogyan hello broker meghívott beolvasása</span><span class="sxs-lookup"><span data-stu-id="90b39-151">How hello broker gets invoked</span></span>
<span data-ttu-id="90b39-152">Ha egy kompatibilis broker hello eszköz, például Microsoft Authenticator alkalmazást, a Microsoft Identity SDK-k hello hello automatikusan telepítve van a hello hívásának hello broker meg, amikor a felhasználó azt jelzi, hogy kívánják toolog bármely fiókkal való munka hello Microsoft Identity platform.</span><span class="sxs-lookup"><span data-stu-id="90b39-152">If a compatible broker is installed on hello device, like hello Microsoft Authenticator application, hello Microsoft Identity SDKs will automatically do hello work of invoking hello broker for you when a user indicates they wish toolog in using any account from hello Microsoft Identity platform.</span></span> <span data-ttu-id="90b39-153">Ez a fiók lehet személyes Microsoft-Account, munkahelyi vagy iskolai fiókját, vagy egy fiókot, amely megadja és az Azure-ban B2C és B2B termékeink állomás.</span><span class="sxs-lookup"><span data-stu-id="90b39-153">This account could be a personal Microsoft Account, a work or school account, or an account that you provide and host in Azure using our B2C and B2B products.</span></span> 
 
 #### <a name="how-we-ensure-hello-application-is-valid"></a><span data-ttu-id="90b39-154">Hogyan biztosítható hello alkalmazás érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="90b39-154">How we ensure hello application is valid</span></span>
 
 <span data-ttu-id="90b39-155">hello kell tooensure hello identitását egy alkalmazás hívás hello broker nyújtunk a támogatott broker bejelentkezési adatok számára elengedhetetlen toohello biztonsági.</span><span class="sxs-lookup"><span data-stu-id="90b39-155">hello need tooensure hello identity of an application call hello broker is crucial toohello security we provide in broker assisted logins.</span></span> <span data-ttu-id="90b39-156">IOS és Android nem kényszeríti az egyedi azonosítók, amelyek csak egy adott alkalmazáshoz tartozó érvényes, így rosszindulatú alkalmazások előfordulhat, hogy "hamis" a jogos alkalmazásazonosító, és azt jelentette, hogy hello jogos alkalmazás hello jogkivonatokat fogadni.</span><span class="sxs-lookup"><span data-stu-id="90b39-156">Neither iOS nor Android enforces unique identifiers that are valid only for a given application, so malicious applications may "spoof" a legitimate application's identifier and receive hello tokens meant for hello legitimate application.</span></span> <span data-ttu-id="90b39-157">mindig tájékoztatjuk futásidőben hello jobb alkalmazással tooensure, kérjük hello fejlesztői tooprovide egyéni redirectURI Microsoft az alkalmazás regisztrálásakor.</span><span class="sxs-lookup"><span data-stu-id="90b39-157">tooensure we are always communicating with hello right application at runtime, we ask hello developer tooprovide a custom redirectURI when registering their application with Microsoft.</span></span> <span data-ttu-id="90b39-158">**Az alábbiakban tárgyalt hogyan a fejlesztők kézműipari az átirányítási URI-t kell.**</span><span class="sxs-lookup"><span data-stu-id="90b39-158">**How developers should craft this redirect URI is discussed in detail below.**</span></span> <span data-ttu-id="90b39-159">A egyéni redirectURI hello tanúsítvány ujjlenyomata hello alkalmazás tartalmazza, és a Google Play áruház hello toobe egyedi toohello alkalmazás biztosítja.</span><span class="sxs-lookup"><span data-stu-id="90b39-159">This custom redirectURI contains hello certificate thumbprint of hello application and is ensured toobe unique toohello application by hello Google Play Store.</span></span> <span data-ttu-id="90b39-160">Amikor egy alkalmazás meghívja hello broker hello broker kéri hello Android operációs rendszer tooprovide hello a tanúsítvány ujjlenyomata a hívott hello broker.</span><span class="sxs-lookup"><span data-stu-id="90b39-160">When an application calls hello broker, hello broker asks hello Android operating system tooprovide it with hello certificate thumbprint that called hello broker.</span></span> <span data-ttu-id="90b39-161">hello broker hello hívás tooour identitásrendszere a tanúsítvány ujjlenyomata tooMicrosoft nyújt.</span><span class="sxs-lookup"><span data-stu-id="90b39-161">hello broker provides this certificate thumbprint tooMicrosoft in hello call tooour identity system.</span></span> <span data-ttu-id="90b39-162">Ha hello alkalmazás ujjlenyomat nem egyezik meg a tanúsítvány-ujjlenyomat hello hello tanúsítvány a regisztráció során megadott toous hello fejlesztő, azt megtagadja a hozzáférést toohello jogkivonatok hello erőforrás hello alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="90b39-162">If hello certificate thumbprint of hello application does not match hello certificate thumbprint provided toous by hello developer during registration, we will deny access toohello tokens for hello resource hello application is requesting.</span></span> <span data-ttu-id="90b39-163">Ez az ellenőrzés biztosítja, hogy csak a regisztrált hello fejlesztő hello alkalmazás-jogkivonatokat kap.</span><span class="sxs-lookup"><span data-stu-id="90b39-163">This check ensures that only hello application registered by hello developer receives tokens.</span></span>

<span data-ttu-id="90b39-164">**hello fejlesztői hello választott rendelkezik, ha a Microsoft Identity SDK hello hello broker meghívja vagy hello nem közvetített támogatott folyamat használja.**</span><span class="sxs-lookup"><span data-stu-id="90b39-164">**hello developer has hello choice of if hello Microsoft Identity SDK calls hello broker or uses hello non-broker assisted flow.**</span></span> <span data-ttu-id="90b39-165">Azonban ha hello fejlesztői nem toouse hello broker támogatású folyamata megszakad a hello előnye, hogy egyszeri bejelentkezési hitelesítő adataival, hogy hello a felhasználó már valószínűleg hello eszközön, és megakadályozza, hogy az alkalmazás használatát a Microsoft üzleti szolgáltatásokkal az ügyfelek, például a feltételes hozzáférés, a Intune kezelési képességek és a tanúsítvány alapú hitelesítést nyújt.</span><span class="sxs-lookup"><span data-stu-id="90b39-165">However if hello developer chooses not toouse hello broker-assisted flow they lose hello benefit of using SSO credentials that hello user may have already added on hello device and prevents their application from being used with business features Microsoft provides its customers such as Conditional Access, Intune Management capabilities, and certificate-based authentication.</span></span>

<span data-ttu-id="90b39-166">Ezek bejelentkezések a következő előnyöket hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="90b39-166">These logins have hello following benefits:</span></span>

* <span data-ttu-id="90b39-167">Felhasználói hello szállító függetlenül az alkalmazások közötti SSO észlel.</span><span class="sxs-lookup"><span data-stu-id="90b39-167">User experiences SSO across all their applications no matter hello vendor.</span></span>
* <span data-ttu-id="90b39-168">Az alkalmazás használhatja a Speciális üzleti szolgáltatásokat, mint a feltételes hozzáférés, de hello InTune termékcsomagot használja.</span><span class="sxs-lookup"><span data-stu-id="90b39-168">Your application can use more advanced business features such as Conditional Access or use hello InTune suite of products.</span></span>
* <span data-ttu-id="90b39-169">Az alkalmazás tanúsítvány alapú hitelesítést is támogatja az üzleti felhasználók.</span><span class="sxs-lookup"><span data-stu-id="90b39-169">Your application can support certificate-based authentication for business users.</span></span>
* <span data-ttu-id="90b39-170">Sokkal biztonságosabb bejelentkezés felhasználói élmény, hello alkalmazás- és hello felhasználói hello identitásának ellenőrzése hello broker alkalmazás további biztonsági algoritmusokkal és a titkosítás.</span><span class="sxs-lookup"><span data-stu-id="90b39-170">Much more secure sign-in experience as hello identity of hello application and hello user are verified by hello broker application with additional security algorithms and encryption.</span></span>

<span data-ttu-id="90b39-171">Ezek bejelentkezések a következő hátrányokkal hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="90b39-171">These logins have hello following drawbacks:</span></span>

* <span data-ttu-id="90b39-172">IOS-hello felhasználó van állapotra váltott ki az alkalmazás élmény közben a hitelesítő adatok közül választ.</span><span class="sxs-lookup"><span data-stu-id="90b39-172">In iOS hello user is transitioned out of your application's experience while credentials are chosen.</span></span>
* <span data-ttu-id="90b39-173">Hello képességét toomanage hello bejelentkezési megszűnését tapasztalhat az ügyfeleknek az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="90b39-173">Loss of hello ability toomanage hello login experience for your customers within your application.</span></span>

<span data-ttu-id="90b39-174">Íme egy hogyan hello Microsoft Identity SDK-k használata hello replikaszervező alkalmazások tooenable SSO ábrázolása:</span><span class="sxs-lookup"><span data-stu-id="90b39-174">Here is a representation of how hello Microsoft Identity SDKs work with hello broker applications tooenable SSO:</span></span>

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

<span data-ttu-id="90b39-175">Volt képes a háttér-információkat felvértezve meg kell tudni toobetter megismerheti és alkalmazhatja a SSO hello Microsoft Identity platform és SDK-k segítségével az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="90b39-175">Armed with this background information you should be able toobetter understand and implement SSO within your application using hello Microsoft Identity platform and SDKs.</span></span>

## <a name="enabling-cross-app-sso-using-adal"></a><span data-ttu-id="90b39-176">Adal-t használó alkalmazások közötti SSO engedélyezése</span><span class="sxs-lookup"><span data-stu-id="90b39-176">Enabling cross-app SSO using ADAL</span></span>
<span data-ttu-id="90b39-177">Jelen példában használjuk hello ADAL Android SDK számára:</span><span class="sxs-lookup"><span data-stu-id="90b39-177">Here we use hello ADAL Android SDK to:</span></span>

* <span data-ttu-id="90b39-178">Nem közvetített támogatott egyszeri Bejelentkezést a csomag az alkalmazások bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="90b39-178">Turn on non-broker assisted SSO for your suite of apps</span></span>
* <span data-ttu-id="90b39-179">Kapcsolja be az SSO broker támogatású támogatása</span><span class="sxs-lookup"><span data-stu-id="90b39-179">Turn on support for broker-assisted SSO</span></span>

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a><span data-ttu-id="90b39-180">Nem közvetített egyszeri Bejelentkezést bekapcsolás támogatott az egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="90b39-180">Turning on SSO for non-broker assisted SSO</span></span>
<span data-ttu-id="90b39-181">Nem közvetített támogatott egyszeri bejelentkezéshez alkalmazásra hello Microsoft Identity SDK-k kezelése nagy részét SSO hello összetettsége meg.</span><span class="sxs-lookup"><span data-stu-id="90b39-181">For non-broker assisted SSO across applications hello Microsoft Identity SDKs manage much of hello complexity of SSO for you.</span></span> <span data-ttu-id="90b39-182">Ez magában foglalja a hello gyorsítótár hello jobb felhasználói keresése és karbantartása az Ön tooquery bejelentkezett felhasználók listáját.</span><span class="sxs-lookup"><span data-stu-id="90b39-182">This includes finding hello right user in hello cache and maintaining a list of logged in users for you tooquery.</span></span>

<span data-ttu-id="90b39-183">egyszeri bejelentkezés tooenable alkalmazásra van szüksége a következő toodo hello saját:</span><span class="sxs-lookup"><span data-stu-id="90b39-183">tooenable SSO across applications you own you need toodo hello following:</span></span>

1. <span data-ttu-id="90b39-184">Győződjön meg arról az összes az alkalmazások felhasználói hello ugyanazon ügyfél-Azonosítót vagy az alkalmazás azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="90b39-184">Ensure all your applications user hello same Client ID or Application ID.</span></span>
2. <span data-ttu-id="90b39-185">Győződjön meg arról az alkalmazások hello azonos SharedUserID beállítása.</span><span class="sxs-lookup"><span data-stu-id="90b39-185">Ensure all your applications have hello same SharedUserID set.</span></span>
3. <span data-ttu-id="90b39-186">Ügyeljen arra, hogy az alkalmazások megosztás hello hello Google Play azonos aláíró tanúsítványt tárolni, így a tárolás megoszthatja a.</span><span class="sxs-lookup"><span data-stu-id="90b39-186">Ensure that all of your applications share hello same signing certificate from hello Google Play store so that you can share storage.</span></span>

#### <a name="step-1-using-hello-same-client-id--application-id-for-all-hello-applications-in-your-suite-of-apps"></a><span data-ttu-id="90b39-187">1. lépés: Használatával hello ugyanazon ügyfél-azonosító / Application ID, az összes hello az alkalmazások a csomagban található alkalmazások</span><span class="sxs-lookup"><span data-stu-id="90b39-187">Step 1: Using hello same Client ID / Application ID for all hello applications in your suite of apps</span></span>
<span data-ttu-id="90b39-188">Hello Microsoft Identity platform tooknow ahhoz, hogy az alkalmazások között tooshare jogkivonatok engedélyezett az alkalmazások egyes kell tooshare hello ugyanazon ügyfél-Azonosítót vagy az alkalmazás azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="90b39-188">In order for hello Microsoft Identity platform tooknow that it's allowed tooshare tokens across your applications, each of your applications will need tooshare hello same Client ID or Application ID.</span></span> <span data-ttu-id="90b39-189">Ez az egyedi azonosító hello tooyou megadott hello portálon az első alkalmazás regisztrálásakor.</span><span class="sxs-lookup"><span data-stu-id="90b39-189">This is hello unique identifier that was provided tooyou when you registered your first application in hello portal.</span></span>

<span data-ttu-id="90b39-190">Előfordulhat, hogy lehet szeretné megtudni, hogy hogyan határozható meg különböző alkalmazások toohello Ha használja a Microsoft Identity service hello ugyanazon alkalmazás azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="90b39-190">You may be wondering how you will identify different apps toohello Microsoft Identity service if it uses hello same Application ID.</span></span> <span data-ttu-id="90b39-191">hello válasz van hello **átirányítási URI-azonosítók**.</span><span class="sxs-lookup"><span data-stu-id="90b39-191">hello answer is with hello **Redirect URIs**.</span></span> <span data-ttu-id="90b39-192">Minden alkalmazás több átirányítási URI-azonosítók hello bevezetési portálon regisztrált rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="90b39-192">Each application can have multiple Redirect URIs registered in hello onboarding portal.</span></span> <span data-ttu-id="90b39-193">A csomagban található összes alkalmazás egy másik átirányítási URI-t fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="90b39-193">Each app in your suite will have a different redirect URI.</span></span> <span data-ttu-id="90b39-194">Ez megjelenésének például nem éri el:</span><span class="sxs-lookup"><span data-stu-id="90b39-194">An example of how this looks is below:</span></span>

<span data-ttu-id="90b39-195">Az App1 átirányítási URI-ja:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`</span><span class="sxs-lookup"><span data-stu-id="90b39-195">App1 Redirect URI: `msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`</span></span>

<span data-ttu-id="90b39-196">App2 átirányítási URI-ja:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`</span><span class="sxs-lookup"><span data-stu-id="90b39-196">App2 Redirect URI: `msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`</span></span>

<span data-ttu-id="90b39-197">App3 átirányítási URI-ja:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`</span><span class="sxs-lookup"><span data-stu-id="90b39-197">App3 Redirect URI: `msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`</span></span>

<span data-ttu-id="90b39-198">....</span><span class="sxs-lookup"><span data-stu-id="90b39-198">....</span></span>

<span data-ttu-id="90b39-199">Ezek a beágyazott hello ugyanazon ügyfél-azonosító / Alkalmazásazonosítót és alapján hello kereshető átirányítási URI-címe toous SDK konfigurációs adja vissza.</span><span class="sxs-lookup"><span data-stu-id="90b39-199">These are nested under hello same client ID / application ID and looked up based on hello redirect URI you return toous in your SDK configuration.</span></span>

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


<span data-ttu-id="90b39-200">*Vegye figyelembe, hogy az átirányítási URI-k formátuma hello ismertetésre vannak. Minden átirányítási URI-t használhatja kivéve toosupport hello broker kívánja, ebben az esetben azok kell kinéznie például a fenti hello*</span><span class="sxs-lookup"><span data-stu-id="90b39-200">*Note that hello format of these Redirect URIs are explained below. You may use any Redirect URI unless you wish toosupport hello broker, in which case they must look something like hello above*</span></span>

#### <a name="step-2-configuring-shared-storage-in-android"></a><span data-ttu-id="90b39-201">2. lépés: Az Android megosztott tárolás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="90b39-201">Step 2: Configuring shared storage in Android</span></span>
<span data-ttu-id="90b39-202">A beállítás hello `SharedUserID` nem ez a dokumentum hello terjed, de hello hello Google Android rendszerhez dokumentáció olvasásával ismernie kell [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html).</span><span class="sxs-lookup"><span data-stu-id="90b39-202">Setting hello `SharedUserID` is beyond hello scope of this document but can be learned by reading hello Google Android documentation on hello [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html).</span></span> <span data-ttu-id="90b39-203">Fontos, hogy úgy dönt, hogy mi a sharedUserID hívja meg a rendszer, és azt használja az alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="90b39-203">What is important is that you decide what you want your sharedUserID will be called and use that across all your applications.</span></span>

<span data-ttu-id="90b39-204">Ha elvégezte a hello `SharedUserID` az összes alkalmazás készen áll a toouse SSO.</span><span class="sxs-lookup"><span data-stu-id="90b39-204">Once you have hello `SharedUserID` in all your applications you are ready toouse SSO.</span></span>

> [!WARNING]
> <span data-ttu-id="90b39-205">Ha tárolási megosztani az alkalmazások bármely alkalmazás törölheti felhasználók, vagy rosszabb összes hello jogkivonatot az alkalmazás minden részében.</span><span class="sxs-lookup"><span data-stu-id="90b39-205">When you share storage across your applications any application can delete users, or worse delete all hello tokens across your application.</span></span> <span data-ttu-id="90b39-206">Ez a különösen katasztrofális, ha a hello jogkivonatok toodo háttérműveletek használó alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="90b39-206">This is particularly disastrous if you have applications that rely on hello tokens toodo background work.</span></span> <span data-ttu-id="90b39-207">Megosztása azt jelenti, hogy a Microsoft Identity SDK-k hello keresztül minden eltávolítása műveletek nagyon gondosan kell.</span><span class="sxs-lookup"><span data-stu-id="90b39-207">Sharing storage means that you must be very careful in any and all remove operations through hello Microsoft Identity SDKs.</span></span>
> 
> 

<span data-ttu-id="90b39-208">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="90b39-208">That's it!</span></span> <span data-ttu-id="90b39-209">a Microsoft Identity SDK hello most fájlmegosztási hitelesítő adatokat az alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="90b39-209">hello Microsoft Identity SDK will now share credentials across all your applications.</span></span> <span data-ttu-id="90b39-210">hello Felhasználólista lesznek megosztva alkalmazáspéldányok között.</span><span class="sxs-lookup"><span data-stu-id="90b39-210">hello user list will also be shared across application instances.</span></span>

### <a name="turning-on-sso-for-broker-assisted-sso"></a><span data-ttu-id="90b39-211">Egyszeri bejelentkezés SSO Broker bekapcsolása esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="90b39-211">Turning on SSO for broker assisted SSO</span></span>
<span data-ttu-id="90b39-212">az egy bármely Broker összetevője hello eszközön telepített alkalmazás toouse képesek hello **alapértelmezés szerint ki van kapcsolva**.</span><span class="sxs-lookup"><span data-stu-id="90b39-212">hello ability for an application toouse any broker that is installed on hello device is **turned off by default**.</span></span> <span data-ttu-id="90b39-213">A rendezés toouse az alkalmazás a hello broker kell tegye néhány további beállításra, és néhány kódot tooyour alkalmazás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="90b39-213">In order toouse your application with hello broker you must do some additional configuration and add some code tooyour application.</span></span>

<span data-ttu-id="90b39-214">hello lépéseket toofollow a következők:</span><span class="sxs-lookup"><span data-stu-id="90b39-214">hello steps toofollow are:</span></span>

1. <span data-ttu-id="90b39-215">Az alkalmazás kódjában hívás toohello MS SDK broker mód engedélyezése</span><span class="sxs-lookup"><span data-stu-id="90b39-215">Enable broker mode in your application code's call toohello MS SDK</span></span>
2. <span data-ttu-id="90b39-216">Állítson be egy új átirányítási URI-t, és tooboth hello alkalmazást és az alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="90b39-216">Establish a new redirect URI and provide that tooboth hello app and your app registration</span></span>
3. <span data-ttu-id="90b39-217">Hello megfelelő engedélyeket az Android-jegyzékfájlból hello beállítása</span><span class="sxs-lookup"><span data-stu-id="90b39-217">Setting up hello correct permissions in hello Android manifest</span></span>

#### <a name="step-1-enable-broker-mode-in-your-application"></a><span data-ttu-id="90b39-218">1. lépés: Az alkalmazás broker üzemmód engedélyezése</span><span class="sxs-lookup"><span data-stu-id="90b39-218">Step 1: Enable broker mode in your application</span></span>
<span data-ttu-id="90b39-219">az alkalmazás toouse hello Broker hello lehetősége van kapcsolva, hello "beállítások" vagy a kezdeti beállítás a hitelesítési példány létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="90b39-219">hello ability for your application toouse hello broker is turned on when you create hello "settings" or initial setup of your Authentication instance.</span></span> <span data-ttu-id="90b39-220">Ehhez a ApplicationSettings beállítástípus a kódban:</span><span class="sxs-lookup"><span data-stu-id="90b39-220">You do this by setting your ApplicationSettings type in your code:</span></span>

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a><span data-ttu-id="90b39-221">2. lépés: Egy új átirányítási URI-t és az URL-séma létrehozása</span><span class="sxs-lookup"><span data-stu-id="90b39-221">Step 2: Establish a new redirect URI with your URL Scheme</span></span>
<span data-ttu-id="90b39-222">A sorrend tooensure, hogy a rendszer mindig visszaadja a hello credential jogkivonatok toohello megfelelő alkalmazás igazolnia kell a toomake meg arról, hogy a telepítésnek vissza oly módon, hogy az Android operációs rendszer hello tooyour alkalmazás ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="90b39-222">In order tooensure that we always return hello credential tokens toohello correct application, we need toomake sure we call back tooyour application in a way that hello Android operating system can verify.</span></span> <span data-ttu-id="90b39-223">hello Android operációs rendszer hello tanúsítvány hello kivonatának hello Google Play áruház használja.</span><span class="sxs-lookup"><span data-stu-id="90b39-223">hello Android operating system uses hello hash of hello certificate in hello Google Play store.</span></span> <span data-ttu-id="90b39-224">Ez a nem egy engedélyezetlen alkalmazás megtévesztésre.</span><span class="sxs-lookup"><span data-stu-id="90b39-224">This cannot be spoofed by a rogue application.</span></span> <span data-ttu-id="90b39-225">Ezért azt használja ki a hello az, hogy a hello jogkivonatok kerüljenek-e vissza a megfelelő alkalmazás toohello broker alkalmazás tooensure URI-val együtt.</span><span class="sxs-lookup"><span data-stu-id="90b39-225">Therefore, we leverage this along with hello URI of our broker application tooensure that hello tokens are returned toohello correct application.</span></span> <span data-ttu-id="90b39-226">Kérjük, tooestablish ez egyedi átirányítási URI-t, mind az alkalmazás és egy átirányítási URI-t, a developer portálon.</span><span class="sxs-lookup"><span data-stu-id="90b39-226">We require you tooestablish this unique redirect URI both in your application and set as a Redirect URI in our developer portal.</span></span>

<span data-ttu-id="90b39-227">Az átirányítási URI-t a hello megfelelő formátumban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="90b39-227">Your redirect URI must be in hello proper form of:</span></span>

`msauth://packagename/Base64UrlencodedSignature`

<span data-ttu-id="90b39-228">például: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*</span><span class="sxs-lookup"><span data-stu-id="90b39-228">ex: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*</span></span>

<span data-ttu-id="90b39-229">Az átirányítási URI-t kell az alkalmazás-regisztrációk hello megadott toobe [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="90b39-229">This Redirect URI needs toobe specified in your app registration using hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="90b39-230">További információ az Azure AD alkalmazás-regisztrációval, lásd: [integrálása az Azure Active Directoryval](active-directory-how-to-integrate.md).</span><span class="sxs-lookup"><span data-stu-id="90b39-230">For more information on Azure AD app registration, see [Integrating with Azure Active Directory](active-directory-how-to-integrate.md).</span></span>

#### <a name="step-3-set-up-hello-correct-permissions-in-your-application"></a><span data-ttu-id="90b39-231">3. lépés: Hello megfelelő engedélyekkel az alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="90b39-231">Step 3: Set up hello correct permissions in your application</span></span>
<span data-ttu-id="90b39-232">Az Android broker alkalmazás alkalmazások hello fiókkezelő szolgáltatás hello Android operációs rendszer toomanage hitelesítő adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="90b39-232">Our broker application in Android uses hello Accounts Manager feature of hello Android OS toomanage credentials across applications.</span></span> <span data-ttu-id="90b39-233">Az alkalmazás jegyzékének rendelés toouse hello broker Android engedélyek toouse AccountManager fiókokat kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="90b39-233">In order toouse hello broker in Android your app manifest must have permissions toouse AccountManager accounts.</span></span> <span data-ttu-id="90b39-234">Erről szól hello részletes [fiókkezelő a Google-dokumentáció Itt](http://developer.android.com/reference/android/accounts/AccountManager.html)</span><span class="sxs-lookup"><span data-stu-id="90b39-234">This is discussed in detail in hello [Google documentation for Account Manager here](http://developer.android.com/reference/android/accounts/AccountManager.html)</span></span>

<span data-ttu-id="90b39-235">Ezek az engedélyek különösen a következőket is:</span><span class="sxs-lookup"><span data-stu-id="90b39-235">In particular, these permissions are:</span></span>

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a><span data-ttu-id="90b39-236">Egyszeri bejelentkezés konfigurálása!</span><span class="sxs-lookup"><span data-stu-id="90b39-236">You've configured SSO!</span></span>
<span data-ttu-id="90b39-237">Most hello Microsoft Identity SDK automatikusan a hitelesítő adatok megosztása az alkalmazások között és hello broker meghívni, ha az megtalálható az eszközükön.</span><span class="sxs-lookup"><span data-stu-id="90b39-237">Now hello Microsoft Identity SDK will automatically both share credentials across your applications and invoke hello broker if it's present on their device.</span></span>

