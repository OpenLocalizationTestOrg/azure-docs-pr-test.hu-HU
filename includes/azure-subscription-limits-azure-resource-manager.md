| Erőforrás | Alapértelmezett korlát | Felső korlát |
| --- | --- | --- |
| Virtuális gépek [előfizetésenként](../articles/billing-buy-sign-up-azure-subscription.md) |10 000 <sup>1</sup> régiónként |10 000 régiónként |
| Virtuálisgép-magok összesen, [előfizetésenként](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> régiónként | Kapcsolatfelvétel a támogatási szolgáltatással |
| Sorozatonkénti (Dv2, F stb.) virtuálisgép-magok [előfizetésenként](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> régiónként | Kapcsolatfelvétel a támogatási szolgáltatással |
| [Társadminisztrátorok](../articles/billing-add-change-azure-subscription-administrator.md) előfizetésenként |Korlátlan |Korlátlan |
| [Tárfiókok](../articles/storage/common/storage-create-storage-account.md) előfizetésenként |200 |200<sup>2</sup> |
| [Erőforráscsoportok](../articles/azure-resource-manager/resource-group-overview.md) előfizetésenként |800 |800 |
| [Rendelkezésre állási csoportok](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) előfizetésenként |2000 régiónként |2000 régiónként |
| Resource Manager API-olvasási műveletek |15 000 óránként |15 000 óránként |
| Resource Manager API-írási műveletek |1200 óránként |1200 óránként |
| Resource Manager API-kérések mérete |4 194 304 bájt |4 194 304 bájt |
| Címkék előfizetésenként<sup>3</sup> |korlátlan |korlátlan |
| Egyedi címkeszámítások előfizetésenként<sup>3</sup> | 10,000 | 10,000 |
| [Felhőszolgáltatások](../articles/cloud-services/cloud-services-choose-me.md) előfizetésenként |Nem alkalmazható<sup>4</sup> |Nem alkalmazható<sup>4</sup> |
| [Affinitáscsoportok](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) előfizetésenként |Nem alkalmazható<sup>4</sup> |Nem alkalmazható<sup>4</sup> |

<sup>1</sup>Az alapértelmezett korlátok az ajánlat kategóriája (pl. ingyenes próba, használatalapú fizetés) és a sorozat (Dv2, F, G stb.) szerint változnak.

<sup>2</sup>Ebbe a standard és a prémium szintű tárfiókok is beletartoznak. Ha több mint 200 tárfiókra van szüksége, nyújtson be egy kérést az [Azure ügyfélszolgálatán](https://azure.microsoft.com/support/faq/) keresztül. hello Azure Storage csapat lesz az üzleti esetek és jóváhagyhatja too250 storage-fiókok létrehozása.

<sup>3</sup>Az előfizetésenként alkalmazott címkék számának nincs korlátja. egy erőforrás vagy erőforráscsoport címkék hello száma korlátozott too15. Erőforrás-kezelő csak adja vissza egy [egyedi címkenév és értékek listájáról](/rest/api/resources/tags#Tags_List) Ha címkék hello száma 10 000 vagy kevesebb hello előfizetésben. Azonban továbbra is található erőforrás kódcímke Ha hello száma meghaladja a 10 000-re.  

<sup>4</sup>ezek a funkciók már nem szükségesek az Azure erőforráscsoport-sablonok és hello Azure Resource Manager.

> [!NOTE]
> Virtuális gép maggal rendelkező egy regionális teljes korlátját, valamint a területi mérete (Dv2, F stb.) adatsorozat korlát külön-külön a kényszerített fontos tooemphasize.  Például tegyük fel, hogy egy előfizetés az USA keleti régiójára vonatkozó teljes magkorlátja 30, az A sorozatú magkorlátja 30, és a D sorozatú magkorlátja is 30.  Ez az előfizetés volna kell toodeploy 30 A1 virtuális gépek, illetve 30 D1 virtuális gépek újra, vagy kombinációja hello két tooexceed összesen 30 mag (például 10 A1 virtuális gépek és 20 D1 virtuális gépeken).  
> <!-- -->
> 
> 

