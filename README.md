Option Explicit

Sub CrearContentControlsVWFS()

    Application.ScreenUpdating = False

    ' ==========================================================
    ' PORTADA
    ' ==========================================================
    AddCCByText "[NOMBRE DE LA INICIATIVA]", "NombreIniciativa", False
    AddCCByText "[Área identificada durante el discovery]", "AreaSolicitante", False
    AddCCByText "[Nombre y datos de contacto disponibles]", "SolicitanteContacto", False
    AddCCByText "[Sponsor propuesto por el usuario o rol propuesto por el agente]", "Sponsor", True
    AddCCByText "[Líder de Negocio / coordinador propuesto]", "LiderNegocio", True
    AddCCByText "[Fecha de la conversación de discovery]", "FechaDiscovery", False

    ' ==========================================================
    ' 1. RESUMEN EJECUTIVO
    ' ==========================================================
    AddCCByText _
        "[En 5 a 8 líneas: problema, objetivo, solución futura esperada, impacto y carril recomendado.]", _
        "ResumenEjecutivo", True

    ' ==========================================================
    ' 2. OBJETIVO
    ' ==========================================================
    AddCCByText _
        "[Redacción ejecutiva del “para qué” y del resultado de negocio esperado.]", _
        "ObjetivoIniciativa", True

    ' ==========================================================
    ' 3. PROBLEMA ACTUAL
    ' ==========================================================
    AddCCByText "[Qué está ocurriendo y dónde se presenta.]", _
                "SituacionActual", True

    AddCCByText "[Errores, tiempos, retrabajo, falta de visibilidad, riesgo u otra afectación.]", _
                "DolorIneficiencia", True

    AddCCByText "[Impacto operativo o de negocio conocido. No inventar cifras.]", _
                "ConsecuenciasActuales", True

    AddCCByText "[Qué puede continuar o agravarse si la iniciativa no avanza.]", _
                "RiesgoNoActuar", True

    ' ==========================================================
    ' 4. AS-IS
    ' ==========================================================
    AddCCByText _
        "[Pasos principales en orden, indicando intervenciones manuales y puntos de dolor.]", _
        "AsIsFlujo", True

    AddCCByText "[Roles, usuarios, áreas o terceros que participan.]", _
                "AsIsActores", True

    AddCCByText "[Información, archivos, solicitudes o eventos que activan el proceso.]", _
                "AsIsEntradas", True

    AddCCByText "[Resultado, documento, decisión, registro o comunicación generada.]", _
                "AsIsSalidas", True

    AddCCByText "[Solo si fue identificado. Vacío si no.]", _
                "AsIsFrecuenciaVolumen", False

    ' El mismo placeholder aparece otra vez para duración.
    ' Se procesa específicamente después:
    AddSecondOccurrenceCC _
        "[Solo si fue identificada. Vacío si no.]", _
        "AsIsDuracion", False

    ' ==========================================================
    ' 5. TO-BE
    ' ==========================================================
    AddCCByText _
        "[Flujo futuro esperado y diferencia principal frente al AS-IS, sin convertirlo en requerimientos funcionales.]", _
        "ToBeVision", True

    AddCCByText _
        "[Actividades que se eliminan, simplifican, automatizan o incorporan.]", _
        "ToBeCambios", True

    AddCCByText _
        "[Decisiones, validaciones o supervisión que permanecen en personas.]", _
        "ToBeParticipacionHumana", True

    AddCCByText _
        "[Qué debería recibir, consultar, decidir o ejecutar el usuario.]", _
        "ToBeResultado", True

    AddCCByText "[Solo excepciones conocidas.]", _
                "ToBeExcepciones", True

    ' ==========================================================
    ' 6. ALCANCE
    ' Las compañías se crean en la celda derecha de cada fila.
    ' ==========================================================
    AddCCToRightCellOfLabel "VW Servicios", "CompaniaVWServicios"
    AddCCToRightCellOfLabel "VW Leasing", "CompaniaVWLeasing"
    AddCCToRightCellOfLabel "VW Bank", "CompaniaVWBank"
    AddCCToRightCellOfLabel "VW IB", "CompaniaVWIB"

    AddCCByText _
        "[Credit / Credit Premium / Leasing / Otro / No aplica]", _
        "Productos", False

    AddCCByText _
        "[PFA / PFP / PFAE / PM / Otro / No aplica]", _
        "TipoCliente", False

    AddCCByText _
        "[Volkswagen / SEAT / Audi / Porsche / VW Vehículos Comerciales / VW Camiones / Otra / No aplica]", _
        "Marcas", True

    ' ==========================================================
    ' 7. IMPACTO
    ' ==========================================================
    AddCCByText _
        "[Calidad, experiencia, control, trazabilidad, velocidad, reducción de riesgo u otros.]", _
        "BeneficiosCualitativos", True

    AddCCByText _
        "[Datos disponibles de tiempo, volumen, coste, capacidad o errores. Vacío si no existen.]", _
        "BeneficiosCuantitativos", True

    AddCCByText _
        "[Perfiles o grupos que recibirían el beneficio.]", _
        "UsuariosBeneficiados", True

    ' KPI repetible
    CrearTablaKPI

    ' ==========================================================
    ' 8. USUARIOS Y ÁREAS
    ' ==========================================================
    AddCCByText _
        "[Perfiles que ejecutan o reciben el proceso.]", _
        "UsuariosPrincipales", True

    AddCCByText _
        "[Área responsable de la necesidad o proceso.]", _
        "AreaPropietaria", False

    AddCCByText _
        "[Áreas que participan, proveen información, validan o reciben resultados.]", _
        "AreasInvolucradas", True

    AddCCByText _
        "[Solo si fueron identificados.]", _
        "TercerosProveedores", True

    ' ==========================================================
    ' 9. SISTEMAS, DATOS Y DEPENDENCIAS
    ' ==========================================================
    AddCCByText _
        "[Aplicaciones, plataformas, hojas de cálculo, canales o herramientas mencionadas.]", _
        "SistemasHerramientas", True

    AddCCByText _
        "[Tipos de información, origen conocido y sensibilidad señalada.]", _
        "DatosInvolucrados", True

    AddCCByText _
        "[Conectores, APIs, MCP, intercambio de archivos u otras mencionadas. No asumir detalles.]", _
        "IntegracionesConocidas", True

    AddCCByText _
        "[Accesos, licencias, equipos, propietarios de datos, proveedores o decisiones externas.]", _
        "Dependencias", True

    AddCCByText _
        "[Limitaciones técnicas, normativas, de seguridad, capacidad o adopción.]", _
        "RestriccionesRiesgos", True

    ' ==========================================================
    ' 10. EVALUACIÓN
    ' ==========================================================
    AddCCByText _
        "[Alto / Medio / Bajo / Requiere análisis] — [justificación breve]", _
        "PotencialAutomatizacion", True

    AddCCByText _
        "[Sí / No / Requiere análisis] — [qué capacidad de IA aportaría valor]", _
        "PosibleUsoIA", True

    AddCCByText _
        "[Sí / No / Requiere análisis] — [qué funciones podría coordinar o ejecutar]", _
        "PosibleUsoAgente", True

    AddCCByText _
        "[Sí / No / Requiere análisis] — [configuración, reglas, workflow o mejora de proceso]", _
        "AlternativaSinIA", True

    AddCCByText _
        "[Baja / Media / Alta] — [basada en dependencias visibles]", _
        "ComplejidadPercibida", True

    ' Marcas de Carril
    AddCCToLeftCellOfCarril "Carril 1", "Carril1Marca"
    AddCCToLeftCellOfCarril "Carril 2", "Carril2Marca"
    AddCCToLeftCellOfCarril "Carril 3", "Carril3Marca"

    AddCCByText _
        "[Motivo, dependencia principal y condiciones que podrían modificar el carril.]", _
        "JustificacionCarril", True

    ' ==========================================================
    ' 11. PENDIENTES
    ' ==========================================================
    CrearTablaPendientes

    AddCCByText _
        "[Acción congruente con el carril: self-service, sesión con Champion o evaluación con IT.]", _
        "SiguientePasoRecomendado", True

    AddCCByText _
        "[Datos relevantes que no se pudieron confirmar. No completar con suposiciones.]", _
        "InformacionNoDisponible", True

    AddCCByText _
        "[Notas ejecutivas indispensables para el equipo evaluador.]", _
        "ObservacionesFinales", True

    Application.ScreenUpdating = True

    MsgBox "Content Controls VWFS creados.", vbInformation

End Sub


' ==========================================================
' CREA CONTROL SOBRE UN TEXTO/PLACEHOLDER
' ==========================================================
Sub AddCCByText(ByVal texto As String, _
                ByVal nombre As String, _
                ByVal multiLinea As Boolean)

    Dim rng As Range
    Dim cc As ContentControl

    If ContentControlExists(nombre) Then Exit Sub

    Set rng = ActiveDocument.Content.Duplicate

    With rng.Find
        .ClearFormatting
        .Text = texto
        .Forward = True
        .Wrap = wdFindStop
        .MatchCase = False
        .MatchWholeWord = False
    End With

    If rng.Find.Execute Then

        Set cc = ActiveDocument.ContentControls.Add( _
                    wdContentControlText, rng)

        cc.Title = nombre
        cc.Tag = nombre

        On Error Resume Next
        cc.MultiLine = multiLinea
        On Error GoTo 0

    Else
        Debug.Print "No encontrado: " & texto & _
                    " -> " & nombre
    End If

End Sub


' ==========================================================
' CONTROL PARA SEGUNDA OCURRENCIA
' ==========================================================
Sub AddSecondOccurrenceCC(ByVal texto As String, _
                          ByVal nombre As String, _
                          ByVal multiLinea As Boolean)

    Dim rng As Range
    Dim contador As Integer
    Dim cc As ContentControl

    If ContentControlExists(nombre) Then Exit Sub

    Set rng = ActiveDocument.Content.Duplicate
    contador = 0

    With rng.Find
        .ClearFormatting
        .Text = texto
        .Forward = True
        .Wrap = wdFindStop

        Do While .Execute

            contador = contador + 1

            If contador = 1 Then

                Set cc = ActiveDocument.ContentControls.Add( _
                            wdContentControlText, rng)

                cc.Title = nombre
                cc.Tag = nombre

                On Error Resume Next
                cc.MultiLine = multiLinea
                On Error GoTo 0

                Exit Do

            End If

            rng.Collapse wdCollapseEnd

        Loop

    End With

End Sub


' ==========================================================
' CREA CONTROL EN CELDA DERECHA SEGÚN ETIQUETA IZQUIERDA
' ==========================================================
Sub AddCCToRightCellOfLabel(ByVal etiqueta As String, _
                            ByVal nombre As String)

    Dim tbl As Table
    Dim fila As Row
    Dim txt As String
    Dim rng As Range
    Dim cc As ContentControl

    If ContentControlExists(nombre) Then Exit Sub

    For Each tbl In ActiveDocument.Tables

        For Each fila In tbl.Rows

            If fila.Cells.Count >= 2 Then

                txt = CleanCellText(fila.Cells(1).Range.Text)

                If InStr(1, txt, etiqueta, vbTextCompare) > 0 Then

                    Set rng = fila.Cells(2).Range.Duplicate
                    rng.End = rng.End - 1

                    rng.Text = ""

                    Set cc = ActiveDocument.ContentControls.Add( _
                                wdContentControlText, rng)

                    cc.Title = nombre
                    cc.Tag = nombre

                    cc.SetPlaceholderText , , "[Sí/No]"

                    Exit Sub

                End If

            End If

        Next fila

    Next tbl

End Sub


' ==========================================================
' CARRILES: PONE CONTROL EN PRIMERA CELDA DE LA FILA
' ==========================================================
Sub AddCCToLeftCellOfCarril(ByVal textoCarril As String, _
                            ByVal nombre As String)

    Dim tbl As Table
    Dim fila As Row
    Dim txt As String
    Dim rng As Range
    Dim cc As ContentControl

    If ContentControlExists(nombre) Then Exit Sub

    For Each tbl In ActiveDocument.Tables

        For Each fila In tbl.Rows

            txt = CleanCellText(fila.Range.Text)

            If InStr(1, txt, textoCarril, vbTextCompare) > 0 Then

                If fila.Cells.Count >= 2 Then

                    Set rng = fila.Cells(1).Range.Duplicate
                    rng.End = rng.End - 1
                    rng.Text = ""

                    Set cc = ActiveDocument.ContentControls.Add( _
                                wdContentControlText, rng)

                    cc.Title = nombre
                    cc.Tag = nombre
                    cc.SetPlaceholderText , , "[ ]"

                    Exit Sub

                End If

            End If

        Next fila

    Next tbl

End Sub


' ==========================================================
' TABLA KPI REPETIBLE
' ==========================================================
Sub CrearTablaKPI()

    Dim tbl As Table
    Dim fila As Row
    Dim rngRow As Range
    Dim rep As ContentControl

    If ContentControlExists("KPIRows") Then Exit Sub

    For Each tbl In ActiveDocument.Tables

        If tbl.Rows.Count >= 2 Then

            If InStr(1, CleanCellText(tbl.Rows(1).Range.Text), _
                     "KPI", vbTextCompare) > 0 _
            And InStr(1, CleanCellText(tbl.Rows(1).Range.Text), _
                     "Justificación", vbTextCompare) > 0 Then

                Set fila = tbl.Rows(2)

                Set rngRow = fila.Range.Duplicate

                Set rep = ActiveDocument.ContentControls.Add( _
                            wdContentControlRepeatingSection, rngRow)

                rep.Title = "KPIRows"
                rep.Tag = "KPIRows"

                AddCCToCell fila.Cells(1), "KPI"
                AddCCToCell fila.Cells(2), "KPIImpacto"
                AddCCToCell fila.Cells(3), "KPIJustificacion"
                AddCCToCell fila.Cells(4), "KPIArea"

                Exit Sub

            End If

        End If

    Next tbl

End Sub


' ==========================================================
' TABLA PENDIENTES REPETIBLE
' ==========================================================
Sub CrearTablaPendientes()

    Dim tbl As Table
    Dim fila As Row
    Dim rngRow As Range
    Dim rep As ContentControl

    If ContentControlExists("PendientesRows") Then Exit Sub

    For Each tbl In ActiveDocument.Tables

        If tbl.Rows.Count >= 2 Then

            If InStr(1, CleanCellText(tbl.Rows(1).Range.Text), _
                     "Pendiente", vbTextCompare) > 0 _
            And InStr(1, CleanCellText(tbl.Rows(1).Range.Text), _
                     "Responsable", vbTextCompare) > 0 Then

                Set fila = tbl.Rows(2)

                Set rngRow = fila.Range.Duplicate

                Set rep = ActiveDocument.ContentControls.Add( _
                            wdContentControlRepeatingSection, rngRow)

                rep.Title = "PendientesRows"
                rep.Tag = "PendientesRows"

                AddCCToCell fila.Cells(1), "PendienteNumero"
                AddCCToCell fila.Cells(2), "PendienteDescripcion"
                AddCCToCell fila.Cells(3), "PendienteResponsable"
                AddCCToCell fila.Cells(4), "PendienteMomento"

                Exit Sub

            End If

        End If

    Next tbl

End Sub


' ==========================================================
' CREA CONTROL EN CELDA
' ==========================================================
Sub AddCCToCell(ByVal celda As Cell, _
                ByVal nombre As String)

    Dim rng As Range
    Dim cc As ContentControl

    If ContentControlExists(nombre) Then Exit Sub

    Set rng = celda.Range.Duplicate

    ' Excluir marcador final de celda
    rng.End = rng.End - 1

    rng.Text = ""

    Set cc = ActiveDocument.ContentControls.Add( _
                wdContentControlText, rng)

    cc.Title = nombre
    cc.Tag = nombre

    On Error Resume Next
    cc.MultiLine = True
    On Error GoTo 0

End Sub


' ==========================================================
' VERIFICA SI YA EXISTE
' ==========================================================
Function ContentControlExists(ByVal nombre As String) As Boolean

    Dim cc As ContentControl

    For Each cc In ActiveDocument.ContentControls

        If StrComp(cc.Title, nombre, vbTextCompare) = 0 Then
            ContentControlExists = True
            Exit Function
        End If

    Next cc

    ContentControlExists = False

End Function


' ==========================================================
' LIMPIA CARACTERES DE FIN DE CELDA
' ==========================================================
Function CleanCellText(ByVal txt As String) As String

    txt = Replace(txt, Chr(13), "")
    txt = Replace(txt, Chr(7), "")
    CleanCellText = Trim(txt)

End Function