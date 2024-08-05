import React, { useState, useEffect } from 'react';
import { Container, Box, Typography, Card, CardActionArea, Grid, CircularProgress, Button, Paper, Fab } from '@mui/material';
import { styled, createTheme, ThemeProvider } from '@mui/material/styles';
import ViewProgram from './ViewProgram.jpg';
import SystemAvailability from './SystemAvailability.jpg';
import AddEditProgram from './AddEditProgram.jpg';
import ProgramCalendar from './ProgramCalendar.jpg';
import FlowCatalogue from './FlowCatalogue.jpg';
import ViewNewProgramView from './components/ViewNewProgramView';
import SystemAvailabilityView from './components/SystemAvailabilityView';
import FlowCatalogueView from './components/FlowCatalogueView';
import ProgramCalendarView from './components/ProgramCalendarView';
import AddEditProgramView from './components/AddEditProgramView';
import CalendarMonth from './components/CalendarMonth';
import Icon from '@mui/material/Icon';
import IconButton from '@mui/material/IconButton';
import HomeIcon from '@mui/icons-material/Home';
import Settings from '@mui/icons-material/Settings';
import ReplyAllIcon from '@mui/icons-material/ReplyAll';
import axios from 'axios';

const theme = createTheme({
  typography: {
    fontFamily: 'Orbitron, sans-serif', // Use Orbitron for a sci-fi look
    h6: {
      fontWeight: 'bold',
      letterSpacing: '1px',
    },
  },
  palette: {
    primary: {
      main: '#0b8bc5',
    },
    secondary: {
      main: '#BABABA',
    },
  },
});

const theme2 = createTheme({
  palette: {
    primary: {
      main: '#0b8bc5',
    },
    secondary: {
      main: '#BABABA',
    },
    red: {
      main: '#5c2a2a',
    },
    button: {
      textTransform: 'none'
    }
  }
});

const defaultImage = 'https://via.placeholder.com/150';

const features = [
  { name: 'View Program', component: 1, icon: ViewProgram },
  { name: 'Add/Edit Program', component: 2, icon: AddEditProgram },
  { name: 'View Catalog', component: 3, icon: FlowCatalogue },
  { name: 'Program Calendar', component: 4, icon: ProgramCalendar },
  { name: 'System Availability', component: 5, icon: SystemAvailability }
];

const StyledCard = styled(Card)`
  width: 200px;
  height: 200px;
  background: #5c2a2a;
  color: white;
  padding: 10px;
  border-radius: 15px;
  margin: 10px;
  box-shadow: 5px 5px 10px rgba(0,0,0,0.5);
  cursor: pointer;
  &:hover {
    transform: scale(1.05);
  }
`;

const StyledCardActionArea = styled(CardActionArea)`
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  height: 100%;
`;

const LandingPage = () => {
  const [selectedComponent, setSelectedComponent] = useState(0);
  const [user, setUser] = useState('');
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  // Function to handle the API call
  const handleSubmit = async () => {
    setLoading(true);
    setError(null); // Reset error state
    try {
      const response = await fetch(`https://intranetws.nomuranow.com/snow-sso/access-token.json`);
      if (response.ok) {
        const data = await response.json();
        if (data.access_token) {
          // Assuming token received is the indication of a valid user
          console.log('Token:', data.access_token);
          setSelectedComponent(1); // Navigate to the main landing page by setting selectedComponent
        } else {
          setError('Sorry, you are not a valid user.');
        }
      } else {
        setError('User details not found');
      }
    } catch (err) {
      setError('Error checking user');
    } finally {
      setLoading(false);
    }
  };

  // Function to render components based on the selected component
  const renderComponent = () => {
    switch (selectedComponent) {
      case 1:
        return <ViewNewProgramView />;
      case 2:
        return <AddEditProgramView />;
      case 3:
        return <FlowCatalogueView />;
      case 4:
        return <ProgramCalendarView />;
      case 5:
        return <SystemAvailabilityView />;
      default:
        return null;
    }
  };

  return (
    <ThemeProvider theme={theme2}>
      <Container>
        {/* Landing screen for user to enter username */}
        {selectedComponent === 0 && (
          <>
            <Typography variant="h4" gutterBottom>
              Enter your Username
            </Typography>
            <TextField
              label="Username"
              variant="outlined"
              fullWidth
              value={user}
              onChange={(e) => setUser(e.target.value)}
            />
            <Button
              variant="contained"
              color="primary"
              onClick={handleSubmit}
              disabled={loading}
              style={{ marginTop: '20px' }}
            >
              {loading ? <CircularProgress size={24} color="inherit" /> : 'Submit'}
            </Button>
            {error && (
              <Typography color="error" style={{ marginTop: '20px' }}>
                {error}
              </Typography>
            )}
          </>
        )}

        {/* Render landing page components if a valid user */}
        {selectedComponent !== 0 && (
          <div style={{ display: 'flex', flexDirection: 'column', alignItems: 'center' }}>
            <Typography variant="h4" gutterBottom>
              Welcome to the Landing Page
            </Typography>
            <Grid container spacing={2} justifyContent="center">
              {features.map((feature, index) => (
                <Grid item key={index}>
                  <StyledCard onClick={() => setSelectedComponent(feature.component)}>
                    <StyledCardActionArea>
                      <img src={feature.icon} alt={feature.name} style={{ height: '100px', width: '100px', objectFit: 'contain' }} />
                      <Typography variant="h6" style={{ color: 'white', paddingTop: '10px' }}>
                        {feature.name}
                      </Typography>
                    </StyledCardActionArea>
                  </StyledCard>
                </Grid>
              ))}
            </Grid>
            <Box mt={4}>
              {renderComponent()}
            </Box>
          </div>
        )}
      </Container>
    </ThemeProvider>
  );
};

export default LandingPage;
