# Agent Guide — VBA (Excel Automation)

> Automate Excel and Office with macros. Reference objects directly, declare your variables, and keep loops fast.
> Category: AI & Data · Source: https://aliarsalan177.github.io/guides/vba/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with VBA (Excel Automation). Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Declare variables — use Option Explicit
- ✅ **APPLY:** Put Option Explicit at the top and Dim every variable with a type; use Set for object variables.
- ⛔ **AVOID:** Implicit undeclared Variants and typos that silently create new variables and hide bugs.

### 2. Never .Select / .Activate
- ✅ **APPLY:** Reference objects directly (ws.Range("A1").Value = 1) and use the With construct to avoid repetition.
- ⛔ **AVOID:** Range().Select then Selection.… — it's slow, fragile, and the #1 macro-recorder anti-pattern.

### 3. Speed up long loops
- ✅ **APPLY:** Set Application.ScreenUpdating = False and Calculation = xlManual around bulk work; read/write arrays in one shot.
- ⛔ **AVOID:** Touching cells one-by-one with screen updating on — it can be 100× slower.

### 4. Handle errors and always clean up
- ✅ **APPLY:** Use On Error GoTo handler, and re-enable ScreenUpdating/Calculation in the handler/exit path.
- ⛔ **AVOID:** On Error Resume Next left on globally — it swallows every error and hides failures.

## Cheat Reference — concepts to remember

- **Declare variables — use Option Explicit** — Put Option Explicit at the top and Dim every variable with a type; use Set for object variables.
- **Never .Select / .Activate** — Reference objects directly (ws.Range("A1").Value = 1) and use the With construct to avoid repetition.
- **Speed up long loops** — Set Application.ScreenUpdating = False and Calculation = xlManual around bulk work; read/write arrays in one shot.
- **Handle errors and always clean up** — Use On Error GoTo handler, and re-enable ScreenUpdating/Calculation in the handler/exit path.

## Full Cheat Sheet — every concept

### Basics
- Option Explicit (force declarations); Dim x As String/Long/Double/Boolean/Variant.
- Set obj = Range("B2") for objects; MsgBox("..."); Call myMacro; Debug.Print.
- Application.WorksheetFunction.CountA("A:A"); operators: / (float), \ (integer div), Mod.

### Control Flow
- If … Then … ElseIf … Else … End If; Select Case.
- Loops: For i = 1 To n … Next; For Each cell In rng … Next; Do While/Until … Loop.

### Objects
- Application → Workbooks → Worksheets → Range/Cells (the object model).
- Range("A1").Value / .Formula / .Font.Bold; Cells(r, c) for index access.
- ws.Range, ThisWorkbook, ActiveSheet; With obj … End With.

### Data & Performance
- Arrays (Dim a(1 To 10)), dynamic ReDim; Collections/Dictionary for keyed data.
- Read a Range into a Variant array, process, write back once — fastest pattern.
- Application.ScreenUpdating/Calculation = False/xlManual around bulk work.

### Errors & Events
- On Error GoTo handler … Exit Sub … handler: … Resume; re-enable settings on exit.
- Event macros: Worksheet_Change, Workbook_Open; store code in Modules.

## Interview Questions

#### Q1. Sub vs Function in VBA?
A Sub performs actions and returns nothing (run as a macro); a Function returns a value and can be used in worksheet formulas or other code. Use Functions for calculations, Subs for procedures.

#### Q2. Why avoid .Select and .Activate?
They're slow, depend on UI state, and break easily. Reference cells/sheets directly (Worksheets("Sheet1").Range("A1")) for faster, more reliable code.

#### Q3. Range vs Cells?
Range("A1") / Range("A1:B2") addresses by reference; Cells(row, col) addresses by numeric index — handy in loops (Cells(i, 1)). They combine: Range(Cells(1,1), Cells(3,2)).

#### Q4. What is the With construct?
A block (With obj … End With) that runs multiple statements against one object without repeating it (.Value, .Font.Bold), improving readability and slightly performance.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
