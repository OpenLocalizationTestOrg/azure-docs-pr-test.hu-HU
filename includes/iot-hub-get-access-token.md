## <a name="obtain-an-azure-resource-manager-token"></a><span data-ttu-id="413b9-101">Az Azure Resource Manager jogkivonat beszerzése</span><span class="sxs-lookup"><span data-stu-id="413b9-101">Obtain an Azure Resource Manager token</span></span>
<span data-ttu-id="413b9-102">Az Azure Active Directory üdvözlő feladataival hello Azure Resource Manager eszközzel a kell hitelesítenie.</span><span class="sxs-lookup"><span data-stu-id="413b9-102">Azure Active Directory must authenticate all hello tasks that you perform on resources using hello Azure Resource Manager.</span></span> <span data-ttu-id="413b9-103">hello az itt bemutatott példában jelszó-hitelesítést használ, más megoldások című [kérelmek hitelesítéséhez az Azure Resource Manager][lnk-authenticate-arm].</span><span class="sxs-lookup"><span data-stu-id="413b9-103">hello example shown here uses password authentication, for other approaches see [Authenticating Azure Resource Manager requests][lnk-authenticate-arm].</span></span>

1. <span data-ttu-id="413b9-104">Adja hozzá a következő kód toohello hello **fő** metódus a Program.cs tooretrieve jogkivonatot az Azure AD hello alkalmazás azonosító és jelszó használatával.</span><span class="sxs-lookup"><span data-stu-id="413b9-104">Add hello following code toohello **Main** method in Program.cs tooretrieve a token from Azure AD using hello application id and password.</span></span>
   
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
2. <span data-ttu-id="413b9-105">Hozzon létre egy **ResourceManagementClient** objektumot, hogy használja a következő kód toohello vége hello hello hozzáadásával token hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="413b9-105">Create a **ResourceManagementClient** object that uses hello token by adding hello following code toohello end of hello **Main** method:</span></span>
   
    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. <span data-ttu-id="413b9-106">Hozzon létre, vagy szerezzen be egy hivatkozást, hello erőforráscsoportot használ:</span><span class="sxs-lookup"><span data-stu-id="413b9-106">Create, or obtain a reference to, hello resource group you are using:</span></span>
   
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