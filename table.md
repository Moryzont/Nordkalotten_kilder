# Interactive Table

[**Back to Home**]({{ '/' | relative_url }})

Below is the interactive table loading data from 
`Sources_by_type/Manntall.csv`.

<!-- top scrollbar container -->
<div id="top-scrollbar">
  <div id="top-scroll-content"></div>
</div>

<!-- main container for the table -->
<div id="table-container">
  <table id="manntall-table" class="table table-striped">
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
      <tr>
        <th><input type="text" placeholder="Type" style="width:100%;"></th>
        <th><input type="text" placeholder="Year" style="width:100%;"></th>
        <th><input type="text" placeholder="Area" style="width:100%;"></th>
        <th><input type="text" placeholder="Detail" style="width:100%;"></th>
        <th><input type="text" placeholder="State" style="width:100%;"></th>
        <th><input type="text" placeholder="Creator" style="width:100%;"></th>
        <th><input type="text" placeholder="dat_grov" style="width:100%;"></th>
        <th><input type="text" placeholder="Info" style="width:100%;"></th>
        <th><input type="text" placeholder="Reference" style="width:100%;"></th>
        <th><input type="text" placeholder="Page" style="width:100%;"></th>
        <th><input type="text" placeholder="DigLink" style="width:100%;"></th>
        <th><input type="text" placeholder="Transcribed" style="width:100%;"></th>
        <th><input type="text" placeholder="Tabulated" style="width:100%;"></th>
        <th><input type="text" placeholder="TransLink" style="width:100%;"></th>
        <th><input type="text" placeholder="TableLink" style="width:100%;"></th>
        <th><input type="text" placeholder="Portal" style="width:100%;"></th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>
</div>

<script>
// 1) define columns with widths that total > 1200px to force horizontal scroll
const columns = [
  { data: 'Census_type', width: '150px' },
  { data: 'Year', width: '80px' },
  { data: 'Geographic_area', width: '160px' },
  { data: 'Detail_level', width: '120px' },
  { data: 'State', width: '100px' },
  { data: 'Creator', width: '120px' },
  { data: 'dat_grov', width: '100px' },
  { data: 'Usefull_info', width: '120px' },
  { data: 'Reference', width: '120px' },
  { data: 'Pagenumber', width: '80px' },
  {
    data: 'Digitized_link', width: '140px',
    render: function(url) {
      if (!url || url.trim().toLowerCase() === 'x') {
        return `<span class="text-danger"><i class="fas fa-frown"></i> No link</span>`;
      }
      return `<a href="${url}" target="_blank" class="btn btn-sm btn-primary">Link</a>`;
    }
  },
  { data: 'Transcribed', width: '80px' },
  { data: 'Tabulated', width: '80px' },
  {
    data: 'Transcription_link', width: '140px',
    render: function(url) {
      if (!url || url.trim().toLowerCase() === 'x') {
        return `<span class="text-danger"><i class="fas fa-frown"></i> No link</span>`;
      }
      return `<a href="${url}" target="_blank" class="btn btn-sm btn-success">Transcription</a>`;
    }
  },
  {
    data: 'Table_link', width: '140px',
    render: function(url) {
      if (!url || url.trim().toLowerCase() === 'x') {
        return `<span class="text-danger"><i class="fas fa-frown"></i> No link</span>`;
      }
      return `<a href="${url}" target="_blank" class="btn btn-sm btn-info">Table</a>`;
    }
  },
  {
    data: 'Archival_portal_link', width: '140px',
    render: function(url) {
      if (!url || url.trim().toLowerCase() === 'x') {
        return `<span class="text-danger"><i class="fas fa-frown"></i> No link</span>`;
      }
      return `<a href="${url}" target="_blank" class="btn btn-sm btn-warning">Archive</a>`;
    }
  }
];

// 2) parse the CSV, debug log to see if we have data
document.addEventListener('DOMContentLoaded', function() {
  console.log("Parsing CSV...");

  Papa.parse("{{ '/Sources_by_type/Manntall.csv' | relative_url }}", {
    download: true,
    header: true,
    skipEmptyLines: true,
    complete: function(results) {
      console.log("Papa Parse complete. rows: ", results.data.length);
      console.log("Sample row: ", results.data[0]);
      // if results.data is empty, your CSV might be missing or in the wrong path

      const table = $('#manntall-table').DataTable({
        data: results.data,
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
          // per-column search
          $('#manntall-table thead tr:eq(1) th').each(function(i) {
            $('input', this).on('keyup change', function() {
              if (api.column(i).search() !== this.value) {
                api.column(i).search(this.value).draw();
              }
            });
          });
          console.log("DataTables initComplete. Table row count: ", api.rows().count());
        }
      });
    }
  });
});

// 3) top scrollbar sync
document.addEventListener('DOMContentLoaded', function() {
  const topScroll = document.getElementById('top-scrollbar');
  const topScrollContent = document.getElementById('top-scroll-content');
  const tableContainer = document.getElementById('table-container');
  const manntallTable = document.getElementById('manntall-table');

  function updateScrollWidths() {
    const w = manntallTable.scrollWidth;
    console.log("table scrollWidth = ", w);
    topScrollContent.style.width = w + 'px';
  }

  // sync events
  topScroll.addEventListener('scroll', () => {
    tableContainer.scrollLeft = topScroll.scrollLeft;
  });
  tableContainer.addEventListener('scroll', () => {
    topScroll.scrollLeft = tableContainer.scrollLeft;
  });

  // wait a bit to let DataTables finalize layout
  setTimeout(updateScrollWidths, 1500);

  window.addEventListener('resize', updateScrollWidths);
});
</script>
<script>
document.addEventListener('DOMContentLoaded', function() {
  // By now, papaparse.min.js is loaded from the layout
  Papa.parse("{{ '/Sources_by_type/Manntall.csv' | relative_url }}", {
    download: true,
    header: true,
    skipEmptyLines: true,
    complete: function(results) {
      // data is in results.data
      // Now you can call DataTables, etc.
    }
  });
});