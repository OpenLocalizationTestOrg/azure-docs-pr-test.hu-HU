---
title: "Ismerkedés az Azure AD Android |} Microsoft Docs"
description: "Hogyan hozhat létre egy Android-alkalmazás, amely az Azure AD bejelentkezési és a hívások Azure AD számára az API-k OAuth használatával védett."
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 01/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 746cad19093fd2a1ad23ddd9412394f8d9da331c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="integrate-azure-ad-into-an-android-app"></a><span data-ttu-id="519fb-103">Az Azure AD integrálása Android-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="519fb-103">Integrate Azure AD into an Android app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> <span data-ttu-id="519fb-104">Az új az előzetes kiadás kipróbálásához [fejlesztői portálján](https://identity.microsoft.com/Docs/Android), amely segít, amelyekből megismerheti az Azure AD csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="519fb-104">Try the preview of our new [developer portal](https://identity.microsoft.com/Docs/Android), which will help you get up and running with Azure AD in just a few minutes.</span></span> <span data-ttu-id="519fb-105">A fejlesztői portálján végigvezeti a folyamat regisztrálja az alkalmazást, és az Azure AD integrálása a kódot.</span><span class="sxs-lookup"><span data-stu-id="519fb-105">The developer portal will walk you through the process of registering an app and integrating Azure AD into your code.</span></span> <span data-ttu-id="519fb-106">Amikor elkészült, akkor kell egy egyszerű alkalmazást, amely képes hitelesíteni a felhasználók számára a bérlő és a háttérből fogadni és-ellenőrzéshez.</span><span class="sxs-lookup"><span data-stu-id="519fb-106">When you’re finished, you'll have a simple application that can authenticate users in your tenant and a back end that can accept tokens and perform validation.</span></span>
>
>

<span data-ttu-id="519fb-107">Ha az asztali alkalmazások, Azure Active Directory (Azure AD) segítségével egyszerű és magától értetődő, hogy a felhasználók hitelesítése a helyszíni Active Directory-fiókok használatával.</span><span class="sxs-lookup"><span data-stu-id="519fb-107">If you're developing a desktop application, Azure Active Directory (Azure AD) makes it simple and straightforward for you to authenticate your users by using their on-premises Active Directory accounts.</span></span> <span data-ttu-id="519fb-108">Emellett lehetővé teszi az alkalmazás minden webes API-t az Azure AD, például az Office 365 API-k vagy az Azure API által védett biztonságosan felhasználását.</span><span class="sxs-lookup"><span data-stu-id="519fb-108">It also enables your application to securely consume any web API protected by Azure AD, such as the Office 365 APIs or the Azure API.</span></span>

<span data-ttu-id="519fb-109">Android-ügyfelek, amelyek a védett erőforrások eléréséhez szükséges az Azure AD az Active Directory Authentication Library (ADAL) biztosít.</span><span class="sxs-lookup"><span data-stu-id="519fb-109">For Android clients that need to access protected resources, Azure AD provides the Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="519fb-110">ADAL kizárólagos célja megkönnyíti a hozzáférési jogkivonatok lekérésére, az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="519fb-110">The sole purpose of ADAL is to make it easy for your app to get access tokens.</span></span> <span data-ttu-id="519fb-111">Annak bemutatásához, hogy milyen egyszerűen, azt fogja Android feladatlista alkalmazás létrehozásához, amely:</span><span class="sxs-lookup"><span data-stu-id="519fb-111">To demonstrate how easy it is, we’ll build an Android To-Do List application that:</span></span>

* <span data-ttu-id="519fb-112">Lekérdezi hozzáférési jogkivonatainak egy tennivalók listája API felület meghívásakor használatával a [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="519fb-112">Gets access tokens for calling a To-Do List API by using the [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="519fb-113">Lekérdezi a felhasználó tennivalók listájára.</span><span class="sxs-lookup"><span data-stu-id="519fb-113">Gets a user's to-do list.</span></span>
* <span data-ttu-id="519fb-114">Felhasználók jeleket.</span><span class="sxs-lookup"><span data-stu-id="519fb-114">Signs out users.</span></span>

<span data-ttu-id="519fb-115">A kezdéshez van szüksége, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="519fb-115">To get started, you need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="519fb-116">Ha még nem rendelkezik a bérlő [beszerzéséről egy](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="519fb-116">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a><span data-ttu-id="519fb-117">1. lépés: Töltse le és futtassa a Node.js REST API TODO minta kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="519fb-117">Step 1: Download and run the Node.js REST API TODO sample server</span></span>
<span data-ttu-id="519fb-118">A Node.js REST API TODO minta kifejezetten a meglévő mintát eredményez, amely a single-bérlő tennivaló REST API létrehozása az Azure AD dolgozhat írása.</span><span class="sxs-lookup"><span data-stu-id="519fb-118">The Node.js REST API TODO sample is written specifically to work against our existing sample for building a single-tenant To-Do REST API for Azure AD.</span></span> <span data-ttu-id="519fb-119">Ennek előfeltétele a gyors üzembe helyezését.</span><span class="sxs-lookup"><span data-stu-id="519fb-119">This is a prerequisite for the Quick Start.</span></span>

<span data-ttu-id="519fb-120">Beállítására kapcsolatos információkért lásd: a meglévő minták [Microsoft Azure Active Directory minta REST API szolgáltatás a Node.js](active-directory-devquickstarts-webapi-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="519fb-120">For information on how to set this up, see our existing samples in [Microsoft Azure Active Directory Sample REST API Service for Node.js](active-directory-devquickstarts-webapi-nodejs.md).</span></span>


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a><span data-ttu-id="519fb-121">2. lépés: A webes API regisztrálása az Azure AD-bérlő</span><span class="sxs-lookup"><span data-stu-id="519fb-121">Step 2: Register your web API with your Azure AD tenant</span></span>
<span data-ttu-id="519fb-122">Az Active Directory támogatja a két típusú alkalmazások hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="519fb-122">Active Directory supports adding two types of applications:</span></span>

- <span data-ttu-id="519fb-123">Webes API-t szolgáltatást kínál a felhasználók számára</span><span class="sxs-lookup"><span data-stu-id="519fb-123">Web APIs that offer services to users</span></span>
- <span data-ttu-id="519fb-124">(A webhely vagy az eszközön futó) alkalmazások, azokat elérő webes API-khoz</span><span class="sxs-lookup"><span data-stu-id="519fb-124">Applications (running either on the web or on a device) that access those web APIs</span></span>

<span data-ttu-id="519fb-125">Ebben a lépésben a webes API-t, hogy ez a minta tesztelési helyben fut éppen regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="519fb-125">In this step, you're registering the web API that you're running locally for testing this sample.</span></span> <span data-ttu-id="519fb-126">A webes API-k általában egy REST-szolgáltatást, amely az alkalmazások eléréséhez használni kívánt funkciót kínál.</span><span class="sxs-lookup"><span data-stu-id="519fb-126">Normally, this web API is a REST service that's offering functionality that you want an app to access.</span></span> <span data-ttu-id="519fb-127">Az Azure AD segítségével biztosíthatja a tetszőleges végpontot.</span><span class="sxs-lookup"><span data-stu-id="519fb-127">Azure AD can help protect any endpoint.</span></span>

<span data-ttu-id="519fb-128">Azt még feltéve, hogy van-e regisztrálása a Teendőlista REST API-t korábban hivatkozott.</span><span class="sxs-lookup"><span data-stu-id="519fb-128">We're assuming that you're registering the TODO REST API referenced earlier.</span></span> <span data-ttu-id="519fb-129">Azonban ez a webes API-k segítségével védheti az Azure Active Directory kívánt működik.</span><span class="sxs-lookup"><span data-stu-id="519fb-129">But this works for any web API that you want Azure Active Directory to help protect.</span></span>

1. <span data-ttu-id="519fb-130">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="519fb-130">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="519fb-131">A felső eszköztáron kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="519fb-131">On the top bar, click your account.</span></span> <span data-ttu-id="519fb-132">Az a **Directory** menüben válassza ki az Azure AD-bérlőt, ahová az alkalmazás regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="519fb-132">In the **Directory** list, choose the Azure AD tenant where you want to register your application.</span></span>
3. <span data-ttu-id="519fb-133">Kattintson a **több szolgáltatások** a bal oldali ablaktáblán, és válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="519fb-133">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="519fb-134">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="519fb-134">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="519fb-135">Adjon egy rövid nevet az alkalmazáshoz (például **TodoListService**) elemre, jelölje be **webalkalmazás és/vagy webes API**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="519fb-135">Enter a friendly name for the application (for example, **TodoListService**), select **Web Application and/or Web API**, and click **Next**.</span></span>
6. <span data-ttu-id="519fb-136">A bejelentkezési URL-címhez adja meg a minta az alap URL-címet.</span><span class="sxs-lookup"><span data-stu-id="519fb-136">For the sign-on URL, enter the base URL for the sample.</span></span> <span data-ttu-id="519fb-137">Alapértelmezés szerint ez a `https://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="519fb-137">By default, this is `https://localhost:8080`.</span></span>
7. <span data-ttu-id="519fb-138">Kattintson a **OK** a regisztráció befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="519fb-138">Click **OK** to complete the registration.</span></span>
8. <span data-ttu-id="519fb-139">Miközben továbbra is az Azure-portálon, nyissa meg az alkalmazás oldalát, keresse meg az alkalmazás azonosító értéket, és másolja.</span><span class="sxs-lookup"><span data-stu-id="519fb-139">While still in the Azure portal, go to your application page, find the application ID value, and copy it.</span></span> <span data-ttu-id="519fb-140">Ezt később szüksége az alkalmazás konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="519fb-140">You'll need this later when configuring your application.</span></span>
9. <span data-ttu-id="519fb-141">Az a **beállítások** -> **tulajdonságok** lapon, a app ID URI frissítése – adja meg `https://<your_tenant_name>/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="519fb-141">From the **Settings** -> **Properties** page, update the app ID URI - enter `https://<your_tenant_name>/TodoListService`.</span></span> <span data-ttu-id="519fb-142">Cserélje le `<your_tenant_name>` az Azure AD-bérlő nevét.</span><span class="sxs-lookup"><span data-stu-id="519fb-142">Replace `<your_tenant_name>` with the name of your Azure AD tenant.</span></span>

## <a name="step-3-register-the-sample-android-native-client-application"></a><span data-ttu-id="519fb-143">3. lépés: A minta Android Native Client alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="519fb-143">Step 3: Register the sample Android Native Client application</span></span>
<span data-ttu-id="519fb-144">Ez a példa regisztrálnia kell a webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="519fb-144">You must register your web application in this sample.</span></span> <span data-ttu-id="519fb-145">Ez lehetővé teszi az alkalmazás a most regisztrált webes API-k folytatott kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="519fb-145">This allows your application to communicate with the just-registered web API.</span></span> <span data-ttu-id="519fb-146">Az Azure AD utasíthatja el lehetővé teszik az alkalmazás kérni a bejelentkezéshez, kivéve, ha regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="519fb-146">Azure AD will refuse to even allow your application to ask for sign-in unless it's registered.</span></span> <span data-ttu-id="519fb-147">A biztonsági modell részét képező.</span><span class="sxs-lookup"><span data-stu-id="519fb-147">That's part of the security of the model.</span></span>

<span data-ttu-id="519fb-148">Azt még feltéve, hogy van-e regisztrálása korábban hivatkozott mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="519fb-148">We're assuming that you're registering the sample application referenced earlier.</span></span> <span data-ttu-id="519fb-149">De bármely alkalmazás, amely kidolgozása Ez az eljárás használható.</span><span class="sxs-lookup"><span data-stu-id="519fb-149">But this procedure works for any app that you're developing.</span></span>

> [!NOTE]
> <span data-ttu-id="519fb-150">Először talán miért kívánja menteni egy alkalmazás és a webes API-k egy bérlő.</span><span class="sxs-lookup"><span data-stu-id="519fb-150">You might wonder why you're putting both an application and a web API in one tenant.</span></span> <span data-ttu-id="519fb-151">Mivel előfordulhat, hogy Ön rendelkezik kitalál, egy alkalmazás olyan külső API-bérlőhöz egy másik Azure AD-ben regisztrált hozzáférő hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="519fb-151">As you might have guessed, you can build an app that accesses an external API that is registered in Azure AD from another tenant.</span></span> <span data-ttu-id="519fb-152">Ha így tesz, az ügyfelek az API-nak az alkalmazás használatához beleegyezését kéri.</span><span class="sxs-lookup"><span data-stu-id="519fb-152">If you do that, your customers will be prompted to consent to the use of the API in the application.</span></span> <span data-ttu-id="519fb-153">Az IOS rendszerhez készült Active Directory Authentication Library gondoskodik a hozzájárulásukat adják meg.</span><span class="sxs-lookup"><span data-stu-id="519fb-153">Active Directory Authentication Library for iOS takes care of this consent for you.</span></span> <span data-ttu-id="519fb-154">Összetettebb funkciók megismeréséhez azt láthatja, hogy ez az egyik fontos része a munka Azure és az Office, valamint az egyéb szolgáltató a Microsoft APIs programcsomag eléréséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="519fb-154">As we explore more advanced features, you'll see that this is an important part of the work needed to access the suite of Microsoft APIs from Azure and Office, as well as any other service provider.</span></span> <span data-ttu-id="519fb-155">Most mert mind a webes API-t, és ugyanannak a bérlőnek, az alkalmazást regisztrálni nem jelenik meg semmilyen beleegyezést kér.</span><span class="sxs-lookup"><span data-stu-id="519fb-155">For now, because you registered both your web API and your application under the same tenant, you won't see any prompts for consent.</span></span> <span data-ttu-id="519fb-156">Általában ez a helyzet, ha az alkalmazás csak a saját vállalati használatára.</span><span class="sxs-lookup"><span data-stu-id="519fb-156">This is usually the case if you're developing an application just for your own company to use.</span></span>

1. <span data-ttu-id="519fb-157">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="519fb-157">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="519fb-158">A felső eszköztáron kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="519fb-158">On the top bar, click your account.</span></span> <span data-ttu-id="519fb-159">Az a **Directory** menüben válassza ki az Azure AD-bérlőt, ahová az alkalmazás regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="519fb-159">In the **Directory** list, choose the Azure AD tenant where you want to register your application.</span></span>
3. <span data-ttu-id="519fb-160">Kattintson a **több szolgáltatások** a bal oldali ablaktáblán, és válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="519fb-160">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="519fb-161">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="519fb-161">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="519fb-162">Adjon egy rövid nevet az alkalmazáshoz (például **TodoListClient-Android**) elemre, jelölje be **natív ügyfélalkalmazás**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="519fb-162">Enter a friendly name for the application (for example, **TodoListClient-Android**), select **Native Client Application**, and click **Next**.</span></span>
6. <span data-ttu-id="519fb-163">Az átirányítási URI-t, írja be `http://TodoListClient`.</span><span class="sxs-lookup"><span data-stu-id="519fb-163">For the redirect URI, enter `http://TodoListClient`.</span></span> <span data-ttu-id="519fb-164">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="519fb-164">Click **Finish**.</span></span>
7. <span data-ttu-id="519fb-165">Az alkalmazás lapon keresse meg az alkalmazás azonosító értéket, és másolja azt.</span><span class="sxs-lookup"><span data-stu-id="519fb-165">From the application page, find the application ID value and copy it.</span></span> <span data-ttu-id="519fb-166">Ezt később szüksége az alkalmazás konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="519fb-166">You'll need this later when configuring your application.</span></span>
8. <span data-ttu-id="519fb-167">Az a **beállítások** lapon jelölje be **szükséges engedélyek** válassza **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="519fb-167">From the **Settings** page, select **Required Permissions** and select **Add**.</span></span>  <span data-ttu-id="519fb-168">Keresse meg és jelölje ki a TodoListService, vegye fel a **hozzáférés TodoListService** engedélyt a **delegált engedélyek**, és kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="519fb-168">Locate and select TodoListService, add the **Access TodoListService** permission under **Delegated Permissions**, and click **Done**.</span></span>

<span data-ttu-id="519fb-169">A Maven build, használhatja a legfelső szinten pom.xml:</span><span class="sxs-lookup"><span data-stu-id="519fb-169">To build with Maven, you can use pom.xml at the top level:</span></span>

1. <span data-ttu-id="519fb-170">A tárház klónozása egy olyan könyvtárba, az Ön által választott:</span><span class="sxs-lookup"><span data-stu-id="519fb-170">Clone this repo into a directory of your choice:</span></span>

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  
2. <span data-ttu-id="519fb-171">Kövesse a [a Maven környezet beállítása Androidhoz készült előfeltételei](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).</span><span class="sxs-lookup"><span data-stu-id="519fb-171">Follow the steps in the [prerequisites to set up your Maven environment for Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).</span></span>
3. <span data-ttu-id="519fb-172">Az SDK 19 emulátor beállítása.</span><span class="sxs-lookup"><span data-stu-id="519fb-172">Set up the emulator with SDK 19.</span></span>
4. <span data-ttu-id="519fb-173">Nyissa meg a gyökérmappájába, ahol a tárházban klónozott.</span><span class="sxs-lookup"><span data-stu-id="519fb-173">Go to the root folder where you cloned the repo.</span></span>
5. <span data-ttu-id="519fb-174">Futtassa ezt a parancsot:`mvn clean install`</span><span class="sxs-lookup"><span data-stu-id="519fb-174">Run this command: `mvn clean install`</span></span>
6. <span data-ttu-id="519fb-175">Módosítsa a könyvtárat arra a gyors üzembe helyezési minta:`cd samples\hello`</span><span class="sxs-lookup"><span data-stu-id="519fb-175">Change the directory to the Quick Start sample: `cd samples\hello`</span></span>
7. <span data-ttu-id="519fb-176">Futtassa ezt a parancsot:`mvn android:deploy android:run`</span><span class="sxs-lookup"><span data-stu-id="519fb-176">Run this command: `mvn android:deploy android:run`</span></span>

   <span data-ttu-id="519fb-177">Meg kell jelennie az alkalmazás elindítása.</span><span class="sxs-lookup"><span data-stu-id="519fb-177">You should see the app starting.</span></span>
8. <span data-ttu-id="519fb-178">Adja meg a teszt felhasználói adatokkal.</span><span class="sxs-lookup"><span data-stu-id="519fb-178">Enter test user credentials to try.</span></span>

<span data-ttu-id="519fb-179">JAR csomagok nyújtanak a AAR csomag mellett.</span><span class="sxs-lookup"><span data-stu-id="519fb-179">JAR packages will be submitted beside the AAR package.</span></span>

## <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a><span data-ttu-id="519fb-180">4. lépés: Töltse le az Android ADAL, és adja hozzá az Eclipse-munkaterület</span><span class="sxs-lookup"><span data-stu-id="519fb-180">Step 4: Download the Android ADAL and add it to your Eclipse workspace</span></span>
<span data-ttu-id="519fb-181">Hajtottunk, könnyen használható adal-t használni az Android-projekt több lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="519fb-181">We've made it easy for you to have multiple options to use ADAL in your Android project:</span></span>

* <span data-ttu-id="519fb-182">A forráskód segítségével ezt a szalagtárat importálása eclipse-ben és a hivatkozás az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="519fb-182">You can use the source code to import this library into Eclipse and link to your application.</span></span>
* <span data-ttu-id="519fb-183">Android Studio használata, AAR csomag formátumot használja, és a bináris fájlok hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="519fb-183">If you're using Android Studio, you can use the AAR package format and reference the binaries.</span></span>

### <a name="option-1-source-zip"></a><span data-ttu-id="519fb-184">1. lehetőség: Forrás Zip</span><span class="sxs-lookup"><span data-stu-id="519fb-184">Option 1: Source Zip</span></span>
<span data-ttu-id="519fb-185">Letöltheti a forráskódot, kattintson a **töltse le a ZIP-** a lap jobb oldalán.</span><span class="sxs-lookup"><span data-stu-id="519fb-185">To download a copy of the source code, click **Download ZIP** on the right side of the page.</span></span> <span data-ttu-id="519fb-186">Illetve [töltse le a Githubról](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).</span><span class="sxs-lookup"><span data-stu-id="519fb-186">Or you can [download from GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).</span></span>

### <a name="option-2-source-via-git"></a><span data-ttu-id="519fb-187">2. lehetőség: Forrás Git keresztül</span><span class="sxs-lookup"><span data-stu-id="519fb-187">Option 2: Source via Git</span></span>
<span data-ttu-id="519fb-188">Ahhoz, hogy az SDK segítségével Git forráskódját, írja be:</span><span class="sxs-lookup"><span data-stu-id="519fb-188">To get the source code of the SDK via Git, type:</span></span>

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

### <a name="option-3-binaries-via-gradle"></a><span data-ttu-id="519fb-189">3. lehetőség: Bináris Gradle keresztül</span><span class="sxs-lookup"><span data-stu-id="519fb-189">Option 3: Binaries via Gradle</span></span>
<span data-ttu-id="519fb-190">A bináris fájlok lekérheti a Maven központi tárházban.</span><span class="sxs-lookup"><span data-stu-id="519fb-190">You can get the binaries from the Maven central repo.</span></span> <span data-ttu-id="519fb-191">Az alábbiak szerint a AAR csomagot is tartalmazza a projekt az Android Studio:</span><span class="sxs-lookup"><span data-stu-id="519fb-191">The AAR package can be included as follows in your project in Android Studio:</span></span>

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

### <a name="option-4-aar-via-maven"></a><span data-ttu-id="519fb-192">4. lehetőség: AAR Maven keresztül</span><span class="sxs-lookup"><span data-stu-id="519fb-192">Option 4: AAR via Maven</span></span>
<span data-ttu-id="519fb-193">A beépülő modul M2Eclipse használata, a függőség adhat meg a pom.xml fájlt:</span><span class="sxs-lookup"><span data-stu-id="519fb-193">If you're using the M2Eclipse plug-in, you can specify the dependency in your pom.xml file:</span></span>

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


### <a name="option-5-jar-package-inside-the-libs-folder"></a><span data-ttu-id="519fb-194">5. lehetőség: JAR csomag az függvénytárak mappában</span><span class="sxs-lookup"><span data-stu-id="519fb-194">Option 5: JAR package inside the libs folder</span></span>
<span data-ttu-id="519fb-195">A JAR-fájlra beszerezni a Maven-tárház, és helyezze be a **függvénytárak** a projekt mappájára.</span><span class="sxs-lookup"><span data-stu-id="519fb-195">You can get the JAR file from the Maven repo and drop it into the **libs** folder in your project.</span></span> <span data-ttu-id="519fb-196">Meg kell másolnia a szükséges erőforrások a projekthez, valamint a JAR-csomagok nem tartalmazza azokat.</span><span class="sxs-lookup"><span data-stu-id="519fb-196">You need to copy the required resources to your project as well, because the JAR packages don't include them.</span></span>

## <a name="step-5-add-references-to-android-adal-to-your-project"></a><span data-ttu-id="519fb-197">5. lépés: Az Android ADAL mutató hivatkozások hozzáadása a projekthez</span><span class="sxs-lookup"><span data-stu-id="519fb-197">Step 5: Add references to Android ADAL to your project</span></span>
1. <span data-ttu-id="519fb-198">Vegye fel a projektbe egy hivatkozást, és adja meg azt az Android tárként.</span><span class="sxs-lookup"><span data-stu-id="519fb-198">Add a reference to your project and specify it as an Android library.</span></span> <span data-ttu-id="519fb-199">Ha bizonytalan ennek módjáról, kaphat további információt a [Android Studio hely](http://developer.android.com/tools/projects/projects-eclipse.html).</span><span class="sxs-lookup"><span data-stu-id="519fb-199">If you're uncertain how to do this, you can get more information on the [Android Studio site](http://developer.android.com/tools/projects/projects-eclipse.html).</span></span>
2. <span data-ttu-id="519fb-200">Adja hozzá a projekt függőség a projektbeállításokat a hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="519fb-200">Add the project dependency for debugging into your project settings.</span></span>
3. <span data-ttu-id="519fb-201">A projekt AndroidManifest.xml fájl frissítése:</span><span class="sxs-lookup"><span data-stu-id="519fb-201">Update your project's AndroidManifest.xml file to include:</span></span>

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
            ....
        <application/>

4. <span data-ttu-id="519fb-202">A fő tevékenységnél AuthenticationContext példányának létrehozása.</span><span class="sxs-lookup"><span data-stu-id="519fb-202">Create an instance of AuthenticationContext at your main activity.</span></span> <span data-ttu-id="519fb-203">A hívás részleteit túlmutat a jelen témakör, de remek kezdőpont kaphat megnézi a [Android Native Client minta](https://github.com/AzureADSamples/NativeClient-Android).</span><span class="sxs-lookup"><span data-stu-id="519fb-203">The details of this call are beyond the scope of this topic, but you can get a good start by looking at the [Android Native Client sample](https://github.com/AzureADSamples/NativeClient-Android).</span></span> <span data-ttu-id="519fb-204">A következő példában SharedPreferences az alapértelmezett gyorsítótár, továbbá hatóság formájában `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:</span><span class="sxs-lookup"><span data-stu-id="519fb-204">In the following example, SharedPreferences is the default cache, and Authority is in the form of `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:</span></span>

    `mContext = new AuthenticationContext(MainActivity.this, authority, true); // mContext is a field in your activity`

5. <span data-ttu-id="519fb-205">Másolja a kódblokk AuthenticationActivity végén kezelni, a felhasználó megadja hitelesítő adatait, és megkapja az engedélyezési kód után:</span><span class="sxs-lookup"><span data-stu-id="519fb-205">Copy this code block to handle the end of AuthenticationActivity after the user enters credentials and receives an authorization code:</span></span>

        @Override
         protected void onActivityResult(int requestCode, int resultCode, Intent data) {
             super.onActivityResult(requestCode, resultCode, data);
             if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
             }
         }

6. <span data-ttu-id="519fb-206">Kérje meg a jogkivonat, definiálni kell egy visszahívási:</span><span class="sxs-lookup"><span data-stu-id="519fb-206">To ask for a token, define a callback:</span></span>

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };

7. <span data-ttu-id="519fb-207">Végül kérje meg a jogkivonat, hogy a visszahívás használatával:</span><span class="sxs-lookup"><span data-stu-id="519fb-207">Finally, ask for a token by using that callback:</span></span>

    `mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                   callback);`

<span data-ttu-id="519fb-208">A paraméterek leírását itt található:</span><span class="sxs-lookup"><span data-stu-id="519fb-208">Here's an explanation of the parameters:</span></span>

* <span data-ttu-id="519fb-209">*erőforrás* szükséges, azonban az erőforrás elérésére tett kísérlet.</span><span class="sxs-lookup"><span data-stu-id="519fb-209">*resource* is required and is the resource you're trying to access.</span></span>
* <span data-ttu-id="519fb-210">*ClientID* szükség, és az Azure AD származik.</span><span class="sxs-lookup"><span data-stu-id="519fb-210">*clientid* is required and comes from Azure AD.</span></span>
* <span data-ttu-id="519fb-211">*RedirectUri* nincs szükség a acquireToken hívásához meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="519fb-211">*RedirectUri* is not required to be provided for the acquireToken call.</span></span> <span data-ttu-id="519fb-212">Állíthat be, mint a csomag neve.</span><span class="sxs-lookup"><span data-stu-id="519fb-212">You can set it up as your package name.</span></span>
* <span data-ttu-id="519fb-213">*PromptBehavior* kérje meg a hitelesítő adatokat hagyja ki a gyorsítótárat, és a cookie-k segítségével.</span><span class="sxs-lookup"><span data-stu-id="519fb-213">*PromptBehavior* helps to ask for credentials to skip the cache and cookie.</span></span>
* <span data-ttu-id="519fb-214">*a visszahívási* után az engedélyezési kód cseréje a jogkivonat neve.</span><span class="sxs-lookup"><span data-stu-id="519fb-214">*callback* is called after the authorization code is exchanged for a token.</span></span> <span data-ttu-id="519fb-215">AuthenticationResult, amelynek hozzáférési jogkivonat objektum rendelkezik, lejárt, és a lexikális elem adatainak azonosító.</span><span class="sxs-lookup"><span data-stu-id="519fb-215">It has an object of AuthenticationResult, which has access token, date expired, and ID token information.</span></span>
* <span data-ttu-id="519fb-216">*acquireTokenSilent* nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="519fb-216">*acquireTokenSilent* is optional.</span></span> <span data-ttu-id="519fb-217">Hívása akkor leíró gyorsítótárazás és a token frissítése.</span><span class="sxs-lookup"><span data-stu-id="519fb-217">You can call it to handle caching and token refresh.</span></span> <span data-ttu-id="519fb-218">A Sync szolgáltatás verzióját is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="519fb-218">It also provides the sync version.</span></span> <span data-ttu-id="519fb-219">Elfogadja a *userId* paraméterként.</span><span class="sxs-lookup"><span data-stu-id="519fb-219">It accepts *userId* as a parameter.</span></span>

        mContext.acquireTokenSilent(resource, clientid, userId, callback );

<span data-ttu-id="519fb-220">Ez a forgatókönyv segítségével kell mi sikeresen integrálni kell az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="519fb-220">By using this walkthrough, you should have what you need to successfully integrate with Azure Active Directory.</span></span> <span data-ttu-id="519fb-221">További példák a, keresse fel a AzureADSamples / GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="519fb-221">For more examples of this working, visit the AzureADSamples/ repository on GitHub.</span></span>

## <a name="important-information"></a><span data-ttu-id="519fb-222">Fontos információk</span><span class="sxs-lookup"><span data-stu-id="519fb-222">Important information</span></span>
### <a name="customization"></a><span data-ttu-id="519fb-223">Testreszabás</span><span class="sxs-lookup"><span data-stu-id="519fb-223">Customization</span></span>
<span data-ttu-id="519fb-224">Az alkalmazás-erőforrásokat felülírhatnak-e projekt könyvtárerőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="519fb-224">Your application resources can overwrite library project resources.</span></span> <span data-ttu-id="519fb-225">Ez akkor fordul elő, amikor az alkalmazás éppen készül.</span><span class="sxs-lookup"><span data-stu-id="519fb-225">This happens when your app is being built.</span></span> <span data-ttu-id="519fb-226">Emiatt testre hitelesítési tevékenység elrendezés a kívánt módon.</span><span class="sxs-lookup"><span data-stu-id="519fb-226">For this reason, you can customize authentication activity layout the way you want.</span></span> <span data-ttu-id="519fb-227">Mindenképpen tartsa a vezérlők Azonosítóját, hogy az ADAL által használt (webes Nézet).</span><span class="sxs-lookup"><span data-stu-id="519fb-227">Be sure to keep the ID of the controls that ADAL uses (WebView).</span></span>

### <a name="broker"></a><span data-ttu-id="519fb-228">Broker</span><span class="sxs-lookup"><span data-stu-id="519fb-228">Broker</span></span>
<span data-ttu-id="519fb-229">A Microsoft Intune vállalati portál alkalmazást a broker összetevő biztosít.</span><span class="sxs-lookup"><span data-stu-id="519fb-229">The Microsoft Intune Company Portal app provides the broker component.</span></span> <span data-ttu-id="519fb-230">A fiók AccountManager jön létre.</span><span class="sxs-lookup"><span data-stu-id="519fb-230">The account is created in AccountManager.</span></span> <span data-ttu-id="519fb-231">A fiók típusa nem "com.microsoft.workaccount."</span><span class="sxs-lookup"><span data-stu-id="519fb-231">The account type is "com.microsoft.workaccount."</span></span> <span data-ttu-id="519fb-232">AccountManager lehetővé teszi, hogy csak egyetlen egyszeri bejelentkezési fiók.</span><span class="sxs-lookup"><span data-stu-id="519fb-232">AccountManager allows only a single SSO account.</span></span> <span data-ttu-id="519fb-233">A felhasználó számára az egyszeri bejelentkezési cookie létrehozza az alkalmazások közül legalább egy eszköz challenge befejezése után.</span><span class="sxs-lookup"><span data-stu-id="519fb-233">It creates an SSO cookie for the user after completing the device challenge for one of the apps.</span></span>

<span data-ttu-id="519fb-234">ADAL használja az átvitelszervező-fiókot, ha egy felhasználói fiók jön létre, és ez a hitelesítő úgy, hogy nem hagyja ki.</span><span class="sxs-lookup"><span data-stu-id="519fb-234">ADAL uses the broker account if one user account is created at this authenticator and you choose not to skip it.</span></span> <span data-ttu-id="519fb-235">Kihagyhatja a broker felhasználót:</span><span class="sxs-lookup"><span data-stu-id="519fb-235">You can skip the broker user with:</span></span>

   `AuthenticationSettings.Instance.setSkipBroker(true);`

<span data-ttu-id="519fb-236">Egy különös RedirectUri broker használati regisztrálnia kell.</span><span class="sxs-lookup"><span data-stu-id="519fb-236">You need to register a special RedirectUri for broker usage.</span></span> <span data-ttu-id="519fb-237">RedirectUri formátumban van `msauth://packagename/Base64UrlencodedSignature`.</span><span class="sxs-lookup"><span data-stu-id="519fb-237">RedirectUri is in the format of `msauth://packagename/Base64UrlencodedSignature`.</span></span> <span data-ttu-id="519fb-238">A RedirectUri kaphat az alkalmazás a parancsfájl brokerRedirectPrint.ps1 vagy az API-hívás mContext.getBrokerRedirectUri használatával.</span><span class="sxs-lookup"><span data-stu-id="519fb-238">You can get your RedirectUri for your app by using the script brokerRedirectPrint.ps1 or the API call mContext.getBrokerRedirectUri.</span></span> <span data-ttu-id="519fb-239">Az aláírás nem kapcsolódik az aláírási tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="519fb-239">The signature is related to your signing certificates.</span></span>

<span data-ttu-id="519fb-240">Az aktuális broker modell csak egy felhasználóhoz.</span><span class="sxs-lookup"><span data-stu-id="519fb-240">The current broker model is for one user.</span></span> <span data-ttu-id="519fb-241">AuthenticationContext biztosít a API-módszer segítségével a broker felhasználó.</span><span class="sxs-lookup"><span data-stu-id="519fb-241">AuthenticationContext provides the API method to get the broker user.</span></span>

   `String brokerAccount =  mContext.getBrokerUser(); //Broker user is returned if account is valid.`

<span data-ttu-id="519fb-242">Az alkalmazás jegyzékének AccountManager fiókok használatára a következő engedélyekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="519fb-242">Your app manifest should have the following permissions to use AccountManager accounts.</span></span> <span data-ttu-id="519fb-243">További információkért lásd: a [AccountManager az információt az Android](http://developer.android.com/reference/android/accounts/AccountManager.html).</span><span class="sxs-lookup"><span data-stu-id="519fb-243">For details, see the [AccountManager information on the Android site](http://developer.android.com/reference/android/accounts/AccountManager.html).</span></span>

* <span data-ttu-id="519fb-244">GET_ACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="519fb-244">GET_ACCOUNTS</span></span>
* <span data-ttu-id="519fb-245">USE_CREDENTIALS</span><span class="sxs-lookup"><span data-stu-id="519fb-245">USE_CREDENTIALS</span></span>
* <span data-ttu-id="519fb-246">MANAGE_ACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="519fb-246">MANAGE_ACCOUNTS</span></span>

### <a name="authority-url-and-ad-fs"></a><span data-ttu-id="519fb-247">Szolgáltató URL-címe és az AD FS</span><span class="sxs-lookup"><span data-stu-id="519fb-247">Authority URL and AD FS</span></span>
<span data-ttu-id="519fb-248">Active Directory összevonási szolgáltatások (AD FS) értéke nem értelmezhető éles STS, ezért meg kell példány felderítés kapcsolni, és hamis át, a AuthenticationContext konstruktor.</span><span class="sxs-lookup"><span data-stu-id="519fb-248">Active Directory Federation Services (AD FS) is not recognized as production STS, so you need to turn of instance discovery and pass false at the AuthenticationContext constructor.</span></span>

<span data-ttu-id="519fb-249">A szolgáltató URL-címe van szüksége az STS-példány és egy [bérlő neve](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="519fb-249">The authority URL needs an STS instance and a [tenant name](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).</span></span>

### <a name="querying-cache-items"></a><span data-ttu-id="519fb-250">Gyorsítótár elemek lekérdezése</span><span class="sxs-lookup"><span data-stu-id="519fb-250">Querying cache items</span></span>
<span data-ttu-id="519fb-251">Adal-t tartalmaz néhány egyszerű gyorsítótár alapértelmezett gyorsítótár SharedPreferences a lekérdezési funkciók.</span><span class="sxs-lookup"><span data-stu-id="519fb-251">ADAL provides a default cache in SharedPreferences with some simple cache query functions.</span></span> <span data-ttu-id="519fb-252">A jelenlegi gyorsítótár letölthető AuthenticationContext használatával:</span><span class="sxs-lookup"><span data-stu-id="519fb-252">You can get the current cache from AuthenticationContext by using:</span></span>

    ITokenCacheStore cache = mContext.getCache();

<span data-ttu-id="519fb-253">A gyorsítótár-megvalósítással, ha szeretné testre szabni, azt is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="519fb-253">You can also provide your cache implementation, if you want to customize it.</span></span>

    mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);

### <a name="prompt-behavior"></a><span data-ttu-id="519fb-254">Parancssor viselkedése</span><span class="sxs-lookup"><span data-stu-id="519fb-254">Prompt behavior</span></span>
<span data-ttu-id="519fb-255">Adal-t Itt adhatja meg a parancssor viselkedése.</span><span class="sxs-lookup"><span data-stu-id="519fb-255">ADAL provides an option to specify prompt behavior.</span></span> <span data-ttu-id="519fb-256">PromptBehavior.Auto megjelenik a felhasználói felület, ha a frissítési token érvénytelen, és felhasználói hitelesítő adatok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="519fb-256">PromptBehavior.Auto will show the UI if the refresh token is invalid and user credentials are required.</span></span> <span data-ttu-id="519fb-257">PromptBehavior.Always a rendszer kihagyja a gyorsítótár-használati és mindig jelenjen meg a felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="519fb-257">PromptBehavior.Always will skip the cache usage and always show the UI.</span></span>

### <a name="silent-token-request-from-cache-and-refresh"></a><span data-ttu-id="519fb-258">A gyorsítótár és a frissítési csendes jogkivonatkérelem</span><span class="sxs-lookup"><span data-stu-id="519fb-258">Silent token request from cache and refresh</span></span>
<span data-ttu-id="519fb-259">Beavatkozás nélküli kérelmek nem használja a felhasználói felület előugró, és nem igényli a tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="519fb-259">A silent token request does not use the UI pop-up and does not require an activity.</span></span> <span data-ttu-id="519fb-260">Vissza a jogkivonatot a gyorsítótárból érhető el.</span><span class="sxs-lookup"><span data-stu-id="519fb-260">It returns a token from the cache if available.</span></span> <span data-ttu-id="519fb-261">Ha a jogkivonat érvényessége lejárt, ez a módszer próbálja frissíti.</span><span class="sxs-lookup"><span data-stu-id="519fb-261">If the token is expired, this method tries to refresh it.</span></span> <span data-ttu-id="519fb-262">Ha a frissítési jogkivonat lejárt vagy nem sikerült, AuthenticationException adja vissza.</span><span class="sxs-lookup"><span data-stu-id="519fb-262">If the refresh token is expired or failed, it returns AuthenticationException.</span></span>

    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );

<span data-ttu-id="519fb-263">Végezhet szinkronizálást is ez a módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="519fb-263">You can also make a sync call by using this method.</span></span> <span data-ttu-id="519fb-264">Állítsa be a visszahívási null, vagy acquireTokenSilentSync használja.</span><span class="sxs-lookup"><span data-stu-id="519fb-264">You can set null to callback or use acquireTokenSilentSync.</span></span>

### <a name="diagnostics"></a><span data-ttu-id="519fb-265">Diagnosztika</span><span class="sxs-lookup"><span data-stu-id="519fb-265">Diagnostics</span></span>
<span data-ttu-id="519fb-266">Az elsődleges információforrások a problémák diagnosztizálása az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="519fb-266">These are the primary sources of information for diagnosing issues:</span></span>

* <span data-ttu-id="519fb-267">Kivételek</span><span class="sxs-lookup"><span data-stu-id="519fb-267">Exceptions</span></span>
* <span data-ttu-id="519fb-268">Logs</span><span class="sxs-lookup"><span data-stu-id="519fb-268">Logs</span></span>
* <span data-ttu-id="519fb-269">Hálózati nyomkövetés</span><span class="sxs-lookup"><span data-stu-id="519fb-269">Network traces</span></span>

<span data-ttu-id="519fb-270">Ne feledje, hogy korrelációs azonosító központi helyet foglalnak el a diagnosztika a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="519fb-270">Note that correlation IDs are central to the diagnostics in the library.</span></span> <span data-ttu-id="519fb-271">A korrelációs állíthatja be az egyéb műveletek során a kérelem egy kérelem alapon, ha azt szeretné, hogy az adal-t összefüggéseket azonosítók.</span><span class="sxs-lookup"><span data-stu-id="519fb-271">You can set your correlation IDs on a per-request basis if you want to correlate an ADAL request with other operations in your code.</span></span> <span data-ttu-id="519fb-272">Ha nem állít egy korrelációs azonosító, a ADAL véletlenszerű egy hoz létre.</span><span class="sxs-lookup"><span data-stu-id="519fb-272">If you don't set a correlation ID, ADAL will generate a random one.</span></span> <span data-ttu-id="519fb-273">Az összes üzenetek naplózása és hálózati hívások majd kell jelölni a korrelációs azonosítót.</span><span class="sxs-lookup"><span data-stu-id="519fb-273">All log messages and network calls will then be stamped with the correlation ID.</span></span> <span data-ttu-id="519fb-274">Az önállóan létrehozott azonosítója módosítások minden kérelemnél meg.</span><span class="sxs-lookup"><span data-stu-id="519fb-274">The self-generated ID changes on each request.</span></span>

#### <a name="exceptions"></a><span data-ttu-id="519fb-275">Kivételek</span><span class="sxs-lookup"><span data-stu-id="519fb-275">Exceptions</span></span>
<span data-ttu-id="519fb-276">A felsoroltakat az első diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="519fb-276">Exceptions are the first diagnostic.</span></span> <span data-ttu-id="519fb-277">Próbálja meg hasznos hibaüzenetek jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="519fb-277">We try to provide helpful error messages.</span></span> <span data-ttu-id="519fb-278">Ha talál, amely nem lehet hasznos, adjon fájlt az kapcsolatos problémát, és ossza meg velünk.</span><span class="sxs-lookup"><span data-stu-id="519fb-278">If you find one that is not helpful, please file an issue and let us know.</span></span> <span data-ttu-id="519fb-279">Eszköz információkat, például a modell és SDK számát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="519fb-279">Include device information such as model and SDK number.</span></span>

#### <a name="logs"></a><span data-ttu-id="519fb-280">Logs</span><span class="sxs-lookup"><span data-stu-id="519fb-280">Logs</span></span>
<span data-ttu-id="519fb-281">Beállíthatja, hogy a szalagtár készítése a naplózási üzenetek problémák diagnosztizálásához használható.</span><span class="sxs-lookup"><span data-stu-id="519fb-281">You can configure the library to generate log messages that you can use to help diagnose issues.</span></span> <span data-ttu-id="519fb-282">Naplózás konfigurálása egy visszahívást, amelyet az adal-t használja kéz ki a naplóüzenetekben hozza létre a következő hívással konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="519fb-282">You configure logging by making the following call to configure a callback that ADAL will use to hand off each log message as it's generated.</span></span>

    Logger.getInstance().setExternalLogger(new ILogger() {
        @Override
        public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
        ...
        // You can write this to log file depending on level or error code.
        writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
        }
    }

<span data-ttu-id="519fb-283">Üzenetek csak írható egyéni naplófájlt használ, az alábbi kódban látható módon.</span><span class="sxs-lookup"><span data-stu-id="519fb-283">Messages can be written to a custom log file, as shown in the following code.</span></span> <span data-ttu-id="519fb-284">Sajnos nincs nem szabványos vonható naplók az eszközről.</span><span class="sxs-lookup"><span data-stu-id="519fb-284">Unfortunately, there is no standard way of getting logs from a device.</span></span> <span data-ttu-id="519fb-285">Egyes szolgáltatások, amelyek segítségével a van.</span><span class="sxs-lookup"><span data-stu-id="519fb-285">There are some services that can help you with this.</span></span> <span data-ttu-id="519fb-286">Akkor is is találjon ki a saját, például a fájlt küld a kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="519fb-286">You can also invent your own, such as sending the file to a server.</span></span>

    private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
    }

<span data-ttu-id="519fb-287">A naplózási szintek a következők:</span><span class="sxs-lookup"><span data-stu-id="519fb-287">These are the logging levels:</span></span>
* <span data-ttu-id="519fb-288">Hiba (kivételek)</span><span class="sxs-lookup"><span data-stu-id="519fb-288">Error (exceptions)</span></span>
* <span data-ttu-id="519fb-289">Figyelmeztetés (figyelmeztetés)</span><span class="sxs-lookup"><span data-stu-id="519fb-289">Warn (warning)</span></span>
* <span data-ttu-id="519fb-290">Info (tájékoztatási céllal)</span><span class="sxs-lookup"><span data-stu-id="519fb-290">Info (information purposes)</span></span>
* <span data-ttu-id="519fb-291">Részletes (További részletekért)</span><span class="sxs-lookup"><span data-stu-id="519fb-291">Verbose (more details)</span></span>

<span data-ttu-id="519fb-292">Beállíthatja a naplózási szint ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="519fb-292">You set the log level like this:</span></span>

    Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

 <span data-ttu-id="519fb-293">Összes naplózási üzenetek küldése a logcat bármilyen egyéni napló visszahívások mellett.</span><span class="sxs-lookup"><span data-stu-id="519fb-293">All log messages are sent to logcat, in addition to any custom log callbacks.</span></span>
<span data-ttu-id="519fb-294">Letölthető egy fájlba logcat az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="519fb-294">You can get a log to a file from logcat as follows:</span></span>

    adb logcat > "C:\logmsg\logfile.txt"

 <span data-ttu-id="519fb-295">További adb parancsokkal kapcsolatos további információkért lásd: a [logcat az információt az Android](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).</span><span class="sxs-lookup"><span data-stu-id="519fb-295">For details about adb commands, see the [logcat information on the Android site](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).</span></span>

#### <a name="network-traces"></a><span data-ttu-id="519fb-296">Hálózati nyomkövetés</span><span class="sxs-lookup"><span data-stu-id="519fb-296">Network traces</span></span>
<span data-ttu-id="519fb-297">Különböző eszközök használatával az adal-t állít elő, HTTP-forgalom rögzítése.</span><span class="sxs-lookup"><span data-stu-id="519fb-297">You can use various tools to capture the HTTP traffic that ADAL generates.</span></span>  <span data-ttu-id="519fb-298">Ez akkor hasznos, ha ismeri az OAuth protokollt, vagy ha meg kell adnia a diagnosztikai adatokat a Microsoft vagy egyéb támogatási csatornáit.</span><span class="sxs-lookup"><span data-stu-id="519fb-298">This is most useful if you're familiar with the OAuth protocol or if you need to provide diagnostic information to Microsoft or other support channels.</span></span>

<span data-ttu-id="519fb-299">Fiddler a HTTP legegyszerűbb eszköz.</span><span class="sxs-lookup"><span data-stu-id="519fb-299">Fiddler is the easiest HTTP tracing tool.</span></span> <span data-ttu-id="519fb-300">Az alábbi hivatkozások segítségével állítsa be megfelelően rekord ADAL hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="519fb-300">Use the following links to set it up to correctly record ADAL network traffic.</span></span> <span data-ttu-id="519fb-301">A nyomkövetés eszköz, például a Fiddler vagy Charles hasznos lehet konfigurálnia kell, hogy titkosítatlan SSL forgalom rögzítése.</span><span class="sxs-lookup"><span data-stu-id="519fb-301">For a tracing tool like Fiddler or Charles to be useful, you must configure it to record unencrypted SSL traffic.</span></span>  

> [!NOTE]
> <span data-ttu-id="519fb-302">Nyomok jön létre, így például a hozzáférési jogkivonatok, felhasználónevek és jelszavak magas szintű jogosultsággal rendelkező adatokat tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="519fb-302">Traces generated in this way might contain highly privileged information such as access tokens, usernames, and passwords.</span></span> <span data-ttu-id="519fb-303">Éles fiókok használata, ne ossza meg a nyomkövetések harmadik felek számára.</span><span class="sxs-lookup"><span data-stu-id="519fb-303">If you're using production accounts, do not share these traces with third parties.</span></span> <span data-ttu-id="519fb-304">Ha szeretne valakinek nyomkövetés megadni ahhoz, hogy segítségre van szüksége, egy ideiglenes fiókot használata a felhasználónevek és jelszavak, amelyek nem mind a megosztás Reprodukálja a hibát.</span><span class="sxs-lookup"><span data-stu-id="519fb-304">If you need to supply a trace to someone in order to get support, reproduce the issue by using a temporary account with usernames and passwords that you don't mind sharing.</span></span>

* <span data-ttu-id="519fb-305">A Telerik webhelyről: [beállítás mentése Fiddler az Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)</span><span class="sxs-lookup"><span data-stu-id="519fb-305">From the Telerik website: [Setting Up Fiddler For Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)</span></span>
* <span data-ttu-id="519fb-306">A Githubból: [ADAL Fiddler szabályainak konfigurálása](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)</span><span class="sxs-lookup"><span data-stu-id="519fb-306">From GitHub: [Configure Fiddler Rules For ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)</span></span>

### <a name="dialog-mode"></a><span data-ttu-id="519fb-307">Párbeszédpanelen mód</span><span class="sxs-lookup"><span data-stu-id="519fb-307">Dialog mode</span></span>
<span data-ttu-id="519fb-308">Tevékenység nélkül acquireToken metódus támogatja a párbeszédpanel megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="519fb-308">The acquireToken method without activity supports a dialog prompt.</span></span>

### <a name="encryption"></a><span data-ttu-id="519fb-309">Titkosítás</span><span class="sxs-lookup"><span data-stu-id="519fb-309">Encryption</span></span>
<span data-ttu-id="519fb-310">Adal-t a jogkivonatokat és SharedPreferences tárban alapértelmezés szerint titkosítja.</span><span class="sxs-lookup"><span data-stu-id="519fb-310">ADAL encrypts the tokens and store in SharedPreferences by default.</span></span> <span data-ttu-id="519fb-311">Megnézheti a StorageHelper osztály a részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="519fb-311">You can look at the StorageHelper class to see the details.</span></span> <span data-ttu-id="519fb-312">Android 4.3 (API 18) biztonságos tárolására titkos kulcsok Android Keystore bevezetni.</span><span class="sxs-lookup"><span data-stu-id="519fb-312">Android introduced Android Keystore for 4.3 (API 18) secure storage of private keys.</span></span> <span data-ttu-id="519fb-313">Adal-t használ, amely az API 18 és az annál magasabb.</span><span class="sxs-lookup"><span data-stu-id="519fb-313">ADAL uses that for API 18 and higher.</span></span> <span data-ttu-id="519fb-314">Ha szeretne adal-t használó SDK alacsonyabb verziójához, adjon meg egy titkos kulcsot következő AuthenticationSettings.INSTANCE.setSecretKey szeretné.</span><span class="sxs-lookup"><span data-stu-id="519fb-314">If you want to use ADAL for lower SDK versions, you need to provide a secret key at AuthenticationSettings.INSTANCE.setSecretKey.</span></span>

### <a name="oauth2-bearer-challenge"></a><span data-ttu-id="519fb-315">Az OAuth2 tulajdonosi kérdés</span><span class="sxs-lookup"><span data-stu-id="519fb-315">OAuth2 bearer challenge</span></span>
<span data-ttu-id="519fb-316">A AuthenticationParameters osztály authorization_uri lekérése az OAuth2 tulajdonosi ellenőrző funkciót biztosít.</span><span class="sxs-lookup"><span data-stu-id="519fb-316">The AuthenticationParameters class provides functionality to get authorization_uri from the OAuth2 bearer challenge.</span></span>

### <a name="session-cookies-in-webview"></a><span data-ttu-id="519fb-317">Webes nézet munkamenet cookie-k</span><span class="sxs-lookup"><span data-stu-id="519fb-317">Session cookies in WebView</span></span>
<span data-ttu-id="519fb-318">Android webes nézet nem törli a munkamenetek cookie-jait, az alkalmazás bezárása után.</span><span class="sxs-lookup"><span data-stu-id="519fb-318">Android WebView does not clear session cookies after the app is closed.</span></span> <span data-ttu-id="519fb-319">A mintakód használatával, amely kezelheti:</span><span class="sxs-lookup"><span data-stu-id="519fb-319">You can handle that by using this sample code:</span></span>

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

<span data-ttu-id="519fb-320">A cookie-k kapcsolatos részletekért lásd: a [CookieSyncManager az információt az Android](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).</span><span class="sxs-lookup"><span data-stu-id="519fb-320">For details about cookies, see the [CookieSyncManager information on the Android site](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).</span></span>

### <a name="resource-overrides"></a><span data-ttu-id="519fb-321">Erőforrás-felülbírálások</span><span class="sxs-lookup"><span data-stu-id="519fb-321">Resource overrides</span></span>
<span data-ttu-id="519fb-322">Az ADAL-könyvtár ProgressDialog üzenetek angol karakterláncot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="519fb-322">The ADAL library includes English strings for ProgressDialog messages.</span></span> <span data-ttu-id="519fb-323">Az alkalmazás mindent felülír Ha azt szeretné, hogy a honosított karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="519fb-323">Your application should overwrite them if you want localized strings.</span></span>

     <string name="app_loading">Loading...</string>
     <string name="broker_processing">Broker is processing</string>
     <string name="http_auth_dialog_username">Username</string>
     <string name="http_auth_dialog_password">Password</string>
     <string name="http_auth_dialog_title">Sign In</string>
     <string name="http_auth_dialog_login">Login</string>
     <string name="http_auth_dialog_cancel">Cancel</string>

### <a name="ntlm-dialog-box"></a><span data-ttu-id="519fb-324">NTLM párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="519fb-324">NTLM dialog box</span></span>
<span data-ttu-id="519fb-325">1.1.0-ás ADAL-verziót támogatja az NTLM párbeszédpanel, amely a WebViewClient onReceivedHttpAuthRequest esemény feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="519fb-325">ADAL version 1.1.0 supports an NTLM dialog box that is processed through the onReceivedHttpAuthRequest event from WebViewClient.</span></span> <span data-ttu-id="519fb-326">Az elrendezés és a párbeszédpanel karakterláncok személyre is szabhatja.</span><span class="sxs-lookup"><span data-stu-id="519fb-326">You can customize the layout and strings for the dialog box.</span></span>

### <a name="cross-app-sso"></a><span data-ttu-id="519fb-327">Alkalmazások közötti SSO</span><span class="sxs-lookup"><span data-stu-id="519fb-327">Cross-app SSO</span></span>
<span data-ttu-id="519fb-328">Ismerje meg, [az Android alkalmazások közötti SSO engedélyezése az ADAL használatával](active-directory-sso-android.md).</span><span class="sxs-lookup"><span data-stu-id="519fb-328">Learn [how to enable cross-app SSO on Android by using ADAL](active-directory-sso-android.md).</span></span>  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
