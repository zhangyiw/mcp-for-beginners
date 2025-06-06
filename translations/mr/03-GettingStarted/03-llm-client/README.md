<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "abbb199eb22fdffa44a0de4db6a5ea49",
  "translation_date": "2025-05-17T10:17:54+00:00",
  "source_file": "03-GettingStarted/03-llm-client/README.md",
  "language_code": "mr"
}
-->
# LLM सह क्लायंट तयार करणे

आतापर्यंत, तुम्ही सर्व्हर आणि क्लायंट कसे तयार करायचे ते पाहिले आहे. क्लायंटने सर्व्हरला त्याची साधने, संसाधने आणि प्रॉम्प्ट्स स्पष्टपणे सूचीबद्ध करण्यासाठी कॉल करण्यास सक्षम केले आहे. तथापि, हे फारसे व्यावहारिक दृष्टिकोन नाही. तुमचा वापरकर्ता एजेंटिक युगात राहतो आणि ते तसे करण्यासाठी प्रॉम्प्ट्स वापरण्याची आणि LLM सह संवाद साधण्याची अपेक्षा करतो. तुमच्या वापरकर्त्यासाठी, तुम्ही तुमच्या क्षमता संग्रहित करण्यासाठी MCP वापरता की नाही याची त्यांना काळजी नाही परंतु ते संवाद साधण्यासाठी नैसर्गिक भाषा वापरण्याची अपेक्षा करतात. तर आपण हे कसे सोडवू? उपाय म्हणजे क्लायंटला LLM जोडणे.

## विहंगावलोकन

या धड्यात आम्ही तुमच्या क्लायंटला LLM जोडण्यावर लक्ष केंद्रित करतो आणि तुमच्या वापरकर्त्यासाठी हे किती चांगला अनुभव प्रदान करते ते दर्शवतो.

## शिक्षण उद्दिष्टे

या धड्याच्या शेवटी, आपण सक्षम असाल:

- LLM सह क्लायंट तयार करा.
- LLM वापरून MCP सर्व्हरसह सहज संवाद साधा.
- क्लायंट साइडवर अंतिम वापरकर्ता अनुभव चांगला प्रदान करा.

## दृष्टिकोन

आम्ही कोणता दृष्टिकोन घेणे आवश्यक आहे ते समजून घेण्याचा प्रयत्न करूया. LLM जोडणे सोपे वाटते, परंतु आपण खरोखर असे करणार आहोत का?

क्लायंट सर्व्हरसह कसे संवाद साधेल:

1. सर्व्हरशी कनेक्शन स्थापित करा.

2. क्षमता, प्रॉम्प्ट्स, संसाधने आणि साधने सूचीबद्ध करा आणि त्यांची योजना जतन करा.

3. LLM जोडा आणि जतन केलेल्या क्षमता आणि त्यांची योजना LLM ला समजणाऱ्या स्वरूपात पास करा.

4. क्लायंटद्वारे सूचीबद्ध केलेल्या साधनांसह ते LLM ला पास करून वापरकर्ता प्रॉम्प्ट हाताळा.

मस्त, आता आपल्याला उच्च स्तरावर हे कसे करता येईल हे समजले आहे, चला खालील व्यायामात प्रयत्न करूया.

## व्यायाम: LLM सह क्लायंट तयार करणे

या व्यायामात, आम्ही आमच्या क्लायंटमध्ये LLM जोडायला शिकू.

### -1- सर्व्हरशी कनेक्ट करा

चला प्रथम आपला क्लायंट तयार करूया:
आपण ऑक्टोबर 2023 पर्यंत डेटा वर प्रशिक्षित आहात. 

छान, आपल्या पुढील चरणासाठी, चला सर्व्हरवरील क्षमता सूचीबद्ध करूया.

### -2 सर्व्हर क्षमता सूचीबद्ध करा

आता आम्ही सर्व्हरशी कनेक्ट होऊ आणि त्याच्या क्षमता विचारू:

### -3- सर्व्हर क्षमता LLM साधनांमध्ये रूपांतरित करा

सर्व्हर क्षमता सूचीबद्ध केल्यानंतर पुढील चरण म्हणजे त्यांना LLM समजणाऱ्या स्वरूपात रूपांतरित करणे. एकदा आपण असे केल्यावर, आम्ही या क्षमता आमच्या LLM ला साधन म्हणून प्रदान करू शकतो.

छान, आम्ही कोणत्याही वापरकर्ता विनंत्या हाताळण्यासाठी सेट केलेले नाही, म्हणून चला पुढे जाऊया.

### -4- वापरकर्ता प्रॉम्प्ट विनंती हाताळा

कोडच्या या भागात, आम्ही वापरकर्ता विनंत्या हाताळू.

छान, तुम्ही ते केले!

## असाइनमेंट

व्यायामातील कोड घ्या आणि काही अधिक साधनांसह सर्व्हर तयार करा. नंतर व्यायामातील प्रमाणे LLM सह क्लायंट तयार करा आणि सर्व्हर साधने गतिशीलपणे कॉल केल्याची खात्री करण्यासाठी वेगवेगळ्या प्रॉम्प्ट्ससह याची चाचणी करा. क्लायंट तयार करण्याचा हा मार्ग म्हणजे अंतिम वापरकर्त्यासाठी उत्कृष्ट वापरकर्ता अनुभव असेल कारण ते अचूक क्लायंट आदेशाऐवजी प्रॉम्प्ट्स वापरण्यास सक्षम आहेत आणि कोणत्याही MCP सर्व्हरला कॉल केल्याचे त्यांना माहीत नसते.

## उपाय

[Solution](/03-GettingStarted/03-llm-client/solution/README.md)

## मुख्य मुद्दे

- तुमच्या क्लायंटला LLM जोडणे MCP सर्व्हरशी संवाद साधण्यासाठी वापरकर्त्यांसाठी चांगला मार्ग प्रदान करते.
- तुम्हाला MCP सर्व्हर प्रतिसाद LLM समजू शकेल अशा स्वरूपात रूपांतरित करण्याची आवश्यकता आहे.

## नमुने

- [Java Calculator](../samples/java/calculator/README.md)
- [.Net Calculator](../../../../03-GettingStarted/samples/csharp)
- [JavaScript Calculator](../samples/javascript/README.md)
- [TypeScript Calculator](../samples/typescript/README.md)
- [Python Calculator](../../../../03-GettingStarted/samples/python)

## अतिरिक्त संसाधने

## पुढे काय

- पुढे: [Visual Studio Code वापरून सर्व्हरचा उपभोग घेणे](/03-GettingStarted/04-vscode/README.md)

**अस्वीकरण**:  
हा दस्तऐवज AI भाषांतर सेवा [Co-op Translator](https://github.com/Azure/co-op-translator) वापरून अनुवादित केला गेला आहे. आम्ही अचूकतेसाठी प्रयत्नशील असलो तरी कृपया जाणून घ्या की स्वयंचलित अनुवादांमध्ये त्रुटी किंवा अचूकतेचा अभाव असू शकतो. मूळ भाषेतील दस्तऐवज प्राधिकृत स्रोत मानला जावा. अत्यावश्यक माहितीकरिता, व्यावसायिक मानव अनुवादाची शिफारस केली जाते. या अनुवादाच्या वापरामुळे उद्भवलेल्या कोणत्याही गैरसमज किंवा चुकीच्या अर्थाबद्दल आम्ही जबाबदार नाही.