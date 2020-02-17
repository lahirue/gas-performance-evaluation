## Algorithm used to Read Spreadsheet Data

```javascript
function readSpreadsheetData () {
  var start = new Date().getTime();
  var sp = SpreadsheetApp.openById("SPREADSHEET_ID");
  var cSheet = sp.getSheetByName("TABLE_NAME");
  var lastRow = sp.getLastRow();

  var itemFound = '';
  var datas = cSheet.getRange(1, 1, lastRow, NUMBER_OF_COLUMNS).getValues();
  var result = datas.forEach(function (element) {
   if (element[1]=='SEARCH_KEY') {
       itemFound = element;
       return;
     }
   });
  var end = new Date().getTime();
  var formatted = Utilities.formatString("%sms", end - start);
  return formatted;
}
```

## Algorithm used to Read Spreadsheet data uing Query

```javascript
function spreadsheetQuery(){
  var start = new Date().getTime();
  var spreadsheetId = 'SPREADSHEET_ID';
  var targetRange = 'TABLE_NAME!A:T';
  var SQL = "select A,B,D, G WHERE B='SEARCH_KEY'"
  var query = '=QUERY('+targetRange+',\"'+SQL+'\")'
  var currentDoc = SpreadsheetApp.openById(spreadsheetId);
  var tempSheet = currentDoc.insertSheet();
  var pushQuery = tempSheet.getRange(1, 1).setFormula(query);
  var pullResult = tempSheet.getDataRange().getValues();
  currentDoc.deleteSheet(tempSheet); 
  var end = new Date().getTime();
  var formatted = Utilities.formatString("%sms", end - start);
  return formatted;
}
```

## Algorithm used to Read Spreadsheet data uing JOIN Query

```javascript
function joinSpreadSheet(){
  var start = new Date().getTime();
  var spreadsheetId = 'SPREADSHEET_ID';
  var sql = "SELECT Col1,Col2,Col3,Col4,Col5 Where Col1 = 'SEARCH_KEY'";
  var range = 'ArrayFormula({TABLE_ONE!A2:U20000,VLOOKUP(TABLE_ONE!A2:A20000,TABLE_TWO!A2:L20000,{2,3,4},FALSE)})';
  var sqlQuery = '=QUERY('+range+',\"'+sql+'\")';

  var currentDoc = SpreadsheetApp.openById(spreadsheetId);
  var tempSheet = currentDoc.insertSheet();

  var pushQuery = tempSheet.getRange(1, 1).setFormula(sqlQuery);
  var pullResult = tempSheet.getDataRange().getValues();
  currentDoc.deleteSheet(tempSheet); 
  var end = new Date().getTime();

  var formatted = Utilities.formatString("%sms", end - start);
  return formatted;
}
```