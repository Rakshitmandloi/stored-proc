import React from 'react';
import { Container, Box, Typography, Button, Grid } from '@mui/material';

function LandingPage() {
  const handleButtonClick = (feature) => {
    alert(`Feature ${feature} clicked`);
  };

  return (
    <Container>
      <Box sx={{ textAlign: 'center', my: 4 }}>
        <Typography variant="h4" component="div" gutterBottom>
          Welcome, User!
        </Typography>
        <Grid container spacing={2} justifyContent="center">
          <Grid item>
            <Button 
              variant="contained" 
              color="primary" 
              sx={{ width: 200, height: 50 }}
              onClick={() => handleButtonClick(1)}
            >
              Feature 1
            </Button>
          </Grid>
          <Grid item>
            <Button 
              variant="contained" 
              color="secondary" 
              sx={{ width: 200, height: 50 }}
              onClick={() => handleButtonClick(2)}
            >
              Feature 2
            </Button>
          </Grid>
          <Grid item>
            <Button 
              variant="contained" 
              color="success" 
              sx={{ width: 200, height: 50 }}
              onClick={() => handleButtonClick(3)}
            >
              Feature 3
            </Button>
          </Grid>
          <Grid item>
            <Button 
              variant="contained" 
              color="warning" 
              sx={{ width: 200, height: 50 }}
              onClick={() => handleButtonClick(4)}
            >
              Feature 4
            </Button>
          </Grid>
          <Grid item>
            <Button 
              variant="contained" 
              color="info" 
              sx={{ width: 200, height: 50 }}
              onClick={() => handleButtonClick(5)}
            >
              Feature 5
            </Button>
          </Grid>
        </Grid>
      </Box>
    </Container>
  );
}

export default LandingPage;
