## Algorithm used to Write data into Spreadsheet

```javascript
function insertRow(coulumnValue, cSheet) {
  var lock = LockService.getScriptLock();
  lock.waitLock(LOCK_TIME_MS);
  try { 
    cSheet.appendRow([coulumnValue]);
    SpreadsheetApp.flush();
  } finally {
    lock.releaseLock();
  }
}
```

## Algorithm used to Write data into MySQL

```javascript
function writeOneRecord(value) {
  var conn = Jdbc.getCloudSqlConnection(DB_URL, USER_NAME, PASSWORD);
  var stmt = conn.prepareStatement('INSERT INTO entries (guestName, content) values (?, ?)');
  stmt.setString(1, value);
  stmt.execute();
}
```

## API that used to Call from Apache Jmeter

```javascript
function doGet(e) {
  if (e.parameter.myq == 'dataItem') {
    var myobj = {};
    myobj.output = "OK";
    myobj.data = "sucesss";
    writeOneRecord(); // no need to send a correct response, since we need to check only the output inside the database
    return ContentService.createTextOutput(JSON.stringify(myobj)).setMimeType(ContentService.MimeType.JSON); 
  }  
}
```
