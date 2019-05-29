# Homework_2

Sub Stock_data()
Dim WKS As Worksheet
   For Each WKS In ActiveWorkbook.Worksheets
   WKS.Activate
       lastrow = WKS.Cells(Rows.Count, 1).End(xlUp).Row
       Cells(1, "I").Value = "Ticker"
       Cells(1, "J").Value = "Yearly Change"
       Cells(1, "K").Value = "Percent Change"
       Cells(1, "L").Value = "Total Stock Volume"

       Dim Open_stock As Double
       Dim Close_stock As Double
       Dim Year_change As Double
       Dim Ticker_name As String
       Dim Percent_change As Double
       Dim Volume As Double
       Dim Row As Double
       Dim Column As Integer
       Dim i As Long

       Volume = 0
       Row = 2
       Column = 1
       Open_stock = Cells(2, Column + 2).Value
       For i = 2 To lastrow
           If Cells(i + 1, Column).Value <> Cells(i, Column).Value Then
               Ticker_name = Cells(i, Column).Value
               Cells(Row, Column + 8).Value = Ticker_name
               Close_stock = Cells(i, Column + 5).Value
               Year_change = Close_stock - Open_stock
               Cells(Row, Column + 9).Value = Year_change
                   If Open_stock = 0 And Close_stock = 0 Then
                       Percent_change = 0
                   ElseIf Open_stock = 0 And Close_stock <> 0 Then
                       Percent_change = 1
                   Else
                       Percent_change = Year_change / Open_stock
                       Cells(Row, Column + 10).Value = Percent_change
                       Cells(Row, Column + 10).NumberFormat = "0.00%"
                   End If
               Volume = Volume + Cells(i, Column + 6).Value
               Cells(Row, Column + 11).Value = Volume

               Row = Row + 1
               Open_stock = Cells(i + 1, Column + 2)
               Volume = 0

           Else
               Volume = Volume + Cells(i, Column + 6).Value
           End If
       Next i

YCLastRow = WKS.Cells(Rows.Count, Column + 8).End(xlUp).Row
       For j = 2 To YCLastRow
           If (Cells(j, Column + 9).Value > 0 Or Cells(j, Column + 9).Value = 0) Then
               Cells(j, Column + 9).Interior.ColorIndex = 10
           ElseIf Cells(j, Column + 9).Value < 0 Then
               Cells(j, Column + 9).Interior.ColorIndex = 3
           End If
       Next j

Cells(2, Column + 14).Value = "Greatest % Increase"
Cells(3, Column + 14).Value = "Greatest % Decrease"
Cells(4, Column + 14).Value = "Greatest Total Volume"
Cells(1, Column + 15).Value = "Ticker"
Cells(1, Column + 16).Value = "Value"

   For k = 2 To YCLastRow
       If Cells(k, Column + 10).Value = Application.WorksheetFunction.Max(WKS.Range("K2:K" & YCLastRow)) Then
               Cells(2, Column + 15).Value = Cells(k, Column + 8).Value
               Cells(2, Column + 16).Value = Cells(k, Column + 10).Value
               Cells(2, Column + 16).NumberFormat = "0.00%"
           ElseIf Cells(k, Column + 10).Value = Application.WorksheetFunction.Min(WKS.Range("K2:K" & YCLastRow)) Then
               Cells(3, Column + 15).Value = Cells(k, Column + 8).Value
               Cells(3, Column + 16).Value = Cells(k, Column + 10).Value
               Cells(3, Column + 16).NumberFormat = "0.00%"
           ElseIf Cells(k, Column + 11).Value = Application.WorksheetFunction.Max(WKS.Range("L2:L" & YCLastRow)) Then
               Cells(4, Column + 15).Value = Cells(k, Column + 8).Value
               Cells(4, Column + 16).Value = Cells(k, Column + 11).Value
           End If
       Next k
   Next WKS


End Sub
