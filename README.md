<!doctype html>
<html>
<head>
    <title>Fetch Windows Username</title>
</head>
<body>
<script type="text/javascript">
    function getUsername() {
        try {
            var WinNetwork = new ActiveXObject("WScript.Network");
            return WinNetwork.UserName;
        } catch (e) {
            console.error("ActiveXObject not supported.");
            return "Unknown User";
        }
    }

    window.localStorage.setItem("username", getUsername());
</script>
</body>
</html>


import React, { useState, useEffect } from 'react';
import { Container, Box, Typography, Card, CardActionArea, Grid, AppBar, Toolbar, CircularProgress } from '@mui/material';
import { styled, createTheme, ThemeProvider } from '@mui/system';
import axios from 'axios';

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
                  image={defaultImage}
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
  const [username, setUsername] = useState(null);

  useEffect(() => {
    const username = window.localStorage.getItem("username");
    setUsername(username);
  }, []);

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
      {renderComponent()}
      {selectedComponent === 0 && (
        <AppBar position="static" color="default">
          <Toolbar>
            {username ? (
              <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
                Welcome, {username}!
              </Typography>
            ) : (
              <CircularProgress color="inherit" />
            )}
            {selectedComponent !== 0 && (
              <Button color="inherit" onClick={() => setSelectedComponent(0)}>
                Back to Home
              </Button>
            )}
          </Toolbar>
        </AppBar>
      )}
    </ThemeProvider>
  );
};

export default LandingPage;

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>React App</title>
    <script type="text/javascript">
        // Check if the username is already set in localStorage
        if (!localStorage.getItem("username")) {
            window.location.href = "/getUser.html";
        }
    </script>
</head>
<body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
</body>
</html>


