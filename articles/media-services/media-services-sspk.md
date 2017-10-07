---
title: "Microsoft® Smooth Streaming ügyfél eljárás Kit aaaLicensing"
description: "További információk a hogyan toolicensing hello Microsoft® Smooth Streaming ügyfél eljárás készlet."
services: media-services
documentationcenter: 
author: xpouyat
manager: cfowler
editor: 
ms.assetid: e3b488e7-8428-4c10-a072-eb3af46c82ad
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: xpouyat
ms.openlocfilehash: 56c3dccda73dd02207bb4dbe8109ba6fda917a6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Licencelési Microsoft® zökkenőmentes adatfolyam ügyfél Kit eljárás
## <a name="overview"></a>Áttekintés
Microsoft Smooth Streaming ügyfél eljárás Kit (**SSPK** röviden) a Smooth Streaming ügyfél megvalósítása beágyazott optimalizált toohelp eszközgyártók, a kábel és a mobil operátorok, a tartalom szolgáltatók, a kézibeszélő gyártók, független szoftvergyártók (ISV-k), és megoldást szolgáltatók toocreate termékek és szolgáltatások folyamatos adaptív adatfolyam-tartalmak Smooth Streaming formátumban. SSPK egy Smooth Streaming, hello licencbe tooany és a platformok által is tartalomfájlokat ügyfél és a platformok független megvalósítása. 

Alábbi egy magas szintű architektúra és IIS Smooth Streaming eljárás Kit hello Smooth Streaming ügyfél megvalósítása a Microsoft által biztosított és Smooth Streaming tartalom lejátszását az összes hello alapvető logikát tartalmaz. Ez az majd legelterjedtebb partnerek egy adott eszköz vagy a platform által megfelelő felületek alkalmazásával. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

## <a name="description"></a>Leírás
SSPK licencelt által biztosított érték kiváló üzleti igényei szerint. SSPK licenc hello iparág biztosítja:

* A c++ Smooth Streaming eljárás Kit forrás 
  * megvalósítja a Smooth Streaming ügyfélfunkciókat
  * hozzáadja a formátum elemzése, heurisztikus pufferelés logika stb.
* Lejátszóalkalmazás API-k 
  * az alkalmazásprogramozási felületek és egy media player alkalmazás közötti interakció
* Platform absztrakciós réteg (PAL) kapcsolat 
  * az alkalmazásprogramozási felületek (szálak, sockets) hello operációs rendszerrel folytatott kommunikációjuktól
* Hardver absztrakciós réteg (HAL) kapcsolat 
  * programozási felületek kölcsönhatások a hardver A / V dekóderek (dekódolás, megjelenítése)
* Digitális tartalomvédelmi (DRM) felügyeleti felület 
  * alkalmazásprogramozási felületek hello DRM-absztrakciós réteg (DAL) keresztül DRM kezelése
  * Microsoft PlayReady eljárás Kit külön érhető el, de integrálja a felületen keresztül. Microsoft PlayReady Device engedélyezéséről további információért kattintson [Itt](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
* Megvalósítása – minták 
  * a minta PAL implementációja Linux
  * a minta HAL implementációja GStreamer

## <a name="licensing-options"></a>Licencelési lehetőségek
Microsoft Smooth Streaming ügyfél eljárás készlet két különböző licencszerződések alapján történik elérhető toolicensees: egy a Smooth Streaming ügyfél ideiglenes termékek és egy másikat a Smooth Streaming ügyfél végső termékek tooend felhasználók terjesztése.

* Lapkakészlet gyártók, rendszerintegrátorok vagy független keresve igénylő forrás kód eljárás kit toodevelop ideiglenes termékek, a Microsoft Smooth Streaming ügyfél eljárás Kit **ideiglenes terméklicenc** kell végrehajtani.
* Az eszközgyártók vagy Smooth Streaming ügyfél végső termékek tooend felhasználók, terjesztési jogokat igénylő ISV-k hello Microsoft Smooth Streaming ügyfél eljárás Kit **végső terméklicenc** végre kell hajtani.

### <a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft Smooth Streaming Kit ideiglenes terméklicenc eljárás ügyfél
Ezt a licencet, a Microsoft nyújt egy Smooth Streaming ügyfél eljárás Kit és szükséges szellemi tulajdonra vonatkozó jog toodevelop hello, és terjesztése Smooth Streaming ügyfél ideiglenes termékek tooother Smooth Streaming ügyfél eljárás Kit eszköz licenctulajdonosok esetében, amelyek terjessze a Smooth Streaming ügyfél végső termékek.

#### <a name="fee-structure"></a>Díjszabása
USA $ 50 000 egyszeri licenc anélkül biztosít hozzáférést toohello Smooth Streaming ügyfél eljárás Kit. 

### <a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft Smooth Streaming Kit végleges terméklicenc eljárás ügyfél
Ezt a licencet, a Microsoft minden szükséges szellemi tulajdonra vonatkozó jog tooreceive Smooth Streaming ügyfél ideiglenes termékek kínál más Smooth Streaming ügyfél eljárás Kit licenctulajdonosok esetében és a vállalat védjegyét Smooth Streaming ügyfél végső toodistribute Termékek tooend felhasználók.

#### <a name="fee-structure"></a>Díjszabása
hello Smooth Streaming ügyfél végső termék alatt, a kiemelt modellben kínálják:

* $0.10 / rendszerrel szállított eszköz végrehajtása
* hello kiemelt tárfiókonként maximum 50 000 $ minden évben
* Nincsenek kiemelt első 10 000 eszköz-megvalósítások esetében minden évben 

## <a name="licensing-procedure-and-sspk-access"></a>Licencelési eljárás és SSPK hozzáférés
Írjon e-mailt [ sspkinfo@microsoft.com ](mailto:sspkinfo@microsoft.com) összes licencelési lekérdezések.

Hello [SSPK terjesztési portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) van elérhető tooregistered ideiglenes licenctulajdonosok esetében.

Ideiglenes és végső SSPK licenctulajdonosok esetében túl is elküldhetik technikai kérdésekkel kapcsolatos[smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).

## <a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft Smooth Streaming ügyfél ideiglenes termék megállapodás licenctulajdonosok esetében
* Adroit üzleti megoldások, Inc
* Speciális digitális szórás rendszergazdai (SA)
* AirTies Kablosuz Iletism Sanayive hagyása Ticaret A.S.
* Albis technológiák Ltd
* Alticast Corporation
* Amazon digitális szolgáltatások, Inc.
* Arion technológia, Inc.
* AVC multimédia szoftver Co., Ltd
* Cavium, Inc.
* EchoStar megvásárlásáról Corporation
* Enseo, Inc.
* Dél Fluendo
* HANDAN BroadInfoCom Co., Ltd
* Infomir GMBH
* Irdeto USA Inc.
* Dél iWEDIA 
* Liberty globális szolgáltatások BV
* MediaTek Inc.
* MStar Co, Ltd
* Nintendo Co., Ltd
* OpenTV, Inc.
* Sáfrány digitális korlátozott
* Szecsuan Changhong elektromos Co., Ltd
* SoftAtHome
* Sony Corporation
* Tatung technológia Inc.
* TCL Technoly Electronics (Huizhou) Co., Ltd
* Felső győzelem beruházások értékét, Ltd
* Ticaret A.S. Vestel Elektronik Sanayi megtörtént
* VisualOn, Inc.
* ZTE Corporation

## <a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft Smooth Streaming ügyfél végső termék megállapodás licenctulajdonosok esetében
* Speciális digitális szórás rendszergazdai (SA)
* AirTies Kablosuz Iletism Sanayive hagyása Ticaret A.S.
* Albis technológiák Ltd
* Amazon digitális szolgáltatások, Inc.
* AmTRAN technológia Co., Ltd
* Arcadyan technológia Corporation
* Arion technológia, Inc.
* ATMACA ELEKTRONİK SAN. TİC MEGTÖRTÉNT. A.Ş
* Brit égbolt szórásos korlátozott
* CastPal technológia Inc. Shenzhen
* Compal Electronics, Inc.
* Dongguan digitális AV technológia Corp., Ltd
* EchoStar megvásárlásáról Corporation
* Enseo, Inc.
* Filmflex filmek korlátozott
* Dél Fluendo
* Gibson Innovációinak korlátozott
* Haier információk Applicantion S.R.L
* HANDAN BroadInfoCom Co., Ltd
* Nemzetközi Hisense, Reinhard 
* Homecast Co., Ltd
* HON Hai pontosság iparági Co., Ltd
* Infomir GMBH
* Kaonmedia Co., Ltd
* KDDI Corporation
* Nintendo Co., Ltd
* Narancssárga rendszergazdai (SA)
* Sáfrány digitális korlátozott
* Sagemcom szélessávú SAS
* Shenzhen Coship Electronics CO., LTD
* Shenzhen Jiuzhou elektromos Co., Ltd
* Shenzhen Skyworth digitális technológia Co., Ltd
* Szecsuan Changhong elektromos Co., Ltd
* Skardin ipari vállalat esetében.
* Égbolt Deutschland Fernsehen GmbH & Co. KG
* Dél SmarDTV
* SoftAtHome
* Sony Corporation
* Korlátozott TCL tengerentúli Marketing (tengeri Makaó kereskedelmi)
* Technicolor szállítási technológiák, SAS
* Tongfang globális Ltd
* Felső győzelem beruházások értékét, Ltd
* Toshiba Lifestyle termékek és szolgáltatások Corporation
* Univerzális Media Corporation /Slovakia/ s.r.o.
* VIZIO, Inc.
* Wistron Corporation
* ZTE Corporation


## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

