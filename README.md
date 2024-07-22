import React, { useState } from 'react';
import { Container, Box, Typography, Card, CardActionArea, Grid, CardMedia, Button, AppBar, Toolbar } from '@mui/material';
import { styled } from '@mui/system';

import FlowCatalogView from './components/FlowCatalogView';
import ViewProgramView from './components/ViewProgramView';
import SystemAvailabilityView from './components/SystemAvailabilityView';
import ProgramCalendarView from './components/ProgramCalendarView';
import AddEditProgramView from './components/AddEditProgramView';

const features = [
  { name: 'View Program', component: 1 },
  { name: 'Add/Edit Program', component: 2 },
  { name: 'Flow Catalog', component: 3 },
  { name: 'Program Calendar', component: 4 },
  { name: 'System Availability', component: 5 },
];

const defaultImage = 'path_to_default_image'; // Replace with your image path

const StyledCard = styled(Card)({
  backgroundColor: 'rgba(0, 0, 0, 0.5)',
  color: 'white',
  height: '100%',
  width: '100%',
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
        <Grid item xs={12} sm={6} md={4} lg={3} key={feature.name} sx={{ height: '200px' }}>
          <StyledCard>
            <CenteredCardActionArea onClick={() => onCardClick(feature.component)}>
              <CardMedia
                component="img"
                image={defaultImage}
                alt={feature.name}
                sx={{ height: '70%', width: '70%', objectFit: 'cover', mb: 1 }}
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
  const [selectedComponent, setSelectedComponent] = useState(0);

  const handleButtonClick = (component) => {
    setSelectedComponent(component);
  };

  const renderComponent = () => {
    switch (selectedComponent) {
      case 1:
        return <ViewProgramView />;
      case 2:
        return <AddEditProgramView />;
      case 3:
        return <FlowCatalogView />;
      case 4:
        return <ProgramCalendarView />;
      case 5:
        return <SystemAvailabilityView />;
      default:
        return <LandingPageContent onCardClick={handleButtonClick} />;
    }
  };

  return (
    <Container sx={{ display: 'flex', flexDirection: 'column', height: '100vh' }}>
      <AppBar position="static">
        <Toolbar>
          <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
            Welcome, User!
          </Typography>
          {selectedComponent !== 0 && (
            <Button color="inherit" onClick={() => setSelectedComponent(0)}>
              Back to Home
            </Button>
          )}
        </Toolbar>
      </AppBar>
      <Box sx={{ flexGrow: 1, display: 'flex', justifyContent: 'center', alignItems: 'center', mt: 4 }}>
        {renderComponent()}
      </Box>
    </Container>
  );
}

export default LandingPage;
