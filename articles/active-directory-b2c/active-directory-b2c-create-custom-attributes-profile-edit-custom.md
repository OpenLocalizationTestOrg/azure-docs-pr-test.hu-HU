---
title: "Az Azure Active Directory B2C: Saját attribútumok toocustom szabályzatok, és használja a profil szerkesztése |} Microsoft Docs"
description: "A forgatókönyv a bővítmény tulajdonságok, egyéni attribútumok használatát, és így azok hello felhasználói felületen"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: 8cc9c6a38d7652797ba54a3e02078ac2bf4a693b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a>Az Azure Active Directory B2C: Létrehozása és az egyéni attribútumok használata egy egyéni profilt a házirend szerkesztése

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Ebben a cikkben egy egyéni attribútum létrehozása a Azure AD B2C-címtárban, és egy egyéni jogcím hello profil szerkesztése felhasználói út ezt az új attribútumot használja.

## <a name="prerequisites"></a>Előfeltételek

Teljes hello hello cikkben ismertetett visszaállítási lépésekkel [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md).

## <a name="use-custom-attributes-toocollect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a>Az ügyfelek az Azure Active Directory B2C egyéni házirendekkel toocollect információt az egyéni attribútumok használata
Azure Active Directory (Azure AD) B2C-címtárban tartalmaz egy beépített attribútumok: megadott név, Vezetéknév, város, irányítószám, userPrincipalName, stb.  Gyakran kell toocreate saját attribútumok.  Példa:
* Egy ügyfélkapcsolati alkalmazást kell toopersist egy attribútum, például a "LoyaltyNumber."
* Az identitásszolgáltató rendelkezik egy egyedi felhasználói azonosító, amelyet kell menteni, például a "uniqueUserGUID." "
* Egyéni felhasználói út kell toopersist hello állapotának felhasználó például "migrationStatus."

Az Azure AD B2C-ben tárolt felhasználói attribútumok készletét hello bővítheti. Is olvasási és írási ezek az attribútumok hello segítségével [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

Bővítmény tulajdonságai hello felhasználói objektum hello könyvtárban hello-séma kiterjesztése.  hello feltételek bővített tulajdonság, az egyéni attribútum és az egyéni jogcímleírásokat tekintse meg a toohello ugyanaz a cikk és hello név hello környezetében hello környezetben (alkalmazás, objektum, a házirend) függ.

Bővítmény tulajdonságai csak regisztrálható az alkalmazásobjektum, annak ellenére, hogy egy felhasználó lehet, hogy adatokat tartalmaznak. hello tulajdonság csatolt toohello alkalmazás. hello alkalmazásobjektum megadott írási tooregister egy bővített tulajdonság kell lennie. 100 bővítmény tulajdonságai (közötti összes típusa és az összes alkalmazás) csak írható tooany egyetlen objektumhoz. Bővítmény tulajdonságai toohello directory céltípus kerülnek, és azonnal elérhetővé hello Azure AD B2C directory-bérlőben.
Hello alkalmazás törlésekor az összes felhasználó számára a bennük található adatokat ilyen bővítmény tulajdonságok is törlődnek. Egy bővített tulajdonság hello alkalmazás törlése, ha eltávolítják azt a hello címtárobjektumok célként, és törölni értékek hello.

Bővítmény tulajdonságai csak a hello kontextusában hello bérlő regisztrált alkalmazás szerepel. hello objektumazonosító alkalmazás, az azt használó TechnicalProfile hello kell szerepelnie.

>[!NOTE]
>hello Azure AD B2C directory többek között a webes alkalmazás neve `b2c-extensions-app`.  Ez az alkalmazás elsősorban a hello hello Azure-portálon létrehozott egyéni jogcímek hello b2c beépített házirendjei használják.  Az alkalmazás tooregister bővítmények b2c egyéni házirendek használatával csak haladó felhasználóknak javasolt.  Ehhez útmutatást hello további lépések című részben szerepelnek.


## <a name="creating-a-new-application-toostore-hello-extension-properties"></a>Egy új alkalmazás toostore hello bővítmény tulajdonságai létrehozása

1. Nyissa meg a böngésző munkamenetet, és keresse meg a toohello [Azure porta](https://portal.azure.com) és jelentkezzen be rendszergazdai hitelesítő adataival hello tooconfigure kívánja B2C-címtárban.
1. Kattintson a **Azure Active Directory** hello bal oldali navigációs menü. Szükség lehet az kiválasztásával további szolgáltatások toofind >.
1. Válassza ki **App regisztrációk** kattintson **új alkalmazás regisztrációja**
1. Adja meg a hello következő ajánlott bejegyzéseket:
  * Adjon meg egy nevet a webalkalmazás hello: **WebApp-GraphAPI-DirectoryExtensions**
  * Alkalmazás típusa: webes alkalmazás/API-t
  * Bejelentkezés URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions
1. Válassza ki ** létrehozása. Sikeres létrehozása után megjelennek a hello **értesítések**
1. Válassza ki az újonnan létrehozott hello webalkalmazás: **WebApp-GraphAPI-DirectoryExtensions**
1. Válassza a beállítások: **szükséges engedélyek**
1. Az API lehetőséget választhatja **Windows Active Directory**
1. Jelölje be az Alkalmazásengedélyek: **címtáradatok olvasása és írása**, és **mentése**
1. Válasszon **engedélyeket** , majd erősítse meg **Igen**.
1. Tooyour vágólapra másolja ki és mentse a webalkalmazás-GraphAPI-DirectoryExtensions azonosítók következő hello > Beállítások > Tulajdonságok >
*  **Alkalmazásazonosító** . Példa:`103ee0e6-f92d-4183-b576-8c3739027780`
* **Objektumazonosító:**. Példa:`80d8296a-da0a-49ee-b6ab-fd232aa45201`



## <a name="modifying-your-custom-policy-tooadd-hello-applicationobjectid"></a>Az egyéni házirend tooadd hello ApplicationObjectId módosítása

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item>
                <Item Key="ClientId">insert appId here</Item>
              </Metadata>
            <!-- End of changes -->
              <CryptographicKeys>
                <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
              </CryptographicKeys>
              <IncludeInSso>false</IncludeInSso>
              <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
            </TechnicalProfile>
        </ClaimsProvider>
    </ClaimsProviders>
```

>[!NOTE]
>Hello <TechnicalProfile Id="AAD-Common"> hivatkozott tooas "általános" azért, mert az elemei szerepel, és használja fel újra az összes hello Azure Active Directory TechnicalProfiles hello elem használatával:`<IncludeTechnicalProfile ReferenceId="AAD-Common" />`

>[!NOTE]
>Hello TechnicalProfile írási műveleteknél hello első alkalommal az újonnan létrehozott toohello bővítmény tulajdonság egy egyszeri hibát tapasztalhatnak.  hello bővített tulajdonság hello jön létre a rendszer első alkalommal.  

## <a name="using-hello-new-extension-property--custom-attribute-in-a-user-journey"></a>Hello új bővítmény tulajdonság használatával / egyéni attribútum a felhasználó út


1. Nyissa meg hello Party(RP) függő fájl, amely leírja a házirend szerkesztése felhasználói út.  Ha indítja, ajánlott toodownload hello RP-PolicyEdit már konfigurált verziójának közvetlenül hello hello Azure portál Azure B2C egyéni házirend szakasz fájlt lehet.  Azt is megteheti nyissa meg az XML-fájl a tárolási mappából.
2. Adja hozzá az egyéni jogcímleírásokat `loyaltyId`.  Hello a jogcím-ot hello egyéni `<RelyingParty>` elem, mint egy paraméterrel toohello UserJourney TechnicalProfiles átadott, és a hello tokenben hello alkalmazáshoz.
```xml
<RelyingParty>
   <DefaultUserJourney ReferenceId="ProfileEdit" />
   <TechnicalProfile Id="PolicyProfile">
     <DisplayName>PolicyProfile</DisplayName>
     <Protocol Name="OpenIdConnect" />
     <OutputClaims>
       <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
       <OutputClaim ClaimTypeReferenceId="city" />

       <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />

     </OutputClaims>
     <SubjectNamingInfo ClaimType="sub" />
   </TechnicalProfile>
 </RelyingParty>
 ```
3. Adjon hozzá egy jogcím definition toohello bővítmény házirend fájlt `TrustFrameworkExtensions.xml` belül hello `<ClaimsSchema>` elem látható módon.
```xml
<ClaimsSchema>
        <ClaimType Id="extension_loyaltyId">
            <DisplayName>Loyalty Identification Tag</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your loyalty number from your membership card</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
</ClaimsSchema>
```
4. Adja hozzá a hello azonos jogcím-definíció toohello alap házirendfájl `TrustFrameworkBase.xml`.  
>Hozzáadás a `ClaimType` definition hello talál és hello bővítmények fájl általában nem szükség, azonban hello lépések hello extension_loyaltyId tooTechnicalProfiles hello alap fájlt adja hozzá, mivel hello házirend érvényesítési elutasítják hello feltöltése hello alap fájl nélkül.
>Hasznos tootrace hello végrehajtási hello felhasználói út nevű hello TrustFrameworkBase.xml fájl "ProfileEdit" lehet.  Ugyanaz a szerkesztőben nevet, és tekintse meg az hív meg, hogy az Orchestration 5. lépés hello TechnicalProfileReferenceID hello hello felhasználói út keresése = "SelfAsserted-ProfileUpdate".  Keresse meg és vizsgálja meg a TechnicalProfile toofamiliarize saját kezűleg a hello folyamata.
5. Adja hozzá a loyaltyId jogcímként bemeneti és kimeneti a hello TechnicalProfile "SelfAsserted-ProfileUpdate"
```xml
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
          <DisplayName>User ID signup</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ContentDefinitionReferenceId">api.selfasserted.profileupdate</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>

            <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
            <InputClaim ClaimTypeReferenceId="userPrincipalName" />

            <!-- Optional claims. These claims are collected from hello user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written toodirectory after being updateed by hello user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />
            <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </InputClaims>
          <OutputClaims>
            <!-- Required claims -->
            <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />

            <!-- Optional claims. These claims are collected from hello user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written toodirectory after being updateed by hello user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <OutputClaim ClaimTypeReferenceId="givenName" />
            <OutputClaim ClaimTypeReferenceId="surname" />
            <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
```
6. Jogcím hozzáadása a "AAD-UserWriteProfileUsingObjectId" TechnicalProfile toopersist hello érték hello jogcím hello bővített tulajdonság, az aktuális felhasználó hello hello könyvtárban.
```xml
<TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
          <Metadata>
            <Item Key="Operation">Write</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">false</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
          </InputClaims>
          <PersistedClaims>
            <!-- Required claims -->
            <PersistedClaim ClaimTypeReferenceId="objectId" />

            <!-- Optional claims -->
            <PersistedClaim ClaimTypeReferenceId="givenName" />
            <PersistedClaim ClaimTypeReferenceId="surname" />
            <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />

          </PersistedClaims>
          <IncludeTechnicalProfile ReferenceId="AAD-Common" />
        </TechnicalProfile>
```
7. Adja hozzá a jogcím TechnicalProfile "AAD-UserReadUsingObjectId" tooread hello hello bővítmény attribútum értékének a minden alkalommal, amikor a felhasználó jelentkezik be. Így sokkal hello folyamat csak a helyi fiókok hello TechnicalProfiles megváltoztak.  Hello új attribútum társadalombiztosítási/összevont fiók hello folyamatában van szükség, ha a TechnicalProfiles különböző szabálykészleteket toobe módosítani kell. Tekintse meg a következő lépéseket.

```xml
<!-- hello following technical profile is used tooread data after user authenticates. -->
     <TechnicalProfile Id="AAD-UserReadUsingObjectId">
       <Metadata>
         <Item Key="Operation">Read</Item>
         <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
       </Metadata>
       <IncludeInSso>false</IncludeInSso>
       <InputClaims>
         <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
       </InputClaims>
       <OutputClaims>
         <!-- Optional claims -->
         <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
         <OutputClaim ClaimTypeReferenceId="displayName" />
         <OutputClaim ClaimTypeReferenceId="otherMails" />
         <OutputClaim ClaimTypeReferenceId="givenName" />
         <OutputClaim ClaimTypeReferenceId="surname" />
         <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
       </OutputClaims>
       <IncludeTechnicalProfile ReferenceId="AAD-Common" />
     </TechnicalProfile>
```


>[!IMPORTANT]
>hello IncludeTechnicalProfile elem hozzáadása az AAD-közös toothis TechnicalProfile összes hello eleme.

## <a name="test-hello-custom-policy-using-run-now"></a>Teszt hello egyéni házirend használatával "Futtatás most"
1. Nyissa meg hello **panel az Azure AD B2C** , és keresse meg a túl**identitás élmény keretrendszer > egyéni házirendek**.
1. Válassza ki a feltöltött hello egyéni házirendet, majd kattintson a hello **futtatása most** gombra.
1. Meg kell tudni toosign be egy e-mail címet.

hello azonosító tokent küldött vissza tooyour alkalmazás hello új bővített tulajdonság tartalmaz egy egyéni jogcímként extension_loyaltyId utasításnak. Lásd a példát.

```
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493581587,
  "auth_time": 1493581587,
  "extension_loyaltyId": "abc",
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Következő lépések

Adja hozzá a hello új jogcímet toohello adatfolyamok a közösségi fiók bejelentkezések során TechnicalProfiles felsorolt hello módosításával. E két TechnicalProfiles társadalombiztosítási/összevont fiók bejelentkezések toowrite használják, és hello alternativeSecurityId használatával, mint a lokátor hello felhasználói objektum hello hello felhasználói adatokat olvasni.
```
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

Használatával hello beépített és egyéni házirendek közötti azonos kiterjesztési attribútumot.
Amikor kiterjesztési attribútumot (más néven egyéni attribútumok) keresztül hello portál élmény, azok hello segítségével regisztrált ** b2c-bővítmények-alkalmazást, amely minden b2c-bérlő szerepel.  toouse ezeket a bővítményattribútumokat, az egyéni házirendek:
1. A b2c-bérlő a portal.azure.com, Ugrás túl**Azure Active Directory** válassza **App regisztrációk**
2. Keresés a **b2c-bővítmények-alkalmazás** , és jelölje ki
3. A "Essentials" rekord hello **Alkalmazásazonosító** és hello **objektum azonosítója**
4. Tartalmazza azokat az AAD-gyakori technikai profil metaadatai között, például a következőképpen:

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is hello "Object ID" from hello "b2c-extensions-app"-->
                <Item Key="ClientId">insert appId here</Item> <!--This is hello "Application ID" from hello "b2c-extensions-app"-->
              </Metadata>
```

tookeep konzisztencia az hello portál révén, ezek az attribútumok hello portál felhasználói felületének használatával hozzon létre *előtt* azokat az egyéni házirendeket használ.  Amikor létrehoz egy attribútum "ActivationStatus" hello portálon, az alábbiak szerint tooit kell hivatkoznia:

```
extension_ActivationStatus in hello custom policy
extension_<app-guid>_ActivationStatus via hello Graph API.
```


## <a name="reference"></a>Referencia

* A **műszaki profil (TP)** -re mint elemtípuson van egy *függvény* , amely meghatározza egy végpont nevét, a metaadatait, a protokollal, és a részletek hello cseréjének, amely identitás hello Felhasználói élmény keretrendszer végre kell hajtania.  Ha ez *függvény* egy vezénylési lépés vagy egy másik TechnicalProfile, InputClaims és OutputClaims vannak megadva, a paraméterek hello hívó hello nevezik.


* A bővítmény tulajdonságai teljes kezelés cikke hello [DIRECTORY-SÉMA bővítményei |} GRAPH API FOGALMAK](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)

>[!NOTE]
>A Graph API a bővítményattribútumokat megnevezett hello konvenció `extension_ApplicationObjectID_attributename`. Egyéni házirendek tooextensions attribútumok extension_attributename, így kihagyásával hello ApplicationObjectId a hello XML, tekintse meg a
