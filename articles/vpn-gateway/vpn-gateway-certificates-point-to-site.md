---
title: "Készítése és a pont-pont tanúsítványok exportálása: PowerShell: Azure |} Microsoft Docs"
description: "A cikkben lépéseket toocreate önaláírt legfelső szintű tanúsítvány hello nyilvános kulcsának exportálásához és a PowerShell-lel Windows 10 ügyféltanúsítványok előállításához."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 27b99f7c-50dc-4f88-8a6e-d60080819a43
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: 11dda015368cda5ce9799fcc4f01d7c542b84fe8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-powershell-on-windows-10"></a>Létrehozása és exportálása a tanúsítványok a PowerShell-lel Windows 10 pont – hely kapcsolatok

Pont – hely kapcsolatok tanúsítványok tooauthenticate használja. Ez a cikk azt ismerteti, hogyan toocreate önaláírt a legfelső szintű tanúsítványt, és a PowerShell-lel Windows 10 ügyféltanúsítványok előállításához. Ha a pont-hely konfigurációs lépések, például hogyan tooupload legfelső szintű tanúsítványok, a következő hello hello "konfigurálása pont-pont" cikkek válasszon ki egy listából:

> [!div class="op_single_selector"]
> * [Hozzon létre önaláírt tanúsítványokat - PowerShell](vpn-gateway-certificates-point-to-site.md)
> * [Hozzon létre önaláírt tanúsítványokat - MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Pont – hely - Resource Manager - Azure-portál konfigurálása](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Pont – hely - erőforrás-kezelő - PowerShell konfigurálása](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Pont – hely - klasszikus - Azure-portál konfigurálása](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 


Ebben a cikkben a Windows 10 rendszerű számítógépre hello lépéseket kell végrehajtania. hello PowerShell-parancsmagok használata toogenerate tanúsítványok hello Windows 10 operációs rendszer része, és a Windows más verziói nem működnek. Windows 10 hello számítógép csak a szükséges toogenerate hello tanúsítványok. Hello tanúsítványok jönnek létre, ha feltöltheti ezeket, vagy telepítse őket a támogatott ügyfél operációs rendszereken. 

Ha Ön nem rendelkezik hozzáféréssel a Windows 10 tooa számítógép, [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) toogenerate tanúsítványokat. hello tanúsítványok módszerek használatával generáló telepíthetők bármely [támogatott](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) ügyfél operációs rendszerét.

## <a name="rootcert"></a>Hozzon létre egy önaláírt legfelső szintű tanúsítványt

Hello New-SelfSignedCertificate parancsmag toocreate önaláírt legfelső szintű tanúsítvány használatára. További információkért lásd: [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

1. Windows 10 rendszerű számítógépeken nyisson meg egy Windows PowerShell-konzolt emelt szintű jogosultságokkal.
2. A következő példa toocreate hello önaláírt legfelső szintű tanúsítvány hello használata. hello alábbi példa létrehoz egy önaláírt legfelső szintű tanúsítványt "P2SRootCert", amely automatikusan telepíti a "Tanúsítványok-aktuális User\Personal\Certificates" nevű. Hello tanúsítvány megtekintéséhez nyissa meg *certmgr.msc*, vagy *kezelheti a felhasználói tanúsítványok*.

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <a name="cer"></a>Exportálás hello nyilvános kulcsát (.cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

hello exported.cer fájl feltöltött tooAzure kell lennie. Útmutatásért lásd: [egy pont – hely kapcsolat beállítása](vpn-gateway-howto-point-to-site-rm-ps.md#upload). egy további megbízható legfelső szintű tanúsítvány tooadd [ebben a szakaszban](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) hello cikk.

### <a name="export-hello-self-signed-root-certificate-and-public-key-toostore-it-optional"></a>Hello önaláírt legfelső szintű tanúsítvány és nyilvános kulcs toostore exportálja azt (nem kötelező)

Érdemes lehet tooexport hello önaláírt a legfelső szintű tanúsítványt, és tárolja biztonságos helyen. Ha kell, később egy másik számítógépre telepítse, és további ügyféltanúsítványok előállításához vagy egy másik .cer-fájl exportálását. tooexport hello önaláírt legfelső szintű tanúsítvány, egy .pfx, jelölje be hello legfelső szintű tanúsítvány és használata hello ugyanazokat a lépéseket a [ügyféltanúsítvány exportálása](#clientexport).

## <a name="clientcert"></a>Ügyfél-tanúsítvány létrehozása

Minden ügyfélszámítógép, amely a tooa VNet pont-pont használatával telepített ügyféltanúsítvánnyal kell rendelkeznie. Hozzon létre egy ügyféltanúsítványt hello önaláírt legfelső szintű tanúsítványt, és exportálni, és hello telepítéséhez. Ha hello ügyféltanúsítvány nincs telepítve, a hitelesítés meghiúsul. 

hello következő lépések végigvezetik történő tanúsítványgenerálás során egy ügyfél egy önaláírt legfelső szintű tanúsítványt. Előfordulhat, hogy több ügyféltanúsítványt generálása hello ugyanazon főtanúsítványhoz. Ügyféltanúsítványok alábbi hello lépéseket használhatja generálásakor hello ügyféltanúsítvány automatikusan számítógépen hello toogenerate hello tanúsítvány használatát. Ha azt szeretné, hogy egy másik ügyfélszámítógépen ügyféltanúsítványt tooinstall, exportálhatja hello tanúsítványt.

a példákban hello hello New-SelfSignedCertificate parancsmag toogenerate egy ügyféltanúsítványt, amely egy év lejár. További információkat, például a különböző lejárati érték hello ügyféltanúsítványt, lásd: [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

### <a name="example-1"></a>1. példa

A példa az előző szakasz hello "$cert" változó deklarált hello. Ha korábban bezárta hello PowerShell konzol létrehozása önaláírt legfelső szintű tanúsítvány hello, vagy egy új PowerShell-konzol munkamenetet további ügyfél tanúsítványok készíti után, hajtsa végre a hello lépéseket [2. példa](#ex2).

Módosíthatja, és futtassa a hello példa toogenerate ügyféltanúsítványt. Ha az alábbi példa azt a módosítása nélkül hello futtatja, hello eredménye "P2SChildCert" nevű ügyfél-tanúsítványt.  Ha azt szeretné, hogy tooname hello a tanúsítványt valami mást, módosítsa a hello CN-értéket. Ne változtassa hello TextExtension ebben a példában futtatásakor. Létrehozhat hello ügyféltanúsítvány "Tanúsítványok - aktuális User\Personal\Certificates" automatikusan települ a számítógépre.

```powershell
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <a name="ex2"></a>2. példa

Ha Ön által létrehozott további ügyfél tanúsítványokat, és vannak nem használ hello azonos PowerShell-munkamenetet, amellyel toocreate a önaláírt főtanúsítványt, használja hello a következő lépéseket:

1. Azonosítsa a hello önaláírt főtanúsítványt hello számítógépen telepítve van. Ez a parancsmag a számítógépen telepített tanúsítványok listáját adja vissza.

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. Keresse meg a hello tulajdonos nevét a listában, majd a Másolás hello ujjlenyomat, amely található következő tooit tooa szöveg visszaadott hello fájlt. A következő példa hello két tanúsítványokat is. hello CN-név, amelyből el kívánja toogenerate gyermek tanúsítvány hello önaláírt legfelső szintű tanúsítvány hello neve. Ebben az esetben "P2SRootCert".

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
3. Hello legfelső szintű tanúsítvány hello ujjlenyomata hello előző lépésben változó deklarálható. Cserélje le UJJLENYOMATÁT hello hello főtanúsítványt, amelyből el kívánja toogenerate gyermek tanúsítvány ujjlenyomata.

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  Például P2SRootCert hello előző lépésben hello ujjlenyomat alkalmaz, hello változó néz ki:

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```
4.  Módosíthatja, és futtassa a hello példa toogenerate ügyféltanúsítványt. Ha az alábbi példa azt a módosítása nélkül hello futtatja, hello eredménye "P2SChildCert" nevű ügyfél-tanúsítványt. Ha azt szeretné, hogy tooname hello a tanúsítványt valami mást, módosítsa a hello CN-értéket. Ne változtassa hello TextExtension ebben a példában futtatásakor. Létrehozhat hello ügyféltanúsítvány "Tanúsítványok - aktuális User\Personal\Certificates" automatikusan települ a számítógépre.

  ```powershell
  New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <a name="clientexport"></a>Ügyfél-tanúsítvány exportálása   

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

## <a name="install"></a>Az exportált ügyféltanúsítvány telepítése

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a>Következő lépések

A pont-hely konfigurációs folytatásához. 

* A **erőforrás-kezelő** telepítési modell lépéseket lásd: [konfigurálása egy pont – hely kapcsolat tooa VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md). 
* A **klasszikus** telepítési modell lépéseket lásd: [konfigurálása egy pont – hely típusú VPN-kapcsolat tooa hálózatok (klasszikus)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).
