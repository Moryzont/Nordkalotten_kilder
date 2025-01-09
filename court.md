---
layout: wide
title: "Court records until 1750"
---

<h1>Court records until</h1>

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
  const csvUrl = "{{ '/Sources_by_type/Rettsdokumenter.csv' | relative_url }}";

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
      // e.g., an array of { title: fieldName, data: fieldName }
      const dynamicColumns = results.meta.fields.map(field => {
        return {
          title: field, // column header text
          data: field   // property name in each row of data
        };
      });

      // 3. Initialize DataTables
      // We’ll attach to #dynamic-table
      // Let DataTables generate the <thead> from column info we pass
      const table = $('#dynamic-table').DataTable({
        data: results.data,       // entire array of objects from CSV
        columns: dynamicColumns,  // generated from CSV headers
        scrollX: true,            // horizontal scroll
        autoWidth: false,
        paging: false,
        searching: true,
        ordering: true,
        info: false,
        // "dom" can be customized if you want "Bfrtip" or other DataTables UI
        dom: 'frtip',  // 'f' filter, 'r' processing display, 't' table, 'i' info, 'p' pagination
        initComplete: function() {
          console.log("DataTables initComplete. Rows:", this.api().rows().count());
        }
      });

      // 4. If you want a global search, DataTables has a search box built in
      //    or you can place a custom search input somewhere else.

      // 5. Optional top-scroll sync
      syncTopScrollbar();
    }
  });
});

// Optional: Sync top scrollbar with table container
function syncTopScrollbar() {
  const topScroll = document.getElementById('top-scrollbar');
  const topScrollContent = document.getElementById('top-scroll-content');
  const tableContainer = document.getElementById('table-container');
  const dynamicTable = document.getElementById('dynamic-table');

  function updateScrollWidths() {
    const w = dynamicTable.scrollWidth;
    topScrollContent.style.width = w + 'px';
  }

  topScroll.addEventListener('scroll', () => {
    tableContainer.scrollLeft = topScroll.scrollLeft;
  });
  tableContainer.addEventListener('scroll', () => {
    topScroll.scrollLeft = tableContainer.scrollLeft;
  });

  // Give DataTables time to finalize layout
  setTimeout(updateScrollWidths, 1000);
  window.addEventListener('resize', updateScrollWidths);
}
</script>
