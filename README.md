const columns = [
  {
    field: 'name',
    headerName: 'Name',
    width: 200,
    renderCell: (params) => (
      <div style={{ display: 'flex', alignItems: 'center' }}>
        <VisibilityIcon style={{ marginRight: 8 }} /> {/* Add this line */}
        {params.value}
      </div>
    ),
  },
  {
    field: 'process',
    headerName: 'Process',
    width: 150,
    renderCell: (params) => (
      <div style={{ display: 'flex', alignItems: 'center' }}>
        <VisibilityIcon style={{ marginRight: 8 }} />
        {params.value}
      </div>
    ),
  },
  {
    field: 'product',
    headerName: 'Product',
    width: 150,
    renderCell: (params) => (
      <div style={{ display: 'flex', alignItems: 'center' }}>
        <VisibilityIcon style={{ marginRight: 8 }} />
        {params.value}
      </div>
    ),
  },
  {
    field: 'region',
    headerName: 'Region',
    width: 150,
    renderCell: (params) => (
      <div style={{ display: 'flex', alignItems: 'center' }}>
        <VisibilityIcon style={{ marginRight: 8 }} />
        {params.value}
      </div>
    ),
  },
];
