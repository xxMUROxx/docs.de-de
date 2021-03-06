---
title: Serialisierung (Visual Basic) | Microsoft-Dokumentation
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
ms.assetid: 67379a76-5465-4af8-a781-0b0b25a62d9a
caps.latest.revision: 3
author: dotnet-bot
ms.author: dotnetcontent
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 9fb4404bf648f108a3b98952234d29e2bc1d4189
ms.lasthandoff: 03/13/2017

---
# <a name="serialization-visual-basic"></a>Serialisierung (Visual Basic)
Serialisierung ist der Prozess der Konvertierung eines Objekts in einen Bytestream, um das Objekt zu speichern oder in den Arbeitsspeicher, eine Datenbank oder eine Datei zu übertragen. Hauptzweck ist es, den Zustand eines Objekts zu speichern, um es bei Bedarf neu erstellen zu können. Der umgekehrte Vorgang wird als Deserialisierung bezeichnet.  
  
## <a name="how-serialization-works"></a>Funktionsweise der Serialisierung  
 Diese Abbildung zeigt den gesamten Ablauf der Serialisierung.  
  
 ![Grafik zur Serialisierung](../../../../csharp/programming-guide/concepts/serialization/media/serialization.gif "Serialisierung")  
  
 Das Objekt wird in einen Stream serialisiert, der nicht nur die Daten, sondern auch Informationen über den Typ des Objekts enthält, wie z.B. Version, Kultur und Assemblyname. Aus diesem Stream kann das Objekt in einer Datenbank, einer Datei oder einem Arbeitsspeicher gespeichert werden.  
  
### <a name="uses-for-serialization"></a>Verwendungszwecke der Serialisierung  
 Mit der Serialisierung kann der Entwickler den Zustand eines Objekts speichern und das Objekt bei Bedarf neu erstellen. Die Serialisierung stellt sowohl Objektspeicher als auch Datenaustausch bereit. Über die Serialisierung kann ein Entwickler verschiedene Aktionen durchführen, wie z.B.: Senden des Objekts an eine Remoteanwendung mithilfe eines Webdiensts, Übergeben eines Objekts von einer Domäne an eine andere, Übergeben eines Objekts über eine Firewall als XML-Zeichenfolge oder anwendungsübergreifendes Verwalten der Sicherheit oder benutzerspezifischer Informationen.  
  
### <a name="making-an-object-serializable"></a>Aktivieren der Serialisierbarkeit eines Objekts  
 Um ein Objekt zu serialisieren, benötigen Sie das entsprechende Objekt, einen Stream, der das serialisierte Objekt enthalten soll, und einen <xref:System.Runtime.Serialization.Formatter>. <xref:System.Runtime.Serialization> enthält die erforderlichen Klassen zum Serialisieren und Deserialisieren von Objekten.  
  
 Wenden Sie das <xref:System.SerializableAttribute>-Attribut auf einen Typ an, um anzugeben, dass Instanzen dieses Typs serialisiert werden können. Eine <xref:System.Runtime.Serialization.SerializationException>-Ausnahme wird ausgelöst, wenn Sie versuchen, eine Serialisierung durchzuführen, der Typ aber kein <xref:System.SerializableAttribute>-Attribut aufweist.  
  
 Wenn ein Feld innerhalb Ihrer Klasse nicht serialisierbar sein soll, wenden Sie das <xref:System.NonSerializedAttribute>-Attribut an. Wenn ein Feld eines serialisierbaren Typs einen Zeiger, ein Handle oder eine andere Datenstruktur enthält, der/die für eine bestimmte Umgebung spezifisch ist, und das Feld in keiner anderen Umgebung sinnvoll wiederhergestellt werden kann, sollten Sie den Typ als nicht serialisierbar markieren.  
  
 Wenn eine serialisierte Klasse Verweise auf Objekte anderer Klassen enthält, die mit <xref:System.SerializableAttribute> markiert sind, werden diese Objekte ebenfalls serialisiert.  
  
## <a name="binary-and-xml-serialization"></a>Binär- und XML-Serialisierung  
 Es kann entweder Binär- oder XML-Serialisierung verwendet werden. Bei der binären Serialisierung werden alle Member einschließlich der schreibgeschützten Member serialisiert, und die Leistung wird verbessert. Die XML-Serialisierung bietet besser lesbaren Code sowie größere Flexibilität bei der Objektfreigabe und -nutzung zu Interoperabilitätszwecken.  
  
### <a name="binary-serialization"></a>Binäre Serialisierung  
 Die binäre Serialisierung verwendet die binäre Codierung, um für Nutzungszwecke wie z.B. Speicherung oder socketbasierte Netzwerkstreams eine kompakte Serialisierung zu ermöglichen.  
  
### <a name="xml-serialization"></a>XML-Serialisierung  
 Bei der XML-Serialisierung werden die öffentlichen Felder und Eigenschaften eines Objekts bzw. die Parameter und Rückgabewerte von Methoden in einen XML-Stream serialisiert, der einem bestimmtem XSD-Dokument (XML Schema Definition) entspricht. Die XML-Serialisierung führt zu stark typisierten Klassen mit öffentlichen Eigenschaften und Feldern, die in XML konvertiert werden. <xref:System.Xml.Serialization> enthält die erforderlichen Klassen zum Serialisieren und Deserialisieren von XML-Code.  
  
 Sie können Attribute auf Klassen und Klassenmember anwenden, um die Art der Serialisierung oder Deserialisierung einer Instanz der Klasse durch <xref:System.Xml.Serialization.XmlSerializer> zu steuern.  
  
## <a name="basic-and-custom-serialization"></a>Einfache und benutzerdefinierte Serialisierung  
 Die Serialisierung kann auf zwei Arten ausgeführt werden: einfach und benutzerdefiniert. Die einfache Serialisierung verwendet .NET Framework, um ein Objekt automatisch zu serialisieren.  
  
### <a name="basic-serialization"></a>Einfache Serialisierung  
 Die einzige Anforderung bei der einfachen Serialisierung ist, dass das <xref:System.SerializableAttribute>-Attribut auf das Objekt angewendet wurde. Das <xref:System.NonSerializedAttribute>-Attribut kann verwendet werden, um die Serialisierung bestimmter Felder zu verhindern.  
  
 Wenn Sie die einfache Serialisierung verwenden, kann die Versionsverwaltung von Objekten zu Problemen führen. In diesem Fall empfiehlt sich die benutzerdefinierte Serialisierung. Die einfache Serialisierung ist die einfachste Möglichkeit zur Serialisierung, bietet allerdings nicht viel Steuerungsmöglichkeiten für den Prozess.  
  
### <a name="custom-serialization"></a>Benutzerdefinierte Serialisierung  
 Bei der benutzerdefinierten Serialisierung können Sie genau angeben, welche Objekte serialisiert werden und wie die Serialisierung erfolgt. Die Klasse muss mit <xref:System.SerializableAttribute> markiert werden und die <xref:System.Runtime.Serialization.ISerializable>-Schnittstelle implementieren.  
  
 Wenn das Objekt auch benutzerdefiniert deserialisiert werden soll, müssen Sie einen benutzerdefinierten Konstruktor verwenden.  
  
## <a name="designer-serialization"></a>Designerserialisierung  
 Die Designerserialisierung ist eine besondere Form der Serialisierung, die auch die Art der Objektpersistenz einbezieht, die in der Regel mit Entwicklungstools verknüpft ist. Bei der Designerserialisierung handelt es sich um den Prozess der Konvertierung eines Objektdiagramms in eine Quelldatei, die später zum Wiederherstellen des Objektdiagramms verwendet werden kann. Eine Quelldatei kann Code-, Markup- oder sogar SQL-Tabelleninformationen enthalten.  
  
##  <a name="BKMK_RelatedTopics"></a> Verwandte Themen und Beispiele  
 [Exemplarische Vorgehensweise: Beibehalten eines Objekts in Visual Studio (Visual Basic)](../../../../visual-basic/programming-guide/concepts/serialization/walkthrough-persisting-an-object-in-visual-studio.md)  
 Veranschaulicht, wie die Serialisierung verwendet werden kann, um die Daten eines Objekts zwischen Instanzen beizubehalten. Dadurch können Sie Werte speichern und abrufen, wenn das Objekt das nächste Mal instanziiert wird.  
  
 [Gewusst wie: Lesen von Objektdaten aus einer XML-Datei (Visual Basic)](../../../../visual-basic/programming-guide/concepts/serialization/how-to-read-object-data-from-an-xml-file.md)  
 Zeigt, wie Objektdaten gelesen werden, die zuvor mit der <xref:System.Xml.Serialization.XmlSerializer>-Klasse in eine XML-Datei geschrieben wurden.  
  
 [Gewusst wie: Schreiben von Objektdaten in eine XML-Datei (Visual Basic)](../../../../visual-basic/programming-guide/concepts/serialization/how-to-write-object-data-to-an-xml-file.md)  
 Zeigt, wie das Objekt mit der <xref:System.Xml.Serialization.XmlSerializer>-Klasse von einer Klasse in eine XML-Datei geschrieben wird.
