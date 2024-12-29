---
layout: default
title: "Manntall Data Table"
---
# Interactive Manntall Table

Below is an example interactive table that loads data from 
`Sources_by_type/Manntall.csv`. You can sort, search, and paginate
these records.

<!-- The HTML table element for DataTables -->
<table id="manntall-table" class="display">
  <thead>
    <tr>
      <!-- Replace these headers with your actual column names -->
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
  <tbody>
    <!-- Data inserted dynamically -->
  </tbody>
</table>

<!-- The script that fetches and displays the CSV -->
<script>
  // Build the path to your CSV:
  // Use the relative_url filter so Jekyll sets the correct prefix if needed
  const csvFilePath = "{{ '/Sources_by_type/Manntall.csv' | relative_url }}";

  // Match these columns to your CSV headers
  const columns = [
    { data: 'Type' },
    { data: 'Årstall' },
    { data: 'Geografisk område' },
    { data: 'Detaljnivå' },
    { data: 'Stat' },
    { data: 'Skaper' },
    { data: 'Grov dat.' },
    { data: 'Nyttig info' },
    { data: 'Referanse' },
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
      complete: function(results) {
        const data = results.data; // array of objects
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
