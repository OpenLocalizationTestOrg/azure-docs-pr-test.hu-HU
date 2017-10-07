---
title: "egy webalkalmazást az Azure App Service hello Azure SDK-t használó Java aaaCreate"
description: "Ismerje meg, hogyan toocreate programozott módon, Azure App Service webalkalmazás hello Javához készült Azure SDK-t."
tags: azure-classic-portal
services: app-service-web
documentationcenter: Java
author: donntrenton
manager: erikre
editor: jimbe
ms.assetid: 8954c456-1275-4d57-aff4-ca7d6374b71e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 02/25/2016
ms.author: v-donntr
ms.openlocfilehash: 42ba86b7fbb5668b3675198d0c5bb454525f706b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-in-azure-app-service-using-hello-azure-sdk-for-java"></a><span data-ttu-id="4b997-103">Az Azure App Service hello Azure SDK-t használó Java-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4b997-103">Create a Web App in Azure App Service using hello Azure SDK for Java</span></span>
<!-- Azure Active Directory workflow is not yet available on hello Azure Portal -->

## <a name="overview"></a><span data-ttu-id="4b997-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4b997-104">Overview</span></span>
<span data-ttu-id="4b997-105">Ez a forgatókönyv bemutatja, hogyan toocreate egy Java-alkalmazást hoz létre egy webalkalmazást az Azure SDK [Azure App Service][Azure App Service], majd telepítenie kell egy alkalmazás tooit.</span><span class="sxs-lookup"><span data-stu-id="4b997-105">This walkthrough shows you how toocreate an Azure SDK for Java application that creates a Web App in [Azure App Service][Azure App Service], then deploy an application tooit.</span></span> <span data-ttu-id="4b997-106">Két részből áll:</span><span class="sxs-lookup"><span data-stu-id="4b997-106">It consists of two parts:</span></span>

* <span data-ttu-id="4b997-107">1. rész bemutatja, hogyan toobuild Java-alkalmazás, amely létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4b997-107">Part 1 demonstrates how toobuild a Java application that creates a web app.</span></span>
* <span data-ttu-id="4b997-108">2. rész bemutatja, hogyan toocreate egy egyszerű "Hello, World" JSP alkalmazást, majd használja az FTP ügyfél toodeploy kód tooApp szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4b997-108">Part 2 demonstrates how toocreate a simple JSP "Hello World" application, then use an FTP client toodeploy code tooApp Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4b997-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4b997-109">Prerequisites</span></span>
### <a name="software-installations"></a><span data-ttu-id="4b997-110">Szoftvertelepítés</span><span class="sxs-lookup"><span data-stu-id="4b997-110">Software Installations</span></span>
<span data-ttu-id="4b997-111">Ebben a cikkben AzureWebDemo alkalmazáskód hello Azure Java SDK 0.7.0, amelyek segítségével lehet telepíteni használatával készült hello [Webplatform-telepítő] [ Web Platform Installer] (WebPI).</span><span class="sxs-lookup"><span data-stu-id="4b997-111">hello AzureWebDemo application code in this article was written using Azure Java SDK 0.7.0, which you can install using hello [Web Platform Installer][Web Platform Installer] (WebPI).</span></span> <span data-ttu-id="4b997-112">Ezenkívül győződjön meg arról, hogy toouse hello legújabb verziójának hello [Eclipse Azure eszköztára][Azure Toolkit for Eclipse].</span><span class="sxs-lookup"><span data-stu-id="4b997-112">In addition, make sure toouse hello latest version of hello [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="4b997-113">Hello SDK telepítése után frissítse az Eclipse-projekt hello függőséget futtatásával **frissítése** a **Maven Tárházak**, majd újra vegye fel a hello minden csomag legújabb verzióját hello  **Függőségek** ablak.</span><span class="sxs-lookup"><span data-stu-id="4b997-113">After you install hello SDK, update hello dependencies in your Eclipse project by running **Update Index** in **Maven Repositories**, then re-add hello latest version of each package in hello **Dependencies** window.</span></span> <span data-ttu-id="4b997-114">Az eclipse-ben telepített szoftvert hello verziója kattintva ellenőrizheti **Súgó > telepítésének részletei**; meg kell rendelkeznie legalább hello következő verziói:</span><span class="sxs-lookup"><span data-stu-id="4b997-114">You can verify hello version of your installed software in Eclipse by clicking **Help > Installation Details**; you should have at least hello following versions:</span></span>

* <span data-ttu-id="4b997-115">A Microsoft Azure-tárak Java 0.7.0.20150309 csomag</span><span class="sxs-lookup"><span data-stu-id="4b997-115">Package for Microsoft Azure Libraries for Java 0.7.0.20150309</span></span>
* <span data-ttu-id="4b997-116">Java EE-fejlesztőknek 4.4.2.20150219 készült eclipse IDE</span><span class="sxs-lookup"><span data-stu-id="4b997-116">Eclipse IDE for Java EE Developers 4.4.2.20150219</span></span>

### <a name="create-and-configure-cloud-resources-in-azure"></a><span data-ttu-id="4b997-117">Hozza létre és konfigurálja a felhőben található erőforrásokat az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="4b997-117">Create and Configure Cloud Resources in Azure</span></span>
<span data-ttu-id="4b997-118">Ez az eljárás megkezdése előtt kell toohave egy aktív Azure-előfizetéssel, és beállítható az alapértelmezett Active Directory (az AD) az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="4b997-118">Before you begin this procedure, you need toohave an active Azure subscription and set up a default Active Directory (AD) on Azure.</span></span>

### <a name="create-an-active-directory-ad-in-azure"></a><span data-ttu-id="4b997-119">Hozzon létre egy Active Directory (AD) az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="4b997-119">Create an Active Directory (AD) in Azure</span></span>
<span data-ttu-id="4b997-120">Ha még nem rendelkezik az Active Directory (AD) az Azure-előfizetése, jelentkezzen be hello [a klasszikus Azure portálon] [ Azure classic portal] Microsoft-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="4b997-120">If you do not already have an Active Directory (AD) on your Azure subscription, log into hello [Azure classic portal][Azure classic portal] with your Microsoft account.</span></span> <span data-ttu-id="4b997-121">Ha több előfizetése van, kattintson a **előfizetések** és select hello alapértelmezett címtár hello előfizetés keresi toouse ebben a projektben.</span><span class="sxs-lookup"><span data-stu-id="4b997-121">If you have multiple subscriptions, click **Subscriptions** and select hello default directory for hello subscription you want toouse for this project.</span></span> <span data-ttu-id="4b997-122">Kattintson a **alkalmaz** tooswitch toothat előfizetési nézet.</span><span class="sxs-lookup"><span data-stu-id="4b997-122">Then click **Apply** tooswitch toothat subscription view.</span></span>

1. <span data-ttu-id="4b997-123">Válassza ki **Active Directory** a bal oldali menüben hello.</span><span class="sxs-lookup"><span data-stu-id="4b997-123">Select **Active Directory** from hello menu at left.</span></span> <span data-ttu-id="4b997-124">**Új kattintson > Directory > egyéni létrehozás**.</span><span class="sxs-lookup"><span data-stu-id="4b997-124">**Click New > Directory > Custom Create**.</span></span>
2. <span data-ttu-id="4b997-125">A **könyvtár hozzáadása**, jelölje be **új könyvtár létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="4b997-125">In **Add Directory**, select **Create New Directory**.</span></span>
3. <span data-ttu-id="4b997-126">A **neve**, adja meg a könyvtár nevét.</span><span class="sxs-lookup"><span data-stu-id="4b997-126">In **Name**, enter a directory name.</span></span>
4. <span data-ttu-id="4b997-127">A **tartomány**, írja be a tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="4b997-127">In **Domain**, enter a domain name.</span></span> <span data-ttu-id="4b997-128">Ez az alapszintű tartománynevet, hogy a címtárban; alapértelmezés szerint hello űrlap rendelkezik `<domain_name>.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="4b997-128">This is a basic domain name that is included by default with your directory; it has hello form `<domain_name>.onmicrosoft.com`.</span></span> <span data-ttu-id="4b997-129">Hello directory vagy egy másik, saját tartománynév alapján nevére.</span><span class="sxs-lookup"><span data-stu-id="4b997-129">You can name it based on hello directory name or another domain name that you own.</span></span> <span data-ttu-id="4b997-130">Később a szervezet már használja egy másik tartománynevet is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="4b997-130">Later, you can add another domain name that your organization already uses.</span></span>
5. <span data-ttu-id="4b997-131">A **ország vagy régió**, válassza ki a területi beállítás.</span><span class="sxs-lookup"><span data-stu-id="4b997-131">In **Country or region**, select your locale.</span></span>

<span data-ttu-id="4b997-132">AD a további információkért lásd: [Mi az Azure AD-címtár][What is an Azure AD directory]?</span><span class="sxs-lookup"><span data-stu-id="4b997-132">For more information on AD, see [What is an Azure AD directory][What is an Azure AD directory]?</span></span>

### <a name="create-a-management-certificate-for-azure"></a><span data-ttu-id="4b997-133">Az Azure felügyeleti tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="4b997-133">Create a Management Certificate for Azure</span></span>
<span data-ttu-id="4b997-134">hello Javához készült Azure SDK-t használ, felügyeleti tanúsítványok tooauthenticate Azure-előfizetések.</span><span class="sxs-lookup"><span data-stu-id="4b997-134">hello Azure SDK for Java uses management certificates tooauthenticate with Azure subscriptions.</span></span> <span data-ttu-id="4b997-135">Ezek a X.509 v3 alapján létrehozott tanúsítványok tooauthenticate hello szolgáltatásfelügyeleti API tooact nevében hello előfizetés tulajdonosának toomanage előfizetéshez kapcsolódó erőforrásokat használó ügyfél-alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="4b997-135">These are X.509 v3 certificates you use tooauthenticate a client application that uses hello Service Management API tooact on behalf of hello subscription owner toomanage subscription resources.</span></span>

<span data-ttu-id="4b997-136">az alábbi eljárással hello kód egy önaláírt tanúsítványt tooauthenticate Azure használ.</span><span class="sxs-lookup"><span data-stu-id="4b997-136">hello code in this procedure uses a self-signed certificate tooauthenticate with Azure.</span></span> <span data-ttu-id="4b997-137">Az eljárás használatához toocreate tanúsítványt kell, és töltse fel toohello [a klasszikus Azure portálon] [ Azure classic portal] előre.</span><span class="sxs-lookup"><span data-stu-id="4b997-137">For this procedure, you need toocreate a certificate and upload it toohello [Azure classic portal][Azure classic portal] beforehand.</span></span> <span data-ttu-id="4b997-138">Ebbe beletartozik az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4b997-138">This involves hello following steps:</span></span>

* <span data-ttu-id="4b997-139">Az ügyféltanúsítvány képviselő PFX-fájl létrehozása, és mentse helyileg.</span><span class="sxs-lookup"><span data-stu-id="4b997-139">Generate a PFX file representing your client certificate and save it locally.</span></span>
* <span data-ttu-id="4b997-140">Felügyeleti tanúsítvány (CER-fájljával) hello PFX-fájlból jön létre.</span><span class="sxs-lookup"><span data-stu-id="4b997-140">Generate a management certificate (CER file) from hello PFX file.</span></span>
* <span data-ttu-id="4b997-141">Töltse fel a hello CER fájl tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="4b997-141">Upload hello CER file tooyour Azure subscription.</span></span>
* <span data-ttu-id="4b997-142">Hello PFX-fájl átalakítása JKS, mert Java használ, hogy formátum tooauthenticate tanúsítványok használatával.</span><span class="sxs-lookup"><span data-stu-id="4b997-142">Convert hello PFX file into JKS, because Java uses that format tooauthenticate using certificates.</span></span>
* <span data-ttu-id="4b997-143">Hello alkalmazás hitelesítési kód, amely hivatkozik a toohello helyi JKS fájl írása.</span><span class="sxs-lookup"><span data-stu-id="4b997-143">Write hello application's authentication code, which refers toohello local JKS file.</span></span>

<span data-ttu-id="4b997-144">Ez az eljárás befejezésekor hello CER tanúsítvány legyen elhelyezve, az Azure-előfizetése, és hello JKS tanúsítványt a helyi meghajtón legyen elhelyezve.</span><span class="sxs-lookup"><span data-stu-id="4b997-144">When you complete this procedure, hello CER certificate will reside in your Azure subscription and hello JKS certificate will reside on your local drive.</span></span> <span data-ttu-id="4b997-145">A felügyeleti tanúsítványokról további információ: [létrehozása és feltöltése az Azure felügyeleti tanúsítvánnyal][Create and Upload a Management Certificate for Azure].</span><span class="sxs-lookup"><span data-stu-id="4b997-145">For more information on management certificates, see [Create and Upload a Management Certificate for Azure][Create and Upload a Management Certificate for Azure].</span></span>

#### <a name="create-a-certificate"></a><span data-ttu-id="4b997-146">Tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="4b997-146">Create a certificate</span></span>
<span data-ttu-id="4b997-147">toocreate a saját önaláírt tanúsítványt, nyissa meg a parancskonzolról, az operációs rendszerben, és futtassa a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="4b997-147">toocreate your own self-signed certificate, open a command console on your operating system and run hello following commands.</span></span>

> <span data-ttu-id="4b997-148">**Megjegyzés:** hello számítógép, amelyen ezt a parancsot futtatja hello telepített JDK kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="4b997-148">**Note:**  hello computer on which you run this command must have hello JDK installed.</span></span> <span data-ttu-id="4b997-149">Hello elérési toohello kulcseszközről is, hello hely telepítésének hello JDK függ.</span><span class="sxs-lookup"><span data-stu-id="4b997-149">Also, hello path toohello keytool depends on hello location in which you install hello JDK.</span></span> <span data-ttu-id="4b997-150">További információkért lásd: [kulcs és a tanúsítvány eszköz (kulcseszközről)] [ Key and Certificate Management Tool (keytool)] a hello Java online dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="4b997-150">For more information, see [Key and Certificate Management Tool (keytool)][Key and Certificate Management Tool (keytool)] in hello Java online docs.</span></span>
> 
> 

<span data-ttu-id="4b997-151">toocreate hello .pfx-fájlt:</span><span class="sxs-lookup"><span data-stu-id="4b997-151">toocreate hello .pfx file:</span></span>

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

<span data-ttu-id="4b997-152">toocreate hello .cer fájl:</span><span class="sxs-lookup"><span data-stu-id="4b997-152">toocreate hello .cer file:</span></span>

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

<span data-ttu-id="4b997-153">Ahol:</span><span class="sxs-lookup"><span data-stu-id="4b997-153">where:</span></span>

* <span data-ttu-id="4b997-154">`<java-install-dir>`az hello elérési toohello könyvtár Java van telepítve.</span><span class="sxs-lookup"><span data-stu-id="4b997-154">`<java-install-dir>` is hello path toohello directory in which you installed Java.</span></span>
* <span data-ttu-id="4b997-155">`<keystore-id>`hello keystore bejegyzés azonosítója (például `AzureRemoteAccess`).</span><span class="sxs-lookup"><span data-stu-id="4b997-155">`<keystore-id>` is hello keystore entry identifier (for example, `AzureRemoteAccess`).</span></span>
* <span data-ttu-id="4b997-156">`<cert-store-dir>`hello elérési toohello könyvtár, amelyben toostore tanúsítványokat kívánja (például `C:/Certificates`).</span><span class="sxs-lookup"><span data-stu-id="4b997-156">`<cert-store-dir>` is hello path toohello directory in which you want toostore certificates (for example `C:/Certificates`).</span></span>
* <span data-ttu-id="4b997-157">`<cert-file-name>`hello tanúsítványfájl hello neve (például `AzureWebDemoCert`).</span><span class="sxs-lookup"><span data-stu-id="4b997-157">`<cert-file-name>` is hello name of hello certificate file (for example `AzureWebDemoCert`).</span></span>
* <span data-ttu-id="4b997-158">`<password>`hello jelszó tooprotect hello tanúsítvány; kiválasztása legalább 6 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4b997-158">`<password>` is hello password you choose tooprotect hello certificate; it must be at least 6 characters long.</span></span> <span data-ttu-id="4b997-159">Nincs jelszava, akkor adhatja meg, bár ez nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="4b997-159">You can enter no password, although this is not recommended.</span></span>
* <span data-ttu-id="4b997-160">`<dname>`hello X.500 megkülönböztető név toobe társított alias, és hello kibocsátó és tulajdonos mezőket hello önaláírt tanúsítványt használt.</span><span class="sxs-lookup"><span data-stu-id="4b997-160">`<dname>` is hello X.500 Distinguished Name toobe associated with alias, and is used as hello issuer and subject fields in hello self-signed certificate.</span></span>

<span data-ttu-id="4b997-161">További információkért lásd: [létrehozása és feltöltése az Azure felügyeleti tanúsítvánnyal][Create and Upload a Management Certificate for Azure].</span><span class="sxs-lookup"><span data-stu-id="4b997-161">For more information, see [Create and Upload a Management Certificate for Azure][Create and Upload a Management Certificate for Azure].</span></span>

#### <a name="upload-hello-certificate"></a><span data-ttu-id="4b997-162">Hello-tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="4b997-162">Upload hello certificate</span></span>
<span data-ttu-id="4b997-163">tooupload egy önaláírt tanúsítványt tooAzure lépjen toohello **beállítások** hello klasszikus portál lapon, majd kattintson az hello **felügyeleti tanúsítványok** fülre. Kattintson a **feltöltése** hello hello alján lapon, és keresse meg a létrehozott hello CER-fájljával toohello helyét.</span><span class="sxs-lookup"><span data-stu-id="4b997-163">tooupload a self-signed certificate tooAzure, go toohello **Settings** page in hello classic portal, then click hello **Management Certificates** tab. Click **Upload** at hello bottom of hello page and navigate toohello location of hello CER file you created.</span></span>

#### <a name="convert-hello-pfx-file-into-jks"></a><span data-ttu-id="4b997-164">Hello PFX-fájl átalakítása JKS</span><span class="sxs-lookup"><span data-stu-id="4b997-164">Convert hello PFX file into JKS</span></span>
<span data-ttu-id="4b997-165">Hello Windows parancssort (rendszergazdaként fut), a cd toohello könyvtárat tartalmazó hello tanúsítványokat, és futtassa a következő parancsot, hello ahol `<java-install-dir>` hello könyvtár, amelyben a számítógépre telepítette Java:</span><span class="sxs-lookup"><span data-stu-id="4b997-165">In hello Windows Command Prompt (running as admin), cd toohello directory containing hello certificates and run hello following command, where `<java-install-dir>` is hello directory in which you installed Java on your computer:</span></span>

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. <span data-ttu-id="4b997-166">Amikor a rendszer kéri, adja meg a hello cél keystore jelszót; Ez lesz hello jelszót hello JKS fájl.</span><span class="sxs-lookup"><span data-stu-id="4b997-166">When prompted, enter hello destination keystore password; this will be hello password for hello JKS file.</span></span>
2. <span data-ttu-id="4b997-167">Amikor a rendszer kéri, adja meg a hello forrás keystore jelszót; Ez a megadott PFX-fájljának hello hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="4b997-167">When prompted, enter hello source keystore password; this is hello password you specified for hello PFX file.</span></span>

<span data-ttu-id="4b997-168">hello két jelszó azonos hello toobe nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4b997-168">hello two passwords do not have toobe hello same.</span></span> <span data-ttu-id="4b997-169">Nincs jelszava, akkor adhatja meg, bár ez nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="4b997-169">You can enter no password, although this is not recommended.</span></span>

## <a name="build-a-web-app-creation-application"></a><span data-ttu-id="4b997-170">A webalkalmazás létrehozása alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4b997-170">Build a Web App creation application</span></span>
### <a name="create-hello-eclipse-workspace-and-maven-project"></a><span data-ttu-id="4b997-171">Hozzon létre Eclipse munkaterületet, és Maven Project hello</span><span class="sxs-lookup"><span data-stu-id="4b997-171">Create hello Eclipse Workspace and Maven Project</span></span>
<span data-ttu-id="4b997-172">Ez a szakasz a munkaterületet, és egy Maven-projektjét, amely hello alkalmazás létrehozását, nevű webalkalmazás AzureWebDemo hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4b997-172">In this section you create a workspace and a Maven project for hello web app creation application, named AzureWebDemo.</span></span>

1. <span data-ttu-id="4b997-173">Hozzon létre egy új Maven-projektet.</span><span class="sxs-lookup"><span data-stu-id="4b997-173">Create a new Maven project.</span></span> <span data-ttu-id="4b997-174">Kattintson a **fájl > Új > Maven Project**.</span><span class="sxs-lookup"><span data-stu-id="4b997-174">Click **File > New > Maven Project**.</span></span> <span data-ttu-id="4b997-175">A **új Maven Project**, jelölje be **hozzon létre egy egyszerű projektet** és **alapértelmezett munkaterület helyet**.</span><span class="sxs-lookup"><span data-stu-id="4b997-175">In **New Maven Project**, select **Create a simple project** and **Use default workspace location**.</span></span>
2. <span data-ttu-id="4b997-176">Második lapján hello **új Maven Project**, adja meg a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="4b997-176">On hello second page of **New Maven Project**, specify hello following:</span></span>
   
   * <span data-ttu-id="4b997-177">Csoport azonosítója:`com.<username>.azure.webdemo`</span><span class="sxs-lookup"><span data-stu-id="4b997-177">Group ID: `com.<username>.azure.webdemo`</span></span>
   * <span data-ttu-id="4b997-178">Összetevő-azonosító: AzureWebDemo</span><span class="sxs-lookup"><span data-stu-id="4b997-178">Artifact ID: AzureWebDemo</span></span>
   * <span data-ttu-id="4b997-179">Verzió: 0.0.1-SNAPSHOT</span><span class="sxs-lookup"><span data-stu-id="4b997-179">Version: 0.0.1-SNAPSHOT</span></span>
   * <span data-ttu-id="4b997-180">Csomagolására: jar</span><span class="sxs-lookup"><span data-stu-id="4b997-180">Packaging: jar</span></span>
   * <span data-ttu-id="4b997-181">Name: AzureWebDemo</span><span class="sxs-lookup"><span data-stu-id="4b997-181">Name: AzureWebDemo</span></span>
     
     <span data-ttu-id="4b997-182">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="4b997-182">Click **Finish**.</span></span>
3. <span data-ttu-id="4b997-183">Nyissa meg a Project Explorer hello új projekt pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="4b997-183">Open hello new project's pom.xml file in Project Explorer.</span></span> <span data-ttu-id="4b997-184">Jelölje be hello **függőségek** fülre. Mivel ez egy új projektet, még felsorolt csomagok száma.</span><span class="sxs-lookup"><span data-stu-id="4b997-184">Select hello **Dependencies** tab. As this is a new project, no packages are listed yet.</span></span>
4. <span data-ttu-id="4b997-185">Nyissa meg hello Maven adattárak megtekintése.</span><span class="sxs-lookup"><span data-stu-id="4b997-185">Open hello Maven Repositories view.</span></span> <span data-ttu-id="4b997-186">**Kattintson az ablak > nézet megjelenítése > más > Maven > Maven Tárházak** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b997-186">**Click Window > Show View > Other > Maven > Maven Repositories** and click **OK**.</span></span> <span data-ttu-id="4b997-187">Hello **Maven Tárházak** nézete hello IDE hello alján jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4b997-187">hello **Maven Repositories** view will appear at hello bottom of hello IDE.</span></span>
5. <span data-ttu-id="4b997-188">Nyissa meg **globális adattárak**, kattintson a jobb gombbal hello **központi** tárház, és válassza ki **Rebuild Index**.</span><span class="sxs-lookup"><span data-stu-id="4b997-188">Open **Global Repositories**, right-click hello **central** repository, and select **Rebuild Index**.</span></span>
   
    ![][1]
   
    <span data-ttu-id="4b997-189">Ez a lépés attól függően, hogy a kapcsolat sebességétől hello több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="4b997-189">This step can take several minutes depending on hello speed of your connection.</span></span> <span data-ttu-id="4b997-190">Hello index újraépíti, amikor látnia kell a Microsoft Azure hello csomagok hello **központi** Maven-tárházat.</span><span class="sxs-lookup"><span data-stu-id="4b997-190">When hello index rebuilds, you should see hello Microsoft Azure packages in hello **central** Maven repository.</span></span>
6. <span data-ttu-id="4b997-191">A **függőségek**, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4b997-191">In **Dependencies**, click **Add**.</span></span> <span data-ttu-id="4b997-192">A **adja meg a csoport azonosítója...**  meg `azure-management`.</span><span class="sxs-lookup"><span data-stu-id="4b997-192">In **Enter Group ID...** enter `azure-management`.</span></span> <span data-ttu-id="4b997-193">Válassza ki az alapszintű és az App Service Web Apps hello csomagok:</span><span class="sxs-lookup"><span data-stu-id="4b997-193">Select hello packages for base management and App Service Web Apps management:</span></span>
   
        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites
   
   > <span data-ttu-id="4b997-194">**Megjegyzés:** hello függőségek követően egy új verzióra frissít, ha szüksége van-e toore-adjon hozzá minden hello függőségek ezen a listán.</span><span class="sxs-lookup"><span data-stu-id="4b997-194">**Note:** If you are updating hello dependencies after a new version release, you need toore-add each of hello dependencies in this list.</span></span>
   > <span data-ttu-id="4b997-195">Miután rákattintott **Hozzáadás** és válassza az egyes függőségek hello hello az új verziószámmal valószínűleg **függőségek** lista.</span><span class="sxs-lookup"><span data-stu-id="4b997-195">After you click **Add** and select each dependency, it appears with hello new version number in hello **Dependencies** list.</span></span>
   > 
   > 

<span data-ttu-id="4b997-196">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="4b997-196">Click **OK**.</span></span> <span data-ttu-id="4b997-197">hello Azure csomagok akkor jelennek meg hello **függőségek** listája.</span><span class="sxs-lookup"><span data-stu-id="4b997-197">hello Azure packages then appear in hello **Dependencies** list.</span></span>

### <a name="writing-java-code-toocreate-a-web-app-by-calling-hello-azure-sdk"></a><span data-ttu-id="4b997-198">A webes alkalmazás Java-kóddal tooCreate írása hívása hello Azure SDK által</span><span class="sxs-lookup"><span data-stu-id="4b997-198">Writing Java Code tooCreate a Web App by Calling hello Azure SDK</span></span>
<span data-ttu-id="4b997-199">Következő lépésként írja meg hello behívó kód API-k hello Azure SDK-t a Java toocreate hello App Service-webalkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="4b997-199">Next, write hello code that calls APIs in hello Azure SDK for Java toocreate hello App Service web app.</span></span>

1. <span data-ttu-id="4b997-200">Hozzon létre egy Java class toocontain hello fő belépési pont kódot.</span><span class="sxs-lookup"><span data-stu-id="4b997-200">Create a Java class toocontain hello main entry point code.</span></span> <span data-ttu-id="4b997-201">A Project Explorer hello projekt csomóponton kattintson a jobb gombbal, és válassza ki **új > osztály**.</span><span class="sxs-lookup"><span data-stu-id="4b997-201">In Project Explorer, right-click on hello project node and select **New > Class**.</span></span>
2. <span data-ttu-id="4b997-202">A **új Java-osztály**, hello osztály neve `WebCreator` , és ellenőrizze a hello **nyilvános statikus "void" main** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="4b997-202">In **New Java Class**, name hello class `WebCreator` and check hello **public static void main** checkbox.</span></span> <span data-ttu-id="4b997-203">hello beállításokat a következőképpen kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4b997-203">hello selections should appear as follows:</span></span>
   
    ![][2]
3. <span data-ttu-id="4b997-204">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="4b997-204">Click **Finish**.</span></span> <span data-ttu-id="4b997-205">hello WebCreator.java fájl Project Explorer jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4b997-205">hello WebCreator.java file appears in Project Explorer.</span></span>

### <a name="calling-hello-azure-api-toocreate-an-app-service-web-app"></a><span data-ttu-id="4b997-206">Hello Azure API tooCreate egy App Service Web App hívja.</span><span class="sxs-lookup"><span data-stu-id="4b997-206">Calling hello Azure API tooCreate an App Service Web App</span></span>
#### <a name="add-necessary-imports"></a><span data-ttu-id="4b997-207">Adja hozzá a szükséges importálása</span><span class="sxs-lookup"><span data-stu-id="4b997-207">Add necessary imports</span></span>
<span data-ttu-id="4b997-208">WebCreator.java adja hozzá a következő importálási utasításban; hello Ezek importálása nyújt hozzáférést tooclasses hello a kezelési kódtárakat Azure API-k fel:</span><span class="sxs-lookup"><span data-stu-id="4b997-208">In WebCreator.java, add hello following imports; these imports provide access tooclasses in hello management libraries for consuming Azure APIs:</span></span>

    // General imports
    import java.net.URI;
    import java.util.ArrayList;

    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;

    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;

    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;

    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-hello-main-entry-point-class"></a><span data-ttu-id="4b997-209">Adja meg a hello fő belépési pont osztály</span><span class="sxs-lookup"><span data-stu-id="4b997-209">Define hello main entry point class</span></span>
<span data-ttu-id="4b997-210">Hello hello AzureWebDemo alkalmazás célja toocreate egy App Service Web App, mert az alkalmazás neve hello fő osztály `WebAppCreator`.</span><span class="sxs-lookup"><span data-stu-id="4b997-210">Because hello purpose of hello AzureWebDemo application is toocreate an App Service Web App, name hello main class for this application `WebAppCreator`.</span></span> <span data-ttu-id="4b997-211">Ez az osztály hello fő belépési pont behívó kód hello Azure szolgáltatásfelügyeleti API toocreate hello webalkalmazás biztosít.</span><span class="sxs-lookup"><span data-stu-id="4b997-211">This class provides hello main entry point code that calls hello Azure Service Management API toocreate hello web app.</span></span>

<span data-ttu-id="4b997-212">Adja hozzá az alábbi paraméterdefiníciókra hello webes alkalmazáshoz és webtárhely hello.</span><span class="sxs-lookup"><span data-stu-id="4b997-212">Add hello following parameter definitions for hello web app and webspace.</span></span> <span data-ttu-id="4b997-213">Szüksége lesz tooprovide saját Azure-előfizetés azonosítója és a tanúsítvány adatait.</span><span class="sxs-lookup"><span data-stu-id="4b997-213">You will need tooprovide your own Azure subscription ID and certificate information.</span></span>

    public class WebAppCreator {

        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";

        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

<span data-ttu-id="4b997-214">Ahol:</span><span class="sxs-lookup"><span data-stu-id="4b997-214">where:</span></span>

* <span data-ttu-id="4b997-215">`<subscription-id>`a rendszer használni kívánt toocreate hello erőforrás hello Azure-előfizetés Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="4b997-215">`<subscription-id>` is hello Azure subscription ID in which you want toocreate hello resource.</span></span>
* <span data-ttu-id="4b997-216">`<certificate-store-path>`hello elérési út és fájlnév toohello JKS fájlt a helyi tanúsítványtárolóban tároló könyvtárban van.</span><span class="sxs-lookup"><span data-stu-id="4b997-216">`<certificate-store-path>` is hello path and filename toohello JKS file in your local certificate store directory.</span></span> <span data-ttu-id="4b997-217">Például `C:/Certificates/CertificateName.jks` Linux és `C:\Certificates\CertificateName.jks` Windows.</span><span class="sxs-lookup"><span data-stu-id="4b997-217">For example, `C:/Certificates/CertificateName.jks` for Linux and `C:\Certificates\CertificateName.jks` for Windows.</span></span>
* <span data-ttu-id="4b997-218">`<certificate-password>`az a JKS tanúsítvány létrehozásakor megadott hello jelszó.</span><span class="sxs-lookup"><span data-stu-id="4b997-218">`<certificate-password>` is hello password you specified when you created your JKS certificate.</span></span>
* <span data-ttu-id="4b997-219">`webAppName`bármilyen nevet választania; Ez az eljárás hello nevét használja `WebDemoWebApp`.</span><span class="sxs-lookup"><span data-stu-id="4b997-219">`webAppName` can be any name you choose; this procedure uses hello name `WebDemoWebApp`.</span></span> <span data-ttu-id="4b997-220">hello teljes tartománynév megadása hello `webAppName` a hello `domainName` fűzött, így ebben az esetben hello teljes tartománya `webdemowebapp.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="4b997-220">hello full domain name is hello `webAppName` with hello `domainName` appended, so in this case hello full domain is `webdemowebapp.azurewebsites.net`.</span></span>
* <span data-ttu-id="4b997-221">`domainName`meg kell adni a fentiek szerint.</span><span class="sxs-lookup"><span data-stu-id="4b997-221">`domainName` should be specified as shown above.</span></span>
* <span data-ttu-id="4b997-222">`webSpaceName`hello definiált hello értékek egyike lehet: [WebSpaceNames] [ WebSpaceNames] osztály.</span><span class="sxs-lookup"><span data-stu-id="4b997-222">`webSpaceName` should be one of hello values defined in hello [WebSpaceNames][WebSpaceNames] class.</span></span>
* <span data-ttu-id="4b997-223">`appServicePlanName`meg kell adni a fentiek szerint.</span><span class="sxs-lookup"><span data-stu-id="4b997-223">`appServicePlanName` should be specified as shown above.</span></span>

> <span data-ttu-id="4b997-224">**Megjegyzés:** minden alkalommal, amikor az alkalmazás futtatásához, toochange hello értékének kell `webAppName` és `appServicePlanName` (vagy az Azure portál hello hello a webalkalmazás törlése) hello alkalmazás ismételt futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="4b997-224">**Note:** Each time you run this application, you need toochange hello value of `webAppName` and `appServicePlanName` (or delete hello web app on hello Azure Portal) before running hello application again.</span></span> <span data-ttu-id="4b997-225">Ellenkező esetben végrehajtása sikertelen lesz, mert hello ugyanaz az erőforrás már létezik az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="4b997-225">Otherwise, execution will fail because hello same resource already exists on Azure.</span></span>
> 
> 

#### <a name="define-hello-web-creation-method"></a><span data-ttu-id="4b997-226">Hello webes létrehozási módjának megadása</span><span class="sxs-lookup"><span data-stu-id="4b997-226">Define hello web creation method</span></span>
<span data-ttu-id="4b997-227">A következő határozza meg a metódus toocreate hello webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4b997-227">Next, define a method toocreate hello web app.</span></span> <span data-ttu-id="4b997-228">Ezzel a módszerrel `createWebApp`, hello web app és hello webtárhely hello paramétereit.</span><span class="sxs-lookup"><span data-stu-id="4b997-228">This method, `createWebApp`, specifies hello parameters of hello web app and hello webspace.</span></span> <span data-ttu-id="4b997-229">Emellett hoz létre, és beállítja az hello App Service Web Apps felügyeleti ügyfél, hello által definiált [WebSiteManagementClient] [ WebSiteManagementClient] objektum.</span><span class="sxs-lookup"><span data-stu-id="4b997-229">It also creates and configures hello App Service Web Apps management client, which is defined by hello [WebSiteManagementClient][WebSiteManagementClient] object.</span></span> <span data-ttu-id="4b997-230">hello felügyeleti ügyfél kulcs toocreating webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="4b997-230">hello management client is key toocreating Web Apps.</span></span> <span data-ttu-id="4b997-231">Engedélyező alkalmazások toomanage webes alkalmazásokat (például létrehozása, update és delete, a műveletek végrehajtása) hello service management API meghívásával RESTful webes szolgáltatásokat nyújt.</span><span class="sxs-lookup"><span data-stu-id="4b997-231">It provides RESTful web services that allow applications toomanage web apps (performing operations such as create, update, and delete) by calling hello service management API.</span></span>

    private static void createWebApp() throws Exception {

        // Specify configuration settings for hello App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path toohello JKS file
            keyStorePassword,  // Password for hello JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create hello App Service Web Apps management client toocall Azure APIs
        // and pass it hello App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for hello web app with hello specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that hello server farm name takes hello Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define hello web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create hello web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output hello HTTP status code of hello response; 200 indicates hello request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output hello name of hello web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

<span data-ttu-id="4b997-232">hello kód kimeneteként hello HTTP-állapot – hello válasz sikerességét vagy sikertelenségét jelző, és ha sikeres, kimeneteként webalkalmazás létrehozása hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="4b997-232">hello code will output hello HTTP status of hello response indicating success or failure, and if successful, will output hello name of hello created web app.</span></span>

#### <a name="define-hello-main-method"></a><span data-ttu-id="4b997-233">Hello main() módszer megadása</span><span class="sxs-lookup"><span data-stu-id="4b997-233">Define hello main() method</span></span>
<span data-ttu-id="4b997-234">Adja meg a hello main() metódus kódot hívások createWebApp() toocreate hello webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4b997-234">Provide hello main() method code that calls createWebApp() toocreate hello web app.</span></span>

<span data-ttu-id="4b997-235">Végezetül hívás `createWebApp` a `main`:</span><span class="sxs-lookup"><span data-stu-id="4b997-235">Finally, call `createWebApp` from `main`:</span></span>

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-hello-application-and-verify-web-app-creation"></a><span data-ttu-id="4b997-236">Hello alkalmazás fut, és ellenőrizze a webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4b997-236">Run hello application and verify web app creation</span></span>
<span data-ttu-id="4b997-237">Kattintson az alkalmazást futtató tooverify **futtatása > futtassa**.</span><span class="sxs-lookup"><span data-stu-id="4b997-237">tooverify that your application runs, click **Run > Run**.</span></span> <span data-ttu-id="4b997-238">Hello alkalmazás befejeződésekor kell megjelennie a következő kimeneti hello Eclipse konzolon hello:</span><span class="sxs-lookup"><span data-stu-id="4b997-238">When hello application completes running, you should see hello following output in hello Eclipse console:</span></span>

    ----------
    Web app created - HTTP response 200

    ----------

    Name of web app created: WebDemoWebApp

    ----------

<span data-ttu-id="4b997-239">Jelentkezzen be a klasszikus Azure portálon hello, majd kattintson a **webalkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4b997-239">Log into hello Azure classic portal and click **Web Apps**.</span></span> <span data-ttu-id="4b997-240">Új webalkalmazás hello meg kell jelennie hello webalkalmazások listájában néhány percen belül.</span><span class="sxs-lookup"><span data-stu-id="4b997-240">hello new web app should appear in hello Web Apps list within a few minutes.</span></span>

## <a name="deploying-an-application-toohello-web-app"></a><span data-ttu-id="4b997-241">Egy alkalmazás toohello webalkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="4b997-241">Deploying an Application toohello Web App</span></span>
<span data-ttu-id="4b997-242">Után futtatta AzureWebDemo hello új webalkalmazás létrehozása, jelentkezzen be a klasszikus portálon hello gombra **Web Apps**, és válassza ki **WebDemoWebApp** a hello **Web Apps** lista.</span><span class="sxs-lookup"><span data-stu-id="4b997-242">After you have run AzureWebDemo and created hello new web app, log into hello classic portal, click **Web Apps**, and select **WebDemoWebApp** in hello **Web Apps** list.</span></span> <span data-ttu-id="4b997-243">Hello webes alkalmazás irányítópult-oldalon, kattintson **Tallózás** (hello URL-címet, vagy `webdemowebapp.azurewebsites.net`) toonavigate tooit.</span><span class="sxs-lookup"><span data-stu-id="4b997-243">In hello web app's dashboard page, click **Browse** (or click hello URL, `webdemowebapp.azurewebsites.net`) toonavigate tooit.</span></span> <span data-ttu-id="4b997-244">Egy üres helyőrző lap jelenik meg, mert nincs tartalma még közzétett toohello webalkalmazás le lett.</span><span class="sxs-lookup"><span data-stu-id="4b997-244">You will see a blank placeholder page, because no content has been published toohello web app yet.</span></span>

<span data-ttu-id="4b997-245">Ezután létrehoz egy "Hello, World" alkalmazást, és toohello webalkalmazás telepítése az.</span><span class="sxs-lookup"><span data-stu-id="4b997-245">Next you will create a "Hello World" application and deploy it toohello web app.</span></span>

### <a name="create-a-jsp-hello-world-application"></a><span data-ttu-id="4b997-246">JSP Hello World-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4b997-246">Create a JSP Hello World application</span></span>
#### <a name="create-hello-application"></a><span data-ttu-id="4b997-247">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4b997-247">Create hello application</span></span>
<span data-ttu-id="4b997-248">A sorrend toodemonstrate hogyan toodeploy egy alkalmazás toohello webes hello a következő eljárás bemutatja, hogyan toocreate egy egyszerű "Hello World" Java-alkalmazást, és töltse fel toohello App Service Web App, az alkalmazás hozott létre.</span><span class="sxs-lookup"><span data-stu-id="4b997-248">In order toodemonstrate how toodeploy an application toohello web, hello following procedure shows you how toocreate a simple "Hello World" Java application and upload it toohello App Service Web App that your application created.</span></span>

1. <span data-ttu-id="4b997-249">Kattintson a **fájl > Új > dinamikus webes projekt**.</span><span class="sxs-lookup"><span data-stu-id="4b997-249">Click **File > New > Dynamic Web Project**.</span></span> <span data-ttu-id="4b997-250">Nevezze el a következőképpen: `JSPHello`.</span><span class="sxs-lookup"><span data-stu-id="4b997-250">Name it `JSPHello`.</span></span> <span data-ttu-id="4b997-251">Nem kell toochange ezen a párbeszédpanelen egyéb beállításait.</span><span class="sxs-lookup"><span data-stu-id="4b997-251">You do not need toochange any other settings in this dialog.</span></span> <span data-ttu-id="4b997-252">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="4b997-252">Click **Finish**.</span></span>
   
    ![][3]
2. <span data-ttu-id="4b997-253">A Project Explorer bontsa ki a hello **JSPHello** projektre, kattintson a jobb gombbal **WebContent**, majd kattintson a **új > JSP-fájl**.</span><span class="sxs-lookup"><span data-stu-id="4b997-253">In Project Explorer, expand hello **JSPHello** project, right-click **WebContent**, then click **New > JSP File**.</span></span> <span data-ttu-id="4b997-254">Hello új JSP-fájl létrehozása párbeszédpanelen hello új fájl neve `index.jsp`.</span><span class="sxs-lookup"><span data-stu-id="4b997-254">In hello New JSP File dialog, name hello new file `index.jsp`.</span></span> <span data-ttu-id="4b997-255">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="4b997-255">Click **Next**.</span></span>
3. <span data-ttu-id="4b997-256">A hello **JSP-sablon kiválasztása** párbeszédpanelen válassza **új JSP-fájl (html)** kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="4b997-256">In hello **Select JSP Template** dialog, select **New JSP File (html)** and click **Finish**.</span></span>
4. <span data-ttu-id="4b997-257">Index.jsp, adja hozzá a következő kódot a hello hello `<head>` és `<body>` szakaszok címkét:</span><span class="sxs-lookup"><span data-stu-id="4b997-257">In index.jsp, add hello following code in hello `<head>` and `<body>` tag sections:</span></span>
   
        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
   
        <body>
          Hello, hello time is <%= date %> 
        </body>

#### <a name="run-hello-hello-world-application-in-localhost"></a><span data-ttu-id="4b997-258">Hello Hello World alkalmazást futtat localhost</span><span class="sxs-lookup"><span data-stu-id="4b997-258">Run hello Hello World application in localhost</span></span>
<span data-ttu-id="4b997-259">Ez az alkalmazás futtatása előtt kell tooconfigure bizonyos tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="4b997-259">Before you run this application, you need tooconfigure a few properties.</span></span>

1. <span data-ttu-id="4b997-260">Kattintson a jobb gombbal hello **JSPHello** projektre, és válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="4b997-260">Right-click hello **JSPHello** project and select **Properties**.</span></span>
2. <span data-ttu-id="4b997-261">A hello **tulajdonságok** párbeszédpanel: Jelölje ki **Java Build elérési**, jelölje be hello **rendezés és exportálási** lapon jelölje **JRE rendszerkönyvtár**, kattintson a **Mentése** toomove azt toohello hello lista elejére.</span><span class="sxs-lookup"><span data-stu-id="4b997-261">In hello **Properties** dialog: select **Java Build Path**, select hello **Order and Export** tab, check **JRE System Library**, then click **Up** toomove it toohello top of hello list.</span></span>
   
    ![][4]
3. <span data-ttu-id="4b997-262">Is a hello **tulajdonságok** párbeszédpanel: válasszon **megcélzott futtatókörnyezetek** kattintson **új**.</span><span class="sxs-lookup"><span data-stu-id="4b997-262">Also in hello **Properties** dialog: select **Targeted Runtimes** and click **New**.</span></span>
4. <span data-ttu-id="4b997-263">A hello **új kiszolgáló Futtatókörnyezetét** párbeszédpanelen válasszon ki egy kiszolgálót, mint **Apache Tomcat v7.0** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="4b997-263">In hello **New Server Runtime Environment** dialog, select a server such as **Apache Tomcat v7.0** and click **Next**.</span></span> <span data-ttu-id="4b997-264">A hello **Tomcat kiszolgálót** párbeszédpanelen, a set **neve** túl`Apache Tomcat v7.0`, és állítsa be **Tomcat telepítési könyvtárát** toohello directory hello verziója van telepítve Azt szeretné, hogy toouse tomcat kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="4b997-264">In hello **Tomcat Server** dialog, set **Name** too`Apache Tomcat v7.0`, and set **Tomcat Installation Directory** toohello directory in which you installed hello version of Tomcat server you want toouse.</span></span>
   
    ![][5]
   
    <span data-ttu-id="4b997-265">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="4b997-265">Click **Finish**.</span></span>
5. <span data-ttu-id="4b997-266">Majd térjen vissza toohello **megcélzott futtatókörnyezetek** hello oldalán **tulajdonságok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4b997-266">You then return toohello **Targeted Runtimes** page of hello **Properties** dialog.</span></span> <span data-ttu-id="4b997-267">Válassza ki **Apache Tomcat v7.0**, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b997-267">Select **Apache Tomcat v7.0**, then click **OK**.</span></span>
   
    ![][6]
6. <span data-ttu-id="4b997-268">Az hello Eclipse **futtassa** menüben kattintson a **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="4b997-268">In hello Eclipse **Run** menu, click **Run**.</span></span> <span data-ttu-id="4b997-269">A hello **futtató** párbeszédablakban válassza **futtassa a kiszolgálón**.</span><span class="sxs-lookup"><span data-stu-id="4b997-269">In hello **Run As** dialog, select **Run on Server**.</span></span> <span data-ttu-id="4b997-270">A hello **futtassa a kiszolgálón** párbeszédablakban válassza **Tomcat v7.0 Server**:</span><span class="sxs-lookup"><span data-stu-id="4b997-270">In hello **Run on Server** dialog, select **Tomcat v7.0 Server**:</span></span>
   
    ![][7]
   
    <span data-ttu-id="4b997-271">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="4b997-271">Click **Finish**.</span></span>
7. <span data-ttu-id="4b997-272">Ha hello az alkalmazás fut, megtekintheti az hello **JSPHello** lap jelenik meg az eclipse-ben localhost ablakban (`http://localhost:8080/JSPHello/`), megjelenítés hello a következő üzenetet:</span><span class="sxs-lookup"><span data-stu-id="4b997-272">When hello application runs, you should see hello **JSPHello** page appear in a localhost window in Eclipse (`http://localhost:8080/JSPHello/`), displaying hello following message:</span></span>
   
    `Hello World, hello time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="export-hello-application-as-a-war"></a><span data-ttu-id="4b997-273">Exportálás hello alkalmazás WAR-fájlként</span><span class="sxs-lookup"><span data-stu-id="4b997-273">Export hello application as a WAR</span></span>
<span data-ttu-id="4b997-274">Hello web project fájlok exportálása egy webes archívumfájl (WAR), hogy Ezután telepítheti azt toohello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4b997-274">Export hello web project files as a web archive (WAR) file so that you can deploy it toohello web app.</span></span> <span data-ttu-id="4b997-275">hello következő web project fájlok találhatók hello WebContent mappában:</span><span class="sxs-lookup"><span data-stu-id="4b997-275">hello following web project files reside in hello WebContent folder:</span></span>

    META-INF
    WEB-INF
    index.jsp

1. <span data-ttu-id="4b997-276">Kattintson a jobb gombbal a hello WebContent mappa, és válassza ki **exportálása**.</span><span class="sxs-lookup"><span data-stu-id="4b997-276">Right-click hello WebContent folder and select **Export**.</span></span>
2. <span data-ttu-id="4b997-277">A hello **exportálása kiválasztása** párbeszédpanel, kattintson a **webes > WAR** fájlt, majd kattintson az **tovább**.</span><span class="sxs-lookup"><span data-stu-id="4b997-277">In hello **Export Select** dialog, click **Web > WAR** file, then click **Next**.</span></span>
3. <span data-ttu-id="4b997-278">A hello **WAR exportálása** párbeszédpanelen válassza ki a hello src directory hello aktuális projektben, és hello WAR-fájlt a hello végén hello nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4b997-278">In hello **WAR Export** dialog, select hello src directory in hello current project, and include hello name of hello WAR file at hello end.</span></span> <span data-ttu-id="4b997-279">Példa:</span><span class="sxs-lookup"><span data-stu-id="4b997-279">For example:</span></span>
   
    `<project-path>/JSPHello/src/JSPHello.war`

<span data-ttu-id="4b997-280">WAR fájlok telepítésével kapcsolatos további információkért lásd: [hozzáadása a Java-alkalmazás tooAzure App Service Web Apps](web-sites-java-add-app.md).</span><span class="sxs-lookup"><span data-stu-id="4b997-280">For more information on deploying WAR files, see [Add a Java application tooAzure App Service Web Apps](web-sites-java-add-app.md).</span></span>

### <a name="deploying-hello-hello-world-application-using-ftp"></a><span data-ttu-id="4b997-281">Hello Hello World alkalmazás használatával FTP telepítése</span><span class="sxs-lookup"><span data-stu-id="4b997-281">Deploying hello Hello World Application Using FTP</span></span>
<span data-ttu-id="4b997-282">Jelöljön ki egy külső FTP ügyfél toopublish hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4b997-282">Select a third-party FTP client toopublish hello application.</span></span> <span data-ttu-id="4b997-283">Ez az eljárás ismerteti a két lehetőség közül választhat: hello Kudu konzol épített Azure; és FileZilla, olyan népszerű eszköz egy kényelmes, grafikus felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="4b997-283">This procedure describes two options: hello Kudu console built into Azure; and FileZilla, a popular tool with a convenient, graphical UI.</span></span>

> <span data-ttu-id="4b997-284">**Megjegyzés:** hello Azure eszközkészlet az eclipse-ben támogatja a központi telepítés toostorage fiókokat és a felhőalapú szolgáltatások, de jelenleg nem támogatja a telepítési tooweb alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="4b997-284">**Note:** hello Azure Toolkit for Eclipse supports deployment toostorage accounts and cloud services, but does not currently support deployment tooweb apps.</span></span> <span data-ttu-id="4b997-285">Toostorage fiókok telepítheti, és a felhőalapú Azure telepítési projekt segítségével, a szolgáltatások [egy Hello World alkalmazás létrehozása az Azure az eclipse-ben](http://msdn.microsoft.com/library/azure/hh690944.aspx), de nem tooweb alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="4b997-285">You can deploy toostorage accounts and cloud services using an Azure Deployment Project as described in [Creating a Hello World Application for Azure in Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), but not tooweb apps.</span></span> <span data-ttu-id="4b997-286">Más módszerekkel például az FTP- vagy GitHub tootransfer fájlok tooyour webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4b997-286">Use other methods such as FTP or GitHub tootransfer files tooyour web app.</span></span>
> 
> <span data-ttu-id="4b997-287">**Megjegyzés:** FTP hello Windows parancssorból (hello parancssori FTP.EXE segédprogram, amely a Windows részét képező) használata nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="4b997-287">**Note:** We do not recommend using FTP from hello Windows command prompt (hello command-line FTP.EXE utility that ships with Windows).</span></span> <span data-ttu-id="4b997-288">FTP-ügyfelek, amelyek aktív FTP FTP.EXE, például gyakran sikertelen toowork tűzfalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="4b997-288">FTP clients that use active FTP, such as FTP.EXE, often fail toowork over firewalls.</span></span> <span data-ttu-id="4b997-289">Aktív FTP belső LAN-alapú címét adja meg, akkor toowhich egy FTP-kiszolgáló valószínűleg tooconnect sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="4b997-289">Active FTP specifies an internal LAN-based address, toowhich an FTP server will likely fail tooconnect.</span></span>
> 
> 

<span data-ttu-id="4b997-290">További információ a központi telepítés tooan App Service web app használatával FTP tekintse meg a következő témakörök hello:</span><span class="sxs-lookup"><span data-stu-id="4b997-290">For more information on deployment tooan App Service web app using FTP, see hello following topics:</span></span>

* [<span data-ttu-id="4b997-291">Az FTP-segédprogrammal történő telepítése</span><span class="sxs-lookup"><span data-stu-id="4b997-291">Deploy using an FTP utility</span></span>](web-sites-deploy.md)

#### <a name="set-up-deployment-credentials"></a><span data-ttu-id="4b997-292">Üzembe helyezési hitelesítő adatok beállítása</span><span class="sxs-lookup"><span data-stu-id="4b997-292">Set up deployment credentials</span></span>
<span data-ttu-id="4b997-293">Ellenőrizze, hogy futtatta hello **AzureWebDemo** alkalmazás toocreate egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4b997-293">Make sure you have run hello **AzureWebDemo** application toocreate a web app.</span></span> <span data-ttu-id="4b997-294">Akkor fogja átvinni fájlokat toothis helyét.</span><span class="sxs-lookup"><span data-stu-id="4b997-294">You will transfer files toothis location.</span></span>

1. <span data-ttu-id="4b997-295">Jelentkezzen be hello klasszikus portálra, és kattintson a **webalkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4b997-295">Log into hello classic portal and click **Web Apps**.</span></span> <span data-ttu-id="4b997-296">Győződjön meg arról, hogy **WebDemoWebApp** azokból a webalkalmazásokból hello listájában jelenik meg, és győződjön meg arról, hogy fut-e.</span><span class="sxs-lookup"><span data-stu-id="4b997-296">Make sure **WebDemoWebApp** appears in hello list of web apps, and make sure that it is running.</span></span> <span data-ttu-id="4b997-297">Kattintson a **WebDemoWebApp** tooopen a **irányítópult** lap.</span><span class="sxs-lookup"><span data-stu-id="4b997-297">Click **WebDemoWebApp** tooopen its **Dashboard** page.</span></span>
2. <span data-ttu-id="4b997-298">A hello **irányítópult** lap **gyors áttekintése**, kattintson a **az üzembe helyezési hitelesítő adatok beállítása** (már üzembe helyezési hitelesítő adatokat, ha ez olvassa be  **A központi telepítési hitelesítő adatok alaphelyzetbe állítása**).</span><span class="sxs-lookup"><span data-stu-id="4b997-298">On hello **Dashboard** page, under **Quick Glance**, click **Set up your deployment credentials** (if you already have deployment credentials, this reads **Reset your deployment credentials**).</span></span>
   
    <span data-ttu-id="4b997-299">Üzembe helyezési hitelesítő adatok társított Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="4b997-299">Deployment credentials are associated with a Microsoft account.</span></span> <span data-ttu-id="4b997-300">Toospecify egy felhasználónév és jelszó használható Git és az FTP használatával toodeploy van szüksége.</span><span class="sxs-lookup"><span data-stu-id="4b997-300">You need toospecify a username and password that you can use toodeploy using Git and FTP.</span></span> <span data-ttu-id="4b997-301">A Microsoft-fiókjához társított összes Azure-előfizetések is használhatja ezeket a hitelesítő adatok toodeploy tooany webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4b997-301">You can use these credentials toodeploy tooany web app in all Azure subscriptions associated with your Microsoft account.</span></span> <span data-ttu-id="4b997-302">Adja meg a Git és az FTP telepítési hitelesítő adatok a hello párbeszédpanel, és a rekord hello felhasználónév és a jelszó későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="4b997-302">Provide Git and FTP deployment credentials in hello dialog, and record hello username and password for future use.</span></span>

#### <a name="get-ftp-connection-information"></a><span data-ttu-id="4b997-303">FTP-kiszolgáló kapcsolati adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="4b997-303">Get FTP connection information</span></span>
<span data-ttu-id="4b997-304">toouse FTP toodeploy alkalmazás fájlok az újonnan létrehozott toohello web app alkalmazásban kell tooobtain kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="4b997-304">toouse FTP toodeploy application files toohello newly created web app, you need tooobtain connection information.</span></span> <span data-ttu-id="4b997-305">Két módon tooobtain kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="4b997-305">There are two ways tooobtain connection information.</span></span> <span data-ttu-id="4b997-306">Egyirányú van toovisit hello webes alkalmazás **irányítópult** lapon; hello más módon toodownload hello webes alkalmazás közzétételi profil.</span><span class="sxs-lookup"><span data-stu-id="4b997-306">One way is toovisit hello web app's **Dashboard** page; hello other way is toodownload hello web app's publish profile.</span></span> <span data-ttu-id="4b997-307">hello közzétételi profil egy XML-fájl, amely például FTP állomás nevét és a bejelentkezési hitelesítő adatokat biztosít a webalkalmazások Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="4b997-307">hello publish profile is an XML file that provides information such as FTP host name and logon credentials for your web apps in Azure App Service.</span></span> <span data-ttu-id="4b997-308">A felhasználónév és jelszó toodeploy tooany webalkalmazás hello Azure-fiókra, nem csak az egyik társított előfizetéseket használhatja.</span><span class="sxs-lookup"><span data-stu-id="4b997-308">You can use this username and password toodeploy tooany web app in all subscriptions associated with hello Azure account, not only this one.</span></span>

<span data-ttu-id="4b997-309">tooobtain FTP csatlakozási adataival hello webalkalmazása panelén a hello [Azure Portal][Azure Portal]:</span><span class="sxs-lookup"><span data-stu-id="4b997-309">tooobtain FTP connection information from hello web app's blade in hello [Azure Portal][Azure Portal]:</span></span>

1. <span data-ttu-id="4b997-310">A **Essentials**, található, és másolja a hello **FTP-állomásnév**.</span><span class="sxs-lookup"><span data-stu-id="4b997-310">Under **Essentials**, find and copy hello **FTP hostname**.</span></span> <span data-ttu-id="4b997-311">Ez az hasonló URI túl`ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="4b997-311">This is a URI similar too`ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.</span></span>
2. <span data-ttu-id="4b997-312">A **Essentials**, található, és másolja **FTP vagy üzembe helyező felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="4b997-312">Under **Essentials**, find and copy **FTP/Deployment username**.</span></span> <span data-ttu-id="4b997-313">Ez lesz a hello űrlap *webappname\deployment-username*, például `WebDemoWebApp\deployer77`.</span><span class="sxs-lookup"><span data-stu-id="4b997-313">This will have hello form *webappname\deployment-username*; for example `WebDemoWebApp\deployer77`.</span></span>

<span data-ttu-id="4b997-314">a közzétételi profil tooobtain FTP csatlakozási adataival hello:</span><span class="sxs-lookup"><span data-stu-id="4b997-314">tooobtain FTP connection information from hello publish profile:</span></span>

1. <span data-ttu-id="4b997-315">Hello webes alkalmazás paneljén kattintson **Get közzétételi profil**.</span><span class="sxs-lookup"><span data-stu-id="4b997-315">In hello web app's blade, click **Get publish profile**.</span></span> <span data-ttu-id="4b997-316">A .publishsettings fájlt tooyour helyi meghajtót letöltése.</span><span class="sxs-lookup"><span data-stu-id="4b997-316">This will download a .publishsettings file tooyour local drive.</span></span>
2. <span data-ttu-id="4b997-317">Nyissa meg a hello .publishsettings fájlt egy XML-szerkesztőben vagy szövegszerkesztőben, és megkeresi hello `<publishProfile>` elemet tartalmazó `publishMethod="FTP"`.</span><span class="sxs-lookup"><span data-stu-id="4b997-317">Open hello .publishsettings file in an XML editor or text editor and find hello `<publishProfile>` element containing `publishMethod="FTP"`.</span></span> <span data-ttu-id="4b997-318">Az alábbihoz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="4b997-318">It should look like hello following:</span></span>
   
        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>
3. <span data-ttu-id="4b997-319">Vegye figyelembe, hogy hello webalkalmazás `publishProfile` beállítások leképezése toohello FileZilla kezelő beállításokat az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="4b997-319">Note that hello web app's `publishProfile` settings map toohello FileZilla Site Manager settings as follows:</span></span>

* <span data-ttu-id="4b997-320">`publishUrl`ugyanaz, mint hello **FTP-állomás neve**, hello beállítása érték **állomás**.</span><span class="sxs-lookup"><span data-stu-id="4b997-320">`publishUrl` is hello same as **FTP host name**, hello value you set in **Host**.</span></span>
* <span data-ttu-id="4b997-321">`publishMethod="FTP"`azt jelenti, hogy megadta **protokoll** túl**FTP - File Transfer Protocol**, és **titkosítási** túl**egyszerű FTP használata**.</span><span class="sxs-lookup"><span data-stu-id="4b997-321">`publishMethod="FTP"` means that you set **Protocol** too**FTP - File Transfer Protocol**, and **Encryption** too**Use plain FTP**.</span></span>
* <span data-ttu-id="4b997-322">`userName`és `userPWD` értékei hello kulcsok tényleges felhasználónevet és jelszót meg, ha alaphelyzetbe állít egy hello üzembe helyezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="4b997-322">`userName` and `userPWD` are keys for hello actual username and password values you specified when you reset hello deployment credentials.</span></span> <span data-ttu-id="4b997-323">`userName`ugyanaz, mint hello **telepítési / FTP-felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4b997-323">`userName` is hello same as **Deployment / FTP user**.</span></span> <span data-ttu-id="4b997-324">Túl leképezik**felhasználói** és **jelszó** a FileZilla.</span><span class="sxs-lookup"><span data-stu-id="4b997-324">They map too**User** and **Password** in FileZilla.</span></span>
* <span data-ttu-id="4b997-325">`ftpPassiveMode="True"`azt jelenti, hogy a hello FTP-hely használja a passzív FTP-átvitel; Válassza ki **passzív** a hello **átvitel beállításainak** fülre.</span><span class="sxs-lookup"><span data-stu-id="4b997-325">`ftpPassiveMode="True"` means that hello FTP site uses passive FTP transfer; select **Passive** on hello **Transfer Settings** tab.</span></span>

#### <a name="configure-hello-web-app-toohost-a-java-application"></a><span data-ttu-id="4b997-326">Hello webalkalmazás toohost Java-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4b997-326">Configure hello Web App toohost a Java application</span></span>
<span data-ttu-id="4b997-327">Hello alkalmazás közzététele előtt kell toochange néhány konfigurációs beállítás, hogy hello webalkalmazás üzemeltethet Java-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="4b997-327">Before you publish hello application, you need toochange a few configuration settings so that hello web app can host a Java application.</span></span>

1. <span data-ttu-id="4b997-328">Hello klasszikus portálon lépjen toohello webes alkalmazás **irányítópult** lapot, és kattintson **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="4b997-328">In hello classic portal, go toohello web app's **Dashboard** page and click **Configure**.</span></span> <span data-ttu-id="4b997-329">A hello **konfigurálása** adja meg azokat a beállításokat a következő hello.</span><span class="sxs-lookup"><span data-stu-id="4b997-329">On hello **Configure** page, specify hello following settings.</span></span>
2. <span data-ttu-id="4b997-330">A **Java-verziót** hello alapértelmezett érték a **ki**; válassza hello Java-verziót a alkalmazás célkitűzések, például a 1.7.0_51.</span><span class="sxs-lookup"><span data-stu-id="4b997-330">In **Java version** hello default is **Off**; select hello Java version your application targets; for example 1.7.0_51.</span></span> <span data-ttu-id="4b997-331">Ezt követően ellenőrizze azt is, amely **webes tároló** Tomcat kiszolgálót tooa verziója van beállítva.</span><span class="sxs-lookup"><span data-stu-id="4b997-331">After you do this, also make sure that **Web container** is set tooa version of Tomcat Server.</span></span>
3. <span data-ttu-id="4b997-332">A **alapértelmezett dokumentumok**adja hozzá az index.jsp és toohello hello lista elejére helyezze.</span><span class="sxs-lookup"><span data-stu-id="4b997-332">In **Default Documents**, add index.jsp and move it up toohello top of hello list.</span></span> <span data-ttu-id="4b997-333">(hello alapértelmezett webalkalmazások fájlja hostingstart.html.)</span><span class="sxs-lookup"><span data-stu-id="4b997-333">(hello default file for web apps is hostingstart.html.)</span></span>
4. <span data-ttu-id="4b997-334">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="4b997-334">Click **Save**.</span></span>

#### <a name="publish-your-application-using-kudu"></a><span data-ttu-id="4b997-335">A Kudu használó alkalmazások közzététele</span><span class="sxs-lookup"><span data-stu-id="4b997-335">Publish your application using Kudu</span></span>
<span data-ttu-id="4b997-336">Egyirányú toopublish hello alkalmazás Kudu hibakereső konzol az Azure beépített toouse hello.</span><span class="sxs-lookup"><span data-stu-id="4b997-336">One way toopublish hello application is toouse hello Kudu debug console built into Azure.</span></span> <span data-ttu-id="4b997-337">A kudu ismert toobe stabil és konzisztens legyen az App Service Web Apps és Tomcat kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="4b997-337">Kudu is known toobe stable and consistent with App Service Web Apps and Tomcat Server.</span></span> <span data-ttu-id="4b997-338">Hello konzol hello webalkalmazás hozzáférni a keresse meg a következő képernyő hello tooa URL-címe:</span><span class="sxs-lookup"><span data-stu-id="4b997-338">You access hello console for hello web app by browsing tooa URL of hello following form:</span></span>

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. <span data-ttu-id="4b997-339">Az eljárás használatához hello Kudu konzol itt található a következő URL-címet; hello: Keresse meg a toothis helye:</span><span class="sxs-lookup"><span data-stu-id="4b997-339">For this procedure, hello Kudu console is located at hello following URL; browse toothis location:</span></span>
   
    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`
2. <span data-ttu-id="4b997-340">Hello felső menüben válassza ki a **konzol Debug > CMD**.</span><span class="sxs-lookup"><span data-stu-id="4b997-340">From hello top menu, select **Debug Console > CMD**.</span></span>
3. <span data-ttu-id="4b997-341">Hello konzol parancssorban lépjen a túl`/site/wwwroot` (vagy kattintson a `site`, majd `wwwroot` hello könyvtár nézetben hello oldal hello tetején):</span><span class="sxs-lookup"><span data-stu-id="4b997-341">In hello console command line, navigate too`/site/wwwroot` (or click `site`, then `wwwroot` in hello directory view at hello top of hello page):</span></span>
   
    `cd /site/wwwroot`
4. <span data-ttu-id="4b997-342">Miután megadta **Java-verziót**, Tomcat kiszolgálót kell létrehoznia a webapps könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="4b997-342">After you specify **Java version**, Tomcat server should create a webapps directory.</span></span> <span data-ttu-id="4b997-343">Hello konzol parancssorban lépjen a toohello webapps könyvtárba:</span><span class="sxs-lookup"><span data-stu-id="4b997-343">In hello console command line, navigate toohello webapps directory:</span></span>
   
    `mkdir webapps`
   
    `cd webapps`
5. <span data-ttu-id="4b997-344">Húzza a JSPHello.war `<project-path>/JSPHello/src/` , és helyezze a hello Kudu könyvtár nézetben a `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="4b997-344">Drag JSPHello.war from `<project-path>/JSPHello/src/` and drop it into hello Kudu directory view under `/site/wwwroot/webapps`.</span></span> <span data-ttu-id="4b997-345">Nem húzza toohello "Húzzon ide tooupload és zip" területen, mert a Tomcat fog csomagolja ki.</span><span class="sxs-lookup"><span data-stu-id="4b997-345">Do not drag it toohello "Drag here tooupload and zip" area, because Tomcat will unzip it.</span></span>
   
   ![][8]

<span data-ttu-id="4b997-346">Első JSPHello.war: hello directory területen megjelenik önmagában:</span><span class="sxs-lookup"><span data-stu-id="4b997-346">At first JSPHello.war appears in hello directory area by itself:</span></span>

  ![][9]

<span data-ttu-id="4b997-347">Rövid időn belül (valószínűleg 5 percnél kevesebb) Tomcat kiszolgálót fog csomagolja ki hello WAR-fájlt egy kicsomagolt JSPHello könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="4b997-347">In a short time (probably less than 5 minutes) Tomcat Server will unzip hello WAR file into an unpacked JSPHello directory.</span></span> <span data-ttu-id="4b997-348">Kattintson a hello ROOT directory toosee, hogy index.jsp unzipped és másolásához.</span><span class="sxs-lookup"><span data-stu-id="4b997-348">Click hello ROOT directory toosee whether index.jsp has been unzipped and copied there.</span></span> <span data-ttu-id="4b997-349">Ha igen, lépjen vissza toohello webapps directory toosee e hello kicsomagolása JSPHello könyvtár létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4b997-349">If so, navigate back toohello webapps directory toosee whether hello unpacked JSPHello directory has been created.</span></span> <span data-ttu-id="4b997-350">Ha nem látja ezeket az elemeket, várja meg, és ismételje meg.</span><span class="sxs-lookup"><span data-stu-id="4b997-350">If you do not see these items, wait and repeat.</span></span>

  ![][10]

#### <a name="publish-your-application-using-filezilla-optional"></a><span data-ttu-id="4b997-351">A FileZilla (nem kötelező) használó alkalmazások közzététele</span><span class="sxs-lookup"><span data-stu-id="4b997-351">Publish your application using FileZilla (optional)</span></span>
<span data-ttu-id="4b997-352">Egy másik toopublish hello alkalmazásnak eszköze FileZilla, a népszerű külső FTP-ügyfél kényelmesen, grafikus felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="4b997-352">Another tool you can use toopublish hello application is FileZilla, a popular third-party FTP client with a convenient, graphical UI.</span></span> <span data-ttu-id="4b997-353">Töltse le, és telepítse a FileZilla [http://filezilla-project.org/](http://filezilla-project.org/) Ha még nem rendelkezik azt.</span><span class="sxs-lookup"><span data-stu-id="4b997-353">You can download and install FileZilla from [http://filezilla-project.org/](http://filezilla-project.org/) if you do not already have it.</span></span> <span data-ttu-id="4b997-354">Hello ügyfél használatával kapcsolatos további információkért lásd: hello [FileZilla dokumentáció](https://wiki.filezilla-project.org/Documentation) és ezt a blogbejegyzést [FTP-ügyfelek - rész 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).</span><span class="sxs-lookup"><span data-stu-id="4b997-354">For more information on using hello client, see hello [FileZilla documentation](https://wiki.filezilla-project.org/Documentation) and this blog entry on [FTP Clients - Part 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).</span></span>

1. <span data-ttu-id="4b997-355">Kattintson a FileZilla, **fájl > kezelő**.</span><span class="sxs-lookup"><span data-stu-id="4b997-355">In FileZilla, click **File > Site Manager**.</span></span>
2. <span data-ttu-id="4b997-356">A hello **kezelő** párbeszédpanel, kattintson a **új hely**.</span><span class="sxs-lookup"><span data-stu-id="4b997-356">In hello **Site Manager** dialog, click **New Site**.</span></span> <span data-ttu-id="4b997-357">Megjelenik egy új, üres FTP-hely **válasszon bejegyzés** tooprovide nevét kéri.</span><span class="sxs-lookup"><span data-stu-id="4b997-357">A new blank FTP site will appear in **Select Entry** prompting you tooprovide a name.</span></span> <span data-ttu-id="4b997-358">Az alábbi eljárással nevezze el `AzureWebDemo-FTP`.</span><span class="sxs-lookup"><span data-stu-id="4b997-358">For this procedure, name it `AzureWebDemo-FTP`.</span></span>
   
    <span data-ttu-id="4b997-359">A hello **általános** lapra, adja meg a következő beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="4b997-359">On hello **General** tab, specify hello following settings:</span></span>
   
   * <span data-ttu-id="4b997-360">**Állomás:** Enter hello **FTP-állomás neve** hello irányítópultról másolt.</span><span class="sxs-lookup"><span data-stu-id="4b997-360">**Host:** Enter hello **FTP Host Name** that you copied from hello dashboard.</span></span>
   * <span data-ttu-id="4b997-361">**Port:** (hagyja üresen a mezőt, a passzív átvitel és hello server hello port toouse határozza meg.)</span><span class="sxs-lookup"><span data-stu-id="4b997-361">**Port:** (Leave this blank, as this is a passive transfer and hello server will determine hello port toouse.)</span></span>
   * <span data-ttu-id="4b997-362">**Protokoll:** FTP fájlátviteli protokoll</span><span class="sxs-lookup"><span data-stu-id="4b997-362">**Protocol:** FTP File Transfer Protocol</span></span>
   * <span data-ttu-id="4b997-363">**Titkosítás:** egyszerű FTP használata</span><span class="sxs-lookup"><span data-stu-id="4b997-363">**Encryption:** Use plain FTP</span></span>
   * <span data-ttu-id="4b997-364">**Bejelentkezési típusa:** normál</span><span class="sxs-lookup"><span data-stu-id="4b997-364">**Logon Type:** Normal</span></span>
   * <span data-ttu-id="4b997-365">**Felhasználó:** Enter hello telepítési / FTP-felhasználó hello irányítópultról másolt.</span><span class="sxs-lookup"><span data-stu-id="4b997-365">**User:** Enter hello Deployment / FTP user that you copied from hello dashboard.</span></span> <span data-ttu-id="4b997-366">Ez a hello teljes FTP felhasználónév, amelynek hello űrlap *webappname\username*.</span><span class="sxs-lookup"><span data-stu-id="4b997-366">This is hello full FTP username, which has hello form *webappname\username*.</span></span>
   * <span data-ttu-id="4b997-367">**Jelszó:** hello jelszó megadni, hogy a megadott hello üzembe helyezési hitelesítő adatok megadása után.</span><span class="sxs-lookup"><span data-stu-id="4b997-367">**Password:** Enter hello password that you specified when you set hello deployment credentials.</span></span>
     
     <span data-ttu-id="4b997-368">A hello **átvitel beállításainak** lapon jelölje be **passzív**.</span><span class="sxs-lookup"><span data-stu-id="4b997-368">On hello **Transfer Settings** tab, select **Passive**.</span></span>
3. <span data-ttu-id="4b997-369">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4b997-369">Click **Connect**.</span></span> <span data-ttu-id="4b997-370">Ha sikeres, FileZilla tartozó konzol megjeleníti a `Status: Connected` üzenet és a probléma egy `LIST` toolist hello könyvtár tartalma parancsot.</span><span class="sxs-lookup"><span data-stu-id="4b997-370">If successful, FileZilla's console will display a `Status: Connected` message and issue a `LIST` command toolist hello directory contents.</span></span>
4. <span data-ttu-id="4b997-371">A hello **helyi** hely panelen, jelölje be hello forráskönyvtár mely hello JSPHello.war fájlban található; hello elérési hasonló toohello következő lesz:</span><span class="sxs-lookup"><span data-stu-id="4b997-371">In hello **Local** site panel, select hello source directory in which hello JSPHello.war file resides; hello path will be similar toohello following:</span></span>
   
    `<project-path>/JSPHello/src/`
5. <span data-ttu-id="4b997-372">A hello **távoli** hely panelen, jelölje be hello célmappát.</span><span class="sxs-lookup"><span data-stu-id="4b997-372">In hello **Remote** site panel, select hello destination folder.</span></span> <span data-ttu-id="4b997-373">Telepíti a WAR-fájl toohello hello `webapps` hello webes alkalmazás legfelső szintű könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="4b997-373">You will deploy hello WAR file toohello `webapps` directory under hello web app's root.</span></span> <span data-ttu-id="4b997-374">Keresse meg a túl`/site/wwwroot`, kattintson a jobb gombbal a `wwwroot`, és válassza ki **könyvtár létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="4b997-374">Navigate too`/site/wwwroot`, right-click on `wwwroot`, and select **Create directory**.</span></span> <span data-ttu-id="4b997-375">Egyező hello könyvtár `webapps` , és írja be a könyvtárhoz.</span><span class="sxs-lookup"><span data-stu-id="4b997-375">Name hello directory `webapps` and enter that directory.</span></span>
6. <span data-ttu-id="4b997-376">Átviteli túl JSPHello.war`/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="4b997-376">Transfer JSPHello.war too`/site/wwwroot/webapps`.</span></span> <span data-ttu-id="4b997-377">Válassza ki a JSPHello.war hello **helyi** lista fájlt, kattintson a jobb gombbal a, és válassza ki **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="4b997-377">Select JSPHello.war in hello **Local** file list, right-click on it and select **Upload**.</span></span> <span data-ttu-id="4b997-378">Akkor jelenik meg kell megjelennie `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="4b997-378">You should see it appear in `/site/wwwroot/webapps`.</span></span>
7. <span data-ttu-id="4b997-379">Miután másolta JSPHello.war toohello webapps könyvtárba, Tomcat kiszolgálót automatikusan csomagolja ki (csomagolja ki) hello hello WAR-fájlt a fájlok.</span><span class="sxs-lookup"><span data-stu-id="4b997-379">After you copy JSPHello.war toohello webapps directory, Tomcat Server will automatically unpack (unzip) hello files in hello WAR file.</span></span> <span data-ttu-id="4b997-380">Bár Tomcat kiszolgálót megkezdése kicsomagolása szinte azonnal is telhet, mire hosszú idő (valószínűleg óra) hello fájlok tooappear hello FTP-ügyfél számára.</span><span class="sxs-lookup"><span data-stu-id="4b997-380">Although Tomcat Server begins unpacking almost immediately, it might take a long time (possibly hours) for hello files tooappear in hello FTP client.</span></span>

#### <a name="run-hello-hello-world-application-on-hello-web-app"></a><span data-ttu-id="4b997-381">A webalkalmazás hello hello Hello World az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="4b997-381">Run hello Hello World application on hello Web App</span></span>
1. <span data-ttu-id="4b997-382">Miután hello WAR-fájl feltöltése, és ellenőrizte a Tomcat kiszolgálón egy kicsomagolt létrehozott `JSPHello` könyvtár, keresse meg a túl`http://webdemowebapp.azurewebsites.net/JSPHello` toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4b997-382">After you have uploaded hello WAR file and verified that Tomcat server has created an unpacked `JSPHello` directory, browse too`http://webdemowebapp.azurewebsites.net/JSPHello` toorun hello application.</span></span>
   
   > <span data-ttu-id="4b997-383">**Megjegyzés:** választva **Tallózás** hello klasszikus portálon, kaphat hello alapértelmezett weblap, mely szerint a "a Java-alapú webalkalmazás létrehozása sikeresen befejeződött."</span><span class="sxs-lookup"><span data-stu-id="4b997-383">**Note:** If you click **Browse** from hello classic portal, you might get hello default webpage, saying "This Java based web application has been successfully created."</span></span> <span data-ttu-id="4b997-384">Lehetséges, hogy toorefresh hello weblap rendelés tooview hello alkalmazás kimenet hello alapértelmezett weblap helyett.</span><span class="sxs-lookup"><span data-stu-id="4b997-384">You might have toorefresh hello webpage in order tooview hello application output instead of hello default webpage.</span></span>
   > 
   > 
2. <span data-ttu-id="4b997-385">Hello alkalmazás futásakor kell: weblapot a következő kimeneti hello</span><span class="sxs-lookup"><span data-stu-id="4b997-385">When hello application runs, you should see a web page with hello following output:</span></span>
   
    `Hello World, hello time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="clean-up-azure-resources"></a><span data-ttu-id="4b997-386">Azure-erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="4b997-386">Clean up Azure resources</span></span>
<span data-ttu-id="4b997-387">Ez az eljárás egy App Service webalkalmazásba hoz.</span><span class="sxs-lookup"><span data-stu-id="4b997-387">This procedure creates an App Service web app.</span></span> <span data-ttu-id="4b997-388">Számlázott hello erőforrás mindaddig, amíg az létezik.</span><span class="sxs-lookup"><span data-stu-id="4b997-388">You will be billed for hello resource as long as it exists.</span></span> <span data-ttu-id="4b997-389">Kivéve, ha azt tervezi, hogy a tesztelési, illetve a fejlesztési hello web app használatával toocontinue, érdemes lehet leállítása vagy törlése.</span><span class="sxs-lookup"><span data-stu-id="4b997-389">Unless you plan toocontinue using hello web app for testing or development, you should consider stopping or deleting it.</span></span> <span data-ttu-id="4b997-390">A webes alkalmazás, amely le lett állítva továbbra is fel Önnek egy kis kell fizetni, de bármikor újraindíthatja azt.</span><span class="sxs-lookup"><span data-stu-id="4b997-390">A web app that has been stopped will still incur a small charge, but you can restart it at any time.</span></span> <span data-ttu-id="4b997-391">A webes alkalmazás törlésével lévő tooit feltöltött adat elvész.</span><span class="sxs-lookup"><span data-stu-id="4b997-391">Deleting a web app erases all data you have uploaded tooit.</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

[1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
[2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
[3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
[4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
[5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
[6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
[7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
[8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
[9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
[10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png


[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web Platform Installer]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure classic portal]: https://manage.windowsazure.com
[What is an Azure AD directory]: http://technet.microsoft.com/library/jj573650.aspx
[Create and Upload a Management Certificate for Azure]: ../cloud-services/cloud-services-certs-create.md
[Key and Certificate Management Tool (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure Portal]: https://portal.azure.com
