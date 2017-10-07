---
title: "Első lépések AD Android aaaAzure |} Microsoft Docs"
description: "Hogyan toobuild egy Android-alkalmazás, amely az Azure AD bejelentkezési és a hívások Azure AD számára az API-k OAuth használatával védett."
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
ms.openlocfilehash: 1aedc8ff60874b405a182a4ccbfb2c8b4d9d3704
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-an-android-app"></a><span data-ttu-id="5fe38-103">Az Azure AD integrálása Android-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="5fe38-103">Integrate Azure AD into an Android app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> <span data-ttu-id="5fe38-104">Próbálja meg az új hello előnézete [fejlesztői portálján](https://identity.microsoft.com/Docs/Android), amely segít, amelyekből megismerheti az Azure AD csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="5fe38-104">Try hello preview of our new [developer portal](https://identity.microsoft.com/Docs/Android), which will help you get up and running with Azure AD in just a few minutes.</span></span> <span data-ttu-id="5fe38-105">hello fejlesztői portálján végigvezeti hello folyamat regisztrálja az alkalmazást, és az Azure AD integrálása a kódot.</span><span class="sxs-lookup"><span data-stu-id="5fe38-105">hello developer portal will walk you through hello process of registering an app and integrating Azure AD into your code.</span></span> <span data-ttu-id="5fe38-106">Amikor elkészült, akkor kell egy egyszerű alkalmazást, amely képes hitelesíteni a felhasználók számára a bérlő és a háttérből fogadni és-ellenőrzéshez.</span><span class="sxs-lookup"><span data-stu-id="5fe38-106">When you’re finished, you'll have a simple application that can authenticate users in your tenant and a back end that can accept tokens and perform validation.</span></span>
>
>

<span data-ttu-id="5fe38-107">Ha az asztali alkalmazások, Azure Active Directory (Azure AD) teszi egyszerű és magától értetődő, tooauthenticate a a felhasználók a helyszíni Active Directory-fiókok használatával.</span><span class="sxs-lookup"><span data-stu-id="5fe38-107">If you're developing a desktop application, Azure Active Directory (Azure AD) makes it simple and straightforward for you tooauthenticate your users by using their on-premises Active Directory accounts.</span></span> <span data-ttu-id="5fe38-108">Emellett lehetővé teszi az alkalmazás toosecurely használatához minden webes API-t az Azure AD által védett, például Office 365 API-k hello vagy hello Azure API.</span><span class="sxs-lookup"><span data-stu-id="5fe38-108">It also enables your application toosecurely consume any web API protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="5fe38-109">Android-ügyfelek, amelyeket tooaccess védett erőforrások az Azure AD hello Active Directory Authentication Library (ADAL) biztosít.</span><span class="sxs-lookup"><span data-stu-id="5fe38-109">For Android clients that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="5fe38-110">hello kizárólagos ADAL célja toomake megkönnyítik az alkalmazás tooget hozzáférési jogkivonatok.</span><span class="sxs-lookup"><span data-stu-id="5fe38-110">hello sole purpose of ADAL is toomake it easy for your app tooget access tokens.</span></span> <span data-ttu-id="5fe38-111">milyen egyszerűen, azt fogja Android feladatlista alkalmazás létrehozásához, amely toodemonstrate:</span><span class="sxs-lookup"><span data-stu-id="5fe38-111">toodemonstrate how easy it is, we’ll build an Android To-Do List application that:</span></span>

* <span data-ttu-id="5fe38-112">Lekérdezi hozzáférési jogkivonatainak egy tennivalók listája API felület meghívásakor hello segítségével [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="5fe38-112">Gets access tokens for calling a To-Do List API by using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="5fe38-113">Lekérdezi a felhasználó tennivalók listájára.</span><span class="sxs-lookup"><span data-stu-id="5fe38-113">Gets a user's to-do list.</span></span>
* <span data-ttu-id="5fe38-114">Felhasználók jeleket.</span><span class="sxs-lookup"><span data-stu-id="5fe38-114">Signs out users.</span></span>

<span data-ttu-id="5fe38-115">tooget elindult, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő kell.</span><span class="sxs-lookup"><span data-stu-id="5fe38-115">tooget started, you need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="5fe38-116">Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="5fe38-116">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-download-and-run-hello-nodejs-rest-api-todo-sample-server"></a><span data-ttu-id="5fe38-117">1. lépés: Töltse le és futtassa a hello Node.js REST API TODO minta kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="5fe38-117">Step 1: Download and run hello Node.js REST API TODO sample server</span></span>
<span data-ttu-id="5fe38-118">hello Node.js REST API TODO minta kifejezetten a meglévő mintát eredményez, amely a single-bérlő tennivaló REST API létrehozása az Azure AD elleni toowork írása.</span><span class="sxs-lookup"><span data-stu-id="5fe38-118">hello Node.js REST API TODO sample is written specifically toowork against our existing sample for building a single-tenant To-Do REST API for Azure AD.</span></span> <span data-ttu-id="5fe38-119">Ez a gyors üzembe helyezés hello előfeltétele.</span><span class="sxs-lookup"><span data-stu-id="5fe38-119">This is a prerequisite for hello Quick Start.</span></span>

<span data-ttu-id="5fe38-120">Hogyan tooset ez, tekintse meg a meglévő minták kapcsolatos [Microsoft Azure Active Directory minta REST API szolgáltatás a Node.js](active-directory-devquickstarts-webapi-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="5fe38-120">For information on how tooset this up, see our existing samples in [Microsoft Azure Active Directory Sample REST API Service for Node.js](active-directory-devquickstarts-webapi-nodejs.md).</span></span>


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a><span data-ttu-id="5fe38-121">2. lépés: A webes API regisztrálása az Azure AD-bérlő</span><span class="sxs-lookup"><span data-stu-id="5fe38-121">Step 2: Register your web API with your Azure AD tenant</span></span>
<span data-ttu-id="5fe38-122">Az Active Directory támogatja a két típusú alkalmazások hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="5fe38-122">Active Directory supports adding two types of applications:</span></span>

- <span data-ttu-id="5fe38-123">Webes API-k által biztosított szolgáltatások toousers</span><span class="sxs-lookup"><span data-stu-id="5fe38-123">Web APIs that offer services toousers</span></span>
- <span data-ttu-id="5fe38-124">(Hello Web vagy az eszközön futó) alkalmazások, azokat elérő webes API-khoz</span><span class="sxs-lookup"><span data-stu-id="5fe38-124">Applications (running either on hello web or on a device) that access those web APIs</span></span>

<span data-ttu-id="5fe38-125">Ebben a lépésben regisztrálása most az hello webes API-t, hogy ez a minta tesztelési helyileg futtatja.</span><span class="sxs-lookup"><span data-stu-id="5fe38-125">In this step, you're registering hello web API that you're running locally for testing this sample.</span></span> <span data-ttu-id="5fe38-126">A webes API-k általában az, hogy szeretné-e egy alkalmazás tooaccess ajánlat funkciók REST-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5fe38-126">Normally, this web API is a REST service that's offering functionality that you want an app tooaccess.</span></span> <span data-ttu-id="5fe38-127">Az Azure AD segítségével biztosíthatja a tetszőleges végpontot.</span><span class="sxs-lookup"><span data-stu-id="5fe38-127">Azure AD can help protect any endpoint.</span></span>

<span data-ttu-id="5fe38-128">Jelenleg folyamatban feltételezve, hogy van-e regisztrálása hello TODO REST API-t korábban hivatkozott.</span><span class="sxs-lookup"><span data-stu-id="5fe38-128">We're assuming that you're registering hello TODO REST API referenced earlier.</span></span> <span data-ttu-id="5fe38-129">Azonban ez a webes API-t, Azure Active Directory toohelp védeni kívánt működik.</span><span class="sxs-lookup"><span data-stu-id="5fe38-129">But this works for any web API that you want Azure Active Directory toohelp protect.</span></span>

1. <span data-ttu-id="5fe38-130">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5fe38-130">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="5fe38-131">Hello felső sávon kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="5fe38-131">On hello top bar, click your account.</span></span> <span data-ttu-id="5fe38-132">A hello **Directory** menüben válassza ki a kívánt tooregister hello Azure AD-bérlő az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5fe38-132">In hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="5fe38-133">Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5fe38-133">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="5fe38-134">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="5fe38-134">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="5fe38-135">Adjon meg egy rövid nevet hello alkalmazás (például **TodoListService**) elemre, jelölje be **webalkalmazás és/vagy webes API**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="5fe38-135">Enter a friendly name for hello application (for example, **TodoListService**), select **Web Application and/or Web API**, and click **Next**.</span></span>
6. <span data-ttu-id="5fe38-136">Bejelentkezési URL-címhez hello hello minta hello alap URL-cím megadása.</span><span class="sxs-lookup"><span data-stu-id="5fe38-136">For hello sign-on URL, enter hello base URL for hello sample.</span></span> <span data-ttu-id="5fe38-137">Alapértelmezés szerint ez a `https://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="5fe38-137">By default, this is `https://localhost:8080`.</span></span>
7. <span data-ttu-id="5fe38-138">Kattintson a **OK** toocomplete hello regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="5fe38-138">Click **OK** toocomplete hello registration.</span></span>
8. <span data-ttu-id="5fe38-139">Hello Azure-portálon, a továbbra is tooyour alkalmazáslap lépjen, hello alkalmazás azonosítóérték található, és másolja azt.</span><span class="sxs-lookup"><span data-stu-id="5fe38-139">While still in hello Azure portal, go tooyour application page, find hello application ID value, and copy it.</span></span> <span data-ttu-id="5fe38-140">Ezt később szüksége az alkalmazás konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="5fe38-140">You'll need this later when configuring your application.</span></span>
9. <span data-ttu-id="5fe38-141">A hello **beállítások** -> **tulajdonságok** lapon, hello app ID URI frissítése – adja meg `https://<your_tenant_name>/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="5fe38-141">From hello **Settings** -> **Properties** page, update hello app ID URI - enter `https://<your_tenant_name>/TodoListService`.</span></span> <span data-ttu-id="5fe38-142">Cserélje le `<your_tenant_name>` hello nevet, az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="5fe38-142">Replace `<your_tenant_name>` with hello name of your Azure AD tenant.</span></span>

## <a name="step-3-register-hello-sample-android-native-client-application"></a><span data-ttu-id="5fe38-143">3. lépés: Hello minta Android Native Client alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="5fe38-143">Step 3: Register hello sample Android Native Client application</span></span>
<span data-ttu-id="5fe38-144">Ez a példa regisztrálnia kell a webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5fe38-144">You must register your web application in this sample.</span></span> <span data-ttu-id="5fe38-145">Ez lehetővé teszi az alkalmazás toocommunicate a hello csak regisztrált web API.</span><span class="sxs-lookup"><span data-stu-id="5fe38-145">This allows your application toocommunicate with hello just-registered web API.</span></span> <span data-ttu-id="5fe38-146">Az Azure AD fog megtagadják tooeven lehetővé teszi az alkalmazás tooask a bejelentkezéshez, kivéve regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="5fe38-146">Azure AD will refuse tooeven allow your application tooask for sign-in unless it's registered.</span></span> <span data-ttu-id="5fe38-147">Az hello biztonsági hello modell, amely része.</span><span class="sxs-lookup"><span data-stu-id="5fe38-147">That's part of hello security of hello model.</span></span>

<span data-ttu-id="5fe38-148">Azt még feltéve, hogy van-e regisztrálása korábban hivatkozott hello mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5fe38-148">We're assuming that you're registering hello sample application referenced earlier.</span></span> <span data-ttu-id="5fe38-149">De bármely alkalmazás, amely kidolgozása Ez az eljárás használható.</span><span class="sxs-lookup"><span data-stu-id="5fe38-149">But this procedure works for any app that you're developing.</span></span>

> [!NOTE]
> <span data-ttu-id="5fe38-150">Először talán miért kívánja menteni egy alkalmazás és a webes API-k egy bérlő.</span><span class="sxs-lookup"><span data-stu-id="5fe38-150">You might wonder why you're putting both an application and a web API in one tenant.</span></span> <span data-ttu-id="5fe38-151">Mivel előfordulhat, hogy Ön rendelkezik kitalál, egy alkalmazás olyan külső API-bérlőhöz egy másik Azure AD-ben regisztrált hozzáférő hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="5fe38-151">As you might have guessed, you can build an app that accesses an external API that is registered in Azure AD from another tenant.</span></span> <span data-ttu-id="5fe38-152">Ha így tesz, az ügyfelek is hello API hello alkalmazásban tooconsent toohello használatát kéri.</span><span class="sxs-lookup"><span data-stu-id="5fe38-152">If you do that, your customers will be prompted tooconsent toohello use of hello API in hello application.</span></span> <span data-ttu-id="5fe38-153">Az IOS rendszerhez készült Active Directory Authentication Library gondoskodik a hozzájárulásukat adják meg.</span><span class="sxs-lookup"><span data-stu-id="5fe38-153">Active Directory Authentication Library for iOS takes care of this consent for you.</span></span> <span data-ttu-id="5fe38-154">Összetettebb funkciók megismeréséhez azt láthatja, hogy ez az hello munkahelyi szükséges tooaccess hello tartalmazó csomag, az Azure és az Office, valamint az egyéb szolgáltató Microsoft APIs fontos része.</span><span class="sxs-lookup"><span data-stu-id="5fe38-154">As we explore more advanced features, you'll see that this is an important part of hello work needed tooaccess hello suite of Microsoft APIs from Azure and Office, as well as any other service provider.</span></span> <span data-ttu-id="5fe38-155">Most, mert a webes API-t és a hello alatt az alkalmazás regisztrálva azonos bérlői, bármely beleegyezést kér fogja látni.</span><span class="sxs-lookup"><span data-stu-id="5fe38-155">For now, because you registered both your web API and your application under hello same tenant, you won't see any prompts for consent.</span></span> <span data-ttu-id="5fe38-156">Ez helyzet általában hello Ha az alkalmazás csak a saját vállalati toouse.</span><span class="sxs-lookup"><span data-stu-id="5fe38-156">This is usually hello case if you're developing an application just for your own company toouse.</span></span>

1. <span data-ttu-id="5fe38-157">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5fe38-157">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="5fe38-158">Hello felső sávon kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="5fe38-158">On hello top bar, click your account.</span></span> <span data-ttu-id="5fe38-159">A hello **Directory** menüben válassza ki a kívánt tooregister hello Azure AD-bérlő az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5fe38-159">In hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="5fe38-160">Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5fe38-160">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="5fe38-161">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="5fe38-161">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="5fe38-162">Adjon meg egy rövid nevet hello alkalmazás (például **TodoListClient-Android**) elemre, jelölje be **natív ügyfélalkalmazás**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="5fe38-162">Enter a friendly name for hello application (for example, **TodoListClient-Android**), select **Native Client Application**, and click **Next**.</span></span>
6. <span data-ttu-id="5fe38-163">Hello az átirányítási URI-címe, adja meg `http://TodoListClient`.</span><span class="sxs-lookup"><span data-stu-id="5fe38-163">For hello redirect URI, enter `http://TodoListClient`.</span></span> <span data-ttu-id="5fe38-164">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="5fe38-164">Click **Finish**.</span></span>
7. <span data-ttu-id="5fe38-165">Hello alkalmazás oldalról hello alkalmazás azonosítóérték található, és másolja azt.</span><span class="sxs-lookup"><span data-stu-id="5fe38-165">From hello application page, find hello application ID value and copy it.</span></span> <span data-ttu-id="5fe38-166">Ezt később szüksége az alkalmazás konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="5fe38-166">You'll need this later when configuring your application.</span></span>
8. <span data-ttu-id="5fe38-167">A hello **beállítások** lapon jelölje be **szükséges engedélyek** válassza **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5fe38-167">From hello **Settings** page, select **Required Permissions** and select **Add**.</span></span>  <span data-ttu-id="5fe38-168">Keresse meg és válassza ki a TodoListService, adja hozzá a hello **hozzáférés TodoListService** engedélyt a **delegált engedélyek**, és kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="5fe38-168">Locate and select TodoListService, add hello **Access TodoListService** permission under **Delegated Permissions**, and click **Done**.</span></span>

<span data-ttu-id="5fe38-169">a Maven toobuild, pom.xml használhatja hello felső szinten:</span><span class="sxs-lookup"><span data-stu-id="5fe38-169">toobuild with Maven, you can use pom.xml at hello top level:</span></span>

1. <span data-ttu-id="5fe38-170">A tárház klónozása egy olyan könyvtárba, az Ön által választott:</span><span class="sxs-lookup"><span data-stu-id="5fe38-170">Clone this repo into a directory of your choice:</span></span>

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  
2. <span data-ttu-id="5fe38-171">Hello kövesse hello [Előfeltételek tooset Android Maven környezet](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).</span><span class="sxs-lookup"><span data-stu-id="5fe38-171">Follow hello steps in hello [prerequisites tooset up your Maven environment for Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).</span></span>
3. <span data-ttu-id="5fe38-172">Az SDK 19 hello emulátor beállítása.</span><span class="sxs-lookup"><span data-stu-id="5fe38-172">Set up hello emulator with SDK 19.</span></span>
4. <span data-ttu-id="5fe38-173">Nyissa meg ahol klónozott tárház hello toohello gyökérmappájába.</span><span class="sxs-lookup"><span data-stu-id="5fe38-173">Go toohello root folder where you cloned hello repo.</span></span>
5. <span data-ttu-id="5fe38-174">Futtassa ezt a parancsot:`mvn clean install`</span><span class="sxs-lookup"><span data-stu-id="5fe38-174">Run this command: `mvn clean install`</span></span>
6. <span data-ttu-id="5fe38-175">Hello directory toohello gyors üzembe helyezési minta módosítása:`cd samples\hello`</span><span class="sxs-lookup"><span data-stu-id="5fe38-175">Change hello directory toohello Quick Start sample: `cd samples\hello`</span></span>
7. <span data-ttu-id="5fe38-176">Futtassa ezt a parancsot:`mvn android:deploy android:run`</span><span class="sxs-lookup"><span data-stu-id="5fe38-176">Run this command: `mvn android:deploy android:run`</span></span>

   <span data-ttu-id="5fe38-177">Meg kell jelennie a hello alkalmazás indítása.</span><span class="sxs-lookup"><span data-stu-id="5fe38-177">You should see hello app starting.</span></span>
8. <span data-ttu-id="5fe38-178">Adja meg a teszt felhasználói hitelesítő adatok tootry.</span><span class="sxs-lookup"><span data-stu-id="5fe38-178">Enter test user credentials tootry.</span></span>

<span data-ttu-id="5fe38-179">JAR csomagok nyújtanak hello AAR csomag mellett.</span><span class="sxs-lookup"><span data-stu-id="5fe38-179">JAR packages will be submitted beside hello AAR package.</span></span>

## <a name="step-4-download-hello-android-adal-and-add-it-tooyour-eclipse-workspace"></a><span data-ttu-id="5fe38-180">4. lépés: Töltse le az Android ADAL hello, és adja hozzá tooyour Eclipse munkaterület</span><span class="sxs-lookup"><span data-stu-id="5fe38-180">Step 4: Download hello Android ADAL and add it tooyour Eclipse workspace</span></span>
<span data-ttu-id="5fe38-181">Hajtottunk azt Ön toohave egyszerűen több beállítások toouse adal-t a Androidos projekt:</span><span class="sxs-lookup"><span data-stu-id="5fe38-181">We've made it easy for you toohave multiple options toouse ADAL in your Android project:</span></span>

* <span data-ttu-id="5fe38-182">Használhat hello forrás kód tooimport tárra eclipse-ben és a hivatkozás tooyour alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="5fe38-182">You can use hello source code tooimport this library into Eclipse and link tooyour application.</span></span>
* <span data-ttu-id="5fe38-183">Android Studio használata, hello AAR csomag formázása és a hivatkozás hello bináris fájljait is használhatja.</span><span class="sxs-lookup"><span data-stu-id="5fe38-183">If you're using Android Studio, you can use hello AAR package format and reference hello binaries.</span></span>

### <a name="option-1-source-zip"></a><span data-ttu-id="5fe38-184">1. lehetőség: Forrás Zip</span><span class="sxs-lookup"><span data-stu-id="5fe38-184">Option 1: Source Zip</span></span>
<span data-ttu-id="5fe38-185">hello forráskódját, másolatának toodownload kattintson **töltse le a ZIP-** hello jobb oldalán található hello.</span><span class="sxs-lookup"><span data-stu-id="5fe38-185">toodownload a copy of hello source code, click **Download ZIP** on hello right side of hello page.</span></span> <span data-ttu-id="5fe38-186">Illetve [töltse le a Githubról](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).</span><span class="sxs-lookup"><span data-stu-id="5fe38-186">Or you can [download from GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).</span></span>

### <a name="option-2-source-via-git"></a><span data-ttu-id="5fe38-187">2. lehetőség: Forrás Git keresztül</span><span class="sxs-lookup"><span data-stu-id="5fe38-187">Option 2: Source via Git</span></span>
<span data-ttu-id="5fe38-188">hello tooget hello forráskódját SDK keresztül Git, írja be:</span><span class="sxs-lookup"><span data-stu-id="5fe38-188">tooget hello source code of hello SDK via Git, type:</span></span>

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

### <a name="option-3-binaries-via-gradle"></a><span data-ttu-id="5fe38-189">3. lehetőség: Bináris Gradle keresztül</span><span class="sxs-lookup"><span data-stu-id="5fe38-189">Option 3: Binaries via Gradle</span></span>
<span data-ttu-id="5fe38-190">Hello bináris fájljai az hello Maven központi tárházban kérheti le.</span><span class="sxs-lookup"><span data-stu-id="5fe38-190">You can get hello binaries from hello Maven central repo.</span></span> <span data-ttu-id="5fe38-191">az alábbiak szerint hello AAR csomagot is tartalmazza a projekt az Android Studio:</span><span class="sxs-lookup"><span data-stu-id="5fe38-191">hello AAR package can be included as follows in your project in Android Studio:</span></span>

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

### <a name="option-4-aar-via-maven"></a><span data-ttu-id="5fe38-192">4. lehetőség: AAR Maven keresztül</span><span class="sxs-lookup"><span data-stu-id="5fe38-192">Option 4: AAR via Maven</span></span>
<span data-ttu-id="5fe38-193">Hello M2Eclipse beépülő modul használata, hello függőségi adhat meg a pom.xml fájlt:</span><span class="sxs-lookup"><span data-stu-id="5fe38-193">If you're using hello M2Eclipse plug-in, you can specify hello dependency in your pom.xml file:</span></span>

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


### <a name="option-5-jar-package-inside-hello-libs-folder"></a><span data-ttu-id="5fe38-194">5. lehetőség: JAR csomag hello függvénytárak mappába</span><span class="sxs-lookup"><span data-stu-id="5fe38-194">Option 5: JAR package inside hello libs folder</span></span>
<span data-ttu-id="5fe38-195">Hello JAR-fájlra beszerezni hello Maven-tárházban, és dobja el, a hello **függvénytárak** a projekt mappájára.</span><span class="sxs-lookup"><span data-stu-id="5fe38-195">You can get hello JAR file from hello Maven repo and drop it into hello **libs** folder in your project.</span></span> <span data-ttu-id="5fe38-196">Toocopy hello szükséges erőforrások tooyour projekt, valamint kell hello JAR csomagok nem tartalmazza azokat.</span><span class="sxs-lookup"><span data-stu-id="5fe38-196">You need toocopy hello required resources tooyour project as well, because hello JAR packages don't include them.</span></span>

## <a name="step-5-add-references-tooandroid-adal-tooyour-project"></a><span data-ttu-id="5fe38-197">5. lépés: Hivatkozás tooAndroid ADAL tooyour projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5fe38-197">Step 5: Add references tooAndroid ADAL tooyour project</span></span>
1. <span data-ttu-id="5fe38-198">Egy hivatkozási tooyour projekt hozzáadása, és adja meg azt az Android tárként.</span><span class="sxs-lookup"><span data-stu-id="5fe38-198">Add a reference tooyour project and specify it as an Android library.</span></span> <span data-ttu-id="5fe38-199">Ha nem Ön hogyan toodo, hello további tájékoztatást kaphat [Android Studio hely](http://developer.android.com/tools/projects/projects-eclipse.html).</span><span class="sxs-lookup"><span data-stu-id="5fe38-199">If you're uncertain how toodo this, you can get more information on hello [Android Studio site](http://developer.android.com/tools/projects/projects-eclipse.html).</span></span>
2. <span data-ttu-id="5fe38-200">Adja hozzá azokat a projektbeállításokat hibakeresési hello projektfüggőségek.</span><span class="sxs-lookup"><span data-stu-id="5fe38-200">Add hello project dependency for debugging into your project settings.</span></span>
3. <span data-ttu-id="5fe38-201">A projekt AndroidManifest.xml fájl tooinclude frissítése:</span><span class="sxs-lookup"><span data-stu-id="5fe38-201">Update your project's AndroidManifest.xml file tooinclude:</span></span>

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

4. <span data-ttu-id="5fe38-202">A fő tevékenységnél AuthenticationContext példányának létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5fe38-202">Create an instance of AuthenticationContext at your main activity.</span></span> <span data-ttu-id="5fe38-203">a hívás hello részleteit Ez a témakör hello terjed, de remek kezdőpont kaphat hello megnézi [Android Native Client minta](https://github.com/AzureADSamples/NativeClient-Android).</span><span class="sxs-lookup"><span data-stu-id="5fe38-203">hello details of this call are beyond hello scope of this topic, but you can get a good start by looking at hello [Android Native Client sample](https://github.com/AzureADSamples/NativeClient-Android).</span></span> <span data-ttu-id="5fe38-204">A következő példa hello, SharedPreferences hello alapértelmezett gyorsítótár, továbbá hatóság hello formájában `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:</span><span class="sxs-lookup"><span data-stu-id="5fe38-204">In hello following example, SharedPreferences is hello default cache, and Authority is in hello form of `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:</span></span>

    `mContext = new AuthenticationContext(MainActivity.this, authority, true); // mContext is a field in your activity`

5. <span data-ttu-id="5fe38-205">Másolja a kód blokk toohandle hello vége AuthenticationActivity hello felhasználó megadja hitelesítő adatait, és megkapja az engedélyezési kód után:</span><span class="sxs-lookup"><span data-stu-id="5fe38-205">Copy this code block toohandle hello end of AuthenticationActivity after hello user enters credentials and receives an authorization code:</span></span>

        @Override
         protected void onActivityResult(int requestCode, int resultCode, Intent data) {
             super.onActivityResult(requestCode, resultCode, data);
             if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
             }
         }

6. <span data-ttu-id="5fe38-206">a jogkivonat tooask egy visszahívási határozhat meg:</span><span class="sxs-lookup"><span data-stu-id="5fe38-206">tooask for a token, define a callback:</span></span>

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

7. <span data-ttu-id="5fe38-207">Végül kérje meg a jogkivonat, hogy a visszahívás használatával:</span><span class="sxs-lookup"><span data-stu-id="5fe38-207">Finally, ask for a token by using that callback:</span></span>

    `mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                   callback);`

<span data-ttu-id="5fe38-208">Hello paraméterek leírását itt található:</span><span class="sxs-lookup"><span data-stu-id="5fe38-208">Here's an explanation of hello parameters:</span></span>

* <span data-ttu-id="5fe38-209">*erőforrás* szükséges, azonban tooaccess próbált hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="5fe38-209">*resource* is required and is hello resource you're trying tooaccess.</span></span>
* <span data-ttu-id="5fe38-210">*ClientID* szükség, és az Azure AD származik.</span><span class="sxs-lookup"><span data-stu-id="5fe38-210">*clientid* is required and comes from Azure AD.</span></span>
* <span data-ttu-id="5fe38-211">*RedirectUri* nincs szükség toobe hello acquireToken hívás előírt.</span><span class="sxs-lookup"><span data-stu-id="5fe38-211">*RedirectUri* is not required toobe provided for hello acquireToken call.</span></span> <span data-ttu-id="5fe38-212">Állíthat be, mint a csomag neve.</span><span class="sxs-lookup"><span data-stu-id="5fe38-212">You can set it up as your package name.</span></span>
* <span data-ttu-id="5fe38-213">*PromptBehavior* tooask a hitelesítő adatok tooskip hello gyorsítótár és a cookie-k segítségével.</span><span class="sxs-lookup"><span data-stu-id="5fe38-213">*PromptBehavior* helps tooask for credentials tooskip hello cache and cookie.</span></span>
* <span data-ttu-id="5fe38-214">*a visszahívási* után hello engedélyezési kód cseréje a jogkivonat neve.</span><span class="sxs-lookup"><span data-stu-id="5fe38-214">*callback* is called after hello authorization code is exchanged for a token.</span></span> <span data-ttu-id="5fe38-215">AuthenticationResult, amelynek hozzáférési jogkivonat objektum rendelkezik, lejárt, és a lexikális elem adatainak azonosító.</span><span class="sxs-lookup"><span data-stu-id="5fe38-215">It has an object of AuthenticationResult, which has access token, date expired, and ID token information.</span></span>
* <span data-ttu-id="5fe38-216">*acquireTokenSilent* nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="5fe38-216">*acquireTokenSilent* is optional.</span></span> <span data-ttu-id="5fe38-217">Hívása akkor toohandle gyorsítótárazás és a frissítési token.</span><span class="sxs-lookup"><span data-stu-id="5fe38-217">You can call it toohandle caching and token refresh.</span></span> <span data-ttu-id="5fe38-218">Hello Sync szolgáltatás verzióját is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5fe38-218">It also provides hello sync version.</span></span> <span data-ttu-id="5fe38-219">Elfogadja a *userId* paraméterként.</span><span class="sxs-lookup"><span data-stu-id="5fe38-219">It accepts *userId* as a parameter.</span></span>

        mContext.acquireTokenSilent(resource, clientid, userId, callback );

<span data-ttu-id="5fe38-220">Ez a forgatókönyv segítségével kell milyen kell toosuccessfully integrálása az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="5fe38-220">By using this walkthrough, you should have what you need toosuccessfully integrate with Azure Active Directory.</span></span> <span data-ttu-id="5fe38-221">További példák a, a Microsoft hello AzureADSamples / GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="5fe38-221">For more examples of this working, visit hello AzureADSamples/ repository on GitHub.</span></span>

## <a name="important-information"></a><span data-ttu-id="5fe38-222">Fontos információk</span><span class="sxs-lookup"><span data-stu-id="5fe38-222">Important information</span></span>
### <a name="customization"></a><span data-ttu-id="5fe38-223">Testreszabás</span><span class="sxs-lookup"><span data-stu-id="5fe38-223">Customization</span></span>
<span data-ttu-id="5fe38-224">Az alkalmazás-erőforrásokat felülírhatnak-e projekt könyvtárerőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="5fe38-224">Your application resources can overwrite library project resources.</span></span> <span data-ttu-id="5fe38-225">Ez akkor fordul elő, amikor az alkalmazás éppen készül.</span><span class="sxs-lookup"><span data-stu-id="5fe38-225">This happens when your app is being built.</span></span> <span data-ttu-id="5fe38-226">Emiatt testre szabhatja a hitelesítési tevékenységet elrendezés hello igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="5fe38-226">For this reason, you can customize authentication activity layout hello way you want.</span></span> <span data-ttu-id="5fe38-227">Hello szabályozza, hogy tookeep hello Azonosítóját kell, hogy az adal-t használ a (webes Nézet).</span><span class="sxs-lookup"><span data-stu-id="5fe38-227">Be sure tookeep hello ID of hello controls that ADAL uses (WebView).</span></span>

### <a name="broker"></a><span data-ttu-id="5fe38-228">Broker</span><span class="sxs-lookup"><span data-stu-id="5fe38-228">Broker</span></span>
<span data-ttu-id="5fe38-229">hello Microsoft Intune vállalati portál alkalmazás hello broker összetevő biztosít.</span><span class="sxs-lookup"><span data-stu-id="5fe38-229">hello Microsoft Intune Company Portal app provides hello broker component.</span></span> <span data-ttu-id="5fe38-230">hello fiók AccountManager jön létre.</span><span class="sxs-lookup"><span data-stu-id="5fe38-230">hello account is created in AccountManager.</span></span> <span data-ttu-id="5fe38-231">hello fiók típus: "com.microsoft.workaccount."</span><span class="sxs-lookup"><span data-stu-id="5fe38-231">hello account type is "com.microsoft.workaccount."</span></span> <span data-ttu-id="5fe38-232">AccountManager lehetővé teszi, hogy csak egyetlen egyszeri bejelentkezési fiók.</span><span class="sxs-lookup"><span data-stu-id="5fe38-232">AccountManager allows only a single SSO account.</span></span> <span data-ttu-id="5fe38-233">Az egyszeri bejelentkezési cookie hello felhasználó hello eszköz kihívás az hello alkalmazások befejezése után hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5fe38-233">It creates an SSO cookie for hello user after completing hello device challenge for one of hello apps.</span></span>

<span data-ttu-id="5fe38-234">ADAL hello broker fiókot használja, ha egy felhasználói fiók a hitelesítő jön létre, és nem tooskip azt.</span><span class="sxs-lookup"><span data-stu-id="5fe38-234">ADAL uses hello broker account if one user account is created at this authenticator and you choose not tooskip it.</span></span> <span data-ttu-id="5fe38-235">Kihagyhatja hello broker felhasználót:</span><span class="sxs-lookup"><span data-stu-id="5fe38-235">You can skip hello broker user with:</span></span>

   `AuthenticationSettings.Instance.setSkipBroker(true);`

<span data-ttu-id="5fe38-236">Egy különös RedirectUri tooregister broker használati van szükség.</span><span class="sxs-lookup"><span data-stu-id="5fe38-236">You need tooregister a special RedirectUri for broker usage.</span></span> <span data-ttu-id="5fe38-237">Hello formátumban van RedirectUri `msauth://packagename/Base64UrlencodedSignature`.</span><span class="sxs-lookup"><span data-stu-id="5fe38-237">RedirectUri is in hello format of `msauth://packagename/Base64UrlencodedSignature`.</span></span> <span data-ttu-id="5fe38-238">A RedirectUri hello parancsfájl brokerRedirectPrint.ps1 vagy hello API-hívás mContext.getBrokerRedirectUri használatával kaphat az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5fe38-238">You can get your RedirectUri for your app by using hello script brokerRedirectPrint.ps1 or hello API call mContext.getBrokerRedirectUri.</span></span> <span data-ttu-id="5fe38-239">hello aláírás aláíró tanúsítványok kapcsolódó tooyour.</span><span class="sxs-lookup"><span data-stu-id="5fe38-239">hello signature is related tooyour signing certificates.</span></span>

<span data-ttu-id="5fe38-240">hello aktuális broker modell csak egy felhasználóhoz.</span><span class="sxs-lookup"><span data-stu-id="5fe38-240">hello current broker model is for one user.</span></span> <span data-ttu-id="5fe38-241">AuthenticationContext hello API metódus tooget hello broker felhasználói biztosít.</span><span class="sxs-lookup"><span data-stu-id="5fe38-241">AuthenticationContext provides hello API method tooget hello broker user.</span></span>

   `String brokerAccount =  mContext.getBrokerUser(); //Broker user is returned if account is valid.`

<span data-ttu-id="5fe38-242">Az alkalmazás jegyzékének rendelkeznie kell a következő engedélyek toouse AccountManager fiókok hello.</span><span class="sxs-lookup"><span data-stu-id="5fe38-242">Your app manifest should have hello following permissions toouse AccountManager accounts.</span></span> <span data-ttu-id="5fe38-243">További információkért lásd: hello [hello Android webhelyéről AccountManager információk](http://developer.android.com/reference/android/accounts/AccountManager.html).</span><span class="sxs-lookup"><span data-stu-id="5fe38-243">For details, see hello [AccountManager information on hello Android site](http://developer.android.com/reference/android/accounts/AccountManager.html).</span></span>

* <span data-ttu-id="5fe38-244">GET_ACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="5fe38-244">GET_ACCOUNTS</span></span>
* <span data-ttu-id="5fe38-245">USE_CREDENTIALS</span><span class="sxs-lookup"><span data-stu-id="5fe38-245">USE_CREDENTIALS</span></span>
* <span data-ttu-id="5fe38-246">MANAGE_ACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="5fe38-246">MANAGE_ACCOUNTS</span></span>

### <a name="authority-url-and-ad-fs"></a><span data-ttu-id="5fe38-247">Szolgáltató URL-címe és az AD FS</span><span class="sxs-lookup"><span data-stu-id="5fe38-247">Authority URL and AD FS</span></span>
<span data-ttu-id="5fe38-248">Active Directory összevonási szolgáltatások (AD FS) értéke nem értelmezhető éles STS, így példány felderítés tooturn kell, és hamis át, hello AuthenticationContext konstruktor.</span><span class="sxs-lookup"><span data-stu-id="5fe38-248">Active Directory Federation Services (AD FS) is not recognized as production STS, so you need tooturn of instance discovery and pass false at hello AuthenticationContext constructor.</span></span>

<span data-ttu-id="5fe38-249">hello szolgáltató URL-címe van szüksége az STS-példány és egy [bérlő neve](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="5fe38-249">hello authority URL needs an STS instance and a [tenant name](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).</span></span>

### <a name="querying-cache-items"></a><span data-ttu-id="5fe38-250">Gyorsítótár elemek lekérdezése</span><span class="sxs-lookup"><span data-stu-id="5fe38-250">Querying cache items</span></span>
<span data-ttu-id="5fe38-251">Adal-t tartalmaz néhány egyszerű gyorsítótár alapértelmezett gyorsítótár SharedPreferences a lekérdezési funkciók.</span><span class="sxs-lookup"><span data-stu-id="5fe38-251">ADAL provides a default cache in SharedPreferences with some simple cache query functions.</span></span> <span data-ttu-id="5fe38-252">Hello aktuális gyorsítótár letölthető AuthenticationContext használatával:</span><span class="sxs-lookup"><span data-stu-id="5fe38-252">You can get hello current cache from AuthenticationContext by using:</span></span>

    ITokenCacheStore cache = mContext.getCache();

<span data-ttu-id="5fe38-253">Is megadhatja a gyorsítótár-megvalósítással, ha azt szeretné, hogy toocustomize azt.</span><span class="sxs-lookup"><span data-stu-id="5fe38-253">You can also provide your cache implementation, if you want toocustomize it.</span></span>

    mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);

### <a name="prompt-behavior"></a><span data-ttu-id="5fe38-254">Parancssor viselkedése</span><span class="sxs-lookup"><span data-stu-id="5fe38-254">Prompt behavior</span></span>
<span data-ttu-id="5fe38-255">Adal-t biztosít egy beállítás toospecify parancssor viselkedése.</span><span class="sxs-lookup"><span data-stu-id="5fe38-255">ADAL provides an option toospecify prompt behavior.</span></span> <span data-ttu-id="5fe38-256">PromptBehavior.Auto hello felhasználói felületén jelennek meg, ha hello frissítési jogkivonat érvénytelen, és felhasználói hitelesítő adatok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="5fe38-256">PromptBehavior.Auto will show hello UI if hello refresh token is invalid and user credentials are required.</span></span> <span data-ttu-id="5fe38-257">PromptBehavior.Always fog hello gyorsítótár-használati kihagyhatja, és mindig jelenjen meg hello UI.</span><span class="sxs-lookup"><span data-stu-id="5fe38-257">PromptBehavior.Always will skip hello cache usage and always show hello UI.</span></span>

### <a name="silent-token-request-from-cache-and-refresh"></a><span data-ttu-id="5fe38-258">A gyorsítótár és a frissítési csendes jogkivonatkérelem</span><span class="sxs-lookup"><span data-stu-id="5fe38-258">Silent token request from cache and refresh</span></span>
<span data-ttu-id="5fe38-259">Csendes kérés hello előugró felhasználói felület nem használ, és nem szükséges egy tevékenység.</span><span class="sxs-lookup"><span data-stu-id="5fe38-259">A silent token request does not use hello UI pop-up and does not require an activity.</span></span> <span data-ttu-id="5fe38-260">A jogkivonat hello gyorsítótárból, ha elérhető adja vissza.</span><span class="sxs-lookup"><span data-stu-id="5fe38-260">It returns a token from hello cache if available.</span></span> <span data-ttu-id="5fe38-261">Ha hello-token érvényessége lejárt, ez a módszer megpróbál toorefresh azt.</span><span class="sxs-lookup"><span data-stu-id="5fe38-261">If hello token is expired, this method tries toorefresh it.</span></span> <span data-ttu-id="5fe38-262">Ha hello frissítési jogkivonat lejárt vagy nem sikerült, AuthenticationException adja vissza.</span><span class="sxs-lookup"><span data-stu-id="5fe38-262">If hello refresh token is expired or failed, it returns AuthenticationException.</span></span>

    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );

<span data-ttu-id="5fe38-263">Végezhet szinkronizálást is ez a módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="5fe38-263">You can also make a sync call by using this method.</span></span> <span data-ttu-id="5fe38-264">Null toocallback beállíthatja vagy acquireTokenSilentSync használja.</span><span class="sxs-lookup"><span data-stu-id="5fe38-264">You can set null toocallback or use acquireTokenSilentSync.</span></span>

### <a name="diagnostics"></a><span data-ttu-id="5fe38-265">Diagnosztika</span><span class="sxs-lookup"><span data-stu-id="5fe38-265">Diagnostics</span></span>
<span data-ttu-id="5fe38-266">Hello elsődleges információforrások a problémák diagnosztizálása az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="5fe38-266">These are hello primary sources of information for diagnosing issues:</span></span>

* <span data-ttu-id="5fe38-267">Kivételek</span><span class="sxs-lookup"><span data-stu-id="5fe38-267">Exceptions</span></span>
* <span data-ttu-id="5fe38-268">Logs</span><span class="sxs-lookup"><span data-stu-id="5fe38-268">Logs</span></span>
* <span data-ttu-id="5fe38-269">Hálózati nyomkövetés</span><span class="sxs-lookup"><span data-stu-id="5fe38-269">Network traces</span></span>

<span data-ttu-id="5fe38-270">Ne feledje, hogy korrelációs azonosító központi toohello diagnosztika hello könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="5fe38-270">Note that correlation IDs are central toohello diagnostics in hello library.</span></span> <span data-ttu-id="5fe38-271">Beállíthatja a korrelációs azonosítók kérelem alapú, ha azt szeretné, hogy az adal-t a kódban az egyéb műveletek kérelem toocorrelate.</span><span class="sxs-lookup"><span data-stu-id="5fe38-271">You can set your correlation IDs on a per-request basis if you want toocorrelate an ADAL request with other operations in your code.</span></span> <span data-ttu-id="5fe38-272">Ha nem állít egy korrelációs azonosító, a ADAL véletlenszerű egy hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5fe38-272">If you don't set a correlation ID, ADAL will generate a random one.</span></span> <span data-ttu-id="5fe38-273">Az összes üzenetek naplózása és hálózati hívások majd lesz megjelölve hello korrelációs azonosítót.</span><span class="sxs-lookup"><span data-stu-id="5fe38-273">All log messages and network calls will then be stamped with hello correlation ID.</span></span> <span data-ttu-id="5fe38-274">hello a saját azonosító módosításokat minden kérelemnél meg.</span><span class="sxs-lookup"><span data-stu-id="5fe38-274">hello self-generated ID changes on each request.</span></span>

#### <a name="exceptions"></a><span data-ttu-id="5fe38-275">Kivételek</span><span class="sxs-lookup"><span data-stu-id="5fe38-275">Exceptions</span></span>
<span data-ttu-id="5fe38-276">Kivételek először diagnosztikai hello vannak.</span><span class="sxs-lookup"><span data-stu-id="5fe38-276">Exceptions are hello first diagnostic.</span></span> <span data-ttu-id="5fe38-277">A Microsoft próbálja tooprovide hasznos hibaüzenetek.</span><span class="sxs-lookup"><span data-stu-id="5fe38-277">We try tooprovide helpful error messages.</span></span> <span data-ttu-id="5fe38-278">Ha talál, amely nem lehet hasznos, adjon fájlt az kapcsolatos problémát, és ossza meg velünk.</span><span class="sxs-lookup"><span data-stu-id="5fe38-278">If you find one that is not helpful, please file an issue and let us know.</span></span> <span data-ttu-id="5fe38-279">Eszköz információkat, például a modell és SDK számát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5fe38-279">Include device information such as model and SDK number.</span></span>

#### <a name="logs"></a><span data-ttu-id="5fe38-280">Logs</span><span class="sxs-lookup"><span data-stu-id="5fe38-280">Logs</span></span>
<span data-ttu-id="5fe38-281">Hello könyvtár toogenerate is beállíthat, amelyeket felhasználhat toohelp naplóüzenetek eseményadatokat.</span><span class="sxs-lookup"><span data-stu-id="5fe38-281">You can configure hello library toogenerate log messages that you can use toohelp diagnose issues.</span></span> <span data-ttu-id="5fe38-282">Naplózási azáltal, hogy hello következő tooconfigure, hogy adal-t használja ki a naplóüzenetekben toohand hozza létre a visszahívás hívása.</span><span class="sxs-lookup"><span data-stu-id="5fe38-282">You configure logging by making hello following call tooconfigure a callback that ADAL will use toohand off each log message as it's generated.</span></span>

    Logger.getInstance().setExternalLogger(new ILogger() {
        @Override
        public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
        ...
        // You can write this toolog file depending on level or error code.
        writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
        }
    }

<span data-ttu-id="5fe38-283">Üzenetek csak írható tooa egyéni naplófájl, ahogy az a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="5fe38-283">Messages can be written tooa custom log file, as shown in hello following code.</span></span> <span data-ttu-id="5fe38-284">Sajnos nincs nem szabványos vonható naplók az eszközről.</span><span class="sxs-lookup"><span data-stu-id="5fe38-284">Unfortunately, there is no standard way of getting logs from a device.</span></span> <span data-ttu-id="5fe38-285">Egyes szolgáltatások, amelyek segítségével a van.</span><span class="sxs-lookup"><span data-stu-id="5fe38-285">There are some services that can help you with this.</span></span> <span data-ttu-id="5fe38-286">A saját, például küldő hello tooa fájlkiszolgáló találjon is ki.</span><span class="sxs-lookup"><span data-stu-id="5fe38-286">You can also invent your own, such as sending hello file tooa server.</span></span>

    private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
    }

<span data-ttu-id="5fe38-287">Hello naplózási szintek a következők:</span><span class="sxs-lookup"><span data-stu-id="5fe38-287">These are hello logging levels:</span></span>
* <span data-ttu-id="5fe38-288">Hiba (kivételek)</span><span class="sxs-lookup"><span data-stu-id="5fe38-288">Error (exceptions)</span></span>
* <span data-ttu-id="5fe38-289">Figyelmeztetés (figyelmeztetés)</span><span class="sxs-lookup"><span data-stu-id="5fe38-289">Warn (warning)</span></span>
* <span data-ttu-id="5fe38-290">Info (tájékoztatási céllal)</span><span class="sxs-lookup"><span data-stu-id="5fe38-290">Info (information purposes)</span></span>
* <span data-ttu-id="5fe38-291">Részletes (További részletekért)</span><span class="sxs-lookup"><span data-stu-id="5fe38-291">Verbose (more details)</span></span>

<span data-ttu-id="5fe38-292">Hello naplózási szint ilyen állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="5fe38-292">You set hello log level like this:</span></span>

    Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

 <span data-ttu-id="5fe38-293">Az összes napló küldés toologcat, továbbá tooany egyéni napló visszahívások.</span><span class="sxs-lookup"><span data-stu-id="5fe38-293">All log messages are sent toologcat, in addition tooany custom log callbacks.</span></span>
<span data-ttu-id="5fe38-294">Letölthető egy naplófájl tooa logcat az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5fe38-294">You can get a log tooa file from logcat as follows:</span></span>

    adb logcat > "C:\logmsg\logfile.txt"

 <span data-ttu-id="5fe38-295">Adb parancsokkal kapcsolatos részletekért lásd: hello [hello Android webhelyéről logcat információk](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).</span><span class="sxs-lookup"><span data-stu-id="5fe38-295">For details about adb commands, see hello [logcat information on hello Android site](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).</span></span>

#### <a name="network-traces"></a><span data-ttu-id="5fe38-296">Hálózati nyomkövetés</span><span class="sxs-lookup"><span data-stu-id="5fe38-296">Network traces</span></span>
<span data-ttu-id="5fe38-297">Használhatja a különböző eszközök toocapture hello HTTP-forgalom, amely az adal-t állít elő.</span><span class="sxs-lookup"><span data-stu-id="5fe38-297">You can use various tools toocapture hello HTTP traffic that ADAL generates.</span></span>  <span data-ttu-id="5fe38-298">Ez akkor hasznos, ha jártas a hello OAuth protokollt, vagy ha tooprovide diagnosztikai adatokat tooMicrosoft vagy egyéb támogatási csatornáit van szüksége.</span><span class="sxs-lookup"><span data-stu-id="5fe38-298">This is most useful if you're familiar with hello OAuth protocol or if you need tooprovide diagnostic information tooMicrosoft or other support channels.</span></span>

<span data-ttu-id="5fe38-299">Fiddler hello legegyszerűbb HTTP eszköz.</span><span class="sxs-lookup"><span data-stu-id="5fe38-299">Fiddler is hello easiest HTTP tracing tool.</span></span> <span data-ttu-id="5fe38-300">Használjon hello következő hivatkozásait tooset toocorrectly rekord ADAL hálózati forgalom fel azt.</span><span class="sxs-lookup"><span data-stu-id="5fe38-300">Use hello following links tooset it up toocorrectly record ADAL network traffic.</span></span> <span data-ttu-id="5fe38-301">Olyan eszköz például a Fiddler vagy Charles toobe hasznos konfigurálnia kell az SSL-titkosítás nélkül toorecord forgalom.</span><span class="sxs-lookup"><span data-stu-id="5fe38-301">For a tracing tool like Fiddler or Charles toobe useful, you must configure it toorecord unencrypted SSL traffic.</span></span>  

> [!NOTE]
> <span data-ttu-id="5fe38-302">Nyomok jön létre, így például a hozzáférési jogkivonatok, felhasználónevek és jelszavak magas szintű jogosultsággal rendelkező adatokat tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="5fe38-302">Traces generated in this way might contain highly privileged information such as access tokens, usernames, and passwords.</span></span> <span data-ttu-id="5fe38-303">Éles fiókok használata, ne ossza meg a nyomkövetések harmadik felek számára.</span><span class="sxs-lookup"><span data-stu-id="5fe38-303">If you're using production accounts, do not share these traces with third parties.</span></span> <span data-ttu-id="5fe38-304">Ha toosupply egy nyomkövetési toosomeone rendelés tooget támogatására van szüksége, reprodukálja hello hibát egy ideiglenes fiókot, hogy nincs ellenére megosztása felhasználónevek és jelszavak használatával.</span><span class="sxs-lookup"><span data-stu-id="5fe38-304">If you need toosupply a trace toosomeone in order tooget support, reproduce hello issue by using a temporary account with usernames and passwords that you don't mind sharing.</span></span>

* <span data-ttu-id="5fe38-305">Hello Telerik webhelyéről: [beállítás mentése Fiddler az Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)</span><span class="sxs-lookup"><span data-stu-id="5fe38-305">From hello Telerik website: [Setting Up Fiddler For Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)</span></span>
* <span data-ttu-id="5fe38-306">A Githubból: [ADAL Fiddler szabályainak konfigurálása](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)</span><span class="sxs-lookup"><span data-stu-id="5fe38-306">From GitHub: [Configure Fiddler Rules For ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)</span></span>

### <a name="dialog-mode"></a><span data-ttu-id="5fe38-307">Párbeszédpanelen mód</span><span class="sxs-lookup"><span data-stu-id="5fe38-307">Dialog mode</span></span>
<span data-ttu-id="5fe38-308">hello acquireToken metódus tevékenység nélkül támogatja a párbeszédpanel megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="5fe38-308">hello acquireToken method without activity supports a dialog prompt.</span></span>

### <a name="encryption"></a><span data-ttu-id="5fe38-309">Titkosítás</span><span class="sxs-lookup"><span data-stu-id="5fe38-309">Encryption</span></span>
<span data-ttu-id="5fe38-310">ADAL hello jogkivonatokat és SharedPreferences tárban alapértelmezés szerint titkosítja.</span><span class="sxs-lookup"><span data-stu-id="5fe38-310">ADAL encrypts hello tokens and store in SharedPreferences by default.</span></span> <span data-ttu-id="5fe38-311">Hello StorageHelper toosee hello részleteit is megtekinthetik.</span><span class="sxs-lookup"><span data-stu-id="5fe38-311">You can look at hello StorageHelper class toosee hello details.</span></span> <span data-ttu-id="5fe38-312">Android 4.3 (API 18) biztonságos tárolására titkos kulcsok Android Keystore bevezetni.</span><span class="sxs-lookup"><span data-stu-id="5fe38-312">Android introduced Android Keystore for 4.3 (API 18) secure storage of private keys.</span></span> <span data-ttu-id="5fe38-313">Adal-t használ, amely az API 18 és az annál magasabb.</span><span class="sxs-lookup"><span data-stu-id="5fe38-313">ADAL uses that for API 18 and higher.</span></span> <span data-ttu-id="5fe38-314">Ha toouse ADAL SDK alacsonyabb verziójához, a titkos kulcs, AuthenticationSettings.INSTANCE.setSecretKey tooprovide kell.</span><span class="sxs-lookup"><span data-stu-id="5fe38-314">If you want toouse ADAL for lower SDK versions, you need tooprovide a secret key at AuthenticationSettings.INSTANCE.setSecretKey.</span></span>

### <a name="oauth2-bearer-challenge"></a><span data-ttu-id="5fe38-315">Az OAuth2 tulajdonosi kérdés</span><span class="sxs-lookup"><span data-stu-id="5fe38-315">OAuth2 bearer challenge</span></span>
<span data-ttu-id="5fe38-316">hello AuthenticationParameters osztály funkció tooget authorization_uri az OAuth2 tulajdonosi challenge hello biztosít.</span><span class="sxs-lookup"><span data-stu-id="5fe38-316">hello AuthenticationParameters class provides functionality tooget authorization_uri from hello OAuth2 bearer challenge.</span></span>

### <a name="session-cookies-in-webview"></a><span data-ttu-id="5fe38-317">Webes nézet munkamenet cookie-k</span><span class="sxs-lookup"><span data-stu-id="5fe38-317">Session cookies in WebView</span></span>
<span data-ttu-id="5fe38-318">Android webes nézet nem törli a munkamenetek cookie-jait hello alkalmazás bezárása után.</span><span class="sxs-lookup"><span data-stu-id="5fe38-318">Android WebView does not clear session cookies after hello app is closed.</span></span> <span data-ttu-id="5fe38-319">A mintakód használatával, amely kezelheti:</span><span class="sxs-lookup"><span data-stu-id="5fe38-319">You can handle that by using this sample code:</span></span>

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

<span data-ttu-id="5fe38-320">A cookie-k, lásd: hello [hello Android webhelyéről CookieSyncManager információk](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).</span><span class="sxs-lookup"><span data-stu-id="5fe38-320">For details about cookies, see hello [CookieSyncManager information on hello Android site](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).</span></span>

### <a name="resource-overrides"></a><span data-ttu-id="5fe38-321">Erőforrás-felülbírálások</span><span class="sxs-lookup"><span data-stu-id="5fe38-321">Resource overrides</span></span>
<span data-ttu-id="5fe38-322">hello ADAL-könyvtár ProgressDialog üzenetek angol karakterláncot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5fe38-322">hello ADAL library includes English strings for ProgressDialog messages.</span></span> <span data-ttu-id="5fe38-323">Az alkalmazás mindent felülír Ha azt szeretné, hogy a honosított karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="5fe38-323">Your application should overwrite them if you want localized strings.</span></span>

     <string name="app_loading">Loading...</string>
     <string name="broker_processing">Broker is processing</string>
     <string name="http_auth_dialog_username">Username</string>
     <string name="http_auth_dialog_password">Password</string>
     <string name="http_auth_dialog_title">Sign In</string>
     <string name="http_auth_dialog_login">Login</string>
     <string name="http_auth_dialog_cancel">Cancel</string>

### <a name="ntlm-dialog-box"></a><span data-ttu-id="5fe38-324">NTLM párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="5fe38-324">NTLM dialog box</span></span>
<span data-ttu-id="5fe38-325">1.1.0-ás ADAL-verziót támogatja az NTLM párbeszédpanel, amely WebViewClient hello onReceivedHttpAuthRequest esemény feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="5fe38-325">ADAL version 1.1.0 supports an NTLM dialog box that is processed through hello onReceivedHttpAuthRequest event from WebViewClient.</span></span> <span data-ttu-id="5fe38-326">Testre szabhatja hello elrendezés és karakterláncok hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5fe38-326">You can customize hello layout and strings for hello dialog box.</span></span>

### <a name="cross-app-sso"></a><span data-ttu-id="5fe38-327">Alkalmazások közötti SSO</span><span class="sxs-lookup"><span data-stu-id="5fe38-327">Cross-app SSO</span></span>
<span data-ttu-id="5fe38-328">Ismerje meg, [hogyan tooenable az ADAL használatával Android alkalmazások közötti SSO](active-directory-sso-android.md).</span><span class="sxs-lookup"><span data-stu-id="5fe38-328">Learn [how tooenable cross-app SSO on Android by using ADAL](active-directory-sso-android.md).</span></span>  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
