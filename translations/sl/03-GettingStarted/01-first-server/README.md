<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "262e6e510f0c3fe1e36180eadcd67c33",
  "translation_date": "2025-06-02T17:49:34+00:00",
  "source_file": "03-GettingStarted/01-first-server/README.md",
  "language_code": "sl"
}
-->
### -2- Ustvari projekt

Zdaj, ko imaš nameščen SDK, ustvarimo naslednji projekt:

### -3- Ustvari datoteke projekta

### -4- Ustvari kodo strežnika

### -5- Dodajanje orodja in vira

Dodaj orodje in vir z naslednjo kodo:

### -6 Končna koda

Dodajmo zadnjo kodo, ki jo potrebujemo, da se strežnik lahko zažene:

### -7- Preizkusi strežnik

Zaženi strežnik z naslednjim ukazom:

### -8- Zaženi z inspektorjem

Inspektor je odlično orodje, ki lahko zažene tvoj strežnik in ti omogoči interakcijo z njim, da preveriš, ali deluje. Zaženi ga:

> [!NOTE]
> v polju "ukaz" je lahko drugačen, saj vsebuje ukaz za zagon strežnika z izbranim runtime-om

Videti bi moral naslednji uporabniški vmesnik:

![Poveži](../../../../translated_images/connect.141db0b2bd05f096fb1dd91273771fd8b2469d6507656c3b0c9df4b3c5473929.sl.png)

1. Poveži se s strežnikom tako, da izbereš gumb Connect  
   Ko se povežeš s strežnikom, bi moral videti naslednje:

   ![Povezan](../../../../translated_images/connected.73d1e042c24075d386cacdd4ee7cd748c16364c277d814e646ff2f7b5eefde85.sl.png)

2. Izberi "Tools" in "listTools", videl boš "Add", izberi "Add" in izpolni vrednosti parametrov.

   Videl boš naslednji odgovor, torej rezultat orodja "add":

   ![Rezultat izvajanja add](../../../../translated_images/ran-tool.a5a6ee878c1369ec1e379b81053395252a441799dbf23416c36ddf288faf8249.sl.png)

Čestitke, uspel si ustvariti in zagnati svoj prvi strežnik!

### Uradni SDK-ji

MCP ponuja uradne SDK-je za več programskih jezikov:
- [C# SDK](https://github.com/modelcontextprotocol/csharp-sdk) - vzdrževan v sodelovanju z Microsoftom
- [Java SDK](https://github.com/modelcontextprotocol/java-sdk) - vzdrževan v sodelovanju s Spring AI
- [TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk) - uradna implementacija za TypeScript
- [Python SDK](https://github.com/modelcontextprotocol/python-sdk) - uradna implementacija za Python
- [Kotlin SDK](https://github.com/modelcontextprotocol/kotlin-sdk) - uradna implementacija za Kotlin
- [Swift SDK](https://github.com/modelcontextprotocol/swift-sdk) - vzdrževan v sodelovanju z Loopwork AI
- [Rust SDK](https://github.com/modelcontextprotocol/rust-sdk) - uradna implementacija za Rust

## Ključne ugotovitve

- Nastavitev MCP razvojnega okolja je preprosta z jezikovno specifičnimi SDK-ji
- Gradnja MCP strežnikov vključuje ustvarjanje in registracijo orodij s jasno določenimi shemami
- Testiranje in odpravljanje napak sta ključna za zanesljive MCP implementacije

## Primeri

- [Java kalkulator](../samples/java/calculator/README.md)
- [.Net kalkulator](../../../../03-GettingStarted/samples/csharp)
- [JavaScript kalkulator](../samples/javascript/README.md)
- [TypeScript kalkulator](../samples/typescript/README.md)
- [Python kalkulator](../../../../03-GettingStarted/samples/python)

## Naloga

Ustvari preprost MCP strežnik z orodjem po svoji izbiri:
1. Implementiraj orodje v svojem priljubljenem jeziku (.NET, Java, Python ali JavaScript).
2. Določi vhodne parametre in povratne vrednosti.
3. Zaženi inspektor, da preveriš, ali strežnik deluje kot pričakovano.
4. Testiraj implementacijo z različnimi vhodi.

## Rešitev

[Rešitev](./solution/README.md)

## Dodatni viri

- [Gradnja agentov z uporabo Model Context Protocol na Azure](https://learn.microsoft.com/azure/developer/ai/intro-agents-mcp)
- [Oddaljeni MCP z Azure Container Apps (Node.js/TypeScript/JavaScript)](https://learn.microsoft.com/samples/azure-samples/mcp-container-ts/mcp-container-ts/)
- [.NET OpenAI MCP agent](https://learn.microsoft.com/samples/azure-samples/openai-mcp-agent-dotnet/openai-mcp-agent-dotnet/)

## Kaj sledi

Naslednje: [Začetek z MCP odjemalci](/03-GettingStarted/02-client/README.md)

**Omejitev odgovornosti**:  
Ta dokument je bil preveden z uporabo AI prevajalske storitve [Co-op Translator](https://github.com/Azure/co-op-translator). Čeprav si prizadevamo za natančnost, vas prosimo, da upoštevate, da avtomatizirani prevodi lahko vsebujejo napake ali netočnosti. Izvirni dokument v njegovem izvirnem jeziku velja za avtoritativni vir. Za pomembne informacije priporočamo strokovni človeški prevod. Nismo odgovorni za morebitna nesporazumevanja ali napačne interpretacije, ki izhajajo iz uporabe tega prevoda.