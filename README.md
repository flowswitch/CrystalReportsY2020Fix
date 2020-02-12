# Crystal Reports 8 Y2020 fix
Crystal Reports 8 year 2020 problem fix

## Problem description
Crystal Reports stops generating reports after 1.1.2020
See https://stackoverflow.com/questions/59580722/odbc-connection-crystal-reports
and https://www.experts-exchange.com/questions/29168408/Crystal-Reports-8-5-Year-2020-problem.html

## Cause
The dBASE driver (p2ixbse.dll) checks temporary .dbt file's last update date field to be within 1.1.1980-31.12.2019 (file validation?).

## Solution
Patch the dll to bypass the year upper limit check

### Example
p2ixbse.dll:  
  size: 245760  
  FileVersion 8.0.100.1  
  sha256sum: c87980725c9fa2642a3e2d0c0cc8968ea8dbf9e7d753b7152e371324eddf1049  
0000A632: 7F -> 90  
0000A633: 30 -> 90  

For the other versions try looking for byte sequence 83 F9 77 7F xx, replace 7F xx with 90 90.
Or open an issue here and share your dll for analysis.
