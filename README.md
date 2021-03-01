# Stock-Analysis

## Project Overview

The purpose of the project is to use VBA Code to analyze stocks data and provide insight on daily volume of stocks traded and the percentage return per Ticker. The raw Data comprises of 2017 and 2018 Stocks trading information, namely the starting price, closing prices and Volumes of stocks traded per ticker and the daily movement (Low and High).

## Method

An existing code was edited or refactored to loop through all the stocks data and determine the following.

•	Tickers of tocks

•	Volumes of stocks traded.

•	Percentage change or return. 

•	Applied appropriate formatting to the output.

## Results.

The code ran very well on both excel sheets (2017 and 2018). Results are shown below. 

## All Stocks 2017
![All Stocks 2017](https://user-images.githubusercontent.com/75961117/109427538-d5a56480-79c0-11eb-8a19-e4edcaa30f75.png)

## All Stocks 2018
![All Stocks 2018](https://user-images.githubusercontent.com/75961117/109427597-09808a00-79c1-11eb-8a1c-6d39784fd1d7.png)



## Code Ran time 2018
![Code Ran time for 2018](https://user-images.githubusercontent.com/75961117/109427684-84e23b80-79c1-11eb-9a0a-48198932e933.png)


## Code Ran time 2017

![Code ran time 2017](https://user-images.githubusercontent.com/75961117/109427926-9f68e480-79c2-11eb-9680-31581ae9c040.png)


## Pros and Cons of Refactoring Code

### Pros

Refactoring helped transform the code to fit with my data and ran faster.
Instead of a new code, a refactored code helped saved time.

### Cons
New bugs could be introduced and difficult to debug.

Overall, refactoring VBA script made it easier to optimize the code and completed the project on time.


## VBA Refactored code


Sub AllStocksAnalysisRefactored()

    Dim startTime As Single
    
    Dim endTime  As Single

    yearValue = InputBox("What year would you like to run the analysis on?")

    startTime = Timer
    
    'Format the output sheet on All Stocks Analysis worksheet
    Worksheets("All Stocks Analysis Refactored").Activate
    
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
    Next i
    
    For i = 2 To RowCount
        'Increase volume for current ticker
        tickerVolumes(tickerIndex) = tickerVolumes(tickerIndex) + Cells(i, 8).Value
        If Cells(i - 1, 1).Value <> tickers(tickerIndex) Then
            tickerStartingPrices(tickerIndex) = Cells(i, 6).Value
        End If
        If Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
            tickerEndingPrices(tickerIndex) = Cells(i, 6).Value
            tickerIndex = tickerIndex + 1
        End If
    Next i
    
    
    '4) Loop through your arrays to output the Ticker, Total Daily Volume, and Return.
    For i = 0 To 11
        
        Worksheets("All Stocks Analysis Refactored").Activate
        
        'add ticker
        Cells(4 + i, 1).Value = tickers(i)
        Cells(4 + i, 2).Value = tickerVolumes(i)
        Cells(4 + i, 3).Value = tickerEndingPrices(i) / tickerStartingPrices(i) - 1
        'add return
        
        
    Next i
    
    'Formatting
    Worksheets("All Stocks Analysis Refactored").Activate
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

Sub CleartheWorksheet()

Cells.Clear

End Sub



