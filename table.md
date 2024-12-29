---
layout: page
title: "Manntall Data Table"
---
# Interactive Manntall Table

[Back to Home]({{ '/' | relative_url }})

Below is the interactive table that loads data from `Sources_by_type/Manntall.csv`.

<table id="manntall-table" class="display">
  <thead>
    <tr>
      <!-- EXACT column names from the CSV header row -->
      <th>Type</th>
      <th>Årstall</th>
      <th>Geografisk område</th>
      <th>Detaljnivå</th>
      <th>Stat</th>
      <th>Skaper</th>
      <th>Grov dat.</th>
      <th>Nyttig info</th>
      <th>Referanse</th>
      <th>Sidetall</th>
      <th>Link - arkiv</th>
      <th>Transk.</th>
      <th>Tabell</th>
      <th>Link - transkribert</th>
      <th>Link - Tabell</th>
      <th>Link - Arkivportal</th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

<script>
  // If your site has baseurl set in _config.yml, relative_url will handle it.
  const csvFilePath = "{{ '/Sources_by_type/Manntall.csv' | relative_url }}";

  // Match columns to your CSV headers
const columns = [
  { data: 'Type manntall' },
  { data: 'Årstall' },
  { data: 'Geografisk område' },
  { data: 'Detaljnivå' },
  { data: 'Stat' },
  { data: 'Skaper' },
  { data: 'Grov dat.' },
  { data: 'Nyttig info' },
  { data: 'Referanse ' },  // if there's a trailing space in the CSV
  { data: 'Sidetall' },
  { data: 'Link - arkiv' },
  { data: 'Transk.' },
  { data: 'Tabell' },
  { data: 'Link - transkribert' },
  { data: 'Link - Tabell' },
  { data: 'Link - Arkivportal' }
];


  $(document).ready(function() {
    Papa.parse(csvFilePath, {
      download: true,
      header: true,
      skipEmptyLines: true,
      delimiter: ";",
      complete: function(results) {
        const data = results.data; // array of objects
        $('#manntall-table').DataTable({
          data: data,
          columns: columns,
          paging: true,
          searching: true,
          ordering: true,
          pageLength: 10,
          lengthMenu: [5, 10, 25, 50, 100]
        });
      }
    });
  });
</script>
