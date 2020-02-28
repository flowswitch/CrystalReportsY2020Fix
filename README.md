# Crystal Reports 8 Y2020 fix
Crystal Reports 8 year 2020 problem fix

## Problem description
Crystal Reports stops generating reports after 1.1.2020
See https://stackoverflow.com/questions/59580722/odbc-connection-crystal-reports
and https://www.experts-exchange.com/questions/29168408/Crystal-Reports-8-5-Year-2020-problem.html

## Cause
The dBASE driver checks temporary .dbt file's last update date field to be within 1.1.1980-31.12.2019 (file validation?).

## Solution
Patch the dll to bypass the year upper limit check

### File locations and names
Look at %WINDIR%\CRYSTAL\ folder for dll files with names ending with "xbse": 
- p2ixbse.dll
- p2bxbse.dll
- pdbxbse.dll

### Known versions
p2ixbse.dll:  
  size: 245760  
  FileVersion 8.0.100.1  
  sha256sum: c87980725c9fa2642a3e2d0c0cc8968ea8dbf9e7d753b7152e371324eddf1049  
0000A632: 7F -> 90  
0000A633: 30 -> 90  

pdbxbse.dll:  
  size: 259072  
  FileVersion: 4.0.0.5  
  sha256sum: d0c70206e90b65496c5b5d7d6d32425dc55bffb0d6c0a3e49c574bc98043c2ec  
0001FAD8: 7F -> 90  
0001FAD9: 2C -> 90  
0002C6CF: 7F -> 90  
0002C6D0: 30 -> 90  

p2bxbse.dll:  
  size: 261632  
  FileVersion: 5.0.0.20  
  sha256sum: 60ebb8901b206ff28c3fe7228226b36dbdf00980c278c1eb2c4ebaf946ce7892  
0000D58F: 0F -> 90  
0000D590: 8F -> 90  
0000D591: 54 -> 90  
0000D592: 00 -> 90  
0000D593: 00 -> 90  
0000D594: 00 -> 90  
00015609: 0F -> 90  
0001560A: 8F -> 90  
0001560B: 40 -> 90  
0001560C: 00 -> 90  
0001560D: 00 -> 90  
0001560E: 00 -> 90  
  

### Other versions
#### Variant 1
Try looking for byte sequence 77 7F xx, replace 7F xx with 90 90. Some versions have two checks - patch both.

#### Variant 2
Try looking for byte sequence 77 0F 8F xx xx xx xx, replace 0F 8F xx xx xx xx with 90 90 90 90 90 90. Some versions have two checks - patch both.

Or open an issue here and share your dll for analysis.

