---
title: "aaaSplit egyesítéses biztonsági beállításai |} Microsoft Docs"
description: "Állítson be x409 titkosítási tanúsítványok"
metakeywords: Elastic Database certificates security
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: f9e89c57-61a0-484f-b787-82dae2349cb6
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 511c04be0598d8a0889aa3e3fcf02be0bf0e96cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="split-merge-security-configuration"></a>Vegyes egyesítéses biztonsági konfiguráció
toouse hello vegyes/egyesítés szolgáltatás megfelelően be kell állítani biztonsági. hello szolgáltatást a Microsoft Azure SQL Database hello rugalmas bővítést szolgáltatás részét képezi. További információkért lásd: [rugalmas méretezési felosztása és egyesítése szolgáltatás oktatóanyag](sql-database-elastic-scale-configure-deploy-split-and-merge.md).

## <a name="configuring-certificates"></a>Tanúsítványok konfigurálása
Két módon a tanúsítványok konfigurálva legyenek. 

1. [tooConfigure hello SSL-tanúsítvány](#to-configure-the-ssl-certificate)
2. [Ügyféltanúsítványok tooConfigure](#to-configure-client-certificates) 

## <a name="tooobtain-certificates"></a>tooobtain tanúsítványok
Tanúsítványok hello vagy nyilvános hitelesítésszolgáltatótól (CA) szerezhetők [Windows tanúsítványszolgáltatást](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx). Ezek azok az előnyben részesített hello módszerek tooobtain tanúsítványokat.

Ha ezek a lehetőségek nem állnak rendelkezésre, létrehozhat **önaláírt tanúsítványokat**.

## <a name="tools-toogenerate-certificates"></a>Eszközök toogenerate tanúsítványok
* [MakeCert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="toorun-hello-tools"></a>toorun hello eszközök
* Az a fejlesztő parancssorból Visual Studios, lásd: [Visual Studio parancssorból](http://msdn.microsoft.com/library/ms229859.aspx) 
  
    Ha telepítette, Ugrás:
  
        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 
* Beolvasása a WDK hello [Windows 8.1: Töltse le a készletek és eszközök](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="tooconfigure-hello-ssl-certificate"></a>tooconfigure hello SSL-tanúsítvány
Egy SSL-tanúsítvány szükséges tooencrypt hello kommunikációt, és hello kiszolgáló hitelesítéséhez. Válassza ki az alábbi három forgatókönyv hello alkalmazható hello, és hajtsa végre az összes lépését:

### <a name="create-a-new-self-signed-certificate"></a>Hozzon létre egy új önaláírt tanúsítványt
1. [Önaláírt tanúsítvány létrehozása](#create-a-self-signed-certificate)
2. [Az önaláírt SSL-tanúsítvány PFX-fájl létrehozása](#create-pfx-file-for-self-signed-ssl-certificate)
3. [Töltse fel az SSL-tanúsítvány tooCloud szolgáltatás](#upload-ssl-certificate-to-cloud-service)
4. [A szolgáltatás konfigurációs fájljában SSL-tanúsítvány frissítése](#update-ssl-certificate-in-service-configuration-file)
5. [SSL-hitelesítésszolgáltató importálása](#import-ssl-certification-authority)

### <a name="toouse-an-existing-certificate-from-hello-certificate-store"></a>toouse hello tanúsítvány létező tanúsítvány tárolásához
1. [SSL-tanúsítvány exportálása a tanúsítványt a tanúsítványtárolóból](#export-ssl-certificate-from-certificate-store)
2. [Töltse fel az SSL-tanúsítvány tooCloud szolgáltatás](#upload-ssl-certificate-to-cloud-service)
3. [A szolgáltatás konfigurációs fájljában SSL-tanúsítvány frissítése](#update-ssl-certificate-in-service-configuration-file)

### <a name="toouse-an-existing-certificate-in-a-pfx-file"></a>toouse egy meglévő tanúsítványt a PFX-fájl
1. [Töltse fel az SSL-tanúsítvány tooCloud szolgáltatás](#upload-ssl-certificate-to-cloud-service)
2. [A szolgáltatás konfigurációs fájljában SSL-tanúsítvány frissítése](#update-ssl-certificate-in-service-configuration-file)

## <a name="tooconfigure-client-certificates"></a>tooconfigure ügyféltanúsítványok
Ügyféltanúsítványok rendelés tooauthenticate kérelmek toohello szolgáltatás kell megadni. Válassza ki az alábbi három forgatókönyv hello alkalmazható hello, és hajtsa végre az összes lépését:

### <a name="turn-off-client-certificates"></a>Ügyféltanúsítványok kikapcsolása
1. [Ügyféltanúsítvány-alapú hitelesítés kikapcsolása](#turn-off-client-certificate-based-authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Új ügyfél önaláírt tanúsítványokat
1. [Hozzon létre egy önaláírt hitelesítésszolgáltató](#create-a-self-signed-certification-authority)
2. [Töltse fel a Hitelesítésszolgáltatói tanúsítvány tooCloud szolgáltatás](#upload-ca-certificate-to-cloud-service)
3. [A szolgáltatás konfigurációs fájljában Hitelesítésszolgáltatói tanúsítvány frissítése](#update-ca-certificate-in-service-configuration-file)
4. [Ügyfél-tanúsítványok kiállításához](#issue-client-certificates)
5. [Az ügyféltanúsítványok PFX-fájlok létrehozása](#create-pfx-files-for-client-certificates)
6. [Ügyfél-tanúsítvány importálása](#Import-Client-Certificate)
7. [Másolja a tanúsítvány-ujjlenyomatok Client](#copy-client-certificate-thumbprints)
8. [Konfigurálja az ügyfeleket engedélyezett a hello szolgáltatás konfigurációs fájlja](#configure-allowed-clients-in-the-service-configuration-file)

### <a name="use-existing-client-certificates"></a>Meglévő ügyfél-tanúsítványok használata
1. [Nyilvános hitelesítésszolgáltató-kulcs megkeresése](#find-ca-public-key)
2. [Töltse fel a Hitelesítésszolgáltatói tanúsítvány tooCloud szolgáltatás](#Upload-CA-certificate-to-cloud-service)
3. [A szolgáltatás konfigurációs fájljában Hitelesítésszolgáltatói tanúsítvány frissítése](#Update-CA-Certificate-in-Service-Configuration-File)
4. [Másolja a tanúsítvány-ujjlenyomatok Client](#Copy-Client-Certificate-Thumbprints)
5. [Konfigurálja az ügyfeleket engedélyezett a hello szolgáltatás konfigurációs fájlja](#configure-allowed-clients-in-the-service-configuration-file)
6. [Konfigurálja az ügyfél visszavonási állapotának ellenőrzése](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Engedélyezett IP-címek
Hozzáférés toohello Szolgáltatásvégpontok IP-címek korlátozott toospecific tartomány lehet.

## <a name="tooconfigure-encryption-for-hello-store"></a>hello Store tooconfigure titkosítás
Tanúsítvány szükség tooencrypt hello hitelesítő adatokat, amelyek hello metaadat-tárolóban. Válassza ki az alábbi három forgatókönyv hello alkalmazható hello, és hajtsa végre az összes lépését:

### <a name="use-a-new-self-signed-certificate"></a>Egy új önaláírt tanúsítvány használatára
1. [Önaláírt tanúsítvány létrehozása](#create-a-self-signed-certificate)
2. [Hozzon létre önaláírt titkosítási tanúsítvány PFX-fájlból](#create-pfx-file-for-self-signed-ssl-certificate)
3. [Töltse fel a titkosítási tanúsítvány tooCloud szolgáltatás](#upload-encryption-certificate-to-cloud-service)
4. [A szolgáltatás konfigurációs fájljában titkosítási tanúsítvány frissítése](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-from-hello-certificate-store"></a>Létező tanúsítvány használatára hello tanúsítványtárolóból
1. [Titkosítási tanúsítvány exportálása a tanúsítványt a tanúsítványtárolóból](#export-encryption-certificate-from-certificate-store)
2. [Töltse fel a titkosítási tanúsítvány tooCloud szolgáltatás](#upload-encryption-certificate-to-cloud-service)
3. [A szolgáltatás konfigurációs fájljában titkosítási tanúsítvány frissítése](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Létező tanúsítvány használatára a PFX-fájl
1. [Töltse fel a titkosítási tanúsítvány tooCloud szolgáltatás](#upload-encryption-certificate-to-cloud-service)
2. [A szolgáltatás konfigurációs fájljában titkosítási tanúsítvány frissítése](#update-encryption-certificate-in-service-configuration-file)

## <a name="hello-default-configuration"></a>hello alapértelmezett konfigurációja
hello alapértelmezett konfiguráció összes hozzáférési toohello HTTP-végpont megtagadja. Ez a beállítás, javasolt, mert hello kérelmek toothese végpontok is elvégezheti a bizalmas információkat, például adatbázis-hitelesítő adatok hello.
hello alapértelmezett konfigurációja lehetővé teszi, hogy minden hozzáférési toohello HTTPS-végponton. Ezzel a beállítással korlátozható tovább.

### <a name="changing-hello-configuration"></a>Hello konfiguráció módosítása
hello tooand végpont vonatkozó hozzáférés-vezérlési szabályok csoportja konfigurált hello  **<EndpointAcls>**  hello szakasz **szolgáltatás konfigurációs fájlja**.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

hozzáférés-vezérlési csoportban hello szabályok úgy vannak konfigurálva, a egy <AccessControl name=""> hello szolgáltatás konfigurációs fájl részét. 

hello formátum esetén, tekintse meg a hálózati hozzáférés-vezérlési listái dokumentációját.
Például tooallow egyetlen IP-címek a hello tartomány 100.100.0.0 too100.100.255.255 tooaccess hello HTTPS-végponton, hello szabályok néz ki:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>Letiltja a szolgáltatás megelőzése
Két különböző mechanizmus toodetect támogatott, és szolgáltatásmegtagadási támadások megelőzése érdekében:

* Korlátozhatja a távoli állomásonként egyidejű kérelmek száma (alapértelmezés szerint kikapcsolva)
* Távoli állomásonként hozzáférési sebesség korlátozása (az alapértelmezés)

Ezek a további részletes ismertetését lásd: az IIS-ben a dinamikus IP-biztonság hello szolgáltatások alapulnak. Ha ez a konfiguráció módosítása figyeljen a hello a következő tényezőket:

* proxyk és a hálózati címfordítás eszközökön keresztüli hello távoli állomásadatai hello viselkedését
* Minden egyes kérelem tooany erőforrás hello webes szerepkörben tekinthető (pl. betöltése parancsfájlok, képek, stb)

## <a name="restricting-number-of-concurrent-accesses"></a>Korlátozza az egyidejű hozzáférések száma
Ez a viselkedés konfigurált hello-beállítások a következők:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Ez a védelem DynamicIpRestrictionDenyByConcurrentRequests tootrue tooenable módosítása

## <a name="restricting-rate-of-access"></a>Gyakorisága a hozzáférés korlátozása
Ez a viselkedés konfigurált hello-beállítások a következők:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-hello-response-tooa-denied-request"></a>Hello válasz tooa konfigurálása elutasította a kérelmet
hello következő beállítással hello válasz tooa elutasította a kérelmet:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Tekintse meg a toohello dokumentáció dinamikus IP-biztonság az IIS-ben más támogatott értékek.

## <a name="operations-for-configuring-service-certificates"></a>Műveletek a szolgáltatás tanúsítványok konfigurálásáról
Ez a témakör csak a hivatkozás van. Hajtsa végre hello leírt konfigurációs lépéseket:

* Hello SSL-tanúsítvány konfigurálása
* Állítson be ügyféltanúsítványokat

## <a name="create-a-self-signed-certificate"></a>Önaláírt tanúsítvány létrehozása
Hajtható végre:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

toocustomize:

* -n hello szolgáltatási URL-cím. Helyettesítő karakterek ("CN = * .cloudapp .net") és az alternatív neveket ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") támogatottak.
* -e a hello tanúsítvány lejárati dátuma hozzon létre egy erős jelszót, és adja meg, amikor a rendszer kéri.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>Hozzon létre önaláírt SSL-tanúsítvány PFX-fájlt
Hajtható végre:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Adja meg a jelszót, és exportálhatja a tanúsítványt, amelynek ezeket a beállításokat:

* Igen, a hello titkos kulcs exportálása
* Minden további tulajdonság exportálása

## <a name="export-ssl-certificate-from-certificate-store"></a>SSL-tanúsítvány exportálása a tanúsítványt a tanúsítványtárolóból
* Tanúsítvány található
* Kattintson a műveletek összes -> feladatok -> Exportálás...
* Exportálja a tanúsítványt egy. Ezek a beállítások a PFX-fájlt:
  * Igen, a hello titkos kulcs exportálása
  * Minden tanúsítvány belefoglalása a tanúsítványláncba, hello lehetőleg * minden további tulajdonság exportálása

## <a name="upload-ssl-certificate-toocloud-service"></a>Töltse fel az SSL-tanúsítvány toocloud szolgáltatás
Feltöltés a hello meglévő tanúsítványt, vagy jön létre. Az SSL-kulcsból álló kulcspárt hello PFX-fájlt:

* Adja meg a hello jelszó hello titkos kulcs adatainak védelme

## <a name="update-ssl-certificate-in-service-configuration-file"></a>A szolgáltatás konfigurációs fájljában SSL-tanúsítvány frissítése
A következő beállítás hello szolgáltatás konfigurációs fájljában hello feltöltött tanúsítvány toohello felhőszolgáltatás hello ujjlenyomattal rendelkező hello hello ujjlenyomat értékének frissítése:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>SSL-hitelesítésszolgáltató importálása
Kövesse az alábbi lépéseket az összes fiók/gépen hello szolgáltatással kommunikáló:

* Kattintson duplán a hello. A Windows Intézőben CER-fájljával
* Hello tanúsítvány párbeszédpanelen kattintson a tanúsítvány telepítése...
* Importálja a tanúsítványt a megbízható legfelső szintű hitelesítésszolgáltatók tárolására hello

## <a name="turn-off-client-certificate-based-authentication"></a>Ügyféltanúsítvány-alapú hitelesítés kikapcsolása
Csak az ügyféltanúsítvány-alapú hitelesítés támogatott, és letiltja azt lehetővé teszi a nyilvános hozzáférés toohello Szolgáltatásvégpontok, kivéve, ha más mechanizmusok helyen (pl. Microsoft Azure virtuális hálózat).

Módosítsa ezen beállítások toofalse hello szolgáltatás konfigurációs fájl tooturn hello szolgáltatásban:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

Ezután másolja ugyanazzal az ujjlenyomattal mint hello SSL-tanúsítvány hello hello Hitelesítésszolgáltatói tanúsítvány beállítása:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Hozzon létre egy önaláírt hitelesítésszolgáltató
A következő lépéseket toocreate egy önaláírt tanúsítványt tooact hitelesítésszolgáltatóként hello hajtható végre:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

toocustomize azt

* -e a hello tanúsítvány lejárati dátuma

## <a name="find-ca-public-key"></a>Nyilvános hitelesítésszolgáltató-kulcs megkeresése
Minden ügyfél kell kiadott tanúsítványok hello szolgáltatás megbízható hitelesítésszolgáltató által. Található hello nyilvános kulcs toohello toobe fog hello ügyféltanúsítványok kiállító hitelesítésszolgáltató-hitelesítéshez használt a rendelés tooupload azt toohello felhőalapú szolgáltatás.

Hello nyilvános kulccsal hello fájl nem érhető el, ha exportálja hello tanúsítványtárolóból:

* Tanúsítvány található
  * Keresse meg az ügyfél által kiállított tanúsítványt hello ugyanazt a hitelesítésszolgáltatót
* Kattintson duplán a hello tanúsítványra.
* Válassza ki a hello az Általános lapon hello tanúsítvány párbeszédpanelen.
* Kattintson duplán hello hitelesítésszolgáltató hello elérési úton.
* Készítsen feljegyzéseket hello tanúsítvány tulajdonságai.
* Bezárás hello **tanúsítvány** párbeszédpanel.
* Tanúsítvány található
  * Keresse meg a fent leírt hitelesítésszolgáltató hello.
* Kattintson a műveletek összes -> feladatok -> Exportálás...
* Exportálja a tanúsítványt egy. CER beállítások segítségével:
  * **Nem, nem exportálja a titkos kulcs hello**
  * Minden tanúsítvány belefoglalása hello tanúsítványláncba, ha lehetséges.
  * Minden további tulajdonság exportálása.

## <a name="upload-ca-certificate-toocloud-service"></a>Töltse fel a Hitelesítésszolgáltatói tanúsítvány toocloud szolgáltatás
Feltöltés a hello meglévő tanúsítványt, vagy jön létre. CER-fájljával hello hitelesítésszolgáltató nyilvános kulccsal.

## <a name="update-ca-certificate-in-service-configuration-file"></a>A szolgáltatás konfigurációs fájljában frissítés Hitelesítésszolgáltatói tanúsítvány
A következő beállítás hello szolgáltatás konfigurációs fájljában hello feltöltött tanúsítvány toohello felhőszolgáltatás hello ujjlenyomattal rendelkező hello hello ujjlenyomat értékének frissítése:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

A következő beállítás a hello azonos hello hello értékét ujjlenyomata:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Ügyfél tanúsítványokat
Minden egyes jogosult tooaccess hello szolgáltatás ügyféltanúsítvánnyal a his/hers kizárólagos használatára kell lennie, és válasszon his/hers saját tooprotect erős jelszót a titkos kulcsot. 

egyazon számítógépen, ahol hello önaláírt hitelesítésszolgáltató tanúsítványát generált és a tárolt hello hello a következő lépéseket kell végrehajtani:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Testreszabása:

* Ezzel a tanúsítvánnyal hitelesítése toohello ügyfélnek azonosítójú -n
* -e a hello tanúsítvány lejárati dátuma
* MyID.pvk és MyID.cer, a egyedi fájlneveket az ügyféltanúsítványt

Ez a parancs felszólítja egy jelszó toobe létrehozott, és egyszer használva. Használjon erős jelszót.

## <a name="create-pfx-files-for-client-certificates"></a>Az ügyfél PFX-fájlok tanúsítványok létrehozása
Minden létrehozott ügyféltanúsítványt hajtható végre:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Testreszabása:

    MyID.pvk and MyID.cer with hello filename for hello client certificate

Adja meg a jelszót, és exportálhatja a tanúsítványt, amelynek ezeket a beállításokat:

* Igen, a hello titkos kulcs exportálása
* Minden további tulajdonság exportálása
* Ez a tanúsítvány kiállítása történik hello egyedi toowhom válasszon hello exportálási jelszó

## <a name="import-client-certificate"></a>Ügyfél-tanúsítvány importálása
Minden egyes személy, amely ügyféltanúsítványt bocsátották hello kulcsból álló kulcspárt kell importálni a hello gépek többé használ toocommunicate hello szolgáltatásban:

* Kattintson duplán a hello. PFX-fájlt a Windows Intézőben
* Tanúsítvány importálása hello személyes tárolójából rendelkező legalább ezt a beállítást:
  * Minden további tulajdonság be van jelölve szerepeltetése

## <a name="copy-client-certificate-thumbprints"></a>Másolja a tanúsítvány-ujjlenyomatok client
Minden egyes személy, amely ügyféltanúsítványt bocsátották kövesse az alábbi lépéseket a rendelés tooobtain hello ujjlenyomata his/hers tanúsítvány, amely belekerül toohello szolgáltatás konfigurációs fájljában:

* Certmgr.exe futtatása
* Válassza ki a hello személyes lap
* Kattintson duplán a hitelesítéshez használt toobe hello ügyféltanúsítványt
* Hello tanúsítvány megnyíló párbeszédpanelen válassza ki hello részletei lapon.
* Győződjön meg arról, hogy megjeleníti az összes megjelenítése
* Ujjlenyomat nevű hello listában válassza hello mező
* Hello ujjlenyomat hello értéket másol ** törlése előtt hello első számjegy láthatatlan Unicode-karaktereket ** csak szóközökből törlése

## <a name="configure-allowed-clients-in-hello-service-configuration-file"></a>Hello szolgáltatás konfigurációs fájljában engedélyezett ügyfelek konfigurálása
A következő beállítás hello szolgáltatás konfigurációs fájljában toohello szolgáltatás engedélyezett hello ügyféltanúsítványok hello ujjlenyomatai vesszővel tagolt listáját tartalmazó hello hello értékének frissítése:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Konfigurálja az ügyfél visszavonási állapotának ellenőrzése
hello alapértelmezett beállítása nem ellenőrzi a hitelesítésszolgáltató hello az ügyfél tanúsítvány-visszavonási állapotát. a hello tooturn ellenőrzi, ha hello hello tanúsítványt kiállító hitelesítésszolgáltató támogatja az ilyen ellenőrzéseket, módosítsa a beállítást követő hello X509RevocationMode számbavételi definiált hello értékek egyikével hello:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>Hozzon létre önaláírt titkosítási tanúsítványok PFX-fájlt
Titkosítási tanúsítványt az alábbi hajtható végre:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Testreszabása:

    MyID.pvk and MyID.cer with hello filename for hello encryption certificate

Adja meg a jelszót, és exportálhatja a tanúsítványt, amelynek ezeket a beállításokat:

* Igen, a hello titkos kulcs exportálása
* Minden további tulajdonság exportálása
* Hello jelszót kell hello tanúsítvány toohello felhőszolgáltatás feltöltésekor.

## <a name="export-encryption-certificate-from-certificate-store"></a>Titkosítási tanúsítvány exportálása a tanúsítványt a tanúsítványtárolóból
* Tanúsítvány található
* Kattintson a műveletek összes -> feladatok -> Exportálás...
* Exportálja a tanúsítványt egy. Ezek a beállítások a PFX-fájlt: 
  * Igen, a hello titkos kulcs exportálása
  * Minden tanúsítvány belefoglalása a tanúsítványláncba, hello Ha lehetséges 
* Minden további tulajdonság exportálása

## <a name="upload-encryption-certificate-toocloud-service"></a>Töltse fel a titkosítási tanúsítvány toocloud szolgáltatás
Feltöltés a hello meglévő tanúsítványt, vagy jön létre. Hello titkosítási kulcsból álló kulcspárt a PFX-fájlt:

* Adja meg a hello jelszó hello titkos kulcs adatainak védelme

## <a name="update-encryption-certificate-in-service-configuration-file"></a>A szolgáltatás konfigurációs fájljában titkosítási tanúsítvány frissítése
A következő beállítások hello szolgáltatás konfigurációs fájljában hello feltöltött tanúsítvány toohello felhőszolgáltatás hello ujjlenyomattal rendelkező hello hello ujjlenyomat értékének frissítése:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Gyakori műveletekhez
* Hello SSL-tanúsítvány konfigurálása
* Állítson be ügyféltanúsítványokat

## <a name="find-certificate"></a>Tanúsítvány található
Kövesse az alábbi lépéseket:

1. Futtassa a mmc.exe.
2. Fájl -> beépülő modul hozzáadása/eltávolítása...
3. Válassza ki **tanúsítványok**.
4. Kattintson az **Add** (Hozzáadás) parancsra.
5. Válassza ki a hello tanúsítvány tárolási helyét.
6. Kattintson a **Befejezés** gombra.
7. Kattintson az **OK** gombra.
8. Bontsa ki a **tanúsítványok**.
9. Bontsa ki a hello tanúsítvány tároló csomópontot.
10. Bontsa ki a hello tanúsítvány gyermekcsomópontja.
11. Jelöljön ki egy tanúsítványt az hello listán.

## <a name="export-certificate"></a>Tanúsítvány exportálása
A hello **Tanúsítványexportáló varázsló**:

1. Kattintson a **Tovább** gombra.
2. Válassza ki **Igen**, majd **hello titkos kulcs exportálása**.
3. Kattintson a **Tovább** gombra.
4. Válassza ki a kívánt fájl formátumba hello.
5. Ellenőrizze a szükségeskonfiguráció-hello-beállítások.
6. Ellenőrizze **jelszó**.
7. Adjon meg egy erős jelszót, és erősítse meg.
8. Kattintson a **Tovább** gombra.
9. Írja be vagy keresse meg a fájl nevét, ahol toostore hello tanúsítvány (használja a. PFX-kiterjesztéssel).
10. Kattintson a **Tovább** gombra.
11. Kattintson a **Befejezés** gombra.
12. Kattintson az **OK** gombra.

## <a name="import-certificate"></a>Tanúsítvány importálása
A Tanúsítványimportáló varázsló hello:

1. Válassza ki a hello tárolóhelyére.
   
   * Válassza ki **aktuális felhasználó** Ha csak az aktuális felhasználó futó folyamatok érik el a hello szolgáltatás
   * Válassza ki **helyi számítógép** Ha a számítógép egyéb folyamatok érik el a hello szolgáltatás
2. Kattintson a **Tovább** gombra.
3. Ha importál egy fájlból, erősítse meg a hello fájl elérési útját.
4. Ha importálni egy. PFX-fájlt:
   1. Adja meg a hello jelszó hello titkos kulcsok védelme
   2. Importálási beállítások megadása
5. Válassza ki a "Hely" tanúsítványok a következő tároló hello
6. Kattintson a **Browse** (Tallózás) gombra.
7. Válassza ki a kívánt tárolót hello.
8. Kattintson a **Befejezés** gombra.
   
   * Hello megbízható legfelső szintű hitelesítésszolgáltató-tároló választása esetén kattintson **Igen**.
9. Kattintson a **OK** összes párbeszédpanel windows rendszeren.

## <a name="upload-certificate"></a>Tanúsítvány feltöltése
A hello [Azure portál](https://portal.azure.com/)

1. Válassza ki **a felhőalapú szolgáltatások**.
2. Válassza ki a hello felhőalapú szolgáltatás.
3. Kattintson a felső menüben hello **tanúsítványok**.
4. Az alsó hello menüsávon kattintson **feltöltése**.
5. Válasszon ki hello tanúsítványfájlt.
6. Ha a rendszer egy. PFX fájlt, írja be a hello jelszót hello titkos kulcsot.
7. Ezt követően másolása hello új bejegyzést hello lista hello tanúsítvány ujjlenyomata.

## <a name="other-security-considerations"></a>Egyéb biztonsági szempontok
a jelen dokumentumban ismertetett hello SSL-beállítások hello szolgáltatás és az ügyfelek közötti kommunikáció titkosításához, a HTTPS-végpont hello használata esetén. Ez azért fontos, mert adatbázis-hozzáférési hitelesítő adatok, és más bizalmas adatokat hello kommunikációs tartalmazza. Ne feledje azonban, hogy hello szolgáltatás továbbra is fennáll belső állapotát, a hitelesítő adatokat, beleértve a hello Microsoft Azure SQL-adatbázis a Microsoft Azure-előfizetéshez tartozó metaadat-tároló megadott belső táblájában. Beállítás a szolgáltatás konfigurációs fájljában a következő hello részeként definiált, hogy az adatbázis (. CSCFG-fájl): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

Ebben az adatbázisban tárolt hitelesítő adatok titkosítása. Azonban célszerű, győződjön meg arról, hogy a szolgáltatástelepítések egyaránt webes és feldolgozói szerepkörök tartják be toodate és biztonságos, mint azok egyaránt rendelkezik hozzáféréssel toohello metaadatok adatbázis és a hello tanúsítvány az tárolt hitelesítő adatok titkosításához és visszafejtéséhez használt. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

