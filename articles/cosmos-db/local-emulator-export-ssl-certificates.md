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
# <a name="export-hello-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a>A Node.js, Java és Python használt hello Azure Cosmos DB emulátor tanúsítványok exportálása

[**Hello emulátor letöltése**](https://aka.ms/cosmosdb-emulator)

hello Azure Cosmos DB emulátor hello Azure Cosmos DB szolgáltatás fejlesztése céljából, beleértve az SSL-kapcsolatok használatát a emuláló helyi környezetet biztosít. A post bemutatja, hogyan tooexport hello SSL-tanúsítványok a nyelv és a futtatókörnyezetek integrálódó nem hello Windows tanúsítványtároló például Java, amely használja a saját [tanúsítványtároló](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) és Python használó[burkolók szoftvercsatorna](https://docs.python.org/2/library/ssl.html) és a Node.js használó [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback). További kapcsolatos hello emulátorának [használata hello Azure Cosmos DB emulátor a fejlesztéshez és teszteléshez](./local-emulator.md).

Ez az oktatóanyag ismerteti a következő feladatok hello:

> [!div class="checklist"]
> * Tanúsítványok elforgatása
> * SSL-tanúsítvány exportálása
> * Tanulási hogyan toouse hello Node.js, Java és Python-tanúsítványt

## <a name="certification-rotation"></a>Hitelesítésszolgáltató Elforgatás

Tanúsítványok hello Azure Cosmos DB helyi emulátor vannak hello hello emulátor első futtatásakor jönnek létre. Két tanúsítványokat is tartalmaz. Egy csatlakozáshoz toohello helyi emulátor és hello emulátor belül titkos kulcsok kezelése céljából. hello tanúsítványt tooexport hello rövid névvel "DocumentDBEmulatorCertificate" hello kapcsolati tanúsítvány.

Mindkét tanúsítványnak helyreállíthatja kattintva **adatok alaphelyzetbe állítása** Azure Cosmos DB-emulátort a Windows tálca hello az alább látható módon. Ha újragenerálni hello tanúsítványokat, és telepítette őket hello Java tanúsítványtárolóba vagy máshol használták őket tooupdate kell őket, ellenkező esetben az alkalmazás már nem kapcsolódik toohello helyi emulátor.

![Az Azure Cosmos DB helyi emulátor adatok visszaállítása](./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png)

## <a name="how-tooexport-hello-azure-cosmos-db-ssl-certificate"></a>Hogyan tooexport hello Azure Cosmos DB SSL-tanúsítvány

1. Indítsa el a Windows tanúsítványkezelő hello certlm.msc futtatásával, és keresse meg személyes toohello -> tanúsítványok mappa és a nyitott hello tanúsítvány hello rövid névvel **DocumentDbEmulatorCertificate**.

    ![Az Azure Cosmos DB helyi emulátor exportálása 1. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png)

2. Kattintson a **részletek** majd **OK**.

    ![Az Azure Cosmos DB helyi emulátor exportálása 2. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png)

3. Kattintson a **tooFile másolja...** .

    ![Az Azure Cosmos DB helyi emulátor exportálása 3. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png)

4. Kattintson a **Tovább** gombra.

    ![Az Azure Cosmos DB helyi emulátor exportálása 4. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png)

5. Kattintson a **nem, nem exportálja a titkos kulcs**, majd kattintson a **következő**.

    ![Az Azure Cosmos DB helyi emulátor exportálása 5. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png)

6. Kattintson a **Base-64 kódolású X.509 (. CER)** , majd **következő**.

    ![Az Azure Cosmos DB helyi emulátor exportálása 6. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png)

7. Adjon meg egy nevet, amely hello tanúsítványt. Ebben az esetben **documentdbemulatorcert** majd **következő**.

    ![Az Azure Cosmos DB helyi emulátor exportálása 7. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png)

8. Kattintson a **Befejezés** gombra.

    ![Az Azure Cosmos DB helyi emulátor exportálja a 8. lépés](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png)

## <a name="how-toouse-hello-certificate-in-java"></a>Hogyan toouse hello Java-tanúsítványt

Java-alkalmazások vagy hello Java ügyfelet használó alkalmazások MongoDB futtatásakor tanúsítványról szó könnyebb tooinstall hello hello Java alapértelmezett tanúsítványtárolóba sikeres hello mint "-Djavax.net.ssl.trustStore=<keystore> - Djavax.net.ssl.trustStorePassword= "<password>" jelzők. Például szereplő hello [Java bemutató alkalmazás](https://localhost:8081/_explorer/index.html) hello alapértelmezett számítógéptanúsítvány-tároló függ.

Hello hello utasításait követve [hozzáadása a Java-hitelesítésszolgáltató tanúsítványtárolójában tanúsítvány toohello](https://docs.microsoft.com/azure/java-add-certificate-ca-store) tooimport hello X.509 tanúsítvány hello alapértelmezett Java tanúsítványtárolójába. Ne mind kívánja dolgozni fog hello % JAVA_HOME % könyvtárban kulcseszköz futtatásakor.

Egyszer hello "CosmosDBEmulatorCertificate" SSL-tanúsítvány telepítve van az alkalmazás képes tooconnect kell lennie, és használja a helyi Azure Cosmos DB emulátor hello. Ha hiba történt a toohave továbbra is érdemes lehet a toofollow hello [hibakeresés SSL/TLS kapcsolatok](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) cikk. Nagyon valószínű hello tanúsítványa nincs telepítve hello %JAVA_HOME%/jre/lib/security/cacerts tárolóba. Az eset például, ha az alkalmazás Java több telepített verzióinak lehet, hogy használja egy másik cacerts tárolóban, mint egy frissített hello.

## <a name="how-toouse-hello-certificate-in-python"></a>Hogyan toouse hello Python-tanúsítványt

Alapértelmezett hello által [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) hello a DocumentDB API nem próbálja és toohello helyi emulátor kapcsolódáskor hello SSL-tanúsítványt használ. Ha azonban azt szeretné, hogy toouse SSL érvényesítési kövesse a hello hello példák [Python szoftvercsatorna burkolók](https://docs.python.org/2/library/ssl.html) dokumentációját.

## <a name="how-toouse-hello-certificate-in-nodejs"></a>Hogyan toouse hello Node.js-tanúsítványt

Alapértelmezett hello által [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) hello a DocumentDB API nem próbálja és toohello helyi emulátor kapcsolódáskor hello SSL-tanúsítványt használ. Ha azonban azt szeretné, hogy toouse SSL érvényesítési kövesse a hello hello példák [Node.js dokumentáció](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello következő régebben már kötöttek:

> [!div class="checklist"]
> * Elforgatott tanúsítványok
> * SSL-tanúsítvány exportált hello
> * Megtudta, hogyan toouse hello a Java, Python, Node.js tanúsítvány

Most már folytathatja toohello fogalmak szakasz Cosmos DB további információt.

> [!div class="nextstepaction"]
> [Globális terjesztés](distribute-data-globally.md) 
