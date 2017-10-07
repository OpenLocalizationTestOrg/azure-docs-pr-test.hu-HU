---
title: "aaaUnderstand az Azure részletes használati |} Microsoft Docs"
description: "Megtudhatja, hogyan tooread és a részletes használati CSV Azure-előfizetése hello szakaszait ismertetése"
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: tonguyen
ms.openlocfilehash: c9284bf94bfa9f36cdd5d39e653a35def7c1aa34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-terms-on-your-microsoft-azure-detailed-usage-charges"></a>A Microsoft Azure részletes használati költségek feltételek megismerése 
hello részletes használati költségek CSV-fájl naponta tartalmaz, és a jelenlegi számlázási időszak hello mérő szintű használati díja. 

tooget a részletes használati fájlt, tekintse meg [hogyan tooget az Azure számlázás számla és a napi használati adatok](billing-download-azure-invoice-daily-usage-date.md).
Rendszerben elérhető megnyithat egy táblázat alkalmazásban vesszővel tagolt (.csv) formátumban. Ha elérhető két verzióját látja, töltse le a 2-es verzióját. Ez az aktuális fájlformátumban hello.

Használati díjak teljes hello **havi** költségek az előfizetést. A használati díjak ne vegye figyelembe, bármely kreditek és az esetleges kedvezményeket.

## <a name="detailed-terms-and-descriptions-of-your-detailed-usage-file"></a>Részletes feltételeket és a részletes használati fájl leírása
hello következő részek a hello fontos fogalmakat tartalmazza a 2-es verziójának hello részletes használati fájl látható.

### <a name="statement"></a>Utasítás
hello hello felső részében részletes használati CSV látható hello Fájlszolgáltatások hello hónap számlázási időszakban használt. hello következő táblázatban hello feltételek és a jelen szakasz útmutatásai leírása.

| Időtartam | Leírás |
| --- | --- |
|Billing Period (Számlázási időszak) |Ha hello mérőszámok használt hello számlázási időszakban |
|Meter Category (Mérési kategória) |Hello legfelső szintű szolgáltatást hello használati azonosító |
|Meter Sub-Category (Mérési alkategória) |Azure-szolgáltatások, amelyek hatással lehetnek a hello arány hello típusú meghatározása |
|Meter Name (Mérés neve) |Hello mértékegységét feldolgozottként hello mérő azonosítja. |
|Meter Region (Mérési régió) |Hello adatközpontban, és olyan szolgáltatásokat, amelyek áron datacenter helye alapján hello helyét azonosítja. |
|SKU |Hello egyedi rendszerazonosító minden Azure mérő azonosítja. |
|Unit (Egység) |Hello hello szolgáltatás fel van töltve a egység azonosítja. Ha például GB, óra, 10 000 s. |
|Consumed Quantity (Felhasznált mennyiség) |hello számlázási időszakban használt hello mérő hello mennyisége |
|Included Quantity (Bennefoglalt mennyiség) |az aktuális elszámolási időszak díjmentesen részét képező hello mérő hello mennyisége |
|Overage Quantity (Kereten túli mennyiség) |Látható hello különbség közötti felhasznált mennyiség hello és hello mennyiség tartalmazza. Díjon számlázzuk, az ezt a mennyiséget. A hello ajánlat nem befoglalt mennyiséggel ajánlatok – használatalapú fizetés az összesített van hello hello felhasznált mennyiség szerint. |
|Within Commitment (Keretfelhasználás) |Hello mérő költségek vannak vonni a 6-os vagy 12 hónap ajánlatát rendelt előfizetési összegét mutatja. A mérési díjak időrendi sorrendben vannak kivonva. |
|Currency (Pénznem) |az aktuális elszámolási időszak használt hello pénznem |
|Overage (Kereten túli díjak) |Hello mérő költségek, amelyek meghaladják a 6-os vagy 12 hónap ajánlatát rendelt előfizetési összegét mutatja |
|Commitment Rate (Keretkihasználtság) |Hello kötelezettségvállalás arány hello teljes kötelezettségvállalás összeg 6 és 12 hónap ajánlatát társított alapján jeleníti meg |
|Rate (Egységár) |most számítjuk fel számlázható egységenként hello arány |
|Érték |Összeszorozza hello Túlhasználati mennyiség oszlop hello arány oszlop szerint hello eredményét mutatja. Ha felhasznált mennyisége nem haladja meg hello hello mennyiség része, akkor nem kell fizetni ebben az oszlopban. |

### <a name="daily-usage"></a>Napi használat

hello napi használati adatai hello CSV-fájl hello számlázási díjszabás érintő használati részleteit jeleníti meg. hello következő táblázatban hello feltételek és a jelen szakasz útmutatásai leírása.

| Időtartam | Leírás |
| --- | --- |
|Usage Date (Használat dátuma) |hello mérő használatakor hello dátuma |
|Meter Category (Mérési kategória) |Hello legfelső szintű szolgáltatást, amelynek használata tartozik azonosító |
|Meter ID (Mérési azonosító) |hello számlázva által használt tooprice számlázási használat mérési azonosítója |
|Meter Sub-Category (Mérési alkategória) |Határozza meg, amelyek hatással lehetnek a hello arány hello Azure szolgáltatás típusa |
|Meter Name (Mérés neve) |Hello mértékegységét feldolgozottként hello mérő azonosítja. |
|Meter Region (Mérési régió) |Hello adatközpontban, és olyan szolgáltatásokat, amelyek áron datacenter helye alapján hello helyét azonosítja. |
|Unit (Egység) |Azonosítja hello egység számítjuk fel az adott hello mérni. Ha például GB, óra, 10 000 s. |
|Consumed Quantity (Felhasznált mennyiség) |hello mérni, hogy az adott napon felhasználta hello mennyisége |
|Resource Location (Erőforrás helye) |Azonosítja a hello mérő futtató hello datacenter |
|Consumed Service (Használt szolgáltatás) |hello Azure platformszolgáltatás, amely használt |
|Erőforráscsoport |hello erőforráscsoport mely hello a telepített mérő futtatja. <br/><br/>További információk: [Azure Resource Manager overview](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) (Az Azure Resource Manager áttekintése). |
|Instance ID (Példányazonosító) | hello mérő hello azonosítója. <br/><br/> hello azonosítója hello mérő létrehozásakor megadott hello nevét tartalmazza. Vagy hello hello erőforrás nevét, vagy hello teljesen minősített erőforrás-azonosító. További információkért lásd: [Azure Resource Manager API](https://docs.microsoft.com/rest/api/resources/resources). |
|Címkék | Hozzárendelt toohello mérő címkét. Címkék toogroup számlázási rekordok használata.<br/><br/>Például hello részleg által használt hello mérő címkék toodistribute költségeket is használhatja. Kibocsátás címkék támogató szolgáltatások a virtuális gépek, tárolási és hálózati szolgáltatások hello használó kiépítve [Azure Resource Manager API](https://docs.microsoft.com/rest/api/resources/resources). További információkért lásd: [címke az Azure-erőforrások rendszerezése](http://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/). |
|Additional Info (További információ) |Szolgáltatással kapcsolatos metaadatok. Például egy kép típusa egy virtuális géphez. |
|Service Info 1 (1. szolgáltatási megjegyzés) |hello szolgáltatás hello projektnevet tooon az előfizetéshez tartozik |
|Service Info 2 (2. szolgáltatási megjegyzés) |A hagyományos mezője, amely rögzíti az opcionális szolgáltatással kapcsolatos metaadatok |

## <a name="how-do-i-make-sure-that-hello-charges-in-my-detailed-usage-file-are-correct"></a>Hogyan tehetem meg arról, hogy helyesen-e a részletes használati fájlban hello díjak?
Ha a részletes használati fájlokat, amelyeket meg szeretne további részleteket a járnak, [a Microsoft Azure a számlázási ismertetése.](./billing-understand-your-bill.md)

## <a name="external"></a>Mi a helyzet a külső szolgáltatási díjak?
Külső szolgáltatások (más néven piactér rendelések) szerezhetők független szolgáltatás, és külön vannak számlázva. hello költségek a hello Azure számla nem jelennek meg. több, lásd: toolearn [az Azure külső szolgáltatási díjak megismeréséhez](billing-understand-your-azure-marketplace-charges.md).

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?) a probléma elhárítva gyors eléréséhez.
