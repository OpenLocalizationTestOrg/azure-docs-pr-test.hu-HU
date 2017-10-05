---
title: "Az Azure Cosmos DB emulátor tanúsítványok exportálása |} Microsoft Docs"
description: "A nyelv és a Windows tanúsítványtároló nem használó futtatókörnyezetek fejlesztése során szüksége lesz exportálni, és az SSL-tanúsítványok kezelését. A post lépésenkénti utasításokat biztosít."
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
ms.openlocfilehash: 4add5028d50972316902cecd8c399781c012cb77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="export-the-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a><span data-ttu-id="482e0-105">A Node.js, Java és Python való használatra Azure Cosmos DB emulátor tanúsítványok exportálása</span><span class="sxs-lookup"><span data-stu-id="482e0-105">Export the Azure Cosmos DB Emulator certificates for use with Java, Python, and Node.js</span></span>

[<span data-ttu-id="482e0-106">**Töltse le az Emulátorban**</span><span class="sxs-lookup"><span data-stu-id="482e0-106">**Download the Emulator**</span></span>](https://aka.ms/cosmosdb-emulator)

<span data-ttu-id="482e0-107">Az Azure Cosmos DB Emulator emulálja a fejlesztéshez, beleértve az SSL-kapcsolatok használatát a Azure Cosmos DB szolgáltatás, helyi környezetet biztosít.</span><span class="sxs-lookup"><span data-stu-id="482e0-107">The Azure Cosmos DB Emulator provides a local environment that emulates the Azure Cosmos DB service for development purposes including its use of SSL connections.</span></span> <span data-ttu-id="482e0-108">A post bemutatja, hogyan kell exportálni a nyelvet és a futtatókörnyezetek, amely integrálható a Windows tanúsítványtárolóban, például a Java, amely használja a saját SSL-tanúsítványok [tanúsítványtároló](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) és Python használó[burkolók szoftvercsatorna](https://docs.python.org/2/library/ssl.html) és a Node.js használó [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span><span class="sxs-lookup"><span data-stu-id="482e0-108">This post demonstrates how to export the SSL certificates for use in languages and runtimes that do not integrate with the Windows Certificate Store such as Java which uses its own [certificate store](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) and Python which uses [socket wrappers](https://docs.python.org/2/library/ssl.html) and Node.js which uses [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span></span> <span data-ttu-id="482e0-109">További tudnivalók az emulátor [fejlesztéshez és teszteléshez használja az Azure Cosmos DB Emulator](./local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="482e0-109">You can read more about the emulator in [Use the Azure Cosmos DB Emulator for development and testing](./local-emulator.md).</span></span>

<span data-ttu-id="482e0-110">Ez az oktatóanyag ismerteti a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="482e0-110">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="482e0-111">Tanúsítványok elforgatása</span><span class="sxs-lookup"><span data-stu-id="482e0-111">Rotating certificates</span></span>
> * <span data-ttu-id="482e0-112">SSL-tanúsítvány exportálása</span><span class="sxs-lookup"><span data-stu-id="482e0-112">Exporting SSL certificate</span></span>
> * <span data-ttu-id="482e0-113">A Node.js, Java és Python tanúsítvány használata</span><span class="sxs-lookup"><span data-stu-id="482e0-113">Learning how to use the certificate in Java, Python, and Node.js</span></span>

## <a name="certification-rotation"></a><span data-ttu-id="482e0-114">Hitelesítésszolgáltató Elforgatás</span><span class="sxs-lookup"><span data-stu-id="482e0-114">Certification rotation</span></span>

<span data-ttu-id="482e0-115">Az Azure Cosmos DB helyi emulátorban tanúsítványok jönnek létre az emulátor első futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="482e0-115">Certificates in the Azure Cosmos DB Local Emulator are generated the first time the emulator is run.</span></span> <span data-ttu-id="482e0-116">Két tanúsítványokat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="482e0-116">There are two certificates.</span></span> <span data-ttu-id="482e0-117">Egy, a másik az emulátor belül titkos kulcsok kezelése a helyi emulátor való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="482e0-117">One used for connecting to the local emulator and one for managing secrets within the emulator.</span></span> <span data-ttu-id="482e0-118">Az exportálni kívánt tanúsítvány a kapcsolati tanúsítványt ezzel a rövid névvel "DocumentDBEmulatorCertificate".</span><span class="sxs-lookup"><span data-stu-id="482e0-118">The certificate you want to export is the connection certificate with the friendly name "DocumentDBEmulatorCertificate".</span></span>

<span data-ttu-id="482e0-119">Mindkét tanúsítványnak helyreállíthatja kattintva **adatok alaphelyzetbe állítása** Azure Cosmos DB-emulátort a a Windows tálca az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="482e0-119">Both certificates can be regenerated by clicking **Reset Data** as shown below from Azure Cosmos DB Emulator running in the Windows Tray.</span></span> <span data-ttu-id="482e0-120">Ha újragenerálni a tanúsítványokat, és telepítette azokat a Java tanúsítványtárolóba vagy használták őket, máshol szüksége lesz a frissítésükhöz, ellenkező esetben az alkalmazás már nem csatlakoznak a helyi emulátor.</span><span class="sxs-lookup"><span data-stu-id="482e0-120">If you regenerate the certificates and have installed them into the Java certificate store or used them elsewhere you will need to update them, otherwise your application will no longer connect to the local emulator.</span></span>

![Az Azure Cosmos DB helyi emulátor adatok visszaállítása](./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png)

## <a name="how-to-export-the-azure-cosmos-db-ssl-certificate"></a><span data-ttu-id="482e0-122">Az Azure Cosmos DB SSL-tanúsítvány exportálása</span><span class="sxs-lookup"><span data-stu-id="482e0-122">How to export the Azure Cosmos DB SSL certificate</span></span>

1. <span data-ttu-id="482e0-123">Indítsa el a Windows tanúsítványkezelője a certlm.msc fut, és keresse meg a személyes Tokens tanúsítványok mappát, és nyissa meg a tanúsítvány rövid nevét **DocumentDbEmulatorCertificate**.</span><span class="sxs-lookup"><span data-stu-id="482e0-123">Start the Windows Certificate manager by running certlm.msc and navigate to the Personal->Certificates folder and open the certificate with the friendly name **DocumentDbEmulatorCertificate**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 1. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png)

2. <span data-ttu-id="482e0-125">Kattintson a **részletek** majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="482e0-125">Click on **Details** then **OK**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 2. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png)

3. <span data-ttu-id="482e0-127">Kattintson a **Másolás fájlba...** .</span><span class="sxs-lookup"><span data-stu-id="482e0-127">Click **Copy to File...**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 3. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png)

4. <span data-ttu-id="482e0-129">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="482e0-129">Click **Next**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 4. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png)

5. <span data-ttu-id="482e0-131">Kattintson a **nem, nem exportálja a titkos kulcs**, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="482e0-131">Click **No, do not export private key**, then click **Next**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 5. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png)

6. <span data-ttu-id="482e0-133">Kattintson a **Base-64 kódolású X.509 (. CER)** , majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="482e0-133">Click on **Base-64 encoded X.509 (.CER)** and then **Next**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 6. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png)

7. <span data-ttu-id="482e0-135">Nevezze el a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="482e0-135">Give the certificate a name.</span></span> <span data-ttu-id="482e0-136">Ebben az esetben **documentdbemulatorcert** majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="482e0-136">In this case **documentdbemulatorcert** and then click **Next**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálása 7. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png)

8. <span data-ttu-id="482e0-138">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="482e0-138">Click **Finish**.</span></span>

    ![Az Azure Cosmos DB helyi emulátor exportálja a 8. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png)

## <a name="how-to-use-the-certificate-in-java"></a><span data-ttu-id="482e0-140">A tanúsítvány a Java használatával</span><span class="sxs-lookup"><span data-stu-id="482e0-140">How to use the certificate in Java</span></span>

<span data-ttu-id="482e0-141">Java-alkalmazások vagy a MongoDB-alkalmazások, a Java-ügyfél használata esetén telepítse a tanúsítványt a tanúsítványtárolóba Java alapértelmezett mint sikeres könnyebben a "-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword = "<password>" jelzők.</span><span class="sxs-lookup"><span data-stu-id="482e0-141">When running Java applications or MongoDB applications that use the Java client it is easier to install the certificate into the Java default certificate store than passing the "-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword="<password>" flags.</span></span> <span data-ttu-id="482e0-142">Ha például a megadott [Java bemutató alkalmazás](https://localhost:8081/_explorer/index.html) attól függ, az alapértelmezett tanúsítványtárolójában.</span><span class="sxs-lookup"><span data-stu-id="482e0-142">For example the included [Java Demo application](https://localhost:8081/_explorer/index.html) depends on the default certificate store.</span></span>

<span data-ttu-id="482e0-143">Kövesse az utasításokat a [tanúsítvány hozzáadása a Java hitelesítésszolgáltató tanúsítványtárolójában](https://docs.microsoft.com/azure/java-add-certificate-ca-store) X.509-tanúsítvány importálása az alapértelmezett Java tanúsítványtárolójába.</span><span class="sxs-lookup"><span data-stu-id="482e0-143">Follow the instructions in the [Adding a Certificate to the Java CA Certificates Store](https://docs.microsoft.com/azure/java-add-certificate-ca-store) to import the X.509 certificate into the default Java certificate store.</span></span> <span data-ttu-id="482e0-144">Ne mind kívánja fog dolgozni a % JAVA_HOME % könyvtárban kulcseszköz futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="482e0-144">Keep in mind you will be working in the %JAVA_HOME% directory when running keytool.</span></span>

<span data-ttu-id="482e0-145">Egyszer a "CosmosDBEmulatorCertificate" SSL-tanúsítvány telepítve van az alkalmazás csatlakozzon, és használja a helyi Azure Cosmos DB emulátor képesnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="482e0-145">Once the "CosmosDBEmulatorCertificate" SSL certificate is installed your application should be able to connect and use the local Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="482e0-146">Ha továbbra is problémákat tapasztal érdemes lehet kövesse a [hibakeresés SSL/TLS kapcsolatok](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) cikk.</span><span class="sxs-lookup"><span data-stu-id="482e0-146">If you continue to have trouble you may want to follow the [Debugging SSL/TLS Connections](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) article.</span></span> <span data-ttu-id="482e0-147">Nagyon valószínű, a tanúsítvány nincs telepítve a %JAVA_HOME%/jre/lib/security/cacerts tárolóba.</span><span class="sxs-lookup"><span data-stu-id="482e0-147">It is very likely the certificate is not installed into the %JAVA_HOME%/jre/lib/security/cacerts store.</span></span> <span data-ttu-id="482e0-148">Az eset például, ha az alkalmazás Java több telepített verzióját használja egy másik cacerts tárolóban, mint egy frissített.</span><span class="sxs-lookup"><span data-stu-id="482e0-148">For example if you have multiple installed versions of Java your application may be using a different cacerts store than the one you updated.</span></span>

## <a name="how-to-use-the-certificate-in-python"></a><span data-ttu-id="482e0-149">A tanúsítvány használatáról Python</span><span class="sxs-lookup"><span data-stu-id="482e0-149">How to use the certificate in Python</span></span>

<span data-ttu-id="482e0-150">Alapértelmezés szerint a [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) a DocumentDB API nem próbálja meg, és nem használja az SSL-tanúsítvány, ha a helyi emulátor csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="482e0-150">By default the [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) for the DocumentDB API will not try and use the SSL certificate when connecting to the local emulator.</span></span> <span data-ttu-id="482e0-151">Ha azonban az SSL-érvényesítési használni kívánt követésével szereplő példák a [Python szoftvercsatorna burkolók](https://docs.python.org/2/library/ssl.html) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="482e0-151">If however you want to use SSL validation you can follow the examples in the [Python socket wrappers](https://docs.python.org/2/library/ssl.html) documentation.</span></span>

## <a name="how-to-use-the-certificate-in-nodejs"></a><span data-ttu-id="482e0-152">A tanúsítvány használata a Node.js-ben</span><span class="sxs-lookup"><span data-stu-id="482e0-152">How to use the certificate in Node.js</span></span>

<span data-ttu-id="482e0-153">Alapértelmezés szerint a [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) a DocumentDB API nem próbálja meg, és nem használja az SSL-tanúsítvány, ha a helyi emulátor csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="482e0-153">By default the [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) for the DocumentDB API will not try and use the SSL certificate when connecting to the local emulator.</span></span> <span data-ttu-id="482e0-154">Ha azonban az SSL-érvényesítési használni kívánt követésével szereplő példák a [Node.js dokumentáció](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span><span class="sxs-lookup"><span data-stu-id="482e0-154">If however you want to use SSL validation you can follow the examples in the [Node.js documentation](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="482e0-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="482e0-155">Next steps</span></span>

<span data-ttu-id="482e0-156">Ebben az oktatóanyagban ezt a következők:</span><span class="sxs-lookup"><span data-stu-id="482e0-156">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="482e0-157">Elforgatott tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="482e0-157">Rotated certificates</span></span>
> * <span data-ttu-id="482e0-158">Az SSL-tanúsítvány exportálása</span><span class="sxs-lookup"><span data-stu-id="482e0-158">Exported the SSL certificate</span></span>
> * <span data-ttu-id="482e0-159">Megtudta, hogyan használhatja a tanúsítványt a Java, Python, Node.js</span><span class="sxs-lookup"><span data-stu-id="482e0-159">Learned how to use the certificate in Java, Python and Node.js</span></span>

<span data-ttu-id="482e0-160">Most már folytathatja a fogalmakat részt Cosmos DB további információt.</span><span class="sxs-lookup"><span data-stu-id="482e0-160">You can now proceed to the Concepts section for more information about Cosmos DB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="482e0-161">Globális terjesztés</span><span class="sxs-lookup"><span data-stu-id="482e0-161">Global distribution</span></span>](distribute-data-globally.md) 
