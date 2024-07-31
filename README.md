import EditIcon from '@mui/icons-material/Edit';
import BookIcon from '@mui/icons-material/Book';
import EventIcon from '@mui/icons-material/Event';
import ReportIcon from '@mui/icons-material/Report';

const StyledCard = styled(Card)`
  max-width: 345px;
  margin: auto;
  transition: transform 0.15s ease-in-out;
  &:hover {
    transform: scale(1.05);
    padding: 5px;
  }
`;

const CenteredCardActionArea = styled(CardActionArea)`
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  flex-direction: column;
`;

const theme = createTheme();

const features = [
  { name: 'View Program', component: 1, icon: <VisibilityIcon style={{ fontSize: 100 }} /> },
  { name: 'Add/Edit Program', component: 2, icon: <EditIcon style={{ fontSize: 100 }} /> },
  { name: 'Flow Catalog', component: 3, icon: <BookIcon style={{ fontSize: 100 }} /> },
  { name: 'Program Calendar', component: 4, icon: <EventIcon style={{ fontSize: 100 }} /> },
  { name: 'System Availability', component: 5, icon: <ReportIcon style={{ fontSize: 100 }} /> }
];

const LandingPageContent = ({ onCardClick }) => (
  <ThemeProvider theme={theme}>
    <Container sx={{ display: 'flex', flexDirection: 'column', height: '100vh' }}>
      <Grid container spacing={7} rowSpacing={8} justifyContent="center" alignItems="center" sx={{ height: '100%' }}>
        {features.map((feature) => (
          <Grid item xs={4} md={4} lg={4} key={feature.name} sx={{ height: '100%' }}>
            <StyledCard>
              <CenteredCardActionArea onClick={() => onCardClick(feature.component, feature.name)}>
                {feature.icon}
                <Typography variant="h5">{feature.name}</Typography>
              </CenteredCardActionArea>
            </StyledCard>
          </Grid>
        ))}
      </Grid>
    </Container>
  </ThemeProvider>
);
