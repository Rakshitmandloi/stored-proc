import React from 'react';
import Paper from '@mui/material/Paper';
import { ThemeProvider, createTheme } from '@mui/material/styles';

const theme = createTheme({
  palette: {
    primary: {
      main: '#beeees',
    },
    secondary: {
      main: '#BABABA',
    },
  },
});

const SystemAvailabilityView = () => {
  const containerStyle = {
    display: 'flex',
    justifyContent: 'center',
    alignItems: 'center',
    height: '100vh', // Full viewport height
  };

  const paperStyle = {
    padding: '20px', // Add some padding for better appearance
    fontSize: '24px', // Adjust font size as needed
    fontFamily: 'Arial, sans-serif', // Change font family if needed
  };

  return (
    <ThemeProvider theme={theme}>
      <div style={containerStyle}>
        <Paper elevation={3} style={paperStyle}>
          System Availability
        </Paper>
      </div>
    </ThemeProvider>
  );
};

export default SystemAvailabilityView;
