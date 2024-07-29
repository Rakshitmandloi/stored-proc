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
  typography: {
    fontFamily: 'Roboto, Arial, sans-serif', // Change this to a modern, techie font
    h6: {
      fontFamily: 'Roboto Mono, monospace', // Example of a modern techie font
    },
  },
});

const features = [
  { name: 'View Program', component: 1, icon: 'path_to_icon_1' },
  { name: 'Add/Edit Program', component: 2, icon: 'path_to_icon_2' },
  { name: 'Flow Catalog', component: 3, icon: 'path_to_icon_3' },
  { name: 'Program Calendar', component: 4, icon: 'path_to_icon_4' },
  { name: 'System Availability', component: 5, icon: 'path_to_icon_5' },
];

const defaultImage = 'path_to_default_image'; // Replace with your image path

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
              <CenteredCardActionArea onClick={() => onCardClick(feature.component, feature.name)}>
                <CardMedia
                  component="img"
                  image={feature.icon || defaultImage}
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
  const [viewName, setViewName] = useState('');
  const [username] = useState('mandloir');
  const [loading, setLoading] = useState(true);
  const [errorMessage, setErrorMessage] = useState(null);

  useEffect(() => {
    const fetchUserDetails = async () => {
      try {
        const response = await fetch(`/check_user/${username}`);
        const result = await response.json();

        if (result.status === 'success') {
          setLoading(false);
        } else {
          setErrorMessage(result.message);
        }
      } catch (error) {
        setErrorMessage('Failed to fetch user details');
      }
    };

    fetchUserDetails();
  }, [username]);

  const handleButtonClick = (component, name) => {
    setSelectedComponent(component);
    setViewName(name);
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

  if (loading) {
    return <CircularProgress />;
  }

  if (errorMessage) {
    return <Typography variant="h6" color="error">{errorMessage}</Typography>;
  }

  return (
    <ThemeProvider theme={theme}>
      {selectedComponent !== 0 && (
        <AppBar position="static" color="default">
          <Toolbar>
            <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
              {viewName}
            </Typography>
            <Button color="inherit" onClick={() => setSelectedComponent(0)}>
              Back to Home
            </Button>
          </Toolbar>
        </AppBar>
      )}
      {renderComponent()}
      {selectedComponent === 0 && (
        <AppBar position="static" color="default">
          <Toolbar>
            <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
              Welcome, {username}!
            </Typography>
          </Toolbar>
        </AppBar>
      )}
    </ThemeProvider>
  );
};

export default LandingPage;
