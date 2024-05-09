# Excel

### **Excel VBA (Macro) Examples:**

**1. Write "Hello, Excel" in Cell A1:**

```
Sub WriteHello()
    Range("A1").Value = "Hello, Excel"
End Sub

```

**2. Create a New Worksheet:**

```
Sub CreateNewWorksheet()
    Sheets.Add
End Sub

```

**3. Format Cell A1 as Bold and Red:**

```
Sub FormatCell()
    Range("A1").Font.Bold = True
    Range("A1").Font.Color = RGB(255, 0, 0) ' Red
End Sub

```

**4. Loop through a Range and Calculate the Sum:**

```
Sub CalculateSum()
    Dim total As Double
    total = 0

    For Each cell In Range("A1:A10")
        total = total + cell.Value
    Next cell

    Range("B1").Value = total
End Sub

```

**5. Display a Message Box:**

```
Sub ShowMessageBox()
    MsgBox "This is a message box", vbInformation, "Information"
End Sub

```

### **Excel Functions Examples:**

**1. SUM Function:**

Calculates the sum of a range of cells.

```
=SUM(A1:A10)

```

**2. IF Function:**

Returns different values based on a condition.

```
=IF(A1 > 10, "Yes", "No")

```

**3. VLOOKUP Function:**

Searches for a value in a table and returns a corresponding value from another column.

```
=VLOOKUP(A1, TableRange, 2, FALSE)

```

**4. CONCATENATE Function:**

Joins multiple text strings into one.

```
=CONCATENATE("First", "Second", "Third")

```

**5. AVERAGE Function:**

Calculates the average of a range of numbers.

```
=AVERAGE(A1:A10)

```

**6. COUNTIF Function:**

Counts the number of cells that meet a specified condition.

```
=COUNTIF(A1:A10, ">5")

```

**7. TODAY Function:**

Returns the current date.

```
=TODAY()

```

**8. INDEX and MATCH Functions:**

Look up a value in a table and return a corresponding value from another column based on a matching condition.

```
=INDEX(ColumnToReturn, MATCH(ValueToMatch, LookupColumn, 0))

```

These are just a few examples of Excel VBA macros and functions. You can use these as starting points to automate tasks and perform calculations in Excel. Remember to use the VBA Editor to create and run macros, and you can use functions directly in Excel cells.

---

### **1. SUM Function:**

Calculates the sum of a range of cells.

**Excel Formula:**

```
=SUM(A1:A10)

```

### **2. AVERAGE Function:**

Calculates the average of a range of numbers.

**Excel Formula:**

```
=AVERAGE(A1:A10)

```

### **3. MAX Function:**

Returns the maximum value from a range of numbers.

**Excel Formula:**

```
=MAX(A1:A10)

```

### **4. MIN Function:**

Returns the minimum value from a range of numbers.

**Excel Formula:**

```
=MIN(A1:A10)

```

### **5. IF Function:**

Returns different values based on a condition.

**Excel Formula:**

```
=IF(A1 > 10, "Yes", "No")

```

### **6. VLOOKUP Function:**

Searches for a value in a table and returns a corresponding value from another column.

**Excel Formula:**

```
=VLOOKUP(A1, TableRange, 2, FALSE)

```

### **7. HLOOKUP Function:**

Similar to VLOOKUP but searches horizontally.

**Excel Formula:**

```
=HLOOKUP(A1, TableRange, 2, FALSE)

```

### **8. CONCATENATE Function:**

Joins multiple text strings into one.

**Excel Formula:**

```
=CONCATENATE("First", "Second", "Third")

```

### **9. TEXT Function:**

Formats a value as text using a specified format.

**Excel Formula:**

```
=TEXT(A1, "dd/mm/yyyy")

```

### **10. LEFT Function:**

Returns a specified number of characters from the beginning of a text string.

**Excel Formula:**

```
=LEFT(A1, 5)

```

### **11. RIGHT Function:**

Returns a specified number of characters from the end of a text string.

**Excel Formula:**

```
=RIGHT(A1, 3)

```

### **12. MID Function:**

Returns a specific number of characters from the middle of a text string.

**Excel Formula:**

```
=MID(A1, 3, 2)

```

### **13. COUNTIF Function:**

Counts the number of cells that meet a specified condition.

**Excel Formula:**

```
=COUNTIF(A1:A10, ">5")

```

### **14. SUMIF Function:**

Calculates the sum of cells that meet a specified condition.

**Excel Formula:**

```
=SUMIF(A1:A10, ">5")

```

### **15. AVERAGEIF Function:**

Calculates the average of cells that meet a specified condition.

**Excel Formula:**

```
=AVERAGEIF(A1:A10, ">5")

```

### **16. COUNTIFS Function:**

Counts the number of cells that meet multiple specified conditions.

**Excel Formula:**

```
=COUNTIFS(A1:A10, ">5", B1:B10, "<10")

```

### **17. SUMIFS Function:**

Calculates the sum of cells that meet multiple specified conditions.

**Excel Formula:**

```
=SUMIFS(A1:A10, ">5", B1:B10, "<10")

```

### **18. INDEX and MATCH Functions:**

Look up a value in a table and return a corresponding value from another column based on a matching condition.

**Excel Formula:**

```
=INDEX(ColumnToReturn, MATCH(ValueToMatch, LookupColumn, 0))

```

### **19. DATE Function:**

Creates a date from year, month, and day values.

**Excel Formula:**

```
=DATE(2023, 10, 15)

```

### **20. TODAY Function:**

Returns the current date.

**Excel Formula:**

```
=TODAY()

```

These are some of the most commonly used Excel functions, each with its code sample. You can use these functions to perform a wide range of calculations and data manipulations in Excel.



以下の手順で、縦と横にドラッグした際に適切に変数が反映されるような関数を作成できます。

1. 最初に、列のインデックスを計算します。例えば、G列から始まるセルの場合、D列からのオフセットは `(COLUMN()-COLUMN($G$8))*138` となります。138は1つのセットごとにD列からのオフセットがどれだけずれるかを示します。

2. `INDEX`関数と`SEQUENCE`関数を使用して、指定されたセル範囲を参照します。

以下は、G8からのセルを参照する関数の例です。この関数をG8セルに入力し、縦や横にドラッグすると、指定されたセル範囲が適切に参照されます。

```excel
=INDEX($D$8:$D$52888,SEQUENCE(27,1,(ROW()-ROW($G$8))*138+1,1))
```

=INDEX($D$8:$D$52888,SEQUENCE(27,1,(ROW()-$G$1)*138+COLUMN()-COLUMN($G$8)+1,1))
この関数は、G8から始まるセル範囲をD列からのデータで参照します。H8から始まるセル範囲を参照する場合は、`COLUMN()-COLUMN($H$8)`を使ってセットごとのオフセットを計算します。