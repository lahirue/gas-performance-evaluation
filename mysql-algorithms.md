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
 

## Algorithm used to Read MySQL data uing JOIN Query

```javascript
function readMySQLJoin() {
 var start = new Date();
 var conn = Jdbc.getCloudSqlConnection(DB_URL, USER_NAME, PASSWORD);
 var stmt = conn.createStatement();
 stmt.setMaxRows(1000);
 var results = stmt.executeQuery("SELECT t1.NIC,t1.Index_No,t1.L_Name,t2.Reg_No,t2.Status FROM studbasic as t1 LEFT JOIN stud as t2 ON t1.NIC=t2.NIC WHERE t2.Reg_No='SEARCH_KEY'");
 var numCols = results.getMetaData().getColumnCount();
  while (results.next()) {
    var rowString = '';
    for (var col = 0; col < numCols; col++) {
      rowString += results.getString(col + 1) + '\t';
    }
    Logger.log(rowString);
    console.log(rowString);
  }
  results.close();
  stmt.close();
  var end = new Date();
  var formatted = Utilities.formatString("%sms", end - start);
  return formatted;
}
```