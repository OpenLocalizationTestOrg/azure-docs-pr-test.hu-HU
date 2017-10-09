---
title: "Hozzáférés-vezérlési szolgáltatásban (Java) hello SAML visszaadott aaaView"
description: "Ismerje meg, hogyan tooview hello hozzáférés-vezérlési szolgáltatásban a Java-alkalmazások által visszaadott SAML Azure-platformon futó."
services: active-directory
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 6cd216f9-eb43-46b4-b30d-f194d0ae2d48
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: b6733bc98b505cfa89a4ce456f368ee15da11427
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooview-saml-returned-by-hello-azure-access-control-service"></a>Hogyan tooview SAML hello Azure Access Control Service által visszaadott
Ez az útmutató bemutatja, hogyan tooview hello az alapul szolgáló Security Assertion Markup Language (SAML) tooyour alkalmazás által visszaadott hello Azure Access Control Service (ACS). hello útmutató épít, hello [hogyan tooAuthenticate webes felhasználók az Azure Access Control Service használatával Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) témakör, adja meg a kódot, amely hello SAML-információkat jeleníti meg. befejeződött hello alkalmazás hasonló toohello következő fog kinézni.

![Példa SAML kimenet][saml_output]

Az ACS további információkért lásd: hello [további lépések](#next_steps) szakasz.

> [!NOTE]
> hello Azure Access Services vezérlő szűrő egy közösségi technológiai előzetes. Kiadás előtti szoftverként azt van hivatalosan Microsoft nem támogatja.
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Ebben az útmutatóban teljes toocomplete hello feladatok hello minta a [hogyan tooAuthenticate webes felhasználók az Azure Access Control Service használatával Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) és hello kiindulási pontjaként ehhez az oktatóanyaghoz használható.

## <a name="add-hello-jspwriter-library-tooyour-build-path-and-deployment-assembly"></a>Hello JspWriter könyvtár tooyour build elérési útját és a központi telepítés szerelvény hozzáadása
Hello tartalmazó hello könyvtár hozzáadása **javax.servlet.jsp.JspWriter** osztály tooyour szerelvény elérési útja és a központi telepítés létrehozása. Tomcat használ, hogy van-e a hello könyvtár **jsp-api.jar**, hello Apache található **lib** mappát.

1. A Project Explorer Eclipse meg, kattintson a jobb gombbal **MyACSHelloWorld**, kattintson **Build elérési**, kattintson **Build elérési konfigurálása**, hello kattintson **szalagtárak** fülre, majd **külső JARs hozzáadása**.
2. A hello **JAR kijelölés** párbeszédpanelen keresse meg a toohello szükséges JAR, válassza ki azt, és kattintson **nyissa meg**.
3. A hello **MyACSHelloWorld tulajdonságainak** párbeszédpanelen megnyitva, kattintson **telepítési szerelvény**.
4. A hello **webes telepítési szerelvény** párbeszédpanel, kattintson a **Hozzáadás**.
5. A hello **új szerelvény irányelv** párbeszédpanel, kattintson a **Java Build elérési bejegyzések** , majd **következő**.
6. Válassza ki a megfelelő hello szalagtárat, és kattintson a **Befejezés**.
7. Kattintson a **OK** tooclose hello **MyACSHelloWorld tulajdonságainak** párbeszédpanel.

## <a name="modify-hello-jsp-file-toodisplay-saml"></a>Hello JSP-fájl toodisplay SAML módosítása
Módosítsa **index.jsp** toouse hello a következő kódot.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {

            try
            {
                String nodeName;
                int nChild, i;

                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");

                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }

                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                    {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    

                                 // If it is a text node, just print hello text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out hello child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                        for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);

                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate hello names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish hello sentence.
                                            out.print(".");
                                        }

                                     }
                                     out.println("<br>");

                                     // Process hello child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>

        <%
        try
        {
            String data  = (String) request.getAttribute("ACSSAML");

            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();

            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA);
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();

            // Iterate hello child nodes of hello doc.
            NodeList list = doc.getChildNodes();

            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e)
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }

        %>
    </body>
    </html>

## <a name="run-hello-application"></a>Hello alkalmazás futtatása
1. Futtassa az alkalmazást a hello számítógép emulátor vagy tooAzure leírt lépéseket hello segítségével telepítheti [hogyan tooAuthenticate webes felhasználók az Azure Access Control Service használatával Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).
2. Elindít egy böngészőt, és nyissa meg a webes alkalmazás. Tooyour alkalmazás bejelentkezést követően megjelenik a SAML információt, beleértve a hello hello identitásszolgáltató által biztosított biztonsági helyességi feltételt.

## <a name="next-steps"></a>Következő lépések
toofurther ACS azon funkcióit és az összetettebb forgatókönyveket tooexperiment című [Access Control Service 2.0][Access Control Service 2.0].

[Prerequisites]: #pre
[Modify hello JSP file toodisplay SAML]: #modify_jsp
[Add hello JspWriter library tooyour build path and deployment assembly]: #add_library
[Run hello application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse]: active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
