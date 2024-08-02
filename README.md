const StyledCard = styled(Card)({
  maxWidth: '100%',
  margin: 'auto',
  transition: 'transform 0.15s ease-in-out',
  backgroundColor: 'maroon',  // Set the background color to maroon
  color: 'white',  // Set the text color to white
  '&:hover': {
    transform: 'scale(1.05)',
  },
  px: 20,
});

const CenteredCardActionArea = styled(CardActionArea)({
  display: 'flex',
  alignItems: 'center',
  justifyContent: 'center',
  height: '100%',
  flexDirection: 'column',
});

const LandingPageContent = ({ onCardClick }) => (
  <ThemeProvider theme={theme}>
    <Container sx={{ display: 'flex', flexDirection: 'column', height: '100vh' }}>
      <h1></h1>
      <Grid container spacing={4} justifyContent="center" alignItems="center" sx={{ height: '70vh' }}>
        {features.map((feature) => (
          <Grid 
            item 
            xs={12} sm={6} md={4} 
            key={feature.name} 
            sx={{ height: { xs: '150px', sm: '200px', md: '250px' } }}
          >
            <StyledCard>
              <CenteredCardActionArea onClick={() => onCardClick(feature.component, feature.name)}>
                <CardMedia
                  component="img"
                  image={feature.icon}
                  title={feature.name}
                  sx={{ height: '70%', width: '100%', mb: 1, objectFit: 'contain' }}
                />
                <Typography variant="h6" sx={{ backgroundColor: 'maroon', color: 'white', padding: '5px', borderRadius: '4px' }}>
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
