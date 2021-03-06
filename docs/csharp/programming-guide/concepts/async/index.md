---
title: "Asynchrone Programmierung mit „async“ und „await“ (C#) | Microsoft-Dokumentation"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: 9bcf896a-5826-4189-8c1a-3e35fa08243a
caps.latest.revision: 5
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 931f984b2267d0782cdbad22cb00f7a698de357e
ms.lasthandoff: 03/13/2017

---
# <a name="asynchronous-programming-with-async-and-await-c"></a>Asynchrone Programmierung mit „async“ und „await“ (C#)
Sie können Leistungsengpässe vermeiden und die Reaktionsfähigkeit der Anwendung insgesamt verbessern, indem Sie asynchrone Programmierung verwenden. Allerdings können herkömmliche Verfahren zum Schreiben von asynchronen Anwendungen kompliziert sein, weshalb es schwierig ist, diese Anwendungen zu schreiben, zu debuggen und zu verwalten.  
  
 In Visual Studio 2012 wurde der vereinfachte Ansatz der asynchronen Programmierung eingeführt, der die asynchrone Unterstützung in .NET Framework 4.5 und höher sowie in der Windows-Runtime nutzt. Der Compiler übernimmt die schwierige Arbeit, die zuvor vom Entwickler ausgeführt wurde, und die Anwendung behält eine logische Struktur bei, die synchronem Code ähnelt. So erhalten Sie alle Vorteile der asynchronen Programmierung mit einem Bruchteil des Aufwands.  
  
 Dieses Thema enthält eine Übersicht über die Verwendungsmöglichkeiten der asynchronen Programmierung sowie Links zu Supportthemen, die weitere Informationen und Beispiele enthalten.  
  
##  <a name="BKMK_WhentoUseAsynchrony"></a> Async verbessert die Reaktionsfähigkeit  
 Asynchronität ist wesentlich für potenziell blockierende Aktivitäten, z. B. wenn die Anwendung auf das Internet zugreift. Der Zugriff auf eine Webressource ist manchmal langsam oder verzögert. Wenn eine solche Aktivität innerhalb eines synchronen Prozesses blockiert wird, muss die gesamte Anwendung warten. In einem asynchronen Prozess kann die Anwendung mit anderer Arbeit fortfahren, die nicht von der Webressource abhängig ist, bis die potenziell blockierende Aufgabe beendet ist.  
  
 In der folgenden Tabelle werden typische Bereichen angezeigt, in denen die asynchrone Programmierung die Reaktionsfähigkeit verbessert. Die aufgelisteten APIs von .NET Framework 4.5 und der Windows-Runtime enthalten Methoden, die die asynchrone Programmierung unterstützen.  
  
|Anwendungsbereich|Unterstützende APIs, die asynchrone Methoden enthalten|  
|----------------------|------------------------------------------------|  
|Webzugriff|<xref:System.Net.Http.HttpClient>, [SyndicationClient](http://go.microsoft.com/fwlink/p/?LinkId=259441)|  
|Arbeiten mit Dateien|[StorageFile](http://go.microsoft.com/fwlink/p/?LinkId=248220), <xref:System.IO.StreamWriter>, <xref:System.IO.StreamReader>, <xref:System.Xml.XmlReader>|  
|Arbeiten mit Bildern|[MediaCapture](http://go.microsoft.com/fwlink/p/?LinkId=261839), [BitmapEncoder](http://go.microsoft.com/fwlink/p/?LinkId=261840), [BitmapDecoder](http://go.microsoft.com/fwlink/p/?LinkId=261841)|  
|WCF-Programmierung|[Synchrone und asynchrone Vorgänge](http://go.microsoft.com/fwlink/p/?LinkID=192382)|   
  
Asynchronität erweist sich als besonders nützlich bei Prozessen, die durch den UI-Thread ausgeführt werden, da sämtliche auf die Benutzeroberfläche bezogenen Aktivitäten sich gemeinhin diesen Thread teilen. Ist ein Prozess in einer synchronen Anwendung blockiert, werden alle blockiert. Die Anwendung reagiert nicht mehr, und Sie denken möglicherweise, es sei ein Fehler aufgetreten, wenn die Anwendung tatsächlich nur wartet.  
  
 Wenn Sie asynchrone Methoden verwenden, reagiert die Anwendung weiterhin auf die Benutzeroberfläche. Sie können beispielsweise die Fenstergröße ändern, das Fenster minimieren oder die Anwendung schließen, wenn Sie nicht warten möchten, bis sie fertig ist.  
  
 Durch den auf Asynchronität basierenden Ansatz wird eine Entsprechung für die automatische Übertragung an die Liste der Optionen hinzugefügt, die beim Entwerfen von asynchronen Vorgängen zur Auswahl stehen. Sie erhalten also alle Vorteile der herkömmlichen asynchronen Programmierung, der Aufwand des Entwicklers ist jedoch wesentlich geringer.  
  
##  <a name="BKMK_HowtoWriteanAsyncMethod"></a> Asynchrone Methoden sind einfacher zu schreiben  
 Die Schlüsselwörter [async](../../../../csharp/language-reference/keywords/async.md) und [await](../../../../csharp/language-reference/keywords/await.md) in C# sind das Kernstück der asynchronen Programmierung. Wenn Sie diese beiden Schlüsselwörter verwenden, können Sie eine asynchrone Methode mithilfe von Ressourcen in .NET Framework oder Windows-Runtime fast genauso einfach erstellen wie eine synchrone Methode. Asynchrone Methoden, die Sie unter Verwendung von `async` und `await` definieren, werden als async-Methoden bezeichnet.  
  
 Das folgende Beispiel zeigt eine Async-Methode. Fast der gesamte Code sollte Ihnen bekannt vorkommen. Die Kommentare rufen die von Ihnen hinzugefügten Funktionen auf, um Asynchronität zu erstellen.  
  
 Eine vollständige WPF-Beispieldatei (Windows Presentation Foundation) finden Sie am Ende dieses Themas, und Sie können das Beispiel unter [Async Sample: Example from "Asynchronous Programming with Async and Await"](http://go.microsoft.com/fwlink/?LinkID=261549) (Beispiel aus der asynchronen Programmierung mit „Async“ und „Await“) herunterladen.  
  
```csharp  
// Three things to note in the signature:  
//  - The method has an async modifier.   
//  - The return type is Task or Task<T>. (See "Return Types" section.)  
//    Here, it is Task<int> because the return statement returns an integer.  
//  - The method name ends in "Async."  
async Task<int> AccessTheWebAsync()  
{   
    // You need to add a reference to System.Net.Http to declare client.  
    HttpClient client = new HttpClient();  
  
    // GetStringAsync returns a Task<string>. That means that when you await the  
    // task you'll get a string (urlContents).  
    Task<string> getStringTask = client.GetStringAsync("http://msdn.microsoft.com");  
  
    // You can do work here that doesn't rely on the string from GetStringAsync.  
    DoIndependentWork();  
  
    // The await operator suspends AccessTheWebAsync.  
    //  - AccessTheWebAsync can't continue until getStringTask is complete.  
    //  - Meanwhile, control returns to the caller of AccessTheWebAsync.  
    //  - Control resumes here when getStringTask is complete.   
    //  - The await operator then retrieves the string result from getStringTask.  
    string urlContents = await getStringTask;  
  
    // The return statement specifies an integer result.  
    // Any methods that are awaiting AccessTheWebAsync retrieve the length value.  
    return urlContents.Length;  
}  
```  
  
 Wenn für `AccessTheWebAsync` zwischen dem Aufrufen von `GetStringAsync` und dem Warten auf die Beendigung keine Arbeit ansteht, können Sie den Code durch Aufrufen und Warten in der folgenden Anweisung vereinfachen.  
  
```csharp  
string urlContents = await client.GetStringAsync();  
```  
  
 Die folgenden Eigenschaften fassen zusammen, wodurch das vorherige Beispiel sich als eine asynchrone Methode auszeichnet.  
  
-   Die Methodensignatur enthält einen `async`-Modifizierer.  
  
-   Der Name einer Async-Methode endet mit dem Suffix "Async".  
  
-   Der Rückgabetyp ist einer der folgenden Typen:  
  
    -   <xref:System.Threading.Tasks.Task%601>, wenn Ihre Methode eine return-Anweisung aufweist, in der der Typ des Operanden „TResult“ ist.  
  
    -   <xref:System.Threading.Tasks.Task>, wenn die Methode keine return-Anweisung oder eine return-Anweisung ohne Operanden aufweist.  
  
    -   `Void`, wenn Sie einen asynchronen Ereignishandler schreiben.  
  
     Weitere Informationen finden Sie unter „Rückgabetypen und Parameter“ weiter unten in diesem Thema.  
  
-   Die Methode umfasst normalerweise mindestens einen await-Ausdruck, der einen Punkt kennzeichnet, an dem die Methode erst nach Abschluss des erwarteten Vorgangs fortgesetzt werden kann. In der Zwischenzeit wird die Methode angehalten, und die Steuerung kehrt zum Aufrufer der Methode zurück. Im nächsten Abschnitt dieses Themas wird veranschaulicht, was am Unterbrechungspunkt geschieht.  
  
 In Asynch-Methoden verwenden Sie die bereitgestellten Schlüsselwörter und Typen, um die gewünschten Aktionen anzugeben. Der Compiler übernimmt den Rest, er verfolgt auch, was geschehen muss, wenn die Steuerung zu einem await-Punkt in einer angehaltenen Methode zurückkehrt. Einige Routinenprozesse, beispielsweise Schleifen und Ausnahmebehandlung, sind im herkömmlichen asynchronen Code möglicherweise schwierig zu handhaben. In einer Async-Methode schreiben Sie diese Elemente fast so wie in einer synchronen Lösung, und das Problem ist gelöst.  
  
 Weitere Informationen zur Asynchronität in vorherigen .NET Framework-Versionen finden Sie unter [TPL und herkömmliche asynchrone Programmierung in .NET Framework](http://msdn.microsoft.com/library/e7b31170-a156-433f-9f26-b1fc7cd1776f).  
  
##  <a name="BKMK_WhatHappensUnderstandinganAsyncMethod"></a> Was geschieht in einer asynchronen Methode?  
 Bei der asynchronen Programmierung ist es sehr wichtig zu verstehen, wie die Ablaufsteuerung von Methode zu Methode springt. In dem folgenden Diagramm werden Sie durch den Prozess geführt.  
  
 ![Asynchrones Programm verfolgen](../../../../csharp/programming-guide/concepts/async/media/navigationtrace.png "NavigationTrace")  
  
 Die Zahlen im Diagramm entsprechen den folgenden Schritten.  
  
1.  Ein Ereignishandler ruft die Async-Methode `AccessTheWebAsync` auf und wartet auf sie.  
  
2.  `AccessTheWebAsync` erstellt eine <xref:System.Net.Http.HttpClient>-Instanz und ruft die asynchrone <xref:System.Net.Http.HttpClient.GetStringAsync%2A>-Methode auf, um die Inhalte einer Website als Zeichenfolge herunterzuladen.  
  
3.  In `GetStringAsync` geschieht etwas, durch das die Ausführung angehalten wird. Möglicherweise muss gewartet werden, bis eine Website heruntergeladen oder eine andere blockierende Aktivität ausgeführt wurde. Um blockierende Ressourcen zu vermeiden, übergibt die Methode `GetStringAsync` die Steuerung an ihren Aufrufer `AccessTheWebAsync`.  
  
     `GetStringAsync` gibt ein <xref:System.Threading.Tasks.Task%601>-Element zurück, bei dem `TResult` eine Zeichenfolge ist, und `AccessTheWebAsync` weist den Task der `getStringTask`-Variablen zu. Die Aufgabe stellt den laufenden Prozess für den Aufruf von `GetStringAsync` dar, mit der Festlegung, dass bei Abschluss der Arbeit ein tatsächlicher Zeichenfolgenwert erzeugt wurde.  
  
4.  Da `getStringTask` noch nicht abgewartet wurde, kann `AccessTheWebAsync` mit anderer Arbeit fortfahren, die nicht vom Endergebnis von `GetStringAsync` abhängt. Diese Aufgaben werden durch einen Aufruf der synchronen Methode `DoIndependentWork` dargestellt.  
  
5.  `DoIndependentWork` ist eine synchrone Methode, die ihre Arbeit ausführt und zu ihrem Aufrufer zurückkehrt.  
  
6.  Für `AccessTheWebAsync` steht keine Arbeit mehr an, die die Methode ohne ein Ergebnis von `getStringTask` ausführen kann. `AccessTheWebAsync` möchte als Nächstes die Länge der heruntergeladenen Zeichenfolge berechnen und zurückgeben, aber die Methode kann diesen Wert erst berechnen, wenn sie über die Zeichenfolge verfügt.  
  
     Daher verwendet `AccessTheWebAsync` einen await-Operator, um die Ausführung anzuhalten und die Steuerung an die Methode zu übergeben, von der `AccessTheWebAsync` aufgerufen wurde. `AccessTheWebAsync` gibt ein `Task<int>`-Element an den Aufrufer zurück. Die Aufgabe stellt eine Zusicherung dar, ein Ganzzahlergebnis zu erzeugen, das die Länge der heruntergeladenen Zeichenfolge ist.  
  
    > [!NOTE]
    >  Wenn die Methode `GetStringAsync` (und daher `getStringTask`) abgeschlossen ist, bevor `AccessTheWebAsync` auf sie wartet, verbleibt die Steuerung bei `AccessTheWebAsync`. Der Aufwand des Anhaltens und der Rückkehr zu `AccessTheWebAsync` wäre Verschwendung, wenn der aufgerufene asynchrone Prozess (`getStringTask`) bereits abgeschlossen ist und "AccessTheWebSync" nicht auf das Endergebnis warten muss.  
  
     Innerhalb des Aufrufers (der Ereignishandler in diesem Beispiel) wird das Prozessmuster fortgesetzt. Der Aufrufer führt möglicherweise andere Arbeit aus, die nicht von dem Ergebnis von `AccessTheWebAsync` abhängt, bevor er auf dieses Ergebnis wartet, oder der Aufrufer wartet sofort auf das Ergebnis.   Der Ereignishandler wartet auf `AccessTheWebAsync`, und `AccessTheWebAsync` wartet auf `GetStringAsync`.  
  
7.  `GetStringAsync` wird abgeschlossen und erstellt ein Zeichenfolgenergebnis. Das Zeichenfolgenergebnis wird von dem Aufruf `GetStringAsync` nicht auf die Art zurückgegeben, die Sie möglicherweise erwarten. (Beachten Sie, dass die Methode bereits in Schritt 3 eine Aufgabe zurückgegeben hat.) Stattdessen wird das Zeichenfolgenergebnis in dem Task gespeichert, der den Abschluss der Methode darstellt: `getStringTask`. Der await-Operator ruft das Ergebnis von `getStringTask` ab. Die Zuweisungsanweisung weist `urlContents` das abgerufene Ergebnis zu.  
  
8.  Wenn `AccessTheWebAsync` über das Zeichenfolgenergebnis verfügt, kann die Methode die Länge der Zeichenfolge berechnen. Dann ist die Arbeit von `AccessTheWebAsync` auch abgeschlossen, und der wartende Ereignishandler kann fortfahren. Im vollständigen Beispiel am Ende des Themas können Sie überprüfen, ob der Ereignishandler den Wert des Längenergebnisses abruft und druckt.  
  
 Wenn die asynchrone Programmierung für Sie neu ist, nehmen Sie sich einen Moment Zeit, um den Unterschied zwischen synchronem und asynchronem Verhalten zu untersuchen. Eine synchrone Methode kehrt zum Aufrufer zurück, wenn ihre Arbeit abgeschlossen ist (Schritt 5). Eine asynchrone Methode hingegen gibt einen Aufgabenwert zurück, wenn die Verarbeitung angehalten wird (Schritt 3 und 6). Wenn die asynchrone Methode schließlich ihre Arbeit abgeschlossen hat, wird die Aufgabe als abgeschlossen markiert und das Ergebnis ggf. in der Aufgabe gespeichert.  
  
 Weitere Informationen zur Ablaufsteuerung finden Sie unter [Ablaufsteuerung in asynchronen Programmen (C#)](../../../../csharp/programming-guide/concepts/async/control-flow-in-async-programs.md).  
  
##  <a name="BKMK_APIAsyncMethods"></a> API-Async-Methoden  
 Sie fragen sich möglicherweise, wo Methoden wie `GetStringAsync` zu finden sind, die die asynchrone Programmierung unterstützen. .NET Framework 4.5 oder höher enthält viele Member, die mit `async` und `await` funktionieren. Erkennen können Sie diese Member am Suffix „Async“, das an den Membernamen angefügt ist, und am Rückgabetyp <xref:System.Threading.Tasks.Task> oder <xref:System.Threading.Tasks.Task%601>. Beispiel: Die `System.IO.Stream`-Klasse enthält Methoden wie <xref:System.IO.Stream.CopyToAsync%2A>, <xref:System.IO.Stream.ReadAsync%2A> und <xref:System.IO.Stream.WriteAsync%2A> sowie die synchronen Methoden <xref:System.IO.Stream.CopyTo%2A>, <xref:System.IO.Stream.Read%2A> und <xref:System.IO.Stream.Write%2A>.  
  
 Die Windows-Runtime enthält außerdem viele Methoden, die Sie mit `async` und `await` in Windows-Apps verwenden können. Weitere Informationen und Beispielmethoden finden Sie unter [Schnellstart: Verwenden des await-Operators für die asynchrone Programmierung](http://go.microsoft.com/fwlink/?LinkId=248545), [Asynchrone Programmierung (Windows Store-Apps)](http://go.microsoft.com/fwlink/?LinkId=259592) und [WhenAny: Überbrückung zwischen .NET Framework und Windows-Runtime](https://msdn.microsoft.com/library/jj635140(v=vs.120).aspx).  
  
##  <a name="BKMK_Threads"></a> Threads  
 Async-Methoden sollen nicht blockierende Vorgänge sein. Ein `await`-Ausdruck in einer asynchronen Methode blockiert den aktuellen Thread nicht, während der abgewartete Task ausgeführt wird. Stattdessen registriert der Ausdruck den Rest der Methode als Fortsetzung und gibt die Steuerung an den Aufrufer der Async-Methode zurück.  
  
 Durch die Schlüsselwörter `async` und `await` werden keine zusätzlichen Threads erstellt. Async-Methoden erfordern kein Multithreading, da eine Async-Methode nicht in einem eigenen Thread ausgeführt wird. Die Methode wird im aktuellen Synchronisierungskontext ausgeführt und verwendet Zeit im Thread nur, wenn sie aktiv ist. Sie können das <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=fullName>-Element verwenden, um die CPU-gebundene Verarbeitung in einen Hintergrundthread zu verschieben. Ein Hintergrundthread hilft Ihnen allerdings nicht bei einem Prozess, der nur darauf wartet, dass Ergebnisse verfügbar werden.  
  
 Der auf Asynchronität basierende Ansatz der asynchronen Programmierung ist vorhandenen Ansätzen in nahezu jedem Fall vorzuziehen. Insbesondere eignet sich dieser Ansatz besser für EA-gebundene Vorgänge als <xref:System.ComponentModel.BackgroundWorker>, da der Code einfacher ist und kein Schutz vor Racebedingungen erforderlich ist. In Kombination mit <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=fullName> eignet sich die asynchrone Programmierung besser für CPU-gebundene Vorgänge als <xref:System.ComponentModel.BackgroundWorker>, da die asynchrone Programmierung die Koordinierungsdetails der Ausführung Ihres Codes von der Arbeit trennt, die `Task.Run` an den Threadpool überträgt.  
  
##  <a name="BKMK_AsyncandAwait"></a> „async“ und „await“  
 Wenn Sie angeben, dass eine Methode eine asynchrone Methode ist, indem Sie einen [async](../../../../csharp/language-reference/keywords/async.md)-Modifizierer verwenden, aktivieren Sie die folgenden zwei Funktionen.  
  
-   Die markierte async-Methode kann [await](../../../../csharp/language-reference/keywords/await.md) verwenden, um Unterbrechungspunkte festzulegen. Der await-Operator informiert den Compiler, dass die Async-Methode erst über diesen Punkt hinaus fortgesetzt werden kann, wenn der abgewartete asynchrone Prozess abgeschlossen ist. In der Zwischenzeit kehrt die Steuerung zum Aufrufer der Async-Methode zurück.  
  
     Die Unterbrechung einer async-Methode an einem `await`-Ausdruck stellt keine Beendigung der Methode dar, und `finally`-Blöcke werden nicht ausgeführt.  
  
-   Auf die markierte Async-Methode können auch Methoden warten, die sie aufrufen.  
  
 Eine async-Methode enthält in der Regel eine oder mehrere Vorkommen eines `await`-Operators, die Abwesenheit von `await`-Ausdrücken verursacht jedoch keinen Compilerfehler. Wenn eine async-Methode keinen `await`-Operator verwendet, um einen Unterbrechungspunkt zu markieren, wird die Methode ungeachtet des `async`-Modifizierers wie eine synchrone Methode ausgeführt. Der Compiler gibt eine Warnung für solche Methoden aus.  
  
 `async` und `await` sind kontextbezogene Schlüsselwörter. Weitere Informationen und Beispiele finden Sie unter den folgenden Themen:  
  
-   [async](../../../../csharp/language-reference/keywords/async.md)  
  
-   [await](../../../../csharp/language-reference/keywords/await.md)  
  
##  <a name="BKMK_ReturnTypesandParameters"></a> Rückgabetypen und Parameter  
 In der .NET Framework-Programmierung gibt eine asynchrone Methode in der Regel ein <xref:System.Threading.Tasks.Task>- oder ein <xref:System.Threading.Tasks.Task%601>-Element zurück. Innerhalb einer async-Methode wird ein `await`-Operator auf einen Task angewendet, der in einem Aufruf einer anderen async-Methode zurückgegeben wurde.  
  
 Sie geben <xref:System.Threading.Tasks.Task%601> als Rückgabetyp an, falls die Methode eine [return](../../../../csharp/language-reference/keywords/return.md)-Anweisung enthält, die einen Operanden vom Typ `TResult` angibt.  
  
 Sie verwenden `Task` als Rückgabetyp, wenn die Methode keine return-Anweisung hat oder über eine return-Anweisung verfügt, die keinen Operanden zurückgibt.  
  
 Das folgende Beispiel zeigt, wie Sie eine Methode deklarieren und aufrufen, die ein <xref:System.Threading.Tasks.Task%601>- oder ein <xref:System.Threading.Tasks.Task>-Element zurückgibt.  
  
```csharp  
// Signature specifies Task<TResult>  
async Task<int> TaskOfTResult_MethodAsync()  
{  
    int hours;  
    // . . .  
    // Return statement specifies an integer result.  
    return hours;  
}  
  
// Calls to TaskOfTResult_MethodAsync  
Task<int> returnedTaskTResult = TaskOfTResult_MethodAsync();  
int intResult = await returnedTaskTResult;  
// or, in a single statement  
int intResult = await TaskOfTResult_MethodAsync();  
  
// Signature specifies Task  
async Task Task_MethodAsync()  
{  
    // . . .  
    // The method has no return statement.    
}  
  
// Calls to Task_MethodAsync  
Task returnedTask = Task_MethodAsync();  
await returnedTask;  
// or, in a single statement  
await Task_MethodAsync();  
  
```  
  
 Jede zurückgegebene Aufgabe stellt derzeit ausgeführte Arbeit dar. Eine Aufgabe kapselt Informationen über den Zustand des asynchronen Prozesses und schließlich entweder das Endergebnis aus dem Prozess oder die Ausnahme, die durch einen Prozessfehler verursacht wird.  
  
 Eine asynchrone Methode kann auch den Rückgabetyp `void` aufweisen. Dieser Rückgabetyp wird hauptsächlich zum Definieren von Ereignishandlern verwendet, wobei ein `void`-Rückgabetyp erforderlich ist. Asynchrone Ereignishandler dienen häufig als Ausgangspunkt für asynchrone Programme.  
  
 Eine async-Methode, die eine `void`-Rückgabetyp aufweist, kann nicht abgewartet werden, und der Aufrufer einer Methode zur Rückgabe von „void“ kann keine Ausnahmen abfangen, die von der Methode ausgelöst werden.  
  
 Eine asynchrone Methode kann keine [ref](../../../../csharp/language-reference/keywords/ref.md)- oder [out](../../../../csharp/language-reference/keywords/out.md)-Parameter deklarieren. Die Methode kann jedoch Methoden aufrufen, die solche Parameter aufweisen.  
  
 Weitere Informationen und Beispiele finden Sie unter [Asynchrone Rückgabetypen (C#)](../../../../csharp/programming-guide/concepts/async/async-return-types.md). Weitere Informationen zum Abfangen von Ausnahmen in async-Methoden finden Sie unter [try-catch](../../../../csharp/language-reference/keywords/try-catch.md).  
  
 Asynchrone APIs in der Windows-Runtime-Programmierung weisen einen der folgenden Rückgabetypen auf, die Tasks ähnlich sind:  
  
-   [IAsyncOperation](http://go.microsoft.com/fwlink/p/?LinkId=261896): Entspricht <xref:System.Threading.Tasks.Task%601>.  
  
-   [IAsyncAction](http://go.microsoft.com/fwlink/p/?LinkId=261897): Entspricht <xref:System.Threading.Tasks.Task>.  
  
-   [IAsyncActionWithProgress](http://go.microsoft.com/fwlink/p/?LinkId=261898)  
  
-   [IAsyncOperationWithProgress](http://go.microsoft.com/fwlink/p/?LinkID=259454)  
  
 Weitere Informationen und ein Beispiel finden Sie unter [Schnellstart: Verwenden des await-Operators für die asynchrone Programmierung](http://go.microsoft.com/fwlink/p/?LinkId=248545).  
  
##  <a name="BKMK_NamingConvention"></a> Benennungskonvention  
 Gemäß der Konvention fügen Sie die Zeichenfolge „Async“ an die Namen von Methoden an, die einen `async`-Modifizierer besitzen.  
  
 Sie können die Konvention ignorieren, wenn ein Ereignis, eine Basisklasse oder ein Schnittstellenvertrag einen anderen Namen vorsieht. Beispielsweise sollten Sie allgemeine Ereignishandler wie `Button1_Click` nicht umbenennen.  
  
##  <a name="BKMK_RelatedTopics"></a> Verwandte Themen und Beispiele (Visual Studio)  
  
|Titel|Beschreibung|Beispiel|  
|-----------|-----------------|------------|  
|[Exemplarische Vorgehensweise: Zugreifen auf das Web mit „async“ und „await“ (C#)](../../../../csharp/programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md)|Zeigt, wie eine synchrone WPF-Projektmappe in eine asynchrone WPF-Projektmappe konvertiert wird. Die Anwendung lädt eine Reihe von Websites herunter.|[Async Sample: Accessing the Web Walkthrough](http://go.microsoft.com/fwlink/p/?LinkID=255191&clcid=0x409) (Asynchrones Beispiel: Aufrufen der exemplarischen Vorgehensweise)|  
|[Gewusst wie: Erweitern der asynchronen exemplarischen Vorgehensweise mit Task.WhenAll (C#)](../../../../csharp/programming-guide/concepts/async/how-to-extend-the-async-walkthrough-by-using-task-whenall.md)|Fügt <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=fullName> zur vorherigen exemplarischen Vorgehensweise hinzu. Bei Verwendung von `WhenAll` werden alle Downloads gleichzeitig gestartet.||  
|[Gewusst wie: Paralleles Erstellen mehrerer Webanforderungen mit „async“ und „await“ (C#)](../../../../csharp/programming-guide/concepts/async/how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await.md)|Veranschaulicht, wie mehrere Aufgaben gleichzeitig gestartet werden.|[Async Sample: Make Multiple Web Requests in Parallel](http://go.microsoft.com/fwlink/p/?LinkID=254906&clcid=0x409) (Asynchrones Beispiel: Paralleles Erstellen mehrerer Webanforderungen)|  
|[Asynchrone Rückgabetypen (C#)](../../../../csharp/programming-guide/concepts/async/async-return-types.md)|Veranschaulicht die Typen, die Async-Methoden zurückgeben können und erklärt, wann die einzelnen Typen geeignet sind.||  
|[Ablaufsteuerung in asynchronen Programmen (C#)](../../../../csharp/programming-guide/concepts/async/control-flow-in-async-programs.md)|Verfolgt die Ablaufsteuerung ausführlich durch eine Reihenfolge von await-Ausdrücken in einem asynchronen Programm.|[Async Sample: Control Flow in Async Programs](http://go.microsoft.com/fwlink/p/?LinkID=255285&clcid=0x409) (Asynchrones Beispiel: Ablaufsteuerung in asynchronen Programmen)|  
|[Feinabstimmung der Async-Anwendung (C#)](../../../../csharp/programming-guide/concepts/async/fine-tuning-your-async-application.md)|Zeigt, wie die folgenden Funktionen der asynchronen Lösung hinzugefügt werden:<br /><br /> -   [Eine asynchrone Aufgabe oder Aufgabenliste abbrechen (C#)](../../../../csharp/programming-guide/concepts/async/cancel-an-async-task-or-a-list-of-tasks.md)<br />-   [Asynchrone Aufgaben nach einer Zeitperiode abbrechen (C#)](../../../../csharp/programming-guide/concepts/async/cancel-async-tasks-after-a-period-of-time.md)<br />-   [Verbleibende asynchrone Aufgaben nach Abschluss einer Aufgabe abbrechen (C#)](../../../../csharp/programming-guide/concepts/async/cancel-remaining-async-tasks-after-one-is-complete.md)<br />-   [Mehrere asynchrone Aufgaben starten und nach Abschluss verarbeiten (C#)](../../../../csharp/programming-guide/concepts/async/start-multiple-async-tasks-and-process-them-as-they-complete.md)|[Async Sample: Fine Tuning Your Application](http://go.microsoft.com/fwlink/p/?LinkID=255046&clcid=0x409) (Asynchrones Beispiel: Feinabstimmung der Anwendung)|  
|[Ablauf des erneuten Eintretens in asynchronen Anwendungen (C#)](../../../../csharp/programming-guide/concepts/async/handling-reentrancy-in-async-apps.md)|Zeigt, wie Fälle gehandhabt werden, in denen ein aktiver asynchroner Vorgang neu gestartet wird, während er ausgeführt wird.||  
|[WhenAny: Überbrückung zwischen .NET Framework und Windows-Runtime](https://msdn.microsoft.com/library/jj635140(v=vs.120).aspx)|Zeigt, wie in [!INCLUDE[wrt](../../../../csharp/includes/wrt_md.md)] zwischen Tasktypen in .NET Framework und „IAsyncOperations“ überbrückt wird, sodass <xref:System.Threading.Tasks.Task.WhenAny%2A> mit einer [!INCLUDE[wrt](../../../../csharp/includes/wrt_md.md)]-Methode verwendet werden kann.|[Async Sample: Bridging between .NET and Windows Runtime (AsTask and WhenAny)](http://go.microsoft.com/fwlink/p/?LinkID=260638) (Asynchrones Beispiel: Überbrückung zwischen .NET und Windows-Runtime [„AsTask“ und „WhenAny“])|  
|Asynchroner Abbruch: Überbrückung zwischen .NET Framework und Windows-Runtime|Zeigt, wie in [!INCLUDE[wrt](../../../../csharp/includes/wrt_md.md)] zwischen Tasktypen in .NET Framework und „IAsyncOperations“ überbrückt wird, sodass <xref:System.Threading.CancellationTokenSource> mit einer [!INCLUDE[wrt](../../../../csharp/includes/wrt_md.md)]-Methode verwendet werden kann.|[Async Sample: Bridging between .NET and Windows Runtime (AsTask & Cancellation)](http://go.microsoft.com/fwlink/p/?LinkId=263004) (Asynchrones Beispiel: Überbrückung zwischen .NET und Windows-Runtime [„AsTask“ und „Cancellation“])|  
|[Verwenden von Async für den Dateizugriff (C#)](../../../../csharp/programming-guide/concepts/async/using-async-for-file-access.md)|Listet die Vorteile der Verwendung von "async" und "await" für den Zugriff auf Dateien auf und veranschaulicht sie.||  
|[Taskbasiertes asynchrones Muster (TAP, Task-based Asynchronous Pattern)](http://msdn.microsoft.com/library/8cef1fcf-6f9f-417c-b21f-3fd8bac75007)|Beschreibt ein neues Muster für Asynchronität in .NET Framework. Das Muster basiert auf den Typen <xref:System.Threading.Tasks.Task> und <xref:System.Threading.Tasks.Task%601>.||  
|[Videos zur asynchronen Programmierung auf Channel 9](http://go.microsoft.com/fwlink/p/?LinkID=267466)|Stellt Links zu einer Vielzahl von Videos über die asynchrone Programmierung bereit.||  
  
##  <a name="BKMK_CompleteExample"></a> Vollständiges Beispiel  
 Beim folgenden Code handelt es sich um die Datei „MainWindow.xaml.cs“ aus der WPF-Anwendung (Windows Presentation Foundation), die in diesem Thema erläutert wird. Sie können das Beispiel unter [Async Sample: Example from "Asynchronous Programming with Async and Await"](http://go.microsoft.com/fwlink/p/?LinkID=261549) (Beispiel aus der asynchronen Programmierung mit „Async“ und „Await“) herunterladen.  
  
```csharp  
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.Threading.Tasks;  
using System.Windows;  
using System.Windows.Controls;  
using System.Windows.Data;  
using System.Windows.Documents;  
using System.Windows.Input;  
using System.Windows.Media;  
using System.Windows.Media.Imaging;  
using System.Windows.Navigation;  
using System.Windows.Shapes;  
  
// Add a using directive and a reference for System.Net.Http;  
using System.Net.Http;  
  
namespace AsyncFirstExample  
{  
    public partial class MainWindow : Window  
    {  
        // Mark the event handler with async so you can use await in it.  
        private async void StartButton_Click(object sender, RoutedEventArgs e)  
        {  
            // Call and await separately.  
            //Task<int> getLengthTask = AccessTheWebAsync();  
            //// You can do independent work here.  
            //int contentLength = await getLengthTask;  
  
            int contentLength = await AccessTheWebAsync();  
  
            resultsTextBox.Text +=  
                String.Format("\r\nLength of the downloaded string: {0}.\r\n", contentLength);  
        }  
  
        // Three things to note in the signature:  
        //  - The method has an async modifier.   
        //  - The return type is Task or Task<T>. (See "Return Types" section.)  
        //    Here, it is Task<int> because the return statement returns an integer.  
        //  - The method name ends in "Async."  
        async Task<int> AccessTheWebAsync()  
        {   
            // You need to add a reference to System.Net.Http to declare client.  
            HttpClient client = new HttpClient();  
  
            // GetStringAsync returns a Task<string>. That means that when you await the  
            // task you'll get a string (urlContents).  
            Task<string> getStringTask = client.GetStringAsync("http://msdn.microsoft.com");  
  
            // You can do work here that doesn't rely on the string from GetStringAsync.  
            DoIndependentWork();  
  
            // The await operator suspends AccessTheWebAsync.  
            //  - AccessTheWebAsync can't continue until getStringTask is complete.  
            //  - Meanwhile, control returns to the caller of AccessTheWebAsync.  
            //  - Control resumes here when getStringTask is complete.   
            //  - The await operator then retrieves the string result from getStringTask.  
            string urlContents = await getStringTask;  
  
            // The return statement specifies an integer result.  
            // Any methods that are awaiting AccessTheWebAsync retrieve the length value.  
            return urlContents.Length;  
        }  
  
        void DoIndependentWork()  
        {  
            resultsTextBox.Text += "Working . . . . . . .\r\n";  
        }  
    }  
}  
  
// Sample Output:  
  
// Working . . . . . . .  
  
// Length of the downloaded string: 41564.  
```  
  
## <a name="see-also"></a>Siehe auch  
 [async](../../../../csharp/language-reference/keywords/async.md)   
 [await](../../../../csharp/language-reference/keywords/await.md)
