import React from 'react';
import { Grid, Typography, Card, CardActionArea, CardContent } from '@mui/material';
import { styled } from '@mui/material/styles';
import ViewAgendaIcon from '@mui/icons-material/ViewAgenda';
import CalendarMonthIcon from '@mui/icons-material/CalendarMonth';
import SettingsIcon from '@mui/icons-material/Settings';
import AssignmentTurnedInIcon from '@mui/icons-material/AssignmentTurnedIn';
import SvgIcon from '@mui/material/SvgIcon';

// Keep the original theme and other code intact

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

export default LandingPageContent;

// Ensure to keep the rest of your code intact and unchanged
