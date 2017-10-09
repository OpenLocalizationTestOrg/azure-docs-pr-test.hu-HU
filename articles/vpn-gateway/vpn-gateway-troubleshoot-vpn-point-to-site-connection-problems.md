---
title: "Azure pont-pont aaaTroubleshoot kapcsolódási problémák |} Microsoft Docs"
description: "Megtudhatja, hogyan tootroubleshoot pont-hely kapcsolódási problémák léptek fel."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: genli
ms.openlocfilehash: 98d66074be62ad8c7153a903f69cb0d01f988cd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a>Hibáinak elhárítása: Az Azure pont – hely kapcsolat problémák

A cikk ismerteti, amelyekkel Ön is szembesülhet pont-pont csatlakozási probléma. Lehetséges okait és megoldásait ezeket a problémákat is ismerteti.

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a>VPN-ügyfél hiba: A tanúsítvány nem található.

### <a name="symptom"></a>Jelenség

Azure-beli virtuális hálózat tooconnect tooan hello VPN-ügyfél segítségével kísérli meg, a hello a következő hibaüzenet jelenhet meg:

**A tanúsítvány nem található, amely használható a bővíthető hitelesítési protokoll. (798 hiba)**

### <a name="cause"></a>Ok

Ez a probléma akkor fordul elő, ha hello ügyféltanúsítvány hiányzik a **tanúsítványok - aktuális User\Personal\Certificates**.

### <a name="solution"></a>Megoldás

Győződjön meg arról, hogy hello ügyfél tanúsítvány telepítve van a hello hello tanúsítványtárolóba (Certmgr.msc) helye a következő:
 
**Tanúsítványok – aktuális User\Personal\Certificates**

Hogyan tooinstall hello ügyféltanúsítvány kapcsolatos további információkért lásd: [tanúsítvány létrehozása és exportálása a pont – hely kapcsolatok](vpn-gateway-certificates-point-to-site.md).

> [!NOTE]
> Hello ügyféltanúsítvány importálásakor, ne válassza hello **titkos kulcs erős védelmének engedélyezése** lehetőséget.

## <a name="vpn-client-error-hello-message-received-was-unexpected-or-badly-formatted"></a>VPN-ügyfél hiba: hello fogadott üzenet nem várt vagy rosszul formázott

### <a name="symptom"></a>Jelenség

Azure-beli virtuális hálózat tooconnect tooan hello VPN-ügyfél segítségével kísérli meg, a hello a következő hibaüzenet jelenhet meg:

**hello üzenet érkezett a váratlan vagy rosszul formázott volt. (0x80090326 hiba)**

### <a name="cause"></a>Ok

Ez a probléma akkor fordul elő, ha hello legfelső szintű tanúsítvány nyilvános kulcsa nem hello Azure VPN gateway van töltve. Ez akkor is előfordulhat, ha hello kulcs sérült vagy lejárt.

### <a name="solution"></a>Megoldás

tooresolve probléma hello állapotát hello legfelső szintű tanúsítványt hello Azure portál toosee e visszavonásra került. Ha nincs visszavonva, próbálja meg toodelete hello legfelső szintű tanúsítvány és reupload. További információkért lásd: [olyan tanúsítványokat hoznak létre](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a>VPN-ügyfél hiba: A tanúsítványlánc feldolgozása, de a megszakadt 

### <a name="symptom"></a>Jelenség 

Azure-beli virtuális hálózat tooconnect tooan hello VPN-ügyfél segítségével kísérli meg, a hello a következő hibaüzenet jelenhet meg:

**Tanúsítványlánc feldolgozása befejeződött, de a hello megbízhatóság-ellenőrző nem megbízható legfelső szintű tanúsítványt megszakadt.**

### <a name="solution"></a>Megoldás

1. Győződjön meg arról, hogy a következő tanúsítványok hello hello megfelelő helyen találhatók:

    | Tanúsítvány | Hely |
    | ------------- | ------------- |
    | AzureClient.pfx  | Aktuális User\Personal\Certificates |
    | Azuregateway -*GUID*. cloudapp.net  | Aktuális User\Trusted legfelső szintű hitelesítésszolgáltatók|
    | AzureGateway -*GUID*. cloudapp.net, AzureRoot.cer    | Helyi számítógép\Megbízható legfelső szintű hitelesítésszolgáltatók|

2. Ha hello tanúsítványok már hello helyen, próbálja toodelete hello tanúsítványokat, és telepítse újra azokat. Hello  **azuregateway -*GUID*. cloudapp.net** tanúsítvány van hello VPN-konfigurációs ügyfélcsomag beszerzett hello Azure-portálon. Használhatja a fájl archivers tooextract hello fájlokat hello csomagból.

## <a name="file-download-error-target-uri-is-not-specified"></a>Letöltési hiba: nincs megadva a cél URI Azonosítóját

### <a name="symptom"></a>Jelenség

Hello a következő hibaüzenet jelenhet meg:

**Hiba történt a fájl letöltése. Nincs megadva a cél URI Azonosítóját.**

### <a name="cause"></a>Ok 

A probléma miatt egy hibás átjáró típusa. 

### <a name="solution"></a>Megoldás

VPN-átjáró típusa hello kell **VPN**, és a VPN-típus hello kell **RouteBased**.

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a>VPN-ügyfél hiba: az Azure VPN egyéni parancsprogram végrehajtása sikertelen volt 

### <a name="symptom"></a>Jelenség

Azure-beli virtuális hálózat tooconnect tooan hello VPN-ügyfél segítségével kísérli meg, a hello a következő hibaüzenet jelenhet meg:

**Egyéni parancsfájl (tooupdate az útválasztási táblázatot) nem sikerült. (8007026f hiba)**

### <a name="cause"></a>Ok

Ez a probléma akkor fordulhat elő, ha tooopen hello pont hely közötti VPN-kapcsolatot egy helyi használatával próbálja.

### <a name="solution"></a>Megoldás 

Nyissa meg a hello VPN csomag közvetlenül nem kell megnyitnia az hello helyi.

## <a name="cannot-install-hello-vpn-client"></a>Hello VPN-ügyfél nem tudja telepíteni.

### <a name="cause"></a>Ok 

Kiegészítő igazolás szükség tootrust hello VPN-átjáró a virtuális hálózat. hello tanúsítvány hello VPN-konfigurációs ügyfélcsomag a hello Azure-portálon létrehozott tartalmazza.

### <a name="solution"></a>Megoldás

Bontsa ki a hello VPN-ügyfélcsomag konfigurációs, és hello .cer fájl található. tooinstall hello tanúsítvány, kövesse az alábbi lépéseket:

1. Nyissa meg a mmc.exe.
2. Adja hozzá a hello **tanúsítványok** beépülő modult.
3. Jelölje be hello **számítógép** fiókot a helyi számítógép hello.
4. Kattintson a jobb gombbal hello **megbízható legfelső szintű hitelesítésszolgáltatók** csomópont. Kattintson a **minden-tevékenység** > **importálása**, és tallózással keresse meg toohello .cer fájl hello VPN-ügyfélcsomag konfigurációs kibontott.
5. Indítsa újra a hello számítógépet. 
6. Próbálja meg tooinstall hello VPN-ügyfél.

## <a name="azure-portal-error-failed-toosave-hello-vpn-gateway-and-hello-data-is-invalid"></a>Az Azure portál hiba: nem sikerült toosave hello VPN-átjárót, és hello adat érvénytelen

### <a name="symptom"></a>Jelenség

Amikor toosave hello változásokat a VPN-átjáró hello hello Azure-portálon, hello a következő hibaüzenet jelenhet meg:

**Virtuális hálózati átjáró hibás toosave &lt;* átjárónevet*&gt;. Tanúsítvány adatainak &lt; *tanúsítvány azonosító* &gt; van invalid.* *

### <a name="cause"></a>Ok 

Ez a probléma akkor fordulhat elő, ha hello legfelső szintű tanúsítvány nyilvános kulcsa feltöltött tartalmaz egy érvénytelen karakter, például egy szóközzel.

### <a name="solution"></a>Megoldás

Győződjön meg arról, hogy hello adatok hello tanúsítvány nem tartalmaz érvénytelen karaktereket, például sortörést (kocsivissza). hello egész értéknek kell lennie egy hosszú sor. a következő szöveg hello látható egy minta hello tanúsítvány:

    -----BEGIN CERTIFICATE-----
    MIIC5zCCAc+gAwIBAgIQFSwsLuUrCIdHwI3hzJbdBjANBgkqhkiG9w0BAQsFADAW
    MRQwEgYDVQQDDAtQMlNSb290Q2VydDAeFw0xNzA2MTUwMjU4NDZaFw0xODA2MTUw
    MzE4NDZaMBYxFDASBgNVBAMMC1AyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEF
    AAOCAQ8AMIIBCgKCAQEAz8QUCWCxxxTrxF5yc5uUpL/bzwC5zZ804ltB1NpPa/PI
    sa5uwLw/YFb8XG/JCWxUJpUzS/kHUKFluqkY80U+fAmRmTEMq5wcaMhp3wRfeq+1
    G9OPBNTyqpnHe+i54QAnj1DjsHXXNL4AL1N8/TSzYTm7dkiq+EAIyRRMrZlYwije
    407ChxIp0stB84MtMShhyoSm2hgl+3zfwuaGXoJQwWiXh715kMHVTSj9zFechYd7
    5OLltoRRDyyxsf0qweTFKIgFj13Hn/bq/UJG3AcyQNvlCv1HwQnXO+hckVBB29wE
    sF8QSYk2MMGimPDYYt4ZM5tmYLxxxvGmrGhc+HWXzMeQIDAQABozEwLzAOBgNVHQ8B
    Af8EBAMCAgQwHQYDVR0OBBYEFBE9zZWhQftVLBQNATC/LHLvMb0OMA0GCSqGSIb3
    DQEBCwUAA4IBAQB7k0ySFUQu72sfj3BdNxrXSyOT4L2rADLhxxxiK0U6gHUF6eWz
    /0h6y4mNkg3NgLT3j/WclqzHXZruhWAXSF+VbAGkwcKA99xGWOcUJ+vKVYL/kDja
    gaZrxHlhTYVVmwn4F7DWhteFqhzZ89/W9Mv6p180AimF96qDU8Ez8t860HQaFkU6
    2Nw9ZMsGkvLePZZi78yVBDCWMogBMhrRVXG/xQkBajgvL5syLwFBo2kWGdC+wyWY
    U/Z+EK9UuHnn3Hkq/vXEzRVsYuaxchta0X2UNRzRq+o706l+iyLTpe6fnvW6ilOi
    e8Jcej7mzunzyjz4chN0/WVF94MtxbUkLkqP
    -----END CERTIFICATE-----

## <a name="azure-portal-error-failed-toosave-hello-vpn-gateway-and-hello-resource-name-is-invalid"></a>Az Azure portál hiba: nem sikerült toosave hello VPN-átjáró, és hello erőforrás neve érvénytelen.

### <a name="symptom"></a>Jelenség

Amikor toosave hello változásokat a VPN-átjáró hello hello Azure-portálon, hello a következő hibaüzenet jelenhet meg: 

**Virtuális hálózati átjáró hibás toosave &lt;* átjárónevet*&gt;. Az erőforrásnév &lt; *tooupload meg a tanúsítvány neve* &gt; van érvénytelen **.

### <a name="cause"></a>Ok

A probléma oka, hogy hello tanúsítvány hello neve érvénytelen karaktert, például egy szóközzel tartalmaz. 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a>Az Azure portál hiba: VPN csomag fájl letöltési hiba 503-as

### <a name="symptom"></a>Jelenség

Amikor toodownload konfigurációs hello VPN-ügyfélcsomag, hello a következő hibaüzenet jelenhet meg:

**Nem sikerült toodownload hello fájlt. Hiba legutolsó részletes adatai: 503-as hiba. hello kiszolgáló túlterhelt.**
 
### <a name="solution"></a>Megoldás

Ez a hiba átmeneti hálózati probléma okozhatja. Toodownload hello VPN csomag próbálkozzon újra néhány perc múlva.

## <a name="azure-vpn-gateway-upgrade-all-p2s-clients-are-unable-tooconnect"></a>Az Azure VPN Gateway frissítése: minden P2S-ügyfelek nem tooconnect

### <a name="cause"></a>Ok

Ha hello tanúsítvány több mint 50 %-a teljes élettartama keresztül hello tanúsítvány frissítése.

### <a name="solution"></a>Megoldás

tooresolve probléma létrehozása, és újra elosztják a VPN-ügyfelek új tanúsítványok toohello. 

## <a name="too-many-vpn-clients-connected-at-once"></a>Túl sok a VPN-ügyfelek egyszerre csatlakoztatva

Minden egyes VPN-átjáró hello engedélyezett kapcsolatok maximális száma 128 esetén. Láthatja, hogy hello hello Azure-portálon a csatlakoztatott ügyfelek teljes száma.

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-toohello-route-table"></a>Pont – hely típusú VPN helytelenül 10.0.0.0/8 toohello útvonaltábla útvonal hozzáadása

### <a name="symptom"></a>Jelenség

Amikor hello hello pont-pont ügyfél VPN-kapcsolatot, a hello VPN-ügyfél kell hello Azure-beli virtuális hálózat felé útvonal hozzáadása. hello IP-segítő szolgáltatás hozzá kell adnia egy útvonal hello alhálózat hello VPN-ügyfelek. 

VPN-ügyfél tartomány hello tooa kisebb alhálózata 10.0.0.0/8, például a 10.0.12.0/24 tartozik. Helyett 10.0.12.0/24 útvonal, a 10.0.0.0/8 adjon hozzá egy útvonalat magasabb prioritással bír, amely. 

Ez helytelen az útvonal más hálózatokkal helyszíni tooanother alhálózati tartományba hello 10.0.0.0/8, például a 10.50.0.0/24, előfordulhat, hogy tartoznak, amelyek nem rendelkeznek egy adott útvonal definiálva kapcsolat megszakad. 

### <a name="cause"></a>Ok

Ez szándékosan van a Windows-ügyfelek. Hello ügyfél hello PPP IPCP protokollt használja, amikor hello kiszolgálótól (ebben az esetben hello VPN-átjáró) beszerzett hello alagútkapcsolat hello IP-címet. Azonban hello protokoll korlátozása, miatt a hello ügyfél nem rendelkezik hello alhálózati maszkot. Mivel nincs más módja tooget, hello ügyfél megkísérli tooguess hello alhálózati maszk hello osztály hello alagút illesztő IP-cím alapján. 

Ezért adjon hozzá egy útvonalat hello statikus leképezés a következő alapján: 

Ha a cím tartozik tooclass A--> alkalmazása /8

Ha a cím tartozik B--> tooclass alkalmazása /16

Ha a cím tartozik C--> tooclass alkalmazása /24

## <a name="vpn-client-cannot-access-network-file-shares"></a>VPN-ügyfél nem tud hozzáférni a hálózati fájlmegosztások

### <a name="symptom"></a>Jelenség

hello VPN-ügyfél csatlakozott toohello Azure-beli virtuális hálózat. Hello ügyfél azonban nem tud hozzáférni hálózati megosztások.

### <a name="cause"></a>Ok

fájlmegosztási hozzáférést hello SMB protokoll használható. Hello kapcsolat elindításakor hello VPN-ügyfél hozzáadása hello munkamenet hitelesítő adatokat, és hello hiba történik. Hello kapcsolat létrejötte után hello ügyfél kényszeríti toouse hello gyorsítótár Kerberos-hitelesítés hitelesítő adatait. A folyamat elindul a lekérdezések toohello kulcsszolgáltató (tartományvezérlő) tooget jogkivonatot. Mivel hello ügyfél csatlakozik az internethez hello, nem feltétlenül tudja tooreach hello tartományvezérlő. Ezért hello ügyfél nem feladatátvételt a Kerberos tooNTLM. 

hello csak időt, hogy az ügyfél hello kéri a hitelesítő adatok esetén érvényes tanúsítvánnyal rendelkezik (tárolóhálózathoz = UPN) kiállító hello tartomány toowhich tartományhoz. hello ügyfél is fizikailag csatlakoztatott toohello tartományi hálózaton kell lennie. Ebben az esetben hello ügyfél megpróbál toouse hello tanúsítványt, és egészítse ki toohello tartományvezérlő. Hello kulcsszolgáltató majd egy "KDC_ERR_C_PRINCIPAL_UNKNOWN" hibaüzenetet adja vissza. hello ügyfél kényszerített toofail tooNTLM felett. 

### <a name="solution"></a>Megoldás

toowork hello a probléma, tiltsa le a tartományi hitelesítő adatait a következő beállításkulcsot hello gyorsítótárazását hello: 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set hello value too1 


## <a name="cannot-find-hello-point-to-site-vpn-connection-in-windows-after-reinstalling-hello-vpn-client"></a>Nem található a Windows hello pont-pont VPN-kapcsolat hello VPN-ügyfél újratelepítése után

### <a name="symptom"></a>Jelenség

Távolítsa el a hello pont-pont VPN-kapcsolatot, és telepítse a hello VPN-ügyfél. Ebben a helyzetben hello VPN-kapcsolat beállítása nem sikerült. Nem látja hello VPN-kapcsolatot a hello **hálózati kapcsolat** a Windows beállításai.

### <a name="solution"></a>Megoldás

tooresolve hello problémát, törölje a hello régi VPN ügyfél konfigurációs fájlokat a **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, majd futtassa újra a hello VPN-ügyfél telepítőjét.
