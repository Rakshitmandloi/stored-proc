import React from 'react';
import { Container, Box, Typography, Card, CardActionArea, Grid } from '@mui/material';

const features = [
  { name: 'View Program' },
  { name: 'Add/Edit Program' },
  { name: 'Flow Catalog' },
  { name: 'Program Calendar' },
  { name: 'System Availability' },
];

function LandingPage() {
  const handleButtonClick = (feature) => {
    alert(`${feature} clicked`);
  };

  return (
    <Container>
      <Box sx={{ textAlign: 'center', my: 4 }}>
        <Typography variant="h4" component="div" gutterBottom>
          Welcome, User!
        </Typography>
        <Grid container spacing={2} justifyContent="center">
          {features.map((feature) => (
            <Grid item xs={12} sm={6} md={4} lg={3} key={feature.name}>
              <Card
                sx={{
                  bgcolor: 'grey.300',
                  color: 'black',
                  height: 150,
                  display: 'flex',
                  alignItems: 'center',
                  justifyContent: 'center',
                  borderRadius: 2,
                }}
              >
                <CardActionArea
                  sx={{ height: '100%' }}
                  onClick={() => handleButtonClick(feature.name)}
                >
                  <Typography variant="h6">{feature.name}</Typography>
                </CardActionArea>
              </Card>
            </Grid>
          ))}
        </Grid>
      </Box>
    </Container>
  );
}

export default LandingPage;
