---
title: "aaaExport hello Azure Cosmos DB emulátor tanúsítványok |} Microsoft Docs"
description: "A nyelvet és a futtatókörnyezetek hello Windows tanúsítványtároló nem használó fejlesztésekor tooexport kell, és hello SSL-tanúsítványok kezelése. A post lépésenkénti utasításokat biztosít."
services: cosmos-db
documentationcenter: 
keywords: "Az Azure Cosmos DB emulátor"
author: voellm
manager: jhubbard
editor: 
ms.assetid: ef43deda-c2e9-4193-99e2-7f6a88a0319f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: tvoellm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: db56cda856fccf93d71ae5b21c4090ccb9aa40a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="export-hello-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a><span data-ttu-id="2d61d-105">A Node.js, Java és Python használt hello Azure Cosmos DB emulátor tanúsítványok exportálása</span><span class="sxs-lookup"><span data-stu-id="2d61d-105">Export hello Azure Cosmos DB Emulator certificates for use with Java, Python, and Node.js</span></span>

[<span data-ttu-id="2d61d-106">**Hello emulátor letöltése**</span><span class="sxs-lookup"><span data-stu-id="2d61d-106">**Download hello Emulator**</span></span>](https://aka.ms/cosmosdb-emulator)

<span data-ttu-id="2d61d-107">hello Azure Cosmos DB emulátor hello Azure Cosmos DB szolgáltatás fejlesztése céljából, beleértve az SSL-kapcsolatok használatát a emuláló helyi környezetet biztosít.</span><span class="sxs-lookup"><span data-stu-id="2d61d-107">hello Azure Cosmos DB Emulator provides a local environment that emulates hello Azure Cosmos DB service for development purposes including its use of SSL connections.</span></span> <span data-ttu-id="2d61d-108">A post bemutatja, hogyan tooexport hello SSL-tanúsítványok a nyelv és a futtatókörnyezetek integrálódó nem hello Windows tanúsítványtároló például Java, amely használja a saját [tanúsítványtároló](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) és Python használó[burkolók szoftvercsatorna](https://docs.python.org/2/library/ssl.html) és a Node.js használó [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span><span class="sxs-lookup"><span data-stu-id="2d61d-108">This post demonstrates how tooexport hello SSL certificates for use in languages and runtimes that do not integrate with hello Windows Certificate Store such as Java which uses its own [certificate store](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) and Python which uses [socket wrappers](https://docs.python.org/2/library/ssl.html) and Node.js which uses [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span></span> <span data-ttu-id="2d61d-109">További kapcsolatos hello emulátorának [használata hello Azure Cosmos DB emulátor a fejlesztéshez és teszteléshez](./local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="2d61d-109">You can read more about hello emulator in [Use hello Azure Cosmos DB Emulator for development and testing](./local-emulator.md).</span></span>

<span data-ttu-id="2d61d-110">Ez az oktatóanyag ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="2d61d-110">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2d61d-111">Tanúsítványok elforgatása</span><span class="sxs-lookup"><span data-stu-id="2d61d-111">Rotating certificates</span></span>
> * <span data-ttu-id="2d61d-112">SSL-tanúsítvány exportálása</span><span class="sxs-lookup"><span data-stu-id="2d61d-112">Exporting SSL certificate</span></span>
> * <span data-ttu-id="2d61d-113">Tanulási hogyan toouse hello Node.js, Java és Python-tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="2d61d-113">Learning how toouse hello certificate in Java, Python, and Node.js</span></span>

## <a name="certification-rotation"></a><span data-ttu-id="2d61d-114">Hitelesítésszolgáltató Elforgatás</span><span class="sxs-lookup"><span data-stu-id="2d61d-114">Certification rotation</span></span>

<span data-ttu-id="2d61d-115">Tanúsítványok hello Azure Cosmos DB helyi emulátor vannak hello hello emulátor első futtatásakor jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="2d61d-115">Certificates in hello Azure Cosmos DB Local Emulator are generated hello first time hello emulator is run.</span></span> <span data-ttu-id="2d61d-116">Két tanúsítványokat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2d61d-116">There are two certificates.</span></span> <span data-ttu-id="2d61d-117">Egy csatlakozáshoz toohello helyi emulátor és hello emulátor belül titkos kulcsok kezelése céljából.</span><span class="sxs-lookup"><span data-stu-id="2d61d-117">One used for connecting toohello local emulator and one for managing secrets within hello emulator.</span></span> <span data-ttu-id="2d61d-118">hello tanúsítványt tooexport hello rövid névvel "DocumentDBEmulatorCertificate" hello kapcsolati tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="2d61d-118">hello certificate you want tooexport is hello connection certificate with hello friendly name "DocumentDBEmulatorCertificate".</span></span>

<span data-ttu-id="2d61d-119">Mindkét tanúsítványnak helyreállíthatja kattintva **adatok alaphelyzetbe állítása** Azure Cosmos DB-emulátort a Windows tálca hello az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="2d61d-119">Both certificates can be regenerated by clicking **Reset Data** as shown below from Azure Cosmos DB Emulator running in hello Windows Tray.</span></span> <span data-ttu-id="2d61d-120">Ha újragenerálni hello tanúsítványokat, és telepítette őket hello Java tanúsítványtárolóba vagy máshol használták őket tooupdate kell őket, ellenkező esetben az alkalmazás már nem kapcsolódik toohello helyi emulátor.</span><span class="sxs-lookup"><span data-stu-id="2d61d-120">If you regenerate hello certificates and have installed them into hello Java certificate store or used them elsewhere you will need tooupdate them, otherwise your application will no longer connect toohello local emulator.</span></span>

![Az Azure Cosmos DB helyi emulátor adatok visszaállítása](./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png)

## <a name="how-tooexport-hello-azure-cosmos-db-ssl-certificate"></a><span data-ttu-id="2d61d-122">Hogyan tooexport hello Azure Cosmos DB SSL-tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="2d61d-122">How tooexport hello Azure Cosmos DB SSL certificate</span></span>

1. <span data-ttu-id="2d61d-123">Indítsa el a Windows tanúsítványkezelő hello certlm.msc futtatásával, és keresse meg személyes toohello -> tanúsítványok mappa és a nyitott hello tanúsítvány hello rövid névvel **DocumentDbEmulatorCertificate**.</span><span class="sxs-lookup"><span data-stu-id="2d61d-123">Start hello Windows Certificate manager by running certlm.msc and navigate toohello Personal->Certificates folder and open hello certificate with hello friendly name **DocumentDbEmulatorCertificate**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 1. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png)

2. <span data-ttu-id="2d61d-125">Kattintson a **részletek** majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d61d-125">Click on **Details** then **OK**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 2. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png)

3. <span data-ttu-id="2d61d-127">Kattintson a **tooFile másolja...** .</span><span class="sxs-lookup"><span data-stu-id="2d61d-127">Click **Copy tooFile...**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 3. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png)

4. <span data-ttu-id="2d61d-129">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="2d61d-129">Click **Next**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 4. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png)

5. <span data-ttu-id="2d61d-131">Kattintson a **nem, nem exportálja a titkos kulcs**, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="2d61d-131">Click **No, do not export private key**, then click **Next**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 5. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png)

6. <span data-ttu-id="2d61d-133">Kattintson a **Base-64 kódolású X.509 (. CER)** , majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="2d61d-133">Click on **Base-64 encoded X.509 (.CER)** and then **Next**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 6. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png)

7. <span data-ttu-id="2d61d-135">Adjon meg egy nevet, amely hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="2d61d-135">Give hello certificate a name.</span></span> <span data-ttu-id="2d61d-136">Ebben az esetben **documentdbemulatorcert** majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="2d61d-136">In this case **documentdbemulatorcert** and then click **Next**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 7. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png)

8. <span data-ttu-id="2d61d-138">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="2d61d-138">Click **Finish**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálja a 8. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png)

## <a name="how-toouse-hello-certificate-in-java"></a><span data-ttu-id="2d61d-140">Hogyan toouse hello Java-tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="2d61d-140">How toouse hello certificate in Java</span></span>

<span data-ttu-id="2d61d-141">Java-alkalmazások vagy hello Java ügyfelet használó alkalmazások MongoDB futtatásakor tanúsítványról szó könnyebb tooinstall hello hello Java alapértelmezett tanúsítványtárolóba sikeres hello mint "-Djavax.net.ssl.trustStore=<keystore> - Djavax.net.ssl.trustStorePassword= "<password>" jelzők.</span><span class="sxs-lookup"><span data-stu-id="2d61d-141">When running Java applications or MongoDB applications that use hello Java client it is easier tooinstall hello certificate into hello Java default certificate store than passing hello "-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword="<password>" flags.</span></span> <span data-ttu-id="2d61d-142">Például szereplő hello [Java bemutató alkalmazás](https://localhost:8081/_explorer/index.html) hello alapértelmezett számítógéptanúsítvány-tároló függ.</span><span class="sxs-lookup"><span data-stu-id="2d61d-142">For example hello included [Java Demo application](https://localhost:8081/_explorer/index.html) depends on hello default certificate store.</span></span>

<span data-ttu-id="2d61d-143">Hello hello utasításait követve [hozzáadása a Java-hitelesítésszolgáltató tanúsítványtárolójában tanúsítvány toohello](https://docs.microsoft.com/azure/java-add-certificate-ca-store) tooimport hello X.509 tanúsítvány hello alapértelmezett Java tanúsítványtárolójába.</span><span class="sxs-lookup"><span data-stu-id="2d61d-143">Follow hello instructions in hello [Adding a Certificate toohello Java CA Certificates Store](https://docs.microsoft.com/azure/java-add-certificate-ca-store) tooimport hello X.509 certificate into hello default Java certificate store.</span></span> <span data-ttu-id="2d61d-144">Ne mind kívánja dolgozni fog hello % JAVA_HOME % könyvtárban kulcseszköz futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="2d61d-144">Keep in mind you will be working in hello %JAVA_HOME% directory when running keytool.</span></span>

<span data-ttu-id="2d61d-145">Egyszer hello "CosmosDBEmulatorCertificate" SSL-tanúsítvány telepítve van az alkalmazás képes tooconnect kell lennie, és használja a helyi Azure Cosmos DB emulátor hello.</span><span class="sxs-lookup"><span data-stu-id="2d61d-145">Once hello "CosmosDBEmulatorCertificate" SSL certificate is installed your application should be able tooconnect and use hello local Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="2d61d-146">Ha hiba történt a toohave továbbra is érdemes lehet a toofollow hello [hibakeresés SSL/TLS kapcsolatok](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) cikk.</span><span class="sxs-lookup"><span data-stu-id="2d61d-146">If you continue toohave trouble you may want toofollow hello [Debugging SSL/TLS Connections](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) article.</span></span> <span data-ttu-id="2d61d-147">Nagyon valószínű hello tanúsítványa nincs telepítve hello %JAVA_HOME%/jre/lib/security/cacerts tárolóba.</span><span class="sxs-lookup"><span data-stu-id="2d61d-147">It is very likely hello certificate is not installed into hello %JAVA_HOME%/jre/lib/security/cacerts store.</span></span> <span data-ttu-id="2d61d-148">Az eset például, ha az alkalmazás Java több telepített verzióinak lehet, hogy használja egy másik cacerts tárolóban, mint egy frissített hello.</span><span class="sxs-lookup"><span data-stu-id="2d61d-148">For example if you have multiple installed versions of Java your application may be using a different cacerts store than hello one you updated.</span></span>

## <a name="how-toouse-hello-certificate-in-python"></a><span data-ttu-id="2d61d-149">Hogyan toouse hello Python-tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="2d61d-149">How toouse hello certificate in Python</span></span>

<span data-ttu-id="2d61d-150">Alapértelmezett hello által [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) hello a DocumentDB API nem próbálja és toohello helyi emulátor kapcsolódáskor hello SSL-tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="2d61d-150">By default hello [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) for hello DocumentDB API will not try and use hello SSL certificate when connecting toohello local emulator.</span></span> <span data-ttu-id="2d61d-151">Ha azonban azt szeretné, hogy toouse SSL érvényesítési kövesse a hello hello példák [Python szoftvercsatorna burkolók](https://docs.python.org/2/library/ssl.html) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="2d61d-151">If however you want toouse SSL validation you can follow hello examples in hello [Python socket wrappers](https://docs.python.org/2/library/ssl.html) documentation.</span></span>

## <a name="how-toouse-hello-certificate-in-nodejs"></a><span data-ttu-id="2d61d-152">Hogyan toouse hello Node.js-tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="2d61d-152">How toouse hello certificate in Node.js</span></span>

<span data-ttu-id="2d61d-153">Alapértelmezett hello által [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) hello a DocumentDB API nem próbálja és toohello helyi emulátor kapcsolódáskor hello SSL-tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="2d61d-153">By default hello [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) for hello DocumentDB API will not try and use hello SSL certificate when connecting toohello local emulator.</span></span> <span data-ttu-id="2d61d-154">Ha azonban azt szeretné, hogy toouse SSL érvényesítési kövesse a hello hello példák [Node.js dokumentáció](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span><span class="sxs-lookup"><span data-stu-id="2d61d-154">If however you want toouse SSL validation you can follow hello examples in hello [Node.js documentation](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d61d-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2d61d-155">Next steps</span></span>

<span data-ttu-id="2d61d-156">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="2d61d-156">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2d61d-157">Elforgatott tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="2d61d-157">Rotated certificates</span></span>
> * <span data-ttu-id="2d61d-158">SSL-tanúsítvány exportált hello</span><span class="sxs-lookup"><span data-stu-id="2d61d-158">Exported hello SSL certificate</span></span>
> * <span data-ttu-id="2d61d-159">Megtudta, hogyan toouse hello a Java, Python, Node.js tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="2d61d-159">Learned how toouse hello certificate in Java, Python and Node.js</span></span>

<span data-ttu-id="2d61d-160">Most már folytathatja toohello fogalmak szakasz Cosmos DB további információt.</span><span class="sxs-lookup"><span data-stu-id="2d61d-160">You can now proceed toohello Concepts section for more information about Cosmos DB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2d61d-161">Globális terjesztés</span><span class="sxs-lookup"><span data-stu-id="2d61d-161">Global distribution</span></span>](distribute-data-globally.md) 
