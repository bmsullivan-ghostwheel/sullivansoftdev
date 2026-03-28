---
title: "MultiComparer"
date: 2008-02-22T00:00:00-06:00
slug: "multicomparer"
url: "/blog/2008/02/22/multicomparer/"
---

My friend and co-worker Kaelin was working on an application that was using a grid to display some custom objects, but wanted to be able to sort by multiple fields like you would do with an SQL query. He came up with something that I think is pretty cool, and very reusable:

```vb
Public Class MultiComparer(Of T)

   Implements IComparer(Of T)

   Private _comparers As List(Of IComparer(Of T))

   Public Sub New(ByVal comparers As List(Of IComparer(Of T)))

      Me._comparers = comparers

   End Sub

   Public Function Compare(ByVal x As T, ByVal y As T) As Integer Implements System.Collections.Generic.IComparer(Of T).Compare

      For Each comparer As IComparer(Of T) In Me._comparers

         Dim i As Integer = comparer.Compare(x, y)

         If i <> 0 Then

            Return i

         End If

      Next

      Return 0

   End Function

End Class
```

Let that sink in for a minute…

So, if you wanted to sort, say, a list of Person objects by last name, then first name, then birthday, you'd create a new `MultiComparer` passing in a list consisting of a `LastNameComparer`, a `FirstNameComparer`, and a `BirthdayComparer` (in that order). Mad props to Kaelin for his understanding of generics!
