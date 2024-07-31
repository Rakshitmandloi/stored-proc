import React, { useState, useEffect } from 'react';
import { Container, Grid, Typography, Card, CardActionArea, CardContent, Button, CircularProgress, AppBar, Toolbar, IconButton } from '@mui/material';
import { styled, createTheme, ThemeProvider } from '@mui/material/styles';
import ViewAgendaIcon from '@mui/icons-material/ViewAgenda';
import CalendarMonthIcon from '@mui/icons-material/CalendarMonth';
import SettingsIcon from '@mui/icons-material/Settings';
import AssignmentTurnedInIcon from '@mui/icons-material/AssignmentTurnedIn';
import MenuIcon from '@mui/icons-material/Menu';
import SvgIcon from '@mui/material/SvgIcon';

const theme = createTheme({
  typography: {
    fontFamily: 'Orbitron, sans-serif', // Sci-fi look
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
  { name: 'View Program', component: 1, icon: <SvgIcon component={ViewAgendaIcon} inheritViewBox /> },
  { name: 'Add Program', component: 2, icon: <SvgIcon component={AssignmentTurnedInIcon} inheritViewBox /> },
  { name: 'Program Calendar', component: 3, icon: <SvgIcon component={CalendarMonthIcon} inheritViewBox /> },
  { name: 'System Availability', component: 5, icon: <SvgIcon component={SettingsIcon} inheritViewBox /> },
];

const StyledCard = styled(Card)({
  maxWidth: 345,
  margin: 20,
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
      <AppBar position="static">
        <Toolbar>
          <IconButton edge="start" color="inherit" aria-label="menu">
            <MenuIcon />
          </IconButton>
          <Typography variant="h6">
            Dashboard
          </Typography>
        </Toolbar>
      </AppBar>
      <Container maxWidth="xl">
        {loading ? (
          <CircularProgress color="inherit" />
        ) : (
          <>
            <Typography variant="h3" component="div" sx={{ flexGrow: 1, textAlign: 'left', marginTop: 2 }}>
              Hello, {username}
            </Typography>
            <Typography variant="h6" component="div" sx={{ position: 'absolute', left: '50%', transform: 'translateX(-50%)', marginTop: 2 }}>
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
