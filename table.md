---
layout: wide
title: "Census and Tax records, until 1750"
---
# Interactive Table

[**Back to Home**]({{ '/' | relative_url }})

Below is the interactive table. It is wide, with a **top** scrollbar:

<!-- top scrollbar container (sticky) -->
<div id="top-scrollbar" style="
  position: sticky;
  top: 0; 
  height: 20px; 
  overflow-x: auto; 
  background: #f0f0f0;
  z-index: 999;
  margin-bottom: 0.5rem;
">
  <div id="top-scroll-content" style="height: 1px;"></div>
</div>

<!-- main container for the table -->
<div id="table-container" style="overflow-x: auto; border: 1px solid #ccc;">

<!-- Use short header text, if needed, and store the full text in title=... -->
<table id="manntall-table" class="table table-striped" style="white-space: nowrap;">
  <thead>
    <tr>
      <th title="Type of census data or record">Census_type</th>
      <th title="Year of the record">Year</th>
      <th title="Geographic area for the record">Geographic_area</th>
      <th title="Detail level of the data">Detail_level</th>
      <th title="State or country">State</th>
      <th title="Name of creator">Creator</th>
      <th title="dat_grov? Some short data field">dat_grov</th>
      <th title="Useful info yes/no?">Usefull_info</th>
      <th title="Reference code or doc #">Reference</th>
      <th title="Page number?">Pagenumber</th>
      <th title="Digitized link to doc?">Digitized_link</th>
      <th title="Is it transcribed?">Transcribed</th>
      <th title="Is it tabulated?">Tabulated</th>
      <th title="URL for transcription doc?">Transcription_link</th>
      <th title="URL for table?">Table_link</th>
      <th title="URL for archival portal?">Archival_portal_link</th>
    </tr>
    <tr>
      <th><input type="text" placeholder="Search Type" style="width:100%;"></th>
      <th><input type="text" placeholder="Year" style="width:100%;"></th>
      <th><input type="text" placeholder="Area" style="width:100%;"></th>
      <th><input type="text" placeholder="Detail" style="width:100%;"></th>
      <th><input type="text" placeholder="State" style="width:100%;"></th>
      <th><input type="text" placeholder="Creator" style="width:100%;"></th>
      <th><input type="text" placeholder="dat_grov" style="width:100%;"></th>
      <th><input type="text" placeholder="Info" style="width:100%;"></th>
      <th><input type="text" placeholder="Reference" style="width:100%;"></th>
      <th><input type="text" placeholder="Page" style="width:100%;"></th>
      <th><input type="text" placeholder="Dig link" style="width:100%;"></th>
      <th><input type="text" placeholder="Transcribed" style="width:100%;"></th>
      <th><input type="text" placeholder="Tabulated" style="width:100%;"></th>
      <th><input type="text" placeholder="Trans link" style="width:100%;"></th>
      <th><input type="text" placeholder="Table link" style="width:100%;"></th>
      <th><input type="text" placeholder="Portal" style="width:100%;"></th>
    </tr>
  </thead>
  <tbody></tbody>
</table>
</div>

<script>
const columns = [
  // Example narrower columns for yes/no fields 
  // or bigger for textual columns. Summation > container width ensures scrolling
  { data: 'Census_type', width: '120px' },
  { data: 'Year', width: '80px' },
  { data: 'Geographic_area', width: '150px' },
  { data: 'Detail_level', width: '120px' },
  { data: 'State', width: '100px' },
  { data: 'Creator', width: '120px' },
  { data: 'dat_grov', width: '100px' },
  { data: 'Usefull_info', width: '100px' },
  { data: 'Reference', width: '100px' },
  { data: 'Pagenumber', width: '80px' },
  {
    data: 'Digitized_link', width: '120px',
    render: function(url) { ... } // same as before
  },
  { data: 'Transcribed', width: '80px' },
  { data: 'Tabulated', width: '80px' },
  {
    data: 'Transcription_link', width: '120px',
    render: function(url) { ... }
  },
  {
    data: 'Table_link', width: '120px',
    render: function(url) { ... }
  },
  {
    data: 'Archival_portal_link', width: '120px',
    render: function(url) { ... }
  }
];

document.addEventListener('DOMContentLoaded', function() {
  // parse CSV
  Papa.parse("{{ '/Sources_by_type/Manntall.csv' | relative_url }}", {
    download: true,
    header: true,
    skipEmptyLines: true,
    complete: function(results) {
      const data = results.data;
      // init DataTables
      const table = $('#manntall-table').DataTable({
        data: data,
        columns: columns,
        scrollX: true,
        autoWidth: false,
        dom: 'Bfrtip',
        buttons: [
          {
            extend: 'csvHtml5',
            text: 'Download CSV',
            className: 'btn btn-primary'
          }
        ],
        paging: false,
        ordering: true,
        searching: true,
        info: false,
        lengthChange: false,
        initComplete: function() {
          const api = this.api();
          // per-column search in 2nd row
          $('#manntall-table thead tr:eq(1) th').each(function(i) {
            $('input', this).on('keyup change', function() {
              if (api.column(i).search() !== this.value) {
                api.column(i).search(this.value).draw();
              }
            });
          });
        }
      });
    }
  });
});

// top scrollbar sync
document.addEventListener('DOMContentLoaded', function() {
  const topScroll = document.getElementById('top-scrollbar');
  const topScrollContent = document.getElementById('top-scroll-content');
  const tableContainer = document.getElementById('table-container');
  const manntallTable = document.getElementById('manntall-table');

  function updateScrollWidths() {
    // after DataTables is done, measure scrollWidth
    const tableWidth = manntallTable.scrollWidth;
    topScrollContent.style.width = tableWidth + 'px';
  }

  // sync horizontal scroll
  topScroll.addEventListener('scroll', () => {
    tableContainer.scrollLeft = topScroll.scrollLeft;
  });
  tableContainer.addEventListener('scroll', () => {
    topScroll.scrollLeft = tableContainer.scrollLeft;
  });

  // Wait a bit so DataTables finishes layout, or use setTimeout
  setTimeout(updateScrollWidths, 1000);
  window.addEventListener('resize', updateScrollWidths);
});
</script>
