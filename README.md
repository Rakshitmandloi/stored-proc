const StyledCard = styled(Card)(({ theme }) => ({
  width: '100%',
  margin: 'auto',
  transition: 'transform 0.15s ease-in-out',
  backgroundColor: 'maroon',
  color: 'white',
  '&:hover': {
    transform: 'scale(1.05)',
  },
  [theme.breakpoints.down('sm')]: {
    maxWidth: '90%',  // Set a max-width for small screens
  },
  [theme.breakpoints.up('md')]: {
    maxWidth: '300px',  // Set a max-width for medium and up screens
  },
  [theme.breakpoints.up('lg')]: {
    maxWidth: '345px',  // Set a max-width for large screens
  },
  px: 20,
}));

const CenteredCardActionArea = styled(CardActionArea)({
  display: 'flex',
  alignItems: 'center',
  justifyContent: 'center',
  height: '100%',
  flexDirection: 'column',
});

const LandingPageContent = ({ onCardClick }) => (
  <ThemeProvider theme={theme}>
    <Container sx={{ display: 'flex', flexDirection: 'column', height: '100vh', justifyContent: 'center' }}>
      <h1></h1>
      <Grid container spacing={4} justifyContent="center" alignItems="center" sx={{ height: 'auto' }}>
        {features.map((feature) => (
          <Grid 
            item 
            xs={12} sm={6} md={4} 
            key={feature.name} 
            sx={{ height: 'auto', flexGrow: 1, display: 'flex', justifyContent: 'center' }}
          >
            <StyledCard>
              <CenteredCardActionArea onClick={() => onCardClick(feature.component, feature.name)}>
                <CardMedia
                  component="img"
                  image={feature.icon}
                  title={feature.name}
                  sx={{ height: 'auto', width: '100%', mb: 1, objectFit: 'contain' }}
                />
                <Typography variant="h6" sx={{ backgroundColor: 'maroon', color: 'white', padding: '5px', borderRadius: '4px', textAlign: 'center' }}>
                  {feature.name}
                </Typography>
              </CenteredCardActionArea>
            </StyledCard>
          </Grid>
        ))}
      </Grid>
    </Container>
  </ThemeProvider>
);
