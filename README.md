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
  const paperStyle = {
    display: 'flex',
    justifyContent: 'center',
    alignItems: 'center',
    height: '100vh', // Full viewport height
    fontSize: '24px', // Adjust font size as needed
    fontFamily: 'Arial, sans-serif', // Change font family if needed
  };

  return (
    <ThemeProvider theme={theme}>
      <Paper elevation={3} style={paperStyle}>
        System Availability
      </Paper>
    </ThemeProvider>
  );
};

export default SystemAvailabilityView;
