# How to get d3 data for Sankey diagram?
  1. Open and edit GoogleSpread [document](https://docs.google.com/spreadsheets/d/1ZPk3lk1_NFbnedp6q4qptXcqvVdhjXMaio2V72AeVjk/edit#gid=1356752871).

  2. Get GoogleSpread document as raw CSV:  
```powershell
$ $url = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vQL6OiLzFaJ1p4KipLTumufZNIwJQRYS3H7IGKf3Q6aFSv3OHXNP8t-mTqkviSXN8q34OaC-tiOcWbe/pub?gid=1356752871&single=true&output=csv'
$ Invoke-WebRequest -Uri $url | Select-Object -ExpandProperty Content
```

  3. Paste contents into [textarea](https://observablehq.com/d/ce0c4fac209fd667)