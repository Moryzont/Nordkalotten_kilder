---
layout: wide
title: "Census and tax records"
---

<h1>Census and tax records</h1>

<p>
  Welcome to the census and tax records table. You can explore the table below, or just download it. The data is available as a .csv file, which you can open in excel or other programs like that. 
  
   The table below can be sorted by clicking on the header, of the column you want to sort. The table is to large to bee seen in its entirety, so you have to scroll horizontally to see the whole table. I recommend downloading it though. 
</p>


<a href="{{ '/Sources_by_type/Manntall.csv' | relative_url }}" download class="download-btn">
  Download Manntall CSV
</a>



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

if (["Digitized_link", "Transcription_link", "Table_link", "Archival_portal_link"].includes(field)) {
  col.render = function(url) {
    if (!url || url.trim().toLowerCase() === 'x') {
  return `<span class="btn no-link"></i> No link</span>`;
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
            // Set placeholder text for global search input
            $('div.dataTables_filter input').attr('placeholder', 'Type here to search');
    
  }
});


    }
  });
});

</script>
