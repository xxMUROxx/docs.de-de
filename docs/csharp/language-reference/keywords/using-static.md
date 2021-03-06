---
title: using static-Direktive (C#-Referenz) | Microsoft-Dokumentation
ms.date: 2017-03-10
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- using static directive [C#]
ms.assetid: 8b8f9e34-c75e-469b-ba85-6f2eb4090314
author: rpetrusha
ms.author: ronpet
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: Human Translation
ms.sourcegitcommit: f111ba3db26f247cbc56ee296b652bc04efb837d
ms.openlocfilehash: 66c68530f5684e4d75b9aaa21334e54dde5b64b6
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017

---
# <a name="using-static-directive-c-reference"></a>using static-Direktive (C#-Referenz)

Die `using static`-Direktive legt einen Typ fest, auf dessen statische Member Sie ohne Angabe eines Typnamens zugreifen können. Die Syntax lautet:

```csharp
using static <fully-qualified-type-name>
```

Wo *vollqualifizierter Typname* der Name des Typs ist, auf dessen statische Member verwiesen werden kann, ohne einen Typnamen anzugeben. Wenn Sie keinen vollqualifizierten Typnamen angeben (der vollständige Namespacename mit dem Typnamen), generiert C# den Compilerfehler CS0246: „The type or namespace name '<type-name>' could not be found“ („Der Typ- oder Namespacename <type-name> konnte nicht gefunden werden.“)

Die `using static`-Direktive gilt für jeden Typ, der über statische Member verfügt, auch wenn er ebenfalls über Instanzmember verfügt. Instanzmember können jedoch nur über die Typinstanz aufgerufen werden.

Die `using static`-Direktive wurde in C# 6 eingeführt.

## <a name="remarks"></a>Hinweise
 
Wenn Sie einen statischen Member aufrufen, geben Sie normalerweise den Typnamen zusammen mit dem Membernamen an. Das wiederholte Eingeben desselben Typnamens zum Aufrufen von Membern dieses Typs kann zu ausführlichem, verwirrendem Code führen. Die folgende Definition einer `Circle`-Klasse verweist z.B. auf mehrere Member der Klasse @System.Math.
  
[!code-cs[using-static#1](../../../../samples/snippets/csharp/language-reference/keywords/using/using-static1.cs#1)]

Da nicht mehr jedes Mal explizit auf die Klasse @System.Math verwiesen werden muss, wenn auf einen Member verwiesen wird, erzeugt die `using static`-Direktive deutlich übersichtlicheren Code:

[!code-cs[using-static#2](../../../../samples/snippets/csharp/language-reference/keywords/using/using-static2.cs#1)]

`using static` importiert nur zugängliche statische Member und geschachtelte Typen, die im angegebenen Typ deklariert sind.  Geerbte Member werden nicht importiert.  Sie können aus jedem benannten Typen mit einer statischen using-Anweisung importieren, einschließlich Visual Basic-Module.  Wenn F#-Funktionen der obersten Ebene in den Metadaten als statische Member eines benannten Typs angezeigt werden, dessen Name ein gültiger C#-Bezeichner ist, können die F#-Funktionen importiert werden.  
  
 `using static` macht Erweiterungsmethoden, die im angegebenen Typ deklariert sind, für die Erweiterungsmethodensuche verfügbar.  Die Namen der Erweiterungsmethoden werden jedoch bei nicht referenzierten Verweisen im Code nicht in den Bereich importiert.  
  
 Methoden mit dem gleichen Namen, die aus verschiedenen Typen von verschiedenen statischen `using static`-Direktiven in der gleichen Kompilierungseinheit oder dem gleichen Namespace importiert wurden, bilden eine Methodengruppe.  Die Überladungsauflösung innerhalb dieser Methodengruppen folgt den normalen C#-Regeln.  
  
## <a name="example"></a>Beispiel

Im folgenden Beispiele wird die `using static`-Direktive verwendet, um die statischen Member der Klassen @System.Console, @System.Math und @System.String zugänglich zu machen, ohne deren Typnamen angeben zu müssen.

[!code-cs[using-static#3](../../../../samples/snippets/csharp/language-reference/keywords/using/using-static3.cs)]

In diesem Beispiel hätte die `using static`-Direktive auch auf den Typ @System.Double angewendet werden können. Dadurch wäre es möglich geworden, die Methode @System.Double.TryParse(System.String,System.Double@) aufzurufen, ohne einen Typnamen anzugeben. Hierdurch wird allerdings weniger lesbarer Code generiert, da die `using static`-Anweisungen überprüft werden müssen, um zu bestimmen, welche `TryParse`-Methode eines numerischen Typs aufgerufen wird.

## <a name="see-also"></a>Siehe auch

[using directive (using-Direktive)](using-directive.md)   
[C#-Referenz](../../../csharp/language-reference/index.md)   
[C#-Schlüsselwörter](../../../csharp/language-reference/keywords/index.md)   
[Using-Namespaces](../../../csharp/programming-guide/namespaces/using-namespaces.md)   
[Namespace Keywords (Schlüsselwörter des Namespace)](../../../csharp/language-reference/keywords/namespace-keywords.md)   
[Namespaces](../../../csharp/programming-guide/namespaces/index.md)   

