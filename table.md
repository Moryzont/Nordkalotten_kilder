---
layout: wide
title: "Census and tax records"
---

<h1>Census and tax records</h1>

<p>
  This page loads the .csv file found in the github repo. The search only works for the whole table, sadly. You can sort the table by clicking on the header. You do not see the whole table, so you have to scroll horizontally to see the whole. I recommend downloading it though.
</p>


<!-- Main table container -->
<div id="table-container" style="overflow-x:auto; border:1px solid #ccc;">
  <!-- We start with an empty table. DataTables + JS will create headers. -->
  <table id="dynamic-table" class="display" style="width:100%;">
    <!-- No <thead> or <tbody> here, we’ll let DataTables handle it dynamically. -->
  </table>
</div>

<script>
// 1. On DOMContentLoaded, parse the CSV and build the table.
document.addEventListener('DOMContentLoaded', function() {
  // Path to your CSV (adjust if needed, or use relative_url if a Jekyll path)
  const csvUrl = "{{ '/Sources_by_type/Manntall.csv' | relative_url }}";

  console.log("Parsing CSV from", csvUrl);

  Papa.parse(csvUrl, {
    download: true,
    header: true,       // first row = headers
    skipEmptyLines: true,
    complete: function(results) {
      if (!results.data || results.data.length === 0) {
        console.warn("CSV parse returned 0 rows");
        return;
      }
      console.log("CSV parse found rows:", results.data.length);
      console.log("Sample row:", results.data[0]);
      console.log("Fields (headers):", results.meta.fields);

     // 2. Dynamically create DataTables column definitions from the CSV headers
const dynamicColumns = results.meta.fields.map(field => {
  // Basic column setup for each field
  let col = {
    title: field,
    data: field
  };

  // If the field matches one of the URL columns, add a custom renderer
  if (["Digitized_link", "Transcription_link", "Table_link", "Archival_portal_link"].includes(field)) {
    col.render = function(url) {
      if (!url || url.trim().toLowerCase() === 'x') {
        return `<span class="text-danger"><i class="fas fa-frown"></i> No link</span>`;
      }
      return `<a href="${url}" target="_blank" class="btn btn-sm btn-primary">Link</a>`;
    };
  }
  return col;
});

// 3. Initialize DataTables with dynamic columns
const table = $('#dynamic-table').DataTable({
  data: results.data,       
  columns: dynamicColumns,  
  scrollX: true,            
  autoWidth: false,
  paging: false,
  searching: true,
  ordering: true,
  info: false,
  dom: 'frtip',
  initComplete: function() {
    console.log("DataTables initComplete. Rows:", this.api().rows().count());
    
  }
});


    }
  });
});

</script>
