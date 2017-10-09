---
title: "aaaHow tooenable adal-t használó IOS alkalmazások közötti SSO |} Microsoft Docs"
description: "Hogyan toouse hello funkcióit hello az alkalmazások között az egyszeri bejelentkezés ADAL SDK tooenable. "
services: active-directory
documentationcenter: 
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: d042d6da-7503-4e20-bb55-06917de01fcd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: b7b4389a8dcd956211ffa1aaa431aaf21ded8961
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-cross-app-sso-on-ios-using-adal"></a><span data-ttu-id="4bf83-103">Hogyan tooenable adal-t használó IOS alkalmazások közötti SSO</span><span class="sxs-lookup"><span data-stu-id="4bf83-103">How tooenable cross-app SSO on iOS using ADAL</span></span>
<span data-ttu-id="4bf83-104">Egyszeri bejelentkezés (SSO) biztosít, így a felhasználók csak kell tooenter hitelesítő adataikkal egyszer, és ezen hitelesítő adatok automatikusan munkahelyi rendelkezik a különböző alkalmazások most várt ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="4bf83-104">Providing Single Sign-On (SSO) so that users only need tooenter their credentials once and have those credentials automatically work across applications is now expected by customers.</span></span> <span data-ttu-id="4bf83-105">hello nehéz kis képernyőjű, gyakran alkalommal együtt egy további tényezőt (2FA) például telefonhívást vagy egy fogva kódot, a felhasználónév és jelszó megadása eredményezi, ha egy felhasználó gyors kapcsolatos van toodo ennek a termékhez egynél többször.</span><span class="sxs-lookup"><span data-stu-id="4bf83-105">hello difficulty in entering their username and password on a small screen, often times combined with an additional factor (2FA) like a phone call or a texted code, results in quick dissatisfaction if a user has toodo this more than one time for your product.</span></span>

<span data-ttu-id="4bf83-106">Ezenkívül ha alkalmaz egy identitás-platform, amely más alkalmazások is használhatnak, például a Microsoft Accounts vagy az Office365 munkahelyi fiókkal, az ügyfelek várt, hogy ezen hitelesítő adatok toobe elérhető toouse az alkalmazások között nem számít, hello szállító.</span><span class="sxs-lookup"><span data-stu-id="4bf83-106">In addition, if you apply an identity platform that other applications may use such as Microsoft Accounts or a work account from Office365, customers expect that those credentials toobe available toouse across all their applications no matter hello vendor.</span></span>

<span data-ttu-id="4bf83-107">hello Microsoft Identity platform, a Microsoft Identity SDK-k, valamint elvégzi a rögzített munka, és képes toodelight egyszeri Bejelentkezést, vagy az alkalmazások vagy, mint a saját suite belül az ügyfelek hello biztosítja a broker funkció és a hitelesítő az alkalmazások, az eszköz teljes hello.</span><span class="sxs-lookup"><span data-stu-id="4bf83-107">hello Microsoft Identity platform, along with our Microsoft Identity SDKs, does all this hard work for you and gives you hello ability toodelight your customers with SSO either within your own suite of applications or, as with our broker capability and Authenticator applications, across hello entire device.</span></span>

<span data-ttu-id="4bf83-108">Ez a forgatókönyv megtudhatja, hogyan tooconfigure az SDK az alkalmazás tooprovide belül a juttatás tooyour ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="4bf83-108">This walkthrough will tell you how tooconfigure our SDK within your application tooprovide this benefit tooyour customers.</span></span>

<span data-ttu-id="4bf83-109">Ez a forgatókönyv a következőkre vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="4bf83-109">This walkthrough applies to:</span></span>

* <span data-ttu-id="4bf83-110">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4bf83-110">Azure Active Directory</span></span>
* <span data-ttu-id="4bf83-111">Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="4bf83-111">Azure Active Directory B2C</span></span>
* <span data-ttu-id="4bf83-112">Az Azure Active Directory B2B</span><span class="sxs-lookup"><span data-stu-id="4bf83-112">Azure Active Directory B2B</span></span>
* <span data-ttu-id="4bf83-113">Azure Active Directory feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="4bf83-113">Azure Active Directory Conditional Access</span></span>

<span data-ttu-id="4bf83-114">hello előző lépéseknél túl hogyan tudja[alkalmazások hello örökölt portálon az Azure Active Directory kiépítése](active-directory-how-to-integrate.md) és az alkalmazás integrálható hello [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).</span><span class="sxs-lookup"><span data-stu-id="4bf83-114">hello document preceding assumes you know how too[provision applications in hello legacy portal for Azure Active Directory](active-directory-how-to-integrate.md) and integrated your application with hello [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).</span></span>

## <a name="sso-concepts-in-hello-microsoft-identity-platform"></a><span data-ttu-id="4bf83-115">A Microsoft Identity Platform hello SSO fogalmak</span><span class="sxs-lookup"><span data-stu-id="4bf83-115">SSO Concepts in hello Microsoft Identity Platform</span></span>
### <a name="microsoft-identity-brokers"></a><span data-ttu-id="4bf83-116">Microsoft – identitáskezelési brókerek</span><span class="sxs-lookup"><span data-stu-id="4bf83-116">Microsoft Identity Brokers</span></span>
<span data-ttu-id="4bf83-117">A Microsoft lehetővé teszi a minden mobileszköz platform, amelyek lehetővé teszik a különböző alkalmazások különböző szállítóktól származó hello hídképzést, a hitelesítő adatok, és lehetővé teszi, hogy honnan egy biztonságos helyen igénylő különleges továbbfejlesztett funkciók toovalidate hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="4bf83-117">Microsoft provides applications for every mobile platform that allow for hello bridging of credentials across applications from different vendors and allows for special enhanced features that require a single secure place from where toovalidate credentials.</span></span> <span data-ttu-id="4bf83-118">Ezek nevezzük **brókerek**.</span><span class="sxs-lookup"><span data-stu-id="4bf83-118">We call these **brokers**.</span></span> <span data-ttu-id="4bf83-119">IOS és Android rendszeren a brókerek szolgáltatáson keresztül letölthető alkalmazás, hogy az ügyfelek telepítése függetlenül vagy egy vállalati, akár az összeset hello eszköz az alkalmazott felügyelő a toohello eszköz továbbíthatja.</span><span class="sxs-lookup"><span data-stu-id="4bf83-119">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed toohello device by a company who manages some or all of hello device for their employee.</span></span> <span data-ttu-id="4bf83-120">Ezek a brókerek kezelése biztonsági csak az egyes alkalmazások vagy hello: az eszköz teljes alapján a rendszergazdák törlését támogatja.</span><span class="sxs-lookup"><span data-stu-id="4bf83-120">These brokers support managing security just for some applications or hello entire device based on what IT Administrators desire.</span></span> <span data-ttu-id="4bf83-121">A Windows rendszerben a funkcióit egy toohello operációs rendszerben, technikailag néven hello Webeshitelesítés-szervező beépített fiók kiválasztásakor.</span><span class="sxs-lookup"><span data-stu-id="4bf83-121">In Windows, this functionality is provided by an account chooser built in toohello operating system, known technically as hello Web Authentication Broker.</span></span>

<span data-ttu-id="4bf83-122">További információt használjuk, a brókerek, és hogyan az ügyfelek előfordulhat, hogy lássák a bejelentkezési folyamat hello Microsoft Identity platform olvasni.</span><span class="sxs-lookup"><span data-stu-id="4bf83-122">For more information on how we use these brokers and how your customers might see them in their login flow for hello Microsoft Identity platform read on.</span></span>

### <a name="patterns-for-logging-in-on-mobile-devices"></a><span data-ttu-id="4bf83-123">Bejelentkezés mobileszközök minták</span><span class="sxs-lookup"><span data-stu-id="4bf83-123">Patterns for logging in on mobile devices</span></span>
<span data-ttu-id="4bf83-124">Hozzáférés toocredentials eszközökön két alapvető mintázatokból hello Microsoft Identity platform kövesse:</span><span class="sxs-lookup"><span data-stu-id="4bf83-124">Access toocredentials on devices follow two basic patterns for hello Microsoft Identity platform:</span></span>

* <span data-ttu-id="4bf83-125">Nem közvetített támogatott bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="4bf83-125">Non-broker assisted logins</span></span>
* <span data-ttu-id="4bf83-126">Támogatott bejelentkezések replikaszervező</span><span class="sxs-lookup"><span data-stu-id="4bf83-126">Broker assisted logins</span></span>

#### <a name="non-broker-assisted-logins"></a><span data-ttu-id="4bf83-127">Nem közvetített támogatott bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="4bf83-127">Non-broker assisted logins</span></span>
<span data-ttu-id="4bf83-128">Nem közvetített támogatott bejelentkezések hello alkalmazás együtt, beágyazottan történik, és hello helyi tárhelyet használó hello eszközön az alkalmazás bejelentkezési élmény.</span><span class="sxs-lookup"><span data-stu-id="4bf83-128">Non-broker assisted logins are login experiences that happen inline with hello application and use hello local storage on hello device for that application.</span></span> <span data-ttu-id="4bf83-129">Ez a tároló lehet osztani alkalmazásra, de hello hitelesítő adatok szorosan kötött toohello alkalmazás vagy csomag alkalmazásaikat, hogy a hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="4bf83-129">This storage may be shared across applications but hello credentials are tightly bound toohello app or suite of apps using that credential.</span></span> <span data-ttu-id="4bf83-130">Valószínűleg tapasztal Ez sok mobilalkalmazások a felhasználónév és jelszó maga hello alkalmazásban beírásakor.</span><span class="sxs-lookup"><span data-stu-id="4bf83-130">You've most likely experienced this in many mobile applications when you enter a username and password within hello application itself.</span></span>

<span data-ttu-id="4bf83-131">Ezek bejelentkezések a következő előnyöket hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="4bf83-131">These logins have hello following benefits:</span></span>

* <span data-ttu-id="4bf83-132">Felhasználói élmény létezik-e teljes mértékben hello alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="4bf83-132">User experience exists entirely within hello application.</span></span>
* <span data-ttu-id="4bf83-133">Hitelesítő adatok meg lehet osztani összes hello által aláírt alkalmazások ugyanazt a tanúsítványt, egy egyszeri bejelentkezéses élményt tooyour suite alkalmazások biztosítása.</span><span class="sxs-lookup"><span data-stu-id="4bf83-133">Credentials can be shared across applications that are signed by hello same certificate, providing a single sign-on experience tooyour suite of applications.</span></span>
* <span data-ttu-id="4bf83-134">Bejelentkezés hello élménye körül vezérlő toohello alkalmazás előtt és után bejelentkezhet valósul meg.</span><span class="sxs-lookup"><span data-stu-id="4bf83-134">Control around hello experience of logging in is provided toohello application before and after sign-in.</span></span>

<span data-ttu-id="4bf83-135">Ezek bejelentkezések a következő hátrányokkal hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="4bf83-135">These logins have hello following drawbacks:</span></span>

* <span data-ttu-id="4bf83-136">A felhasználó csak azokat az alkalmazás van-e beállítva a Microsoft Identities között a Microsoft Identity használó összes alkalmazások között nem számára az egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4bf83-136">User cannot experience single-sign on across all apps that use a Microsoft Identity, only across those Microsoft Identities that your application has configured.</span></span>
* <span data-ttu-id="4bf83-137">Az alkalmazás nem használható például a feltételes hozzáférés, vagy használjon hello InTune termékcsomagot speciális üzleti szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="4bf83-137">Your application cannot be used with more advanced business features such as Conditional Access or use hello InTune suite of products.</span></span>
* <span data-ttu-id="4bf83-138">Az alkalmazás nem támogatja a Tanúsítványalapú hitelesítés üzleti felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="4bf83-138">Your application can't support certificate-based authentication for business users.</span></span>

<span data-ttu-id="4bf83-139">Íme egy hello Microsoft Identity SDK-k működése megosztott hello tárolására az alkalmazások tooenable SSO ábrázolása:</span><span class="sxs-lookup"><span data-stu-id="4bf83-139">Here is a representation of how hello Microsoft Identity SDKs work with hello shared storage of your applications tooenable SSO:</span></span>

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| ADAL SDK  |  |  ADAL SDK  |  |  ADAK SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a><span data-ttu-id="4bf83-140">Támogatott bejelentkezések replikaszervező</span><span class="sxs-lookup"><span data-stu-id="4bf83-140">Broker assisted logins</span></span>
<span data-ttu-id="4bf83-141">Bejelentkezések Broker támogatású bejelentkezési élmény hello broker alkalmazáson belül és hello tárolási és biztonsági hello broker tooshare hitelesítő adatok használata minden alkalmazásra hello eszközön, amelyek érvényesek a Microsoft Identity platform hello.</span><span class="sxs-lookup"><span data-stu-id="4bf83-141">Broker-assisted logins are login experiences that occur within hello broker application and use hello storage and security of hello broker tooshare credentials across all applications on hello device that apply hello Microsoft Identity platform.</span></span> <span data-ttu-id="4bf83-142">Ez azt jelenti, hogy az alkalmazások támaszkodnak hello broker toosign felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="4bf83-142">This means that your applications rely on hello broker toosign users in.</span></span> <span data-ttu-id="4bf83-143">IOS és Android rendszeren a brókerek szolgáltatáson keresztül letölthető alkalmazás, hogy az ügyfelek telepítése függetlenül vagy olyan cég, amely kezeli a felhasználó hello az eszközön a toohello eszköz továbbíthatja.</span><span class="sxs-lookup"><span data-stu-id="4bf83-143">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed toohello device by a company who manages hello device for their user.</span></span> <span data-ttu-id="4bf83-144">Ez az alkalmazástípus példa: hello Microsoft Authenticator alkalmazás IOS rendszerű eszközökön.</span><span class="sxs-lookup"><span data-stu-id="4bf83-144">An example of this type of application is hello Microsoft Authenticator application on iOS.</span></span> <span data-ttu-id="4bf83-145">A Windows e funkcióit egy toohello operációs rendszerben, technikailag néven hello Webeshitelesítés-szervező beépített fiók kiválasztásakor.</span><span class="sxs-lookup"><span data-stu-id="4bf83-145">In Windows this functionality is provided by an account chooser built in toohello operating system, known technically as hello Web Authentication Broker.</span></span>
<span data-ttu-id="4bf83-146">hello élmény platformonként változó, és előfordulhat zavaró toousers nem esetén megfelelően.</span><span class="sxs-lookup"><span data-stu-id="4bf83-146">hello experience varies by platform and can sometimes be disruptive toousers if not managed correctly.</span></span> <span data-ttu-id="4bf83-147">Ismeri valószínűleg legtöbb ebben a mintában Ha hello Facebook-alkalmazás telepítve, és Facebook csatlakozni egy másik alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="4bf83-147">You're probably most familiar with this pattern if you have hello Facebook application installed and use Facebook Connect from another application.</span></span> <span data-ttu-id="4bf83-148">hello Microsoft – identitáskezelési platform által használt hello ugyanilyen mintájú.</span><span class="sxs-lookup"><span data-stu-id="4bf83-148">hello Microsoft Identity platform uses hello same pattern.</span></span>

<span data-ttu-id="4bf83-149">Az IOS-es ennek eredménye tooa "átmenet" animáció ahol az alkalmazás küldése közben hello Microsoft Authenticator alkalmazások toohello háttér elérhető lesz a hello felhasználói tooselect toohello előtér melyik fiókot szeretné toosign be.</span><span class="sxs-lookup"><span data-stu-id="4bf83-149">For iOS this leads tooa "transition" animation where your application is sent toohello background while hello Microsoft Authenticator applications comes toohello foreground for hello user tooselect which account they would like toosign in with.</span></span>  

<span data-ttu-id="4bf83-150">Android és Windows hello fiók kiválasztásakor az alkalmazás, amely kevésbé zavaró toohello felhasználói felett jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4bf83-150">For Android and Windows hello account chooser is displayed on top of your application which is less disruptive toohello user.</span></span>

#### <a name="how-hello-broker-gets-invoked"></a><span data-ttu-id="4bf83-151">Hogyan hello broker meghívott beolvasása</span><span class="sxs-lookup"><span data-stu-id="4bf83-151">How hello broker gets invoked</span></span>
<span data-ttu-id="4bf83-152">Ha egy kompatibilis broker hello eszköz, például Microsoft Authenticator alkalmazást, a Microsoft Identity SDK-k hello hello automatikusan telepítve van a hello hívásának hello broker meg, amikor a felhasználó azt jelzi, hogy kívánják toolog bármely fiókkal való munka hello Microsoft Identity platform.</span><span class="sxs-lookup"><span data-stu-id="4bf83-152">If a compatible broker is installed on hello device, like hello Microsoft Authenticator application, hello Microsoft Identity SDKs will automatically do hello work of invoking hello broker for you when a user indicates they wish toolog in using any account from hello Microsoft Identity platform.</span></span> <span data-ttu-id="4bf83-153">Ez a fiók lehet személyes Microsoft-Account, munkahelyi vagy iskolai fiókját, vagy egy fiókot, amely megadja és az Azure-ban B2C és B2B termékeink állomás.</span><span class="sxs-lookup"><span data-stu-id="4bf83-153">This account could be a personal Microsoft Account, a work or school account, or an account that you provide and host in Azure using our B2C and B2B products.</span></span> 

 #### <a name="how-we-ensure-hello-application-is-valid"></a><span data-ttu-id="4bf83-154">Hogyan biztosítható hello alkalmazás érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="4bf83-154">How we ensure hello application is valid</span></span>
 
 <span data-ttu-id="4bf83-155">hello kell tooensure hello identitását egy alkalmazás hívás hello broker nyújtunk a támogatott broker bejelentkezési adatok számára elengedhetetlen toohello biztonsági.</span><span class="sxs-lookup"><span data-stu-id="4bf83-155">hello need tooensure hello identity of an application call hello broker is crucial toohello security we provide in broker assisted logins.</span></span> <span data-ttu-id="4bf83-156">IOS és Android nem kényszeríti az egyedi azonosítók, amelyek csak egy adott alkalmazáshoz tartozó érvényes, így rosszindulatú alkalmazások előfordulhat, hogy "hamis" a jogos alkalmazásazonosító, és azt jelentette, hogy hello jogos alkalmazás hello jogkivonatokat fogadni.</span><span class="sxs-lookup"><span data-stu-id="4bf83-156">Neither iOS nor Android enforces unique identifiers that are valid only for a given application, so malicious applications may "spoof" a legitimate application's identifier and receive hello tokens meant for hello legitimate application.</span></span> <span data-ttu-id="4bf83-157">mindig tájékoztatjuk futásidőben hello jobb alkalmazással tooensure, kérjük hello fejlesztői tooprovide egyéni redirectURI Microsoft az alkalmazás regisztrálásakor.</span><span class="sxs-lookup"><span data-stu-id="4bf83-157">tooensure we are always communicating with hello right application at runtime, we ask hello developer tooprovide a custom redirectURI when registering their application with Microsoft.</span></span> <span data-ttu-id="4bf83-158">**Az alábbiakban tárgyalt hogyan a fejlesztők kézműipari az átirányítási URI-t kell.**</span><span class="sxs-lookup"><span data-stu-id="4bf83-158">**How developers should craft this redirect URI is discussed in detail below.**</span></span> <span data-ttu-id="4bf83-159">Az egyéni redirectURI hello hello alkalmazás alkalmazáscsomag-Azonosítót tartalmaz, és a hello Apple App Store toobe egyedi toohello alkalmazás biztosítja.</span><span class="sxs-lookup"><span data-stu-id="4bf83-159">This custom redirectURI contains hello Bundle ID of hello application and is ensured toobe unique toohello application by hello Apple App Store.</span></span> <span data-ttu-id="4bf83-160">Amikor egy alkalmazás hívások hello broker hello broker megkérdezi hello iOS operációs rendszer tooprovide azt hello hello broker nevű alkalmazáscsomag-Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="4bf83-160">When an application calls hello broker, hello broker asks hello iOS operating system tooprovide it with hello Bundle ID that called hello broker.</span></span> <span data-ttu-id="4bf83-161">hello broker hello hívás tooour identitásrendszere ezt az Alkalmazásköteg-Azonosítót tooMicrosoft nyújt.</span><span class="sxs-lookup"><span data-stu-id="4bf83-161">hello broker provides this Bundle ID tooMicrosoft in hello call tooour identity system.</span></span> <span data-ttu-id="4bf83-162">Ha hello hello alkalmazás Csomagazonosítóját nem egyezik meg a regisztráció során a Csomagazonosító toous hello fejlesztő megadott hello, azt megtagadja a hozzáférést toohello jogkivonatok hello erőforrás hello alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="4bf83-162">If hello Bundle ID of hello application does not match hello Bundle ID provided toous by hello developer during registration, we will deny access toohello tokens for hello resource hello application is requesting.</span></span> <span data-ttu-id="4bf83-163">Ez az ellenőrzés biztosítja, hogy csak a regisztrált hello fejlesztő hello alkalmazás-jogkivonatokat kap.</span><span class="sxs-lookup"><span data-stu-id="4bf83-163">This check ensures that only hello application registered by hello developer receives tokens.</span></span>

<span data-ttu-id="4bf83-164">**hello fejlesztői hello választott rendelkezik, ha a Microsoft Identity SDK hello hello broker meghívja vagy hello nem közvetített támogatott folyamat használja.**</span><span class="sxs-lookup"><span data-stu-id="4bf83-164">**hello developer has hello choice of if hello Microsoft Identity SDK calls hello broker or uses hello non-broker assisted flow.**</span></span> <span data-ttu-id="4bf83-165">Azonban ha hello fejlesztői nem toouse hello broker támogatású folyamata megszakad a hello előnye, hogy egyszeri bejelentkezési hitelesítő adataival, hogy hello a felhasználó már valószínűleg hello eszközön, és megakadályozza, hogy az alkalmazás használatát a Microsoft üzleti szolgáltatásokkal az ügyfelek, például a feltételes hozzáférés, a Intune kezelési képességek és a tanúsítvány alapú hitelesítést nyújt.</span><span class="sxs-lookup"><span data-stu-id="4bf83-165">However if hello developer chooses not toouse hello broker-assisted flow they lose hello benefit of using SSO credentials that hello user may have already added on hello device and prevents their application from being used with business features Microsoft provides its customers such as Conditional Access, Intune Management capabilities, and certificate-based authentication.</span></span>

<span data-ttu-id="4bf83-166">Ezek bejelentkezések a következő előnyöket hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="4bf83-166">These logins have hello following benefits:</span></span>

* <span data-ttu-id="4bf83-167">Felhasználói hello szállító függetlenül az alkalmazások közötti SSO észlel.</span><span class="sxs-lookup"><span data-stu-id="4bf83-167">User experiences SSO across all their applications no matter hello vendor.</span></span>
* <span data-ttu-id="4bf83-168">Az alkalmazás használhatja a Speciális üzleti szolgáltatásokat, mint a feltételes hozzáférés, de hello InTune termékcsomagot használja.</span><span class="sxs-lookup"><span data-stu-id="4bf83-168">Your application can use more advanced business features such as Conditional Access or use hello InTune suite of products.</span></span>
* <span data-ttu-id="4bf83-169">Az alkalmazás tanúsítvány alapú hitelesítést is támogatja az üzleti felhasználók.</span><span class="sxs-lookup"><span data-stu-id="4bf83-169">Your application can support certificate-based authentication for business users.</span></span>
* <span data-ttu-id="4bf83-170">Sokkal biztonságosabb bejelentkezés felhasználói élmény, hello alkalmazás- és hello felhasználói hello identitásának ellenőrzése hello broker alkalmazás további biztonsági algoritmusokkal és a titkosítás.</span><span class="sxs-lookup"><span data-stu-id="4bf83-170">Much more secure sign-in experience as hello identity of hello application and hello user are verified by hello broker application with additional security algorithms and encryption.</span></span>

<span data-ttu-id="4bf83-171">Ezek bejelentkezések a következő hátrányokkal hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="4bf83-171">These logins have hello following drawbacks:</span></span>

* <span data-ttu-id="4bf83-172">IOS-hello felhasználó van állapotra váltott ki az alkalmazás élmény közben a hitelesítő adatok közül választ.</span><span class="sxs-lookup"><span data-stu-id="4bf83-172">In iOS hello user is transitioned out of your application's experience while credentials are chosen.</span></span>
* <span data-ttu-id="4bf83-173">Hello képességét toomanage hello bejelentkezési megszűnését tapasztalhat az ügyfeleknek az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="4bf83-173">Loss of hello ability toomanage hello login experience for your customers within your application.</span></span>

<span data-ttu-id="4bf83-174">Íme egy hogyan hello Microsoft Identity SDK-k használata hello replikaszervező alkalmazások tooenable SSO ábrázolása:</span><span class="sxs-lookup"><span data-stu-id="4bf83-174">Here is a representation of how hello Microsoft Identity SDKs work with hello broker applications tooenable SSO:</span></span>

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
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

<span data-ttu-id="4bf83-175">Volt képes a háttér-információkat felvértezve meg kell tudni toobetter megismerheti és alkalmazhatja a SSO hello Microsoft Identity platform és SDK-k segítségével az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="4bf83-175">Armed with this background information you should be able toobetter understand and implement SSO within your application using hello Microsoft Identity platform and SDKs.</span></span>

## <a name="enabling-cross-app-sso-using-adal"></a><span data-ttu-id="4bf83-176">Adal-t használó alkalmazások közötti SSO engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4bf83-176">Enabling cross-app SSO using ADAL</span></span>
<span data-ttu-id="4bf83-177">Jelen példában használjuk hello ADAL iOS SDK számára:</span><span class="sxs-lookup"><span data-stu-id="4bf83-177">Here we use hello ADAL iOS SDK to:</span></span>

* <span data-ttu-id="4bf83-178">Nem közvetített támogatott egyszeri Bejelentkezést a csomag az alkalmazások bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="4bf83-178">Turn on non-broker assisted SSO for your suite of apps</span></span>
* <span data-ttu-id="4bf83-179">Kapcsolja be az SSO broker támogatású támogatása</span><span class="sxs-lookup"><span data-stu-id="4bf83-179">Turn on support for broker-assisted SSO</span></span>

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a><span data-ttu-id="4bf83-180">Nem közvetített egyszeri Bejelentkezést bekapcsolás támogatott az egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4bf83-180">Turning on SSO for non-broker assisted SSO</span></span>
<span data-ttu-id="4bf83-181">Nem közvetített támogatott egyszeri bejelentkezéshez alkalmazásra hello Microsoft Identity SDK-k kezelése nagy részét SSO hello összetettsége meg.</span><span class="sxs-lookup"><span data-stu-id="4bf83-181">For non-broker assisted SSO across applications hello Microsoft Identity SDKs manage much of hello complexity of SSO for you.</span></span> <span data-ttu-id="4bf83-182">Ez magában foglalja a hello gyorsítótár hello jobb felhasználói keresése és karbantartása az Ön tooquery bejelentkezett felhasználók listáját.</span><span class="sxs-lookup"><span data-stu-id="4bf83-182">This includes finding hello right user in hello cache and maintaining a list of logged in users for you tooquery.</span></span>

<span data-ttu-id="4bf83-183">egyszeri bejelentkezés tooenable alkalmazásra van szüksége a következő toodo hello saját:</span><span class="sxs-lookup"><span data-stu-id="4bf83-183">tooenable SSO across applications you own you need toodo hello following:</span></span>

1. <span data-ttu-id="4bf83-184">Győződjön meg arról az összes az alkalmazások felhasználói hello ugyanazon ügyfél-Azonosítót vagy az alkalmazás azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="4bf83-184">Ensure all your applications user hello same Client ID or Application ID.</span></span>
2. <span data-ttu-id="4bf83-185">Ügyeljen arra, hogy az alkalmazások megosztás hello azonos aláíró tanúsítvány az Apple-től, így keychains megoszthatja a</span><span class="sxs-lookup"><span data-stu-id="4bf83-185">Ensure that all of your applications share hello same signing certificate from Apple so that you can share keychains</span></span>
3. <span data-ttu-id="4bf83-186">Kérelem hello azonos kulcslánc jogosultság az összes az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="4bf83-186">Request hello same keychain entitlement for each of your applications.</span></span>
4. <span data-ttu-id="4bf83-187">Hogy hello Microsoft Identity SDK-k kapcsolatos hello megosztott kulcslánc velünk toouse.</span><span class="sxs-lookup"><span data-stu-id="4bf83-187">Tell hello Microsoft Identity SDKs about hello shared keychain you want us toouse.</span></span>

#### <a name="using-hello-same-client-id--application-id-for-all-hello-applications-in-your-suite-of-apps"></a><span data-ttu-id="4bf83-188">Ugyanazon ügyfél-azonosító segítségével hello / Application ID, az összes hello az alkalmazások a csomagban található alkalmazások</span><span class="sxs-lookup"><span data-stu-id="4bf83-188">Using hello same Client ID / Application ID for all hello applications in your suite of apps</span></span>
<span data-ttu-id="4bf83-189">Hello Microsoft Identity platform tooknow ahhoz, hogy az alkalmazások között tooshare jogkivonatok engedélyezett az alkalmazások egyes kell tooshare hello ugyanazon ügyfél-Azonosítót vagy az alkalmazás azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="4bf83-189">In order for hello Microsoft Identity platform tooknow that it's allowed tooshare tokens across your applications, each of your applications will need tooshare hello same Client ID or Application ID.</span></span> <span data-ttu-id="4bf83-190">Ez az egyedi azonosító hello tooyou megadott hello portálon az első alkalmazás regisztrálásakor.</span><span class="sxs-lookup"><span data-stu-id="4bf83-190">This is hello unique identifier that was provided tooyou when you registered your first application in hello portal.</span></span>

<span data-ttu-id="4bf83-191">Előfordulhat, hogy lehet szeretné megtudni, hogy hogyan határozható meg különböző alkalmazások toohello Ha használja a Microsoft Identity service hello ugyanazon alkalmazás azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="4bf83-191">You may be wondering how you will identify different apps toohello Microsoft Identity service if it uses hello same Application ID.</span></span> <span data-ttu-id="4bf83-192">hello válasz van hello **átirányítási URI-azonosítók**.</span><span class="sxs-lookup"><span data-stu-id="4bf83-192">hello answer is with hello **Redirect URIs**.</span></span> <span data-ttu-id="4bf83-193">Minden alkalmazás több átirányítási URI-azonosítók hello bevezetési portálon regisztrált rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="4bf83-193">Each application can have multiple Redirect URIs registered in hello onboarding portal.</span></span> <span data-ttu-id="4bf83-194">A csomagban található összes alkalmazás egy másik átirányítási URI-t fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="4bf83-194">Each app in your suite will have a different redirect URI.</span></span> <span data-ttu-id="4bf83-195">Ez megjelenésének például nem éri el:</span><span class="sxs-lookup"><span data-stu-id="4bf83-195">An example of how this looks is below:</span></span>

<span data-ttu-id="4bf83-196">Az App1 átirányítási URI-ja:`x-msauth-mytestiosapp://com.myapp.mytestapp`</span><span class="sxs-lookup"><span data-stu-id="4bf83-196">App1 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp`</span></span>

<span data-ttu-id="4bf83-197">App2 átirányítási URI-ja:`x-msauth-mytestiosapp://com.myapp.mytestapp2`</span><span class="sxs-lookup"><span data-stu-id="4bf83-197">App2 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp2`</span></span>

<span data-ttu-id="4bf83-198">App3 átirányítási URI-ja:`x-msauth-mytestiosapp://com.myapp.mytestapp3`</span><span class="sxs-lookup"><span data-stu-id="4bf83-198">App3 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp3`</span></span>

<span data-ttu-id="4bf83-199">....</span><span class="sxs-lookup"><span data-stu-id="4bf83-199">....</span></span>

<span data-ttu-id="4bf83-200">Ezek a beágyazott hello ugyanazon ügyfél-azonosító / Alkalmazásazonosítót és alapján hello kereshető átirányítási URI-címe toous SDK konfigurációs adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4bf83-200">These are nested under hello same client ID / application ID and looked up based on hello redirect URI you return toous in your SDK configuration.</span></span>

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


<span data-ttu-id="4bf83-201">*Vegye figyelembe, hogy az átirányítási URI-k formátuma hello ismertetésre vannak. Minden átirányítási URI-t használhatja kivéve toosupport hello broker kívánja, ebben az esetben azok kell kinéznie például a fenti hello*</span><span class="sxs-lookup"><span data-stu-id="4bf83-201">*Note that hello format of these Redirect URIs are explained below. You may use any Redirect URI unless you wish toosupport hello broker, in which case they must look something like hello above*</span></span>

#### <a name="create-keychain-sharing-between-applications"></a><span data-ttu-id="4bf83-202">A kulcslánc megosztása az alkalmazások között létrehozása</span><span class="sxs-lookup"><span data-stu-id="4bf83-202">Create keychain sharing between applications</span></span>
<span data-ttu-id="4bf83-203">A kulcslánc megosztása nem ez a dokumentum hello terjed, és a dokumentum az Apple által szabályozott [Adding Capabilities](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html).</span><span class="sxs-lookup"><span data-stu-id="4bf83-203">Enabling keychain sharing is beyond hello scope of this document and covered by Apple in their document [Adding Capabilities](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html).</span></span> <span data-ttu-id="4bf83-204">A fontos eldöntheti, milyen a kulcslánc toobe nevezik, és adja hozzá ezt a funkciót az alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="4bf83-204">What is important is that you decide what you want your keychain toobe called and add that capability across all your applications.</span></span>

<span data-ttu-id="4bf83-205">Ha készen áll, megfelelő jogosultságok tekintse meg a projektkönyvtárban jogosult a fájl `entitlements.plist` , amely tartalmazza, amelyet a következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="4bf83-205">When you do have entitlements set up correctly you should see a file in your project directory entitled `entitlements.plist` that contains something that looks like hello following:</span></span>

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

<span data-ttu-id="4bf83-206">Miután hello kulcslánc jogosultság minden alkalmazásból engedélyezve van, és készen áll a toouse SSO, adja hello Microsoft Identity SDK a kulcslánc-beállítás a következő hello segítségével a `ADAuthenticationSettings` a beállítás a következő hello:</span><span class="sxs-lookup"><span data-stu-id="4bf83-206">Once you have hello keychain entitlement enabled in each of your applications, and you are ready toouse SSO, tell hello Microsoft Identity SDK about your keychain by using hello following setting in your `ADAuthenticationSettings` with hello following setting:</span></span>

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> <span data-ttu-id="4bf83-207">Az alkalmazások között egy kulcslánc megosztásakor bármely alkalmazás törölheti felhasználók vagy rosszabb összes hello jogkivonatot az alkalmazás minden részében.</span><span class="sxs-lookup"><span data-stu-id="4bf83-207">When you share a keychain across your applications any application can delete users or worse delete all hello tokens across your application.</span></span> <span data-ttu-id="4bf83-208">Ez a különösen katasztrofális, ha a hello jogkivonatok toodo háttérműveletek használó alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="4bf83-208">This is particularly disastrous if you have applications that rely on hello tokens toodo background work.</span></span> <span data-ttu-id="4bf83-209">A kulcslánc megosztása azt jelenti, hogy a minden nagyon gondosan kell eltávolítási műveletek hello Microsoft Identity SDK-k használatával.</span><span class="sxs-lookup"><span data-stu-id="4bf83-209">Sharing a keychain means that you must be very careful in any and all remove operations through hello Microsoft Identity SDKs.</span></span>
> 
> 

<span data-ttu-id="4bf83-210">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="4bf83-210">That's it!</span></span> <span data-ttu-id="4bf83-211">a Microsoft Identity SDK hello most fájlmegosztási hitelesítő adatokat az alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="4bf83-211">hello Microsoft Identity SDK will now share credentials across all your applications.</span></span> <span data-ttu-id="4bf83-212">hello Felhasználólista lesznek megosztva alkalmazáspéldányok között.</span><span class="sxs-lookup"><span data-stu-id="4bf83-212">hello user list will also be shared across application instances.</span></span>

### <a name="turning-on-sso-for-broker-assisted-sso"></a><span data-ttu-id="4bf83-213">Egyszeri bejelentkezés SSO Broker bekapcsolása esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="4bf83-213">Turning on SSO for broker assisted SSO</span></span>
<span data-ttu-id="4bf83-214">az egy bármely Broker összetevője hello eszközön telepített alkalmazás toouse képesek hello **alapértelmezés szerint ki van kapcsolva**.</span><span class="sxs-lookup"><span data-stu-id="4bf83-214">hello ability for an application toouse any broker that is installed on hello device is **turned off by default**.</span></span> <span data-ttu-id="4bf83-215">A rendezés toouse az alkalmazás a hello broker kell tegye néhány további beállításra, és néhány kódot tooyour alkalmazás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="4bf83-215">In order toouse your application with hello broker you must do some additional configuration and add some code tooyour application.</span></span>

<span data-ttu-id="4bf83-216">hello lépéseket toofollow a következők:</span><span class="sxs-lookup"><span data-stu-id="4bf83-216">hello steps toofollow are:</span></span>

1. <span data-ttu-id="4bf83-217">Az alkalmazás kódjában hívás toohello MS SDK broker mód engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4bf83-217">Enable broker mode in your application code's call toohello MS SDK.</span></span>
2. <span data-ttu-id="4bf83-218">Állítson be egy új átirányítási URI-t, és tooboth hello alkalmazást és az alkalmazás regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="4bf83-218">Establish a new redirect URI and provide that tooboth hello app and your app registration.</span></span>
3. <span data-ttu-id="4bf83-219">Regisztrálás egy URL-sémát.</span><span class="sxs-lookup"><span data-stu-id="4bf83-219">Registering a URL Scheme.</span></span>
4. <span data-ttu-id="4bf83-220">iOS9 támogatási: Adjon hozzá egy engedély tooyour info.plist fájlt.</span><span class="sxs-lookup"><span data-stu-id="4bf83-220">iOS9 Support: Add a permission tooyour info.plist file.</span></span>

#### <a name="step-1-enable-broker-mode-in-your-application"></a><span data-ttu-id="4bf83-221">1. lépés: Az alkalmazás broker üzemmód engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4bf83-221">Step 1: Enable broker mode in your application</span></span>
<span data-ttu-id="4bf83-222">az alkalmazás toouse hello Broker hello lehetősége van kapcsolva, hello "context" vagy a kezdeti beállítás és a hitelesítési objektum létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="4bf83-222">hello ability for your application toouse hello broker is turned on when you create hello "context" or initial setup of your Authentication object.</span></span> <span data-ttu-id="4bf83-223">Ehhez úgy, hogy a hitelesítő adatok típusa a kódban:</span><span class="sxs-lookup"><span data-stu-id="4bf83-223">You do this by setting your credentials type in your code:</span></span>

```
/*! See hello ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
<span data-ttu-id="4bf83-224">Hello `AD_CREDENTIALS_AUTO` beállítás lehetővé teszi a hello Microsoft Identity SDK tootry toocall toohello broker kimenő `AD_CREDENTIALS_EMBEDDED` megakadályozza, hogy a Microsoft Identity SDK hello toohello broker hívja.</span><span class="sxs-lookup"><span data-stu-id="4bf83-224">hello `AD_CREDENTIALS_AUTO` setting will allow hello Microsoft Identity SDK tootry toocall out toohello broker, `AD_CREDENTIALS_EMBEDDED` will prevent hello Microsoft Identity SDK from calling toohello broker.</span></span>

#### <a name="step-2-registering-a-url-scheme"></a><span data-ttu-id="4bf83-225">2. lépés: Egy URL-séma regisztrálása</span><span class="sxs-lookup"><span data-stu-id="4bf83-225">Step 2: Registering a URL Scheme</span></span>
<span data-ttu-id="4bf83-226">hello Microsoft Identity platform URL-címek tooinvoke hello és a vezérlő majd térjen vissza tooyour alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="4bf83-226">hello Microsoft Identity platform uses URLs tooinvoke hello broker and then return control back tooyour application.</span></span> <span data-ttu-id="4bf83-227">toofinish, hogy egy URL-sémát kell oda-vissza regisztrálva az alkalmazáshoz, hogy a Microsoft Identity platform fog tudni hello.</span><span class="sxs-lookup"><span data-stu-id="4bf83-227">toofinish that round trip you need a URL scheme registered for your application that hello Microsoft Identity platform will know about.</span></span> <span data-ttu-id="4bf83-228">Ez lehet továbbá tooany más alkalmazás rendszerek, előfordulhat, hogy már korábban regisztrált az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="4bf83-228">This can be in addition tooany other app schemes you may have previously registered with your application.</span></span>

> [!WARNING]
> <span data-ttu-id="4bf83-229">Azt javasoljuk, hogy hello URL-cím sémája viszonylag egyedi toominimize hello esélyét hello használata egy másik alkalmazás azonos URL-sémát.</span><span class="sxs-lookup"><span data-stu-id="4bf83-229">We recommend making hello URL scheme fairly unique toominimize hello chances of another app using hello same URL scheme.</span></span> <span data-ttu-id="4bf83-230">Apple nem kényszeríti ki az URL-sémákat hello app Store-ból regisztrált hello egyediségét.</span><span class="sxs-lookup"><span data-stu-id="4bf83-230">Apple does not enforce hello uniqueness of URL schemes that are registered in hello app store.</span></span>
> 
> 

<span data-ttu-id="4bf83-231">Alább példája hogyan Ez jelenik meg a projekt konfigurációjában.</span><span class="sxs-lookup"><span data-stu-id="4bf83-231">Below is an example of how this appears in your project configuration.</span></span> <span data-ttu-id="4bf83-232">Előfordulhat, hogy is ehhez az xcode-ban, valamint:</span><span class="sxs-lookup"><span data-stu-id="4bf83-232">You may also do this in XCode as well:</span></span>

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a><span data-ttu-id="4bf83-233">3. lépés: Egy új átirányítási URI-t és az URL-séma létrehozása</span><span class="sxs-lookup"><span data-stu-id="4bf83-233">Step 3: Establish a new redirect URI with your URL Scheme</span></span>
<span data-ttu-id="4bf83-234">A sorrend tooensure, hogy a rendszer mindig visszaadja a hello credential jogkivonatok toohello megfelelő alkalmazás igazolnia kell a toomake meg arról, hogy a telepítésnek vissza oly módon, hogy az iOS operációs rendszer hello tooyour alkalmazás ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="4bf83-234">In order tooensure that we always return hello credential tokens toohello correct application, we need toomake sure we call back tooyour application in a way that hello iOS operating system can verify.</span></span> <span data-ttu-id="4bf83-235">hello iOS operációs rendszer jelentések toohello Microsoft broker alkalmazások hello hívja az hello alkalmazás Csomagazonosítóját.</span><span class="sxs-lookup"><span data-stu-id="4bf83-235">hello iOS operating system reports toohello Microsoft broker applications hello Bundle ID of hello application calling it.</span></span> <span data-ttu-id="4bf83-236">Ez a nem egy engedélyezetlen alkalmazás megtévesztésre.</span><span class="sxs-lookup"><span data-stu-id="4bf83-236">This cannot be spoofed by a rogue application.</span></span> <span data-ttu-id="4bf83-237">Ezért azt használja ki a hello az, hogy a hello jogkivonatok kerüljenek-e vissza a megfelelő alkalmazás toohello broker alkalmazás tooensure URI-val együtt.</span><span class="sxs-lookup"><span data-stu-id="4bf83-237">Therefore, we leverage this along with hello URI of our broker application tooensure that hello tokens are returned toohello correct application.</span></span> <span data-ttu-id="4bf83-238">Kérjük, tooestablish ez egyedi átirányítási URI-t, mind az alkalmazás és egy átirányítási URI-t, a developer portálon.</span><span class="sxs-lookup"><span data-stu-id="4bf83-238">We require you tooestablish this unique redirect URI both in your application and set as a Redirect URI in our developer portal.</span></span>

<span data-ttu-id="4bf83-239">Az átirányítási URI-t a hello megfelelő formátumban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="4bf83-239">Your redirect URI must be in hello proper form of:</span></span>

`<app-scheme>://<your.bundle.id>`

<span data-ttu-id="4bf83-240">például: *x-msauth-mytestiosapp://com.myapp.mytestapp*</span><span class="sxs-lookup"><span data-stu-id="4bf83-240">ex: *x-msauth-mytestiosapp://com.myapp.mytestapp*</span></span>

<span data-ttu-id="4bf83-241">Az átirányítási URI-t kell az alkalmazás-regisztrációk hello megadott toobe [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4bf83-241">This Redirect URI needs toobe specified in your app registration using hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="4bf83-242">További információ az Azure AD alkalmazás-regisztrációval, lásd: [integrálása az Azure Active Directoryval](active-directory-how-to-integrate.md).</span><span class="sxs-lookup"><span data-stu-id="4bf83-242">For more information on Azure AD app registration, see [Integrating with Azure Active Directory](active-directory-how-to-integrate.md).</span></span>

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-toosupport-certificate-based-authentication"></a><span data-ttu-id="4bf83-243">3a. lépés: az alkalmazás és a fejlesztői portál toosupport tanúsítvány alapú hitelesítést egy átirányítási URI hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4bf83-243">Step 3a: Add a redirect URI in your app and dev portal toosupport certificate based authentication</span></span>
<span data-ttu-id="4bf83-244">egy második "msauth" toosupport-Tanúsítványalapú hitelesítés kell regisztrálni az alkalmazás- és hello toobe [Azure-portálon](https://portal.azure.com/) toohandle tanúsítvány alapú hitelesítést, ha tooadd, amelyek támogatják az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4bf83-244">toosupport cert based authentication a second "msauth"  needs toobe registered in your application and hello [Azure portal](https://portal.azure.com/) toohandle certificate authentication if you wish tooadd that support in your application.</span></span>

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

<span data-ttu-id="4bf83-245">például: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*</span><span class="sxs-lookup"><span data-stu-id="4bf83-245">ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*</span></span>

#### <a name="step-4-ios9-add-a-configuration-parameter-tooyour-app"></a><span data-ttu-id="4bf83-246">4. lépés: iOS9: egy konfigurációs paraméter tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4bf83-246">Step 4: iOS9: Add a configuration parameter tooyour app</span></span>
<span data-ttu-id="4bf83-247">Adal-t használ – canOpenURL: toocheck Ha hello broker hello eszközön van telepítve.</span><span class="sxs-lookup"><span data-stu-id="4bf83-247">ADAL uses –canOpenURL: toocheck if hello broker is installed on hello device.</span></span> <span data-ttu-id="4bf83-248">Az iOS 9-es Apple zárolva mi is kereshet séma egy alkalmazáskészletet.</span><span class="sxs-lookup"><span data-stu-id="4bf83-248">In iOS 9 Apple locked down what schemes an application can query for.</span></span> <span data-ttu-id="4bf83-249">Szüksége lesz a tooadd "msauth" toohello info.plist szakasza a `info.plist file`.</span><span class="sxs-lookup"><span data-stu-id="4bf83-249">You will need tooadd “msauth” toohello LSApplicationQueriesSchemes section of your `info.plist file`.</span></span>

<span data-ttu-id="4bf83-250"><key>Info.plist</key></span><span class="sxs-lookup"><span data-stu-id="4bf83-250"><key>LSApplicationQueriesSchemes</key></span></span>

<span data-ttu-id="4bf83-251"><array><string>msauth</string>
</array></span><span class="sxs-lookup"><span data-stu-id="4bf83-251"><array> <string>msauth</string>
</array></span></span>

### <a name="youve-configured-sso"></a><span data-ttu-id="4bf83-252">Egyszeri bejelentkezés konfigurálása!</span><span class="sxs-lookup"><span data-stu-id="4bf83-252">You've configured SSO!</span></span>
<span data-ttu-id="4bf83-253">Most hello Microsoft Identity SDK automatikusan a hitelesítő adatok megosztása az alkalmazások között és hello broker meghívni, ha az megtalálható az eszközükön.</span><span class="sxs-lookup"><span data-stu-id="4bf83-253">Now hello Microsoft Identity SDK will automatically both share credentials across your applications and invoke hello broker if it's present on their device.</span></span>

