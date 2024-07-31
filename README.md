import * as React from 'react';
import { Card, CardContent, CardMedia, Typography, Grid, Container } from '@mui/material';
import { styled } from '@mui/system';
import HomeIcon from '@mui/icons-material/Home';
import SettingsIcon from '@mui/icons-material/Settings';
import InfoIcon from '@mui/icons-material/Info';

const StyledCard = styled(Card)({
  maxWidth: 345,
  margin: 'auto',
  transition: 'transform 0.15s ease-in-out',
  '&:hover': {
    transform: 'scale(1.05)',
  },
});

const iconMap = {
  home: <HomeIcon sx={{ fontSize: 70 }} />,
  settings: <SettingsIcon sx={{ fontSize: 70 }} />,
  info: <InfoIcon sx={{ fontSize: 70 }} />,
};

const LandingPageContent = ({ onCardClick, features = [] }) => {
  return (
    <Container sx={{ display: 'flex', flexDirection: 'column', height: '100vh' }}>
      <Typography variant="h1">Landing Page</Typography>
      <Grid container spacing={4} justifyContent="center" alignItems="center" sx={{ height: '70vh' }}>
        {features.map((feature) => (
          <Grid item xs={12} md={4} lg={4} key={feature.name} sx={{ height: '200px' }}>
            <StyledCard onClick={() => onCardClick(feature.component, feature.name)}>
              <CardContent sx={{ display: 'flex', flexDirection: 'column', alignItems: 'center' }}>
                {iconMap[feature.icon]}
                <Typography variant="h5" component="div" sx={{ marginTop: 2 }}>
                  {feature.name}
                </Typography>
              </CardContent>
            </StyledCard>
          </Grid>
        ))}
      </Grid>
    </Container>
  );
};

export default LandingPageContent;
