---
title: "Webalkalmazás létrehozása az Azure App Service szolgáltatásban az Azure SDK for Java használatával"
description: "Útmutató az Azure App Service programozott módon, az Azure SDK Java-webalkalmazás létrehozása."
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
ms.openlocfilehash: 08bb53de8cf437a5a2b1c3b38bce9f81b8349493
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a><span data-ttu-id="22e6d-103">Webalkalmazás létrehozása az Azure App Service szolgáltatásban az Azure SDK for Java használatával</span><span class="sxs-lookup"><span data-stu-id="22e6d-103">Create a Web App in Azure App Service using the Azure SDK for Java</span></span>
<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a><span data-ttu-id="22e6d-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="22e6d-104">Overview</span></span>
<span data-ttu-id="22e6d-105">Ez a forgatókönyv bemutatja, hogyan hozzon létre egy Java-alkalmazást hoz létre egy webalkalmazást az Azure SDK [Azure App Service][Azure App Service], majd az alkalmazást telepíti.</span><span class="sxs-lookup"><span data-stu-id="22e6d-105">This walkthrough shows you how to create an Azure SDK for Java application that creates a Web App in [Azure App Service][Azure App Service], then deploy an application to it.</span></span> <span data-ttu-id="22e6d-106">Két részből áll:</span><span class="sxs-lookup"><span data-stu-id="22e6d-106">It consists of two parts:</span></span>

* <span data-ttu-id="22e6d-107">1. rész bemutatja, hogyan hozhat létre egy Java-alkalmazást, amely létrehozza a webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="22e6d-107">Part 1 demonstrates how to build a Java application that creates a web app.</span></span>
* <span data-ttu-id="22e6d-108">2. rész bemutatja, hogyan hozzon létre egy egyszerű "Hello, World" JSP alkalmazást, majd használja az FTP-ügyfél központi telepítése a kódot az App Service.</span><span class="sxs-lookup"><span data-stu-id="22e6d-108">Part 2 demonstrates how to create a simple JSP "Hello World" application, then use an FTP client to deploy code to App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22e6d-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="22e6d-109">Prerequisites</span></span>
### <a name="software-installations"></a><span data-ttu-id="22e6d-110">Szoftvertelepítés</span><span class="sxs-lookup"><span data-stu-id="22e6d-110">Software Installations</span></span>
<span data-ttu-id="22e6d-111">Ebben a cikkben a AzureWebDemo alkalmazáskód Azure Java SDK 0.7.0, amelyek segítségével lehet telepíteni használatával íródott a [Webplatform-telepítő] [ Web Platform Installer] (WebPI).</span><span class="sxs-lookup"><span data-stu-id="22e6d-111">The AzureWebDemo application code in this article was written using Azure Java SDK 0.7.0, which you can install using the [Web Platform Installer][Web Platform Installer] (WebPI).</span></span> <span data-ttu-id="22e6d-112">Emellett ügyeljen arra, hogy a legújabb verzióját használja a [Eclipse Azure eszköztára][Azure Toolkit for Eclipse].</span><span class="sxs-lookup"><span data-stu-id="22e6d-112">In addition, make sure to use the latest version of the [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="22e6d-113">Az SDK telepítése után frissítse a függőségek az Eclipse-projekt futtatásával **frissítése** a **Maven Tárházak**, majd újra vegye fel a minden csomag legújabb verzióját a  **Függőségek** ablak.</span><span class="sxs-lookup"><span data-stu-id="22e6d-113">After you install the SDK, update the dependencies in your Eclipse project by running **Update Index** in **Maven Repositories**, then re-add the latest version of each package in the **Dependencies** window.</span></span> <span data-ttu-id="22e6d-114">Az eclipse-ben telepített szoftvert verziója kattintva ellenőrizheti **Súgó > telepítésének részletei**; kell legalább a következő verziók:</span><span class="sxs-lookup"><span data-stu-id="22e6d-114">You can verify the version of your installed software in Eclipse by clicking **Help > Installation Details**; you should have at least the following versions:</span></span>

* <span data-ttu-id="22e6d-115">A Microsoft Azure-tárak Java 0.7.0.20150309 csomag</span><span class="sxs-lookup"><span data-stu-id="22e6d-115">Package for Microsoft Azure Libraries for Java 0.7.0.20150309</span></span>
* <span data-ttu-id="22e6d-116">Java EE-fejlesztőknek 4.4.2.20150219 készült eclipse IDE</span><span class="sxs-lookup"><span data-stu-id="22e6d-116">Eclipse IDE for Java EE Developers 4.4.2.20150219</span></span>

### <a name="create-and-configure-cloud-resources-in-azure"></a><span data-ttu-id="22e6d-117">Hozza létre és konfigurálja a felhőben található erőforrásokat az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="22e6d-117">Create and Configure Cloud Resources in Azure</span></span>
<span data-ttu-id="22e6d-118">Ez az eljárás megkezdése előtt kell egy aktív Azure-előfizetéssel rendelkezik, és állítsa be az alapértelmezett Active Directory (az AD) az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="22e6d-118">Before you begin this procedure, you need to have an active Azure subscription and set up a default Active Directory (AD) on Azure.</span></span>

### <a name="create-an-active-directory-ad-in-azure"></a><span data-ttu-id="22e6d-119">Hozzon létre egy Active Directory (AD) az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="22e6d-119">Create an Active Directory (AD) in Azure</span></span>
<span data-ttu-id="22e6d-120">Ha még nem rendelkezik az Active Directory (AD) az Azure-előfizetése, jelentkezzen be a [a klasszikus Azure portálon] [ Azure classic portal] Microsoft-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="22e6d-120">If you do not already have an Active Directory (AD) on your Azure subscription, log into the [Azure classic portal][Azure classic portal] with your Microsoft account.</span></span> <span data-ttu-id="22e6d-121">Ha több előfizetése van, kattintson a **előfizetések** és jelöljön ki az alapértelmezett könyvtárat a projekthez használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="22e6d-121">If you have multiple subscriptions, click **Subscriptions** and select the default directory for the subscription you want to use for this project.</span></span> <span data-ttu-id="22e6d-122">Kattintson a **alkalmaz** , hogy előfizetési nézetet.</span><span class="sxs-lookup"><span data-stu-id="22e6d-122">Then click **Apply** to switch to that subscription view.</span></span>

1. <span data-ttu-id="22e6d-123">Válassza ki **Active Directory** a bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="22e6d-123">Select **Active Directory** from the menu at left.</span></span> <span data-ttu-id="22e6d-124">**Új kattintson > Directory > egyéni létrehozás**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-124">**Click New > Directory > Custom Create**.</span></span>
2. <span data-ttu-id="22e6d-125">A **könyvtár hozzáadása**, jelölje be **új könyvtár létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-125">In **Add Directory**, select **Create New Directory**.</span></span>
3. <span data-ttu-id="22e6d-126">A **neve**, adja meg a könyvtár nevét.</span><span class="sxs-lookup"><span data-stu-id="22e6d-126">In **Name**, enter a directory name.</span></span>
4. <span data-ttu-id="22e6d-127">A **tartomány**, írja be a tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="22e6d-127">In **Domain**, enter a domain name.</span></span> <span data-ttu-id="22e6d-128">Ez az alapszintű tartománynevet, hogy a címtárban; alapértelmezés szerint az űrlap rendelkezik `<domain_name>.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-128">This is a basic domain name that is included by default with your directory; it has the form `<domain_name>.onmicrosoft.com`.</span></span> <span data-ttu-id="22e6d-129">A könyvtárnév vagy egy másik, a saját tartománynév alapján nevére.</span><span class="sxs-lookup"><span data-stu-id="22e6d-129">You can name it based on the directory name or another domain name that you own.</span></span> <span data-ttu-id="22e6d-130">Később a szervezet már használja egy másik tartománynevet is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="22e6d-130">Later, you can add another domain name that your organization already uses.</span></span>
5. <span data-ttu-id="22e6d-131">A **ország vagy régió**, válassza ki a területi beállítás.</span><span class="sxs-lookup"><span data-stu-id="22e6d-131">In **Country or region**, select your locale.</span></span>

<span data-ttu-id="22e6d-132">AD a további információkért lásd: [Mi az Azure AD-címtár][What is an Azure AD directory]?</span><span class="sxs-lookup"><span data-stu-id="22e6d-132">For more information on AD, see [What is an Azure AD directory][What is an Azure AD directory]?</span></span>

### <a name="create-a-management-certificate-for-azure"></a><span data-ttu-id="22e6d-133">Az Azure felügyeleti tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="22e6d-133">Create a Management Certificate for Azure</span></span>
<span data-ttu-id="22e6d-134">Az Azure SDK for Java felügyeleti tanúsítványokat használ az Azure-előfizetések való hitelesítéshez szükséges.</span><span class="sxs-lookup"><span data-stu-id="22e6d-134">The Azure SDK for Java uses management certificates to authenticate with Azure subscriptions.</span></span> <span data-ttu-id="22e6d-135">Ezek a X.509 v3 alapján létrehozott tanúsítványok használatával egy ügyfélalkalmazást, amely a szolgáltatás felügyeleti API használatával előfizetési erőforrások kezeléséhez az előfizetés tulajdonosa nevében végezze a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="22e6d-135">These are X.509 v3 certificates you use to authenticate a client application that uses the Service Management API to act on behalf of the subscription owner to manage subscription resources.</span></span>

<span data-ttu-id="22e6d-136">A kódot az alábbi eljárással egy önaláírt tanúsítványt használ az Azure-ral hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="22e6d-136">The code in this procedure uses a self-signed certificate to authenticate with Azure.</span></span> <span data-ttu-id="22e6d-137">Ezzel az eljárással kell hozzon létre egy tanúsítványt, és töltse fel azt a [a klasszikus Azure portálon] [ Azure classic portal] előre.</span><span class="sxs-lookup"><span data-stu-id="22e6d-137">For this procedure, you need to create a certificate and upload it to the [Azure classic portal][Azure classic portal] beforehand.</span></span> <span data-ttu-id="22e6d-138">Ez a következő lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="22e6d-138">This involves the following steps:</span></span>

* <span data-ttu-id="22e6d-139">Az ügyféltanúsítvány képviselő PFX-fájl létrehozása, és mentse helyileg.</span><span class="sxs-lookup"><span data-stu-id="22e6d-139">Generate a PFX file representing your client certificate and save it locally.</span></span>
* <span data-ttu-id="22e6d-140">Felügyeleti tanúsítvány (CER-fájljával) a PFX-fájlból jön létre.</span><span class="sxs-lookup"><span data-stu-id="22e6d-140">Generate a management certificate (CER file) from the PFX file.</span></span>
* <span data-ttu-id="22e6d-141">Töltse fel az Azure-előfizetéshez a CER-fájljával.</span><span class="sxs-lookup"><span data-stu-id="22e6d-141">Upload the CER file to your Azure subscription.</span></span>
* <span data-ttu-id="22e6d-142">A PFX-fájl átalakítása JKS, mert a Java, hogy a formátum segítségével hitelesíti a tanúsítványok használatával.</span><span class="sxs-lookup"><span data-stu-id="22e6d-142">Convert the PFX file into JKS, because Java uses that format to authenticate using certificates.</span></span>
* <span data-ttu-id="22e6d-143">Az alkalmazás hitelesítési kód, amely hivatkozik a helyi JKS fájl írása.</span><span class="sxs-lookup"><span data-stu-id="22e6d-143">Write the application's authentication code, which refers to the local JKS file.</span></span>

<span data-ttu-id="22e6d-144">Ez az eljárás befejezésekor a CER-tanúsítványt az Azure-előfizetése legyen elhelyezve, és a JKS tanúsítványt a helyi meghajtón legyen elhelyezve.</span><span class="sxs-lookup"><span data-stu-id="22e6d-144">When you complete this procedure, the CER certificate will reside in your Azure subscription and the JKS certificate will reside on your local drive.</span></span> <span data-ttu-id="22e6d-145">A felügyeleti tanúsítványokról további információ: [létrehozása és feltöltése az Azure felügyeleti tanúsítvánnyal][Create and Upload a Management Certificate for Azure].</span><span class="sxs-lookup"><span data-stu-id="22e6d-145">For more information on management certificates, see [Create and Upload a Management Certificate for Azure][Create and Upload a Management Certificate for Azure].</span></span>

#### <a name="create-a-certificate"></a><span data-ttu-id="22e6d-146">Tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="22e6d-146">Create a certificate</span></span>
<span data-ttu-id="22e6d-147">A saját önaláírt tanúsítvány létrehozása, nyissa meg a parancskonzolról, az operációs rendszerben, és futtassa a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="22e6d-147">To create your own self-signed certificate, open a command console on your operating system and run the following commands.</span></span>

> <span data-ttu-id="22e6d-148">**Megjegyzés:** ezt a parancsot futtató számítógépen telepíteni kell a telepített JDK.</span><span class="sxs-lookup"><span data-stu-id="22e6d-148">**Note:**  The computer on which you run this command must have the JDK installed.</span></span> <span data-ttu-id="22e6d-149">Az elérési útját a kulcseszköz is, a JDK telepítésének helyét függ.</span><span class="sxs-lookup"><span data-stu-id="22e6d-149">Also, the path to the keytool depends on the location in which you install the JDK.</span></span> <span data-ttu-id="22e6d-150">További információkért lásd: [kulcs és a tanúsítvány eszköz (kulcseszközről)] [ Key and Certificate Management Tool (keytool)] a a Java online dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="22e6d-150">For more information, see [Key and Certificate Management Tool (keytool)][Key and Certificate Management Tool (keytool)] in the Java online docs.</span></span>
> 
> 

<span data-ttu-id="22e6d-151">A .pfx fájl létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="22e6d-151">To create the .pfx file:</span></span>

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

<span data-ttu-id="22e6d-152">A .cer fájl létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="22e6d-152">To create the .cer file:</span></span>

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

<span data-ttu-id="22e6d-153">Ahol:</span><span class="sxs-lookup"><span data-stu-id="22e6d-153">where:</span></span>

* <span data-ttu-id="22e6d-154">`<java-install-dir>`a könyvtár elérési útját a Java telepítve van.</span><span class="sxs-lookup"><span data-stu-id="22e6d-154">`<java-install-dir>` is the path to the directory in which you installed Java.</span></span>
* <span data-ttu-id="22e6d-155">`<keystore-id>`a kulcstár bejegyzés azonosítója (például `AzureRemoteAccess`).</span><span class="sxs-lookup"><span data-stu-id="22e6d-155">`<keystore-id>` is the keystore entry identifier (for example, `AzureRemoteAccess`).</span></span>
* <span data-ttu-id="22e6d-156">`<cert-store-dir>`a könyvtár elérési útját, amelyben a tanúsítványok tárolni szeretné van (például `C:/Certificates`).</span><span class="sxs-lookup"><span data-stu-id="22e6d-156">`<cert-store-dir>` is the path to the directory in which you want to store certificates (for example `C:/Certificates`).</span></span>
* <span data-ttu-id="22e6d-157">`<cert-file-name>`a tanúsítványfájl neve (például `AzureWebDemoCert`).</span><span class="sxs-lookup"><span data-stu-id="22e6d-157">`<cert-file-name>` is the name of the certificate file (for example `AzureWebDemoCert`).</span></span>
* <span data-ttu-id="22e6d-158">`<password>`a jelszó úgy dönt, hogy megvédeni a tanúsítványt; legalább 6 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="22e6d-158">`<password>` is the password you choose to protect the certificate; it must be at least 6 characters long.</span></span> <span data-ttu-id="22e6d-159">Nincs jelszava, akkor adhatja meg, bár ez nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="22e6d-159">You can enter no password, although this is not recommended.</span></span>
* <span data-ttu-id="22e6d-160">`<dname>`alias társítandó X.500 megkülönböztető nevét, és a kibocsátó és a tulajdonos mezőt az önaláírt tanúsítvány használatos.</span><span class="sxs-lookup"><span data-stu-id="22e6d-160">`<dname>` is the X.500 Distinguished Name to be associated with alias, and is used as the issuer and subject fields in the self-signed certificate.</span></span>

<span data-ttu-id="22e6d-161">További információkért lásd: [létrehozása és feltöltése az Azure felügyeleti tanúsítvánnyal][Create and Upload a Management Certificate for Azure].</span><span class="sxs-lookup"><span data-stu-id="22e6d-161">For more information, see [Create and Upload a Management Certificate for Azure][Create and Upload a Management Certificate for Azure].</span></span>

#### <a name="upload-the-certificate"></a><span data-ttu-id="22e6d-162">A tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="22e6d-162">Upload the certificate</span></span>
<span data-ttu-id="22e6d-163">Önaláírt tanúsítvány feltöltése az Azure-ba, keresse fel a **beállítások** a klasszikus portálon lapon, majd kattintson a **felügyeleti tanúsítványok** fülre. Kattintson a **feltöltése** a lap alján, és keresse meg a létrehozott CER-fájl helyét.</span><span class="sxs-lookup"><span data-stu-id="22e6d-163">To upload a self-signed certificate to Azure, go to the **Settings** page in the classic portal, then click the **Management Certificates** tab. Click **Upload** at the bottom of the page and navigate to the location of the CER file you created.</span></span>

#### <a name="convert-the-pfx-file-into-jks"></a><span data-ttu-id="22e6d-164">A PFX-fájl átalakítása JKS</span><span class="sxs-lookup"><span data-stu-id="22e6d-164">Convert the PFX file into JKS</span></span>
<span data-ttu-id="22e6d-165">A a Windows parancssor (admin néven fut), CD-ről a könyvtárba, a tanúsítványokat tartalmazó és a következő parancsot, ahol `<java-install-dir>` címtár, amelyben a számítógépen telepítette a Java:</span><span class="sxs-lookup"><span data-stu-id="22e6d-165">In the Windows Command Prompt (running as admin), cd to the directory containing the certificates and run the following command, where `<java-install-dir>` is the directory in which you installed Java on your computer:</span></span>

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. <span data-ttu-id="22e6d-166">Amikor a rendszer kéri, adja meg a cél keystore jelszót; Ez lesz a JKS fájlhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="22e6d-166">When prompted, enter the destination keystore password; this will be the password for the JKS file.</span></span>
2. <span data-ttu-id="22e6d-167">Amikor a rendszer kéri, adja meg a forrás keystore jelszót; Ez az a PFX-fájljának megadott jelszót.</span><span class="sxs-lookup"><span data-stu-id="22e6d-167">When prompted, enter the source keystore password; this is the password you specified for the PFX file.</span></span>

<span data-ttu-id="22e6d-168">A két jelszó nem kell azonosnak lennie.</span><span class="sxs-lookup"><span data-stu-id="22e6d-168">The two passwords do not have to be the same.</span></span> <span data-ttu-id="22e6d-169">Nincs jelszava, akkor adhatja meg, bár ez nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="22e6d-169">You can enter no password, although this is not recommended.</span></span>

## <a name="build-a-web-app-creation-application"></a><span data-ttu-id="22e6d-170">A webalkalmazás létrehozása alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="22e6d-170">Build a Web App creation application</span></span>
### <a name="create-the-eclipse-workspace-and-maven-project"></a><span data-ttu-id="22e6d-171">Az Eclipse munkaterület és a Maven Project létrehozása</span><span class="sxs-lookup"><span data-stu-id="22e6d-171">Create the Eclipse Workspace and Maven Project</span></span>
<span data-ttu-id="22e6d-172">Ez a szakasz a munkaterületet, és egy Maven-projektjét, amely az alkalmazás létrehozását, nevű webalkalmazás AzureWebDemo hoz létre.</span><span class="sxs-lookup"><span data-stu-id="22e6d-172">In this section you create a workspace and a Maven project for the web app creation application, named AzureWebDemo.</span></span>

1. <span data-ttu-id="22e6d-173">Hozzon létre egy új Maven-projektet.</span><span class="sxs-lookup"><span data-stu-id="22e6d-173">Create a new Maven project.</span></span> <span data-ttu-id="22e6d-174">Kattintson a **fájl > Új > Maven Project**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-174">Click **File > New > Maven Project**.</span></span> <span data-ttu-id="22e6d-175">A **új Maven Project**, jelölje be **hozzon létre egy egyszerű projektet** és **alapértelmezett munkaterület helyet**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-175">In **New Maven Project**, select **Create a simple project** and **Use default workspace location**.</span></span>
2. <span data-ttu-id="22e6d-176">A második lapján **új Maven Project**, adja meg a következőket:</span><span class="sxs-lookup"><span data-stu-id="22e6d-176">On the second page of **New Maven Project**, specify the following:</span></span>
   
   * <span data-ttu-id="22e6d-177">Csoport azonosítója:`com.<username>.azure.webdemo`</span><span class="sxs-lookup"><span data-stu-id="22e6d-177">Group ID: `com.<username>.azure.webdemo`</span></span>
   * <span data-ttu-id="22e6d-178">Összetevő-azonosító: AzureWebDemo</span><span class="sxs-lookup"><span data-stu-id="22e6d-178">Artifact ID: AzureWebDemo</span></span>
   * <span data-ttu-id="22e6d-179">Verzió: 0.0.1-SNAPSHOT</span><span class="sxs-lookup"><span data-stu-id="22e6d-179">Version: 0.0.1-SNAPSHOT</span></span>
   * <span data-ttu-id="22e6d-180">Csomagolására: jar</span><span class="sxs-lookup"><span data-stu-id="22e6d-180">Packaging: jar</span></span>
   * <span data-ttu-id="22e6d-181">Name: AzureWebDemo</span><span class="sxs-lookup"><span data-stu-id="22e6d-181">Name: AzureWebDemo</span></span>
     
     <span data-ttu-id="22e6d-182">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="22e6d-182">Click **Finish**.</span></span>
3. <span data-ttu-id="22e6d-183">Nyissa meg az új projekt pom.xml fájlt a Project Explorer.</span><span class="sxs-lookup"><span data-stu-id="22e6d-183">Open the new project's pom.xml file in Project Explorer.</span></span> <span data-ttu-id="22e6d-184">Válassza ki a **függőségek** fülre. Mivel ez egy új projektet, még felsorolt csomagok száma.</span><span class="sxs-lookup"><span data-stu-id="22e6d-184">Select the **Dependencies** tab. As this is a new project, no packages are listed yet.</span></span>
4. <span data-ttu-id="22e6d-185">A Maven adattárak nézet megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="22e6d-185">Open the Maven Repositories view.</span></span> <span data-ttu-id="22e6d-186">**Kattintson az ablak > nézet megjelenítése > más > Maven > Maven Tárházak** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-186">**Click Window > Show View > Other > Maven > Maven Repositories** and click **OK**.</span></span> <span data-ttu-id="22e6d-187">A **Maven Tárházak** nézete jelenik meg az IDE alján.</span><span class="sxs-lookup"><span data-stu-id="22e6d-187">The **Maven Repositories** view will appear at the bottom of the IDE.</span></span>
5. <span data-ttu-id="22e6d-188">Nyissa meg **globális adattárak**, kattintson a jobb gombbal a **központi** tárház, és válassza ki **Rebuild Index**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-188">Open **Global Repositories**, right-click the **central** repository, and select **Rebuild Index**.</span></span>
   
    ![][1]
   
    <span data-ttu-id="22e6d-189">Ez a lépés a kapcsolat sebességétől függően több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="22e6d-189">This step can take several minutes depending on the speed of your connection.</span></span> <span data-ttu-id="22e6d-190">Amikor újraépíti az indexet, megjelenik a Microsoft Azure csomagok a **központi** Maven-tárházat.</span><span class="sxs-lookup"><span data-stu-id="22e6d-190">When the index rebuilds, you should see the Microsoft Azure packages in the **central** Maven repository.</span></span>
6. <span data-ttu-id="22e6d-191">A **függőségek**, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-191">In **Dependencies**, click **Add**.</span></span> <span data-ttu-id="22e6d-192">A **adja meg a csoport azonosítója...**  meg `azure-management`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-192">In **Enter Group ID...** enter `azure-management`.</span></span> <span data-ttu-id="22e6d-193">Válassza ki az alapvető felügyeleti és az App Service Web Apps felügyeleti csomagok:</span><span class="sxs-lookup"><span data-stu-id="22e6d-193">Select the packages for base management and App Service Web Apps management:</span></span>
   
        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites
   
   > <span data-ttu-id="22e6d-194">**Megjegyzés:** követően egy új verzióra frissítésekor a függőségeket, azt újra hozzá kell minden, a lista a függőségi.</span><span class="sxs-lookup"><span data-stu-id="22e6d-194">**Note:** If you are updating the dependencies after a new version release, you need to re-add each of the dependencies in this list.</span></span>
   > <span data-ttu-id="22e6d-195">Miután rákattintott **Hozzáadás** akkor jelenik meg, az új verziószámmal az egyes függőségek válassza ki a **függőségek** listája.</span><span class="sxs-lookup"><span data-stu-id="22e6d-195">After you click **Add** and select each dependency, it appears with the new version number in the **Dependencies** list.</span></span>
   > 
   > 

<span data-ttu-id="22e6d-196">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="22e6d-196">Click **OK**.</span></span> <span data-ttu-id="22e6d-197">Az Azure csomagok majd megjelennek a **függőségek** listája.</span><span class="sxs-lookup"><span data-stu-id="22e6d-197">The Azure packages then appear in the **Dependencies** list.</span></span>

### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a><span data-ttu-id="22e6d-198">A webalkalmazás létrehozása az Azure SDK meghívásával Java programozás</span><span class="sxs-lookup"><span data-stu-id="22e6d-198">Writing Java Code to Create a Web App by Calling the Azure SDK</span></span>
<span data-ttu-id="22e6d-199">Következő lépésként írja meg az App Service webalkalmazás létrehozása Javához készült Azure SDK API-k behívó kód.</span><span class="sxs-lookup"><span data-stu-id="22e6d-199">Next, write the code that calls APIs in the Azure SDK for Java to create the App Service web app.</span></span>

1. <span data-ttu-id="22e6d-200">Hozzon létre egy Java-osztály a fő belépési pont kódot tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="22e6d-200">Create a Java class to contain the main entry point code.</span></span> <span data-ttu-id="22e6d-201">A Project Explorer, kattintson a jobb gombbal a projektcsomópontra, és válassza ki **új > osztály**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-201">In Project Explorer, right-click on the project node and select **New > Class**.</span></span>
2. <span data-ttu-id="22e6d-202">A **új Java-osztály**, az osztály neve `WebCreator` , és ellenőrizze a **nyilvános statikus "void" main** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="22e6d-202">In **New Java Class**, name the class `WebCreator` and check the **public static void main** checkbox.</span></span> <span data-ttu-id="22e6d-203">A megadott beállítások megjelenjen-e az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="22e6d-203">The selections should appear as follows:</span></span>
   
    ![][2]
3. <span data-ttu-id="22e6d-204">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="22e6d-204">Click **Finish**.</span></span> <span data-ttu-id="22e6d-205">A WebCreator.java fájl Project Explorer jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="22e6d-205">The WebCreator.java file appears in Project Explorer.</span></span>

### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a><span data-ttu-id="22e6d-206">Az App Service webalkalmazás létrehozása az Azure API hívása</span><span class="sxs-lookup"><span data-stu-id="22e6d-206">Calling the Azure API to Create an App Service Web App</span></span>
#### <a name="add-necessary-imports"></a><span data-ttu-id="22e6d-207">Adja hozzá a szükséges importálása</span><span class="sxs-lookup"><span data-stu-id="22e6d-207">Add necessary imports</span></span>
<span data-ttu-id="22e6d-208">WebCreator.java adja hozzá az alábbi importálásokat; Ezek importálja az osztályokat a kezelési kódtárakat az Azure API-k fel hozzáférést biztosítanak:</span><span class="sxs-lookup"><span data-stu-id="22e6d-208">In WebCreator.java, add the following imports; these imports provide access to classes in the management libraries for consuming Azure APIs:</span></span>

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


#### <a name="define-the-main-entry-point-class"></a><span data-ttu-id="22e6d-209">Adja meg a fő belépési pont osztály</span><span class="sxs-lookup"><span data-stu-id="22e6d-209">Define the main entry point class</span></span>
<span data-ttu-id="22e6d-210">A AzureWebDemo alkalmazás célja egy App Service webalkalmazás létrehozása, mert az alkalmazás neve a fő osztályban `WebAppCreator`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-210">Because the purpose of the AzureWebDemo application is to create an App Service Web App, name the main class for this application `WebAppCreator`.</span></span> <span data-ttu-id="22e6d-211">Ez az osztály a fő belépési pont a webalkalmazás létrehozása az Azure Service Management API behívó kód biztosít.</span><span class="sxs-lookup"><span data-stu-id="22e6d-211">This class provides the main entry point code that calls the Azure Service Management API to create the web app.</span></span>

<span data-ttu-id="22e6d-212">Adja hozzá az alábbi paraméterdefiníciókra web app és webtárhely.</span><span class="sxs-lookup"><span data-stu-id="22e6d-212">Add the following parameter definitions for the web app and webspace.</span></span> <span data-ttu-id="22e6d-213">Meg kell adnia a saját Azure-előfizetés azonosítója és a tanúsítvány adatait.</span><span class="sxs-lookup"><span data-stu-id="22e6d-213">You will need to provide your own Azure subscription ID and certificate information.</span></span>

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

<span data-ttu-id="22e6d-214">Ahol:</span><span class="sxs-lookup"><span data-stu-id="22e6d-214">where:</span></span>

* <span data-ttu-id="22e6d-215">`<subscription-id>`az Azure-előfizetése Azonosítóját, amelyen létrehozásához az erőforrás van.</span><span class="sxs-lookup"><span data-stu-id="22e6d-215">`<subscription-id>` is the Azure subscription ID in which you want to create the resource.</span></span>
* <span data-ttu-id="22e6d-216">`<certificate-store-path>`az elérési út és fájlnév a JKS fájlt a helyi tanúsítványtárolóban tároló könyvtárban van.</span><span class="sxs-lookup"><span data-stu-id="22e6d-216">`<certificate-store-path>` is the path and filename to the JKS file in your local certificate store directory.</span></span> <span data-ttu-id="22e6d-217">Például `C:/Certificates/CertificateName.jks` Linux és `C:\Certificates\CertificateName.jks` Windows.</span><span class="sxs-lookup"><span data-stu-id="22e6d-217">For example, `C:/Certificates/CertificateName.jks` for Linux and `C:\Certificates\CertificateName.jks` for Windows.</span></span>
* <span data-ttu-id="22e6d-218">`<certificate-password>`az a jelszó a JKS tanúsítvány létrehozásakor megadott.</span><span class="sxs-lookup"><span data-stu-id="22e6d-218">`<certificate-password>` is the password you specified when you created your JKS certificate.</span></span>
* <span data-ttu-id="22e6d-219">`webAppName`bármilyen nevet választania; Ez az eljárás nevét használja `WebDemoWebApp`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-219">`webAppName` can be any name you choose; this procedure uses the name `WebDemoWebApp`.</span></span> <span data-ttu-id="22e6d-220">A teljes tartománynév a `webAppName` rendelkező a `domainName` fűzött, így ebben az esetben a teljes tartomány pedig `webdemowebapp.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-220">The full domain name is the `webAppName` with the `domainName` appended, so in this case the full domain is `webdemowebapp.azurewebsites.net`.</span></span>
* <span data-ttu-id="22e6d-221">`domainName`meg kell adni a fentiek szerint.</span><span class="sxs-lookup"><span data-stu-id="22e6d-221">`domainName` should be specified as shown above.</span></span>
* <span data-ttu-id="22e6d-222">`webSpaceName`megadott értékek egyike lehet: a [WebSpaceNames] [ WebSpaceNames] osztály.</span><span class="sxs-lookup"><span data-stu-id="22e6d-222">`webSpaceName` should be one of the values defined in the [WebSpaceNames][WebSpaceNames] class.</span></span>
* <span data-ttu-id="22e6d-223">`appServicePlanName`meg kell adni a fentiek szerint.</span><span class="sxs-lookup"><span data-stu-id="22e6d-223">`appServicePlanName` should be specified as shown above.</span></span>

> <span data-ttu-id="22e6d-224">**Megjegyzés:** minden alkalommal, amikor az alkalmazás futtatásához, módosítania kell a értékének `webAppName` és `appServicePlanName` (vagy törölje a webalkalmazást az Azure-portál) az alkalmazás ismételt futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="22e6d-224">**Note:** Each time you run this application, you need to change the value of `webAppName` and `appServicePlanName` (or delete the web app on the Azure Portal) before running the application again.</span></span> <span data-ttu-id="22e6d-225">Ellenkező esetben végrehajtása sikertelen lesz, mert ugyanaz az erőforrás már létezik az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="22e6d-225">Otherwise, execution will fail because the same resource already exists on Azure.</span></span>
> 
> 

#### <a name="define-the-web-creation-method"></a><span data-ttu-id="22e6d-226">A webalkalmazás-létrehozási módjának megadása</span><span class="sxs-lookup"><span data-stu-id="22e6d-226">Define the web creation method</span></span>
<span data-ttu-id="22e6d-227">A következő határozza meg a webalkalmazás létrehozása egy metódust.</span><span class="sxs-lookup"><span data-stu-id="22e6d-227">Next, define a method to create the web app.</span></span> <span data-ttu-id="22e6d-228">Ezzel a módszerrel `createWebApp`, a web app és az webtárhely paramétereket határozza meg.</span><span class="sxs-lookup"><span data-stu-id="22e6d-228">This method, `createWebApp`, specifies the parameters of the web app and the webspace.</span></span> <span data-ttu-id="22e6d-229">Emellett hoz létre, és konfigurálja az App Service Web Apps-felügyeleti ügyfél, által definiált a [WebSiteManagementClient] [ WebSiteManagementClient] objektum.</span><span class="sxs-lookup"><span data-stu-id="22e6d-229">It also creates and configures the App Service Web Apps management client, which is defined by the [WebSiteManagementClient][WebSiteManagementClient] object.</span></span> <span data-ttu-id="22e6d-230">A felügyeleti ügyfél webalkalmazások létrehozása a kulcs.</span><span class="sxs-lookup"><span data-stu-id="22e6d-230">The management client is key to creating Web Apps.</span></span> <span data-ttu-id="22e6d-231">RESTful webes szolgáltatásokat, amelyek lehetővé teszik az alkalmazások (például létrehozása, update és delete, a műveletek végrehajtása) webes alkalmazásainak kezeléséhez a szolgáltatáskezelő API meghívásával nyújt.</span><span class="sxs-lookup"><span data-stu-id="22e6d-231">It provides RESTful web services that allow applications to manage web apps (performing operations such as create, update, and delete) by calling the service management API.</span></span>

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
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
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

<span data-ttu-id="22e6d-232">A kód a HTTP-állapot, a válasz sikerességét vagy sikertelenségét jelző kimeneteként, és ha sikeres, a létrehozott webalkalmazás neve lesz kimeneti.</span><span class="sxs-lookup"><span data-stu-id="22e6d-232">The code will output the HTTP status of the response indicating success or failure, and if successful, will output the name of the created web app.</span></span>

#### <a name="define-the-main-method"></a><span data-ttu-id="22e6d-233">A main() módszer megadása</span><span class="sxs-lookup"><span data-stu-id="22e6d-233">Define the main() method</span></span>
<span data-ttu-id="22e6d-234">Adja meg, amely behívja a webalkalmazás létrehozása createWebApp() main() metódus kódot.</span><span class="sxs-lookup"><span data-stu-id="22e6d-234">Provide the main() method code that calls createWebApp() to create the web app.</span></span>

<span data-ttu-id="22e6d-235">Végezetül hívás `createWebApp` a `main`:</span><span class="sxs-lookup"><span data-stu-id="22e6d-235">Finally, call `createWebApp` from `main`:</span></span>

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a><span data-ttu-id="22e6d-236">Futtassa az alkalmazást, és ellenőrizze a webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="22e6d-236">Run the application and verify web app creation</span></span>
<span data-ttu-id="22e6d-237">Győződjön meg arról, hogy az alkalmazás fut, kattintson a **futtatása > futtassa**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-237">To verify that your application runs, click **Run > Run**.</span></span> <span data-ttu-id="22e6d-238">Az alkalmazás befejeződésekor az Eclipse-konzolon a következő kimenetet kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="22e6d-238">When the application completes running, you should see the following output in the Eclipse console:</span></span>

    ----------
    Web app created - HTTP response 200

    ----------

    Name of web app created: WebDemoWebApp

    ----------

<span data-ttu-id="22e6d-239">Jelentkezzen be a klasszikus Azure portálra, majd kattintson a **webalkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-239">Log into the Azure classic portal and click **Web Apps**.</span></span> <span data-ttu-id="22e6d-240">Az új webalkalmazásba meg kell jelennie a webes alkalmazások listájában néhány percen belül.</span><span class="sxs-lookup"><span data-stu-id="22e6d-240">The new web app should appear in the Web Apps list within a few minutes.</span></span>

## <a name="deploying-an-application-to-the-web-app"></a><span data-ttu-id="22e6d-241">A webalkalmazás az alkalmazások központi telepítése</span><span class="sxs-lookup"><span data-stu-id="22e6d-241">Deploying an Application to the Web App</span></span>
<span data-ttu-id="22e6d-242">Miután futtatta AzureWebDemo, és kattintson az új webalkalmazásba, jelentkezzen be a klasszikus portálon létrehozott **Web Apps**, és válassza ki **WebDemoWebApp** a a **Web Apps** lista.</span><span class="sxs-lookup"><span data-stu-id="22e6d-242">After you have run AzureWebDemo and created the new web app, log into the classic portal, click **Web Apps**, and select **WebDemoWebApp** in the **Web Apps** list.</span></span> <span data-ttu-id="22e6d-243">A webes alkalmazás irányítópult-oldalon, kattintson **Tallózás** (kattintson az URL-címet, vagy `webdemowebapp.azurewebsites.net`) is.</span><span class="sxs-lookup"><span data-stu-id="22e6d-243">In the web app's dashboard page, click **Browse** (or click the URL, `webdemowebapp.azurewebsites.net`) to navigate to it.</span></span> <span data-ttu-id="22e6d-244">Egy üres helyőrző lap jelenik meg, mert nincs tartalom is közzé lett téve a webalkalmazáson még.</span><span class="sxs-lookup"><span data-stu-id="22e6d-244">You will see a blank placeholder page, because no content has been published to the web app yet.</span></span>

<span data-ttu-id="22e6d-245">Ezután létrehoz a "Hello, World" alkalmazást, és központilag telepítenie kell a webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="22e6d-245">Next you will create a "Hello World" application and deploy it to the web app.</span></span>

### <a name="create-a-jsp-hello-world-application"></a><span data-ttu-id="22e6d-246">JSP Hello World-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="22e6d-246">Create a JSP Hello World application</span></span>
#### <a name="create-the-application"></a><span data-ttu-id="22e6d-247">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="22e6d-247">Create the application</span></span>
<span data-ttu-id="22e6d-248">Ahhoz, hogy bemutatják, hogyan lehet a webes alkalmazás központi telepítése, az alábbi eljárás bemutatja, hogyan hozzon létre egy egyszerű "Hello, World" Java-alkalmazást, és töltse fel az App Service Web-alkalmazást létrehozni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="22e6d-248">In order to demonstrate how to deploy an application to the web, the following procedure shows you how to create a simple "Hello World" Java application and upload it to the App Service Web App that your application created.</span></span>

1. <span data-ttu-id="22e6d-249">Kattintson a **fájl > Új > dinamikus webes projekt**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-249">Click **File > New > Dynamic Web Project**.</span></span> <span data-ttu-id="22e6d-250">Nevezze el a következőképpen: `JSPHello`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-250">Name it `JSPHello`.</span></span> <span data-ttu-id="22e6d-251">Nem kell módosítania a ezen a párbeszédpanelen egyéb beállításait.</span><span class="sxs-lookup"><span data-stu-id="22e6d-251">You do not need to change any other settings in this dialog.</span></span> <span data-ttu-id="22e6d-252">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="22e6d-252">Click **Finish**.</span></span>
   
    ![][3]
2. <span data-ttu-id="22e6d-253">A Project Explorer bontsa ki a **JSPHello** projektre, kattintson a jobb gombbal **WebContent**, majd kattintson a **új > JSP-fájl**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-253">In Project Explorer, expand the **JSPHello** project, right-click **WebContent**, then click **New > JSP File**.</span></span> <span data-ttu-id="22e6d-254">Új JSP-fájl párbeszédablakban nevezze el az új fájlt `index.jsp`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-254">In the New JSP File dialog, name the new file `index.jsp`.</span></span> <span data-ttu-id="22e6d-255">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="22e6d-255">Click **Next**.</span></span>
3. <span data-ttu-id="22e6d-256">Az a **JSP-sablon kiválasztása** párbeszédpanelen válassza **új JSP-fájl (html)** kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-256">In the **Select JSP Template** dialog, select **New JSP File (html)** and click **Finish**.</span></span>
4. <span data-ttu-id="22e6d-257">Index.jsp, adja hozzá a következő kódot a `<head>` és `<body>` szakaszok címkét:</span><span class="sxs-lookup"><span data-stu-id="22e6d-257">In index.jsp, add the following code in the `<head>` and `<body>` tag sections:</span></span>
   
        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
   
        <body>
          Hello, the time is <%= date %> 
        </body>

#### <a name="run-the-hello-world-application-in-localhost"></a><span data-ttu-id="22e6d-258">Futtassa a Hello World alkalmazásról localhost</span><span class="sxs-lookup"><span data-stu-id="22e6d-258">Run the Hello World application in localhost</span></span>
<span data-ttu-id="22e6d-259">Ez az alkalmazás futtatása előtt kell néhány tulajdonságainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="22e6d-259">Before you run this application, you need to configure a few properties.</span></span>

1. <span data-ttu-id="22e6d-260">Kattintson a jobb gombbal a **JSPHello** projektre, és válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-260">Right-click the **JSPHello** project and select **Properties**.</span></span>
2. <span data-ttu-id="22e6d-261">Az a **tulajdonságok** párbeszédpanel: Jelölje ki **Java Build elérési**, jelölje be a **rendezés és exportálása** lapon jelölje **JRE rendszerkönyvtár**, kattintson a **Mentése** áthelyezni a lista elejére.</span><span class="sxs-lookup"><span data-stu-id="22e6d-261">In the **Properties** dialog: select **Java Build Path**, select the **Order and Export** tab, check **JRE System Library**, then click **Up** to move it to the top of the list.</span></span>
   
    ![][4]
3. <span data-ttu-id="22e6d-262">Is a **tulajdonságok** párbeszédpanel: válasszon **megcélzott futtatókörnyezetek** kattintson **új**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-262">Also in the **Properties** dialog: select **Targeted Runtimes** and click **New**.</span></span>
4. <span data-ttu-id="22e6d-263">Az a **új kiszolgáló Futtatókörnyezetét** párbeszédpanelen válasszon ki egy kiszolgálót, mint **Apache Tomcat v7.0** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-263">In the **New Server Runtime Environment** dialog, select a server such as **Apache Tomcat v7.0** and click **Next**.</span></span> <span data-ttu-id="22e6d-264">Az a **Tomcat kiszolgálót** párbeszédpanelen, a set **neve** való `Apache Tomcat v7.0`, és állítsa be **Tomcat telepítési könyvtárát** azt a könyvtárat, amelyben a Tomcat verziójának telepítése használni kívánt kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="22e6d-264">In the **Tomcat Server** dialog, set **Name** to `Apache Tomcat v7.0`, and set **Tomcat Installation Directory** to the directory in which you installed the version of Tomcat server you want to use.</span></span>
   
    ![][5]
   
    <span data-ttu-id="22e6d-265">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="22e6d-265">Click **Finish**.</span></span>
5. <span data-ttu-id="22e6d-266">Majd térjen vissza a **megcélzott futtatókörnyezetek** oldalán a **tulajdonságok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="22e6d-266">You then return to the **Targeted Runtimes** page of the **Properties** dialog.</span></span> <span data-ttu-id="22e6d-267">Válassza ki **Apache Tomcat v7.0**, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-267">Select **Apache Tomcat v7.0**, then click **OK**.</span></span>
   
    ![][6]
6. <span data-ttu-id="22e6d-268">Az az eclipse-ben **futtatása** menüben kattintson a **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-268">In the Eclipse **Run** menu, click **Run**.</span></span> <span data-ttu-id="22e6d-269">Az a **futtató** párbeszédablakban válassza **futtassa a kiszolgálón**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-269">In the **Run As** dialog, select **Run on Server**.</span></span> <span data-ttu-id="22e6d-270">Az a **futtassa a kiszolgálón** párbeszédablakban válassza **Tomcat v7.0 Server**:</span><span class="sxs-lookup"><span data-stu-id="22e6d-270">In the **Run on Server** dialog, select **Tomcat v7.0 Server**:</span></span>
   
    ![][7]
   
    <span data-ttu-id="22e6d-271">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="22e6d-271">Click **Finish**.</span></span>
7. <span data-ttu-id="22e6d-272">Az alkalmazás futtatásakor, kell megjelennie a **JSPHello** lap jelenik meg az eclipse-ben localhost ablakban (`http://localhost:8080/JSPHello/`), a következő üzenet:</span><span class="sxs-lookup"><span data-stu-id="22e6d-272">When the application runs, you should see the **JSPHello** page appear in a localhost window in Eclipse (`http://localhost:8080/JSPHello/`), displaying the following message:</span></span>
   
    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="export-the-application-as-a-war"></a><span data-ttu-id="22e6d-273">Az alkalmazások exportálásáról a WAR-fájlként</span><span class="sxs-lookup"><span data-stu-id="22e6d-273">Export the application as a WAR</span></span>
<span data-ttu-id="22e6d-274">A web project fájlok exportálása egy webes archívumfájl (WAR), hogy a webes alkalmazás ezután telepítheti azt.</span><span class="sxs-lookup"><span data-stu-id="22e6d-274">Export the web project files as a web archive (WAR) file so that you can deploy it to the web app.</span></span> <span data-ttu-id="22e6d-275">A következő webes projekt fájljai a WebContent mappában találhatók:</span><span class="sxs-lookup"><span data-stu-id="22e6d-275">The following web project files reside in the WebContent folder:</span></span>

    META-INF
    WEB-INF
    index.jsp

1. <span data-ttu-id="22e6d-276">Kattintson a jobb gombbal a WebContent mappát, és válassza ki **exportálása**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-276">Right-click the WebContent folder and select **Export**.</span></span>
2. <span data-ttu-id="22e6d-277">A a **exportálása kiválasztása** párbeszédpanel, kattintson a **Web > WAR** fájlt, majd kattintson az **tovább**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-277">In the **Export Select** dialog, click **Web > WAR** file, then click **Next**.</span></span>
3. <span data-ttu-id="22e6d-278">Az a **WAR exportálása** párbeszédpanelen válassza ki a src könyvtár az aktuális projektben, és a végén a WAR-fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="22e6d-278">In the **WAR Export** dialog, select the src directory in the current project, and include the name of the WAR file at the end.</span></span> <span data-ttu-id="22e6d-279">Példa:</span><span class="sxs-lookup"><span data-stu-id="22e6d-279">For example:</span></span>
   
    `<project-path>/JSPHello/src/JSPHello.war`

<span data-ttu-id="22e6d-280">WAR fájlok telepítésével kapcsolatos további információkért lásd: [hozzáadása a Java-alkalmazások az Azure App Service Web Apps](web-sites-java-add-app.md).</span><span class="sxs-lookup"><span data-stu-id="22e6d-280">For more information on deploying WAR files, see [Add a Java application to Azure App Service Web Apps](web-sites-java-add-app.md).</span></span>

### <a name="deploying-the-hello-world-application-using-ftp"></a><span data-ttu-id="22e6d-281">A Hello World alkalmazás használatával az FTP telepítése</span><span class="sxs-lookup"><span data-stu-id="22e6d-281">Deploying the Hello World Application Using FTP</span></span>
<span data-ttu-id="22e6d-282">Válassza ki a külső FTP-ügyfél számára tegye közzé az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="22e6d-282">Select a third-party FTP client to publish the application.</span></span> <span data-ttu-id="22e6d-283">Ez az eljárás ismerteti a két lehetőség közül választhat: a Kudu konzol épített Azure; és FileZilla, olyan népszerű eszköz egy kényelmes, grafikus felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="22e6d-283">This procedure describes two options: the Kudu console built into Azure; and FileZilla, a popular tool with a convenient, graphical UI.</span></span>

> <span data-ttu-id="22e6d-284">**Megjegyzés:** Eclipse Azure eszköztára támogatja a központi telepítést, hogy a storage-fiókok és a felhőalapú szolgáltatások, de jelenleg nem támogatja a webalkalmazások központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="22e6d-284">**Note:** The Azure Toolkit for Eclipse supports deployment to storage accounts and cloud services, but does not currently support deployment to web apps.</span></span> <span data-ttu-id="22e6d-285">Storage-fiókokra telepítheti, és a felhőalapú Azure telepítési projekt segítségével, a szolgáltatások [egy Hello World alkalmazás létrehozása az Azure az eclipse-ben](http://msdn.microsoft.com/library/azure/hh690944.aspx), de nem a webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="22e6d-285">You can deploy to storage accounts and cloud services using an Azure Deployment Project as described in [Creating a Hello World Application for Azure in Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), but not to web apps.</span></span> <span data-ttu-id="22e6d-286">Más módszerekkel például az FTP- vagy GitHub fájlok átvitele a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="22e6d-286">Use other methods such as FTP or GitHub to transfer files to your web app.</span></span>
> 
> <span data-ttu-id="22e6d-287">**Megjegyzés:** FTP-használ, a Windows parancssorából (a parancssori FTP.EXE segédprogram, amely a Windows részét képező) nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="22e6d-287">**Note:** We do not recommend using FTP from the Windows command prompt (the command-line FTP.EXE utility that ships with Windows).</span></span> <span data-ttu-id="22e6d-288">FTP-ügyfelek, amelyek aktív FTP, például a FTP.EXE, gyakran nem működik a tűzfalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="22e6d-288">FTP clients that use active FTP, such as FTP.EXE, often fail to work over firewalls.</span></span> <span data-ttu-id="22e6d-289">Aktív FTP címet adja meg egy belső LAN-alapú, amelyhez az FTP-kiszolgáló lesz valószínűleg nem tudnak majd kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="22e6d-289">Active FTP specifies an internal LAN-based address, to which an FTP server will likely fail to connect.</span></span>
> 
> 

<span data-ttu-id="22e6d-290">Egy App Service webalkalmazásba FTP használatával telepített további információkért lásd a következő témaköröket:</span><span class="sxs-lookup"><span data-stu-id="22e6d-290">For more information on deployment to an App Service web app using FTP, see the following topics:</span></span>

* [<span data-ttu-id="22e6d-291">Az FTP-segédprogrammal történő telepítése</span><span class="sxs-lookup"><span data-stu-id="22e6d-291">Deploy using an FTP utility</span></span>](web-sites-deploy.md)

#### <a name="set-up-deployment-credentials"></a><span data-ttu-id="22e6d-292">Üzembe helyezési hitelesítő adatok beállítása</span><span class="sxs-lookup"><span data-stu-id="22e6d-292">Set up deployment credentials</span></span>
<span data-ttu-id="22e6d-293">Győződjön meg arról, hogy futtatta a **AzureWebDemo** alkalmazás hozzon létre egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="22e6d-293">Make sure you have run the **AzureWebDemo** application to create a web app.</span></span> <span data-ttu-id="22e6d-294">Ezen a helyen lesz átvitelhez.</span><span class="sxs-lookup"><span data-stu-id="22e6d-294">You will transfer files to this location.</span></span>

1. <span data-ttu-id="22e6d-295">Jelentkezzen be a klasszikus portálra, és kattintson a **webalkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-295">Log into the classic portal and click **Web Apps**.</span></span> <span data-ttu-id="22e6d-296">Győződjön meg arról, hogy **WebDemoWebApp** jelenik meg a webes alkalmazások listájának, és győződjön meg arról, hogy fut-e.</span><span class="sxs-lookup"><span data-stu-id="22e6d-296">Make sure **WebDemoWebApp** appears in the list of web apps, and make sure that it is running.</span></span> <span data-ttu-id="22e6d-297">Kattintson a **WebDemoWebApp** megnyitásához a **irányítópult** lap.</span><span class="sxs-lookup"><span data-stu-id="22e6d-297">Click **WebDemoWebApp** to open its **Dashboard** page.</span></span>
2. <span data-ttu-id="22e6d-298">A a **irányítópult** lap **gyors áttekintése**, kattintson **az üzembe helyezési hitelesítő adatok beállítása** (Ha már rendelkezik üzembe helyezési hitelesítő adatokat, ez olvassa be  **A központi telepítési hitelesítő adatok alaphelyzetbe állítása**).</span><span class="sxs-lookup"><span data-stu-id="22e6d-298">On the **Dashboard** page, under **Quick Glance**, click **Set up your deployment credentials** (if you already have deployment credentials, this reads **Reset your deployment credentials**).</span></span>
   
    <span data-ttu-id="22e6d-299">Üzembe helyezési hitelesítő adatok társított Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="22e6d-299">Deployment credentials are associated with a Microsoft account.</span></span> <span data-ttu-id="22e6d-300">Meg kell adnia a felhasználónevet és jelszót, amely segítségével telepítheti a Git és az FTP használatával.</span><span class="sxs-lookup"><span data-stu-id="22e6d-300">You need to specify a username and password that you can use to deploy using Git and FTP.</span></span> <span data-ttu-id="22e6d-301">Ezek a hitelesítő adatok segítségével telepítése a Microsoft-fiókjához társított összes Azure-előfizetések bármely webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="22e6d-301">You can use these credentials to deploy to any web app in all Azure subscriptions associated with your Microsoft account.</span></span> <span data-ttu-id="22e6d-302">Adja meg a Git és az FTP telepítési hitelesítő adatok a párbeszédpanelen, majd jegyezze fel a felhasználónevet és jelszót későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="22e6d-302">Provide Git and FTP deployment credentials in the dialog, and record the username and password for future use.</span></span>

#### <a name="get-ftp-connection-information"></a><span data-ttu-id="22e6d-303">FTP-kiszolgáló kapcsolati adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="22e6d-303">Get FTP connection information</span></span>
<span data-ttu-id="22e6d-304">FTP központi telepítéséhez használandó alkalmazásfájlok az újonnan létrehozott webalkalmazáshoz, meg kell szereznie kapcsolati adatokat.</span><span class="sxs-lookup"><span data-stu-id="22e6d-304">To use FTP to deploy application files to the newly created web app, you need to obtain connection information.</span></span> <span data-ttu-id="22e6d-305">Két módon lehet kapcsolati információkhoz.</span><span class="sxs-lookup"><span data-stu-id="22e6d-305">There are two ways to obtain connection information.</span></span> <span data-ttu-id="22e6d-306">Egyik módja az, hogy keresse fel a web app **irányítópult** lapon; a más módon kell letölteni a webes alkalmazás közzétételi profil.</span><span class="sxs-lookup"><span data-stu-id="22e6d-306">One way is to visit the web app's **Dashboard** page; the other way is to download the web app's publish profile.</span></span> <span data-ttu-id="22e6d-307">A közzétételi profil XML-fájl, amely például FTP állomás nevét és a bejelentkezési hitelesítő adatokat biztosít a webalkalmazások Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="22e6d-307">The publish profile is an XML file that provides information such as FTP host name and logon credentials for your web apps in Azure App Service.</span></span> <span data-ttu-id="22e6d-308">Felhasználónév és jelszó használatával telepítheti az az Azure-fiókra, nem csak a másikat társított előfizetéseket bármely webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="22e6d-308">You can use this username and password to deploy to any web app in all subscriptions associated with the Azure account, not only this one.</span></span>

<span data-ttu-id="22e6d-309">A webalkalmazás panelen az FTP-kiszolgáló kapcsolati adatainak beszerzése a [Azure Portal][Azure Portal]:</span><span class="sxs-lookup"><span data-stu-id="22e6d-309">To obtain FTP connection information from the web app's blade in the [Azure Portal][Azure Portal]:</span></span>

1. <span data-ttu-id="22e6d-310">A **Essentials**, található, és másolja a **FTP-állomásnév**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-310">Under **Essentials**, find and copy the **FTP hostname**.</span></span> <span data-ttu-id="22e6d-311">Ez a hasonló URI `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-311">This is a URI similar to `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.</span></span>
2. <span data-ttu-id="22e6d-312">A **Essentials**, található, és másolja **FTP vagy üzembe helyező felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-312">Under **Essentials**, find and copy **FTP/Deployment username**.</span></span> <span data-ttu-id="22e6d-313">Ez lesz az űrlap *webappname\deployment-username*, például `WebDemoWebApp\deployer77`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-313">This will have the form *webappname\deployment-username*; for example `WebDemoWebApp\deployer77`.</span></span>

<span data-ttu-id="22e6d-314">Az FTP-kapcsolati adatok lekérését a közzétételi profil:</span><span class="sxs-lookup"><span data-stu-id="22e6d-314">To obtain FTP connection information from the publish profile:</span></span>

1. <span data-ttu-id="22e6d-315">A webalkalmazás panelen kattintson **Get közzétételi profil**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-315">In the web app's blade, click **Get publish profile**.</span></span> <span data-ttu-id="22e6d-316">A .publishsettings fájl letöltése a helyi meghajtóra.</span><span class="sxs-lookup"><span data-stu-id="22e6d-316">This will download a .publishsettings file to your local drive.</span></span>
2. <span data-ttu-id="22e6d-317">Nyissa meg a .publishsettings-fájlt egy XML-szerkesztőben vagy szövegszerkesztőben, és keresse a `<publishProfile>` elemet tartalmazó `publishMethod="FTP"`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-317">Open the .publishsettings file in an XML editor or text editor and find the `<publishProfile>` element containing `publishMethod="FTP"`.</span></span> <span data-ttu-id="22e6d-318">Például a következő formában:</span><span class="sxs-lookup"><span data-stu-id="22e6d-318">It should look like the following:</span></span>
   
        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>
3. <span data-ttu-id="22e6d-319">Vegye figyelembe, hogy a webalkalmazás `publishProfile` beállítások térkép FileZilla kezelő beállításai az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="22e6d-319">Note that the web app's `publishProfile` settings map to the FileZilla Site Manager settings as follows:</span></span>

* <span data-ttu-id="22e6d-320">`publishUrl`ugyanaz, mint **FTP-állomás neve**, az érték beállítása **állomás**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-320">`publishUrl` is the same as **FTP host name**, the value you set in **Host**.</span></span>
* <span data-ttu-id="22e6d-321">`publishMethod="FTP"`azt jelenti, hogy megadta **protokoll** való **FTP - File Transfer Protocol**, és **titkosítási** való **egyszerű FTP használata**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-321">`publishMethod="FTP"` means that you set **Protocol** to **FTP - File Transfer Protocol**, and **Encryption** to **Use plain FTP**.</span></span>
* <span data-ttu-id="22e6d-322">`userName`és `userPWD` kulcsok tényleges felhasználónév és jelszó értékeinek meg, ha alaphelyzetbe állítja az üzembe helyezési hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="22e6d-322">`userName` and `userPWD` are keys for the actual username and password values you specified when you reset the deployment credentials.</span></span> <span data-ttu-id="22e6d-323">`userName`ugyanaz, mint **telepítési / FTP-felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-323">`userName` is the same as **Deployment / FTP user**.</span></span> <span data-ttu-id="22e6d-324">A leképezik **felhasználói** és **jelszó** a FileZilla.</span><span class="sxs-lookup"><span data-stu-id="22e6d-324">They map to **User** and **Password** in FileZilla.</span></span>
* <span data-ttu-id="22e6d-325">`ftpPassiveMode="True"`azt jelenti, hogy az FTP-hely használja-e a passzív FTP-átvitel; Válassza ki **passzív** a a **átvitel beállításainak** fülre.</span><span class="sxs-lookup"><span data-stu-id="22e6d-325">`ftpPassiveMode="True"` means that the FTP site uses passive FTP transfer; select **Passive** on the **Transfer Settings** tab.</span></span>

#### <a name="configure-the-web-app-to-host-a-java-application"></a><span data-ttu-id="22e6d-326">A webalkalmazás üzemeltetéséhez Java-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="22e6d-326">Configure the Web App to host a Java application</span></span>
<span data-ttu-id="22e6d-327">Az alkalmazás közzététele előtt kell néhány konfigurációs beállításokat módosítaná, így a web app, Java-alkalmazások rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="22e6d-327">Before you publish the application, you need to change a few configuration settings so that the web app can host a Java application.</span></span>

1. <span data-ttu-id="22e6d-328">A klasszikus portálon lépjen a webes alkalmazás **irányítópult** lapot, és kattintson **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-328">In the classic portal, go to the web app's **Dashboard** page and click **Configure**.</span></span> <span data-ttu-id="22e6d-329">Az a **konfigurálása** lapján adja meg a következő beállításokat.</span><span class="sxs-lookup"><span data-stu-id="22e6d-329">On the **Configure** page, specify the following settings.</span></span>
2. <span data-ttu-id="22e6d-330">A **Java-verziót** az alapértelmezett érték **ki**; válassza ki a Java verzióját a alkalmazás célkitűzések, például a 1.7.0_51.</span><span class="sxs-lookup"><span data-stu-id="22e6d-330">In **Java version** the default is **Off**; select the Java version your application targets; for example 1.7.0_51.</span></span> <span data-ttu-id="22e6d-331">Ezt követően ellenőrizze azt is, amely **webes tároló** Tomcat Server verzióra van beállítva.</span><span class="sxs-lookup"><span data-stu-id="22e6d-331">After you do this, also make sure that **Web container** is set to a version of Tomcat Server.</span></span>
3. <span data-ttu-id="22e6d-332">A **alapértelmezett dokumentumok**, adja hozzá az index.jsp, és akár a lista elejére helyezze.</span><span class="sxs-lookup"><span data-stu-id="22e6d-332">In **Default Documents**, add index.jsp and move it up to the top of the list.</span></span> <span data-ttu-id="22e6d-333">(Az alapértelmezett webes alkalmazások fájlja hostingstart.html.)</span><span class="sxs-lookup"><span data-stu-id="22e6d-333">(The default file for web apps is hostingstart.html.)</span></span>
4. <span data-ttu-id="22e6d-334">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="22e6d-334">Click **Save**.</span></span>

#### <a name="publish-your-application-using-kudu"></a><span data-ttu-id="22e6d-335">A Kudu használó alkalmazások közzététele</span><span class="sxs-lookup"><span data-stu-id="22e6d-335">Publish your application using Kudu</span></span>
<span data-ttu-id="22e6d-336">Egy tegye közzé az alkalmazást módja az Azure beépített Kudu hibakereső konzolt használja.</span><span class="sxs-lookup"><span data-stu-id="22e6d-336">One way to publish the application is to use the Kudu debug console built into Azure.</span></span> <span data-ttu-id="22e6d-337">A kudu ismert, hogy stabil és konzisztens legyen az App Service Web Apps és Tomcat kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="22e6d-337">Kudu is known to be stable and consistent with App Service Web Apps and Tomcat Server.</span></span> <span data-ttu-id="22e6d-338">A webalkalmazás a konzol érhető próbálja elérni az egy URL-cím a következő:</span><span class="sxs-lookup"><span data-stu-id="22e6d-338">You access the console for the web app by browsing to a URL of the following form:</span></span>

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. <span data-ttu-id="22e6d-339">Az eljárás végrehajtásához a Kudu konzol itt található: a következő URL-címet; Keresse meg a következő helyről:</span><span class="sxs-lookup"><span data-stu-id="22e6d-339">For this procedure, the Kudu console is located at the following URL; browse to this location:</span></span>
   
    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`
2. <span data-ttu-id="22e6d-340">A felső menüben válassza ki a **konzol Debug > CMD**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-340">From the top menu, select **Debug Console > CMD**.</span></span>
3. <span data-ttu-id="22e6d-341">A konzol parancssorban navigáljon `/site/wwwroot` (vagy kattintson a `site`, majd `wwwroot` a könyvtár nézetben a lap tetején):</span><span class="sxs-lookup"><span data-stu-id="22e6d-341">In the console command line, navigate to `/site/wwwroot` (or click `site`, then `wwwroot` in the directory view at the top of the page):</span></span>
   
    `cd /site/wwwroot`
4. <span data-ttu-id="22e6d-342">Miután megadta **Java-verziót**, Tomcat kiszolgálót kell létrehoznia a webapps könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="22e6d-342">After you specify **Java version**, Tomcat server should create a webapps directory.</span></span> <span data-ttu-id="22e6d-343">A konzol parancssorban keresse meg a webapps könyvtárba:</span><span class="sxs-lookup"><span data-stu-id="22e6d-343">In the console command line, navigate to the webapps directory:</span></span>
   
    `mkdir webapps`
   
    `cd webapps`
5. <span data-ttu-id="22e6d-344">Húzza a JSPHello.war `<project-path>/JSPHello/src/` , és helyezze be a Kudu könyvtár nézetben a `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-344">Drag JSPHello.war from `<project-path>/JSPHello/src/` and drop it into the Kudu directory view under `/site/wwwroot/webapps`.</span></span> <span data-ttu-id="22e6d-345">Nem húzza a "Húzza ide a zip- és feltöltése" területet, mert a Tomcat fog csomagolja ki.</span><span class="sxs-lookup"><span data-stu-id="22e6d-345">Do not drag it to the "Drag here to upload and zip" area, because Tomcat will unzip it.</span></span>
   
   ![][8]

<span data-ttu-id="22e6d-346">Első JSPHello.war, megjelenik a könyvtár munkaterület önmagában:</span><span class="sxs-lookup"><span data-stu-id="22e6d-346">At first JSPHello.war appears in the directory area by itself:</span></span>

  ![][9]

<span data-ttu-id="22e6d-347">Rövid időn belül (valószínűleg 5 percnél kevesebb) Tomcat kiszolgálót fog csomagolja ki a WAR-fájlt egy kicsomagolt JSPHello könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="22e6d-347">In a short time (probably less than 5 minutes) Tomcat Server will unzip the WAR file into an unpacked JSPHello directory.</span></span> <span data-ttu-id="22e6d-348">Kattintson a gyökérkönyvtár, hogy index.jsp unzipped és másolásához.</span><span class="sxs-lookup"><span data-stu-id="22e6d-348">Click the ROOT directory to see whether index.jsp has been unzipped and copied there.</span></span> <span data-ttu-id="22e6d-349">Ha igen, lépjen vissza a webapps könyvtárba, megtekintéséhez, hogy a kibontott JSPHello directory készült.</span><span class="sxs-lookup"><span data-stu-id="22e6d-349">If so, navigate back to the webapps directory to see whether the unpacked JSPHello directory has been created.</span></span> <span data-ttu-id="22e6d-350">Ha nem látja ezeket az elemeket, várja meg, és ismételje meg.</span><span class="sxs-lookup"><span data-stu-id="22e6d-350">If you do not see these items, wait and repeat.</span></span>

  ![][10]

#### <a name="publish-your-application-using-filezilla-optional"></a><span data-ttu-id="22e6d-351">A FileZilla (nem kötelező) használó alkalmazások közzététele</span><span class="sxs-lookup"><span data-stu-id="22e6d-351">Publish your application using FileZilla (optional)</span></span>
<span data-ttu-id="22e6d-352">Egy másik eszköz segítségével teheti közzé az alkalmazást az FileZilla, a népszerű külső FTP-ügyfél kényelmesen, grafikus felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="22e6d-352">Another tool you can use to publish the application is FileZilla, a popular third-party FTP client with a convenient, graphical UI.</span></span> <span data-ttu-id="22e6d-353">Töltse le, és telepítse a FileZilla [http://filezilla-project.org/](http://filezilla-project.org/) Ha még nem rendelkezik azt.</span><span class="sxs-lookup"><span data-stu-id="22e6d-353">You can download and install FileZilla from [http://filezilla-project.org/](http://filezilla-project.org/) if you do not already have it.</span></span> <span data-ttu-id="22e6d-354">Az ügyfél használatáról további információkért lásd: a [FileZilla dokumentáció](https://wiki.filezilla-project.org/Documentation) és ezt a blogbejegyzést [FTP-ügyfelek - rész 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).</span><span class="sxs-lookup"><span data-stu-id="22e6d-354">For more information on using the client, see the [FileZilla documentation](https://wiki.filezilla-project.org/Documentation) and this blog entry on [FTP Clients - Part 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).</span></span>

1. <span data-ttu-id="22e6d-355">Kattintson a FileZilla, **fájl > kezelő**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-355">In FileZilla, click **File > Site Manager**.</span></span>
2. <span data-ttu-id="22e6d-356">Az a **kezelő** párbeszédpanel, kattintson a **új hely**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-356">In the **Site Manager** dialog, click **New Site**.</span></span> <span data-ttu-id="22e6d-357">Megjelenik egy új, üres FTP-hely **válasszon bejegyzés** kéri, adjon meg egy nevet.</span><span class="sxs-lookup"><span data-stu-id="22e6d-357">A new blank FTP site will appear in **Select Entry** prompting you to provide a name.</span></span> <span data-ttu-id="22e6d-358">Az alábbi eljárással nevezze el `AzureWebDemo-FTP`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-358">For this procedure, name it `AzureWebDemo-FTP`.</span></span>
   
    <span data-ttu-id="22e6d-359">Az a **általános** lapra, adja meg a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="22e6d-359">On the **General** tab, specify the following settings:</span></span>
   
   * <span data-ttu-id="22e6d-360">**Állomás:** adja meg a **FTP-állomás neve** az irányítópultról másolt.</span><span class="sxs-lookup"><span data-stu-id="22e6d-360">**Host:** Enter the **FTP Host Name** that you copied from the dashboard.</span></span>
   * <span data-ttu-id="22e6d-361">**Port:** (ezt üresen hagyja, mert ez egy passzív átviteli, és a kiszolgáló használni kívánt portot határozza meg.)</span><span class="sxs-lookup"><span data-stu-id="22e6d-361">**Port:** (Leave this blank, as this is a passive transfer and the server will determine the port to use.)</span></span>
   * <span data-ttu-id="22e6d-362">**Protokoll:** FTP fájlátviteli protokoll</span><span class="sxs-lookup"><span data-stu-id="22e6d-362">**Protocol:** FTP File Transfer Protocol</span></span>
   * <span data-ttu-id="22e6d-363">**Titkosítás:** egyszerű FTP használata</span><span class="sxs-lookup"><span data-stu-id="22e6d-363">**Encryption:** Use plain FTP</span></span>
   * <span data-ttu-id="22e6d-364">**Bejelentkezési típusa:** normál</span><span class="sxs-lookup"><span data-stu-id="22e6d-364">**Logon Type:** Normal</span></span>
   * <span data-ttu-id="22e6d-365">**Felhasználó:** adja meg a központi telepítés / FTP-felhasználó, az irányítópultról másolt.</span><span class="sxs-lookup"><span data-stu-id="22e6d-365">**User:** Enter the Deployment / FTP user that you copied from the dashboard.</span></span> <span data-ttu-id="22e6d-366">Ez a teljes FTP felhasználónév, amely az űrlap *webappname\username*.</span><span class="sxs-lookup"><span data-stu-id="22e6d-366">This is the full FTP username, which has the form *webappname\username*.</span></span>
   * <span data-ttu-id="22e6d-367">**Jelszó:** adja meg, hogy a megadott az üzembe helyezési hitelesítő adatok megadása után a jelszót.</span><span class="sxs-lookup"><span data-stu-id="22e6d-367">**Password:** Enter the password that you specified when you set the deployment credentials.</span></span>
     
     <span data-ttu-id="22e6d-368">Az a **átvitel beállításainak** lapon jelölje be **passzív**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-368">On the **Transfer Settings** tab, select **Passive**.</span></span>
3. <span data-ttu-id="22e6d-369">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="22e6d-369">Click **Connect**.</span></span> <span data-ttu-id="22e6d-370">Ha sikeres, FileZilla tartozó konzol megjeleníti a `Status: Connected` üzenet és a probléma egy `LIST` paranccsal listát készíthet a könyvtár tartalma.</span><span class="sxs-lookup"><span data-stu-id="22e6d-370">If successful, FileZilla's console will display a `Status: Connected` message and issue a `LIST` command to list the directory contents.</span></span>
4. <span data-ttu-id="22e6d-371">Az a **helyi** hely panelen, jelölje ki a forráskönyvtár a JSPHello.war fájl található; az elérési út az alábbihoz hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="22e6d-371">In the **Local** site panel, select the source directory in which the JSPHello.war file resides; the path will be similar to the following:</span></span>
   
    `<project-path>/JSPHello/src/`
5. <span data-ttu-id="22e6d-372">Az a **távoli** hely panelen, jelölje ki a célmappát.</span><span class="sxs-lookup"><span data-stu-id="22e6d-372">In the **Remote** site panel, select the destination folder.</span></span> <span data-ttu-id="22e6d-373">Ha telepíti a WAR-fájlt a `webapps` könyvtárhoz, a webes alkalmazás legfelső szintű.</span><span class="sxs-lookup"><span data-stu-id="22e6d-373">You will deploy the WAR file to the `webapps` directory under the web app's root.</span></span> <span data-ttu-id="22e6d-374">Navigáljon a `/site/wwwroot`, kattintson a jobb gombbal a `wwwroot`, és válassza ki **könyvtár létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-374">Navigate to `/site/wwwroot`, right-click on `wwwroot`, and select **Create directory**.</span></span> <span data-ttu-id="22e6d-375">A könyvtár nevet `webapps` , és írja be a könyvtárhoz.</span><span class="sxs-lookup"><span data-stu-id="22e6d-375">Name the directory `webapps` and enter that directory.</span></span>
6. <span data-ttu-id="22e6d-376">A JSPHello.war átviteli `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-376">Transfer JSPHello.war to `/site/wwwroot/webapps`.</span></span> <span data-ttu-id="22e6d-377">Válassza ki a JSPHello.war a **helyi** lista fájlt, kattintson a jobb gombbal a, és válassza ki **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="22e6d-377">Select JSPHello.war in the **Local** file list, right-click on it and select **Upload**.</span></span> <span data-ttu-id="22e6d-378">Akkor jelenik meg kell megjelennie `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="22e6d-378">You should see it appear in `/site/wwwroot/webapps`.</span></span>
7. <span data-ttu-id="22e6d-379">Miután JSPHello.war másolta a webapps könyvtárba, Tomcat kiszolgálót automatikusan csomagolja ki (csomagolja ki) a fájlokat a WAR-fájlt.</span><span class="sxs-lookup"><span data-stu-id="22e6d-379">After you copy JSPHello.war to the webapps directory, Tomcat Server will automatically unpack (unzip) the files in the WAR file.</span></span> <span data-ttu-id="22e6d-380">Bár Tomcat kiszolgálót megkezdése kicsomagolása szinte azonnal is telhet, mire hosszú idő (valószínűleg óra) a fájlok megjelennek az FTP-ügyfél számára.</span><span class="sxs-lookup"><span data-stu-id="22e6d-380">Although Tomcat Server begins unpacking almost immediately, it might take a long time (possibly hours) for the files to appear in the FTP client.</span></span>

#### <a name="run-the-hello-world-application-on-the-web-app"></a><span data-ttu-id="22e6d-381">A webalkalmazás a Hello World az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="22e6d-381">Run the Hello World application on the Web App</span></span>
1. <span data-ttu-id="22e6d-382">Miután a WAR-fájl feltöltése, és ellenőrizte a Tomcat kiszolgálón egy kicsomagolt létrehozott `JSPHello` könyvtár, tallózással keresse meg a `http://webdemowebapp.azurewebsites.net/JSPHello` az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="22e6d-382">After you have uploaded the WAR file and verified that Tomcat server has created an unpacked `JSPHello` directory, browse to `http://webdemowebapp.azurewebsites.net/JSPHello` to run the application.</span></span>
   
   > <span data-ttu-id="22e6d-383">**Megjegyzés:** választva **Tallózás** a klasszikus portálon kaphat az alapértelmezett weblap közli, hogy "a Java-alapú webalkalmazás létrehozása sikeresen befejeződött."</span><span class="sxs-lookup"><span data-stu-id="22e6d-383">**Note:** If you click **Browse** from the classic portal, you might get the default webpage, saying "This Java based web application has been successfully created."</span></span> <span data-ttu-id="22e6d-384">Lehetséges, hogy az alkalmazás kimenete helyett az alapértelmezett weblap megtekintéséhez a képernyőn látható weblapon frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="22e6d-384">You might have to refresh the webpage in order to view the application output instead of the default webpage.</span></span>
   > 
   > 
2. <span data-ttu-id="22e6d-385">Az alkalmazás futásakor, megjelenik egy weblap, a következő eredménnyel:</span><span class="sxs-lookup"><span data-stu-id="22e6d-385">When the application runs, you should see a web page with the following output:</span></span>
   
    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="clean-up-azure-resources"></a><span data-ttu-id="22e6d-386">Azure-erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="22e6d-386">Clean up Azure resources</span></span>
<span data-ttu-id="22e6d-387">Ez az eljárás egy App Service webalkalmazásba hoz.</span><span class="sxs-lookup"><span data-stu-id="22e6d-387">This procedure creates an App Service web app.</span></span> <span data-ttu-id="22e6d-388">A számlázás történik az erőforrás mindaddig, amíg az létezik.</span><span class="sxs-lookup"><span data-stu-id="22e6d-388">You will be billed for the resource as long as it exists.</span></span> <span data-ttu-id="22e6d-389">Ha szeretné folytatni a web app használatával tesztelési, illetve a fejlesztési, érdemes lehet leállítása vagy törlése.</span><span class="sxs-lookup"><span data-stu-id="22e6d-389">Unless you plan to continue using the web app for testing or development, you should consider stopping or deleting it.</span></span> <span data-ttu-id="22e6d-390">A webes alkalmazás, amely le lett állítva továbbra is fel Önnek egy kis kell fizetni, de bármikor újraindíthatja azt.</span><span class="sxs-lookup"><span data-stu-id="22e6d-390">A web app that has been stopped will still incur a small charge, but you can restart it at any time.</span></span> <span data-ttu-id="22e6d-391">A webes alkalmazás törlésével feltöltött az összes adatot törli.</span><span class="sxs-lookup"><span data-stu-id="22e6d-391">Deleting a web app erases all data you have uploaded to it.</span></span>

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
