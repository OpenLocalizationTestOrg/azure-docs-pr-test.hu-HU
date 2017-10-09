---
title: "Készítése és a pont-pont tanúsítványok exportálása: MakeCert: Azure |} Microsoft Docs"
description: "A cikkben lépéseket toocreate önaláírt legfelső szintű tanúsítvány, és hello nyilvános kulcsának exportálásához a MakeCert használatával ügyféltanúsítványok előállításához."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: 0b795ccf74f1f97fbd3de8a0dc339f9cb0b48183
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-makecert"></a>Létrehozása és exportálása a MakeCert használatával pont – hely kapcsolatok tanúsítványai

Pont – hely kapcsolatok tanúsítványok tooauthenticate használja. Ez a cikk bemutatja, hogyan toocreate önaláírt a legfelső szintű tanúsítványt, és a MakeCert használatával ügyféltanúsítványok előállításához. Ha a pont-hely konfigurációs lépések, például hogyan tooupload legfelső szintű tanúsítványok, a következő hello hello "konfigurálása pont-pont" cikkek válasszon ki egy listából:

> [!div class="op_single_selector"]
> * [Hozzon létre önaláírt tanúsítványokat - PowerShell](vpn-gateway-certificates-point-to-site.md)
> * [Hozzon létre önaláírt tanúsítványokat - MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Pont – hely - Resource Manager - Azure-portál konfigurálása](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Pont – hely - erőforrás-kezelő - PowerShell konfigurálása](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Pont – hely - klasszikus - Azure-portál konfigurálása](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Amíg hello használata javasolt [Windows 10 PowerShell lépéseket](vpn-gateway-certificates-point-to-site.md) toocreate a tanúsítványok nyújtunk MakeCert utasítások választható módszerként. hello tanúsítványok módszerek használatával generáló telepíthető [bármely támogatott ügyfél operációs rendszer](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq). Azonban a MakeCert korlátozása a következő hello rendelkezik:

* MakeCert elavult. Ez azt jelenti, hogy sikerült-e az eszköz eltávolítani bármely pontján. Már létrehozott MakeCert használatával tanúsítványokat nem érinti, ha a MakeCert már nem érhető el. MakeCert nem csak a felhasznált toogenerate hello tanúsítványok érvényesítése mechanizmusaként.

## <a name="rootcert"></a>Hozzon létre egy önaláírt legfelső szintű tanúsítványt

hello lépések bemutatják, hogyan toocreate egy önaláírt tanúsítvány MakeCert használatával. Ezeket a lépéseket csak vonatkozó központi telepítési modellt. Akkor érvényesek az erőforrás-kezelő és a klasszikus.

1. Töltse le és telepítse [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx).
2. A telepítés után általában található hello makecert.exe segédprogramot az elérési úton: "C:\Program Files (x86) \Windows Kits\10\bin\<architektúrája >". Bár, lehetséges, hogy volt-e telepített tooanother helyét. Nyisson meg egy parancssort rendszergazdaként, és keresse meg a MakeCert segédprogram hello toohello helyét. A következő példa, beállítása a megfelelő hely hello hello használhatja:

  ```cmd
  cd C:\Program Files (x86)\Windows Kits\10\bin\x64
  ```
3. Hozzon létre és telepítsen egy tanúsítványt a hello személyes tanúsítványtárolójához azon a számítógépen. hello alábbi példakód létrehozza a megfelelő *.cer* fájl tooAzure feltöltése P2S konfigurálásakor. Cserélje le a "P2SRootCert" és "P2SRootCert.cer" hello nevű megjeleníteni kívánt toouse hello tanúsítvány. a "tanúsítványok - aktuális User\Personal\Certificates" Hello tanúsítvány található.

  ```cmd
  makecert -sky exchange -r -n "CN=P2SRootCert" -pe -a sha256 -len 2048 -ss My
  ```

## <a name="cer"></a>Exportálás hello nyilvános kulcsát (.cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

hello exported.cer fájl feltöltött tooAzure kell lennie. Útmutatásért lásd: [egy pont – hely kapcsolat beállítása](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile). egy további megbízható legfelső szintű tanúsítvány tooadd lásd [ebben a szakaszban](vpn-gateway-howto-point-to-site-resource-manager-portal.md#add) hello cikk.

### <a name="export-hello-self-signed-certificate-and-private-key-toostore-it-optional"></a>Hello önaláírt tanúsítvány és titkos kulcs toostore exportálja azt (nem kötelező)

Érdemes lehet tooexport hello önaláírt a legfelső szintű tanúsítványt, és tárolja biztonságos helyen. Ha kell, később egy másik számítógépre telepítse, és további ügyféltanúsítványok előállításához vagy egy másik .cer-fájl exportálását. tooexport hello önaláírt legfelső szintű tanúsítvány, egy .pfx, jelölje be hello legfelső szintű tanúsítvány és használata hello ugyanazokat a lépéseket a [ügyféltanúsítvány exportálása](#clientexport).

## <a name="create-and-install-client-certificates"></a>Hozzon létre és telepítsen ügyfél-tanúsítványok

Önaláírt tanúsítvány hello közvetlenül ügyfélszámítógépen hello ne telepítse. Toogenerate hello önaláírt tanúsítványt az ügyféltanúsítvány szükséges. Majd exportálja és az hello ügyfél tanúsítvány toohello ügyfélszámítógép telepítse. a lépéseket követve hello nincsenek telepítési-modell specifikus. Akkor érvényesek az erőforrás-kezelő és a klasszikus.

### <a name="clientcert"></a>Ügyfél-tanúsítvány létrehozása

Minden ügyfélszámítógép, amely a tooa VNet pont-pont használatával telepített ügyféltanúsítvánnyal kell rendelkeznie. Hozzon létre egy ügyféltanúsítványt hello önaláírt legfelső szintű tanúsítványt, és exportálni, és hello telepítéséhez. Ha hello ügyféltanúsítvány nincs telepítve, a hitelesítés meghiúsul. 

hello következő lépések végigvezetik történő tanúsítványgenerálás során egy ügyfél egy önaláírt legfelső szintű tanúsítványt. Előfordulhat, hogy több ügyféltanúsítványt generálása hello ugyanazon főtanúsítványhoz. Ügyféltanúsítványok alábbi hello lépéseket használhatja generálásakor hello ügyféltanúsítvány automatikusan számítógépen hello toogenerate hello tanúsítvány használatát. Ha azt szeretné, hogy egy másik ügyfélszámítógépen ügyféltanúsítványt tooinstall, exportálhatja hello tanúsítványt.
 
1. A hello ugyanazon a számítógépen, hogy használja-e toocreate hello önaláírt tanúsítványt, nyissa meg a parancssort rendszergazdaként.
2. Módosíthatja, és futtassa a hello minta toogenerate ügyféltanúsítványt.
  * Változás *"P2SRootCert"* hello önaláírt legfelső szintű hello ügyféltanúsítványt a generál toohello nevét. Ellenőrizze, hogy hello legfelső szintű tanúsítvány, amely bármilyen hello hello nevét használja "CN =' érték volt, hogy a megadott hello önaláírt legfelső szintű létrehozásakor.
  * Változás *P2SChildCert* azt szeretné, hogy egy ügyfél-tanúsítvány toobe toogenerate toohello nevét.

  Ha futtatja a következő példa azt a módosítása nélkül hello, hello eredménye a személyes tanúsítványtárolóban lett létrehozva, a legfelső szintű tanúsítvány P2SRootCert P2SChildcert nevű ügyfél-tanúsítványt.

  ```cmd
  makecert.exe -n "CN=P2SChildCert" -pe -sky exchange -m 96 -ss My -in "P2SRootCert" -is my -a sha256
  ```

### <a name="clientexport"></a>Ügyfél-tanúsítvány exportálása

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

### <a name="install"></a>Az exportált ügyféltanúsítvány telepítése

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a>Következő lépések

A pont-hely konfigurációs folytatásához. 

* A **erőforrás-kezelő** telepítési modell lépéseket lásd: [konfigurálása egy pont – hely kapcsolat tooa VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* A **klasszikus** telepítési modell lépéseket lásd: [konfigurálása egy pont – hely típusú VPN-kapcsolat tooa hálózatok (klasszikus)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).
