---
layout: single
title: "Census Data Table"
classes: wide
---

# Interactive Table

[**Back to Home**]({{ '/' | relative_url }})

Below is the interactive table loading data from:
`Sources_by_type/Manntall.csv`

<div class="table-container">
  <table id="manntall-table" class="table table-striped">
  <thead>
    <!-- First row: actual column headings -->
    <tr>
      <th>Census_type</th>
      <th>Year</th>
      <th>Geographic_area</th>
      <th>Detail_level</th>
      <th>State</th>
      <th>Creator</th>
      <th>dat_grov</th>
      <th>Usefull_info</th>
      <th>Reference</th>
      <th>Pagenumber</th>
      <th>Digitized_link</th>
      <th>Transcribed</th>
      <th>Tabulated</th>
      <th>Transcription_link</th>
      <th>Table_link</th>
      <th>Archival_portal_link</th>
    </tr>
    <!-- Second row: input boxes for searching each column -->
    <tr>
      <th><input type="text" placeholder="Search Census_type" style="width:100%;"></th>
      <th><input type="text" placeholder="Year" style="width:100%;"></th>
      <th><input type="text" placeholder="Geographic_area" style="width:100%;"></th>
      <th><input type="text" placeholder="Detail_level" style="width:100%;"></th>
      <th><input type="text" placeholder="State" style="width:100%;"></th>
      <th><input type="text" placeholder="Creator" style="width:100%;"></th>
      <th><input type="text" placeholder="dat_grov" style="width:100%;"></th>
      <th><input type="text" placeholder="Usefull_info" style="width:100%;"></th>
      <th><input type="text" placeholder="Reference" style="width:100%;"></th>
      <th><input type="text" placeholder="Pagenumber" style="width:100%;"></th>
      <th><input type="text" placeholder="Digitized_link" style="width:100%;"></th>
      <th><input type="text" placeholder="Transcribed" style="width:100%;"></th>
      <th><input type="text" placeholder="Tabulated" style="width:100%;"></th>
      <th><input type="text" placeholder="Transcription_link" style="width:100%;"></th>
      <th><input type="text" placeholder="Table_link" style="width:100%;"></th>
      <th><input type="text" placeholder="Archival_portal_link" style="width:100%;"></th>
    </tr>
  </thead>
  <tbody>
    <!-- DataTables will populate this -->
  </tbody>
</table>
</div>
