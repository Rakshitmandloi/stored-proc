import React, { useState } from 'react';
import { Container, Box, Typography, Card, CardActionArea, Grid, CardMedia, Button } from '@mui/material';
import { styled } from '@mui/system';

import FlowCatalogView from './components/FlowCatalogView';
import ViewProgramView from './components/ViewProgramView';
import SystemAvailabilityView from './components/SystemAvailabilityView';
import ProgramCalendarView from './components/ProgramCalendarView';
import AddEditProgramView from './components/AddEditProgramView';

const features = [
  { name: 'View Program', component: ViewProgramView },
  { name: 'Add/Edit Program', component: AddEditProgramView },
  { name: 'Flow Catalog', component: FlowCatalogView },
  { name: 'Program Calendar', component: ProgramCalendarView },
  { name: 'System Availability', component: SystemAvailabilityView },
];

const defaultImage = 'path_to_default_image'; // Replace with your image path

const StyledCard = styled(Card)({
  backgroundColor: 'rgba(0, 0, 0, 0.5)',
  color: 'white',
  height: 150,
  width: 150,
  display: 'flex',
  alignItems: 'center',
  justifyContent: 'center',
  borderRadius: 16,
  transition: 'background-color 0.3s ease',
  '&:hover': {
    backgroundColor: 'red',
  },
});

const CenteredCardActionArea = styled(CardActionArea)({
  display: 'flex',
  alignItems: 'center',
  justifyContent: 'center',
  height: '100%',
  flexDirection: 'column',
});

const LandingPageContent = ({ onCardClick }) => (
  <Box sx={{ flexGrow: 1, display: 'flex', justifyContent: 'center', alignItems: 'center' }}>
    <Grid container spacing={2} justifyContent="center" alignItems="center">
      {features.map((feature) => (
        <Grid item xs={12} sm={6} md={4} lg={3} key={feature.name}>
          <StyledCard>
            <CenteredCardActionArea onClick={() => onCardClick(feature.component)}>
              <CardMedia
                component="img"
                image={defaultImage}
                alt={feature.name}
                sx={{ height: '80%', width: '80%', objectFit: 'cover', mb: 1 }}
              />
              <Typography variant="h6">{feature.name}</Typography>
            </CenteredCardActionArea>
          </StyledCard>
        </Grid>
      ))}
    </Grid>
  </Box>
);

function LandingPage() {
  const [selectedComponent, setSelectedComponent] = useState('default');

  const handleButtonClick = (component) => {
    setSelectedComponent(component);
  };

  const renderComponent = () => {
    if (selectedComponent === 'default') {
      return <LandingPageContent onCardClick={handleButtonClick} />;
    }

    const Component = selectedComponent;
    return <Component />;
  };

  return (
    <Container sx={{ display: 'flex', flexDirection: 'column', height: '100vh', justifyContent: 'center', alignItems: 'center' }}>
      <Box sx={{ textAlign: 'center', mb: 4 }}>
        <Typography variant="h4" component="div" gutterBottom>
          Welcome, User!
        </Typography>
        {selectedComponent !== 'default' && (
          <Button variant="contained" onClick={() => setSelectedComponent('default')}>
            Back to Home
          </Button>
        )}
      </Box>
      <Box sx={{ flexGrow: 1, display: 'flex', justifyContent: 'center', alignItems: 'center' }}>
        {renderComponent()}
      </Box>
    </Container>
  );
}

export default LandingPage;
