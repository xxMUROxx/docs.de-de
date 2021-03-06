---
title: Feinabstimmung der Async-Anwendung (C#) | Microsoft-Dokumentation
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
ms.assetid: 97696eb9-81fc-4940-9655-84daa8eb4d5c
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 74cab732debe2381cbd3b9106b2431e2490ef57e
ms.lasthandoff: 03/13/2017

---
# <a name="fine-tuning-your-async-application-c"></a>Feinabstimmung der Async-Anwendung (C#)
Sie können Genauigkeit und Flexibilität zu Ihren asynchronen Anwendungen hinzufügen, indem Sie die Methoden und Eigenschaften verwenden, die der Typ <xref:System.Threading.Tasks.Task> bereitstellt. Die Themen in diesem Abschnitt zeigen Beispiele, in denen <xref:System.Threading.CancellationToken> und wichtige `Task`-Methoden verwendet werden, wie z.B. <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=fullName> und <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=fullName>.  
  
 Mit `WhenAny` und `WhenAll` können Sie mehrere Aufgaben leichter starten und ihren Abschluss abwarten, indem Sie eine einzelne Aufgabe überwachen.  
  
-   `WhenAny` gibt eine Aufgabe zurück, die abgeschlossen wird, wenn jede Aufgabe in einer Auflistung abgeschlossen ist.  
  
     Beispiele für die Verwendung von `WhenAny` finden Sie unter [Cancel Remaining Async Tasks after One Is Complete (C#) (Verbleibende asynchrone Aufgaben nach Abschluss einer Aufgabe abbrechen (C#))](../../../../csharp/programming-guide/concepts/async/cancel-remaining-async-tasks-after-one-is-complete.md) und [Start Multiple Async Tasks and Process Them As They Complete (C#) (Mehrere asynchrone Aufgaben starten und nach Abschluss verarbeiten (C#))](../../../../csharp/programming-guide/concepts/async/start-multiple-async-tasks-and-process-them-as-they-complete.md).  
  
-   `WhenAll` gibt eine Aufgabe zurück, die abgeschlossen wird, wenn alle Aufgaben in einer Auflistung abgeschlossen sind.  
  
     Weitere Informationen und ein Beispiel, `WhenAll`, finden Sie unter [How to: Extend the async Walkthrough by Using Task.WhenAll (C#) (Vorgehensweise: Erweitern der asynchronen exemplarischen Vorgehensweise mit Task.WhenAll (C#))](../../../../csharp/programming-guide/concepts/async/how-to-extend-the-async-walkthrough-by-using-task-whenall.md).  
  
 Dieser Abschnitt enthält die folgenden Beispiele:  
  
-   [Cancel an Async Task or a List of Tasks (C#) (Eine asynchrone Aufgabe oder Aufgabenliste abbrechen (C#))](../../../../csharp/programming-guide/concepts/async/cancel-an-async-task-or-a-list-of-tasks.md)  
  
-   [Cancel Async Tasks after a Period of Time (C#) (Asynchrone Aufgaben nach einer Zeitperiode abbrechen (C#))](../../../../csharp/programming-guide/concepts/async/cancel-async-tasks-after-a-period-of-time.md)  
  
-   [Cancel Remaining Async Tasks after One Is Complete (C#) (Verbleibende asynchrone Aufgaben nach Abschluss einer Aufgabe abbrechen (C#))](../../../../csharp/programming-guide/concepts/async/cancel-remaining-async-tasks-after-one-is-complete.md)  
  
-   [Start Multiple Async Tasks and Process Them As They Complete (C#) (Mehrere asynchrone Aufgaben starten und nach Abschluss verarbeiten (C#))](../../../../csharp/programming-guide/concepts/async/start-multiple-async-tasks-and-process-them-as-they-complete.md)  
  
> [!NOTE]
>  Zum Ausführen der Beispiele müssen Visual Studio 2012 oder höher sowie .NET Framework 4.5 oder höher auf dem Computer installiert sein.  
  
 Die Projekte erstellen eine Benutzeroberfläche mit einer Schaltfläche zum Starten und einer Schaltfläche zum Abbrechen des Prozesses, wie in der folgenden Abbildung ersichtlich. Die Schaltflächen werden mit `startButton` und `cancelButton` bezeichnet.  
  
 ![WPF-Fenster mit Schaltfläche „Abbrechen“](../../../../csharp/programming-guide/concepts/async/media/cancellation.png "Abbruch")  
  
 Sie können alle Windows Presentation Foundation (WPF)-Projekte von [Async Sample: Fine Tuning Your Application (Async-Beispiel: Feinabstimmung der Anwendung)](http://go.microsoft.com/fwlink/?LinkId=255046) herunterladen.  
  
## <a name="see-also"></a>Siehe auch  
 [Asynchronous Programming with async and await (C#) (Asynchrone Programmierung mit Async und Await (C#))](../../../../csharp/programming-guide/concepts/async/index.md)
