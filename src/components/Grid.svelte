<div class="charts">
    <slot />
  </div>
  
  <style>
    .charts {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
      gap: 20px;
      padding: 20px;
    }
    @media (min-width: 1860px) {
      .charts {
        grid-template-columns: repeat(3, 1fr); /* Maximum 3 charts per row */
      }
    }
    @media (min-width: 1240px) and (max-width: 1859px) {
      .charts {
        grid-template-columns: repeat(2, 1fr); /* 2 charts per row */
      }
    }
    @media (max-width: 1239px) {
      .charts {
        grid-template-columns: 1fr; /* 1 chart per row */
      }
    }
  </style>
  