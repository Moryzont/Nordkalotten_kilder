---
layout: page
title: "Manntall Data Table"
---
# Interactive Manntall Table

<script>
  $(document).ready(function(){
    // DataTables + PapaParse code here
  });
</script>

[**Back to Home**]({{ '/' | relative_url }})

Below is the interactive table loading data from:
`Sources_by_type/Manntall.csv`

<table id="manntall-table" class="display">
  <thead>
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
  </thead>
  <tbody></tbody>
</table>

<script>
  // Because your baseurl is "/Nordkalotten_kilder", and you have
  // the CSV in "Sources_by_type/Manntall.csv" at the project root,
  // we use relative_url:
  const csvFilePath = "{{ '/Sources_by_type/Manntall.csv' | relative_url }}";

  // Must match EXACTLY the header names from your CSV:
  const columns = [
    { data: 'Census_type' },
    { data: 'Year' },
    { data: 'Geographic_area' },
    { data: 'Detail_level' },
    { data: 'State' },
    { data: 'Creator' },
    { data: 'dat_grov' },
    { data: 'Usefull_info' },
    { data: 'Reference' },
    { data: 'Pagenumber' },
    { data: 'Digitized_link' },
    { data: 'Transcribed' },
    { data: 'Tabulated' },
    { data: 'Transcription_link' },
    { data: 'Table_link' },
    { data: 'Archival_portal_link' }
  ];

  $(document).ready(function() {
    Papa.parse(csvFilePath, {
      download: true,
      header: true,
      skipEmptyLines: true,
      // Because you're using commas now, we don't need delimiter: ';'.
      complete: function(results) {
        const data = results.data; // an array of objects
        // Initialize DataTables
        $('#manntall-table').DataTable({
          data: data,
          columns: columns,
          paging: true,
          searching: true,
          ordering: true,
          pageLength: 10,
          lengthMenu: [5, 10, 25, 50, 100],
          language: {
            search: 'Filter:'
          }
        });
      }
    });
  });
</script>
