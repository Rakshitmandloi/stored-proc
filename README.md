<AppBar position="static" color="default">
  <Toolbar sx={{ display: 'flex', justifyContent: 'space-between', position: 'relative' }}>
    <Typography variant="h6" component="div" sx={{ flexGrow: 1, textAlign: 'left' }}>
      Welcome, {username}!
    </Typography>
    <Typography variant="h6" component="div" sx={{ position: 'absolute', left: '50%', transform: 'translateX(-50%)' }}>
      {viewName}
    </Typography>
    {selectedComponent !== 0 && (
      <Button
        color="inherit"
        onClick={() => {
          setSelectedComponent(0);
          setViewName('');
        }}
        sx={{ flexGrow: 0, textAlign: 'right' }}
      >
        Back to Home
      </Button>
    )}
  </Toolbar>
</AppBar>
