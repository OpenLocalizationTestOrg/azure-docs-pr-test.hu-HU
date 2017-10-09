## <a name="obtain-an-azure-resource-manager-token"></a>Az Azure Resource Manager jogkivonat beszerzése
Az Azure Active Directory üdvözlő feladataival hello Azure Resource Manager eszközzel a kell hitelesítenie. hello az itt bemutatott példában jelszó-hitelesítést használ, más megoldások című [kérelmek hitelesítéséhez az Azure Resource Manager][lnk-authenticate-arm].

1. Adja hozzá a következő kód toohello hello **fő** metódus a Program.cs tooretrieve jogkivonatot az Azure AD hello alkalmazás azonosító és jelszó használatával.
   
    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.microsoftonline.com/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
   
    if (token == null)
    {
      Console.WriteLine("Failed tooobtain hello token");
      return;
    }
    ```
2. Hozzon létre egy **ResourceManagementClient** objektumot, hogy használja a következő kód toohello vége hello hello hozzáadásával token hello **fő** módszert:
   
    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. Hozzon létre, vagy szerezzen be egy hivatkozást, hello erőforráscsoportot használ:
   
    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx