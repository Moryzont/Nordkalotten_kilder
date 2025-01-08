---
layout: wide
title: "Census and Tax records, until 1750"
---
<h1>Interactive Table</h1>
<p>
  <a href="{{ '/' | relative_url }}">Back to Home</a>
</p>

<p>
  Below is the interactive table loading data from, you can also access it
  from the main repo:
  <code>Sources_by_type/Manntall.csv</code>
</p>

<div class="content-wide">

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
    <!-- This will match table width dynamically -->
    <div id="top-scroll-content" style="height: 1px;"></div>
  </div>

  <!-- main table container with overflow-x -->
  <div id="table-container" style="overflow-x: auto; border: 1px solid #ccc;">
    <table id="manntall-table" class="table table-striped" style="white-space: nowrap;">
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
      <tbody></tbody>
    </table>
  </div>
</div>

<script>
// Adjust column widths to be large enough so scrolling is meaningful
const columns = [
  { data: 'Census_type', width: '150px' },
  { data: 'Year', width: '150px' },
  { data: 'Geographic_area', width: '150px' },
  { data: 'Detail_level', width: '150px' },
  { data: 'State', width: '150px' },
  { data: 'Creator', width: '150px' },
  { data: 'dat_grov', width: '150px' },
  { data: 'Usefull_info', width: '150px' },
  { data: 'Reference', width: '150px' },
  { data: 'Pagenumber', width: '150px' },
  {
    data: 'Digitized_link', width: '150px',
    render: function(url) {
      if (!url || url.trim().toLowerCase() === 'x') {
        return `<span class="text-danger"><i class="fas fa-frown"></i> No link</span>`;
      }
      return `<a href="${url}" target="_blank" class="btn btn-sm btn-primary">Link</a>`;
    }
  },
  { data: 'Transcribed', width: '150px' },
  { data: 'Tabulated', width: '150px' },
  {
    data: 'Transcription_link', width: '150px',
    render: function(url) {
      if (!url || url.trim().toLowerCase() === 'x') {
        return `<span class="text-danger"><i class="fas fa-frown"></i> No link</span>`;
      }
      return `<a href="${url}" target="_blank" class="btn btn-sm btn-success">Transcription</a>`;
    }
  },
  {
    data: 'Table_link', width: '150px',
    render: function(url) {
      if (!url || url.trim().toLowerCase() === 'x') {
        return `<span class="text-danger"><i class="fas fa-frown"></i> No link</span>`;
      }
      return `<a href="${url}" target="_blank" class="btn btn-sm btn-info">Table</a>`;
    }
  },
  {
    data: 'Archival_portal_link', width: '150px',
    render: function(url) {
      if (!url || url.trim().toLowerCase() === 'x') {
        return `<span class="text-danger"><i class="fas fa-frown"></i> No link</span>`;
      }
      return `<a href="${url}" target="_blank" class="btn btn-sm btn-warning">Archive</a>`;
    }
  }
];

document.addEventListener('DOMContentLoaded', function() {
  // parse CSV
  Papa.parse("{{ '/Sources_by_type/Manntall.csv' | relative_url }}", {
    download: true,
    header: true,
    skipEmptyLines: true,
    complete: function(results) {
      // initialize DataTables
      const data = results.data;
      const table = $('#manntall-table').DataTable({
        data: data,
        columns: columns,
        scrollX: true,
        autoWidth: false, // ensures we use our 'width' settings
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
        }
      });
    }
  });
});

// Top scrollbar sync
document.addEventListener('DOMContentLoaded', function() {
  const topScroll = document.getElementById('top-scrollbar');
  const topScrollContent = document.getElementById('top-scroll-content');
  const tableContainer = document.getElementById('table-container');
  const manntallTable = document.getElementById('manntall-table');

  function updateScrollWidths() {
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

  // recalc after load or resize
  updateScrollWidths();
  window.addEventListener('resize', updateScrollWidths);
});
</script>
