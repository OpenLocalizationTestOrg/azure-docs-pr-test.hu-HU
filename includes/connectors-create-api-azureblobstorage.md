### <a name="prerequisites"></a>Előfeltételek
* Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)
* Egy [Azure Blob Storage-fiók](../articles/storage/common/storage-create-storage-account.md) például hello tárfiók neve és a hozzáférési kulcsot. Ez az információ hello tulajdonságainak hello storage-fiókot hello Azure-portálon jelenik meg. Tudjon meg többet az [Azure Storage](../articles/storage/common/storage-introduction.md).

A logikai alkalmazást az Azure Blob Storage-fiók használatához csatlakozás tooyour Azure Blob Storage-fiók. Ehhez egyszerűen a logikai alkalmazásban a hello Azure-portálon.  

Csatlakozás tooyour Azure Blob Storage-fiók használatával hello a következő lépéseket:  

1. Logikai alkalmazás létrehozása. Hello Logic Apps-tervezőben adja hozzá egy eseményindító, és adja hozzá az műveletet. Válassza ki **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, és adja meg "blob" hello Keresés mezőbe. Hello műveletek közül választhat:  
   
    ![Az Azure Blob Storage kapcsolat létrehozását lépést](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  
2. Ha nem korábban hozott létre a kapcsolatokat tooAzure tárolási, kéri hello csatlakozási adatait:   
   
    ![Az Azure Blob Storage kapcsolat létrehozását lépést](./media/connectors-create-api-azureblobstorage/connection-details.png)  
3. Adja meg a hello tárfiókadatok. Tulajdonságok csillaggal szükség.
   
   | Tulajdonság | Részletek |
   | --- | --- |
   | Kapcsolat neve * |Adjon meg egy tetszőleges nevet a kapcsolat. |
   | Az Azure Storage-fiók neve * |Adja meg a hello tárfiók neve. hello tárfióknév hello tárolási tulajdonságok hello Azure-portálon jelenik meg. |
   | Azure Storage-fiók hozzáférési kulcs * |Adja meg a hello tárfiók kulcsa. hello hívóbetűk hello tárolási tulajdonságok hello Azure-portálon jelennek meg. |
   
    Ezek a hitelesítő adatok használt tooauthorize a logic app tooconnect, és hozzáférhet az adatokhoz. 
4. Kattintson a **Létrehozás** gombra.
5. Értesítés hello kapcsolat létrejött. Most, folytassa a Logic Apps alkalmazást hello más szükséges lépések: 
   
    ![Az Azure Blob Storage kapcsolat létrehozását lépést](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  

