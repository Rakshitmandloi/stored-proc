import React, { useState, useEffect } from 'react';
import { Container, Box, Typography, Card, CardActionArea, Grid, CardMedia, AppBar, Toolbar, CircularProgress, Button } from '@mui/material';
import { styled, createTheme, ThemeProvider } from '@mui/material/styles';

import FlowCatalogView from './components/FlowCatalogView';
import ViewProgramView from './components/ViewProgramView';
import SystemAvailabilityView from './components/SystemAvailabilityView';
import ProgramCalenderView from './components/ProgramCalenderView';
import AddEditProgramView from './components/AddEditProgramView';
import './App.css';

const theme = createTheme({
  palette: {
    primary: {
      main: '#b0bec5',
    },
    secondary: {
      main: '#BABABA',
    },
  },
});

const features = [
  { name: 'View Program', component: 1, icon: 'https://via.placeholder.com/150' }, // Placeholder icon URL
  { name: 'Add/Edit Program', component: 2, icon: 'https://via.placeholder.com/150' }, // Placeholder icon URL
  { name: 'Flow Catalog', component: 3, icon: 'https://via.placeholder.com/150' }, // Placeholder icon URL
  { name: 'Program Calendar', component: 4, icon: 'https://via.placeholder.com/150' }, // Placeholder icon URL
  { name: 'System Availability', component: 5, icon: 'https://via.placeholder.com/150' }, // Placeholder icon URL
];

const defaultImage = 'https://via.placeholder.com/150'; // Placeholder image URL

const StyledCard = styled(Card)({
  backgroundColor: 'rgba(0, 0, 0, 0.5)',
  color: 'white',
  height: '100%',
  display: 'flex',
  alignItems: 'center',
  justifyContent: 'center',
  borderRadius: 16,
  transition: 'background-color 0.3s ease',
  '&:hover': {
    backgroundColor: 'grey',
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
  <Container sx={{ display: 'flex', flexDirection: 'column', height: '100vh' }}>
    <Box sx={{ flexGrow: 1, display: 'flex', justifyContent: 'center', alignItems: 'center' }}>
      <Grid container spacing={2} justifyContent="center" alignItems="center">
        {features.map((feature) => (
          <Grid item xs={12} md={4} lg={3} key={feature.name} sx={{ height: '200px' }}>
            <StyledCard>
              <CenteredCardActionArea onClick={() => onCardClick(feature.component)}>
                <CardMedia
                  component="img"
                  image={feature.icon}
                  title={feature.name}
                  sx={{ height: '70%', width: '100%', objectFit: 'cover', mb: 1 }}
                />
                <Typography variant="h6">{feature.name}</Typography>
              </CenteredCardActionArea>
            </StyledCard>
          </Grid>
        ))}
      </Grid>
    </Box>
  </Container>
);

const LandingPage = () => {
  const [selectedComponent, setSelectedComponent] = useState(0);
  const [username, setUsername] = useState('mandloir');
  const [loading, setLoading] = useState(true);
  const [userExists, setUserExists] = useState(false);

  useEffect(() => {
    // Simulate checking the username in the database
    setTimeout(() => {
      const existingUsers = ['mandloir', 'user1', 'user2']; // Simulate existing users in the database
      if (existingUsers.includes(username)) {
        setUserExists(true);
      } else {
        setUserExists(false);
        // Simulate adding the user to the database
        console.log(`Adding ${username} to the database...`);
      }
      setLoading(false);
    }, 1000); // Simulate a delay for checking username
  }, [username]);

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
        return <ProgramCalenderView />;
      case 5:
        return <SystemAvailabilityView />;
      default:
        return <LandingPageContent onCardClick={handleButtonClick} />;
    }
  };

  return (
    <ThemeProvider theme={theme}>
      <AppBar position="static" color="default">
        <Toolbar>
          {loading ? (
            <CircularProgress color="inherit" />
          ) : userExists ? (
            <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
              Welcome, {username}!
            </Typography>
          ) : (
            <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
              User {username} not found, adding to database...
            </Typography>
          )}
          {selectedComponent !== 0 && (
            <Button color="inherit" onClick={() => setSelectedComponent(0)}>
              Back to Home
            </Button>
          )}
        </Toolbar>
      </AppBar>
      {userExists && renderComponent()}
    </ThemeProvider>
  );
};

export default LandingPage;
