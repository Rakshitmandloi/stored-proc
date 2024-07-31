import React, { useState, useEffect } from 'react';
import { Container, Grid, Typography, Card, CardActionArea, CardContent, Button, CircularProgress } from '@mui/material';
import { styled, createTheme, ThemeProvider } from '@mui/material/styles';
import ViewAgendaIcon from '@mui/icons-material/ViewAgenda';
import CalendarMonthIcon from '@mui/icons-material/CalendarMonth';
import SettingsIcon from '@mui/icons-material/Settings';
import AssignmentTurnedInIcon from '@mui/icons-material/AssignmentTurnedIn';

const theme = createTheme({
  typography: {
    fontFamily: 'Orbitron, sans-serif', // Sci-fi look
    fontWeightBold: 700,
    letterSpacing: '1px',
  },
  palette: {
    primary: {
      main: '#6b8e23',
    },
    secondary: {
      main: '#8b8b8b',
    },
    error: {
      main: '#ff6347',
    },
  },
});

const features = [
  { name: 'View Program', component: 1, icon: <ViewAgendaIcon fontSize="large" /> },
  { name: 'Add Program', component: 2, icon: <AssignmentTurnedInIcon fontSize="large" /> },
  { name: 'Program Calendar', component: 3, icon: <CalendarMonthIcon fontSize="large" /> },
  { name: 'System Availability', component: 5, icon: <SettingsIcon fontSize="large" /> },
];

const StyledCard = styled(Card)({
  maxWidth: 345,
  margin: 20,
  transition: 'transform 0.3s ease-in-out',
  '&:hover': {
    transform: 'scale(1.05)',
  },
});

const CenteredCardActionArea = styled(CardActionArea)({
  display: 'flex',
  justifyContent: 'center',
  alignItems: 'center',
  height: '100%',
  flexDirection: 'column',
});

const LandingPageContent = ({ onCardClick }) => {
  return (
    <Grid container spacing={4} justifyContent="center" alignItems="center" sx={{ height: '70vh' }}>
      {features.map((feature, index) => (
        <Grid item key={index} xs={12} sm={6} md={4} lg={3}>
          <StyledCard>
            <CenteredCardActionArea onClick={() => onCardClick(feature.component)}>
              {feature.icon}
              <CardContent>
                <Typography variant="h6" component="div" sx={{ textAlign: 'center' }}>
                  {feature.name}
                </Typography>
              </CardContent>
            </CenteredCardActionArea>
          </StyledCard>
        </Grid>
      ))}
    </Grid>
  );
};

const LandingPage = () => {
  const [selectedComponent, setSelectedComponent] = useState(0);
  const [loading, setLoading] = useState(false);
  const [username, setUsername] = useState('');
  const [userDetails, setUserDetails] = useState({});

  useEffect(() => {
    const checkUser = async () => {
      try {
        const response = await axios.post('/check_user', { username });
        if (response.data.exists) {
          const userDetailsResponse = await axios.post('/get_user', { username });
          setUserDetails(userDetailsResponse.data);
        } else {
          setUserDetails({});
        }
      } catch (error) {
        console.error('Error checking user:', error);
      }
    };
    checkUser();
  }, [username]);

  const handleButtonClick = (component, name) => {
    setSelectedComponent(component);
    setUsername(name);
  };

  const renderComponent = () => {
    switch (selectedComponent) {
      case 1:
        return <ViewProgramView />;
      case 2:
        return <AddProgramView />;
      case 3:
        return <ProgramCalendarView />;
      case 5:
        return <SystemAvailabilityView />;
      default:
        return <LandingPageContent onCardClick={handleButtonClick} />;
    }
  };

  return (
    <ThemeProvider theme={theme}>
      <Container maxWidth="xl">
        {loading ? (
          <CircularProgress color="inherit" />
        ) : (
          <>
            <Typography variant="h3" component="div" sx={{ flexGrow: 1, textAlign: 'left' }}>
              Hello, {username}
            </Typography>
            <Typography variant="h6" component="div" sx={{ position: 'absolute', left: '50%', transform: 'translateX(-50%)' }}>
              Welcome to the Dashboard
            </Typography>
            {selectedComponent !== 0 && (
              <Button color="inherit" onClick={() => { setSelectedComponent(0); setUsername(''); }}>
                Back to Home
              </Button>
            )}
            {renderComponent()}
          </>
        )}
      </Container>
    </ThemeProvider>
  );
};

export default LandingPage;
