<!-- markdownlint-disable MD041 -->

In this exercise you'll learn how to dynamically retrieve user identity and token values from Azure Communication Services using Azure Functions. Once retrieved, the values will be passed to the ACS UI composite to enable a call to be made by a customer.

:::image type="content" source="./media/4-acs-identity-token.png" alt-text="Create ACS Identity and Token":::


::: zone pivot="programming-language-csharp"
[!INCLUDE [C#](./05-Create-ACS-Identity-Token-CS.md)]
::: zone-end

::: zone pivot="programming-language-typescript"
[!INCLUDE [TypeScript](./05-Create-ACS-Identity-Token-TS.md)]
::: zone-end