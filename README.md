const StyledTypography = styled(Typography)({
  backgroundColor: 'red',
  color: 'white',
  padding: '5px',
  borderRadius: '4px',
});

const LandingPageContent = ({ onCardClick }) => (
  <ThemeProvider theme={theme}>
    <Container sx={{ display: 'flex', flexDirection: 'column', height: '100vh' }}>
      <h1></h1>
      <Grid container spacing={7} rowSpacing={0} justifyContent="center" alignItems="center" sx={{ height: '70vh' }}>
        {features.map((feature) => (
          <Grid item xs={4} md={4} lg={4} sm={4} key={feature.name} sx={{ height: '200px' }}>
            <StyledCard>
              <CenteredCardActionArea onClick={() => onCardClick(feature.component, feature.name)}>
                <CardMedia
                  component="img"
                  image={feature.icon}
                  title={feature.name}
                  sx={{ height: '70%', width: '100%', mb: 1 }}
                />
                <StyledTypography variant="h6">{feature.name}</StyledTypography>
              </CenteredCardActionArea>
            </StyledCard>
          </Grid>
        ))}
      </Grid>
    </Container>
  </ThemeProvider>
);
