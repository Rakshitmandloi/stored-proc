import React from 'react';
import { Container, Box, Typography, Card, CardActionArea, Grid } from '@mui/material';
import { styled } from '@mui/system';

const features = [
  { name: 'View Program' },
  { name: 'Add/Edit Program' },
  { name: 'Flow Catalog' },
  { name: 'Program Calendar' },
  { name: 'System Availability' },
];

const StyledCard = styled(Card)({
  backgroundColor: 'rgba(0, 0, 0, 0.5)',
  color: 'white',
  height: 150,
  display: 'flex',
  alignItems: 'center',
  justifyContent: 'center',
  borderRadius: 16,
  transition: 'background-color 0.3s ease',
  '&:hover': {
    backgroundColor: 'red',
  },
});

function LandingPage() {
  const handleButtonClick = (feature) => {
    alert(`${feature} clicked`);
  };

  return (
    <Container sx={{ display: 'flex', flexDirection: 'column', height: '100vh', justifyContent: 'center', alignItems: 'center' }}>
      <Box sx={{ textAlign: 'center', mb: 4 }}>
        <Typography variant="h4" component="div" gutterBottom>
          Welcome, User!
        </Typography>
      </Box>
      <Box sx={{ flexGrow: 1, display: 'flex', justifyContent: 'center', alignItems: 'center' }}>
        <Grid container spacing={2} justifyContent="center" alignItems="center">
          {features.map((feature) => (
            <Grid item xs={12} sm={6} md={4} lg={3} key={feature.name}>
              <StyledCard>
                <CardActionArea sx={{ height: '100%' }} onClick={() => handleButtonClick(feature.name)}>
                  <Typography variant="h6">{feature.name}</Typography>
                </CardActionArea>
              </StyledCard>
            </Grid>
          ))}
        </Grid>
      </Box>
    </Container>
  );
}

export default LandingPage;
