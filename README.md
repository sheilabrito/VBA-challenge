# VBA-challenge
VBA hw challange

Sub Stockanalysis():

For Each ws In Worksheets

' Tittles
ws.Range("I1").Value = "Ticker"
ws.Range("J1").Value = "Yearly Change"
ws.Range("K1").Value = "Percent Change"
ws.Range("L1").Value = "Total Stock Volume"
ws.Range("A1:Q1").WrapText = True
ws.Range("A1:Q1").HorizontalAlignment = xlCenter
ws.Range("A1:Q1").VerticalAlignment = xlCenter
ws.Columns(12).AutoFit
ws.Columns(15).AutoFit
ws.Columns(17).AutoFit
ws.Range("P1").Value = "Ticker"
ws.Range("Q1").Value = "Value"
ws.Range("O2").Value = "Greatest % Increase"
ws.Range("O3").Value = "Greatest % Decrease"
ws.Range("O4").Value = "Greatest Total Volume"

' Lastrow in Worksheet
Dim lastrow As Long
lastrow = ws.cells(rows.Count, 1).End(xlUp).row

' Variables
Dim bopenp As Double
Dim eopenp As Double
Dim volume As LongLong
Dim fila As Integer
Dim filad As Integer
Dim filat As Integer
Dim ychange As Double
Dim pchange As Double

fila = 1
filad = 1
filat = 1
volume = 0

' ForLoop
Dim i As Long
For i = 2 To lastrow

If (ws.cells(i + 1, 1).Value = ws.cells(i, 1).Value And ws.cells(i - 1, 1).Value <> ws.cells(i, 1).Value) Then
' Beggining Opening Price
bopenp = ws.cells(i, 3).Value
ws.cells(1 + fila, 9).Value = ws.cells(i, 1).Value
' Add Volume
volume = volume + ws.cells(i, 7).Value
fila = fila + 1

ElseIf (ws.cells(i + 1, 1).Value <> ws.cells(i, 1).Value And ws.cells(i - 1, 1).Value = ws.cells(i, 1).Value) Then
' Ending Opening Price
eopenp = ws.cells(i, 3).Value
' Yearly Change
ychange = eopenp - bopenp
ws.cells(1 + filat, 10).Value = ychange
' Percentage Change
pchange = ychange / bopenp
ws.cells(1 + filat, 11).Value = pchange
filat = filat + 1
End If

If ws.cells(i + 1, 1).Value = ws.cells(i, 1).Value Then
' Add more Volume
volume = volume + ws.cells(i, 7).Value
Else: ws.cells(1 + filad, 12).Value = volume
filad = filad + 1
' Reset Volume
volume = 0
End If

Next i

' Coloring Yearly Change
Dim lastrowd As Integer
lastrowd = ws.cells(rows.Count, 10).End(xlUp).row
Dim j As Integer
For j = 2 To lastrowd
If cells(j, 10).Value > 0 Then
ws.cells(j, 10).Interior.ColorIndex = 4
Else
ws.cells(j, 10).Interior.ColorIndex = 3
End If

Next j
ws.Range("K2:K" & lastrowd).NumberFormat = "0.00%"
ws.Range("J2:J" & lastrowd).NumberFormat = "$0.00"

'Max increase, decrease, volume and ticker

ws.Range("Q2") = "%" & WorksheetFunction.Max(ws.Range("K2:K" & lastrowd)) * 100
ws.Range("Q3") = "%" & WorksheetFunction.Min(ws.Range("K2:K" & lastrowd)) * 100
ws.Range("Q4") = WorksheetFunction.Max(ws.Range("L2:L" & lastrowd))

increase_number = WorksheetFunction.Match(WorksheetFunction.Max(ws.Range("K2:K" & lastrowd)), ws.Range("K2:K" & lastrowd), 0)
decrease_number = WorksheetFunction.Match(WorksheetFunction.Min(ws.Range("K2:K" & lastrowd)), ws.Range("K2:K" & lastrowd), 0)
volume_number = WorksheetFunction.Match(WorksheetFunction.Max(ws.Range("L2:L" & lastrowd)), ws.Range("L2:L" & lastrowd), 0)

ws.Range("P2") = ws.cells(increase_number + 1, 9)
ws.Range("P3") = ws.cells(decrease_number + 1, 9)
ws.Range("P4") = ws.cells(volume_number + 1, 9)

Next ws

End Sub
