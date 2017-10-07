---
title: "egy tanúsítvány toohello Java hitelesítésszolgáltató-tárolójába aaaAdd |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd tanúsítvány hitelesítésszolgáltatói tanúsítvány toohello Java CA-tanúsítvány (cacerts) tárolására Twilio szolgáltatáshoz vagy Azure Service Bus."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: d3699b0a-835c-43fb-844d-9c25344e5cda
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 030e43129580023942dee662e72d2f443167f308
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-a-certificate-toohello-java-ca-certificates-store"></a>A tanúsítvány toohello Java Hitelesítésszolgáltatói tanúsítványok tárolójában hozzáadása
hello lépések bemutatják, hogyan tooadd tanúsítvány hitelesítésszolgáltatói tanúsítvány toohello Java CA-tanúsítvány (cacerts) tárolja. hello példában hello CA-tanúsítvány szükséges a hello Twilio szolgáltatáshoz. Később hello témakörben található információk azt ismerteti, hogyan tooinstall hello Hitelesítésszolgáltatói tanúsítvány hello Azure Service Bus számára. 

A JDK és hozzáadná tooyour Azure-projekt használhatja kulcseszközről tooadd hello Hitelesítésszolgáltatói tanúsítvány előzetes toozipping **approot** mappát, vagy egy kulcseszköz tooadd hello tanúsítványt használó Azure indítási feladat sikerült futtatása. Ez a példa feltételezi, hogy a Hitelesítésszolgáltatói tanúsítvány előzetes toohello alatt zip JDK adhat. Ezenkívül egy adott CA-tanúsítvány használandó hello példa, de hello lépéseket, hogy egy másik hitelesítésszolgáltató tanúsítvány, és importálása hello cacerts tároló hasonló lenne.

## <a name="tooadd-a-certificate-toohello-cacerts-store"></a>tooadd egy tanúsítvány toohello cacerts tárolásához
1. A beállított tooyour JDK parancsot a parancssorba **jdk\jre\lib\security** mappa hello toosee milyen tanúsítványok telepítését követően futtassa:
   
    `keytool -list -keystore cacerts`
   
    Kéri hello tárolóban jelszót. hello alapértelmezett jelszó **változtatás**. (Ha azt szeretné, hogy toochange hello jelszó, a dokumentációban hello kulcseszközről: <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) Ebben a példában feltételezzük, hogy MD5 ujjlenyomat 67:CB:9 D hello tanúsítványt: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 nem szerepel a listában, és, hogy szeretné-e tooimport informatikai (hello Twilio API szolgáltatás által az adott tanúsítvány szükséges).
2. Hello tanúsítvány beszerzése hello listája megtalálható a tanúsítványok [GeoTrust legfelső szintű tanúsítványok](http://www.geotrust.com/resources/root-certificates/). Kattintson a jobb gombbal a hello tanúsítvány sorozatszámát 35:DE:F4:CF hello hivatkozásra, és mentse azt toohello **jdk\jre\lib\security** mappa. Ebben a példában a nevű tooa fájl mentése **Equifax\_biztonságos\_tanúsítvány\_Authority.cer**.
3. Hello tanúsítvány importálása a következő parancs hello keresztül:
   
    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`
   
    Amikor a rendszer megkérdezi, tootrust ezt a tanúsítványt hello tanúsítványnál MD5 ujjlenyomat 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, írja be a válasz **y**.
4. Futtassa a következő parancs tooensure hello Hitelesítésszolgáltatói tanúsítvány hello sikeresen importálva lett:
   
    `keytool -list -keystore cacerts`
5. A ZIP-hello JDK, és adja hozzá tooyour Azure-projekt **approot** mappát.

Kulcseszköz kapcsolatos információkért lásd: <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Az Azure legfelső szintű tanúsítványok
Az Azure-szolgáltatásokhoz (például az Azure Service Bus) használó alkalmazások tootrust hello Baltimore CyberTrust legfelső szintű tanúsítványra van szükség. (2013. április 15 verziótól Azure megkezdte hello GTE CyberTrust globális Root toohello Baltimore CyberTrust Root áttelepítéséhez. Ez az áttelepítés alatt zajlott le több hónapig toocomplete.)

hello Baltimore tanúsítvány előfordulhat, hogy telepítve legyen a cacerts tárolóban, ezért ne feledje toorun hello **kulcseszköz-lista** első toosee parancsot, ha már létezik.

Ha tooadd hello Baltimore CyberTrust Root, sorozatszám 02:00:00:b9 és SHA1 ujjlenyomat d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Letölthető a <https://cacert.omniroot.com/bc2025.crt>, tooa helyi fájlt mentette kiterjesztésű **.cer**, majd importálja a használatával **kulcseszköz** a fentiek szerint.

## <a name="next-steps"></a>Következő lépések
Azure által használt hello legfelső szintű tanúsítványok kapcsolatos további információkért lásd: [Azure legfelső szintű tanúsítvány áttelepítési](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Java kapcsolatos további információkért lásd: [Azure Java-fejlesztőknek](/java/azure).

