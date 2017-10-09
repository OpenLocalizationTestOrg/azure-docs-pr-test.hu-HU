## <a name="prepare-tooauthenticate-azure-resource-manager-requests"></a>Azure Resource Manager-kérelmek tooauthenticate előkészítése
Összes hello művelet hajt végre hello eszközzel kell hitelesítenie [Azure Resource Manager] [ lnk-authenticate-arm] az Azure Active Directory (AD). Ennek legegyszerűbb módja tooconfigure hello toouse PowerShell vagy az Azure parancssori felület.

Telepítse a hello [Azure PowerShell-parancsmagok] [ lnk-powershell-install] a folytatás előtt.

a következő lépéseket megjelenítése hogyan hello tooset PowerShell használatával AD alkalmazások jelszó-hitelesítést. Ezek a parancsok a szabványos PowerShell-munkamenetben futtathatja.

1. Jelentkezzen be tooyour Azure-előfizetés a következő parancs hello használata:

    ```powershell
    Login-AzureRmAccount
    ```

1. Ha több Azure-előfizetéssel rendelkezik, birtokában tooall hozzáférhet tooAzure bejelentkezés hello Azure-előfizetéssel társított hitelesítő adatait. A következő parancs toolist hello Azure-előfizetések elérhető az Ön toouse hello használata:

    ```powershell
    Get-AzureRMSubscription
    ```

    A következő parancs tooselect előfizetést, amelyet toouse toorun hello parancsok toomanage az IoT hub hello használata. Hello előfizetés név vagy azonosító hello kimenetből hello előző parancs használható:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. Jegyezze fel a **TenantId** és **SubscriptionId**. Ezeket később szüksége.
3. Hozzon létre egy új Azure Active Directory-alkalmazást a következő parancsot, cseréje hello hely tulajdonosainak hello:
   
   * **{Név}:** egy megjelenítési nevet az alkalmazásnak, többek között **MySampleApp**
   * **{Kezdőlap URL-cím}:** hello kezdőlap, az alkalmazás URL-címe például hello **http://mysampleapp/home**. Az URL-cím nem kell toopoint tooa valós alkalmazás.
   * **{Alkalmazásazonosító}:** egyedi azonosítója, mint **http://mysampleapp**. Az URL-cím nem kell toopoint tooa valós alkalmazás.
   * **{Jelszó}:** tooauthenticate használhatja az alkalmazást a jelszót.
     
     ```powershell
     New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
     ```
4. Jegyezze fel a hello **ApplicationId** létrehozott hello alkalmazás. Később szüksége.
5. Hozzon létre egy új egyszerű szolgáltatás használata a következő parancsot, hogy hello **{MyApplicationId}** a hello **ApplicationId** hello előző lépésben:
   
    ```powershell
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. Állítsa be a következő parancsot, hogy hello segítségével szerepkör-hozzárendelés **{MyApplicationId}** rendelkező a **ApplicationId**.
   
    ```powershell
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

Most már befejezte a hello Azure AD-alkalmazást, amely lehetővé teszi tooauthenticate az egyéni C#-alkalmazás létrehozása. Hello követő értékek az oktatóanyag későbbi részében szüksége:

* A TenantId
* SubscriptionId
* ApplicationId
* Jelszó

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
