Sub keepSelectedColumns()
    Dim ws As Worksheet
    Dim columnsToKeep As Variant
    Dim lastCol As Long, i As Long
    Dim headerName As String

    Set ws = ActiveSheet

    ' Edit this list with the exact header names they want to keep
    columnsToKeep = Array("Contrato", "Cliente", "Fecha Inicio", "Oferta", "Monto")

    lastCol = ws.Cells(1, ws.Columns.Count).End(xlToLeft).Column

    ' Iterate backwards so deleting doesn't shift the columns we haven't checked yet
    For i = lastCol To 1 Step -1
        headerName = Trim(ws.Cells(1, i).Value)
        If IsError(Application.Match(headerName, columnsToKeep, 0)) Then
            ws.Columns(i).Delete
        End If
    Next i
End Sub
