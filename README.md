# Stock Analysis In Excel using VBA

## Overview

### Purpose And Data

The client wants to expand the dataset to include the entire stock market over the last few years (2017 and 2018). Refactoring is a key part of the coding process; a previous code was refactored to hopefully make the VBA script run faster. In the refactored code, no new aren’t new functionality are added, instead just make the code more efficient through taking fewer steps, using less memory, or improving the logic of the code to make it easier for future users to read. After the code was refactored, the analysis can help the client in determining if investing in certain stocks is worth it. The data used includes two spreadsheets with information on 12 different stocks. The stock information available includes a ticker value, the date the stock was issued, the opening, closing and adjusted closing price, the highest and lowest price, and the volume of the stock. The goal is to retrieve the ticker, the total daily volume, and the return on each stock and to determine how much time it took to execute.

### Analysis Code
Sub AllStocksAnalysisRefactored()
    Dim startTime As Single
    Dim endTime  As Single

    yearValue = InputBox("What year would you like to run the analysis on?")

    startTime = Timer
    
    'Format the output sheet on All Stocks Analysis worksheet
    Worksheets("All Stocks Analysis").Activate
    
    Range("A1").Value = "All Stocks (" + yearValue + ")"
    
    'Create a header row
    Cells(3, 1).Value = "Ticker"
    Cells(3, 2).Value = "Total Daily Volume"
    Cells(3, 3).Value = "Return"

    'Initialize array of all tickers
    Dim tickers(12) As String
    
    tickers(0) = "AY"
    tickers(1) = "CSIQ"
    tickers(2) = "DQ"
    tickers(3) = "ENPH"
    tickers(4) = "FSLR"
    tickers(5) = "HASI"
    tickers(6) = "JKS"
    tickers(7) = "RUN"
    tickers(8) = "SEDG"
    tickers(9) = "SPWR"
    tickers(10) = "TERP"
    tickers(11) = "VSLR"
    
    'Activate data worksheet
    Worksheets(yearValue).Activate
    
    'Get the number of rows to loop over
    RowCount = Cells(Rows.Count, "A").End(xlUp).Row
    
    '1a) Create a ticker Index
    tickerIndex = 0

    '1b) Create three output arrays
    Dim tickerVolumes(12) As Long
    Dim tickerStartingPrices(12) As Single
    Dim tickerEndingPrices(12) As Single
    
    ''2a) Create a for loop to initialize the tickerVolumes to zero.
    For i = 0 To 11
    tickerVolumes(i) = 0
    tickerStartingPrices(i) = 0
    tickerEndingPrices(i) = 0
Next i
        
   ''2b) Loop over all the rows in the spreadsheet.
For i = 2 To RowCount

    '3a) Increase volume for current ticker
    tickerVolumes(tickerIndex) = tickerVolumes(tickerIndex) + Cells(i, 8).Value
    
    '3b) Check if the current row is the first row with the selected tickerIndex.
    'If  Then
    If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i - 1, 1).Value <> tickers(tickerIndex) Then
        tickerStartingPrices(tickerIndex) = Cells(i, 6).Value
    End If
    
    '3c) check if the current row is the last row with the selected ticker
    'If  Then
     If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
        tickerEndingPrices(tickerIndex) = Cells(i, 6).Value
     End If

        '3d Increase the tickerIndex.
         If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
            tickerIndex = tickerIndex + 1
        End If

Next i

'4) Loop through your arrays to output the Ticker, Total Daily Volume, and Return.
For i = 0 To 11
    
    Worksheets("All Stocks Analysis").Activate
    Cells(4 + i, 1).Value = tickers(i)
    Cells(4 + i, 2).Value = tickerVolumes(i)
    Cells(4 + i, 3).Value = tickerEndingPrices(i) / tickerStartingPrices(i) - 1
    
Next i
    
    'Formatting
    Worksheets("All Stocks Analysis").Activate
    Range("A3:C3").Font.FontStyle = "Bold"
    Range("A3:C3").Borders(xlEdgeBottom).LineStyle = xlContinuous
    Range("B4:B15").NumberFormat = "#,##0"
    Range("C4:C15").NumberFormat = "0.0%"
    Columns("B").AutoFit

    dataRowStart = 4
    dataRowEnd = 15

    For i = dataRowStart To dataRowEnd
        
        If Cells(i, 3) > 0 Then
            
            Cells(i, 3).Interior.Color = vbGreen
            
        Else
        
            Cells(i, 3).Interior.Color = vbRed
            
        End If
        
    Next i
 
    endTime = Timer
    MsgBox "This code ran in " & (endTime - startTime) & " seconds for the year " & (yearValue)

End Sub
### Results
<img width="257" alt="VBA_Challenge_2017" src="https://user-images.githubusercontent.com/98627709/156926297-1d906d31-07d9-4e84-91f8-f0249af33de8.png">
<img width="257" alt="VBA_Challenge_2018" src="https://user-images.githubusercontent.com/98627709/156926303-6b515191-fafe-4b8a-ac47-868aa900892f.png">

Excel File [VBA_Challenge.xlsm.zip](https://github.com/cgomez1995/stock-analysis/files/8192716/VBA_Challenge.xlsm.zip)

## Conclusion
Refactoring code helps make the process more organized. Some disadvantages that may not allow code to be refactored are large applications that are too large and pose some risk to ruin the code. As a result of refactoring the code, the in macro run time was a decreased. The original analysis took longer run, the new analysis took 0.046875 seconds for 2017 and 0.05859375 seconds for 2018, as showcased above. 
